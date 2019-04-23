---
title: Waarschuwingen over toepassingsprestaties maken met behulp van Azure Monitor voor containers | Microsoft Docs
description: In dit artikel wordt beschreven hoe u Azure-Monitor voor containers gebruiken om aangepaste waarschuwingen op basis van Logboeken-query's voor geheugen en CPU-gebruik te maken.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/01/2019
ms.author: magoedte
ms.openlocfilehash: ebe2c2b488e3d71597dd24f5504a14dd7ce6671e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59791538"
---
# <a name="how-to-set-up-alerts-for-performance-problems-in-azure-monitor-for-containers"></a>Over het instellen van waarschuwingen voor problemen met de prestaties in Azure Monitor voor containers
Azure Monitor voor containers bewaakt de prestaties van containerworkloads die zijn geïmplementeerd in Azure Container Instances of in het beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS).

In dit artikel wordt beschreven hoe u waarschuwingen voor de volgende situaties inschakelen:

* Bij gebruik van de CPU of geheugen op de knooppunten van een opgegeven drempelwaarde overschrijdt
* Bij gebruik van de CPU of geheugen op een willekeurige container binnen een domeincontroller is groter dan een ingestelde drempel is gekomen in vergelijking met een limiet die ingesteld op de bijbehorende resource
* *NotReady* status knooppunt wordt geteld
*  *Kan niet*, *in behandeling*, *onbekende*, *met*, of *geslaagd* pod-fase wordt geteld

Om u te waarschuwen voor hoge CPU- of geheugengebruik op clusterknooppunten, gebruikt u de query's die beschikbaar zijn om een waarschuwing voor metrische gegevens of een waarschuwing voor een meting van metrische gegevens te maken. Metrische waarschuwingen hebben lagere latentie dan waarschuwingen. Maar waarschuwingen bieden geavanceerde query's en meer verfijning. Query's een datum/tijd bij de huidige vergelijken met behulp van waarschuwingen voor activiteitenlogboeken de *nu* operator en back-één uur te gaan. (Met azure Monitor voor containers worden alle datums in de indeling in Coordinated Universal Time (UTC).)

Als u niet bekend met Azure Monitor-waarschuwingen bent, Zie [overzicht van waarschuwingen in Microsoft Azure](../platform/alerts-overview.md) voordat u begint. Zie voor meer informatie over waarschuwingen die gebruikmaken van Logboeken-query's, [waarschuwingen voor activiteitenlogboeken in Azure Monitor](../platform/alerts-unified-log.md). Zie voor meer informatie over metrische waarschuwingen, [metrische waarschuwingen in Azure Monitor](../platform/alerts-metric-overview.md).

## <a name="resource-utilization-log-search-queries"></a>Resource-gebruik log zoekquery 's
De query's in deze sectie ondersteuning voor elk scenario waarschuwingen. Ze worden gebruikt in stap 7 van de [waarschuwing maken](#create-an-alert-rule) sectie van dit artikel.

De volgende query wordt de gemiddelde CPU-gebruik als een gemiddelde van lid knooppunten CPU-gebruik per minuut berekend.  

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'cpuCapacityNanoCores';
let usageCounterName = 'cpuUsageNanoCores';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
// cluster filter would go here if multiple clusters are reporting to the same Log Analytics workspace
| distinct ClusterName, Computer
| join hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime
  | where TimeGenerated >= startDateTime
  | where ObjectName == 'K8SNode'
  | where CounterName == capacityCounterName
  | summarize LimitValue = max(CounterValue) by Computer, CounterName, bin(TimeGenerated, trendBinSize)
  | project Computer, CapacityStartTime = TimeGenerated, CapacityEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer
