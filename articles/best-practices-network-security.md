---
title: Aanbevolen beveiligingsprocedures voor Azure-netwerk | Microsoft Docs
description: Meer informatie over sommige van de belangrijkste functies die beschikbaar zijn in Azure voor het maken van beveiligde netwerkomgevingen
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: d169387a-1243-4867-a602-01d6f2d8a2a1
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: fb5e399d4ab02a7f2805cc280b213bf5b44f6993
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="microsoft-cloud-services-and-network-security"></a>Microsoft cloud services en netwerk-beveiliging
Microsoft cloud-services leveren hyperschaal services en -infrastructuur, bedrijfsniveau mogelijkheden en veel keuzes voor hybride verbindingen. Klanten kunnen kiezen voor toegang tot deze services via Internet of met Azure ExpressRoute, waarmee u verbinding met het particuliere netwerk. Het Microsoft Azure-platform kan klanten probleemloos hun infrastructuur in de cloud uitbreiden en bouwen van meerdere lagen architecturen. Bovendien kunt derden uitgebreide mogelijkheden inschakelen door het aanbieden van beveiligingsservices en virtuele apparaten. In dit artikel biedt een overzicht van beveiliging en architectuur problemen die klanten rekening houden moeten bij het gebruik van Microsoft-cloudservices is toegankelijk via ExpressRoute. Het omvat tevens veiliger services in Azure virtuele netwerken maken.

## <a name="fast-start"></a>Snel starten
De volgende logica-grafiek kunt sturen u een voorbeeld van een specifieke van veel beveiligingstechnieken beschikbaar met de Azure-platform. Voor snelle naslag vinden in het voorbeeld dat het beste past bij uw aanvraag. Uitgebreide uitleg blijven lezen via het papier.
[![0]][0]

