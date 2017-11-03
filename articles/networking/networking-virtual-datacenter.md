---
title: Microsoft Azure virtuele Data Center | Microsoft Docs
description: Informatie over het bouwen van uw virtuele datacenter in Azure
services: networking
author: tracsman
manager: rossort
tags: azure-resource-manager
ms.service: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jonor
ms.openlocfilehash: 7dcc6b77bde8b8a7b485525105c1a07c53301f8e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="microsoft-azure-virtual-data-center"></a>Microsoft Azure virtuele Data Center
**Microsoft Azure**: sneller, geld besparen, integreren lokale apps en gegevens

## <a name="overview"></a>Overzicht
On-premises toepassingen naar Azure migreren, biedt zelfs zonder aanzienlijke wijzigingen (een methode die bekend staat als 'lift- en verschuiven'), organisaties de voordelen van een beveiligde en voordelige infrastructuur. Ondernemingen moeten echter de meeste van de flexibiliteit om mogelijk te maken met cloud computing, evolueren hun architecturen om te profiteren van de mogelijkheden van Azure. Microsoft Azure biedt hyperschaal services en infrastructuur, bedrijfsniveau mogelijkheden en betrouwbaarheid en veel keuzes voor hybride verbindingen. Klanten kunnen kiezen voor toegang tot deze cloudservices via Internet of met Azure ExpressRoute, waarmee u verbinding met het particuliere netwerk. Het Microsoft Azure-platform kan klanten probleemloos hun infrastructuur in de cloud uitbreiden en bouwen van meerdere lagen architecturen. Daarnaast bieden Microsoft-partners uitgebreide mogelijkheden die door het aanbieden van beveiligingsservices en virtuele apparaten die zijn geoptimaliseerd voor het uitvoeren in Azure.

Dit artikel bevat een overzicht van patronen en modellen die kunnen worden gebruikt voor het oplossen van de problemen die architecturale schaal, prestaties en beveiliging veel klanten face wanneer u nadenkt over en masse verplaatsen naar de cloud. Een overzicht van hoe u kunt aanpassen aan verschillende organisatie-IT-functies voor het beheer en toezicht van het systeem is ook besproken, met nadruk in de beveiligingsvereisten van de en kosten optimalisatie.

## <a name="what-is-a-virtual-data-center"></a>Wat is er een virtuele-Datacenter?
In het begin zijn cloudoplossingen ontworpen om één, relatief geïsoleerde hosttoepassingen, in het openbare spectrum. Deze aanpak goed functioneren voor een aantal jaren. Echter, als de voordelen van cloudcomputing oplossingen is gebleken en meerdere grootschalige workloads zijn gehost op de cloud, beveiliging, betrouwbaarheid, prestaties, adressering en de kosten van de problemen van implementaties in een of meer regio's is geworden essentieel gedurende de levenscyclus van de cloudservice.

Het volgende diagram voor cloud-implementatie ziet u enkele voorbeelden van beveiliging hiaten (rood kader) en ruimte voor de optimalisatie van virtuele netwerkapparaten via werkbelastingen (geel vak).

[![0]][0]

