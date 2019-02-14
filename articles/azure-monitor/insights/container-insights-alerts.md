---
title: Waarschuwingen over toepassingsprestaties maken met Azure Monitor voor containers | Microsoft Docs
description: Dit artikel wordt beschreven hoe u aangepaste Azure-waarschuwingen op basis van Logboeken-query's voor geheugen en CPU-gebruik van Azure Monitor voor containers kunt maken.
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
ms.date: 02/12/2019
ms.author: magoedte
ms.openlocfilehash: c275d50ab927895eca3aa018b71ab6989a4dde5a
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56238854"
---
# <a name="how-to-set-up-alerts-in-azure-monitor-for-container-performance-problems"></a>Over het instellen van waarschuwingen in Azure Monitor voor prestatieproblemen van container
Azure Monitor voor containers monitors de prestaties van containerwerkbelastingen geïmplementeerd in een Azure Container Instances of beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS). 

In dit artikel wordt beschreven hoe u mogelijk waarschuwingen bij het gebruik van processors en geheugenbronnen op de knooppunten van het cluster is groter dan de ingestelde drempel is gekomen.

## <a name="create-alert-on-cluster-cpu-or-memory-utilization"></a>Waarschuwing maken voor cluster CPU of geheugengebruik
Om u te waarschuwen wanneer CPU of geheugen gebruik hoge voor een cluster is, maakt u een waarschuwingsregel voor de meting van metrische gegevens die is gebaseerd op logboekzoekopdrachten logboeken-query's opgegeven. De query's vergelijken een datum/tijd aan de aanwezig zijn met de `now` operator en back-één uur uitvalt. Alle datums die zijn opgeslagen door Azure Monitor voor containers worden in UTC-notatie.  

Voordat u begint, als u niet bekend met waarschuwingen in Azure Monitor bent, Zie [overzicht van waarschuwingen in Microsoft Azure](../platform/alerts-overview.md). Zie voor meer informatie over waarschuwingen via Logboeken-query's, [waarschuwingen voor activiteitenlogboeken in Azure Monitor](../platform/alerts-unified-log.md)

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Monitor** in het linkervenster in Azure portal. Onder de **Insights** sectie, selecteer **Containers**.    
3. Uit de **bewaakt Clusters** tabblad, selecteert u een cluster in de lijst door te klikken op de naam van het cluster.
4. In het linkerdeelvenster onder de **bewaking** sectie, selecteer **logboeken** meldt zich gegevensfactorypagina gebruikt om te schrijven en uitvoeren van Azure Log Analytics-query's te openen van de Azure Monitor.
5. Op de **logboeken** pagina, klikt u op **+ nieuwe waarschuwingsregel**.
6. Onder de **voorwaarde** sectie, klikt u op de voorwaarde vooraf gedefinieerde aangepaste logboek **wanneer het aangepaste zoeken in Logboeken is <logic undefined>** . De **aangepaste zoekopdrachten in logboeken** signaaltype wordt automatisch geselecteerd voor ons, omdat we het maken van een waarschuwingsregel rechtstreeks vanuit de pagina van Azure Monitor logboeken gestart.  
7. Plak een van de query's hieronder in de **zoekquery** veld. De volgende query wordt het gemiddelde CPU-gebruik als een gemiddelde van lid knooppunten CPU-gebruik per minuut berekend.

    ```
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

    De volgende query wordt het gemiddelde geheugengebruik als een gemiddelde van geheugengebruik lidstaten-knooppunten per minuut berekend.

    ```
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

8. Configureer de waarschuwing met de volgende gegevens:

    a. Selecteer in de vervolgkeuzelijst **Gebaseerd op** de optie **Meting van metrische gegevens**. Een meting van metrische gegevens maakt een waarschuwing voor elk object in de query met een waarde die de opgegeven drempelwaarde overschrijdt.  
    b. Voor de **voorwaarde**, selecteer **groter is dan** en voer **75** als een initiële basislijnkopie **drempelwaarde** of voer een waarde die voldoet aan uw criteria.  
    c. Onder **Trigger waarschuwingen op basis van** sectie, selecteer **achtereenvolgende schendingen** en uit de vervolgkeuzelijst selecteren **groter is dan** Voer een waarde van **2**.  
    d. Onder **Evaluated op basis van** sectie, wijzigt u de **periode** waarde en 60 minuten. De regel wordt elke vijf minuten wordt uitgevoerd en records die zijn gemaakt in het afgelopen uur van de huidige tijd retourneren. Door een ruimere periode in te stellen wordt rekening gehouden met eventuele gegevenslatentie en wordt ervoor gezorgd dat de query gegevens retourneert zodat er geen fout-negatieve waarde wordt gegenereerd in geval de waarschuwing nooit wordt geactiveerd. 

9. Klik op **Gereed** om de waarschuwingsregel te voltooien.
10. Geef een naam op van de waarschuwing in de **naam waarschuwingsregel** veld. Geef een **beschrijving** met gedetailleerde informatie over specificaties voor de waarschuwing en selecteer een geschikte ernst van de opties die zijn opgegeven.
11. Als u de waarschuwingsregel direct na het maken wilt activeren, accepteert u de standaardwaarde voor **Regel inschakelen wanneer deze wordt gemaakt**.
12. Voor de laatste stap wordt u een bestaande selecteren of maak een nieuwe **actiegroep**, die garandeert dat telkens wanneer een waarschuwing wordt geactiveerd en kan worden gebruikt voor elke regel u definieert de dezelfde acties worden genomen. Configureren op basis van hoe uw IT-afdeling of DevOps operations incidenten beheert. 
13. Klik op **Waarschuwingsregel maken** om de waarschuwingsregel te voltooien. Deze wordt onmiddellijk uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

* Bekijk enkele van de [voorbeelden van aanmelden](container-insights-analyze.md#search-logs-to-analyze-data) meer informatie over de vooraf gedefinieerde query's of voorbeelden om te evalueren en te gebruiken of aanpassen voor andere scenario's met waarschuwingen. 
* Om door te gaan met het leren over het gebruik van Azure Monitor en controleren van andere aspecten van uw AKS-cluster [weergave Azure Kubernetes Service health](container-insights-analyze.md)
