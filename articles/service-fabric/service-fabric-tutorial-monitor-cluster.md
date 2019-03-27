---
title: Een Service Fabric-cluster in Azure controleren | Microsoft Docs
description: In deze zelfstudie leert u hoe u een cluster bewaken met Service Fabric-gebeurtenissen bekijken, uitvoeren van query's de APIs EventStore, bewaking van prestatiemeteritems en statusrapporten weergeven.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/13/2019
ms.author: srrengar
ms.custom: mvc
ms.openlocfilehash: 66f5651f394c5d91327f6f804f6f15034bba7883
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58500029"
---
# <a name="tutorial-monitor-a-service-fabric-cluster-in-azure"></a>Zelfstudie: Een Service Fabric-cluster in Azure controleren

Controle en diagnose zijn essentieel voor het ontwikkelen, testen en implementeren van workloads in een cloudomgeving. Deze zelfstudie is deel twee van een reeks en ziet u hoe u controle en diagnose van een Service Fabric-cluster met behulp van gebeurtenissen, prestatiemeteritems en statusrapporten.   Lees voor meer informatie het overzicht over [clustercontrole](service-fabric-diagnostics-overview.md#platform-cluster-monitoring) en [infrastructuurbewaking](service-fabric-diagnostics-overview.md#infrastructure-performance-monitoring).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Service Fabric-gebeurtenissen weergeven
> * Query EventStore APIs voor Clustergebeurtenissen
> * Infrastructuur/verzamelen prestatiemeteritems bewaken
> * Cluster-statusrapporten weergeven

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * Een beveiligd [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) maken in Azure met behulp van een sjabloon
> * Een cluster bewaken
> * [Een cluster in- of uitschalen](service-fabric-tutorial-scale-cluster.md)
> * [De runtime van een cluster upgraden](service-fabric-tutorial-upgrade-cluster.md)
> * [Een cluster verwijderen](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Installeer de [Azure Powershell-module, versie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) of de [Azure CLI](/cli/azure/install-azure-cli).
* Maak een veilig [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) 
* Setup [diagnostische verzameling](service-fabric-tutorial-create-vnet-and-windows-cluster.md#configurediagnostics_anchor) voor het cluster
* Schakel de [EventStore service](service-fabric-tutorial-create-vnet-and-windows-cluster.md#configureeventstore_anchor) in het cluster
* Configureer [Azure Monitor-logboeken en de Log Analytics-agent](service-fabric-tutorial-create-vnet-and-windows-cluster.md#configureloganalytics_anchor) voor het cluster

## <a name="view-service-fabric-events-using-azure-monitor-logs"></a>Service Fabric-gebeurtenissen weergeven met behulp van Azure Monitor-Logboeken

Azure Monitor-logboeken verzamelt en analyseert telemetrie van toepassingen en services die worden gehost in de cloud biedt analyse hulpprogramma's waarmee u hun beschikbaarheid en prestaties te maximaliseren. U kunt query's uitvoeren in Azure Monitor-logboeken inzicht en problemen met wat er gebeurt in uw cluster.

Voor toegang tot de Service Fabric-analyse-oplossing, gaat u naar de [Azure-portal](https://portal.azure.com) en selecteer de resourcegroep waarin u de Service Fabric-analyse-oplossing hebt gemaakt.

Selecteer de resource **ServiceFabric(mysfomsworkspace)**.

In **overzicht** u tegels in de vorm van een grafiek zien voor elk van de oplossingen ingeschakeld, met inbegrip van een Service Fabric. Klik op de **Service Fabric** graph om door te gaan naar de Service Fabric-analyse-oplossing.

![Service Fabric-oplossing](media/service-fabric-tutorial-monitor-cluster/oms-service-fabric-summary.png)

De volgende afbeelding ziet de startpagina van de Service Fabric-analyse-oplossing. Deze pagina introductiepagina bevat een momentopname van wat in uw cluster gebeurt er.

![Service Fabric-oplossing](media/service-fabric-tutorial-monitor-cluster/oms-service-fabric-solution.png)

 Als u diagnostische gegevens bij het maken van een cluster hebt ingeschakeld, kunt u gebeurtenissen voor bekijken 

* [Service Fabric-cluster-gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md)
* [Reliable Actors programming modelgebeurtenissen](service-fabric-reliable-actors-diagnostics.md)
* [Reliable Services programming modelgebeurtenissen](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Naast de Service Fabric-gebeurtenissen uit het vak, meer gedetailleerde systeemgebeurtenissen kunnen worden verzameld door [bijwerken van de configuratie van de extensie voor diagnostische gegevens](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

### <a name="view-service-fabric-events-including-actions-on-nodes"></a>Service Fabric-gebeurtenissen weergeven, met inbegrip van acties op knooppunten

Klik op de pagina Service Fabric-analyse op de grafiek voor **Clustergebeurtenissen**.  De logboeken voor alle systeemgebeurtenissen die zijn verzameld, worden weergegeven. Voor een verwijzing naar deze zijn van de **WADServiceFabricSystemEventsTable** in Azure Storage-account en op dezelfde manier de reliable services en actors-gebeurtenissen ziet u naast afkomstig zijn van de respectieve tabellen.
    
![Query operationele kanaal](media/service-fabric-tutorial-monitor-cluster/oms-service-fabric-events.png)

De query gebruikt de Kusto-query-taal, die u aanpassen kunt om te verfijnen wat u zoekt. Bijvoorbeeld, als u wilt zoeken in alle acties die worden uitgevoerd op knooppunten in het cluster, kunt u de volgende query uit. De gebeurtenis-id's gebruikt hieronder vindt u in de [operationele kanaal gebeurtenissen verwijzing](service-fabric-diagnostics-event-generation-operational.md).

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

De Kusto-query-taal is krachtig. Hier volgen enkele andere handige query's.

Maak een *ServiceFabricEvent* opzoektabel als de gebruiker gedefinieerde functie door de query op te slaan als een functie met een alias ServiceFabricEvent:

```kusto
let ServiceFabricEvent = datatable(EventId: int, EventName: string)
[
    ...
    18603, 'NodeUpOperational',
    18604, 'NodeDownOperational',
    ...
];
ServiceFabricEvent
```

Geretourneerde operationele gebeurtenissen vastgelegd in het afgelopen uur:
```kusto
ServiceFabricOperationalEvent
| where TimeGenerated > ago(1h)
| join kind=leftouter ServiceFabricEvent on EventId
| project EventId, EventName, TaskName, Computer, ApplicationName, EventMessage, TimeGenerated
| sort by TimeGenerated
```

Operationele gebeurtenissen met de gebeurtenis-id geretourneerd == 18604 en EventName == 'NodeDownOperational':
```kusto
ServiceFabricOperationalEvent
| where EventId == 18604
| project EventId, EventName = 'NodeDownOperational', TaskName, Computer, EventMessage, TimeGenerated
| sort by TimeGenerated 
```

Operationele gebeurtenissen met de gebeurtenis-id geretourneerd == 18604 en EventName == 'NodeUpOperational':
```kusto
ServiceFabricOperationalEvent
| where EventId == 18603
| project EventId, EventName = 'NodeUpOperational', TaskName, Computer, EventMessage, TimeGenerated
| sort by TimeGenerated 
``` 
 
Retourneert de statusrapporten aan HealthState == 3 (fout) en de aanvullende eigenschappen onttrekken aan het veld EventMessage:

```kusto
ServiceFabricOperationalEvent
| join kind=leftouter ServiceFabricEvent on EventId
| extend HealthStateId = extract(@"HealthState=(\S+) ", 1, EventMessage, typeof(int))
| where TaskName == 'HM' and HealthStateId == 3
| extend SourceId = extract(@"SourceId=(\S+) ", 1, EventMessage, typeof(string)),
         Property = extract(@"Property=(\S+) ", 1, EventMessage, typeof(string)),
         HealthState = case(HealthStateId == 0, 'Invalid', HealthStateId == 1, 'Ok', HealthStateId == 2, 'Warning', HealthStateId == 3, 'Error', 'Unknown'),
         TTL = extract(@"TTL=(\S+) ", 1, EventMessage, typeof(string)),
         SequenceNumber = extract(@"SequenceNumber=(\S+) ", 1, EventMessage, typeof(string)),
         Description = extract(@"Description='([\S\s, ^']+)' ", 1, EventMessage, typeof(string)),
         RemoveWhenExpired = extract(@"RemoveWhenExpired=(\S+) ", 1, EventMessage, typeof(bool)),
         SourceUTCTimestamp = extract(@"SourceUTCTimestamp=(\S+)", 1, EventMessage, typeof(datetime)),
         ApplicationName = extract(@"ApplicationName=(\S+) ", 1, EventMessage, typeof(string)),
         ServiceManifest = extract(@"ServiceManifest=(\S+) ", 1, EventMessage, typeof(string)),
         InstanceId = extract(@"InstanceId=(\S+) ", 1, EventMessage, typeof(string)),
         ServicePackageActivationId = extract(@"ServicePackageActivationId=(\S+) ", 1, EventMessage, typeof(string)),
         NodeName = extract(@"NodeName=(\S+) ", 1, EventMessage, typeof(string)),
         Partition = extract(@"Partition=(\S+) ", 1, EventMessage, typeof(string)),
         StatelessInstance = extract(@"StatelessInstance=(\S+) ", 1, EventMessage, typeof(string)),
         StatefulReplica = extract(@"StatefulReplica=(\S+) ", 1, EventMessage, typeof(string))
```

Een grafiek met gebeurtenissen met gebeurtenis-id geretourneerd! = 17523:

```kusto
ServiceFabricOperationalEvent
| join kind=leftouter ServiceFabricEvent on EventId
| where EventId != 17523
| summarize Count = count() by Timestamp = bin(TimeGenerated, 1h), strcat(tostring(EventId), " - ", case(EventName != "", EventName, "Unknown"))
| render timechart 
```

Haal de Service Fabric operationele gebeurtenissen samengevoegd met de specifieke service en het knooppunt:

```kusto
ServiceFabricOperationalEvent
| where ApplicationName  != "" and ServiceName != ""
| summarize AggregatedValue = count() by ApplicationName, ServiceName, Computer 
```

Het aantal gebeurtenissen van de Service Fabric op gebeurtenis-id weergeven / EventName met behulp van een query meerdere bronnen:

```kusto
app('PlunkoServiceFabricCluster').traces
| where customDimensions.ProviderName == 'Microsoft-ServiceFabric'
| extend EventId = toint(customDimensions.EventId), TaskName = tostring(customDimensions.TaskName)
| where EventId != 17523
| join kind=leftouter ServiceFabricEvent on EventId
| extend EventName = case(EventName != '', EventName, 'Undocumented')
| summarize ["Event Count"]= count() by bin(timestamp, 30m), EventName = strcat(tostring(EventId), " - ", EventName)
| render timechart
```

### <a name="view-service-fabric-application-events"></a>Service Fabric-toepassing-gebeurtenissen weergeven

U kunt gebeurtenissen voor de betrouwbare services en reliable actors bekijken die zijn geïmplementeerd op het cluster.  Klik op de pagina Service Fabric-analyse op de grafiek voor **toepassingsgebeurtenissen**.

Voer de volgende query uit om gebeurtenissen van uw servicetoepassingen van betrouwbare weer te geven:
```kusto
ServiceFabricReliableServiceEvent
| sort by TimeGenerated desc
```

U kunt verschillende gebeurtenissen voor zien wanneer de service runasync is gestart en dit gebeurt gewoonlijk op implementaties en upgrades voltooid.

![Service Fabric Reliable Services-oplossing](media/service-fabric-tutorial-monitor-cluster/oms-reliable-services-events-selection.png)

U kunt ook gebeurtenissen vinden voor de betrouwbare service met de servicenaam == ' fabric: / Watchdog/WatchdogService ':

```kusto
ServiceFabricReliableServiceEvent
| where ServiceName == "fabric:/Watchdog/WatchdogService"
| project TimeGenerated, EventMessage
| order by TimeGenerated desc  
```
 
Reliable actor-gebeurtenissen kunnen op dezelfde manier worden weergegeven:

```kusto
ServiceFabricReliableActorEvent
| sort by TimeGenerated desc
```
Als u wilt meer gedetailleerde gegevens van gebeurtenissen voor betrouwbare actoren configureert, kunt u de `scheduledTransferKeywordFilter` in de configuratie voor de diagnostische extensie in de clustersjabloon. Details van de waarden voor deze zijn in de [reliable actors-gebeurtenissen verwijzing](service-fabric-reliable-actors-diagnostics.md#keywords).

```json
"EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
```

## <a name="view-performance-counters-with-azure-monitor-logs"></a>De prestatiemeteritems weergeven met Azure Monitor-Logboeken
Als u wilt weergeven van prestatiemeteritems, gaat u naar de [Azure-portal](https://portal.azure.com) en de resourcegroep waarin u de Service Fabric-analyse-oplossing hebt gemaakt. 

Selecteer de resource **ServiceFabric(mysfomsworkspace)**, klikt u vervolgens **Log Analytics-werkruimte**, en vervolgens **geavanceerde instellingen**.

Klik op **gegevens**, klikt u vervolgens op **Windows-prestatiemeteritems**. Er is een lijst van standaard-prestatiemeteritems waarop die u kiezen kunt om in te schakelen en u kunt het interval voor de verzameling te instellen. U kunt ook toevoegen [aanvullende prestatiemeteritems](service-fabric-diagnostics-event-generation-perf.md) te verzamelen. De juiste indeling wordt verwezen in dit [artikel](/windows/desktop/PerfCtrs/specifying-a-counter-path). Klik op **opslaan**, klikt u vervolgens op **OK**.

Sluit de blade geavanceerde instellingen en selecteer **werkruimte overzicht** onder de **algemene** kop. Voor elk van de oplossingen die zijn ingeschakeld, er is een grafische tegel, met inbegrip van een Service Fabric. Klik op de **Service Fabric** graph om door te gaan naar de Service Fabric-analyse-oplossing.

Er zijn grafische tegels voor operationele kanaal en betrouwbare services-gebeurtenissen. De grafische weergave van de gegevens die voor de items die u hebt geselecteerd worden weergegeven onder **metrische knooppuntgegevens**. 

Selecteer de **Container metriek** grafiek om meer details te bekijken. U kunt ook een query op gegevens van prestatiemeteritems op soortgelijke wijze aan Clustergebeurtenissen en filtert u op de knooppunten, naam van het prestatiemeteritem voor prestaties en waarden met behulp van de Kusto-query-taal.

## <a name="query-the-eventstore-service"></a>Query uitvoeren op de service EventStore
De [EventStore service](service-fabric-diagnostics-eventstore.md) biedt een manier om te beoordelen wat de status van uw cluster of de werkbelastingen op een bepaald tijdstip. De EventStore is een stateful Service Fabric-service die wordt onderhouden door gebeurtenissen uit het cluster. De gebeurtenissen worden weergegeven via de [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), REST en API's. Het cluster van EventStore query's direct voor diagnostische gegevens over een entiteit in uw cluster om te zien van een volledige lijst van gebeurtenissen die beschikbaar zijn in de EventStore, Zie [Service Fabric-gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md).

De APIs EventStore kunnen worden opgevraagd programmatisch met behulp van de [Service Fabric-clientbibliotheek](/dotnet/api/overview/azure/service-fabric?view=azure-dotnet#client-library).

Hier volgt een van de voorbeeldaanvraag voor alle gebeurtenissen tussen 2018-cluster-04-03T18:00:00Z en 2018-04-04T18:00:00Z, via de functie GetClusterEventListAsync.

```csharp
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl, settings);

var clstrEvents = sfhttpClient.EventsStore.GetClusterEventListAsync(
    "2018-04-03T18:00:00Z",
    "2018-04-04T18:00:00Z")
    .GetAwaiter()
    .GetResult()
    .ToList();
```

Hier volgt een voorbeeld dat query's voor de clusterstatus en alle knooppunt-gebeurtenissen in September 2018 en ze afgedrukt.

```csharp
const int timeoutSecs = 60;
var clusterUrl = new Uri(@"http://localhost:19080"); // This example is for a Local cluster
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl);

var clusterHealth = sfhttpClient.Cluster.GetClusterHealthAsync().GetAwaiter().GetResult();
Console.WriteLine("Cluster Health: {0}", clusterHealth.AggregatedHealthState.Value.ToString());


Console.WriteLine("Querying for node events...");
var nodesEvents = sfhttpClient.EventsStore.GetNodesEventListAsync(
    "2018-09-01T00:00:00Z",
    "2018-09-30T23:59:59Z",
    timeoutSecs,
    "NodeDown,NodeUp")
    .GetAwaiter()
    .GetResult()
    .ToList();
Console.WriteLine("Result Count: {0}", nodesEvents.Count());

foreach (var nodeEvent in nodesEvents)
{
    Console.Write("Node event happened at {0}, Node name: {1} ", nodeEvent.TimeStamp, nodeEvent.NodeName);
    if (nodeEvent is NodeDownEvent)
    {
        var nodeDownEvent = nodeEvent as NodeDownEvent;
        Console.WriteLine("(Node is down, and it was last up at {0})", nodeDownEvent.LastNodeUpAt);
    }
    else if (nodeEvent is NodeUpEvent)
    {
        var nodeUpEvent = nodeEvent as NodeUpEvent;
        Console.WriteLine("(Node is up, and it was last down at {0})", nodeUpEvent.LastNodeDownAt);
    }
}
```


## <a name="monitor-cluster-health"></a>Clusterstatus controleren
Service Fabric introduceert een [statusmodel](service-fabric-health-introduction.md) met health-entiteiten in welke onderdelen van het systeem en watchdogs kunt rapport lokale voorwaarden die ze bewaken. De [health store](service-fabric-health-introduction.md#health-store) alle health-gegevens om te bepalen of entiteiten in orde zijn.

Het cluster wordt automatisch gevuld met statusrapporten die zijn verzonden door de onderdelen van het systeem. Meer informatie [systeemstatusrapporten gebruiken om op te lossen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Service Fabric biedt statusquery's voor elk van de ondersteunde [Entiteitstypen](service-fabric-health-introduction.md#health-entities-and-hierarchy). Ze kunnen worden geopend via de API, met behulp van methoden op [FabricClient.HealthManager](/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell-cmdlets en REST. Deze query's retourneren statusinformatie over de entiteit: de geaggregeerde status, entiteit health-gebeurtenissen, statussen onderliggende (indien van toepassing), evaluaties met slechte status (als de entiteit niet in orde is) en kinderen health statistieken (wanneer van toepassing).

### <a name="get-cluster-health"></a>Clusterstatus ophalen
De [cmdlet Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) geeft als resultaat de status van de cluster-entiteit en bevat de statussen van toepassingen en knooppunten (onderliggende items van het cluster).  Eerst verbinding maken met het cluster met behulp van de [cmdlet Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps).

De status van het cluster is 11 knooppunten, het system-toepassing en de fabric: / Voting geconfigureerd zoals wordt beschreven.

Het volgende voorbeeld wordt de clusterstatus met behulp van het statusbeleid standaard. De 11 knooppunten in orde zijn, maar de status van de cluster samengevoegd is fout omdat de fabric: / Voting toepassing er een fout. Houd er rekening mee hoe de evaluaties met slechte status informatie bieden over de voorwaarden die de geaggregeerde status wordt geactiveerd.

```powershell
Get-ServiceFabricClusterHealth

AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          100% (1/1) applications are unhealthy. The evaluation tolerates 0% unhealthy applications.
                          
                          Application 'fabric:/Voting' is in Error.
                          
                            33% (1/3) deployed applications are unhealthy. The evaluation tolerates 0% unhealthy deployed applications.
                          
                            Deployed application on node '_nt2vm_3' is in Error.
                          
                                50% (1/2) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '8723eb73-9b83-406b-9de3-172142ba15f3' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376195593305'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                          
NodeHealthStates        : 
                          NodeName              : _nt2vm_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_4
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt2vm_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt2vm_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt2vm_0
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_0
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt3vm_0
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt2vm_4
                          AggregatedHealthState : Ok
                          
ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok
                          
                          ApplicationName       : fabric:/Voting
                          AggregatedHealthState : Error
                          
HealthEvents            : None
HealthStatistics        : 
                          Node                  : 11 Ok, 0 Warning, 0 Error
                          Replica               : 4 Ok, 0 Warning, 0 Error
                          Partition             : 2 Ok, 0 Warning, 0 Error
                          Service               : 2 Ok, 0 Warning, 0 Error
                          DeployedServicePackage : 3 Ok, 1 Warning, 1 Error
                          DeployedApplication   : 1 Ok, 1 Warning, 1 Error
                          Application           : 0 Ok, 0 Warning, 1 Error
```

Het volgende voorbeeld wordt de status van het cluster met behulp van een aangepaste toepassing-beleid. Deze filtert de resultaten om alleen de toepassingen en knooppunten in een fout of waarschuwing. Er zijn geen knooppunten worden in dit voorbeeld geretourneerd, omdat ze allemaal in orde zijn. Alleen de fabric: / Voting toepassing rekening wordt gehouden met het filter toepassingen. Omdat het aangepaste beleid bepaalt u rekening moet houden waarschuwingen als fouten voor de fabric: / Voting toepassing, de toepassing, zoals in de fout wordt geëvalueerd en dus is het cluster.

```powershell
$appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/Voting"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics

AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          100% (1/1) applications are unhealthy. The evaluation tolerates 0% unhealthy applications.
                          
                          Application 'fabric:/Voting' is in Error.
                          
                            100% (5/5) deployed applications are unhealthy. The evaluation tolerates 0% unhealthy deployed applications.
                          
                            Deployed application on node '_nt2vm_3' is in Error.
                          
                                50% (1/2) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '8723eb73-9b83-406b-9de3-172142ba15f3' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376195593305'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                            Deployed application on node '_nt2vm_2' is in Error.
                          
                                50% (1/2) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '2466f2f9-d5fd-410c-a6a4-5b1e00630cca' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376486201388'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                            Deployed application on node '_nt2vm_4' is in Error.
                          
                                100% (1/1) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '5faa5201-eede-400a-865f-07f7f886aa32' is in Error.
                          
                                    'System.Hosting' reported Warning for property 'CodePackageActivation:Code:SetupEntryPoint:131959376207396204'. The evaluation treats 
                          Warning as Error.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                            Deployed application on node '_nt2vm_0' is in Error.
                          
                                100% (1/1) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '204f1783-f774-4f3a-b371-d9983afaf059' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959375885791093'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                            Deployed application on node '_nt3vm_0' is in Error.
                          
                                50% (1/2) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '2533ae95-2d2a-4f8b-beef-41e13e4c0081' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376108346272'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1                         
                          
NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/Voting
                          AggregatedHealthState : Error
                          
HealthEvents            : None
```

### <a name="get-node-health"></a>Knooppuntstatus ophalen
De [cmdlet Get-ServiceFabricNodeHealth](/powershell/module/servicefabric/get-servicefabricnodehealth) geeft als resultaat de status van een entiteit knooppunt en de health-gebeurtenissen gemeld op het knooppunt bevat. Eerst verbinding met het cluster met behulp van de [cmdlet Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps). Het volgende voorbeeld wordt de status van een specifiek knooppunt met behulp van standaard statusbeleid:

```powershell
Get-ServiceFabricNodeHealth _nt1vm_3
```

Het volgende voorbeeld wordt de status van alle knooppunten in het cluster:
```powershell
Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize
```

### <a name="get-system-service-health"></a>Systeem-servicestatus ophalen 

Haal de cumulatieve status van de systeemservices:

```powershell
Get-ServiceFabricService -ApplicationName fabric:/System | Get-ServiceFabricServiceHealth | select ServiceName, AggregatedHealthState | ft -AutoSize
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Service Fabric-gebeurtenissen weergeven
> * Query EventStore APIs voor Clustergebeurtenissen
> * Infrastructuur/verzamelen prestatiemeteritems bewaken
> * Cluster-statusrapporten weergeven

Ga vervolgens verder naar de volgende zelfstudie voor meer informatie over het schalen van een cluster.
> [!div class="nextstepaction"]
> [Een cluster schalen](service-fabric-tutorial-scale-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json