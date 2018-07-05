---
title: 'Azure Virtual Datacenter: een Netwerkperspectief'
description: Informatie over het bouwen van uw virtuele datacenter in Azure
services: networking
author: tracsman
manager: rossort
tags: azure-resource-manager
ms.service: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/3/2018
ms.author: jonor
ms.openlocfilehash: 2c8ca8bcce43596d521fa9c81438ac6a16f6dcdf
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445378"
---
# <a name="azure-virtual-datacenter-a-network-perspective"></a>Azure Virtual Datacenter: Een Netwerkperspectief
**Microsoft Azure**: wees sneller, geld te besparen, on-premises toepassingen en gegevens integreren

## <a name="overview"></a>Overzicht
On-premises toepassingen naar Azure migreert, biedt zelfs als u geen belangrijke wijzigingen (een aanpak die bekend staat als 'lift and shift'), organisaties de voordelen van een beveiligde en betaalbare infrastructuur. Ondernemingen moeten echter de meeste van de flexibiliteit om mogelijk te maken met cloud computing, evolueren hun architecturen om te profiteren van de mogelijkheden van Azure. Microsoft Azure biedt grootschalige services en infrastructuur, geavanceerde mogelijkheden en de betrouwbaarheid en veel opties voor hybride verbindingen. Klanten kunnen kiezen voor toegang tot deze cloudservices via Internet of met Azure ExpressRoute, waarmee u verbinding met het particuliere netwerk. De Microsoft Azure-platform kan klanten naadloos de infrastructuur uitbreiden naar de cloud en bouw architecturen met meerdere lagen. Microsoft-partners bieden bovendien verbeterde mogelijkheden die door het aanbieden van security-services en virtuele apparaten die zijn geoptimaliseerd voor het uitvoeren in Azure.

In dit artikel bevat een overzicht van patronen en mogelijk te maken die kunnen worden gebruikt voor het oplossen van de architectuur schaal, prestaties en beveiliging betrekking heeft op veel klanten worden geconfronteerd bij het nadenken over het verplaatsen van weg naar de cloud. Een overzicht van hoe u aan verschillende organisatie-IT-rollen voor het beheer en bestuur van het systeem wordt ook beschreven, waarbij de nadruk ligt aan de beveiligingsvereisten voor en kosten van optimalisatie.

## <a name="what-is-a-virtual-data-center"></a>Wat is een virtuele Data Center?
In het begin cloudoplossingen zijn zo ontworpen dat host één, relatief geïsoleerd, toepassingen, in de openbare spectrum. Deze benadering heeft gewerkt voor een paar jaar. Echter, als de voordelen van cloud solutions is gebleken en meerdere grootschalige workloads zijn die worden gehost op de cloud, beveiliging, betrouwbaarheid, prestaties,-adressering en kosten van problemen van implementaties in een of meer regio's is gedurende de levenscyclus essentieel geworden van de cloudservice.

De volgende cloud implementatie diagram ziet u enkele voorbeelden van lacunes in de beveiliging (met een rood kader) en ruimte voor virtuele netwerkapparaten optimalisatie voor workloads (geel vak).

[![0]][0]

