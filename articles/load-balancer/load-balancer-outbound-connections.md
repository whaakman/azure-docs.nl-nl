---
title: Inzicht in uitgaande verbindingen in Azure | Microsoft Docs
description: Dit artikel wordt uitgelegd hoe de Azure VM's om te communiceren met het openbare internetservices kunt.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/05/2018
ms.author: kumud
ms.openlocfilehash: e1a2b4c281194ec4c70e4d9fe1bc97c5aa61436e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="understanding-outbound-connections-in-azure"></a>Uitleg over uitgaande verbindingen in Azure

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]


Azure biedt uitgaande verbindingen voor implementaties van klanten via diverse verschillende methoden.  Dit artikel wordt beschreven wat de scenario's zijn, wanneer ze van toepassing zijn, hoe deze werken en hoe deze te beheren.

Een Azure-implementatie kan communiceren met eindpunten buiten Azure in openbare IP-adresruimte. Wanneer een exemplaar een uitgaande stroom naar een bestemming in openbare IP-adresruimte initieert, Azure de privé IP-adres dynamisch toegewezen aan een openbaar IP-adres.  Zodra deze toewijzing is gemaakt, kan return verkeer voor deze uitgaande oorsprong verkeersstroom ook bereiken de privé IP-adres waarvan de stroom afkomstig is.

