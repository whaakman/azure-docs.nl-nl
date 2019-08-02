---
title: Binnenkomend verkeer in azure Australia beheren
description: Een hand leiding voor het beheren van binnenkomend verkeer in azure Australië om te voldoen aan de vereisten van de Australische overheid voor beveiligde Internet gateways
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 808a615885129af1be9b7fdcdb64d5a8c5a25e40
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571651"
---
# <a name="controlling-ingress-traffic-in-azure-australia"></a>Binnenkomend verkeer in azure Australia beheren

Een kern element van het beveiligen van ICT-systemen is het beheren van het netwerk verkeer. Verkeer moet worden beperkt tot alleen die nood zakelijk om een systeem te laten beschikken over functies om te voor komen dat de kans op inbreuken kan worden beperkt.

Deze hand leiding bevat informatie over hoe binnenkomend netwerk verkeer binnen Azure werkt en aanbevelingen voor het implementeren van netwerk beveiligings controles voor een systeem dat is verbonden met internet.

De netwerk besturings elementen worden uitgelijnd met de ACSC-richt lijnen van het Australische Cyber Security Center (de gebruikers handleiding) en het doel van de ACSC Information Security Manual (ISM).

## <a name="requirements"></a>Vereisten

De algemene beveiligings vereisten voor de Gemenebests systemen worden gedefinieerd in de ISM. Voor de ondersteuning van gemenebests entiteiten bij het implementeren van netwerk beveiliging [heeft de ACSC ACSC-beveiliging gepubliceerd: Implementatie van netwerk segmentatie en](https://www.acsc.gov.au/publications/protect/network_segmentation_segregation.htm)-schei ding, en om te helpen bij het beveiligen van systemen in Cloud omgevingen, heeft de ACSC [Cloud Computing Security voor tenants](https://www.acsc.gov.au/publications/protect/Cloud_Computing_Security_for_Tenants.pdf)gepubliceerd.

Deze hand leidingen bevatten een overzicht van de context voor het implementeren van netwerk beveiliging en het beheren van verkeer en praktische aanbevelingen voor het ontwerpen en configureren van netwerken.

De micro soft [Cloud Computing Security voor tenants van Microsoft Azure](https://aka.ms/au-irap) Guide op de pagina Australische van de service Trust-Portal markeert specifieke micro soft-technologieën waarmee u aan het advies in de ACSC-publicaties kunt voldoen.

De volgende belang rijke vereisten, die zijn geïdentificeerd in de publicaties van de ACSC, zijn belang rijk voor het beheren van binnenkomend verkeer in Azure:

|Description|Source|
|---|---|
|**Implementeer netwerk segmentatie en schei ding, bijvoorbeeld: n-tier-architectuur, met behulp van op hosts gebaseerde firewalls en de netwerk toegangscontrole functies van de CSP om de inkomende en uitgaande VM-netwerk verbinding alleen te beperken tot de vereiste poorten/protocollen.**| _Cloud computing voor tenants_|
|**Implementeer voldoende hoge band breedte, lage latentie, betrouw bare netwerk connectiviteit** tussen de Tenant (inclusief de externe gebruikers van de Tenant) en de Cloud service om te voldoen aan de beschikbaarheids vereisten van de Tenant  | _Cloud computing voor tenants_|
|**Pas technologieën toe op meer dan alleen de netwerklaag**. Elke host en elk netwerk moeten, waar mogelijk, worden gesegmenteerd en gescheiden op het laagste niveau dat nagenoeg kan worden beheerd. In de meeste gevallen zijn segmentatie en schei ding van toepassing op de gegevenskoppelings-laag tot en met de toepassingslaag; in gevoelige omgevingen is het echter mogelijk dat de fysieke isolatie geschikt is. Op de host gebaseerde en netwerk-brede maat regelen moeten op een complementaire manier worden geïmplementeerd en centraal worden bewaakt. Het gebruik van een firewall of beveiligings apparaat als enige beveiligings maatregel is niet voldoende. |_ACSC beveiligen: Netwerk segmentatie en schei ding implementeren_|
|**Gebruik de beginselen van de minimale bevoegdheid en u hebt ‐ nodig om ‐ te weten**. Als een host, service of netwerk niet met een andere host, service of netwerk hoeft te communiceren, mag deze niet worden toegestaan. Als een host, service of netwerk alleen hoeft te communiceren met een andere host, service of netwerk met behulp van specifieke poorten of protocollen, moeten andere poorten of protocollen worden uitgeschakeld. Het nemen van deze principes in een netwerk is een aanvulling op de minimale gebruikers bevoegdheden en verhoogt de algehele beveiligings postuur van de omgeving aanzienlijk. |_ACSC beveiligen: Netwerk segmentatie en schei ding implementeren_|
|**Scheid hosts en netwerken op basis van hun gevoeligheid of kritiekheid voor zakelijke activiteiten**. U kunt schei ding bereiken met behulp van verschillende hardware of platforms, afhankelijk van verschillende beveiligings classificaties, beveiligings domeinen of Beschik baarheid/integriteits vereisten voor bepaalde hosts of netwerken. Met name afzonderlijke beheer netwerken en overwegen fysiek isoleren van out-of-band-beheer netwerken voor gevoelige omgevingen. |_ACSC beveiligen: Netwerk segmentatie en schei ding implementeren_|
|**U kunt de toegang van alle entiteiten aan alle andere entiteiten identificeren, verifiëren en toestaan**. Voor alle gebruikers, hosts en services moet de toegang beperkt zijn tot alleen de andere gebruikers, hosts en services die nodig zijn om hun aangewezen taken of functies uit te voeren. Alle verouderde of lokale services die de sterkte van identificatie-, authenticatie-en autorisatie Services omzeilen of downgradeen, moeten worden uitgeschakeld en het gebruik moet nauw keurig worden bewaakt. |_ACSC beveiligen: Netwerk segmentatie en schei ding implementeren_|
|**Implementatie van netwerk verkeer toestaan in plaats van vermelding weigeren**. Sta alleen toegang toe voor bekend goed netwerk verkeer (dat wil zeggen, geverifieerd en geautoriseerd), in plaats van toegang tot bekende beschadigde netwerk verkeer te weigeren (bijvoorbeeld door een specifiek adres of service te blok keren). White list resulteert in een superieure beveiligings beleid om in de zwarte lijst te worden geplaatst en verbetert de capaciteit van een organisatie om mogelijke netwerk indringers te detecteren en te beoordelen. |_ACSC beveiligen: Netwerk segmentatie en schei ding implementeren_|
|

In dit artikel vindt u informatie en aanbevelingen over hoe aan deze vereisten kan worden voldaan voor systemen die worden geïmplementeerd in azure met behulp van IaaS (Infrastructure as a Service) en platform as a Service (PaaS). Lees ook het artikel over het beheren van uitgaand [verkeer in azure Australia](gateway-egress-traffic.md) voor een volledig inzicht in het beheer van netwerk verkeer binnen Azure.

## <a name="architecture"></a>Architectuur

Als u betrokken bent bij het ontwerp of de implementatie van de besturings elementen netwerk beveiliging en ingangs verkeer, moet u eerst begrijpen hoe binnenkomend netwerk verkeer binnen Azure werkt in zowel IaaS als PaaS. In deze sectie vindt u een overzicht van de mogelijke ingangs punten waar netwerk verkeer een resource kan bereiken die in azure wordt gehost, en de beveiligings controles die beschikbaar zijn om dit verkeer te beperken en te beheren. Elk van deze onderdelen wordt uitvoerig besproken in de overige secties van deze hand leiding.

### <a name="architecture-components"></a>Architectuur onderdelen

In het weer gegeven architectuur diagram ziet u de mogelijke paden die netwerk verkeer kan nemen om verbinding te maken met een service die wordt gehost in Azure. Deze onderdelen zijn onderverdeeld in azure, IaaS ingress, PaaS inkomend en beveiligings beheer, afhankelijk van de functie die ze bieden voor binnenkomend verkeer.

![Architectuur](media/ingress-traffic.png)

### <a name="azure-components"></a>Azure-onderdelen

|Onderdeel | Description|
|---|---|
|**DDoS Protection** | Met DDoS-aanvallen (Distributed Denial of service) wordt geprobeerd om de resources van een toepassing te laten uitgeputen, waardoor de toepassing niet beschikbaar is voor legitieme gebruikers. DDoS-aanvallen kunnen worden gericht op elk eindpunt dat openbaar bereikbaar is via internet. Azure bevat DDoS Protection automatisch via het Azure-platform en biedt extra mogelijkheden voor risico beperking die kunnen worden ingeschakeld voor specifieke toepassingen voor meer gedetailleerde controle.|
| **Traffic Manager** | Azure Traffic Manager is een op Domain Name System (DNS) gebaseerd verkeer load balancer dat verkeer optimaal kan distribueren naar Services in azure-regio's, terwijl hoge Beschik baarheid en reactie tijd wordt geboden. Traffic Manager DNS gebruikt om client aanvragen te sturen naar het meest geschikte eind punt op basis van een verkeers routerings methode en de status van de eind punten.|
| **ExpressRoute** | ExpressRoute is een toegewijde netwerk verbinding voor het gebruik van micro soft-Cloud Services. Het is ingericht via een connectiviteits provider en biedt meer betrouw baarheid, hogere snelheden, lagere wacht tijden en hogere beveiliging dan typische verbindingen via internet. Een ExpressRoute-circuit vertegenwoordigt de logische verbinding tussen de on-premises infra structuur en micro soft-Cloud Services via een connectiviteits provider.|
| **ExpressRoute persoonlijke peering** | Persoonlijke ExpressRoute-peering is een verbinding tussen de on-premises omgeving en persoonlijke Azure Virtual Networks. Privé-peering maakt toegang tot Azure-Services, zoals Virtual Machines, die in een virtueel netwerk worden geïmplementeerd. De bronnen en virtuele netwerken die via persoonlijke peering worden gebruikt, worden beschouwd als een uitbrei ding van het basis netwerk van een organisatie. Privé-peering biedt bidirectionele connectiviteit tussen het on-premises netwerk en virtuele Azure-netwerken met behulp van privé-IP-adressen.|
| **ExpressRoute micro soft-peering** | ExpressRoute micro soft-peering is een verbinding tussen de on-premises omgeving en de open bare services van micro soft en Azure. Dit omvat connectiviteit met Office 365, Dynamics 365 en Azure PaaS Services. Peering wordt tot stand gebracht via open bare IP-adressen die eigendom zijn van de organisatie of connectiviteits provider. Er zijn standaard geen services toegankelijk via ExpressRoute micro soft-peering en een organisatie moet zich aanmelden bij de services die vereist zijn. Dit proces biedt vervolgens connectiviteit met dezelfde eind punten die beschikbaar zijn op internet.|
|

### <a name="iaas-ingress-components"></a>IaaS ingress-onderdelen

|Onderdeel | Description|
|---|---|
|**Netwerk interface** | Een netwerk interface is een resource die zich in azure bevindt. Het is gekoppeld aan een virtuele machine en er is een persoonlijk, niet door Internet routeerbaar IP-adres toegewezen van het subnet waaraan het is gekoppeld. Dit IP-adres is dynamisch of statisch toegewezen via Azure Resource Manager.|
|**Subnet** | Een subnet is een IP-adres bereik dat is gemaakt binnen een VNet. Er kunnen meerdere subnetten worden gemaakt binnen een VNet voor netwerk segmentatie.|
| **Virtual Network (VNet)** | Een VNet is een kern bron binnen Azure die een platform en grens biedt voor het implementeren van resources en het inschakelen van communicatie. Het VNet bevindt zich in een Azure-regio en definieert de IP-adres ruimte en routerings grenzen voor VNet-geïntegreerde resources zoals Virtual Machines.|
| **VNET-peering** | VNet-peering is een Azure-configuratie optie waarmee directe communicatie tussen twee VNets mogelijk is zonder dat een Virtual Network gateway nodig is. Zodra de twee VNets zijn gekoppeld, kunnen ze rechtstreeks communiceren en kan de configuratie worden gebruikt om het gebruik van Virtual Network gateways en andere doorvoer opties te beheren.|
| **Openbare IP** | Een openbaar IP-adres is een bron die een van de door micro soft in eigendom zijnde, Internet Routeer bare IP-adressen van de opgegeven regio voor gebruik in het virtuele netwerk reserveert. Het kan worden gekoppeld aan een specifieke netwerk interface, waardoor de bron toegankelijk is vanaf het Internet, de ExpressRoute-en PaaS-systemen.|
| **ExpressRoute-gateway** | Een ExpressRoute-gateway is een object in een Virtual Network biedt connectiviteit en route ring van de Virtual Network naar on-premises netwerken via particuliere peering op een ExpressRoute-circuit.|
| **VPN Gateway** | Een VPN Gateway is een object in een Virtual Network dat een versleutelde tunnel van een Virtual Network naar een extern netwerk biedt. De versleutelde tunnel kan site-naar-site zijn voor bidirectionele communicatie met een on-premises omgeving, een ander virtueel netwerk of een punt-naar-site voor communicatie met één eind punt.|
| **PaaS VNet-integratie** | Veel PaaS-mogelijkheden kunnen worden geïmplementeerd in of geïntegreerd met, een Virtual Network. Sommige PaaS-mogelijkheden kunnen volledig worden geïntegreerd met een VNet en alleen toegankelijk zijn via privé-IP-adressen. Andere, zoals Azure Load Balancer en Azure-toepassing gateway, kunnen een externe interface hebben met een openbaar IP-adres en een interne interface met een privé-IP-adres in het virtuele netwerk. In dit geval kan verkeer binnenkomen via de PaaS-functie van de Virtual Network.|
|

### <a name="paas-ingress-components"></a>PaaS ingress-onderdelen

|Onderdeel | Description|
|---|---|
|**Hostnaam** | Azure PaaS-mogelijkheden worden geïdentificeerd aan de hand van een unieke open bare hostnaam die wordt toegewezen wanneer de resource wordt gemaakt. Deze hostnaam wordt vervolgens geregistreerd bij een openbaar DNS-domein, waar het kan worden omgezet naar een openbaar IP-adres.|
|**Openbare IP** | Tenzij geïmplementeerd in een VNet-geïntegreerde configuratie, zijn Azure PaaS-mogelijkheden toegankelijk via een openbaar IP-adres dat via internet kan worden gerouteerd. Dit adres kan worden toegewezen aan de specifieke resources, zoals een openbaar Load Balancer, of kan worden gekoppeld aan een specifieke mogelijkheid met een gedeeld toegangs punt voor meerdere exemplaren, zoals Storage of SQL. Dit open bare IP-adres is toegankelijk vanaf internet, ExpressRoute of van IaaS open bare IP-adressen via het Azure-backbone-netwerk.|
|**Service-eindpunten** | Service-eind punten bieden een directe, particuliere verbinding van een Virtual Network naar een specifieke PaaS-mogelijkheid. Service-eind punten, die alleen beschikbaar zijn voor een subset van PaaS-mogelijkheden, bieden betere prestaties en beveiliging voor bronnen in een VNet dat toegang krijgt tot PaaS.|
|

### <a name="security-controls"></a>Beveiligings controles

|Onderdeel | Description|
|---|---|
|**Netwerk beveiligings groepen (Nsg's)** | Nsg's beheert het verkeer naar en van virtuele netwerk bronnen in Azure. Nsg's regels Toep assen voor de verkeers stromen die zijn toegestaan of geweigerd, waaronder verkeer binnen Azure en tussen Azure en externe netwerken, zoals on-premises of het internet. Nsg's worden toegepast op subnetten binnen een virtueel netwerk of op afzonderlijke netwerk interfaces.|
|**PaaS-firewall** | Veel PaaS-mogelijkheden, zoals opslag en SQL, hebben een ingebouwde firewall voor het beheren van binnenkomend netwerk verkeer naar de specifieke resource. Er kunnen regels worden gemaakt voor het toestaan of weigeren van verbindingen van specifieke IP-adressen en/of virtuele netwerken.|
|**PaaS-verificatie en-Access Control** | Als onderdeel van een gelaagde aanpak van beveiliging bieden PaaS-mogelijkheden meerdere mechanismen voor het verifiëren van gebruikers en het beheren van bevoegdheden en toegang.|
|**Azure Policy** | Azure Policy is een service in azure voor het maken, toewijzen en beheren van beleid. Dit beleid gebruikt regels voor het beheren van de typen resources die kunnen worden geïmplementeerd en de configuratie van deze resources. Beleids regels kunnen worden gebruikt om naleving af te dwingen door te voor komen dat bronnen worden geïmplementeerd als ze niet aan de vereisten voldoen of kunnen worden gebruikt voor bewaking om te rapporteren over de nalevings status.|
|

## <a name="general-guidance"></a>Algemene richtlijnen

Als u beveiligde oplossingen wilt ontwerpen en bouwen in azure, is het belang rijk om het netwerk verkeer te begrijpen en te beheren zodat alleen geïdentificeerde en geautoriseerde communicatie kan optreden. De bedoeling van deze richt lijnen en de specifieke richt lijnen voor onderdelen in latere secties zijn het beschrijven van de hulpprogram ma's en services die kunnen worden toegepast voor het Toep assen van de principes die _worden beschreven in de ACSC-beveiliging: Implementatie van netwerk segmentering en schei ding_ over Azure-workloads. Dit omvat informatie over het maken van een virtuele architectuur voor het beveiligen van resources wanneer het niet mogelijk is om dezelfde traditionele fysieke en netwerk controles toe te passen die mogelijk zijn in een on-premises omgeving.

### <a name="specific-focus-areas"></a>Specifieke focus gebieden

* Het aantal toegangs punten voor virtuele netwerken beperken
* Het aantal open bare IP-adressen beperken
* Overweeg het gebruik van een hub-en spoke-netwerk ontwerp voor virtuele netwerken zoals beschreven in de micro soft Virtual Data Center (VDC)-documentatie
* Gebruik producten met ingebouwde beveiligings mogelijkheden voor inkomende verbindingen van Internet (bijvoorbeeld Application Gateway, API-gateway, virtuele netwerk apparaten)
* Beperk communicatie stromen tot PaaS-mogelijkheden tot alleen die nood zakelijk zijn voor systeem functionaliteit
* Implementeer PaaS in een VNet-geïntegreerde configuratie voor meer schei ding en controle
* Systemen configureren voor het gebruik van versleutelings mechanismen in overeenstemming met de ACSC en ISM van de gebruikers handleiding
* Op identiteit gebaseerde beveiligingen, zoals verificatie en op rollen gebaseerd toegangs beheer, naast traditionele netwerk besturings elementen gebruiken
* ExpressRoute implementeren voor connectiviteit met on-premises netwerken
* Vpn's implementeren voor beheer verkeer en integratie met externe netwerken
* Gebruik Azure Policy om de regio's en resources te beperken tot alleen die nood zakelijk zijn voor systeem functionaliteit
* Azure Policy voor het afdwingen van Baseline-beveiligings configuratie voor bronnen die toegankelijk zijn via Internet

### <a name="additional-resources"></a>Aanvullende resources

|Resource | Koppelen|
|---|---|
|Documenten voor Australische regelgeving en beleids naleving met inbegrip van de gebruikers handleiding|[https://aka.ms/au-irap](https://aka.ms/au-irap)|
|Azure Virtual Data Center|[https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter)|
|ACSC-netwerk segmentatie|[https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)|
|ACSC Cloud beveiliging voor tenants| [https://acsc.gov.au/publications/protect/cloud-security-tenants.htm](https://acsc.gov.au/publications/protect/cloud-security-tenants.htm)|
|Hand matig ACSC Information Security|[https://acsc.gov.au/infosec/ism/index.htm](https://acsc.gov.au/infosec/ism/index.htm)|

## <a name="component-guidance"></a>Onderdeel richtlijnen

In deze sectie vindt u meer informatie over de afzonderlijke onderdelen die relevant zijn voor binnenkomend verkeer naar systemen die zijn geïmplementeerd in Azure. In elke sectie wordt het doel van het specifieke onderdeel beschreven met koppelingen naar documentatie en configuratie handleidingen die kunnen worden gebruikt om te helpen bij het ontwerpen en bouwen van activiteiten.

## <a name="azure"></a>Azure

Alle communicatie met resources binnen Azure loopt via de beheerde netwerk infrastructuur van micro soft, die connectiviteit en beveiligings functionaliteit biedt. Micro soft plaatst automatisch een bereik van beveiligingen ter bescherming van het Azure-platform en de netwerk infrastructuur en er zijn extra mogelijkheden beschikbaar als Services in azure om netwerk verkeer te beheren en netwerk segmentatie in te stellen en scheiding.

### <a name="ddos-protection"></a>DDoS Protection

Via internet toegankelijke bronnen zijn gevoelig voor DDoS-aanvallen. Ter bescherming tegen deze aanvallen biedt Azure DDoS-beveiligingen op een basis-en standaard niveau.

Basic wordt automatisch ingeschakeld als onderdeel van het Azure-platform, inclusief altijd verkeer bewaken en real-time beperking van veelvoorkomende aanvallen op netwerk niveau, waarbij dezelfde verdedigingen worden geboden als bij de onlineservices van micro soft. De volledige schaal van het wereld wijde netwerk van Azure kan worden gebruikt voor het distribueren en beperken van het aanvals verkeer tussen regio's. Beveiliging is beschikbaar voor IPv4-en IPv6 Azure open bare IP-adressen

Standard biedt extra mogelijkheden voor risico beperking ten opzichte van de Basic-servicelaag die specifiek zijn afgestemd op Azure Virtual Network-Resources. Beveiligings beleid wordt afgestemd met behulp van toegewezen verkeers bewaking en machine learning-algoritmen. Beveiliging is beschikbaar voor open bare IP-adressen van IPv4 Azure.

|Resource|Koppelen|
|---|---|
|Overzicht van Azure DDoS Protection|[https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)|
|Best practices voor Azure DDoS|[https://docs.microsoft.com/azure/security/azure-ddos-best-practices](https://docs.microsoft.com/azure/security/azure-ddos-best-practices)|
|DDoS Protection beheren|[https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)|
|

### <a name="traffic-manager"></a>Traffic Manager

Traffic Manager wordt gebruikt om binnenkomend verkeer te beheren door te bepalen welke eind punten van een toepassing verbindingen ontvangen. Om te beschermen tegen een verlies van Beschik baarheid van systemen of toepassingen vanwege een Cyber-beveiligings aanval, of om services te herstellen na een inbreuk op het systeem, kan Traffic Manager worden gebruikt om verkeer om te leiden naar werkende, beschik bare toepassings exemplaren.

|Resource|Koppelen|
|---|---|
|Overzicht Traffic Manager | [https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)|
|Herstel na nood geval met behulp van Azure DNS en Traffic Manager gids | [https://docs.microsoft.com/azure/networking/disaster-recovery-dns-traffic-manager](https://docs.microsoft.com/azure/networking/disaster-recovery-dns-traffic-manager)|
|

### <a name="expressroute"></a>ExpressRoute

ExpressRoute kan worden gebruikt om een persoonlijk pad te maken van een on-premises omgeving naar systemen die worden gehost in Azure. Deze verbinding kan betere betrouw baarheid en gegarandeerde prestaties bieden met verbeterde privacy voor netwerk communicatie. Met Express route kunnen gemenebests afdelingen inkomend verkeer beheren vanuit de on-premises omgeving en specifieke adressen definiëren die specifiek zijn voor de organisatie voor het gebruik van binnenkomende firewall regels en toegangs beheer lijsten.

|Resource | Koppelen|
|---|---|
|Overzicht van ExpressRoute | [https://docs.microsoft.com/azure/expressroute/](https://docs.microsoft.com/azure/expressroute/)|
|ExpressRoute-connectiviteits modellen | [https://docs.microsoft.com/azure/expressroute/expressroute-connectivity-models](https://docs.microsoft.com/azure/expressroute/expressroute-connectivity-models)|
|

### <a name="expressroute-private-peering"></a>ExpressRoute persoonlijke peering

Privé-peering biedt een mechanisme voor het uitbreiden van een on-premises omgeving in azure met alleen particuliere IP-adressen. Hierdoor kunnen gemenebests entiteiten Azure Virtual Networks en adresbereiken integreren met bestaande on-premises systemen en services. Privé-peering biedt de zekerheid dat de communicatie tussen ExpressRoute alleen betrekking heeft op virtuele netwerken die door de organisatie worden toegelaten. Als u privé-peering gebruikt, moeten Gemenebests-entiteiten netwerk virtuele apparaten (NVA) implementeren in plaats van Azure VPN Gateway om de veilige VPN-communicatie met uw on-premises netwerken tot stand te brengen zoals vereist door de ACSC-gebruikers richtlijnen.

|Resource | Koppelen|
|---|---|
|Overzicht van ExpressRoute persoonlijke peering | [https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#routingdomains](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#routingdomains)|
|Hand leiding voor ExpressRoute persoonlijke peering | [https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager#private](https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager#private)|
|

### <a name="expressroute-microsoft-peering"></a>ExpressRoute micro soft-peering

Micro soft-peering biedt een snelle verbinding met lage latentie voor open bare services van micro soft zonder dat het internet hoeft te passeren. Dit biedt meer betrouw baarheid, prestaties en privacy voor verbindingen. Met behulp van route filters kunnen gemenebests afdelingen de communicatie beperken tot alleen de Azure-regio's die ze nodig hebben, maar dit omvat services die worden gehost door andere organisaties en kunnen extra filters of inspectie mogelijkheden vereisen tussen de on-premises omgeving en micro soft.

Gemenebests kunnen de toegewezen open bare IP-adressen die via de peering-relatie tot stand worden gebracht, gebruiken om de on-premises omgeving uniek te identificeren voor gebruik in firewalls en toegangs beheer lijsten binnen PaaS-mogelijkheden.

Als alternatief kunnen gemenebests afdelingen gebruikmaken van ExpressRoute micro soft-peering als een aan-netwerk voor het tot stand brengen van VPN-verbindingen via Azure VPN Gateway. In dit model is er geen actieve communicatie van het interne on-premises netwerk naar open bare Azure-Services via ExpressRoute, maar wordt de connectiviteit via particuliere virtuele netwerken gewaarborgd conform de richt lijnen van de ACSC-consument.

|Resource | Koppelen|
|---|---|
|Overzicht van ExpressRoute micro soft-peering | [https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#routingdomains](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#routingdomains)|
|Hand leiding voor micro soft-peering voor ExpressRoute | [https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager#msft](https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager#msft)|
|

## <a name="iaas-ingress"></a>IaaS ingang

Deze sectie bevat de onderdeel richtlijnen voor het beheren van binnenkomend verkeer naar IaaS-onderdelen. IaaS bevat Virtual Machines en andere reken resources die kunnen worden geïmplementeerd en beheerd in een Virtual Network in Azure. Voor verkeer dat kan worden bereikt op systemen die worden geïmplementeerd met IaaS, moet het een ingangs punt hebben voor de Virtual Network, die kan worden ingesteld via een openbaar IP-adres, Virtual Network gateway of Virtual Network peering-relatie.

### <a name="network-interface"></a>Netwerkinterface

Netwerk interfaces zijn de ingangs punten voor al het verkeer naar een virtuele machine. Netwerk interfaces bieden de configuratie van IP-adres sering en kunnen worden gebruikt voor het Toep assen van Nsg's of voor het routeren van verkeer via een virtueel netwerk apparaat. De netwerk interfaces voor Virtual Machines moeten op de juiste wijze worden gepland en geconfigureerd om te worden uitgelijnd met de algehele netwerk segmentatie en de schei ding van de doel stellingen.

|Resource | Koppelen|
|---|---|
|Een netwerk interface maken, wijzigen of verwijderen | [https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface)|
|IP-adres sering van netwerk interface | [https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses)|
|

### <a name="subnet"></a>Subnet

Subnetten zijn een cruciaal onderdeel voor netwerk segmentatie en schei ding binnen Azure. Subnetten kunnen worden gebruikt om een schei ding tussen systemen te bieden. Nsg's kan worden toegepast op subnetten om ingangs communicatie stromen alleen te beperken tot die nood zakelijk zijn voor systeem functionaliteit. Subnetten kunnen worden gebruikt als bron-en doel adressen voor firewall regels en toegangs beheer lijsten en kunnen worden geconfigureerd voor service-eind punten om connectiviteit te bieden aan PaaS-mogelijkheden.

|Resource | Koppelen|
|---|---|
|Een subnet van een virtueel netwerk toevoegen, wijzigen of verwijderen | [https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)|
|

### <a name="virtual-network-vnet"></a>Virtueel netwerk (VNet)

VNets zijn een van de fundamentele bouw stenen voor netwerken in Azure. Met virtuele netwerken kunt u een IP-adres ruimte en een routerings grens definiëren die op verschillende systemen worden gebruikt. Virtuele netwerken worden onderverdeeld in subnetten en alle subnetten in een Virtual Network een directe netwerk route naar elkaar hebben. Door Virtual Network gateways (ExpressRoute of VPN) te gebruiken, kunnen systemen binnen een Virtual Network toegankelijk worden gemaakt voor on-premises en externe omgevingen. Informatie over virtuele netwerken en de bijbehorende configuratie parameters en route ring is van cruciaal belang voor het beheren van binnenkomend netwerk verkeer.

|Resource | Koppelen|
|---|---|
|Overzicht van virtuele netwerken | [https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)|
|Instructies voor het plannen van virtuele netwerken | [https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)|
Een Virtual Network Snelstartgids maken | [https://docs.microsoft.com/azure/virtual-network/quick-create-portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)|
|

### <a name="vnet-peering"></a>VNet-peering

VNet-peering wordt gebruikt om een direct communicatie traject tussen twee virtuele netwerken op te geven. Zodra peering tot stand is gebracht, hebben hosts in een Virtual Network een snelle route ring rechtstreeks naar hosts in een andere Virtual Network. Nsg's nog steeds van toepassing op het verkeer als normale en geavanceerde configuratie parameters kunnen worden gebruikt om te bepalen of communicatie via Virtual Network gateways of van andere externe systemen is toegestaan.

|Resource | Koppelen|
|---|---|
|Overzicht van Virtual Network peering |  [https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)|
|Een peering op een virtueel netwerk maken, wijzigen of verwijderen | [https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering)|
|

### <a name="public-ip-on-vnet"></a>Openbaar IP-adres op VNET

Open bare IP-adressen worden gebruikt om een ingangs communicatie traject te bieden aan services die zijn geïmplementeerd in een Virtual Network. Gemenebests entiteiten moeten de toewijzing van open bare IP-adressen zorgvuldig plannen en deze alleen toewijzen aan resources waarvoor een legitieme vereiste is. Als algemene ontwerp procedure moeten open bare IP-adressen worden toegewezen aan bronnen met ingebouwde beveiligings mogelijkheden, zoals Application Gateway of virtuele netwerk apparaten, om een veilig, beheerd openbaar toegangs punt te bieden voor een Virtual Network.

|Resource | Koppelen|
|---|---|
|Overzicht van Openbare IP-adressen | [https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses)|
|Een openbaar IP-adres maken, wijzigen of verwijderen | [https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)|
|

### <a name="expressroute-gateway"></a>ExpressRoute-gateway

ExpressRoute-gateways bieden een ingangs punt uit de on-premises omgeving en moeten worden geïmplementeerd om te voldoen aan de vereisten voor beveiliging, Beschik baarheid, financiële situatie en prestaties. ExpressRoute-gateways bieden een gedefinieerde netwerk bandbreedte en maken gebruik van de kosten na de implementatie. Virtuele netwerken kunnen slechts één ExpressRoute-gateway hebben, maar dit kan worden verbonden met meerdere ExpressRoute-circuits en kan worden gebruikt door meerdere virtuele netwerken met behulp van VNet-peering, waardoor meerdere virtuele netwerken band breedte en connectiviteit kunnen delen. Zorg ervoor dat de route ring tussen on-premises omgevingen en virtuele netwerken met behulp van ExpressRoute-gateways wordt geconfigureerd om end-to-end connectiviteit te garanderen met bekende, beheerde netwerk ingangs punten. Gemenebests-entiteiten die gebruikmaken van ExpressRoute-gateway, moeten ook virtuele netwerk apparaten implementeren om een VPN-verbinding met de on-premises omgeving tot stand te brengen om te voldoen aan de ACSC-gebruikers richtlijnen.

|Resource | Koppelen|
|---|---|
|Overzicht van ExpressRoute-gateway | [https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways)|
|Een virtuele netwerkgateway configureren voor ExpressRoute | [https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager)|
|

### <a name="vpn-gateway"></a>VPN Gateway

Azure VPN Gateway biedt een ingangs netwerk punt van een extern netwerk voor beveiligde site-naar-site-of punt-naar-site-verbindingen. VPN-gateways bieden een gedefinieerde netwerk bandbreedte en maken gebruik van de kosten na de implementatie. De gemenebests-entiteiten die VPN Gateway, moeten ervoor zorgen dat deze zijn geconfigureerd in overeenstemming met de ACSC-richt lijnen voor consumenten. Virtuele netwerken kunnen slechts één VPN Gateway hebben, maar dit kan worden geconfigureerd met meerdere tunnels en kan worden gebruikt door meerdere virtuele netwerken met behulp van VNet-peering, waardoor meerdere virtuele netwerken band breedte en connectiviteit kunnen delen. VPN-gateways kunnen via internet of via ExpressRoute via micro soft-peering tot stand worden gebracht.

|Resource | Koppelen|
|---|---|
|Overzicht van VPN Gateway | [https://docs.microsoft.com/azure/vpn-gateway/](https://docs.microsoft.com/azure/vpn-gateway/)|
|Planning en ontwerp voor VPN Gateway | [https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)|
|VPN Gateway configuratie voor Australische overheids instanties|[IPSEC-configuratie vereist voor de Australische overheids instellingen](vpn-gateway.md)|
|

### <a name="paas-vnet-integration"></a>PaaS VNet-integratie

Het gebruik van PaaS kan verbeterde functionaliteit en beschik baarheid bieden en de beheer overhead verlagen, maar moet op de juiste manier worden beveiligd. Als u de controle wilt verg Roten, netwerk segmentatie wilt afdwingen of een veilig ingangs punt voor toepassingen en services wilt bieden, kunnen veel PaaS-mogelijkheden worden geïntegreerd met een Virtual Network.

Om een veilig ingangs punt te bieden, kunnen de PaaS-mogelijkheden, zoals Application Gateway, worden geconfigureerd met een externe, open bare interface en een interne, persoonlijke interface voor communicatie met toepassings Services. Hiermee wordt voor komen dat toepassings servers met open bare IP-adressen worden geconfigureerd en beschikbaar worden gemaakt voor externe netwerken.

Micro soft biedt meerdere mechanismen voor het implementeren van PaaS in een Virtual Network om PaaS te gebruiken als een geïntegreerd onderdeel van systeem-of toepassings architectuur. De implementatie methodologie beperkt de inkomende toegang van externe netwerken, zoals het Internet, en biedt connectiviteit en integratie met interne systemen en toepassingen. Voor beelden zijn App Service omgevingen, SQL Managed instances en meer.

|Resource | Koppelen|
|---|---|
|Integratie van virtueel netwerk voor Azure-Services | [https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)|
|Uw app integreren met een Azure Virtual Network hand leiding | [https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)|
|

## <a name="paas-ingress"></a>PaaS ingang

PaaS-mogelijkheden bieden mogelijkheden voor een betere mogelijkheden en een vereenvoudigd beheer, maar voeren complexe waarden uit voor de adres vereisten voor netwerk segmentatie en schei ding. De PaaS-mogelijkheden worden meestal geconfigureerd met open bare IP-adressen en zijn toegankelijk via internet.  Wanneer u systemen bouwt met behulp van PaaS-mogelijkheden, moet u ervoor zorgen dat alle benodigde communicatie stromen tussen onderdelen binnen de systeem-en netwerk beveiligings regels worden geïdentificeerd die alleen deze communicatie toestaan. Als onderdeel van een verdedigings-diep gaande benadering moeten de PaaS-mogelijkheden worden geconfigureerd met versleuteling, authenticatie en de juiste toegangs beheer en-machtigingen.  

### <a name="hostname"></a>Hostnaam

De PaaS-mogelijkheden worden uniek geïdentificeerd door hostnamen, zodat meerdere exemplaren van dezelfde service kunnen worden gehost op hetzelfde open bare IP-adres. Er worden unieke hostnamen opgegeven wanneer resources worden gemaakt en bestaan in micro soft DNS-domeinen in eigendom. De specifieke hostnamen voor geautoriseerde services kunnen worden gebruikt binnen beveiligings hulpprogramma's met filter functies op toepassings niveau. Bepaalde services kunnen ook worden geconfigureerd met aangepaste domeinen als dat nodig is.

|Resource | Koppelen|
|---|---|
|Veel open bare naam ruimten die door Azure-Services worden gebruikt, kunnen worden verkregen via Power shell door de opdracht Get-AzureRMEnvironment uit te voeren | [https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermenvironment](https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermenvironment)|
|Een aangepaste domein naam configureren voor een Azure-Cloud service | App Services en anderen kunnen aangepaste domeinen hebben[https://docs.microsoft.com/azure/cloud-services/cloud-services-custom-domain-name-portal](https://docs.microsoft.com/azure/cloud-services/cloud-services-custom-domain-name-portal)|
|

### <a name="public-ip-for-paas"></a>Openbaar IP-adres voor PaaS

Open bare IP-adressen voor PaaS-mogelijkheden worden toegewezen op basis van de regio waarin de service wordt gehost of geïmplementeerd. Voor het bouwen van de juiste netwerk beveiligings regels en routerings topologie voor netwerk segmentatie en schei ding met virtuele Azure-netwerken, PaaS en ExpressRoute en Internet connectiviteit, is een goed idee van open bare IP-adres toewijzing en regio's Vereist. Azure wijst IP-adressen toe uit een groep die is toegewezen aan elke Azure-regio. Micro soft maakt de adressen die in elke regio worden gebruikt, beschikbaar voor downloaden, die op een regel matige en gecontroleerde manier worden bijgewerkt. De services die beschikbaar zijn in elke regio, veranderen ook vaak als nieuwe services worden vrijgegeven of services worden uitgebreid geïmplementeerd. De gemenebests van deze materialen moeten regel matig worden gecontroleerd en kunnen gebruikmaken van automatisering om systemen waar nodig te onderhouden. Specifieke IP-adressen voor sommige services die in elke regio worden gehost, kunnen worden verkregen door contact op te nemen met micro soft ondersteuning.

|Resource | Koppelen|
|---|---|
|IP-adresbereiken van Microsoft Azure Data Center | [https://www.microsoft.com/download/details.aspx?id=41653](https://www.microsoft.com/download/details.aspx?id=41653)|
|Azure-Services per regio | [https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional, Australië-centraal, Australië-centraal-2, Australië-Oost, Australië-Zuidoost & Products = all](https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional,australia-central,australia-central-2,australia-east,australia-southeast&products=all)|
|

### <a name="service-endpoints"></a>Service-eindpunten

Virtual Network Service-eind punten bieden een snelle, persoonlijke binnengekomen netwerk verbinding voor subnetten binnen een Virtual Network om specifieke PaaS mogelijkheden te gebruiken. Voor een volledige netwerk segmentatie en schei ding van de PaaS-mogelijkheid moet de PaaS-mogelijkheid worden geconfigureerd om alleen verbindingen te accepteren van de benodigde virtuele netwerken. Niet alle PaaS-mogelijkheden ondersteunen een combi natie van firewall regels die service-eind punten en traditionele regels op basis van IP-adres bevatten. Daarom moet worden gezorgd voor een inzicht in de stroom van de communicatie die vereist is voor toepassings functionaliteit en-beheer de implementatie van deze beveiligings controles heeft geen invloed op de beschik baarheid van de service.

|Resource | Koppelen|
|---|---|
|Overzicht van service-eind punten | [https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)
|Zelfstudie |[https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)|
|

## <a name="security"></a>Beveiliging

Het implementeren van de besturings elementen voor netwerk segmentatie en schei ding op IaaS-en PaaS-mogelijkheden wordt bereikt door de mogelijkheden zelf te beveiligen en door beheerde communicatie paden te implementeren van de systemen die zullen communiceren met de voorzieningen.

Het ontwerpen en bouwen van oplossingen in Azure is een proces van het maken van een logische architectuur voor het begrijpen, beheren en bewaken van netwerk bronnen in de hele Azure-aanwezigheid. Deze logische architectuur is software die is gedefinieerd binnen het Azure-platform en neemt de plaats van een fysieke netwerk topologie die in traditionele netwerk omgevingen is geïmplementeerd.

De logische architectuur die wordt gemaakt, moet de functionaliteit bieden die nodig is voor de bruikbaarheid, maar moet ook de zicht baarheid en controle bieden die nodig is voor beveiliging en integriteit.

Het bereiken van deze uitkomst is gebaseerd op het implementeren van de benodigde netwerk segmentatie en hulpprogram ma's voor schei ding, maar ook bij het beveiligen en afdwingen van de netwerk topologie en de implementatie van deze hulpprogram ma's.

De informatie in deze hand leiding kan worden gebruikt om de bronnen van binnenkomend verkeer te identificeren die moeten worden toegestaan en de manieren waarop het verkeer verder kan worden beheerd of beperkt.

### <a name="network-security-groups-nsgs"></a>Netwerk beveiligings groepen (Nsg's)

Nsg's worden gebruikt om het binnenkomende en uitgaande verkeer op te geven dat is toegestaan voor een subnet of een specifieke netwerk interface. Bij het configureren van Nsg's moeten gemenebests-entiteiten een white list-benadering gebruiken waarbij regels worden geconfigureerd voor het toestaan van het benodigde verkeer met een standaard regel die is geconfigureerd om al het verkeer te weigeren dat niet overeenkomt met een specifieke instructie voor toestaan. Er moet rekening worden gehouden bij het plannen en configureren van Nsg's om ervoor te zorgen dat alle benodigde inkomend en uitgaand verkeer op de juiste wijze wordt vastgelegd. Dit omvat het identificeren en goed begrip van alle privé-IP-adresbereiken in azure Virtual Networks en de on-premises omgeving, en specifieke micro soft-Services, zoals Azure Load Balancer en PaaS beheer vereisten. Personen die betrokken zijn bij het ontwerp en de implementatie van netwerk beveiligings groepen moeten ook inzicht krijgen in het gebruik van service tags en toepassings beveiligings groepen voor het maken van nauw keurige, service-en toepassingsspecifieke beveiligings regels.

|Resource | Koppelen|
|---|---|
|Overzicht van netwerk beveiliging | [https://docs.microsoft.com/azure/virtual-network/security-overview](https://docs.microsoft.com/azure/virtual-network/security-overview)
|Een netwerk beveiligings groep maken, wijzigen of verwijderen | [https://docs.microsoft.com/azure/virtual-network/manage-network-security-group](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)|
|

## <a name="paas-firewall"></a>PaaS-firewall

Een PaaS-firewall is een netwerk toegangscontrole functie die kan worden toegepast op bepaalde PaaS-Services. Hiermee kunnen IP-adressen worden gefilterd of gefilterd van specifieke virtuele netwerken, zodat ingangs verkeer naar het specifieke PaaS-exemplaar wordt beperkt. Voor PaaS-mogelijkheden met een firewall moet het netwerk toegangscontrole beleid zodanig worden geconfigureerd dat alleen het vereiste binnenkomend verkeer op basis van de toepassings vereisten wordt toegestaan.  

|Resource | Koppelen|
|---|---|
|Azure SQL Database en SQL Data Warehouse IP-firewall regels | [https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)|
|Beveiliging van opslag netwerk | [https://docs.microsoft.com/azure/storage/common/storage-network-security](https://docs.microsoft.com/azure/storage/common/storage-network-security)|
|

## <a name="paas-authentication-and-access-control"></a>Verificatie en toegangs beheer van PaaS

Afhankelijk van de functionaliteit van de PaaS en het doel ervan, is het gebruik van netwerk besturings elementen om de toegang te beperken mogelijk niet mogelijk of praktisch. Als onderdeel van het gelaagde beveiligings model voor PaaS biedt Azure diverse mechanismen voor verificatie en toegangs beheer om de toegang tot een service te beperken, zelfs als het netwerk verkeer is toegestaan. Typische verificatie mechanismen voor PaaS-mogelijkheden zijn Azure Active Directory, verificatie op toepassings niveau en gedeelde sleutels of toegangs handtekeningen. Zodra een gebruiker veilig is geïdentificeerd, kunnen rollen worden benut om de acties te beheren die de gebruiker kan uitvoeren. Deze hulpprogram ma's kunnen worden aangewend als een alternatieve of als een gratis meting om de toegang tot services te beperken.

|Resource | Koppelen|
|---|---|
|Database toegang beheren en verlenen aan SQL Database en SQL Data Warehouse | [https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins)|
|Autorisatie voor de Azure Storage services | [https://docs.microsoft.com/rest/api/storageservices/authorization-for-the-Azure-Storage-Services](https://docs.microsoft.com/rest/api/storageservices/authorization-for-the-Azure-Storage-Services)|
|

## <a name="azure-policy"></a>Azure-beleid

Azure Policy is een belang rijk onderdeel voor het afdwingen en onderhouden van de integriteit van de logische architectuur van de Azure-omgeving. Op basis van de verschillende services en binnenkomende netwerk verkeer paden die beschikbaar zijn via Azure-Services, is het van cruciaal belang dat de resources die zich in hun omgeving bevinden en de beschik bare netwerk ingangs punten op de hoogte zijn. Om ervoor te zorgen dat niet-geautoriseerde netwerk ingangs punten niet worden gemaakt in de Azure-omgeving, moeten Gemenebests entiteiten gebruikmaken van Azure Policy om de typen resources te beheren die kunnen worden geïmplementeerd en de configuratie van die bronnen. Praktische voor beelden zijn het beperken van resources tot alleen degenen die zijn toegestaan en goedgekeurd voor gebruik, waarbij HTTPS-versleuteling wordt afgedwongen voor opslag en Nsg's moet worden toegevoegd aan subnetten.

|Resource | Koppelen|
|---|---|
|Overzicht van Azure Policy | [https://docs.microsoft.com/azure/governance/policy/overview](https://docs.microsoft.com/azure/governance/policy/overview)|
|Voor beeld van toegestane resource typen | [https://docs.microsoft.com/azure/governance/policy/samples/allowed-resource-types](https://docs.microsoft.com/azure/governance/policy/samples/allowed-resource-types)
|Een voor beeld van een HTTPS-opslag account garanderen|[https://docs.microsoft.com/azure/governance/policy/samples/ensure-https-storage-account](https://docs.microsoft.com/azure/governance/policy/samples/ensure-https-storage-account)_
|NSG afdwingen op een voor beeld van een subnet| [https://docs.microsoft.com/azure/governance/policy/samples/nsg-on-subnet](https://docs.microsoft.com/azure/governance/policy/samples/nsg-on-subnet)|
|

## <a name="next-steps"></a>Volgende stappen

Raadpleeg het artikel over het beheer van uitvoer verkeer van de [gateway en beheer](gateway-egress-traffic.md) voor meer informatie over het beheren van verkeers stromen van uw Azure-omgeving naar andere netwerken die gebruikmaken van uw gateway onderdelen in Azure.