Het virtuele Data Center (VDC's) is geboren uit deze noodzaak voor het schalen van ter ondersteuning van werklast en de noodzaak om te gaan met de problemen die ontstaan doordat ondersteunende grootschalige toepassingen in de openbare cloud.

Een vDC is niet de toepassingsworkloads in de cloud, maar ook het netwerk, beveiliging, beheer en infrastructuur (bijvoorbeeld DNS- en Directory Services). Meestal bovendien een particuliere verbinding terug naar een lokaal netwerk of data center. Als u meer werkbelastingen naar Azure verplaatst, is het belangrijk om na te denken over de ondersteunende infrastructuur en de objecten die in deze werkbelastingen worden geplaatst. Nadenken over de structuur van resources zorgvuldig kunt voorkomen dat de komst van honderden 'werkbelasting eilanden' moeten afzonderlijk worden beheerd met onafhankelijke gegevensstroom, beveiligingsmodellen en naleving uitdagingen.

Een virtuele Data Center is in feite een verzameling afzonderlijke maar gerelateerde entiteiten met het gemeenschappelijke ondersteunende functies, onderdelen en infrastructuur. Door uw werkbelastingen als een geïntegreerde vDC bekijkt, zult u ontdekken lagere kosten als gevolg van de schaalvoordelen, geoptimaliseerde beveiliging door middel van onderdeel en gegevens stroom centralisering, samen met eenvoudiger bewerkingen, management en compliance-audits.

> [!NOTE]
> Het is belangrijk te begrijpen dat de vDC **niet** een discrete Azure product, maar de combinatie van verschillende functies en mogelijkheden om te voldoen aan de vereisten van uw exacte. vDC is een manier nadenken over uw workloads en het gebruik van Azure om uw resources en de mogelijkheden in de cloud te maximaliseren. De virtuele domeincontroller is daarom een modulaire benadering over het bouwen van IT-services in Azure, inachtneming van organisatie-rollen en verantwoordelijkheden.

De vDC kunt ondernemingen werkbelastingen en toepassingen in Azure ophalen voor de volgende scenario's:

-   Meerdere gerelateerde workloads die als host fungeert
-   Migreren werkbelastingen van een on-premises-omgeving naar Azure
-   Uitvoering van de gedeelde of gecentraliseerde beveiliging en vereisten voor toegang via werkbelastingen
-   De combinatie van DevOps en gecentraliseerd IT op de juiste wijze voor een grote onderneming

De sleutel voor het ontgrendelen van de voordelen van vDC, is een gecentraliseerde topologie (hub en spaken) met een combinatie van Azure-functies: [Azure VNet][VNet], [nsg's][NSG], [VNet-Peering][VNetPeering], [door de gebruiker gedefinieerde Routes (UDR)][UDR], en de identiteit van Azure met [rol Base toegangsbeheer (RBAC)][RBAC].

## <a name="who-needs-a-virtual-data-center"></a>Die behoefte hebben aan een virtuele Datacenter?
Elke Azure klant die meer dan een paar workloads in Azure kan profiteren van het denken over het gebruik van algemene bronnen is verplaatst. Afhankelijk van de magnitude kunnen zelfs enkele toepassingen profiteren van de patronen en onderdelen die worden gebruikt voor het bouwen van een vDC.

Als uw organisatie een gecentraliseerde IT, het netwerk, beveiliging heeft en/of/nalevingsafdeling team, een vDC kunt u beleid punten scheiding van rechten, afdwingen en uniformiteit van de onderliggende common componenten terwijl waarbij toepassing teams zoveel vrijheid en het besturingselement geschikt is voor uw vereisten.

Organisaties die op zoek bent naar DevOps kunnen gebruikmaken van de concepten vDC geautoriseerde zakken van Azure-resources en zorg ervoor dat de totale controle binnen die groep (abonnement of resourcegroep groep in een algemene abonnement) hebben, maar de grenzen van netwerk- en blijven compatibel zoals gedefinieerd door een gecentraliseerde beleid in een hub VNet en resourcegroep.

## <a name="considerations-on-implementing-a-virtual-data-center"></a>Overwegingen bij het implementeren van een virtuele Data Center
Bij het ontwerpen van een vDC, zijn er enkele belangrijke overwegingen:

-   Identiteit en directoryservices
-   Infrastructuur voor de beveiliging
-   De verbinding met de cloud
-   Connectiviteit in de cloud

##### <a name="identity-and-directory-service"></a>*Identiteits- en Active Directory*
Identiteit en Directory services zijn een belangrijk aspect van alle gegevens draait, zowel on-premises en in de cloud. Identiteit is gerelateerd aan alle aspecten van toegang en de machtiging voor services binnen de vDC. Om ervoor te zorgen dat alleen geautoriseerde gebruikers en processen toegang hebben tot uw Azure-Account en resources, Azure verschillende soorten referenties gebruikt voor authenticatie. Het gaat hierbij om wachtwoorden (voor toegang tot het Azure-account), cryptografische sleutels, digitale handtekeningen en certificaten. [*Azure multi-factor Authentication* (MFA)] [ MFA] is een extra beveiligingslaag voor toegang tot Azure-services. Azure MFA biedt geavanceerde verificatie met een bereik van eenvoudige verificatie-opties, telefoongesprek, tekstbericht of mobiele app-melding: en klanten om te kiezen hoe u ze wil toestaan.

Een grote onderneming moet een proces voor het beheer van identiteit die het beheer van afzonderlijke identiteiten, hun verificatie, autorisatie, rollen en bevoegdheden binnen of tussen de vDC beschrijft definiëren. De doelstellingen van dit proces moet de beveiliging en productiviteit verhogen en het verlagen van kosten, uitvaltijd en herhaalde manuele taken.

Werknemers hebben vaak verschillende rollen wanneer betrokken met verschillende projecten en ondernemingen/vereist een veeleisende combinatie van services voor verschillende regel van bedrijven (LOB's). Een vDC vereist goede samenwerking tussen verschillende teams, elk met specifieke roldefinities systemen met met goed bestuur ophalen. De matrix van verantwoordelijkheden, de toegang en de rechten kan zeer complex zijn. Identiteitsbeheer in vDC wordt geïmplementeerd via [ *Azure Active Directory* (AAD)] [ AAD] en op rollen gebaseerde toegangsbeheer (RBAC).

Een adreslijstservice biedt een infrastructuur gedeelde informatie voor het zoeken, beheren, beheren en ordenen van dagelijkse items en netwerkbronnen. Deze resources kunnen volumes, mappen, bestanden, printers, gebruikers, groepen, apparaten en andere objecten bevatten. Elke resource in het netwerk wordt beschouwd als een object door de directoryserver. Informatie over een bron wordt opgeslagen als een verzameling van kenmerken die zijn gekoppeld aan deze resource of object.

Alle Microsoft online Bedrijfsservices vertrouwen op Azure Active Directory (AAD) voor aanmelding en andere identiteit moet. Azure Active Directory is een uitgebreide en zeer beschikbare cloudoplossing voor identiteits- en toegangsbeheer die kernadreslijstservices, geavanceerd identiteitsbeheer en toegangsbeheer voor toepassingen combineert. AAD kan worden geïntegreerd met de lokale Active Directory kunt u eenmalige aanmelding voor alle cloud-gebaseerde en lokaal gehoste (lokale) toepassingen. De kenmerken van de gebruiker van de lokale Active Directory kunnen automatisch worden gesynchroniseerd naar AAD.

Één globale beheerder is niet vereist om alle machtigingen in een vDC te wijzen. In plaats daarvan kan elke bepaalde afdeling (of de groep van gebruikers of services in Active Directory) de vereiste machtigingen voor het beheren van hun eigen resources binnen een vDC hebben. Structureren machtigingen vereist netwerktaakverdeling. Te veel machtigingen kunnen belemmeren prestaties efficiëntie, en te weinig of losse machtigingen beveiligingsrisico's kunnen verhogen. Azure op rollen gebaseerde toegangsbeheer (RBAC) kunt u dit probleem oplossen door het aanbieden van Geavanceerd toegangsbeheer voor vDC-bronnen.

##### <a name="security-infrastructure"></a>*Infrastructuur voor de beveiliging*
Infrastructuur voor de beveiliging, in de context van een vDC hoofdzakelijk is gerelateerd aan de scheiding van verkeer in de vDC specifiek virtuele-netwerksegment en het beheren van inkomende en uitgaande doorloopt de vDC. Azure is gebaseerd op een architectuur met meerdere tenants waarmee wordt voorkomen onbevoegde en onbedoelde verkeer tussen implementaties dat, met behulp van virtueel netwerk (VNet) isolatie toegangsbeheerlijsten (ACL's), load balancers en IP-filters, samen met verkeer stroom beleidsregels. Netwerkadresomzetting (NAT) scheidt interne netwerkverkeer van externe verkeer.

De Azure-infrastructuur wijst er infrastructuurresources aan tenantwerkbelastingen en communicatie van en naar virtuele machines (VM's) beheert. De Azure hypervisor afgedwongen geheugen en proces scheiding tussen VM's en veilig netwerkverkeer routeert Gast OS tenants.

##### <a name="connectivity-to-the-cloud"></a>*De verbinding met de cloud*
De vDC moet verbinding met externe netwerken services aanbieden aan klanten, partners en/of interne gebruikers. Dit betekent meestal dat de verbinding niet alleen met Internet, maar ook met lokale netwerken en gegevenscentra.

Klanten kunnen hun beveiligingsbeleid om te bepalen wat bouwen en hoe specifieke vDC gehoste services toegankelijk is vanaf Internet met behulp van de virtuele netwerkapparaten (met filteren en verkeer inspection) en aangepaste routering beleidsregels en netwerk filteren (gedefinieerd Routering en Netwerkbeveiligingsgroepen) zijn.

Bedrijven moeten vaak verbinding maken met de Copy-VDC's lokale datacenters of andere bronnen. De connectiviteit tussen Azure en on-premises netwerken is daarom een aspect van cruciaal belang bij het ontwerpen van een effectieve architectuur. Bedrijven hebben twee verschillende manieren een koppeling tussen vDC en on-premises maken in Azure: overdracht via het Internet en/of persoonlijke rechtstreekse verbindingen.

Een [ **Azure Site-naar-Site VPN** ] [ VPN] is een onderling verbonden service via Internet tussen on-premises netwerken en de vDC, tot stand gebracht via beveiligde versleutelde verbindingen (IPsec/IKE-tunnels). Azure Site-naar-Site-verbinding is flexibel en snel kan worden gemaakt, en vereist geen eventuele verdere aankopen, net als alle verbindingen via internet verbinding.

[**ExpressRoute** ] [ ExR] is een verbinding met Azure-service waarmee u particuliere verbindingen maken tussen vDC en de on-premises netwerken. ExpressRoute-verbindingen niet via het openbare Internet en bieden betere beveiliging, betrouwbaarheid en hogere snelheden (maximaal 10 Gbps) samen met consistente latentie. ExpressRoute is zeer nuttig is voor Copy-VDC's, als ExpressRoute klanten kunnen de voordelen van compliantieregels die zijn gekoppeld aan particuliere verbindingen krijgen.

ExpressRoute-verbindingen implementeren omvat bezighouden met een ExpressRoute-serviceprovider. Voor klanten die willen snel starten, dit wordt meestal in eerste instantie Site-naar-Site VPN-verbinding tot stand brengen van verbindingen tussen de vDC gebruikt en on-premises resources, en vervolgens migreren naar een ExpressRoute-verbinding.

##### <a name="connectivity-within-the-cloud"></a>*Connectiviteit in de cloud*
[VNets] [ VNet] en [VNet-Peering] [ VNetPeering] de netwerken connectiviteit basisservices binnen een vDC zijn. De grens van een natuurlijke van isolatie voor vDC-resources wordt gegarandeerd dat een VNet en VNet-peering kunt onderling tussen verschillende VNets binnen dezelfde Azure-regio. Beheer van netwerkverkeer binnen een VNet en tussen VNets moet overeenkomen met een set beveiligingsregels opgegeven met de Access Control Lists ([Netwerkbeveiligingsgroep][NSG]), [virtuele netwerkapparaten][NVA], en aangepaste routeringstabellen ([UDR][UDR]).

## <a name="virtual-data-center-overview"></a>Virtuele Data Center-overzicht

### <a name="topology"></a>Topologie
Hub en spaken model uitgebreid het virtuele Data Center binnen één Azure-regio

[![1]][1]

De hub is de centrale zone die u beheert en inspecteert inkomend en/of uitgaande verkeer tussen verschillende zones: Internet, on-premises en de spokes. De hub en spoke-topologie biedt de IT-afdeling een effectieve manier om af te dwingen beveiligingsbeleid op een centrale locatie, de mogelijkheid van onjuiste configuratie en de blootstelling te verlagen.

De hub bevat algemene onderdelen van de service door de spokes verbruikt. Hier volgen enkele typische voorbeelden van algemene centrale services:

-   De Windows Active Directory-infrastructuur (met de verwante ADFS-service) vereist voor de verificatie van derden vanuit niet-vertrouwde netwerken te openen voordat u toegang tot de werkbelastingen in de spoke
-   Een DNS-service om op te lossen naamgeving voor de werkbelasting in de spokes, met toegang tot bronnen on-premises en via Internet
-   Een PKI-infrastructuur voor het implementeren van eenmalige aanmelding op werkbelastingen
-   Datatransportbesturing (TCP/UDP) tussen de spaken en Internet
-   Datatransportbesturing tussen spoke en on-premises
-   Indien gewenst, datatransportbesturing tussen één spoke

Totale kosten beperkt de vDC met behulp van de infrastructuur gedeelde hub tussen meerdere spaken.

De rol van elke spoke kan zijn op verschillende typen werkbelastingen host. De spokes biedt ook een modulaire benadering voor herhaalbare implementaties (bijvoorbeeld, ontwikkeling en tests, gebruikersacceptatietests, preproductie en productie) van de dezelfde werkbelastingen. De spokes kunnen ook worden gebruikt om te scheiden en verschillende groepen binnen uw organisatie (bijvoorbeeld DevOps groepen) inschakelen. Binnen een spoke is het mogelijk om een basic werkbelasting of complexe werkbelastingen van meerdere lagen met beheer van netwerkverkeer tussen de lagen te implementeren.

##### <a name="subscription-limits-and-multiple-hubs"></a>Abonnementen en meerdere hubs
Elk onderdeel, ongeacht het type, wordt in Azure worden geïmplementeerd in een Azure-abonnement. De isolatie van Azure-componenten in verschillende Azure-abonnementen kunt voldoen aan de vereisten van verschillende LOB's, zoals het gedifferentieerde niveaus van toegang en autorisatie in te stellen.

Een enkele vDC kunt schalen naar groot aantal spaken, hoewel net als bij elke IT-systeem, zijn er beperkingen voor platforms gelden. De implementatie van de hub is gebonden aan een specifieke Azure-abonnement, met beperkingen en limieten (bijvoorbeeld een maximum aantal VNet-peerings - Zie [Azure-abonnement en Servicelimieten, quota's en beperkingen] [ Limits] voor meer informatie). In gevallen waar limieten mogelijk een probleem, de architectuur kan opschalen verder omhoog door het model van een enkele hub-spaken uitbreiden naar een cluster van hub en spaken. Meerdere hubs in een of meer Azure-regio's kunnen ook worden verbonden met Express Route of site-naar-site VPN.

[![2]][2]

De introductie van meerdere hubs verhoogt de inspanning kosten en het beheer van het systeem en alleen gerechtvaardigd zijn door schaalbaarheid (voorbeelden: systeemlimieten of redundantie) en regionale replicatie (voorbeelden: eindgebruikers prestaties of herstel na noodgevallen). In scenario's moet vereisen van meerdere hubs, alle hubs streven naar dezelfde set van services voor operationele gemak bieden.

##### <a name="interconnection-between-spokes"></a>Koppeling tussen spaken
Binnen een enkele spoke is het mogelijk voor het implementeren van complexe met meerdere lagen werkbelastingen. Configuraties met meerdere lagen kunnen worden geïmplementeerd met behulp van subnetten (één voor elke laag) in hetzelfde VNet en het filteren van de stromen met nsg's.

Een architect wilt aan de andere kant een werkbelasting met meerdere lagen in meerdere VNets implementeren. Met behulp van VNet-peering, spaken verbinding kunnen maken met andere spaken in dezelfde hub of andere hubs. Een typisch voorbeeld van dit scenario is het geval waarbij de verwerking van de toepassing servers bevinden zich in één spoke (VNet), terwijl de database wordt geïmplementeerd in een andere spoke (VNet). In dit geval is het eenvoudig de spokes met VNet-peering verbinding kunnen maken en daardoor wilt voorkomen dat bij doorvoer over de hub. Een zorgvuldige beoordeling van de architectuur en beveiliging moet worden uitgevoerd om ervoor te zorgen dat het omzeilen van de hub niet overslaan belangrijke beveiligings- of controle-punten dat mag alleen bestaan in de hub.

[![3]][3]

Spaken kunnen ook worden verbonden met een spoke die als een hub fungeert. Deze methode maakt u een hiërarchie met twee niveaus: de spoke in het hogere niveau (niveau 0), worden de hub van lagere spaken (niveau 1) van de hiërarchie. De spokes van vDC moeten het verkeer naar de centrale hub om contact op met de on-premises netwerk of internet te sturen. Een architectuur met twee niveaus van hub introduceert complexe routering die de voordelen van een eenvoudige hub spoke relatie verwijdert.

Hoewel Azure complexe topologieën toestaat, is een van de belangrijkste principes van het concept vDC herhaalbaarheid en eenvoud. Om het gemakkelijker voor beheer, is het ontwerp voor een eenvoudige hub spoke de referentiearchitectuur aanbevolen vDC.

### <a name="components"></a>Onderdelen
Een virtuele Data Center bestaat uit vier elementaire onderdeeltypen: **infrastructuur**, **perimeternetwerken**, **werkbelastingen**, en **bewaking**.

Elk onderdeeltype bestaat uit verschillende Azure-functies en bronnen. Uw vDC bestaat uit exemplaren van typen met meerdere onderdelen en meerdere variaties van hetzelfde componenttype. Mogelijk hebt u bijvoorbeeld veel exemplaren van de werkbelasting afwijken, logisch gescheiden, waarbij verschillende toepassingen. U gebruikt deze typen verschillende onderdelen en exemplaren om uiteindelijk de vDC samen te stellen.

[![4]][4]

De architectuur van de voorgaande op hoog niveau van een vDC bevat verschillende onderdelen die worden gebruikt in verschillende zones van de hub spaken-topologie. Het diagram toont de onderdelen van de infrastructuur in verschillende onderdelen van de architectuur.

Als een goede gewoonte (voor een lokale DC of vDC) toegang moet rechten en bevoegdheden op basis van een groep. Omgaan met groepen, kunnen in plaats van afzonderlijke gebruikers toegangsbeleid consistent van binnen teams en hulpmiddelen in voor het minimaliseren van fouten in de configuratie. Toe te wijzen en verwijderen van gebruikers en naar de juiste groepen kunt de bevoegdheden van een specifieke gebruiker up-to-date te houden.

Elke rolgroep moet een uniek voorvoegsel hebben voor hun namen zodat u gemakkelijk kunt bepalen welke groep is gekoppeld aan welke werkbelasting. Bijvoorbeeld, een werkbelasting die als host fungeert voor een verificatieservice wellicht groepen met de naam *AuthServiceNetOps, AuthServiceSecOps AuthServiceDevOps en AuthServiceInfraOps.* Op dezelfde manier geldt voor gecentraliseerde rollen of functies niet gerelateerd aan een bepaalde service, kan worden voorafgegaan door "Corp" *CorpNetOps* bijvoorbeeld.

Een variant van de volgende groepen veel organisaties gebruiken om een grote verdeling van rollen:

-   De *centrale IT-groep (Corp)* de eigendom rechten voor het beheren van onderdelen van de infrastructuur (zoals netwerk- en beveiliging), en daarom moet de rol van inzender hebben voor het abonnement (en hebben de controle van de hub) en Inzender-rechten in de spokes netwerk. Deze verantwoordelijkheid management tussen meerdere teams zoals; grote organisatie vaak opsplitsen een groep netwerkbewerkingen (CorpNetOps) (met exclusieve toegang is geactiveerd) en een groep beveiligingsbewerkingen (CorpSecOps) (die verantwoordelijk is voor de firewall-en beveiligingsbeleid). In dit specifieke geval moeten twee verschillende groepen worden gemaakt voor de toewijzing van deze aangepaste rollen.
-   De *dev & testgroep (AppDevOps)* is verantwoordelijk voor het implementeren van werkbelastingen (Apps of Services). Deze groep heeft de rol van inzender van de virtuele Machine voor IaaS-implementaties en/of een of meer PaaS-Inzender rollen (Zie [ingebouwde functies voor op rollen gebaseerd toegangsbeheer][Roles]). Het team dev & testen wilt eventueel zichtbaarheid op beveiligingsbeleid (nsg's) en routeringsbeleid (UDR) in de hub of een specifieke spoke hebben. Daarom naast de rol van inzender voor werkbelastingen moet deze groep ook de rol van netwerk-lezer.
-   De *bewerking en onderhoud groep (CorpInfraOps of AppInfraOps)* verantwoordelijk zijn voor het beheren van werkbelastingen in productie. Deze groep moet een abonnement Inzender voor werkbelastingen in een productie-abonnementen. Sommige organisaties mogelijk ook evalueren of ze een extra escalatie team ondersteuningsgroep met de rol van Inzender abonnement in productie en in het centrale hub-abonnement nodig om het oplossen van configuratieproblemen met de in de productieomgeving.

Er is een vDC gestructureerd zodat groepen die zijn gemaakt voor de centrale IT-groepen beheren van de hub bijbehorende groepen op het niveau van de werkbelasting hebben. Naast het beheren van resources hub is alleen de centrale IT-groepen in staat om externe toegang en op het hoogste niveau machtigingen voor het abonnement te beheren. Werkbelastinggroepen zou kunnen beheren van resources en de machtigingen van hun VNet onafhankelijk op centrale IT-afdeling.

De vDC moet worden gepartitioneerd als host voor meerdere projecten veilig over verschillende regel van bedrijven (LOB's). Alle projecten vereist verschillende geïsoleerde omgevingen (Dev, UAT, productie). Afzonderlijke Azure-abonnementen voor elk van deze omgevingen bieden natuurlijke isolatie.

[![5]][5]

Het voorgaande diagram toont de relatie tussen een organisatie projecten, gebruikers, groepen en de omgevingen waarin de Azure-onderdelen zijn geïmplementeerd.

Normaal gesproken in IT, een omgeving (of laag) is een systeem waarin meerdere toepassingen worden geïmplementeerd en uitgevoerd. Grote ondernemingen gebruiken een ontwikkelomgeving (waar wijzigingen oorspronkelijk en getest) en een productie-omgeving (wat eindgebruikers gebruiken). Deze omgevingen worden gescheiden, vaak met verschillende faseringsomgevingen tussen deze gefaseerde implementatie (implementatie) toestaan, testen en terugdraaien in geval van problemen. Implementatie-architecturen aanzienlijk verschillen, maar meestal het basisproces voor op ontwikkeling (DEV) begint en eindigt bij productie (PROD) nog steeds wordt gevolgd.

Een algemene architectuur voor deze typen omgevingen met meerdere lagen bestaat uit DevOps (ontwikkelen en testen), UAT (fasering) en productieomgevingen. Organisaties kunnen gebruikmaken van één of meerdere Azure AD-tenants voor het definiëren van toegang en machtigingen voor deze omgevingen. Het vorige diagram ziet u een aanvraag wanneer twee andere Azure AD-tenants worden gebruikt: één voor DevOps en UAT en andere zijn uitsluitend bedoeld is voor productie.

De aanwezigheid van andere Azure AD-tenants dwingt de scheiding tussen omgevingen. Dezelfde groep gebruikers (als u bijvoorbeeld centrale IT-afdeling) moet worden geverifieerd met een andere URI voor toegang tot een andere AD-tenant wijzigen de rollen of machtigingen van de DevOps- of productie-omgeving van een project. De aanwezigheid van andere gebruikersverificatie voor toegang tot verschillende omgevingen vermindert mogelijke storingen en andere problemen veroorzaakt door menselijke fouten.

#### <a name="component-type-infrastructure"></a>Onderdeeltype: infrastructuur
Dit onderdeel is waar de meeste van de ondersteunende infrastructuur zich bevindt. Het is ook waar uw centrale IT, beveiliging en/of naleving teams besteden aan de meeste van de tijd.

[![6]][6]

Onderdelen van de infrastructuur bieden een koppeling tussen de verschillende onderdelen van een vDC en aanwezig zijn in zowel de hub en de spokes. De verantwoordelijkheid voor het beheren en onderhouden van de onderdelen van de infrastructuur wordt meestal toegewezen aan de centrale IT en/of beveiligingsteam.

Een van de primaire taken van het team voor IT-infrastructuur is om te waarborgen van de consistentie van IP-adres schema's in het hele bedrijf. De privé-IP-adresruimte toegewezen aan de behoeften vDC consistent en niet overlappen met particuliere IP-adressen toegewezen op uw on-premises netwerken.

Terwijl NAT op de lokale randrouters of in Azure-omgevingen IP-adresconflicten te voorkomen kan, wordt problemen toegevoegd aan de onderdelen van uw infrastructuur. Eenvoud van management is een van de belangrijkste doelstellingen van vDC, zodat u met behulp van de NAT voor het afhandelen van IP-problemen is geen aanbevolen oplossing.

Onderdelen van de infrastructuur bevatten de volgende functionaliteit:

-   [**Identiteits-en**][AAD]. Toegang tot elke brontype in Azure wordt beheerd door een identiteit die is opgeslagen in Active Directory. De directoryservice slaat niet alleen de lijst met gebruikers, maar ook de toegangsrechten tot bronnen in een specifieke Azure-abonnement. Deze services alleen in de cloud kunnen bestaan of ze kunnen worden gesynchroniseerd met de on-premises identiteits opgeslagen in Active Directory.
-   [**Virtueel netwerk**][VPN]. Virtuele netwerken zijn een van de belangrijkste onderdelen van een vDC en kunnen u een grens van de isolatie van verkeer op de Azure-platform maken. Een virtueel netwerk bestaat uit één of meerdere virtuele netwerksegmenten, elk met een bepaald IP-netwerk voorvoegsel (subnet). Het virtuele netwerk definieert een interne perimeter-gebied waar IaaS virtuele machines en PaaS-services besloten communicatie kunnen maken. Virtuele machines (en PaaS-services) in een virtueel netwerk kan niet rechtstreeks met virtuele machines communiceren (en PaaS-services) in een ander virtueel netwerk, zelfs als beide virtuele netwerken van dezelfde klant onder hetzelfde abonnement worden gemaakt. Isolatie is een kritieke eigenschap die ervoor zorgt dat de klant VM's en communicatie blijft persoonlijke binnen een virtueel netwerk.
-   [**UDR**][UDR]. Verkeer in een virtueel netwerk wordt doorgestuurd op basis van de routeringstabel system standaard. Een Route voor het definiëren van de gebruiker is een aangepaste routetabel netwerkbeheerders kunt koppelen aan een of meer subnetten overschrijven het gedrag van de routeringstabel van systeem en het definiëren van een communicatiepad binnen een virtueel netwerk. De aanwezigheid van udr's zorgt ervoor dat uitgaande verkeer van de doorvoer spoke door specifieke aangepaste VM's en/of virtuele netwerkapparaten en netwerktaakverdelers aanwezig in de hub en in de spokes.
-   [**NSG**][NSG]. Een Netwerkbeveiligingsgroep is een lijst met beveiligingsregels voor verbindingen die fungeren als verkeer filteren op IP-bronnen, doel-IP-protocollen, bron-IP-poorten en doel-IP-poorten. Het NSG kan worden toegepast op een subnet, een virtuele NIC-kaart die is gekoppeld aan een Azure VM of beide. Het nsg's zijn essentieel voor een juiste datatransportbesturing implementeren in de hub en in de spokes. Het niveau van beveiliging die worden geboden door de NSG is een functie van welke poorten u opent, en wat is het doel. Klanten wordt aangeraden extra VM filters met een host gebaseerde firewall zoals IPtables of de Windows Firewall.
-   **DNS**. De naamomzetting van resources in de VNets van een vDC is beschikbaar via DNS. Het bereik van naamomzetting van de standaard DNS is beperkt tot het VNet. Normaal gesproken een aangepaste DNS-service moet worden geïmplementeerd in de hub als onderdeel van algemene services, maar de belangrijkste consumenten van DNS-services zich in de spoke bevinden. Indien nodig, kunnen klanten een hiërarchische structuur voor DNS-kunnen maken met de overdracht van DNS-zones naar de spokes.
-   [** Abonnement] [ SubMgmt] en [resourcegroep Management][RGMgmt]**. Een abonnement definieert een natuurlijke grens voor het maken van meerdere groepen van resources in Azure. Resources in een abonnement worden samen in logische containers met de naam resourcegroepen worden samengesteld. De resourcegroep vertegenwoordigt een logische groep om de resources van een vDC te organiseren.
-   [**RBAC**][RBAC]. Via RBAC is het mogelijk te kaart organisatierol samen met de rechten voor toegang tot specifieke Azure-resources, zodat u kunt gebruikers beperken tot alleen een bepaalde subset van acties. Met RBAC, kunt u de juiste rol toewijzen aan gebruikers, groepen en toepassingen binnen het bereik van de relevante toegang verlenen. Het bereik van een roltoewijzing kan dit een Azure-abonnement, resourcegroep of één resource. RBAC kunt overname van machtigingen. Een rol die is toegewezen aan een bovenliggend bereik verleent toegang ook naar de onderliggende elementen erin opgenomen. Met RBAC kunt u taken scheiden en de hoeveelheid toegang verlenen aan gebruikers die ze nodig hebben voor het uitvoeren van hun taken. Bijvoorbeeld, gebruikmaken van RBAC te laten een werknemer virtuele machines in een abonnement beheren terwijl een andere SQL-databases binnen hetzelfde abonnement kunt beheren.
-   [**VNet-Peering**][VNetPeering]. De fundamentele functie gebruikt voor het maken van de infrastructuur van een vDC is VNet-Peering, een mechanisme dat verbinding twee virtuele netwerken (vnet's) in dezelfde regio via het netwerk van het Azure Datacenter maakt.

#### <a name="component-type-perimeter-networks"></a>Onderdeeltype: Perimeternetwerken
[Perimeternetwerk] [ DMZ] componenten (ook wel een DMZ-netwerk) kunt u de netwerkverbinding voorzien van uw on-premises of fysieke datacenternetwerken, samen met eventuele connectiviteit en naar Internet. Het is ook waar uw netwerk- en teams waarschijnlijk te besteden aan de meeste van de tijd.

Inkomende pakketten moeten stromen via de beveiligingsapparaten in de hub, zoals de firewall-id's en IP-Adressen, voordat de back-end-servers in de spokes is bereikt. Internet-gebonden pakketten van de werkbelastingen moeten ook door de beveiligingsapparaten in het perimeternetwerk is bedoeld voor het afdwingen van beleid, controle en controledoeleinden, stromen voordat deze uit het netwerk.

Perimeter-netwerkonderdelen bieden de volgende functies:

-   [Virtuele netwerken][VNet], [UDR][UDR], [NSG][NSG]
-   [Virtueel netwerkapparaat][NVA]
-   [De Load Balancer][ALB]
-   [Toepassingsgateway][AppGW] / [WAF][WAF]
-   [Openbare IP-adressen][PIP]

Normaal gesproken de centrale IT en beveiliging teams verantwoordelijkheid voor de Vereistedefinitie en bewerkingen van de perimeternetwerken hebben.

[![7]][7]

Het voorgaande diagram toont het afdwingen van twee verbindingen met toegang tot het internet en een on-premises netwerk zowel residente in de hub. Het perimeternetwerk met internet kunt in een enkel hub opschalen ter ondersteuning van grote aantallen LOB's, met behulp van meerdere farms van Web Application Firewalls (WAFs) en/of firewalls.

[**Virtuele netwerken** ] [ VNet] de hub wordt gewoonlijk samengesteld op een VNet met meerdere subnetten voor het hosten van de verschillende typen van services voor het filteren en verkeer naar of van het internet via NVAs WAFs en Azure Toepassingsgateways te bekijken.

[**UDR** ] [ UDR] UDR met behulp van klanten kunnen implementeren firewalls, -id's / IP-Adressen en andere virtuele apparaten en doorsturen van netwerkverkeer via deze beveiligingsapparaten voor beveiliging grens wordt afgedwongen, controle en controle. Udr's kunnen worden gemaakt in de hub en de spokes te waarborgen dat verkeer passages via de specifieke aangepaste virtuele machines, virtuele netwerkapparaten en gebruikt door de vDC netwerktaakverdelers. Om ervoor te zorgen dat verkeer naar de juiste virtuele apparaten gegenereerd op basis van virtuele machines in de doorvoer spoke woont, moet een UDR worden ingesteld in de subnetten van het spoke door het instellen van de front-end-IP-adres van de interne load balancer als de volgende hop. De interne load balancer distribueert het interne verkeer naar de virtuele apparaten (groep load balancer back-end).

[![8]][8]

[**Virtuele apparaten** ] [ NVA] In de hub normaal gesproken het perimeternetwerk met toegang tot het internet wordt beheerd via een farm met firewalls en/of Web Application Firewalls (WAFs).

Andere LOB's gebruiken doorgaans veel webtoepassingen en deze toepassingen vaak last van verschillende beveiligingsproblemen en potentiële aanvallen. Web Applications Firewalls zijn een speciale vorm van het product gebruikt voor het detecteren van aanvallen tegen de webtoepassingen (HTTP/HTTPS) in de diepte van meer dan een algemene firewall. Vergeleken met traditie firewall-technologie, hebben WAFs een set van specifieke functies interne webservers te beschermen tegen bedreigingen.

Een firewall-farm is groep firewalls werkt in combinatie onder dezelfde algemene beheer, met een reeks regels voor het beveiligen van de werkbelastingen die worden gehost in de spokes en toegang tot on-premises netwerken. Een firewall-farm heeft minder gespecialiseerde software vergeleken met een WAF, maar heeft een brede toepassingsbereik om te filteren en inspecteren van elk type verkeer en uitgangsclaims. Firewall-farms worden normaal gesproken geïmplementeerd in Azure met behulp van de virtuele netwerkapparaten (NVAs) die beschikbaar in de Azure marketplace zijn.

Het is raadzaam om een reeks NVAs gebruiken voor verkeer dat afkomstig is op het Internet, en één voor verkeer van oorsprong on-premises. Met behulp van slechts één set met NVAs voor beide vormt een beveiligingsrisico, aangezien deze geen beveiliging perimeter tussen de twee sets van netwerkverkeer biedt. Met afzonderlijke NVAs vermindert de complexiteit van het controleren van de beveiligingsregels voor verbindingen en is dit duidelijk welke regels overeenkomen met welke inkomende aanvraag voor het netwerk.

Meest grote ondernemingen beheren meerdere domeinen. Azure DNS kan worden gebruikt voor het hosten van de DNS-records voor een bepaald domein. Als voorbeeld, kunnen virtuele IP-adres (VIP) van de Azure externe load balancer (of de WAFs) worden geregistreerd in de A-record van een Azure DNS-record.

[**Azure Load Balancer** ] [ ALB] Azure load balancer biedt een hoge beschikbaarheid laag 4 (TCP, UDP)-service, die binnenkomend verkeer over service-exemplaren die zijn gedefinieerd in een set met gelijke taakverdeling kunt distribueren. Verkeer dat wordt verzonden naar de load balancer van front-eindpunten (openbare IP-eindpunten of persoonlijke IP-eindpunten) kan worden gedistribueerd met of zonder NAT op een reeks backend-IP-adresgroep (voorbeelden wordt; Virtuele netwerkapparaten of VM's).

Azure Load Balancer kan de status van de verschillende exemplaren van server ook probe en wanneer een test niet reageert niet meer de load balancer verkeer kunnen verzenden naar het exemplaar niet in orde. In een vDC hebben we de aanwezigheid van een externe load balancer in de hub (bijvoorbeeld verdelen het verkeer naar NVAs) en in de spokes (taken uit te voeren zoals het verkeer tussen verschillende virtuele machines van een toepassing met meerdere lagen balancing).

[**Toepassingsgateway** ] [ AppGW] Microsoft Azure Application Gateway is een toegewezen virtueel apparaat toepassing levering controller (ADC) bieden als een service biedt u verschillende laag 7 taakverdeling mogelijkheden voor uw toepassing. Hiermee kunt u web-farm productiviteit optimaliseren door het offloaden van CPU intensief SSL-beëindiging aan de toepassingsgateway. Het bevat ook andere routeringsmogelijkheden voor laag 7, met inbegrip van round robin-distributie van inkomend verkeer, cookies op basis van sessieaffiniteit, routering van URL's op padbasis en de mogelijkheid voor het hosten van meerdere websites achter één toepassingsgateway. Een WAF (Web Application Firewall) is ook beschikbaar als onderdeel van de WAF SKU van de toepassingsgateway. De SKU biedt beveiliging voor webtoepassingen van algemene web beveiligingsproblemen en aanvallen. Application Gateway kan worden geconfigureerd als op internet gerichte gateway, interne enige gateway of een combinatie van beide. 

[**Openbare IP-adressen** ] [ PIP] sommige Azure-functies kunt u service-eindpunten naar een openbare IP-adres waarmee de bron worden geopend vanaf het internet te koppelen. Dit eindpunt maakt gebruik van NAT (Network Address Translation) voor het routeren van verkeer naar de interne adres en poort op de virtuele Azure-netwerk. Dit pad is de belangrijkste manier voor het externe verkeer door te geven in het virtuele netwerk. Het openbare IP-adressen kunnen worden geconfigureerd om te bepalen welk verkeer is doorgegeven en hoe en waar deze wordt omgezet naar het virtuele netwerk.

#### <a name="component-type-monitoring"></a>Onderdeeltype: bewaking
Bewaking componenten bieden zichtbaarheid en waarschuwingen van alle andere onderdelen typen. Alle teams moeten toegang hebben tot bewaking voor de onderdelen en services die toegang hebben tot. Als u een gecentraliseerde help helpdesk of de operations-teams hebt, moet ze toegang tot de gegevens van deze onderdelen hebt geïntegreerd.

Azure biedt verschillende soorten logboekregistratie en controle van services voor het bijhouden van het gedrag van Azure gehoste bronnen. Beheer en controle van workloads in Azure is gebaseerde logboekgegevens niet alleen op het verzamelen, maar ook de mogelijkheid acties starten op basis van specifieke gemelde gebeurtenissen.

Er zijn twee belangrijke gebeurtenistypen Logboeken in Azure:

-   [**Activiteitenlogboeken** ] [ ActLog] (ook als 'Operationele logboek' genoemd) bieden inzicht in de bewerkingen die zijn uitgevoerd op resources in de Azure-abonnement. Deze logboeken rapporteert de besturingselement-vlak-gebeurtenissen voor uw abonnementen. Elke Azure-resource produceert controlelogboeken.

-   [**Azure diagnostische logboeken** ] [ DiagLog] logboeken die worden gegenereerd door een resource die uitgebreide, regelmatig gegevens over de werking van die bron leveren. De inhoud van deze logboeken varieert per resourcetype.

[![9]][9]

In een vDC is het zeer belangrijk voor het bijhouden van de logboeken van de nsg's, met name deze informatie:

-   [**Gebeurtenislogboeken**][NSGLog]: bevat informatie over welke NSG-regels worden toegepast op virtuele machines en de exemplaar-functies op basis van MAC-adres.
-   [**Logboeken teller**][NSGLog]: houdt het aantal keren dat elke NSG-regel is uitgevoerd om te weigeren of verkeer toestaan.

Alle logboeken kunnen worden opgeslagen in Azure Storage-Accounts voor controle, statische analyses of back-updoeleinden. Wanneer u de logboeken worden opgeslagen in Azure storage-account, kunnen klanten verschillende soorten frameworks gebruiken als u wilt ophalen, voorbereiding, analyseren en visualiseren van deze gegevens om te rapporteren van de status en gezondheid van cloudresources.

Grote ondernemingen moeten al een standaard framework voor het bewaken van on-premises systemen hebt aangeschaft, en dat framework voor het integreren van logboeken die worden gegenereerd door cloudimplementaties kunnen uitbreiden. Voor organisaties die u wilt behouden van de logboekregistratie in de cloud, [Microsoft Operations Management Suite (OMS)] [ OMS] is een uitstekende keuze. Aangezien OMS wordt geïmplementeerd als een cloudservice, kunt u er al snel mee aan de slag, en dat met minimale investeringen in infrastructuurservices. OMS kan ook worden geïntegreerd met System Center-onderdelen, zoals System Center Operations Manager uit te breiden van uw bestaande investeringen management in de cloud.

OMS Log analytics is een onderdeel van de OMS-framework voor het verzamelen, correleren, zoeken en reageren op logboek en prestaties gegevens die zijn gegenereerd door de besturingssystemen, toepassingen, onderdelen van cloud-infrastructuur. Dit biedt klanten realtime operationeel inzicht geïntegreerd zoeken en aangepaste dashboards gebruiken voor het analyseren van alle records tussen uw werkbelastingen in een vDC.

#### <a name="component-type-workloads"></a>Onderdeeltype: werkbelastingen
Onderdelen van de werkbelasting zijn waarin de werkelijke toepassingen en services zich bevinden. Het is ook waar ontwikkelteams toepassing te besteden aan de meeste van de tijd.

De mogelijkheden van de werkbelasting zijn volledig eindeloze. Hier volgen enkele van de typen werkbelasting mogelijk:

**Interne LOB-toepassingen**

Line-of-business-toepassingen zijn computertoepassingen die cruciaal zijn voor de actieve bewerking van een onderneming. LOB-toepassingen hebben een aantal gemeenschappelijke kenmerken:

-   **Interactieve**. LOB-toepassingen, zijn interactief aard: gegevens worden ingevoerd en resultaat/rapporten worden geretourneerd.
-   **Gegevensgestuurde**. LOB-toepassingen zijn intensieve met regelmatige toegang tot de databases of andere opslag van gegevens.
-   **Geïntegreerde**. LOB-toepassingen aanbieding integratie met andere systemen binnen of buiten de organisatie.

**Klantgerichte websites (Internet of een intern facing)** de meeste toepassingen die interactie met Internet hebben zijn websites. Azure biedt de mogelijkheid voor het uitvoeren van een website op een IaaS-VM of vanaf een [Azure Web Apps] [ WebApps] site (PaaS). Azure Web Apps ondersteuning voor integratie met VNets die de implementatie van de Web-Apps in de spoke van een vDC toestaan. Met de integratie van VNET, u hoeft niet te blootstellen van een Internet-eindpunt voor uw toepassingen, maar kunt in plaats daarvan het resources persoonlijke niet-routeerbaar internetadres van uw persoonlijke VNet gebruiken.

**BIG gegevensanalyse** wanneer gegevens moeten worden uitgebreid naar een groot volume, databases kunnen niet omhoog schalen goed. Hadoop-technologie biedt een systeem in gedistribueerde query's parallel worden uitgevoerd op een groot aantal knooppunten. Klanten hebben de mogelijkheid om uit te voeren data-workloads in IaaS VM's of PaaS ([HDInsight][HDI]). HDInsight ondersteunt implementeren in een VNet op basis van locatie, kunnen worden geïmplementeerd op een cluster in een spoke van de vDC.

**Gebeurtenissen en berichten**
[Azure Event Hubs] [ EventHubs] is een verwerkingsservice voor grote hoeveelheden telemetrie opname-service die worden verzameld, transformeert en miljoenen gebeurtenissen worden opgeslagen. Als een gedistribueerde streaming platform biedt lage latentie en een configureerbare tijd bewaren, zodat u kunt voor het opnemen van grote hoeveelheden telemetrie in Azure en die gegevens lezen uit meerdere toepassingen. Met Event Hubs, kan één stream realtime en op basis van batch pijplijnen ondersteunen.

Een uiterst betrouwbare cloud messaging-service tussen toepassingen en services, kunnen worden geïmplementeerd via [Azure Service Bus] [ ServiceBus] dat aanbiedingen asynchrone brokered messaging tussen client en server, samen met gestructureerd first in first out (FIFO) messaging en publiceren/abonneren mogelijkheden.

[![10]][10]

### <a name="multiple-vdc"></a>Meerdere vDC
In dit artikel is tot nu toe gericht op een enkele vDC, met een beschrijving van de basisonderdelen en architectuur bijdragen aan een robuuste vDC. Azure-functies zoals Azure load balancer, NVAs, beschikbaarheidssets-schaalsets, samen met andere mechanismen bijdragen aan een systeem waarmee u kunt effen SLA niveaus inbouwen in uw productie-services.

Echter een enkel vDC wordt gehost binnen één regio en is kwetsbaar voor belangrijke storing die invloed kan zijn op de hele regio. Klanten die u wilt bereiken hoge Sla's moeten de services via implementaties van hetzelfde project in twee (of meer) Copy-VDC's, geplaatst in verschillende regio's te beveiligen.

Naast SLA weergegeven zijn er enkele algemene scenario's waarin het implementeren van meerdere Copy-VDC's zinvol:

-   Regionale/Global aanwezigheid
-   Herstel na noodgevallen
-   Methode voor het omleiden van verkeer tussen DC

#### <a name="regionalglobal-presence"></a>Regionale/Global aanwezigheid
Azure-datacenters zijn aanwezig in verschillende regio's over de hele wereld. Als u meerdere Azure-datacenters, klanten moeten rekening houden met twee factoren gerelateerde: geografische afstanden en latentie. Klanten moeten de geografische afstand tussen de Copy-VDC's en de afstand tussen de vDC en de eindgebruikers te bieden de beste gebruikerservaring te evalueren.

De Azure-regio waar Copy-VDC's worden gehost, moet ook om te voldoen aan wettelijke voorschriften van eventuele rechtsgebied, waaronder uw organisatie werkt.

#### <a name="disaster-recovery"></a>Herstel na noodgevallen
De uitvoering van een noodherstelplan sterk betrekking heeft op het type werkbelasting betrokken, en de mogelijkheid om te synchroniseren van de status van de werkbelasting tussen verschillende Copy-VDC's. In het ideale geval wilt de meeste klanten toepassingsgegevens synchroniseren tussen implementaties die worden uitgevoerd in twee verschillende Copy-VDC's voor het implementeren van een snelle failover-mechanisme. De meeste toepassingen zijn gevoelig is voor latentie en die kan ertoe leiden dat mogelijke time-outs en vertraging in de gegevenssynchronisatie.

Synchronisatie of heartbeat-bewaking van toepassingen in andere Copy-VDC's moet communicatie ertussen. Twee Copy-VDC's in verschillende regio's kunnen worden verbonden via:

-   ExpressRoute persoonlijke peering wanneer de vDC-hubs zijn verbonden met hetzelfde ExpressRoute-circuit
-   meerdere ExpressRoute-circuits die zijn verbonden via uw bedrijfsnetwerken en uw mesh vDC verbonden met de ExpressRoute-circuits
-   Site-naar-Site VPN-verbindingen tussen uw vDC-hubs in elke Azure-regio

De ExpressRoute-verbinding is meestal de beste manier vanwege de hogere bandbreedte en consistente latentie wanneer bij doorvoer over de Microsoft-backbone.

Er is geen magische recept valideren van een toepassing die zijn gedistribueerd tussen twee (of meer) andere Copy-VDC's zich in verschillende regio's. Klanten moeten kwalificatie netwerktests om te controleren of de latentie en bandbreedte van de verbindingen en de doelserver, of synchrone of asynchrone gegevensreplicatie geschikt is en welke de optimale beoogde hersteltijd (RTO) voor uw werkbelastingen kan worden uitgevoerd.

#### <a name="mechanism-to-divert-traffic-between-dc"></a>Methode voor het omleiden van verkeer tussen DC
Een effectieve techniek om te leiden van het binnenkomende verkeer in één domeincontroller is gebaseerd op DNS. [Azure Traffic Manager] [ TM] maakt gebruik van het mechanisme System DNS (Domain Name) voor de eindgebruiker verkeer omleiden naar de meest geschikte openbaar eindpunt in een specifieke vDC. Via tests Traffic Manager controleert regelmatig de servicestatus van openbare eindpunten in verschillende Copy-VDC's en in geval van storing van deze eindpunten deze stuurt automatisch naar de secundaire vDC.

Traffic Manager werkt op Azure openbare eindpunten en kunnen worden gebruikt, bijvoorbeeld, moeten het verkeer naar Azure VM's en Web-Apps in de juiste vDC besturingselement/leiden. Traffic Manager is tegen zelfs met betrekking tot het mislukken van een volledige Azure-regio en de distributie van gebruikersverkeer voor service-eindpunten in verschillende Copy-VDC's op basis van verschillende criteria (bijvoorbeeld: fout van een service in een specifieke vDC of de vDC selecteren met de laagste netwerklatentie voor de client) kan beheren.

### <a name="conclusion"></a>Conclusie
Het virtuele Data Center is een methode om data center migreren naar de cloud die gebruikmaakt van een combinatie van functies en mogelijkheden voor het maken van een schaalbare architectuur in Azure die gebruik van cloud-bronnen, maximaliseert de kosten te verlagen en system governance te vereenvoudigen. Het concept vDC is gebaseerd op een hub spaken-topologie opgeeft algemene gedeelde services in de hub en specifieke toepassingen/werkbelastingen in de spokes toestaan. Een vDC komt overeen met de structuur van het bedrijf rollen, waarbij verschillende afdelingen (centrale IT-afdeling, DevOps, bewerking en onderhoud) samen, elk met een specifieke lijst met functies en rechten werken. Een vDC voldoet aan de vereisten voor de migratie van een 'Lift-en Shift', maar ook biedt veel voordelen voor systeemeigen cloudimplementaties.

## <a name="references"></a>Verwijzingen
De volgende functies zijn in dit document besproken. Klik op de koppelingen voor meer informatie.

| | | |
|-|-|-|
|Network-functies|Taakverdeling|Connectiviteit|
|[Virtuele netwerken in Azure][VNet]</br>[Netwerkbeveiligingsgroepen][NSG]</br>[NSG-Logboeken][NSGLog]</br>[De gebruiker gedefinieerde routering][UDR]</br>[Virtuele netwerkapparaten][NVA]</br>[Openbare IP-adressen][PIP]|[Azure Load Balancer (N3)][ALB]</br>[Toepassingsgateway (N7)][AppGW]</br>[Web Application Firewall][WAF]</br>[Met Azure Traffic Manager][TM] |[VNet-Peering][VNetPeering]</br>[Virtueel particulier netwerk][VPN]</br>[ExpressRoute][ExR]
|Identiteit</br>|Bewaking</br>|Beste praktijken</br>|
|[Azure Active Directory][AAD]</br>[Multi-factor Authentication][MFA]</br>[Role Base Access besturingselementen][RBAC]</br>[Standaardrollen AAD][Roles] |[Activiteitenlogboeken][ActLog]</br>[Diagnostische logboeken][DiagLog]</br>[Microsoft Operations Management Suite][OMS]</br> |[Aanbevolen procedures voor perimeter-netwerken][DMZ]</br>[Beheer van abonnementen][SubMgmt]</br>[Beheer van resourcegroep][RGMgmt]</br>[Limieten voor Azure-abonnement][Limits] |
|Andere Azure-Services|
|[Azure-Web-Apps][WebApps]</br>[HDInsights (Hadoop)][HDI]</br>[Event Hubs][EventHubs]</br>[Service Bus][ServiceBus]|



## <a name="next-steps"></a>Volgende stappen
 - Verken [VNet-Peering][VNetPeering], de basis-technologie voor vDC-hub en spoke-ontwerpen
 - Implementeer [AAD] [ AAD] aan de slag met [RBAC] [ RBAC] exploratie
 - Een abonnement en de Resource management-model te ontwikkelen en RBAC model om te voldoen aan de structuur, vereisten en het beleid van uw organisatie. De belangrijkste activiteit is de planning. Plan zo veel mogelijk reorganisaties, fusies, nieuwe regels van het product, enzovoort.

<!--Image References-->
[0]: ./media/networking-virtual-datacenter/redundant-equipment.png "voorbeelden van onderdeel overlappen" 
[1]: ./media/networking-virtual-datacenter/vdc-high-level.png "op hoog niveau voorbeeld van een hub en spoke vDC"
[2]: ./media/networking-virtual-datacenter/hub-spokes-cluster.png "cluster met hubs en spaken"
[3]: ./media/networking-virtual-datacenter/spoke-to-spoke.png "spoke-spoke-"
[4]: ./media/networking-virtual-datacenter/vdc-block-level-diagram.png "blok niveau diagram van de vDC"
[5]: ./media/networking-virtual-datacenter/users-groups-subsciptions.png "gebruikers, groepen, abonnementen en projecten"
[6]: ./media/networking-virtual-datacenter/infrastructure-high-level.png "op hoog niveau infrastructuurdiagram"
[7]: ./media/networking-virtual-datacenter/highlevel-perimeter-networks.png "op hoog niveau infrastructuurdiagram"
[8]: ./media/networking-virtual-datacenter/vnet-peering-perimeter-neworks.png "VNet-Peering en perimeter netwerken"
[9]: ./media/networking-virtual-datacenter/high-level-diagram-monitoring.png "op hoog niveau diagram voor bewaking"
[10]: ./media/networking-virtual-datacenter/high-level-workloads.png "conceptdiagram op voor de werkbelasting"

<!--Link References-->
[Limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits
[Roles]: https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles
[VNet]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview
[NSG]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg 
[VNetPeering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview 
[UDR]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview 
[RBAC]: https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is
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
[ActLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs 
[DiagLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs
[NSGLog]: https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log
[OMS]: https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview
[WebApps]: https://docs.microsoft.com/azure/app-service/
[HDI]: https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-introduction
[EventHubs]: https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs 
[ServiceBus]: https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview
[TM]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview
