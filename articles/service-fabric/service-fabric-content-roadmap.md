---
title: Meer informatie over Azure Service Fabric | Microsoft Docs
description: Meer informatie over de belangrijkste concepten en belangrijke gebieden van Azure Service Fabric. Biedt een uitgebreid overzicht van Service Fabric en microservices maken.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/08/2017
ms.author: ryanwi
ms.openlocfilehash: 9360d29eb30171651b0bcc688fe7884614b50cf4
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="so-you-want-to-learn-about-service-fabric"></a>Daarom wilt u meer informatie over Service Fabric?
Azure Service Fabric is een gedistribueerde systemen platform waarmee u gemakkelijk pakket, implementeren en beheren van schaalbare en betrouwbare microservices.  Service Fabric is een grote oppervlak echter en is er veel voor meer informatie.  In dit artikel biedt een overzicht van Service Fabric en beschrijft de belangrijkste concepten, programmeren van de levenscyclus van de toepassing, testen, clusters en statuscontrole-modellen. Lees de [overzicht](service-fabric-overview.md) en [wat zijn microservices?](service-fabric-overview-microservices.md) voor een inleiding en hoe Service Fabric kunnen worden gebruikt voor het maken van microservices. In dit artikel bevat een uitgebreide lijst van de inhoud niet, maar koppelen aan het overzicht en ophalen van gestarte artikelen voor elk gebied van Service Fabric. 

## <a name="core-concepts"></a>Basisconcepten
[Service Fabric-terminologie](service-fabric-technical-overview.md), [toepassingsmodel](service-fabric-application-model.md), en [ondersteund programmeermodellen](service-fabric-choose-framework.md) bieden meer concepten en beschrijvingen, maar dit zijn de basisbeginselen.

<table><tr><th>Basisconcepten</th><th>Ontwerptijd</th><th>Tijdens het gebruik</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/CoreConceptsVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965"><img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">
<img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td></tr>
</table>

### <a name="design-time-application-type-service-type-application-package-and-manifest-service-package-and-manifest"></a>Ontwerptijd: toepassingstype, servicetype, toepassingspakket en manifest, servicepakket en manifest
Een toepassingstype is de naam of de versie die is toegewezen aan een verzameling van servicetypen. Dit is gedefinieerd in een *ApplicationManifest.xml* bestand, dat is ingesloten in een pakket toepassingsmap. Het toepassingspakket wordt vervolgens gekopieerd naar het archief van de installatiekopie van het Service Fabric-cluster. Vervolgens kunt u een toepassing met de naam van dit toepassingstype, die vervolgens wordt uitgevoerd binnen het cluster maken. 

Een service is de naam of de versie die is toegewezen aan een service-code pakketten, gegevenspakketten en configuratiepakketten. Dit is gedefinieerd in een bestand ServiceManifest.xml, die is ingesloten in een pakket servicemap. De map van de service-pakket vervolgens naar wordt verwezen door een toepassingspakket *ApplicationManifest.xml* bestand. Binnen het cluster kunt na het maken van een toepassing met de naam, u een service met de naam van een van het toepassingstype servicetypen. Een servicetype is beschreven door de *ServiceManifest.xml* bestand. Type van de service bestaat uit uitvoerbare code service configuratie-instellingen, die worden geladen tijdens runtime, en statische gegevens dat wordt gebruikt door de service.

![Service Fabric-toepassing waardetypen en servicetypen][cluster-imagestore-apptypes]

Het toepassingspakket is een map met het toepassingstype *ApplicationManifest.xml* bestand, dat verwijst naar de servicepakketten voor elk servicetype dat het toepassingstype. Een toepassingspakket voor een e-mailtype toepassing kan bijvoorbeeld verwijzingen naar een wachtrij servicepakket, frontend servicepakket en servicepakket database bevatten. De bestanden in de toepassingsmap pakket gekopieerd naar het archief van de installatiekopie van het Service Fabric-cluster. 

Een servicepakket is een map met het servicetype *ServiceManifest.xml* bestand, dat verwijst naar de code, statische gegevens en configuratiepakketten voor servicetype. De bestanden in de map van de service-pakket wordt verwezen door het toepassingstype *ApplicationManifest.xml* bestand. Bijvoorbeeld, kan een servicepakket naar de code, statische gegevens en configuratiepakketten die gezamenlijk een database-service verwijzen.

