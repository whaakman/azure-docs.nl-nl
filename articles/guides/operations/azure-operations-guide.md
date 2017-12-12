---
title: Instructiehandleiding voor Azure IT-operators | Microsoft Docs
description: Korte handleiding voor Azure IT-operators
services: 
documentationcenter: 
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.devlang: 
ms.topic: 
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 06/12/2017
ms.author: nepeters
ms.openlocfilehash: 4a913e188dd40b0306be375b016b9e8a3739ed72
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="introduction-to-cloud-computing-and-microsoft-azure"></a>Inleiding tot cloudcomputing en Microsoft Azure

Deze handleiding bevat de belangrijkste concepten die betrekking hebben op de implementatie en beheer van een Microsoft Azure-infrastructuur. Als u niet bekend bent met cloud computing of Azure zelf, deze handleiding helpt u op weg met concepten, implementatie en beheer details snel ophalen. Veel secties van deze handleiding Bespreek een bewerking, zoals het implementeren van een virtuele machine en geef vervolgens een koppeling voor gedetailleerde technische details.


## <a name="cloud-computing-overview"></a>Overzicht van cloudcomputing

Cloud computing biedt een moderne alternatief voor het traditionele on-premises datacenter. Openbare cloud leveranciers leveren en beheren van alle IT-infrastructuur en de onderliggende software voor beheer. Deze leveranciers bieden een groot aantal cloudservices. Een cloudservice in dit geval mogelijk een virtuele machine, een webserver of cloud-gebaseerde database-engine. Als een klant van de provider cloud lease u deze cloudservices op een als dat nodig is. Zo kunt u de investering kosten voor onderhoud van hardware converteren naar een operationele kosten. Een cloudservice biedt ook de volgende voordelen:

-   Snelle implementatie van grote berekeningsomgevingen

-   Snelle toewijzing is opgeheven van systemen die niet langer vereist zijn

-   Eenvoudige implementatie van oudsher complexe systemen, zoals taakverdelers

-   Mogelijkheid om flexibele rekencapaciteit of schalen wanneer deze nodig is

-   Meer rendabele computeromgevingen

-   Toegang van een willekeurige plaats met een webportal of programmatische automation

-   Cloud-gebaseerde services om de meeste berekenings- en behoeften te voldoen

Met on-premises infrastructuur hebt u volledige controle over de hardware en software die is geïmplementeerd. In het verleden hebben heeft dit geleid tot hardware inkoop beslissingen die gericht zijn op het omhoog schalen. Een voorbeeld met het aanschaffen van een server met meer cores piek-prestaties behoeften. Deze infrastructuur kan helaas volledig worden gebruikt buiten het venster van een aanvraag. Met Azure, kunt u alleen de infrastructuur die u moet implementeren en dit omhoog of omlaag op elk gewenst moment aanpassen. Dit leidt tot een focus op door de implementatie van extra rekenknooppunten uitbreiden om te voldoen aan de behoeften van een prestaties. Cloudservices uitbreiden is kosteneffectiever zijn dan het omhoog schalen door dure hardware.

Microsoft heeft veel Azure-datacenters over de hele wereld, met meer geplande geïmplementeerd. Microsoft is bovendien soevereine clouds in regio's zoals China en Duitsland verhogen. Alleen de grootste globale ondernemingen kan datacenters op deze manier kunnen implementeren zodat het gebruik van Azure kunt u gemakkelijk voor ondernemingen van elke grootte voor het implementeren van hun services dicht bij hun klanten.

Voor kleine bedrijven kunt Azure u een lage kosten toegangspunt met de mogelijkheid te schalen snel vraag naar compute toeneemt. Hiermee voorkomt u dat een grote investering investeringen in infrastructuur en biedt de flexibiliteit om te bouwen en systemen opnieuw bouwen indien nodig. Het gebruik van cloudcomputing past goed met het model voor scale-fast en mislukken snel starten van de groei.

