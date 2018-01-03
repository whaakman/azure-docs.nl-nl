---
title: Meer informatie over Azure Service Fabric-terminologie | Microsoft Docs
description: Een overzicht van de terminologie van Service Fabric. Belangrijkste termen concepten en termen die in de rest van de documentatie besproken.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: chackdan;subramar
ms.assetid: 3a970679-e19e-43b3-9be8-71773f307c57
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/18/2017
ms.author: ryanwi
ms.openlocfilehash: dc7e536ce40bf95e1950e1e44844cd8fe26ea1a1
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="service-fabric-terminology-overview"></a>Overzicht van service Fabric-terminologie
Azure Service Fabric is een gedistribueerde systemen platform waarmee u gemakkelijk pakket, implementeren en beheren van schaalbare en betrouwbare microservices. In dit artikel wordt de terminologie die wordt gebruikt door de Service Fabric om te begrijpen van de termen die worden gebruikt in de documentatie.

De concepten die worden vermeld in deze sectie worden ook beschreven in de volgende video's van Microsoft Virtual Academy: <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">concepten Core</a>, <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965">ontwerptijd concepten</a>, en <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">Runtime concepten</a>.

## <a name="infrastructure-concepts"></a>Concepten van de infrastructuur
**Cluster**: een set netwerk verbonden virtuele of fysieke machines waarin uw microservices worden geïmplementeerd en beheerd.  Clusters kunnen worden geschaald naar duizenden computers.

**Knooppunt**: een machine of virtuele machine die deel uitmaakt van een cluster heet een *knooppunt*. Elk knooppunt wordt de knooppuntnaam van een (een tekenreeks) toegewezen. Knooppunten hebben kenmerken, zoals plaatsingseigenschappen. Elke computer of virtuele machine heeft een Windows-service van het automatisch starten, `FabricHost.exe`, die wordt gestart bij het opstarten en start vervolgens twee uitvoerbare bestanden: `Fabric.exe` en `FabricGateway.exe`. Deze twee uitvoerbare bestanden, vormen het knooppunt. Voor het testen van scenario's, kunt u meerdere knooppunten op een enkele computer of virtuele machine host door het uitvoeren van meerdere exemplaren van `Fabric.exe` en `FabricGateway.exe`.

## <a name="application-concepts"></a>Concepten van toepassing
**Toepassingstype**: de naam of de versie die is toegewezen aan een verzameling van servicetypen. Dit is gedefinieerd in een `ApplicationManifest.xml` bestands- en ingesloten in een pakket toepassingsmap. De map wordt vervolgens gekopieerd naar het archief van de installatiekopie van het Service Fabric-cluster. Vervolgens kunt u een toepassing met de naam van dit toepassingstype binnen het cluster maken.

Lees de [toepassingsmodel](service-fabric-application-model.md) artikel voor meer informatie.

**Toepassingspakket**: een map met het toepassingstype `ApplicationManifest.xml` bestand. Verwijst naar de servicepakketten voor elk servicetype dat het toepassingstype. De bestanden in de toepassingsmap pakket gekopieerd naar de installatiekopieopslag Service Fabric-cluster. Een toepassingspakket voor een e-mailtype toepassing bevat mogelijk verwijzingen naar een pakket queue-service, een pakket frontend-service en een database-service-pakket.

**Naam application**: nadat u een pakket naar de image store kopiëren, maakt u een exemplaar van de toepassing binnen het cluster. U kunt geen exemplaar maken wanneer u het toepassingspakket toepassingstype opgeeft met de naam of versie. Elk type toepassingsexemplaar is een uniform resource identifier (URI)-naam die lijkt op toegewezen: `"fabric:/MyNamedApp"`. U kunt meerdere benoemde toepassingen binnen een cluster maken van een type één toepassing. U kunt ook benoemde toepassingen maken van verschillende toepassingstypen. Elke toepassing met de naam is onafhankelijk van elkaar worden beheerd en samengestelde.      

