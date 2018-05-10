---
title: Uitgaande verbindingen in Azure (klassiek) | Microsoft Docs
description: Dit artikel wordt uitgelegd hoe Azure kunt cloudservices om te communiceren met het openbare internet-services.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2018
ms.author: kumud
ms.openlocfilehash: f6452d8f88b91fe0cbf144ce951b84ba4cec0047
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="outbound-connections-classic"></a>Uitgaande verbindingen (klassiek)

Azure biedt uitgaande verbindingen voor implementaties van klanten via diverse verschillende methoden. Dit artikel wordt beschreven wat de scenario's zijn, wanneer ze van toepassing zijn, hoe deze werken en hoe deze te beheren.

>[!NOTE]
>Dit artikel behandelt alleen voor klassieke implementaties.  Bekijk [uitgaande verbindingen](load-balancer-outbound-connections.md) voor alle implementatiescenario's van Resource Manager in Azure.

Een Azure-implementatie kan communiceren met eindpunten buiten Azure in de openbare IP-adresruimte. Wanneer een exemplaar een uitgaande stroom naar een bestemming in de openbare IP-adresruimte initieert, Azure de privé IP-adres dynamisch toegewezen aan een openbaar IP-adres. Nadat deze toewijzing is gemaakt, kan return verkeer voor deze uitgaande oorsprong verkeersstroom ook bereiken de privé IP-adres waarvan de stroom afkomstig is.

