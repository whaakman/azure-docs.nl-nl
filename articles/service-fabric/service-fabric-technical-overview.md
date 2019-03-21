---
title: Terminologie voor Azure Service Fabric | Microsoft Docs
description: Een overzicht van de terminologie van Service Fabric. Belangrijke terminologie concepten en termen die worden gebruikt in de rest van de documentatie besproken.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: chackdan;subramar
ms.assetid: 3a970679-e19e-43b3-9be8-71773f307c57
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/17/2018
ms.author: ryanwi
ms.openlocfilehash: fda6af0f253457aaf3aef1e8444850592255b318
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58113670"
---
# <a name="service-fabric-terminology-overview"></a>Overzicht van service Fabric-terminologie
Azure Service Fabric is een gedistribueerde systemen platform waarmee u gemakkelijk pakket, implementeren en beheren van schaalbare en betrouwbare microservices.  U kunt [host Service Fabric-clusters overal](service-fabric-deploy-anywhere.md): Azure, in een on-premises datacenter of op elke andere cloudprovider.  Service Fabric is de orchestrator dat wordt gebruikt door [Azure Service Fabric NET](/azure/service-fabric-mesh). U kunt elk gewenst framework schrijven van uw services en kiezen waar u wilt uitvoeren van de toepassing uit meerdere mogelijkheden van de omgeving. Dit artikel worden de termen die door Service Fabric om te begrijpen van de termen die worden gebruikt in de documentatie.

## <a name="infrastructure-concepts"></a>Infrastructuur-concepten
**Cluster**: Een netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd.  Clusters kunnen naar duizenden machines worden geschaald.

**Knooppunt**: Een machine of virtuele machine die deel uitmaakt van een cluster, heet een *knooppunt*. Aan elk knooppunt wordt een knooppuntnaam toegewezen (een tekenreeks). Knooppunten hebben kenmerken zoals plaatsingseigenschappen. Elke computer of virtuele machine heeft een Windows-service die automatisch wordt gestart, `FabricHost.exe`, die wordt uitgevoerd bij het opstarten en start vervolgens twee uitvoerbare bestanden: `Fabric.exe` en `FabricGateway.exe`. Deze twee uitvoerbare bestanden zijn vormen van het knooppunt. Voor het testen van scenario's, kunt u meerdere knooppunten op een enkele machine of virtuele machine host door het uitvoeren van meerdere exemplaren van `Fabric.exe` en `FabricGateway.exe`.

## <a name="application-and-service-concepts"></a>Toepassing en serviceconcepten

**Service Fabric-toepassing voor Mesh**: Service Fabric NET-toepassingen worden beschreven door de Resource-Model (YAML en JSON-resource-bestanden) en kan worden geïmplementeerd voor een omgeving waarin de Service Fabric wordt uitgevoerd.

**Service Fabric-systeemeigen toepassing**: Native service Fabric-toepassingen worden beschreven door de systeemeigen toepassing-Model (op basis van een XML-toepassings- en servicemanifesten).  Service Fabric-systeemeigen toepassingen uitvoeren niet in Service Fabric NET.

### <a name="service-fabric-mesh-application-concepts"></a>Concepten van service Fabric NET-toepassing

**Toepassing**: Een toepassing is de eenheid van de implementatie, versiebeheer en levensduur van een Mesh-toepassing. De levenscyclus van elk toepassingsexemplaar kan onafhankelijk van elkaar worden beheerd.  Toepassingen bestaan uit een of meer code-servicepakketten en -instellingen. Een toepassing is gedefinieerd met behulp van het schema van de Azure Resource-Model (RM).  Services worden omschreven als de eigenschappen van de bron van de toepassing in een RM-sjabloon.  Netwerken en de volumes die worden gebruikt door de toepassing wordt verwezen door de toepassing.  Bij het maken van een toepassing, zijn de toepassing, de service (s), het netwerk en de volumes gemodelleerd met behulp van het Resourcemodel dat Service Fabric.

**Service**: Een service in een toepassing een microservice vertegenwoordigt en vervult een functie volledig en zelfstandig. Elke service bestaat uit een of meer codepakketten die alles wat nodig is om uit te voeren van de containerinstallatiekopie die is gekoppeld aan het codepakket wordt beschreven.  Het aantal services in een toepassing kunt omhoog en omlaag worden geschaald.

