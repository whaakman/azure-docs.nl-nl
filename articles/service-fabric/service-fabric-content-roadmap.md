---
title: Meer informatie over Azure Service Fabric | Microsoft Docs
description: Meer informatie over de belangrijkste concepten en belangrijke gebieden van Azure Service Fabric. Biedt een uitgebreid overzicht van Service Fabric en microservices maken.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/08/2017
ms.author: ryanwi
ms.openlocfilehash: edb1638f4568656761f923c73ba8cdc0dd968d96
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296479"
---
# <a name="so-you-want-to-learn-about-service-fabric"></a>Zo wilt u meer informatie over Service Fabric?
Azure Service Fabric is een gedistribueerde systemen platform waarmee u gemakkelijk pakket, implementeren en beheren van schaalbare en betrouwbare microservices.  Service Fabric is een grote-gebied, en er is veel te leren.  In dit artikel biedt een overzicht van Service Fabric en beschrijft de belangrijkste concepten, programmeermodellen, levensduur van toepassingen, testen, clusters en statuscontrole. Lees de [overzicht](service-fabric-overview.md) en [wat zijn microservices?](service-fabric-overview-microservices.md) voor een inleiding en hoe Service Fabric kan worden gebruikt om te maken van microservices. In dit artikel bevat een uitgebreide lijst met inhoud, maar koppelen aan het overzicht en artikelen aan de slag voor elk gebied van Service Fabric. 

## <a name="core-concepts"></a>Basisconcepten
[Service Fabric-terminologie](service-fabric-technical-overview.md), [toepassingsmodel](service-fabric-application-model.md), en [ondersteunde programmeermodellen](service-fabric-choose-framework.md) bieden meer concepten en beschrijvingen, maar hier zijn de basisprincipes.

<table><tr><th>Basisconcepten</th><th>Ontwerptijd</th><th>Tijdens het gebruik</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/CoreConceptsVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965"><img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">
<img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td></tr>
</table>

### <a name="design-time-application-type-service-type-application-package-and-manifest-service-package-and-manifest"></a>Ontwerptijd: toepassingstype, servicetype, toepassingspakket en manifest, service-pakket en het manifest
Een toepassingstype is de naam/versie toegewezen aan een verzameling van servicetypen. Dit is gedefinieerd in een *ApplicationManifest.xml* -bestand, dat is ingesloten in een pakket toepassingsmap. Het toepassingspakket wordt vervolgens gekopieerd naar de installatiekopieopslag van het Service Fabric-cluster. U kunt vervolgens een toepassing met de naam van dit toepassingstype, die vervolgens wordt uitgevoerd binnen het cluster maken. 

Een servicetype is de naam/versie toegewezen aan de pakketten, gegevenspakketten en van configuratiepakketten van een service. Dit is gedefinieerd in een ServiceManifest.xml-bestand, dat is ingesloten in de map van een service-pakket. De map van de service-pakket vervolgens naar wordt verwezen door een toepassingspakket *ApplicationManifest.xml* bestand. Binnen het cluster kunt na het maken van een toepassing met de naam, u een benoemde service van een van het toepassingstype servicetypen. Een servicetype wordt beschreven door de *ServiceManifest.xml* bestand. Type van de service bestaat uit uitvoerbare code service configuratie-instellingen, die tijdens runtime worden geladen, en statische gegevens die wordt gebruikt door de service.

![Service Fabric-toepassingstypen en servicetypen][cluster-imagestore-apptypes]

Het toepassingspakket is een map met het toepassingstype *ApplicationManifest.xml* bestand, die verwijst naar de servicepakketten voor elk servicetype dat het toepassingstype. Een toepassingspakket voor een e-mailtype toepassing kan bijvoorbeeld bevatten verwijzingen naar een servicepakket wachtrij, een front-end-service-pakket en een database-service-pakket. De bestanden in de toepassingsmap pakket worden gekopieerd naar de installatiekopieopslag van het Service Fabric-cluster. 

Een servicepakket is een map met het servicetype *ServiceManifest.xml* bestand, die verwijst naar de code, statische gegevens en configuratiepakketten voor het type van de service. De bestanden in de map van de service-pakket wordt verwezen door het toepassingstype *ApplicationManifest.xml* bestand. Bijvoorbeeld, kan een servicepakket verwijzen naar de code, statische gegevens en configuratiepakketten die gezamenlijk een databaseservice.