**Servicetype**: de naam of de versie die is toegewezen aan een service-code pakketten, gegevenspakketten en configuratiepakketten. Type van de service is gedefinieerd in de `ServiceManifest.xml` bestands- en ingesloten in een pakket servicemap. De map van de service-pakket vervolgens naar wordt verwezen door een toepassingspakket `ApplicationManifest.xml` bestand. Binnen het cluster kunt na het maken van een toepassing met de naam, u een service met de naam van een van het toepassingstype servicetypen. Het servicetype `ServiceManifest.xml` bestand beschrijving van de service.

Lees de [toepassingsmodel](service-fabric-application-model.md) artikel voor meer informatie.

Er zijn twee soorten services:

* **Staatloze**: een stateless service gebruiken als de permanente status van de service is opgeslagen in een service voor externe opslag, zoals Azure Storage, Azure SQL Database of Azure Cosmos DB. Gebruik een stateless service als de service geen permanente opslag heeft. Bijvoorbeeld: voor een service Rekenmachine waarbij waarden worden doorgegeven aan de service, een berekening die gebruikmaakt van deze waarden wordt uitgevoerd en wordt een resultaat geretourneerd.
* **Stateful**: een stateful service gebruiken als u wilt dat de Service Fabric voor het beheren van de status van uw service via de betrouwbare verzamelingen of Reliable Actors programming modellen. Wanneer u een benoemde service maakt, Geef op hoeveel partities die u uw status gespreid wilt over schaalbaarheid. Ook kunt u opgeven hoe vaak uw status repliceren op knooppunten voor betrouwbaarheid. Elke benoemde service heeft een enkele primaire replica en meerdere secundaire replica's. U kunt uw benoemde servicestatus wijzigen bij het schrijven naar de primaire replica. Deze status service Fabric vervolgens gerepliceerd naar alle secundaire replica's naar uw status synchroon te houden. Service Fabric wordt automatisch gedetecteerd wanneer een primaire replica is mislukt en bijdraagt aan een bestaande secundaire replica naar een primaire replica. Service Fabric maakt vervolgens een nieuwe secundaire replica.  

**Replica's of exemplaren** verwijzen naar de code (en status voor stateful services) van een service die wordt geïmplementeerd en uitgevoerd. Zie [replica's en -exemplaren](service-fabric-concepts-replica-lifecycle.md).

**Herconfiguratie** verwijst naar het proces van elke wijziging in de replicaset van een service. Zie [herconfiguratie](service-fabric-concepts-reconfiguration.md).

**Servicepakket**: een map met het servicetype `ServiceManifest.xml` bestand. Dit bestand verwijst naar de code, statische gegevens en configuratiepakketten voor servicetype. De bestanden in de map van de service-pakket wordt verwezen door het toepassingstype `ApplicationManifest.xml` bestand. Bijvoorbeeld, kunt een servicepakket verwijzen naar de code, statische gegevens en configuratiepakketten die gezamenlijk een database-service.

**Service met de naam**: nadat u een benoemde toepassing maakt, kunt u een exemplaar van een van de servicetypen binnen het cluster maken. U opgeven het servicetype met behulp van de naam of versie. Elk service type-exemplaar is de naam van een URI die onder de URI van de toepassing van het benoemde bereik toegewezen. Bijvoorbeeld, als u een service binnen een toepassing met de naam 'MyNamedApp' met de naam 'MijnDatabase' maakt, de URI eruit: `"fabric:/MyNamedApp/MyDatabase"`. In een toepassing met de naam, kunt u verschillende benoemde services maken. Elke benoemde service kan zijn eigen partitieschema en exemplaar of replica telt.