**Netwerk**: Een netwerkbron maakt een particulier netwerk voor uw toepassingen en staat los van de toepassingen of services die naar deze verwijzen kunnen. Meerdere services van verschillende toepassingen kunnen deel uitmaken van hetzelfde netwerk. Netwerken zijn implementeerbare resources waarnaar wordt verwezen door toepassingen.

**Codepakket**: Codepakketten beschrijven alles wat nodig is om uit te voeren van de installatiekopie van de container zijn gekoppeld aan de codepakket, met inbegrip van het volgende:

* De containernaam van de, versie en -register
* CPU en geheugen-resources die vereist voor elke container
* Network-eindpunten
* Volumes te koppelen aan de container, die verwijst naar een ander volume-resource.

Alle codepakketten die worden gedefinieerd als onderdeel van de bron van een toepassing worden geïmplementeerd en geactiveerd als een groep.

**Volume**: Volumes zijn mappen die in uw containerinstanties die u gebruiken kunt om vast te leggen de status worden gekoppeld. Het volume-stuurprogramma voor Azure Files koppelt een Azure-bestandsshare naar een container en biedt betrouwbare gegevensopslag via een API die ondersteuning biedt voor bestandsopslag. Volumes zijn implementeerbare resources waarnaar wordt verwezen door toepassingen.

### <a name="service-fabric-native-application-concepts"></a>Concepten van Native service Fabric-toepassing

**Toepassing**: Een toepassing is een verzameling van de services die een bepaalde functie of functies uitvoeren. De levenscyclus van elk toepassingsexemplaar kan onafhankelijk van elkaar worden beheerd.

**Service**: Een service kunt vervult een functie volledig en zelfstandig en starten en onafhankelijk van andere services worden uitgevoerd. Een service bestaat uit code, configuratie en gegevens. Voor elke service code bestaat uit de uitvoerbare binaire bestanden, configuratie bestaat uit service-instellingen die kunnen worden geladen tijdens de uitvoering en gegevens bestaat uit willekeurige statische gegevens om te worden verbruikt door de service.

**Toepassingstype**: De naam of de versie die is toegewezen aan een verzameling van servicetypen. Dit is gedefinieerd in een `ApplicationManifest.xml` bestand en de ingesloten in een pakket toepassingsmap. De map wordt vervolgens gekopieerd naar de installatiekopieopslag van het Service Fabric-cluster. U kunt vervolgens een toepassing met de naam van dit toepassingstype binnen het cluster maken.

Lees de [toepassingsmodel](service-fabric-application-model.md) artikel voor meer informatie.

**Toepassingspakket**: Een map met het toepassingstype `ApplicationManifest.xml` bestand. Verwijst naar de servicepakketten voor elk servicetype dat het toepassingstype. De bestanden in de toepassingsmap pakket worden gekopieerd naar de installatiekopieopslag van Service Fabric-cluster. Een toepassingspakket voor een e-mailtype toepassing kan bijvoorbeeld verwijzingen naar een pakket queue-service, een front-end-service-pakket en een database-service-pakket bevatten.

**Met de naam toepassing**: Nadat u een toepassingspakket naar de installatiekopieopslag kopiëren, maakt u een exemplaar van de toepassing binnen het cluster. U kunt een exemplaar maken wanneer u het toepassingspakket toepassingstype, opgeeft met behulp van de naam of versie. Elk type-exemplaar is de naam van een uniform resource identifier (URI) die als eruitziet toegewezen: `"fabric:/MyNamedApp"`. Binnen een cluster, kunt u meerdere benoemde toepassingen kunt maken van een type één toepassing. U kunt ook met de naam toepassingen van verschillende toepassingstypen maken. Elke toepassing met de naam is onafhankelijk beheerd en is samengesteld.

**Servicetype**: De naam of de versie die is toegewezen aan de pakketten, gegevenspakketten en van configuratiepakketten van een service. Type van de service is gedefinieerd in de `ServiceManifest.xml` bestands- en opgenomen in de map van een service-pakket. De map van de service-pakket vervolgens naar wordt verwezen door een toepassingspakket `ApplicationManifest.xml` bestand. Binnen het cluster kunt na het maken van een toepassing met de naam, u een benoemde service van een van het toepassingstype servicetypen. Het servicetype `ServiceManifest.xml` bestand beschrijft de service.

Lees de [toepassingsmodel](service-fabric-application-model.md) artikel voor meer informatie.

Er zijn twee soorten services:

* **Stateless**: Gebruik een stateless service als de permanente status van de service is opgeslagen in een service voor externe opslag, zoals Azure Storage, Azure SQL Database of Azure Cosmos DB. Gebruik een stateless service als de service geen permanente opslag heeft. Bijvoorbeeld, voor een rekenmachine-service waar waarden worden doorgegeven aan de service, een berekening wordt uitgevoerd die gebruikmaakt van deze waarden en vervolgens wordt een resultaat geretourneerd.
* **Stateful**: Een stateful service gebruiken als u wilt dat de Service Fabric voor het beheren van de status van uw service via de betrouwbare verzamelingen of Reliable Actors programmeermodellen. Wanneer u een benoemde service maakt, geeft u het aantal partities die u uw status gespreid wilt over voor schaalbaarheid. Ook kunt u opgeven hoe vaak voor het repliceren van uw status over meerdere knooppunten voor betrouwbaarheid. Elke benoemde service heeft een enkele primaire replica en meerdere secundaire replica's. U wijzigen de status van uw service met de naam bij het schrijven naar de primaire replica. Service Fabric wordt deze status vervolgens gerepliceerd naar alle secundaire replica's naar de status gesynchroniseerd houden. Service Fabric wordt automatisch gedetecteerd wanneer een primaire replica is mislukt en het schrijven van een bestaande secundaire replica naar een primaire replica. Service Fabric maakt vervolgens een nieuwe secundaire replica.  

**Replica's of instanties** verwijzen naar de code (en de status voor stateful services) van een service die wordt geïmplementeerd en uitgevoerd. Zie [replica's en exemplaren](service-fabric-concepts-replica-lifecycle.md).

**Herconfiguratie** verwijst naar het proces van eventuele wijzigingen in de replicaset van een service. Zie [herconfiguratie](service-fabric-concepts-reconfiguration.md).

**Servicepakket**: Een map met het servicetype `ServiceManifest.xml` bestand. Dit bestand verwijst naar de code, statische gegevens en configuratiepakketten voor het type van de service. De bestanden in de map van de service-pakket wordt verwezen door het toepassingstype `ApplicationManifest.xml` bestand. Bijvoorbeeld, kunt een servicepakket verwijzen naar de code, statische gegevens en configuratiepakketten die gezamenlijk een databaseservice.

**Met de naam service**: Nadat u een toepassing met de naam maakt, kunt u een exemplaar van een van de servicetypen binnen het cluster kunt maken. U opgeven het servicetype met behulp van de naam/versie. Elk type service-exemplaar is de naam van een URI die binnen het bereik van onder de naam van de toepassing URI toegewezen. Bijvoorbeeld, als u een service in een toepassing met de naam 'MyNamedApp' met de naam 'MijnDatabase' maakt, de URI ziet eruit als: `"fabric:/MyNamedApp/MyDatabase"`. In een toepassing met de naam, kunt u verschillende services worden gebruikt met de naam maken. Elke benoemde service kan hebben een eigen partitieschema en exemplaar of replica wordt geteld.

**Codepakket**: Een diskette-map met het servicetype uitvoerbare bestanden, doorgaans EXE/DLL-bestanden. De bestanden in de pakketmap code wordt verwezen door het servicetype `ServiceManifest.xml` bestand. Wanneer u een benoemde service maakt, wordt het codepakket gekopieerd naar het knooppunt of knooppunten die zijn geselecteerd om uit te voeren van de benoemde service. Vervolgens de code wordt gestart om uit te voeren. Er zijn twee soorten code pakket uitvoerbare bestanden:

* **Gastbestanden**: Uitvoerbare bestanden die worden uitgevoerd als-is op het hostbesturingssysteem (Windows of Linux). Deze uitvoerbare bestanden niet koppelen aan of verwijzen naar een Service Fabric-runtime-bestanden en daarom geen gebruik van een Service Fabric-programmeermodellen. Deze uitvoerbare bestanden zijn niet sommige functies van de Service Fabric, zoals de naming-service voor de eindpuntdetectie van gebruiken. Gastbestanden meetwaarden die specifiek voor elk service-exemplaar zijn kan niet rapporteren.
* **Uitvoerbare bestanden host-service**: Uitvoerbare bestanden die gebruikmaken van Service Fabric-programmeermodellen door een koppeling naar Service Fabric-runtime-bestanden, Service Fabric-functies inschakelen. Bijvoorbeeld, een benoemd service-exemplaar eindpunten kunt registreren bij de Service Fabric Naming-Service en kan de meetwaarden ook rapporteren.