Het virtuele Data Center (VDC's) is voortgekomen uit deze noodzakelijk voor het schalen ter ondersteuning van grote workloads en hoeft te bekommeren om de problemen die ontstaan doordat ondersteunende grootschalige toepassingen in de openbare cloud.

Een vDC is niet alleen de werkbelastingen van toepassingen in de cloud, maar ook het netwerk, beveiliging, beheer en -infrastructuur (bijvoorbeeld DNS- en Directory Services). Gewoonlijk biedt ook een particuliere verbinding naar een on-premises netwerk of data center. Als u meer workloads naar Azure verplaatst, is het belangrijk om na te denken over de ondersteunende infrastructuur en de objecten die in deze werkbelastingen worden geplaatst. Denken zorgvuldig hoe resources worden opgebouwd kunt voorkomen dat de verspreiding van honderden "werkbelasting eilanden" moeten afzonderlijk worden beheerd met onafhankelijke gegevensstroom, beveiligingsmodellen en naleving uitdagingen.

Een virtuele Data Center is in feite een verzameling van verschillende maar gerelateerde entiteiten met algemene ondersteunende functies, functies en -infrastructuur. U kunt door uw workloads weergeven als een geïntegreerde vDC, lagere kosten vanwege schaalvoordelen, geoptimaliseerde beveiliging door middel van component- en flow centralisatie, samen met eenvoudiger bewerkingen, beheer en naleving, audits realiseren.

> [!NOTE]
> Het is belangrijk om te begrijpen dat de vDC **niet** een afzonderlijke Azure-product, maar de combinatie van verschillende functies en mogelijkheden om te voldoen aan uw exacte behoeften. vDC is een manier van denken over uw workloads en het gebruik van Azure om uw resources en mogelijkheden in de cloud te maximaliseren. De virtuele domeincontroller is daarom een modulaire benadering van het bouwen van IT-services in Azure, met inachtneming van de organisatie-rollen en verantwoordelijkheden.

De vDC helpen ondernemingen ophalen van werkbelastingen en toepassingen in Azure voor de volgende scenario's:

-   Hosting van meerdere gerelateerde workloads
-   Workloads van een on-premises omgeving migreren naar Azure
-   Implementatie van gedeelde of gecentraliseerde beveiliging en vereisten voor gegevenstoegang voor workloads
-   Met een combinatie van DevOps en centrale IT op de juiste wijze voor een grote onderneming

De sleutel voor het ontgrendelen van de voordelen van vDC, is een gecentraliseerde topologie (hub en spokes) met een combinatie van Azure-functies: [Azure VNet][VNet], [nsg's] [ NSG], [VNet-Peering][VNetPeering], [gebruiker gedefinieerde Routes (UDR)][UDR], en Azure-identiteit met [rol Base Toegangsbeheer (RBAC)][RBAC].

## <a name="who-needs-a-virtual-data-center"></a>Die behoefte hebben aan een virtueel Datacenter?
Een Azure-klant die moeten worden verplaatst van meer dan een paar workloads in Azure kan profiteren van denken over het gebruik van algemene resources. Afhankelijk van de omvang kunnen zelfs enkele toepassingen profiteren van de patronen en -onderdelen die worden gebruikt voor het bouwen van een vDC.

Als uw organisatie een gecentraliseerde beveiliging van de IT-, netwerk heeft, en/of/nalevingsafdeling team, een vDC u kunt afdwingen van beleid punten voor scheiding van rechten, en de uniformiteit van de algemene onderdelen en beschikt toepassingsteams zoveel vrijheid en het besturingselement geschikt is voor uw vereisten.

Organisaties die op zoek bent naar DevOps kunnen gebruikmaken van de vDC-concepten voor het geautoriseerde zakken van Azure-resources en zorg ervoor dat ze hebben volledige controle binnen die groep (abonnement of resourcegroep groep in een algemene abonnement), maar het netwerk en grenzen voor netwerkbeveiliging blijven voldoen zoals gedefinieerd door een centraal beleid in een hub VNet en een resourcegroep.

## <a name="considerations-on-implementing-a-virtual-data-center"></a>Overwegingen over het implementeren van een virtueel Datacenter
Bij het ontwerpen van een vDC, zijn er verschillende problemen van pivotal om te overwegen:

-   Identiteits- en directoryservices
-   Infrastructuur voor de beveiliging
-   Verbinding met de cloud
-   Connectiviteit in de cloud

##### <a name="identity-and-directory-service"></a>*Identiteits- en Active Directory*
Identiteits- en Directory services zijn een belangrijk aspect van alle gegevens, datacenters, zowel on-premises en in de cloud. Identiteit heeft betrekking op alle aspecten van toegang en autorisatie voor de services binnen de vDC. Om ervoor te zorgen dat alleen geautoriseerde gebruikers en processen toegang uw Azure-Account en resources tot, gebruikt Azure verschillende soorten referenties voor verificatie. Het gaat hierbij om wachtwoorden (voor toegang tot het Azure-account), cryptografische sleutels, digitale handtekeningen en certificaten. [*Azure multi-factor Authentication* (MFA)] [ MFA] is een extra beveiligingslaag voor toegang tot Azure-services. Azure MFA biedt een robuuste verificatie met een scala aan gebruiksvriendelijke verificatieopties, telefoongesprek, tekstbericht of mobiele app-meldingen, zodat klanten kiezen geschikte methode kunnen kiezen.

Een grote onderneming moet een proces voor het beheren van identiteiten die worden beschreven van het beheer van afzonderlijke identiteiten, de verificatie, autorisatie, rollen en bevoegdheden binnen of tussen de vDC definiëren. De doelstellingen van dit proces moet u de beveiliging en productiviteit verhogen terwijl u kosten, downtime en herhaalde manuele taken te verkleinen.

Enterprise-organisaties kan vereisen een veeleisende combinatie van services voor verschillende regel van bedrijven (LOB's) en medewerkers hebben vaak verschillende rollen wanneer betrokken met verschillende projecten. Een vDC vereist goede samenwerking tussen verschillende teams, elk met specifieke roldefinities om op te halen van systemen die worden uitgevoerd met goed bestuur. De matrix van taken, toegang en rechten kan zeer complex zijn. Identiteitsbeheer in vDC wordt geïmplementeerd via [ *Azure Active Directory* (AAD)] [ AAD] en Role-Based Access Control (RBAC).

Een Directory-Service is een gedeelde infrastructuur voor het zoeken, beheren, beheren en organiseren van dagelijkse artikelen en netwerkbronnen. Deze resources kunnen opnemen, volumes, mappen, bestanden, printers, gebruikers, groepen, apparaten en andere objecten. Elke resource op het netwerk wordt beschouwd als een object van de directory-server. Informatie over een bron wordt opgeslagen als een verzameling van kenmerken die zijn gekoppeld aan die resource of het object.

Alle Microsoft online Bedrijfsservices Vertrouw op Azure Active Directory (AAD) voor aanmelding bij en andere identiteit moet. Azure Active Directory is een uitgebreide en zeer beschikbare cloudoplossing voor identiteits- en toegangsbeheer die kernadreslijstservices, geavanceerd identiteitsbeheer en toegangsbeheer voor toepassingen combineert. AAD kan worden geïntegreerd met on-premises Active Directory om eenmalige aanmelding voor alle cloud-gebaseerde en lokaal gehoste (on-premises) toepassingen. De kenmerken van de gebruiker van on-premises Active Directory kunnen automatisch worden gesynchroniseerd naar AAD.

Één globale beheerder is niet vereist voor alle machtigingen in een vDC toe te wijzen. In plaats daarvan kan elke bepaalde afdeling (of een groep gebruikers of services in de Active Directory) de machtigingen die vereist zijn voor het beheren van hun eigen resources binnen een vDC hebben. Structureren van machtigingen, moet netwerktaakverdeling. Te veel machtigingen kunnen belemmeren efficiëntie van de prestaties, en te weinig of losse machtigingen kunnen beveiligingsrisico's vergroten. Azure Role-Based Access Control (RBAC) kunt u dit probleem oplossen door het aanbieden van verfijnd toegangsbeheer voor vDC-resources.

##### <a name="security-infrastructure"></a>*Infrastructuur voor de beveiliging*
Beveiligingsinfrastructuur aanwezig zijn, in de context van een vDC, voornamelijk betrekking heeft op de scheiding van verkeer in van de vDC specifieke virtuele-segment en over het beheren van inkomende en uitgaande stromen in de vDC. Azure is gebaseerd op de architectuur met meerdere tenants waarmee wordt voorkomen onbevoegde of onbedoelde verkeer tussen implementaties dat, met behulp van Virtueelnetwerk (VNet) isolatie, toegangsbeheerlijsten (ACL's), load balancers en IP-filters, samen met beleid voor gegevensverkeer stroom. Netwerkadresomzetting (NAT) scheidt interne netwerkverkeer van het externe verkeer.

De Azure-infrastructuur infrastructuurresources aan tenant-workloads en beheert de communicatie naar en van virtuele machines (VM's). De Azure-hypervisor dwingt de geheugen en het processcheiding tussen virtuele machines en veilig routeert het netwerkverkeer voor guest OS tenants.

##### <a name="connectivity-to-the-cloud"></a>*Verbinding met de cloud*
De vDC moet verbinding met externe netwerken kunnen diensten aanbieden aan klanten, partners en/of interne gebruikers. Dit betekent meestal dat de verbinding niet alleen met Internet, maar ook voor on-premises netwerken en data centers.

Klanten kunnen hun beveiligingsbeleid om te bepalen wat bouwen en hoe specifieke vDC gehoste services zijn toegankelijk is vanaf Internet met behulp van virtuele netwerkapparaten (met de inspectie van verkeer en filteren) en aangepaste routering van beleid en het netwerk (filteren Door de gebruiker gedefinieerde Routering en Netwerkbeveiligingsgroepen).

Bedrijven moeten vaak VDC verbinden met on-premises datacenters of andere bronnen. De connectiviteit tussen Azure en on-premises netwerken is daarom een essentieel aspect bij het ontwerpen van een doeltreffende architectuur. Bedrijven hebben twee verschillende manieren om u te maken van een onderlinge verbinding tussen vDC- en on-premises in Azure: doorvoer via Internet en/of particuliere directe verbindingen.

Een [ **Azure Site-naar-Site VPN** ] [ VPN] is een geheugengebieden service via Internet tussen on-premises netwerken en de vDC, tot stand gebracht via veilig versleuteld verbindingen (IPsec/IKE-tunnels). Azure Site-naar-Site-verbinding is flexibel en snel te maken, en vereist geen eventuele verdere aanschaffing, zoals alle verbindingen via internet verbinding maken.

[**ExpressRoute** ] [ ExR] is een verbinding met Azure-service waarmee u particuliere verbindingen maken tussen vDC en de on-premises netwerken. ExpressRoute-verbindingen niet het openbare Internet en bieden betere beveiliging, betrouwbaarheid en hogere snelheden (maximaal 10 Gbps) samen met consistente latentie. ExpressRoute is zeer nuttig voor VDC, als het ExpressRoute-klanten van nalevingsregels voor die zijn gekoppeld aan particuliere verbindingen profiteren kunnen.

Omvat het implementeren van ExpressRoute-verbindingen omgang met een ExpressRoute-serviceprovider. Voor klanten die snel aan de slag wilt, het is gebruikelijk voor Site-naar-Site VPN in eerste instantie gebruiken om verbinding tussen de vDC te maken en on-premises bronnen, en vervolgens migreren naar ExpressRoute-verbinding.

##### <a name="connectivity-within-the-cloud"></a>*Connectiviteit in de cloud*
[VNets] [ VNet] en [VNet-Peering] [ VNetPeering] de basic netwerkservices met een connectiviteit binnen een vDC zijn. Een VNet garandeert een grens van een natuurlijke van isolatie voor vDC-resources en VNet-peering kunt onderling tussen verschillende VNets binnen hetzelfde Azure-regio of zelfs meerdere regio's. Beheer van netwerkverkeer binnen een VNet en tussen VNets moeten overeenkomen met een set beveiligingsregels opgegeven via Access Control Lists ([Network Security Group][NSG]), [virtuele netwerkapparaten ] [ NVA], en aangepaste routering tabellen ([UDR][UDR]).

## <a name="virtual-data-center-overview"></a>Virtuele Data Center-overzicht

### <a name="topology"></a>Topologie
Hub en spokes model uitgebreid het virtuele Data Center binnen één Azure-regio

[![1]][1]

De hub is de centrale zone waarmee inkomend verkeer en/of uitgaande verkeer tussen verschillende zones inspecteert: Internet, on-premises en de knooppunten. De hub en spoke-topologie biedt de IT-afdeling een effectieve manier om af te dwingen beveiligingsbeleid op een centrale locatie, en tegelijkertijd de mogelijkheden voor onjuiste configuratie en blootstelling te beperken.

De hub bevat de algemene serviceonderdelen die worden gebruikt door de knooppunten. Hier volgen enkele typische voorbeelden van algemene centrale services:

-   Het Windows Active Directory-infrastructuur (met de gerelateerde AD FS-service) nodig is voor de verificatie van derden openen vanuit niet-vertrouwde netwerken voordat er toegang tot de werkbelastingen in het knooppunt
-   Een DNS-service om op te lossen naamgeving voor de werkbelasting in de knooppunten, met toegang tot resources on-premises en via Internet
-   Een PKI-infrastructuur, voor het implementeren van eenmalige aanmelding voor werkbelastingen
-   Datatransportbesturing (TCP/UDP) tussen de knooppunten en Internet
-   Datatransportbesturing tussen het knooppunt en on-premises
-   Indien gewenst kunt u datatransportbesturing tussen één knooppunt en een andere

De vDC reduceert de totale kosten met behulp van de infrastructuur gedeelde hub tussen meerdere knooppunten.

De rol van elk knooppunt kan worden op verschillende typen workloads host. De spokes biedt ook een modulaire benadering voor herhaalbare implementaties (bijvoorbeeld, ontwikkelen en testen, gebruikersacceptatietests, Pre-productie- en productie) van de dezelfde werkbelastingen. De spokes kunnen ook worden gebruikt om te scheiden en verschillende groepen binnen uw organisatie (bijvoorbeeld, DevOps-groepen) in te schakelen. Binnen een knooppunt is het mogelijk om een eenvoudige werkbelasting of complexe werkvolumes met meerdere lagen met beheer van netwerkverkeer tussen de lagen te implementeren.

##### <a name="subscription-limits-and-multiple-hubs"></a>Limieten voor een abonnement en meerdere hubs
Elk onderdeel, ongeacht het type, wordt in Azure geïmplementeerd in een Azure-abonnement. De isolatie van Azure-onderdelen in verschillende Azure-abonnementen kunt voldoen aan de vereisten van verschillende LOB's, zoals het instellen van gedifferentieerde niveaus van toegang en autorisatie.

Een enkele vDC kunt omhoog schalen naar een groot aantal knooppunten, hoewel, net als bij elke IT-systeem, er zijn limieten voor platforms. De implementatie van de hub is gebonden aan een specifieke Azure-abonnement, met beperkingen en limieten (bijvoorbeeld een maximum aantal VNet-peerings - Zie [Azure-abonnement en Servicelimieten, quotums en beperkingen] [ Limits] voor meer informatie). In gevallen waarbij limieten mogelijk een probleem, de architectuur kan worden geschaald verder omhoog door het uitbreiden van het model van een enkele hub-knooppunten in een cluster van de hub en spokes. Meerdere hubs in een of meer Azure-regio's kunnen ook worden verbonden met het VNet-Peering, ExpressRoute of site-naar-site VPN.

[![2]][2]

De introductie van meerdere hubs verhoogt de moeite kosten en het beheer van het systeem en kan alleen worden onderbouwd door schaalbaarheid (voorbeelden: systeemlimieten of redundantie) en regionale replicatie (voorbeelden: de prestaties of ramp herstel door eindgebruikers). In scenario's moet waarvoor meerdere hubs, alle hubs streven naar dezelfde set services voor operationele eenvoudig bieden.

##### <a name="interconnection-between-spokes"></a>Onderlinge verbinding tussen knooppunten
Binnen een enkel knooppunt is het mogelijk voor het implementeren van complexe werkvolumes met meerdere lagen. Configuraties met meerdere lagen kunnen worden geïmplementeerd met behulp van subnetten (één voor elke laag) in hetzelfde VNet en filteren van de stromen met nsg's.

Een architect wilt aan de andere kant implementeren van een werkbelasting met meerdere lagen op meerdere VNets. Met behulp van VNet-peering, kunnen knooppunten verbinden met andere knooppunten in de hub dezelfde of verschillende hubs. Een typisch voorbeeld van dit scenario is het geval waarin de verwerking van de toepassing servers bevinden zich in één knooppunt (VNet), terwijl de database wordt geïmplementeerd in een ander knooppunt (VNet). In dit geval is het eenvoudig interconnect van de knooppunten met het VNet-peering en daardoor wilt voorkomen dat doorvoer via de hub. Een zorgvuldige controle-architectuur en beveiliging moet worden uitgevoerd om ervoor te zorgen dat het overslaan van de hub niet overslaan belangrijk beveiligings- of controle van de punten die mogelijk alleen aanwezig zijn in de hub.

[![3]][3]

Knooppunten kunnen ook ook worden verbonden met een knooppunt die als een hub fungeert. Deze methode maakt u een hiërarchie met twee niveaus: het knooppunt in het hogere niveau (niveau 0) worden de hub van lagere knooppunten (level 1) van de hiërarchie. De spokes van vDC moeten het verkeer doorgestuurd naar de centrale hub contact op met de on-premises netwerk of internet. Een architectuur met twee niveaus van hub introduceert complexe routering die Hiermee verwijdert u de voordelen van een eenvoudige hub-spoke-relatie.

Hoewel Azure kunt complexe topologieën, is een van de belangrijkste principes van het concept vDC herhaalbaarheid en eenvoud. Voor het beheer gemakkelijker, is het eenvoudig hub-spoke-ontwerp de aanbevolen vDC-referentiearchitectuur.

### <a name="components"></a>Onderdelen
Een virtuele Data Center bestaat uit vier eenvoudige onderdeeltypen: **infrastructuur**, **perimeternetwerken**, **Workloads**, en **bewaking**.

Elk onderdeeltype bestaat uit verschillende Azure-functies en resources. Uw vDC bestaat van exemplaren van meerdere onderdelen typen en meerdere varianten van hetzelfde onderdeeltype. U hebt bijvoorbeeld veel verschillende, logisch gescheiden, workload-exemplaren die staan voor verschillende toepassingen. U kunt deze typen verschillende onderdelen en exemplaren uiteindelijk de vDC bouwen.

[![4]][4]

De voorgaande architectuur op hoog niveau van een vDC bevat de typen van de verschillende onderdelen die worden gebruikt in verschillende zones van de topologie van de hub-knooppunten. Het diagram toont de onderdelen van de infrastructuur in verschillende onderdelen van de architectuur.

Als een goede gewoonte (voor een on-premises DC of vDC) toegang moeten rechten en bevoegdheden worden op basis van een groep. Omgaan met groepen, kunnen in plaats van afzonderlijke gebruikers onderhouden toegangsbeleid consistent tussen teams en hulpmiddelen bij het minimaliseren van fouten in de configuratie. Toe te wijzen en te verwijderen van gebruikers en naar de juiste groepen kunt de bevoegdheden van een specifieke gebruiker up-to-date te houden.

Elke rolgroep moet een uniek voorvoegsel hebben voor hun namen zodat u eenvoudig kunt identificeren welke groep is gekoppeld aan welke werkbelasting. Bijvoorbeeld, een werkbelasting die als host fungeert voor een authentication-service mogelijk groepen met de naam *AuthServiceNetOps, AuthServiceSecOps AuthServiceDevOps en AuthServiceInfraOps.* Evenzo voor gecentraliseerde rollen of functies die niet gerelateerd aan een bepaalde service, kan worden voorafgegaan door 'Corp' *CorpNetOps* bijvoorbeeld.

Veel organisaties gebruiken een variant van de volgende groepen voor een grote uitsplitsing van functies:

-   De *centrale IT-groep (Corp)* de eigendomsrechten voor het beheren van onderdelen van de infrastructuur (zoals netwerken en beveiliging), en moeten daarom hebt de rol van inzender voor het abonnement (en de controle van de hub) en Inzender-rechten voor het netwerk in de knooppunten. Deze verantwoordelijkheden management tussen meerdere teams, zoals; grote organisatie vaak opsplitsen een groep netwerkbewerkingen (CorpNetOps) (met de exclusieve focus op netwerken) en een groep beveiligingsbewerkingen (CorpSecOps) (die verantwoordelijk is voor de firewall-en beveiligingsbeleid). In dit specifieke geval moeten twee verschillende groepen worden gemaakt voor de toewijzing van deze aangepaste rollen.
-   De *dev & testgroep (AppDevOps)* is de verantwoordelijkheid om workloads (Apps of Services) te implementeren. Deze groep neemt de rol van inzender voor virtuele machines voor IaaS-implementaties en/of een of meer PaaS-Inzender-rollen (Zie [ingebouwde rollen voor op rollen gebaseerd toegangsbeheer][Roles]). Het team voor ontwikkelen en testen kan eventueel moet hebben zichtbaarheid op beveiligingsbeleid (nsg's) en routeringsbeleid (UDR) in de hub of een specifiek knooppunt. Daarom, naast de rol van inzender voor werkbelastingen, deze groep moet ook de rol van lezer netwerk.
-   De *gebruik en onderhoud groep (CorpInfraOps of AppInfraOps)* verantwoordelijk zijn voor het beheren van workloads in de productieomgeving. Deze groep moet een bijdrager aan het abonnement op werkbelastingen in uw productieabonnementen. Sommige organisaties kunnen ook evalueren of ze een extra escalatie team ondersteuningsgroep met de rol van inzender in productie en in het centrale hub-abonnement nodig om mogelijke configuratieproblemen oplossen in de productie -omgeving.

Een vDC is gestructureerd zodat de groepen die zijn gemaakt voor de centrale IT-groepen beheren van de hub bijbehorende groepen hebben op het niveau van de werkbelasting. Naast het hub-resources beheren zou alleen de centrale IT-groepen kunnen zijn voor het beheren van externe toegang en machtigingen voor op het hoogste niveau van het abonnement. Werkbelastinggroepen zou echter kunnen voor het beheren van resources en machtigingen van het VNet onafhankelijk van elkaar op de centrale IT-afdeling.

De vDC moet worden gepartitioneerd voor meerdere projecten veilig hosten in een andere regel van bedrijven (LOB's). Alle projecten vereist verschillende geïsoleerde omgevingen (ontwikkelen, UAT, productie). Afzonderlijke Azure-abonnementen voor elk van deze omgevingen bieden natuurlijke isolatie.

[![5]][5]

Het vorige diagram toont de relatie tussen een organisatie-projecten, gebruikers, groepen en de omgevingen waarin de Azure-onderdelen zijn geïmplementeerd.

Doorgaans in IT, een omgeving (of laag) is een systeem waarin meerdere toepassingen worden geïmplementeerd en uitgevoerd. Grote ondernemingen gebruikt een ontwikkelomgeving (wijzigingen oorspronkelijk gemaakt en getest) en een productie-omgeving (wat eindgebruikers gebruiken). Deze omgevingen worden gescheiden, vaak met verschillende faseringsomgevingen tussen deze gefaseerde implementatie (implementatie) toestaan, testen en terugdraaien in het geval van problemen. Implementatie-architecturen aanzienlijk verschillen, maar meestal het basisproces begint bij de ontwikkeling (DEV) en eindigt op de productie (PROD) nog steeds wordt gevolgd.

Een veelvoorkomende architectuur voor deze typen omgevingen met meerdere lagen bestaat uit DevOps (ontwikkelen en testen), UAT (fasering) en productieomgevingen. Organisaties kunnen gebruikmaken van één of meerdere Azure AD-tenants voor het definiëren van toegang en machtigingen voor deze omgevingen. Het vorige diagram toont een aanvraag indien twee verschillende Azure AD-tenants worden gebruikt: één voor DevOps en UAT en andere zijn uitsluitend bedoeld voor productie.

De aanwezigheid van andere Azure AD-tenants dwingt de scheiding tussen omgevingen. Dezelfde groep gebruikers (bijvoorbeeld: centrale IT-afdeling) moet worden geverifieerd met behulp van een andere URI voor toegang tot een andere AD-tenant wijzigen de rollen of machtigingen van de DevOps- of productie-omgevingen van een project. De aanwezigheid van verschillende gebruikersverificatie voor toegang tot verschillende omgevingen vermindert mogelijke storingen en andere problemen veroorzaakt door menselijke fouten.

#### <a name="component-type-infrastructure"></a>Onderdeeltype: infrastructuur
Dit onderdeeltype is waarin de meeste van de ondersteunende infrastructuur zich bevindt. Het is ook waar uw centrale IT, beveiliging, en/of naleving teams het merendeel van de tijd.

[![6]][6]

Onderdelen van de infrastructuur bieden een onderlinge verbinding tussen de verschillende onderdelen van een vDC, en zijn aanwezig zijn in zowel de hub en de knooppunten. De verantwoordelijkheid voor het beheer en onderhoud van de onderdelen van de infrastructuur is doorgaans toegewezen aan de centrale IT en/of beveiligingsteam.

Een van de primaire taken van de IT-infrastructuur-team is om te waarborgen van de consistentie van IP-adres-schema's binnen de hele organisatie. De privé-IP-adresruimte toegewezen aan de behoeften van uw vDC consistent en niet overlapt met het particuliere IP-adressen toegewezen aan uw on-premises netwerken.

Terwijl NAT op het edge-routers on-premises of in Azure-omgevingen IP-adresconflicten te voorkomen kan, wordt deze complicaties toegevoegd aan de infrastructuuronderdelen van uw. Eenvoud van management is een van de belangrijkste doelstellingen van vDC, zodat u met behulp van NAT voor het afhandelen van problemen voor het IP-is geen aanbevolen oplossing.

Onderdelen van de infrastructuur bevatten de volgende functionaliteit:

-   [**Identiteits- en directory services**][AAD]. Toegang tot elk resourcetype in Azure wordt bepaald door een identiteit die is opgeslagen in Active Directory. De service slaat niet alleen de lijst met gebruikers, maar ook de toegangsrechten tot resources in een specifieke Azure-abonnement. Deze services kunnen bestaan alleen in de cloud, of ze kunnen worden gesynchroniseerd met on-premises identiteit die zijn opgeslagen in Active Directory.
-   [**Virtueel netwerk**][VPN]. Virtuele netwerken zijn een van de belangrijkste onderdelen van een vDC, en kunnen u een verkeersisolatiegrens maken op het Azure-platform. Een Virtueelnetwerk bestaat uit één of meerdere virtuele netwerksegmenten, elk met een voorvoegsel van specifieke IP-netwerk (subnet). Het Virtueelnetwerk definieert een interne perimeter-gebied waar virtuele machines van IaaS en PaaS-services persoonlijke communicatie kunnen instellen. Virtuele machines (en PaaS-services) in een virtueel netwerk kan niet rechtstreeks met virtuele machines communiceren (en PaaS-services) in een ander virtueel netwerk, zelfs als beide virtuele netwerken worden gemaakt van dezelfde klant, onder hetzelfde abonnement. Isolatie is een belangrijke eigenschap die ervoor zorgt dat de klant-VM's en communicatie blijft privé binnen een virtueel netwerk.
-   [**UDR**][UDR]. Verkeer in een Virtueelnetwerk wordt doorgestuurd op basis van de routeringstabel system standaard. Een Route definiëren van de gebruiker is een aangepaste routeringstabel die netwerkbeheerders kunt koppelen aan een of meer subnetten te overschrijven van het gedrag van de routeringstabel system en een communicatiepad binnen een virtueel netwerk te definiëren. De aanwezigheid van udr's garandeert dat uitgaand verkeer van de doorvoer van het knooppunt door specifieke aangepaste VM's en/of Network Virtual Appliances en load balancers aanwezig in de hub en de knooppunten.
-   [**NSG**][NSG]. Een Netwerkbeveiligingsgroep is een lijst met beveiligingsregels die fungeren als verkeer filteren op IP-bronnen, doel-IP-protocollen, bron-IP-poorten en doel-IP-poorten. De NSG kan worden toegepast op een subnet, een virtuele NIC-kaart die is gekoppeld aan een Azure-VM of beide. De nsg's zijn essentieel voor het implementeren van een juiste datatransportbesturing in de hub en in de knooppunten. Het niveau van beveiliging die wordt geboden door de NSG is een functie van welke poorten u open, en voor welk doel. Klanten moeten aanvullende VM-filters met host-gebaseerde firewall, zoals IPtables of de Windows-Firewall toepassen.
-   [**DNS**][DNS]. De naamomzetting van resources in de VNets van een vDC wordt geboden via DNS. Azure biedt DNS-services voor zowel [openbare][DNS] en [persoonlijke] [ PrivateDNS] naamomzetting. Privézones bieden naamomzetting zowel binnen een virtueel netwerk en tussen virtuele netwerken. U kunt niet alleen span privézones hebben tussen virtuele netwerken in dezelfde regio, maar ook in regio's en abonnementen. Voor openbare resolutie biedt Azure DNS een hostingservice voor DNS-domeinen die naamomzetting met behulp van Microsoft Azure-infrastructuur biedt. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services.
-   [**Abonnement** ] [ SubMgmt] en [ **Resource Group Management**][RGMgmt]. Een abonnement definieert een natuurlijke grens voor het maken van meerdere groepen van resources in Azure. Resources in een abonnement worden samen in logische containers met de naam resourcegroepen worden samengesteld. De resourcegroep vertegenwoordigt een logische groep om de resources van een vDC te organiseren.
-   [**RBAC**][RBAC]. Via RBAC is het mogelijk te kaart organisatierol samen met rechten voor toegang tot specifieke Azure-resources, zodat u kunt gebruikers beperken tot alleen een bepaalde subset van acties. Met RBAC kunt u toegang verlenen door de juiste rol toewijzen aan gebruikers, groepen en toepassingen binnen het bereik van de relevante. Het bereik van een roltoewijzing mag bestaan uit een Azure-abonnement, een resourcegroep of één resource. RBAC kunt overname van machtigingen. Een rol die is toegewezen aan een bovenliggend bereik ook verleent toegang aan de onderliggende objecten die hierin zijn opgenomen. Met RBAC kunt u taken scheiden en alleen de mate van toegang verlenen aan gebruikers die ze nodig hebben om hun werk te kunnen. Bijvoorbeeld, RBAC gebruiken om één werknemer virtuele machines in een abonnement beheren terwijl een andere SQL-databases binnen hetzelfde abonnement beheren kunt.
-   [**VNet-Peering**][VNetPeering]. De fundamentele functie gebruikt voor het maken van de infrastructuur van een vDC is VNet-Peering, een mechanisme dat twee virtuele netwerken (VNets) verbindt in dezelfde regio via het netwerk van Azure data center of met behulp van de wereldwijde Azure-backbone tussen regio's.

#### <a name="component-type-perimeter-networks"></a>Onderdeeltype: Perimeternetwerken
[Perimeternetwerk] [ DMZ] onderdelen (ook wel een DMZ-netwerk) kunnen u de netwerkverbinding voorzien van uw on-premises of fysieke datacenternetwerken, samen met de connectiviteit van en naar Internet. Het is ook waar uw netwerk- en beveiligingsteam betrekken waarschijnlijk het merendeel van de tijd.

Inkomende pakketten moeten stromen via de beveiligingsapparaten in de hub, zoals de firewall,-id's en IP-Adressen, voordat de back-end-servers in de knooppunten wordt bereikt. Afhankelijk van de Internet-pakketten van de werkbelastingen moeten ook stromen via de beveiligingsapparaten in het perimeternetwerk voor het afdwingen van beleid, controle en controledoeleinden, voordat deze uit het netwerk.

Perimeter-netwerkonderdelen bieden de volgende functies:

-   [Virtuele netwerken][VNet], [UDR][UDR], [NSG][NSG]
-   [Virtueel netwerkapparaat][NVA]
-   [Load Balancer][ALB]
-   [Application Gateway][AppGW] / [WAF][WAF]
-   [Openbare IP-adressen][PIP]

Normaal gesproken de centrale IT en beveiligingsteams verantwoordelijk voor de Vereistedefinitie en bewerkingen van de perimeternetwerken zijn.

[![7]][7]

Het vorige diagram toont het afdwingen van twee verbindingen met toegang tot het internet en een on-premises netwerk, zowel in de hub woont. Het perimeternetwerk met internet kunt in één hub, omhoog schalen voor de ondersteuning van grote aantallen LOB's, met behulp van meerdere farms met Web Application Firewalls (WAF's) en/of firewalls.

[**Virtuele netwerken** ] [ VNet] de hub is doorgaans gebaseerd op een VNet met meerdere subnetten voor het hosten van de verschillende typen services filteren op te halen en verkeer naar of van het internet via de NVA's, WAF's en Azure Toepassingsgateways.

[**UDR** ] [ UDR] UDR met behulp van klanten kunnen implementeren firewalls, IDS/IPS, en andere virtuele apparaten en netwerkverkeer routeren via deze beveiligingsapparaten voor beveiligingsbeleid opleggen aan grens, controle en controle. Udr's kunnen worden gemaakt in de hub en de knooppunten om te waarborgen dat verkeer passages door middel van de specifieke aangepaste virtuele machines, virtuele netwerkapparaten en load balancers die worden gebruikt door de vDC. Om ervoor te zorgen dat verkeer dat is gegenereerd op basis van virtuele machines in de doorvoer spoke woont naar de juiste virtuele apparaten, moet een UDR worden ingesteld in de subnetten van het knooppunt door de front-end-IP-adres van de interne load balancer als de volgende hop. De interne load balancer verdeelt het interne verkeer naar de virtuele apparaten (load balancer back-end-pool).

[![8]][8]

[**Virtuele netwerkapparaten** ] [ NVA] In de hub, meestal het perimeternetwerk met toegang tot het internet wordt beheerd via een farm met firewalls en/of Web Application Firewalls (WAF's).

Verschillende LOB's gebruiken vaak veel web-apps en deze toepassingen meestal te maken krijgen met verschillende beveiligingsproblemen en potentiële aanvallen. Web Applications Firewalls zijn een speciale vorm van het product gebruikt voor het detecteren van aanvallen tegen de webtoepassingen (HTTP/HTTPS) in de diepte van meer dan een algemene firewall. Vergeleken met traditie firewalltechnologie, hebben WAF's een aantal specifieke functies interne webservers tegen bedreigingen te beschermen.

Een firewall-farm is firewalls werkt in combinatie onder de dezelfde algemene beheer, met een set regels voor het beveiligen van de werkbelastingen die worden gehost in de knooppunten, groep en toegang tot on-premises netwerken. Een firewall-farm is minder gespecialiseerde software vergeleken met een WAF, maar heeft een brede toepassing scope om te filteren en elk type van uitgaande en binnenkomende netwerkverkeer te inspecteren. Firewall-farms zijn normaal gesproken geïmplementeerd in Azure door virtuele netwerkapparaten (NVA's), die beschikbaar in de Azure marketplace zijn.

Het verdient aanbeveling met één set NVA's voor verkeer dat afkomstig is op het Internet, en één voor verkeer dat afkomstig is on-premises. Met behulp van slechts één set NVA's voor zowel is gevaarlijk zijn, omdat er geen beveiligingsperimeter tussen de twee sets netwerkverkeer. Met behulp van afzonderlijke NVA's vermindert de complexiteit van de beveiligingsregels controleren, en maakt duidelijk welke regels overeenkomen met die inkomende netwerkaanvraag.

Meest grote ondernemingen die beheren meerdere domeinen. Azure DNS kan worden gebruikt voor het hosten van de DNS-records voor een bepaald domein. Voorbeeld: kan het virtuele IP-adres (VIP) van de externe load balancer van Azure (of de WAF's) worden geregistreerd in de A-record van een Azure DNS-record.

[**Azure Load Balancer** ] [ ALB] van Azure load balancer biedt een hoge beschikbaarheid Layer 4 (TCP, UDP)-service, die inkomend verkeer over service-exemplaren die zijn gedefinieerd in een load balancer-set kunt distribueren. Verkeer dat wordt verzonden naar de load balancer van front-eindpunten (openbare IP-eindpunten of privé-IP-adreseindpunten) kan opnieuw worden gedistribueerd, met of zonder adresomzetting naar een set van back-end IP-adresgroep (voorbeelden wordt; Virtuele netwerkapparaten of VM's).

De Azure Load Balancer kan de status van de verschillende exemplaren van server ook test en wanneer een test niet reageert niet meer de load balancer verkeer verzenden naar het exemplaar niet in orde. In een vDC hebben we de aanwezigheid van een externe load balancer in de hub (bijvoorbeeld verdelen het verkeer naar de NVA's) en in de knooppunten (om uit te voeren taken, zoals taakverdeling van verkeer tussen verschillende virtuele machines van een toepassing met meerdere lagen).

[**Application Gateway** ] [ AppGW] Microsoft Azure Application Gateway is een speciaal virtueel apparaat application delivery controller (ADC) leveren als een service biedt verschillende layer 7 taakverdeling mogelijkheden voor uw toepassing. Hiermee kunt u de productiviteit van webfarms optimaliseren door het offloaden van CPU intensieve SSL-beëindiging aan de toepassingsgateway. Het bevat ook andere routeringsmogelijkheden voor laag 7, met inbegrip van round robin-distributie van inkomend verkeer, cookies op basis van sessieaffiniteit, routering van URL's op padbasis en de mogelijkheid voor het hosten van meerdere websites achter één toepassingsgateway. Een WAF (Web Application Firewall) is ook beschikbaar als onderdeel van de WAF SKU van de toepassingsgateway. Deze SKU biedt beveiliging voor webtoepassingen tegen veelvoorkomende beveiligingsproblemen op het web. Application Gateway kan worden geconfigureerd als op internet gerichte gateway, interne enige gateway of een combinatie van beide. 

[**Openbare IP-adressen** ] [ PIP] sommige Azure-functies kunt u service-eindpunten naar een openbare IP-adres dat kunt u uw resource worden benaderd vanaf het internet te koppelen. Dit eindpunt maakt gebruik van NAT (Network Address Translation) voor het routeren van verkeer naar de interne-adres en poort op Azure virtual network. Dit pad is de eenvoudigste manier voor het externe verkeer om door te geven in het virtuele netwerk. De openbare IP-adressen kunnen worden geconfigureerd om te bepalen welk verkeer wordt doorgegeven, en hoe en waar deze wordt omgezet naar het virtuele netwerk.

#### <a name="component-type-monitoring"></a>Onderdeeltype: bewaking
Bewaking onderdelen bieden zichtbaarheid en waarschuwingen vanuit alle andere onderdelen typen. Alle teams moeten toegang hebben tot bewaking voor de onderdelen en services dat ze toegang hebben. Als u een gecentraliseerde help helpdesk of de operations-teams hebt, moet ze toegang tot de gegevens van deze onderdelen zijn geïntegreerd.

Azure biedt verschillende typen logboekregistratie en bewaking van services voor het volgen van het gedrag van Azure gehoste bronnen. Beheer en controle van workloads in Azure is op basis van logboekgegevens niet alleen op verzamelen, maar ook de mogelijkheid om acties starten op basis van specifieke gerapporteerde gebeurtenissen.

[**Azure Monitor** ] [ Monitor] -Azure bevat meerdere services die afzonderlijk uitvoeren van een specifieke rol of de taak in de ruimte bewaking. Samen bieden deze services een uitgebreide oplossing voor het verzamelen en analyseren van telemetrie en het reageren daarop vanuit uw toepassing en de Azure-resources die deze ondersteunen. Ze kunnen ook werken aan het bewaken van kritieke on-premises bronnen om een ​​hybride bewakingsomgeving te bieden. Begrip van de beschikbare hulpprogramma’s en gegevens is de eerste stap in het ontwikkelen van een complete bewakingsstrategie voor uw toepassing.

Er zijn twee belangrijke gebeurtenistypen Logboeken in Azure:

-   [**Activiteitenlogboeken** ] [ ActLog] (ook als 'Operationeel logboek' genoemd) bieden inzicht in de bewerkingen die zijn uitgevoerd voor resources in het Azure-abonnement. Deze logboeken rapporteren de controlelaag gebeurtenissen voor uw abonnementen. Elke Azure-resource produceert auditlogboeken.

-   [**Diagnostische logboeken in Azure** ] [ DiagLog] zijn logboeken die worden gegenereerd door een resource met uitgebreide, regelmatig gegevens over de werking van die resource. Resourcetype is afhankelijk van de inhoud van deze logboeken.

[![9]][9]

In een vDC is het uiterst belangrijk om bij te houden van de logboeken van nsg's, met name deze informatie:

-   [**Gebeurtenislogboeken**][NSGLog]: bevat informatie over welke NSG-regels worden toegepast op virtuele machines en rolinstanties op basis van MAC-adres.
-   [**Logboeken teller**][NSGLog]: houdt bij hoe vaak elke NSG-regel is uitgevoerd om te weigeren of verkeer toestaan.

Alle logboeken kunnen worden opgeslagen in Azure Storage-Accounts voor controle-, statische analysis- of back-updoeleinden. Wanneer de logboeken worden opgeslagen in Azure storage-account, kunnen klanten verschillende soorten frameworks gebruiken om te halen, voorbereiden, analyseren en visualiseren van deze gegevens om te rapporteren van de status en integriteit van cloudresources.

Grote ondernemingen moeten al een standaard framework voor het bewaken van on-premises systemen hebben aangeschaft en die framework voor het integreren van logboeken die worden gegenereerd door cloudimplementaties kunnen uitbreiden. Organisaties die willen houden de registratie in de cloud, is Log Analytics [LogAnalytics] een uitstekende keuze. Sinds de Log Analytics wordt geïmplementeerd als een cloud-gebaseerde service, kunt u laten actief en werkend snel met minimale investeringen in infrastructuurservices. Log Analytics kan ook worden geïntegreerd met System Center-onderdelen, zoals System Center Operations Manager om uit te breiden van uw bestaande investeringen in beheerfuncties in de cloud.

Log Analytics is een service in Azure waarmee u kunt verzamelen, correleren, zoeken en erop logboek- en prestatiegegevens die worden gegenereerd door besturingssystemen, toepassingen en onderdelen van de infrastructuur-cloud. Dit biedt klanten real-time operational insights met behulp van geïntegreerde Zoek- en aangepaste dashboards voor het analyseren van alle records voor uw workloads in een vDC.

De [(Netwerkprestatiemeter)] [ NPM] oplossing in OMS kunt bieden gedetailleerde informatie end-to-end, met inbegrip van een weergave van uw Azure-netwerken en on-premises netwerken. Met specifieke controleprogramma's voor ExpressRoute- en openbare services.

#### <a name="component-type-workloads"></a>Onderdeeltype: Workloads
Onderdelen van de werkbelasting zijn waar uw werkelijke toepassingen en services zich bevinden. Het is ook waar uw toepassing development-teams het merendeel van de tijd.

De workload-mogelijkheden zijn echt eindeloos. Hier volgen slechts enkele van de typen werkbelasting mogelijk:

**Interne LOB-toepassingen**

Line-of-business-toepassingen zijn kritieke computertoepassingen aan de lopende bewerking van een onderneming. LOB-toepassingen hebben enkele algemene kenmerken:

-   **Interactieve**. LOB-toepassingen, zijn interactief aard: gegevens worden ingevoerd en resultaat/rapporten worden geretourneerd.
-   **Gegevensgestuurde**. LOB-toepassingen zijn gegevens intensieve met vaak toegang tot de databases of andere opslag.
-   **Geïntegreerde**. LOB-toepassingen bieden integratie met andere systemen binnen of buiten de organisatie.

**Websites (Internet of een intern gerichte) klantgerichte** de meeste toepassingen die interactie met Internet hebben zijn websites. Azure biedt de mogelijkheid voor het uitvoeren van een website in een IaaS-VM of vanaf een [Azure Web Apps] [ WebApps] site (PaaS). Azure Web Apps ondersteuning bieden voor integratie met vnet's die de implementatie van de Web-Apps in het knooppunt van een vDC toestaan. Ga hiervoor naar interne gerichte websites, gaat u met de VNET-integratie, moet u niet een Internet-eindpunt voor uw toepassingen beschikbaar maken, maar kan in plaats daarvan gebruik van de bronnen via niet-internet routeerbare privéadressen van uw privé-VNet.

**BIG Data en analyse** wanneer gegevens moeten worden uitgebreid naar een zeer groot, databases kunnen niet omhoog schalen naar behoren. Hadoop-technologie biedt een systeem in gedistribueerde query's parallel worden uitgevoerd op een groot aantal knooppunten. Klanten hebben de mogelijkheid om uit te voeren van data-workloads in de IaaS-VM's of PaaS ([HDInsight][HDI]). HDInsight worden geïmplementeerd in een VNet op basis van locatie, kunnen worden geïmplementeerd in een cluster in een knooppunt van de vDC.

**Gebeurtenissen en -berichten**
[Azure Event Hubs] [ EventHubs] is een grootschalige service voor telemetrieopname die u verzamelt, transformeert en opslaat miljoenen gebeurtenissen. Als een gedistribueerd streamingplatform is biedt het lage latentie en configureerbare retentietijd, zodat u enorme hoeveelheden telemetriegegevens worden opgenomen in Azure maken en die gegevens lezen vanuit diverse toepassingen. Met Event Hubs, kan één stroom zowel real-time en batch op basis van pijplijnen ondersteunen.

Een zeer betrouwbare cloudberichtenservice tussen toepassingen en services, kan worden geïmplementeerd via [Azure Service Bus] [ ServiceBus] dat biedt asynchrone brokered messaging tussen client en server samen met gestructureerde (FIFO-principe)-berichten first in first out en mogelijkheden voor publiceren/abonneren.

[![10]][10]

### <a name="multiple-vdc"></a>Meerdere vDC
In dit artikel is tot nu toe gericht op een enkele vDC, met een beschrijving van de fundamentele onderdelen en architectuur die bijdragen aan een robuuste vDC. Azure-functies, zoals Azure load balancer, NVA's, beschikbaarheidssets, schaalsets, samen met andere methoden bijdragen aan een systeem waarmee u effen SLA's in uw productie-services.

Echter een enkel vDC wordt gehost in een enkele regio en is kwetsbaar voor grote storing die van invloed die hele regio. Klanten die u wilt bereiken van hoge Sla's moeten de services via implementaties van hetzelfde project in twee (of meer) VDC, geplaatst in verschillende regio's beveiligen.

Naast SLA problemen zijn er enkele algemene scenario's waarin het implementeren van meerdere VDC zinvol:

-   Regionale/Global aanwezigheid
-   Herstel na noodgevallen
-   Mechanisme voor het omleiden van verkeer tussen DC

#### <a name="regionalglobal-presence"></a>Regionale/Global aanwezigheid
Azure-datacenters zijn aanwezig in verschillende regio's over de hele wereld. Bij het selecteren van meerdere Azure-datacenters, klanten moeten rekening houden met twee factoren gerelateerde: geografische afstanden en latentie. Klanten moeten de geografische afstand tussen de VDC en de afstand tussen de vDC en de eindgebruikers, bieden de beste gebruikerservaring te evalueren.

Er moet ook de Azure-regio waar VDC worden gehost om te voldoen aan wettelijke eisen die tot stand gebracht door elke jurisdictie waarmee uw organisatie werkt.

#### <a name="disaster-recovery"></a>Herstel na noodgevallen
De implementatie van een herstelplan na noodgevallen is sterk met betrekking tot het type workload betrokken, en de mogelijkheid om te synchroniseren van de status van de werkbelasting tussen verschillende VDC. In het ideale geval willen de meeste klanten om toepassingsgegevens tussen implementaties die worden uitgevoerd in twee verschillende VDC voor het implementeren van een mechanisme voor snelle failover te synchroniseren. De meeste toepassingen zijn gevoelig voor latentie, en dat kan leiden tot mogelijke time-out en vertraging in het synchroniseren van gegevens.

Synchronisatie of heartbeat-bewaking van toepassingen in verschillende VDC moet communicatie ertussen. Twee VDC in verschillende regio's kunnen worden verbonden via:

-   VNet-Peering - VNet-Peering verbinding hubs tussen regio's
-   ExpressRoute persoonlijke peering wanneer de vDC-hubs zijn verbonden met hetzelfde ExpressRoute-circuit
-   meerdere ExpressRoute-circuits die zijn verbonden via uw bedrijfsnetwerken en uw mesh vDC verbonden met de ExpressRoute-circuits
-   Site-naar-Site VPN-verbindingen tussen uw vDC-hubs in elke Azure-regio

VNet-Peering- of ExpressRoute-verbindingen zijn doorgaans de beste manier vanwege de hogere bandbreedte en consistente latentie als doorvoer via de Microsoft-backbone.

Er is geen magic recept voor het valideren van een toepassing die wordt gedistribueerd tussen twee (of meer) verschillende VDC zich in verschillende regio's. Klanten moeten kwalificatie netwerktests om te controleren of de latentie en bandbreedte van de verbindingen en het doel of synchrone of asynchrone gegevensreplicatie geschikt is en wat de optimale beoogde hersteltijd (RTO) voor uw workloads kan worden uitgevoerd.

#### <a name="mechanism-to-divert-traffic-between-dc"></a>Mechanisme voor het omleiden van verkeer tussen DC
Een effectieve methode om te leiden van het binnenkomende verkeer in één domeincontroller naar een andere is gebaseerd op DNS. [Met Azure Traffic Manager] [ TM] maakt gebruik van het mechanisme Domain Name System (DNS) om het verkeer van eindgebruikers door naar het meest geschikte openbare eindpunt in een specifieke vDC te regelen. Via tests, Traffic Manager controleert periodiek de servicestatus van openbare eindpunten in verschillende VDC en in geval van storing van deze eindpunten wordt gerouteerd automatisch naar de secundaire vDC.

Traffic Manager werkt op Azure openbare eindpunten en kunnen worden gebruikt, bijvoorbeeld op het besturingselement/omleiden van verkeer naar virtuele Azure-machines en Web-Apps in de juiste vDC. Traffic Manager is tegen zelfs bij een failover van de gehele Azure-regio en de distributie van gebruikersverkeer voor service-eindpunten in verschillende VDC op basis van verschillende criteria (bijvoorbeeld fout van een service in een specifieke vDC of Selecteer de vDC kunt beheren met de laagste netwerklatentie voor de client).

### <a name="conclusion"></a>Conclusie
Het virtuele Data Center is een benadering voor data center migratie naar de cloud die gebruikmaakt van een combinatie van functies en mogelijkheden voor een schaalbare architectuur maken in Azure wordt gemaximaliseerd gebruik van cloud-bronnen, kosten te verlagen en system-beheer te vereenvoudigen. Het concept vDC is gebaseerd op de topologie van een hub-knooppunten, leveren algemene gedeelde services in de hub en zodat specifieke toepassingen/werkbelastingen op de knooppunten. Een vDC komt overeen met de structuur van het bedrijf rollen, waarbij verschillende afdelingen (centrale IT-afdeling, DevOps, bewerking en onderhoud) samen, elk met een specifieke lijst met rollen en taken werken. Een vDC voldoet aan de vereisten voor een 'Lift and Shift'-migratie, maar biedt ook veel voordelen met systeemeigen cloudimplementaties.

## <a name="references"></a>Verwijzingen
De volgende functies zijn in dit document besproken. Klik op de koppelingen voor meer informatie.

| | | |
|-|-|-|
|Network-functies|Taakverdeling|Connectiviteit|
|[Virtuele netwerken van Azure][VNet]</br>[Netwerkbeveiligingsgroepen][NSG]</br>[NSG-Logboeken][NSGLog]</br>[Door de gebruiker gedefinieerde routering][UDR]</br>[Virtuele netwerkapparaten][NVA]</br>[Openbare IP-adressen][PIP]</br>[DNS]|[Azure Load Balancer (N3) ][ALB]</br>[Application Gateway (L7) ][AppGW]</br>[Web Application Firewall][WAF]</br>[Azure Traffic Manager][TM] |[VNet-Peering][VNetPeering]</br>[Virtueel particulier netwerk][VPN]</br>[ExpressRoute][ExR]
|Identiteit</br>|Bewaking</br>|Beste praktijken</br>|
|[Azure Active Directory][AAD]</br>[Multi-factor Authentication][MFA]</br>[Role Base Access besturingselementen][RBAC]</br>[Standaard-AAD-rollen][Roles] |[Azure Monitor][Monitor]</br>[Activiteitenlogboeken][ActLog]</br>[Diagnostische logboeken][DiagLog]</br>[Microsoft Operations Management Suite][OMS]</br>[Network Performance Monitor][NPM]|[Aanbevolen procedures voor perimeter-netwerken][DMZ]</br>[Beheer van abonnementen][SubMgmt]</br>[Beheer van resourcegroep][RGMgmt]</br>[Limieten voor Azure-abonnement][Limits] |
|Andere Azure-Services|
|[Azure-Web-Apps][WebApps]</br>[HDInsights (Hadoop) ][HDI]</br>[Event Hubs][EventHubs]</br>[Service Bus][ServiceBus]|



## <a name="next-steps"></a>Volgende stappen
 - Verken [VNet-Peering][VNetPeering], de technologie onderbouwing voor vDC-hub en spoke-ontwerpen
 - Implementeer [AAD] [ AAD] aan de slag met [RBAC] [ RBAC] verkennen
 - Ontwikkel een model voor het beheer en de resourcegroep en RBAC model om te voldoen aan de structuur, vereisten, en het beleid van uw organisatie. De belangrijkste activiteit is van plan. Plan zo veel mogelijk reorganisaties, fusies, nieuwe productregels, enzovoort.

<!--Image References-->
[0]: ./media/networking-virtual-datacenter/redundant-equipment.png "voorbeelden van onderdeel overlapping" 
[1]: ./media/networking-virtual-datacenter/vdc-high-level.png "voorbeeld op hoog niveau van de hub en spoke-vDC"
[2]: ./media/networking-virtual-datacenter/hub-spokes-cluster.png "cluster van hubs en spokes"
[3]: ./media/networking-virtual-datacenter/spoke-to-spoke.png "Spoke-to-spoke"
[4]: ./media/networking-virtual-datacenter/vdc-block-level-diagram.png "niveau diagram van de vDC blokkeren"
[5]: ./media/networking-virtual-datacenter/users-groups-subsciptions.png "gebruikers, groepen, abonnementen en -projecten"
[6]: ./media/networking-virtual-datacenter/infrastructure-high-level.png "op hoog niveau infrastructuurdiagram"
[7]: ./media/networking-virtual-datacenter/highlevel-perimeter-networks.png "op hoog niveau infrastructuurdiagram"
[8]: ./media/networking-virtual-datacenter/vnet-peering-perimeter-neworks.png "en omtrek van VNet-Peering-netwerken"
[9]: ./media/networking-virtual-datacenter/high-level-diagram-monitoring.png "diagram op hoog niveau voor bewaking"
[10]: ./media/networking-virtual-datacenter/high-level-workloads.png "diagram op hoog niveau voor de werkbelasting"

<!--Link References-->
[Limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits
[Roles]: https://docs.microsoft.com/azure/role-based-access-control/built-in-roles
[VNet]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview
[NSG]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg
[DNS]: https://docs.microsoft.com/azure/dns/dns-overview
[PrivateDNS]: https://docs.microsoft.com/azure/dns/private-dns-overview
[VNetPeering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview 
[UDR]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview 
[RBAC]: https://docs.microsoft.com/azure/role-based-access-control/overview
[MFA]: https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication
[AAD]: https://docs.microsoft.com/azure/active-directory/active-directory-whatis
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways 
[ExR]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction 
[NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[SubMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-governance 
[RGMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview
[DMZ]: https://docs.microsoft.com/azure/best-practices-network-security
[ALB]: https://docs.microsoft.com/azure/load-balancer/load-balancer-overview
[PIP]: https://docs.microsoft.com/azure/virtual-network/resource-groups-networking#public-ip-address
[AppGW]: https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[WAF]: https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview
[Monitor]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/
[ActLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs 
[DiagLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs
[NSGLog]: https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log
[OMS]: https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview
[NPM]: https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor
[WebApps]: https://docs.microsoft.com/azure/app-service/
[HDI]: https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-introduction
[EventHubs]: https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs 
[ServiceBus]: https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview
[TM]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview
