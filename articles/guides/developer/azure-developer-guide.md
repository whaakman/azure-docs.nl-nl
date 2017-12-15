---
title: Get gestart handleiding voor ontwikkelaars die in Azure | Microsoft Docs
description: "Dit onderwerp bevat essentiële informatie voor ontwikkelaars die aan de slag met het Microsoft Azure-platform voor hun ontwikkelingsbehoeften."
services: 
cloud: 
documentationcenter: 
author: ggailey777
manager: erikre
ms.assetid: 
ms.service: na
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: glenga
ms.openlocfilehash: b54b806aad1e15702d2167dcf2870ba19c4708df
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="get-started-guide-for-azure-developers"></a>Aan de slag-handleiding voor Azure-ontwikkelaars

## <a name="what-is-azure"></a>Wat is Azure?

Azure is een volledig cloud-platform die u kunt uw bestaande toepassingen worden gehost, de ontwikkeling van nieuwe toepassingen te stroomlijnen en on-premises toepassingen zelfs te verbeteren. Azure integreert de cloudservices die u wilt ontwikkelen, testen, implementeren en beheren van uw toepassingen, en tegelijkertijd profiteren van de efficiëntie van cloud computing.

U kunt uw toepassingen in Azure hosten, begin klein en schaal van uw toepassing eenvoudig omhoog wanneer uw vraag van klanten groeit. Azure biedt ook de betrouwbaarheid die nodig is voor hoge beschikbaarheid-toepassingen, waaronder zelfs failover tussen verschillende regio's. De [Azure-portal](https://portal.azure.com) kunt u eenvoudig alle Azure-services te beheren. U kunt uw services ook programmatisch beheren met behulp van de service-API's en sjablonen.

**Wie dit**: in deze handleiding bevat een inleiding tot Azure-platform voor toepassingsontwikkelaars. Biedt richtlijnen en richting die u nodig hebt om nieuwe toepassingen in Azure bouwen of migreren van bestaande toepassingen naar Azure te starten.

## <a name="where-do-i-start"></a>Waar moet ik beginnen?

Met alle services die Azure biedt, kan geen sinecure om te achterhalen welke services u wilt ondersteunen, de architectuur van uw oplossing zijn. Deze sectie worden de Azure-services die ontwikkelaars vaak gebruikt. Zie voor een lijst van alle Azure-services de [Azure-documentatie](../../index.md).

U moet eerst beslissen over de voor het hosten van uw toepassing in Azure. U hoeft voor het beheren van uw volledige infrastructuur als een virtuele machine (VM). Kunt u de platform-management-installaties die Azure biedt? Mogelijk moet u een framework zonder server host code kan worden uitgevoerd alleen?

Uw toepassing moet cloudopslag, die Azure biedt verschillende opties voor. U kunt profiteren van de enterprise-verificatie van Azure. Er zijn ook hulpprogramma's voor cloud-gebaseerde ontwikkeling en bewaking en de meeste hostingservices DevOps-integratie bieden.

Nu bekijken we enkele van de specifieke services die we aanbevelen voor uw toepassingen onderzoeken.

### <a name="application-hosting"></a>Toepassingen hosten

Azure biedt dat verschillende cloud-gebaseerde compute aanbiedingen voor het uitvoeren van uw toepassing zodat u niet hoeft te hoeven maken over de details van de infrastructuur. U kunt eenvoudig omhoog schalen of uitschalen van uw resources wanneer het toepassingsgebruik van uw groeit.

Azure biedt services die ondersteuning bieden voor uw toepassing ontwikkelen en hosten van behoeften. Azure biedt een infrastructuur-as-a-service (IaaS) zodat u volledige controle over het hosten van uw toepassing. Azure platform as a service (PaaS)-aanbiedingen bieden de volledig beheerde service in te schakelen van uw apps. Er is zelfs true zonder server die als host fungeert in Azure waarbij hoeft u uw code te schrijven.

![Azure-toepassing hosting-opties](./media/azure-developer-guide/azure-developer-hosting-options.png)


#### <a name="azure-app-service"></a>Azure App Service 

Als u wilt dat de snelste pad naar uw projecten op het web publiceren, houd rekening met Azure App Service. App Service kunt gemakkelijk uitbreiden van uw web-apps voor de ondersteuning van uw mobiele clients en eenvoudig verbruikte REST-API's te publiceren. Dit platform biedt verificatie via sociale providers, automatisch schalen op basis van verkeer testen in productie en continue en op basis van een container-implementaties.

U kunt de web-apps, back-ends van mobiele Apps en API apps maken.

Omdat alle drie app-typen de runtime-App Service delen, kunt u host voor een website, mobiele clients ondersteunen en uw API's in Azure, beschikbaar via hetzelfde project of oplossing. Zie voor meer informatie over App Service, [wat is Azure Web Apps](../../app-service/app-service-web-overview.md).