**Gegevenspakket**: Een diskette-map die het servicetype statisch, alleen-lezen-gegevensbestanden, doorgaans foto's, Beeld en geluid bestanden bevat. De bestanden in de map wordt verwezen door het servicetype `ServiceManifest.xml` bestand. Wanneer u een benoemde service maakt, wordt het gegevenspakket gekopieerd naar het knooppunt of knooppunten die zijn geselecteerd om uit te voeren van de benoemde service. De code wordt uitgevoerd en hebben nu toegang tot de bestanden.

**Het configuratiepakket**: Een diskette-map met het servicetype statisch, alleen-lezen-configuratiebestanden, doorgaans tekstbestanden. De bestanden in de map van het pakket configuration wordt verwezen door het servicetype `ServiceManifest.xml` bestand. Wanneer u een benoemde service maakt, de bestanden in het configuratiepakket voor de gekopieerde één of meer knooppunten hebt geselecteerd de benoemde service uit te voeren. Vervolgens begint de code uit te voeren en kunt nu toegang tot de configuratiebestanden.

**Containers**: Standaard wordt Service Fabric implementeert en services als processen wordt geactiveerd. Service Fabric kunt u ook services in containerinstallatiekopieën implementeren. Containers zijn een virtualisatietechnologie die het onderliggende besturingssysteem van toepassingen virtualiseert. Een toepassing en de runtime, afhankelijkheden en systeembibliotheken worden uitgevoerd binnen een container. De container heeft volledige, persoonlijke toegang tot de container eigen geïsoleerde weergave van de concepten van het besturingssysteem. Service Fabric biedt ondersteuning voor Docker-containers op Linux- en Windows Server-containers. Lees voor meer informatie, [Service Fabric en containers](service-fabric-containers-overview.md).

**Partitieschema**: Wanneer u een benoemde service maakt, kunt u een partitieschema opgeven. Services met een aanzienlijke hoeveelheid status splitsen van de gegevens over meerdere partities, die de status verspreidt over knooppunten van het cluster. Door het splitsen van de gegevens over meerdere partities, de status van de benoemde service kan worden geschaald. Binnen een partitie hebben stateless services voor benoemde exemplaren, terwijl stateful services met de naam replica's hebben. Stateless benoemde services hebben meestal slechts één partitie, omdat ze geen interne status hebben. De partitie-instanties bieden voor beschikbaarheid. Als één instantie is mislukt, doorgaan met andere exemplaren normaal functioneren en Service Fabric maakt vervolgens een nieuw exemplaar. Stateful services met de naam behouden hun status binnen de replica's en elke partitie heeft een eigen replica instellen, zodat de status wordt bewaard gesynchroniseerd. Een replica mislukt, bouwt Service fabrics een nieuwe replica van de bestaande replica's.

Lees de [Partition Service Fabric betrouwbare services](service-fabric-concepts-partitioning.md) artikel voor meer informatie.

## <a name="system-services"></a>Systeemservices
Er zijn systeemservices die zijn gemaakt in elk cluster waarmee de mogelijkheden van het platform van Service Fabric.

**Naamgevingsservice**: Elke Service Fabric-cluster heeft een Naming-Service, die de servicenamen worden omgezet in een locatie in het cluster. U beheert de servicenamen en de eigenschappen op, zoals een internet Domain Name System (DNS) voor het cluster. Clients communiceren veilig met een willekeurig knooppunt in het cluster met behulp van de Naming-Service een servicenaam en de locatie op te lossen. Toepassingen verplaatsen binnen het cluster. Dit kan bijvoorbeeld zijn vanwege fouten, bron-balancing of de grootte van het cluster. U kunt ontwikkelen services en -clients om de huidige netwerklocatie op te lossen. Clients downloaden de werkelijke machine IP-adres en poort waar deze momenteel wordt uitgevoerd.

Lezen [communiceren met services](service-fabric-connect-and-communicate-with-services.md) voor meer informatie over de communicatie-client en service-API's die met de Naming-Service werken.

**Afbeelding Store-service**: Elke Service Fabric-cluster heeft een installatiekopie van Store-service waar geïmplementeerd, is samengesteld toepassingspakketten worden gehouden. Een toepassingspakket kopiëren naar de Store-installatiekopie en Registreer het toepassingstype dat is opgenomen in die toepassingspakket. Nadat het toepassingstype is ingericht, maakt u een benoemde toepassing uit. U kunt de registratie van een toepassingstype van de installatiekopie van Store-service ongedaan maken nadat alle benoemde toepassingen zijn verwijderd.

