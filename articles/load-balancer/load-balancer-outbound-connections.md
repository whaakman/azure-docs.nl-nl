---
title: Uitgaande verbindingen in Azure | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe virtuele machines om te communiceren met het openbare internet-services in Azure mogelijk maakt.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/27/2018
ms.author: kumud
ms.openlocfilehash: ea8e8ae9b0f487481ac2f25d4e2b9c5733e15431
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43842252"
---
# <a name="outbound-connections-in-azure"></a>Uitgaande verbindingen in Azure

Azure biedt uitgaande connectiviteit voor implementaties van klanten via meerdere verschillende mechanismen. Dit artikel wordt beschreven wat de scenario's zijn, wanneer ze van toepassing zijn, hoe deze werken en hoe deze te beheren.

>[!NOTE] 
>Dit artikel behandelt alleen voor implementaties van Resource Manager. Beoordeling [uitgaande verbindingen (klassiek)](load-balancer-outbound-connections-classic.md) voor alle klassieke implementatiescenario's in Azure.

Een implementatie in Azure kan communiceren met eindpunten buiten Azure in de openbare IP-adresruimte. Wanneer een exemplaar een uitgaande stroom naar een bestemming in de openbare IP-adresruimte initieert, Azure dynamisch privé IP-adres toegewezen aan een openbaar IP-adres. Nadat u deze toewijzing is gemaakt, kan retourverkeer voor deze uitgaande afkomstige stroom ook bereiken het privé IP-adres waarvan de stroom afkomstig is.

