---
title: Architectuur van de Azure-netwerk
description: In dit artikel biedt een algemene beschrijving van het netwerk voor Microsoft Azure-infrastructuur.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 67781f196b445c9330e0dcb1fc7d8b0a1a53cbc0
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102177"
---
# <a name="azure-network-architecture"></a>Architectuur van de Azure-netwerk
De Azure-netwerkarchitectuur volgt een aangepaste versie van het industrie standaard distributie-Core/toegang model met verschillende hardwareabstractielagen. De lagen zijn onder andere:

- Core (Datacenter Routers)
- Distributiepunt (toegang Routers en aggregatie L2) - de distributie-laag scheidt N3 omschakelt L2 routering
- Toegang (L2 Host schakelopties)

De netwerkarchitectuur heeft twee niveaus van layer 2-switches, één laag aggregeren van verkeer van de andere en layer 2 lussen zodat redundantie. Dit biedt voordelen zoals een flexibelere VLAN-footprint en verbetert de poort schalen. De architectuur houdt L2 als N3 uniek is, dat staat het gebruik van hardware in elk van de verschillende lagen in het netwerk en minimaliseert veroorzaakt in een laag van invloed zijn op de andere laag/lagen. Het gebruik van transmissielijnen kunt delen, zoals de verbinding met de infrastructuur N3 van resources.

## <a name="network-configuration"></a>Netwerkconfiguratie
De netwerkarchitectuur van een cluster met Microsoft Azure binnen een datacenter bestaat uit de volgende apparaten:

- Routers (Datacenter, toegang Router en Leaf-Randrouters)
- Switches (aggregatie en bovenaan Rack-Switches)
- Digi CMs
- PDU's of Nucleons

De onderstaande afbeelding ziet u een overzicht van Azure netwerkarchitectuur binnen een cluster. Azure heeft twee afzonderlijke architecturen. Sommige bestaande Azure-klanten en gedeelde services zich bevinden op de standaard LAN-architectuur (DLA) dat nieuwe regio's en virtuele-klanten worden benaderd via Quantum 10 (Q10)-architectuur. De architectuur DLA is een traditionele structuur ontwerp met actieve passieve toegang routers en beveiligings-ACL's op de routers van de toegang wordt toegepast. De architectuur van de 10 Quantum is een clos/mesh-ontwerp routers waarbij ACL's worden niet toegepast op de routers, maar onder de routering via Software Load Balancing (SLB) of software-gedefinieerde VLAN's.

![Azure-netwerk][1]

### <a name="quantum-10-devices"></a>Quantum 10-apparaten
Het ontwerp van de Quantum 10 voert laag 3-switch verspreid over meerdere apparaten in een CLOS/mesh-ontwerp. De voordelen van het ontwerp van de Q10 zijn grotere capaciteit en meer mogelijkheden om te schalen bestaande netwerkinfrastructuur. Het ontwerp worden de veiligheidsmaatregelen leaf-border-routers, switches as en boven van Rack Routers verkeer doorgeven aan clusters op meerdere routes toe voor fouttolerantie. Beveiliging services zoals netwerkadresomzetting via het taakverdeling via Software in plaats van op hardwareapparaten worden verwerkt.

### <a name="access-routers"></a>Toegang tot Routers
De primaire routeringsfunctionaliteit uitvoeren de distributie/access N3-routers (ARs) voor de distributie en toegang lagen. Deze apparaten worden geïmplementeerd als een paar en de standaardgateway voor subnetten zijn. Elk paar AR biedt ondersteuning voor meerdere L2 aggregatie switch paren, afhankelijk van capaciteit. Het maximum aantal is afhankelijk van de capaciteit van het apparaat, evenals de domeinen van de fout. Een typische nummer op basis van de verwachte capaciteit zou drie L2 aggregatie switch paar per AR-paar zijn.

### <a name="l2-aggregation-switches"></a>L2 Aggregatie Switches  
Deze apparaten fungeren als een aggregatiepunt voor L2-verkeer. Ze zijn van de distributie-laag voor de fabric L2 en grote hoeveelheden verkeer kunnen verwerken. Omdat deze apparaten statistische verkeer, 802.1Q-functionaliteit is vereist en hoge bandbreedte technologieën zoals poort aggregatie en 10GE worden gebruikt.