Lezen [inzicht in de instelling ImageStoreConnectionString](service-fabric-image-store-connection-string.md) voor meer informatie over de installatiekopie van Store-service.

Lees de [implementeren van een toepassing](service-fabric-deploy-remove-applications.md) artikel voor meer informatie over het implementeren van toepassingen op de installatiekopie van Store-service.

**Failover Manager-service**: Elke Service Fabric-cluster heeft een Failover Manager-service die verantwoordelijk is voor de volgende acties:
   - Functies met betrekking tot hoge beschikbaarheid en consistentie van de services worden uitgevoerd.
   - Toepassing en cluster upgrades coördineert.
   - Communiceert met andere onderdelen van het systeem.

**Reparatiemanager service**: Dit is een optionele systeemservice waarmee acties voor herstellen op een cluster in een veilige, automatiseerbare en transparante manier worden uitgevoerd. Reparatiemanager wordt gebruikt in:
   - Uitvoeren van onderhoud van Azure herstelt op [Silver en Gold duurzaamheid](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) Azure Service Fabric-clusters.
   - Uitvoering van acties voor herstellen voor [Patch Orchestration-toepassing](service-fabric-patch-orchestration-application.md)

## <a name="deployment-and-application-models"></a>Implementatie en toepassing modellen 

Voor het implementeren van uw services, moet u om te beschrijven hoe u deze wilt uitvoeren. Service Fabric ondersteunt drie verschillende implementatiemodellen:

### <a name="resource-model-preview"></a>Resource-model (preview)
Service Fabric-Resources worden alle items die afzonderlijk kunnen worden geïmplementeerd naar Service Fabric; met inbegrip van toepassingen, services, netwerken en volumes. Resources worden gedefinieerd met behulp van een JSON-bestand naar een clustereindpunt kan worden geïmplementeerd.  Voor Service Fabric NET, wordt het Model van Azure-Resource-schema gebruikt. Een YAML-bestandsschema kan ook worden gebruikt om eenvoudiger definitiebestanden. Resources kunnen worden geïmplementeerd waar dan ook die service Fabric wordt uitgevoerd. Het resourcemodel is de eenvoudigste manier om te beschrijven van uw Service Fabric-toepassingen. De nadruk ligt op de eenvoudige implementatie en beheer van services in containers. Voor meer informatie lezen [Inleiding tot de Service Fabric-Resourcemodel](/azure/service-fabric-mesh/service-fabric-mesh-service-fabric-resources).

### <a name="native-model"></a>Systeemeigen model
Het model systeemeigen toepassing biedt uw toepassingen met volledige op laag niveau toegang tot Service Fabric. Toepassingen en services worden gedefinieerd als het geregistreerde type in XML-manifestbestanden.

De systeemeigen model biedt ondersteuning voor de Reliable Services en Reliable Actors-frameworks, waarmee u toegang tot de Service Fabric-runtime-API's en cluster-API's in C# en Java. Het oorspronkelijke gegevensmodel biedt ook ondersteuning voor willekeurige containers en uitvoerbare bestanden. Het oorspronkelijke gegevensmodel wordt niet ondersteund in de [Service Fabric-NET-omgeving](/azure/service-fabric-mesh/service-fabric-mesh-overview).

**Reliable Services**: Een API voor het bouwen van staatloze en stateful services. Stateful services voor het opslaan van hun status in Reliable Collections, zoals een woordenlijst of een wachtrij. U kunt ook aansluiten in verschillende communicatie-stacks, zoals Web-API en Windows Communication Foundation (WCF).

**Reliable Actors**: Een API voor het bouwen van staatloze en stateful objecten door de virtuele Actor-programmeermodel. Dit model is nuttig wanneer u veel onafhankelijke eenheden van berekening of status hebt. Dit model gebruikt een beurt threadmodel, zodat het is raadzaam om te voorkomen dat code aan andere actors of services aanroept omdat andere binnenkomende aanvragen door een afzonderlijke actor kan niet worden verwerkt totdat alle uitgaande aanvragen zijn voltooid.

U kunt ook uw bestaande toepassingen uitvoeren in Service Fabric:

