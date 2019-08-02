---
title: Uitgaand verkeer beheren in azure Australië
description: Belangrijkste elementen voor het beheren van uitgaand verkeer in azure om te voldoen aan de vereisten van de Australische overheid voor beveiligde Internet gateways
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: grgale
ms.openlocfilehash: 7922846d161b7a0cbda32101b6bbb40a1741b323
ms.sourcegitcommit: 15f7b641a67f3d6cf4fb4b4c11eaee18cf335923
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602085"
---
# <a name="controlling-egress-traffic-in-azure-australia"></a>Uitgaand verkeer beheren in azure Australië

Een fundamenteel onderdeel van het beveiligen van ICT-systemen is het beheren van het netwerk verkeer. Het beperken van de communicatie met alleen het verkeer dat nodig is om een systeem te laten functioneren, verkleint de kans op inbreuk. Zicht baarheid en controle over de externe systemen waarmee uw toepassingen en services communiceren met helpt beschadigde systemen te detecteren, en poging tot het uitvoeren van gegevens exfiltration. Dit artikel bevat informatie over hoe uitgaand netwerk verkeer binnen Azure werkt en geeft aanbevelingen voor het implementeren van netwerk beveiligings controles voor een Internet verbonden systeem dat wordt afgestemd op het Australische Cyber Security Center (ACSC) Gebruikers richtlijnen en het doel van de ACSC Information Security-hand leiding (ISM).

## <a name="requirements"></a>Vereisten

De algemene beveiligings vereisten voor de Gemenebests systemen worden gedefinieerd in de ISM. Voor de ondersteuning van gemenebests entiteiten bij het implementeren van netwerk beveiliging _heeft de ACSC ACSC-beveiliging gepubliceerd: Implementatie van netwerk segmentatie en_-schei ding, en om te helpen bij het beveiligen van systemen in Cloud omgevingen, heeft de ACSC _Cloud Computing Security voor tenants_gepubliceerd.

Het ACSC-document bevat een overzicht van de context voor het implementeren van netwerk beveiliging en het beheren van verkeer, en praktische aanbevelingen voor het ontwerpen en configureren van het netwerk.

De volgende belang rijke vereisten voor het beheren van uitgaand verkeer in azure zijn geïdentificeerd in de ACSC-documenten.

