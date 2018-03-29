---
title: Uitgaande verbindingen in Azure | Microsoft Docs
description: Dit artikel wordt uitgelegd hoe de Azure VM's om te communiceren met het openbare internetservices kunt.
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
ms.date: 03/21/2018
ms.author: kumud
ms.openlocfilehash: 990abc5c4e546d72d093bcd9e8f37932e93cbeb4
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="outbound-connections-in-azure"></a>Uitgaande verbindingen in Azure

Azure biedt uitgaande verbindingen voor implementaties van klanten via diverse verschillende methoden. Dit artikel wordt beschreven wat de scenario's zijn, wanneer ze van toepassing zijn, hoe deze werken en hoe deze te beheren.

>[!NOTE] 
>Dit artikel behandelt alleen implementaties van Resource Manager. Bekijk [uitgaande verbindingen (klassiek)](load-balancer-outbound-connections-classic.md) voor alle klassieke implementatiescenario's in Azure.

Een Azure-implementatie kan communiceren met eindpunten buiten Azure in de openbare IP-adresruimte. Wanneer een exemplaar een uitgaande stroom naar een bestemming in de openbare IP-adresruimte initieert, Azure de privé IP-adres dynamisch toegewezen aan een openbaar IP-adres. Nadat deze toewijzing is gemaakt, kan return verkeer voor deze uitgaande oorsprong verkeersstroom ook bereiken de privé IP-adres waarvan de stroom afkomstig is.