**Containers**:  Service Fabric ondersteunt de implementatie van Docker-containers op Linux en Windows Server-containers in Windows Server 2016, samen met de ondersteuning voor Hyper-V-isolatiemodus. In de Service Fabric [toepassingsmodel](service-fabric-application-model.md), een container vertegenwoordigt een toepassingshost in welke meerdere service replica's worden geplaatst. Service Fabric kan geen containers worden uitgevoerd en het scenario is vergelijkbaar met het Gast uitvoerbare scenario, waarin u een bestaande toepassing binnen een container verpakken. Bovendien kunt u [Service Fabric-services in containers uitvoeren](service-fabric-services-inside-containers.md) ook.

**Gastbestanden**: Als een service kunt u elk type code, zoals Node.js, Java of C++ uitvoeren in Azure Service Fabric. Service Fabric verwijst naar deze typen services als uitvoerbare gastbestanden, worden behandeld als stateless services. De voordelen van het uitvoeren van een door gasten uitvoerbare bestanden in een Service Fabric-cluster zijn hoge beschikbaarheid, statuscontrole, levensduurbeheer van toepassingen, met hoge dichtheid en zichtbaarheid.

Lees de [programmeermodel voor uw service kiezen](service-fabric-choose-framework.md) artikel voor meer informatie.

### <a name="docker-compose"></a>Docker Compose 
[Docker Compose](https://docs.docker.com/compose/) maakt deel uit van de Docker-project. Service Fabric biedt beperkte ondersteuning voor [implementeren van toepassingen met behulp van het model Docker Compose](service-fabric-docker-compose.md).

## <a name="environments"></a>Omgevingen

Service Fabric is een open-source platform-technologie die verschillende verschillende services en producten zijn gebaseerd op. Microsoft biedt de volgende opties:

 - **Azure Service Fabric Mesh**: Een volledig beheerde service voor het uitvoeren van Service Fabric-toepassingen in Microsoft Azure.
 - **Azure Service Fabric**: De door Azure gehoste Service Fabric-cluster-aanbieding. Deze service biedt integratie tussen Service Fabric en de Azure-infrastructuur, samen met de upgrade en configuratie van beheer van Service Fabric-clusters.
 - **Zelfstandige service Fabric**: Een set hulpprogramma's de installatie en configuratie voor [overal Service Fabric-clusters implementeren](/azure/service-fabric/service-fabric-deploy-anywhere) (on-premises of op elke andere cloudprovider). Niet beheerd door Azure.
 - **Service Fabric-cluster voor ontwikkeling**: Biedt een lokale ontwikkeling onder Windows, Linux of Mac voor ontwikkeling van Service Fabric-toepassingen.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Omgeving framework en implementatiemodel ondersteuningsmatrix
Verschillende omgevingen hebben een ander niveau van ondersteuning van frameworks en -implementatiemodellen. De volgende tabel beschrijft de ondersteunde framework en combinaties van implementatie-model.

| Type van de toepassing | Wordt beschreven in | Azure Service Fabric Mesh | Azure Service Fabric-Clusters (elk besturingssysteem)| Lokale cluster | Zelfstandig cluster |
|---|---|---|---|---|---|
| Service Fabric Mesh-toepassingen | Resource-Model (YAML & JSON) | Ondersteund |Niet ondersteund | Windows - ondersteund, Linux en Mac-niet ondersteund | Windows-niet ondersteund |
|Native service Fabric-toepassingen | Systeemeigen toepassingsmodel (XML) | Niet ondersteund| Ondersteund|Ondersteund|Windows: ondersteund|

De volgende tabel beschrijft de modellen van de andere toepassing en de tooling die voor hen op basis van Service Fabric bestaat.

| Type van de toepassing | Wordt beschreven in | Visual Studio | Eclipse | SFCTL | AZ CLI | PowerShell|
|---|---|---|---|---|---|---|
| Service Fabric Mesh-toepassingen | Resource-Model (YAML & JSON) | VS 2017 |Niet ondersteund |Niet ondersteund | Ondersteund - alleen Mesh-omgeving | Niet ondersteund|
|Native service Fabric-toepassingen | Systeemeigen toepassingsmodel (XML) | Visual Studio 2017 en VS 2015| Ondersteund|Ondersteund|Ondersteund|Ondersteund|

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over Service Fabric:

* [Overzicht van Service Fabric](service-fabric-overview.md)
* [Waarom een microservices-benadering voor het ontwikkelen van toepassingen?](service-fabric-overview-microservices.md)
* [Toepassingsscenario's](service-fabric-application-scenarios.md)

Voor meer informatie over Service Fabric NET:

* [Overzicht van Service Fabric Mesh](/azure/service-fabric-mesh/service-fabric-mesh-overview)
