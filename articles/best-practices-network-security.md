---
title: Aanbevolen beveiligingsprocedures voor Azure-netwerk | Microsoft Docs
description: Leer enkele van de belangrijkste functies die beschikbaar zijn in Azure voor het maken van veilige netwerkomgevingen
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: ''
ms.assetid: d169387a-1243-4867-a602-01d6f2d8a2a1
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 74a46c7788b0a0dc729ab977489ed6d97a387a6e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60619379"
---
# <a name="microsoft-cloud-services-and-network-security"></a>Microsoft cloud services en -netwerkbeveiliging
Microsoft-cloudservices bieden grootschalige services en infrastructuur, geavanceerde mogelijkheden en veel opties voor hybride verbindingen. Klanten kunnen kiezen voor toegang tot deze services via Internet of met Azure ExpressRoute, waarmee u verbinding met het particuliere netwerk. De Microsoft Azure-platform kan klanten naadloos de infrastructuur uitbreiden naar de cloud en bouw architecturen met meerdere lagen. Externe partijen kunt bovendien verbeterde mogelijkheden inschakelen door het aanbieden van security-services en virtuele apparaten. In dit artikel biedt een overzicht van beveiliging en architectonische problemen waarmee klanten rekening houden moeten bij het gebruik van Microsoft-cloudservices is toegankelijk via ExpressRoute. Ook wordt behandeld veiliger services in virtuele Azure-netwerken maken.

## <a name="fast-start"></a>Snel aan de slag
De volgende logische-grafiek kan sturen u een voorbeeld van de technieken die veel security zijn beschikbaar met de Azure-platform. Voor snelle naslag vinden in het voorbeeld dat het beste past bij uw situatie. Voor een uitgebreide uitleg blijven lezen het document.
[![0]][0]