App Service is ontworpen met DevOps in gedachten. Biedt ondersteuning voor verschillende hulpprogramma's voor publiceren en continue integratie implementaties, met inbegrip van GitHub webhooks, Jenkins, Visual Studio Team Services, TeamCity en anderen.

U kunt uw bestaande toepassingen naar App Service migreren met behulp van de [online Migratiehulpmiddel](https://www.migratetoazure.net/).

>**Wanneer gebruikt u**: App Service gebruiken wanneer u bent migreren van bestaande webtoepassingen naar Azure, en wanneer u een volledig beheerd platform voor hosting nodig hebt voor uw web-apps. U kunt ook een App Service gebruiken als u nodig hebt voor de ondersteuning van mobiele clients of REST-API's beschikbaar met uw app.

>**Aan de slag**: App Service kunt u gemakkelijk maken en implementeren van uw eerste [web-app](../../app-service/app-service-web-get-started-dotnet.md), [mobiele app](../../app-service-mobile/app-service-mobile-ios-get-started.md), of [API-app](../../app-service/app-service-web-tutorial-rest-api.md).

>**Probeer het nu**: App Service kunt u een eenvoudige app om te proberen het platform zonder te hoeven aanmelden voor een Azure-account inrichten. Probeer het platform en [maken van uw app in Azure App Service](https://tryappservice.azure.com/).

#### <a name="azure-virtual-machines"></a>Azure Virtual Machines

Als een infrastructure as a service (IaaS)-provider kunt Azure u op implementeren of migreren van uw toepassing op Windows of Linux-machines. Samen met Azure Virtual Network, virtuele Machines van Azure biedt ondersteuning voor de implementatie van Windows of Linux-machines naar Azure. Met virtuele machines hebt u volledige controle over de configuratie van de machine. Wanneer u virtuele machines, bent u verantwoordelijk voor alle software-installatie, configuratie, onderhoud en besturingssysteem patches server.

Vanwege de mate van controle die u met virtuele machines hebt, kunt u een breed scala aan server-werkbelasting uitvoeren op Azure die niet passen in een PaaS-model. Deze werkbelastingen behoren databaseservers, Windows Server Active Directory en Microsoft SharePoint. Zie voor meer informatie de documentatie van de virtuele Machines voor [Linux](/azure/virtual-machines/linux/) of [Windows](/azure/virtual-machines/windows/).

>**Wanneer gebruikt u**: virtuele Machines gebruiken als u volledig wilt beheren via de infrastructuur van uw toepassing of lokale toepassing werklast naar Azure migreren zonder wijzigingen aanbrengen.

>**Aan de slag**: Maak een [Linux VM](../../virtual-machines/virtual-machines-linux-quick-create-portal.md) of [Windows VM](../../virtual-machines/virtual-machines-windows-hero-tutorial.md) vanuit de Azure-portal.

#### <a name="azure-functions-serverless"></a>Azure Functions (zonder server)

In plaats van zorgen te hoeven maken over het bouwen van en beheren van een complete toepassing of de infrastructuur voor het uitvoeren van uw code. Wat gebeurt er als kan u zojuist hebt uw code te schrijven en uitvoeren in reactie op gebeurtenissen of volgens een schema?  [Azure Functions](../../azure-functions/functions-overview.md) is een 'zonder server'-stijl-aanbieding die u kunt alleen de code schrijven moet u. Met functies, wordt de uitvoering van code geactiveerd door de HTTP-aanvragen, webhooks, cloud servicegebeurtenissen of volgens een schema. U kunt de code in uw taal ontwikkeling keuze, zoals C\#, F\#, Node.js, Python of PHP. U betaalt alleen voor de tijd dat uw code wordt uitgevoerd en Azure schaalt naar behoefte met facturering op basis van verbruik.

>**Wanneer gebruikt u**: Azure Functions gebruiken wanneer u code die wordt geactiveerd door een andere Azure-services door gebeurtenissen op basis van een webpagina of volgens een planning hebt. U kunt ook functies gebruiken wanneer u niet de overhead van een volledige gehoste project of hoeft wanneer u alleen wilt betalen voor de tijd dat uw code wordt uitgevoerd. Zie voor meer informatie, [overzicht van Azure Functions](../../azure-functions/functions-overview.md).

>**Aan de slag**: de functies Quick Start-zelfstudie te volgen [maken van uw eerste functie](../../azure-functions/functions-create-first-azure-function.md) vanuit de portal.

>**Probeer het nu**: Azure Functions, kunt u de code uitvoeren zonder te hoeven aanmelden voor een Azure-account. Probeer het nu aan en [uw eerste Azure-functie maken](https://tryappservice.azure.com/).

#### <a name="azure-service-fabric"></a>Azure Service Fabric

Azure Service Fabric is een platform voor gedistribueerde systemen waarmee u gemakkelijk bouwen, verpakken, implementeren en beheren van schaalbare en betrouwbare microservices. Het bevat ook uitgebreide toepassing beheermogelijkheden voor inrichting, implementatie, bewaking, bijwerken/patchen en verwijderen van toepassingen worden geïmplementeerd. Apps die worden uitgevoerd op een gedeelde groep machines, kunnen begin klein en naar behoefte worden uitgebreid naar honderden of duizenden computers.

Service Fabric ondersteunt WebAPI met Open Web Interface voor .NET (OWIN) en ASP.NET Core. Het biedt SDK's voor het bouwen van services op Linux in zowel .NET Core en Java. Zie voor meer informatie over Service Fabric, de [leertraject voor Service Fabric](https://azure.microsoft.com/documentation/learning-paths/service-fabric/).

>**Gebruik:** Service Fabric is een goede keuze wanneer u een toepassing maken of een bestaande toepassing te gebruiken een microservice-architectuur herschrijven. Service Fabric gebruikt als u meer controle over of directe toegang tot de onderliggende infrastructuur.

>**Aan de slag:** [uw eerste Azure Service Fabric-toepassing maken](../../service-fabric/service-fabric-create-your-first-application-in-visual-studio.md).

### <a name="enhance-your-applications-with-azure-services"></a>Uw toepassingen met Azure-services te verbeteren

Azure biedt naast toepassingen hosten, serviceaanbiedingen die de functionaliteit, ontwikkeling en onderhoud van uw toepassingen, zowel in de cloud en on-premises kunnen verbeteren.

#### <a name="hosted-storage-and-data-access"></a>Gehoste opslag- en -toegang

De meeste toepassingen moeten opslaan van gegevens, dus ongeacht hoe u besluit voor het hosten van uw toepassing in Azure, kunt u een of meer van de volgende opslag- en -services.

-   **Azure Cosmos DB**: een globaal gedistribueerd en modellen database-service waarmee u kunt de doorvoer en opslag via verschillende geografische regio's met een uitgebreide SLA schalen. 
    >**Gebruik:** wanneer uw toepassing moet een document, tabel of grafiek databases, met inbegrip van MongoDB-databases met meerdere goed gedefinieerde consistentie modellen. 

    >**Aan de slag**: [bouwen van een web-app van Azure DB die Cosmos](../../cosmos-db/create-sql-api-dotnet.md). Als u een ontwikkelaar MongoDB, Zie [bouwen van een MongoDB-web-app met Azure Cosmos DB](../../cosmos-db/create-mongodb-dotnet.md).

-   **Azure Storage**: biedt duurzaam en maximaal beschikbare opslag voor blobs, wachtrijen, bestanden en andere soorten niet-relationele gegevens. Storage vormt de opslagbasis voor virtuele machines.

    >**Wanneer gebruikt u**: wanneer uw app slaat de niet-relationele gegevens, zoals sleutel / waarde-paren (tabellen), blobs, bestanden, shares, of berichten (wachtrijen).

    >**Aan de slag**: Kies een van deze typen opslag: [blobs](../../storage/blobs/storage-dotnet-how-to-use-blobs.md), [tabellen](../../cosmos-db/table-storage-how-to-use-dotnet.md), [wachtrijen](../../storage/queues/storage-dotnet-how-to-use-queues.md), of [bestanden](../../storage/files/storage-dotnet-how-to-use-files.md).

-   **Azure SQL Database**: een Azure-versie van de Microsoft SQL Server-engine voor het opslaan van relationele tabellaire gegevens in de cloud. SQL-Database biedt voorspelbare prestaties, schaalbaarheid zonder uitvaltijd, bedrijfscontinuïteit en gegevensbeveiliging.

    >**Wanneer gebruikt u**: wanneer uw toepassing gegevensopslag met referentiële integriteit, transactieondersteuning en ondersteuning vereist voor TSQL-query's.

    >**Aan de slag**: [maken van een SQL-database in minuten met behulp van de Azure-portal](../../sql-database/sql-database-get-started.md).


U kunt [Azure Data Factory](../../data-factory/introduction.md) verplaatsen van bestaande on-premises gegevens naar Azure. Als u klaar om gegevens te verplaatsen naar de cloud niet [hybride verbindingen](../../biztalk-services/integration-hybrid-connection-overview.md) in BizTalk Services kunt u verbinding maken uw App Service-app lokale bronnen gehost. U kunt ook verbinding maken met Azure gegevens en services van uw on-premises toepassingen.

#### <a name="docker-support"></a>Docker-ondersteuning

Docker-containers, een vorm van OS virtualisatie, kunnen u toepassingen implementeren op een manier efficiënter en voorspelbare. Een beperkte toepassing werkt in productie dezelfde manier als op uw systemen ontwikkeling en tests. U kunt de containers beheren met standaard Docker-hulpprogramma's. U kunt uw bestaande vaardigheden en populaire open-source hulpprogramma's gebruiken om te implementeren en beheren van toepassingen op basis van een container in Azure.

Azure biedt verschillende manieren voor gebruik van containers in uw toepassingen.

-   **Azure Docker VM-extensie**: Hiermee kunt u uw virtuele machine configureren met Docker-hulpprogramma's om te fungeren als een Docker-host.

    >**Wanneer gebruikt u**: wanneer u wilt genereren consistent containerimplementaties voor uw toepassingen op een virtuele machine, of wanneer u wilt gebruiken [Docker Compose](https://docs.docker.com/compose/overview/).

    >**Aan de slag**: [een Docker-omgeving maken in Azure met behulp van de Docker-VM-extensie](../../virtual-machines/virtual-machines-linux-dockerextension.md).

-   **Azure Container Service**: kunt maken, configureren en beheren van een cluster van virtuele machines die vooraf zijn geconfigureerd voor beperkte toepassingen worden uitgevoerd. Zie voor meer informatie over de Containerservice, [inleiding Azure Container Service](../../container-service/container-service-intro.md).

    >**Wanneer gebruikt u**: als u wilt bouwen gereed is voor productie, schaalbare omgevingen die extra hulpprogramma's voor planning en beheer bieden, of wanneer u een Docker Swarm-cluster implementeert.

    >**Aan de slag**: [een Container Service-cluster implementeren](../../container-service/dcos-swarm/container-service-deployment.md).

-   **Docker-Machine**: u kunt installeren en beheren van een Docker-Engine op de virtuele hosts met behulp van docker-machine-opdrachten.

    >**Wanneer gebruikt u**: wanneer u moet snel prototype een app door het maken van een afzonderlijke Docker-host.

-   **Aangepaste Docker-installatiekopie voor App Service**: Hiermee kunt u Docker-containers uit het register van een container of een klant-container gebruiken wanneer u een web-app op Linux implementeert.

    >**Wanneer gebruikt u**: bij het implementeren van een web-app op Linux aan een Docker-installatiekopie.

    >**Aan de slag**: [gebruik van een aangepaste Docker-afbeelding voor App-Service op Linux](../../app-service/containers/quickstart-custom-docker-image.md).

### <a name="authentication"></a>Authentication

Het is essentieel niet alleen weten wie uw toepassingen wordt gebruikt, maar ook om te voorkomen dat onbevoegde toegang tot uw resources. Azure biedt verschillende manieren om uw app-clients te verifiëren.

-   **Azure Active Directory (Azure AD)**: Microsoft multitenant, cloud-gebaseerde identiteit en toegang management-service. U kunt eenmalige aanmelding (SSO) aan uw toepassingen toevoegen door te integreren met Azure AD. Eigenschappen van de map is toegankelijk via de Azure AD Graph API rechtstreeks of de Microsoft Graph API. U kunt integreren met Azure AD-ondersteuning voor de OAuth2.0 autorisatie framework en Open ID Connect met behulp van native HTTP-/ REST-eindpunten en de multiplatform Azure AD-verificatiebibliotheken.

    >**Wanneer gebruikt u**: als u een SSO-ervaring bieden wilt, werken met gegevens op basis van de grafiek of verificatie van gebruikers op basis van een domein.

    >**Aan de slag**: Zie voor meer informatie, de [ontwikkelaarshandleiding Azure Active Directory](../../active-directory/develop/active-directory-developers-guide.md).

-   **Verificatie van App Service**: wanneer u App Service voor het hosten van uw app kiest, u ook ondersteuning voor ingebouwde verificatie voor Azure AD, samen met sociale identiteitsproviders, waaronder Facebook, Google, Microsoft en Twitter.

    >**Wanneer gebruikt u**: als u verificatie inschakelen in een App Service-app wilt met Azure AD sociale identiteitsproviders of beide.

    >**Aan de slag**: Zie voor meer informatie over verificatie in App Service, [verificatie en autorisatie in Azure App Service](../../app-service/app-service-authentication-overview.md).

Zie voor meer informatie over aanbevolen procedures voor beveiliging in Azure, [Azure-beveiliging aanbevolen procedures en patronen](../../security/security-best-practices-and-patterns.md).

### <a name="monitoring"></a>Bewaking

Met uw toepassing en worden uitgevoerd in Azure, moet u kunnen bijhouden van de prestaties, Bekijk voor problemen en Zie hoe klanten uw app gebruiken. Azure biedt verschillende opties voor controle.

-   **Visual Studio Application Insights**: een Azure gehoste uitbreidbare Analyseservice die kan worden geïntegreerd met Visual Studio voor het bewaken van uw live-webtoepassingen. Dit biedt u de gegevens die u nodig hebt voor het continu verbeteren de prestaties en bruikbaarheid van uw apps, of ze zijn gehost op Azure of niet.

    >**Aan de slag**: Volg de [Application Insights-zelfstudie](../../application-insights/app-insights-overview.md).

-   **Monitor voor Azure**: een service die u helpt bij het visualiseren, query, routeren, archiveren en reageren op de metrische gegevens en de logboeken die worden gegenereerd door uw Azure-infrastructuur en de bronnen. Monitor biedt de gegevensweergaven u zien in de Azure portal en één bron voor het bewaken van de Azure-resources is.
 
    >**Aan de slag**: [aan de slag met Azure Monitor](../../monitoring-and-diagnostics/monitoring-get-started.md).

### <a name="devops-integration"></a>Integratie met DevOps

Of het inrichten van virtuele machines of uw web-apps met continue integratie publiceren, Azure kan worden geïntegreerd met de meeste van de populaire DevOps-hulpprogramma's. Met ondersteuning voor hulpprogramma's zoals Jenkins, GitHub, Puppet, Chef, TeamCity, Ansible, VSTS en anderen, kunt u werken met de hulpprogramma's die u al hebt en uw bestaande ervaring te maximaliseren.

>**Probeer het nu:** [uitproberen verschillende van de DevOps-integraties](https://azure.microsoft.com/try/devops/).

>**Aan de slag**: Zie DevOps-opties voor een App Service-app [continue implementatie naar Azure App Service](../../app-service/app-service-continuous-deployment.md).


## <a name="azure-regions"></a>Azure-regio's

Azure is een globale cloudplatform die algemeen beschikbaar is in veel regio's over de hele wereld. Wanneer u een service, toepassing of virtuele machine in Azure inricht, wordt u gevraagd een regio, wat staat voor een specifieke datacenter selecteren waarin de toepassing wordt uitgevoerd of waar uw gegevens worden opgeslagen. Deze gebieden komen overeen met specifieke locaties die worden gepubliceerd op de [Azure-regio's](https://azure.microsoft.com/regions/) pagina.

### <a name="choose-the-best-region-for-your-application-and-data"></a>Kies de beste regio voor uw toepassing en gegevens

Een van de voordelen van het gebruik van Azure is dat u uw toepassingen op verschillende datacenters over de hele wereld implementeren kunt. De regio die u kiest, kan de prestaties van uw toepassing beïnvloeden. Bijvoorbeeld, is het beter om te kiezen een regio die zich dichter bij de meeste klanten kunnen de latentie in netwerkaanvragen te verminderen. U kunt ook uw regio om te voldoen aan de wettelijke vereisten voor het distribueren van uw app in bepaalde landen selecteren. Het is altijd een aanbevolen procedure voor het opslaan van toepassingsgegevens in hetzelfde datacenter of in een datacenter als in de buurt mogelijk zijn in het datacenter dat als host voor uw toepassing fungeert.

### <a name="multi-region-apps"></a>Meerdere landen/regio-apps

Hoewel onwaarschijnlijk, is het niet onmogelijk voor een hele datacenter offline te gaan vanwege een gebeurtenis, zoals een natuurramp of Internet-fout. Het is een best practice om als host belangrijke zakelijke toepassingen in meer dan één datacenter voor maximale beschikbaarheid. Met behulp van meerdere regio's kan ook de latentie voor globale gebruikers te verminderen en bieden meer mogelijkheden voor flexibiliteit bij het bijwerken van toepassingen.

Sommige services, zoals virtuele Machine en App-Services, gebruik [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md) ondersteuning voor meerdere landen/regio met failover tussen regio's ter ondersteuning van hoge beschikbaarheid enterprise toepassingen inschakelen. Zie voor een voorbeeld [naslaginformatie over Azure-architectuur: webtoepassing met hoge beschikbaarheid](../../guidance/guidance-web-apps-multi-region.md).

>**Wanneer gebruikt u**: wanneer u een enterprise- en hoge beschikbaarheid toepassingen die van failover- en replicatie profiteren hebt.

## <a name="how-do-i-manage-my-applications-and-projects"></a>Hoe beheer ik mijn toepassingen en projecten?

Azure biedt een uitgebreide reeks ervaringen voor u maken en beheren van uw Azure-resources, toepassingen en projecten: zowel programmatisch en in de [Azure-portal](https://portal.azure.com/).

### <a name="command-line-interfaces-and-powershell"></a>Opdrachtregelinterfaces en PowerShell

Azure biedt twee manieren voor het beheren van uw toepassingen en services vanaf de opdrachtregel met behulp van Bash, Terminal, de opdrachtprompt of het opdrachtregelhulpprogramma van keuze. Meestal kunt u dezelfde taken kunt uitvoeren vanaf de opdrachtregel als in de Azure-portal, zoals het maken en configureren van virtuele machines, virtuele netwerken, web-apps en andere services.

-   [Azure-opdrachtregelinterface (CLI)](../../xplat-cli-install.md): Hiermee kunt u verbinding maken met een Azure-abonnement en diverse taken op basis van de Azure-resources vanaf de opdrachtregel programma.

-   [Azure PowerShell](../../powershell-install-configure.md): biedt een reeks modules met cmdlets waarmee u kunt Azure-resources beheren met behulp van Windows PowerShell.

### <a name="azure-portal"></a>Azure Portal

De Azure portal is een webtoepassing die u kunt maken, beheren en Azure-resources en services verwijderen. De Azure-portal bevindt zich op <https://portal.azure.com>. Het bevat een aangepast dashboard, hulpprogramma's voor het beheren van Azure-resources en de toegang tot de abonnementsinstellingen en factureringsgegevens. Zie voor meer informatie de [overzicht van Azure portal](../../azure-portal-overview.md).

### <a name="rest-apis"></a>REST-API’s

Azure is gebouwd op een set REST-API's die ondersteuning bieden voor de gebruikersinterface van de Azure portal. De meeste van deze REST-API's worden ook ondersteund zodat u programmatisch inrichten en beheren van uw Azure-resources en toepassingen vanaf elk apparaat toegang tot Internet hebben. Zie voor de volledige reeks REST API-documentatie, de [REST-SDK van Azure verwijzing](https://docs.microsoft.com/rest/api/).

### <a name="apis"></a>API's

Naast de REST-API's kunnen vele Azure-services u ook resources van uw toepassingen programmatisch te beheren met behulp van de platform-specifieke Azure SDK's, met inbegrip van SDK's voor de volgende ontwikkelplatforms:

-   [.NET](https://go.microsoft.com/fwlink/?linkid=834925)
-   [Node.js](http://azure.github.io/azure-sdk-for-node/)
-   [Java](https://docs.microsoft.com/java/api/)
-   [PHP](https://github.com/Azure/azure-sdk-for-php/blob/master/README.md)
-   [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/)
-   [Ruby](https://github.com/Azure/azure-sdk-for-ruby/blob/master/README.md)

Services, zoals [Mobile Apps](../../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md) en [Azure Media Services](../../media-services/media-services-dotnet-how-to-use.md) clientzijde SDK's om u te laten bieden toegang tot de services van web- en voor mobiele ClientApps.

### <a name="azure-resource-manager"></a>Azure Resource Manager 
    
Uw app uitgevoerd op Azure waarschijnlijk omvat het werken met meerdere Azure-services, die allemaal de dezelfde levenscyclus volgen en kunnen worden beschouwd als een logische eenheid. Een web-app kan bijvoorbeeld Web-Apps, SQL-Database, opslag, Azure Redis-Cache en Azure Content Delivery Network services gebruiken. [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) kunt u met de resources in uw toepassing als groep werken. U kunt implementeren, bijwerken of verwijderen van alle resources in een enkele, gecoördineerde bewerking.

Azure Resource Manager bevat naast logisch groeperen en beheren van gerelateerde resources, implementatiemogelijkheden waarmee u de implementatie en configuratie van gerelateerde resources aanpassen. U kunt met Resource Manager, bijvoorbeeld, implementeren en configureren van een toepassing die uit meerdere virtuele machines, een load balancer en een Azure SQL database als één eenheid bestaat.

U kunt deze implementaties ontwikkelen met behulp van een Azure Resource Manager-sjabloon een document JSON-indeling is. Sjablonen kunt u een implementatie definiëren en beheren van uw toepassingen met behulp van declaratieve sjablonen, in plaats van scripts. De sjablonen kunnen werken voor verschillende omgevingen, zoals testen, Faseren en productie. Met behulp van sjablonen, kunt u een knop kunt toevoegen aan een GitHub-repo-waarmee de code in de opslagplaats wordt geïmplementeerd op een reeks Azure-services met één klik.

>**Wanneer gebruikt u**: gebruik Resource Manager-sjablonen als u wilt dat een implementatie op basis van een sjabloon voor uw app die u programmatisch beheren kunt met behulp van REST-API's, Azure CLI en Azure PowerShell.

>**Aan de slag**: om te beginnen met behulp van sjablonen, Zie [Azure Resource Manager-sjablonen samenstellen](../../resource-group-authoring-templates.md).

## <a name="understanding-accounts-subscriptions-and-billing"></a>Wat zijn accounts, abonnementen en facturering

Ontwikkelaars, die we graag rechts Duik in de code en probeer het aan de slag zo snel mogelijk bij het maken van onze toepassingen worden uitgevoerd. We willen zeker raden u aan de slag in Azure net zo eenvoudig mogelijk. Om zo eenvoudig en Azure biedt een [gratis proefversie](https://azure.microsoft.com/free/). Sommige services zelfs een 'Gratis proberen'-functionaliteit hebben, zoals [Azure App Service](https://tryappservice.azure.com/), die, hoeft u zelfs een account wilt maken. Als plezier zoals u wilt Duik in coderen en implementeren van uw toepassing naar Azure, wordt is het ook belangrijk om het even duren om te begrijpen hoe Azure werkt uit oogpunt van gebruikersaccounts, abonnementen en facturering.

### <a name="what-is-an-azure-account"></a>Wat is een Azure-account?

Om te kunnen maken of te werken met een Azure-abonnement, moet u een Azure-account hebben. Een Azure-account is gewoon een identiteit in Azure AD of in een map, zoals de organisatie van een werk- of schoolaccount, die wordt vertrouwd door Azure AD. Als u niet deel uit van een dergelijke organisatie, kunt u altijd een abonnement maken met behulp van uw Microsoft-Account wordt vertrouwd door Azure AD. Zie voor meer informatie over het lokale Windows Server Active Directory integreren met Azure AD, [uw on-premises identiteiten integreren met Azure Active Directory](../../active-directory/active-directory-aadconnect.md).

Voor elk Azure-abonnement is er een vertrouwensrelatie met een Azure AD-exemplaar. Dit betekent dat er op die directory wordt vertrouwd voor het verifiëren van gebruikers, services en apparaten. Meerdere abonnementen kunnen dezelfde directory vertrouwen, maar een abonnement vertrouwt slechts één directory. Zie voor meer informatie, [hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](../../active-directory/active-directory-how-subscriptions-associated-directory.md).

Naast het definiëren van afzonderlijke Azure account identiteiten, ook wel *gebruikers*, kunt u definiëren *groepen* in Azure AD. Gebruikersgroepen maken is een uitstekende manier voor het beheren van toegang tot bronnen in een abonnement met behulp van op rollen gebaseerde toegangsbeheer (RBAC). Zie voor meer informatie over hoe u groepen maakt, [een groep maken in Azure Active Directory-preview](../../active-directory/active-directory-groups-create-azure-portal.md). U kunt ook groepen maken en beheren door [met behulp van PowerShell](../../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).

### <a name="manage-your-subscriptions"></a>Uw abonnementen beheren

Een abonnement is een logische eenheid van Azure-services die is gekoppeld aan een Azure-account. Elke gekoppelde account heeft een rol in een abonnement. Facturering voor Azure-services wordt uitgevoerd op basis van per abonnement. Zie voor een lijst van de aanbiedingen beschikbaar abonnement door type [Details van Microsoft Azure-aanbieding](https://azure.microsoft.com/support/legal/offer-details/).

#### <a name="administrator-roles"></a>Beheerdersrollen

Een Azure-abonnement heeft meerdere account administratorrollen die u op elk gewenst moment kunt toewijzen.

-   **Accountbeheerder**: deze rol heeft volledig beheer over het abonnement en het account dat verantwoordelijk is voor facturering.

-   **Servicebeheerder**: deze rol heeft controle over alle services in het abonnement. Standaard is dit hetzelfde als het Account Administrator-account.

-   **Medebeheerder**: deze rol heeft dezelfde toegangsrechten als de servicebeheerder, behalve dat deze de koppeling van het abonnement niet in een Azure-map wijzigen.

Zie voor meer informatie over beheerdersrollen [toevoegen of wijzigen van de Azure-beheerdersrollen](../../billing/billing-add-change-azure-subscription-administrator.md#add-an-admin-for-a-subscription).

#### <a name="resource-groups"></a>Resourcegroepen

Wanneer u een nieuwe Azure-services inricht, kunt u dit doen in een bepaald abonnement. Afzonderlijke Azure-services, ook wel resources genoemd, worden gemaakt in de context van een resourcegroep. Resourcegroepen eenvoudiger te implementeren en beheren van resources van uw toepassing. Een resourcegroep moet alle resources voor uw toepassing die u wilt werken als een eenheid bevatten. U kunt resources verplaatsen tussen resourcegroepen en zelfs met verschillende abonnementen behoren. Zie voor meer informatie over het verplaatsen van resources, [resources verplaatsen naar de nieuwe resourcegroep of abonnement](../../resource-group-move-resources.md).

De Azure Resourceverkenner is een uitstekend hulpprogramma voor het visualiseren van de resources die u al hebt gemaakt in uw abonnement. Zie voor meer informatie, [Azure Resource Explorer gebruiken om te bekijken en wijzigen van resources](../../resource-manager-resource-explorer.md).

#### <a name="grant-access-to-resources"></a>Toegang verlenen tot bronnen

Als u toegang tot Azure-resources toestaat, maar het is altijd een best practice om gebruikers te bieden met de minimale bevoegdheden die nodig is om een bepaalde taak uitvoeren.

-   **Op rollen gebaseerde toegangsbeheer (RBAC)**: In Azure, u kunt toegang verlenen tot gebruikersaccounts (principals) bij een opgegeven bereik: abonnement, resourcegroep of afzonderlijke resources. RBAC kunt u een set resources implementeren in een resourcegroep en machtigingen verlenen voor een specifieke gebruiker of groep. Ook kunt u de toegang beperken tot alleen de resources die deel uitmaken van de doelresourcegroep. U kunt ook toegang verlenen tot één resource, zoals een virtuele machine of een virtueel netwerk. Om toegang te verlenen, kunt u een rol toewijzen aan de gebruiker, groep of service-principal. Er zijn veel vooraf gedefinieerde rollen en u kunt ook uw eigen aangepaste rollen definiëren.

    >**Wanneer gebruikt u**: wanneer u Geavanceerd toegangsbeheer voor gebruikers en groepen nodig.

    >**Aan de slag**: Zie voor meer informatie, [aan de slag met toegangsbeheer in Azure portal](../../active-directory/role-based-access-control-what-is.md).

-   **Service-principals**: naast toegang verlenen tot principals die gebruikers en groepen, kunt u dezelfde toegang verlenen aan een service-principal.

    > **Wanneer gebruikt u**: wanneer u programmatisch beheer van Azure-resources of verlenen van toegang voor toepassingen. Zie voor meer informatie [een Active Directory-toepassing en service-principal](../../resource-group-create-service-principal-portal.md).

#### <a name="tags"></a>Tags

Azure Resource Manager kunt u aangepaste labels toewijzen aan afzonderlijke resources. Labels die sleutel-waardeparen, kunnen nuttig zijn als u nodig hebt om resources voor facturering of bewaking te organiseren. Tags bieden een manier om bij te houden van bronnen tussen meerdere resourcegroepen. U kunt de labels in de portal in de Azure Resource Manager-sjabloon of programmatisch met behulp van de REST-API, de Azure CLI of PowerShell kunt toewijzen. U kunt meerdere labels toewijzen aan elke resource. Zie voor meer informatie, [met labels om uw Azure-resources te organiseren](../../resource-group-using-tags.md).

### <a name="billing"></a>Facturering

Zijn belangrijke problemen in het verplaatsen van on-premises computers tot de cloud gehoste services schatten gebruik van de service en gerelateerde kosten en bij te houden. Het is belangrijk om te schatten nieuwe resources kosten om uit te voeren op maandelijkse basis. U moet ook mogelijk om de weergave van de facturering voor een bepaalde maand op basis van de huidige uitgaven te projecteren.

#### <a name="get-resource-usage-data"></a>Gegevens over brongebruik ophalen

Azure biedt een reeks facturering REST-API's waarmee toegang tot het brongebruik en informatie over metagegevens voor Azure-abonnementen. Deze API facturering's bieden u de mogelijkheid om beter te voorspellen en beheren van Azure kosten. U kunt bijhouden en analyseren uitgaven in perioden van een uur, bestedingslimiet waarschuwingen maken en voorspellen van toekomstige facturering op basis van huidige trends in gebruik.

>**Aan de slag**: Zie voor meer informatie over het gebruik van de API's van facturering, [overzicht van Azure-facturering gebruik en RateCard APIs](../../billing-usage-rate-card-overview.md).

#### <a name="predict-future-costs"></a>Voorspellen van toekomstige kosten

Hoewel het moeilijk is om in te schatten kosten tevoren, Azure heeft een [prijscategorie Rekenmachine](https://azure.microsoft.com/pricing/calculator/) resources die u gebruiken kunt wanneer u een schatting maken van de kosten van geïmplementeerd. U kunt ook de blade facturering in de portal en de facturering REST API's gebruiken om in te schatten toekomstige kosten, op basis van huidige verbruik.

>**Aan de slag**: Zie [overzicht van Azure-facturering gebruik en RateCard APIs](../../billing-usage-rate-card-overview.md).

#### <a name="set-up-billing-alerts"></a>Meldingen voor facturering instellen

Nadat u uw toepassing of oplossing op Azure hebt geïmplementeerd, kunt u waarschuwingen die dat u een e-mail verzendt wanneer u de bestedingslimiet limieten die zijn gedefinieerd in de waarschuwing benadert.

>**Aan de slag**: Zie voor meer informatie, [waarschuwingen voor uw Microsoft Azure-abonnementen facturering instellen](../../billing-set-up-alerts.md).