### <a name="l2-host-switches"></a>L2 Host Switches
Hosts rechtstreeks verbinding maken met deze switches. Ze kunnen worden gekoppeld rack-switches of chassis-implementaties. De 802.1Q-standaard kunnen de aanwijzing van één VLAN als een systeemeigen VLAN die VLAN behandelen als normale (ongecodeerde) Ethernet-frametekens. Onder normale omstandigheden, frames in het systeemeigen VLAN worden verzonden en ontvangen zonder tags op een 802.1Q trunk-poort. Deze functie is ontworpen voor migratie naar 802.1Q en compatibiliteit met niet-802.1Q apparaten. In deze architectuur maakt gebruik van de infrastructuur van het netwerk de systeemeigen VLAN.

Deze architectuur Hiermee geeft u een standaard voor de selectie van systeemeigen VLAN die ervoor zorgt dat, indien mogelijk, dat de AR-apparaten hebben een unieke systeemeigen VLAN op voor elke trunk en de L2Aggregation naar L2Aggregation transmissielijnen. De L2Aggregation naar L2Host Switch transmissielijnen hebben een niet-standaard systeemeigen VLAN.

### <a name="link-aggregation-8023ad"></a>Link Aggregation (802.3ad)
(Vertraging aggregatie) kunt meerdere afzonderlijke koppelingen worden samen gebundeld en behandeld als één logische koppeling. Het getal dat wordt gebruikt om aan te wijzen poort kanaal interfaces moet worden genormaliseerd dus als u operationele foutopsporing gemakkelijker, de rest van het netwerk hetzelfde nummer aan beide uiteinden van een poort-kanaal gebruikt. Hiervoor moet een standaard om te bepalen welke einde van de poort-kanaal te gebruiken voor het definiëren van het volgende beschikbare nummer.

De getallen die is opgegeven voor de L2Agg met L2Host switch zijn de poort-kanaal getallen aan de kant L2Agg gebruikt. Omdat het bereik van getallen meer beperkte aan de kant L2Host, wordt de standaard is het gebruik van de getallen 1 en 2 aan de kant L2Host om te verwijzen naar de poort-kanaal zal de "a" kant en de "b" respectievelijk.

### <a name="vlans"></a>VLAN 's
De netwerkarchitectuur maakt gebruik van VLAN's aan de groepsservers samen tot één broadcast-domein. 802.1Q VLAN-nummers conform normen die VLAN's ondersteunt genummerde 1 – 4094 liggen.

