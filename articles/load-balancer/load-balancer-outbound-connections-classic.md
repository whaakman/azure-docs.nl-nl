---
title: Uitgaande verbindingen in Azure (klassiek) | Microsoft Docs
description: Dit artikel wordt uitgelegd hoe Azure biedt de mogelijkheid cloudservices om te communiceren met services met openbare internet.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2018
ms.author: kumud
ms.openlocfilehash: 5cb0647148d2cd90ad4cce6e16de30b72fff8429
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219661"
---
# <a name="outbound-connections-classic"></a>Uitgaande verbindingen (klassiek)

Azure biedt uitgaande connectiviteit voor implementaties van klanten via meerdere verschillende mechanismen. Dit artikel wordt beschreven wat de scenario's zijn, wanneer ze van toepassing zijn, hoe deze werken en hoe deze te beheren.

>[!NOTE]
>Dit artikel behandelt alleen voor klassieke implementaties.  Beoordeling [uitgaande verbindingen](load-balancer-outbound-connections.md) voor alle Resource Manager-implementatiescenario's in Azure.

Een implementatie in Azure kan communiceren met eindpunten buiten Azure in de openbare IP-adresruimte. Wanneer een exemplaar een uitgaande stroom naar een bestemming in de openbare IP-adresruimte initieert, Azure dynamisch privé IP-adres toegewezen aan een openbaar IP-adres. Nadat u deze toewijzing is gemaakt, kan retourverkeer voor deze uitgaande afkomstige stroom ook bereiken het privé IP-adres waarvan de stroom afkomstig is.