Azure maakt gebruik van bron-NAT (SNAT) om uit te voeren van deze functie. Wanneer meerdere persoonlijke IP-adressen zijn zich achter een enkel openbaar IP-adres, gebruikt Azure [netwerkadresomzetting (PAT)-poort](#pat) aan het privé IP-adressen zich voordoen. Kortstondige poorten worden gebruikt voor PAT en [vooraf toegewezen](#preallocatedports) op basis van de grootte van de opslaggroep.

Er zijn meerdere [scenario's voor uitgaande](#scenarios). U kunt deze scenario's kunt combineren, indien nodig. Bekijk deze zorgvuldig voor meer informatie over de mogelijkheden, beperkingen en patronen als ze van toepassing op uw implementatiemodel zijn en het toepassingsscenario. Bekijk de richtlijnen voor [beheren van deze scenario's](#snatexhaust).

>[!IMPORTANT] 
>Standard Load Balancer introduceert nieuwe mogelijkheden en ander gedrag aan uitgaande connectiviteit.   Bijvoorbeeld, [scenario 3](#defaultsnat) bestaat niet als een interne Standard Load Balancer aanwezig is en de verschillende stappen dienen te worden genomen.   Zorgvuldig door dit hele document voor meer informatie over de algemene concepten en verschillen tussen SKU's.

## <a name="scenarios"></a>Scenario-overzicht

Azure Load Balancer en verwante resources expliciet worden gedefinieerd als u [Azure Resource Manager](#arm).  Azure biedt momenteel drie verschillende methoden voor het bereiken van uitgaande connectiviteit voor Azure Resource Manager-resources. 

| Scenario | Methode | IP-protocollen | Beschrijving |
| --- | --- | --- | --- |
| [1. VM met een Instance Level Public IP-adres (met of zonder Load Balancer)](#ilpip) | SNAT, poort zich niet gebruikt. | TCP, UDP, ICMP, ESP | Azure maakt gebruik van het openbare IP-adres toegewezen aan de IP-configuratie van het exemplaar van de NIC. Het exemplaar heeft alle kortstondige poorten die beschikbaar zijn. |
| [2. Een openbare Load Balancer die zijn gekoppeld aan een virtuele machine (geen Instance Level Public IP-adres op het exemplaar)](#lb) | SNAT met poort onechte (PAT) met behulp van de Load Balancer-front-ends | TCP, UDP |Azure deelt het openbare IP-adres van de openbare Load Balancer-front-ends met meerdere privé IP-adressen. Kortstondige poorten van de front-ends voor PAT maakt gebruik van Azure. |
| [3. Standalone VM (Er is geen Load Balancer, geen Instance Level Public IP-adres)](#defaultsnat) | SNAT met poort onechte (PAT) | TCP, UDP | Azure automatisch een openbaar IP-adres voor SNAT aanwijst, deelt dit openbare IP-adres met meerdere privé IP-adressen van de beschikbaarheidsset en maakt gebruik van kortstondige poorten van dit openbare IP-adres. Dit is een alternatief scenario voor het voorgaande scenario's. Wordt niet aanbevolen als u nodig, zichtbaarheid en controle hebt. |

Als u niet dat een virtuele machine om te communiceren met eindpunten buiten Azure in openbare IP-adresruimte wilt, kunt u netwerkbeveiligingsgroepen (nsg's) gebruiken om de toegang zo nodig te blokkeren. De sectie [uitgaande connectiviteit te voorkomen dat](#preventoutbound) nsg's vindt u meer details. Hulp bij het ontwerpen, implementeren en beheren van een virtueel netwerk zonder een uitgaande toegang is buiten het bereik van dit artikel.

### <a name="ilpip"></a>Scenario 1: Virtuele machine met een Instance Level Public IP-adres

In dit scenario wordt heeft de virtuele machine een exemplaar van het niveau van openbare IP (ILPIP) zijn toegewezen. Wat betreft uitgaande verbindingen zijn, het maakt niet uit of de virtuele machine verdeeld wordt, of niet. In dit scenario heeft voorrang op de andere. Wanneer een ILPIP wordt gebruikt, wordt de virtuele machine de ILPIP gebruikt voor alle uitgaande stromen.  

Een openbare IP die zijn toegewezen aan een virtuele machine is een 1:1-relatie (plaats 1:many) en geïmplementeerd als een stateless 1:1 NAT bevinden.  Poort onechte (PAT) wordt niet gebruikt en de virtuele machine heeft alle kortstondige poorten beschikbaar voor gebruik.

Als uw toepassing veel uitgaande stromen initieert en u SNAT poortuitputting ondervindt, kunt u overwegen een [ILPIP SNAT beperkingen beperken](#assignilpip). Beoordeling [SNAT beheren uitputting](#snatexhaust) in zijn geheel toe.

### <a name="lb"></a>Scenario 2: Netwerktaakverdeling VM zonder een Instance Level Public IP-adres

In dit scenario wordt uitmaakt de virtuele machine deel van een openbare Load Balancer-back-endpool. De virtuele machine heeft geen openbaar IP-adres toegewezen. De Load Balancer-resource moet worden geconfigureerd met een load balancer-regel om een koppeling tussen het openbare IP-frontend met de back-endpool te maken.

Als u deze regelconfiguratie ze niet worden voltooid, is het gedrag zoals beschreven in het scenario voor het [Standalone VM met geen Instance-Level Public IP](#defaultsnat). Het is niet nodig zijn voor de regel om een listener werken in de back-endpool voor de statustest te voltooien.

Wanneer de VM met load balancing een uitgaande stroom maakt, wordt het particuliere IP-bronadres van de uitgaande stroom naar het openbare IP-adres van de openbare Load Balancer-frontend omgezet in Azure. Azure maakt gebruik van SNAT om uit te voeren van deze functie. Azure gebruikt ook [PAT](#pat) naar meerdere privé IP-adressen achter een openbaar IP-adres zich voordoen. 

Kortstondige poorten van de load balancer openbare IP-adres frontend worden gebruikt om te onderscheiden van afzonderlijke stromen afkomstig van de virtuele machine. Dynamisch maakt gebruik van SNAT [vooraf toegewezen kortstondige poorten](#preallocatedports) wanneer uitgaande stromen worden gemaakt. In deze context worden de kortstondige poorten die voor SNAT SNAT poorten genoemd.

SNAT poorten zijn vooraf toegewezen zoals beschreven in de [Understanding SNAT en PAT](#snat) sectie. Zijn in feite een eindige resource die kan worden verbruikt. Het is belangrijk om te weten hoe ze zijn [verbruikt](#pat). Als u wilt weten hoe u voor dit verbruik ontwerpen en te beperken indien nodig, Bekijk [SNAT beheren uitputting](#snatexhaust).

Wanneer [meerdere (openbare) IP-adressen zijn gekoppeld aan de Load Balancer Basic](load-balancer-multivip-overview.md), of een van deze openbare IP-adressen zijn een [kandidaat voor uitgaande stromen](#multivipsnat), en een is geselecteerd.  

Voor het controleren van de status van uitgaande verbindingen met Load Balancer Basic, kunt u [Log Analytics voor Load Balancer](load-balancer-monitor-log.md) en [waarschuwen gebeurtenislogboeken](load-balancer-monitor-log.md#alert-event-log) om te controleren op SNAT poort uitputting van berichten.

### <a name="defaultsnat"></a>Scenario 3: Standalone VM zonder een Instance Level Public IP-adres

In dit scenario wordt de virtuele machine maakt geen deel uit van een openbare Load Balancer-groep (en geen deel uitmaakt van een interne Standard Load Balancer-groep) en heeft geen een ILPIP-adres is toegewezen. Wanneer de virtuele machine een uitgaande stroom maakt, zet Azure privé IP-bronadres van de uitgaande stroom aan een openbare IP-adres. Het openbare IP-adres gebruikt voor deze uitgaande stroom kan niet worden geconfigureerd en worden niet meegeteld in openbare IP-resource-limiet van het abonnement.

>[!IMPORTANT] 
>In dit scenario geldt ook wanneer __alleen__ een interne Basic Load Balancer is gekoppeld. Scenario 3 is __niet beschikbaar__ wanneer een interne Standard Load Balancer is gekoppeld aan een virtuele machine.  U moet expliciet maken [scenario 1](#ilpip) of [scenario 2](#lb) naast het gebruik van een interne Standard Load Balancer.

Azure maakt gebruik van SNAT met poort onechte ([PAT](#pat)) om uit te voeren van deze functie. In dit scenario is vergelijkbaar met [scenario 2](#lb), maar er geen controle over het IP-adres gebruikt is. Dit is een alternatief scenario voor wanneer scenario 1 en 2 niet bestaan. In dit scenario wordt niet aanbevolen als u wilt dat de controle over het uitgaande adres. Als u uitgaande verbindingen zijn een belangrijk onderdeel van uw toepassing, moet u een ander scenario kiezen.

SNAT poorten zijn vooraf toegewezen zoals beschreven in de [Understanding SNAT en PAT](#snat) sectie.  Het aantal virtuele machines delen een Beschikbaarheidsset bepaalt welke laag voorafgaande toewijzing is van toepassing.  Een zelfstandige virtuele machine zonder een Beschikbaarheidsset is in feite een pool van 1 voor de vaststelling van de voorafgaande toewijzing (1024 SNAT poorten). SNAT poorten zijn een eindige resource die kan worden verbruikt. Het is belangrijk om te weten hoe ze zijn [verbruikt](#pat). Als u wilt weten hoe u voor dit verbruik ontwerpen en te beperken indien nodig, Bekijk [SNAT beheren uitputting](#snatexhaust).

### <a name="combinations"></a>Scenario's met meerdere, gecombineerde

U kunt de scenario's beschreven in de voorgaande secties om te realiseren van een bepaalde uitkomst combineren. Als er meerdere scenario's aanwezig zijn, een volgorde van prioriteit is van toepassing: [scenario 1](#ilpip) voorrang boven [scenario 2](#lb) en [3](#defaultsnat). [Scenario 2](#lb) onderdrukkingen [scenario 3](#defaultsnat).

Een voorbeeld is de implementatie van een Azure Resource Manager waar de toepassing is sterk afhankelijk van de uitgaande verbindingen met een beperkt aantal bestemmingen, maar ook via een Load Balancer-frontend ontvangt inkomende stromen. In dit geval kunt u scenario's 1 en 2 voor vrijstelling combineren. Raadpleeg voor meer patronen [SNAT beheren uitputting](#snatexhaust).

### <a name="multife"></a> Meerdere front-ends voor uitgaande stromen

#### <a name="load-balancer-standard"></a>Standaardversie van Load Balancer

Load Balancer Standard maakt gebruik van alle kandidaten voor uitgaande stromen tegelijkertijd is het tijd wanneer [meerdere (openbare) IP-front-ends](load-balancer-multivip-overview.md) aanwezig is. Elke front-end vermenigvuldigt het aantal beschikbare vooraf toegewezen SNAT poorten als een taakverdelingsregel is ingeschakeld voor uitgaande verbindingen.

U kunt onderdrukken een frontend-IP-adres voor uitgaande verbindingen met een nieuwe load balancing regeloptie kan worden gebruikt:

```json    
      "loadBalancingRules": [
        {
          "disableOutboundSnat": false
        }
      ]
```

Normaal gesproken deze optie standaard ingesteld op _false_ en geeft aan dat deze regel programma's voor uitgaande SNAT voor de bijbehorende virtuele machines in de back-endpool van de load balancer-regel.  Dit kan worden gewijzigd in _waar_ om te voorkomen dat de Load Balancer met behulp van het bijbehorende front-end-IP-adres voor uitgaande verbindingen voor de virtuele machine zich in de back-endpool van deze regel voor taakverdeling.  En u kunt ook nog steeds een specifiek IP-adres voor uitgaande stromen opgeven zoals beschreven in [meerdere scenario's gecombineerd](#combinations) ook.

#### <a name="load-balancer-basic"></a>Load Balancer Basic

Load Balancer Basic kiest voor een enkele frontend moet worden gebruikt voor uitgaande stromen wanneer [meerdere (openbare) IP-front-ends](load-balancer-multivip-overview.md) kandidaten zijn voor uitgaande stromen. Deze selectie kan niet worden geconfigureerd en u moet rekening houden met de algoritme voor het selecteren worden willekeurige. U kunt een specifiek IP-adres voor uitgaande stromen kunt opgeven, zoals beschreven in [meerdere scenario's gecombineerd](#combinations).

### <a name="az"></a> Beschikbaarheidszones

Bij het gebruik van [Standard Load Balancer met Beschikbaarheidszones](load-balancer-standard-availability-zones.md), zone-redundante front-ends krijgt u een zone-redundante uitgaande SNAT-verbindingen en SNAT programmeren blijft zone-fout.  Wanneer zonegebonden front-ends worden gebruikt, delen uitgaande SNAT-verbindingen gedrag met de zone waartoe ze behoren.

## <a name="snat"></a>Inzicht krijgen in SNAT en PAT

### <a name="pat"></a>Poort onechte SNAT (PAT)

Wanneer een openbare Load Balancer-resource gekoppeld aan VM-exemplaren is, wordt elke bron uitgaande verbindingen herschreven. De bron is van het virtuele netwerk privé IP-adresruimte herschreven aan de front-end openbare IP-adres van de load balancer. In de openbare IP-adresruimte, moet de 5-tuple van de stroom (bron-IP-adres, bronpoort, transportprotocol IP, doel-IP-adres, doelpoort) uniek zijn.  Onechte SNAT poort kan worden gebruikt met TCP of UDP IP-protocollen.

Kortstondige poorten (SNAT) worden gebruikt om dit te bereiken nadat het herschrijven van de persoonlijke bron-IP-adres, omdat meerdere stromen afkomstig van een enkel openbaar IP-adres zijn. De poort onechte SNAT algoritme wijst SNAT poorten anders voor UDP of TCP.

#### <a name="tcp"></a>SNAT-TCP-poorten

Een poort met SNAT wordt verbruikt per overdracht voor een enkel IP-adres, poort. Voor meerdere TCP-stromen voor hetzelfde doel-IP-adres, poort en protocol verbruikt elke TCP-stroom één SNAT-poort. Dit zorgt ervoor dat de stromen uniek zijn als ze afkomstig uit hetzelfde openbare IP-adres zijn en gaat u naar het hetzelfde doel-IP-adres, poort en protocol. 

Meerdere stromen, elk op een andere doel-IP-adres, poort en protocol, delen een enkel SNAT-poort. De doel-IP-adres, poort en protocol maken stromen uniek zonder de noodzaak voor extra bron poorten te onderscheiden van stromen in de openbare IP-adresruimte.

#### <a name="udp"></a> UDP-poorten voor SNAT

SNAT UDP-poorten worden beheerd door een ander algoritme dan SNAT TCP-poorten.  Load Balancer gebruikt een algoritme bekend als 'poort beperkte kegel NAT' voor UDP.  Een SNAT-poort wordt gebruikt voor elke stroom, ongeacht de doel-IP-adres, poort.

#### <a name="exhaustion"></a>Uitputting

Wanneer SNAT poort resources verbruikt, wordt uitgaande stromen mislukken totdat bestaande stromen SNAT poorten. Load Balancer maakt SNAT poorten wanneer de stroom wordt gesloten en maakt gebruik van een [time-out voor inactiviteit van 4 minuten durende](#idletimeout) voor het vrijmaken van SNAT poorten van niet-actieve stromen.

SNAT UDP-poorten in het algemeen dat verbruikt zal zijn veel sneller dan de poorten TCP SNAT vanwege het verschil in de algoritme die wordt gebruikt. U moeten ontwerp- en schaal testen met dit verschil in aanmerking genomen.

Voor patronen om te beperken van voorwaarden die vaak tot uitputting van SNAT poort leiden, raadpleegt u de [SNAT beheren](#snatexhaust) sectie.

### <a name="preallocatedports"></a>Tijdelijke poort voorafgaande toewijzing voor de onechte SNAT (PAT)-poort

Azure maakt gebruik van een algoritme om te bepalen het aantal vooraf toegewezen SNAT poorten die beschikbaar zijn op basis van de grootte van de back-endpool bij het gebruik van poort onechte SNAT ([PAT](#pat)). SNAT poorten zijn kortstondige poorten die beschikbaar zijn voor een bepaalde openbare IP-bronadres.

Hetzelfde aantal SNAT poorten zijn vooraf toegewezen voor UDP en TCP respectievelijk en onafhankelijk van elkaar per IP-transportprotocol verbruikt.  Het gebruik van SNAT poort is echter verschillend, afhankelijk van of de stroom UDP of TCP is.

>[!IMPORTANT]
>Standaard SKU SNAT programmeren is per IP-transportprotocol en afgeleid van de taakverdelingsregel.  Als er slechts een taakverdelingsregel voor TCP bestaat, is SNAT alleen beschikbaar voor TCP. Als u alleen een TCP-regel voor taakverdeling en uitgaande SNAT voor UDP moet, maakt u een UDP-load balancing-regel op basis van de dezelfde frontend naar de dezelfde back-endadresgroep.  Dit wordt SNAT programmeren voor UDP geactiveerd.  Een werkende regel of de status van de test is niet vereist.  Basic SKU SNAT SNAT wordt altijd programma's voor beide IP-transportprotocol, ongeacht het protocol-transport is opgegeven in de load balancer-regel.

Azure preallocates SNAT poorten op de IP-configuratie van de NIC van elke virtuele machine. Wanneer een IP-configuratie wordt toegevoegd aan de groep, worden de poorten SNAT vooraf toegewezen voor dit IP-configuratie op basis van de grootte van de back-end-groep. Wanneer uitgaande stromen worden gemaakt, [PAT](#pat) dynamisch verbruikt (tot de vooraf toegewezen limiet) en deze poorten worden vrijgegeven wanneer de stroom wordt gesloten of [inactief time-outs](#idletimeout) gebeuren.

De volgende tabel toont de preallocations SNAT poort voor de lagen van de back-end poolgrootten:

| Poolgrootte (VM-exemplaren) | Vooraf toegewezen SNAT poorten per IP-configuratie|
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1000 | 32 |

>[!NOTE]
> Bij het gebruik van Standard Load Balancer met [meerdere front-ends](load-balancer-multivip-overview.md), [elke front-end-IP-adres wordt het aantal beschikbare poorten van SNAT vermenigvuldigd](#multivipsnat) in de vorige tabel. Bijvoorbeeld, een back endpool van 50 VM's met 2 regels voor taakverdeling, elk met een afzonderlijke frontend-IP-adres, (2 x 1024) 2048 SNAT poorten per IP-configuratie gebruikt. Zie de details voor [meerdere front-ends](#multife).

Houd er rekening mee dat het aantal beschikbare poorten op de SNAT geen rechtstreeks naar het aantal stromen vertaalt. Één poort zijn SNAT kan worden hergebruikt voor meerdere unieke doelen. Poorten die worden verbruikt alleen indien nodig stromen om uniek te maken. Voor richtlijnen voor ontwerp en risicobeperking, raadpleegt u de sectie over [over het beheren van deze resource onuitputtelijk](#snatexhaust) en de sectie waarin wordt beschreven [PAT](#pat).

Wijzigen van de grootte van uw back-endgroep invloed zijn op enkele van uw bestaande stromen. Als de grootte van de back-end-groep verhoogt en in de volgende laag overgangen, wordt de helft van de vooraf toegewezen SNAT poorten vrijgemaakt tijdens de overgang naar de volgende laag voor grotere back-end-pool. Stromen die gekoppeld aan een geregenereerde SNAT poort zijn time-out en moeten opnieuw tot stand worden gebracht. Als een nieuwe stroom wordt uitgevoerd, wordt de stroom onmiddellijk slaagt, zolang vooraf toegewezen poorten beschikbaar zijn.

Als de grootte van de back-end-groep wordt verlaagd en in een lagere laag verandert, verhoogt het aantal beschikbare poorten van SNAT. In dit geval SNAT poorten bestaande toegewezen en hun respectieve stromen worden niet beïnvloed.

SNAT poorten toewijzingen zijn specifieke IP-transportprotocol (TCP en UDP-worden afzonderlijk onderhouden) en worden uitgebracht onder de volgende voorwaarden:

### <a name="tcp-snat-port-release"></a>Release van SNAT TCP-poort

- Als beide server/client FIN/ACK verzendt, wordt na 240 seconden SNAT poort uitgebracht.
- Wanneer er een eerste worden gedetecteerd, wordt poort SNAT na 15 seconden worden vrijgegeven.
- time-out voor inactiviteit is bereikt

### <a name="udp-snat-port-release"></a>Release van SNAT UDP-poort

- time-out voor inactiviteit is bereikt

## <a name="problemsolving"></a> Problemen oplossen 

Deze sectie is bedoeld om te beperken SNAT uitputting en andere scenario's die met uitgaande verbindingen in Azure optreden kunnen.

### <a name="snatexhaust"></a> Poortuitputting SNAT (PAT) beheren
[Kortstondige poorten](#preallocatedports) gebruikt voor [PAT](#pat) vormen een onuitputtelijk resource, zoals beschreven in [Standalone VM zonder een Instance Level Public IP-adres](#defaultsnat) en [met Load Balancing virtuele machine zonder een Instantie-Level Public IP-adres](#lb).

Als u weet dat u bij het initiëren van veel uitgaande TCP of UDP-verbindingen op hetzelfde IP-doeladres en poort, en u observeren uitgaande verbindingen mislukken of door het ondersteuningsteam van wordt aangeraden dat u uitput SNAT-poorten bent (vooraf toegewezen [kortstondige poorten](#preallocatedports) die worden gebruikt door [PAT](#pat)), hebt u verschillende algemene risicobeperkende mogelijkheden. Bekijk deze opties en bepaal wat beschikbaar is en voor uw scenario het meest geschikt is. Het is mogelijk dat een of meer kan het beheer van dit scenario.

Als u problemen bij het begrijpen van het gedrag van de uitgaande verbindingen ondervindt, kunt u IP-stack-statistieken (netstat). Of het gedrag van de verbinding met behulp van pakketopnamen in acht nemen handig kan zijn. U kunt deze pakketopnamen uitvoeren in het gastbesturingssysteem van uw exemplaar of [Network Watcher voor pakketopname](../network-watcher/network-watcher-packet-capture-manage-portal.md).

#### <a name="connectionreuse"></a>De toepassing om verbindingen opnieuw te wijzigen 
U kunt de vraag naar kortstondige poorten die worden gebruikt voor SNAT door verbindingen in uw toepassing opnieuw te verminderen. Dit geldt met name voor protocollen zoals HTTP/1.1, waarbij het hergebruik van de verbinding de standaardinstelling is. En andere protocollen die HTTP als transportmechanisme gebruiken (bijvoorbeeld REST gebruiken) op zijn beurt kunnen profiteren. 

Hergebruik is altijd beter dan afzonderlijke, atomic TCP-verbindingen voor elke aanvraag. Hergebruiken resulteert in meer prestaties, zeer efficiënt TCP-transacties.

#### <a name="connection pooling"></a>De toepassing te gebruiken Groepsgewijze verbinding wijzigen
U kunt gebruikmaken van een groepsgewijze verbindingen schema in uw toepassing, waar aanvragen intern worden verdeeld over een vaste set van verbindingen (elke hergebruiken indien mogelijk). Dit schema Hiermee beperkt u het aantal kortstondige poorten in gebruik en maakt u een omgeving beter te voorspellen. Dit schema kunt ook de doorvoer van aanvragen door meerdere gelijktijdige bewerkingen toe wanneer een enkele verbinding wordt geblokkeerd door op het antwoord van een bewerking te verhogen.  

Groepsgewijze verbinding bestaat mogelijk al in het kader die u gebruikt voor het ontwikkelen van uw toepassing of de configuratie-instellingen voor uw toepassing. U kunt combineren Groepsgewijze verbinding met de verbinding opnieuw gebruiken. Uw meerdere aanvragen worden vervolgens een vast, voorspelbaar aantal poorten op de dezelfde IP-doeladres en poort gebruiken. De aanvragen worden ook profiteren van efficiënt gebruik van TCP-transacties latentie en resource-gebruik verminderen. UDP-transacties kunnen ook profiteren, omdat op zijn beurt beheren van het aantal UDP-stromen kunt uitlaatgas voorwaarden voorkomen en het gebruik van de poort SNAT beheren.

#### <a name="retry logic"></a>Wijzig de toepassing te gebruiken minder agressief logica voor opnieuw proberen
Wanneer [vooraf toegewezen kortstondige poorten](#preallocatedports) gebruikt voor [PAT](#pat) zijn uitgeput of toepassing fouten optreden, agressief of beveiligingsaanvallen pogingen zonder verval en uitstel logica leiden tot uitputting of behouden. Aanvraag voor kortstondige poorten kunt u beperken met behulp van een minder agressief logica voor opnieuw proberen. 

Kortstondige poorten hebben een 4 minuten durende time-out voor inactiviteit (niet-aanpasbare). Als de nieuwe pogingen te agressief, heeft de uitputting geen mogelijkheid om te wissen op zichzelf. Overwegen hoe-- en hoe vaak--uw toepassing opnieuw transacties probeert is daarom een belangrijk onderdeel van het ontwerp.

#### <a name="assignilpip"></a>Een exemplaar op openbare IP-adres toewijzen aan elke virtuele machine
Uw scenario toewijzen van een ILPIP verandert [Instance Level Public IP aan een virtuele machine](#ilpip). Alle kortstondige poorten van het openbare IP-adres die worden gebruikt voor elke virtuele machine zijn beschikbaar voor de virtuele machine. (In plaats van scenario's waarbij kortstondige poorten van een openbaar IP-adres worden gedeeld met alle virtuele machines die zijn gekoppeld aan de desbetreffende back-endpool.) Er zijn wisselwerking om te overwegen, zoals de extra kosten van openbare IP-adressen en de impact van opname in de whitelist een groot aantal afzonderlijke IP-adressen.

>[!NOTE] 
>Deze optie is niet beschikbaar voor webwerkrollen.

#### <a name="multifesnat"></a>Meerdere front-ends gebruiken

Wanneer u een openbare Standard Load Balancer, die u toewijst [meerdere front-end-IP-adressen voor uitgaande verbindingen](#multife) en [Vermenigvuldig het aantal beschikbare poorten op de SNAT](#preallocatedports).  U moet maken van een front-end-IP-configuratie, de regel en de back-endpool voor het activeren van het programmeren van SNAT naar het openbare IP-adres van de front-end.  De regel niet hoeft te werken en een statustest hoeft niet te voltooien.  Als u meerdere front-ends voor gebruik, evenals Inkomend (in plaats van alleen voor uitgaande), moet u aangepaste statustests en om ervoor te zorgen betrouwbaarheid.

>[!NOTE]
>In de meeste gevallen is uitputting van SNAT poorten het een teken van slecht ontwerp.  Zorg ervoor dat u weten waarom u uitput poorten zijn voordat u meer front-ends met SNAT poorten toevoegen.  U mogelijk een probleem dat tot mislukte later leiden kan worden maskeren.

#### <a name="scaleout"></a>Uitschalen

[Vooraf toegewezen poorten](#preallocatedports) zijn toegewezen op basis van de grootte van de back-end-groep en onderverdeeld in lagen om onderbreking te minimaliseren als deel van de poorten moet opnieuw worden toegewezen om te voldoen aan de volgende grotere back-end-pool grootte laag.  Mogelijk hebt u een optie de intensiteit van het gebruik van SNAT-poort voor een bepaalde front-end verbeteren door het schalen van uw back-endpool naar de maximale grootte voor een bepaalde laag.  Dit is vereist voor de toepassing efficiënt uitschalen.

2 virtuele machines in de back-endpool zou bijvoorbeeld 1024 SNAT poorten die per IP-configuratie, zodat u een totaal van 2048 SNAT poorten voor de implementatie beschikbaar.  Als de implementatie worden verhoogd tot 50 virtuele kunnen machines, zelfs als het aantal poorten blijft constante per virtuele machine, een totaal van 51,200 (50 x 1024) vooraf toegewezen SNAT poorten worden gebruikt door de implementatie.  Als u uitbreiden van uw implementatie wilt, controleert u het aantal [vooraf toegewezen poorten](#preallocatedports) per laag om te controleren of u uw scale-out naar het maximum voor de desbetreffende laag vorm.  In het voorgaande voorbeeld, als u had gekozen voor de schaal vergroten tot 51 in plaats van 50 instanties voortgang u naar de volgende laag en het einde van met minder SNAT poorten per VM ook zoals in totaal.

Als u de schaal vergroten tot de volgende grotere back-end-pool grootte laag, is er mogelijk voor een aantal uitgaande verbindingen met time-out als toegewezen poorten moeten opnieuw worden toegewezen.  Als u slechts enkele van de poorten SNAT, is uitschalen over de grootte van de volgende grotere back-end-groep niet van belang zijn.  De helft van de bestaande poorten wordt opnieuw telkens wanneer u naar de volgende laag van de back-end-groep worden toegewezen.  Als u niet dat dit wilt om te worden toegepast, moet u de vorm van uw implementatie om de laaggrootte van de.  Of zorg ervoor dat uw toepassing kan detecteren en probeer dit nodig is.  TCP-keepalives kan helpen in detecteren wanneer SNAT niet langer functie poorten vanwege opnieuw wordt toegewezen.

### <a name="idletimeout"></a>Keepalives gebruiken om te herstellen van de uitgaande time-out voor inactiviteit

Uitgaande verbindingen hebben een time-out voor inactiviteit 4 minuten. Deze time-out is niet aanpasbaar. U kunt echter transport (bijvoorbeeld TCP keepalives) of de toepassingslaag keepalives gebruiken vernieuwen van een niet-actieve stroom en opnieuw instellen van deze time-out voor inactiviteit indien nodig.  

Als u TCP keepalives gebruikt, is het voldoende zijn om ze op een-zijde van de verbinding. Bijvoorbeeld, is het voldoende om ze op de server alleen de timer voor inactiviteit van de stroom opnieuw in te schakelen en is niet die nodig zijn voor beide zijden gestart TCP keepalives.  Dezelfde concepten bestaan voor het niveau van toepassing, met inbegrip van database client / server-configuraties.  Controleer de serverzijde voor welke opties beschikbaar voor specifieke keepalives van toepassing.

## <a name="discoveroutbound"></a>Het openbare IP-adres dat gebruikmaakt van een virtuele machine detecteren
Er zijn veel manieren om te bepalen van het openbare IP-bronadres van een uitgaande verbinding. OpenDNS is een service waarmee u het openbare IP-adres van uw virtuele machine kunt weergeven. 

Met behulp van de nslookup-opdracht, kunt u een DNS-query voor de naam myip.opendns.com verzenden naar de conflictoplosser OpenDNS. De service retourneert de bron-IP-adres dat is gebruikt voor het verzenden van de query. Wanneer u de volgende query vanaf uw virtuele machine uitvoeren, is het antwoord het openbare IP-adres gebruikt voor die VM:

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventoutbound"></a>Zo wordt voorkomen dat uitgaande connectiviteit
Soms is het ongewenste voor een virtuele machine moet worden toegestaan om een uitgaande stroom te maken. Of er mogelijk een vereiste voor het beheren van welke doelen kunnen worden bereikt met uitgaande stromen, of welke bestemmingen kunnen beginnen met het inkomende stromen. In dit geval kunt u [netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) voor het beheren van de doelen die de virtuele machine kan bereiken. U kunt nsg's ook gebruiken voor het beheren van welke openbare bestemming inkomende stromen kan initiëren.

Wanneer u een Netwerkbeveiligingsgroep op een VM met load balancing toepast, aandacht aan de [servicetags](../virtual-network/security-overview.md#service-tags) en [standaard beveiligingsregels](../virtual-network/security-overview.md#default-security-rules). U moet ervoor zorgen dat de virtuele machine health test aanvragen van Azure Load Balancer ontvangen kan. 

Als een NSG health test aanvragen van de code van de standaard AZURE_LOADBALANCER blokkeert, de statustest van uw virtuele machine is mislukt en de virtuele machine is gemarkeerd omlaag. Load Balancer reageert nieuwe stromen te verzenden naar die virtuele machine.

## <a name="limitations"></a>Beperkingen
- DisableOutboundSnat is niet beschikbaar als een optie bij het configureren van een regel in de portal voor taakverdeling.  Gebruik hulpprogramma's voor REST, sjabloon of -client in plaats daarvan.
- Webwerkrollen zonder dat u een VNet en andere Microsoft-platform-services kunnen worden geopend als alleen een interne standaardversie van Load Balancer wordt gebruikt vanwege een neveneffect van hoe het pre-VNet-services en andere platform functie services. U moet niet afhankelijk zijn van dit neveneffect als de desbetreffende service zelf of het onderliggende platform kan zonder kennisgeving worden gewijzigd. U moet altijd aannemen dat u moet expliciet uitgaande verbinding maken indien gewenst bij het gebruik van een interne Standard Load Balancer alleen. De [SNAT standaard](#defaultsnat) scenario 3 wordt beschreven in dit artikel is niet beschikbaar.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Load Balancer](load-balancer-overview.md).
- Meer informatie over [Standard Load Balancer](load-balancer-standard-overview.md).
- Meer informatie over [netwerkbeveiligingsgroepen](../virtual-network/security-overview.md).
- Informatie over een aantal van de andere sleutel [netwerkmogelijkheden die](../networking/networking-overview.md) in Azure.