### <a name="run-time-clusters-and-nodes-named-applications-named-services-partitions-and-replicas"></a>Uitvoeringstijd: clusters en knooppunten, toepassingen, services, partities en replica's met de naam met de naam
Een [Service Fabric-cluster](service-fabric-deploy-anywhere.md) is een met het netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Clusters kunnen worden geschaald naar duizenden computers.

Een machine of virtuele machine die deel uitmaakt van een cluster wordt een knooppunt genoemd. Elk knooppunt wordt de knooppuntnaam van een (een tekenreeks) toegewezen. Knooppunten hebben kenmerken zoals plaatsingseigenschappen. Elke computer of virtuele machine heeft een Windows-service van het automatisch starten, `FabricHost.exe`, die begint bij het opstarten worden uitgevoerd en start vervolgens twee uitvoerbare bestanden: `Fabric.exe` en `FabricGateway.exe`. Deze twee uitvoerbare bestanden, vormen het knooppunt. Voor ontwikkeling of Testscenario's, kunt u meerdere knooppunten op een enkele computer of virtuele machine host door het uitvoeren van meerdere exemplaren van `Fabric.exe` en `FabricGateway.exe`.

Een toepassing met de naam is een verzameling van benoemde services die een bepaalde functie of functies uitvoert. Een service vervult een functie volledig en zelfstandig (kunt starten en onafhankelijk van andere services worden uitgevoerd) en bestaat uit code, configuratie en gegevens. Nadat een toepassingspakket is gekopieerd naar de image store, maakt u een exemplaar van de toepassing binnen het cluster door te geven van het toepassingspakket toepassingstype (met de naam/versie). Elk type toepassingsexemplaar krijgt een URI-naam die lijkt op *fabric: / MyNamedApp*. U kunt meerdere benoemde toepassingen binnen een cluster maken van een type één toepassing. U kunt ook benoemde toepassingen maken van verschillende toepassingstypen. Elke toepassing met de naam is onafhankelijk van elkaar worden beheerd en samengestelde.

Na het maken van een benoemde toepassing, kunt u een exemplaar van een van de servicetypen (een benoemde service) binnen het cluster door te geven van het type van de service (met de naam/versie). Elk service type-exemplaar is de naam van een URI die onder de URI van de toepassing van het benoemde bereik toegewezen. Bijvoorbeeld, als u een service binnen een toepassing met de naam 'MyNamedApp' met de naam 'MijnDatabase' maakt, de URI eruit: *fabric: / MyNamedApp/MijnDatabase*. In een toepassing met de naam, kunt u een of meer benoemde services maken. Elke benoemde service kan de eigen partitieschema en exemplaar/replica telt. 

Er zijn twee soorten services: staatloze en stateful. Stateless services kunnen permanente status opslaan in een service voor externe opslag zoals Azure Storage, Azure SQL Database of Azure Cosmos DB. Gebruik een stateless service wanneer de service geen permanente opslag helemaal heeft. Service Fabric een stateful service gebruikt voor het beheren van de status van uw service via de betrouwbare verzamelingen of Reliable Actors programmeermodellen. 

Bij het maken van een benoemde service, kunt u een partitieschema opgeven. De gegevens verdelen Services met grote hoeveelheden status over partities. Elke partitie is verantwoordelijk voor een deel van de volledige status van de service, die is verdeeld over knooppunten van het cluster.  

Het volgende diagram toont de relatie tussen toepassingen en service-exemplaren, partities en replica's.