| join kind=inner hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime + trendBinSize
  | where TimeGenerated >= startDateTime - trendBinSize
  | where ObjectName == 'K8SNode'
  | where CounterName == usageCounterName
  | project Computer, UsageValue = CounterValue, TimeGenerated
) on Computer
| where TimeGenerated >= CapacityStartTime and TimeGenerated < CapacityEndTime
| project ClusterName, Computer, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize), ClusterName
```

De volgende query worden gemiddelde geheugengebruik als een gemiddelde van geheugengebruik lidstaten-knooppunten per minuut berekend.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'memoryCapacityBytes';
let usageCounterName = 'memoryRssBytes';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
// cluster filter would go here if multiple clusters are reporting to the same Log Analytics workspace
| distinct ClusterName, Computer
| join hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime
  | where TimeGenerated >= startDateTime
  | where ObjectName == 'K8SNode'
  | where CounterName == capacityCounterName
  | summarize LimitValue = max(CounterValue) by Computer, CounterName, bin(TimeGenerated, trendBinSize)
  | project Computer, CapacityStartTime = TimeGenerated, CapacityEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer
| join kind=inner hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime + trendBinSize
  | where TimeGenerated >= startDateTime - trendBinSize
  | where ObjectName == 'K8SNode'
  | where CounterName == usageCounterName
  | project Computer, UsageValue = CounterValue, TimeGenerated
) on Computer
| where TimeGenerated >= CapacityStartTime and TimeGenerated < CapacityEndTime
| project ClusterName, Computer, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize), ClusterName
```
>[!IMPORTANT]
>De volgende query's gebruikt u de tijdelijke aanduiding voor waarden \<uw cluster-naam > en \<uw domeincontroller-naam > voor uw cluster en de domeincontroller. Bij het instellen van waarschuwingen, moet u deze vervangen door waarden die specifiek zijn voor uw omgeving.

De volgende query wordt het gemiddelde CPU-gebruik van alle containers in een domeincontroller als een gemiddelde van CPU-gebruik van elke containerinstantie in een controller voor elke minuut berekend. De meting is een percentage van de limiet ingesteld voor een container.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'cpuLimitNanoCores';
let usageCounterName = 'cpuUsageNanoCores';
let clusterName = '<your-cluster-name>';
let controllerName = '<your-controller-name>';
KubePodInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| where ClusterName == clusterName
| where ControllerName == controllerName
| extend InstanceName = strcat(ClusterId, '/', ContainerName),
         ContainerName = strcat(controllerName, '/', tostring(split(ContainerName, '/')[1]))
| distinct Computer, InstanceName, ContainerName
| join hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ObjectName == 'K8SContainer'
    | where CounterName == capacityCounterName
    | summarize LimitValue = max(CounterValue) by Computer, InstanceName, bin(TimeGenerated, trendBinSize)
    | project Computer, InstanceName, LimitStartTime = TimeGenerated, LimitEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer, InstanceName
| join kind=inner hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime + trendBinSize
    | where TimeGenerated >= startDateTime - trendBinSize
    | where ObjectName == 'K8SContainer'
    | where CounterName == usageCounterName
    | project Computer, InstanceName, UsageValue = CounterValue, TimeGenerated
) on Computer, InstanceName
| where TimeGenerated >= LimitStartTime and TimeGenerated < LimitEndTime
| project Computer, ContainerName, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize) , ContainerName
```

De volgende query wordt het gemiddelde geheugengebruik van alle containers in een domeincontroller als een gemiddelde van het geheugengebruik van elke containerinstantie in een controller voor elke minuut berekend. De meting is een percentage van de limiet ingesteld voor een container.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'memoryLimitBytes';
let usageCounterName = 'memoryRssBytes';
let clusterName = '<your-cluster-name>';
let controllerName = '<your-controller-name>';
KubePodInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| where ClusterName == clusterName
| where ControllerName == controllerName
| extend InstanceName = strcat(ClusterId, '/', ContainerName),
         ContainerName = strcat(controllerName, '/', tostring(split(ContainerName, '/')[1]))
| distinct Computer, InstanceName, ContainerName
| join hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ObjectName == 'K8SContainer'
    | where CounterName == capacityCounterName
    | summarize LimitValue = max(CounterValue) by Computer, InstanceName, bin(TimeGenerated, trendBinSize)
    | project Computer, InstanceName, LimitStartTime = TimeGenerated, LimitEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer, InstanceName
| join kind=inner hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime + trendBinSize
    | where TimeGenerated >= startDateTime - trendBinSize
    | where ObjectName == 'K8SContainer'
    | where CounterName == usageCounterName
    | project Computer, InstanceName, UsageValue = CounterValue, TimeGenerated
) on Computer, InstanceName
| where TimeGenerated >= LimitStartTime and TimeGenerated < LimitEndTime
| project Computer, ContainerName, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize) , ContainerName
```