Zie voor meer informatie over de beschikbare Azure-regio's [Azure-regio's](https://azure.microsoft.com/regions/).

### <a name="cloud-computing-is-classified-into-three-categories-saas-paas-and-iaas"></a>Cloudcomputing is ingedeeld in drie categorieën: SaaS PaaS en IaaS.

#### <a name="saas-software-as-a-service"></a>SaaS: Software als een service

SaaS is software die centraal wordt gehost en beheerd. Meestal gebaseerd op een multitenant-architectuur: één versie van de toepassing wordt gebruikt voor alle klanten. Het kan worden uitgebreid naar meerdere exemplaren om te controleren of de beste prestaties op alle locaties. SaaS-software is meestal gelicentieerd via een maandelijkse of jaarlijkse abonnement.

Microsoft Office 365 is een goed voorbeeld van een SaaS aanbieden. Maandelijkse of jaarlijkse abonnement van abonnees betalen en krijgen ze Microsoft Exchange, Microsoft OneDrive en de rest van de Microsoft Office suite als een service. Abonnees altijd de meest recente versie downloaden en de Exchange-server wordt beheerd voor u. Vergeleken met de installatie en upgrade van Office elk jaar, dit is minder dure en vereist minder moeite.

#### <a name="paas-platform-as-a-service"></a>PaaS: Platform als een service 

Met PaaS, moet u uw toepassing in een omgeving met de leverancier van de cloud implementeert. De leverancier doet al het beheer van infrastructuur zodat u zich op de ontwikkeling van toepassingen richten kunt.

Azure biedt dat verschillende PaaS compute-aanbiedingen, met inbegrip van de functie Web Apps van Azure App Service en Azure Cloud Services (web- en werkrollen rollen). In beide gevallen hebben ontwikkelaars verschillende manieren voor het implementeren van de toepassing zonder te weten over de nuts and bolts die dit ondersteunen. Ontwikkelaars hoeft te maken van virtuele machines (VM's), Remote Desktop Protocol (RDP) aan te melden bij elkaar gebruiken of de toepassing installeren. Ze net een knop bereikt (of te sluiten) en de hulpprogramma's van Microsoft voor het inrichten van de virtuele machines implementeren en installeren van de toepassing op deze.

#### <a name="iaas-infrastructure-as-a-service"></a>IaaS: Infrastructuur als een service

De leverancier van een IaaS-cloud wordt uitgevoerd en beheert alle fysieke resources en de vereiste software om in te schakelen van computer-virtualisatie. Een klant van deze service implementeert virtuele machines in deze gehoste datacenters. Hoewel de virtuele machines bevinden zich in een datacenter offsite, heeft de consument IaaS controle over de configuratie en het beheer ervan.

Azure bevat diverse IaaS-oplossingen, met inbegrip van virtuele machines, virtuele-machineschaalsets en de verwante netwerkinfrastructuur. Virtuele machines zijn een populaire keuze voor het in eerste instantie migreren naar Azure-services omdat hij Hiermee een model van de migratie 'lift- en verschuiven'. U kunt configureren van een virtuele machine als de infrastructuur van uw services momenteel wordt uitgevoerd in uw datacenter en vervolgens uw software naar de nieuwe virtuele machine te migreren. U mogelijk wilt maken van configuratie-updates, zoals URL's voor andere services of de opslag, maar veel toepassingen op deze manier kunnen worden gemigreerd.

Virtuele-machineschaalsets boven op Azure Virtual Machines zijn ingebouwd en bieden een eenvoudige manier voor het implementeren van clusters van identieke virtuele machines. Virtuele-machineschaalsets bieden ook ondersteuning voor automatisch schalen, zodat de nieuwe virtuele machines automatisch kunnen worden geïmplementeerd als dat nodig. Dit maakt virtuele-machineschaalsets een ideaal platform op een hoger niveau microservice compute hostclusters, zoals Azure Service Fabric en Azure Container Service.

## <a name="azure-services"></a>Azure-services

Azure biedt tal van services in de cloud computing-platform. Deze services biedt de volgende.

### <a name="compute-services"></a>Compute services

Services voor het hosten en uitvoeren van toepassing werklast:

-   Virtuele Machines in Azure, Linux- en Windows

-   App-Services (Web-Apps, mobiele Apps, Logic Apps, API-Apps en Apps van de functie)

-   Azure Batch (voor grootschalige parallelle en batchtaken compute)

-   Azure Service Fabric

-   Azure Container Service

### <a name="data-services"></a>Gegevensservices

Services voor het opslaan en beheren van gegevens:

-   Azure-opslag (omvat de services van Azure Blob, Queue, Table en -bestand)

-   Azure SQL Database

-   Azure Cosmos DB

-   Microsoft Azure StorSimple

-   Azure Redis-cache

### <a name="application-services"></a>Toepassingsservices

Services voor het bouwen en operationele toepassingen:

-   Azure Active Directory (Azure AD)

-   Azure Service Bus voor het verbinden van gedistribueerde systemen

-   Azure HDInsight voor het verwerken van big data

-   Azure Scheduler

-   Azure Media Services

### <a name="network-services"></a>Netwerkservices

Services voor netwerken in Azure, en ook tussen Azure en on-premises datacenters:

-   Azure Virtual Network

-   ExpressRoute van Azure

-   Azure DNS-

-   Azure Traffic Manager

-   Azure Content Delivery Network

Zie voor gedetailleerde documentatie over Azure-services [documentatie van Azure service](https://docs.microsoft.com/azure).

## <a name="azure-key-concepts"></a>Azure belangrijkste concepten

### <a name="datacenters-and-regions"></a>Datacenters en regio 's


Azure is een globale cloudplatform die algemeen beschikbaar is in veel regio's over de hele wereld. Wanneer u een service, toepassing of virtuele machine in Azure inricht, wordt u gevraagd om een regio te selecteren. De geselecteerde regio vertegenwoordigt een speciﬁc datacenter waarin de toepassing wordt uitgevoerd. Zie voor meer informatie [Azure-regio's](https://azure.microsoft.com/regions/).

Een van de beneﬁts van het gebruik van Azure is dat u uw toepassingen in verschillende datacenters over de hele wereld kunt implementeren. De regio die u kunt de prestaties van uw toepassing aﬀect. Het is kiezen van een regio die zich dichter bij meest uw klanten te verminderen latentie in netwerkaanvragen optimale. U kunt ook een regio om te voldoen aan de wettelijke vereisten voor het distribueren van uw app in bepaalde landen selecteren.

### <a name="azure-portal"></a>Azure Portal


De Azure portal is een webtoepassing die kan worden gebruikt voor het maken, beheren en Azure-resources en services verwijderen. De Azure-portal bevindt zich op https://portal.azure.com. Het bevat een aangepast dashboard en tooling voor het beheren van Azure-resources. Het bevat ook informatie voor facturering en abonnementen. Zie voor meer informatie [overzicht van Microsoft Azure-portal](https://azure.microsoft.com/documentation/articles/azure-portal-overview/) en [beheren Azure-resources via de portal](https://docs.microsoft.com/azure/azure-portal/resource-group-portal).

### <a name="resources"></a>Resources

Azure-resources zijn afzonderlijke compute, netwerk, gegevens of app die als host fungeert voor services die zijn geïmplementeerd in een Azure-abonnement. Sommige algemene resources zijn virtuele machines, opslagaccounts of SQL-databases. Azure-services bestaan vaak uit verschillende Azure gerelateerde resources. Een virtuele machine van Azure kan bijvoorbeeld een VM, de storage-account, de netwerkadapter en het openbare IP-adres bevatten. Deze resources worden gemaakt, beheerd en afzonderlijk of als een groep verwijderd. Azure-resources worden behandeld in meer detail verderop in deze handleiding.

### <a name="resource-groups"></a>Resourcegroepen

Een Azure-resourcegroep is een container met verwante resources voor een Azure-oplossing. De resourcegroep kan alle resources voor de oplossing of alleen de resources die u wilt beheren als groep bevatten. Azure-resourcegroepen worden behandeld in meer detail verderop in deze handleiding.

### <a name="resource-manager-templates"></a>Resource Manager-sjablonen

Een Azure Resource Manager-sjabloon is een JSON JavaScript Object Notation ()-bestand dat definieert een of meer resources om te implementeren voor een resourcegroep. Het definieert ook de afhankelijkheden tussen geïmplementeerde resources. Resource Manager-sjablonen worden behandeld in meer detail verderop in deze handleiding.

### <a name="automation"></a>Automatisering


Naast het maken, beheren en verwijderen van de resources met behulp van de Azure-portal, kunt u deze activiteiten automatiseren met behulp van PowerShell of de Azure-opdrachtregelinterface (CLI).

**Azure PowerShell**

Azure PowerShell is een set van modules die cmdlets voor het beheren van Azure bieden. U kunt de cmdlets gebruiken om te maken, beheren en verwijderen van de Azure-services. De cmdlets kunt u consistent herhaalbare en zonder implementaties kunt bereiken. Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps) voor meer informatie.

**Azure-opdrachtregelinterface**

De Azure-opdrachtregelinterface is een hulpprogramma waarmee u kunt maken, beheren en Azure-resources verwijderen vanaf de opdrachtregel. De Azure CLI is beschikbaar voor Linux-, Mac OS X- en Windows. Zie voor meer informatie en technische details [Azure CLI installeren](/cli/azure/install-azure-cli.md).

**REST API's** Azure is gebouwd op een set REST-API's die ondersteuning bieden voor de gebruikersinterface van de Azure portal. De meeste van deze REST-API's worden ook ondersteund zodat u programmatisch inrichten en beheren van uw Azure-resources en apps vanaf elk apparaat toegang tot Internet hebben. Zie voor meer informatie de [naslaginformatie over Azure REST SDK](https://docs.microsoft.com/rest/api/index).


## <a name="azure-subscriptions"></a>Azure-abonnementen


Een abonnement is een logische groepering van Azure-services die is gekoppeld aan een Azure-account. Een afzonderlijke Azure-account kan meerdere abonnementen bevatten. Facturering voor Azure-services wordt uitgevoerd op basis van per abonnement. Azure-abonnementen hebben een accountbeheerder die volledige controle over het abonnement heeft en een servicebeheerder die controle over alle services in het abonnement heeft. Naast de beheerders, individuele accounts kunnen worden verleend gedetailleerde controle over Azure-resources via RBAC.

### <a name="select-and-enable-an-azure-subscription"></a>Selecteren en inschakelen van een Azure-abonnement

Voordat u met Azure-services werken kunt, moet u een abonnement. Verschillende typen van de abonnementen zijn beschikbaar.

**Gratis accounts**: de koppeling aan te melden voor een gratis account is op de [Azure-website](https://azure.microsoft.com/). Dit biedt u een creditcard in de loop van 30 dagen om te proberen een combinatie van resources in Azure. Als u uw tegoed bedrag overschrijdt, wordt uw account is onderbroken. Aan het einde van de proefversie van uw services buiten werking worden gesteld en werkt niet meer. U kunt upgraden naar een abonnement op gebruiksbasis op elk gewenst moment.

**MSDN-abonnement**: als u een MSDN-abonnement hebt, krijgt u een bepaald bedrag in Azure-tegoeden elke maand. Bijvoorbeeld, als u een Microsoft Visual Studio Enterprise met MSDN-abonnement hebt, krijgt u \$150 per maand in Azure-tegoeden.

Als u de hoeveelheid tegoed overschrijdt, wordt uw service zijn uitgeschakeld totdat de volgende maand wordt gestart. U kunt de bestedingslimiet uitschakelen en toevoegen van een creditcard moet worden gebruikt voor de extra kosten. Sommige van deze kosten zijn korting voor MSDN-accounts. Bijvoorbeeld, u de Linux-prijs betaalt voor virtuele machines waarop Windows Server wordt uitgevoerd en er zijn geen extra kosten voor Microsoft-servers zoals Microsoft SQL Server. Dit zijn MSDN-accounts ideaal voor ontwikkeling en Testscenario's.

**BizSpark accounts**: de Microsoft-BizSpark programma biedt veel voordelen voor startups. Een van deze voordelen is toegang tot alle Microsoft-software voor ontwikkeling en testomgevingen voor maximaal vijf MSDN-accounts. Ophalen van $150 in Azure-tegoeden voor elk van deze vijf MSDN-accounts en betaalt u verlaagde tarieven voor verschillende van de Azure-services, zoals virtuele Machines.

**Betalen naar gebruik**: met dit abonnement dat u betaalt voor gebruik door een creditcard of betaalpas aan het account koppelen. Als u een organisatie, kunt u ook worden goedgekeurd voor facturering.

**Enterprise overeenkomsten**: met een enterprise agreement, die u voor het gebruik van een bepaald aantal services in Azure gedurende het volgende jaar doorvoeren en u dit bedrag tevoren betaalt. De vastlegging die u aanbrengt wordt in het jaar verbruikt. Als u het toezeggingsbedrag overschrijdt, kunt u de overschrijding in achterstanden betalen. Afhankelijk van de grootte van de vastlegging krijgt u een korting van de services in Azure.



### <a name="grant-administrative-access-to-an-azure-subscription"></a>Verleen beheerderstoegang tot een Azure-abonnement


Meerdere account beheerdersrollen beschikbaar zijn en op elk gewenst moment kunnen worden gewijzigd. Er zijn twee belangrijke functies:

-   **Servicebeheerder**: deze rol is geautoriseerd voor het beheren van Azure-services. Standaard krijgt toegang tot hetzelfde account zijn als de accountbeheerder.

-   **Medebeheerder**: deze rol heeft dezelfde toegangsrechten als de servicebeheerder. Deze rol kan echter niet de koppeling van een abonnement voor Azure directory's wijzigen.

Zie voor meer informatie [toevoegen of wijzigen van de Azure-beheerdersrollen](../../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="view-billing-information-in-the-azure-portal"></a>Factuurgegevens bekijken in de Azure portal


Een belangrijk onderdeel van het gebruik van Azure is de mogelijkheid om factureringsgegevens weer te geven. De Azure-portal biedt meer inzicht in Azure factureringsgegevens.

Zie voor meer informatie [het downloaden van uw Azure facturering facturen en dagelijks gebruiksgegevens](../../billing/billing-download-azure-invoice-daily-usage-date.md).

### <a name="get-billing-information-from-billing-apis"></a>Factureringsgegevens ophalen uit het facturering API 's


U kunt niet alleen de facturering weergeven in de portal, de factureringsgegevens openen met behulp van een script of een programma via de REST API's van de Azure-facturering:

-   U kunt de API voor het gebruik van Azure gebruiken voor het ophalen van de gebruiksgegevens. Labels van gerelateerde Azure-resources kunt u de gebruiksgegevens van de facturering verfijnen. U kunt bijvoorbeeld tag elk van de resources in een resourcegroep met een afdelingsnaam of de projectnaam en vervolgens het bijhouden van de kosten specifiek voor dat één label.

-   U kunt de API van Azure snelheid kaart voor een lijst met alle beschikbare bronnen, samen met de metagegevens en prijzen van informatie over elk van deze resources gebruiken.

Zie voor meer informatie [inzicht in uw Microsoft Azure-brongebruik](../../billing/billing-usage-rate-card-overview.md).

### <a name="forecast-cost-with-the-pricing-calculator"></a>Kosten prognose met de prijscategorie Rekenmachine

De prijzen voor elke service in Azure verschilt. Veel Azure-services bieden categorieën Basic, Standard en Premium. Elke laag heeft meestal verschillende niveaus van prijs- en prestaties. Met behulp van de [online prijscategorie Rekenmachine](http://azure.microsoft.com/pricing/calculator), kunt u de prijscategorie maakt een schatting maken. De Rekenmachine bevat flexibiliteit voor een schatting van de kosten voor één resource of een groep resources.

### <a name="set-up-billing-alerts"></a>Meldingen voor facturering instellen

Nadat u uw toepassing of oplossing op Azure hebt geïmplementeerd, kunt u waarschuwingen die u als u benadering bestedingslimieten gedefinieerd in de waarschuwing e verzenden. Zie voor meer informatie [waarschuwingen voor uw Microsoft Azure-abonnementen facturering instellen](../../billing/billing-set-up-alerts.md).

## <a name="azure-resource-manager"></a>Azure Resource Manager

Azure Resource Manager is een implementatie, beheer en organisatie-mechanisme voor Azure-resources. Met Resource Manager, kunt u veel afzonderlijke resources samen plaatsen in een resourcegroep.

Resource Manager bevat ook implementatiemogelijkheden die staan voor de aanpasbare implementatie en configuratie van de bijbehorende resources. U kunt bijvoorbeeld een toepassing die uit meerdere virtuele machines, een load balancer en een SQL-database als één eenheid bestaat implementeren met behulp van Resource Manager. U kunt deze implementaties ontwikkelen met behulp van een Resource Manager-sjabloon.

Resource Manager biedt diverse voordelen:

-   U kunt alle resources voor uw oplossing implementeren, beheren en bewaken als groep, in plaats van deze resources afzonderlijk te verwerken.

-   U kunt herhaaldelijk gedurende de ontwikkelingscyclus uw oplossing voor het implementeren en erop vertrouwen dat uw resources worden geïmplementeerd in een consistente status.

-   U kunt uw infrastructuur beheren via declaratieve sjablonen in plaats van scripts.

-   U kunt de afhankelijkheden tussen resources zo definiëren dat deze in de juiste volgorde worden geïmplementeerd.

-   U kunt toegangsbeheer toepassen op alle services in de resourcegroep omdat RBAC is geïntegreerd in het beheerplatform.

-   U kunt tags toepassen op resources te organiseren logisch alle resources in uw abonnement.

-   U kunt facturering voor uw organisatie verduidelijken door het bekijken van de kosten voor een groep resources die dezelfde tag delen.

### <a name="tips-for-creating-resource-groups"></a>Tips voor het maken van resourcegroepen


Wanneer u bent u beslissingen neemt over uw resourcegroepen, houd rekening met de volgende tips:

-   Alle resources in een resourcegroep moeten dezelfde levenscyclus hebben.

-   U kunt een resource toewijzen aan slechts één groep tegelijk.

-   U kunt toevoegen of een resource verwijdert uit een resourcegroep op elk gewenst moment. Elke bron moet behoren tot een resourcegroep. Dus als u een resource van de ene groep verwijdert, moet u deze toevoegen aan een andere.

-   U kunt de meeste typen resources verplaatsen naar een andere resourcegroep op elk gewenst moment.

-   De resources in een resourcegroep kunnen zich in verschillende regio's.

-   U kunt een resourcegroep gebruiken voor het toegangsbeheer voor de resources in deze.

### <a name="building-resource-manager-templates"></a>Resource Manager-sjablonen samenstellen

Resource Manager-sjablonen worden de resources en de resource-configuraties die worden geïmplementeerd in één resourcegroep declaratief definiëren. U kunt Resource Manager-sjablonen indelen van complexe implementaties zonder dat hiervoor extra scripts of handmatige configuratie nodig. Nadat u een sjabloon ontwikkelt, kunt u dit implementeren meerdere keren, telkens met een identieke uitkomst.

Resource Manager-sjabloon bestaat uit vier secties:

-   **Parameters**: dit zijn ingangen voor de implementatie. Parameterwaarden kunnen worden opgegeven door een persoon of een geautomatiseerd proces. Een voorbeeld van de parameter is mogelijk een admin-gebruikersnaam en wachtwoord voor een virtuele machine van Windows. De parameterwaarden die worden gebruikt tijdens de implementatie wanneer ze zijn opgegeven.

-   **Variabelen**: deze worden gebruikt voor het opslaan van de waarden die in de implementatie worden gebruikt. In tegenstelling tot parameters, is een variabele waarde niet opgegeven tijdens de implementatie. Het is in plaats daarvan harde code vastgelegd of dynamisch wordt gegenereerd.

-   **Resources**: in dit gedeelte van de sjabloon definieert de bronnen worden geïmplementeerd, zoals virtuele machines, opslagaccounts en virtuele netwerken.

-   **Uitvoer**: nadat een implementatie is voltooid, Resource Manager gegevens zoals dynamisch gegenereerde verbindingsreeksen kunt terugkeren.

De volgende mechanismen zijn beschikbaar voor implementatie automation:

-   **Functies**: U kunt verschillende functies gebruiken in de Resource Manager-sjablonen. Deze omvatten bewerkingen zoals het converteren van een tekenreeks in kleine letters, het implementeren van meerdere exemplaren van een gedefinieerde bron en de doelresourcegroep dynamisch te retourneren. Resource Manager-functies helpen bij het bouwen van dynamische implementaties.

-   **Bronafhankelijkheden**: wanneer u meerdere resources, sommige bronnen implementeert heeft een afhankelijkheid voor anderen. Om te bevorderen implementatie, kunt u de declaratie van een afhankelijkheid zodat afhankelijke resources worden geïmplementeerd voordat u de andere.

-   **Sjabloon koppelen**: uit binnen één Resource Manager-sjabloon, u kunt koppelen aan een andere sjabloon. Hierdoor kan afbreken van de implementatie in een set van gerichte, doel-specifieke sjablonen.

U kunt opbouwen Resource Manager-sjablonen in een teksteditor. De Azure SDK voor Visual Studio biedt echter hulpprogramma's om u te helpen. Met behulp van Visual Studio, kunt u resources toevoegen aan de sjabloon met een wizard en vervolgens implementeren en fouten opsporen in de sjabloon rechtstreeks vanuit Visual Studio. Zie voor meer informatie [Azure Resource Manager-sjablonen samenstellen](../../resource-group-authoring-templates.md).

Ten slotte kunt u bestaande resourcegroepen converteren naar een herbruikbare sjabloon van de Azure-portal. Dit kan nuttig zijn als u wilt maken van een implementeerbaar sjabloon van een bestaande resourcegroep of u wilt onderzoeken van de onderliggende JSON. Als u wilt exporteren van een resourcegroep, selecteer de **automatiseringsscript** knop uit de instellingen voor de resourcegroep.

## <a name="security-of-azure-resources-rbac"></a>Beveiliging van de Azure-resources (RBAC)

U kunt operationele toegang verlenen tot gebruikersaccounts bij een opgegeven bereik: abonnement, resourcegroep of afzonderlijke resource. Dit betekent dat u kunt een aantal resources in een resourcegroep, zoals een virtuele machine en alle bijbehorende resources implementeren en machtigingen verlenen voor een specifieke gebruiker of groep. Deze aanpak toegang wordt beperkt tot alleen de resources die deel uitmaken van de doelresourcegroep. U kunt ook toegang verlenen tot één resource, zoals een virtuele machine of een virtueel netwerk.

Om toegang te verlenen, kunt u een rol toewijzen aan de gebruiker of gebruikersgroep. Er zijn veel vooraf gedefinieerde rollen. U kunt ook uw eigen aangepaste rollen definiëren.

Hier volgen een paar voorbeeld rollen die zijn ingebouwd in Azure:

-   **Eigenaar**: een gebruiker met deze rol kan alles beheren, inclusief toegang.

-   **Lezer**: een gebruiker met deze rol kunnen bronnen van alle typen (met uitzondering van geheimen) lezen maar geen wijzigingen aanbrengen.

-   **Virtuele machine Inzender**: een gebruiker met deze rol virtuele machines kunt beheren, maar niet het virtuele netwerk te beheren die zijn verbonden of het opslagaccount waarin het VHD-bestand zich bevindt.

-   **SQL DB contributor**: een gebruiker met deze rol SQL-databases, maar niet hun beleid betrekking hebben op beveiliging kunt beheren.

-   **SQL security manager**: het beleid betrekking hebben op de beveiliging van SQL-servers en databases kunt beheren door een gebruiker met deze rol.

-   **Storage-account Inzender**: een gebruiker met deze rol storage-accounts kunt beheren, maar kan geen toegang tot de storage-accounts beheren.

Zie voor meer informatie [roltoewijzingen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](../../active-directory/role-based-access-control-configure.md).

## <a name="azure-virtual-machines"></a>Azure Virtual Machines

Virtuele Machines in Azure is een van de centrale IaaS-services in Azure. Virtuele Machines van Azure ondersteunt de implementatie van Windows of Linux virtuele machines in een Microsoft Azure-datacenter. Met Azure Virtual Machines die u hebt volledige controle over de VM-configuratie en verantwoordelijk zijn voor alle software-installatie, configuratie en onderhoud.

Wanneer u een virtuele machine in Azure implementeert, kunt kunt u een installatiekopie maken vanuit Azure Marketplace, of u u eigen gegeneraliseerde installatiekopie. Deze installatiekopie wordt gebruikt voor het besturingssysteem en de initiële configuratie toepassen. Tijdens de implementatie van wordt Resource Manager afgehandeld tijdens bepaalde configuratie-instellingen, zoals het toewijzen van de computernaam, beheerdersreferenties en netwerkconfiguratie. Uitbreidingen van de virtuele machine van Azure kunt u configuraties, zoals software-installatie, configuratie van antivirus en bewakingsoplossingen verder worden geautomatiseerd.

In veel verschillende grootten, kunt u virtuele machines maken. De grootte van virtuele machine bepaalt resourcetoewijzing zoals verwerking, geheugen en opslagcapaciteit. In sommige gevallen zijn specifieke functies zoals RDMA netwerkadapters en SSD-schijven zijn alleen beschikbaar voor bepaalde VM-grootten. Zie voor een volledige lijst met VM-grootten en mogelijkheden, 'Grootten voor virtuele machines in Azure' voor [Windows](../../virtual-machines/windows/sizes.md) en [Linux](../../virtual-machines/linux/sizes.md).

### <a name="use-cases"></a>Gebruiksvoorbeelden

Omdat de virtuele machines van Azure bieden volledige controle over de configuratie, zijn ze ideaal voor een breed scala aan server-werkbelasting die niet in een PaaS-model passen. Server-werkbelastingen zoals databaseservers (SQL Server, Oracle of MongoDB), Windows Server Active Directory, Microsoft SharePoint en veel nog meer mogelijk uit te voeren op de Microsoft Azure-platform. Indien gewenst, kunt u voor deze werkbelastingen verplaatsen van een on-premises datacenter naar een of meer Azure-regio's, zonder een grote hoeveelheid herconfiguratie.

### <a name="deployment-of-virtual-machines"></a>Implementatie van virtuele machines

U kunt virtuele machines in Azure implementeren met behulp van de Azure-portal, met behulp van automatisering met de Azure PowerShell-module of met behulp van automatisering met de platformoverschrijdende CLI.

**Portal**

Een virtuele machine implementeert met behulp van de Azure-portal vereist alleen een actief Azure-abonnement en de toegang tot een webbrowser. Kunt u installatiekopieën van veel verschillende besturingssystemen met verschillende configuraties. Alle opslag- en netwerkvereisten worden geconfigureerd tijdens de implementatie. Zie voor meer informatie 'Een virtuele machine in de Azure portal maken' voor [Windows](../../virtual-machines/windows/quick-create-portal.md) en [Linux](../../virtual-machines/linux/quick-create-portal.md).

Naast het implementeren van een virtuele machine van de Azure-portal, kunt u vanuit de portal een Azure Resource Manager-sjabloon implementeren. Dit laten implementeren en configureren van alle resources, zoals gedefinieerd in de sjabloon. Zie voor meer informatie [implementeren van resources met Resource Manager-sjablonen en Azure-portal](../../azure-resource-manager/resource-group-template-deploy-portal.md).


**PowerShell**

Kan Azure een virtuele machine implementeren met behulp van PowerShell voor automatisering van de volledige implementatie van alle verwante virtuele machinebronnen, met inbegrip van opslag en netwerken. Zie voor meer informatie [maken van een Windows-VM met Resource Manager en PowerShell](../../virtual-machines/windows/quick-create-powershell.md).

Naast het Azure-rekenresources afzonderlijk implementeren, kunt u de Azure PowerShell-module voor het implementeren van een Azure Resource Manager-sjabloon. Zie voor meer informatie [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md).

**Opdrachtregelinterface (CLI)**

Net als bij de PowerShell-module is de Azure-opdrachtregelinterface implementatie automation biedt en kan worden gebruikt op Windows-, OS X- of Linux-systemen. Wanneer u de Azure CLI **vm snelle invoer** uitvoert, en alle gerelateerde bronnen voor virtuele machines (met inbegrip van opslag en netwerken) en de virtuele machine zelf zijn geïmplementeerd. Zie voor meer informatie [een Linux-VM maken in Azure met behulp van de CLI](../../virtual-machines/linux/quick-create-cli.md).

U kunt ook de Azure CLI gebruiken om een Azure Resource Manager-sjabloon te implementeren. Zie voor meer informatie [implementeren van resources met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md).

### <a name="access-and-security-for-virtual-machines"></a>Toegang en beveiliging voor virtuele machines

Een virtuele machine benaderen via Internet is vereist voor het bijbehorende netwerk interface of load balancer indien van toepassing moet worden geconfigureerd met een openbaar IP-adres. Het openbare IP-adres bevat een DNS-naam die wordt omgezet in de virtuele machine of load balancer. Zie voor meer informatie [IP-adressen in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).

U kunt toegang tot de virtuele machine beheren via het openbare IP-adres met behulp van een netwerkbron security-groep (NSG). Een NSG fungeert als een firewall en verleent of weigert verkeer via de netwerkinterface of een subnet van een reeks gedefinieerde poorten. Maakt een extern-bureaubladsessie met een virtuele machine in Azure, moet u bijvoorbeeld het NSG zodat binnenkomend verkeer op poort 3389 configureren. Zie voor meer informatie [openen van poorten voor een virtuele machine in Azure met behulp van de Azure-portal](../../virtual-machines/windows/nsg-quickstart-portal.md).

Ten slotte, net als bij het beheer van elk computersysteem, moet u zelf beveiliging voor een virtuele machine van Azure op het besturingssysteem met behulp van beveiligingsreferenties en softwarefirewalls.

## <a name="azure-storage"></a>Azure Storage

Azure Storage is een door Microsoft beheerde service waarmee duurzame, schaalbare en redundante opslag. U kunt een Azure storage-account als een resource toevoegen aan een resourcegroep met behulp van de implementatiemethode van elke resource. Azure bevat vier opslagtypen: Blob-opslag, File Storage, Table storage en Queue storage. Bij het implementeren van een opslagaccount wordt twee accounttypen zijn beschikbaar, algemene en blob-opslag. Een algemeen opslagaccount hebt u toegang tot alle vier opslagtypen. BLOB storage-accounts zijn vergelijkbaar met algemene accounts, maar bevatten gespecialiseerde blobs die warme en koude toegangslagen bevatten. Zie voor meer informatie over blob-opslag [Azure Blob storage](../../storage/blobs/storage-blob-storage-tiers.md).

Azure storage-accounts kunnen worden geconfigureerd met verschillende niveaus van redundantie:

-   **Lokaal redundante opslag** zorgt voor hoge beschikbaarheid door ervoor te zorgen dat drie kopieën van alle gegevens synchroon zijn gedaan voordat een schrijfbewerking wordt geacht geslaagd. Deze kopieën zijn opgeslagen in één faciliteit in één regio. De replica's zich bevinden in domeinen met afzonderlijke fouten en upgradedomeinen. Dit betekent dat de gegevens zijn beschikbaar, zelfs als een opslagknooppunt die van uw gegevens is mislukt bezit is of een offline te halen worden bijgewerkt.

-   **Geografisch redundante opslag** zorgt ervoor dat drie synchrone kopieën van de gegevens in de primaire regio voor hoge beschikbaarheid en vervolgens asynchroon drie replica's in een gekoppelde gebied voor herstel na noodgevallen.

-   **Geografisch redundante opslag met leestoegang** is geografisch redundante opslag plus de mogelijkheid om te lezen van de gegevens in de secundaire regio. Deze mogelijkheid maakt het geschikt is voor gedeeltelijke noodherstel. Als er een probleem met de primaire regio, kunt u uw toepassing alleen-lezen toegang hebben tot de gekoppelde regio.

### <a name="use-cases"></a>Gebruiksvoorbeelden 

Elk opslagtype heeft een andere gebruiksvoorbeeld.

**Blob Storage** 

Het woord *blob* is een acroniem voor *BLOB*. BLOBs zijn niet-gestructureerde bestanden zoals die u op uw computer opslaat. In Blob Storage kan elk type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstallatieprogramma, worden opgeslagen. U kunt Blob Storage zien als een vorm van objectopslag. Azure Blob storage bevat ook gegevensschijven Azure Virtual Machines.

Azure Storage ondersteunt drie typen blobs:

-   **Blok-blobs** worden gebruikt voor het opslaan van gewone bestanden 195 GB (4 MB x 50.000 blokken) groot. Het primaire gebruiksvoorbeeld voor blok-blobs is de opslag van bestanden die worden gelezen van begin tot einde, zoals mediabestanden of afbeeldingsbestanden voor websites. Ze zijn blok-blobs genoemd, omdat de bestanden groter dan 64 MB moeten worden geüpload als kleine blokken. Deze blokken zijn vervolgens geconsolideerd (of toegewezen) in de laatste blob.

-   **Pagina-blobs** bestemd is voor willekeurige toegang tot 1 TB in de grootte van bestanden. Pagina-blobs zijn voornamelijk gebruikt om de opslag van back-ups maken voor de virtuele harde schijven die duurzame schijven voor Azure Virtual Machines bieden, de IaaS compute-service in Azure. Ze worden pagina-blobs genoemd omdat ze willekeurige lees-/ schrijftoegang tot 512-byte-pagina's bieden.

-   **Toevoeg-blobs** bestaan uit blokken, zoals blok-blobs, maar ze zijn geoptimaliseerd voor toevoegbewerkingen. Deze worden vaak gebruikt voor logboekregistratie gegevens uit een of meer bronnen naar de dezelfde blob. U kunt alle uw traceerlogboeken bijvoorbeeld schrijven naar dezelfde toevoeg-blob voor een toepassing die wordt uitgevoerd op meerdere virtuele machines. Een enkele toevoeg-blob kan maximaal 195 GB zijn.

Zie voor meer informatie [aan de slag met Azure Blob storage met .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md).

**Opslag van bestanden**

Azure File storage is een service die bestandsshares in de cloud aanbiedt met behulp van de standaard Server Message Block (SMB)-protocol. De service ondersteunt zowel SMB 2.1 als SMB 3.0. U kunt toepassingen die afhankelijk van bestandsshares naar Azure snel en zonder kostbare regeneraties zijn te migreren met Azure File storage. Toepassingen worden uitgevoerd op virtuele machines in Azure, in de cloud-services of on-premises clients kunnen een bestandsshare koppelen in de cloud. Dit is vergelijkbaar met hoe een bureaubladtoepassing een gewone SMB-share. Daarna kan een willekeurig aantal toepassingsonderdelen de File Storage-share tegelijkertijd koppelen en gebruiken.

Omdat een File storage-share een standaard SMB-bestandsshare is, kunnen de toepassingen in Azure wordt uitgevoerd toegang tot gegevens in de share via I/O APIs-bestandssysteem. Ontwikkelaars kunnen daarom hun bestaande code en vaardigheden gebruiken voor het migreren van bestaande toepassingen. IT-professionals kunnen PowerShell-cmdlets gebruiken om te maken, koppelen en beheren van bestandsshares voor opslag als onderdeel van het beheer van Azure-toepassingen.

Zie voor meer informatie [aan de slag met Azure File storage in Windows](../../storage/files/storage-how-to-use-files-windows.md) of [Azure File storage gebruiken met Linux](../../storage/files/storage-how-to-use-files-linux.md).

**Table Storage**

Azure Table Storage is een service waarmee gestructureerde NoSQL-gegevens worden opgeslagen in de cloud. Table storage is een sleutel-/ kenmerkopslag met een schema-less-ontwerp. Omdat Table storage schema minder is, is uw kunt gegevens eenvoudig aanpassen naarmate de behoeften van uw toepassing veranderen. De toegang tot gegevens verloopt snel en kostenefficiënt voor alle soorten toepassingen. Table Storage is doorgaans aanzienlijk goedkoper dan traditionele SQL voor vergelijkbare gegevensvolumes.

U kunt Table Storage gebruiken voor het opslaan van flexibele gegevenssets, zoals gebruikersgegevens voor webtoepassingen, adresboeken, apparaatgegevens en alle overige typen metagegevens die uw service nodig heeft. U kunt een willekeurig aantal entiteiten opslaan in een tabel. Een opslagaccount kan een onbeperkt aantal tabellen, tot de capaciteitslimiet van het opslagaccount bevatten.

Zie voor meer informatie [aan de slag met Azure Table storage](../../cosmos-db/table-storage-how-to-use-dotnet.md).

**Queue Storage**

Azure Queue Storage biedt uitwisseling van berichten tussen toepassingsonderdelen in de cloud. Bij het ontwerpen van schaalbare toepassingen, worden toepassingsonderdelen vaak ontkoppeld zodat ze onafhankelijk kunnen schalen. Queue Storage biedt asynchrone uitwisseling van berichten voor communicatie tussen toepassingsonderdelen, of deze nu worden uitgevoerd in de cloud, op een desktopcomputer, op een on-premises server of op een mobiel apparaat. Queue Storage biedt ook ondersteuning voor het beheren van asynchrone taken en het samenstellen van proceswerkstromen.

Zie voor meer informatie [aan de slag met Azure Queue storage](../../storage/queues/storage-dotnet-how-to-use-queues.md).

### <a name="deploying-a-storage-account"></a>Een opslagaccount implementeren

Er zijn verschillende opties voor het implementeren van een opslagaccount.

**Portal**

Implementatie van een opslagaccount met behulp van de Azure-portal vereist alleen een actief Azure-abonnement en de toegang tot een webbrowser. U kunt een nieuw opslagaccount implementeren in een nieuwe of bestaande resourcegroep. Nadat u het opslagaccount hebt gemaakt, maakt u een blob-container of bestandsshare met behulp van de portal. U kunt de tabel maken en entiteiten van opslag via een programma in de wachtrij. Zie voor meer informatie [een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account).

Naast het implementeren van een opslagaccount van de Azure-portal, kunt u vanuit de portal een Azure Resource Manager-sjabloon implementeren. Dit laten implementeren en configureren van alle resources, zoals gedefinieerd in de sjabloon, met inbegrip van alle opslagaccounts. Zie voor meer informatie [implementeren van resources met Resource Manager-sjablonen en Azure-portal](../../azure-resource-manager/resource-group-template-deploy-portal.md).

**PowerShell**

Kan het implementeren van een Azure storage-account met behulp van PowerShell voor automatisering van de volledige implementatie van het opslagaccount. Zie voor meer informatie [Azure PowerShell gebruiken met Azure Storage](../../storage/common/storage-powershell-guide-full.md).

Naast het Azure-resources afzonderlijk implementeren, kunt u de Azure PowerShell-module voor het implementeren van een Azure Resource Manager-sjabloon. Zie voor meer informatie [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md).

**Opdrachtregelinterface (CLI)**

Net als bij de PowerShell-module is de Azure-opdrachtregelinterface implementatie automation biedt en kan worden gebruikt op Windows-, OS X- of Linux-systemen. U kunt de Azure CLI **storage-account maken** opdracht om een opslagaccount te maken. Zie voor meer informatie [met de Azure CLI met Azure Storage.](../../storage/common/storage-azure-cli.md)

U kunt ook de Azure CLI gebruiken om een Azure Resource Manager-sjabloon te implementeren. Zie voor meer informatie [implementeren van resources met Resource Manager-sjablonen en Azure CLI](../../resource-group-template-deploy-cli.md).

### <a name="access-and-security-for-azure-storage"></a>Toegang en beveiliging voor Azure Storage

Azure-opslag wordt op verschillende manieren echter de Azure-portal tijdens het maken van VM's en bewerking en vanaf opslagclientbibliotheken geopend. 

**Schijven voor virtuele machine**

Wanneer u een virtuele machine implementeert, moet u ook een opslagaccount voor het opslaan van de schijf van de virtuele machine-besturingssysteem en eventuele extra gegevensschijven maken. U kunt een bestaand opslagaccount selecteren of een nieuwe maken. Omdat de maximale grootte van een blob 1.024 GB is, heeft één VM-schijf een maximale grootte van 1023 GB. U kunt meerdere gegevensschijven aan de virtuele machine aanwezig en groep ze samen als één logische schijf voor het configureren van een grotere gegevensschijf. Zie voor meer informatie ' beheren schijven van Azure ' voor [Windows](../../virtual-machines/windows/tutorial-manage-data-disk.md) en [Linux](../../virtual-machines/linux/tutorial-manage-disks.md).

**Hulpprogramma's voor opslag**

Azure storage-accounts kunnen worden geopend via veel verschillende storage explorers, zoals Visual Studio Cloud Explorer. Deze hulpprogramma's kunnen u bladeren door de storage-accounts en gegevens. Zie voor meer informatie en een lijst met beschikbare opslag explorers [clienthulpprogramma's van Azure Storage](../../storage/common/storage-explorers.md).

**Storage-API**

Storage-resources toegankelijk zijn voor elke taal waarvoor HTTP/HTTPS-aanvragen. Daarnaast biedt Azure Storage programmeringsbibliotheken voor verschillende veelgebruikte talen. Deze bibliotheken vereenvoudigen werken met Azure Storage door details zoals synchrone en asynchrone aanroepafhandeling, batchverwerking van bewerkingen, Uitzonderingsbeheer en automatische nieuwe pogingen. Zie voor meer informatie [naslaginformatie over REST API van Azure Storage-service](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference).

**Toegangssleutels voor opslag**

Elk opslagaccount heeft twee verificatiesleutels, een primaire en een secundaire database. Een kan worden gebruikt voor toegang tot opslagbewerkingen. Deze opslagsleutels worden gebruikt om te helpen beveiligen van een opslagaccount en zijn vereist voor programmatisch toegang tot gegevens. Er zijn twee sleutels om toe te staan van incidentele rollover van de sleutels om beveiliging te verbeteren. Het is essentieel om de sleutels beveiligen omdat hun bezit, samen met de accountnaam, kunt onbeperkte toegang tot gegevens in het opslagaccount.

**Handtekeningen voor gedeelde toegang**

Als u toestaan dat gebruikers hebben toegang tot uw storage-resources worden beheerd wilt, kunt u een shared access signature maken. Een shared access signature is een token die kan worden toegevoegd aan een URL waarmee gedelegeerde toegang tot een opslagresource. Iedereen die het token in bezit hebben toegang tot de bron waarnaar wordt verwezen met de machtigingen dat wordt hiermee aangegeven, voor de periode dat deze geldig's. Zie voor meer informatie [gedeelde handtekeningen voor toegang](../../storage/common/storage-dotnet-shared-access-signature-part-1.md).

## <a name="azure-virtual-network"></a>Azure Virtual Network


Virtuele netwerken zijn nodig voor de ondersteuning van communicatie tussen virtuele machines. U kunt definiëren subnetten, aangepaste IP-adres, DNS-instellingen, Beveiligingsfiltering en taakverdeling. U kunt virtuele Azure-netwerken verbinden met uw on-premises netwerken via een VPN-gateway of een ExpressRoute-circuit.

### <a name="use-cases"></a>Gebruiksvoorbeelden

Er zijn verschillende gebruiksvoorbeelden voor Azure-netwerken.

**Alleen in de cloud virtuele netwerken**

Een Azure-netwerk standaard is alleen toegankelijk voor resources die zijn opgeslagen in Azure. Resources die zijn verbonden met hetzelfde virtuele netwerk kunnen met elkaar communiceren. U kunt koppelen netwerkinterfaces van virtuele machines en taakverdelers met een openbaar IP-adres op de virtuele machine om toegankelijk te maken via Internet. U kunt beveiligde toegang tot de resources vrij toegankelijke met behulp van een netwerkbeveiligingsgroep.

**Cross-premises virtuele netwerken**

U kunt een on-premises netwerk verbinding maken met een Azure-netwerk via ExpressRoute of een site-naar-site VPN-verbinding. In deze configuratie is het Azure-netwerk in wezen een cloud-gebaseerde uitbreiding van uw on-premises netwerk.

Omdat de virtuele Azure-netwerk is verbonden met uw on-premises netwerk, cross-premises virtuele netwerken moeten een unieke deel van de adresruimte die gebruikmaakt van uw organisatie gebruiken. Azure wordt een andere locatie op dezelfde manier die verschillende zakelijke locaties met een specifiek IP-subnet worden toegewezen, zoals het uitbreiden van uw netwerk.

###<a name="deploying-a-virtual-network"></a>Een virtueel netwerk implementeren

Er zijn verschillende opties voor het implementeren van een virtueel netwerk.

**Portal**

Implementatie van een Azure-netwerk met behulp van de Azure-portal vereist alleen een actief Azure-abonnement en de toegang tot een webbrowser. U kunt een nieuw virtueel netwerk kunt implementeren in een nieuwe of bestaande resourcegroep. Wanneer u een nieuwe virtuele machine vanuit de portal maakt, kunt u een bestaand virtueel netwerk selecteren of een nieuwe maken. Zie voor meer informatie [een virtueel netwerk maken met de Azure-portal](../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

Naast het implementeren van een Azure-netwerk van de Azure-portal, kunt u vanuit de portal een Azure Resource Manager-sjabloon implementeren. Dit laten implementeren en configureren van alle resources, zoals gedefinieerd in de sjabloon, met inbegrip van alle resources van het virtuele netwerk. Zie voor meer informatie [implementeren van resources met Resource Manager-sjablonen en Azure-portal](../../azure-resource-manager/resource-group-template-deploy-portal.md).

**PowerShell**

Kan een virtuele Azure-netwerk implementeren met behulp van PowerShell voor automatisering van de volledige implementatie van het opslagaccount. Zie voor meer informatie [een virtueel netwerk maken met behulp van PowerShell](../../virtual-network/virtual-networks-create-vnet-arm-ps.md).

Naast het Azure-resources afzonderlijk implementeren, kunt u de Azure PowerShell-module voor het implementeren van een Azure Resource Manager-sjabloon. Zie voor meer informatie [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md).

**Opdrachtregelinterface (CLI)**

Net als bij de PowerShell-module is de Azure-opdrachtregelinterface implementatie automation biedt en kan worden gebruikt op Windows-, OS X- of Linux-systemen. U kunt de Azure CLI **network vnet maken** opdracht voor het maken van een virtueel netwerk. Zie voor meer informatie [een virtueel netwerk maken met behulp van de Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md).

U kunt ook de Azure CLI gebruiken om een Azure Resource Manager-sjabloon te implementeren. Zie voor meer informatie [implementeren van resources met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md).

### <a name="access-and-security-for-virtual-networks"></a>Toegang en beveiliging voor virtuele netwerken

U kunt beveiligde virtuele Azure-netwerken met behulp van een netwerkbeveiligingsgroep. Nsg's bevatten een lijst van de toegangsbeheerlijst (ACL) toegangscontroleregels toestaan of weigeren van netwerkverkeer naar uw VM-exemplaren in een virtueel netwerk. U kunt nsg's koppelen aan subnetten of afzonderlijke VM-exemplaren in dat subnet. Wanneer u een NSG aan een subnet koppelen, worden de ACL-regels toepassen op alle VM-exemplaren in dat subnet. Bovendien kunt u verder beperken verkeer naar een afzonderlijke virtuele machine door het koppelen van een NSG rechtstreeks aan die VM. Zie voor meer informatie [filteren van netwerkverkeer met netwerkbeveiligingsgroepen](../../virtual-network/virtual-networks-nsg.md).

## <a name="next-steps"></a>Volgende stappen

- [Windows VM maken](/virtual-machines/windows/quick-create-portal.md)
- [Een virtuele Linux-machine maken](../../virtual-machines/linux/quick-create-portal.md)