[Voorbeeld 1: Een perimeternetwerk (ook wel DMZ, gedemilitariseerde zone of gescreend subnet) ter bescherming van toepassingen met netwerkbeveiligingsgroepen (nsg's) maken.](#example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs)</br>
[Voorbeeld 2: Bouw een perimeternetwerk ter bescherming van toepassingen met een firewall en nsg's.](#example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs)</br>
[Voorbeeld 3: Een perimeternetwerk ter bescherming van netwerken met een firewall, de gebruiker gedefinieerde route (UDR) en het NSG bouwen.](#example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-and-udr-and-nsg)</br>
[Voorbeeld 4: Voeg een hybride verbinding met een site-naar-site, een virtueel apparaat virtueel particulier netwerk (VPN).](#example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Voorbeeld 5: Een hybride verbinding met een site-naar-site Azure VPN-gateway wordt toegevoegd.](#example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway)</br>
[Voorbeeld 6: Een hybride verbinding met ExpressRoute toevoegen.](#example-6-add-a-hybrid-connection-with-expressroute)</br>
Voorbeelden voor het toevoegen van de verbindingen tussen virtuele netwerken, hoge beschikbaarheid en service-koppeling wordt toegevoegd aan dit document via de komende maanden.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Microsoft-naleving en infrastructuur beveiliging
Waarmee organisaties kunnen voldoen aan de nationaal, regionaal en branchespecifieke vereisten voor het verzamelen en gebruiken van personen gegevens, biedt Microsoft meer dan 40 certificeringen en verklaringen. De meest uitgebreide set van een cloud serviceprovider.

Zie de informatie over de compatibiliteit op voor meer informatie de [Microsoft Trust Center][TrustCenter].

Microsoft heeft een uitgebreide benadering van cloudinfrastructuur die nodig zijn voor het uitvoeren van de globale services hyperschaal beveiligen. Microsoft-cloudinfrastructuur omvat hardware, software, netwerken, en administratieve en medewerkers, naast de fysieke-datacenters.

![2]

Deze aanpak biedt een veiliger basis voor klanten om hun services in de cloud van Microsoft te implementeren. De volgende stap is voor klanten ontwerpen en maken van een beveiligingsarchitectuur ter bescherming van deze services.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Traditionele beveiliging architecturen en perimeternetwerken
Hoewel Microsoft sterk investeert in de cloudinfrastructuur beveiligen, moeten klanten ook beveiligen hun cloud-services en -resourcegroepen. Een benadering van meerlaagse beveiliging biedt de beste verdediging. De beveiligingszone van een perimeter-netwerk beveiligt interne netwerkbronnen vanaf een niet-vertrouwd netwerk. Een perimeternetwerk verwijst naar de randen of het delen van het netwerk tussen het Internet en de beveiligde enterprise IT-infrastructuur.

In typische ondernemingsnetwerken, wordt de basisinfrastructuur sterk verbeterd op zowel met meerdere lagen beveiligingsapparaten. De grens van elke laag bestaat uit de apparaten en beleid afdwingingspunten. Elke laag kan een combinatie van de volgende beveiliging netwerkapparaten bevatten: firewalls, voorkomen van Denial of Service (DoS), inbraakdetectie of beveiliging systemen (id's / IP-Adressen) en VPN-apparaten. Afdwingen van beleid kan duren voordat de vorm van de firewall-beleid, toegangsbeheerlijsten (ACL's) of specifieke routering. De eerste regel verdediging in het netwerk, rechtstreeks accepteren van binnenkomend verkeer vanaf Internet, is een combinatie van deze mechanismen voor blok aanvallen en schadelijk verkeer terwijl verdere legitieme aanvragen in het netwerk. Dit verkeer stuurt rechtstreeks naar de resources in het perimeternetwerk. Resource kan vervolgens "praten" bronnen dieper in het netwerk, door de volgende grens voor validatie van de eerste. De buitenste laag wordt het perimeternetwerk genoemd, omdat dit deel van het netwerk wordt blootgesteld aan Internet, meestal met een vorm van beveiliging aan beide zijden. De volgende afbeelding toont een voorbeeld van een perimeternetwerk één subnet in een bedrijfsnetwerk met twee beveiligingsgrenzen.

![3]

Er zijn vele architecturen die worden gebruikt voor het implementeren van een perimeternetwerk. Deze architecturen kunnen variëren van een eenvoudige load balancer met een perimeternetwerk met meerdere subnetten met verschillende mechanismen op elke grens aan verkeer blokkeren en Beveilig de diepere lagen van het bedrijfsnetwerk. Hoe het perimeternetwerk wordt gemaakt, is afhankelijk van de specifieke vereisten van de organisatie en de algehele risicotolerantie.

Terwijl klanten hun werkbelasting naar openbare clouds, is het essentieel dat u vergelijkbare functies ondersteunen voor perimeter netwerkarchitectuur in Azure om te voldoen aan het nalevings- en beveiligingsvereisten. Dit document bevat richtlijnen over hoe klanten een beveiligde netwerkomgeving in Azure kunnen samenstellen. Het is gericht op het perimeternetwerk, maar bevat ook een uitgebreide bespreking van veel aspecten van netwerkbeveiliging. Deze discussie in kennis van de volgende vragen:

* Hoe kunt u een perimeternetwerk in Azure gemaakt?
* Wat zijn enkele van de Azure-functies beschikbaar zijn voor het perimeternetwerk bouwen?
* Hoe kunnen back-end-werkbelastingen beveiligd?
* Hoe worden internetcommunicatie bepaald aan de werkbelastingen in Azure?
* Hoe de on-premises netwerken worden beschermd tegen implementaties in Azure?
* Wanneer moeten systeemeigen Azure beveiligingsfuncties worden gebruikt in plaats van de apparaten van derden of services?

Het volgende diagram toont de verschillende beveiligingslagen aan te brengen die Azure klanten biedt. Deze lagen zijn zowel systeemeigen in de Azure-platform zelf en de klant gedefinieerde functies:

![4]

Binnenkomend verkeer vanaf Internet, Azure DDoS tegen grootschalige aanvallen op Azure beschermt. De volgende beveiligingslaag is door de klant gedefinieerde openbare IP-adressen (eindpunten) die worden gebruikt om te bepalen welk verkeer via de cloudservice kunt doorgeven aan het virtuele netwerk. Systeemeigen Azure virtuele netwerkisolatie garandeert volledige isolatie van alle andere netwerken en dat alleen via paden van de gebruiker die is geconfigureerd en methoden verkeersstromen. Deze paden en methoden zijn de volgende laag, waar de nsg's, UDR en virtuele netwerkapparaten kunnen worden gebruikt beveiligingsgrenzen ter bescherming van de implementaties van toepassingen in het beveiligde netwerk maken.

De volgende sectie biedt een overzicht van virtuele netwerken in Azure. Deze virtuele netwerken worden gemaakt door klanten en wat hun geïmplementeerde werkbelastingen zijn verbonden met zijn. Virtuele netwerken zijn het uitgangspunt voor alle netwerkbeveiligingsfuncties vereist tot stand brengen van een perimeternetwerk ter bescherming van implementaties van klanten in Azure.

## <a name="overview-of-azure-virtual-networks"></a>Overzicht van virtuele netwerken in Azure
Voordat het internetverkeer toegang krijgen tot de virtuele netwerken in Azure, zijn er twee lagen beveiliging inherent aan het Azure-platform:

1.    **DDoS-bescherming**: DDoS-bescherming wordt een laag van het Azure fysieke netwerk dat het Azure-platform zelf tegen aanvallen van grootschalige op het Internet beschermt. Deze aanvallen maken gebruik van meerdere 'bot' knooppunten in een poging een internetservice overbelast. Azure heeft een robuuste DDoS-bescherming net voor alle binnenkomende, uitgaande en meerdere Azure-regio connectiviteit. Deze laag DDoS-beveiliging heeft geen gebruiker configureerbare kenmerken en is niet toegankelijk voor de klant. De laag DDoS-beveiliging beveiligt Azure als een platform van grootschalige aanvallen, ook wordt bewaakt uitgaande gebonden verkeer en meerdere Azure-regio. Met virtuele netwerkapparaten in het VNet, worden extra verificatielagen herstelmogelijkheden geconfigureerd door de klant voor een kleinere schaal aanval die de beveiliging op netwerkniveau platform niet krachtvoertuigen. Een voorbeeld van een DDoS in actie; Als een internetgericht IP-adres van een grootschalige DDoS-aanval is aangevallen, zou Azure de bronnen van deze aanvallen te detecteren en de strijdige verkeer verwijderen voordat deze de bestemming bereikt. In bijna alle gevallen wordt niet het aangevallen eindpunt beïnvloed door de aanval. In het zeldzame gevallen heeft invloed op een eindpunt, wordt geen verkeer naar de andere eindpunten, alleen het aangevallen eindpunt beïnvloed. Andere klanten en -services ziet geen gevolgen van deze aanval. Het is essentieel om te weten dat Azure DDoS alleen grootschalige aanvallen zoekt. Het is mogelijk dat uw specifieke service overbelast kan voordat u de beveiliging op netwerkniveau platform drempelwaarden worden overschreden. Bijvoorbeeld: een website op een enkele A0 IIS-server kan offline worden gehaald van een DDoS-aanval voordat Azure-platform niveau DDoS-bescherming een bedreiging geregistreerd.

2.  **Openbare IP-adressen**: openbare IP-adres (ingeschakeld via de service-eindpunten, openbare IP-adressen, Application Gateway en andere Azure-functies die een openbaar IP-adres met het internet doorgestuurd naar de resource) adressen worden gebruikt om cloudservices of bron groepen aanpassen aan het openbare Internet IP-adressen en poorten open zijn. Het eindpunt maakt gebruik van NAT (Network Address Translation) voor het routeren van verkeer naar de interne adres en poort op de virtuele Azure-netwerk. Dit pad is de belangrijkste manier voor het externe verkeer door te geven in het virtuele netwerk. Het openbare IP-adressen kunnen worden geconfigureerd om te bepalen welk verkeer is doorgegeven en hoe en waar deze wordt omgezet naar het virtuele netwerk.

Zodra verkeer het virtuele netwerk bereikt, zijn er veel functies die een rol spelen. Virtuele netwerken van Azure vormen de basis voor klanten om hun werkbelasting en waarin de basisprincipes van beveiliging op netwerkniveau van toepassing te koppelen. Een particulier netwerk (een virtueel netwerk overlay) is in Azure het voor klanten met de volgende functies en kenmerken:

* **Isolatie van verkeer**: een virtueel netwerk is de grens van de isolatie van verkeer op de Azure-platform. Virtuele machines (VM's) in één virtueel netwerk kan niet communiceren rechtstreeks met virtuele machines in een ander virtueel netwerk, zelfs als beide virtuele netwerken worden gemaakt van dezelfde klant. Isolatie is een kritieke eigenschap die ervoor zorgt dat de klant VM's en communicatie blijft persoonlijke binnen een virtueel netwerk.

>[!NOTE]
>Isolatie van verkeer verwijst alleen naar verkeer *inkomende* aan het virtuele netwerk. Standaard uitgaand verkeer van het VNet met het internet is toegestaan, maar kan worden voorkomen door het nsg's desgewenst.
>
>

* **Topologie met meerdere niveaus**: virtuele netwerken kunnen klanten topologie met meerdere niveaus door het toewijzen van subnetten en het toewijzen van afzonderlijke adresruimten voor verschillende elementen of 'laag' van hun werkbelasting definiëren. Deze logische groeperingen en topologieën kunnen klanten verschillende toegangsbeleid op basis van de typen werkbelasting definiëren en ook bepalen verkeersstromen tussen de lagen.
* **Cross-premises connectiviteit**: klanten kunnen cross-premises-connectiviteit tussen een virtueel netwerk en meerdere on-premises sites of andere virtuele netwerken maken in Azure. Kan geen verbinding, kunnen klanten VNet-Peering, Azure VPN-Gateways, virtuele apparaten van derden of ExpressRoute gebruiken. Azure biedt ondersteuning voor site-naar-site (S2S) VPN-verbindingen met IPsec/IKE standaardprotocollen en ExpressRoute particuliere connectiviteit.
* **NSG** kunnen klanten regels (ACL's) maken op het gewenste niveau van granulatie: network interfaces, afzonderlijke virtuele machines of virtuele subnetten. Klanten kunnen toegang door toestaan of weigeren van communicatie tussen de werkbelastingen binnen een virtueel netwerk, van systemen op klantnetwerken via cross-premises-connectiviteit te controleren of directe communicatie met Internet.
* **UDR** en **doorsturen via IP** kunnen klanten voor het definiëren van de communicatiepaden voor de tussen de verschillende lagen binnen een virtueel netwerk. Klanten kunnen een firewall, -id's / IP-Adressen en andere virtuele apparaten implementeren en netwerkverkeer te routeren via deze beveiligingsapparaten voor beveiliging grens wordt afgedwongen, controle en controle.
* **Virtuele apparaten** in Azure Marketplace: beveiligingsapparaten, zoals firewalls, netwerktaakverdelers en id's / IP-Adressen beschikbaar zijn in Azure Marketplace en de VM-installatiekopie-galerie. Klanten kunnen deze apparaten naar hun virtuele netwerken en in het bijzonder op hun beveiligingsgrenzen (inclusief de perimeter netwerksubnetten) implementeren voor het voltooien van een meerdere lagen beveiligde netwerkomgeving.

Met deze functies en mogelijkheden is een voorbeeld van hoe een perimeter-netwerkarchitectuur kan worden samengesteld in Azure het volgende diagram:

![5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Perimeter netwerkkenmerken en vereisten
Het perimeternetwerk is de front-end van het netwerk, rechtstreeks in aanraking komt communicatie via Internet. De binnenkomende pakketten moeten door de beveiligingsapparaten, zoals de firewall, -id's en IP-Adressen, stromen alvorens de back-endservers. Internet-gebonden pakketten van de werkbelastingen die kunnen ook door de beveiligingsapparaten in het perimeternetwerk is bedoeld voor het afdwingen van beleid, controle en controledoeleinden, voordat deze uit het netwerk stromen. Het perimeternetwerk kan bovendien cross-premises VPN-gateways tussen virtuele netwerken van klanten en on-premises netwerken te hosten.

### <a name="perimeter-network-characteristics"></a>Perimeter-netwerkeigenschappen
Verwijst naar de vorige afbeelding, zijn een deel van de kenmerken van een goede perimeternetwerk als volgt:

* Internetgerichte:
  * Het perimeternetwerk netwerksubnet zelf is internetgerichte, rechtstreeks communiceren met Internet.
  * Openbare IP-adressen, VIP's en/of service-eindpunten die niet aan de front-end-netwerk en apparaten internetverkeer doorgeven.
  * Binnenkomend verkeer van het Internet passeert beveiligingsapparaten voordat andere bronnen op de front-endnetwerk.
  * Als u uitgaande beveiliging is ingeschakeld, passeert verkeer beveiligingsapparaten, als de laatste stap voordat het wordt doorgegeven met het Internet.
* Beveiligd netwerk:
  * Er is geen directe pad van het Internet naar de basisinfrastructuur.
  * De basisinfrastructuur kanalen moeten passeren via beveiligingsapparaten, zoals het nsg's, firewalls of VPN-apparaten.
  * Andere apparaten moeten niet via Internet en de basisinfrastructuur overbruggen.
  * Van beveiligingsapparaten op Internet gerichte en het beveiligde netwerk geconfronteerd met grenzen van het perimeternetwerk (bijvoorbeeld de twee firewall pictogrammen weergegeven in de vorige afbeelding) is een enkel virtueel apparaat met gedifferentieerde regels of interfaces daadwerkelijk mogelijk voor elke grens. Bijvoorbeeld één fysieke apparaat, logisch van elkaar gescheiden, voor het verwerken van de belasting voor beide grenzen van het perimeternetwerk.
* Andere algemene procedures en beperkingen:
  * Werkbelastingen moeten essentiële bedrijfsgegevens niet opslaan.
  * Toegang en updates voor perimeter netwerkconfiguraties en implementaties zijn beperkt tot alleen gemachtigde beheerders.

### <a name="perimeter-network-requirements"></a>Perimeter netwerkvereisten
Volg deze richtlijnen op virtueel netwerkvereisten voor het implementeren van een geslaagde perimeternetwerk zodat deze kenmerken:

* **Subnet-architectuur:** Geef het virtuele netwerk zodanig dat is een hele subnet als het perimeternetwerk toegewezen wordt gescheiden van andere subnetten in hetzelfde virtuele netwerk. Deze scheiding zorgt ervoor dat het verkeer tussen het perimeternetwerk en andere interne of particuliere subnet lagen stromen via een firewall of een virtueel apparaat-id's / IP-Adressen.  Gebruiker gedefinieerde routes op de grens subnetten zijn vereist voor het doorsturen van verkeer op het virtuele apparaat.
* **NSG:** het perimeternetwerk netwerksubnet zelf moet open zijn voor de communicatie met Internet, maar dat betekent niet dat klanten moeten de omzeilen nsg's. Volg algemene beveiligingsprocedures voor het netwerk oppervlak blootgesteld aan Internet beperken. Vergrendelen op de externe adresbereiken toegang mogen krijgen tot de implementaties of de specifieke toepassingsprotocollen en poorten die geopend zijn. Mogelijk zijn er echter omstandigheden waarin een volledige Lockdown niet mogelijk is. Bijvoorbeeld, als klanten een externe website in Azure, het perimeternetwerk moet de binnenkomende webaanvragen van openbare IP-adressen toestaan, maar moet alleen de web application poorten openen: TCP op poort 80 en/of TCP op poort 443.
* **Routeringstabel:** het perimeternetwerk netwerksubnet zelf moet rechtstreeks communiceren met het Internet, maar mag niet worden directe communicatie van en naar de back-end- of on-premises netwerken zonder gebruik te maken via een firewall of beveiliging apparaat.
* **Beveiligingsconfiguratie toestel:** doorgestuurd en inspecteren van pakketten tussen het perimeternetwerk en de rest van de beveiligde netwerken, de beveiligingsapparaten, zoals firewall, id's en IP-Adressen apparaten mogelijk multihomed. Ze hebben wellicht gescheiden NIC's voor het perimeternetwerk en de back-end-subnetten. De NIC's in het perimeternetwerk communiceren rechtstreeks naar en van het Internet, met de bijbehorende nsg's en de netwerk-routeringstabel perimeternetwerk. De NIC's die verbinding maken met de back-end-subnetten hebben meer beperkte nsg's en routeringstabellen van de bijbehorende back-end-subnetten.
* **Functie voor beveiliging toestel:** de beveiligingsapparaten meestal geïmplementeerd in het perimeternetwerk voeren de volgende functionaliteit:
  * Firewall: Afdwingen firewallregels of beleid voor toegangsbeheer voor binnenkomende aanvragen.
  * Detectie en preventie van dreiging: opsporen en schadelijke aanvallen via Internet te beperken.
  * Controle en logboekregistratie: gedetailleerde logboeken voor controle en analyse onderhouden.
  * Omgekeerde proxy: de inkomende aanvragen omleiden naar de bijbehorende back-endservers. Deze omleiding omvat het toewijzing en meestal vertalen van de doel-adressen op de front-apparaten, firewalls, naar de back-endserver-adressen.
  * Proxy doorsturen: bieden NAT en het uitvoeren van controle-instellingen voor communicatie vanaf binnen het virtuele netwerk gestart met het Internet.
  * Router: Doorsturen van binnenkomende en meerdere subnetten verkeer binnen het virtuele netwerk.
  * VPN-apparaat: fungeert als de cross-premises VPN-gateways voor cross-premises VPN-connectiviteit tussen klant on-premises netwerken en virtuele netwerken in Azure.
  * VPN-server: voor het accepteren van VPN-clients verbinding maken met virtuele netwerken in Azure.

> [!TIP]
> Gescheiden houden van de volgende twee groepen: de personen die toegang hebben tot de perimeter network security versnelling en de personen die gemachtigd als beheerders voor ontwikkeling, -implementatie of -bewerkingen. Deze groepen en gescheiden te houden voor een scheiding van taken kunt voorkomen dat één persoon zowel beveiliging van toepassingen en besturingselementen voor de beveiliging.
>
>

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Vragen om te worden gevraagd tijdens het bouwen van de grenzen van netwerken
In deze sectie tenzij specifiek vermeld, de term "netwerken" verwijst naar persoonlijke virtuele netwerken in Azure gemaakt door de abonnementsbeheerder van een. De term verwijst niet naar de onderliggende fysieke netwerken in Azure.

Virtuele netwerken in Azure worden ook vaak gebruikt voor het uitbreiden van traditionele on-premises netwerken. Het is mogelijk site-naar-site of ExpressRoute hybride netwerkoplossingen met perimeter netwerkarchitectuur opnemen. Deze hybride-koppeling is een belangrijk aandachtspunt bij het bouwen van beveiligingsgrenzen netwerk.

De volgende drie vragen zijn te beantwoorden als u bij het bouwen van een netwerk met een perimeternetwerk en meerdere beveiligingsgrenzen kritieke.

#### <a name="1-how-many-boundaries-are-needed"></a>1) hoeveel grenzen nodig zijn?
De eerste beslissingspunt is om te bepalen hoeveel beveiligingsgrenzen nodig zijn in een bepaald scenario:

* De grens van een enkele: één op de front-perimeternetwerk, tussen het virtuele netwerk en Internet.
* Twee grenzen: één met de Internet-zijde van het perimeternetwerk, en een andere tussen het perimeternetwerk netwerksubnet en de back-end-subnetten in de virtuele netwerken in Azure.
* Drie grenzen: op de Internet-zijde van het perimeternetwerk, één tussen het perimeternetwerk en de back-end-subnetten en één tussen de back-end-subnetten en de on-premises netwerk.
* N grenzen: het nummer van een variabele. Er is geen limiet aan het aantal beveiligingsgrenzen die in een bepaalde netwerk kunnen worden toegepast, afhankelijk van de beveiligingsvereisten.

Het aantal en type van grenzen hangt nodig gebaseerd op een bedrijf risicotolerantie en het specifieke scenario wordt geïmplementeerd. Deze beslissing wordt vaak samen met meerdere groepen binnen een organisatie, inclusief vaak een team risico en naleving, een netwerk en platform-team en het ontwikkelteam van een toepassing uitgevoerd. Mensen met kennis van beveiliging, de betrokken gegevens en de technologieën die wordt gebruikt, moet een zeg hebben in dit besluit om te controleren of de juiste beveiligingsrechten houding ten voor elke implementatie.

> [!TIP]
> Gebruik het kleinste aantal grenzen die voldoen aan de beveiligingsvereisten voor een bepaalde situatie. Met meer grenzen bewerkingen en het oplossen van problemen kunnen lastig zijn meer, evenals de overhead voor het beheren van meerdere beleidsregels voor de grens gedurende een bepaalde periode. Onvoldoende grenzen worden echter risico verhogen. Zoeken naar het saldo is essentieel.
>
>

![6]

De voorgaande afbeelding toont een globaal overzicht van een netwerk met drie beveiliging grens. De grenzen zijn tussen het perimeternetwerk en Internet, de Azure front-end en back-end particuliere subnetten, en het Azure back-end-subnet en de on-premises zakelijke netwerk.

#### <a name="2-where-are-the-boundaries-located"></a>2) waar zich de grenzen bevinden?
Als het aantal grenzen zijn besloten, is waar u ze implementeren de volgende beslissingspunt. Er zijn doorgaans drie opties:

* Met behulp van een Internet-gebaseerd tussenliggende-service (bijvoorbeeld een cloud-gebaseerde Web application firewall, dit niet in dit document beschreven wordt)
* Gebruik systeemeigen functies en/of virtuele netwerkapparaten in Azure
* Met behulp van fysieke apparaten op de on-premises netwerk

De opties zijn uitsluitend Azure netwerken systeemeigen Azure-functies (bijvoorbeeld Azure Load Balancers) of virtuele netwerkapparatuur van het ecosysteem van de uitgebreide partner van Azure (bijvoorbeeld firewalls Check Point).

Als een grens is vereist tussen Azure en een on-premises netwerk, kunnen de beveiligingsapparaten zich aan beide zijden van de verbinding (of beide zijden). Een beslissing moet dus worden aangebracht op de locatie beveiliging tandwielpictogram plaatsen.

In de vorige afbeelding de Internet-perimeternetwerk en de front-to-back-end-grenzen volledig zijn opgenomen in Azure, en moeten systeemeigen Azure-functies of netwerk virtuele apparaten. Beveiligingsapparaten op de grens tussen Azure (back-end subnet) en het bedrijfsnetwerk kunnen zijn op de Azure-zijde of de lokale kant of zelfs een combinatie van apparaten aan beide zijden. Kan er aanzienlijke voordelen en nadelen van beide opties die ernstig moeten worden beschouwd.

Bijvoorbeeld, heeft met behulp van bestaande fysieke beveiliging versnelling op het lokale netwerk aan clientzijde het voordeel dat er geen nieuwe tandwielpictogram is vereist. Deze moet herconfiguratie. Het nadeel is echter dat alle verkeer van Azure naar de on-premises netwerk terugkeren moet kunnen worden bekeken door de versnelling beveiliging. Dus Azure naar Azure verkeer aanzienlijke latentie veroorzaken en invloed op toepassingsprestaties en gebruikerservaring, als deze is gedwongen terug naar de on-premises netwerk voor de beveiliging wordt afgedwongen.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) hoe worden de grenzen geïmplementeerd?
Elke beveiligingsgrens wordt waarschijnlijk hebben verschillende vereisten (bijvoorbeeld-id's en firewallregels voor de Internet-zijde van het perimeternetwerk, maar alleen ACL's tussen het perimeternetwerk en het subnet van de back-end). Bepalen waarop apparaat (of het aantal apparaten) gebruiken afhankelijk van de vereisten voor het scenario en beveiliging. In de volgende sectie bespreken voorbeelden 1, 2 en 3 enkele mogelijkheden die kunnen worden gebruikt. De functies systeemeigen Azure-netwerk en de apparaten die beschikbaar zijn in Azure uit het ecosysteem van de partner te controleren, ziet u de allerlei opties die beschikbaar zijn voor het oplossen van vrijwel elk scenario.

Een ander beslissingspunt voor de implementatie van de sleutel is hoe u de on-premises netwerk verbindt met Azure. Moet u de virtuele Azure-gateway of een virtueel netwerkapparaat gebruiken? Deze opties worden behandeld in meer detail in de volgende sectie (voorbeelden 4, 5 en 6).

Bovendien het verkeer tussen virtuele netwerken in Azure nodig. Deze scenario's worden in de toekomst toegevoegd.

Zodra u de antwoorden op de vorige vragen, weet de [snel starten](#fast-start) sectie kunt achterhalen welke voorbeelden zijn het meest geschikt is voor een bepaald scenario.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Voorbeelden: Beveiligingsgrenzen met virtuele netwerken in Azure bouwen
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Voorbeeld 1 bouwen in een perimeternetwerk te beschermen toepassingen met nsg 's
[Terug naar de snel starten](#fast-start) | [gedetailleerde instructies voor dit voorbeeld bouwen][Example1]

[![7]][7]

#### <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement met de volgende resources:

- Één resourcegroep
- Een virtueel netwerk met twee subnetten: 'FrontEnd' en back-end'
- Een Netwerkbeveiligingsgroep dat wordt toegepast op beide subnetten
- Een Windows-server waarmee een toepassing webserver ('IIS01')
- Twee Windows-servers die vertegenwoordigen toepassingsservers van back-end ('AppVM01', 'AppVM02')
- Een Windows-server met een DNS-server ('DNS01')
- Een openbaar IP-adres die zijn gekoppeld aan de webserver van de toepassing

Zie voor scripts en een Azure Resource Manager-sjabloon de [gedetailleerde instructies voor de build][Example1].

#### <a name="nsg-description"></a>NSG-beschrijving
In dit voorbeeld is een groep NSG gebouwd en vervolgens met zes regels worden geladen.

> [!TIP]
> Over het algemeen moet u uw specifieke regels voor 'Toestaan' eerst, gevolgd door de algemene regels voor "Deny". De opgegeven prioriteit bepaalt welke regels eerst worden geëvalueerd. Als verkeer toepassen op een specifieke regel wordt gevonden, wordt er geen verdere regels worden geëvalueerd. NSG-regels kunnen toepassen in de binnenkomende of uitgaande richting (vanuit het perspectief van het subnet).
>
>

De volgende regels zijn declaratief, gebouwd voor binnenkomend verkeer:

1. Interne DNS-verkeer (poort 53) is toegestaan.
2. RDP-verkeer (poort 3389) van het Internet aan een virtuele Machine is toegestaan.
3. HTTP-verkeer (poort 80) van het Internet met webserver (IIS01) is toegestaan.
4. Alle verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan.
5. Al het verkeer (alle poorten) van het Internet naar het volledige virtuele netwerk (beide subnetten) is geweigerd.
6. Verkeer (alle poorten) van de front-end-subnet met het subnet voor back-end is geweigerd.

Met deze regels gekoppeld aan elk subnet als een HTTP-aanvraag inkomende verkeer van Internet naar de webserver, beide regels 3 was (toestaan) en 5 (weigeren) toepassing zou zijn. Maar omdat regel 3 een hogere prioriteit heeft, alleen zou worden toegepast en regel 5 niet in de play zou komen. De HTTP-aanvraag zou dus naar de webserver worden toegestaan. Als dat dezelfde verkeer is probeert de server DNS01 bereiken, regel 5 (weigeren) zijn de eerste om toe te passen en het verkeer niet mogen worden doorgegeven aan de server. Regel 6 (weigeren) blokkeert het front-end-subnet van het praten met het subnet van de back-end (met uitzondering van toegestane verkeer in regels 1 en 4). Deze regelset beschermt de back-endnetwerk als een aanvaller de webtoepassing op de front-end wordt aangetast. De aanvaller zou beperkt toegang tot het 'beveiligde' back-end-netwerk (alleen voor resources weergegeven op de server AppVM01).

Er is een uitgaande standaardregel waarmee uitgaand verkeer naar Internet. Voor dit voorbeeld bent we uitgaand verkeer toestaat en alle regels voor uitgaande verbindingen niet wijzigen. Als u wilt vergrendelen verkeer in beide richtingen, gebruiker gedefinieerde routering is vereist (Zie voorbeeld 3).

#### <a name="conclusion"></a>Conclusie
In dit voorbeeld is een relatief eenvoudig en snel manier van het subnet van de back-end van binnenkomend verkeer isoleren. Zie voor meer informatie de [gedetailleerde instructies voor de build][Example1]. Deze instructies zijn onder andere:

* Het bouwen van deze perimeternetwerk met klassieke PowerShell-scripts.
* Het bouwen van deze perimeternetwerk met een Azure Resource Manager-sjabloon.
* Gedetailleerde beschrijvingen van elke NSG-opdracht.
* Gedetailleerde verkeer stroom scenario's, die laat zien hoe het verkeer wordt toegestaan of geweigerd in elke laag.


### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Voorbeeld 2 een perimeternetwerk te beschermen toepassingen met een firewall en nsg's maken
[Terug naar de snel starten](#fast-start) | [gedetailleerde instructies voor dit voorbeeld bouwen][Example2]

[![8]][8]

#### <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement met de volgende resources:

* Één resourcegroep
* Een virtueel netwerk met twee subnetten: 'FrontEnd' en back-end'
* Een Netwerkbeveiligingsgroep dat wordt toegepast op beide subnetten
* Een virtueel netwerkapparaat, in dit geval een firewall, verbonden met het front-end-subnet
* Een Windows-server waarmee een toepassing webserver ('IIS01')
* Twee Windows-servers die vertegenwoordigen toepassingsservers van back-end ('AppVM01', 'AppVM02')
* Een Windows-server met een DNS-server ('DNS01')

Zie voor scripts en een Azure Resource Manager-sjabloon de [gedetailleerde instructies voor de build][Example2].

#### <a name="nsg-description"></a>NSG-beschrijving
In dit voorbeeld is een groep NSG gebouwd en vervolgens met zes regels worden geladen.

> [!TIP]
> Over het algemeen moet u uw specifieke regels voor 'Toestaan' eerst, gevolgd door de algemene regels voor "Deny". De opgegeven prioriteit bepaalt welke regels eerst worden geëvalueerd. Als verkeer toepassen op een specifieke regel wordt gevonden, wordt er geen verdere regels worden geëvalueerd. NSG-regels kunnen toepassen in de binnenkomende of uitgaande richting (vanuit het perspectief van het subnet).
>
>

De volgende regels zijn declaratief, gebouwd voor binnenkomend verkeer:

1. Interne DNS-verkeer (poort 53) is toegestaan.
2. RDP-verkeer (poort 3389) van het Internet aan een virtuele Machine is toegestaan.
3. Alle internetverkeer (alle poorten) op het virtuele netwerk-apparaat (firewall) is toegestaan.
4. Alle verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan.
5. Al het verkeer (alle poorten) van het Internet naar het volledige virtuele netwerk (beide subnetten) is geweigerd.
6. Verkeer (alle poorten) van de front-end-subnet met het subnet voor back-end is geweigerd.

Met deze regels gekoppeld aan elk subnet als een HTTP-aanvraag inkomende verkeer van Internet aan de firewall, beide regels 3 was (toestaan) en 5 (weigeren) toepassing zou zijn. Maar omdat regel 3 een hogere prioriteit heeft, alleen zou worden toegepast en regel 5 niet in de play zou komen. De HTTP-aanvraag zou dus mag worden de firewall. Als dat dezelfde verkeer is probeert te bereiken van de server IIS01, zelfs als het op de front-end-subnet, regel 5 (weigeren) toepassing zou zijn, en het verkeer zou niet moeten worden doorgegeven aan de server is toegestaan. Regel 6 (weigeren) blokkeert het front-end-subnet van het praten met het subnet van de back-end (met uitzondering van toegestane verkeer in regels 1 en 4). Deze regelset beschermt de back-endnetwerk als een aanvaller de webtoepassing op de front-end wordt aangetast. De aanvaller zou beperkt toegang tot het 'beveiligde' back-end-netwerk (alleen voor resources weergegeven op de server AppVM01).

Er is een uitgaande standaardregel waarmee uitgaand verkeer naar Internet. Voor dit voorbeeld bent we uitgaand verkeer toestaat en alle regels voor uitgaande verbindingen niet wijzigen. Als u wilt vergrendelen verkeer in beide richtingen, gebruiker gedefinieerde routering is vereist (Zie voorbeeld 3).

#### <a name="firewall-rule-description"></a>De beschrijving van de firewall-regel
Op de firewall moet doorsturen regels worden gemaakt. Omdat hierbij alleen routes internetverkeer in is gebonden aan de firewall en vervolgens naar de webserver slechts één doorsturen netwerkadresomzetting (NAT) wordt regel vereist.

De regel voor doorsturen accepteert alle binnenkomende bronadressen die treffers in de firewall probeert te bereiken HTTP (poort 80 of 443 voor HTTPS). Deze heeft verzonden buiten de firewall lokale interface en omgeleid naar de webserver met het IP-adres van 10.0.1.5.

#### <a name="conclusion"></a>Conclusie
In dit voorbeeld is een relatief eenvoudige manier van het beveiligen van uw toepassing met een firewall en het subnet van de back-end van binnenkomend verkeer isoleren. Zie voor meer informatie de [gedetailleerde instructies voor de build][Example2]. Deze instructies zijn onder andere:

* Het bouwen van deze perimeternetwerk met klassieke PowerShell-scripts.
* Het bouwen van dit voorbeeld met een Azure Resource Manager-sjabloon.
* Gedetailleerde beschrijvingen van elke NSG-opdracht en firewall-regel.
* Gedetailleerde verkeer stroom scenario's, die laat zien hoe het verkeer wordt toegestaan of geweigerd in elke laag.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-and-udr-and-nsg"></a>Voorbeeld 3 bouwen in een perimeternetwerk te beschermen netwerken met een firewall en UDR en NSG
[Terug naar de snel starten](#fast-start) | [gedetailleerde instructies voor dit voorbeeld bouwen][Example3]

[![9]][9]

#### <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement met de volgende resources:

* Één resourcegroep
* Een virtueel netwerk met drie subnetten: 'SecNet', 'FrontEnd' en back-end'
* Een virtueel netwerkapparaat, in dit geval een firewall, die is verbonden met het subnet SecNet
* Een Windows-server waarmee een toepassing webserver ('IIS01')
* Twee Windows-servers die vertegenwoordigen toepassingsservers van back-end ('AppVM01', 'AppVM02')
* Een Windows-server met een DNS-server ('DNS01')

Zie voor scripts en een Azure Resource Manager-sjabloon de [gedetailleerde instructies voor de build][Example3].

#### <a name="udr-description"></a>UDR beschrijving
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

De VNETLocal is altijd een of meer gedefinieerde adresvoorvoegsels waaruit het virtuele netwerk voor het specifieke netwerk (dat wil zeggen, verandert van virtueel netwerk met virtueel netwerk, afhankelijk van hoe elk specifiek virtueel netwerk is gedefinieerd). De resterende systeemroutes zijn statisch en standaard aangegeven in de tabel.

In dit voorbeeld zijn twee routeringstabellen gemaakt, één voor de front-end en back-end-subnetten. Elke tabel is geladen met statische routes die geschikt is voor het opgegeven subnet. In dit voorbeeld wordt elke tabel heeft drie routes die alle verkeer (0.0.0.0/0) via de firewall (volgende hop = virtueel apparaat IP-adres):

1. Lokaal subnetverkeer met geen volgende Hop gedefinieerd zodat lokaal subnetverkeer voor het overslaan van de firewall.
2. Virtueel netwerkverkeer met een volgende Hop gedefinieerd als een firewall. Deze volgende hop overschrijft de standaardregel waarmee lokale virtueel netwerkverkeer te routeren rechtstreeks.
3. Alle resterende verkeer (0/0) met een volgende Hop gedefinieerd als de firewall.

> [!TIP]
> De vermelding van het lokale subnet hoeft niet in de communicatie UDR einden lokale subnet.
>
> * In ons voorbeeld is de 10.0.1.0/24 die verwijst naar VNETLocal kritieke! Zonder dit mislukken pakket verlaten van de webserver (10.0.1.4) dat is bestemd voor een andere lokale server (bijvoorbeeld) 10.0.1.25 als ze worden verzonden naar de NVA. De NVA wordt worden verzonden naar het subnet en het subnet wordt deze opnieuw verzenden naar de NVA in een oneindige lus.
> * De kans op een routeringslus zijn meestal hoger op apparaten met meerdere NIC's die zijn verbonden met verschillende subnetten, dit is vaak van traditionele, lokale apparaten.
>
>

Zodra de routeringstabellen zijn gemaakt, moeten ze worden gebonden aan hun subnetten. De front-end-subnet routeringstabel eenmaal gemaakt en gekoppeld aan het subnet eruit als deze uitvoer:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

> [!NOTE]
> UDR kan nu worden toegepast op het gatewaysubnet waarop het ExpressRoute-circuit is verbonden.
>
> Voorbeelden voor het inschakelen van uw perimeternetwerk met ExpressRoute of site-naar-site-netwerken worden weergegeven in de voorbeelden 3 en 4.
>
>

#### <a name="ip-forwarding-description"></a>Beschrijving doorsturen via IP
Doorsturen via IP is een functie companion UDR. Doorsturen via IP is een instelling op een virtueel apparaat dat is toegestaan voor het ontvangen verkeer niet specifiek zijn gericht op het toestel en vervolgens doorsturen dat verkeer naar de uiteindelijke bestemming.

Bijvoorbeeld, als AppVM01 wordt een aanvraag bij de server DNS01 indient, zou UDR dit verkeer routeren aan de firewall. Met het doorsturen via IP is ingeschakeld, wordt het verkeer voor de bestemming DNS01 (10.0.2.4) geaccepteerd door het toestel (10.0.0.4) en vervolgens doorgestuurd naar de uiteindelijke bestemming (10.0.2.4). Zonder doorsturen via IP ingeschakeld op de firewall, wordt verkeer niet geaccepteerd door het toestel ondanks dat de routetabel de firewall als de volgende hop is. Voor het gebruik van een virtueel apparaat, is het essentieel moet samen met UDR doorsturen via IP inschakelen.

#### <a name="nsg-description"></a>NSG-beschrijving
In dit voorbeeld is een groep NSG gebouwd en vervolgens geladen met een enkele regel. Deze groep wordt vervolgens uitsluitend gekoppeld aan de front-end en back-end-subnetten (niet de SecNet). Declaratief wordt samengesteld met de volgende regel:

* Al het verkeer (alle poorten) van het Internet naar het volledige virtuele netwerk (alle subnetten) is geweigerd.

Hoewel het nsg's in dit voorbeeld worden gebruikt, worden de belangrijkste doel is als een laag secundaire beveiliging tegen handmatige onjuiste configuratie. Het doel is om alle binnenkomend verkeer van Internet naar de front-end- of back-end subnetten blokkeren. Verkeersstroom moet alleen via het subnet SecNet aan de firewall (en vervolgens, indien van toepassing op de front- of back-end subnetten). Plus, aan de regels UDR aanwezig is, verkeer om het in de front-end- of back-end-subnetten zou worden omgeleid uit aan de firewall (dankzij UDR). De firewall verkeer zou zien als een asymmetrische stroom en het uitgaande verkeer wilt verwijderen. Er zijn dus drie lagen van beveiliging voor het beveiligen van de subnetten:

* Er is geen openbare IP-adressen op elke front-end of back-end-NIC's.
* Nsg's voor het weigeren van verkeer van het Internet.
* De firewall weggehaald asymmetrische verkeer.

Een interessant punt met betrekking tot het NSG in dit voorbeeld is dat deze slechts één regel, dat wil zeggen weigeren internetverkeer naar het volledige virtuele netwerk, met inbegrip van het subnet beveiliging bevat. Echter, aangezien het NSG wordt alleen gekoppeld aan de front-end en back-end-subnetten, de regel is niet verwerkt op het verkeer inkomend verkeer naar het subnet van de beveiliging. Als gevolg hiervan verkeersstromen met het subnet voor beveiliging.

#### <a name="firewall-rules"></a>Firewall-regels
Op de firewall moet doorsturen regels worden gemaakt. Omdat de firewall geblokkeerd of doorsturen van alle binnenkomende, uitgaande en binnen virtueel netwerkverkeer is, worden veel firewallregels vereist. Alle binnenkomend verkeer treffers ook de beveiligingsservice openbaar IP-adres (op verschillende poorten), moeten worden verwerkt door de firewall. Er is een best practice diagram van de logische stroom voor het instellen van de subnetten te en firewall-regels om te voorkomen dat later bijwerken. De volgende afbeelding is een logische weergave van de firewallregels voor dit voorbeeld:

![10]

> [!NOTE]
> Op basis van het virtuele netwerk-apparaat gebruikt, variëren de beheerpoorten. In dit voorbeeld wordt een Barracuda NextGen Firewall naar wordt verwezen, waarbij poorten 22, 801 en 807 wordt gebruikt. Raadpleeg de documentatie van de leverancier toestel om te zoeken welke poorten gebruikt voor het beheer van het apparaat wordt gebruikt.
>
>

#### <a name="firewall-rules-description"></a>Beschrijving van de firewall-regels
In de voorgaande logisch diagram wordt het subnet van de beveiliging niet weergegeven omdat de firewall de enige resource in dat subnet is. Het diagram worden weergegeven voor de firewall-regels en hoe ze logisch toestaan of verkeersstromen, niet de werkelijke gerouteerde pad weigeren. Ook de externe poorten voor de RDP-verkeer hoger zijn geselecteerd varieerde poorten (8014 – 8026) en los uitgelijnd met de laatste twee octetten van het lokale IP-adres voor de leesbaarheid eenvoudiger zijn geselecteerd (bijvoorbeeld 10.0.1.4-adres van de lokale server is gekoppeld externe poort 8014). Eventuele niet-conflicterende hoger poorten, kunnen echter worden gebruikt.

Bijvoorbeeld moeten we zeven typen regels:

* Externe regels (voor inkomend verkeer):
  1. Firewallregel management: deze App omleiden regel staat toe dat verkeer door te geven aan de management-poorten van het virtuele netwerk-apparaat.
  2. RDP-regels (voor elke WindowsServer): deze vier regels (één voor elke server) toestaan van beheer van de afzonderlijke servers via RDP. De vier regels voor RDP kunnen ook worden samengevoegd tot één regel, afhankelijk van de mogelijkheden van het virtuele netwerk-apparaat wordt gebruikt.
  3. Regels voor het netwerkverkeer van toepassing: Er zijn twee van deze regels, de eerste voor het verkeer van de front-end-webserver en de tweede voor het back-end-verkeer (bijvoorbeeld webserver gegevenslaag). De configuratie van deze regels is afhankelijk van de netwerkarchitectuur (waar uw servers worden geplaatst), en er verkeer stroomt (welke richting de verkeersstromen en welke poorten worden gebruikt).
     * De eerste regel kan het verkeer van de toepassing om de toepassingsserver te bereiken. Terwijl de andere regels toestaan voor beveiliging en beheer, zijn toepassing verkeersregels wat toestaan van externe gebruikers of services toegang krijgen tot de toepassingen. In dit voorbeeld moet u er één webserver is op poort 80. Een firewallregel voor één toepassing wordt dus binnenkomend verkeer omgeleid naar het externe IP-adres, het web servers interne IP-adres. De sessie omgeleide verkeer zou via NAT worden vertaald naar de interne server.
     * De tweede regel wordt de back-end-regel wilt toestaan dat de webserver Neem contact op met de server AppVM01 (maar niet AppVM02) via een willekeurige poort.
* Interne regels (voor binnen virtueel netwerkverkeer)
  1. Uitgaand naar Internet regel: met deze regel kunnen het verkeer van een netwerk te geven aan de geselecteerde netwerken. Deze regel is meestal een standaardregel al op de firewall, maar een uitgeschakelde status. Deze regel moet worden ingeschakeld voor dit voorbeeld.
  2. Een DNS-regel: met deze regel kunnen alleen DNS (poort 53)-verkeer door te geven aan de DNS-server. De meeste verkeer van de front-end naar de back-end is geblokkeerd voor deze omgeving. Deze regel kunnen specifiek DNS van een lokale subnet.
  3. Subnet naar subnet regel: met deze regel wordt als u een server op het subnet van de back-end verbinding maken met een willekeurige server in het front-end-subnet (maar niet andersom).
* Foutveilige regel (voor verkeer dat niet voldoet aan een van de vorige):
  1. Alle verkeersregel voor weigeren: deze regel voor weigeren moet altijd de laatste regel (in termen van prioriteit), en als zodanig als netwerkverkeer niet overeen met een van de voorgaande regels door deze regel wordt geannuleerd. Deze regel is een standaardregel en meestal in-place en actief. Er zijn geen wijzigingen zijn meestal nodig voor deze regel.

> [!TIP]
> Op de tweede verkeersregel van de toepassing om te vereenvoudigen in dit voorbeeld is elke poort toegestaan. In een echte scenario moeten het meest specifiek poort en -adresbereiken worden gebruikt om de kwetsbaarheid van deze regel te beperken.
>
>

Zodra de vorige regels zijn gemaakt, is het belangrijk om te controleren van de prioriteit van elke regel om te controleren of het verkeer wordt toegestaan of geweigerd desgewenst. In dit voorbeeld zijn de regels in volgorde van prioriteit.

#### <a name="conclusion"></a>Conclusie
In dit voorbeeld is een complexere maar manier van het beveiligen en isoleren van het netwerk dan de eerdere voorbeelden te voltooien. (Voorbeeld 2 beveiligt alleen de toepassing en net isoleert subnetten voorbeeld 1). Dit ontwerp kunt u verkeer in beide richtingen, bewaking en niet alleen de inkomende toepassingsserver beveiligt, maar wordt afgedwongen beveiligingsbeleid voor alle servers in dit netwerk netwerk. Bovendien, afhankelijk van het toestel gebruikt om kunnen verkeer van de volledige controle en het bewustzijn worden bereikt. Zie voor meer informatie de [gedetailleerde instructies voor de build][Example3]. Deze instructies zijn onder andere:

* Het bouwen van dit voorbeeld perimeternetwerk met klassieke PowerShell-scripts.
* Het bouwen van dit voorbeeld met een Azure Resource Manager-sjabloon.
* Gedetailleerde beschrijvingen van elke UDR, NSG opdracht en de firewallregel.
* Gedetailleerde verkeer stroom scenario's, die laat zien hoe het verkeer wordt toegestaan of geweigerd in elke laag.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn"></a>Voorbeeld 4 een hybride verbinding met een site-naar-site, een virtueel apparaat VPN toevoegen
[Terug naar de snel starten](#fast-start) | Gedetailleerde build instructies beschikbaar binnenkort

[![11]][11]

#### <a name="environment-description"></a>Beschrijving van de omgeving
Hybride netwerken met behulp van een virtueel netwerkapparaat (NVA) kan worden toegevoegd aan een van de typen perimeternetwerk dat wordt beschreven in de voorbeelden 1, 2 of 3.

Zoals u in de vorige afbeelding, wordt een VPN-verbinding via Internet (site-naar-site) verbinding maken met een on-premises netwerk een Azure-netwerk via een NVA gebruikt.

> [!NOTE]
> Als u ExpressRoute met de optie openbare Azure-Peering is ingeschakeld gebruikt, kan een statische route moet worden gemaakt. Deze statische route moet doorsturen naar het NVA VPN-IP-adres van uw zakelijke Internet en niet via de ExpressRoute-verbinding. Het NAT-apparaat vereist op de optie ExpressRoute Azure openbare Peering kunt verbreken van de VPN-sessie.
>
>

Nadat de VPN-verbinding gemaakt is, wordt de NVA de centrale hub voor netwerken en -subnetten. De firewallregels voor doorsturen bepalen welke verkeersstromen toegestaan, worden omgezet via NAT, worden omgeleid of worden verwijderd (zelfs voor verkeersstromen tussen de on-premises netwerk en Azure).

Verkeersstromen moeten zorgvuldig worden beschouwd als ze kunnen worden geoptimaliseerd of gedegradeerd door dit ontwerp gaat, afhankelijk van de specifieke gebruiksvoorbeeld.

Met behulp van de omgeving die is ingebouwd in voorbeeld 3 en vervolgens toe te voegen een site-naar-site VPN-hybride netwerkverbinding, geeft het ontwerp van de volgende:

[![12]][12]

De lokale router of andere netwerkapparaten die compatibel is met uw NVA voor VPN, zou de VPN-client zijn. Deze fysieke apparaat zou zijn verantwoordelijk voor het initiëren en onderhouden van de VPN-verbinding met uw NVA.

Logisch naar de NVA ziet het netwerk er vier afzonderlijke "beveiligingszones" aan de regels op de NVA wordt de primaire directeur van verkeer tussen deze zones:

![13]

#### <a name="conclusion"></a>Conclusie
Het toevoegen van een site-naar-site VPN-hybride netwerkverbinding met een Azure-netwerk kunt de on-premises netwerk uitbreiden naar Azure op een veilige manier. Bij het gebruik van een VPN-verbinding verkeer worden versleuteld en gerouteerd via Internet. De NVA in dit voorbeeld biedt een centrale locatie om het beheren van het beveiligingsbeleid af te dwingen. Raadpleeg de gedetailleerde build-instructies (binnenkort uitgebracht) voor meer informatie. Deze instructies zijn onder andere:

* Het bouwen van dit voorbeeld perimeternetwerk met PowerShell-scripts.
* Het bouwen van dit voorbeeld met een Azure Resource Manager-sjabloon.
* Gedetailleerde verkeer stroom scenario's, die laat zien hoe verkeer via dit ontwerp loopt.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway"></a>Voorbeeld 5 een hybride verbinding met een Azure VPN-gateway voor site-naar-site toevoegen
[Terug naar de snel starten](#fast-start) | Gedetailleerde build instructies beschikbaar binnenkort

[![14]][14]

#### <a name="environment-description"></a>Beschrijving van de omgeving
Hybride netwerken met behulp van een Azure VPN-gateway kan worden toegevoegd aan beide type perimeternetwerk dat wordt beschreven in de voorbeelden 1 of 2.

Zoals u in de voorgaande afbeelding, wordt een VPN-verbinding via Internet (site-naar-site) verbinding maken met een on-premises netwerk een Azure-netwerk via een Azure VPN-gateway gebruikt.

> [!NOTE]
> Als u ExpressRoute met de optie openbare Azure-Peering is ingeschakeld gebruikt, kan een statische route moet worden gemaakt. Deze statische route moet doorsturen naar het NVA VPN-IP-adres uit uw zakelijke Internet en niet via het WAN ExpressRoute. Het NAT-apparaat vereist op de optie ExpressRoute Azure openbare Peering kunt verbreken van de VPN-sessie.
>
>

De volgende afbeelding ziet de twee netwerk randen in dit voorbeeld. Op de eerste rand bepalen de NVA en nsg's verkeersstromen voor binnen Azure-netwerken en tussen Azure en het Internet. De tweede rand is de Azure VPN-gateway een afzonderlijke en geïsoleerd netwerk-rand tussen on-premises en Azure is.

Verkeersstromen moeten zorgvuldig worden beschouwd als ze kunnen worden geoptimaliseerd of gedegradeerd door dit ontwerp gaat, afhankelijk van de specifieke gebruiksvoorbeeld.

Met behulp van de omgeving die is ingebouwd in voorbeeld 1 en vervolgens toe te voegen een site-naar-site VPN-hybride netwerkverbinding, geeft het ontwerp van de volgende:

[![15]][15]

#### <a name="conclusion"></a>Conclusie
Het toevoegen van een site-naar-site VPN-hybride netwerkverbinding met een Azure-netwerk kunt de on-premises netwerk uitbreiden naar Azure op een veilige manier. Met behulp van de systeemeigen Azure VPN-gateway het verkeer is IPSec-versleuteld en gerouteerd via Internet. Met behulp van de Azure VPN-gateway kunt ook een optie voor lagere kosten (Er is geen aanvullende licenties kosten net als bij de derde partij NVAs) opgeven. Deze optie is meest voordelige in voorbeeld 1, waarbij geen NVA wordt gebruikt. Raadpleeg de gedetailleerde build-instructies (binnenkort uitgebracht) voor meer informatie. Deze instructies zijn onder andere:

* Het bouwen van dit voorbeeld perimeternetwerk met PowerShell-scripts.
* Het bouwen van dit voorbeeld met een Azure Resource Manager-sjabloon.
* Gedetailleerde verkeer stroom scenario's, die laat zien hoe verkeer via dit ontwerp loopt.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Voorbeeld 6 een hybride verbinding toevoegen met ExpressRoute
[Terug naar de snel starten](#fast-start) | Gedetailleerde build instructies beschikbaar binnenkort

[![16]][16]

#### <a name="environment-description"></a>Beschrijving van de omgeving
Hybride netwerken met behulp van een ExpressRoute-persoonlijke peering verbinding kan worden toegevoegd aan een type perimeternetwerk dat wordt beschreven in de voorbeelden 1 of 2.

Zoals u in de voorgaande afbeelding, biedt privépeering ExpressRoute een rechtstreekse verbinding tussen uw on-premises netwerk en de virtuele Azure-netwerk. Verkeer transits alleen de serviceprovider-netwerk en het Microsoft Azure-netwerk, nooit door Internet aan te raken.

> [!TIP]
> Met behulp van ExpressRoute, houdt u zakelijke netwerkverkeer uit het Internet. Daarnaast kunt u serviceovereenkomsten van uw ExpressRoute-provider. De Azure-Gateway kunt maximaal 10 Gbps met ExpressRoute kunt doorgeven dat de maximale doorvoer van Azure-Gateway met site-naar-site-VPN's, 200 Mbps.
>
>

Zoals in het volgende diagram kunt zien, met deze optie heeft de omgeving nu twee randen van het netwerk. De NVA en NSG beheren verkeersstromen voor binnen Azure-netwerken en tussen Azure en het Internet, terwijl de gateway een afzonderlijke en geïsoleerd netwerk-rand tussen on-premises en Azure is.

Verkeersstromen moeten zorgvuldig worden beschouwd als ze kunnen worden geoptimaliseerd of gedegradeerd door dit ontwerp gaat, afhankelijk van de specifieke gebruiksvoorbeeld.

Met behulp van de omgeving die is ingebouwd in voorbeeld 1 en vervolgens toe te voegen een ExpressRoute-verbinding voor hybride netwerk, geeft het ontwerp van de volgende:

[![17]][17]

#### <a name="conclusion"></a>Conclusie
Het toevoegen van een netwerkverbinding ExpressRoute persoonlijke Peering kunt de on-premises netwerk uitbreiden naar Azure in een beveiligde, lagere latentie, manier hoger uitvoeren. Met behulp van de systeemeigen Azure-Gateway, zoals in dit voorbeeld biedt ook een optie goedkoper (Er is geen nadere net als bij de derde partij NVAs-licentieverlening). Raadpleeg de gedetailleerde build-instructies (binnenkort uitgebracht) voor meer informatie. Deze instructies zijn onder andere:

* Het bouwen van dit voorbeeld perimeternetwerk met PowerShell-scripts.
* Het bouwen van dit voorbeeld met een Azure Resource Manager-sjabloon.
* Gedetailleerde verkeer stroom scenario's, die laat zien hoe verkeer via dit ontwerp loopt.

## <a name="references"></a>Verwijzingen
### <a name="helpful-websites-and-documentation"></a>Documentatie en nuttige websites
* Toegang van Azure met Azure Resource Manager:
* Toegang tot Azure met PowerShell: [https://docs.microsoft.com/powershell/azureps-cmdlets-docs/](/powershell/azure/overview)
* Informatie over virtuele netwerken: [https://docs.microsoft.com/azure/virtual-network/](https://docs.microsoft.com/azure/virtual-network/)
* Network security groep documentatie: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg](virtual-network/virtual-networks-nsg.md)
* Gebruiker gedefinieerde routering documentatie: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview](virtual-network/virtual-networks-udr-overview.md)
* Azure virtuele gateways: [https://docs.microsoft.com/azure/vpn-gateway/](https://docs.microsoft.com/azure/vpn-gateway/)
* Site-naar-Site VPN-verbindingen: [https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell](vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* ExpressRoute-documentatie (Zorg ervoor dat de secties 'Aan de slag' en "How To" uitchecken): [https://docs.microsoft.com/azure/expressroute/](https://docs.microsoft.com/azure/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Stroomdiagram beveiligingsopties"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "azure beveiligingsfuncties"
[3]: ./media/best-practices-network-security/dmzcorporate.png "DMZ A in een bedrijfsnetwerk"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "architectuur van de azure-beveiliging"
[5]: ./media/best-practices-network-security/dmzazure.png "een DMZ in een Azure-netwerk"
[6]: ./media/best-practices-network-security/dmzhybrid.png "hybride netwerk met drie beveiligingsgrenzen"
[7]: ./media/best-practices-network-security/example1design.png "Inkomende DMZ met NSG"
[8]: ./media/best-practices-network-security/example2design.png "Inkomende DMZ met NVA en NSG"
[9]: ./media/best-practices-network-security/example3design.png "Bidirectionele DMZ met NVA, NSG en UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "logische weergave van de Firewall-regels"
[11]: ./media/best-practices-network-security/example3designoptions.png "Perimeternetwerk met NVA hybride netwerk verbonden"
[12]: ./media/best-practices-network-security/example4designs2s.png "Perimeternetwerk met NVA verbonden via een Site-naar-Site-VPN"
[13]: ./media/best-practices-network-security/example4networklogical.png "logisch netwerk vanuit NVA perspectief"
[14]: ./media/best-practices-network-security/example5designoptions.png "Perimeternetwerk met Azure-Gateway verbonden Site-naar-Site hybride netwerk"
[15]: ./media/best-practices-network-security/example5designs2s.png "Perimeternetwerk met Azure via Site-naar-Site VPN-Gateway"
[16]: ./media/best-practices-network-security/example6designoptions.png "Perimeternetwerk met Azure-Gateway verbonden ExpressRoute hybride netwerk"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "Perimeternetwerk met Azure-Gateway met behulp van een ExpressRoute-verbinding"

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
