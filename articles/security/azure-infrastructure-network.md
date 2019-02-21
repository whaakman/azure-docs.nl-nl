---
title: Architectuur van Azure-netwerk
description: Dit artikel bevat een algemene beschrijving van het netwerk voor Microsoft Azure-infrastructuur.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2019
ms.author: terrylan
ms.openlocfilehash: 48a7e52d4284e5c2db1d77d24d91fd4701aad8d7
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56455753"
---
# <a name="azure-network-architecture"></a>Architectuur van Azure-netwerk
De architectuur van Azure-netwerk heeft een gewijzigde versie van de bedrijfstak distributie-core/toegang standaardmodel, met verschillende hardwareabstractielagen. De lagen zijn onder andere:

- Core (datacenter routers)
- Distributie (toegang routers en aggregatie L2). De distributie-laag worden gescheiden L3 routering van L2 overschakelen.
- Toegang (switches L2-host)

De netwerkarchitectuur heeft twee niveaus van layer 2-switches. Verkeer in één laag statistische functies van de andere laag. De tweede laag wordt uitgevoerd om op te nemen van redundantie. De architectuur biedt een meer flexibele VLAN-footprint en verbetert de poort schalen. De architectuur is bedoeld om L2 en N3 distinct, die het gebruik van hardware kunnen in elk van de verschillende lagen in het netwerk en minimaliseert de fout in één laag die betrekking hebben op de andere lagen. Het gebruik van schachten kan bronnen delen, zoals de verbinding met de L3-infrastructuur.

## <a name="network-configuration"></a>Netwerkconfiguratie
De netwerkarchitectuur van een Azure-cluster in een datacenter bestaat uit de volgende apparaten:

- Routers (datacenter, toegang router en leaf-randrouters)
- Switches (aggregatie en boven-of-rack-switches)
- Digi-CMs
- Power distribution units

Azure heeft twee verschillende architecturen. Sommige bestaande Azure-klanten en gedeelde services bevinden zich op de standaard-LAN-architectuur (DLA), terwijl nieuwe regio's en virtuele klanten bevinden zich op architectuur Quantum 10 (Q10). De architectuur DLA is een ontwerp traditionele structuur met routers actief/passief-toegang en beveiliging toegangscontrolelijsten (ACL's) toegepast op de routers toegang. Het Quantum-10-architectuur is een sluiten/mesh-ontwerp van routers, waar de ACL's niet worden toegepast op de routers. In plaats daarvan de ACL's worden toegepast onder de routering, via Software Load Balancing (SLB) of software-gedefinieerde VLAN's.

Het volgende diagram biedt een overzicht van de netwerkarchitectuur binnen een Azure-cluster:

![Diagram van Azure-netwerk][1]

### <a name="quantum-10-devices"></a>Quantum-10-apparaten
Het ontwerp van de Quantum 10 voert laag-3-switch verspreid over meerdere apparaten in een Clos/mesh-ontwerp. De voordelen van het ontwerp Q10 omvatten grotere capaciteit en betere mogelijkheden voor het schalen van bestaande netwerkinfrastructuur. Het ontwerp maakt gebruik van leaf-randrouters, rug switches en routers top-of-rack verkeer doorgeven aan clusters op meerdere routes, zodat voor fouttolerantie. Software load balancing, in plaats van hardware-apparaten, verwerkt security-services zoals netwerkadresomzetting.

### <a name="access-routers"></a>Toegang tot routers
De primaire routeringsfunctionaliteit uitvoeren de distributie/toegang L3-routers (ARs) voor de lagen distributie- en toegangsbeheer. Deze apparaten worden geïmplementeerd als een paar en de standaardgateway voor subnetten zijn. Elk paar AR biedt ondersteuning voor meerdere L2-aggregatie switch paren, afhankelijk van capaciteit. Het maximum aantal is afhankelijk van de capaciteit van het apparaat, evenals de domeinen van de fout. Een typische getal is drie L2-aggregatie switch paren per AR paar.

### <a name="l2-aggregation-switches"></a>L2-aggregatie switches  
Deze apparaten fungeren als een aggregatiepunt voor L2-verkeer. Ze zijn van de distributie-laag voor de L2-infrastructuur, en grote hoeveelheden verkeer kunnen verwerken. Omdat deze apparaten verkeer aggregeren, ze nodig hebben 802.1Q-functionaliteit en hoge bandbreedte technologieën zoals aggregatie van de poort en 10GE.

### <a name="l2-host-switches"></a>L2-host-switches
Hosts rechtstreeks verbinding maken met deze switches. Ze kunnen worden rek switches of chassis-implementaties. De 802.1Q-standaard kunnen de aanwijzing van een VLAN als een systeemeigen VLAN dat VLAN behandelen als normale (niet-gecodeerde) Ethernet-frametekens. Onder normale omstandigheden, frames op de systeemeigen VLAN worden verzonden en ontvangen zonder tags op een 802.1Q trunk-poort. Deze functie is ontworpen voor migratie naar 802.1Q en compatibiliteit met niet-802.1Q apparaten. In deze architectuur maakt gebruik van de infrastructuur van het netwerk de systeemeigen VLAN.

Deze architectuur bevat een standaard voor de selectie van systeemeigen VLAN. De standaard zorgt ervoor dat, indien mogelijk, dat de AR-apparaten hebben een unieke, systeemeigen VLAN voor elke trunk en de L2Aggregation naar L2Aggregation schachten. De L2Aggregation naar L2Host Switch schachten hebben een systeemeigen VLAN van niet-standaard.