Azure gebruikt bron netwerkadresomzetting (snat omzetten) om deze functie niet uitvoeren. Wanneer u meerdere particuliere IP-adressen zijn zich voordoet achter één openbaar IP-adres, Azure gebruikt [poort netwerkadresomzetting (PAT)](#pat) naar zich voordoen privé IP-adressen. Kortstondige poorten worden gebruikt voor PAT en zijn [vooraf toegewezen](#preallocatedports) op basis van de grootte van de groep van toepassingen.

Er zijn meerdere [uitgaande scenario's](#scenarios). U kunt deze scenario's kunt combineren, indien nodig. Deze zorgvuldig om te begrijpen van de mogelijkheden, beperkingen en patronen, aangezien deze van toepassing op uw implementatiemodel te controleren en toepassingsscenario. Bekijk de richtlijnen voor [het beheren van deze scenario's](#snatexhaust).

## <a name="scenarios"></a>Scenario-overzicht

Azure biedt drie verschillende methoden om uitgaande verbinding klassieke implementaties bereiken.  Niet alle klassieke implementaties hebben alle drie scenario's die voor hen beschikbaar:

| Scenario | Methode | IP-protocollen | Beschrijving | Web-Werkrol | IaaS | 
| --- | --- | --- | --- | --- | --- |
| [1. Virtuele machine met een Instance Level Public IP-adres](#ilpip) | Snat omzetten, poort zich voordoet niet gebruikt. | TCP, UDP, ICMP, ESP | Azure maakt gebruik van het openbare IP-adres toegewezen virtuele Machine. Het exemplaar heeft alle kortstondige poorten beschikbaar. | Nee | Ja |
| [2. openbaar eindpunt voor netwerktaakverdeling](#publiclbendpoint) | Snat omzetten met poort zich voordoet (PAT) naar het openbare-eindpunt | TCP, UDP | Het openbare IP-adres openbaar eindpunt deelt Azure met meerdere persoonlijke eindpunten. Azure gebruikt kortstondige poorten van het openbare-eindpunt voor PAT. | Ja | Ja |
| [3. Zelfstandige VM ](#defaultsnat) | Snat omzetten met poort zich voordoet (PAT) | TCP, UDP | Azure automatisch een openbaar IP-adres aanwijst voor snat omzetten, deelt dit openbare IP-adres met de volledige implementatie en kortstondige poorten van het eindpunt voor openbare IP-adres voor PAT gebruikt. Dit is een alternatieve scenario voor de bovenstaande scenario's. Wordt niet aanbevolen het als u nodig hebt zichtbaarheid en controle. | Ja | Ja |

Dit is een subset van uitgaande verbinding functionaliteit beschikbaar voor implementaties van Resource Manager in Azure.  

Verschillende implementaties in de klassieke hebben verschillende functionaliteit:

| Klassieke implementatie | De functie is beschikbaar | 
| --- | --- |
| Virtuele machine | scenario [1](#ilpip), [2](#publiclbendpoint), of [3](#defaultsnat) |
| Web-Werkrol | alleen scenario [2](#publiclbendpoint), [3](#defaultsnat) | 

[Risicobeperking strategieën](#snatexhaust) hebben de dezelfde verschillen.

De [-algoritme dat wordt gebruikt voor het vooraf toewijzen kortstondige poorten](#ephemeralports) voor PAT voor klassieke implementaties hetzelfde als voor implementaties van Azure Resource Manager-resource is.

### <a name="ilpip"></a>Scenario 1: VM met een Instance Level Public IP-adres

In dit scenario heeft de virtuele machine een exemplaar niveau openbare IP (ILPIP) toegewezen. Zo ver mogelijk uitgaande verbindingen betreft, het maakt niet uit of de virtuele machine taakverdeling eindpunt of niet heeft. Dit scenario heeft voorrang op de andere. Wanneer een ILPIP wordt gebruikt, gebruikt de virtuele machine de ILPIP voor alle uitgaande stromen.  

Een openbare IP-toegewezen aan een virtuele machine is een 1:1 relatie (plaats 1:many) en geïmplementeerd als een stateless 1:1 NAT bevinden.  Poort onechte (PAT) wordt niet gebruikt en de virtuele machine heeft alle kortstondige poorten beschikbaar voor gebruik.

Als uw toepassing veel uitgaande stromen initieert en u snat omzetten poort uitputting ervaren, kunt u toewijzen van een [ILPIP te verhelpen snat omzetten beperkingen](#assignilpip). Bekijk [beheren snat omzetten uitputting](#snatexhaust) in zijn geheel.

### <a name="publiclbendpoint"></a>Scenario 2: Openbaar eindpunt voor netwerktaakverdeling

In dit scenario is de virtuele machine of Web-Werkrol gekoppeld aan een openbaar IP-adres via het eindpunt met gelijke taakverdeling. De virtuele machine heeft geen een openbaar IP-adres toegewezen. 

Wanneer de VM Netwerktaakverdeling een uitgaande stroom maakt, vertaalt Azure het persoonlijke bron-IP-adres van de uitgaande stroom het openbare IP-adres van het openbare eindpunt met gelijke taakverdeling. Azure gebruikt snat omzetten om deze functie niet uitvoeren. Azure gebruikt ook [PAT](#pat) naar meerdere particuliere IP-adressen achter een openbaar IP-adres zich voordoen. 

Kortstondige poorten van de load balancer openbare IP-adres frontend worden gebruikt om te onderscheiden van afzonderlijke stromen afkomstig is van het door de virtuele machine. Snat omzetten dynamisch gebruikt [vooraf toegewezen kortstondige poorten](#preallocatedports) wanneer uitgaande stromen worden gemaakt. In deze context worden de kortstondige poorten gebruikt voor snat omzetten snat omzetten poorten genoemd.

Snat omzetten poorten zijn vooraf toegewezen zoals beschreven in de [Understanding snat omzetten en PAT](#snat) sectie. Ze zijn een eindige resource die kan worden verbruikt. Het is belangrijk te weten hoe ze zijn [verbruikt](#pat). Om te begrijpen hoe u voor dit verbruik ontwerpen en zo nodig te verhelpen, Bekijk [beheren snat omzetten uitputting](#snatexhaust).

Wanneer [meerdere openbare taakverdeling eindpunten](load-balancer-multivip.md) bestaat, een van deze openbare IP-adressen een [geschikt is voor uitgaande stromen](#multivipsnat), en een willekeurig is geselecteerd.  

### <a name="defaultsnat"></a>Scenario 3: Er is geen openbaar IP-adres dat is gekoppeld

In dit scenario wordt maakt de virtuele machine of Web-Werkrol geen deel uit van een openbaar eindpunt met gelijke taakverdeling.  En in het geval van een virtuele machine, deze geen een ILPIP-adres toegewezen. Wanneer de virtuele machine een uitgaande stroom maakt, vertaalt Azure het persoonlijke bron-IP-adres van de uitgaande stroom aan een openbare IP-adres. Het openbare IP-adres voor deze uitgaande stroom geldt kan niet worden geconfigureerd en niet ten opzichte van het abonnement openbare IP-resource limiet.  Dit adres wordt automatisch toegewezen door Azure.

Azure maakt gebruik van snat omzetten met poort zich voordoet ([PAT](#pat)) voor deze functie. Dit scenario is vergelijkbaar met [scenario 2](#lb), maar er geen controle over het IP-adres gebruikt is. Dit is een alternatieve scenario voor wanneer scenario 1 en 2 bestaan niet. Dit scenario wordt niet aanbevolen als u controle over het uitgaande adres wilt. Als uitgaande verbindingen zijn een belangrijk onderdeel van uw toepassing, moet u een ander scenario hebt gekozen.

Snat omzetten poorten zijn vooraf toegewezen zoals beschreven in de [Understanding snat omzetten en PAT](#snat) sectie.  Het aantal virtuele machines of werkrollen Web-delen van het openbare IP-adres bepaalt het aantal vooraf toegewezen kortstondige poorten.   Het is belangrijk te weten hoe ze zijn [verbruikt](#pat). Om te begrijpen hoe u voor dit verbruik ontwerpen en zo nodig te verhelpen, Bekijk [beheren snat omzetten uitputting](#snatexhaust).

## <a name="snat"></a>Wat zijn snat omzetten en PAT

### <a name="pat"></a>Poort onechte snat omzetten (PAT)

Wanneer een implementatie een uitgaande verbinding maakt, wordt elke bron uitgaande verbinding herschreven. De bron opnieuw van de persoonlijke IP-adresruimte wordt geschreven naar het openbare IP-adres zijn gekoppeld aan de implementatie (op basis van scenario's die hierboven worden beschreven). De 5-tuple van stroom (IP-bronadres, bronpoort, IP-protocol (transport), IP-doeladres, doelpoort) moet uniek zijn in de openbare IP-adresruimte.  

Kortstondige poorten (snat omzetten) worden gebruikt om dit te bereiken nadat het herschrijven van de persoonlijke IP-bronadres, omdat meerdere stromen afkomstig van één openbaar IP-adres zijn. 

Één snat omzetten poort wordt per overdracht voor een enkel IP-adres, poort en protocol gebruikt. Voor meerdere stromen met dezelfde IP-doeladres, poort en protocol verbruikt elke flow één poort snat omzetten. Dit zorgt ervoor dat de stromen wanneer ze afkomstig uit hetzelfde openbare IP-adres zijn en gaat u naar het hetzelfde doel-IP-adres, poort en protocol uniek zijn. 

Meerdere stromen, elk op een andere bestemming IP-adres, poort en protocol, delen één poort snat omzetten. De doel-IP-adres, poort en protocol maken stromen uniek zonder dat hiervoor extra bron nodig poorten te onderscheiden van stromen in het openbare IP-adresruimte.

Wanneer snat omzetten poort resources zijn uitgeput, mislukken, uitgaande stromen totdat bestaande stromen release snat omzetten poorten. Taakverdeling vrijgekomen snat omzetten poorten wanneer de stroom wordt gesloten en maakt gebruik van een [time-out voor inactiviteit van 4 minuten](#idletimeout) voor het vrijmaken van snat omzetten poorten uit niet-actieve stromen.

Bekijk voor patronen te verhelpen voorwaarden die vaak tot uitputting van de poort snat omzetten leiden de [beheren snat omzetten](#snatexhaust) sectie.

### <a name="preallocatedports"></a>Tijdelijke poort voorafgaande toewijzing voor de poort die zich voordoet snat omzetten (PAT)

Azure maakt gebruik van een algoritme om te bepalen het aantal vooraf toegewezen snat omzetten beschikbare poorten op basis van de grootte van de back-endpool bij gebruik van poort onechte snat omzetten ([PAT](#pat)). Snat omzetten poorten zijn kortstondige poorten beschikbaar voor een bepaalde openbare IP-bronadres.

Azure preallocates snat omzetten poorten wanneer een exemplaar wordt geïmplementeerd op basis van hoeveel exemplaren van virtuele machine of Web-Werkrol een openbare IP-adres delen.  Wanneer uitgaande stromen worden gemaakt, [PAT](#pat) dynamisch verbruikt (maximaal de vooraf toegewezen limiet) en deze poorten worden vrijgegeven wanneer de stroom wordt gesloten of [inactief time-outs](#ideltimeout) gebeuren.

De volgende tabel toont de snat omzetten poort preallocations voor lagen van back-end-pool grootten:

| Exemplaren | Vooraf toegewezen snat omzetten poorten per exemplaar |
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |

Houd er rekening mee dat het aantal beschikbare poorten voor snat omzetten niet rechtstreeks aan het aantal gegevensoverdrachten wordt omgezet. Een losse poort zijn snat omzetten kan opnieuw worden gebruikt voor meerdere unieke bestemmingen. Poorten die worden verbruikt alleen als het is nodig om stromen uniek te maken. Voor de ontwerp- en risicobeperking, Raadpleeg het gedeelte over [het beheren van deze resource onuitputtelijk](#snatexhaust) en de sectie waarin wordt beschreven [PAT](#pat).

Het wijzigen van de grootte van uw implementatie mogelijk van invloed zijn op uw tot stand gebrachte stromen. Als de grootte van de back-end-groep verhoogt en overgangen die in de volgende laag, wordt de helft van de vooraf toegewezen snat omzetten poorten vrijgemaakt tijdens de overgang naar de volgende groter back-end-pool laag. Stromen die gekoppeld aan een geregenereerde snat omzetten poort zijn time-out en moeten opnieuw worden gemaakt. Als u een nieuwe stroom wordt uitgevoerd, slaagt de stroom onmiddellijk zolang vooraf toegewezen poorten beschikbaar zijn.

Als de implementatie wordt verkleind en verhoogt de overgangen in een lagere laag, het aantal beschikbare poorten met snat omzetten. In dit geval snat omzetten poorten bestaande toegewezen en hun respectieve stromen worden niet beïnvloed.

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
[Kortstondige poorten](#preallocatedports) gebruikt voor [PAT](#pat) zijn een onuitputtelijk resource, zoals beschreven in [geen openbare IP-adres dat is gekoppeld](#defaultsnat) en [openbaar eindpunt taakverdeling](#publiclbendpoint).

Als u weet dat u bij het initiëren van veel uitgaande TCP of UDP-verbindingen met dezelfde IP-adres en poort, en u zien uitgaande verbindingen mislukken of door ondersteuning wordt aangeraden dat je put snat omzetten poorten (vooraf toegewezen [kortstondige poorten](#preallocatedports) die wordt gebruikt door [PAT](#pat)), hebt u verschillende mogelijkheden voor algemene risicobeperking. Bekijk deze opties en bepalen wat beschikbaar is en beste voor uw scenario. Het is mogelijk dat een of meer kunnen helpen bij dit scenario.

Als u hebt met het gedrag van de uitgaande verbinding problemen, kunt u IP-stack-statistieken (netstat). Of kan het handig om te observeren verbinding gedrag met behulp van pakket opnamen zijn.

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
Toewijzen van een ILPIP verandert uw scenario naar [Instance Level Public IP op een virtuele machine](#ilpip). Alle tijdelijke het openbare IP-poorten die worden gebruikt voor elke virtuele machine zijn beschikbaar voor de virtuele machine. (In plaats van scenario's waarbij kortstondige poorten van een openbare IP-adres worden gedeeld met alle virtuele machines die zijn gekoppeld aan de betreffende implementatie.) Er zijn verschillen rekening moet houden, zoals de potentiële impact van door een groot aantal afzonderlijke IP-adressen.

>[!NOTE] 
>Deze optie is niet beschikbaar voor web-werkrollen.

### <a name="idletimeout"></a>Keepalives gebruiken om in te stellen van de uitgaande time-out voor inactiviteit

Uitgaande verbindingen hebben een niet-actieve time-out van 4 minuten. Deze time-out is niet aanpasbaar. U kunt echter transport (bijvoorbeeld TCP keepalives) of toepassingslaag keepalives gebruiken een niet-actieve stroom vernieuwd en deze niet-actieve time-out opnieuw instellen, indien nodig.  Neem contact op met de leverancier van ingepakte software op of dit wordt ondersteund of het inschakelen ervan.  In het algemeen moet alleen een-zijde keepalives als u de time-out voor inactiviteit opnieuw wilt genereren. 

## <a name="discoveroutbound"></a>Het openbare IP-adres die gebruikmaakt van een virtuele machine detecteren
Er zijn veel manieren om te bepalen van de bron van openbare IP-adres van een uitgaande verbinding. OpenDNS biedt een service waarmee u het openbare IP-adres van uw virtuele machine kunt weergeven. 

Met behulp van de opdracht nslookup, kunt u een DNS-query voor de naam myip.opendns.com verzenden naar de OpenDNS-resolver. De service retourneert het bron-IP-adres dat is gebruikt voor het verzenden van de query. Wanneer u de volgende query uit uw virtuele machine uitvoert, is het antwoord het openbare IP-adres gebruikt voor die VM:

    nslookup myip.opendns.com resolver1.opendns.com


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Load Balancer](load-balancer-overview.md) gebruikt in implementaties van Resource Manager.
- Modus voor informatie over [uitgaande verbinding](load-balancer-outbound-connections.md) scenario's die beschikbaar zijn in implementaties van Resource Manager.
