---
title: Samengevoegde status van Azure Service Fabric-entiteiten weergeven | Microsoft Docs
description: Beschrijft hoe u query's uitvoeren, bekijken en evalueren van de Azure Service Fabric-entiteiten geaggregeerde status, door middel van statusquery's en algemene query's.
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: ''
ms.assetid: fa34c52d-3a74-4b90-b045-ad67afa43fe5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: c6d5954ed3547666236130753dfd53d10475df43
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52308985"
---
# <a name="view-service-fabric-health-reports"></a>Service Fabric-statusrapporten weergeven
Azure Service Fabric introduceert een [statusmodel](service-fabric-health-introduction.md) met health-entiteiten in welke onderdelen van het systeem en watchdogs kunt rapport lokale voorwaarden die ze bewaken. De [health store](service-fabric-health-introduction.md#health-store) alle health-gegevens om te bepalen of entiteiten in orde zijn.

Het cluster wordt automatisch gevuld met statusrapporten die zijn verzonden door de onderdelen van het systeem. Meer informatie [systeemstatusrapporten gebruiken om op te lossen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Service Fabric biedt verschillende manieren om op te halen van de cumulatieve status van de entiteiten:

* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) of andere hulpprogramma's voor visualisatie
* Statusquery's (via PowerShell, -API of REST)
* Algemene query's dat retourneren een lijst van entiteiten die status als een van de eigenschappen (via PowerShell, -API of REST hebben)

Om te demonstreren van deze opties, gebruiken we een lokaal cluster met vijf knooppunten en de [fabric: / WordCount-toepassing](https://aka.ms/servicefabric-wordcountapp). De **fabric: / WordCount** toepassing bevat twee standaardservices, een stateful service van het type `WordCountServiceType`, en een stateless service van het type `WordCountWebServiceType`. Ik heb gewijzigd de `ApplicationManifest.xml` zeven replica's voor de stateful service en een partitie als doel vereist. Omdat er slechts vijf knooppunten in het cluster, rapport de onderdelen van het systeem een waarschuwing over de servicepartitie, omdat deze lager dan het aantal doel is.

```xml
<Service Name="WordCountService">
  <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
    <UniformInt64Partition PartitionCount="[WordCountService_PartitionCount]" LowKey="1" HighKey="26" />
  </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Status in Service Fabric Explorer
Service Fabric Explorer biedt een visuele weergave van het cluster. In de onderstaande afbeelding ziet u dat:

* De toepassing **fabric: / WordCount** is rood (fout), omdat er een foutgebeurtenis gemeld door **MyWatchdog** voor de eigenschap **beschikbaarheid**.
* Een van de services, **fabric: / WordCount/WordCountService** wordt geel (in de waarschuwing). De service is geconfigureerd met zeven replica's en het cluster heeft vijf knooppunten, zodat twee repicas kan niet worden geplaatst. Hoewel deze wordt hier niet getoond, de servicepartitie monitor geel is vanwege een rapport van `System.FM` mededeling dat `Partition is below target replica or instance count`. De gele partitie wordt de gele service geactiveerd.
* Het cluster is rood vanwege de rode toepassing.

Standaard-beleid van het clustermanifest en het manifest van de toepassing maakt gebruik van de evaluatie. Strikte beleidsregels zijn en niet een storing kan tolereren.

Weergave van het cluster met Service Fabric Explorer:

![Weergave van het cluster met Service Fabric Explorer.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [!NOTE]
> Meer informatie over [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
>
>

## <a name="health-queries"></a>Statusquery 's
Service Fabric biedt statusquery's voor elk van de ondersteunde [Entiteitstypen](service-fabric-health-introduction.md#health-entities-and-hierarchy). Ze kunnen worden geopend via de API, met behulp van methoden op [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell-cmdlets en REST. Deze query's retourneren statusinformatie over de entiteit: de geaggregeerde status, entiteit health-gebeurtenissen, statussen onderliggende (indien van toepassing), evaluaties met slechte status (als de entiteit niet in orde is) en kinderen health statistieken (wanneer van toepassing).

> [!NOTE]
> Een health-entiteit wordt geretourneerd wanneer het volledig wordt gevuld in health store. De entiteit moet actief zijn (geen verwijderd) en een rapport. De bovenliggende entiteiten in de keten van de hiërarchie moeten ook systeemrapporten hebben. Als een van deze voorwaarden niet wordt voldaan, de status retourneren vraagt een [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) met [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` wordt uitgelegd waarom de entiteit niet wordt geretourneerd.
>
>

De health-query's moeten in de entiteit-id, afhankelijk van het entiteitstype doorgeven. De query's accepteren optioneel health beleidsparameters. Als er geen statusbeleid zijn opgegeven, de [statusbeleid](service-fabric-health-introduction.md#health-policies) uit een manifest van het cluster of de toepassing worden gebruikt voor de evaluatie. Als de manifesten geen een definitie voor statusbeleid bevatten, worden de standaardbeleidsregels voor health gebruikt voor de evaluatie. De standaard-statusbeleid doen niet tolereren fouten. De query's ook accepteren filters voor het retourneren van slechts gedeeltelijke kinderen of gebeurtenissen--die aansluiten bij de opgegeven filters. Een ander filter kunt met uitzondering van de statistieken van de onderliggende items.

> [!NOTE]
> De uitvoerfilters worden toegepast op de server, zodat de grootte van het bericht beantwoorden wordt verminderd. U wordt aangeraden dat u de uitvoerfilters gebruiken om te beperken van de gegevens die zijn geretourneerd, in plaats van filters toepassen op de client.
>
>

Status van een entiteit bevat:

* De samengevoegde status van de entiteit. Door de health store op basis van de statusrapporten entiteit, onderliggende statussen (indien van toepassing) en statusbeleid wordt berekend. Meer informatie over [evalueren entiteit](service-fabric-health-introduction.md#health-evaluation).  
* De health-gebeurtenissen voor de entiteit.
* De verzameling van statussen van alle onderliggende items voor de entiteiten die onderliggende elementen kunnen hebben. De statussen bevatten entiteit-id's en de geaggregeerde status. Als u de status voor een kind, aanroepen van de status van de query voor het type van de onderliggende entiteit en doorgeven in de onderliggende-id.
* De evaluaties met slechte status die naar het rapport dat de status van de entiteit geactiveerd verwijzen als de entiteit niet in orde is. De beoordelingen zijn recursieve, met de health-evaluaties van kinderen waarmee huidige status is geactiveerd. Bijvoorbeeld een watchdog gerapporteerd voor een fout op basis van een replica. De status van de toepassing bevat een onjuiste evaluatie vanwege een slechte service; de service is niet in orde vanwege een partitie in de fout. de partitie heeft een slechte status vanwege een replica in een fout. de replica is niet in orde omdat het statusrapport watchdog-fout.
* De health-statistieken voor alle onderliggende items typen van de entiteiten die onderliggende elementen hebben. Bijvoorbeeld clusterstatus toont het totale aantal toepassingen, services, partities, replica's en entiteiten in het cluster geïmplementeerd. Service health toont het totale aantal partities en replica's onder de opgegeven service.

## <a name="get-cluster-health"></a>Clusterstatus ophalen
Retourneert de status van de cluster-entiteit en bevat de statussen van toepassingen en knooppunten (onderliggende items van het cluster). Invoer:

* [Optioneel] Het cluster statusbeleid gebruikt voor het evalueren van de knooppunten en de Clustergebeurtenissen.
* [Optioneel] De toepassing health policy-toewijzing, met het statusbeleid dat wordt gebruikt voor het overschrijven van de application manifest beleidsregels.
* [Optioneel] Filters voor gebeurtenissen, knooppunten en -toepassingen die welke items opgeven van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen, knooppunten en -toepassingen worden gebruikt om te evalueren van de entiteitsstatus samengevoegd, ongeacht het filter.
* [Optioneel] Filteren als u wilt uitsluiten van de van gezondheidsstatistieken.
* [Optioneel] Filter om op te nemen van fabric: / System health statistieken in de status van statistieken. Alleen van toepassing wanneer de health-statistieken niet worden uitgesloten. Standaard bevatten de health-statistieken worden alleen de statistieken voor gebruikerstoepassingen en niet de toepassing van het systeem.

### <a name="api"></a>API
Als u de clusterstatus, maakt een `FabricClient` en roep de [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) methode voor de **HealthManager**.

De volgende aanroep wordt de clusterstatus:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

De volgende code haalt de clusterstatus met behulp van een beleid voor aangepaste cluster status en filters voor knooppunten en toepassingen. Dit geeft aan dat de health-statistieken de fabric bevatten: / statistieken van het systeem. Worden er [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), die de invoergegevens bevat.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var healthStatisticsFilter = new ClusterHealthStatisticsFilter()
{
    ExcludeHealthStatistics = false,
    IncludeSystemApplicationHealthStatistics = true
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
    HealthStatisticsFilter = healthStatisticsFilter
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de clusterstatus is [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

De status van het cluster is vijf knooppunten, het system-toepassing en de fabric: / WordCount geconfigureerd zoals wordt beschreven.

De volgende cmdlet haalt de clusterstatus met behulp van het statusbeleid standaard. De samengevoegde status van de waarschuwing, omdat de fabric: / WordCount-toepassing is in de waarschuwing. Houd er rekening mee hoe de evaluaties met slechte status informatie bieden over de voorwaarden die de geaggregeerde status wordt geactiveerd.

```xml
PS D:\ServiceFabric> Get-ServiceFabricClusterHealth


AggregatedHealthState   : Warning
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                          
                          
NodeHealthStates        : 
                          NodeName              : _Node_4
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_0
                          AggregatedHealthState : Ok
                          
ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok
                          
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning
                          
HealthEvents            : None
HealthStatistics        : 
                          Node                  : 5 Ok, 0 Warning, 0 Error
                          Replica               : 6 Ok, 0 Warning, 0 Error
                          Partition             : 1 Ok, 1 Warning, 0 Error
                          Service               : 1 Ok, 1 Warning, 0 Error
                          DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                          DeployedApplication   : 5 Ok, 0 Warning, 0 Error
                          Application           : 0 Ok, 1 Warning, 0 Error
```

De volgende PowerShell-cmdlet haalt de status van het cluster met behulp van een aangepaste toepassing-beleid. Deze filtert de resultaten om alleen de toepassingen en knooppunten in een fout of waarschuwing. Als gevolg hiervan worden geen knooppunten geretourneerd, omdat ze allemaal in orde zijn. Alleen de fabric: / WordCount-toepassing rekening wordt gehouden met het filter toepassingen. Omdat het aangepaste beleid bepaalt u rekening moet houden waarschuwingen als fouten voor de fabric: / WordCount-toepassing, de toepassing zoals in de fout wordt geëvalueerd, en dus is het cluster.

```powershell
PS D:\ServiceFabric> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics


AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                          
                          
NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error
                          
HealthEvents            : None
```

### <a name="rest"></a>REST
U krijgt de clusterstatus van het met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-node-health"></a>Knooppuntstatus ophalen
Retourneert de status van een entiteit knooppunt en de health-gebeurtenissen gemeld op het knooppunt bevat. Invoer:

* (Vereist) De naam van het knooppunt waarmee het knooppunt.
* [Optioneel] De cluster health beleidsinstellingen gebruikt voor het evalueren van de status.
* [Optioneel] Filters voor gebeurtenissen die aangeven welke items van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen worden gebruikt voor het evalueren van de entiteitsstatus samengevoegd, ongeacht het filter.

### <a name="api"></a>API
Als u de gezondheid van knooppunt via de API, maakt een `FabricClient` en roep de [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) methode voor de HealthManager.

De volgende code wordt de knooppuntstatus voor de naam van het opgegeven knooppunt:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

De volgende code wordt de knooppuntstatus voor de naam van het opgegeven knooppunt en wordt doorgegeven in de gebeurtenissen filteren en aangepaste beleid via [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de knooppuntstatus is [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.
De volgende cmdlet wordt de knooppuntstatus opgehaald met behulp van standaard statusbeleid:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 7/13/2017 4:39:23 PM
                        ReceivedAt            : 7/13/2017 4:40:47 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 4:40:47 PM, LastWarning = 1/1/0001 12:00:00 AM
```

De volgende cmdlet wordt de status van alle knooppunten in het cluster:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_4                     Ok
_Node_3                     Ok
_Node_2                     Ok
_Node_1                     Ok
_Node_0                     Ok
```

### <a name="rest"></a>REST
U krijgt de knooppuntstatus van het met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-application-health"></a>Status van de toepassing ophalen
Hiermee wordt de status van een Toepassingsentiteit. De statussen van de geïmplementeerde toepassing en service onderliggende items bevat. Invoer:

* (Vereist) De toepassingsnaam (URI) die de toepassing identificeert.
* [Optioneel] Het statusbeleid voor de toepassing gebruikt voor het overschrijven van de application manifest beleidsregels.
* [Optioneel] Filters voor gebeurtenissen, services en toepassingen die welke items opgeeft van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen, services en toepassingen worden gebruikt om te evalueren van de entiteitsstatus samengevoegd, ongeacht het filter.
* [Optioneel] Als u wilt uitsluiten van de health-statistieken filteren. Als niet is opgegeven, de health-statistieken bevatten de ok, waarschuwing en het aantal fouten voor alle onderliggende items van toepassing: services, partities, replica's, geïmplementeerde toepassingen en servicepakketten wordt geïmplementeerd.

### <a name="api"></a>API
Als u de status van de toepassing, maak een `FabricClient` en roep de [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) methode voor de HealthManager.

De volgende code wordt de status van de toepassing voor de opgegeven toepassingsnaam (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

De volgende code wordt de status van de toepassing voor de opgegeven toepassingsnaam (URI), met filters en aangepaste beleidsregels die zijn opgegeven via [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de status van de toepassing is [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

De volgende cmdlet retourneert de status van de **fabric: / WordCount** toepassing:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok
                                  
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning
                                  
DeployedApplicationHealthStates : 
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok
                                  
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/13/2017 5:57:05 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
                                  
HealthStatistics                : 
                                  Replica               : 6 Ok, 0 Warning, 0 Error
                                  Partition             : 1 Ok, 1 Warning, 0 Error
                                  Service               : 1 Ok, 1 Warning, 0 Error
                                  DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                                  DeployedApplication   : 5 Ok, 0 Warning, 0 Error
```

De volgende PowerShell-cmdlet wordt doorgegeven in het aangepaste beleid. Deze filters ook onderliggende items en gebeurtenissen.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error
                                  
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>REST
U krijgt de status van de toepassing met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-service-health"></a>Servicestatus ophalen
Hiermee wordt de status van een service-entiteit. De statussen van de partitie bevat. Invoer:

* (Vereist) De servicenaam (URI) dat de service identificeert.
* [Optioneel] Het statusbeleid voor de toepassing gebruikt voor het overschrijven van het manifest beleid van toepassing.
* [Optioneel] Filters voor gebeurtenissen en partities die welke items opgeeft van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen en partities worden gebruikt voor het evalueren van de entiteitsstatus samengevoegd, ongeacht het filter.
* [Optioneel] Filteren als u wilt uitsluiten van de van gezondheidsstatistieken. Als dat niet wordt opgegeven, de statistieken van de status ok, waarschuwing, weergeven en fout telt voor alle partities en replica's van de service.

### <a name="api"></a>API
Als u de status van de service via de API, maakt u een `FabricClient` en roep de [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) methode voor de HealthManager.

Het volgende voorbeeld wordt de status van een service met de opgegeven service-naam (URI):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

De volgende code verkrijgt de servicestatus voor de opgegeven servicenaam (URI), filters en via aangepaste beleid op te geven [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de status van de service is [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

De volgende cmdlet wordt de status van de service met behulp van standaard statusbeleid opgehaald:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                        
                        Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                        
                            Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
PartitionHealthStates : 
                        PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                        AggregatedHealthState : Warning
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 15
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
                        Partition             : 0 Ok, 1 Warning, 0 Error
```

### <a name="rest"></a>REST
U krijgt de status van de service met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-partition-health"></a>Status van de partitie ophalen
Hiermee wordt de status van een entiteit partitie. Het bevat de replica-statussen. Invoer:

* (Vereist) De partitie-ID (GUID) die de partitie identificeert.
* [Optioneel] Het statusbeleid voor de toepassing gebruikt voor het overschrijven van het manifest beleid van toepassing.
* [Optioneel] Filters voor gebeurtenissen en replica's die welke items opgeeft van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen en replica's worden gebruikt voor het evalueren van de entiteitsstatus samengevoegd, ongeacht het filter.
* [Optioneel] Filteren als u wilt uitsluiten van de van gezondheidsstatistieken. Indien niet opgegeven, de health-statistieken laten zien hoeveel replica's in ok, waarschuwing en fout Staten.

### <a name="api"></a>API
Als u de status van de partitie via de API, maakt een `FabricClient` en roep de [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) methode voor de HealthManager. Als u optionele parameters, maken [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de status van de partitie is [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

De volgende cmdlet wordt de status voor alle partities van de **fabric: / WordCount/WordCountService** -service en uitgefilterd replica statussen:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 72
                        SentAt                : 7/13/2017 5:57:29 PM
                        ReceivedAt            : 7/13/2017 5:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/P RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/S RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 7/13/2017 5:57:48 PM, LastError = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131444445174851664
                        SentAt                : 7/13/2017 6:35:17 PM
                        ReceivedAt            : 7/13/2017 6:35:18 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        Secondary replica could not be placed due to the following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A
                        
                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.
                        
                        Nodes Eliminated By Constraints:
                        
                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/13/2017 5:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a>REST
U krijgt de status van de partitie met een [GET-aanvraag](/rest/api/servicefabric/sfclient-api-getpartitionhealth) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-replica-health"></a>Replica-status ophalen
Hiermee wordt de status van een stateful service-replica of een stateless service-exemplaar. Invoer:

* (Vereist) De partitie-ID (GUID) en de replica-ID waarmee de replica.
* [Optioneel] De toepassing health beleidsparameters gebruikt voor het overschrijven van de application manifest beleidsregels.
* [Optioneel] Filters voor gebeurtenissen die aangeven welke items van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen worden gebruikt voor het evalueren van de entiteitsstatus samengevoegd, ongeacht het filter.

### <a name="api"></a>API
Als u de status replica via de API, maakt u een `FabricClient` en roep de [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) methode voor de HealthManager. Gebruik het geavanceerde parameters opgeven [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de status van de replica is [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

De volgende cmdlet wordt de status van de primaire replica voor alle partities van de service:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
U krijgt de status van de replica met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-deployed-application-health"></a>Status van de geïmplementeerde toepassing ophalen
Hiermee wordt de status van een toepassing is geïmplementeerd op een knooppunt-entiteit. De statussen van de geïmplementeerde service-pakket bevat. Invoer:

* (Vereist) De toepassingsnaam (URI) en een naam van het knooppunt (tekenreeks) waarmee de geïmplementeerde toepassing.
* [Optioneel] Het statusbeleid voor de toepassing gebruikt voor het overschrijven van de application manifest beleidsregels.
* [Optioneel] Filters voor gebeurtenissen en geïmplementeerde servicepakketten die welke items opgeeft van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen en geïmplementeerde servicepakketten worden gebruikt voor het evalueren van de entiteitsstatus samengevoegd, ongeacht het filter.
* [Optioneel] Filteren als u wilt uitsluiten van de van gezondheidsstatistieken. Indien niet opgegeven, wordt in de health-statistieken het aantal geïmplementeerde servicepakketten in ok, waarschuwing en fout statussen weergeven.

### <a name="api"></a>API
Als u de status van een toepassing is geïmplementeerd op een knooppunt via de API, maakt u een `FabricClient` en roep de [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) methode voor de HealthManager. Gebruik het optionele parameters opgeven [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de status van de geïmplementeerde toepassing is [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet. Als u wilt weten waar een toepassing wordt geïmplementeerd, worden uitgevoerd [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) en kijken naar de geïmplementeerde toepassing onderliggende objecten.

De volgende cmdlet wordt de status van de **fabric: / WordCount** toepassing die is geïmplementeerd op **_Node_2**.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_0


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_0
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
                                     ServiceManifestName   : WordCountWebServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131444422261848308
                                     SentAt                : 7/13/2017 5:57:06 PM
                                     ReceivedAt            : 7/13/2017 5:57:17 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/13/2017 5:57:17 PM, LastWarning = 1/1/0001 12:00:00 AM
                                     
HealthStatistics                   : 
                                     DeployedServicePackage : 2 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a>REST
U krijgt de status van de geïmplementeerde toepassing met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-deployed-service-package-health"></a>Pakket van geïmplementeerde servicestatus ophalen
Hiermee wordt de status van een geïmplementeerde service package-entiteit. Invoer:

* (Vereist) De toepassingsnaam (URI), de naam van het knooppunt (tekenreeks) en de service manifest van de naam (tekenreeks) die het pakket geïmplementeerde service identificeren.
* [Optioneel] Het statusbeleid voor de toepassing gebruikt voor het overschrijven van het manifest beleid van toepassing.
* [Optioneel] Filters voor gebeurtenissen die aangeven welke items van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen worden gebruikt voor het evalueren van de entiteitsstatus samengevoegd, ongeacht het filter.

### <a name="api"></a>API
Als u de status van een geïmplementeerde service-pakket via de API, maakt u een `FabricClient` en roep de [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) methode voor de HealthManager. Gebruik het optionele parameters opgeven [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de status geïmplementeerde service-pakket is [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet. Als u wilt zien waarop een toepassing wordt geïmplementeerd, [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) en kijken naar de geïmplementeerde toepassingen. Als u wilt zien welke service-pakketten in een toepassing zijn, kijken naar de geïmplementeerde service pakket onderliggende objecten in de [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) uitvoer.

De volgende cmdlet wordt de status van de **WordCountServicePkg** servicepakket van de **fabric: / WordCount** toepassing die is geïmplementeerd op **_Node_2**. De entiteit heeft **System.Hosting** rapporten voor service-pakket en invoerpunt voor activeren is gelukt en succesvolle registratie van het type van de service.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_2
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131444422267693359
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : The ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131444422267903345
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : The CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131444422272458374
                             SentAt                : 7/13/2017 5:57:07 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : The ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
U krijgt een geïmplementeerde service pakket health gebruiken met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="health-chunk-queries"></a>Query's van de gezondheid van segment
Het segment statusquery's kunnen meerdere niveaus cluster kinderen (recursief), per invoer filters retourneren. Deze biedt ondersteuning voor geavanceerde filters waarmee u veel flexibiliteit bij het kiezen van de onderliggende objecten moeten worden geretourneerd. De filters kunnen onderliggende items door de unieke id of door andere groeps-id's en/of de statussen opgeven. Standaard zijn geen onderliggende items opgenomen, in plaats van health-opdrachten die altijd eerste niveau onderliggende elementen bevatten.

De [statusquery's](service-fabric-view-entities-aggregated-health.md#health-queries) alleen onderliggende eerste niveau van de opgegeven entiteit per vereist filters retourneren. Als u de onderliggende objecten van de onderliggende objecten, moet u extra health API's aanroepen voor elke entiteit die van belang zijn. Op dezelfde manier als u de status van specifieke entiteiten, moet u een status-API aanroepen voor elke gewenste entiteit. Chunk kunt Geavanceerd filteren u de query om aan te vragen van meerdere items in één query, het minimaliseren van de grootte van het bericht en het aantal berichten van belang.

De waarde van de query segment, is dat u de status voor meer cluster entiteiten (mogelijk alle cluster entiteiten vanaf vereist hoofdmap) in één aanroep. U kunt de gezondheid van complexe query zoals express:

* Alleen toepassingen met geretourneerde fout, en voor deze toepassingen zijn alle services in waarschuwing of fout. Neem alle partities voor de geretourneerde services.
* Retourneert alleen de status van de vier toepassingen, die zijn opgegeven met hun namen.
* Retourneert alleen de status van toepassingen van een toepassingstype van de gewenste.
* Alle geïmplementeerde entiteiten op een knooppunt retourneren. Alle toepassingen, retourneert alle toepassingen op het opgegeven knooppunt en alle geïmplementeerde servicepakketten op dat knooppunt geïmplementeerd.
* Alle replica's in een fout geretourneerd. Retourneert alle toepassingen, services, partities en alleen replica's in de fout.
* Retourneren van alle toepassingen. Neem alle partities voor een opgegeven service.

De status chunk query wordt op dit moment blootgesteld alleen voor de cluster-entiteit. Het resultaat een cluster health segment, die bevat:

* De status van de cluster geaggregeerd.
* De status status chunk lijst met knooppunten die aansluiten bij invoer filters.
* De status status chunk lijst met toepassingen die aansluiten bij invoer filters. Elk segment application health status bevat een lijst segment met alle services die aansluiten bij invoer filters en een chunk-lijst met alle geïmplementeerde toepassingen die aansluiten bij de filters. Hetzelfde voor de onderliggende objecten van services en toepassingen. Op deze manier alle entiteiten in het cluster kunnen worden mogelijk geretourneerd als in een hiërarchische manier aangevraagd.

### <a name="cluster-health-chunk-query"></a>Cluster health chunk query
De status van de cluster-entiteit retourneert en de hiërarchische health state segmenten van vereiste onderliggende items bevat. Invoer:

* [Optioneel] Het cluster statusbeleid gebruikt voor het evalueren van de knooppunten en de Clustergebeurtenissen.
* [Optioneel] De toepassing health policy-toewijzing, met het statusbeleid dat wordt gebruikt voor het overschrijven van de application manifest beleidsregels.
* [Optioneel] Filters voor knooppunten en -toepassingen die welke items opgeven van belang zijn en moeten worden geretourneerd in het resultaat. De filters zijn specifiek voor een entiteitsgroep van entiteiten of zijn van toepassing op alle entiteiten op dat niveau. De lijst met filters kan bevatten een algemeen filter-en/of filters voor specifieke id's voor gebruikerssegmentatie entiteiten geretourneerd door de query. Als dit leeg is, worden de onderliggende objecten niet standaard geretourneerd.
  Meer informatie over de filters [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) en [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter). De toepassing filters kunnen recursief geavanceerde filters opgeven voor kinderen.

Het resultaat van het segment bevat de onderliggende objecten die aansluiten bij de filters.

Op dit moment retourneert de segment-query geen evaluaties met slechte status of entiteit gebeurtenissen. Deze extra gegevens kan worden verkregen met behulp van het bestaande cluster health query.

### <a name="api"></a>API
Als u de cluster health segment, maken een `FabricClient` en roep de [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) methode voor de **HealthManager**. U kunt doorgeven [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) om te beschrijven van statusbeleid en geavanceerde filters.

De volgende code verkrijgt cluster health segment met geavanceerde filters.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de clusterstatus is [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

De volgende code verkrijgt knooppunten alleen als deze fout, met uitzondering van een bepaald knooppunt zijn, moet altijd worden geretourneerd.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters


HealthState                  : Warning
NodeHealthStateChunks        : 
                               TotalCount            : 1
                               
                               NodeName              : _Node_1
                               HealthState           : Ok
                               
ApplicationHealthStateChunks : None
```

De volgende cmdlet haalt cluster segment met toepassingsfilters.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 1
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks : 
                                TotalCount            : 1
                               
                                ServiceName           : fabric:/WordCount/WordCountService
                                HealthState           : Error
                                PartitionHealthStateChunks : 
                                    TotalCount            : 1
                               
                                    PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                                    HealthState           : Error
                                    ReplicaHealthStateChunks : 
                                        TotalCount            : 5
                               
                                        ReplicaOrInstanceId   : 131444422293118720
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293118721
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113678
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113679
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422260002646
                                        HealthState           : Error
```

De volgende cmdlet retourneert alle geïmplementeerde entiteiten op een knooppunt.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 2
                               
                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : FAS
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
                               
                               
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : WordCountServicePkg
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
```

### <a name="rest"></a>REST
U krijgt cluster health segment met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) die statusbeleid en geavanceerde filters die worden beschreven in de hoofdtekst bevat.

## <a name="general-queries"></a>Algemene query 's
Algemene query's retourneren een lijst met Service Fabric-entiteiten van een bepaald type. Ze worden weergegeven via de API (via de methoden op **FabricClient.QueryManager**), PowerShell-cmdlets en REST. Deze query's samenvoegen subquery's uit meerdere onderdelen. Een van beide is de [health store](service-fabric-health-introduction.md#health-store), die de cumulatieve status van het resultaat van elke query wordt gevuld.  

> [!NOTE]
> Algemene query's retourneren van de cumulatieve status van de entiteit en hebben geen uitgebreide statusgegevens. Als een entiteit niet in orde is, kunt u zich kunt opvolgen met statusquery's om op te halen van alle de health-informatie, met inbegrip van gebeurtenissen, statussen van de onderliggende en evaluaties met slechte status.
>
>

Als algemene query's een onbekende status voor een entiteit retourneren, is het mogelijk dat de health store geen volledige gegevens over de entiteit. Het is ook mogelijk dat een subquery naar de health store niet geslaagd is (bijvoorbeeld, er is een communicatiefout is opgetreden of het statusarchief wordt beperkt). Contact opnemen met een status-query voor de entiteit. Als de subquery tijdelijke fouten, zoals netwerkproblemen aangetroffen, wordt deze follow-up query mogelijk wel. Ook kan geven u meer details in health store over waarom de entiteit is niet beschikbaar gemaakt.

De query's met **HealthState** voor entiteiten zijn:

* Knooppuntenlijst: retourneert de lijst met knooppunten in het cluster (wisselbaar geheugen:).
  * API: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)
  * PowerShell: Get-ServiceFabricNode
* Lijst met toepassingen: retourneert de lijst met toepassingen in het cluster (wisselbaar geheugen:).
  * API: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)
  * PowerShell: Get-ServiceFabricApplication
* Lijst met Services: retourneert de lijst met services in een toepassing (wisselbaar geheugen:).
  * API: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)
  * PowerShell: Get-ServiceFabricService
* Partitielijst: retourneert de lijst met partities in een service (wisselbaar geheugen:).
  * API: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)
  * PowerShell: Get-ServiceFabricPartition
* Lijst met replica's: retourneert de lijst met replica's in een partitie (wisselbaar geheugen:).
  * API: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)
  * PowerShell: Get-ServiceFabricReplica
* Lijst met toepassingen geïmplementeerd: retourneert de lijst met geïmplementeerde toepassingen op een knooppunt.
  * API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication
* De lijst van de service-pakket wordt geïmplementeerd: retourneert de lijst met service-pakketten in een geïmplementeerde toepassing.
  * API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication

> [!NOTE]
> Sommige van de query's retourneren pagina's met zoekresultaten. Het rendement van deze query's is een lijst die is afgeleid van [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1). Als de resultaten niet zijn voor een bericht geschikt, wordt alleen een pagina wordt geretourneerd en wordt een ContinuationToken die worden bijgehouden waar opsomming is gestopt. Doorgaan met het aanroepen van dezelfde query en van de eerdere query voor het ophalen van de volgende resultaten in het vervolgtoken dat wordt doorgegeven.
>
>

### <a name="examples"></a>Voorbeelden
De volgende code wordt de beschadigde toepassingen in het cluster:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

De volgende cmdlet haalt de Toepassingdetails voor de fabric: / WordCount-toepassing. U ziet dat de status bij waarschuwing is.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

De volgende cmdlet haalt de services met een status van de fout:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Error"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Error
```

## <a name="cluster-and-application-upgrades"></a>Upgrades van cluster en de toepassing
Tijdens een bewaakte upgrade van het cluster en de toepassing controleert Service Fabric health om ervoor te zorgen dat alles in orde blijft. Als een entiteit niet in orde is als geëvalueerd met behulp van de geconfigureerde statusbeleid, is de upgrade upgrade-specifiek beleid om te bepalen van de volgende actie van toepassing. De upgrade kan worden onderbroken als u wilt toestaan dat tussenkomst van de gebruiker (zoals de vaststelling van de fout of wijzigen van beleid) of deze mogelijk automatisch terugkeren naar de vorige versie van de goede.

Tijdens een *cluster* upgrade, krijgt u de upgradestatus van het cluster. De status van de upgrade bevat evaluaties met slechte status, die verwijzen naar wat in het cluster niet in orde is. Als de upgrade is teruggedraaid vanwege problemen met de status, onthoudt de upgradestatus weer de laatste redenen niet in orde. Deze informatie kan helpen beheerders onderzoeken wat er mis ging nadat de upgrade is teruggedraaid of gestopt.

Op deze manier tijdens een *toepassing* upgrade, alle evaluaties met slechte status zijn opgenomen in de upgradestatus van toepassing.

Hieronder ziet u de status van de toepassing bijwerken voor een gewijzigde fabric: / WordCount-toepassing. Een watchdog heeft een fout gerapporteerd op een van de replica's. De upgrade is beschikbaar omdat de statuscontroles niet worden nageleefd.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2017 5:23:26 PM
FailureTimestampUtc           : 4/21/2017 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Meer informatie over de [Service Fabric-toepassingsupgrade](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Gebruik health evaluaties om op te lossen
Wanneer er een probleem met het cluster of een toepassing is, bekijkt u de status van het cluster of de toepassing naar wat er mis is met deze functie. De evaluaties met slechte status vindt u informatie over wat de huidige status niet in orde geactiveerd. Als u wilt, kunt u zich kunt inzoomen op beschadigde onderliggende entiteiten om te identificeren van de hoofdoorzaak te achterhalen.

Neem bijvoorbeeld een toepassing niet in orde omdat er een foutenrapport op een van de replica's. De volgende Powershell-cmdlet ziet u de evaluaties met slechte status:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount -EventsFilter None -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.
                                  
                                        Unhealthy replica: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', ReplicaOrInstanceId='131444422260002646', AggregatedHealthState='Error'.
                                  
                                            Error event: SourceId='MyWatchdog', Property='Memory'.
                                  
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

U kunt de replica voor meer informatie bekijken:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricReplicaHealth -ReplicaOrInstanceId 131444422260002646 -PartitionId af2e3e44-a8f8-45ac-9f31-4093eb897600


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Error
UnhealthyEvaluations  : 
                        Error event: SourceId='MyWatchdog', Property='Memory'.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
                        SourceId              : MyWatchdog
                        Property              : Memory
                        HealthState           : Error
                        SequenceNumber        : 131444451657749403
                        SentAt                : 7/13/2017 6:46:05 PM
                        ReceivedAt            : 7/13/2017 6:46:05 PM
                        TTL                   : Infinite
                        Description           : 
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 7/13/2017 6:46:05 PM, LastOk = 1/1/0001 12:00:00 AM
```

> [!NOTE]
> De evaluaties met slechte status weergegeven in dat de eerste reden de entiteit wordt geëvalueerd aan de huidige status. Mogelijk zijn er meerdere andere gebeurtenissen die deze status activeren, maar ze worden niet weergegeven in de evaluaties. Voor meer informatie, Inzoomen op de van statusentiteiten om de beschadigde rapporten in het cluster te achterhalen.
>
>

## <a name="next-steps"></a>Volgende stappen
[Use system health reports to troubleshoot (Systeemstatusrapporten gebruiken om problemen op te lossen)](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Aangepaste statusrapporten van Service Fabric toevoegen](service-fabric-report-health.md)

[Het servicestatus rapporteren en controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Controle en diagnose van services lokaal](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric-toepassingsupgrade](service-fabric-application-upgrade.md)