Azure maakt gebruik van bron-NAT (SNAT) om uit te voeren van deze functie. Wanneer meerdere persoonlijke IP-adressen zijn zich achter een enkel openbaar IP-adres, gebruikt Azure [netwerkadresomzetting (PAT)-poort](#pat) aan het privé IP-adressen zich voordoen. Kortstondige poorten worden gebruikt voor PAT en [vooraf toegewezen](#preallocatedports) op basis van de grootte van de opslaggroep.

Er zijn meerdere [scenario's voor uitgaande](#scenarios). U kunt deze scenario's kunt combineren, indien nodig. Bekijk deze zorgvuldig voor meer informatie over de mogelijkheden, beperkingen en patronen als ze van toepassing op uw implementatiemodel zijn en het toepassingsscenario. Bekijk de richtlijnen voor [beheren van deze scenario's](#snatexhaust).

## <a name="scenarios"></a>Scenario-overzicht

Azure biedt drie verschillende methoden voor het bereiken van uitgaande connectiviteit klassieke implementaties.  Niet alle klassieke implementaties hebben alle drie scenario's die voor hen beschikbaar:

| Scenario | Methode | IP-protocollen | Beschrijving | Web-Werkrol | IaaS | 
| --- | --- | --- | --- | --- | --- |
| [1. VM met een Instance Level Public IP-adres](#ilpip) | SNAT, poort zich niet gebruikt. | TCP, UDP, ICMP, ESP | Azure maakt gebruik van het openbare IP-adres toegewezen virtuele Machine. Het exemplaar heeft alle kortstondige poorten die beschikbaar zijn. | Nee | Ja |
| [2. openbare load balancing-eindpunt](#publiclbendpoint) | SNAT met poort onechte (PAT) naar het openbare eindpunt | TCP, UDP | Azure heeft het openbare eindpunt van openbare IP-adres met meerdere privé-eindpunten. Azure maakt gebruik van kortstondige poorten van het openbare eindpunt voor PAT. | Ja | Ja |
| [3. Zelfstandige virtuele machine ](#defaultsnat) | SNAT met poort onechte (PAT) | TCP, UDP | Azure automatisch een openbaar IP-adres voor SNAT aanwijst, deelt dit openbare IP-adres met de volledige implementatie, en gebruikt kortstondige poorten van de IP-adres van het openbare eindpunt voor PAT. Dit is een alternatief scenario voor het voorgaande scenario's. Wordt niet aanbevolen als u nodig, zichtbaarheid en controle hebt. | Ja | Ja |

Dit is een subset van de uitgaande verbinding-functionaliteit is beschikbaar voor implementaties in Azure Resource Manager.  

Verschillende implementaties in de klassieke versie hebben verschillende functionaliteit:

| Klassieke implementatie | Functionaliteit die beschikbaar is | 
| --- | --- |
| Virtuele machine | scenario [1](#ilpip), [2](#publiclbendpoint), of [3](#defaultsnat) |
| Web-Werkrol | alleen scenario [2](#publiclbendpoint), [3](#defaultsnat) | 

[Strategieën voor risicobeperking](#snatexhaust) hebben ook de dezelfde verschillen.

De [algoritme dat wordt gebruikt voor het vooraf toewijzen van kortstondige poorten](#ephemeralports) voor PAT voor klassieke implementaties dezelfde als die voor implementaties van Azure Resource Manager-resource is.

### <a name="ilpip"></a>Scenario 1: Virtuele machine met een Instance Level Public IP-adres

In dit scenario wordt heeft de virtuele machine een exemplaar van het niveau van openbare IP (ILPIP) zijn toegewezen. Wat betreft uitgaande verbindingen zijn, het maakt niet uit of de virtuele machine heeft een eindpunt met gelijke of niet. In dit scenario heeft voorrang op de andere. Wanneer een ILPIP wordt gebruikt, wordt de virtuele machine de ILPIP gebruikt voor alle uitgaande stromen.  

Een openbare IP die zijn toegewezen aan een virtuele machine is een 1:1-relatie (plaats 1:many) en geïmplementeerd als een stateless 1:1 NAT bevinden.  Poort onechte (PAT) wordt niet gebruikt en de virtuele machine heeft alle kortstondige poorten beschikbaar voor gebruik.

Als uw toepassing veel uitgaande stromen initieert en u SNAT poortuitputting ondervindt, kunt u overwegen een [ILPIP SNAT beperkingen beperken](#assignilpip). Beoordeling [SNAT beheren uitputting](#snatexhaust) in zijn geheel toe.

### <a name="publiclbendpoint"></a>Scenario 2: Openbare load balancing-eindpunt

In dit scenario zijn de virtuele machine of de Web-Werkrol is gekoppeld aan een openbaar IP-adres via het eindpunt met load balancing. De virtuele machine heeft geen openbaar IP-adres toegewezen. 

Wanneer de VM met load balancing een uitgaande stroom maakt, wordt het privé IP-bronadres van de uitgaande stroom naar het openbare IP-adres van het openbare eindpunt met gelijke omgezet in Azure. Azure maakt gebruik van SNAT om uit te voeren van deze functie. Azure gebruikt ook [PAT](#pat) naar meerdere privé IP-adressen achter een openbaar IP-adres zich voordoen. 

Kortstondige poorten van de load balancer openbare IP-adres frontend worden gebruikt om te onderscheiden van afzonderlijke stromen afkomstig van de virtuele machine. Dynamisch maakt gebruik van SNAT [vooraf toegewezen kortstondige poorten](#preallocatedports) wanneer uitgaande stromen worden gemaakt. In deze context worden de kortstondige poorten die voor SNAT SNAT poorten genoemd.

SNAT poorten zijn vooraf toegewezen zoals beschreven in de [Understanding SNAT en PAT](#snat) sectie. Zijn in feite een eindige resource die kan worden verbruikt. Het is belangrijk om te weten hoe ze zijn [verbruikt](#pat). Als u wilt weten hoe u voor dit verbruik ontwerpen en te beperken indien nodig, Bekijk [SNAT beheren uitputting](#snatexhaust).

Wanneer [meerdere openbare taakverdeling eindpunten](load-balancer-multivip.md) bestaat, een van deze openbare IP-adressen, een [kandidaat voor uitgaande stromen](#multivipsnat), en een willekeurig is geselecteerd.  

### <a name="defaultsnat"></a>Scenario 3: Er is geen openbare IP-adres dat is gekoppeld

In dit scenario wordt maakt de virtuele machine of Worker-Webrol geen deel uit van een openbare load balancing-eindpunt.  En in het geval van een virtuele machine, het heeft geen een ILPIP-adres is toegewezen. Wanneer de virtuele machine een uitgaande stroom maakt, zet Azure privé IP-bronadres van de uitgaande stroom aan een openbare IP-adres. Het openbare IP-adres gebruikt voor deze uitgaande stroom kan niet worden geconfigureerd en worden niet meegeteld in openbare IP-resource-limiet van het abonnement.  Dit adres wordt automatisch toegewezen door Azure.

Azure maakt gebruik van SNAT met poort onechte ([PAT](#pat)) om uit te voeren van deze functie. In dit scenario is vergelijkbaar met [scenario 2](#lb), maar er geen controle over het IP-adres gebruikt is. Dit is een alternatief scenario voor wanneer scenario 1 en 2 niet bestaan. In dit scenario wordt niet aanbevolen als u wilt dat de controle over het uitgaande adres. Als u uitgaande verbindingen zijn een belangrijk onderdeel van uw toepassing, moet u een ander scenario hebt gekozen.

SNAT poorten zijn vooraf toegewezen zoals beschreven in de [Understanding SNAT en PAT](#snat) sectie.  Het aantal virtuele machines of Webwerkrollen delen van het openbare IP-adres bepaalt het aantal kortstondige poorten genoemd vooraf toegewezen.   Het is belangrijk om te weten hoe ze zijn [verbruikt](#pat). Als u wilt weten hoe u voor dit verbruik ontwerpen en te beperken indien nodig, Bekijk [SNAT beheren uitputting](#snatexhaust).

## <a name="snat"></a>Inzicht krijgen in SNAT en PAT

### <a name="pat"></a>Poort onechte SNAT (PAT)

Wanneer een implementatie een uitgaande verbinding maakt, wordt elke bron uitgaande verbindingen herschreven. De bron is van de privé IP-adresruimte herschreven naar het openbare IP-adres dat is gekoppeld aan de implementatie (op basis van scenario's zoals hierboven beschreven). In de openbare IP-adresruimte, moet de 5-tuple van de stroom (bron-IP-adres, bronpoort, transportprotocol IP, doel-IP-adres, doelpoort) uniek zijn.  

Kortstondige poorten (SNAT) worden gebruikt om dit te bereiken nadat het herschrijven van de persoonlijke bron-IP-adres, omdat meerdere stromen afkomstig van een enkel openbaar IP-adres zijn. 

Een poort met SNAT wordt verbruikt per overdracht naar een enkel IP-adres, poort en protocol. Voor meerdere stromen naar de doel-IP-adres, poort en protocol verbruikt elke stroom één SNAT-poort. Dit zorgt ervoor dat de stromen uniek zijn als ze afkomstig uit hetzelfde openbare IP-adres zijn en gaat u naar het hetzelfde doel-IP-adres, poort en protocol. 

Meerdere stromen, elk op een andere doel-IP-adres, poort en protocol, delen een enkel SNAT-poort. De doel-IP-adres, poort en protocol maken stromen uniek zonder de noodzaak voor extra bron poorten te onderscheiden van stromen in de openbare IP-adresruimte.

Wanneer SNAT poort resources verbruikt, wordt uitgaande stromen mislukken totdat bestaande stromen SNAT poorten. Load Balancer maakt SNAT poorten wanneer de stroom wordt gesloten en maakt gebruik van een [time-out voor inactiviteit van 4 minuten durende](#idletimeout) voor het vrijmaken van SNAT poorten van niet-actieve stromen.

Voor patronen om te beperken van voorwaarden die vaak tot uitputting van SNAT poort leiden, raadpleegt u de [SNAT beheren](#snatexhaust) sectie.

### <a name="preallocatedports"></a>Tijdelijke poort voorafgaande toewijzing voor de onechte SNAT (PAT)-poort

Azure maakt gebruik van een algoritme om te bepalen het aantal vooraf toegewezen SNAT poorten die beschikbaar zijn op basis van de grootte van de back-endpool bij het gebruik van poort onechte SNAT ([PAT](#pat)). SNAT poorten zijn kortstondige poorten die beschikbaar zijn voor een bepaalde openbare IP-bronadres.

Azure preallocates SNAT poorten wanneer een exemplaar is geïmplementeerd op basis van hoeveel exemplaren van virtuele machine of Werkrol Web een opgegeven openbare IP-adres delen.  Wanneer uitgaande stromen worden gemaakt, [PAT](#pat) dynamisch verbruikt (tot de vooraf toegewezen limiet) en deze poorten worden vrijgegeven wanneer de stroom wordt gesloten of [inactief time-outs](#ideltimeout) gebeuren.

De volgende tabel toont de preallocations SNAT poort voor de lagen van de back-end poolgrootten:

| Exemplaren | Vooraf toegewezen SNAT poorten per exemplaar |
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |

Houd er rekening mee dat het aantal beschikbare poorten op de SNAT geen rechtstreeks naar het aantal stromen vertaalt. Één poort zijn SNAT kan worden hergebruikt voor meerdere unieke doelen. Poorten die worden verbruikt alleen indien nodig stromen om uniek te maken. Voor richtlijnen voor ontwerp en risicobeperking, raadpleegt u de sectie over [over het beheren van deze resource onuitputtelijk](#snatexhaust) en de sectie waarin wordt beschreven [PAT](#pat).

Wijzigen van de grootte van uw implementatie mogelijk van invloed op enkele van uw bestaande stromen. Als de grootte van de back-end-groep verhoogt en in de volgende laag overgangen, wordt de helft van de vooraf toegewezen SNAT poorten vrijgemaakt tijdens de overgang naar de volgende laag voor grotere back-end-pool. Stromen die gekoppeld aan een geregenereerde SNAT poort zijn time-out en moeten opnieuw tot stand worden gebracht. Als een nieuwe stroom wordt uitgevoerd, wordt de stroom onmiddellijk slaagt, zolang vooraf toegewezen poorten beschikbaar zijn.

Als de implementatiegrootte van de wordt verlaagd en in een lagere laag verandert, verhoogt het aantal beschikbare poorten van SNAT. In dit geval SNAT poorten bestaande toegewezen en hun respectieve stromen worden niet beïnvloed.

Als een service in de cloud wordt opnieuw geïmplementeerd of gewijzigd, de infrastructuur mogelijk tijdelijk de back-endpool worden maximaal twee keer zo groot als werkelijke rapporteren en Azure wordt op zijn beurt wijzen minder SNAT poorten per exemplaar dan verwacht.  Hierdoor kan de kans op SNAT poortuitputting tijdelijk toenemen. Uiteindelijk de poolgrootte op werkelijke grootte veranderen en Azure automatisch verhoogd vooraf toegewezen SNAT poorten op het verwachte aantal aan de hand van de bovenstaande tabel.  Dit gedrag is inherent aan het ontwerp en kan niet worden geconfigureerd.

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
[Kortstondige poorten](#preallocatedports) gebruikt voor [PAT](#pat) vormen een onuitputtelijk resource, zoals beschreven in [geen openbaar IP-adres dat is gekoppeld](#defaultsnat) en [openbaar eindpunt met gelijke](#publiclbendpoint).

Als u weet dat u bij het initiëren van veel uitgaande TCP of UDP-verbindingen op hetzelfde IP-doeladres en poort, en u observeren uitgaande verbindingen mislukken of door het ondersteuningsteam van wordt aangeraden dat u uitput SNAT-poorten bent (vooraf toegewezen [kortstondige poorten](#preallocatedports) die worden gebruikt door [PAT](#pat)), hebt u verschillende algemene risicobeperkende mogelijkheden. Bekijk deze opties en bepaal wat beschikbaar is en voor uw scenario het meest geschikt is. Het is mogelijk dat een of meer kan het beheer van dit scenario.

Als u problemen bij het begrijpen van het gedrag van de uitgaande verbindingen ondervindt, kunt u IP-stack-statistieken (netstat). Of het gedrag van de verbinding met behulp van pakketopnamen in acht nemen handig kan zijn.

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
Uw scenario toewijzen van een ILPIP verandert [Instance Level Public IP aan een virtuele machine](#ilpip). Alle kortstondige poorten van het openbare IP-adres die worden gebruikt voor elke virtuele machine zijn beschikbaar voor de virtuele machine. (In plaats van scenario's waarbij kortstondige poorten van een openbaar IP-adres worden gedeeld met alle virtuele machines die zijn gekoppeld aan de betreffende implementatie.) Er zijn wisselwerking om te overwegen, zoals de potentiële impact van opname in de whitelist een groot aantal afzonderlijke IP-adressen.

>[!NOTE] 
>Deze optie is niet beschikbaar voor webwerkrollen.

### <a name="idletimeout"></a>Keepalives gebruiken om te herstellen van de uitgaande time-out voor inactiviteit

Uitgaande verbindingen hebben een time-out voor inactiviteit 4 minuten. Deze time-out is niet aanpasbaar. U kunt echter transport (bijvoorbeeld TCP keepalives) of de toepassingslaag keepalives gebruiken vernieuwen van een niet-actieve stroom en opnieuw instellen van deze time-out voor inactiviteit indien nodig.  Neem contact op met de leverancier van alle verpakte software op of dit wordt ondersteund of het inschakelen ervan.  In het algemeen moet alleen een-zijde keepalives als u de time-out voor inactiviteit opnieuw wilt genereren. 

## <a name="discoveroutbound"></a>Het openbare IP-adres dat gebruikmaakt van een virtuele machine detecteren
Er zijn veel manieren om te bepalen van het openbare IP-bronadres van een uitgaande verbinding. OpenDNS is een service waarmee u het openbare IP-adres van uw virtuele machine kunt weergeven. 

Met behulp van de nslookup-opdracht, kunt u een DNS-query voor de naam myip.opendns.com verzenden naar de conflictoplosser OpenDNS. De service retourneert de bron-IP-adres dat is gebruikt voor het verzenden van de query. Wanneer u de volgende query vanaf uw virtuele machine uitvoeren, is het antwoord het openbare IP-adres gebruikt voor die VM:

    nslookup myip.opendns.com resolver1.opendns.com


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Load Balancer](load-balancer-overview.md) gebruikt in implementaties van Resource Manager.
- Meer informatie over [uitgaande verbindingen](load-balancer-outbound-connections.md) scenario's die beschikbaar zijn in Resource Manager-implementaties.
