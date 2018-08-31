---
title: Introductiehandleiding voor Azure IT-operators | Microsoft Docs
description: Ophalen van de slag-handleiding voor Azure IT-operators
services: ''
documentationcenter: ''
author: themichaelbender-ms
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.devlang: ''
ms.topic: ''
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 08/24/2018
ms.author: mibender
ms.openlocfilehash: 06365e4397075d18150095f7e77367f457e3308c
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190422"
---
# <a name="get-started-for-azure-it-operators"></a>Aan de slag voor Azure IT-operators

Deze handleiding introduceert de belangrijkste concepten met betrekking tot de implementatie en het beheer van een Microsoft Azure-infrastructuur. Als u niet bekend in de cloud computing, of Azure zelf, deze handleiding helpt snel aan de slag met de concepten, implementatie en beheer van meer informatie. Veel secties van deze handleiding bespreken van een bewerking zoals het implementeren van een virtuele machine, en geef vervolgens een koppeling voor uitgebreide technische beschrijving.

## <a name="cloud-computing-overview"></a>Overzicht van cloudcomputing

Cloud computing biedt een moderne alternatief voor de traditionele on-premises datacenter. Openbare cloudleveranciers leveren en beheren van alle IT-infrastructuur en de onderliggende software voor beheer. Deze leveranciers bieden tal van cloudservices. Een service in de cloud kan in dit geval zijn een virtuele machine, een webserver of cloud-gebaseerde database-engine. Als een klant van de provider cloud lease u deze cloudservices op basis van behoefte. In dat geval, kunt u de noodzaak om kosten van hardwareonderhoud converteren naar een operationele kosten. Een cloudservice biedt ook de volgende voordelen:

- Snelle implementatie van grote rekenomgevingen

- Snelle ongedaan wordt gemaakt van systemen die niet langer vereist zijn

- Eenvoudige implementatie van oudsher complexe systemen, zoals load balancers

- Mogelijkheid om flexibele rekencapaciteit of schalen wanneer dat nodig is

- Meer rendabele computeromgevingen

- Overal toegang krijgen tot uit met een webportal of programmatische automation

- Cloud-gebaseerde services om te voldoen aan de meeste reken- en toepassingsbehoeften

Met on-premises infrastructuur hebt u volledige controle over de hardware en software die is geïmplementeerd. In het verleden heeft dit geleid tot hardware procurement beslissingen die gericht op het omhoog schalen. Een voorbeeld is een server met meer cores om te voldoen aan de prestatiebehoeften piek aanschaffen. Helaas, deze infrastructuur mogelijk volledig worden gebruikt buiten een aanvraag-venster. U kunt met Azure, implementeren, alleen de infrastructuur die u nodig hebt en dit omhoog of omlaag op elk gewenst moment aanpassen. Dit leidt tot een focus op via de implementatie van aanvullende rekenknooppunten uitschalen naar een geschikt is voor prestaties. Cloudservices schalen is rendabeler dan omhoog schalen via dure hardware.

Microsoft heeft veel Azure-datacenters over de hele wereld, met meer geplande geïmplementeerd. Microsoft neemt ook onafhankelijke clouds in regio's, zoals China en Duitsland. Alleen de grootste internationale ondernemingen kunnen datacenters op deze manier kunt implementeren, zodat met behulp van Azure kunt u eenvoudig voor bedrijven van elke grootte om hun services dicht bij hun klanten te implementeren.

Voor kleine bedrijven kunt met Azure voor een lage kosten toegangspunt met de mogelijkheid om te schalen naarmate de vraag naar rekenkracht toeneemt. Dit voorkomt dat een vooraf grote investeringen in infrastructuur, en biedt de flexibiliteit om te bouwen en opnieuw ontwerpen van systemen zo nodig. Het gebruik van cloud computing past goed met het model schaal snel en fail-fast startup groei.