[Voorbeeld 1: Bouw een perimeternetwerk (ook wel DMZ, gedemilitariseerde zone of screened subnet genoemd) ter bescherming van toepassingen met netwerkbeveiligingsgroepen (nsg's).](#example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs)</br>
[Voorbeeld 2: Bouw een perimeternetwerk ter bescherming van toepassingen met een firewall en nsg's.](#example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs)</br>
[Voorbeeld 3: Bouw een perimeternetwerk ter bescherming van netwerken met een firewall, de gebruiker gedefinieerde route (UDR) en de NSG.](#example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-and-udr-and-nsg)</br>
[Voorbeeld 4: Een hybride verbinding met een site-naar-site, een virtueel apparaat virtueel particulier netwerk (VPN) toevoegen.](#example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Voorbeeld 5: Een hybride verbinding met de Azure VPN-gateway van een site-naar-site toevoegen.](#example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway)</br>
[Voorbeeld 6: Een hybride verbinding met ExpressRoute toevoegen.](#example-6-add-a-hybrid-connection-with-expressroute)</br>
Voorbeelden voor het toevoegen van verbindingen tussen virtuele netwerken, hoge beschikbaarheid en servicechaining wordt toegevoegd aan dit document in de komende maanden.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Microsoft-naleving en infrastructuur voor beveiliging
Microsoft biedt waarmee organisaties kunnen voldoen aan de nationale, regionale en branchespecifieke betreffende het verzamelen en gebruiken van personen gegevens, meer dan 40 certificeringen en verklaringen. De meest uitgebreide set met elke cloudserviceprovider.

Zie voor meer informatie, de informatie over de naleving op de [Microsoft Trust Center][TrustCenter].

Microsoft heeft een uitgebreide benadering voor het beveiligen van cloud-infrastructuur die nodig zijn voor het uitvoeren van grootschalige globale services. Microsoft cloud-infrastructuur omvat hardware, software, netwerken, samen met beheerdersreferenties en operationele medewerkers, naast de fysieke datacenters.

![2]

Deze benadering biedt een meer veilige basis voor klanten om hun services in de Microsoft-cloud te implementeren. De volgende stap is voor klanten om te ontwerpen en maken van een beveiligingsarchitectuur ter bescherming van deze services.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Traditionele beveiligingsprogramma architecturen en perimeternetwerken
Hoewel Microsoft sterk investeert bij het beschermen van de cloudinfrastructuur, moeten klanten ook cloudservices en resourcegroepen beveiligen. Een meerlaagse aanpak voor beveiliging biedt de beste verdediging. Een zone perimeter network security beveiligt interne netwerkbronnen vanaf een niet-vertrouwd netwerk. Een perimeternetwerk verwijst naar de randen of delen van het netwerk tussen Internet en de beveiligde enterprise IT-infrastructuur.

In de standaard enterprise-netwerken, wordt de basisinfrastructuur sterk verbeterd op de verbindingen, met meerdere lagen van beveiligingsapparaten. De grens van elke laag bestaat uit de apparaten en beleid afdwingingspunten. Elke laag een combinatie van de volgende netwerkapparaten voor beveiliging kunt opnemen: firewalls, voorkomen van Denial of Service (DoS), inbraakdetectie of Protection systemen (IDS/IPS) en VPN-apparaten. Afdwingen van beleid kan duren voordat de vorm van firewall-beleid, toegangsbeheerlijsten (ACL's) of specifieke routering. De eerste verdedigingslinie in het netwerk, rechtstreeks accepteren van binnenkomende verkeer vanaf Internet, is een combinatie van deze methoden voor blok-aanvallen en schadelijke verkeer terwijl legitieme aanvragen verder in het netwerk. Dit verkeer routeert rechtstreeks naar resources in het perimeternetwerk. Deze resource kan vervolgens "praten" dieper in het netwerk, die de grens voor validatie van de volgende resources eerste. De buitenste laag wordt het perimeternetwerk genoemd, omdat deze deel uitmaken van het netwerk wordt blootgesteld aan Internet, meestal met een vorm van beveiliging aan beide zijden. De volgende afbeelding toont een voorbeeld van een perimeternetwerk één subnet in een bedrijfsnetwerk bevinden, met twee grenzen voor netwerkbeveiliging.

![3]

Er zijn veel architecturen gebruikt voor het implementeren van een perimeternetwerk bevinden. Deze architecturen kunnen variëren van een eenvoudige load balancer met een perimeternetwerk met meerdere subnetten met gevarieerde mechanismen op elke grens aan verkeer blokkeren en de diepere lagen van het bedrijfsnetwerk beveiligd. Hoe het perimeternetwerk wordt gemaakt, is afhankelijk van de specifieke vereisten van de organisatie en de algehele omgaan met risico's.

Wanneer klanten hun workloads naar openbare clouds, is het essentieel is voor de ondersteuning voor vergelijkbare mogelijkheden voor perimeter netwerkarchitectuur in Azure om te voldoen aan het nalevings- en beveiligingsvereisten. Dit document bevat richtlijnen over hoe klanten een beveiligde netwerkomgeving in Azure kunnen samenstellen. Het is gericht op het perimeternetwerk, maar bevat ook een uitgebreide bespreking van veel aspecten van netwerkbeveiliging. Deze discussie in kennis van de volgende vragen:

* Hoe kan een perimeternetwerk in Azure worden gemaakt?
* Wat zijn enkele van de Azure-functies beschikbaar zijn voor het bouwen van het perimeternetwerk?
* Hoe kunnen back-end-workloads worden beveiligd?
* Hoe worden de communicatie via Internet beheerd om de workloads in Azure?
* Hoe kunnen de on-premises netwerken uit in Azure-implementaties worden beveiligd?
* Wanneer moeten systeemeigen Azure beveiligingsfuncties die worden gebruikt ten opzichte van externe toepassingen of services?

Het volgende diagram toont verschillende beveiligingslagen die Azure aan klanten biedt. Deze lagen zijn zowel systeemeigen in de Azure-platform zelf wordt geboden door de klant gedefinieerde functies:

![4]

Inkomend verkeer van Internet, Azure DDoS helpt tegen grootschalige aanvallen op Azure beschermen. De volgende laag is door de klant gedefinieerde openbare IP-adressen (eindpunten), die worden gebruikt om te bepalen welk verkeer via de service in de cloud kunt doorgeven aan het virtuele netwerk. Systeemeigen Azure virtuele netwerkisolatie garandeert volledige isolatie van alle andere netwerken en dat verkeer alleen via paden van de gebruiker die is geconfigureerd en methoden stromen. Deze paden en methoden zijn de volgende laag, waar de nsg's, UDR en virtuele netwerkapparaten kunnen worden gebruikt om de beveiligingsgrenzen ter bescherming van de implementaties van toepassingen in het beveiligde netwerk te maken.

De volgende sectie bevat een overzicht van virtuele netwerken van Azure. Deze virtuele netwerken worden gemaakt door klanten, en wat hun geïmplementeerde werkbelastingen zijn verbonden met zijn. Virtuele netwerken vormen de basis van alle network security functies die zijn vereist voor het maken van een perimeternetwerk ter bescherming van implementaties van klanten in Azure.

## <a name="overview-of-azure-virtual-networks"></a>Overzicht van virtuele netwerken van Azure
Voordat het verkeer van Internet toegang krijgen tot de Azure-netwerken, zijn er twee beveiligingslagen documentatiesysteem dat in het Azure-platform:

1.    **DDoS protection**: DDoS protection is een laag van Azure fysieke netwerk die worden beveiligd met de Azure-platform zelf wordt geboden tegen aanvallen van grootschalige clientbeheer op Internet. Deze aanvallen voor het gebruik van meerdere "bot" knooppunten in een poging een Internet-service blijvend wordt overbelast. Azure biedt een robuuste DDoS protection net op alle binnenkomende, uitgaande en cross-Azure-regio connectiviteit. Deze laag DDoS-beveiliging heeft geen kenmerken voor het configureren van gebruiker en is niet toegankelijk voor de klant. De DDoS protection-laag beschermt Azure als een platform van grootschalige aanvallen, het bewaakt ook verkeer van uitgaande gebonden en cross-Azure-regio. Met virtuele netwerkapparaten in het VNet, kunnen extra verificatielagen veerkracht worden geconfigureerd door de klant voor een kleinere schaal aanval die de beveiliging op netwerkniveau van platform niet veroorzaken. Een voorbeeld van DDoS in actie. Als u een internetgerichte IP-adres van een grootschalige DDoS-aanval is aangevallen, zou Azure de bronnen van de aanvallen detecteren en verwijderen van de strijdige verkeer voordat deze de bestemming bereikt. In bijna alle gevallen moet wordt niet het eindpunt van de aangevallen beïnvloed door de aanval. In het zeldzame gevallen dat een eindpunt wordt beïnvloed, geldt geen verkeer naar andere eindpunten, alleen het eindpunt van de aangevallen. Andere klanten en -services ziet geen invloed van deze aanval. Het is essentieel om te weten dat Azure DDoS is alleen op zoek naar grootschalige aanvallen. Het is mogelijk dat uw specifieke service overbelast raken voordat de beveiliging op netwerkniveau platform drempelwaarden worden overschreden. Bijvoorbeeld, kan een website op een enkele A0 IIS-server offline gezet door een DDoS-aanval voordat Azure-platform-niveau DDoS protection geregistreerd een bedreiging wordt beschouwd.

2.  **Openbare IP-adressen**: Openbare IP-adressen zijn (ingeschakeld via de service-eindpunten, openbare IP-adressen, Application Gateway en andere Azure-functies die een openbaar IP-adres met het internet doorgestuurd naar de resource) toestaan cloud services of -brongroepen openbaar Internet-IP-adres hebben adressen en poorten die worden weergegeven. Het eindpunt maakt gebruik van NAT (Network Address Translation) voor het routeren van verkeer naar de interne-adres en poort op Azure virtual network. Dit pad is de eenvoudigste manier voor het externe verkeer om door te geven in het virtuele netwerk. De openbare IP-adressen kunnen worden geconfigureerd om te bepalen welk verkeer wordt doorgegeven, en hoe en waar deze wordt omgezet naar het virtuele netwerk.

Zodra verkeer het virtuele netwerk bereikt, zijn er veel functies die worden geleverd aan de orde. Virtuele netwerken van Azure vormen de basis voor klanten om hun workloads en waar de basisvereisten voor beveiliging op netwerkniveau toepassing te koppelen. Het is een particulier netwerk (een overlay met een virtueel netwerk) in Azure voor klanten met de volgende functies en -kenmerken:

* **Isolatie van verkeer**: Een virtueel netwerk is de verkeersisolatiegrens op het Azure-platform. Virtuele machines (VM's) in één virtueel netwerk kan niet communiceren rechtstreeks op virtuele machines in een ander virtueel netwerk, zelfs als beide virtuele netwerken worden gemaakt van dezelfde klant. Isolatie is een belangrijke eigenschap die ervoor zorgt dat de klant-VM's en communicatie blijft privé binnen een virtueel netwerk.

>[!NOTE]
>Isolatie van verkeer verwijst alleen naar verkeer *inkomende* aan het virtuele netwerk. Standaard uitgaand verkeer van het VNet met het internet is toegestaan, maar indien gewenst door nsg's kan worden voorkomen.
>
>

* **Topologie met meerdere niveaus**: Virtuele netwerken kunnen klanten voor het definiëren van de topologie met meerdere niveaus door bij het toewijzen van subnetten en het toewijzen van afzonderlijke adresruimten voor verschillende elementen of 'laag' van hun workloads. Deze logische groeperingen en topologieën kunnen klanten voor het definiëren van beleid voor andere toegang op basis van de typen werkbelasting, en ook bepalen verkeersstromen tussen de lagen.
* **Cross-premises connectiviteit**: Klanten kunnen tot stand brengen van cross-premises-connectiviteit tussen een virtueel netwerk en meerdere on-premises sites of andere virtuele netwerken in Azure. Klanten kunnen een verbinding kan, VNet-Peering, Azure VPN-Gateways, virtuele apparaten van derden network of ExpressRoute gebruiken. Azure biedt ondersteuning voor site-naar-site (S2S) VPN-verbindingen met behulp van standaard IPsec/IKE-protocollen en particuliere ExpressRoute-connectiviteit.
* **NSG** kunnen klanten regels (ACL's) maken op het gewenste niveau van gedetailleerdheid: network interfaces, afzonderlijke virtuele machines of virtuele subnetten. Klanten kunnen toegang toestaan of weigeren van communicatie tussen de werkbelastingen binnen een virtueel netwerk, van systemen op klantnetwerken via cross-premises-connectiviteit, beheren of directe communicatie met Internet.
* **UDR** en **doorsturen via IP** kunnen klanten voor het definiëren van de communicatiepaden tussen verschillende lagen binnen een virtueel netwerk. Klanten kunnen een firewall, IDS/IPS en andere virtuele apparaten implementeren en het routeren van netwerkverkeer via deze beveiligingsapparaten voor het beveiligingsbeleid opleggen aan grens, controleren en controle.
* **Virtuele netwerkapparaten** in de Azure Marketplace: Beveiligingsapparaten, zoals firewalls, netwerktaakverdelers en id's / IP-Adressen zijn beschikbaar in de Azure Marketplace en de galerie met installatiekopieën van virtuele machine. Klanten kunnen deze apparaten in hun virtuele netwerken en met name op hun beveiligingsgrenzen (met inbegrip van de subnetten van perimeter network) implementeren voor het voltooien van een multi-tiered beveiligde netwerkomgeving.

Met deze functies en mogelijkheden is een voorbeeld van hoe een perimeternetwerk netwerkarchitectuur kan worden gebouwd in Azure in het volgende diagram:

![5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Perimeter network kenmerken en vereisten
Het perimeternetwerk is de front-end van het netwerk, rechtstreeks communicatie communicatie via Internet. De inkomende pakketten moeten stromen via de beveiligingsapparaten, zoals de firewall, -id's en IP-Adressen, voordat de back-endservers wordt bereikt. Afhankelijk van de Internet-pakketten van de werkbelastingen kunnen ook door de beveiligingsapparaten in het perimeternetwerk van de voor het afdwingen van beleid, controle en controledoeleinden, voordat deze uit het netwerk stromen. Bovendien kan het perimeternetwerk de cross-premises VPN-gateways tussen virtuele netwerken van klanten en on-premises netwerken hosten.

### <a name="perimeter-network-characteristics"></a>Perimeter network kenmerken
Verwijst naar de vorige afbeelding, zijn enkele van de kenmerken van een goede perimeternetwerk als volgt:

* Internet-facing:
  * De perimeter network-subnet zelf is internetgerichte, rechtstreeks communiceren met Internet.
  * Openbare IP-adressen, VIP's en/of service-eindpunten doorgeven internetverkeer voor de front-endnetwerk en apparaten.
  * Inkomend verkeer van Internet wordt doorgegeven via beveiligingsapparaten voordat u andere resources op de front-endnetwerk.
  * Als u uitgaande beveiliging is ingeschakeld, verkeer wordt doorgegeven via beveiligingsapparaten, als de laatste stap wordt vóór doorgeven aan Internet.
* Beveiligd netwerk:
  * Er is geen pad van het Internet naar de basisinfrastructuur.
  * Kanalen op de basisinfrastructuur moeten passeren via beveiligingsapparaten, zoals nsg's, firewalls of VPN-apparaten.
  * Andere apparaten moeten geen brug Internet en de infrastructuur.
  * Van beveiligingsapparaten op Internet gerichte en het beveiligde netwerk gerichte grenzen van het perimeternetwerk (bijvoorbeeld, de twee firewall pictogrammen weergegeven in de vorige afbeelding) mogelijk daadwerkelijk een enkel virtueel apparaat met gedifferentieerde regels of interfaces voor elke grens. Bijvoorbeeld, een fysiek apparaat logisch van elkaar gescheiden, verwerken van de workload voor beide grenzen van het perimeternetwerk.
* Andere algemene procedures en beperkingen:
  * Workloads moeten kritieke bedrijfsgegevens niet worden opgeslagen.
  * Toegang en updates voor perimeter network configuraties en implementaties zijn beperkt tot alleen geautoriseerde beheerders.

### <a name="perimeter-network-requirements"></a>Perimeter-vereisten
Volg deze richtlijnen op virtueel netwerk-vereisten voor het implementeren van een geslaagde perimeternetwerk zodat deze kenmerken:

* **Subnet-architectuur:** Geef het virtuele netwerk dat een hele subnet als het perimeternetwerk, gescheiden van andere subnetten in hetzelfde virtuele netwerk is toegewezen. Dankzij deze scheiding zorgt ervoor dat het verkeer tussen het perimeternetwerk en andere interne of particuliere subnet lagen stromen via een firewall of een virtueel apparaat-id's / IP-Adressen.  Gebruiker gedefinieerde routes op de subnetten van de grens zijn vereist voor het doorsturen van verkeer naar het virtuele apparaat.
* **NSG:** De perimeter network-subnet zelf is geopend voor de communicatie met Internet, maar dat betekent niet dat klanten moeten de overslaan nsg's. Volg algemene beveiligingsprocedures om te beperken van de netwerk-oppervlakken blootgesteld aan Internet. Vergrendelen op de externe adresbereiken die toegang hebben tot de implementaties of de specifieke toepassingsprotocollen en poorten die geopend zijn. Mogelijk zijn er situaties echter waarin een volledige Lockdown niet mogelijk is. Bijvoorbeeld, als klanten een externe website in Azure hebben, het perimeternetwerk moet de binnenkomende webaanvragen van alle openbare IP-adressen toestaan, maar moet alleen de web application poorten openen: TCP op poort 80 en/of TCP op poort 443.
* **Routeringstabel:** De perimeter network-subnet zelf moet kunnen communiceren rechtstreeks met Internet, maar kunt u voorkomen dat rechtstreekse communicatie naar en van de back-end- of on-premises netwerken zonder tussenkomst van een apparaat of een firewall.
* **Configuratie van het apparaat beveiliging:** Als u wilt routeren en inspecteren van pakketten tussen het perimeternetwerk en de rest van de beveiligde netwerken, de beveiligingsapparaten, zoals firewall, -id's en IP-Adressen mogelijk apparaten multihomed. Ze kunnen afzonderlijke NIC's voor het perimeternetwerk en de back-end-subnetten hebben. De NIC's in het perimeternetwerk communiceren rechtstreeks van en naar het Internet, met de bijbehorende nsg's en de perimeter network-routeringstabel. De NIC's verbinding maken met de back-end-subnetten hebben meer beperkte nsg's en -routeringstabel van de bijbehorende back-end-subnetten.
* **Functie voor beveiliging op apparaat:** De beveiligingsapparaten geïmplementeerd in het perimeternetwerk normaal gesproken voert u de volgende functionaliteit:
  * Firewall: Afdwingen van firewall-regels of beleid voor toegangsbeheer voor de inkomende aanvragen.
  * Detectie van bedreigingen en preventie: Detecteren en vereenvoudigen van schadelijke aanvallen via Internet.
  * Controle en logboekregistratie: Het onderhouden van gedetailleerde logboeken voor controle en analyse.
  * Omgekeerde proxy: De binnenkomende aanvragen omleiden naar de bijbehorende back-endservers. Deze omleiding omvat toewijzing en meestal vertalen van de doeladressen op de front-apparaten, firewalls, naar de back-endserver-adressen.
  * Proxy voor doorsturen: Bieden NAT en het uitvoeren van controle voor de communicatie van binnen het virtuele netwerk gestart met het Internet.
  * Router: Doorsturen van verkeer binnenkomende en meerdere subnetten binnen het virtuele netwerk.
  * VPN-apparaat: Fungeert als de cross-premises VPN-gateways voor cross-premises VPN-connectiviteit tussen klanten on-premises netwerken en virtuele netwerken van Azure.
  * VPN server: VPN-clients verbinding maken met virtuele netwerken van Azure accepteert.

> [!TIP]
> Gescheiden houden van de volgende twee groepen: de personen die gemachtigd voor toegang tot de perimeter network security versnelling en de personen die gemachtigd als beheerders voor ontwikkeling, implementatie of bewerkingen. Deze groepen afzonderlijke houden, kunt u een scheiding van taken, en voorkomt u dat één persoon zowel beveiliging van toepassingen en maatregelen voor netwerk.
>
>

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Vragen om te worden gevraagd bij het bouwen van netwerkgrenzen
In deze sectie, tenzij uitdrukkelijk vermeld, verwijst de term 'netwerken' naar persoonlijke Azure-netwerken die zijn gemaakt door de beheerder van een abonnement. De term verwijst niet naar de onderliggende fysieke netwerken in Azure.

Virtuele Azure-netwerken worden ook vaak gebruikt voor de traditionele on-premises netwerken uitbreiden. Het is mogelijk om op te nemen van site-naar-site of ExpressRoute hybride netwerkoplossingen met perimeter network-architecturen. Deze hybride-koppeling is een belangrijk aandachtspunt bij het bouwen van netwerkgrenzen voor beveiliging.

De volgende drie vragen zijn essentieel voor het beantwoorden wanneer u een netwerk met een perimeternetwerk en meerdere beveiligingsgrenzen bouwt.

#### <a name="1-how-many-boundaries-are-needed"></a>(1) hoeveel grenzen zijn nodig?
Het eerste beslissingspunt is om te bepalen hoeveel beveiligingsgrenzen nodig zijn in een bepaald scenario:

* Eén grens: Een op de front-perimeternetwerk, tussen het virtuele netwerk en Internet.
* Twee grenzen: Een aan de Internet-zijde van het perimeternetwerk, en een andere tussen het perimeternetwerk netwerksubnet en de back-end-subnetten in de Azure-netwerken.
* Drie grenzen: Op de Internet-zijde van het perimeternetwerk, tussen het perimeternetwerk en de back-end-subnetten en een tussen de back-end-subnetten en het on-premises netwerk.
* N grenzen: Het nummer van een variabele. Er is geen limiet aan het aantal grenzen voor netwerkbeveiliging die kunnen worden toegepast in een bepaald netwerk, afhankelijk van de beveiligingsvereisten.

Het aantal en type van grenzen die nodig zijn variëren op basis van omgaan met risico's van een bedrijf en het specifieke scenario wordt geïmplementeerd. Dit besluit wordt vaak samen met meerdere groepen binnen een organisatie, vaak met inbegrip van een team risico en de naleving, een netwerk en platform-team en het ontwikkelingsteam van een toepassing uitgevoerd. Mensen met kennis van beveiliging, de betrokken gegevens en de technologieën die wordt gebruikt, moet een zeg hebben in deze beslissing om te controleren of de juiste koers voor elke implementatie.

> [!TIP]
> Gebruik het kleinste aantal grenzen die voldoen aan de beveiligingsvereisten voor een bepaalde situatie. Met meer grenzen, bewerkingen en het oplossen van problemen kunnen lastig zijn meer, evenals de beheer-overhead die betrokken zijn bij het beheren van meerdere beleidsregels voor de grens na verloop van tijd. Onvoldoende grenzen worden echter risico verhogen. De balans te vinden is essentieel.
>
>

![6]

De voorgaande afbeelding ziet u een weergave op hoog niveau van een netwerk met drie beveiliging grens. De grenzen zijn tussen het perimeternetwerk en het Internet, de Azure front-end en back-end particuliere subnetten, en het Azure back-end-subnet en het bedrijfsnetwerk van de on-premises.

#### <a name="2-where-are-the-boundaries-located"></a>2) waar zich de grenzen bevinden?
Als het aantal grenzen zijn besloten, is waar u ze implementeren de volgende beslissing. Er zijn in het algemeen drie opties:

* Met behulp van een Internet-intermediaire service (bijvoorbeeld een cloud-gebaseerde Web application firewall, die niet wordt besproken in dit document)
* Met behulp van systeemeigen functies en/of virtuele netwerkapparaten in Azure
* Met behulp van fysieke apparaten op de on-premises netwerk

Op alleen Azure-netwerken zijn de opties voor systeemeigen Azure-functies (bijvoorbeeld Azure Load Balancers) of virtuele netwerkapparaten uit de uitgebreide partnerecosysteem van Azure (bijvoorbeeld, firewalls van Check Point).

Als een grens tussen Azure en een on-premises-netwerk is vereist, kan de security-apparaten kunnen zich bevinden aan beide zijden van de verbinding (of beide zijden). Een beslissing moet dus worden gemaakt op de locatie security gear plaatsen.

In de vorige afbeelding, de Internet-perimeternetwerk en de grenzen van de front-to-back-end zijn volledig zijn opgenomen in Azure, en moeten systeemeigen Azure-functies of netwerk virtuele apparaten. Beveiligingsapparaten op de grens tussen Azure (back-end-subnet) en het bedrijfsnetwerk kunnen zijn op de Azure-kant of de kant van de on-premises of zelfs een combinatie van apparaten aan beide zijden. Er kan worden grote voordelen en nadelen aan een van de opties die ernstig moeten worden beschouwd.

Bijvoorbeeld, heeft met behulp van bestaande fysieke beveiliging tandwiel aan de kant van on-premises netwerk het voordeel dat er geen nieuwe gear nodig is. Er is alleen nodig herconfiguratie. Het nadeel is echter dat al het verkeer van Azure naar de on-premises netwerk terugkeren moet kunnen worden bekeken door het tandrad van beveiliging. Dus Azure-naar-Azure-verkeer aanzienlijke latentie kan betekenen en invloed op toepassingsprestaties en gebruikerservaring, als deze is gedwongen terug naar de on-premises netwerk voor beveiliging wordt afgedwongen.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) hoe worden de grenzen geïmplementeerd?
Elke beveiligingsgrens heeft waarschijnlijk verschillende vereisten (bijvoorbeeld-id's en firewall-regels aan de Internet-zijde van het perimeternetwerk, maar alleen ACL's tussen het perimeternetwerk en de back-end-subnet). Bepalen waarop apparaat (of het aantal apparaten) te gebruiken afhankelijk van het scenario en beveiligingsvereisten. In de volgende sectie bespreken voorbeelden 1, 2 en 3 enkele mogelijkheden die kunnen worden gebruikt. Controleren van de functies van de systeemeigen Azure-netwerk en de apparaten die beschikbaar zijn in Azure uit de partnerecosysteem, ziet u de allerlei opties beschikbaar om op te lossen vrijwel elk scenario.

Een ander beslissingspunt voor implementatie van de sleutel is hoe u de on-premises netwerk verbinden met Azure. Moet u de Azure-virtuele gateway of een virtueel netwerkapparaat gebruiken? Deze opties worden in meer detail in de volgende sectie (voorbeelden 4, 5 en 6) besproken.

Bovendien het verkeer tussen virtuele netwerken in Azure nodig. Deze scenario's wordt in de toekomst worden toegevoegd.

Als u de antwoorden op de vorige vragen, weet de [snel starten](#fast-start) sectie kunt identificeren welke voorbeelden zijn het meest geschikt is voor een bepaald scenario.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Voorbeelden: Het bouwen van grenzen voor netwerkbeveiliging met Azure virtual networks
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Voorbeeld 1 een perimeternetwerk ter bescherming van toepassingen met nsg's maken
[Terug naar snel aan de slag](#fast-start) | [gedetailleerde instructies voor dit voorbeeld maken][Example1]

[![7]][7]

#### <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement met de volgende bronnen:

- Een enkele resourcegroep
- Een virtueel netwerk met twee subnetten: 'FrontEnd' en 'Back-end'
- Een Netwerkbeveiligingsgroep die is toegepast op beide subnetten
- Een Windows-server die staat voor een toepassing webserver ("IIS01")
- Twee Windows-servers die staan voor toepassingsservers van back-end ("AppVM01", "AppVM02")
- Een Windows-server die staat voor een DNS-server ("DNS01")
- Een openbaar IP-adres dat is gekoppeld aan de webserver van de toepassing

Zie voor scripts en een Azure Resource Manager-sjabloon, de [gedetailleerde instructies build][Example1].

#### <a name="nsg-description"></a>Beschrijving van de NSG
In dit voorbeeld is een NSG-groep gemaakt en vervolgens geladen met zes regels.

> [!TIP]
> In het algemeen moet u uw specifieke regels voor 'Toestaan' eerst, gevolgd door de algemene regels voor "Deny". De opgegeven prioriteit bepaalt welke regels worden eerst geëvalueerd. Wanneer verkeer toe te passen op een specifieke regel wordt gevonden, wordt er geen verdere regels worden geëvalueerd. NSG-regels kunnen toepassen in de binnenkomende of uitgaande richting (vanuit het perspectief van het subnet).
>
>

De volgende regels zijn declaratief, gebouwd voor binnenkomend verkeer:

1. Interne DNS-verkeer (poort 53) is toegestaan.
2. RDP-verkeer (poort 3389) van het Internet op elke virtuele Machine is toegestaan.
3. HTTP-verkeer (poort 80) van het Internet naar de webserver (IIS01) is toegestaan.
4. Al het verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan.
5. Al het verkeer (alle poorten) van het Internet naar het hele virtuele netwerk (beide subnetten) is geweigerd.
6. Al het verkeer (alle poorten) van het front-end-subnet naar de back-end-subnet wordt geweigerd.

Met deze regels gekoppeld aan elk subnet, als een HTTP-aanvraag binnenkomend verkeer van Internet naar de webserver, beide regels 3 is (toestaan) en 5 (weigeren) wilt toepassen. Maar omdat regel 3 een hogere prioriteit heeft, alleen zou worden toegepast en regel 5 niet aan de orde komt. De HTTP-aanvraag zou dus met de webserver worden toegestaan. Als dat dezelfde verkeer is probeert te bereiken van de server DNS01, regel 5 (weigeren) is de eerste om toe te passen en het verkeer niet mogen worden doorgegeven aan de server. Regel 6 (weigeren) Hiermee blokkeert u de front-end-subnet van de communicatie met de back-end-subnet (met uitzondering van toegestaan verkeer in regels 1 en 4). Deze regelset beveiligt de back-endnetwerk in het geval een aanvaller de web-App op de front-end wordt aangetast. De aanvaller zou beperkt toegang tot het 'beveiligde' back-end-netwerk (alleen voor resources weergegeven op de server AppVM01).

Er is een uitgaande standaardregel waarmee uitgaand verkeer naar Internet. In dit voorbeeld zijn we uitgaand verkeer wordt toegestaan en alle regels voor uitgaand verkeer niet wordt gewijzigd. Als u wilt vergrendelen verkeer in beide richtingen, gebruiker gedefinieerde routering is vereist (Zie het voorbeeld 3).

#### <a name="conclusion"></a>Conclusie
In dit voorbeeld is een relatief eenvoudige en duidelijke manier van het isoleren van de back-end-subnet van het binnenkomende verkeer. Zie voor meer informatie de [gedetailleerde instructies build][Example1]. Deze instructies zijn onder andere:

* Over het bouwen van deze perimeternetwerk met klassieke PowerShell-scripts.
* Over het bouwen van deze perimeternetwerk met een Azure Resource Manager-sjabloon.
* Gedetailleerde beschrijvingen van elke NSG-opdracht.
* Gedetailleerde verkeer stroom scenario's wordt weergegeven hoe verkeer wordt toegestaan of geweigerd in elke laag.


### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Voorbeeld 2 een perimeternetwerk ter bescherming van toepassingen met een firewall en nsg's maken
[Terug naar snel aan de slag](#fast-start) | [gedetailleerde instructies voor dit voorbeeld maken][Example2]

[![8]][8]

#### <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement met de volgende bronnen:

* Een enkele resourcegroep
* Een virtueel netwerk met twee subnetten: 'FrontEnd' en 'Back-end'
* Een Netwerkbeveiligingsgroep die is toegepast op beide subnetten
* Een virtueel netwerkapparaat, in dit geval een firewall, die is verbonden met het front-end-subnet
* Een Windows-server die staat voor een toepassing webserver ("IIS01")
* Twee Windows-servers die staan voor toepassingsservers van back-end ("AppVM01", "AppVM02")
* Een Windows-server die staat voor een DNS-server ("DNS01")

Zie voor scripts en een Azure Resource Manager-sjabloon, de [gedetailleerde instructies build][Example2].

#### <a name="nsg-description"></a>Beschrijving van de NSG
In dit voorbeeld is een NSG-groep gemaakt en vervolgens geladen met zes regels.

> [!TIP]
> In het algemeen moet u uw specifieke regels voor 'Toestaan' eerst, gevolgd door de algemene regels voor "Deny". De opgegeven prioriteit bepaalt welke regels worden eerst geëvalueerd. Wanneer verkeer toe te passen op een specifieke regel wordt gevonden, wordt er geen verdere regels worden geëvalueerd. NSG-regels kunnen toepassen in de binnenkomende of uitgaande richting (vanuit het perspectief van het subnet).
>
>

De volgende regels zijn declaratief, gebouwd voor binnenkomend verkeer:

1. Interne DNS-verkeer (poort 53) is toegestaan.
2. RDP-verkeer (poort 3389) van het Internet op elke virtuele Machine is toegestaan.
3. Alle internetverkeer (alle poorten) naar het virtuele netwerkapparaat (firewall) is toegestaan.
4. Al het verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan.
5. Al het verkeer (alle poorten) van het Internet naar het hele virtuele netwerk (beide subnetten) is geweigerd.
6. Al het verkeer (alle poorten) van het front-end-subnet naar de back-end-subnet wordt geweigerd.

Met deze regels gekoppeld aan elk subnet, als een HTTP-aanvraag binnenkomend verkeer van Internet naar de firewall, beide regels 3 is (toestaan) en 5 (weigeren) wilt toepassen. Maar omdat regel 3 een hogere prioriteit heeft, alleen zou worden toegepast en regel 5 niet aan de orde komt. De HTTP-aanvraag zou dus worden toegestaan aan de firewall. Als dat dezelfde verkeer is probeert te bereiken van de server IIS01, zelfs als deze zich op het front-end-subnet, regel 5 (weigeren) wilt toepassen, en het verkeer niet mogen worden doorgegeven aan de server. Regel 6 (weigeren) Hiermee blokkeert u de front-end-subnet van de communicatie met de back-end-subnet (met uitzondering van toegestaan verkeer in regels 1 en 4). Deze regelset beveiligt de back-endnetwerk in het geval een aanvaller de web-App op de front-end wordt aangetast. De aanvaller zou beperkt toegang tot het 'beveiligde' back-end-netwerk (alleen voor resources weergegeven op de server AppVM01).

Er is een uitgaande standaardregel waarmee uitgaand verkeer naar Internet. In dit voorbeeld zijn we uitgaand verkeer wordt toegestaan en alle regels voor uitgaand verkeer niet wordt gewijzigd. Als u wilt vergrendelen verkeer in beide richtingen, gebruiker gedefinieerde routering is vereist (Zie het voorbeeld 3).

#### <a name="firewall-rule-description"></a>De beschrijving van de firewall-regel
Op de firewall, moet regels voor doorsturen worden gemaakt. Regel is nodig omdat hierbij alleen routes internetverkeer in gebonden aan de firewall en vervolgens naar de webserver, slechts één doorsturen via het netwerk netwerkadresomzetting (NAT).

De regel voor doorsturen accepteert inkomende bronadres die komt binnen via de firewall probeert te bereiken HTTP (poort 80 of 443 voor HTTPS). Deze heeft verzonden buiten de firewall van de lokale interface en omgeleid naar de webserver met het IP-adres van 10.0.1.5.

#### <a name="conclusion"></a>Conclusie
In dit voorbeeld is een relatief eenvoudig manier van het beveiligen van uw toepassing met een firewall en isoleren van de back-end-subnet van het binnenkomende verkeer. Zie voor meer informatie de [gedetailleerde instructies build][Example2]. Deze instructies zijn onder andere:

* Over het bouwen van deze perimeternetwerk met klassieke PowerShell-scripts.
* Over het bouwen van dit voorbeeld met een Azure Resource Manager-sjabloon.
* Gedetailleerde beschrijvingen van elke NSG opdracht en firewall-regel.
* Gedetailleerde verkeer stroom scenario's wordt weergegeven hoe verkeer wordt toegestaan of geweigerd in elke laag.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-and-udr-and-nsg"></a>Voorbeeld 3 bouwen in een perimeternetwerk ter bescherming van netwerken met een firewall, UDR en NSG
[Terug naar snel aan de slag](#fast-start) | [gedetailleerde instructies voor dit voorbeeld maken][Example3]

[![9]][9]

#### <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement met de volgende bronnen:

* Een enkele resourcegroep
* Een virtueel netwerk met drie subnetten: “SecNet”, “FrontEnd”, and “BackEnd”
* Een virtueel netwerkapparaat, in dit geval een firewall, die is verbonden met het subnet SecNet
* Een Windows-server die staat voor een toepassing webserver ("IIS01")
* Twee Windows-servers die staan voor toepassingsservers van back-end ("AppVM01", "AppVM02")
* Een Windows-server die staat voor een DNS-server ("DNS01")

Zie voor scripts en een Azure Resource Manager-sjabloon, de [gedetailleerde instructies build][Example3].

#### <a name="udr-description"></a>UDR-beschrijving
Standaard worden de volgende systeemroutes gedefinieerd als:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

De VNETLocal is altijd een of meer gedefinieerde adresvoorvoegsels die gezamenlijk het virtuele netwerk voor dat specifieke netwerk (dat wil zeggen, verandert van virtueel netwerk met virtuele netwerk, afhankelijk van hoe elk specifieke virtueel netwerk is gedefinieerd). De resterende systeemroutes statisch zijn en de standaardinstellingen van zoals aangegeven in de tabel.

In dit voorbeeld worden twee routeringstabellen gemaakt, één voor de front-end en back-end-subnetten. Elke tabel is geladen met statische routes die geschikt is voor het opgegeven subnet. In dit voorbeeld elke tabel heeft drie routes die alle verkeer (0.0.0.0/0) via de firewall (volgende hop = virtuele toestel IP-adres):

1. Lokaal subnetverkeer met geen ' volgende hop ' gedefinieerd voor het lokale subnetverkeer toestaan om de firewall over te slaan.
2. Verkeer in virtuele netwerken met een ' volgende hop ' gedefinieerd als een firewall. Deze volgende hop overschrijft de standaardregel waarmee lokale verkeer in virtuele netwerken te routeren rechtstreeks.
3. Alle resterende verkeer (0/0) met een ' volgende hop ' gedefinieerd als de firewall.

> [!TIP]
> Omdat u de vermelding van het lokale subnet niet hoeft in de communicatie van UDR-einden lokale subnet.
>
> * In ons voorbeeld is de 10.0.1.0/24 die verwijst naar VNETLocal kritieke! Zonder deze mislukken de Web Server (10.0.1.4) dat is bestemd voor een andere lokale server (bijvoorbeeld) 10.0.1.25 verlaten-pakket als ze worden verzonden naar de NVA. De NVA wordt verzenden naar het subnet en het subnet wordt te verzenden naar de NVA in een oneindige lus.
> * De kans op een routeringslus zijn doorgaans hoger op apparaten met meerdere NIC's die zijn verbonden met verschillende subnetten, dit is vaak van traditionele, on-premises apparaten.
>
>

Zodra de routeringstabel zijn gemaakt, moeten ze worden gekoppeld aan hun subnetten. De front-end-subnet-routeringstabel, nadat deze zijn gemaakt en gekoppeld aan het subnet, eruit als deze uitvoer:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

> [!NOTE]
> UDR kan nu worden toegepast op het gatewaysubnet waarop het ExpressRoute-circuit is verbonden.
>
> Voorbeelden voor het inschakelen van uw perimeternetwerk ExpressRoute of site-naar-site-netwerk worden weergegeven in de voorbeelden 3 en 4.
>
>

#### <a name="ip-forwarding-description"></a>Beschrijving doorsturen via IP
Doorsturen via IP is een functie companion UDR. Doorsturen via IP is een instelling op een virtueel apparaat dat kan worden ontvangen van verkeer niet specifiek zijn gericht op het apparaat en stuur deze vervolgens dat verkeer naar de uiteindelijke bestemming.

Bijvoorbeeld, als AppVM01 maakt een aanvraag naar de server DNS01, zou UDR dit verkeer gerouteerd naar de firewall. Met het IP-doorsturen is ingeschakeld, wordt het verkeer voor de doel-DNS01 (10.0.2.4) geaccepteerd door het apparaat (10.0.0.4) en vervolgens doorgestuurd naar de uiteindelijke bestemming (10.0.2.4). Zonder doorsturen via IP op de firewall is ingeschakeld, wordt verkeer niet geaccepteerd door het apparaat zelfs als de routetabel de firewall als de volgende hop is. Voor het gebruik van een virtueel apparaat, is het essentieel om te onthouden om in te schakelen, samen met de UDR doorsturen via IP.

#### <a name="nsg-description"></a>Beschrijving van de NSG
In dit voorbeeld is een NSG-groep gemaakt en vervolgens met een enkele regel geladen. Deze groep is vervolgens alleen aan de front-end en back-end-subnetten (niet de SecNet) gebonden. Declaratief wordt met de volgende regel gemaakt:

* Al het verkeer (alle poorten) van het Internet naar het hele virtuele netwerk (alle subnetten) is geweigerd.

Hoewel nsg's in dit voorbeeld worden gebruikt, is het belangrijkste doel als een secundaire laag van beveiliging tegen handmatige onjuiste configuratie. Het doel is om alle binnenkomend verkeer van Internet aan de front-end en back-end subnetten blokkeren. Verkeer alleen via het subnet SecNet aan de firewall moet stromen (en vervolgens, indien van toepassing op de front-end en back-end-subnetten). Bovendien aan de regels UDR aanwezig is, zou al het verkeer dat gemakkelijk in de front-end en back-end-subnetten worden omgeleid om aan de firewall (dankzij de udr-route). De firewall verkeer zou zien als een asymmetrische stroom en het uitgaande verkeer wilt verwijderen. Er zijn dus drie beveiligingslagen beveiligen van de subnetten:

* Er zijn geen openbare IP-adressen voor elk front-end en back-end-NIC's.
* Nsg's weigeren verkeer vanaf Internet.
* De firewall laten vallen asymmetrische verkeer.

Een interessante met betrekking tot de NSG in dit voorbeeld is dat deze slechts één regel, die is om te weigeren internetverkeer naar het hele virtuele netwerk, met inbegrip van het subnet beveiliging bevat. Echter, omdat de NSG wordt alleen gekoppeld aan de front-end en back-end-subnetten, de regel is niet verwerkt op het verkeer inkomend in het subnet van de beveiliging. Als gevolg hiervan stroomt verkeer met het subnet van de beveiliging.

#### <a name="firewall-rules"></a>Firewall-regels
Op de firewall, moet regels voor doorsturen worden gemaakt. Aangezien de firewall blokkeert of doorsturen van alle binnenkomende, uitgaande en binnen virtuele netwerkverkeer is, worden veel firewallregels die nodig zijn. Alle binnenkomend verkeer treffers ook de beveiligingsservice openbare IP-adres (op verschillende poorten), om te worden verwerkt door de firewall. Een best practice is om het diagram van de logische stromen voordat het instellen van de subnetten en firewall-regels om te voorkomen dat later bijwerken. De volgende afbeelding is een logische weergave van de firewall-regels voor dit voorbeeld:

![10]

> [!NOTE]
> Op basis van het virtuele netwerkapparaat gebruikt, variëren de beheerpoorten. In dit voorbeeld wordt een Barracuda NextGen Firewall naar wordt verwezen, waarbij poort 22, 801 en 807 worden gebruikt. Raadpleeg de documentatie van de leverancier apparaat voor de exacte poorten gebruikt voor het beheer van het apparaat wordt gebruikt.
>
>

#### <a name="firewall-rules-description"></a>Beschrijving van de firewall-regels
In het bovenstaande logische diagram wordt het subnet van de beveiliging niet weergegeven omdat de firewall de enige bron op dat subnet is. Het diagram worden weergegeven voor de firewall-regels en hoe ze logisch toestaan of verkeersstromen, niet de werkelijke gerouteerde pad weigeren. Ook de externe poorten voor de RDP-verkeer hoger zijn geselecteerd variabele poorten (8014 – 8026) en zijn geselecteerd om te losjes zijn afgestemd op de laatste twee octetten van het lokale IP-adres voor de leesbaarheid te vergemakkelijken (bijvoorbeeld 10.0.1.4-adres van de lokale server is gekoppeld externe poort 8014). Een niet-conflicterende hogere poorten, kunnen echter worden gebruikt.

In dit voorbeeld moet zeven soorten regels:

* Externe regels (voor binnenkomend verkeer):
  1. Firewallregel voor beheer: Deze omleidings-App-regel staat verkeer moet worden doorgegeven aan de beheerpoorten van het virtuele netwerkapparaat.
  2. RDP-regels (voor elke Windows-server): Deze vier regels (één voor elke server) kunnen beheer van de afzonderlijke servers via RDP. De vier regels voor RDP kunnen ook worden samengevoegd tot één regel, afhankelijk van de mogelijkheden van het virtuele netwerkapparaat wordt gebruikt.
  3. Regels voor netwerkverkeer van toepassing: Er zijn twee van deze regels, de eerste voor de front-end-webverkeer te genereren en het tweede voor de back-end-verkeer (bijvoorbeeld webserver als de gegevenslaag). De configuratie van deze regels is afhankelijk van de netwerkarchitectuur (waarin uw servers worden geplaatst) en het verkeer naar stromen (welke richting het verkeer verloopt, en op welke poorten worden gebruikt).
     * De eerste regel staat de feitelijke toepassingsverkeer bereiken van de toepassingsserver. Terwijl de andere regels voor beveiliging en beheer, zijn regels voor netwerkverkeer toepassing wat kunnen externe gebruikers of services voor toegang tot de toepassingen. In dit voorbeeld moet u er één webserver is op poort 80. Een firewallregel voor één toepassing leidt dus binnenkomend verkeer naar het externe IP-adres, naar het interne IP-adres van web-servers. De sessie omgeleide verkeer zou via NAT worden vertaald naar de interne server.
     * De tweede regel wordt de back-end-regel waarmee de webserver om te communiceren met de server AppVM01 (maar niet AppVM02) via een willekeurige poort.
* Interne regels (voor de verkeersstroom binnen virtual network-verkeer)
  1. Uitgaand naar internetregel: Deze regel staat verkeer van elk netwerk moeten worden doorgegeven aan de geselecteerde netwerken. Deze regel is meestal een standaardregel al op de firewall, maar een uitgeschakelde status. Deze regel moet worden ingeschakeld voor dit voorbeeld.
  2. DNS-regel: Deze regel kunnen alleen DNS (poort 53)-verkeer moeten worden doorgegeven aan de DNS-server. De meeste verkeer van de front-end naar de back-end is geblokkeerd voor deze omgeving. Deze regel kunnen specifiek DNS vanuit een lokale subnet.
  3. Subnet naar subnet regel: Deze regel is om een server op de back-end-subnet verbinding maken met elke server in het front-end-subnet (maar niet omgekeerd).
* Failsafe regel (voor verkeer dat niet voldoet aan een van de vorige):
  1. Alle verkeersregel voor weigeren: Deze regel voor weigeren moet altijd de laatste regel (voor wat betreft prioriteit) en als zodanig als netwerkverkeer niet overeenkomt met een van de vorige regels wordt deze verwijderd door deze regel. Deze regel is een standaardregel en meestal in-place en actief. Er zijn geen wijzigingen zijn meestal nodig voor deze regel.

> [!TIP]
> Op de tweede toepassing verkeersregel, ter vereenvoudiging van dit voorbeeld wordt is een willekeurige poort toegestaan. In een echte scenario, moeten de meest specifieke poort- en adresbereiken te verminderen van de kwetsbaarheid voor aanvallen van deze regel worden gebruikt.
>
>

Nadat u de vorige regels hebt gemaakt, is het belangrijk om te controleren van de prioriteit van elke regel om te controleren of verkeer wordt toegestaan of geweigerd naar wens. In dit voorbeeld worden de regels in volgorde van prioriteit.

#### <a name="conclusion"></a>Conclusie
In dit voorbeeld is een complexere maar manier voor het beveiligen en isoleren van het netwerk dan de vorige voorbeelden te voltooien. (Voorbeeld 2 beveiligt alleen de toepassing en subnetten alleen Hiermee isoleert u voorbeeld 1). Dit ontwerp kunt voor het bewaken van verkeer in beide richtingen, en niet alleen de binnenkomende toepassingsserver beveiligt, maar wordt afgedwongen beveiligingsbeleid netwerk voor alle servers in dit netwerk. Ook, afhankelijk van het apparaat gebruikt, volledige verkeer controle en het bewustzijn kunnen worden bereikt. Zie voor meer informatie de [gedetailleerde instructies build][Example3]. Deze instructies zijn onder andere:

* Over het bouwen van dit voorbeeld perimeternetwerk met klassieke PowerShell-scripts.
* Over het bouwen van dit voorbeeld met een Azure Resource Manager-sjabloon.
* Gedetailleerde beschrijvingen van elke UDR, NSG opdracht- en firewall-regel.
* Gedetailleerde verkeer stroom scenario's wordt weergegeven hoe verkeer wordt toegestaan of geweigerd in elke laag.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn"></a>Voorbeeld 4 een hybride verbinding toevoegen met een site-naar-site, een virtueel apparaat VPN
[Terug naar snel aan de slag](#fast-start) | Build instructies beschikbaar snel gedetailleerde

[![11]][11]

#### <a name="environment-description"></a>Beschrijving van de omgeving
Hybride netwerken met behulp van een virtueel netwerkapparaat (NVA) kan worden toegevoegd aan een van de typen perimeternetwerk die worden beschreven in de voorbeelden 1, 2 of 3.

Zoals u in de vorige afbeelding, wordt een VPN-verbinding via Internet (site-naar-site) naar een on-premises netwerk verbinden met een Azure-netwerk via een NVA gebruikt.

> [!NOTE]
> Als u ExpressRoute met de openbare Azure-Peering-optie is ingeschakeld gebruikt, kan een statische route moet worden gemaakt. Deze statische route moet routeren naar de NVA VPN-IP-adres van uw zakelijke Internet en niet via de ExpressRoute-verbinding. Het NAT-apparaat vereist op de optie ExpressRoute openbare Azure-Peering kunt de VPN-sessie verbreken.
>
>

Nadat de VPN-verbinding ingesteld is, wordt de NVA de centrale hub voor alle netwerken en subnetten. De firewall-regels voor doorsturen te bepalen welke verkeersstromen zijn toegestaan, worden omgezet via NAT, worden omgeleid of verwijderd (zelfs voor verkeersstromen tussen de on-premises netwerk en Azure).

Verkeersstromen moeten zorgvuldig worden overwogen als ze kunnen worden geoptimaliseerd of gedegradeerd door dit ontwerppatroon zijn afhankelijk van de specifieke use-case.

Met behulp van de omgeving die is ingebouwd in voorbeeld 3 en vervolgens een site-naar-site VPN hybride netwerkverbinding, toe te voegen geeft het ontwerp van de volgende:

[![12]][12]

De on-premises router of andere netwerkapparaten die compatibel is met uw NVA voor VPN-verbinding, wordt de VPN-client. Deze fysieke apparaat zou zijn verantwoordelijk voor het initiëren en onderhouden van de VPN-verbinding met uw NVA.

Logisch naar de NVA, het netwerk ziet eruit als vier afzonderlijke "beveiligingszones" aan de regels op de NVA wordt de primaire directeur van verkeer tussen deze zones:

![13]

#### <a name="conclusion"></a>Conclusie
Het toevoegen van een site-naar-site VPN hybride netwerkverbinding met een Azure-netwerk kan de on-premises netwerk uitbreiden naar Azure op een veilige manier. In met behulp van een VPN-verbinding, het verkeer wordt versleuteld en leidt via Internet. De NVA in het volgende voorbeeld biedt een centrale locatie voor het beheren van het beveiligingsbeleid af te dwingen. Zie de gedetailleerde build-instructies (bewezen) voor meer informatie. Deze instructies zijn onder andere:

* Over het bouwen van dit voorbeeld perimeternetwerk met PowerShell-scripts.
* Over het bouwen van dit voorbeeld met een Azure Resource Manager-sjabloon.
* Gedetailleerde verkeer stroom scenario's wordt weergegeven hoe verkeer stroomt via dit ontwerp.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway"></a>Voorbeeld 5 een hybride verbinding met de Azure VPN-gateway van een site-naar-site toevoegen
[Terug naar snel aan de slag](#fast-start) | Build instructies beschikbaar snel gedetailleerde

[![14]][14]

#### <a name="environment-description"></a>Beschrijving van de omgeving
Hybride netwerken met behulp van een Azure VPN-gateway kan worden toegevoegd aan een type perimeternetwerk die worden beschreven in de voorbeelden 1 of 2.

Zoals u in de afbeelding hierboven, wordt een VPN-verbinding via Internet (site-naar-site) gebruikt een on-premises netwerk verbinden met een Azure-netwerk via een Azure VPN-gateway.

> [!NOTE]
> Als u ExpressRoute met de openbare Azure-Peering-optie is ingeschakeld gebruikt, kan een statische route moet worden gemaakt. Deze statische route moet routeren naar de NVA VPN-IP-adres van uw zakelijke Internet en niet via het WAN ExpressRoute. Het NAT-apparaat vereist op de optie ExpressRoute openbare Azure-Peering kunt de VPN-sessie verbreken.
>
>

De volgende afbeelding toont de twee netwerk randen in dit voorbeeld. Op de eerste rand bepalen de NVA en nsg's verkeersstromen voor verkeersstroom binnen Azure-netwerken en tussen Azure en Internet. De tweede rand is de Azure VPN-gateway is van de rand van een afzonderlijke en geïsoleerd netwerk tussen on-premises en Azure.

Verkeersstromen moeten zorgvuldig worden overwogen als ze kunnen worden geoptimaliseerd of gedegradeerd door dit ontwerppatroon zijn afhankelijk van de specifieke use-case.

Met behulp van de omgeving die is ingebouwd in voorbeeld 1 en vervolgens een site-naar-site VPN hybride netwerkverbinding, toe te voegen geeft het ontwerp van de volgende:

[![15]][15]

#### <a name="conclusion"></a>Conclusie
Het toevoegen van een site-naar-site VPN hybride netwerkverbinding met een Azure-netwerk kan de on-premises netwerk uitbreiden naar Azure op een veilige manier. Met behulp van de systeemeigen Azure VPN-gateway, uw verkeer is IPSec-versleuteld en stuurt via Internet. Met behulp van de Azure VPN-gateway kan ook een goedkopere optie (Er zijn geen aanvullende licenties kosten net als bij externe NVA's) bieden. Deze optie is meest voordelige in voorbeeld 1, waarbij er geen NVA wordt gebruikt. Zie de gedetailleerde build-instructies (bewezen) voor meer informatie. Deze instructies zijn onder andere:

* Over het bouwen van dit voorbeeld perimeternetwerk met PowerShell-scripts.
* Over het bouwen van dit voorbeeld met een Azure Resource Manager-sjabloon.
* Gedetailleerde verkeer stroom scenario's wordt weergegeven hoe verkeer stroomt via dit ontwerp.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Voorbeeld 6 toevoegen, een hybride verbinding met ExpressRoute
[Terug naar snel aan de slag](#fast-start) | Build instructies beschikbaar snel gedetailleerde

[![16]][16]

#### <a name="environment-description"></a>Beschrijving van de omgeving
Hybride netwerken met behulp van een particuliere ExpressRoute-peeringverbinding kan worden toegevoegd aan een van beide type perimeternetwerk die worden beschreven in de voorbeelden 1 of 2.

Zoals u in de afbeelding hierboven, biedt ExpressRoute-privépeering een rechtstreekse verbinding tussen uw on-premises netwerk en de Azure-netwerk. Verkeer transits alleen het serviceprovider-netwerk en het Microsoft Azure-netwerk, het Internet nooit aan te raken.

> [!TIP]
> Met behulp van ExpressRoute, houdt u zakelijke verkeer buiten Internet. Bovendien kunnen hierdoor serviceovereenkomsten van uw ExpressRoute-provider. De Azure-Gateway kan maximaal 10 Gbps met ExpressRoute kunt doorgeven, terwijl de maximale doorvoer van Azure-Gateway met site-naar-site-VPN's, 200 Mbps.
>
>

Zoals in het volgende diagram wordt weergegeven, met deze optie heeft de omgeving nu twee netwerk randen. De NVA en NSG beheren van verkeersstromen voor verkeersstroom binnen Azure-netwerken en tussen Azure en het Internet, terwijl de gateway de rand van een afzonderlijke en geïsoleerd netwerk tussen on-premises en Azure is.

Verkeersstromen moeten zorgvuldig worden overwogen als ze kunnen worden geoptimaliseerd of gedegradeerd door dit ontwerppatroon zijn afhankelijk van de specifieke use-case.

Met behulp van de omgeving die is ingebouwd in voorbeeld 1 en vervolgens toe te voegen een ExpressRoute-verbinding voor hybride netwerk, geeft het ontwerp van de volgende:

[![17]][17]

#### <a name="conclusion"></a>Conclusie
Het toevoegen van een particuliere ExpressRoute-Peering-netwerkverbinding kan de on-premises netwerk uitbreiden naar Azure in een beveiligde, lagere latentie, betere prestaties manier. Met behulp van de systeemeigen Azure-Gateway, zoals in dit voorbeeld biedt ook een goedkopere optie (geen aanvullende licentievereisten net als bij de NVA's van derden). Zie de gedetailleerde build-instructies (bewezen) voor meer informatie. Deze instructies zijn onder andere:

* Over het bouwen van dit voorbeeld perimeternetwerk met PowerShell-scripts.
* Over het bouwen van dit voorbeeld met een Azure Resource Manager-sjabloon.
* Gedetailleerde verkeer stroom scenario's wordt weergegeven hoe verkeer stroomt via dit ontwerp.

## <a name="references"></a>Verwijzingen
### <a name="helpful-websites-and-documentation"></a>Nuttige websites en documentatie
* Toegang tot Azure met Azure Resource Manager:
* Toegang tot Azure met PowerShell: [https://docs.microsoft.com/powershell/azureps-cmdlets-docs/](/powershell/azure/overview)
* Documentatie voor virtuele netwerken: [https://docs.microsoft.com/azure/virtual-network/](https://docs.microsoft.com/azure/virtual-network/)
* Documentatie over beveiligingsgroepen in het netwerk: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg](virtual-network/security-overview.md)
* Gebruiker gedefinieerde routering documentatie: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview](virtual-network/virtual-networks-udr-overview.md)
* Virtuele Azure-gateways: [https://docs.microsoft.com/azure/vpn-gateway/](https://docs.microsoft.com/azure/vpn-gateway/)
* Site-naar-Site VPN-verbindingen: [https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell](vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* Documentatie voor ExpressRoute (Zorg ervoor dat de secties 'Aan de slag' en "How To" Bekijk): [https://docs.microsoft.com/azure/expressroute/](https://docs.microsoft.com/azure/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "stroomdiagram beveiligingsopties"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "functies van azure-beveiliging"
[3]: ./media/best-practices-network-security/dmzcorporate.png "een Perimeternetwerk in een bedrijfsnetwerk"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "architectuur van azure-beveiliging"
[5]: ./media/best-practices-network-security/dmzazure.png "een Perimeternetwerk in een Azure-netwerk"
[6]: ./media/best-practices-network-security/dmzhybrid.png "hybride netwerk met drie grenzen voor netwerkbeveiliging"
[7]: ./media/best-practices-network-security/example1design.png "inkomende DMZ met NSG"
[8]: ./media/best-practices-network-security/example2design.png "inkomende DMZ met NVA en NSG"
[9]: ./media/best-practices-network-security/example3design.png "bidirectionele DMZ met NVA, NSG en UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "logische weergave van de Firewall-regels"
[11]: ./media/best-practices-network-security/example3designoptions.png "DMZ met NVA hybride netwerk verbonden"
[12]: ./media/best-practices-network-security/example4designs2s.png "DMZ met een NVA die zijn verbonden via een Site-naar-Site-VPN"
[13]: ./media/best-practices-network-security/example4networklogical.png "logisch netwerk vanuit het perspectief van de NVA van"
[14]: ./media/best-practices-network-security/example5designoptions.png "DMZ met Azure-Gateway verbonden Site-naar-Site hybride netwerk"
[15]: ./media/best-practices-network-security/example5designs2s.png "DMZ met Azure via Site-naar-Site VPN-Gateway"
[16]: ./media/best-practices-network-security/example6designoptions.png "DMZ met Azure-Gateway verbonden ExpressRoute hybride netwerk"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "DMZ met Azure-Gateway met behulp van een ExpressRoute-verbinding"

<!--Link References-->
[TrustCenter]: https://azure.microsoft.com/support/trust-center/compliance/
[Example1]: ./virtual-network/virtual-networks-dmz-nsg.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