![Partities en replica's binnen een service][cluster-application-instances]

### <a name="partitioning-scaling-and-availability"></a>Partitioneren, schaalbaarheid en beschikbaarheid
[Partitioneren](service-fabric-concepts-partitioning.md) is niet uniek is voor Service Fabric. Een bekende vorm van partitioneren is partitioneren van gegevens of sharding. De gegevens verdelen stateful services met grote hoeveelheden status over partities. Elke partitie is verantwoordelijk voor een deel van de volledige status van de service. 

De replica's van elke partitie worden verdeeld over de clusterknooppunten, waardoor uw benoemde service de status [scale](service-fabric-concepts-scalability.md). Wanneer de gegevens groeien behoeften, groeien partities en Service Fabric rebalances partities over knooppunten efficiënt gebruikmaken van de hardwarebronnen. Als u nieuwe knooppunten aan het cluster toevoegt, opnieuw Service Fabric de partitie replica's verdelen over het toegenomen aantal knooppunten. De algemene verbetert de prestaties van toepassingen en verkleint u conflicten over toegang tot het geheugen. Als de knooppunten in het cluster niet efficiënt worden gebruikt, kunt u het aantal knooppunten in het cluster verminderen. Service Fabric rebalances opnieuw de partitie replica's via het kleinere aantal knooppunten beter te benutten van de hardware op elk knooppunt.

Binnen een partitie hebben staatloze benoemde services de exemplaren stateful benoemde services replica's. Normaal gesproken staatloze benoemde services ooit slechts één partitie hebben omdat ze geen interne status hebben. Geef op de exemplaren van de partitie voor [beschikbaarheid](service-fabric-availability-services.md). Als één exemplaar is mislukt, doorgaan met andere exemplaren normaal functioneren en Service Fabric maakt vervolgens een nieuw exemplaar. Stateful services met de naam behouden hun status binnen de replica's en elke partitie heeft een eigen replicaset. Lezen en schrijven bewerkingen worden uitgevoerd op één replica (de primaire genoemd). Wijzigingen in de status van schrijven bewerkingen worden gerepliceerd naar meerdere andere replica's (actieve secundaire replica's genoemd). Moet een replica mislukken, Service Fabric-bouwt voort op een nieuwe replica van de bestaande replica's.

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Staatloze en stateful microservices voor Service Fabric
Service Fabric kunt u toepassingen ontwikkelen die bestaan uit microservices of containers. Staatloze microservices (zoals protocol gateways en webproxy) gedurende niet een veranderlijke status buiten een aanvraag en de reactie van de service. Azure Cloud Services-werkrollen zijn een voorbeeld van een staatloze service. Stateful microservices (zoals gebruikersaccounts, databases, apparaten, on en wachtrijen) onderhouden de status van een veranderlijke, gezaghebbende afgezien van de aanvraag en het antwoord. De hedendaagse Internet-toepassingen bestaan uit een combinatie van staatloze en stateful microservices. 