Azure gebruikt bron netwerkadresomzetting (snat omzetten) om deze functie niet uitvoeren. Wanneer u meerdere particuliere IP-adressen zijn zich voordoet achter één openbaar IP-adres, Azure gebruikt [poort netwerkadresomzetting (PAT)](#pat) naar zich voordoen privé IP-adressen. Kortstondige poorten worden gebruikt voor PAT en zijn [vooraf toegewezen](#preallocatedports) op basis van de grootte van de groep van toepassingen.

Er zijn meerdere [uitgaande scenario's](#scenarios). U kunt deze scenario's kunt combineren, indien nodig. Deze zorgvuldig om te begrijpen van de mogelijkheden, beperkingen en patronen, aangezien deze van toepassing op uw implementatiemodel te controleren en toepassingsscenario. Bekijk de richtlijnen voor [het beheren van deze scenario's](#snatexhaust).

>[!IMPORTANT] 
>Standaard Load Balancer introduceert nieuwe mogelijkheden en ander gedrag aan uitgaande verbinding.   Bijvoorbeeld: [scenario 3](#defaultsnat) bestaat niet bij een interne standaard Load Balancer aanwezig is en verschillende stappen dienen te worden genomen.   Zorgvuldig door dit hele document voor informatie over de algemene concepten en verschillen tussen SKU's.

## <a name="scenarios"></a>Scenario-overzicht

Azure Load Balancer en verwante resources expliciet worden gedefinieerd als u [Azure Resource Manager](#arm).  Azure biedt momenteel drie verschillende methoden om uitgaande verbinding voor Azure Resource Manager-bronnen te bereiken. 

| Scenario | Methode | Beschrijving |
| --- | --- | --- |
| [1. Virtuele machine met een Instance Level Public IP-adres (met of zonder Load Balancer)](#ilpip) | Snat omzetten, poort zich voordoet niet gebruikt. |Azure maakt gebruik van het openbare IP-adres toegewezen aan de IP-configuratie van het exemplaar NIC. Het exemplaar heeft alle kortstondige poorten beschikbaar. |
| [2. Openbare Load Balancer die zijn gekoppeld aan een virtuele machine (geen Instance Level Public IP-adres op het exemplaar)](#lb) | Snat omzetten met poort (PAT) zich voordoet met behulp van de Load Balancer-frontends |Het openbare IP-adres van de openbare Load Balancer frontends deelt Azure met meerdere particuliere IP-adressen. Kortstondige poorten van de frontends naar PAT maakt gebruik van Azure. |
| [3. Standalone VM (Er is geen Load Balancer, geen Instance Level Public IP-adres)](#defaultsnat) | Snat omzetten met poort zich voordoet (PAT) | Azure automatisch een openbaar IP-adres aanwijst voor snat omzetten, dit openbare IP-adres deelt met meerdere particuliere IP-adressen van de beschikbaarheidsset en kortstondige poorten van dit openbare IP-adres gebruikt. Dit is een alternatieve scenario voor de bovenstaande scenario's. Wordt niet aanbevolen het als u nodig hebt zichtbaarheid en controle. |

Als u niet dat een virtuele machine wilt te communiceren met eindpunten buiten Azure in openbare IP-adresruimte, kunt u netwerkbeveiligingsgroepen (nsg's) om de toegang zo nodig te blokkeren. De sectie [belemmeren de uitgaande verbinding](#preventoutbound) nsg's vindt u meer details. Hulp bij het ontwerpen, implementeren en beheren van een virtueel netwerk zonder eventuele uitgaande toegang valt buiten het bereik van dit artikel.

### <a name="ilpip"></a>Scenario 1: VM met een Instance Level Public IP-adres

In dit scenario heeft de virtuele machine een exemplaar niveau openbare IP (ILPIP) toegewezen. Zo ver mogelijk uitgaande verbindingen betreft, het maakt niet uit of de virtuele machine verdeeld wordt, of niet. Dit scenario heeft voorrang op de andere. Wanneer een ILPIP wordt gebruikt, gebruikt de virtuele machine de ILPIP voor alle uitgaande stromen.  

Een openbare IP-toegewezen aan een virtuele machine is een 1:1 relatie (plaats 1:many) en geïmplementeerd als een stateless 1:1 NAT bevinden.  Poort onechte (PAT) wordt niet gebruikt en de virtuele machine heeft alle kortstondige poorten beschikbaar voor gebruik.

Als uw toepassing veel uitgaande stromen initieert en u snat omzetten poort uitputting ervaren, kunt u toewijzen van een [ILPIP te verhelpen snat omzetten beperkingen](#assignilpip). Bekijk [beheren snat omzetten uitputting](#snatexhaust) in zijn geheel.

### <a name="lb"></a>Scenario 2: Netwerktaakverdeling VM zonder een Instance Level Public IP-adres

In dit scenario is de virtuele machine deel uitmaakt van een openbare Load Balancer back-endpool. De virtuele machine heeft geen een openbaar IP-adres toegewezen. De Load Balancer-bron moet worden geconfigureerd met een load balancer-regel om een koppeling tussen het openbare IP-frontend met de back-endpool te maken.

Als u deze regelconfiguratie niet uitvoert, is het gedrag zoals beschreven in het scenario voor [Standalone VM met geen Instance Level Public IP](#defaultsnat). Het is niet nodig zijn voor de regel een listener werken in de groep back-end voor de test van de status mislukt hebben.

Wanneer de VM Netwerktaakverdeling een uitgaande stroom maakt, vertaalt Azure het persoonlijke bron-IP-adres van de uitgaande stroom voor het openbare IP-adres van de openbare Load Balancer-frontend. Azure gebruikt snat omzetten om deze functie niet uitvoeren. Azure gebruikt ook [PAT](#pat) naar meerdere particuliere IP-adressen achter een openbaar IP-adres zich voordoen. 

Kortstondige poorten van de load balancer openbare IP-adres frontend worden gebruikt om te onderscheiden van afzonderlijke stromen afkomstig is van het door de virtuele machine. Snat omzetten dynamisch gebruikt [vooraf toegewezen kortstondige poorten](#preallocatedports) wanneer uitgaande stromen worden gemaakt. In deze context worden de kortstondige poorten gebruikt voor snat omzetten snat omzetten poorten genoemd.

Snat omzetten poorten zijn vooraf toegewezen zoals beschreven in de [Understanding snat omzetten en PAT](#snat) sectie. Ze zijn een eindige resource die kan worden verbruikt. Het is belangrijk te weten hoe ze zijn [verbruikt](#pat). Om te begrijpen hoe u voor dit verbruik ontwerpen en zo nodig te verhelpen, Bekijk [beheren snat omzetten uitputting](#snatexhaust).

Wanneer [meerdere (openbare) IP-adressen zijn gekoppeld aan een Load Balancer Basic](load-balancer-multivip-overview.md), een van deze openbare IP-adressen zijn een [geschikt is voor uitgaande stromen](#multivipsnat), en een is geselecteerd.  

Voor het bewaken van de status van uitgaande verbindingen met Load Balancer Basic, kunt u [Log Analytics voor Load Balancer](load-balancer-monitor-log.md) en [waarschuwing gebeurtenislogboeken](load-balancer-monitor-log.md#alert-event-log) om te controleren op snat omzetten poort uitputting van berichten.

### <a name="defaultsnat"></a>Scenario 3: Standalone VM zonder een Instance Level Public IP-adres

In dit scenario wordt de virtuele machine maakt geen deel uit van een openbare Load Balancer-toepassingen (en geen deel uit van een interne standaard Load Balancer-adresgroep) en heeft geen een ILPIP-adres toegewezen. Wanneer de virtuele machine een uitgaande stroom maakt, vertaalt Azure het persoonlijke bron-IP-adres van de uitgaande stroom aan een openbare IP-adres. Het openbare IP-adres voor deze uitgaande stroom geldt kan niet worden geconfigureerd en niet ten opzichte van het abonnement openbare IP-resource limiet.

>[!IMPORTANT] 
>Dit scenario geldt ook wanneer __alleen__ een interne basis Load Balancer is gekoppeld. Scenario 3 is __niet beschikbaar__ wanneer er een interne standaard Load Balancer is gekoppeld aan een virtuele machine.  U moet expliciet maken [scenario 1](#ilpip) of [scenario 2](#lb) naast het gebruik van een interne standaard Load Balancer.

Azure maakt gebruik van snat omzetten met poort zich voordoet ([PAT](#pat)) voor deze functie. Dit scenario is vergelijkbaar met [scenario 2](#lb), maar er geen controle over het IP-adres gebruikt is. Dit is een alternatieve scenario voor wanneer scenario 1 en 2 bestaan niet. Dit scenario wordt niet aanbevolen als u controle over het uitgaande adres wilt. Als uitgaande verbindingen zijn een belangrijk onderdeel van uw toepassing, moet u een ander scenario hebt gekozen.

Snat omzetten poorten zijn vooraf toegewezen zoals beschreven in de [Understanding snat omzetten en PAT](#snat) sectie.  Het aantal virtuele machines delen van een Beschikbaarheidsset bepaalt welke laag voorafgaande toewijzing is van toepassing.  Een zelfstandige virtuele machine, zonder een Beschikbaarheidsset is in feite een pool van 1 voor de vaststelling van de voorafgaande toewijzing (1024 snat omzetten poorten). Snat omzetten poorten zijn een eindige resource die kan worden verbruikt. Het is belangrijk te weten hoe ze zijn [verbruikt](#pat). Om te begrijpen hoe u voor dit verbruik ontwerpen en zo nodig te verhelpen, Bekijk [beheren snat omzetten uitputting](#snatexhaust).

### <a name="combinations"></a>Scenario's met meerdere, gecombineerde

U kunt de scenario's beschreven in de voorgaande secties voor een bepaalde uitkomst combineren. Als er meerdere scenario's aanwezig zijn, een volgorde van prioriteit is van toepassing: [scenario 1](#ilpip) heeft voorrang op [scenario 2](#lb) en [3](#defaultsnat). [Scenario 2](#lb) overschrijft [scenario 3](#defaultsnat).

Een voorbeeld is de implementatie van een Azure Resource Manager waar de toepassing is sterk afhankelijk van uitgaande verbindingen via een beperkt aantal bestemmingen maar ook via een Load Balancer-frontend ontvangt inkomende stromen. In dit geval kunt u scenario 1 en 2 voor vrijstelling combineren. Raadpleeg voor meer patronen [beheren snat omzetten uitputting](#snatexhaust).

### <a name="multife"></a> Meerdere frontends voor uitgaande stromen

#### <a name="load-balancer-standard"></a>Standaardversie van Load Balancer

Load Balancer standaard maakt gebruik van alle kandidaten voor uitgaande stromen op hetzelfde tijdstip waarop [meerdere (openbare) IP-frontends](load-balancer-multivip-overview.md) aanwezig is. Elke frontend vermenigvuldigt het aantal beschikbare poorten van vooraf toegewezen snat omzetten als een load-balancingregel is ingeschakeld voor uitgaande verbindingen.

U kunt onderdrukken een frontend-IP-adres wordt gebruikt voor uitgaande verbindingen met een nieuwe load balancing regeloptie:

```json    
      "loadBalancingRules": [
        {
          "disableOutboundSnat": false
        }
      ]
```

Normaal gesproken deze optie wordt standaard ingesteld op _false_ en geeft aan of uitgaande snat omzetten voor de bijbehorende virtuele machines in de back-endpool van de taakverdelingsregel-programma's met deze regel.  Dit kan worden gewijzigd in _true_ de in de back-endpool van deze regel voor taakverdeling om te voorkomen dat de Load Balancer met behulp van het bijbehorende frontend-IP-adres voor uitgaande verbindingen voor de virtuele machine.  En u kunt ook nog steeds een specifiek IP-adres voor uitgaande stromen aanwijzen zoals beschreven in [meerdere scenario's gecombineerd](#combinations) ook.

#### <a name="load-balancer-basic"></a>Load Balancer Basic

Load Balancer Basic kiest een enkele frontend moet worden gebruikt voor uitgaande stromen wanneer [meerdere (openbare) IP-frontends](load-balancer-multivip-overview.md) kandidaten zijn voor uitgaande stromen. Deze selectie kan niet worden geconfigureerd en moet u rekening houden met het algoritme selectie een willekeurige. U kunt een specifiek IP-adres voor uitgaande stromen aanwijzen, zoals beschreven in [meerdere scenario's gecombineerd](#combinations).

### <a name="az"></a> Beschikbaarheid Zones

Wanneer u [standaard Load Balancer met beschikbaarheid Zones](load-balancer-standard-availability-zones.md), zone-redundante frontends zone-redundante uitgaande snat omzetten verbindingen kan voorzien en snat omzetten programmering blijft zone-fout.  Wanneer zonal frontends worden gebruikt, delen uitgaande verbindingen met snat omzetten gedrag met de zone waartoe ze behoren.

## <a name="snat"></a>Wat zijn snat omzetten en PAT

### <a name="pat"></a>Poort onechte snat omzetten (PAT)

Wanneer een resource met openbare Load Balancer gekoppeld aan VM-instanties is, wordt elke bron uitgaande verbinding herschreven. De bron is van het virtuele netwerk persoonlijke IP-adresruimte herschreven naar de frontend openbare IP-adres van de load balancer. De 5-tuple van stroom (IP-bronadres, bronpoort, IP-protocol (transport), IP-doeladres, doelpoort) moet uniek zijn in de openbare IP-adresruimte.  

Kortstondige poorten (snat omzetten) worden gebruikt om dit te bereiken nadat het herschrijven van de persoonlijke IP-bronadres, omdat meerdere stromen afkomstig van één openbaar IP-adres zijn. 

Één snat omzetten poort wordt per overdracht voor een enkel IP-adres, poort en protocol gebruikt. Voor meerdere stromen met dezelfde IP-doeladres, poort en protocol verbruikt elke flow één poort snat omzetten. Dit zorgt ervoor dat de stromen wanneer ze afkomstig uit hetzelfde openbare IP-adres zijn en gaat u naar het hetzelfde doel-IP-adres, poort en protocol uniek zijn. 

Meerdere stromen, elk op een andere bestemming IP-adres, poort en protocol, delen één poort snat omzetten. De doel-IP-adres, poort en protocol maken stromen uniek zonder dat hiervoor extra bron nodig poorten te onderscheiden van stromen in het openbare IP-adresruimte.

Wanneer snat omzetten poort resources zijn uitgeput, mislukken, uitgaande stromen totdat bestaande stromen release snat omzetten poorten. Taakverdeling vrijgekomen snat omzetten poorten wanneer de stroom wordt gesloten en maakt gebruik van een [time-out voor inactiviteit van 4 minuten](#idletimeout) voor het vrijmaken van snat omzetten poorten uit niet-actieve stromen.

Bekijk voor patronen te verhelpen voorwaarden die vaak tot uitputting van de poort snat omzetten leiden de [beheren snat omzetten](#snatexhaust) sectie.

### <a name="preallocatedports"></a>Tijdelijke poort voorafgaande toewijzing voor de poort die zich voordoet snat omzetten (PAT)

Azure maakt gebruik van een algoritme om te bepalen het aantal vooraf toegewezen snat omzetten beschikbare poorten op basis van de grootte van de back-endpool bij gebruik van poort onechte snat omzetten ([PAT](#pat)). Snat omzetten poorten zijn kortstondige poorten beschikbaar voor een bepaalde openbare IP-bronadres.

Hetzelfde aantal snat omzetten poorten zijn vooraf toegewezen voor UDP en TCP respectievelijk en onafhankelijk per IP-transportprotocol worden verbruikt. 

>[!IMPORTANT]
>Standaard SKU SNAT programming per IP-transportprotocol en afgeleid van de load-balancingregel.  Als er slechts een taakverdelingsregel voor TCP bestaat, is snat omzetten alleen beschikbaar voor TCP. Als u alleen een TCP-taakverdelingsregel hebt en uitgaande snat omzetten voor UDP, maakt u een UDP-regel op basis van dezelfde frontend dezelfde back-end-opslaggroep voor taakverdeling.  Dit activeert de domeincontroller voor het programmeren voor UDP snat omzetten.  Een werkende regel of health test is niet vereist.  Basic SKU SNAT programma altijd snat omzetten voor beide IP-protocol (transport), ongeacht het protocol-transport is opgegeven in de taakverdelingsregel.

Azure preallocates snat omzetten poorten voor de IP-adresconfiguratie van de NIC van elke virtuele machine. Wanneer een IP-configuratie wordt toegevoegd aan de groep, worden de poorten snat omzetten, vooraf voor deze IP-configuratie op basis van de grootte van de back-end-groep toegewezen. Wanneer uitgaande stromen worden gemaakt, [PAT](#pat) dynamisch verbruikt (maximaal de vooraf toegewezen limiet) en deze poorten worden vrijgegeven wanneer de stroom wordt gesloten of [inactief time-outs](#ideltimeout) gebeuren.

De volgende tabel toont de snat omzetten poort preallocations voor lagen van back-end-pool grootten:

| Poolgrootte (VM-exemplaren) | Vooraf toegewezen snat omzetten poorten per IP-configuratie|
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1,000 | 32 |

>[!NOTE]
> Bij gebruik van standaard Load Balancer is met [meerdere frontends](load-balancer-multivip-overview.md), [elk frontend-IP-adres vermenigvuldigt u het aantal beschikbare poorten met snat omzetten](#multivipsnat) in de vorige tabel. Bijvoorbeeld, een back-endpool van 50 van de virtuele machine met 2 regels voor taakverdeling, elk met een afzonderlijke frontend-IP-adressen, 2048 (2 x 1024) snat omzetten poorten per IP-configuratie gebruikt. Zie details voor [meerdere frontends](#multife).

Houd er rekening mee dat het aantal beschikbare poorten voor snat omzetten niet rechtstreeks aan het aantal gegevensoverdrachten wordt omgezet. Een losse poort zijn snat omzetten kan opnieuw worden gebruikt voor meerdere unieke bestemmingen. Poorten die worden verbruikt alleen als het is nodig om stromen uniek te maken. Voor de ontwerp- en risicobeperking, Raadpleeg het gedeelte over [het beheren van deze resource onuitputtelijk](#snatexhaust) en de sectie waarin wordt beschreven [PAT](#pat).

Het wijzigen van de grootte van uw back endpool mogelijk van invloed zijn op uw tot stand gebrachte stromen. Als de grootte van de back-end-groep verhoogt en overgangen die in de volgende laag, wordt de helft van de vooraf toegewezen snat omzetten poorten vrijgemaakt tijdens de overgang naar de volgende groter back-end-pool laag. Stromen die gekoppeld aan een geregenereerde snat omzetten poort zijn time-out en moeten opnieuw worden gemaakt. Als u een nieuwe stroom wordt uitgevoerd, slaagt de stroom onmiddellijk zolang vooraf toegewezen poorten beschikbaar zijn.

Als de back-end-poolgrootte afneemt en verhoogt de overgangen in een lagere laag, het aantal beschikbare poorten met snat omzetten. In dit geval snat omzetten poorten bestaande toegewezen en hun respectieve stromen worden niet beïnvloed.

Snat omzetten poorten toewijzingen zijn specifieke IP-transportprotocol (TCP en UDP worden afzonderlijk onderhouden) en de volgende voorwaarden worden vrijgegeven:

### <a name="tcp-snat-port-release"></a>SNAT TCP-poort release

- Als beide serverclient FIN/ACK verzendt, wordt poort snat omzetten vrijgegeven na 240 seconden.
- Als een eerste wordt weergegeven, wordt poort snat omzetten vrijgegeven na 15 seconden.
- time-out voor inactiviteit is bereikt.

### <a name="udp-snat-port-release"></a>SNAT UDP-poort release

- time-out voor inactiviteit is bereikt.

## <a name="problemsolving"></a> Het oplossen van problemen 

Deze sectie is bedoeld om te beperken snat omzetten uitputting en andere scenario's die zich met uitgaande verbindingen in Azure voordoen kunnen.

### <a name="snatexhaust"></a> Het beheren van uitputting van de poort snat omzetten (PAT)
[Kortstondige poorten](#preallocatedports) gebruikt voor [PAT](#pat) zijn een onuitputtelijk resource, zoals beschreven in [Standalone VM zonder een Instance Level Public IP-adres](#defaultsnat) en [taakverdeling VM zonder een Exemplaar van niveau openbare IP-adres](#lb).

Als u weet dat u bij het initiëren van veel uitgaande TCP of UDP-verbindingen met dezelfde IP-adres en poort, en u zien uitgaande verbindingen mislukken of door ondersteuning wordt aangeraden dat je put snat omzetten poorten (vooraf toegewezen [kortstondige poorten](#preallocatedports) die wordt gebruikt door [PAT](#pat)), hebt u verschillende mogelijkheden voor algemene risicobeperking. Bekijk deze opties en bepalen wat beschikbaar is en beste voor uw scenario. Het is mogelijk dat een of meer kunnen helpen bij dit scenario.

Als u hebt met het gedrag van de uitgaande verbinding problemen, kunt u IP-stack-statistieken (netstat). Of kan het handig om te observeren verbinding gedrag met behulp van pakket opnamen zijn. U kunt deze opnamen pakket uitvoeren in het gastbesturingssysteem van uw exemplaar of [netwerk-Watcher voor pakketopname](../network-watcher/network-watcher-packet-capture-manage-portal.md).

#### <a name="connectionreuse"></a>Wijzigen van de toepassing opnieuw verbindingen gebruiken 
U kunt de vraag naar kortstondige poorten die worden gebruikt voor snat omzetten door verbindingen in uw toepassing opnieuw te verminderen. Dit geldt met name voor protocollen zoals HTTP/1.1 waar hergebruik van verbindingen de standaardinstelling is. En andere protocollen die HTTP als transportmechanisme gebruiken (bijvoorbeeld REST gebruiken) op zijn beurt kunnen profiteren. 

Hergebruik is altijd beter dan afzonderlijke, atomic TCP-verbindingen voor elke aanvraag. Hergebruik resulteert in meer zodat, zeer efficiënte TCP transacties.

#### <a name="connection pooling"></a>De toepassing te gebruiken verbindingsgroepering wijzigen
U kunt gebruikmaken van een groepsgewijze schema in uw toepassing, waarin aanvragen intern zijn verdeeld over een vaste set van (elke hergebruiken indien mogelijk)-verbindingen. Dit schema Hiermee beperkt u het aantal kortstondige poorten in gebruik en maakt u een beter voorspelbaar omgeving. Dit schema kunt ook de doorvoer van aanvragen door meerdere gelijktijdige bewerkingen toe wanneer één verbinding is geblokkeerd voor het antwoord van een bewerking te verhogen.  

Verbindingsgroepering mogelijk bestaat al in het kader die u gebruikt voor het ontwikkelen van uw toepassing of de configuratie-instellingen voor uw toepassing. U kunt combineren verbindingsgroepering met hergebruik van verbindingen. Uw meerdere aanvragen wordt vervolgens een vaste, voorspelbare aantal poorten naar hetzelfde doel-IP-adres en poort gebruiken. De aanvragen worden ook profiteren van efficiënt gebruik van TCP-transacties latentie en resource-gebruik verminderen. UDP-transacties kunnen ook profiteren, omdat het beheren van het aantal UDP-overdrachten kunt op zijn beurt uitlaatgas voorwaarden voorkomen en beheren van het gebruik van de poort snat omzetten.

#### <a name="retry logic"></a>Wijzigen van de toepassing minder agressieve Pogingslogica gebruiken
Wanneer [vooraf toegewezen kortstondige poorten](#preallocatedports) gebruikt voor [PAT](#pat) zijn uitgeput, of een toepassing storingen optreden, agressieve of brute force pogingen zonder decay en backoff logica leiden tot uitputting optreden of behouden blijven. Aanvraag voor tijdelijke poorten kunt u met behulp van een minder agressieve Pogingslogica verminderen. 

Kortstondige poorten hebben een 4 minuten inactiviteit (niet-aanpasbare). Als de pogingen te agressief, heeft de uitputting geen mogelijkheid om op te ruimen op zichzelf. Daarom is overwegen hoe-- en hoe vaak--uw toepassing opnieuw transacties probeert een belangrijk onderdeel van het ontwerp.

#### <a name="assignilpip"></a>Een exemplaar niveau openbare IP-adres toewijzen aan elke virtuele machine
Toewijzen van een ILPIP verandert uw scenario naar [Instance Level Public IP op een virtuele machine](#ilpip). Alle tijdelijke het openbare IP-poorten die worden gebruikt voor elke virtuele machine zijn beschikbaar voor de virtuele machine. (In plaats van scenario's waarbij kortstondige poorten van een openbare IP-adres worden gedeeld met alle virtuele machines die zijn gekoppeld aan de betreffende back-endpool.) Er zijn verschillen rekening moet houden, zoals de extra kosten van het openbare IP-adressen en de impact van door een groot aantal afzonderlijke IP-adressen.

>[!NOTE] 
>Deze optie is niet beschikbaar voor web-werkrollen.

#### <a name="multifesnat"></a>Meerdere frontends gebruiken

Wanneer u openbare standaard Load Balancer, die u toewijst [meerdere frontend-IP-adressen voor uitgaande verbindingen](#multife) en [Vermenigvuldig het aantal beschikbare poorten voor snat omzetten](#preallocatedports).  U moet een frontend-IP-configuratie, de regel en de back-endpool voor het activeren van het programmeren van snat omzetten naar het openbare IP-adres van de frontend maken.  De regel niet hoeft te werken en een health test hoeft niet te laten slagen.  Als u gebruik maakt van meerdere frontends voor binnenkomend verkeer ook (in plaats van alleen voor uitgaand), moet u aangepaste statuscontroles goed om te verzekeren van betrouwbaarheid.

>[!NOTE]
>In de meeste gevallen is vormen van uitputting van de poorten snat omzetten in een teken van slecht ontwerp.  Zorg ervoor dat u weten waarom u put poorten zijn voordat u meer frontends met snat omzetten poorten toevoegen.  Kan worden dat maskering een probleem, wat tot mislukte later leiden kan weergegeven.

#### <a name="scaleout"></a>Uitschalen

[Vooraf toegewezen poorten](#preallocatedports) zijn toegewezen op basis van de grootte van de groep back-end en onderverdeeld in lagen onderbreking minimaliseren wanneer een deel van de poorten opnieuw worden toegewezen moet aan de volgende groter back-end groep grootte laag.  Mogelijk hebt u een optie intensiteit snat omzetten poort gebruik van een bepaalde frontend verbeteren door het schalen van uw back-endpool tot maximale grootte voor een bepaalde laag.  Dit is vereist voor de toepassing uit te schalen efficiënt.

2 virtuele machines in de back-endpool zou bijvoorbeeld 1024 snat omzetten poorten beschikbaar per IP-configuratie, zodat u een totaal van 2048 snat omzetten poorten voor de implementatie.  Als de implementatie worden verhoogd tot 50 virtuele kunnen machines, zelfs als het aantal poorten blijft constante per virtuele machine, een totaal van 51,200 (50 x 1024) vooraf toegewezen snat omzetten poorten worden gebruikt door de implementatie.  Als u uitbreiden van uw implementatie wilt, controleert u het aantal [vooraf toegewezen poorten](#preallocatedports) per laag om te controleren of u uw scale-out op het maximum voor de respectieve laag vorm.  In het voorgaande voorbeeld, als u had gekozen moet worden uitgebreid tot en met 51 in plaats van 50 exemplaren uw voortgang naar de volgende laag en einde van met minder snat omzetten poorten per VM ook als in totaal.

Als u daarentegen scale-out op de volgende groter back-end groep grootte laag mogelijk uitgaande verbindingen als toegewezen poorten moeten opnieuw worden toegewezen.  Als u niet dat dit wilt te kunnen uitvoeren, moet u de vorm van uw implementatie om de laaggrootte van de.  Of zorg ervoor dat uw toepassing kunt detecteren en opnieuw proberen indien nodig.  TCP-keepalives kan helpen in detecteren wanneer snat omzetten niet langer functie poorten vanwege opnieuw wordt toegewezen.

### <a name="idletimeout"></a>Keepalives gebruiken om in te stellen van de uitgaande time-out voor inactiviteit

Uitgaande verbindingen hebben een niet-actieve time-out van 4 minuten. Deze time-out is niet aanpasbaar. U kunt echter transport (bijvoorbeeld TCP keepalives) of toepassingslaag keepalives gebruiken een niet-actieve stroom vernieuwd en deze niet-actieve time-out opnieuw instellen, indien nodig.  

Bij gebruik van TCP-keepalives volstaat zodat ze een-zijde van de verbinding. Bijvoorbeeld, voldoende zodat ze op de server alleen om in te stellen de timer voor inactiviteit van de stroom is en het is niet nodig zijn voor beide kanten op geïnitieerd TCP keepalives.  Vergelijkbare concepten bestaan voor de toepassingslaag, waaronder configuraties van de client / server-database.  Controleer aan de serverzijde voor welke opties beschikbaar voor specifieke keepalives van toepassing.

## <a name="discoveroutbound"></a>Het openbare IP-adres die gebruikmaakt van een virtuele machine detecteren
Er zijn veel manieren om te bepalen van de bron van openbare IP-adres van een uitgaande verbinding. OpenDNS biedt een service waarmee u het openbare IP-adres van uw virtuele machine kunt weergeven. 

Met behulp van de opdracht nslookup, kunt u een DNS-query voor de naam myip.opendns.com verzenden naar de OpenDNS-resolver. De service retourneert het bron-IP-adres dat is gebruikt voor het verzenden van de query. Wanneer u de volgende query uit uw virtuele machine uitvoert, is het antwoord het openbare IP-adres gebruikt voor die VM:

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventoutbound"></a>Zo wordt voorkomen dat uitgaande verbinding
Soms is het ongewenste voor een virtuele machine moet worden toegestaan voor het maken van een uitgaande stroom. Mogelijk is er een vereiste voor het beheren van welke doelen kunnen worden bereikt met uitgaande stromen of welke bestemmingen inkomende stromen kunnen beginnen. In dit geval kunt u [netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) voor het beheren van de doelen die de virtuele machine kan bereiken. U kunt nsg's ook gebruiken om te beheren welke openbare bestemming inkomende stromen kan initiëren. 

Wanneer u een NSG aan een VM taakverdeling toepast, moet u aandacht schenken aan de [standaard labels](../virtual-network/virtual-networks-nsg.md#default-tags) en [regels standaard](../virtual-network/virtual-networks-nsg.md#default-rules). U moet ervoor zorgen dat de virtuele machine van Azure Load Balancer health test aanvragen kan ontvangen. 

Als een NSG health test aanvragen van het label van de standaard AZURE_LOADBALANCER blokkeert, uw VM health test mislukt en de virtuele machine niet actief is gemarkeerd. Load Balancer reageert nieuwe stromen verzenden naar die virtuele machine.

## <a name="limitations"></a>Beperkingen
- DisableOutboundSnat is niet beschikbaar als een optie bij het configureren van een regel in de portal voor taakverdeling.  Gebruik in plaats daarvan REST, sjabloon of client hulpprogramma's.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Load Balancer](load-balancer-overview.md).
- Meer informatie over [standaard Load Balancer](load-balancer-standard-overview.md).
- Meer informatie over [netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md).
- Informatie over een aantal van de andere sleutel [netwerkmogelijkheden](../networking/networking-overview.md) in Azure.