### <a name="run-time-clusters-and-nodes-named-applications-named-services-partitions-and-replicas"></a>Uitvoeringstijd: clusters en knooppunten, met de naam van toepassingen, services, partities en replica's met de naam
Een [Service Fabric-cluster](service-fabric-deploy-anywhere.md) is een met het netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Clusters kunnen naar duizenden machines worden geschaald.

Een machine of VM die onderdeel uitmaakt van een cluster, heet een knooppunt. Aan elk knooppunt wordt een knooppuntnaam toegewezen (een tekenreeks). Knooppunten hebben kenmerken zoals plaatsingseigenschappen. Elke computer of virtuele machine heeft een Windows-service die automatisch wordt gestart, `FabricHost.exe`, die wordt uitgevoerd bij het opstarten en start vervolgens twee uitvoerbare bestanden: `Fabric.exe` en `FabricGateway.exe`. Deze twee uitvoerbare bestanden zijn vormen van het knooppunt. Voor ontwikkeling of Testscenario's, kunt u meerdere knooppunten op een enkele machine of virtuele machine host door het uitvoeren van meerdere exemplaren van `Fabric.exe` en `FabricGateway.exe`.

Een toepassing met de naam is een verzameling van services met de naam die een bepaalde functie of functies uitvoert. Een service vervult een functie volledig en zelfstandig (kunt starten en onafhankelijk van andere services worden uitgevoerd) en bestaat uit code, configuratie en gegevens. Nadat een toepassingspakket is gekopieerd naar de installatiekopieopslag, kunt u een exemplaar van de toepassing binnen het cluster maken door op te geven van het toepassingspakket toepassingstype (met de naam/versie). Elk type-exemplaar is de naam van een URI die als eruitziet toegewezen *fabric: / MyNamedApp*. Binnen een cluster, kunt u meerdere benoemde toepassingen kunt maken van een type één toepassing. U kunt ook met de naam toepassingen van verschillende toepassingstypen maken. Elke toepassing met de naam is onafhankelijk beheerd en is samengesteld.

Na het maken van een toepassing met de naam, kunt u een exemplaar van een van de servicetypen (een benoemde service) binnen het cluster maken door op te geven van het type van de service (met de naam/versie). Elk type service-exemplaar is de naam van een URI die binnen het bereik van onder de naam van de toepassing URI toegewezen. Bijvoorbeeld, als u een service in een toepassing met de naam 'MyNamedApp' met de naam 'MijnDatabase' maakt, de URI ziet eruit als: *fabric: / MyNamedApp/MijnDatabase*. In een toepassing met de naam, kunt u een of meer benoemde services maken. Elke benoemde service kan een eigen partitieschema en exemplaren/replica wordt geteld. 

Er zijn twee soorten services: staatloze en stateful. Stateless services opslaan in de service de status niet. Stateless services hebt geen permanente opslag in alle of permanente status opslaan in een service voor externe opslag, zoals Azure Storage, Azure SQL Database of Azure Cosmos DB. Een stateful service slaat de status in de service en maakt gebruik van Reliable Collections of Reliable Actors programmeermodellen voor het beheren van status. 

Bij het maken van een benoemde service, kunt u een partitieschema opgeven. Services met grote hoeveelheden status splitsen van de gegevens over meerdere partities. Elke partitie is verantwoordelijk voor een gedeelte van de volledige status van de service, die is verdeeld over knooppunten van het cluster.  

Het volgende diagram toont de relatie tussen toepassingen en service-exemplaren, partities en replica's.

