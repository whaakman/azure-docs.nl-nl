---
title: Introductiehandleiding voor ontwikkelaars op Azure | Microsoft Docs
description: Dit onderwerp bevat essentiële informatie voor ontwikkelaars die aan de slag met behulp van de Microsoft Azure-platform voor hun ontwikkeling.
services: ''
cloud: ''
documentationcenter: ''
author: ggailey777
manager: erikre
ms.assetid: ''
ms.service: na
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: glenga
ms.openlocfilehash: 2c6db85763b448133d53b22c0600b27b533b2041
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39424225"
---
# <a name="get-started-guide-for-azure-developers"></a>Aan de slag-handleiding voor Azure-ontwikkelaars

## <a name="what-is-azure"></a>Wat is Azure?

Azure is een compleet cloudplatform dat u kunt uw bestaande toepassingen hosten, de ontwikkeling van nieuwe toepassingen stroomlijnen en zelfs on-premises toepassingen te verbeteren. Azure integreert het cloud-services die u wilt ontwikkelen, testen, implementeren en beheren van uw toepassingen, terwijl u profiteert van de efficiëntie van cloud computing.

U kunt uw toepassingen in Azure hosten, begin klein en eenvoudig uw toepassing schalen als uw vraag van klanten groeit. Azure biedt ook de betrouwbaarheid die nodig voor hoge beschikbaarheid-toepassingen is, waaronder zelfs failover tussen verschillende regio's. De [Azure-portal](https://portal.azure.com) kunt u eenvoudig alle uw Azure-services te beheren. U kunt uw services ook programmatisch beheren met behulp van service-API's en sjablonen.

**Wie dit**: in deze handleiding bevat een inleiding tot het Azure-platform voor ontwikkelaars van toepassingen. Het biedt richtlijnen en de richting die u nodig hebt om te beginnen het bouwen van nieuwe toepassingen in Azure of bestaande toepassingen naar Azure migreert.

## <a name="where-do-i-start"></a>Waar moet ik beginnen?

Bij alle services die Azure biedt, mag geen sinecure om te achterhalen welke services u wilt uw oplossingsarchitectuur ondersteunen. In deze sectie worden de Azure-services die ontwikkelaars gewoonlijk gebruiken. Zie voor een lijst van alle Azure-services, de [documentatie voor Azure](../../index.md).

Eerst moet u bepalen hoe u voor het hosten van uw toepassing in Azure. U hoeft voor het beheren van uw volledige infrastructuur als een virtuele machine (VM). Kunt u de faciliteiten voor het beheer van platform die Azure biedt? Mogelijk moet u een serverloos framework host code kan worden uitgevoerd alleen?

De toepassing nodig heeft voor cloudopslag, die Azure biedt verschillende opties voor. U kunt profiteren van de Azure enterprise-verificatie. Er zijn ook hulpprogramma's voor ontwikkeling van cloud-gebaseerde en bewaking en de meeste hostingservices integratie met DevOps bieden.

Nu gaan we bekijken een van de specifieke services die wij adviseren onderzoeken voor uw toepassingen.

### <a name="application-hosting"></a>Hosting voor toepassingen

Azure biedt dat verschillende cloud-gebaseerde aanbiedingen voor het uitvoeren van uw toepassing, zodat u hoeft zich geen zorgen over de details van de infrastructuur te berekenen. U kunt eenvoudig omhoog of schaal uit uw resources naarmate het toepassingsgebruik toeneemt.

Azure biedt services die ondersteuning bieden voor ontwikkeling van toepassingen en die als host fungeert behoeften. Azure bieden Infrastructure as a Service (IaaS) waarin u volledige controle over uw toepassing die als host fungeert. Het Azure-Platform als een Service (PaaS)-aanbiedingen bieden de volledig beheerde services die nodig zijn voor uw Apps. Er is zelfs waar hosten zonder server in Azure waarbij u hoeft alleen uw code te schrijven.

![Azure-toepassing die als host fungeert voor opties](./media/azure-developer-guide/azure-developer-hosting-options.png)


#### <a name="azure-app-service"></a>Azure App Service 

Wanneer u wilt dat het snelste pad naar uw projecten op het web publiceren, moet u rekening houden met Azure App Service. App Service kunt eenvoudig uitbreiden van uw web-apps voor het ondersteunen van uw mobiele clients en eenvoudig verbruikte REST-API's publiceren. Dit platform biedt verificatie met behulp van providers van sociale netwerken, automatisch schalen op basis van verkeer testen in productie en doorlopend en op basis van een container-implementaties.

U kunt web-apps, back-ends voor mobiele Apps en API apps maken.

Omdat alle drie app-typen de App Service-runtime delen, kunt u een website host, ondersteuning voor mobiele clients en beschikbaar maken van uw API's in Azure, allemaal met hetzelfde project of oplossing. Zie voor meer informatie over App Service, [wat is Azure Web Apps](../../app-service/app-service-web-overview.md).

