---
title: Het weergeven van Azure Service Fabric-entiteiten geaggregeerd health | Microsoft Docs
description: Beschrijft hoe een query, bekijken en evalueren van de Azure Service Fabric-entiteiten geaggregeerde status, door middel van statusquery's en algemene query's.
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: fa34c52d-3a74-4b90-b045-ad67afa43fe5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/11/2017
ms.author: oanapl
ms.openlocfilehash: acd3168adc6624e172099c8d62124f7b5ae4839a
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="view-service-fabric-health-reports"></a>Service Fabric-statusrapporten weergeven
Azure Service Fabric introduceert een [statusmodel](service-fabric-health-introduction.md) met health entiteiten op welke onderdelen van het systeem en watchdogs kunt rapport lokale voorwaarden die ze bewaken. De [health store](service-fabric-health-introduction.md#health-store) aggregeert alle health gegevens om te bepalen of de entiteiten in orde zijn.

Het cluster wordt automatisch gevuld met statusrapporten dat is verzonden door de onderdelen van het systeem. Meer informatie op [systeemstatusrapporten gebruiken om op te lossen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Service Fabric bevat meerdere manieren om op te halen van de cumulatieve status van de entiteiten:

* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) of andere visualisatie hulpprogramma's
* Statusquery's (via PowerShell, API of REST)
* Algemene query's dat retourneren een lijst van entiteiten met status als een van de eigenschappen (via PowerShell, API of REST)

Ter illustratie van deze opties we gebruiken een lokaal cluster met vijf knooppunten en de [fabric: / WordCount-toepassing](http://aka.ms/servicefabric-wordcountapp). De **fabric: / WordCount** toepassing bevat twee standaardservices, een stateful service van het type `WordCountServiceType`, en een stateless service van het type `WordCountWebServiceType`. Ik heb gewijzigd de `ApplicationManifest.xml` vereisen zeven replica's voor de stateful service en één partitie zijn gericht. Omdat er slechts vijf knooppunten in het cluster, de onderdelen van het systeem een rapport een waarschuwing over de partitie van de service omdat deze lager dan het aantal doel is.

```xml
<Service Name="WordCountService">
  <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
    <UniformInt64Partition PartitionCount="[WordCountService_PartitionCount]" LowKey="1" HighKey="26" />
  </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Status in Service Fabric Explorer
Service Fabric Explorer biedt een visuele weergave van het cluster. In de onderstaande afbeelding ziet u dat:

* De toepassing **fabric: / WordCount** is rood (fout) omdat er een foutgebeurtenis gemeld door **MyWatchdog** voor de eigenschap **beschikbaarheid**.
* Een van de services ontvangt, **fabric: / WordCount/WordCountService** geel gekleurd (in de waarschuwing). De service is geconfigureerd met zeven replica's en het cluster heeft vijf knooppunten en daarom twee repicas kan niet worden geplaatst. Hoewel dit niet wordt weergegeven hier, de service-partitie geel is vanwege een rapport van `System.FM` mededeling dat `Partition is below target replica or instance count`. De gele partitie activeert de gele service.
* Het cluster is rood vanwege de rode toepassing.

De evaluatie gebruikt standaardbeleidsregels van het clustermanifest en het toepassingsmanifest. Strikte beleidsregels zijn en niet een storing kan tolereren.

Weergave van het cluster met Service Fabric Explorer:

![Weergave van het cluster met Service Fabric Explorer.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [!NOTE]
> Lees meer over [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
>
>

## <a name="health-queries"></a>Statusquery 's
Service Fabric statusquery's beschrijft voor elk van de ondersteunde [Entiteitstypen](service-fabric-health-introduction.md#health-entities-and-hierarchy). Toegankelijk zijn via de API, met behulp van methoden op [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell-cmdlets en REST. Deze query's retourneren voltooid statusgegevens van de entiteit: de geaggregeerde status, entiteit health gebeurtenissen onderliggende statussen (indien van toepassing), slechte evaluaties (als de entiteit is niet in orde) en statistieken van kinderen health (wanneer van toepassing).

> [!NOTE]
> Een health-entiteit wordt geretourneerd wanneer het volledig is gevuld in de health store. De entiteit moet actief zijn (geen verwijderd) en een rapport. De bovenliggende entiteiten in de keten van de hiërarchie moeten ook systeemrapporten hebben. Als een van deze voorwaarden niet wordt voldaan, de status retourneren vraagt een [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) met [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` die ziet waarom de entiteit is niet geretourneerd.
>
>

De health-query's moeten in de entiteit-id is afhankelijk van het entiteitstype doorgeven. De query's accepteren optionele health beleidsparameters. Als er geen statusbeleid worden opgegeven, de [statusbeleid](service-fabric-health-introduction.md#health-policies) uit het cluster of application manifest worden gebruikt voor evaluatie. Als de manifesten geen definitie voor statusbeleid bevatten, worden de standaardbeleidsregels voor health gebruikt voor evaluatie. Het standaardbeleid voor health doen niet zonder fouten. De query's ook accepteren filters voor het retourneren van slechts gedeeltelijk onderliggende elementen of gebeurtenissen--de waarden die de opgegeven filters respecteren. Een ander filter kunt met uitzondering van de statistieken van de onderliggende elementen.

> [!NOTE]
> De uitvoerfilters worden toegepast op de server, zodat het bericht beantwoorden wordt verkleind. Het is raadzaam dat u de uitvoerfilters gebruiken om te beperken van de gegevens die zijn geretourneerd, in plaats van filters toepassen op de client.
>
>

De status van de entiteit bevat:

* De cumulatieve status van de entiteit. Door de health store op basis van entiteit statusrapporten, onderliggende statussen (indien van toepassing) en statusbeleid wordt berekend. Lees meer over [entiteit de statusevaluatie](service-fabric-health-introduction.md#health-evaluation).  
* De health-gebeurtenissen voor de entiteit.
* De verzameling van de status van alle onderliggende items voor de entiteiten die onderliggende elementen kunnen hebben. De statussen bevatten entiteit-id's en de geaggregeerde status. Als u de status voor een kind, aanroepen van de status van de query voor het entiteitstype onderliggende en in de onderliggende-id.
* De slechte evaluaties die naar het rapport dat de status van de entiteit geactiveerd verwijzen als de entiteit is niet in orde. De beoordelingen zijn recursieve, met de onderliggende elementen health evaluaties waarmee huidige status is geactiveerd. Bijvoorbeeld, een watchdog een fout op basis van een replica gemeld. De toepassingsstatus bevat een onjuiste evaluatie vanwege een onjuiste service; de service is beschadigd vanwege een partitie in error; de partitie is beschadigd vanwege een replica in error; de replica is beschadigd vanwege het statusrapport watchdog-fout.
* De health-statistieken voor alle typen voor onderliggende elementen van de entiteiten die onderliggende elementen hebben. Bijvoorbeeld, cluster-status toont het totale aantal toepassingen, services, partities, replica's en entiteiten in het cluster wordt geïmplementeerd. Servicestatus toont het totale aantal partities en replica's onder de opgegeven service.

## <a name="get-cluster-health"></a>Status van de cluster ophalen
Retourneert de status van de entiteit van het cluster en de statussen van toepassingen en de knooppunten (onderliggende elementen van het cluster) bevat. Invoer:

* [Optioneel] Het cluster statusbeleid gebruikt voor het evalueren van de knooppunten en de Clustergebeurtenissen.
* [Optioneel] De toepassing health beleid kaart met het statusbeleid dat wordt gebruikt voor het overschrijven van de application manifest beleidsregels.
* [Optioneel] Filters voor gebeurtenissen, knooppunten en toepassingen die welke vermeldingen opgeeft van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen, knooppunten en toepassingen worden gebruikt voor het evalueren van de entiteitsstatus geaggregeerd, ongeacht het filter.
* [Optioneel] Filter moeten worden uitgesloten van de van gezondheidsstatistieken.
* [Optioneel] Opgenomen fabric: / System health statistieken in de health-statistieken. Alleen van toepassing wanneer de health-statistieken niet worden uitgesloten. Standaard bevatten de health-statistieken worden alleen de statistieken voor toepassingen en niet de systeemtoepassing.

### <a name="api"></a>API
Als u het cluster health, maak een `FabricClient` en roept u de [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) methode op de **HealthManager**.

De volgende aanroep wordt de status van het cluster:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

De volgende code haalt de status van het cluster met behulp van een aangepaste cluster statusbeleid en filters voor knooppunten en toepassingen. Hiermee geeft u dat de health-statistieken de fabric bevatten: / statistieken van het systeem. Het maken van [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), die de invoergegevens bevat.

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
De cmdlet om op te halen van de status van het cluster is [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

De status van het cluster is vijf knooppunten, de systeemtoepassing en fabric: / WordCount geconfigureerd zoals wordt beschreven.

De volgende cmdlet haalt de gezondheid van het cluster met behulp van standaard statusbeleid. De geaggregeerde status waarschuwing, omdat de fabric: / WordCount-toepassing bevindt zich in de waarschuwing. Houd er rekening mee hoe de slechte evaluaties gedetailleerde informatie bevat over de voorwaarden waarmee de geaggregeerde status is geactiveerd.

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

De volgende PowerShell-cmdlet haalt de status van het cluster met een aangepaste toepassing-beleid. Deze filtert de resultaten om alleen de toepassingen en de knooppunten in de fout of waarschuwing. Als gevolg hiervan worden geen knooppunten geretourneerd, omdat ze allemaal in orde. Alleen de fabric: / WordCount-toepassing rekening wordt gehouden met het filter toepassingen. Omdat het aangepaste beleid bepaalt u moet overwegen waarschuwingen als fouten voor de fabric: / WordCount-toepassing, de toepassing wordt geëvalueerd als in de fout en is daarom het cluster.

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
U kunt de status van de cluster met krijgen een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-node-health"></a>Ophalen van de gezondheid van knooppunt
Retourneert de status van de entiteit van een knooppunt en de health-gebeurtenissen gemeld op het knooppunt bevat. Invoer:

* [Vereist] De knooppuntnaam waarin het knooppunt.
* [Optioneel] De cluster health beleidsinstellingen gebruikt voor het evalueren van de status.
* [Optioneel] Filters voor gebeurtenissen die aangeven welke posten van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen die worden gebruikt voor het evalueren van de entiteitsstatus geaggregeerd, ongeacht het filter.

### <a name="api"></a>API
Als u het knooppunt status via de API, maakt u een `FabricClient` en roept u de [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) methode op de HealthManager.

De volgende code wordt de status van het knooppunt voor de naam van het opgegeven knooppunt:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

De volgende code haalt u de status van het knooppunt voor de naam van het opgegeven knooppunt en wordt doorgegeven in gebeurtenissen filteren en aangepast beleid via [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de status van het knooppunt is [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.
De volgende cmdlet wordt de status van het knooppunt met behulp van standaard statusbeleid opgehaald:

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
U krijgt de gezondheid van knooppunt met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-application-health"></a>Toepassingsstatus ophalen
Retourneert de status van een Toepassingsentiteit. De statussen van de geïmplementeerde toepassing en service onderliggende items bevat. Invoer:

* [Vereist] De toepassingsnaam (URI) die de toepassing te identificeren.
* [Optioneel] Het statusbeleid voor toepassing gebruikt voor het overschrijven van de application manifest beleidsregels.
* [Optioneel] Filters voor gebeurtenissen, services en geïmplementeerde toepassingen die welke vermeldingen opgeeft van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen, services en geïmplementeerde toepassingen worden gebruikt voor het evalueren van de entiteitsstatus geaggregeerd, ongeacht het filter.
* [Optioneel] Als u wilt uitsluiten van de health-statistieken filteren. Als niet wordt opgegeven, de statistieken health bevatten de ok, waarschuwing en fout aantal voor alle onderliggende objecten van toepassing: services, partities, replica's, geïmplementeerde toepassingen en geïmplementeerde servicepakketten.

### <a name="api"></a>API
Als u de toepassing health, maak een `FabricClient` en roept u de [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) methode op de HealthManager.

De volgende code wordt de toepassingsstatus voor de opgegeven toepassingsnaam (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

De volgende code wordt de toepassingsstatus voor de opgegeven toepassingsnaam (URI), met filters en aangepaste beleidsregels opgegeven via [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).

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
De cmdlet om op te halen van de toepassingsstatus is [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

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

De volgende PowerShell-cmdlet wordt doorgegeven in een aangepast beleid. Deze filters ook onderliggende elementen en gebeurtenissen.

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
U krijgt toepassingsstatus met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-service-health"></a>Servicestatus ophalen
Retourneert de status van een service-entiteit. Het bevat de statussen van de partitie. Invoer:

* [Vereist] De servicenaam (URI) dat de service identificeert.
* [Optioneel] Het statusbeleid voor toepassing gebruikt voor het onderdrukken van het application manifest-beleid.
* [Optioneel] Filters voor gebeurtenissen en partities die welke vermeldingen opgeeft van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen en partities worden gebruikt voor het evalueren van de entiteitsstatus geaggregeerd, ongeacht het filter.
* [Optioneel] Filter moeten worden uitgesloten van de van gezondheidsstatistieken. Als dat niet wordt opgegeven, de statistieken van de status ok, waarschuwing, weergeven en fout tellen voor alle partities en replica's van de service.

### <a name="api"></a>API
Als u de status van de service via de API, maakt u een `FabricClient` en roept u de [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) methode op de HealthManager.

Het volgende voorbeeld wordt de status van een service met de opgegeven service-naam (URI):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

De volgende code haalt de servicestatus voor de opgegeven service-naam (URI), filters en aangepast beleid via geven [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):

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
Retourneert de status van een entiteit van de partitie. De replica-statussen bevat. Invoer:

* [Vereist] De partitie-ID (GUID) die de partitie identificeert.
* [Optioneel] Het statusbeleid voor toepassing gebruikt voor het onderdrukken van het application manifest-beleid.
* [Optioneel] Filters voor gebeurtenissen en replica's die welke vermeldingen opgeeft van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen en replica's worden gebruikt voor het evalueren van de entiteitsstatus geaggregeerd, ongeacht het filter.
* [Optioneel] Filter moeten worden uitgesloten van de van gezondheidsstatistieken. Als niet wordt opgegeven, wordt de health-statistieken weergeven hoeveel replica's in ok, waarschuwing en fout statussen.

### <a name="api"></a>API
Als u de status van de partitie via de API, maakt u een `FabricClient` en roept u de [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) methode op de HealthManager. Optionele als parameters wilt opgeven, maken [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de status van de partitie is [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

De volgende cmdlet wordt de status voor alle partities van de **fabric: / WordCount/WordCountService** -service en uitsluit replica statussen:

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
U kunt de status van de partitie met krijgen een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-replica-health"></a>De status replica ophalen
Retourneert de status van de replica van een stateful service of een staatloze service-exemplaar. Invoer:

* [Vereist] De partitie-ID (GUID) en de replica-ID waarmee de replica.
* [Optioneel] De toepassing health beleidsparameters gebruikt voor het overschrijven van de application manifest beleidsregels.
* [Optioneel] Filters voor gebeurtenissen die aangeven welke posten van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen die worden gebruikt voor het evalueren van de entiteitsstatus geaggregeerd, ongeacht het filter.

### <a name="api"></a>API
Als u de status replica via de API, maakt u een `FabricClient` en roept u de [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) methode op de HealthManager. Geavanceerde als parameters wilt opgeven, gebruikt u [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).

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
U krijgt de status replica met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-deployed-application-health"></a>Status van de geïmplementeerde toepassing ophalen
Retourneert de status van een toepassing is geïmplementeerd op een knooppunt-entiteit. De statussen van de geïmplementeerde service-pakket bevat. Invoer:

* [Vereist] De toepassingsnaam (URI) en de knooppuntnaam (tekenreeks) waarmee de geïmplementeerde toepassing worden geïdentificeerd.
* [Optioneel] Het statusbeleid voor toepassing gebruikt voor het overschrijven van de application manifest beleidsregels.
* [Optioneel] Filters voor gebeurtenissen en geïmplementeerde servicepakketten die welke vermeldingen opgeeft van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen en geïmplementeerde servicepakketten worden gebruikt voor het evalueren van de entiteitsstatus geaggregeerd, ongeacht het filter.
* [Optioneel] Filter moeten worden uitgesloten van de van gezondheidsstatistieken. Als niet wordt opgegeven, weergeven de statistieken van de gezondheid van het aantal geïmplementeerde servicepakketten in de status ok, waarschuwing en fout.

### <a name="api"></a>API
Als u de status van een toepassing is geïmplementeerd op een knooppunt via de API, maakt u een `FabricClient` en roept u de [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) methode op de HealthManager. Gebruik het optionele parameters opgeven [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de status geïmplementeerde toepassing is [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet. Uitvoeren als u wilt weten waar een toepassing wordt geïmplementeerd, [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) en kijk in de geïmplementeerde toepassing onderliggende elementen.

De volgende cmdlet wordt de status van de **fabric: / WordCount** toepassing geïmplementeerd op **_Node_2**.

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
U krijgt de status geïmplementeerde toepassing met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="get-deployed-service-package-health"></a>Status van geïmplementeerde service pakket ophalen
Retourneert de status van een geïmplementeerde service pakket entiteit. Invoer:

* [Vereist] De toepassingsnaam (URI), knooppuntnaam (tekenreeks) en manifest servicenaam (tekenreeks) die het pakket geïmplementeerde service identificeren.
* [Optioneel] Het statusbeleid voor toepassing gebruikt voor het onderdrukken van het application manifest-beleid.
* [Optioneel] Filters voor gebeurtenissen die aangeven welke posten van belang zijn en moeten worden geretourneerd in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen die worden gebruikt voor het evalueren van de entiteitsstatus geaggregeerd, ongeacht het filter.

### <a name="api"></a>API
Als u de status van een geïmplementeerde service-pakket via de API, maakt u een `FabricClient` en roept u de [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) methode op de HealthManager. Gebruik het optionele parameters opgeven [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
De cmdlet om op te halen van de status geïmplementeerde service-pakket is [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet. Als u wilt zien waar een toepassing wordt geïmplementeerd, [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) en kijk in de geïmplementeerde toepassingen. Als u wilt zien welke service pakketten in een toepassing zijn, kijken naar de onderliggende geïmplementeerde service pakket in de [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) uitvoer.

De volgende cmdlet wordt de status van de **WordCountServicePkg** servicepakket van de **fabric: / WordCount** toepassing geïmplementeerd op **_Node_2**. De entiteit heeft **System.Hosting** rapporten voor geslaagde pakket service en ingangspunt activering en succesvolle registratie van het type van de service.

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
U kunt de status van de geïmplementeerde service pakket met krijgen een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) die wordt beschreven in de hoofdtekst van het statusbeleid bevat.

## <a name="health-chunk-queries"></a>Health chunk query 's
Cluster met meerdere niveaus van kinderen (recursief) per invoer filters kunnen worden geretourneerd door de chunk statusquery's. Biedt ondersteuning voor geavanceerde filters waarmee een grote flexibiliteit bij het kiezen van de onderliggende items moeten worden geretourneerd. De filters kunnen onderliggende items opgeven door de unieke id of door andere groeps-id's en/of de statussen. Geen onderliggende elementen zijn standaard opgenomen in plaats van de health-opdrachten die altijd eerste niveau onderliggende elementen bevatten.

De [statusquery's](service-fabric-view-entities-aggregated-health.md#health-queries) alleen eerste niveau onderliggende elementen van de opgegeven entiteit per vereist filters retourneren. Als u de onderliggende leden van de onderliggende elementen, moet u extra health API's voor elke entiteit van belang aanroepen. Op dezelfde manier als u de status van specifieke entiteiten, moet u voor elke gewenste entiteit één health API aanroepen. Chunk kunt geavanceerde filteren u de query voor het aanvragen van meerdere items in een query, de grootte van het bericht en het aantal berichten voor het minimaliseren van belang.

De waarde van de query chunk is dat u de status voor meer cluster entiteiten (mogelijk alle cluster entiteiten starten in de hoofdmap van de vereiste) kunt krijgen in één aanroep. U kunt de gezondheid van complexe query zoals express:

* Retour alleen toepassingen in de fout en voor die toepassingen bevatten alle services in waarschuwing of fout. Voor de geretourneerde services omvatten alle partities.
* Alleen de status van de opgegeven door de namen van de vier toepassingen retourneren.
* Retourneert alleen de status van toepassingen van een type van de gewenste toepassing.
* Retourneert alle geïmplementeerde entiteiten op een knooppunt. Retourneert alle toepassingen, alle toepassingen op het opgegeven knooppunt en alle geïmplementeerde servicepakketten op dat knooppunt geïmplementeerd.
* Alle replica's in een fout geretourneerd. Retourneert alle toepassingen, services, partities en alleen replica's in de fout.
* Retourneren van alle toepassingen. Voor een opgegeven service omvatten alle partities.

Op dit moment wordt de status chunk query blootgesteld alleen voor de entiteit van het cluster. Een cluster health chunk, waarin wordt:

* De status van het cluster geaggregeerd.
* De health status chunk lijst met knooppunten die fungeren als invoer filters respecteren.
* De health status chunk lijst met toepassingen die invoer filters respecteren. Elk segment application health-status bevat een lijst chunk met alle services die invoer filters en een chunk-lijst met alle geïmplementeerde toepassingen die de filters respecteren respecteren. Hetzelfde voor de onderliggende elementen van services en geïmplementeerde toepassingen. Op deze manier alle entiteiten in het cluster kunnen worden mogelijk geretourneerd als in een hiërarchische aangevraagd.

### <a name="cluster-health-chunk-query"></a>Cluster health chunk query
Retourneert de status van de entiteit van het cluster en de hiërarchische health status segmenten van vereiste onderliggende items bevat. Invoer:

* [Optioneel] Het cluster statusbeleid gebruikt voor het evalueren van de knooppunten en de Clustergebeurtenissen.
* [Optioneel] De toepassing health beleid kaart met het statusbeleid dat wordt gebruikt voor het overschrijven van de application manifest beleidsregels.
* [Optioneel] Filters voor knooppunten en toepassingen die welke vermeldingen opgeeft van belang zijn en moeten worden geretourneerd in het resultaat. De filters zijn specifiek voor een entiteit of de groep van entiteiten of zijn van toepassing op alle entiteiten op dat niveau. De lijst met filters kan bevatten één algemene filter en/of filters voor specifieke id's voor fijnmazige entiteiten die door de query zijn geretourneerd. Als u niets opgeeft, worden de onderliggende elementen niet standaard geretourneerd.
  Meer informatie over de filters op [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) en [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter). De toepassing filters kunnen recursief geavanceerde filters voor kinderen opgeven.

Het resultaat van het segment bevat de onderliggende items waarvan de filters respecteren.

De query chunk levert op dit moment geen slechte evaluaties of entiteit gebeurtenissen. Deze extra informatie kan worden verkregen met behulp van de bestaande query van de cluster-status.

### <a name="api"></a>API
Als u het cluster health chunk, maakt een `FabricClient` en roept u de [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) methode op de **HealthManager**. U kunt doorgeven [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) te beschrijven statusbeleid en geavanceerde filters.

De volgende code haalt cluster health chunk met geavanceerde filters.

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
De cmdlet om op te halen van de status van het cluster is [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk). Eerst verbinding met het cluster met behulp van de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

De volgende code haalt knooppunten alleen als deze fout, met uitzondering van een specifiek knooppunt dat moet altijd worden geretourneerd.

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

De volgende cmdlet haalt cluster chunk met toepassingsfilters.

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
U krijgt cluster health chunk gevonden met een [GET-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) of een [POST-aanvraag](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) die statusbeleid en geavanceerde filters wordt beschreven in de hoofdtekst bevat.

## <a name="general-queries"></a>Algemene query 's
Algemene query's retourneren een lijst met Service Fabric-entiteiten van een bepaald type. Ze beschikbaar worden gesteld via de API (via de methoden op **FabricClient.QueryManager**), PowerShell-cmdlets en REST. Deze query's samenvoegen subquery's uit meerdere onderdelen. Een ervan de [health store](service-fabric-health-introduction.md#health-store), die de geaggregeerde status voor elke queryresultaat wordt gevuld.  

> [!NOTE]
> Algemene query's terug van de cumulatieve status van de entiteit en bevatten geen uitgebreide statusgegevens. Als u een entiteit is niet in orde, kunt u met statusquery's voor alle de health-informatie, zoals gebeurtenissen, onderliggende statussen en slecht evaluaties opvolgen.
>
>

Als algemene query's een onbekende status voor een entiteit retourneren, is het mogelijk dat de health store geen volledige gegevens over de entiteit. Het is ook mogelijk dat een subquery naar de health store niet geslaagd is (bijvoorbeeld: Er is een communicatiefout opgetreden of de health store is beperkt). Opvolgen met een status-query voor de entiteit. Als de subquery tijdelijke fouten, zoals netwerkproblemen aangetroffen, kan deze vervolgzelfstudie query slagen. Ook kan geven u meer informatie uit de store health over waarom de entiteit is niet beschikbaar gemaakt.

De query's met **HealthState** voor entiteiten zijn:

* Lijst met knooppunten: de lijst met knooppunten in het cluster (wisselbare) retourneert.
  * API: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)
  * PowerShell: Get-ServiceFabricNode
* Lijst met toepassingen: de lijst met toepassingen in het cluster (wisselbare) retourneert.
  * API: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)
  * PowerShell: Get-ServiceFabricApplication
* De lijst service: de lijst met services in een toepassing (wisselbare) retourneert.
  * API: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)
  * PowerShell: Get-ServiceFabricService
* Lijst met partitie: de lijst met partities in een service (wisselbare) retourneert.
  * API: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)
  * PowerShell: Get-ServiceFabricPartition
* Lijst met replica's: de lijst met replica's in een partitie (wisselbare) retourneert.
  * API: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)
  * PowerShell: Get-ServiceFabricReplica
* Lijst met toepassingen geïmplementeerd: de lijst met geïmplementeerde toepassingen op een knooppunt retourneert.
  * API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication
* De lijst van de service-pakket wordt geïmplementeerd: retourneert de lijst met servicepakketten in een geïmplementeerde toepassing.
  * API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication

> [!NOTE]
> Sommige van de query's wisselbare resultaten geretourneerd. Het retourtype van deze query's is een lijst die is afgeleid van [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1). Als de resultaten een bericht niet past alleen een pagina wordt geretourneerd en een ContinuationToken die houdt waarin de opsomming is gestopt. Aanroepen van dezelfde query- en in het vervolgtoken uit de vorige query ophalen van de volgende resultaten blijven.
>
>

### <a name="examples"></a>Voorbeelden
De volgende code haalt de slechte toepassingen in het cluster:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

De volgende cmdlet haalt de toepassingsgegevens voor de fabric: / WordCount-toepassing. U ziet dat de status op de waarschuwing.

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

De volgende cmdlet wordt opgehaald van de services met een status van de fout:

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

## <a name="cluster-and-application-upgrades"></a>Cluster en de toepassing upgrades
Tijdens een bewaakte upgrade van het cluster en de toepassing controleert Service Fabric health om ervoor te zorgen dat alles in orde blijft. Als een entiteit niet in orde is als geëvalueerd met behulp van de geconfigureerde statusbeleid, geldt de upgrade upgrade-specifiek beleid om te bepalen van de volgende actie. De upgrade kan worden onderbroken zodat gebruikersinteractie (zoals de vaststelling van de fout of wijzigen van beleid) of het mogelijk automatisch terugkeren naar de vorige versie goed.

Tijdens een *cluster* bijwerkt, krijgt u de upgradestatus van het cluster. De upgradestatus omvat slecht evaluaties die verwijzen naar wat is er niet in orde in het cluster. Als de upgrade is teruggedraaid vanwege statusproblemen met de, onthoudt de upgradestatus van de laatste slecht redenen. Deze informatie kunt beheerders onderzoeken wat er mis ging nadat de upgrade is teruggedraaid of gestopt.

Op deze manier tijdens een *toepassing* bijwerkt, alle slechte beoordelingen zijn opgenomen in de upgradestatus van toepassing.

Hieronder vindt u de status van de toepassing bijwerken voor een gewijzigde fabric: / WordCount-toepassing. Een watchdog heeft een fout gerapporteerd op een van de replica's. De upgrade is rolling omdat de statuscontroles niet worden nageleefd.

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

Meer informatie over de [upgrade van de Service Fabric-toepassing](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Gebruik health evaluaties om op te lossen
Wanneer er een probleem met het cluster of een toepassing wordt gebruikt, bekijkt u de status van het cluster of de toepassing te achterhalen wat is het probleem. De slechte evaluaties bieden informatie over wat de huidige slecht geactiveerd. Als u wilt, kunt u inzoomen op beschadigde onderliggende entiteiten voor het identificeren van de hoofdoorzaak te achterhalen.

Neem bijvoorbeeld een toepassing slecht omdat er een foutenrapport op een van de replica's. De volgende Powershell-cmdlet ziet u de slechte evaluaties:

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
> De slechte evaluaties weergeven in dat de eerste reden de entiteit wordt geëvalueerd naar de huidige status. Mogelijk zijn er meerdere andere gebeurtenissen die deze status activeren, maar ze niet zijn doorgevoerd bij de evaluatie. Voor meer informatie, Inzoomen op de health-entiteiten om alle slechte rapporten in het cluster te achterhalen.
>
>

## <a name="next-steps"></a>Volgende stappen
[Use system health reports to troubleshoot (Systeemstatusrapporten gebruiken om problemen op te lossen)](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Aangepaste Service Fabric-statusrapporten toevoegen](service-fabric-report-health.md)

[Het rapport en controleer de servicestatus van de](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Controle en diagnose van lokaal services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade van de service Fabric-toepassing](service-fabric-application-upgrade.md)