Een sleutel differentation met Service Fabric wordt de nadruk op het ontwikkelen van stateful services, hetzij met de [gebouwd voor het programmeren van modellen ](service-fabric-choose-framework.md) of met beperkte stateful services. De [toepassingsscenario's](service-fabric-application-scenarios.md) beschrijven de scenario's waar stateful services worden gebruikt.

Waarom er stateful microservices samen met staatloze die zijn? De twee belangrijkste redenen zijn:

* U kunt opbouwen hoge doorvoer, lage latentie, fout fouttolerante online transactieverwerking (OLTP-systemen)-services door de code en sluit de gegevens op dezelfde machine. Voorbeelden zijn interactieve winkelobjecten, zoeken, Internet der dingen (IoT)-systemen, handelssystemen, creditcard verwerking en fraude detectie systemen en persoonlijke record management.
* Het ontwerp van toepassing, kunt u vereenvoudigen. Stateful microservices verwijderen de noodzaak van aanvullende wachtrijen en caches die traditioneel nodig zijn om de beschikbaarheid en latentie vereisten van een zuiver stateless toepassing. Stateful services zijn natuurlijk hoge beschikbaarheid en lage latentie, waardoor het aantal bewegende delen beheren in uw toepassing als geheel.

## <a name="supported-programming-models"></a>Ondersteunde programmeermodellen
Service Fabric biedt verschillende manieren om te schrijven en beheren van uw services. Services kunnen gebruiken van de Service Fabric-API's te kunnen profiteren van het platform functies en toepassingsframeworks. Services kunnen ook worden alle gecompileerde uitvoerbaar programma geschreven in elke taal en gehost op een Service Fabric-cluster. Zie voor meer informatie [programmeermodellen ondersteund](service-fabric-choose-framework.md).

### <a name="containers"></a>Containers
Standaard wordt Service Fabric implementeert en services als processen wordt geactiveerd. Service Fabric kunnen ook services implementeren in [containers](service-fabric-containers-overview.md). Belangrijker nog, kunt u services in processen en -services in containers in dezelfde toepassing elkaar. Service Fabric ondersteunt de implementatie van Linux-containers Windows containers op Windows Server 2016. U kunt bestaande toepassingen, stateless services of stateful services in containers implementeren. 

### <a name="reliable-services"></a>Reliable Services
[Reliable Services](service-fabric-reliable-services-introduction.md) is een lichtgewicht framework voor het schrijven van services die integreren met het Service Fabric-platform en profiteren van de volledige set van platformfuncties. Reliable Services kunnen worden staatloze (vergelijkbaar met de meeste platforms van de service, zoals webservers of werkrollen in Azure Cloud Services), waarbij status is opgeslagen in een externe oplossing, zoals Azure DB of Azure Table Storage. Reliable Services kunnen ook stateful, worden waar status rechtstreeks in de service zelf betrouwbare verzamelingen is opgeslagen. Staat worden gesteld [maximaal beschikbare](service-fabric-availability-services.md) door middel van replicatie en gedistribueerde via [partitioneren](service-fabric-concepts-partitioning.md), alle beheerde automatisch door de Service Fabric.

### <a name="reliable-actors"></a>Reliable Actors
Gebouwd op Reliable Services de [betrouwbare Actor](service-fabric-reliable-actors-introduction.md) framework is een toepassingsframework dat het virtuele Actor-patroon, op basis van het ontwerppatroon actor implementeert. Onafhankelijke eenheden van de berekenings- en status van het betrouwbare Actor-framework gebruikt met één thread uitvoering actoren aangeroepen. De betrouwbare Actor framework biedt ingebouwde communicatie voor actoren en vooraf ingesteld status persistentie en scale-out-configuraties.

### <a name="aspnet-core"></a>ASP.NET Core
Service Fabric kan worden geïntegreerd met [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) als een eerste-klas programmeermodel voor het bouwen van websites en toepassingen van de API.  ASP.NET Core kunnen worden gebruikt op twee verschillende manieren in Service Fabric:

- Als het uitvoerbare bestand van een gastbesturingssysteem wordt gehost. Dit is vooral op bestaande ASP.NET Core toepassingen uitvoeren op Service Fabric zonder wijzigingen code gebruikt.
- Binnen een betrouwbare Service uitgevoerd. Dit kunt betere integratie met de Service Fabric-runtime en stateful ASP.NET Core services.

### <a name="guest-executables"></a>Gast uitvoerbare bestanden
Een [Gast uitvoerbaar bestand](service-fabric-deploy-existing-app.md) is een bestaande, willekeurige uitvoerbaar bestand (geschreven in een andere taal) die worden gehost op een Service Fabric-cluster goed samen met andere services. Uitvoerbare bestanden Gast kan niet rechtstreeks te integreren met Service Fabric-API's. Maar ze nog steeds profiteren van het platform, zoals aangepaste health biedt functies laden reporting en zichtbaarheid service door het aanroepen van REST-API's. Ze hebben ook de volledige toepassing lifecycle ondersteuning. 

## <a name="application-lifecycle"></a>Toepassingslevenscyclus
Zoals met andere platforms wordt een toepassing op Service Fabric meestal de volgende fasen doorloopt: ontwerpen, ontwikkelen, testen, implementatie, upgrade, onderhoud en verwijderen. Service Fabric biedt uitstekende ondersteuning voor de levenscyclus van de volledige toepassing van cloud-toepassingen, van ontwikkeling tot implementatie, dagelijkse beheer en onderhoud voor uiteindelijke buiten gebruik stellen. Het service-model kan verschillende verschillende rollen onafhankelijk deelnemen aan de levenscyclus van de toepassing. [De levenscyclus van de service Fabric-toepassing](service-fabric-application-lifecycle.md) biedt een overzicht van de API's en hoe ze worden gebruikt door de verschillende rollen in de fasen van de levenscyclus van de Service Fabric-toepassing. 

De volledige levenscyclus kan worden beheerd via [PowerShell-cmdlets](/powershell/module/ServiceFabric/), [CLI-opdrachten](service-fabric-sfctl.md), [C#-API's](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [Java-API's](/java/api/system.fabric._application_management_client), en [ REST-API's](/rest/api/servicefabric/). U kunt ook continue integratie/continue implementatie pijplijnen met hulpprogramma's zoals instellen [Visual Studio Team Services](service-fabric-set-up-continuous-integration.md) of [Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md).

De volgende video voor Microsoft Virtual Academy wordt beschreven hoe voor het beheren van de levenscyclus van uw toepassing:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=My3Ka56yC_6106218965">
<img src="./media/service-fabric-content-roadmap/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="test-applications-and-services"></a>Testtoepassingen en -services
Werkelijk cloud scale als services wilt maken, is het essentieel om te controleren of uw toepassingen en services echte fouten kunnen tolereren. De fout Analysis Services is ontworpen voor het testen van de services die zijn gebouwd op Service Fabric. Met de [fout Analysis Service](service-fabric-testability-overview.md), kunt u zinvolle fouten veroorzaken en volledige Testscenario's uitvoeren op uw toepassingen. Deze fouten en scenario's uitoefenen en valideren van de talloze statussen en overgangen die een service tijdens de levensduur, allemaal in een gecontroleerde, veilige en consistente manier optreden zal.

[Acties](service-fabric-testability-actions.md) doel van een service voor het testen met behulp van de afzonderlijke fouten. Een ontwikkelaar van de service kunt deze als bouwstenen voor het schrijven van complexe scenario's gebruiken. Voorbeelden van gesimuleerde fouten zijn:

* Start opnieuw op een knooppunt om te simuleren van een willekeurig aantal situaties waarbij een computer of virtuele machine opnieuw is opgestart.
* Verplaatsen van een replica van de stateful service om te simuleren taakverdeling, failover of upgrade van de toepassing.
* Quorumverlies op een stateful service voor het maken van een situatie waarin schrijfbewerkingen kunnen niet doorgaan omdat er niet voldoende 'back-up' of 'secundair' replica's te accepteren van nieuwe gegevens worden aangeroepen.
* Verlies van gegevens op een stateful service voor het maken van een situatie waarin alle geheugenstatus volledig wordt gewist uit worden aangeroepen.

[Scenario's](service-fabric-testability-scenarios.md) complexe bewerkingen bestaan uit een of meer acties. De fout Analysis Services bevat twee ingebouwde voltooien van scenario's:

* [Chaos scenario](service-fabric-controlled-chaos.md)-simuleert continue, interleaved fouten (correcte en geforceerde afsluiting) in het cluster gedurende langere perioden.
* [Failover-scenario](service-fabric-testability-scenarios.md#failover-test)-een versie van het scenario voor het testen van chaos die gericht is op een specifieke service partitie terwijl andere services niet is beïnvloed.

## <a name="clusters"></a>Clusters
Een [Service Fabric-cluster](service-fabric-deploy-anywhere.md) is een met het netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Clusters kunnen worden geschaald naar duizenden computers. Een machine of virtuele machine die deel uitmaakt van een cluster wordt een clusterknooppunt genoemd. Elk knooppunt wordt de knooppuntnaam van een (een tekenreeks) toegewezen. Knooppunten hebben kenmerken zoals plaatsingseigenschappen. Elke computer of virtuele machine heeft een service automatisch starten, `FabricHost.exe`, die begint bij het opstarten worden uitgevoerd en start vervolgens twee uitvoerbare bestanden: Fabric.exe en FabricGateway.exe. Deze twee uitvoerbare bestanden, vormen het knooppunt. Voor het testen van scenario's, kunt u meerdere knooppunten op een enkele computer of virtuele machine host door het uitvoeren van meerdere exemplaren van `Fabric.exe` en `FabricGateway.exe`.

Service Fabric-clusters kunnen worden gemaakt op virtuele of fysieke machines met Windows Server- of Linux. U kunt implementeren en uitvoeren van Service Fabric-toepassingen in een omgeving waar het hebben van een set van Windows Server of Linux-computers die onderling verbonden zijn: on-premises op Microsoft Azure of op elke cloudprovider.

De video volgende Microsoft Virtual Academy beschrijving van Service Fabric-clusters:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/ClusterOverview.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="clusters-on-azure"></a>Clusters op Azure
Service Fabric-clusters biedt op Azure integratie met andere Azure-functies en services, waardoor bewerkingen en beheer van het cluster eenvoudiger en betrouwbaarder. Een cluster is een Azure Resource Manager-bron, waarmee u model kunt clusters zoals elke andere resources in Azure. Resource Manager biedt ook eenvoudig beheer van alle resources die door het cluster wordt gebruikt als één eenheid. Clusters op Azure zijn geïntegreerd met Azure diagnoses en Log Analytics. Knooppunttypen cluster zijn [virtuele-machineschaalsets](/azure/virtual-machine-scale-sets/index), zodat de functionaliteit voor automatisch schalen is ingebouwd in.

U kunt een cluster maken op Azure via de [Azure-portal](service-fabric-cluster-creation-via-portal.md), van een [sjabloon](service-fabric-cluster-creation-via-arm.md), of van [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md).

Service Fabric op Linux kunt u bouwen, implementeren en beheren van maximaal beschikbare, sterk schaalbare toepassingen op Linux, net zoals u zou in Windows doen. De Service Fabric-frameworks (Reliable Services en Reliable Actors) zijn beschikbaar in Java op Linux, naast de C# (.NET Core). U kunt ook samenstellen [uitvoerbare gastservices](service-fabric-deploy-existing-app.md) met elke taal of elk framework. Docker-containers organiseren wordt ook ondersteund. Docker-containers kunnen worden uitgevoerd Gast uitvoerbare bestanden of systeemeigen Service Fabric-services, die de Service Fabric-frameworks gebruiken. Lees voor meer informatie over [Service Fabric op Linux](service-fabric-deploy-anywhere.md).

Er zijn bepaalde functies die worden ondersteund op Windows, maar niet op Linux. Voor meer informatie lezen [verschillen tussen Service Fabric op Linux en Windows](service-fabric-linux-windows-differences.md).

### <a name="standalone-clusters"></a>Zelfstandige clusters
Service Fabric bevat een installatiepakket voor u zelfstandige Service Fabric-clusters lokale maken of op elke cloudprovider. Zelfstandige-clusters bieden u de vrijheid voor het hosten van een cluster elke gewenste locatie. Als uw gegevens beheerst door naleving of voorschriften wordt; of u wilt uw gegevens lokaal te houden, kunt u uw eigen cluster en de toepassingen hosten. Service Fabric-toepassingen kunnen worden uitgevoerd in omgevingen met meerdere hosting zonder wijzigingen, zodat uw kennis van het bouwen van toepassingen via vanaf een hosting-omgeving naar een andere uitvoert. 

[Uw eerste Service Fabric zelfstandige cluster maken](service-fabric-get-started-standalone-cluster.md)

Linux-clusters voor zelfstandige zijn nog niet ondersteund.

### <a name="cluster-security"></a>Clusterbeveiliging
Clusters moeten worden beveiligd om te voorkomen dat onbevoegde gebruikers verbinding maken met uw cluster, vooral wanneer er productieworkloads uitgevoerd. Hoewel het mogelijk om een niet-beveiligde cluster te maken, kan hierdoor anonieme gebruikers verbinding kunnen maken als eindpunten voor beheer worden blootgesteld aan het openbare internet. Het is niet mogelijk later beveiliging inschakelen in een niet-beveiligde cluster: clusterbeveiliging van het is ingeschakeld tijdens het maken van een cluster.

De cluster security scenario's:
* Knooppunt naar beveiliging
* Beveiliging van de client-naar-knooppunt
* Op rollen gebaseerde toegangsbeheer (RBAC)

Lees voor meer informatie [beveiligen van een cluster](service-fabric-cluster-security.md).

### <a name="scaling"></a>Schalen
Als u nieuwe knooppunten aan het cluster toevoegt, rebalances Service Fabric de replica's en exemplaren op tussen het toegenomen aantal knooppunten. De algemene verbetert de prestaties van toepassingen en verkleint u conflicten over toegang tot het geheugen. Als de knooppunten in het cluster niet efficiënt worden gebruikt, kunt u het aantal knooppunten in het cluster verminderen. Service Fabric rebalances opnieuw de replica's en exemplaren op tussen het kleinere aantal knooppunten beter te benutten van de hardware op elk knooppunt. U kunt ofwel clusters op Azure schalen [handmatig](service-fabric-cluster-scale-up-down.md) of [programmatisch](service-fabric-cluster-programmatic-scaling.md). Zelfstandige clusters kunnen worden geschaald [handmatig](service-fabric-cluster-windows-server-add-remove-nodes.md).

### <a name="cluster-upgrades"></a>Cluster-upgrades
Regelmatig zijn nieuwe versies van de Service Fabric-runtime uitgebracht. Runtime of fabric, upgrades uitvoeren op uw cluster zodat u altijd uitvoert een [ondersteunde versie](service-fabric-support.md). Naast de fabric-upgrades kunt u ook clusterconfiguratie zoals certificaten of poorten voor toepassing bijwerken.

Een Service Fabric-cluster is een resource die u bezit, maar gedeeltelijk wordt beheerd door Microsoft. Microsoft is verantwoordelijk voor het onderliggende besturingssysteem patchen en fabric-upgrades uitvoeren op uw cluster. U kunt uw cluster automatische fabric-upgrades ontvangt wanneer Microsoft een nieuwe versie loslaat ingesteld of kies een ondersteunde fabric-versie die u wilt selecteren. Fabric en configuratie-upgrades kunnen worden ingesteld via de Azure-portal of via Resource Manager. Lees voor meer informatie [upgraden van een Service Fabric-cluster](service-fabric-cluster-upgrade.md). 

Een zelfstandige cluster is een resource die u hebt volledig zelf. U bent zelf verantwoordelijk voor het herstellen van het onderliggende besturingssysteem en het starten van de fabric-upgrades. Als uw cluster verbinding met maken kan [https://www.microsoft.com/download](https://www.microsoft.com/download), kunt u het cluster automatisch downloaden en inrichten van het nieuwe pakket van de Service Fabric-runtime instellen. U zou vervolgens de upgrade initiëren. Als uw cluster heeft geen toegang tot [https://www.microsoft.com/download](https://www.microsoft.com/download), kunt u handmatig het nieuwe runtime-pakket te downloaden vanaf een internet verbonden computer en start vervolgens de upgrade. Lees voor meer informatie [upgraden van een zelfstandige Service Fabric-cluster](service-fabric-cluster-upgrade-windows-server.md).

## <a name="health-monitoring"></a>Statuscontrole
Service Fabric introduceert een [statusmodel](service-fabric-health-introduction.md) ontworpen om te markeren slecht cluster en de voorwaarden van toepassing op specifieke entiteiten (zoals clusterknooppunten en service-replica's). Het statusmodel maakt gebruik van health-rapporteurs (onderdelen van het systeem en watchdogs). Het doel is snel en gemakkelijk diagnose en herstel. Schrijvers van de service nodig om na te denken over status vooraf en hoe [ontwerpen health reporting](service-fabric-report-health.md#design-health-reporting). Een voorwaarde die van invloed kan status moet worden gerapporteerd, vooral als deze vlag problemen dicht bij de hoofdmap kan helpen. De statusgegevens bespaart tijd en moeite voor foutopsporing en onderzoek eenmaal in de service actief is op de gewenste schaal in productie.

De Service Fabric-rapporteurs monitor geïdentificeerd voorwaarden van belang. Ze een rapport over deze voorwaarden op basis van hun lokale weergave. De [health store](service-fabric-health-introduction.md#health-store) aggregeert statusgegevens verzonden door alle rapporteurs om te bepalen of entiteiten globaal in orde zijn. Het model is bedoeld om uitgebreide, flexibel en eenvoudig te gebruiken. De kwaliteit van de statusrapporten over bepaalt de nauwkeurigheid van de health-weergave van het cluster. Fout-positieven waarin ten onrechte slecht problemen weergegeven kunnen nadelig upgrades of andere services die gebruikmaken van statusgegevens. Voorbeelden van dergelijke services zijn reparatie en waarschuwen mechanismen. Voorzichtig is daarom nodig is voor rapporten die voorwaarden van belang zijn in de beste manier vastleggen.

Rapportage kan worden gedaan via:
* De bewaakte Service Fabric-service replica of het exemplaar.
* Interne watchdogs geïmplementeerd als een Service Fabric-service (bijvoorbeeld een Service Fabric staatloze service dat wordt bewaakt voorwaarden en rapporten). De watchdogs kunnen worden geïmplementeerd op alle knooppunten of kunnen worden wachtrijen op de bewaakte service.
* Interne watchdogs die worden uitgevoerd op de Service Fabric-knooppunten, maar niet als Service Fabric-services worden geïmplementeerd.
* Externe watchdogs die buiten het Service Fabric-cluster (bijvoorbeeld bewaking service zoals Gomez) waaruit de bron-test.

Buiten het vak een health Service Fabric-onderdelen rapport over alle entiteiten in het cluster. [Systeemstatusrapporten](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) bieden inzicht in het cluster en de toepassing functionaliteit en vlag problemen via health. Voor toepassingen en services controleren systeemstatusrapporten of entiteiten worden geïmplementeerd en correct vanuit het perspectief van de Service Fabric-runtime werkt zijn. De rapporten bieden een statuscontrole van de zakelijke logica van de service of niet vastgelopen processen te detecteren. Specifieke informatie toevoegen aan uw service-logica [implementeren aangepaste health reporting](service-fabric-report-health.md) in uw services.

Service Fabric bevat meerdere manieren om te [statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md) geaggregeerd in de health store:
* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) of andere visualisatie hulpprogramma's.
* Statusquery's (via [PowerShell](/powershell/module/ServiceFabric/), [CLI](service-fabric-sfctl.md), wordt de [C# FabricClient APIs](/dotnet/api/system.fabric.fabricclient.healthclient) en [Java FabricClient APIs](/java/api/system.fabric._health_client), of [REST API's](/rest/api/servicefabric)).
* Algemene query's dat retourneren een lijst van entiteiten met status als een van de eigenschappen (via PowerShell, CLI, de API's of REST).

De volgende video voor Microsoft Virtual Academy beschrijft het statusmodel van de Service Fabric en hoe deze wordt gebruikt:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-content-roadmap/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="monitoring-and-diagnostics"></a>Controle en diagnose
[Controle en diagnostische gegevens](service-fabric-diagnostics-overview.md) cruciaal zijn voor het ontwikkelen, testen en implementeren van toepassingen en services in elke omgeving. Service Fabric-oplossingen werken het beste wanneer u plannen en implementeren van controle en diagnostische gegevens waarmee Zorg ervoor toepassingen dat en services werkt zoals verwacht in een lokale ontwikkelingsomgeving of in de productieomgeving.

De belangrijkste doelstellingen van controle en diagnostische gegevens zijn naar:

- Detecteren en onderzoeken van problemen met hardware- en infrastructuur
- Problemen met software- en app detecteren, de service uitvaltijd beperken
- Resource gebruiks- en help station operations beslissingen begrijpen
- Toepassings-, service- en infrastructuur optimaliseren
- Zakelijke inzichten genereren en te identificeren gebieden van de gebruikerservaring
 
De algemene werkstroom van controle en diagnostische gegevens bestaat uit drie stappen:

1. Genereren van gebeurtenis: dit bevat gebeurtenissen (Logboeken, traceringen, aangepaste gebeurtenissen) op de infrastructuur (cluster), platform en toepassing / service niveau
2. Aggregatie van gebeurtenis: gegenereerde gebeurtenissen moeten worden verzameld en geaggregeerd voordat ze kunnen worden weergegeven
3. Analyse: gebeurtenissen moeten worden gevisualiseerde en toegankelijk zijn in sommige indeling, toestaan voor analyse en indien nodig wordt weergegeven

Meerdere producten zijn beschikbaar die betrekking op deze drie gebieden en u bent gratis verschillende technologieën voor elk kiezen. Lees voor meer informatie [controle en diagnostische gegevens voor Azure Service Fabric](service-fabric-diagnostics-overview.md).

## <a name="next-steps"></a>Volgende stappen
* Leer hoe u een [cluster maakt in Azure](service-fabric-cluster-creation-via-portal.md) of hoe u een [zelfstandig cluster maakt in Windows](service-fabric-cluster-creation-for-windows-server.md).
* Maak een service met het programmeermodel [Reliable Services](service-fabric-reliable-services-quick-start.md) of [Reliable Actors](service-fabric-reliable-actors-get-started.md).
* Meer informatie over hoe [migreren van Cloudservices](service-fabric-cloud-services-migration-differences.md).
* Informatie over het [controle en diagnose van services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). 
* Informatie over het [testen van uw apps en services](service-fabric-testability-overview.md).
* Informatie over het [beheren en te organiseren clusterbronnen](service-fabric-cluster-resource-manager-introduction.md).
* Bekijk de [Service Fabric-voorbeelden](http://aka.ms/servicefabricsamples).
* Meer informatie over [Service Fabric-ondersteuningsopties](service-fabric-support.md).
* Lees de [teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/) voor artikelen en aankondigingen.


[cluster-application-instances]: media/service-fabric-content-roadmap/cluster-application-instances.png
[cluster-imagestore-apptypes]: ./media/service-fabric-content-roadmap/cluster-imagestore-apptypes.png