De volgende query retourneert alle knooppunten en aantallen die de status van *gereed* en *NotReady*.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let clusterName = '<your-cluster-name>';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| distinct ClusterName, Computer, TimeGenerated
| summarize ClusterSnapshotCount = count() by bin(TimeGenerated, trendBinSize), ClusterName, Computer
| join hint.strategy=broadcast kind=inner (
    KubeNodeInventory
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | summarize TotalCount = count(), ReadyCount = sumif(1, Status contains ('Ready'))
                by ClusterName, Computer,  bin(TimeGenerated, trendBinSize)
    | extend NotReadyCount = TotalCount - ReadyCount
) on ClusterName, Computer, TimeGenerated
| project   TimeGenerated,
            ClusterName,
            Computer,
            ReadyCount = todouble(ReadyCount) / ClusterSnapshotCount,
            NotReadyCount = todouble(NotReadyCount) / ClusterSnapshotCount
| order by ClusterName asc, Computer asc, TimeGenerated desc
```
De volgende query retourneert pod fase telt gebaseerd op alle fasen: *Kan niet*, *in behandeling*, *onbekende*, *met*, of *geslaagd*.  

```kusto
let endDateTime = now();
    let startDateTime = ago(1h);
    let trendBinSize = 1m;
    let clusterName = '<your-cluster-name>';
    KubePodInventory
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ClusterName == clusterName
    | distinct ClusterName, TimeGenerated
    | summarize ClusterSnapshotCount = count() by bin(TimeGenerated, trendBinSize), ClusterName
    | join hint.strategy=broadcast (
        KubePodInventory
        | where TimeGenerated < endDateTime
        | where TimeGenerated >= startDateTime
        | distinct ClusterName, Computer, PodUid, TimeGenerated, PodStatus
        | summarize TotalCount = count(),
                    PendingCount = sumif(1, PodStatus =~ 'Pending'),
                    RunningCount = sumif(1, PodStatus =~ 'Running'),
                    SucceededCount = sumif(1, PodStatus =~ 'Succeeded'),
                    FailedCount = sumif(1, PodStatus =~ 'Failed')
                 by ClusterName, bin(TimeGenerated, trendBinSize)
    ) on ClusterName, TimeGenerated
    | extend UnknownCount = TotalCount - PendingCount - RunningCount - SucceededCount - FailedCount
    | project TimeGenerated,
              TotalCount = todouble(TotalCount) / ClusterSnapshotCount,
              PendingCount = todouble(PendingCount) / ClusterSnapshotCount,
              RunningCount = todouble(RunningCount) / ClusterSnapshotCount,
              SucceededCount = todouble(SucceededCount) / ClusterSnapshotCount,
              FailedCount = todouble(FailedCount) / ClusterSnapshotCount,
              UnknownCount = todouble(UnknownCount) / ClusterSnapshotCount
