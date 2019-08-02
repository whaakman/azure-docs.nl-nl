---
title: Uitgaande verbindingen in azure (klassiek)
titlesuffix: Azure Load Balancer
description: In dit artikel wordt uitgelegd hoe u met Azure Cloud Services kunt communiceren met open bare Internet Services.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2018
ms.author: allensu
ms.openlocfilehash: 10af3b4838aae1565bac1d996997c117a74cedbc
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274670"
---
# <a name="outbound-connections-classic"></a>Uitgaande verbindingen (klassiek)

Azure biedt uitgaande connectiviteit voor klant implementaties via verschillende mechanismen. In dit artikel wordt beschreven wat de scenario's zijn, wanneer deze van toepassing zijn, hoe ze werken en hoe u ze beheert.

>[!NOTE]
>In dit artikel worden alleen klassieke implementaties behandeld.  Controleer de [uitgaande verbindingen](load-balancer-outbound-connections.md) voor alle implementatie Scenario's van resource manager in Azure.

Een implementatie in azure kan communiceren met eind punten buiten Azure in de open bare IP-adres ruimte. Wanneer een exemplaar een uitgaande stroom initieert naar een bestemming in de open bare IP-adres ruimte, wijst Azure het privé-IP-adres dynamisch toe aan een openbaar IP-adres. Nadat deze toewijzing is gemaakt, kan het retour verkeer voor deze uitgaande stroom van het doel ook het privé-IP-adres bereiken waarvan de stroom afkomstig is.