**Codepakket**: een map met het servicetype uitvoerbare bestanden doorgaans EXE/DLL-bestanden. De bestanden in de pakketmap code wordt verwezen door het servicetype `ServiceManifest.xml` bestand. Wanneer u een benoemde service maakt, wordt het codepakket gekopieerd naar het knooppunt of knooppunten die zijn geselecteerd voor het uitvoeren van de benoemde service. Vervolgens wordt de code gestart om uit te voeren. Er zijn twee soorten code pakket uitvoerbare bestanden:

* **Uitvoerbare bestanden Gast**: uitvoerbare bestanden die worden uitgevoerd als-is op de host-besturingssysteem (Windows of Linux). Deze uitvoerbare bestanden, niet koppelen aan of verwijst naar een Service Fabric-runtime-bestanden en daarom geen gebruikmaken van een Service Fabric programming modellen. Deze uitvoerbare bestanden kunnen niet worden sommige Service Fabric-functies, zoals de naamgevingsservice voor de detectie van het eindpunt te gebruiken. Gast uitvoerbare bestanden kan niet rapporteren load metrische gegevens die specifiek voor elk service-exemplaar zijn.
* **Service-host uitvoerbare bestanden**: uitvoerbare bestanden die gebruikmaken van Service Fabric programming modellen door te koppelen aan de Service Fabric-runtime-bestanden, Service Fabric-functies inschakelen. Bijvoorbeeld, een benoemd exemplaar eindpunten kunt registreren met Naming Service-Fabric-Service en load metrische gegevens ook kan rapporteren.      

**Gegevenspakket**: een map die het servicetype statische, alleen-lezen gegevensbestanden, doorgaans foto's, Beeld en geluid bestanden bevat. De bestanden in de gegevensdirectory van het pakket wordt verwezen door het servicetype `ServiceManifest.xml` bestand. Wanneer u een benoemde service maakt, wordt het gegevenspakket gekopieerd naar het knooppunt of knooppunten die zijn geselecteerd voor het uitvoeren van de benoemde service. De code wordt gestart en hebben nu toegang tot de gegevensbestanden.

**Configuratiepakket**: een map waarin het servicetype statische, alleen-lezen configuratiebestanden, doorgaans tekstbestanden. De bestanden in de configuratiemap voor het pakket wordt verwezen door het servicetype `ServiceManifest.xml` bestand. Wanneer u een benoemde service maakt, de bestanden in het configuratiepakket gekopieerde één of meer knooppunten hebt geselecteerd de benoemde service uit te voeren. Vervolgens wordt de code gestart uitgevoerd en kan nu toegang tot de configuratiebestanden.

**Containers**: standaard, Service Fabric worden geïmplementeerd en wordt geactiveerd services als processen. Service Fabric kunnen ook services implementeren in container-installatiekopieën. Containers zijn een virtualisatietechnologie die het onderliggende besturingssysteem van toepassingen virtualiseert. Een toepassing en de runtime, afhankelijkheden en systeembibliotheken worden uitgevoerd binnen een container. De container heeft volledige, persoonlijke toegang tot de container eigen geïsoleerde weergave van de concepten van het besturingssysteem. Service Fabric ondersteunt Docker-containers voor Linux en Windows Server-containers. Lees voor meer informatie [Service Fabric en containers](service-fabric-containers-overview.md).

**Partitieschema**: wanneer u een benoemde service maakt, u een partitieschema opgeven. De gegevens verdelen Services met een aanzienlijke hoeveelheid status over partities die de status verspreid over de knooppunten van het cluster. Door de gegevens meerdere partities verdelen kunt uw benoemde servicestatus schalen. Binnen een partitie hebben staatloze benoemde services gevallen terwijl stateful services met de naam replica's hebben. Staatloze benoemde services hebben meestal slechts één partitie omdat ze geen interne status hebben. De exemplaren van de partitie is voorzien van beschikbaarheid. Als één exemplaar is mislukt, doorgaan met andere exemplaren normaal functioneren en Service Fabric maakt vervolgens een nieuw exemplaar. Stateful services met de naam behouden hun status binnen de replica's en elke partitie heeft een eigen replica instellen, zodat de status wordt gesynchroniseerd. Moet een replica mislukken, Service Fabric-bouwt voort op een nieuwe replica van de bestaande replica's.