| summarize AggregatedValue = avg(PendingCount) by bin(TimeGenerated, trendBinSize)
```

>[!NOTE]
>Om te waarschuwen wanneer bepaalde fasen pod zoals *in behandeling*, *mislukt*, of *onbekende*, wijzigt u de laatste regel van de query. Bijvoorbeeld, om een waarschuwing te *FailedCount* gebruiken: <br/>`| summarize AggregatedValue = avg(FailedCount) by bin(TimeGenerated, trendBinSize)`

## <a name="create-an-alert-rule"></a>Een waarschuwingsregel maken
Volg deze stappen voor het maken van een waarschuwing in Azure Monitor met behulp van een van de regels in een logboekbestand zoeken die eerder is opgegeven.  

>[!NOTE]
>De volgende procedure om te maken van een waarschuwingsregel voor brongebruik door de container, u moet overschakelen naar een nieuw logboekbestand API waarschuwingen, zoals beschreven in [overschakelen API voorkeur voor logboekwaarschuwingen](../platform/alerts-log-api-switch.md).
>

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Monitor** in het deelvenster aan de linkerkant. Onder **Insights**, selecteer **Containers**.
3. Op de **bewaakt Clusters** tabblad, selecteert u een cluster in de lijst.
4. In het deelvenster aan de linkerkant onder **bewaking**, selecteer **logboeken** om de pagina Azure Monitor-logboeken te openen. U kunt deze pagina gebruiken om te schrijven en uitvoeren van Azure Log Analytics-query's.
5. Op de **logboeken** weergeeft, schakelt **+ nieuwe waarschuwingsregel**.
6. In de **voorwaarde** sectie, selecteer de **wanneer het aangepaste zoeken in Logboeken is \<logica niet gedefinieerd >** vooraf gedefinieerde aangepaste logboekvoorwaarde. De **aangepaste zoekopdrachten in logboeken** signaaltype wordt automatisch geselecteerd omdat we het maken van een waarschuwingsregel rechtstreeks vanuit de pagina Azure Monitor-Logboeken.  
7. Plak een van de [query's](#resource-utilization-log-search-queries) eerder hebt opgegeven in de **zoekquery** veld.
8. De waarschuwing als volgt configureren:

    1. Selecteer in de vervolgkeuzelijst **Gebaseerd op** de optie **Meting van metrische gegevens**. Een meting van metrische gegevens maakt een waarschuwing voor elk object in de query's met een waarde hoger dan de opgegeven drempelwaarde.
    1. Voor **voorwaarde**, selecteer **groter is dan**, en voer **75** als een initiële basislijnkopie **drempelwaarde**. Of voer een andere waarde die voldoet aan uw criteria voldoen.
    1. In de **Trigger waarschuwingen op basis van** sectie, selecteer **achtereenvolgende schendingen**. Selecteer in de vervolgkeuzelijst **groter is dan**, en voer **2**.
    1. Het configureren van een waarschuwing voor container CPU of geheugengebruik, onder **cumulatieve op**, selecteer **ContainerName**. 
    1. In de **Evaluated op basis van** sectie, stelt u de **periode** waarde die moet worden **60 minuten**. De regel worden uitgevoerd om de 5 minuten en records die zijn gemaakt in het afgelopen uur van de huidige tijd retourneren. De periode instellen op een breed venster accounts voor mogelijke gegevenslatentie van. Ook zorgt u ervoor dat de query gegevens retourneert om te voorkomen dat een false negatief waarin de waarschuwing wordt nooit geactiveerd.

9. Selecteer **gedaan** om uit te voeren van de waarschuwingsregel.
10. Voer een naam in de **naam waarschuwingsregel** veld. Geef een **beschrijving** die bevat gedetailleerde informatie over de waarschuwing. En selecteer een juiste ernstniveau van de opties die zijn opgegeven.
11. Als u wilt activeren onmiddellijk de waarschuwingsregel, accepteer de standaardwaarde voor **regel inschakelen bij het maken van**.
12. Selecteer een bestaande **actiegroep** of maak een nieuwe groep. Deze stap zorgt ervoor dat elke keer dat een waarschuwing wordt geactiveerd door de dezelfde acties worden uitgevoerd. Configureren op basis van hoe uw IT-afdeling of DevOps operationele team beheert incidenten.
13. Selecteer **waarschuwingsregel maken** om uit te voeren van de waarschuwingsregel. Deze wordt onmiddellijk uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

* Weergave [Meld u voorbeelden van](container-insights-analyze.md#search-logs-to-analyze-data) voor meer informatie over vooraf gedefinieerde query's en voorbeelden om te evalueren of aanpassen voor andere scenario's voor de waarschuwing.
* Zie voor meer informatie over Azure Monitor en andere aspecten van uw AKS-cluster bewaken, [weergave Azure Kubernetes Service health](container-insights-analyze.md).
