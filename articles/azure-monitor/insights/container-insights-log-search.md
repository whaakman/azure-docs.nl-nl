---
title: Hoe u Logboeken voor query's van Azure Monitor voor containers | Microsoft Docs
description: Azure Monitor voor containers metrische gegevens en logboekbestanden gegevens verzameld en in dit artikel beschrijft de records en bevat voorbeeldquery's.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2019
ms.author: magoedte
ms.openlocfilehash: 66fc55d8c3dbb8487d1e796d5f30b08a94f717f6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60494764"
---
# <a name="how-to-query-logs-from-azure-monitor-for-containers"></a>Hoe u Logboeken voor query's van Azure Monitor voor containers
Azure Monitor voor containers verzamelt metrische gegevens voor prestaties, inventarisgegevens en informatie over de status van containerhosts en containers, en deze doorstuurt naar de Log Analytics-werkruimte in Azure Monitor. Gegevens worden verzameld om de drie minuten. Deze gegevens zijn beschikbaar voor [query](../../azure-monitor/log-query/log-query-overview.md) in Azure Monitor. U kunt deze gegevens toepassen op scenario's met migratieplanning, analyse, detectie en het oplossen van prestaties op aanvraag.

## <a name="container-records"></a>Container-records

Voorbeelden van records die door Azure Monitor worden verzameld voor containers en de gegevenstypen die worden weergegeven in de resultaten van de logboekzoekopdracht worden weergegeven in de volgende tabel:

| Gegevenstype | Het gegevenstype in zoeken in Logboeken | Velden |
| --- | --- | --- |
| Prestaties voor hosts en -containers | `Perf` | Computer, ObjectName, CounterName &#40;% processortijd, schijf leest MB, schijf schrijft MB, gebruik van geheugen (MB), netwerk ontvangen Bytes, netwerk verzenden Bytes, Processor gebruik seconde, netwerk&#41;, CounterValue, TimeGenerated, itempad, SourceSystem |
| Container-inventaris | `ContainerInventory` | TimeGenerated, de Computer, de containernaam, ContainerHostname, afbeelding, ImageTag, ContainerState, ExitCode, EnvironmentVar, opdracht, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Container-logboek | `ContainerLog` | TimeGenerated, Computer, afbeeldings-ID, containernaam van de, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Inventaris van containerknooppunten | `ContainerNodeInventory`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Inventaris van schillen in een Kubernetes-cluster | `KubePodInventory` | TimeGenerated, Computer, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, servicenaam, ControllerKind, controllernaam, ContainerStatus,  ContainerStatusReason, ContainerID, ContainerName, naam, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Inventarisatie van een deel van de knooppunten van een Kubernetes-cluster | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Kubernetes-gebeurtenissen | `KubeEvents` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, bericht, SourceSystem | 
| Services in het Kubernetes-cluster | `KubeServices` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Metrische gegevens voor prestaties voor een deel van de knooppunten van het Kubernetes-cluster | Perf &#124; waarbij ObjectName == "K8SNode" | Computer, ObjectName, CounterName &#40;cpuAllocatableBytes, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes, memoryRssBytes, cpuUsageNanoCores, memoryWorkingsetBytes, restartTimeEpoch&#41;, CounterValue, TimeGenerated, itempad, SourceSystem | 
| Metrische gegevens voor prestaties voor containers die deel uitmaken van het Kubernetes-cluster | Perf &#124; waarbij ObjectName == "K8SContainer" | CounterName &#40; cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryWorkingSetBytes, restartTimeEpoch, cpuUsageNanoCores, memoryRssBytes&#41;, CounterValue, TimeGenerated, itempad, SourceSystem | 
| InsightsMetrics | De oorsprong, SourceSystem, labels, TimeGenerated, typt, computer, de naam, Namespace, waarde |

## <a name="search-logs-to-analyze-data"></a>Zoeken in Logboeken om gegevens te analyseren
Logboeken in Azure Monitor kunt u zoeken naar bestedingstrends en een diagnose stellen van knelpunten, prognose, of het correleren gegevens die u kan helpen te bepalen of de huidige configuratie van het cluster optimaal presteert. Vooraf gedefinieerde zoekopdrachten in Logboeken vindt u onmiddellijk kunt beginnen met of om aan te passen als u wilt de gegevens van de manier waarop die u wilt retourneren. 

U kunt uitvoeren interactieve analyses van gegevens in de werkruimte selecteren de **weergave Kubernetes gebeurtenislogboeken** of **containerlogboeken bekijken** optie in het voorbeeldvenster. De **zoeken in logboeken** pagina wordt weergegeven aan de rechterkant van de Azure portal-pagina die u al had geopend.

![Gegevens analyseren in Log Analytics](./media/container-insights-analyze/container-health-log-search-example.png)   

De uitvoer van de container-logboeken die wordt doorgestuurd naar uw werkruimte zijn STDOUT en STDERR. Omdat Azure Monitor wordt bewaakt door Azure beheerde Kubernetes (AKS), Kube-systeem niet vandaag verzameld vanwege het grote aantal gegenereerde gegevens. 

### <a name="example-log-search-queries"></a>Voorbeeld van de logboekbestanden zoekquery 's
Vaak is het handig om te maken van query's die beginnen met een voorbeeld of twee en vervolgens kan aanpassen aan uw behoeften. Om te helpen meer geavanceerde query's, kunt u experimenteren met de volgende voorbeeldquery's:

| Query’s uitvoeren | Description | 
|-------|-------------|
| ContainerInventory<br> &#124;Computer, de naam, afbeelding, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime project<br> &#124;tabel weergeven | Lijst met alle van de container levenscyclus van gegevens| 
| KubeEvents_CL<br> &#124;waar not(isempty(Namespace_s))<br> &#124;sorteren op TimeGenerated desc<br> &#124;tabel weergeven | Kubernetes-gebeurtenissen|
| ContainerImageInventory<br> &#124;summarize AggregatedValue = count() by afbeelding, ImageTag, actief | Voorraad | 
| **Selecteer de optie lijndiagram weergeven**:<br> Prestaties<br> &#124;waarbij ObjectName == "K8SContainer" en CounterName == "cpuUsageNanoCores" &#124; AvgCPUUsageNanoCores samenvatten avg(CounterValue) door bin (TimeGenerated, 30 min.), InstanceName = | Container CPU | 
| **Selecteer de optie lijndiagram weergeven**:<br> Prestaties<br> &#124;waarbij ObjectName == "K8SContainer" en CounterName == "memoryRssBytes" &#124; AvgUsedRssMemoryBytes samenvatten avg(CounterValue) door bin (TimeGenerated, 30 min.), InstanceName = | Container-geheugen |

## <a name="next-steps"></a>Volgende stappen
Azure Monitor voor containers bevat geen een vooraf gedefinieerde set waarschuwingen. Controleer de [prestatiewaarschuwingen maken met Azure Monitor voor containers](container-insights-alerts.md) voor meer informatie over aanbevolen waarschuwingen maken voor gebruik van hoge CPU en het geheugen voor de ondersteuning van uw DevOps of operationele processen en procedures. 