Lees de [partitie Service Fabric betrouwbare services](service-fabric-concepts-partitioning.md) artikel voor meer informatie.

## <a name="system-services"></a>Systeemservices
Er zijn systeemservices die zijn gemaakt in elk cluster waarmee de platformmogelijkheden van Service Fabric.

**Naming Service**: elke Service Fabric-cluster heeft een Naming Service die wordt omgezet servicenamen naar een locatie in het cluster. U beheert de servicenamen en eigenschappen, zoals een internet Domain Name System (DNS) voor het cluster. Clients worden veilig communiceren met een willekeurig knooppunt in het cluster met behulp van de Naming Service een servicenaam en de locatie op te lossen. Toepassingen verplaatsen binnen het cluster. Dit kan bijvoorbeeld zijn vanwege fouten, resource netwerktaakverdeling of de grootte van het cluster. U kunt ontwikkelen services en clients die de huidige netwerklocatie oplossen. Clients downloaden de werkelijke machine IP-adres en poort waar deze momenteel wordt uitgevoerd.

Lees [communiceren met services](service-fabric-connect-and-communicate-with-services.md) voor meer informatie over de client en service communicatie API's die met de Naming Service werken.

**Image Store-service**: elke Service Fabric-cluster heeft een Image Store-service waarop de geïmplementeerde, samengestelde toepassingspakketten worden bewaard. Toepassingspakketten kopiëren naar de Image Store en Registreer het toepassingstype die zich in die toepassingspakket. Nadat het toepassingstype is ingericht, maakt u een toepassing met de naam van het. U kunt registratie van een toepassingstype van de Image Store-service nadat alle benoemde toepassingen zijn verwijderd.

Lees [inzicht in de instelling ImageStoreConnectionString](service-fabric-image-store-connection-string.md) voor meer informatie over de Image Store-service.

Lees de [implementeren van een toepassing](service-fabric-deploy-remove-applications.md) artikel voor meer informatie over het implementeren van toepassingen op de Image Store-service.

**Failover Manager service**: elke Service Fabric-cluster heeft een Failover Manager-service die verantwoordelijk is voor de volgende acties:
   - Voert de functies voor hoge beschikbaarheid en consistentie van services.
   - Toepassings- en cluster upgrades ingedeeld.
   - Communiceert met andere onderdelen van het systeem.

## <a name="built-in-programming-models"></a>Ingebouwde programmeermodellen
.NET Framework-programmeermodellen zijn beschikbaar voor u het Service Fabric-services bouwen:

**Reliable Services**: een API staatloze en stateful services bouwen. Stateful services voor het opslaan van hun status in betrouwbare verzamelingen, zoals een woordenboek of een wachtrij. U kunt ook verschillende communicatie-stacks, zoals Web-API en Windows Communication Foundation (WCF) aansluiten.

**Reliable Actors**: een API voor het bouwen van staatloze en stateful-objecten via het virtuele Actor-programmeermodel. Dit model is nuttig wanneer er veel onafhankelijke eenheden van de berekening of status. Dit model maakt gebruik van een Schakel gebaseerde threadmodel, is het verstandig om te voorkomen dat code die naar andere actoren of services illustreert omdat andere binnenkomende aanvragen door een afzonderlijke actor kan niet worden verwerkt totdat alle uitgaande aanvragen zijn voltooid.

Lees de [kiezen een programmeermodel voor uw service](service-fabric-choose-framework.md) artikel voor meer informatie.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over Service Fabric:

* [Overzicht van Service Fabric](service-fabric-overview.md)
* [Waarom een microservices-benadering voor het ontwikkelen van toepassingen?](service-fabric-overview-microservices.md)
* [Toepassingsscenario's](service-fabric-application-scenarios.md)