Zie voor meer informatie over de beschikbare Azure-regio's, [Azure-regio's](https://azure.microsoft.com/regions/).

### <a name="cloud-computing-model"></a>Cloud computing-model

Azure maakt gebruik van een model op basis van categorieën van de service aan klanten aangeboden voor cloudcomputing. De drie categorieën van de service zijn Infrastructure as a Service (IaaS), Platform as a Service (PaaS) en Software as a Service (SaaS). Leveranciers delen sommige of alle van de verantwoordelijkheid voor onderdelen in de stack of in elk van deze categorieën. Laten we eens in elk van de categorieën voor cloud computing.
![Cloud Computing Stack vergelijking](./media/cloud-computing-comparison.png)

#### <a name="iaas-infrastructure-as-a-service"></a>IaaS: Infrastructuur als een service

Een IaaS-cloudleverancier wordt uitgevoerd en beheert alle fysieke resources en de vereiste software om in te schakelen van computer-virtualisatie. Een klant van deze service wordt geïmplementeerd voor virtuele machines in deze gehoste datacenters. Hoewel de virtuele machines bevinden zich in een datacenter op een externe locatie, heeft de IaaS-consument controle over de configuratie en beheer van het besturingssysteem verlaten van de onderliggende infrastructuur aan de cloudleverancier van de.

Azure bevat diverse IaaS-oplossingen, met inbegrip van virtuele machines, virtuele-machineschaalsets en de bijbehorende netwerkinfrastructuur. Virtuele machines zijn een populaire keuze voor het in eerste instantie migreren naar Azure-services omdat hij Hiermee een 'lift and shift'-migratie-model. U kunt een virtuele machine, zoals de infrastructuur van uw services momenteel wordt uitgevoerd in uw datacenter configureren en vervolgens uw software naar de nieuwe virtuele machine migreren. U moet mogelijk te maken van configuratie-updates, zoals URL's voor andere services of opslag, maar u vele toepassingen op deze manier kunt migreren.

Schaalsets voor virtuele machines zijn gebouwd boven op Azure Virtual Machines en bieden een eenvoudige manier om clusters van identieke VM's implementeren. Virtuele-machineschaalsets bieden ook ondersteuning voor automatisch schalen, zodat de nieuwe virtuele machines automatisch kunnen worden geïmplementeerd als vereist. Dit maakt virtuele-machineschaalsets een ideaal platform voor de host op een hoger niveau microservice rekenclusters, zoals Azure Service Fabric en Azure Container Service.

#### <a name="paas-platform-as-a-service"></a>PaaS: Platform als een service

Met PaaS implementeert u uw toepassing in een omgeving waarin de leverancier van de cloud biedt. De leverancier wordt alle infrastructuurbeheer zodat u zich op de ontwikkeling van toepassingen richten kunt en gegevens beheren.

Azure biedt dat verschillende PaaS compute-aanbiedingen, met inbegrip van de functie Web Apps van Azure App Service en Azure Cloud Services (web- en werkrollen rollen). Ontwikkelaars hebben in beide gevallen meerdere manieren voor het implementeren van hun toepassing zonder te weten niets van de nuts en bolts die dit ondersteunen. Ontwikkelaars hoeven niet te maken van virtuele machines (VM's), Remote Desktop Protocol (RDP) zich aanmeldt bij elkaar te gebruiken of de toepassing te installeren. Ze alleen klikt u op een (of deze in de buurt) en de hulpprogramma's van Microsoft voor de virtuele machines inrichten en implementeren en installeren van de toepassing op deze.

#### <a name="saas-software-as-a-service"></a>SaaS: Software als een service

SaaS is software die centraal wordt gehost en beheerd. Dit wordt meestal op basis van een architectuur met meerdere tenants, één versie van de toepassing wordt gebruikt voor alle klanten. Het kan worden uitgebreid naar meerdere exemplaren om de beste prestaties op alle locaties. SaaS-software wordt doorgaans via een abonnement maandelijkse of jaarlijkse licentie. SaaS-software wordt doorgaans via een abonnement maandelijkse of jaarlijkse licentie. SaaS-softwareleveranciers zijn verantwoordelijk voor alle onderdelen van de softwarestack zodat u beheert de services die worden geleverd.

Microsoft Office 365 is een goed voorbeeld van een SaaS-aanbieding. -Abonnees betalen een maandelijkse of jaarlijkse abonnementskosten en krijgen ze Microsoft Exchange, Microsoft OneDrive en de rest van de Microsoft Office-suite als een service. Abonnees altijd kunnen genieten van de meest recente versie en de Exchange-server wordt voor u beheerd. Vergeleken met het installeren en upgraden Office elk jaar, dit is minder duur en vereist minder moeite.

## <a name="azure-services"></a>Azure-services

Azure biedt tal van services in de cloud-platform voor cloudcomputing. Deze services omvatten het volgende.

### <a name="compute-services"></a>Compute services

Services voor hosten en uitvoeren van de workload van toepassing:

- Azure Virtual Machines-Linux- en Windows

- App-Services (Web-Apps, mobiele Apps, Logic Apps, API-Apps en functie-Apps)

- Azure Batch (voor grootschalige parallelle en batchrekentaken)

- Azure Service Fabric

- Azure Container Service

### <a name="data-services"></a>Gegevensservices

Services voor het opslaan en beheren van gegevens:

- Azure Storage (omvat de services van Azure Blob, wachtrij, tabel en bestand)

- Azure SQL Database

- Azure Cosmos DB

- Microsoft Azure StorSimple

- Azure Redis-cache

### <a name="application-services"></a>Toepassingsservices

Services voor het bouwen en gebruiken van toepassingen:

- Azure Active Directory (Azure AD)

- Azure Service Bus voor het verbinden van gedistribueerde systemen

- Azure HDInsight voor big data verwerken

- Azure Scheduler

- Azure Media Services

### <a name="network-services"></a>Netwerkservices

Services voor netwerken in Azure zowel tussen Azure en on-premises datacenters:

- Azure Virtual Network

- ExpressRoute van Azure

- Azure-provided DNS

- Azure Traffic Manager

- Azure Content Delivery Network

Zie voor gedetailleerde informatie over Azure-services, [documentatie voor Azure service](https://docs.microsoft.com/azure).

## <a name="azure-key-concepts"></a>Azure-belangrijkste concepten

### <a name="datacenters-and-regions"></a>Datacenters en regio 's

Azure is een wereldwijde cloudplatform dat is algemeen beschikbaar in veel regio's over de hele wereld. Als u een service, toepassing of virtuele machine in Azure inricht, kunt u wordt gevraagd om een regio te selecteren. De geselecteerde regio vertegenwoordigt een speciﬁc datacenter waarin uw toepassing wordt uitgevoerd. Zie voor meer informatie, [Azure-regio's](https://azure.microsoft.com/regions/).

Een van de beneﬁts van het gebruik van Azure is dat u uw toepassingen in verschillende datacenters over de hele wereld kunt implementeren. De regio die u kunt de prestaties van uw toepassing aﬀect. Het is kiezen van een regio die zich dichter bij de meeste uw klanten te verminderen latentie van netwerkaanvragen optimale. U kunt ook een regio om te voldoen aan de wettelijke vereisten voor het distribueren van uw app in bepaalde landen selecteren.

### <a name="azure-portal"></a>Azure Portal

De Azure-portal is een webgebaseerde toepassing die kan worden gebruikt om te maken, beheren en verwijderen van Azure-resources en services. De Azure portal bevindt zich in [Shell.Azure.com](https://portal.azure.com). Het bevat een aanpasbaar dashboard en hulpprogramma's voor het beheren van Azure-resources. Het biedt ook informatie over facturering en abonnementen. Zie voor meer informatie, [overzicht van Microsoft Azure portal](https://azure.microsoft.com/documentation/articles/azure-portal-overview/) en [Azure-resources beheren via portal](https://docs.microsoft.com/azure/azure-portal/resource-group-portal).

### <a name="resources"></a>Resources

Azure-resources zijn afzonderlijke compute, netwerken, gegevens of app die als host fungeert voor services die zijn geïmplementeerd in een Azure-abonnement. Sommige algemene resources zijn virtuele machines, storage-accounts of SQL-databases. Azure-services bestaan vaak uit meerdere verwante Azure-resources. Een virtuele Azure-machine kan bijvoorbeeld een virtuele machine, de storage-account, de netwerkadapter en het openbare IP-adres bevatten. Deze resources kunnen worden gemaakt, beheerd en afzonderlijk of als een groep verwijderd. Azure-resources worden behandeld in meer detail verderop in deze handleiding.

### <a name="resource-groups"></a>Resourcegroepen

Een Azure-resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. De resourcegroep kan alle resources voor de oplossing, of alleen de resources die u wilt beheren als een groep bevatten. Azure-resourcegroepen worden behandeld in meer detail verderop in deze handleiding.

### <a name="resource-manager-templates"></a>Resource Manager-sjablonen

Een Azure Resource Manager-sjabloon is een JavaScript Object Notation (JSON)-bestand dat definieert een of meer resources om te implementeren in een resourcegroep. Het definieert ook de afhankelijkheden tussen de geïmplementeerde resources. Resource Manager-sjablonen worden behandeld in meer detail verderop in deze handleiding.

### <a name="automation"></a>Automation

Naast het maken, beheren en -resources verwijderen met behulp van Azure portal, kunt u deze activiteiten automatiseren met behulp van PowerShell of de Azure-opdrachtregelinterface (CLI).

#### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell is een set van modules die cmdlets voor het beheren van Azure. U kunt de cmdlets gebruiken om te maken, beheren en verwijderen van Azure-services. De cmdlets kunt u consistente, herhaalbare en zonder implementaties kunt bereiken. Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps) voor meer informatie.

#### <a name="azure-command-line-interface"></a>Azure-opdrachtregelinterface

De opdrachtregelinterface van Azure is een hulpprogramma waarmee u kunt maken, beheren en verwijderen van Azure-resources vanaf de opdrachtregel. De Azure CLI is beschikbaar voor Linux, Mac OS X en Windows. Zie voor meer informatie en technische details [Azure CLI installeren](/cli/azure/install-azure-cli).

#### <a name="rest-apis"></a>REST-API’s

Azure is gebouwd op een set REST-API's die ondersteuning bieden voor de gebruikersinterface van Azure portal. De meeste van deze REST-API's worden ook ondersteund zodat u via een programma inrichten en beheren van uw Azure-resources en apps vanaf elk apparaat toegang tot Internet hebben. Zie voor meer informatie de [Azure REST SDK-naslaginformatie](https://docs.microsoft.com/rest/api/index).

### <a name="azure-cloud-shell"></a>Azure Cloud Shell

Beheerders hebben toegang tot Azure PowerShell en Azure CLI via een browser toegankelijk is via Azure Cloud Shell met de naam. Interactieve interface biedt een veelzijdig hulpmiddel voor Linux en Windows-beheerders het gebruik de opdrachtregelinterface van keuze, Bash of PowerShell. Azure Cloud Shell kunnen zijn op te vragen via de portal, als een zelfstandige webinterface op [shell.azure.com](https://shell.azure.com), of van een aantal andere toegangspunten. Zie voor meer informatie, [overzicht van Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

## <a name="azure-subscriptions"></a>Azure-abonnementen

Een abonnement is een logische groepering van Azure-services die is gekoppeld aan een Azure-account. Een afzonderlijke Azure-account kan meerdere abonnementen bevatten. Facturering voor Azure-services is uitgevoerd op basis van een per abonnement. Azure-abonnementen hebben een accountbeheerder, die volledige controle heeft over het abonnement en een servicebeheerder, die controle heeft over alle services in het abonnement. Naast de beheerders, individuele accounts kunnen worden verleend gedetailleerd beheer van Azure-resources via op rollen gebaseerd toegangsbeheer (RBAC).

### <a name="select-and-enable-an-azure-subscription"></a>Selecteren en inschakelen van een Azure-abonnement

Voordat u met Azure-services werken kunt, moet u een abonnement. Verschillende typen van de abonnementen zijn beschikbaar.

**Gratis accounts**: de koppeling om u te registreren voor een gratis account is op de [Azure-website](https://azure.microsoft.com/). Dit biedt u een tegoed dat u in de loop van 30 dagen om te proberen een combinatie van resources in Azure. Als u uw tegoed overschrijdt, wordt uw account is geblokkeerd. Aan het einde van de proefversie van uw services buiten gebruik zijn gesteld en werkt niet meer. U kunt upgraden naar een betalen per gebruik-abonnement op elk gewenst moment.

**MSDN-abonnementen**: als u een MSDN-abonnement hebt, krijgt u een bepaald bedrag in Azure-tegoed per maand. Bijvoorbeeld, als u een Microsoft Visual Studio Enterprise met MSDN-abonnement hebt, krijgt u \$150 per maand in de Azure-tegoed.

Als u de hoeveelheid tegoed overschrijdt, wordt uw service zijn uitgeschakeld totdat de volgende maand wordt gestart. U kunt de bestedingslimiet uitschakelen en toevoegen van een creditcard moet worden gebruikt voor de extra kosten. Sommige van deze kosten gelden kortingen voor MSDN-accounts. Bijvoorbeeld, u de Linux prijs betalen voor virtuele machines waarop Windows Server wordt uitgevoerd en er zijn geen extra kosten voor Microsoft-servers zoals Microsoft SQL Server. Dit zijn MSDN-accounts ideaal voor scenario's voor ontwikkelen en testen.

**BizSpark-accounts**: het Microsoft BizSpark-programma biedt veel voordelen voor Start-UPS. Een van deze voordelen is toegang tot alle Microsoft-software voor ontwikkel- en testomgevingen voor maximaal vijf MSDN-accounts. U $150 Azure-tegoed voor elk van deze vijf MSDN-accounts, waarna u betalen een gereduceerd tarief voor verschillende Azure-services, zoals virtuele Machines.

**Betalen per gebruik**: met dit abonnement, u betaalt voor wat u gebruikt door het koppelen van een creditcard of betaalpas aan het account. Als u een organisatie, kunt u ook worden goedgekeurd voor facturering.

**Enterprise-overeenkomsten**: met een enterprise agreement, die u doorvoert voor het gebruik van een bepaald aantal services in Azure in het volgende jaar, en u die hoeveelheid tevoren betaalt. De toezegging die u aanbrengt, wordt gedurende het jaar verbruikt. Als u het toezeggingsbedrag overschrijdt, kunt u de overschrijding te betalen achteraf in rekening gebracht. Afhankelijk van de grootte van de toezegging krijgt u een korting op de services in Azure.

### <a name="grant-administrative-access-to-an-azure-subscription"></a>Beheerderstoegang verlenen tot een Azure-abonnement

Meerdere account beheerder functies zijn beschikbaar en kunnen op elk gewenst moment worden gewijzigd. Er zijn twee belangrijke functies:

- **Servicebeheerder**: deze rol is gemachtigd voor het beheren van Azure-services. Standaard krijgt het toegang tot hetzelfde account zijn als de accountbeheerder.

- **CO-beheerder**: deze rol heeft dezelfde toegangsrechten als de service-beheerder. Deze rol kan echter de koppeling van een abonnement wijzigen naar Azure-mappen.

Zie voor meer informatie, [toevoegen of wijzigen van de Azure-beheerdersrollen](../../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="view-billing-information-in-the-azure-portal"></a>Factureringsgegevens bekijken in de Azure portal

Een belangrijk onderdeel van het gebruik van Azure is de mogelijkheid om factuurgegevens weer te geven. De Azure-portal biedt gedetailleerde informatie over Azure-Factureringsinformatie.

Zie voor meer informatie, [het downloaden van uw Azure-factuur en de dagelijkse gebruiksgegevens](../../billing/billing-download-azure-invoice-daily-usage-date.md).

### <a name="get-billing-information-from-billing-apis"></a>Facturering-gegevens ophalen uit de facturering van API 's

Naast de facturering in de portal bekijkt, kunt u toegang tot de informatie over facturering met behulp van een script of het programma via de REST API's van de Azure-facturering:

- U kunt de API voor het gebruik van Azure gebruiken om op te halen van de gebruiksgegevens. U kunt de facturering informatie over het gebruik afstemmen door te labelen gerelateerde Azure-resources. U kunt bijvoorbeeld taggen elk van de resources in een resourcegroep met een naam van afdeling of de naam van het project en klikt u vervolgens bijhouden van de kosten speciaal voor één code.

- U kunt de Azure tarief kaart API gebruiken om alle beschikbare resources, samen met de metagegevens en informatie over elk van deze resources prijzen weer te geven.

Zie voor meer informatie, [inzicht in het gebruik van de Microsoft Azure-resources](../../billing/billing-usage-rate-card-overview.md).

### <a name="forecast-cost-with-the-pricing-calculator"></a>Prognose kosten met de prijscalculator

De prijzen voor elke service in Azure is anders. Veel Azure-services bieden Basic, Standard en Premium-lagen. Elke categorie heeft meestal verschillende niveaus van prijs- prestatieverhouding. Met behulp van de [online prijscalculator](http://azure.microsoft.com/pricing/calculator), kunt u schattingen voor prijzen. De Rekenmachine bevat de flexibiliteit om kosten op een enkele bron of een groep resources te schatten.

### <a name="set-up-billing-alerts"></a>Meldingen voor facturering instellen

Nadat u uw toepassing of oplossing op Azure hebt geïmplementeerd, kunt u waarschuwingen die dat u een e-mail verzendt wanneer u benadering bestedingslimieten gedefinieerd in de waarschuwing kunt maken. Zie voor meer informatie, [waarschuwingen voor uw Microsoft Azure-abonnementen voor facturering instellen](../../billing/billing-set-up-alerts.md).

## <a name="azure-resource-manager"></a>Azure Resource Manager

Azure Resource Manager is een implementatie, beheer en organisatie-mechanisme voor Azure-resources. Met behulp van Resource Manager, kunt u veel afzonderlijke resources samen plaatsen in een resourcegroep.

Resource Manager bevat ook implementatiemogelijkheden die voor aanpasbare implementatie en configuratie van de gerelateerde resources zorgen. U kunt bijvoorbeeld een toepassing die uit meerdere virtuele machines, een load balancer en een SQL-database als één eenheid bestaat implementeren met behulp van Resource Manager. U kunt deze implementaties ontwikkelen met behulp van Resource Manager-sjabloon.

Resource Manager biedt diverse voordelen:

- U kunt alle resources voor uw oplossing implementeren, beheren en bewaken als groep, in plaats van deze resources afzonderlijk te verwerken.

- U kunt herhaaldelijk gedurende de ontwikkelingscyclus uw oplossing implementeren, en u kunt vertrouwen dat uw resources worden geïmplementeerd in een consistente status hebben.

- U kunt uw infrastructuur beheren via declaratieve sjablonen in plaats van scripts.

- U kunt de afhankelijkheden tussen resources zo definiëren dat deze in de juiste volgorde worden geïmplementeerd.

- U kunt toegangsbeheer toepassen op alle services in uw resourcegroep omdat RBAC is geïntegreerd in het beheerplatform.

- U kunt tags toepassen op resources om alle resources in uw abonnement logische manier te organiseren.

- U kunt facturering voor uw organisatie verduidelijken door te kijken naar de kosten voor een groep resources die dezelfde tag delen.

### <a name="tips-for-creating-resource-groups"></a>Tips voor het maken van resourcegroepen

Wanneer u bent het nemen van besluiten over uw resourcegroepen, houd rekening met de volgende tips:

- Alle resources in een resourcegroep, moeten dezelfde levenscyclus hebben.

- U kunt een resource toewijzen aan slechts één groep tegelijk.

- U kunt toevoegen of verwijderen van een resource van een resourcegroep op elk gewenst moment. Elke resource moet behoren tot een resourcegroep. Dus als u een resource van de ene groep verwijdert, u deze aan een andere toevoegen moet.

- U kunt de meeste typen resources verplaatsen naar een andere resourcegroep op elk gewenst moment.

- De resources in een resourcegroep kunnen zich in verschillende regio's.

- U kunt een resourcegroep gebruiken voor het beheren van toegang voor de resources in deze.

### <a name="building-resource-manager-templates"></a>Resource Manager-sjablonen maken

Resource Manager-sjablonen definiëren declaratief de resources en resourceconfiguraties die worden geïmplementeerd in één resourcegroep bestaan. U kunt Resource Manager-sjablonen gebruiken voor het indelen van complexe implementaties zonder de noodzaak voor overtollige uitvoeren van scripts of handmatige configuratie. Nadat u een sjabloon ontwikkelt, kunt u deze implementeren meerdere keren, telkens met een identieke uitkomst.

Resource Manager-sjabloon bestaat uit vier secties:

- **Parameters**: dit zijn de invoer voor de implementatie. Parameterwaarden kunnen worden geleverd door een mens of een geautomatiseerd proces. Een voorbeeld van de parameter is mogelijk een beheerdersnaam en het wachtwoord voor een virtuele Windows-machine. De parameterwaarden worden gebruikt tijdens de implementatie als ze zijn opgegeven.

- **Variabelen**: deze worden gebruikt voor het opslaan van waarden die worden gebruikt tijdens de implementatie. In tegenstelling tot parameters, is een variabele waarde niet opgegeven tijdens de implementatie. In plaats daarvan is het moeilijk gecodeerd of dynamisch gegenereerd.

- **Resources**: in deze sectie van de sjabloon definieert de resources om te worden geïmplementeerd, zoals virtuele machines, opslagaccounts en virtuele netwerken.

- **Uitvoer**: nadat een implementatie is voltooid, Resource Manager gegevens, zoals dynamisch gegenereerde verbindingsreeksen kunt terugkeren.

De volgende mechanismen zijn beschikbaar voor automatisering van implementatie:

- **Functies**: U kunt verschillende functies gebruiken in Resource Manager-sjablonen. Hieronder vallen bewerkingen zoals het converteren van een tekenreeks naar kleine letters, implementatie van meerdere exemplaren van een bepaalde resource en de doelresourcegroep dynamisch te retourneren. Resource Manager-functies kunnen opbouwen van dynamische implementaties.

- **Bronafhankelijkheden**: wanneer u meerdere resources, sommige resources implementeert, heeft een afhankelijkheid op de andere. Om te kunnen implementeren, kunt u een afhankelijkheidsverklaring zodat afhankelijke resources worden geïmplementeerd voordat u de andere.

- **Sjabloon koppelen**: vanuit binnen één Resource Manager-sjabloon, u kunt een koppeling naar een andere sjabloon. Hiermee wordt uitgevouwen implementatie in een set van gerichte, doel-specifieke sjablonen.

U kunt Resource Manager-sjablonen in een teksteditor kunt bouwen. De Azure SDK voor Visual Studio bevat echter hulpprogramma's om u te helpen. Met behulp van Visual Studio, kunt u resources toevoegen aan de sjabloon die via een wizard, en vervolgens implementeren en fouten opsporen in de sjabloon rechtstreeks vanuit Visual Studio. Zie voor meer informatie, [Authoring Azure Resource Manager-sjablonen](../../resource-group-authoring-templates.md).

Ten slotte kunt u bestaande resourcegroepen converteren naar een herbruikbare sjabloon vanuit Azure portal. Dit kan nuttig zijn als u wilt maken van een implementeerbare sjabloon van een bestaande resourcegroep of u alleen wilt controleren van de onderliggende JSON. Voor het exporteren van een resourcegroep, selecteer de **automatiseringsscript** knop van instellingen voor de resourcegroep.

## <a name="security-of-azure-resources-rbac"></a>Beveiliging van Azure-resources (RBAC)

U kunt operationele toegang verlenen tot accounts voor gebruikers met een opgegeven bereik: abonnement, resourcegroep of afzonderlijke resource. Dit betekent dat u kunt een set met resources in een resourcegroep, zoals een virtuele machine en alle gerelateerde resources implementeren en machtigingen verlenen aan een specifieke gebruiker of groep. Deze aanpak beperkt tot alleen de resources die deel uitmaken van de doelresourcegroep. U kunt ook toegang verlenen tot een enkele resource, zoals een virtuele machine of een virtueel netwerk.

Om toegang te verlenen, kunt u een rol toewijzen aan de gebruiker of groep. Er zijn veel vooraf gedefinieerde rollen. U kunt ook uw eigen aangepaste rollen definiëren.

Hier volgen enkele voorbeeld van de rollen die zijn ingebouwd in Azure:

- **De eigenaar van**: een gebruiker met deze rol kan alles beheren, inclusief toegang.

- **Lezer**: een gebruiker met deze rol kunnen bronnen van alle typen (met uitzondering van geheimen) lezen, maar kan geen wijzigingen aanbrengen.

- **Inzender voor virtuele machines**: een gebruiker met deze rol virtuele machines kunt beheren, maar niet het virtuele netwerk te beheren die zijn verbonden of het opslagaccount waarin het VHD-bestand zich bevindt.

- **' SQL DB Contributor '**: een gebruiker met deze rol SQL-databases, maar niet hun beveiligingsbeleid kunt beheren.

- **SQL-beveiligingsbeheer**: een gebruiker met deze rol kan het beveiligingsbeleid van de SQL-servers en databases beheren.

- **Inzender voor opslagaccounts**: een gebruiker met deze rol kan opslagaccounts beheren, maar niet de toegang tot de storage-accounts beheren.

Zie voor meer informatie, [roltoewijzingen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](../../role-based-access-control/role-assignments-portal.md).

## <a name="azure-virtual-machines"></a>Azure Virtual Machines

Virtuele Machines van Azure is een van de centrale IaaS-services in Azure. Virtuele Machines van Azure biedt ondersteuning voor de implementatie van Windows of Linux-machines in een Microsoft Azure-datacenter. Met Azure Virtual Machines die u hebt volledige controle over de VM-configuratie en zijn verantwoordelijk voor alle software-installatie, configuratie en onderhoud.

Wanneer u een virtuele Azure-machine implementeert, kunt kunt u een installatiekopie van de Azure Marketplace, of u u eigen gegeneraliseerde installatiekopie. Deze installatiekopie wordt gebruikt om het besturingssysteem en de eerste configuratie van toepassing. Resource Manager wordt tijdens de implementatie van bepaalde configuratie-instellingen, zoals het toewijzen van de computernaam, beheerdersreferenties en configuratie van het netwerk worden verwerkt. Extensies van virtuele machine van Azure kunt u configuraties, zoals software-installatie, configuratie van antivirus en bewakingsoplossingen verder te automatiseren.

U kunt virtuele machines maken in veel verschillende grootten. De grootte van virtuele machine bepaalt de toewijzing van resources, zoals rekenkracht, geheugen en de storage-capaciteit. In sommige gevallen zijn specifieke functies zoals RDMA netwerkadapters en SSD-schijven zijn alleen beschikbaar voor bepaalde VM-grootten. Zie voor een volledige lijst van de VM-grootten en mogelijkheden, 'Grootten voor virtuele machines in Azure' voor [Windows](../../virtual-machines/windows/sizes.md) en [Linux](../../virtual-machines/linux/sizes.md).

### <a name="use-cases"></a>Gebruiksvoorbeelden

Omdat Azure-machines volledige controle over de configuratie bieden, zijn ze ideaal voor een breed scala aan server-workloads die niet in een PaaS-model passen. Server-workloads, zoals de database-servers (SQL Server, Oracle of MongoDB), Windows Server Active Directory, Microsoft SharePoint en veel meer worden mogelijk uit te voeren op de Microsoft Azure-platform. Indien gewenst, kunt u deze werkbelastingen verplaatsen van een on-premises datacenter naar een of meer Azure-regio's, zonder dat een grote hoeveelheid herconfiguratie.

### <a name="deployment-of-virtual-machines"></a>Implementatie van virtuele machines

U kunt virtuele machines van Azure kunt implementeren met behulp van Azure portal, met behulp van automation met de Azure PowerShell-module of met behulp van automation met de platformoverschrijdende CLI.

#### <a name="portal"></a>Portal

Een virtuele machine met behulp van Azure portal implementeert, moet alleen een actief Azure-abonnement en de toegang tot een webbrowser. U kunt installatiekopieën van veel verschillende besturingssystemen met verschillende configuraties kunt selecteren. Alle opslag- en netwerkvereisten zijn geconfigureerd tijdens de implementatie. Zie voor meer informatie 'Een virtuele machine in Azure portal maken' voor [Windows](../../virtual-machines/windows/quick-create-portal.md) en [Linux](../../virtual-machines/linux/quick-create-portal.md).

Naast het implementeren van een virtuele machine van Azure portal, kunt u een Azure Resource Manager-sjabloon uit de portal implementeren. Dit implementeren en configureren van alle resources, zoals gedefinieerd in de sjabloon. Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure portal](../../azure-resource-manager/resource-group-template-deploy-portal.md).

#### <a name="powershell"></a>PowerShell

Kan een virtuele Azure-machine implementeren met behulp van PowerShell voor het automatiseren van de volledige implementatie van alle verwante virtuele machine resources, met inbegrip van opslag en netwerken. Zie voor meer informatie, [een Windows VM maken met Resource Manager en PowerShell](../../virtual-machines/windows/quick-create-powershell.md).

Naast het Azure-rekenresources afzonderlijk implementeren, kunt u de Azure PowerShell-module om een Azure Resource Manager-sjabloon te implementeren. Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md).

#### <a name="command-line-interface-cli"></a>Opdrachtregelinterface (CLI)

Net als bij de PowerShell-module, de Azure-opdrachtregelinterface biedt automatisering van de implementatie en kan worden gebruikt op Windows, OS X of Linux-systemen. Wanneer u de Azure CLI **vm snel maken** opdracht, alle gerelateerde resources van de virtuele machine (met inbegrip van opslag en netwerken) en de virtuele machine zelf worden geïmplementeerd. Zie voor meer informatie, [een Linux-VM maken in Azure met behulp van de CLI](../../virtual-machines/linux/quick-create-cli.md).

U kunt ook de Azure CLI gebruiken om een Azure Resource Manager-sjabloon te implementeren. Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md).

### <a name="access-and-security-for-virtual-machines"></a>Toegang en beveiliging voor virtuele machines

Een virtuele machine benaderen via Internet moet de bijbehorende netwerk interface of load balancer indien van toepassing is, kan worden geconfigureerd met een openbaar IP-adres. Het openbare IP-adres bevat een DNS-naam die wordt omgezet in de virtuele machine of load balancer. Zie voor meer informatie, [IP-adressen in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).

U kunt toegang tot de virtuele machine beheren via het openbare IP-adres met behulp van een resource network security group (NSG). Een NSG fungeert als een firewall en verkeer toestaat of weigert via netwerkinterface of een subnet in een set van gedefinieerde poorten. Voor het maken van een extern-bureaubladsessie met een Azure-VM, moet u bijvoorbeeld het configureren van de NSG om toe te staan van binnenkomend verkeer op poort 3389. Zie voor meer informatie, [poorten openen voor een virtuele machine in Azure met behulp van de Azure-portal](../../virtual-machines/windows/nsg-quickstart-portal.md).

Ten slotte, net als bij het beheer van elk computersysteem, moet u zelf beveiliging voor een Azure-machine op het besturingssysteem met behulp van beveiligingsreferenties en softwarefirewalls.

## <a name="azure-storage"></a>Azure Storage

Azure Storage is een service voor Microsoft beheerde die duurzaam, schaalbaar en redundant storage biedt. U kunt een Azure storage-account toevoegen als een resource naar een resourcegroep met behulp van de implementatiemethode van elke resource. Azure bevat vier opslagtypen: Blob storage, File Storage, Table storage en Queue storage. Bij het implementeren van een storage-account, worden de twee accounttypen beschikbaar zijn voor algemeen gebruik en blob-opslag. Een algemeen opslagaccount hebt u toegang tot alle vier opslagtypen. BLOB storage-accounts zijn vergelijkbaar met accounts voor algemeen gebruik, maar gespecialiseerde blobs met warme en koude toegangslagen bevatten. Zie voor meer informatie over blob-opslag, [Azure Blob-opslag](../../storage/blobs/storage-blob-storage-tiers.md).

Azure storage-accounts kunnen worden geconfigureerd met verschillende niveaus van redundantie:

- **Lokaal redundante opslag** zorgt voor hoge beschikbaarheid door ervoor te zorgen dat drie kopieën van alle gegevens synchroon zijn gedaan voordat een schrijfbewerking wordt als voltooid beschouwd. Deze kopieën worden opgeslagen in één faciliteit in één regio. De replica's bevinden zich in afzonderlijke foutdomeinen en upgradedomeinen. Dit betekent dat de gegevens zijn beschikbaar, zelfs als een opslagknooppunt die van uw gegevens mislukt bezit is of een offline te worden bijgewerkt.

- **Geografisch redundante opslag** zorgt ervoor dat drie synchrone kopieën van de gegevens in de primaire regio voor hoge beschikbaarheid, en vervolgens asynchroon drie replica's in een gekoppelde regio voor herstel na noodgevallen.

- **Geografisch redundante opslag met leestoegang** is geografisch redundante opslag plus de mogelijkheid om de gegevens in de secundaire regio te lezen. Deze mogelijkheid is het geschikt is voor gedeeltelijke noodherstel. Als er een probleem met de primaire regio, kunt u uw toepassing kunnen alleen-lezen toegang hebben tot de gekoppelde regio.

### <a name="use-cases"></a>Gebruiksvoorbeelden

Elk opslagtype heeft een verschillende use case.

#### <a name="blob-storage"></a>Blob Storage

Het woord *blob* is een acroniem voor *BLOB*. BLOBs zijn niet-gestructureerde bestanden zoals u die u op uw computer opslaat. In Blob Storage kan elk type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstallatieprogramma, worden opgeslagen. U kunt Blob Storage zien als een vorm van objectopslag. Azure Blob-opslag bevat ook gegevensschijven van virtuele Machines van Azure.

Azure Storage ondersteunt drie typen blobs:

- **Blok-blobs** worden gebruikt voor het opslaan van gewone bestanden tot 195 GB groot (4 MB x 50.000 blokken). De primaire use-case voor blok-blobs is de opslag van bestanden die worden gelezen van begin tot eind, zoals mediabestanden of afbeeldingsbestanden voor websites. Ze zijn blok-blobs met de naam omdat bestanden die groter zijn dan 64 MB moeten worden geüpload als kleine blokken. Deze blokken worden vervolgens geconsolideerd (of toegewezen) in de laatste blob.

- **Pagina-blobs** worden gebruikt voor het opslaan van willekeurige toegang tot 1 TB in grootte-bestanden. Pagina-blobs gebruikt voornamelijk als de opslag van back-ups maken voor de VHD's die duurzame schijven voor Azure Virtual Machines bieden, het IaaS compute-service in Azure. Ze zijn pagina-blobs genoemd omdat ze voorzien in willekeurige lees-/ schrijftoegang tot 512-byte-pagina's.

- **Toevoeg-blobs** bestaan uit blokken zoals blok-blobs, maar ze zijn geoptimaliseerd voor toevoegbewerkingen. Deze worden vaak gebruikt voor logboekregistratie informatie van een of meer bronnen in dezelfde blob. Bijvoorbeeld, kan u al uw traceerlogboekregistratie schrijven naar de dezelfde toevoeg-blob voor een toepassing die wordt uitgevoerd op meerdere virtuele machines. Eén toevoeg-blob kan maximaal 195 GB zijn.

Zie voor meer informatie, [aan de slag met Azure Blob storage met .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md).

#### <a name="file-storage"></a>File Storage

Azure File storage is een service die bestandsshares in de cloud aanbiedt met behulp van het standaardprotocol voor Server Message Block (SMB). De service ondersteunt zowel SMB 2.1 als SMB 3.0. Met Azure File storage, kunt u toepassingen die afhankelijk van bestandsshares naar Azure snel en zonder kostbare regeneraties zijn migreren. Toepassingen worden uitgevoerd op Azure virtual machines, cloudservices of on-premises clients kunnen een bestandsshare koppelen in de cloud. Dit is vergelijkbaar met hoe een bureaubladtoepassing een gewone SMB-share koppelt. Daarna kan een willekeurig aantal toepassingsonderdelen de File Storage-share tegelijkertijd koppelen en gebruiken.

Omdat een File storage-share een standaard SMB-bestandsshare is, kunnen de toepassingen die in Azure worden uitgevoerd toegang tot gegevens in de share via I/O APIs het bestandssysteem. Ontwikkelaars kunnen daarom hun bestaande code en vaardigheden gebruiken om bestaande toepassingen te migreren. IT-professionals kunnen PowerShell-cmdlets gebruiken om te maken, koppelen en beheren van bestandsshares voor opslag als onderdeel van het beheer van Azure-toepassingen.

Zie voor meer informatie, [aan de slag met Azure File storage in Windows](../../storage/files/storage-how-to-use-files-windows.md) of [Azure File storage gebruiken met Linux](../../storage/files/storage-how-to-use-files-linux.md).

#### <a name="table-storage"></a>Table Storage

Azure Table Storage is een service waarmee gestructureerde NoSQL-gegevens worden opgeslagen in de cloud. Tabelopslag is een sleutel-/ kenmerkopslag met een ontwerp zonder schema. Omdat Table storage zonder schema is, is het uw kunt gegevens eenvoudig aanpassen naarmate de behoeften van uw toepassing veranderen. De toegang tot gegevens verloopt snel en kostenefficiënt voor alle soorten toepassingen. Table Storage is doorgaans aanzienlijk goedkoper dan traditionele SQL voor vergelijkbare gegevensvolumes.

U kunt Table Storage gebruiken voor het opslaan van flexibele gegevenssets, zoals gebruikersgegevens voor webtoepassingen, adresboeken, apparaatgegevens en alle overige typen metagegevens die uw service nodig heeft. U kunt een willekeurig aantal entiteiten opslaan in een tabel. Een opslagaccount kan een onbeperkt aantal tabellen, tot de capaciteitslimiet van het opslagaccount bevatten.

Zie voor meer informatie, [aan de slag met Azure Table storage](../../cosmos-db/table-storage-how-to-use-dotnet.md).

#### <a name="queue-storage"></a>Queue Storage

Azure Queue Storage biedt uitwisseling van berichten tussen toepassingsonderdelen in de cloud. Bij het ontwerpen van toepassingen voor schaling, worden toepassingsonderdelen vaak losgekoppeld zodat ze kunnen onafhankelijk worden geschaald. Queue Storage biedt asynchrone uitwisseling van berichten voor communicatie tussen toepassingsonderdelen, of deze nu worden uitgevoerd in de cloud, op een desktopcomputer, op een on-premises server of op een mobiel apparaat. Queue Storage biedt ook ondersteuning voor het beheren van asynchrone taken en het samenstellen van proceswerkstromen.

Zie voor meer informatie, [aan de slag met Azure Queue storage](../../storage/queues/storage-dotnet-how-to-use-queues.md).

### <a name="deploying-a-storage-account"></a>Implementatie van een storage-account

Er zijn verschillende opties voor het implementeren van een storage-account.

#### <a name="portal"></a>Portal

Een storage-account implementeren met behulp van de Azure-portal vereist alleen een actief Azure-abonnement en de toegang tot een webbrowser. U kunt een nieuw opslagaccount implementeren in een nieuwe of bestaande resourcegroep. Nadat u de storage-account hebt gemaakt, kunt u een blob-container of bestandsshare maken met behulp van de portal. U kunt de tabel maken en storage-entiteiten via een programma in de wachtrij. Zie voor meer informatie, [een opslagaccount maken](../../storage/common/storage-quickstart-create-account.md).

Naast het implementeren van een storage-account vanuit Azure portal, kunt u een Azure Resource Manager-sjabloon uit de portal implementeren. Dit implementeren en configureren van alle resources, zoals gedefinieerd in de sjabloon, met inbegrip van alle opslagaccounts. Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure portal](../../azure-resource-manager/resource-group-template-deploy-portal.md).

#### <a name="powershell"></a>PowerShell

Een Azure storage-account implementeren met behulp van PowerShell kunt voor het automatiseren van de volledige implementatie van het opslagaccount. Zie voor meer informatie, [Azure PowerShell gebruiken met Azure Storage](../../storage/common/storage-powershell-guide-full.md).

Naast het implementeren van afzonderlijke Azure-resources, kunt u de Azure PowerShell-module om een Azure Resource Manager-sjabloon te implementeren. Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md).

#### <a name="command-line-interface-cli"></a>Opdrachtregelinterface (CLI)

Net als bij de PowerShell-module, de Azure-opdrachtregelinterface biedt automatisering van de implementatie en kan worden gebruikt op Windows, OS X of Linux-systemen. U kunt de Azure CLI **storage-account maken** opdracht om een opslagaccount te maken. Zie voor meer informatie, [met de Azure CLI met Azure Storage.](../../storage/common/storage-azure-cli.md)

U kunt ook de Azure CLI gebruiken om een Azure Resource Manager-sjabloon te implementeren. Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure CLI](../../resource-group-template-deploy-cli.md).

### <a name="access-and-security-for-azure-storage"></a>Toegang en beveiliging voor Azure Storage

Azure Storage is toegankelijk op verschillende manieren, waaronder via Azure portal tijdens het maken van VM en het gebruik en van Storage-clientbibliotheken.

#### <a name="virtual-machine-disks"></a>Schijven van virtuele machines

Wanneer u een virtuele machine implementeert, moet u ook om een opslagaccount voor het opslaan van de schijf van de virtuele machine-besturingssysteem en eventuele extra gegevensschijven te maken. U kunt een bestaand opslagaccount selecteren of een nieuwe maken. Omdat de maximale grootte van een blob 1.024 GB is, heeft één VM-schijf een maximale grootte van 1023 GB. U kunt meerdere gegevensschijven aan de virtuele machine aanwezig en pool ze samen als één logische schijf voor het configureren van een grotere gegevensschijf. Zie voor meer informatie "beheren Azure-schijven ' voor [Windows](../../virtual-machines/windows/tutorial-manage-data-disk.md) en [Linux](../../virtual-machines/linux/tutorial-manage-disks.md).

#### <a name="storage-tools"></a>Hulpprogramma's van Storage

Azure storage-accounts kunnen worden geopend via veel verschillende opslagverkenners, zoals Visual Studio Cloud Explorer. Deze hulpprogramma's kunnen u bladeren door storage-accounts en gegevens. Zie voor meer informatie en een lijst met beschikbare opslagverkenners [Azure Storage-clienthulpprogramma's](../../storage/common/storage-explorers.md).

#### <a name="storage-api"></a>Storage-API

Storage-resources zijn toegankelijk via elke taal waarmee HTTP/HTTPS-aanvragen. Daarnaast biedt Azure Storage programmeringsbibliotheken voor verschillende veelgebruikte talen. Deze bibliotheken vereenvoudigen werken met Azure Storage door details zoals synchrone en asynchrone aanroepafhandeling, batchverwerking van bewerkingen, Uitzonderingsbeheer en automatische nieuwe pogingen. Zie voor meer informatie, [naslaginformatie over REST API van Azure Storage-service](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference).

#### <a name="storage-access-keys"></a>Toegangssleutels voor opslag

Elk opslagaccount heeft twee verificatiesleutels, een primaire en een secundaire. Een kan worden gebruikt voor toegang-opslagbewerkingen. Deze opslagsleutels worden gebruikt voor het beveiligen van een storage-account en zijn vereist voor het programmatisch toegang tot gegevens. Er zijn twee sleutels om toe te staan van incidentele rollover van de sleutels om beveiliging te verbeteren. Het is essentieel dat de sleutels veilig houden omdat hun bezit, samen met de accountnaam, onbeperkte toegang tot gegevens in het opslagaccount dat toestaat.

#### <a name="shared-access-signatures"></a>Handtekeningen voor gedeelde toegang

Als u toestaan dat gebruikers toegang tot uw opslagresources hebt gecontroleerd wilt, kunt u een shared access signature maken. Een shared access signature is een token die kan worden toegevoegd aan een URL waarmee gedelegeerde toegang tot een opslagresource. Iedereen die de token in bezit hebben toegang tot de resource waarnaar wordt verwezen met de machtigingen die deze Hiermee geeft u de voor de periode dat het is ongeldig. Zie voor meer informatie, [voor gedeelde toegangshandtekeningen](../../storage/common/storage-dotnet-shared-access-signature-part-1.md).

## <a name="azure-virtual-network"></a>Azure Virtual Network

Virtuele netwerken zijn nodig voor het ondersteunen van communicatie tussen virtuele machines. U kunt definiëren van subnetten, aangepaste IP-adres, DNS-instellingen, Beveiligingsfiltering en taakverdeling. Azure biedt ondersteuning voor verschillende gebruiksvoorbeelden: netwerken alleen in de cloud of hybride virtuele netwerken.

### <a name="cloud-only-virtual-networks"></a>Alleen in de cloud virtuele netwerken

Een Azure-netwerk standaard is alleen toegankelijk voor resources die zijn opgeslagen in Azure. Resources die zijn verbonden met hetzelfde virtuele netwerk kunnen met elkaar communiceren. U kunt koppelen van netwerkinterfaces van virtuele machines en load balancers met een openbaar IP-adres aan de virtuele machine via Internet toegankelijk maken. U kunt beveiligde toegang tot de publiekelijk blootgestelde resources met behulp van een netwerkbeveiligingsgroep.

![Azure-netwerk voor een tier-2-webtoepassing](https://docs.microsoft.com/azure/load-balancer/media/load-balancer-internal-overview/ic744147.png)

### <a name="hybrid-virtual-networks"></a>Hybride virtuele netwerken

U kunt een on-premises netwerk verbinding maken met een Azure-netwerk via ExpressRoute of site-naar-site VPN-verbinding. In deze configuratie is Azure virtual network in feite een cloud-gebaseerde uitbreiding van uw on-premises netwerk.
![Hybride Virtueelnetwerk met behulp van VPN](https://docs.microsoft.com/azure/architecture/reference-architectures/_images/blueprints/hybrid-network-vpn.png)

Omdat het Azure-netwerk is verbonden met uw on-premises netwerk, cross-premises virtuele netwerken moeten een unieke deel van de adresruimte die gebruikmaakt van uw organisatie gebruiken. Azure wordt een andere locatie op dezelfde manier die verschillende zakelijke locaties met een specifiek IP-subnet worden toegewezen, wanneer u uw netwerk uitbreidt.
Er zijn verschillende opties voor het implementeren van een virtueel netwerk.

- [Portal](../..//virtual-network/quick-create-portal.md)

- [PowerShell](../../virtual-network/quick-create-powershell.md)

- [Opdrachtregelinterface (CLI)](../../virtual-network/quick-create-cli.md)

- Azure Resource Manager-sjablonen

>**Wanneer u**: telkens wanneer u met virtuele machines in Azure werkt, werkt u met virtuele netwerken. Hiermee wordt voor het segmenteren van uw virtuele machines in openbare en particuliere subnetten vergelijkbaar on-premises datacenters.

>**Aan de slag**: een Azure-netwerk implementeren met behulp van de Azure-portal vereist alleen een actief Azure-abonnement en de toegang tot een webbrowser. U kunt een nieuw virtueel netwerk implementeren in een nieuwe of bestaande resourcegroep. Wanneer u een nieuwe virtuele machine vanuit de portal maakt, kunt u een bestaand virtueel netwerk selecteren of een nieuwe maken. Aan de slag en [een virtueel netwerk maken met de Azure-portal](../../virtual-network/quick-create-portal.md).

### <a name="access-and-security-for-virtual-networks"></a>Toegang en beveiliging voor virtuele netwerken

U kunt beveiligde Azure-netwerken met behulp van een netwerkbeveiligingsgroep. Nsg's bevatten een lijst van de toegangsbeheerlijst (ACL) toegangsbeheerregels waarmee toestaan of weigeren van netwerkverkeer naar uw VM-exemplaren in een virtueel netwerk. U kunt nsg's koppelen aan subnetten of afzonderlijke VM-exemplaren in dat subnet. Als u een NSG aan een subnet koppelt, worden de ACL-regels toepassen op alle VM-exemplaren in dat subnet. U kunt bovendien verder verkeer beperken tot een afzonderlijke virtuele machine door het koppelen van een NSG rechtstreeks aan die virtuele machine. Zie voor meer informatie, [netwerkverkeer filteren met netwerkbeveiligingsgroepen](../../virtual-network/security-overview.md).

## <a name="next-steps"></a>Volgende stappen

- [Een Windows VM maken](../../virtual-machines/windows/quick-create-portal.md)
- [Een Linux-VM maken](../../virtual-machines/linux/quick-create-portal.md)