Azure gebruikt bron Network Address Translation (SNAT) om deze functie uit te voeren. Wanneer meerdere privé-IP-adressen worden gemaskeerd achter één openbaar IP-adres, gebruikt Azure [poort adres omzetting (Pat)](#pat) voor het maskeren van privé-IP-adressen. Tijdelijke poorten worden gebruikt voor PAT en zijn [vooraf toegewezen](#preallocatedports) op basis van pool grootte.

Er zijn meerdere [uitgaande scenario's](#scenarios). U kunt deze scenario's zo nodig combi neren. Lees deze zorgvuldig door om inzicht te krijgen in de mogelijkheden, beperkingen en patronen zoals die van toepassing zijn op uw implementatie model en toepassings scenario. Raadpleeg de richt lijnen voor het [beheren van deze scenario's](#snatexhaust).

## <a name="scenarios"></a>Scenario-overzicht

Azure biedt drie verschillende methoden voor het bezorgen van klassieke connectiviteits implementaties.  Niet alle klassieke implementaties hebben alle drie de scenario's die beschikbaar zijn:

| Scenario | Methode | IP-protocollen | Description | Web Worker-rol | IaaS | 
| --- | --- | --- | --- | --- | --- |
| [1. VM met een openbaar IP-adres op exemplaar niveau](#ilpip) | SNAT, poort masker niet gebruikt | TCP, UDP, ICMP, ESP | Azure maakt gebruik van de aan het open bare IP-adres van de virtuele machine. Alle tijdelijke poorten zijn beschikbaar voor het exemplaar. | Nee | Ja |
| [2. openbaar eind punt met gelijke taak verdeling](#publiclbendpoint) | SNAT met poort maskering (PAT) naar het open bare eind punt | TCP, UDP | Azure deelt het open bare IP-adres openbaar eind punt met meerdere persoonlijke eind punten. Azure gebruikt tijdelijke poorten van het open bare eind punt voor PAT. | Ja | Ja |
| [3. Zelfstandige VM](#defaultsnat) | SNAT met poort maskering (PAT) | TCP, UDP | Azure wijst automatisch een openbaar IP-adres toe voor SNAT, deelt dit open bare IP-adres met de volledige implementatie en maakt gebruik van tijdelijke poorten van het IP-adres van het open bare eind punt voor PAT. Dit is een terugval scenario voor de voor gaande scenario's. Het is niet raadzaam om het te controleren en te beheren. | Ja | Ja |

Dit is een subset van de functionaliteit voor uitgaande verbindingen die beschikbaar is voor implementaties van resource manager in Azure.  

Verschillende implementaties in het klassiek hebben verschillende functionaliteit:

| Klassieke implementatie | Beschik bare functionaliteit | 
| --- | --- |
| Virtuele Machine | scenario [1](#ilpip), [2](#publiclbendpoint)of [3](#defaultsnat) |
| Web Worker-rol | alleen scenario [2](#publiclbendpoint), [3](#defaultsnat) | 

[Strategieën voor risico beperking](#snatexhaust) hebben ook dezelfde verschillen.

De algoritme die wordt gebruikt voor het voortoewijzen van tijdelijke poorten voor PAT voor klassieke implementaties is hetzelfde als voor de implementatie van Azure Resource Manager-resources.

### <a name="ilpip"></a>Scenario 1: VM met een openbaar IP-adres op exemplaar niveau

In dit scenario heeft de virtuele machine een openbaar IP-adres (ILPIP) met een instantie niveau toegewezen. Voor uitgaande verbindingen is het niet van belang of de VM een eind punt met gelijke taak verdeling heeft of niet. Dit scenario heeft voor rang op de andere. Wanneer er een ILPIP wordt gebruikt, gebruikt de virtuele machine de ILPIP voor alle uitgaande stromen.  

Een openbaar IP-adres dat is toegewezen aan een virtuele machine is een 1:1-relatie (in plaats van 1: veel) en geïmplementeerd als een stateless 1:1 NAT.  Poort maskering (PAT) wordt niet gebruikt en de virtuele machine heeft alle tijdelijke poorten die beschikbaar zijn voor gebruik.

Als uw toepassing veel uitgaande stromen initieert en u de SNAT-poort uitputtt, kunt u overwegen een [ILPIP toe te wijzen om de SNAT-beperkingen te beperken](#assignilpip). Bekijk het beheer van de [SNAT](#snatexhaust) -uitputting in zijn geheel.

### <a name="publiclbendpoint"></a>Scenario 2: Openbaar eind punt met gelijke taak verdeling

In dit scenario wordt de rol VM of Web Worker gekoppeld aan een openbaar IP-adres met behulp van het eind punt met gelijke taak verdeling. Aan de virtuele machine is geen openbaar IP-adres toegewezen. 

Wanneer de virtuele machine met taak verdeling een uitgaande stroom maakt, vertaalt Azure het IP-adres van de particuliere bron van de uitgaande stroom naar het open bare IP-adres van het open bare eind punt met gelijke taak verdeling. Azure gebruikt SNAT om deze functie uit te voeren. Azure gebruikt ook [Pat](#pat) om meerdere privé-IP-adressen achter een openbaar IP-adres te maskeren. 

Tijdelijke poorten van de open bare IP-adres-frontend van de load balancer worden gebruikt voor het onderscheiden van de afzonderlijke stromen die afkomstig zijn van de virtuele machine. SNAT maakt dynamisch gebruik van [vooraf toegewezen tijdelijke poorten](#preallocatedports) wanneer uitgaande stromen worden gemaakt. In deze context worden de tijdelijke poorten die worden gebruikt voor SNAT de SNAT-poorten genoemd.

SNAT-poorten zijn vooraf toegewezen, zoals beschreven in de sectie [over SNAT en Pat](#snat) . Ze zijn een eindige resource die kan worden uitgeput. Het is belang rijk om te begrijpen hoe [](#pat)ze worden verbruikt. Als u wilt weten hoe u dit verbruik kunt ontwerpen en zo nodig kunt beperken, raadpleegt u de [beheer SNAT](#snatexhaust)-uitputting.

Als er [meerdere open bare eind punten met gelijke taak verdeling](load-balancer-multivip.md) bestaan, zijn een van deze open bare IP-adressen een kandidaat voor uitgaande stromen en wordt er een wille keurige selectie geselecteerd.  

### <a name="defaultsnat"></a>Scenario 3: Geen openbaar IP-adres gekoppeld

In dit scenario maakt de rol VM of Web Worker geen deel uit van een openbaar eind punt met gelijke taak verdeling.  In het geval van een virtuele machine is er geen ILPIP-adres toegewezen aan de VM. Wanneer de virtuele machine een uitgaande stroom maakt, vertaalt Azure het IP-adres van de privé bron van de uitgaande stroom naar een IP-adres van een open bare bron. Het open bare IP-adres dat voor deze uitgaande stroom wordt gebruikt, kan niet worden geconfigureerd en telt niet op basis van de open bare IP-resource limiet van het abonnement.  Dit adres wordt automatisch door Azure toegewezen.

Azure gebruikt SNAT met poort maskering ([Pat](#pat)) om deze functie uit te voeren. Dit scenario is vergelijkbaar met scenario 2, maar er is geen controle over het gebruikte IP-adres. Dit is een terugval scenario voor situaties waarin de scenario's 1 en 2 niet bestaan. Dit scenario wordt niet aanbevolen als u het uitgaande adres wilt beheren. Als uitgaande verbindingen een belang rijk onderdeel zijn van uw toepassing, moet u een ander scenario kiezen.

SNAT-poorten zijn vooraf toegewezen, zoals beschreven in de sectie [over SNAT en Pat](#snat) .  Het aantal Vm's of Web Worker-rollen die het open bare IP-adres delen, bepaalt het aantal vooraf toegewezen tijdelijke poorten.   Het is belang rijk om te begrijpen hoe [](#pat)ze worden verbruikt. Als u wilt weten hoe u dit verbruik kunt ontwerpen en zo nodig kunt beperken, raadpleegt u de [beheer SNAT](#snatexhaust)-uitputting.

## <a name="snat"></a>Meer informatie over SNAT en PAT

### <a name="pat"></a>Poort maskering SNAT (PAT)

Wanneer een implementatie een uitgaande verbinding maakt, wordt elke uitgaande verbindings bron herschreven. De bron wordt herschreven van de privé-IP-adres ruimte naar de open bare IP die is gekoppeld aan de implementatie (op basis van de scenario's die hierboven worden beschreven). In de open bare IP-adres ruimte moet de 5-tuple van de stroom (bron-IP-adres, bron poort, IP-transport protocol, doel-IP-adres, doel poort) uniek zijn.  

Tijdelijke poorten (SNAT-poorten) worden gebruikt om dit te verhelpen na het herschrijven van het IP-adres van de privé bron, omdat meerdere stromen afkomstig zijn van één openbaar IP-adres. 

Eén SNAT-poort wordt per stroom verbruikt naar één doel-IP-adres, poort en protocol. Voor meerdere stromen naar hetzelfde doel-IP-adres, dezelfde poort en hetzelfde protocol gebruikt elke stroom één SNAT-poort. Dit zorgt ervoor dat de stromen uniek zijn wanneer ze afkomstig zijn uit hetzelfde open bare IP-adres en naar hetzelfde doel-IP-adres, dezelfde poort en hetzelfde protocol gaan. 

Meerdere stromen, elk naar een ander doel-IP-adres, poort en protocol, delen één SNAT-poort. Het doel-IP-adres, de poort en het Protocol maken stroom uniek zonder dat er extra bron poorten nodig zijn om stromen te onderscheiden van de open bare IP-adres ruimte.

Wanneer de SNAT-poort resources zijn uitgeput, mislukken uitgaande stromen totdat bestaande stromen van SNAT-poorten worden vrijgegeven. Load Balancer overschrijden de SNAT-poorten wanneer de stroom wordt gesloten en gebruikt een [time-out van 4 minuten](#idletimeout) voor het vrijmaken van de SNAT-poorten van niet-actieve stromen.

Raadpleeg de sectie [SNAT beheren](#snatexhaust) als u patronen wilt beperken die vaak leiden tot een SNAT-poort uitputting.

### <a name="preallocatedports"></a>Tijdelijke poort voortoewijzing voor poort maskering SNAT (PAT)

Azure gebruikt een algoritme om te bepalen hoeveel vooraf toegewezen SNAT-poorten beschikbaar zijn op basis van de grootte van de back-end-pool wanneer poort maskering SNAT ([Pat](#pat)) wordt gebruikt. SNAT-poorten zijn tijdelijke poorten die beschikbaar zijn voor een bepaald openbaar IP-bron adres.

Azure wijst SNAT-poorten vooraf toe wanneer een exemplaar wordt geïmplementeerd op basis van het aantal exemplaren van virtuele machines of Web-Workers die een gegeven openbaar IP-adres delen.  Wanneer uitgaande stromen worden gemaakt, gebruikt [Pat](#pat) dynamisch (Maxi maal de vooraf toegewezen limiet) en worden deze poorten vrijgegeven wanneer de stroom wordt gesloten of er time-outs voor inactiviteit optreden.

In de volgende tabel ziet u de SNAT-poort voortoewijzingen voor lagen van back-endadresgroep-Pools:

| exemplaren | Vooraf toegewezen SNAT-poorten per instantie |
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |

Houd er rekening mee dat het aantal beschik bare SNAT-poorten niet rechtstreeks naar het aantal stromen kan worden vertaald. Eén SNAT-poort kan opnieuw worden gebruikt voor meerdere unieke bestemmingen. Poorten worden alleen gebruikt als het nodig is om stromen uniek te maken. Raadpleeg de sectie over het [beheren van deze exhaustible-resource](#snatexhaust) en de sectie met [Pat](#pat)voor instructies voor het ontwerpen en beperken van problemen.

Het wijzigen van de grootte van uw implementatie kan invloed hebben op sommige van uw vastgelegde stromen. Als de grootte van de back-end-groep toeneemt en overschakelt naar de volgende laag, worden de helft van uw vooraf toegewezen SNAT-poorten vrijgemaakt tijdens de overgang naar de volgende grotere back-endadresgroep. Stromen die zijn gekoppeld aan een vrijgestelde SNAT-poort, worden getimed en moeten opnieuw worden ingesteld. Als er een nieuwe stroom wordt gedaan, wordt de stroom onmiddellijk uitgevoerd zolang er vooraf toegewezen poorten beschikbaar zijn.

Als de implementatie grootte vermindert en overgangen naar een lagere laag, neemt het aantal beschik bare SNAT-poorten toe. In dit geval worden bestaande toegewezen SNAT-poorten en de bijbehorende stromen niet beïnvloed.

Als een Cloud service opnieuw wordt geïmplementeerd of gewijzigd, kan de-infra structuur de back-end-pool tijdelijk zo hoog als werkelijk instellen, zodat Azure minder SNAT-poorten per instantie vooraf toewijst dan verwacht.  Dit kan de waarschijnlijkheid van de SNAT-poort uitgeput tijdelijk verhogen. Uiteindelijk wordt de grootte van de pool overgezet naar de werkelijke grootte en Azure worden vooraf toegewezen SNAT-poorten automatisch verhoogd naar het verwachte aantal volgens de bovenstaande tabel.  Dit gedrag is inherent aan het ontwerp en kan niet worden geconfigureerd.

Toewijzing van SNAT-poorten is een specifiek IP-transport protocol (TCP en UDP worden afzonderlijk onderhouden) en worden vrijgegeven onder de volgende voor waarden:

### <a name="tcp-snat-port-release"></a>TCP SNAT-poort release

- Als zowel de server als de client FIN/ACK verzendt, wordt de SNAT-poort na 240 seconden vrijgegeven.
- Als een RST wordt gezien, wordt de SNAT-poort na 15 seconden vrijgegeven.
- time-out voor inactiviteit is bereikt

### <a name="udp-snat-port-release"></a>UDP SNAT-poort release

- time-out voor inactiviteit is bereikt

## <a name="problemsolving"></a>Probleem oplossen 

Deze sectie is bedoeld om de SNAT-uitputting en andere scenario's die kunnen worden uitgevoerd met uitgaande verbindingen in azure te beperken.

### <a name="snatexhaust"></a>Poort uitputting van SNAT (PAT) beheren
[Tijdelijke poorten](#preallocatedports) die worden gebruikt voor [Pat](#pat) zijn een exhaustible-bron, zoals beschreven in [geen openbaar IP-adres dat is gekoppeld aan](#defaultsnat) het [open bare eind punt](#publiclbendpoint).

Als u weet dat u een groot aantal uitgaande TCP-of UDP-verbindingen met hetzelfde doel-IP-adres en dezelfde poort wilt initiëren, kunt u zien dat uitgaande verbindingen mislukken of worden aanbevolen door ondersteuning te bieden voor de SNAT-poorten (vooraf toegewezen [tijdelijke poorten](#preallocatedports) ) gebruikt door [Pat](#pat)), hebt u enkele algemene beperkende opties. Bekijk deze opties en beslis wat er beschikbaar en beste is voor uw scenario. Het is mogelijk dat een of meer informatie kan helpen bij het beheren van dit scenario.

Als u problemen ondervindt met het gedrag van uitgaande verbindingen, kunt u IP-stack statistieken (netstat) gebruiken. Het kan ook handig zijn om verbindings gedrag te observeren met behulp van pakket opnamen.

#### <a name="connectionreuse"></a>Wijzig de toepassing om verbindingen opnieuw te gebruiken 
U kunt de vraag beperken voor tijdelijke poorten die worden gebruikt voor SNAT door verbindingen in uw toepassing opnieuw te gebruiken. Dit geldt met name voor protocollen zoals HTTP/1.1, waarbij hergebruik van verbindingen de standaard instelling is. En andere protocollen die gebruikmaken van HTTP als Trans Port (bijvoorbeeld REST), kunnen op hun beurt profiteren van. 

Hergebruik is altijd beter dan afzonderlijke, atomische TCP-verbindingen voor elke aanvraag. Het opnieuw gebruiken van resultaten in meer uitvoerende, zeer efficiënte TCP-trans acties.

#### <a name="connection pooling"></a>De toepassing wijzigen voor het gebruik van groepsgewijze verbindingen
U kunt een schema voor groepsgewijze verbindingen gebruiken in uw toepassing, waarbij aanvragen intern worden gedistribueerd via een vaste set verbindingen (waarbij elk gebruik waar mogelijk wordt gebruikt). Dit schema beperkt het aantal tijdelijke poorten dat in gebruik is en maakt een meer voorspel bare omgeving. Dit schema kan ook de door Voer van aanvragen verhogen door meerdere gelijktijdige bewerkingen toe te staan wanneer één verbinding wordt geblokkeerd bij het beantwoorden van een bewerking.  

Verbindings groeperingen kunnen al bestaan in het Framework dat u gebruikt voor het ontwikkelen van uw toepassing of de configuratie-instellingen voor uw toepassing. U kunt groepsgewijze verbindingen combi neren met het opnieuw gebruiken van verbindingen. Uw meerdere aanvragen verbruiken vervolgens een vast, voorspelbaar aantal poorten op hetzelfde doel-IP-adres en dezelfde poort. De aanvragen profiteren ook van efficiënt gebruik van TCP-trans acties waarbij de latentie en het resource gebruik worden verminderd. UDP-trans acties kunnen ook profiteren van het voor deel, omdat het beheer van het aantal UDP-stromen mogelijk is om de uitlaat omstandigheden te voor komen en het SNAT-poort gebruik te beheren.

#### <a name="retry logic"></a>De toepassing wijzigen voor het gebruik van minder agressieve nieuwe logica
Wanneer [tijdelijke poorten](#preallocatedports) die worden gebruikt voor [Pat](#pat) , worden uitgeput of er fouten in de toepassing optreden, worden pogingen voor agressieve of brute Forces zonder onverwachting en uitstel Logic uitputting veroorzaakt of blijven ze behouden. U kunt de vraag naar tijdelijke poorten verminderen door een minder agressieve pogings logica te gebruiken. 

Tijdelijke poorten hebben een time-out van 4 minuten (niet aanpasbaar). Als de nieuwe pogingen te agressief zijn, heeft de uitputting geen mogelijkheid om zichzelf zelf te wissen. Daarom is het een belang rijk onderdeel van het ontwerp om te overwegen hoe--en hoe vaak uw toepassingen nieuwe pogingen doen.

#### <a name="assignilpip"></a>Een openbaar IP-adres op instantie niveau toewijzen aan elke VM
Als u een ILPIP toewijst, wordt uw scenario gewijzigd in [een open bare IP-adres op instantie niveau](#ilpip). Alle tijdelijke poorten van het open bare IP-adres dat voor elke VM worden gebruikt, zijn beschikbaar voor de VM. (In tegens telling tot scenario's waarbij tijdelijke poorten van een openbaar IP-adres worden gedeeld met alle virtuele machines die zijn gekoppeld aan de betreffende implementatie.) Er zijn zaken die u moet overwegen, zoals de potentiële impact van het white list van een groot aantal afzonderlijke IP-adressen.

>[!NOTE] 
>Deze optie is niet beschikbaar voor Web Worker-rollen.

### <a name="idletimeout"></a>Keepalives gebruiken om de uitgaande time-out voor inactiviteit opnieuw in te stellen

Uitgaande verbindingen hebben een time-out van 4 minuten. Deze time-out kan niet worden aangepast. U kunt echter Trans Port (bijvoorbeeld TCP-keepalives) of keepalives op toepassings niveau gebruiken om een niet-actieve stroom te vernieuwen en zo nodig de time-out voor inactiviteit opnieuw in te stellen.  Neem contact op met de leverancier van verpakte software, of dit wordt ondersteund en hoe u deze functie inschakelt.  Over het algemeen moet slechts één zijde keepalives genereren om de time-out voor inactiviteit opnieuw in te stellen. 

## <a name="discoveroutbound"></a>Het open bare IP-adres dat door een virtuele machine wordt gebruikt, wordt gedetecteerd
Er zijn veel manieren om het IP-adres van de open bare bron van een uitgaande verbinding te bepalen. OpenDNS biedt een service waarmee u het open bare IP-adres van uw virtuele machine kunt weer geven. 

Met de opdracht nslookup kunt u een DNS-query voor de naam myip.opendns.com verzenden naar de OpenDNS-resolver. De service retourneert het bron-IP-adres dat is gebruikt om de query te verzenden. Wanneer u de volgende query uitvoert vanaf uw virtuele machine, is het antwoord het open bare IP-adres dat voor die virtuele machine wordt gebruikt:

    nslookup myip.opendns.com resolver1.opendns.com


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Load Balancer](load-balancer-overview.md) gebruikt in Resource Manager-implementaties.
- Lees modus over scenario's voor [uitgaande verbindingen](load-balancer-outbound-connections.md) die beschikbaar zijn in implementaties van Resource Manager.