### <a name="link-aggregation-8023ad"></a>Link aggregation (802.3ad)
Link aggregation kunnen meerdere afzonderlijke koppelingen worden gebundeld en behandeld als één logische koppeling. Om te kunnen operationele foutopsporing, moet het nummer dat wordt gebruikt om aan te geven van de poort-kanaal interfaces worden herleid. De rest van het netwerk gebruikt hetzelfde aantal aan beide uiteinden van een poort-kanaal.

De getallen die is opgegeven voor de L2Agg L2Host switch zijn de poort-kanaal getallen die aan de L2Agg wordt gebruikt. Omdat het bereik van de getallen beperktere aan de zijkant L2Host is, wordt de standaard is het gebruik van de getallen 1 en 2 aan de zijkant L2Host. Verwijzen naar de poort-kanaal te gaan naar de "a" zijde en de zijde "b", respectievelijk.

### <a name="vlans"></a>VLAN 's
De netwerkarchitectuur maakt gebruik van VLAN's aan de groepsservers samen in een enkele broadcast-domein. VLAN-nummers voldoen aan 802.1Q-standaard, die ondersteuning biedt voor VLAN's genummerde 1 – 4094 liggen.

### <a name="customer-vlans"></a>Klant-VLAN 's
U hebt verschillende opties voor de implementatie van VLAN u via de Azure-portal om te voldoen aan de architectuur en scheiding behoeften van uw oplossing kunt implementeren. U implementeert deze oplossingen via virtuele machines. Zie voor voorbeelden van klanten referentie-architectuur, [Azure-referentiearchitecturen](https://docs.microsoft.com/azure/architecture/reference-architectures/).

### <a name="edge-architecture"></a>Edge-architectuur
Azure-datacenters zijn die zijn gebouwd met zeer redundante en goed ingerichte netwerkinfrastructuren. Microsoft implementeert netwerken in de Azure-datacenters met 'moet plus één' (N + 1) redundantie architecturen of hoger. Volledige failover-functies binnen en tussen datacenters helpen om netwerk- en service-beschikbaarheid te garanderen. Extern, worden datacenters aangeboden door toegewezen netwerk met hoge bandbreedte circuits. Eigenschappen van verbinden deze circuits toch toe met meer dan 1200 internetproviders wereldwijd in meerdere peering punten. Dit biedt meer dan 2000 Gbps van potentiële edge-capaciteit via het netwerk.

Filterrouters op het niveau van het edge- en toegangsbeheer van het Azure-netwerk biedt goede beveiliging op het pakketniveau van het en voorkomt u dat niet-geautoriseerde pogingen tot verbinding maken met Azure. De routers helpen ervoor te zorgen dat de werkelijke inhoud van de pakketten die gegevens in de verwachte indeling bevatten en in overeenstemming met het verwachte client/server-communicatie-schema zijn. Een gelaagde architectuur, die bestaat uit de volgende netwerkscheiding en de onderdelen van beheer van toegang tot de implementatie van Azure:

- **Randrouters.** Deze scheiden de omgeving van het internet. Randrouters zijn ontworpen voor bescherming tegen vervalsen en de toegang beperken met behulp van ACL's.
- **Distributie (toegang) routers.** Deze staan dat alleen Microsoft goedgekeurd IP-adressen, bieden anti-adresvervalsing en verbindingen tot stand brengen met behulp van ACL's.

### <a name="ddos-mitigation"></a>DDOS-risicobeperking
Distributed denial of service (DDoS)-aanvallen blijven bieden een echte bedreiging wat betreft de betrouwbaarheid van online services. Zoals aanvallen meer gerichte en geavanceerde worden, en als de services biedt Microsoft meer geografisch verspreide, het identificeren en minimaliseert ook is de impact van deze aanvallen een hoge prioriteit.

[Azure DDoS Protection standaard](../virtual-network/ddos-protection-overview.md) biedt beveiliging tegen DDoS-aanvallen. Zie [Azure DDoS Protection: Aanbevolen procedures en referentiearchitecturen](azure-ddos-best-practices.md) voor meer informatie.

> [!NOTE]
> Microsoft biedt DDoS protection wordt standaard voor alle Azure-klanten.
>
>

## <a name="network-connection-rules"></a>Regels voor Network-verbinding
Op het netwerk implementeert Azure edge-routers die beveiliging op het pakketniveau van het om te voorkomen dat niet-geautoriseerde pogingen tot verbinding maken met Azure. Randrouters Zorg ervoor dat de werkelijke inhoud van de pakketten die gegevens in de verwachte indeling bevatten, en aan de verwachte client/server-communicatie-schema voldoen.

Randrouters scheiden de omgeving van het internet. Dankzij deze routers zijn ontworpen voor bescherming tegen vervalste, en de toegang beperken met behulp van ACL's. Microsoft configureren edge-routers met behulp van een gelaagde benadering ACL op limiet netwerkprotocollen die zijn toegestaan voor de randrouters doorvoer en toegang krijgen tot routers

Microsoft posities netwerkapparaten op toegangs- en edge-locaties, om te fungeren als grens punten waar binnenkomende of uitgaande filters worden toegepast.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over wat Microsoft doet om te helpen beveiligen van de Azure-infrastructuur:

- [Azure faciliteiten, lokale en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Azure productienetwerk](azure-production-network.md)
- [Azure SQL Database-beveiligingsfuncties](azure-infrastructure-sql.md)
- [-Azure-productiebewerkingen en beheer](azure-infrastructure-operations.md)
- [Azure-infrastructuur bewaken](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Azure-klant-gegevensbeveiliging](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-network/network-arch.png