Azure gebruikt bron netwerkadresomzetting (snat omzetten) om deze functie niet uitvoeren.  Wanneer u meerdere particuliere IP-adressen zijn zich voordoet achter één openbaar IP-adres, Azure gebruikt [poort netwerkadresomzetting (PAT)](#pat) naar zich voordoen privé IP-adressen.  Kortstondige poorten worden gebruikt voor PAT en zijn [vooraf toegewezen](#preallocatedports) op basis van de grootte van de groep van toepassingen.

Er zijn meerdere [uitgaande scenario's](#scenarios). Deze scenario's kunnen worden gecombineerd, indien nodig. Deze zorgvuldig om te begrijpen van de mogelijkheden, beperkingen en patronen, aangezien deze van toepassing op uw implementatiemodel te controleren en toepassingsscenario.  Bekijk de richtlijnen voor [het beheren van deze scenario's](#snatexhaust).

## <a name="scenarios"></a>Scenario-overzicht

Azure heeft twee belangrijke implementatiemodellen: Azure Resource Manager en het klassieke model. Load Balancer en verwante resources expliciet zijn gedefinieerd wanneer u [Azure Resource Manager-resources](#arm).  Klassieke implementaties abstracte het concept van een load balancer en express een vergelijkbare functie via de definitie van de eindpunten van een [cloudservice](#classic). De toepasselijke [scenario's](#scenarios) voor uw implementatie afhankelijke op welke implementatie model wordt gebruikt.

### <a name="arm"></a>Azure Resource Manager

Azure biedt momenteel drie verschillende methoden om uitgaande verbinding voor Azure Resource Manager-bronnen te bereiken.  [Klassieke](#classic) implementaties hebben een subset van deze scenario's.

| Scenario | Methode | Beschrijving |
| --- | --- | --- |
| [1. Virtuele machine met Instance Level Public IP-adres (met of zonder load balancer)](#ilpip) | Snat omzetten, poort zich voordoet niet gebruikt. |Azure maakt gebruik van het openbare IP-adres toegewezen aan de IP-configuratie van het exemplaar NIC.  Het exemplaar heeft alle kortstondige poorten beschikbaar. |
| [2. Openbare Load Balancer die zijn gekoppeld aan virtuele machine (geen Instance Level Public IP-adres op exemplaar)](#lb) | Snat omzetten met poort (PAT) zich voordoet met behulp van de Load Balancer-frontend(s) |Azure deelt het openbare IP-adres van de openbare Load Balancer frontend(s) met meerdere persoonlijke IP-adressen en kortstondige poorten van de frontend(s) naar PAT gebruikt. |
| [3. Standalone VM (Er is geen load balancer, geen Instance Level Public IP-adres)](#defaultsnat) | Snat omzetten met poort zich voordoet (PAT) | Azure automatisch een openbaar IP-adres aanwijst voor snat omzetten, dit openbare IP-adres deelt met meerdere particuliere IP-adressen van de Beschikbaarheidsset en kortstondige poorten van dit openbare IP-adres gebruikt.  Dit scenario is een alternatieve scenario voor het voorgaande scenario's 1 en 2 en niet aanbevolen als u zichtbaarheid en controle moet. |

Als u niet dat een virtuele machine wilt te communiceren met eindpunten buiten Azure in openbare IP-adresruimte, kunt u Netwerkbeveiligingsgroep groepen (NSG) toegang zo nodig te blokkeren.  Met behulp van de nsg's wordt besproken in nader [belemmeren de uitgaande verbinding](#preventoutbound).  Ontwerp richtlijnen en implementatie details voor het ontwerpen en beheren van een virtueel netwerk, zonder eventuele uitgaande toegang buiten het bereik van dit artikel is.

### <a name="classic"></a>Klassieke (cloudservices)

De scenario's zijn beschikbaar voor klassieke implementaties zijn een subset van scenario's die beschikbaar zijn voor [Azure Resource Manager](#arm) implementaties en Load Balancer Basic.

Een klassieke virtuele Machine heeft de dezelfde drie belangrijke scenario's zoals beschreven voor Azure Resource Manager-resources ([1](#ilpip), [2](#lb), [3](#defaultsnat)). Een klassieke Worker Webrol heeft slechts twee scenario's ([2](#lb), [3](#defaultsnat)).  [Risicobeperking strategieën](#snatexhaust) hebben de dezelfde verschillen.

De algoritme die wordt gebruikt voor [het vooraf toewijzen kortstondige poorten](#ephemeralprots) voor PAT voor klassieke implementaties hetzelfde als voor implementaties van Azure Resource Manager-resource is.  

### <a name="ilpip"></a>Scenario 1: VM met een Instance Level Public IP-adres

In dit scenario heeft de virtuele machine een exemplaar niveau openbare IP (ILPIP) toegewezen. Zo ver mogelijk uitgaande verbindingen betreft, het maakt niet uit of de virtuele machine verdeeld wordt, of niet.  Dit scenario heeft voorrang op de andere. Wanneer een ILPIP wordt gebruikt, gebruikt de virtuele machine de ILPIP voor alle uitgaande stromen.  

Poort onechte (PAT) wordt niet gebruikt en de virtuele machine heeft alle kortstondige poorten beschikbaar voor gebruik.

Als uw toepassing veel uitgaande stromen initieert en snat omzetten poort bronuitputting treedt, kunt u overwegen toewijzen van een [ILPIP te verhelpen snat omzetten beperkingen](#assignilpip). Bekijk [beheren snat omzetten uitputting](#snatexhaust) deze volledig.

### <a name="lb"></a>Scenario 2: Netwerktaakverdeling VM zonder Instance Level Public IP-adres

In dit scenario is de virtuele machine deel uitmaakt van een openbare Load Balancer back-endpool.  De virtuele machine heeft geen een openbaar IP-adres toegewezen. De Load Balancer-bron moet worden geconfigureerd met een load balancer-regel om een koppeling tussen het openbare IP-frontend met de back-endpool te maken. Als u deze regelconfiguratie niet uitvoert, is het gedrag zoals beschreven in het scenario voor [Standalone VM met geen Instance Level Public IP](#defaultsnat).  Het is niet nodig zijn voor de regel moet een werkende listener in de back-endpool of de health-test mislukt.

Wanneer de VM Netwerktaakverdeling een uitgaande stroom maakt, vertaalt Azure het persoonlijke bron-IP-adres van de uitgaande stroom voor het openbare IP-adres van de openbare Load Balancer-frontend. Azure bron Network Address Translation (snat omzetten) gebruikt voor het uitvoeren van deze functie, evenals [poort netwerkadresomzetting (PAT)](#pat) naar meerdere particuliere IP-adressen achter een openbaar IP-adres zich voordoen. Kortstondige poorten van de Load Balancer openbare IP-adres frontend worden gebruikt om te onderscheiden van afzonderlijke stromen afkomstig is van het door de virtuele machine. Snat omzetten dynamisch gebruikt [vooraf toegewezen kortstondige poorten](#preallocatedports) wanneer uitgaande stromen worden gemaakt. In deze context worden de kortstondige poorten gebruikt voor snat omzetten aangeduid als snat omzetten poorten.

Snat omzetten poorten zijn vooraf toegewezen zoals beschreven in de [Understanding snat omzetten en PAT](#snat) gedeelte en een beperkte resource die kan worden uitgeput zijn. Het is belangrijk te weten hoe ze zijn [verbruikt](#pat). Bekijk [beheren snat omzetten uitputting](#snatexhaust) om te begrijpen hoe ontwerpen voor en zo nodig te verhelpen.

Wanneer [meerdere (openbare) IP-adressen zijn gekoppeld aan een Load Balancer Basic](load-balancer-multivip-overview.md), een van deze openbare IP-adressen zijn een [geschikt is voor uitgaande stromen en één is geselecteerd](#multivipsnat).  

U kunt [Log Analytics voor Load Balancer](load-balancer-monitor-log.md) en [waarschuwing gebeurtenislogboeken om te controleren op snat omzetten poort uitputting berichten](load-balancer-monitor-log.md#alert-event-log) status van uitgaande verbindingen met Load Balancer Basic worden gecontroleerd.

### <a name="defaultsnat"></a>Scenario 3: Standalone VM zonder Instance Level Public IP-adres

De virtuele machine maakt geen deel uit van een pool Load Balancer van Azure en heeft geen een Instance Level Public IP (ILPIP)-adres toegewezen. Wanneer de virtuele machine een uitgaande stroom maakt, vertaalt Azure het persoonlijke bron-IP-adres van de uitgaande stroom aan een openbare IP-adres. Het openbare IP-adres voor deze uitgaande stroom geldt kan niet worden geconfigureerd en niet ten opzichte van het abonnement openbare IP-resource limiet. Azure maakt gebruik van bron Network Address Translation (snat omzetten) met poort zich voordoet ([PAT](#pat)) voor deze functie. Dit scenario is vergelijkbaar met [scenario 2](#lb), maar er geen controle over het IP-adres gebruikt is.  Dit is een alternatieve scenario voor wanneer scenario's 1 en 2 scenario bestaan niet.  Dit scenario wordt niet aanbevolen als u controle over het uitgaande adres.

Snat omzetten poorten zijn vooraf toegewezen zoals beschreven in de [Understanding snat omzetten en PAT](#snat) gedeelte en een beperkte resource die kan worden uitgeput zijn. Het is belangrijk te weten hoe ze zijn [verbruikt](#pat). Bekijk [beheren snat omzetten uitputting](#snatexhaust) om te begrijpen hoe ontwerpen voor en zo nodig te verhelpen.

### <a name="combinations"></a>Scenario's met meerdere, gecombineerde

De scenario's beschreven in de voorgaande secties kunnen worden gecombineerd om te zorgen voor een bepaalde uitkomst.  Als er meerdere scenario's aanwezig zijn, een volgorde van prioriteit is van toepassing: [scenario 1](#ilpip) heeft voorrang op [scenario 2](#lb) en [3](#defaultsnat) (alleen Azure Resource-Manager), en [scenario 2](#lb) overschrijft [scenario 3](#defaultsnat) (Azure Resource Manager & klassiek).

Een voorbeeld is de implementatie van een Azure Resource Manager waar de toepassing is sterk afhankelijk van uitgaande verbindingen via een beperkt aantal bestemmingen maar ook via een Load Balancer-frontend ontvangt inkomende stromen. In dit geval kunt u scenario's 1 en 2 voor vrijstelling combineren.  Bekijk [beheren snat omzetten uitputting](#snatexhaust) voor extra patronen.

### <a name="multivipsnat"></a>Meerdere frontends voor uitgaande stromen

Load Balancer Basic kiest een enkele frontend gebruikte uitgaande stromen worden wanneer [meerdere (openbare) IP-frontends](load-balancer-multivip-overview.md) kandidaten zijn voor uitgaande stromen.  Deze selectie kan niet worden geconfigureerd en de selectie-algoritme moet worden beschouwd als willekeurige.  U kunt een specifiek IP-adres voor aanwijzen uitgaande zoals beschreven in [scenario's gecombineerd](#combinations).

## <a name="snat"></a>Wat zijn snat omzetten en PAT

### <a name="pat"></a>Poort onechte snat omzetten (PAT)

Wanneer een resource met openbare Load Balancer gekoppeld aan VM-instanties is, wordt elke bron uitgaande verbinding herschreven. De bron wordt herschreven van het virtuele netwerk persoonlijke IP-adresruimte in het front-openbare IP-adres van de load balancer. De 5-tuple van stroom (IP-bronadres, bronpoort, IP-protocol (transport), IP-doeladres, doelpoort) moet uniek zijn in de openbare IP-adresruimte.  Kortstondige poorten worden gebruikt om dit te bereiken nadat het herschrijven van de persoonlijke IP-bronadres omdat meerdere stromen afkomstig uit één openbaar IP-adres zijn.  Deze kortstondige poorten worden snat omzetten poorten genoemd. 

Één snat omzetten poort wordt per overdracht voor een enkel IP-adres, poort en protocol gebruikt. Voor meerdere stromen met dezelfde IP-doeladres, poort en protocol verbruikt elke flow één poort snat omzetten. Dit zorgt ervoor dat de stromen uniek wanneer hetzelfde openbare IP-adres voor hetzelfde doel-IP-adres, poort en protocol afkomstig zijn. 

Meerdere stromen, elk op een andere bestemming IP-adres, poort en protocol, deelt een losse poort zijn snat omzetten. De doel-IP-adres, poort en protocol maakt stromen uniek zonder de noodzaak van extra bron poorten die worden gebruikt om te onderscheiden van stromen in openbare IP-adresruimte.

Wanneer snat omzetten poort resources zijn uitgeput, mislukken uitgaande stromen tot snat omzetten poorten zijn vrijgegeven door bestaande stromen. Taakverdeling vrijgekomen snat omzetten poorten wanneer de stroom wordt gesloten en maakt gebruik van een [time-out voor inactiviteit van 4 minuten](#idletimeout) voor het vrijmaken van snat omzetten poorten uit niet-actieve stromen.

Controleer de [beheren snat omzetten](#snatexhaust) sectie naar patronen te verhelpen voorwaarden die vaak tot uitputting van de poort snat omzetten leiden.

### <a name="preallocatedports"></a>Tijdelijke poort voorafgaande toewijzing voor de poort die zich voordoet snat omzetten (PAT)

Azure maakt gebruik van een algoritme om te bepalen het aantal vooraf toegewezen snat omzetten beschikbare poorten op basis van de grootte van de back-endpool bij gebruik van poort onechte snat omzetten ([PAT](#pat)).  Snat omzetten poorten zijn kortstondige poorten beschikbaar voor een opgegeven openbare IP-bronadres.

Azure preallocates snat omzetten poorten voor de IP-adresconfiguratie van de NIC van elke virtuele machine. Wanneer een IP-configuratie wordt toegevoegd aan de groep, worden de poorten snat omzetten, vooraf voor deze IP-configuratie op basis van de grootte van de back-end-groep toegewezen. De toewijzing is voor klassieke Web werkrollen, per rolexemplaar.  Wanneer uitgaande stromen worden gemaakt, [PAT](#pat) dynamisch verbruikt (maximaal de vooraf toegewezen limiet) en deze poorten worden vrijgegeven wanneer de stroom wordt gesloten of [inactief time-outs](#ideltimeout).

De volgende tabel toont de preallocations snat omzetten poort voor het segment van de back-end-pool grootten:

| Poolgrootte (VM-exemplaren) | Vooraf toegewezen snat omzetten poorten per IP-configuratie|
| --- | --- |
| 1 - 50 | 1024 |
| 51 - 100 | 512 |
| 101 - 200 | 256 |
| 201 - 400 | 128 |
| 401 - 800 | 64 |
| 801 - 1,000 | 32 |

Het is belangrijk te weten dat het aantal beschikbare poorten voor snat omzetten niet rechtstreeks aan het aantal verbindingen wordt omgezet. Een losse poort zijn snat omzetten kan opnieuw worden gebruikt voor meerdere unieke bestemmingen. Poorten zijn alleen als het nodig zijn om ervoor te stromen unieke verbruikt. Raadpleeg [het beheren van deze resource onuitputtelijk](#snatexhaust) voor ontwerp en risicobeperking richtlijnen evenals de sectie beschrijven [PAT](#pat).

Het wijzigen van de grootte van uw back endpool mogelijk van invloed zijn sommige van uw tot stand gebrachte stromen. Als de back-end-poolgrootte verhoogt en overgangen die in de volgende laag, wordt de helft van de vooraf toegewezen snat omzetten poorten vrijgemaakt tijdens de overgang naar de volgende groter back-end-pool laag. Stromen die gekoppeld aan een geregenereerde snat omzetten poort zijn time-out en moeten opnieuw worden gemaakt. Nieuwe verbindingspogingen mislukt onmiddellijk zolang vooraf toegewezen poorten beschikbaar zijn.

Als de back-end-poolgrootte afneemt en in een lagere laag overgangen, verhoogt het aantal beschikbare poorten met snat omzetten. In dit geval snat omzetten poorten bestaande toegewezen en hun respectieve stromen worden niet beïnvloed.

## <a name="snatexhaust"></a>Het beheren van uitputting van de poort snat omzetten (PAT)

[Kortstondige poorten](#preallocatedports) gebruikt voor [PAT](#pat) zijn van een onuitputtelijk resource, zoals beschreven in [Standalone VM zonder Instance Level Public IP-adres](#defaultsnat) en [VM zonder netwerktaakverdeling Exemplaar van niveau openbare IP-adres](#lb).

Als u weet dat u tot stand brengen veel uitgaande TCP of UDP-verbindingen naar het hetzelfde doel-IP-adres en poort, en zien uitgaande verbindingen mislukken of door ondersteuning wordt aangeraden u put snat omzetten poorten zijn (vooraf toegewezen [kortstondige poorten](#preallocatedports)die wordt gebruikt door [PAT](#pat)), hebt u verschillende mogelijkheden voor algemene risicobeperking.  Bekijk deze opties en bepalen wat beschikbaar is en beste voor uw scenario.  Het is mogelijk een of meer kunnen helpen bij dit scenario.

Als u hebt met het gedrag van de uitgaande verbinding problemen, kunt u IP-stack-statistieken (netstat) of kan het handig om te observeren verbinding gedrag met behulp van pakket opnamen zijn.  U kunt deze opnamen pakket uitvoeren in het gastbesturingssysteem van uw exemplaar of [netwerk-Watcher voor pakketopname](../network-watcher/network-watcher-packet-capture-manage-portal.md).

### <a name="connectionreuse"></a>Toepassing hergebruiken verbindingen wijzigen 
U kunt de vraag naar kortstondige poorten gebruikt voor snat omzetten door verbindingen in uw toepassing opnieuw te verminderen.  Dit geldt met name voor protocollen HTTP/1.1 waarbij opnieuw verbinding wilt gebruiken, is de standaardwaarde.  En andere protocollen die via HTTP als transportmechanisme gebruiken (dat wil zeggen REST) op zijn beurt kunnen profiteren.  Hergebruik is altijd beter dan afzonderlijke, atomic TCP-verbindingen voor elke aanvraag en resulteert in meer zodat, zeer efficiënte TCP transacties.

### <a name="connection pooling"></a>Toepassing gebruiken verbindingsgroepering wijzigen
U kunt gebruikmaken van een groepsgewijze schema in uw toepassing, waarin aanvragen intern zijn verdeeld over een vaste set van (elke hergebruiken indien mogelijk)-verbindingen.  Deze beperkingen voor het aantal tijdelijke poorten in gebruik en maakt u een beter voorspelbaar omgeving.  Dit kunt ook de doorvoer van aanvragen door meerdere gelijktijdige bewerkingen toe wanneer één verbinding is geblokkeerd voor het antwoord van een bewerking te verhogen.  Verbindingsgroepering bestaat al in het kader die u gebruikt voor het ontwikkelen van uw toepassing of de configuratie-instellingen voor uw toepassing.  U kunt dit combineren met het hergebruik van verbindingen en uw meerdere aanvragen een vaste, voorspelbare aantal poorten naar het hetzelfde IP-adres en poort gebruiken terwijl het ook profiteren van zeer efficiënt gebruik van TCP-transacties, waardoor latentie en resource gebruik.  UDP-transacties kunnen ook profiteren omdat het beheren van het nummer van UDP stromen op zijn beurt uitlaatgas voorwaarden voorkomen en het beheer kunnen het gebruik van de poort snat omzetten.

### <a name="retry logic"></a>Wijzigen van de toepassing minder agressieve Pogingslogica gebruiken
Wanneer [vooraf toegewezen kortstondige poorten](#preallocatedports) gebruikt voor [PAT](#pat) zijn uitgeput, of een toepassing storingen optreden, agressieve of brute force pogingen zonder decay en backoff logica leiden tot uitputting optreden of behouden blijven. Aanvraag voor tijdelijke poorten kunt u met behulp van een minder agressieve Pogingslogica verminderen.   Kortstondige poorten hebben een 4 minuten inactiviteit (niet-aanpasbare) en als de pogingen te agressief, de uitputting heeft geen mogelijkheid om op te ruimen op zichzelf.  Overwegen hoe en hoe vaak uw toepassing opnieuw transacties probeert is daarom essentieel overweging van het ontwerp.

### <a name="assignilpip"></a>Een openbaar IP op exemplaarniveau toewijzen aan elke virtuele machine
Hiermee kunt u uw scenario naar [instantieniveau openbare IP-adres aan een VM](#ilpip).  Alle kortstondige poorten van het openbare IP-adres gebruikt voor elke virtuele machine zijn beschikbaar voor de virtuele machine (in plaats van scenario's waarbij kortstondige poorten van een openbare IP-adres worden gedeeld met alle van de VM gekoppeld aan de betreffende back-endpool).  Er zijn verschillen rekening moet houden, zoals extra kosten van openbare IP-adressen en de mogelijke invloed op door een groot aantal afzonderlijke IP-adressen.

>[!NOTE] 
>Deze optie is niet beschikbaar voor Web-werkrollen.

### <a name="idletimeout"></a>Keepalives gebruiken om in te stellen uitgaande time-out voor inactiviteit

Uitgaande verbindingen hebben een niet-actieve time-out van 4 minuten. Dit is geen aanpasbare. Echter, kunt u transport (dat wil zeggen TCP-keepalives) of application layer keepalives vernieuwen van een niet-actieve stroom en deze niet-actieve time-out opnieuw instellen als vereist.

## <a name="discoveroutbound"></a>Het openbare IP-adres gebruikt door een bepaalde virtuele machine detecteren
Er zijn veel manieren om te bepalen van de bron van openbare IP-adres van een uitgaande verbinding. OpenDNS biedt een service waarmee u het openbare IP-adres van uw virtuele machine kunt weergeven. Met de opdracht nslookup, kunt u een DNS-query voor de naam myip.opendns.com verzenden naar de OpenDNS-resolver. De service retourneert het bron-IP-adres dat is gebruikt voor het verzenden van de query. Als u de volgende query uit uw virtuele machine uitvoert, is het antwoord het openbare IP-adres gebruikt voor die VM.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventoutbound"></a>Zo wordt voorkomen dat uitgaande verbinding
Soms wordt byserverservice voor een virtuele machine moet worden toegestaan voor het maken van een uitgaande stroom of kan er een vereiste voor het beheren van welke doelen kunnen worden bereikt met uitgaande stromen of welke bestemmingen inkomende stromen kunnen beginnen. In dit geval gebruikt u [Netwerkbeveiligingsgroep groepen (NSG)](../virtual-network/virtual-networks-nsg.md) voor het beheren van de doelen die de virtuele machine kan ook bereiken als welke openbare bestemming inkomende stromen kunnen initiëren. Wanneer u een NSG aan een VM taakverdeling toepassen, moet u aandacht te besteden aan de [standaard labels](../virtual-network/virtual-networks-nsg.md#default-tags) en [regels standaard](../virtual-network/virtual-networks-nsg.md#default-rules).

U moet ervoor zorgen dat de virtuele machine van Azure Load Balancer health test aanvragen kan ontvangen. Als een NSG health test aanvragen van het label van de standaard AZURE_LOADBALANCER blokkeert, uw VM health test mislukt en de virtuele machine niet actief is gemarkeerd. Load Balancer reageert nieuwe stromen verzenden naar die virtuele machine.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Load Balancer Basic](load-balancer-overview.md).
- Meer informatie over [Netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md).
- Informatie over een aantal van de andere sleutel [netwerkmogelijkheden](../networking/networking-overview.md) in Azure.