![Partities en replica's in een service][cluster-application-instances]

### <a name="partitioning-scaling-and-availability"></a>Partitionering, schaalbaarheid en beschikbaarheid
[Partitioneren](service-fabric-concepts-partitioning.md) is niet uniek in Service Fabric. Een bekende vorm van partitioneren is het partitioneren van gegevens, of sharding. Stateful services met grote hoeveelheden status splitsen van de gegevens over meerdere partities. Elke partitie is verantwoordelijk voor een gedeelte van de volledige status van de service. 

De replica's van elke partitie worden verdeeld over de clusterknooppunten, waardoor uw benoemde service staat [schaal](service-fabric-concepts-scalability.md). Als de gegevens groeien behoeften, partities groeien en Service Fabric rebalances partities over knooppunten voor efficiënt gebruik van hardwarebronnen. Als u nieuwe knooppunten aan het cluster toevoegen, Service Fabric wordt de partitie partitiereplica's opnieuw indelen in het grotere aantal knooppunten. Algemene verbetert de prestaties van toepassingen en conflicten voor toegang tot het geheugen vermindert. Als de knooppunten in het cluster niet efficiënt worden gebruikt, kunt u het aantal knooppunten in het cluster verminderen. Service Fabric rebalances de partitiereplica's opnieuw via het kleinere aantal knooppunten om beter gebruik van de hardware op elk knooppunt.

Binnen een partitie hebben stateless services voor benoemde exemplaren terwijl stateful benoemde services replica's hebben. Normaal gesproken hebben stateless benoemde services slechts één partitie omdat ze geen interne status hebben. De partitie-instanties bieden voor [beschikbaarheid](service-fabric-availability-services.md). Als één instantie is mislukt, doorgaan met andere exemplaren normaal functioneren en Service Fabric maakt vervolgens een nieuw exemplaar. Stateful services met de naam behouden hun status binnen de replica's en elke partitie heeft een eigen replicaset. Bewerkingen voor lezen en schrijven worden uitgevoerd op een van de replica (de primaire genoemd). Wijzigingen in de status van schrijfbewerkingen bewerkingen zijn gerepliceerd naar meerdere andere replica's (actieve secundaire databases genoemd). Een replica mislukt, bouwt Service fabrics een nieuwe replica van de bestaande replica's.

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Staatloze en stateful microservices voor Service Fabric
Met Service Fabric kunt u toepassingen bouwen die uit microservices of containers bestaan. Staatloze microservices (zoals protocolgateways en webproxy's) handhaven buiten een aanvraag en de reactie erop van de service geen veranderlijke status. Azure Cloud Services-werkrollen zijn een voorbeeld van een staatloze service. Stateful microservices (zoals gebruikersaccounts, databases, apparaten, winkelwagentjes en wachtrijen) handhaven een veranderlijke, gezaghebbende status, buiten de aanvraag en de reactie erop. De huidige toepassingen op internetschaal bestaan uit een combinatie van staatloze en stateful microservices. 

Een sleutel differentation met Service Fabric is de sterke focus op het bouwen van stateful services, met de [ingebouwde programmeermodellen ](service-fabric-choose-framework.md) of met stateful services in containers. De [toepassingsscenario's](service-fabric-application-scenarios.md) beschrijven de scenario's waarin stateful services worden gebruikt.

Waarom zijn stateful microservices, samen met de stateless die? De twee belangrijkste redenen zijn:

* U kunt hoge doorvoer, lage latentie, fout fouttolerante online verwerking van transacties (OLTP)-services door code en gegevens dichtbij houden op dezelfde computer kunt bouwen. Enkele voorbeelden zijn interactieve storefronts, zoeken, Internet of Things (IoT)-systemen, handelssystemen, creditcard verwerking en fraude systemen voor het detecteren en persoonlijke beheren.
* U kunt het ontwerp van toepassing vereenvoudigen. Stateful microservices noodzaak de voor extra wachtrijen en -caches, die doorgaans nodig zijn om de vereisten voor beschikbaarheid en latentie van een toepassing met puur stateless. Stateful services zijn op natuurlijke wijze hoge beschikbaarheid en lage latentie, waardoor het aantal bewegende onderdelen om te beheren in uw toepassing als geheel.

## <a name="supported-programming-models"></a>Ondersteunde programmeermodellen
Service Fabric biedt verschillende manieren om te schrijven en beheren van uw services. Services kunnen u de Service Fabric-API's gebruiken om te profiteren van de functies van het platform en de App-frameworks. Services kunnen ook worden alle gecompileerde uitvoerbaar programma geschreven in elke taal en die worden gehost op een Service Fabric-cluster. Zie voor meer informatie, [ondersteunde programmeermodellen](service-fabric-choose-framework.md).

### <a name="containers"></a>Containers
Standaard wordt Service Fabric implementeert en services als processen wordt geactiveerd. Service Fabric kunnen ook implementeren van services in [containers](service-fabric-containers-overview.md). Bovendien kunt u services in processen en services in containers binnen dezelfde toepassing combineren. Service Fabric biedt ondersteuning voor implementatie van Linux-containers en Windows-containers op Windows Server 2016. U kunt bestaande toepassingen, stateless services of stateful services in containers kunt implementeren. 

### <a name="reliable-services"></a>Reliable Services
[Reliable Services](service-fabric-reliable-services-introduction.md) is een lichtgewicht framework voor het schrijven van services die integreren met het Service Fabric-platform en profiteren van de volledige set functies van het platform. Betrouwbare Services kunnen worden stateless (vergelijkbaar met de meeste service-platformen, zoals webservers of werkrollen in Azure Cloud Services), waarbij status is opgeslagen in een externe oplossing, zoals Azure DB of Azure-tabelopslag. Betrouwbare Services kunnen ook stateful, worden waar staat rechtstreeks in de service zelf met behulp van betrouwbare verzamelingen worden bewaard. Staat wordt gesteld [maximaal beschikbare](service-fabric-availability-services.md) door middel van replicatie en gedistribueerde via [partitioneren](service-fabric-concepts-partitioning.md), alle beheerde automatisch door Service Fabric.

### <a name="reliable-actors"></a>Reliable Actors
Gebaseerd op Reliable Services de [Reliable Actor](service-fabric-reliable-actors-introduction.md) framework is een toepassingsframework dat het virtuele Actor-patroon, op basis van het ontwerppatroon actor implementeert. Onafhankelijke eenheden van reken- en staat het Reliable Actor-framework gebruikt met één thread worden uitgevoerd met de naam actoren. De Reliable Actor framework is ingebouwd in de communicatie voor actors en vooraf ingesteld status persistentie en scale-out-configuraties.

### <a name="aspnet-core"></a>ASP.NET Core
Service Fabric kan worden geïntegreerd met [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) als een eersteklas programmeermodel voor het bouwen van web-apps en API-Apps.  ASP.NET Core kan op twee verschillende manieren in Service Fabric worden gebruikt:

- Als een Gast kan worden uitgevoerd wordt gehost. Dit wordt hoofdzakelijk gebruikt bestaande ASP.NET Core-toepassingen uitvoeren in Service Fabric met zonder code te wijzigen.
- Binnen een betrouwbare Service wordt uitgevoerd. Dit zorgt voor een betere integratie met de Service Fabric-runtime en stateful ASP.NET Core, kunnen services.

### <a name="guest-executables"></a>Uitvoerbare gastbestanden
Een [Gast kan worden uitgevoerd](service-fabric-guest-executables-introduction.md) is een bestaande, willekeurige uitvoerbaar bestand (geschreven in elke taal) die worden gehost op een Service Fabric-cluster samen met andere services. Gastbestanden integreren niet rechtstreeks met Service Fabric-API's. Maar ze nog steeds profiteren van het platform, zoals de gezondheid van aangepaste biedt functies laden reporting en zichtbaarheid door het aanroepen van REST-API's-service. Ze hebben ook de volledige levenscyclus van ondersteuning. 

## <a name="application-lifecycle"></a>Toepassingslevenscyclus
Wanneer een toepassing in Service Fabric met andere platforms, meestal niet via de volgende fasen: ontwerpen, ontwikkelen, testen, implementatie, upgrade, onderhoud en verwijderen. Service Fabric biedt uitstekende ondersteuning voor de volledige levenscyclus van cloudtoepassingen, van ontwikkeling tot implementatie, dagelijkse beheer en onderhoud en uiteindelijk buiten gebruik stellen. Het service-model kunt meerdere verschillende rollen om deel te nemen onafhankelijk van elkaar in de levensduur van toepassingen. [Service Fabric-toepassingslevenscyclus](service-fabric-application-lifecycle.md) biedt een overzicht van de API's en hoe ze worden gebruikt door de verschillende rollen in de fasen van de levensduur van de Service Fabric-toepassingen. 

De hele app-levenscyclus kan worden beheerd met behulp van [PowerShell-cmdlets](/powershell/module/ServiceFabric/), [CLI-opdrachten](service-fabric-sfctl.md), [C#-API's](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [Java-API's](/java/api/system.fabric._application_management_client), en [ REST-API's](/rest/api/servicefabric/). U kunt ook instellen van continue integratie/continue implementatie pijplijnen met behulp van hulpprogramma's zoals [Azure pijplijnen](service-fabric-set-up-continuous-integration.md) of [Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md).

De volgende video van Microsoft Virtual Academy wordt beschreven hoe u de levensduur van uw toepassingen beheren: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=My3Ka56yC_6106218965">
<img src="./media/service-fabric-content-roadmap/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="test-applications-and-services"></a>Testtoepassingen en -services
Als u wilt maken echt schaalbare services, is het essentieel om te controleren of dat uw toepassingen en services echte fouten kunnen beschikken. De Fault Analysis Service is ontworpen voor het testen van services die zijn ingebouwd in Service Fabric. Met de [Fault Analysis Service](service-fabric-testability-overview.md), kunt u zinvolle fouten veroorzaken en volledige Testscenario's uitvoeren in uw toepassingen. Deze fouten en scenario's maken en valideren van de talloze Staten en overgangen die een service tijdens de levensduur, allemaal in een gecontroleerde, veilige en consistente manier optreden zal.

[Acties](service-fabric-testability-actions.md) gericht op een service voor het testen met behulp van de afzonderlijke fouten. Een ontwikkelaar van de service kunt gebruiken als bouwstenen schrijven van complexe scenario's. Voorbeelden van gesimuleerde fouten zijn:

* Start opnieuw op een knooppunt om te simuleren van een willekeurig aantal situaties waarin een machine of virtuele machine opnieuw wordt opgestart.
* Verplaatsen van een replica van de stateful service voor het simuleren van taakverdeling, failover of upgrade van de toepassing.
* Quorumverlies in een stateful service voor het maken van een situatie waarbij schrijfbewerkingen kunnen niet doorgaan omdat er niet voldoende 'back-up' of 'secundair' replica's te accepteren van nieuwe gegevens worden aangeroepen.
* Roep verlies van gegevens op een stateful service voor het maken van een situatie waarbij alle in het geheugen status volledig zijn gewist.

[Scenario's](service-fabric-testability-scenarios.md) complexe bewerkingen bestaan uit een of meer acties. De Fault Analysis Service biedt twee ingebouwde voltooid scenario's:

* [Chaos-scenario](service-fabric-controlled-chaos.md)-simuleert continue, interleaved fouten (vensters en geforceerde afsluiting) in het cluster gedurende langere tijd.
* [Failover-scenario](service-fabric-testability-scenarios.md#failover-test)-een versie van het scenario voor het testen van chaos die gericht is op een specifieke service-partitie terwijl andere services niet beïnvloed.

## <a name="clusters"></a>Clusters
Een [Service Fabric-cluster](service-fabric-deploy-anywhere.md) is een met het netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Clusters kunnen naar duizenden machines worden geschaald. Een machine of virtuele machine die deel uitmaakt van een cluster wordt een clusterknooppunt genoemd. Aan elk knooppunt wordt een knooppuntnaam toegewezen (een tekenreeks). Knooppunten hebben kenmerken zoals plaatsingseigenschappen. Elke computer of virtuele machine is een service automatisch wordt gestart, `FabricHost.exe`, die wordt uitgevoerd bij het opstarten en start vervolgens twee uitvoerbare bestanden: Fabric.exe en FabricGateway.exe. Deze twee uitvoerbare bestanden zijn vormen van het knooppunt. Voor het testen van scenario's, kunt u meerdere knooppunten op een enkele machine of virtuele machine host door het uitvoeren van meerdere exemplaren van `Fabric.exe` en `FabricGateway.exe`.

Service Fabric-clusters kunnen worden gemaakt op virtuele of fysieke machines waarop Windows Server of Linux wordt uitgevoerd. Om te implementeren en uitvoeren van Service Fabric-toepassingen in een omgeving waarin u een set van Windows Server of Linux-computers onderling hebben: on-premises, op Microsoft Azure, of op elke andere cloudprovider.

De video volgende Microsoft Virtual Academy beschrijving van Service Fabric-clusters: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/ClusterOverview.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="clusters-on-azure"></a>Clusters op Azure
Service Fabric-clusters worden uitgevoerd op Azure biedt integratie met andere Azure-functies en services, waardoor bewerkingen en het beheer van het cluster, eenvoudiger en betrouwbaarder maakt. Een cluster is een Azure Resource Manager-resource, dus u kunt een model van clusters, zoals alle andere resources in Azure. Resource Manager biedt ook eenvoudig beheer van alle resources die worden gebruikt door het cluster als één eenheid. Clusters in Azure zijn geïntegreerd met Azure diagnostics en Log Analytics. Knooppunt clustertypen [virtuele-machineschaalsets](/azure/virtual-machine-scale-sets/index), zodat de functionaliteit voor automatisch schalen is ingebouwd in.

U kunt een cluster maken op Azure via de [Azure-portal](service-fabric-cluster-creation-via-portal.md), van een [sjabloon](service-fabric-cluster-creation-via-arm.md), of van [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md).

Service Fabric in Linux kunt bouwen, implementeren en beheren van maximaal beschikbare, zeer schaalbare toepassingen op Linux, net zoals u zou in Windows doen. De Service Fabric-frameworks (Reliable Services en Reliable Actors) zijn beschikbaar in Java op Linux, naast de C# (.NET Core). U kunt ook bouwen [uitvoerbare gastservices](service-fabric-guest-executables-introduction.md) met elke taal of framework. Docker-containers indelen wordt ook ondersteund. Docker-containers kunnen gasten uitvoerbare bestanden of native Service Fabric-services, waarmee de Service Fabric-frameworks uitgevoerd. Lees voor meer informatie over [Service Fabric in Linux](service-fabric-deploy-anywhere.md).

Er zijn enkele functies die worden ondersteund op Windows, maar niet op Linux. Voor meer informatie lezen [verschillen tussen Service Fabric in Linux en Windows](service-fabric-linux-windows-differences.md).

### <a name="standalone-clusters"></a>Zelfstandige clusters
Service Fabric biedt een installatiepakket voor u maken van zelfstandige Service Fabric-clusters on-premises of op elke andere cloudprovider. Zelfstandige clusters bieden u de vrijheid voor het hosten van een cluster waar u maar wilt. Als uw gegevens onderworpen aan het nalevings- of regelgeving beperkingen is of u wilt uw gegevens lokaal te houden, kunt u uw eigen cluster en de toepassingen hosten. Service Fabric-toepassingen kunnen uitvoeren in meerdere hostingomgevingen zonder wijzigingen, zodat uw kennis van het bouwen van toepassingen wordt doorgevoerd in van de ene host-omgeving naar een andere. 

[Uw eerste zelfstandige Service Fabric-cluster maken](service-fabric-cluster-creation-for-windows-server.md)

Linux zelfstandige clusters worden nog niet ondersteund.

### <a name="cluster-security"></a>Clusterbeveiliging
Clusters moeten worden beveiligd om te voorkomen dat onbevoegde gebruikers verbinding maken met uw cluster, met name wanneer er productieworkloads die erop worden uitgevoerd. Hoewel het mogelijk te maken van een niet-beveiligd cluster, kan hierdoor anonieme gebruikers verbinding maken met het als-beheereindpunten worden blootgesteld aan het openbare internet. Het is niet mogelijk om in te schakelen later beveiliging op een niet-beveiligd cluster: clusterbeveiliging is ingeschakeld tijdens het maken van een cluster.

De clusterbeveiligingsscenario's zijn:
* Beveiliging van knooppunt-naar-knooppunt
* Beveiliging van de client-naar-knooppunt
* Toegangsbeheer op basis van rollen (RBAC)

Lees voor meer informatie, [een cluster beveiligen](service-fabric-cluster-security.md).

### <a name="scaling"></a>Schaal aanpassen
Als u nieuwe knooppunten aan het cluster toevoegt, rebalances Service Fabric de partitiereplica's en -exemplaren in het grotere aantal knooppunten. Algemene verbetert de prestaties van toepassingen en conflicten voor toegang tot het geheugen vermindert. Als de knooppunten in het cluster niet efficiënt worden gebruikt, kunt u het aantal knooppunten in het cluster verminderen. Service Fabric rebalances opnieuw de partitiereplica's en exemplaren voor het kleinere aantal knooppunten om beter gebruik van de hardware op elk knooppunt. U kunt clusters op Azure ofwel schalen [handmatig](service-fabric-cluster-scale-up-down.md) of [programmatisch](service-fabric-cluster-programmatic-scaling.md). Zelfstandige clusters kunnen worden geschaald [handmatig](service-fabric-cluster-windows-server-add-remove-nodes.md).

### <a name="cluster-upgrades"></a>Upgraden van clusters
Periodiek worden nieuwe versies van de Service Fabric-runtime worden gepubliceerd. Runtime of fabric, upgrades uitvoeren op uw cluster, zodat u altijd wordt uitgevoerd een [ondersteunde versie](service-fabric-support.md). Naast het fabric-upgrades, kunt u ook clusterconfiguratie, zoals certificaten of toepassingspoorten bijwerken.

Een Service Fabric-cluster is een resource die u bezit, maar gedeeltelijk wordt beheerd door Microsoft. Microsoft is verantwoordelijk voor het patchen van het onderliggende besturingssysteem en het uitvoeren van fabric-upgrades op het cluster. U kunt instellen van uw cluster automatische infrastructuurupgrades, ontvangen wanneer een nieuwe versie door Microsoft worden uitgegeven of kies een ondersteunde fabric-versie die u wilt. Upgrades van infrastructuur en configuratie kunnen worden ingesteld via de Azure-portal of via Resource Manager. Lees voor meer informatie, [upgraden van een Service Fabric-cluster](service-fabric-cluster-upgrade.md). 

Een zelfstandige cluster is een resource die u helemaal zelf. U bent verantwoordelijk voor het patchen van het onderliggende besturingssysteem en het initialiseren van de fabric-upgrades. Als uw cluster verbinding met maken kunt [ https://www.microsoft.com/download ](https://www.microsoft.com/download), kunt u uw cluster te downloaden en het nieuwe pakket van de Service Fabric-runtime inrichten instellen. U wordt vervolgens de upgrade start. Als u geen toegang tot uw cluster [ https://www.microsoft.com/download ](https://www.microsoft.com/download), kunt u handmatig de nieuwe runtime-pakket downloaden van een internet verbonden virtuele machine en vervolgens de upgrade initiëren. Lees voor meer informatie, [een zelfstandige Service Fabric-cluster upgraden](service-fabric-cluster-upgrade-windows-server.md).

## <a name="health-monitoring"></a>Statuscontrole
Service Fabric introduceert een [statusmodel](service-fabric-health-introduction.md) ontworpen om u te markeren niet in orde cluster en de voorwaarden van toepassing op specifieke entiteiten (zoals clusterknooppunten en service-replica's). Het statusmodel maakt gebruik van health-rapporteurs (onderdelen van het systeem en watchdogs). Het doel is eenvoudig en snel diagnose en herstel. Schrijvers van de service nodig hebt om na te denken over de status van vooraf en hoe u [ontwerpen health reporting](service-fabric-report-health.md#design-health-reporting). Een voorwaarde die van invloed kan status moet worden gerapporteerd, met name als het de problemen met de vlag dicht bij de hoofdmap helpen kan. De health-gegevens kunt Bespaar tijd en moeite voor foutopsporing en onderzoek zodra de service actief en werkend is op schaal in productie.

De Service Fabric-rapporteurs monitor geïdentificeerd voorwaarden van belang zijn. Ze een rapport over deze voorwaarden op basis van hun lokale weergave. De [health store](service-fabric-health-introduction.md#health-store) health-gegevens die door alle rapporteurs wordt verzonden om te bepalen of entiteiten wereldwijd in orde zijn. Het model is bedoeld als uitgebreide, flexibele en eenvoudig te gebruiken. De kwaliteit van de statusrapporten bepaalt de nauwkeurigheid van de weergave van de status van het cluster. Fout-positieven waarin ten onrechte niet in orde problemen kunnen negatieve invloed hebben op upgrades of andere services die gebruikmaken van health-gegevens. Voorbeelden van dergelijke services zijn reparatie en waarschuwingsmechanisme. Daarom zetten is dat nodig is voor rapporten waarin de voorwaarden van belang zijn in de beste manier worden vastgelegd.

Rapportage kan worden gedaan in:
* De bewaakte Service Fabric service-replica of het exemplaar.
* Interne watchdogs geïmplementeerd als een Service Fabric-service (bijvoorbeeld, een Service Fabric stateless service waarmee voorwaarden worden gecontroleerd en problemen met rapporten). De watchdogs kunnen worden geïmplementeerd op alle knooppunten of kunnen worden wachtrijen op de bewaakte service.
* Interne watchdogs die worden uitgevoerd op de Service Fabric-knooppunten, maar niet zijn geïmplementeerd als een Service Fabric-services.
* Externe watchdogs die de resource van buiten het Service Fabric-cluster (bijvoorbeeld monitoring-service, zoals Gomez)-test.

Buiten het vak een health Service Fabric-onderdelen rapport over alle entiteiten in het cluster. [Systeemstatusrapporten](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) bieden inzicht in het cluster en de toepassing functionaliteit en de vlag problemen via health. Voor toepassingen en services controleren systeemstatusrapporten of entiteiten worden geïmplementeerd en correct vanuit het perspectief van de Service Fabric-runtime gedragen zich. De rapporten niet bieden een statuscontrole van de bedrijfslogica van de service of vastgelopen processen te detecteren. Specifieke gezondheidsinformatie toevoegen aan de logica van uw service, [implementeren aangepaste health reporting](service-fabric-report-health.md) in uw services.

Service Fabric biedt verschillende manieren voor het [statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md) samengevoegd in de health store:
* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) of andere hulpprogramma's voor visualisatie.
* Statusquery's (via [PowerShell](/powershell/module/ServiceFabric/), [CLI](service-fabric-sfctl.md), wordt de [C# FabricClient APIs](/dotnet/api/system.fabric.fabricclient.healthclient) en [Java FabricClient APIs](/java/api/system.fabric), of [REST API's](/rest/api/servicefabric)).
* Algemene query's dat retourneren een lijst van entiteiten die status als een van de eigenschappen (via PowerShell, CLI, de API's of REST hebben).

De volgende video van Microsoft Virtual Academy beschrijft het statusmodel van de Service Fabric en hoe deze worden gebruikt: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-content-roadmap/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="monitoring-and-diagnostics"></a>Controle en diagnose
[Controle en diagnose](service-fabric-diagnostics-overview.md) essentieel zijn voor het ontwikkelen, testen en implementeren van toepassingen en services in elke omgeving. Service Fabric-oplossingen werken het beste wanneer u plannen en implementeren van bewaking en diagnostische gegevens waarmee Zorg ervoor toepassingen dat en services werken zoals verwacht in een lokale ontwikkelingsomgeving of in de productieomgeving.

De belangrijkste doelstellingen van controle en diagnostische gegevens zijn:

- Hardware- en infrastructuurbeheer problemen detecteren en onderzoeken
- Software- en app problemen detecteren, service-uitvaltijd verlagen
- Resource-verbruik en help station operations beslissingen begrijpen
- Prestaties van de toepassing, service en -infrastructuur
- Genereren van zakelijke inzichten en verbeterpunten identificeren
 
De algemene werkstroom van bewaking en diagnostiek bestaat uit drie stappen:

1. Gebeurtenisgeneratie: dit omvat gebeurtenissen (Logboeken, traceringen, aangepaste gebeurtenissen) op de infrastructuur (cluster), platform en toepassing / service-niveau
2. Aggregatie van gebeurtenis: gegenereerde gebeurtenissen moeten worden verzameld en samengevoegd voordat ze kunnen worden weergegeven
3. Analyse: gebeurtenissen moeten worden zinvoller visualiseren en toegankelijk is in een indeling, toestaan voor analyse en zo nodig weergeven

Meerdere producten zijn beschikbaar die uit de volgende drie gebieden, en u verschillende technologieën voor elk kiezen. Lees voor meer informatie, [bewaking en diagnose voor Azure Service Fabric](service-fabric-diagnostics-overview.md).

## <a name="next-steps"></a>Volgende stappen
* Leer hoe u een [cluster maakt in Azure](service-fabric-cluster-creation-via-portal.md) of hoe u een [zelfstandig cluster maakt in Windows](service-fabric-cluster-creation-for-windows-server.md).
* Maak een service met het programmeermodel [Reliable Services](service-fabric-reliable-services-quick-start.md) of [Reliable Actors](service-fabric-reliable-actors-get-started.md).
* Meer informatie over het [migreren uit Cloud Services](service-fabric-cloud-services-migration-differences.md).
* Meer informatie over het [bewaken en diagnoses uitvoeren services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). 
* Meer informatie over het [test uw apps en services](service-fabric-testability-overview.md).
* Meer informatie over het [beheren en te organiseren van clusterbronnen](service-fabric-cluster-resource-manager-introduction.md).
* Bekijk de [voorbeelden van Service Fabric](http://aka.ms/servicefabricsamples).
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md).
* Lees de [teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/) voor artikelen en aankondigingen.


[cluster-application-instances]: media/service-fabric-content-roadmap/cluster-application-instances.png
[cluster-imagestore-apptypes]: ./media/service-fabric-content-roadmap/cluster-imagestore-apptypes.png