Description|Source
--------------- |------------------
**Implementeer netwerk segmentatie en schei ding**, bijvoorbeeld een architectuur met n-tier, met behulp van op hosts gebaseerde firewalls en netwerk toegangs beheer om de inkomende en uitgaande netwerk verbinding te beperken tot alleen de vereiste poorten en protocollen.| [Cloud computing voor tenants](https://acsc.gov.au/publications/protect/cloud-security-tenants.htm)
**Implementeer voldoende hoge band breedte, lage latentie, betrouw bare netwerk connectiviteit** tussen de Tenant (inclusief de externe gebruikers van de Tenant) en de Cloud service om te voldoen aan de beschikbaarheids vereisten van de Tenant  | [ACSC beveiligen: Netwerk segmentatie en schei ding implementeren](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
**Pas technologieën toe op meer dan alleen de netwerklaag**. Elke host en elk netwerk moeten, waar mogelijk, worden gesegmenteerd en gescheiden op het laagste niveau dat nagenoeg kan worden beheerd. In de meeste gevallen is dit van toepassing vanuit de gegevenskoppelings-laag tot en met de toepassingslaag; in gevoelige omgevingen is het echter mogelijk dat de fysieke isolatie geschikt is. Op de host gebaseerde en netwerk-brede maat regelen moeten op een complementaire manier worden geïmplementeerd en centraal worden bewaakt. U hoeft alleen maar een firewall of beveiligings apparaat te implementeren als de enige beveiligings maatregel niet voldoende is. |[ACSC beveiligen: Netwerk segmentatie en schei ding implementeren](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
**Gebruik de beginselen van de minimale bevoegdheid en u hebt ‐ nodig om ‐ te weten**. Als een host, service of netwerk niet met een andere host, service of netwerk hoeft te communiceren, mag deze niet worden toegestaan. Als een host, service of netwerk alleen hoeft te communiceren met een andere host, service of netwerk op een specifieke poort of protocol, moet deze beperkt zijn tot alleen die poorten en protocollen. Het nemen van deze principes in een netwerk is een aanvulling op de minimale gebruikers bevoegdheden en verhoogt de algehele beveiligings postuur van de omgeving aanzienlijk. |[ACSC beveiligen: Netwerk segmentatie en schei ding implementeren](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
**Scheid hosts en netwerken op basis van hun gevoeligheid of kritiekheid voor zakelijke activiteiten**. Dit kan ook worden gebruikt voor het gebruik van verschillende hardware of platforms, afhankelijk van verschillende beveiligings classificaties, beveiligings domeinen of Beschik baarheid/integriteits vereisten voor bepaalde hosts of netwerken. Met name afzonderlijke beheer netwerken en overwegen fysiek isoleren van out-of-band-beheer netwerken voor gevoelige omgevingen. |[ACSC beveiligen: Netwerk segmentatie en schei ding implementeren](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
**U kunt de toegang van alle entiteiten aan alle andere entiteiten identificeren, verifiëren en toestaan**. Alle gebruikers, hosts en services moeten hun toegang hebben tot alle gebruikers, hosts en services die alleen zijn toegestaan voor de taken of functies die nodig zijn voor het uitvoeren van hun specifieke taak. Alle verouderde of lokale services die de sterkte van de identificatie-, authenticatie-en autorisatie Services negeren of downgradeen, moeten waar mogelijk worden uitgeschakeld en hun gebruik nauw keurig worden gecontroleerd. |[ACSC beveiligen: Netwerk segmentatie en schei ding implementeren](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
**Implementatie van netwerk verkeer toestaan in plaats van vermelding weigeren**. Sta alleen toegang toe voor bekend goed netwerk verkeer (verkeer dat is geïdentificeerd, geverifieerd en geautoriseerd), in plaats van toegang tot bekende beschadigde netwerk verkeer te weigeren (bijvoorbeeld door een specifiek adres of service te blok keren). Lijsten toestaan hebben als resultaat een superieure beveiligings beleid voor het weigeren van lijsten, waardoor de capaciteit aanzienlijk wordt verbeterd om mogelijke netwerk indringers te detecteren en te beoordelen. |[ACSC beveiligen: Netwerk segmentatie en schei ding implementeren](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
Als u **een lijst met toegestane websites definieert en alle niet-vermelde websites blokkeert,** wordt een van de meest voorkomende technieken voor het leveren van gegevens en exfiltration die door een kwaadwillende persoon worden gebruikt, verwijderd. Als gebruikers een rechtmatige vereiste hebben om toegang te krijgen tot talloze websites of een snel veranderende lijst met websites; u moet rekening houden met de kosten van een dergelijke implementatie. Zelfs een relatief strikte lijst met toegestane machtigingen biedt betere beveiliging dan het gebruik van deny-lijsten of geen beperkingen, terwijl de implementatie kosten nog steeds worden verminderd. Een voor beeld van een lijst met toegestane toestaan kan worden toegestaan voor het hele Australische subdomein, dat is ' *. au ', of het toestaan van de populairste 1.000-sites van de Alexa site (na het filteren van dynamische Domain Name System (DDNS)-domeinen en andere ongepaste domeinen).| [Australische overheid Information hand matige beveiliging (ISM)](https://www.cyber.gov.au/ism)
|

In dit artikel vindt u informatie en aanbevelingen over hoe netwerk verkeer uw Azure-omgeving laat worden beheerd. Het behandelt systemen die in azure zijn geïmplementeerd met behulp van IaaS (Infrastructure as a Service) en platform as a Service (PaaS).

Het artikel verkeer van de [Gateway](gateway-ingress-traffic.md) heeft betrekking op het netwerk verkeer dat uw Azure-omgeving binnenkomt en is de aanvulling op dit artikel voor volledige netwerk beheer dekking.

## <a name="architecture"></a>Architectuur

Als u het uitgaande verkeer op de juiste manier wilt beheren, moet u eerst begrijpen hoe uitgaand netwerk verkeer binnen Azure werkt in zowel IaaS als PaaS, wanneer u netwerk beveiliging ontwerpt en implementeert. In deze sectie wordt een overzicht gegeven van hoe uitgaand verkeer dat wordt gegenereerd door een resource die wordt gehost in azure wordt verwerkt, en welke beveiligings controles beschikbaar zijn om het verkeer te beperken en te beheren.

### <a name="architecture-components"></a>Architectuur onderdelen

In het weer gegeven architectuur diagram ziet u de mogelijke paden die netwerk verkeer kan ondervinden bij het afsluiten van een systeem dat is geïmplementeerd in een subnet in een virtueel netwerk. Verkeer in een virtueel netwerk wordt beheerd en geregeld op subnetniveau, met routerings-en beveiligings regels die van toepassing zijn op de resources in. De onderdelen die betrekking hebben op uitgaand verkeer, worden onderverdeeld in systemen, actieve routes, typen van de volgende hop, beveiligings controles en uitkomende PaaS.

![Architectuur](media/egress-traffic.png)

### <a name="systems"></a>Remsystemen

Systemen zijn de Azure-resources en gerelateerde onderdelen die uitgaand verkeer genereren binnen een IP-subnet dat deel uitmaakt van een virtueel netwerk.

| Onderdeel | Description |
| --- | ---|
|Virtueel netwerk (VNet) | Een VNet is een kern bron binnen Azure die een platform en grens biedt voor het implementeren van resources en het inschakelen van communicatie. Het VNet bevindt zich in een Azure-regio en definieert de IP-adres ruimte en routerings grenzen voor VNet-geïntegreerde resources zoals Virtual Machines.|
|Subnet | Een subnet is een IP-adres bereik dat is gemaakt binnen een VNet. Er kunnen meerdere subnetten worden gemaakt binnen een VNet voor netwerk segmentatie.|
|Netwerkinterface| Een netwerk interface is een resource die zich in azure bevindt. Het is gekoppeld aan een virtuele machine en er is een persoonlijk, niet door Internet routeerbaar IP-adres toegewezen van het subnet waaraan het is gekoppeld. Dit IP-adres is dynamisch of statisch toegewezen via Azure Resource Manager.|
|Openbare IP-adressen| Een openbaar IP-adres is een bron die een van de door micro soft in eigendom zijnde, Internet Routeer bare IP-adressen van de opgegeven regio voor gebruik in het virtuele netwerk reserveert. Het kan worden gekoppeld aan een specifieke netwerk-of PaaS-bron, waardoor de bron kan communiceren met het Internet, ExpressRoute en andere PaaS-systemen.|
|

### <a name="routes"></a>Routes

Het pad dat uitkomt van het uitgaande verkeer, is afhankelijk van de doel matige routes voor die bron. Dit is de resulterende verzameling routes die wordt bepaald door de combi natie van routes van alle mogelijke bronnen en de toepassing van de logica van Azure-route ring.

| Onderdeel | Description |
|--- | ---|
|Systeemroutes| Azure maakt automatisch systeemroutes en wijst de routes toe aan elk subnet in een virtueel netwerk. Systeem routes kunnen niet worden gemaakt of verwijderd, maar sommige kunnen worden overschreven door aangepaste routes. Azure maakt standaard systeem routes voor elk subnet en voegt extra optionele standaard routes toe aan specifieke subnetten, of elk subnet, wanneer er specifieke Azure-mogelijkheden worden benut.|
|Service-eindpunten| Service-eind punten bieden een directe, persoonlijke uitgaande verbinding van een subnet naar een specifieke PaaS-mogelijkheid. Service-eind punten, die alleen beschikbaar zijn voor een subset van PaaS-mogelijkheden, bieden betere prestaties en beveiliging voor bronnen in een VNet dat toegang krijgt tot PaaS.|
|Routetabellen| Een route tabel is een resource in azure die kan worden gemaakt om door de gebruiker gedefinieerde routes (Udr's) op te geven die systeem routes of routes kunnen aanvullen of negeren via Border Gateway Protocol. Elk UDR geeft een netwerk, een netwerk masker en een volgende hop. Een route tabel kan worden gekoppeld aan een subnet en dezelfde route tabel kan aan meerdere subnetten worden gekoppeld, maar een subnet kan slechts nul of één route tabel hebben.|
|Border Gateway Protocol (BGP)| BGP is een inter-autonoom systeem routering protocol. Een autonoom systeem is een netwerk of groep netwerken onder een gemeen schappelijk beheer en met een gemeen schappelijk routerings beleid. BGP wordt gebruikt voor het uitwisselen van routerings gegevens tussen autonome systemen. BGP kan worden geïntegreerd in virtuele netwerken via virtuele netwerk gateways.|
|

### <a name="next-hop-types-defined"></a>Typen voor de volgende hop gedefinieerd

Elke route in azure omvat het netwerk bereik en het bijbehorende subnetmasker en de volgende hop, waarmee wordt bepaald hoe het verkeer wordt verwerkt.

Volgend hoptype | Description
---- | ----
**Virtueel netwerk** | Routeert verkeer tussen adresbereiken binnen de adres ruimte van een virtueel netwerk. Azure maakt een route met een adresvoorvoegsel dat overeenkomt met elk-adresbereik dat is gedefinieerd in de adresruimte van een virtueel netwerk. Als voor de adres ruimte van het virtuele netwerk meerdere adresbereiken zijn gedefinieerd, maakt Azure een afzonderlijke route voor elk adres bereik. Azure routeert automatisch verkeer tussen subnetten binnen een VNet met behulp van de routes die zijn gemaakt voor elk adres bereik.
**VNet-peering** | Wanneer een virtuele netwerk peering tussen twee virtuele netwerken wordt gemaakt, wordt er een route toegevoegd voor elk adres bereik van elk virtueel netwerk naar het virtuele netwerk waaraan het is gekoppeld. Verkeer wordt gerouteerd tussen de gekoppelde virtuele netwerken op dezelfde manier als subnetten binnen een virtueel netwerk.
**Gateway van virtueel netwerk** | Een of meer routes met een virtuele netwerk gateway die wordt vermeld als het volgende hop-type worden toegevoegd wanneer een virtuele netwerk gateway wordt toegevoegd aan een virtueel netwerk. De routes die zijn opgenomen, zijn die zijn geconfigureerd in de lokale netwerk gateway resource en alle routes die via BGP worden beschreven.
**Virtueel apparaat** | Een virtueel apparaat voert doorgaans een netwerk toepassing uit, zoals een firewall. Het virtuele apparaat biedt extra verwerking van het verkeer, zoals filteren, inspectie of adres omzetting. Elke route met het hop-type van het virtuele apparaat moet ook een IP-adres van de volgende hop opgeven.
**VirtualNetworkServiceEndpoint** | De open bare IP-adressen voor een specifieke service worden toegevoegd als routes naar een subnet met een volgende hop van VirtualNetworkServiceEndpoint wanneer een service-eind punt is ingeschakeld. Service-eind punten zijn ingeschakeld voor afzonderlijke services op afzonderlijke subnetten binnen een virtueel netwerk. De openbare IP-adressen van Azure-services worden periodiek gewijzigd. Azure beheert de adressen in de routetabel automatisch als de adressen worden gewijzigd.
**Internet** | Verkeer met een volgende hop van Internet zal het virtuele netwerk afsluiten en automatisch worden omgezet naar een openbaar IP-adres uit een dynamische groep die beschikbaar is in de gekoppelde Azure-regio, of door gebruik te maken van een openbaar IP-adres dat voor die bron is geconfigureerd. Als het doel adres voor een van de services van Azure is, wordt het verkeer rechtstreeks naar de service gerouteerd via het backbone-netwerk van Azure, in plaats van het verkeer naar Internet te routeren. Verkeer tussen Azure-services loopt niet via internet, ongeacht de Azure-regio waarin het virtuele netwerk zich bevindt of in welke Azure-regio een instantie van de Azure-service is geïmplementeerd.
**Geen** | Verkeer met de volgende hop van none wordt verwijderd. Azure maakt standaard systeem routes voor gereserveerde adres voorvoegsels met geen als type van de volgende hop. Routes met een volgende hop van geen kunnen ook worden toegevoegd met behulp van route tabellen om te voor komen dat verkeer wordt doorgestuurd naar specifieke netwerken.
|

### <a name="security-controls"></a>Beveiligings controles

Beheer | Description
----- | -----
**Netwerk beveiligings groepen (Nsg's)** | Nsg's beheert het verkeer naar en van virtuele netwerk bronnen in Azure. Nsg's regels Toep assen voor de verkeers stromen die zijn toegestaan of geweigerd, waaronder verkeer binnen Azure en tussen Azure en externe netwerken, zoals on-premises of het internet. Nsg's worden toegepast op subnetten binnen een virtueel netwerk of op afzonderlijke netwerk interfaces.
**Azure Firewall** | Azure Firewall is een beheerde, Cloud service voor netwerk beveiliging die virtuele Azure-netwerk bronnen beveiligt. Het is een volledige stateful firewall als een service met ingebouwde hoge beschikbaarheid en onbeperkte cloudschaalbaarheid. Azure Firewall kan worden geconfigureerd met traditionele regels voor netwerk filtering op basis van IP-adressen, protocollen en poorten, maar biedt ook ondersteuning voor filteren op basis van FQDN (Fully Qualified Domain names), service tags en inbouwende bedreigings informatie.
**Virtueel netwerk apparaat (NVA)** | Virtuele netwerk apparaten zijn virtuele-machine media die netwerken, beveiliging en andere functies kunnen bieden aan Azure. Nva's biedt ondersteuning voor netwerk functionaliteit en-services in de vorm van Vm's in virtuele netwerken en implementaties. Nva's kan worden gebruikt om specifieke vereisten te verhelpen, te integreren met beheer-en operationele hulpprogram ma's, of om consistentie te bieden met bestaande producten. Azure ondersteunt een grote hoeveelheid netwerkapparaten van derden, waaronder Web Application Firewall (WAF), firewalls, gateways/routers, Application Delivery Controller (ADC) en WAN-optimalisatoren.
**Service-eindpunt beleid (preview-versie)** | Met het eindpunt beleid van de virtuele netwerk service kunt u virtueel netwerk verkeer filteren op Azure-Services, zodat alleen specifieke Azure-service resources worden uitgevoerd via service-eind punten. Beleid voor eindpunten zorgt voor nauwkeurig toegangsbeheer voor verkeer van Virtual Network naar Azure-services.
**Azure Policy** | Azure Policy is een service in azure voor het maken, toewijzen en beheren van beleid. Dit beleid gebruikt regels voor het beheren van de typen resources die kunnen worden geïmplementeerd en de configuratie van deze resources. Beleids regels kunnen worden gebruikt om naleving af te dwingen door te voor komen dat bronnen worden geïmplementeerd als ze niet aan de vereisten voldoen of kunnen worden gebruikt voor bewaking om te rapporteren over de nalevings status.
|

### <a name="paas-egress"></a>PaaS uitgang

Het meren deel van PaaS-resources genereert geen uitgaand verkeer, maar reageert op binnenkomende aanvragen (zoals een Application Gateway, opslag, SQL Database enz.) of door sturen van gegevens van andere bronnen (zoals Service Bus en Azure Relay). De netwerk communicatie stromen tussen PaaS-bronnen, zoals App Services naar opslag-of SQL-data bases, worden beheerd en opgenomen in Azure en beveiligd met behulp van identiteits-en andere configuratie besturings elementen voor bronnen in plaats van netwerk segmentatie of schei ding.

PaaS-resources die zijn geïmplementeerd in een virtueel netwerk ontvangen specifieke IP-adressen en zijn onderhevig aan routerings besturings elementen en Nsg's op dezelfde manier als andere resources in het virtuele netwerk. PaaS resources die niet bestaan in een virtueel netwerk, maken gebruik van een pool met IP-adressen die worden gedeeld door alle exemplaren van de resource, die zijn gepubliceerd via micro soft-documentatie of kunnen worden bepaald door Azure Resource Manager.

## <a name="general-guidance"></a>Algemene richtlijnen

Als u beveiligde oplossingen wilt ontwerpen en bouwen in azure, is het belang rijk om het netwerk verkeer te begrijpen en te beheren zodat alleen geïdentificeerde en geautoriseerde communicatie kan optreden. De bedoeling van deze richt lijnen en de specifieke richt lijnen voor onderdelen in latere secties is het beschrijven van de hulpprogram ma's en services die kunnen worden toegepast voor het Toep assen van de [principes die worden beschreven in de ACSC-beveiliging: Implementatie van netwerk segmentering en schei ding](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm) over Azure-workloads. Dit omvat informatie over het maken van een virtuele architectuur voor het beveiligen van resources wanneer het niet mogelijk is om dezelfde traditionele fysieke en netwerk controles toe te passen die mogelijk zijn in een on-premises omgeving.

### <a name="guidance"></a>Richtlijnen

* Het aantal uitgangs punten voor virtuele netwerken beperken
* De standaard route van het systeem onderdrukken voor alle subnetten die geen directe uitgaande communicatie met internet nodig hebben
* Ontwerp en implementeer een volledige netwerk architectuur om alle ingangs-en uitgangs punten voor Azure-resources te identificeren en te beheren
* Overweeg het gebruik van een hub-en spoke-netwerk ontwerp voor virtuele netwerken zoals beschreven in de micro soft Virtual Data Center (VDC)-documentatie
* Gebruik producten met ingebouwde beveiligings mogelijkheden voor uitgaande verbindingen met internet (bijvoorbeeld Azure Firewall, virtuele netwerk apparaten of Web-Proxy's)
* Gebruik identiteits controles zoals op rollen gebaseerde toegang, voorwaardelijke toegang en multi-factor Authentication (MFA) om de netwerk configuratie bevoegdheden te beperken
* Implementeer vergren deling om te voor komen dat belang rijke elementen van de netwerk configuratie worden gewijzigd of verwijderd
* Implementeer PaaS in een VNet-geïntegreerde configuratie voor meer schei ding en controle
* ExpressRoute implementeren voor connectiviteit met on-premises netwerken
* Vpn's implementeren voor integratie met externe netwerken
* Gebruik Azure Policy om de regio's en resources te beperken tot alleen die nood zakelijk zijn voor systeem functionaliteit
* Azure Policy voor het afdwingen van basislijn beveiligings configuratie voor resources
* Gebruik Network Watcher en Azure Monitor voor logboek registratie, controle en zicht baarheid van netwerk verkeer binnen Azure

### <a name="resources"></a>Resources

Item | Koppelen
-----------| ---------
_Documenten voor Australische regelgeving en beleids naleving met inbegrip van de gebruikers handleiding_ | [https://aka.ms/au-irap](https://aka.ms/au-irap)
_Azure Virtual Data Center_ | [https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter)
_ACSC-netwerk segmentatie_ | [https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)
_ACSC Cloud beveiliging voor tenants_ | [https://acsc.gov.au/publications/protect/cloud-security-tenants.htm](https://acsc.gov.au/publications/protect/cloud-security-tenants.htm)
_Hand matig ACSC Information Security_ | [https://acsc.gov.au/infosec/ism/index.htm](https://acsc.gov.au/infosec/ism/index.htm)
|

## <a name="component-guidance"></a>Onderdeel richtlijnen

Deze sectie bevat verdere richt lijnen voor de afzonderlijke onderdelen die relevant zijn voor het uitbrengen van verkeer voor systemen die zijn geïmplementeerd in Azure. In elke sectie wordt het doel van het specifieke onderdeel beschreven met koppelingen naar documentatie en configuratie handleidingen die kunnen worden gebruikt om te helpen bij het ontwerpen en bouwen van activiteiten.

### <a name="systems-security"></a>Systeem beveiliging

Alle communicatie met resources binnen Azure loopt via de beheerde netwerk infrastructuur van micro soft, die connectiviteit en beveiligings functionaliteit biedt. Micro soft plaatst automatisch een bereik van beveiligingen ter bescherming van het Azure-platform en de netwerk infrastructuur en er zijn extra mogelijkheden beschikbaar als Services in azure om netwerk verkeer te beheren en netwerk segmentatie in te stellen en scheiding.

### <a name="virtual-network-vnet"></a>Virtueel netwerk (VNet)

Virtuele netwerken zijn een van de fundamentele bouw stenen voor netwerken in Azure. Met virtuele netwerken kunt u een IP-adres ruimte en een routerings grens definiëren die op verschillende systemen worden gebruikt. Virtuele netwerken worden onderverdeeld in subnetten en alle subnetten in een Virtual Network een directe netwerk route naar elkaar hebben. Met behulp van virtuele netwerk gateways (ExpressRoute of VPN) kunnen systemen binnen een virtueel netwerk worden geïntegreerd met on-premises en externe omgevingen en via Azure meegeleverde NAT (Network Address Translation) en toewijzing van open bare IP-adressen. systemen kunnen Maak verbinding met internet of andere Azure-regio's en-services. Informatie over virtuele netwerken en de bijbehorende configuratie parameters en route ring is van cruciaal belang voor het beheren van uitgaand netwerk verkeer.

Als virtuele netwerken vormen de basis adres ruimte en de grenzen van de route ring binnen Azure en kunnen ze worden gebruikt als een primaire bouw steen van netwerk segmentatie en schei ding.

| Resource | Koppelen |
| --- | --- |
| *Overzicht van virtuele netwerken* | [https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) |
| *Instructies voor het plannen van virtuele netwerken*  | [https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm) |
| *Een Virtual Network Snelstartgids maken* | [https://docs.microsoft.com/azure/virtual-network/quick-create-portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)
|

### <a name="subnet"></a>Subnet

Subnetten zijn een cruciaal onderdeel voor netwerk segmentatie en schei ding binnen Azure. Subnetten kunnen worden gebruikt om een schei ding tussen systemen mogelijk te maken. Een subnet is de resource in een virtueel netwerk waar Nsg's, route tabellen en service-eind punten worden toegepast. Subnetten kunnen worden gebruikt als bron-en doel adressen voor firewall regels en toegangs beheer lijsten.

De subnetten binnen een virtueel netwerk moeten worden gepland om te voldoen aan de vereisten van werk belastingen en systemen. Personen die betrokken zijn bij het ontwerp of de implementatie van subnetten, moeten de ACSC-richt lijnen voor netwerk segmentatie raadplegen om te bepalen hoe systemen moeten worden gegroepeerd in een subnet.

|Resource|Koppelen|
|---|---|
|*Een subnet van een virtueel netwerk toevoegen, wijzigen of verwijderen* | [https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)
|

### <a name="network-interface"></a>Netwerkinterface

Netwerk interfaces zijn de bron voor alle uitgaand verkeer van een virtuele machine. Netwerk interfaces bieden de configuratie van IP-adres sering en kunnen worden gebruikt voor het Toep assen van Nsg's of voor het routeren van verkeer via een NVA. De netwerk interfaces voor virtuele machines moeten op de juiste wijze worden gepland en geconfigureerd om te worden uitgelijnd met de algemene netwerk segmentatie en schei ding van de doel stellingen.

|Resource|Koppelen|
|---|---|
|*Een netwerk interface maken, wijzigen of verwijderen* | [https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface) |
|*IP-adres sering van netwerk interface*               | [https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses)
|

### <a name="vnet-integrated-paas"></a>Met VNet geïntegreerde PaaS

PaaS kan verbeterde functionaliteit en beschik baarheid bieden en de beheer overhead verlagen, maar moet op de juiste manier worden beveiligd. Om de controle te verg Roten, netwerk segmentatie af te dwingen of een veilig uitgangs punt te bieden voor toepassingen en services, kunnen veel PaaS-mogelijkheden worden geïntegreerd met een virtueel netwerk.

Micro soft biedt met behulp van PaaS als geïntegreerd onderdeel van systeem-of toepassings architectuur meerdere mechanismen voor het implementeren van PaaS in een virtueel netwerk. De implementatie methodologie kan de toegang helpen beperken tijdens de connectiviteit en integratie met interne systemen en toepassingen. Voor beelden zijn App Service omgevingen, SQL Managed instances en meer.

Bij het implementeren van PaaS in een virtueel netwerk waarop Routing and NSG Controls zijn geïmplementeerd, is het belang rijk om inzicht te krijgen in de specifieke communicatie vereisten van de resource, waaronder beheer toegang van micro soft-Services en het pad dat communicatie verkeer wordt uitgevoerd bij het beantwoorden van binnenkomende aanvragen van deze services.

| Resource  | Koppelen  |
| --- | --- |
| *Integratie van virtuele netwerken voor Azure-services* | [https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services) |
| *Uw app integreren met een Azure Virtual Network hand leiding* | [https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)
|

### <a name="public-ip"></a>Openbaar IP

Open bare IP-adressen worden gebruikt voor communicatie buiten een virtueel netwerk. Dit omvat PaaS bronnen en alle routes met een volgende hop van Internet. Gemenebests entiteiten moeten de toewijzing van open bare IP-adressen zorgvuldig plannen en deze alleen toewijzen aan resources waarvoor een legitieme vereiste is. Als algemene ontwerp procedure moeten open bare IP-adressen worden toegewezen aan beheerde uitgangs punten voor het virtuele netwerk, zoals Azure Firewall, VPN Gateway of virtuele netwerk apparaten.

|Resource|Koppelen|
|---|---|
|*Overzicht van Openbare IP-adressen*  | [https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses) |
|*Een openbaar IP-adres maken, wijzigen of verwijderen* | [https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)
|

## <a name="effective-routes"></a>Effectieve routes

Efficiënte routes zijn de resulterende verzameling routes die worden bepaald door de combi natie van systeem routes, service-eind punten, route tabellen en BGP en de toepassing van de Azure-routerings logica. Als er uitgaand verkeer wordt verzonden vanuit een subnet, selecteert Azure een route op basis van het doel-IP-adres, met behulp van een algoritme voor het matchen van het langste voorvoegsel. Als meerdere routes hetzelfde adresvoorvoegsel bevatten, selecteert Azure het routetype op basis van de volgende prioriteit:

1. Door de gebruiker gedefinieerde route
2. BGP-route
3. Systeemroute

Het is belang rijk te weten dat systeem routes voor verkeer dat is gerelateerd aan virtuele netwerken, peerings voor virtuele netwerken of service-eind punten van virtuele netwerken, voorkeurs routes zijn, zelfs als BGP-routes specifieker zijn.

Personen die betrokken zijn bij het ontwerp of de implementatie van routerings topologieën in azure, moeten begrijpen hoe Azure verkeer routeert en een architectuur ontwikkelt waarmee de benodigde functionaliteit van systemen wordt gebalanceerd met de vereiste beveiliging en zicht baarheid. Daarom moet worden getracht de omgeving op de juiste wijze te plannen om buitensporige interventies en uitzonde ringen voor routerings gedrag te voor komen, omdat dit de complexiteit verg root en het oplossen van problemen en fout opsporing moeilijker en tijdrovend kan zijn.

| Resource | Koppelen  |
| --- | --- |
| *Efficiënte routes weer geven* | <https://docs.microsoft.com/azure/virtual-network/manage-route-table#view-effective-routes> |
|

### <a name="system-routes"></a>Systeemroutes

Voor [systeem routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#system-routes)moeten personen die betrokken zijn bij het ontwerp of de implementatie van virtuele netwerken inzicht krijgen in de standaard systeem routes en de beschik bare opties voor het aanvullen of negeren van deze routes.

### <a name="service-endpoints"></a>Service-eindpunten

Het inschakelen van [service-eind punten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) in een subnet biedt een direct communicatie traject naar de bijbehorende PaaS-resource. Dit kan betere prestaties en beveiliging bieden door het beschik bare communicatie traject te beperken tot alleen die service. Door het gebruik van service-eind punten wordt een mogelijk data exfiltration-pad geïntroduceerd als de standaard configuratie toegang biedt tot alle instanties van de PaaS-service in plaats van de specifieke exemplaren die vereist zijn voor een toepassing of systeem.

De gemenebests entiteiten moeten het risico evalueren dat is gekoppeld aan het verlenen van rechtstreekse toegang tot de PaaS-resource, met inbegrip van de waarschijnlijkheid en het gevolg van het pad dat wordt gebruikt.

Om mogelijke Risico's voor service-eind punten te verminderen, implementeert u service-eindpunt beleid waar mogelijk of overweegt u service-eind punten in te scha kelen op een Azure Firewall-of NVA-subnet en het verkeer van specifieke subnetten door te sturen, waar extra filteren, bewaking of inspectie kan worden toegepast.

|Resource|Koppelen|
|---|---|
|*Zelfstudie: Netwerk toegang tot PaaS-resources beperken met virtuele netwerk service-eind punten met behulp van de Azure Portal* |[https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)|
|

### <a name="route-tables"></a>Routetabellen

Route tabellen bieden een door de beheerder geconfigureerd mechanisme voor het beheren van netwerk verkeer binnen Azure. Route tabellen kunnen worden benut om verkeer door te sturen naar een Azure Firewall of NVA, rechtstreeks verbinding te maken met externe bronnen of Azure-systeem routes te onderdrukken. Route tabellen kunnen ook worden gebruikt om te voor komen dat netwerken die via een gateway van een virtueel netwerk worden weer gegeven, beschikbaar worden gesteld aan bronnen in een subnet door doorgifte van Gateway routes van het virtuele netwerk uit te scha kelen.

|Resource|Koppelen|
|---|---|
|*Routerings concepten-aangepaste routes* |[https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes)|
|*Zelfstudie: Netwerk verkeer routeren* |[https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal)|
|

### <a name="border-gateway-protocol-bgp"></a>Border Gateway Protocol (BGP)

BGP kan worden gebruikgemaakt van virtuele netwerk gateways voor het dynamisch uitwisselen van routerings gegevens met on-premises of andere externe netwerken. BGP is van toepassing op een virtueel netwerk wanneer dit is geconfigureerd via een ExpressRoute virtuele netwerk gateway over ExpressRoute persoonlijke peering en wanneer ingeschakeld op een Azure-VPN Gateway.

Personen die betrokken zijn bij het ontwerp of de implementatie van virtuele netwerken en virtuele netwerk gateways in azure, moeten tijd in beslag nemen om inzicht te krijgen in het gedrag en de configuratie opties die beschikbaar zijn voor BGP in Azure.

|Resource|Koppelen|
|---|---|
|*Routerings concepten: BGP* | [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#next-hop-types-across-azure-tools](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#next-hop-types-across-azure-tools)|
|*Routerings vereisten voor ExpressRoute* | [https://docs.microsoft.com/azure/expressroute/expressroute-routing](https://docs.microsoft.com/azure/expressroute/expressroute-routing)|
|*Over BGP met Azure VPN Gateway* |[https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview)|
|*Zelfstudie: Een site-naar-site-VPN configureren via ExpressRoute micro soft-peering* |[https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering](https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering)|
|

## <a name="next-hop-types"></a>Typen volgende hop

### <a name="virtual-network"></a>Virtueel netwerk

Routes met een volgende hop van Virtual Network worden automatisch toegevoegd als systeem routes, maar kunnen ook worden toegevoegd aan door de gebruiker gedefinieerde routes om verkeer terug te sturen naar het virtuele netwerk in gevallen waarin de systeem route is overschreven.

### <a name="vnet-peering"></a>VNet-peering

Met VNet-peering wordt communicatie tussen twee verschillende virtuele netwerken ingeschakeld. Het configureren van VNet-peering moet zijn ingeschakeld op elk virtueel netwerk, maar de virtuele netwerken hoeven zich niet in dezelfde regio, abonnement of gekoppeld aan dezelfde Azure Active Directory-Tenant (Azure AD) te bevinden.

Bij het configureren van VNet-peering is het essentieel dat personen die betrokken zijn bij het ontwerp of de implementatie van VNet-peering inzicht krijgen in de vier bijbehorende configuratie parameters en hoe ze op elke zijde van de peer worden toegepast:

1. **Toegang tot virtueel netwerk toestaan:** Selecteer **ingeschakeld** (standaard) om communicatie tussen de twee virtuele netwerken in te scha kelen. Door communicatie tussen virtuele netwerken in te scha kelen, kunnen resources die zijn verbonden met een virtueel netwerk communiceren met elkaar met dezelfde band breedte en latentie alsof ze zijn verbonden met hetzelfde virtuele netwerk.
2. **Doorgestuurd verkeer toestaan:** Schakel dit selectie vakje in om verkeer dat is *doorgestuurd* door een netwerk verkeer dat niet afkomstig is uit het virtuele netwerk te laten stromen naar dit virtuele netwerk via een peering. Deze instelling is van cruciaal belang voor het implementeren van een hub-en-spoke-netwerk topologie.
3. **Transit door gateway toestaan:** Schakel dit selectie vakje in om het peered virtuele netwerk toe te staan de virtuele netwerk gateway te maken die is gekoppeld aan dit virtuele netwerk. *Toestaan dat gateway-door Voer* is ingeschakeld op het virtuele netwerk met de gateway bron van het virtuele netwerk, maar alleen van toepassing als *externe gateways gebruiken* is ingeschakeld op het andere virtuele netwerk.
4. **Externe gateways gebruiken:** Schakel dit selectie vakje in om verkeer van dit virtuele netwerk te laten stromen via een virtuele netwerk gateway die is gekoppeld aan het virtuele netwerk dat is verbonden met. *Externe gateways gebruiken* is ingeschakeld op het virtuele netwerk zonder een virtuele netwerk gateway en is alleen van toepassing als de optie *gateway-door Voer toestaan* is ingeschakeld op het andere virtuele netwerk.

|Resource|Koppelen|
|---|---|
| Concepten: Peering op virtueel netwerk                   | [https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) |
| Een peering op een virtueel netwerk maken, wijzigen of verwijderen | [https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering)|
|

### <a name="virtual-network-gateway"></a>Gateway van een virtueel netwerk

Virtuele netwerk gateways bieden een mechanisme voor de integratie van virtuele netwerken met externe netwerken, zoals on-premises omgevingen, partner omgevingen en andere Cloud implementaties. De twee typen virtuele netwerk gateway zijn ExpressRoute en VPN.

#### <a name="expressroute-gateway"></a>ExpressRoute-gateway

ExpressRoute gateways bieden een uitgangs punt van het virtuele netwerk naar een on-premises omgeving en moeten worden geïmplementeerd om te voldoen aan de vereisten voor beveiliging, Beschik baarheid, financiële situatie en prestaties. ExpressRoute-gateways bieden een gedefinieerde netwerk bandbreedte en maken gebruik van de kosten na de implementatie. Virtuele netwerken kunnen slechts één ExpressRoute-gateway hebben, maar dit kan worden verbonden met meerdere ExpressRoute-circuits en kan worden gebruikt door meerdere virtuele netwerken via VNet-peering, zodat band breedte en connectiviteit kunnen worden gedeeld. Zorg ervoor dat route ring tussen on-premises omgevingen en virtuele netwerken met behulp van ExpressRoute-gateways wordt geconfigureerd om end-to-end connectiviteit te garanderen met bekende, beheerde netwerk uitsluitings punten. Gemenebests entiteiten die gebruikmaken van de ExpressRoute-gateway via ExpressRoute-persoonlijke peering moeten ook virtuele netwerk apparaten (NVA) implementeren om een VPN-verbinding met de on-premises omgeving tot stand te brengen om te voldoen aan de richt lijnen voor de ACSC-consument.

Het is belang rijk te weten dat ExpressRoute-gateways beperkingen hebben op de adresbereiken, community's en andere specifieke configuratie-items die via BGP worden uitgewisseld.

| Resource  | Koppelen  |
|---|---|
| Overzicht van ExpressRoute-gateway | [https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) |
| Een virtuele netwerkgateway configureren voor ExpressRoute | [https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager)
|

#### <a name="vpn-gateway"></a>VPN Gateway

Azure VPN Gateway biedt een uitgangs punt van het virtuele netwerk naar een extern netwerk voor beveiligde site-naar-site-connectiviteit. VPN-gateways bieden een gedefinieerde netwerk bandbreedte en maken gebruik van de kosten na de implementatie. De gemenebests-entiteiten die VPN Gateway, moeten ervoor zorgen dat deze zijn geconfigureerd in overeenstemming met de ACSC-richt lijnen voor consumenten. Virtuele netwerken kunnen slechts één VPN Gateway hebben, maar dit kan worden geconfigureerd met meerdere tunnels en kan worden gebruikt door meerdere virtuele netwerken met behulp van VNet-peering, waardoor meerdere virtuele netwerken band breedte en connectiviteit kunnen delen. VPN-gateways kunnen via internet of via ExpressRoute via micro soft-peering tot stand worden gebracht.

| Resource  | Koppelen |
| --- | --- |
| Overzicht van VPN Gateway| [https://docs.microsoft.com/azure/vpn-gateway](https://docs.microsoft.com/azure/vpn-gateway)|
| Planning en ontwerp voor VPN Gateway | [https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)|
| Azure-VPN Gateway in azure Australië | [Azure-VPN Gateway in azure Australië](vpn-gateway.md)
|

### <a name="next-hop-of-virtual-appliance"></a>Volgende hop van virtueel apparaat

De volgende hop van het virtuele apparaat biedt de mogelijkheid om netwerk verkeer te verwerken buiten de Azure-netwerk-en routerings topologie die wordt toegepast op virtuele netwerken. Virtuele apparaten kunnen beveiligings regels Toep assen, adressen omzetten, Vpn's instellen, proxy verkeer of een bereik van andere mogelijkheden. De volgende hop van het virtuele apparaat wordt toegepast via Udr's in een route tabel en kan worden gebruikt om verkeer door te sturen naar een Azure Firewall, afzonderlijke NVA of Azure Load Balancer Beschik baarheid over meerdere Nva's. Als u een virtueel apparaat voor route ring wilt gebruiken, moeten de bijbehorende netwerk interfaces zijn ingeschakeld voor door sturen via IP.

| Resource  | Koppelen |
| --- | ---|
| Routerings concepten: Aangepaste routes | [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes) |
| Door sturen via IP in-of uitschakelen | [https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#enable-or-disable-ip-forwarding](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#enable-or-disable-ip-forwarding)
|

### <a name="next-hop-of-virtualnetworkserviceendpoint"></a>Volgende hop van VirtualNetworkServiceEndpoint

Routes met het type van de volgende hop van VirtualNetworkServiceEndpoint worden alleen toegevoegd wanneer een service-eind punt wordt geconfigureerd op een subnet en kan niet hand matig worden geconfigureerd via route tabellen.

### <a name="next-hop-of-internet"></a>Volgende hop van Internet

Het volgende hop-Internet wordt gebruikt om resources te bereiken die gebruikmaken van een openbaar IP-adres, waaronder Internet en PaaS en Azure-Services in azure-regio's. Voor het volgende hop Internet is geen standaard route (0.0.0.0/0) vereist voor alle netwerken, maar kan worden gebruikt om routerings paden naar specifieke open bare Services in te scha kelen. De volgende hop van Internet moet worden gebruikt voor het toevoegen van routes aan geautoriseerde services en mogelijkheden die vereist zijn voor systeem functies, zoals micro soft-beheer adressen.

Voor beelden van services die kunnen worden toegevoegd met behulp van de volgende hop van Internet zijn:

1. Key Management Services voor Windows-activering
2. App Service Environment beheer

|Resource|Koppelen|
|---|---|
| Uitgaande verbindingen in azure | [https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) |
| Aangepaste Azure-routes gebruiken om KMS-activering in te scha kelen | [https://docs.microsoft.com/bs-latn-ba/azure/virtual-machines/troubleshooting/custom-routes-enable-kms-activation](https://docs.microsoft.com/bs-latn-ba/azure/virtual-machines/troubleshooting/custom-routes-enable-kms-activation) |
| Een App Service Environment vergren delen  | [https://docs.microsoft.com/azure/app-service/environment/firewall-integration](https://docs.microsoft.com/azure/app-service/environment/firewall-integration) |
|

### <a name="next-hop-of-none"></a>Volgende hop van geen

De volgende hop van geen kan worden gebruikt om communicatie met een specifiek netwerk te voor komen. In tegens telling tot een NSG, waarmee wordt bepaald of het verkeer wordt toegestaan of geweigerd bij het passeren van een beschik bare netwerkpad, met behulp van de volgende hop van none wordt het netwerkpad volledig verwijderd. U moet er rekening mee houden bij het maken van routes met een volgende hop van geen, met name bij het Toep assen van de route op een standaard router van 0.0.0.0/0, omdat dit onbedoelde gevolgen kan hebben en het oplossen van problemen met systeem problemen complex en tijdrovend kan zijn.

## <a name="security"></a>Beveiliging

Het implementeren van de besturings elementen voor netwerk segmentatie en schei ding op IaaS-en PaaS-mogelijkheden wordt bereikt door de mogelijkheden zelf te beveiligen en door beheerde communicatie paden te implementeren van de systemen die zullen communiceren met de voorzieningen.

Het ontwerpen en bouwen van oplossingen in Azure is een proces van het maken van een logische architectuur voor het begrijpen, beheren en bewaken van netwerk bronnen in de hele Azure-aanwezigheid. Deze logische architectuur is software die is gedefinieerd binnen het Azure-platform en neemt de plaats van een fysieke netwerk topologie die in traditionele netwerk omgevingen is geïmplementeerd.

De logische architectuur die wordt gemaakt, moet de functionaliteit bieden die nodig is voor de bruikbaarheid, maar moet ook de zicht baarheid en controle bieden die nodig is voor beveiliging en integriteit.

Het bereiken van deze uitkomst is gebaseerd op het implementeren van de benodigde netwerk segmentatie en hulpprogram ma's voor schei ding, maar ook bij het beveiligen en afdwingen van de netwerk topologie en de implementatie van deze hulpprogram ma's.

### <a name="network-security-groups-nsgs"></a>Netwerk beveiligings groepen (Nsg's)

Nsg's worden gebruikt om het binnenkomende en uitgaande verkeer op te geven dat is toegestaan voor een subnet of een specifieke netwerk interface. Bij het configureren van Nsg's moeten gemenebests-entiteiten een white list-benadering gebruiken waarbij regels worden geconfigureerd voor het toestaan van het benodigde verkeer met een standaard regel die is geconfigureerd om al het verkeer te weigeren dat niet overeenkomt met een specifieke instructie voor toestaan. Bij het plannen en configureren van Nsg's moet ervoor worden gezorgd dat alle benodigde inkomend en uitgaand verkeer op de juiste wijze wordt vastgelegd. Dit omvat het identificeren en goed begrip van alle privé-IP-adresbereiken in virtuele netwerken en de on-premises omgeving, en specifieke micro soft-Services, zoals Azure Load Balancer-en PaaS-beheer vereisten. Personen die betrokken zijn bij het ontwerp en de implementatie van Nsg's moeten ook inzicht krijgen in het gebruik van service tags en toepassings beveiligings groepen voor het maken van verfijnde, service-en toepassingsspecifieke beveiligings regels.

Het is belang rijk te weten dat de standaard configuratie voor een NSG uitgaand verkeer naar alle adressen in het virtuele netwerk en alle open bare IP-adressen toestaat.

|Resource|Koppelen|
|---|---|
|Overzicht van netwerk beveiliging | [https://docs.microsoft.com/azure/virtual-network/security-overview](https://docs.microsoft.com/azure/virtual-network/security-overview)|
|Een netwerk beveiligings groep maken, wijzigen of verwijderen | [https://docs.microsoft.com/azure/virtual-network/manage-network-security-group](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)|
|

### <a name="azure-firewall"></a>Azure Firewall

Azure Firewall kan worden gebruikgemaakt van het bouwen van een hub-en-spoke-netwerk topologie en het afdwingen van gecentraliseerde netwerk beveiligings controles. Azure Firewall kan worden gebruikt om te voldoen aan de nood zakelijke vereisten van de ISM voor uitgaand verkeer door een vermelding voor een lijst met toegestane gegevens te implementeren waarbij alleen de IP-adressen, protocollen, poorten en FQDN-namen die vereist zijn voor de systeem functionaliteit, worden toegestaan. Gemenebests-entiteiten moeten een aanpak op basis van een risico hebben om te bepalen of de beveiligings mogelijkheden van Azure Firewall voldoende zijn voor hun vereisten. Voor scenario's waarin extra beveiligings mogelijkheden dan die van Azure Firewall zijn vereist, moeten gemenebests organisaties overwegen om Nva's te implementeren.

|Resource|Koppelen|
|---|---|
|*Documentatie over Azure Firewall* | [https://docs.microsoft.com/azure/firewall](https://docs.microsoft.com/azure/firewall)|
|*Zelfstudie: Azure Firewall implementeren en configureren in een hybride netwerk met behulp van Azure PowerShell* | [https://docs.microsoft.com/azure/firewall/tutorial-hybrid-ps](https://docs.microsoft.com/azure/firewall/tutorial-hybrid-ps)|
|

### <a name="network-virtual-appliances-nvas"></a>Virtuele netwerk apparaten (Nva's)

Nva's kan worden gebruikt om een hub-en-spoke-netwerk topologie te bouwen, verbeterde of aanvullende netwerk mogelijkheden te bieden of kan worden gebruikt als alternatief voor Azure-netwerk mechanismen voor vertrouwde en consistente ondersteuning en beheer met on-premises netwerk services. Nva's kan worden geïmplementeerd om te voldoen aan specifieke beveiligings vereisten, zoals; scenario's waarbij identiteits bewustmaking is gekoppeld aan netwerk verkeer, HTTPS-ontsleuteling, inhouds inspectie, filters of andere beveiligings mogelijkheden. Nva's moet worden geïmplementeerd in een configuratie met hoge Beschik baarheid en personen die betrokken zijn bij het ontwerp of de implementatie van Nva's, moeten de juiste leveranciers documentatie raadplegen voor richt lijnen voor de implementatie in Azure.

|Resource|Koppelen|
|---|---|
|*Virtuele netwerk apparaten met hoge Beschik baarheid implementeren* | [https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha)|
|

### <a name="service-endpoint-policies-preview"></a>Service-eindpunt beleid (preview-versie)

Configureer beleids regels voor service-eind punten op basis van de beschik baarheid van de service en een beveiligings risico beoordeling van de kans en impact van gegevens exfiltration. U moet rekening houden met het beleid voor service-eind punten voor Azure Storage en per geval worden beheerd voor andere services op basis van het bijbehorende risico profiel.

| Resource | Koppelen  |
| --- | --- |
| *Overzicht van service-eindpunt beleid* | [https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview) |
| *Service-eindpunt beleid maken, wijzigen of verwijderen met behulp van de Azure Portal* | [https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-portal](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-portal)
|

### <a name="azure-policy"></a>Azure-beleid

Azure Policy is een belang rijk onderdeel voor het afdwingen en onderhouden van de integriteit van de logische architectuur van de Azure-omgeving. Er zijn verschillende services en uitgaand netwerk verkeer paden beschikbaar via Azure-Services. Het is van cruciaal belang dat Gemenebests entiteiten op de hoogte zijn van de resources die zich in hun omgeving bevinden en de beschik bare netwerk uituitgangs punten. Om ervoor te zorgen dat niet-geautoriseerde netwerk uitzonderings punten niet worden gemaakt in de Azure-omgeving, moeten Gemenebests entiteiten Azure Policy gebruiken om de typen resources te beheren die kunnen worden geïmplementeerd en de configuratie van deze resources. Praktische voor beelden hiervan zijn het beperken van resources tot alleen degenen die zijn toegestaan en goedgekeurd voor gebruik en waarvoor Nsg's moet worden toegevoegd aan subnetten.

|Resource | Koppelen|
|---|---|
|*Overzicht van Azure Policy* | [https://docs.microsoft.com/azure/governance/policy/overview](https://docs.microsoft.com/azure/governance/policy/overview)|
|*Voor beeld van toegestane resource typen* | [https://docs.microsoft.com/azure/governance/policy/samples/allowed-resource-types](https://docs.microsoft.com/azure/governance/policy/samples/allowed-resource-types)|
|*NSG afdwingen op een voor beeld van een subnet*| [https://docs.microsoft.com/azure/governance/policy/samples/nsg-on-subnet](https://docs.microsoft.com/azure/governance/policy/samples/nsg-on-subnet)|
|

## <a name="paas-egress-capabilities"></a>PaaS uitgangs mogelijkheden

PaaS-mogelijkheden bieden mogelijkheden voor meer functionaliteit en vereenvoudigd beheer, maar complexe vereisten voor het adresseren van netwerk segmentatie en schei ding. De PaaS-mogelijkheden worden meestal geconfigureerd met open bare IP-adressen en zijn toegankelijk via internet.  Als u PaaS-mogelijkheden in uw systemen en oplossingen gebruikt, moet u ervoor zorgen dat de communicatie stromen tussen onderdelen worden geïdentificeerd en netwerk beveiligings regels worden gemaakt om deze communicatie alleen toe te staan. Als onderdeel van een verdedigings-diep gaande benadering moeten de PaaS-mogelijkheden worden geconfigureerd met versleuteling, authenticatie en de juiste toegangs beheer en-machtigingen.  

### <a name="public-ip-for-paas"></a>Openbaar IP-adres voor PaaS

Open bare IP-adressen voor PaaS-mogelijkheden worden toegewezen op basis van de regio waarin de service wordt gehost of geïmplementeerd. Er is een uitleg van de toewijzing van open bare IP-adressen en regio's vereist als u de juiste netwerk beveiligings regels en-routerings topologie wilt bouwen voor netwerk segmentatie en schei ding met Azure Virtual Networks, PaaS en ExpressRoute en Internet verbinding. Azure wijst IP-adressen toe uit een groep die is toegewezen aan elke Azure-regio. Micro soft maakt de adressen die in elke regio worden gebruikt, beschikbaar voor downloaden, die op een regel matige en gecontroleerde manier worden bijgewerkt. De services die beschikbaar zijn in elke regio, veranderen ook vaak als nieuwe services worden vrijgegeven of services worden uitgebreid geïmplementeerd. Gemenebests entiteiten moeten deze materialen regel matig bekijken en automatisering kunnen gebruiken om systemen waar nodig te onderhouden. Specifieke IP-adressen voor sommige services die in elke regio worden gehost, kunnen worden verkregen door contact op te nemen met micro soft ondersteuning.

| Resource | Koppelen |
| --- | --- |
| *Microsoft Azure Datacenter IP-bereiken*                   | [https://www.microsoft.com/download/details.aspx?id=41653](https://www.microsoft.com/download/details.aspx?id=41653) |
| *Azure-Services per regio*                              | [https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional, Australië-centraal, Australië-centraal-2, Australië-Oost, Australië-Zuidoost & Products = all](https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional,australia-central,australia-central-2,australia-east,australia-southeast&products=all) |
| *Binnenkomende en uitgaande IP-adressen in Azure App Service* | [https://docs.microsoft.com/azure/app-service/overview-inbound-outbound-ips](https://docs.microsoft.com/azure/app-service/overview-inbound-outbound-ips)
|

## <a name="next-steps"></a>Volgende stappen

Vergelijk uw algehele architectuur en ontwerp met de gepubliceerde [beveiligde blauw drukken voor IaaS-en Paas](https://aka.ms/au-protected)-webtoepassingen.