### <a name="customer-vlans"></a>Klant VLAN 's
Klanten hebben verschillende opties voor de implementatie van VLAN ze kunnen implementeren via de Azure-Portal om te voldoen aan de behoeften scheiding en architectuur van de oplossing. Deze oplossingen worden geïmplementeerd via virtuele machines. Voor de klantreferentie architectuur voorbeelden gaat u naar [Azure verwijzing architecturen](https://docs.microsoft.com/azure/architecture/reference-architectures/).

### <a name="edge-architecture"></a>Edge-architectuur
Azure-datacenters zijn gebaseerd op maximaal redundante en goed ingerichte netwerkinfrastructuur. Netwerken in de Azure-datacenters worden geïmplementeerd met 'nodig plus één' (N + 1) redundantie architecturen of hoger. Volledige failover-functies binnen en tussen datacenters helpen om netwerk- en beschikbaarheid te garanderen. Extern, worden datacenters door specifieke netwerken met hoge bandbreedte circuits die eigenschappen toch toe verbinding te maken met meer dan 1200 Internet Service Providers globaal op meerdere peering punten bieden meer dan 2.000 Gbps van potentiële capaciteit van de rand behandeld via het netwerk.

Filteren routers op de laag van de Microsoft Azure-netwerk van rand en toegang bieden ingeburgerd beveiliging op het pakketniveau van het om te voorkomen dat onbevoegde gebruikers hebben geprobeerd verbinding maken met Azure. Ze helpen om ervoor te zorgen dat de inhoud van de pakketten die gegevens in de verwachte indeling bevatten en aan het schema van verwachte client/server-communicatie voldoen. Azure implementeert een gelaagde architectuur die bestaan uit de volgende netwerk scheiding en access control-onderdelen:

- Edge-Routers - scheiden van de toepassingsomgeving van het Internet. Randrouters zijn ontworpen om bieden bescherming tegen vervalste en beperken van toegang met behulp van toegangsbeheerlijsten (ACL's).
- Distributie (Access) Routers - toegang routers zijn ontworpen om anti-adresvervalsing (spoofing) en tot stand gebrachte verbinding met ACL's toe te staan dat alleen Microsoft goedgekeurd IP-adressen.

### <a name="a10-ddos-mitigation-architecture"></a>A10 DDOS risicobeperking architectuur
Denial of service-aanvallen blijven echte gevaar opleveren voor de betrouwbaarheid van Microsoft online services. Als de aanvallen worden meer gericht, meer geavanceerde, en als Microsoft services worden meer geografisch verspreide, de mogelijkheid om te bieden effectieve mechanismen voor het identificeren en beperken de impact van dergelijke aanvallen is een hoge prioriteit.

De volgende informatie wordt uitgelegd hoe het A10 DDOS risicobeperking systeem vanuit het oogpunt van netwerk-architectuur van wordt geïmplementeerd.  
Microsoft Azure gebruikt A10 netwerkapparaten op DCR (Datacenter-Router) met automatische detectie en risicobeperking. De oplossing A10 maakt gebruik van Azure Netwerkbewaking steekproef Net stroom pakketten en bepalen of er een aanval. Zodra de aanval wordt gedetecteerd, worden A10 apparaten gebruikt als gasreinigers verkleinen van aanvallen. Na risicobeperking, is verder schone verkeer toegestaan in de Azure-datacenter rechtstreeks vanuit de DCR. De oplossing A10 wordt gebruikt voor het beveiligen van de infrastructuur van Azure-netwerk.

DDoS-bescherming in de oplossing A10 zijn onder andere:

- Bescherming tegen UDP IPv4 en IPv6 overstromingen
- ICMP IPv4 en IPv6 bescherming tegen overstromingen
- TCP IPv4 en IPv6-bescherming tegen overstromingen
- Beveiliging tegen aanvallen TCP SYN voor IPv4 en IPv6
- Fragmentatie van de aanval

> [!NOTE]
> DDoS-bescherming is standaard beschikbaar voor alle Azure-klanten.
>
>

## <a name="network-connection-rules"></a>Netwerk verbinding regels
Azure implementeert randrouters op het netwerk waarmee de beveiliging op het pakketniveau van het om te voorkomen dat onbevoegde gebruikers hebben geprobeerd verbinding maken met Microsoft Azure. Ze ervoor dat de inhoud van de pakketten die gegevens in de verwachte indeling bevatten en in overeenstemming met het verwachte client/server-communicatie-schema zijn.

Randrouters scheiden de toepassingsomgeving vanaf Internet. Randrouters zijn ontworpen om bieden bescherming tegen vervalste en beperken van toegang met behulp van toegangsbeheerlijsten (ACL's). Deze randrouters zijn geconfigureerd met behulp van een gelaagde benadering limiet netwerkprotocollen die zijn toegestaan routers van de rand doorvoer en toegang hebben tot routers van ACL.

Netwerkapparaten worden geplaatst op locaties voor toegang en rand en fungeren als grens punten waar inkomend en/of uitgaande filters zijn toegepast.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Microsoft biedt voor het beveiligen van de Azure-infrastructuur:

- [Azure opslagruimten, ruimten en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van de Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Azure productienetwerk](azure-production-network.md)
- [Beveiligingsfuncties van Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Azure productie-uitvoering en beheer](azure-infrastructure-operations.md)
- [Bewaking van Azure-infrastructuur](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Beveiliging van gegevens van de klant in Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-network/network-arch.png