App Service is ontworpen met DevOps in gedachten. Ondersteunt verschillende hulpprogramma's voor publiceren en continue integratie-implementaties, met inbegrip van GitHub webhooks, Jenkins, Visual Studio Team Services, TeamCity en anderen.

U kunt uw bestaande toepassingen naar App Service kunt migreren met behulp van de [online migratiehulpprogramma](https://www.migratetoazure.net/).

>**Wanneer u**: web-App Service gebruiken als u wilt migreren van bestaande toepassingen naar Azure, en wanneer u een volledig beheerd platform voor hosting nodig hebt voor uw web-apps. U kunt ook een App Service gebruiken wanneer u nodig hebt voor de ondersteuning van mobiele clients of REST-API's beschikbaar met uw app.

>**Aan de slag**: App Service kunt u eenvoudig maken en implementeren van uw eerste [web-app](../../app-service/app-service-web-get-started-dotnet.md), [mobiele app](../../app-service-mobile/app-service-mobile-ios-get-started.md), of [API-app](../../app-service/app-service-web-tutorial-rest-api.md).

>**Probeer het nu**: App Service kunt u een eenvoudige app om te proberen het platform zonder te hoeven aanmelden voor een Azure-account inrichten. Probeer het platform en [maken van uw app in Azure App Service](https://tryappservice.azure.com/).

#### <a name="azure-virtual-machines"></a>Azure Virtual Machines

Als een infrastructuur als een serviceprovider (IaaS) kunt met Azure u om te implementeren of migreren van uw toepassing in Windows of Linux-machines. Samen met Azure Virtual Network, Azure Virtual Machines biedt ondersteuning voor de implementatie van Windows of Linux-machines naar Azure. Met virtuele machines hebt u volledige controle over de configuratie van de machine. Wanneer u virtuele machines gebruikt, bent u verantwoordelijk voor alle software-installatie, configuratie, onderhoud en besturingssysteem patches server.

Vanwege de mate van controle die u met VM's hebt, kunt u een breed scala aan server-workloads uitvoeren op Azure die niet passen in een PaaS-model. Deze werkbelastingen omvatten database-servers, Windows Server Active Directory en Microsoft SharePoint. Zie voor meer informatie de documentatie voor virtuele Machines voor [Linux](/azure/virtual-machines/linux/) of [Windows](/azure/virtual-machines/windows/).

>**Wanneer u**: virtuele Machines gebruiken wanneer u volledig wilt beheren via de infrastructuur van uw toepassingen of werkbelastingen van on-premises toepassingen migreren naar Azure zonder wijzigingen aanbrengen.

>**Aan de slag**: Maak een [Linux-VM](../../virtual-machines/virtual-machines-linux-quick-create-portal.md) of [Windows VM](../../virtual-machines/virtual-machines-windows-hero-tutorial.md) vanuit Azure portal.

#### <a name="azure-functions-serverless"></a>Azure Functions (serverloze)

In plaats van u zorgen te maken over het bouwen en beheren van een complete toepassing of de infrastructuur voor het uitvoeren van uw code. Wat gebeurt er als u kan alleen uw code te schrijven en deze uitvoeren als reactie op gebeurtenissen of volgens een schema?  [Azure Functions](../../azure-functions/functions-overview.md) is een 'serverloze'-stijl-aanbieding waarmee u alleen de code schrijven die u nodig hebt. Met Functions, worden code-uitvoering wordt geactiveerd door HTTP-aanvragen, webhooks, cloud service-gebeurtenissen of volgens een schema. U kunt de code in uw ontwikkeling taal naar keuze, zoals C\#, F\#, Node.js, Python of PHP. U betaalt alleen voor de tijd dat uw code wordt uitgevoerd, en Azure kan worden geschaald naar behoefte met facturering op basis van gebruik.

>**Wanneer u**: Azure Functions gebruiken wanneer u de code die door andere Azure-services wordt geactiveerd door gebeurtenissen op basis van de web of volgens een planning hebt. U kunt ook functies gebruiken wanneer u niet nodig voor de overhead van een volledige gehoste project of hebt als u wilt dat alleen te betalen voor de tijd dat uw code wordt uitgevoerd. Zie voor meer informatie, [overzicht van Azure Functions](../../azure-functions/functions-overview.md).

>**Aan de slag**: de functies Quick Start-zelfstudie te volgen [uw eerste functie maken](../../azure-functions/functions-create-first-azure-function.md) vanuit de portal.

>**Probeer het nu**: Azure Functions kunt u de code uitvoeren zonder te hoeven aanmelden voor een Azure-account. Probeer het nu op en [maken van uw eerste Azure-functie](https://tryappservice.azure.com/).

#### <a name="azure-service-fabric"></a>Azure Service Fabric

Azure Service Fabric is een platform voor gedistribueerde systemen waarmee u eenvoudig te bouwen, verpakken, implementeren en beheren van schaalbare en betrouwbare microservices. Het biedt ook mogelijkheden voor uitgebreide Toepassingsbeheer voor het inrichten, implementeren, bewaken, upgraden/patchen en verwijderen van geïmplementeerde toepassingen. Apps die worden uitgevoerd op een gedeelde groep machines, kunnen klein beginnen en schaalt naar honderden of duizenden machines zo nodig.

Service Fabric ondersteunt WebAPI met Open Web Interface voor .NET (OWIN) en ASP.NET Core. Het biedt SDK's voor het bouwen van services op Linux in zowel .NET Core als Java. Zie voor meer informatie over Service Fabric, de [leertraject voor Service Fabric](https://azure.microsoft.com/documentation/learning-paths/service-fabric/).

>**Wanneer u:** Service Fabric is een goede keuze wanneer u het maken van een toepassing of een bestaande toepassing voor het gebruik van een microservice-architectuur te herschrijven. Service Fabric gebruikt wanneer u meer controle over en directe toegang tot de onderliggende infrastructuur nodig hebt.

>**Aan de slag:** [uw eerste Azure Service Fabric-toepassing maken](../../service-fabric/service-fabric-create-your-first-application-in-visual-studio.md).

### <a name="enhance-your-applications-with-azure-services"></a>Verbeter uw toepassingen met Azure-services

Naast de hosting van toepassingen, biedt Azure service-aanbiedingen die de functionaliteit, ontwikkeling en onderhoud van uw toepassingen, zowel in de cloud en on-premises kunnen verbeteren.

#### <a name="hosted-storage-and-data-access"></a>Gehoste opslag en toegang tot

De meeste toepassingen gegevens moeten worden opgeslagen, dus ongeacht hoe u besluit om uw toepassing in Azure te hosten, kunt u een of meer van de volgende services voor opslag en gegevens.

-   **Azure Cosmos DB**: een wereldwijd gedistribueerde, multi-model databaseservice waarmee u kunt de doorvoer en opslag elastisch schalen naar een willekeurig aantal geografische regio's met een uitgebreide SLA. 
    >**Wanneer u:** wanneer uw toepassing moet de document-, tabel- of grafiekdatabases, met inbegrip van MongoDB-databases met meerdere duidelijk gedefinieerde consistentiemodellen. 

    >**Aan de slag**: [bouwen van een web-app van Azure Cosmos DB](../../cosmos-db/create-sql-api-dotnet.md). Als u een MongoDB-ontwikkelaar bent, Zie [bouwen van een MongoDB-web-app met Azure Cosmos DB](../../cosmos-db/create-mongodb-dotnet.md).

-   **Azure Storage**: biedt een duurzame, maximaal beschikbare opslag voor blobs, wachtrijen, bestanden en andere soorten niet-gerelateerde gegevens. Storage vormt de basis van de opslag voor virtuele machines.

    >**Wanneer u**: wanneer uw app niet-relationele gegevens, zoals de sleutel / waarde-paren (tabellen), slaat blobs, bestanden shares of berichten (wachtrijen).

    >**Aan de slag**: Kies een van deze typen opslag: [blobs](../../storage/blobs/storage-dotnet-how-to-use-blobs.md), [tabellen](../../cosmos-db/table-storage-how-to-use-dotnet.md), [wachtrijen](../../storage/queues/storage-dotnet-how-to-use-queues.md), of [bestanden](../../storage/files/storage-dotnet-how-to-use-files.md).

-   **Azure SQL Database**: een Azure-versie van de Microsoft SQL Server-engine voor het opslaan van relationele gegevens in tabelvorm in de cloud. SQL Database biedt voorspelbare prestaties, schaalbaarheid zonder uitvaltijd, bedrijfscontinuïteit en gegevensbeveiliging.

    >**Wanneer u**: wanneer opslag van gegevens met de referentiële integriteit aannemen, transactionele ondersteuning en ondersteuning in uw toepassing is vereist voor TSQL-query's.

    >**Aan de slag**: [een SQL-database maken in enkele minuten met behulp van de Azure-portal](../../sql-database/sql-database-get-started.md).


U kunt [Azure Data Factory](../../data-factory/introduction.md) te verplaatsen van bestaande on-premises gegevens naar Azure. Als u nog niet klaar bent om gegevens te verplaatsen naar de cloud, [hybride verbindingen](../../biztalk-services/integration-hybrid-connection-overview.md) in BizTalk Services kunt u verbinding maken met uw App Service app met on-premises bronnen die worden gehost. U kunt ook verbinding maken met Azure gegevens en services van uw on-premises toepassingen.

#### <a name="docker-support"></a>Docker-ondersteuning

Docker-containers, een vorm van OS-virtualisatie, kunnen u toepassingen implementeren op een efficiëntere en voorspelbare manier. Een beperkte toepassing werkt in de productieomgeving dezelfde manier als in uw ontwikkelings- en -systemen. U kunt containers beheren met behulp van standaard Docker-hulpprogramma's. U kunt uw bestaande vaardigheden en populaire open-source-hulpprogramma's gebruiken om te implementeren en beheren van containertoepassingen op Azure.

Azure biedt verschillende manieren voor het gebruik van containers in uw toepassingen.

-   **Azure Docker VM-extensie**: Hiermee kunt u uw virtuele machine met Docker-hulpprogramma's om te fungeren als een Docker-host configureren.

    >**Wanneer u**: als u wilt genereren van consistente containerimplementaties voor uw toepassingen op een virtuele machine, of wanneer u wilt gebruiken [Docker Compose](https://docs.docker.com/compose/overview/).

    >**Aan de slag**: [een Docker-omgeving maken in Azure met behulp van de Docker VM-extensie](../../virtual-machines/virtual-machines-linux-dockerextension.md).

-   **Azure Container Service**: kunt u maken, configureren en beheren van een cluster van virtuele machines die vooraf zijn geconfigureerd voor het uitvoeren van beperkte toepassingen. Zie voor meer informatie over Container Service, [Kennismaking met Azure Container Service](../../container-service/container-service-intro.md).

    >**Wanneer u**: als u wilt bouwen kant- en schaalbare omgevingen die aanvullende tools voor planning en beheer bieden, of wanneer u een Docker Swarm-cluster implementeert.

    >**Aan de slag**: [een Container Service-cluster implementeren](../../container-service/dcos-swarm/container-service-deployment.md).

-   **Docker Machine**: kunt u installeren en beheren van een Docker-Engine op virtuele hosts met behulp van docker-machine-opdrachten.

    >**Wanneer u**: als u wilt snel prototype een app met het maken van een enkele Docker-host.

-   **Aangepaste Docker-installatiekopie voor App Service**: kunt u Docker-containers uit een container registry of een klant-container gebruiken wanneer u een web-app op Linux implementeert.

    >**Wanneer u**: bij het implementeren van een web-app op Linux met een Docker-installatiekopie.

    >**Aan de slag**: [gebruiken een aangepaste Docker-installatiekopie voor App Service on Linux](../../app-service/containers/quickstart-docker-go.md).

### <a name="authentication"></a>Verificatie

Is het cruciaal niet alleen weten wie uw toepassingen gebruikt, maar ook om te voorkomen dat onbevoegde toegang tot uw resources. Azure biedt verschillende manieren om uw app-clients te verifiëren.

-   **Azure Active Directory (Azure AD)**: Microsoft multitenant, cloud-gebaseerde identiteits- en toegangsbeheer management-service. U kunt eenmalige aanmelding (SSO) aan uw toepassingen toevoegen door te integreren met Azure AD. U kunt toegang krijgen tot de eigenschappen van de map met behulp van de Azure AD Graph-API rechtstreeks of de Microsoft Graph API. U kunt integreren met Azure AD-ondersteuning voor OAuth 2.0 machtiging framework en Open ID Connect met behulp van native HTTP-/ REST-eindpunten en de multiplatform Azure AD-verificatiebibliotheken.

    >**Wanneer u**: als u een SSO-ervaring bieden wilt, werken met gegevens op basis van een grafiek of verificatie van gebruikers op basis van een domein.

    >**Aan de slag**: Zie voor meer informatie, de [ontwikkelaarsgids van Azure Active Directory](../../active-directory/develop/azure-ad-developers-guide.md).

-   **App Service-verificatie**: wanneer u App Service voor het hosten van uw app kunt u ook ondersteuning voor ingebouwde verificatie voor Azure AD, samen met sociale id-providers, zoals Facebook, Google, Microsoft en Twitter.

    >**Wanneer u**: als u verificatie inschakelen in een App Service-app wilt met behulp van Azure AD, sociale id-providers, of beide.

    >**Aan de slag**: Zie voor meer informatie over verificatie in App Service, [verificatie en autorisatie in Azure App Service](../../app-service/app-service-authentication-overview.md).

Zie voor meer informatie over aanbevolen procedures voor beveiliging in Azure, [Azure-beveiliging aanbevolen procedures en patronen](../../security/security-best-practices-and-patterns.md).

### <a name="monitoring"></a>Bewaking

Met uw toepassing en worden uitgevoerd in Azure, moet u kunnen zijn voor het bewaken van prestaties, Bekijk voor problemen en Zie hoe klanten uw app gebruiken. Azure biedt verschillende controle-opties.

-   **Visual Studio Application Insights**: een Azure gehoste uitbreidbare Analyseservice die is geïntegreerd met Visual Studio voor het bewaken van uw live web-apps. Dit biedt u de gegevens die u moet het continu verbeteren de prestaties en bruikbaarheid van uw apps, of ze worden gehost op Azure of niet.

    >**Aan de slag**: Volg de [Application Insights-zelfstudie](../../application-insights/app-insights-overview.md).

-   **Azure Monitor**: een service die u helpt bij het visualiseren, query's uitvoeren, routeren, archiveren en reageren op de metrische gegevens en logboeken die worden gegenereerd door uw Azure-infrastructuur en resources. Monitor biedt de weergaven van gegevens dat u in de Azure-portal ziet en is van één bron voor het bewaken van Azure-resources.
 
    >**Aan de slag**: [aan de slag met Azure Monitor](../../monitoring-and-diagnostics/monitoring-get-started.md).

### <a name="devops-integration"></a>Integratie met DevOps

Of het de inrichting van virtuele machines of publiceren van uw WebApps met continue integratie, Azure kan worden geïntegreerd met de meeste van de populaire DevOps-hulpprogramma's. Met ondersteuning voor hulpprogramma's zoals Jenkins, GitHub, Puppet, Chef, TeamCity, Ansible, VSTS en anderen, kunt u werken met de hulpprogramma's die u al hebt en uw bestaande ervaring te maximaliseren.

>**Nu uitproberen:** [uitproberen verschillende van de DevOps-integraties](https://azure.microsoft.com/try/devops/).

>**Aan de slag**: DevOps-opties voor een App Service-app, Zie [continue implementatie in Azure App Service](../../app-service/app-service-continuous-deployment.md).


## <a name="azure-regions"></a>Azure-regio's

Azure is een wereldwijde cloudplatform dat is algemeen beschikbaar in veel regio's over de hele wereld. Als u een service, toepassing of virtuele machine in Azure inricht, kunt u wordt gevraagd om te selecteren van een regio, wat staat voor een specifieke datacenter waarin uw toepassing wordt uitgevoerd of waar uw gegevens worden opgeslagen. Deze regio's overeen met specifieke locaties die worden gepubliceerd op de [Azure-regio's](https://azure.microsoft.com/regions/) pagina.

### <a name="choose-the-best-region-for-your-application-and-data"></a>Kies de aanbevolen regio voor uw toepassing en gegevens

Een van de voordelen van het gebruik van Azure is dat u uw toepassingen met verschillende datacenters over de hele wereld kunt implementeren. De regio die u kiest, kan invloed hebben op de prestaties van uw toepassing. Bijvoorbeeld, is het beter om te kiezen van een regio die zich dichter bij de meeste van uw klanten te verminderen latentie van netwerkaanvragen. U kunt ook om uw regio om te voldoen aan de wettelijke vereisten voor het distribueren van uw app in bepaalde landen te selecteren. Het is altijd een aanbevolen procedure voor het opslaan van toepassingsgegevens in hetzelfde datacenter of in een datacenter als in de buurt mogelijk naar het datacenter dat als host voor uw toepassing fungeert.

### <a name="multi-region-apps"></a>Apps voor meerdere regio 's

Hoewel dit onwaarschijnlijk, is het niet onmogelijk voor een heel datacenter off vanwege een gebeurtenis, zoals een natuurramp of Internet-fout. Dit is een aanbevolen procedure voor de host belangrijke zakelijke toepassingen in meer dan één datacenter voor een maximale beschikbaarheid. Met behulp van meerdere regio's kan ook Verminder de latentie voor globale gebruikers en extra mogelijkheden voor flexibiliteit bieden bij het bijwerken van toepassingen.

Een aantal services, zoals virtuele machines en Services van de App, [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md) ondersteuning voor meerdere regio's met failover tussen regio's voor de ondersteuning van hoge beschikbaarheid zakelijke toepassingen inschakelen. Zie voor een voorbeeld [Azure-referentiearchitectuur: een webtoepassing uitvoeren in meerdere regio's](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region).

>**Wanneer u**: wanneer u een enterprise- en hoge beschikbaarheid van de toepassingen die van een failover- en replicatie profiteren hebt.

## <a name="how-do-i-manage-my-applications-and-projects"></a>Hoe kan ik mijn toepassingen en projecten beheren?

Azure biedt een uitgebreide set ervaringen voor u maken en beheren van uw Azure-resources, toepassingen en projecten, zowel via een programma en klik in de [Azure-portal](https://portal.azure.com/).

### <a name="command-line-interfaces-and-powershell"></a>Opdrachtregelinterfaces en PowerShell

Azure biedt twee manieren voor het beheren van uw toepassingen en services vanaf de opdrachtregel met behulp van Bash, Terminal, de opdrachtprompt of de opdrachtregel-hulpprogramma naar keuze. Meestal kunt u de dezelfde taken uitvoeren vanaf de opdrachtregel, zoals in de Azure-portal, zoals het maken en configureren van virtuele machines, virtuele netwerken, web-apps en andere services.

-   [Azure-opdrachtregelinterface (CLI)](../../xplat-cli-install.md): kunt u verbinding maken met een Azure-abonnement en de verschillende taken in Azure-resources vanaf de opdrachtregel-programma.

-   [Azure PowerShell](../../powershell-install-configure.md): biedt een reeks modules met cmdlets waarmee u Azure-resources beheren met behulp van Windows PowerShell.

### <a name="azure-portal"></a>Azure Portal

De Azure-portal is een webgebaseerde toepassing die u gebruiken kunt om te maken, beheren en verwijderen van Azure-resources en services. De Azure portal bevindt zich in <https://portal.azure.com>. Het bevat een aanpasbaar dashboard, hulpprogramma's voor het beheren van Azure-resources, en de toegang tot de instellingen en factureringsgegevens. Zie voor meer informatie de [overzicht van Azure portal](../../azure-portal-overview.md).

### <a name="rest-apis"></a>REST-API’s

Azure is gebouwd op een set REST-API's die ondersteuning bieden voor de gebruikersinterface van Azure portal. De meeste van deze REST-API's worden ook ondersteund zodat u via een programma inrichten en beheren van uw Azure-resources en toepassingen vanaf elk apparaat toegang tot Internet hebben. Zie voor de volledige set REST API-documentatie, de [Azure REST SDK-verwijzing](https://docs.microsoft.com/rest/api/).

### <a name="apis"></a>API's

Naast de REST API's en kunnen veel Azure-services u ook programmatisch resources van uw toepassingen beheren met behulp van de platform-specifieke Azure-SDK's, zoals SDK's voor de volgende ontwikkelplatforms:

-   [.NET](https://go.microsoft.com/fwlink/?linkid=834925)
-   [Node.js](https://docs.microsoft.com/javascript/azure)
-   [Java](https://docs.microsoft.com/java/azure)
-   [PHP](https://github.com/Azure/azure-sdk-for-php/blob/master/README.md)
-   [Python](https://docs.microsoft.com/python/azure)
-   [Ruby](https://github.com/Azure/azure-sdk-for-ruby/blob/master/README.md)
-   [Go](https://docs.microsoft.com/go/azure)

Services, zoals [Mobile Apps](../../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md) en [Azure Media Services](../../media-services/previous/media-services-dotnet-how-to-use.md) client-side SDK's om u te laten geven toegang tot services van web- en mobiele client-apps.

### <a name="azure-resource-manager"></a>Azure Resource Manager 
    
Omvat het uitvoeren van uw app op Azure kunnen werken met meerdere Azure-services, die allemaal dezelfde levenscyclus volgen en kan worden beschouwd als een logische eenheid. Een web-app kan bijvoorbeeld Web-Apps, SQL-Database, Storage, Azure Redis-Cache en Azure Content Delivery Network services gebruiken. [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) kunt u met de resources in uw toepassing als groep werken. U kunt implementeren, bijwerken of verwijderen van alle resources in een enkele, gecoördineerde bewerking.

Azure Resource Manager bevat naast logisch groeperen en beheren van gerelateerde resources, implementatiemogelijkheden waarmee u de implementatie en configuratie van de gerelateerde resources aanpassen. U kunt met behulp van Resource Manager, bijvoorbeeld, implementeren en configureren van een toepassing die uit meerdere virtuele machines, een load balancer en een Azure SQL-database als één eenheid bestaat.

U kunt deze implementaties ontwikkelen met behulp van een Azure Resource Manager-sjabloon een document JSON-indeling is. Sjablonen kunt u een implementatie definiëren en beheren van uw toepassingen met behulp van de declaratieve sjablonen in plaats van scripts. De sjablonen kunnen werken voor verschillende omgevingen, zoals testen, Faseren en productie. Met behulp van sjablonen, kunt u bijvoorbeeld een knop toevoegen aan een GitHub-opslagplaats die u implementeert u de code in de opslagplaats naar een set Azure-services met één klik.

>**Wanneer u**: gebruik van Resource Manager-sjablonen wanneer u een implementatie op basis van een sjabloon wilt gebruiken voor uw app die u programmatisch beheren kunt met behulp van REST-API's, de Azure CLI en Azure PowerShell.

>**Aan de slag**: om te beginnen met behulp van sjablonen, Zie [Authoring Azure Resource Manager-sjablonen](../../resource-group-authoring-templates.md).

## <a name="understanding-accounts-subscriptions-and-billing"></a>Inzicht in accounts, abonnementen en facturering

Ontwikkelaars zoals we meteen in de code en probeer het aan de slag zo snel mogelijk is met het maken van onze toepassingen worden uitgevoerd. Zeker willen we raden u aan de slag in Azure zo eenvoudig mogelijk. Om te maken het eenvoudig, Azure biedt een [gratis proefversie](https://azure.microsoft.com/free/). Sommige services hebben zelfs een 'Gratis uitproberen'-functionaliteit, zoals [Azure App Service](https://tryappservice.azure.com/), die hoeft u zelfs een account te maken. Als leuke als het Duik in programmeren en implementeren van uw toepassing naar Azure, is het ook belangrijk om het even duren om te begrijpen hoe Azure werkt vanuit het oogpunt van gebruikersaccounts, abonnementen en facturering van.

### <a name="what-is-an-azure-account"></a>Wat is een Azure-account?

Als u wilt maken of te werken met een Azure-abonnement, moet u een Azure-account hebben. Een Azure-account is gewoon een identiteit in Azure AD of in een map, zoals de organisatie van een werk- of schoolaccount, dat wordt vertrouwd door Azure AD. Als u geen deel van die een organisatie uitmaken, kunt u altijd een abonnement maken met behulp van uw Microsoft-Account, dat wordt vertrouwd door Azure AD. Zie voor meer informatie over het integreren van on-premises Windows Server Active Directory met Azure AD, [uw on-premises identiteiten integreren met Azure Active Directory](../../active-directory/active-directory-aadconnect.md).

Voor elk Azure-abonnement is er een vertrouwensrelatie met een Azure AD-exemplaar. Dit betekent dat er op die directory wordt vertrouwd voor het verifiëren van gebruikers, services en apparaten. Meerdere abonnementen kunnen dezelfde directory vertrouwen, maar een abonnement vertrouwt slechts één directory. Zie voor meer informatie, [hoe Azure-abonnementen zijn gekoppeld aan Azure Active Directory](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

Naast het definiëren van afzonderlijke Azure-account identiteiten, ook wel genoemd *gebruikers*, u kunt ook definiëren *groepen* in Azure AD. Het maken van gebruikersgroepen is een goede manier voor het beheren van toegang tot resources in een abonnement met behulp van op rollen gebaseerd toegangsbeheer (RBAC). Zie voor meer informatie over het maken van groepen, [een groep maken in Azure Active Directory-preview](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). U kunt ook groepen maken en beheren door [met behulp van PowerShell](../../active-directory/users-groups-roles/groups-settings-v2-cmdlets.md).

### <a name="manage-your-subscriptions"></a>Uw abonnementen beheren

Een abonnement is een logische eenheid van de Azure-services die is gekoppeld aan een Azure-account. Elke gekoppelde account heeft een rol in een abonnement. Facturering voor Azure-services is uitgevoerd op basis van een per abonnement. Zie voor een lijst van de abonnementaanbiedingen beschikbaar per type [aanbiedingsdetails voor Microsoft Azure](https://azure.microsoft.com/support/legal/offer-details/).

#### <a name="administrator-roles"></a>Beheerdersrollen

Een Azure-abonnement heeft meerdere account beheerder functies, die u op elk gewenst moment kunt toewijzen.

-   **Accountbeheerder**: deze rol heeft volledige controle over het abonnement en is het account dat is verantwoordelijk voor de facturering.

-   **Servicebeheerder**: deze rol heeft controle over alle services in het abonnement. Standaard is dit hetzelfde account als de accountbeheerder.

-   **CO-beheerder**: deze rol heeft dezelfde toegangsrechten als de servicebeheerder, behalve dat deze de koppeling van het abonnement naar een Azure-adreslijst wijzigen kan.

Zie voor meer informatie over beheerdersrollen [toevoegen of wijzigen van de Azure-beheerdersrollen](../../billing/billing-add-change-azure-subscription-administrator.md#add-an-admin-for-a-subscription).

#### <a name="resource-groups"></a>Resourcegroepen

Als u nieuwe Azure-services inricht, kunt u dat doen in een bepaald abonnement. Afzonderlijke Azure-services, die worden resources ook genoemd, worden gemaakt in de context van een resourcegroep. Resourcegroepen kunt u gemakkelijker te implementeren en beheren van resources van uw toepassing. Een resourcegroep kan alle resources voor uw toepassing die u wilt werken als een eenheid moet bevatten. U kunt resources verplaatsen tussen resourcegroepen en zelfs met verschillende abonnementen. Zie voor meer informatie over het verplaatsen van resources, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../../resource-group-move-resources.md).

Azure Resource Explorer is een uitstekend hulpprogramma voor het visualiseren van de resources die u al hebt gemaakt in uw abonnement. Zie voor meer informatie, [Azure Resource Explorer gebruiken om te bekijken en wijzigen van resources](../../resource-manager-resource-explorer.md).

#### <a name="grant-access-to-resources"></a>Toegang verlenen tot resources

Wanneer u toegang tot Azure-resources toestaan, maar het is altijd een aanbevolen procedure zodat gebruikers beschikken over de minimale bevoegdheden die is vereist voor het uitvoeren van een bepaalde taak.

-   **Op rollen gebaseerd toegangsbeheer (RBAC)**: In Azure, u kunt toegang verlenen tot gebruikersaccounts (principals) bij een opgegeven bereik: abonnement, resourcegroep of afzonderlijke resources. Met RBAC kunt u een set met resources implementeren in een resourcegroep en machtigingen verlenen aan een specifieke gebruiker of groep. Ook kunt u de toegang beperken tot alleen de resources die deel uitmaken van de doelresourcegroep. U kunt ook toegang verlenen tot een enkele resource, zoals een virtuele machine of virtuele netwerk. Om toegang te verlenen, kunt u een rol toewijzen aan de gebruiker, groep of service-principal. Er zijn veel vooraf gedefinieerde rollen, en u kunt ook uw eigen aangepaste rollen definiëren.

    >**Wanneer u**: wanneer u over Geavanceerd toegangsbeheer moet voor gebruikers en groepen.

    >**Aan de slag**: Zie voor meer informatie, [aan de slag met toegangsbeheer in Azure portal](../../role-based-access-control/overview.md).

-   **Service-principalobjecten**: naast het verlenen van toegang aan beveiligings-gebruikersprincipals en groepen, kunt u dezelfde toegang verlenen aan een service-principal.

    > **Wanneer u**: wanneer u programmatisch beheren van Azure-resources of verlenen van toegang voor toepassingen. Zie voor meer informatie, [een Active Directory-toepassing en service-principal](../../resource-group-create-service-principal-portal.md).

#### <a name="tags"></a>Tags

Azure Resource Manager kunt u aangepaste labels toewijzen aan afzonderlijke resources. Tags zijn sleutel-waardeparen, kunnen nuttig zijn wanneer u nodig hebt om resources voor facturering of bewaking te organiseren. Tags bieden een manier om bij te houden van resources in meerdere resourcegroepen. U kunt tags in de portal, in de Azure Resource Manager-sjabloon of via een programma, met behulp van de REST-API, Azure CLI of PowerShell. U kunt meerdere labels toewijzen aan elke resource. Zie voor meer informatie, [met tags voor het ordenen van uw Azure-resources](../../resource-group-using-tags.md).

### <a name="billing"></a>Billing

Zijn belangrijke opmerkingen bij de verplaatsing van on-premises computers naar de cloud gehoste services, bijhouden en het gebruik van de service en de bijbehorende kosten schatten. Het is belangrijk dat u om in te schatten nieuwe resources kosten om uit te voeren op maandbasis. U moet ook mogelijk om te projecteren hoe de facturering eruitziet voor een bepaalde maand op basis van de huidige uitgaven.

#### <a name="get-resource-usage-data"></a>Ophalen van gegevens over brongebruik

Azure biedt een set facturering REST-API's waarmee toegang tot het gebruik van resources en metagegevensinformatie voor Azure-abonnementen. Deze facturerings-API's bieden u de mogelijkheid om beter te voorspellen en beheren van Azure-kosten. U kunt bijhouden en analyseren uitgaven in stappen van per uur, de bestedingslimiet waarschuwingen maken en voorspellen van toekomstige facturering op basis van de huidige trends in gebruik.

>**Aan de slag**: Zie voor meer informatie over het gebruik van de facturerings-API's, [facturering Azure-gebruik en RateCard APIs overzicht](../../billing-usage-rate-card-overview.md).

#### <a name="predict-future-costs"></a>Toekomstige kosten voorspellen

Hoewel het moeilijk om in te schatten kosten vooraf, Azure heeft een [prijscalculator](https://azure.microsoft.com/pricing/calculator/) dat u gebruiken kunt wanneer u een schatting maken van de kosten van geïmplementeerde resources. U kunt ook de blade facturering in de portal en de facturering REST API's gebruiken om te schatten van de toekomstige kosten, op basis van huidige verbruik.

>**Aan de slag**: Zie [facturering Azure-gebruik en RateCard APIs overzicht](../../billing-usage-rate-card-overview.md).

#### <a name="set-up-billing-alerts"></a>Meldingen voor facturering instellen

Nadat u uw toepassing of oplossing op Azure hebt geïmplementeerd, kunt u waarschuwingen die dat u een e-mail verzendt wanneer u bij het benaderen van de bestedingslimieten die zijn gedefinieerd in de waarschuwing kunt maken.

>**Aan de slag**: Zie voor meer informatie, [waarschuwingen voor uw Microsoft Azure-abonnementen voor facturering instellen](../../billing-set-up-alerts.md).
