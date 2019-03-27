---
title: Analyseren van logboekgegevens in Azure Monitor | Microsoft Docs
description: U moet een logboekquery voor het ophalen van logboekgegevens van Azure Monitor.  In dit artikel wordt beschreven hoe u met nieuwe logboekbestanden query's worden gebruikt in Azure Monitor en concepten die u nodig hebt om te begrijpen voordat deze is gemaakt.
services: log-analytics
author: bwren
ms.service: log-analytics
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: bwren
ms.openlocfilehash: 6ed3a98282221d5ac148e88b6646bfaa4da768be
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58446431"
---
# <a name="analyze-log-data-in-azure-monitor"></a>Analyseren van logboekgegevens in Azure Monitor

Logboekgegevens die zijn verzameld door Azure Monitor is opgeslagen in een Log Analytics-werkruimte die is gebaseerd op [Azure Data Explorer](/azure/data-explorer). Het verzamelt telemetrie van een groot aantal bronnen en maakt gebruik van de [Kusto-querytaal](/azure/kusto/query) door Data Explorer gebruikt om te halen en analyseren van gegevens.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]


## <a name="log-queries"></a>Logboekquery's

U moet een logboekquery voor het ophalen van alle logboekgegevens van Azure Monitor.  Of u bent [analyseren van gegevens in de portal](portals.md), [configureren van een waarschuwingsregel](../platform/alerts-metric.md) om te worden geïnformeerd over een bepaalde voorwaarde of bij het ophalen van gegevens met de [logboeken-API van Azure Monitor](https://dev.loganalytics.io/) , gaat u een query maken om op te geven van de gegevens die u wilt.  In dit artikel wordt beschreven hoe de logboeken-query's worden gebruikt in Azure Monitor en concepten die u weten moet voordat u een maakt.



## <a name="where-log-queries-are-used"></a>Wanneer logboeken-query's worden gebruikt


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

De verschillende manieren die u query's in Azure Monitor gebruikt omvatten het volgende:


- **Portal.** U kunt uitvoeren interactieve analyses van logboekgegevens in de [Azure-portal](portals.md).  Hiermee kunt u uw query bewerken en analyseer de resultaten in verschillende indelingen en visualisaties.  
- **Regels voor waarschuwingen.** [Waarschuwingsregels](../platform/alerts-overview.md) proactief problemen van gegevens in uw werkruimte te identificeren.  De waarschuwingsregel is gebaseerd op een logboekzoekopdracht die automatisch met regelmatige tussenpozen wordt uitgevoerd.  De resultaten worden gecontroleerd om te bepalen of een waarschuwing moet worden gemaakt.
- **Dashboards.** U kunt de resultaten van elke query in vastmaken een [Azure-dashboard](../learn/tutorial-logs-dashboards.md) waarmee u logboek- en metrische gegevens bij elkaar te visualiseren en (optioneel) delen met andere Azure-gebruikers. 
- **Weergaven.**  Kunt u visualisaties van gegevens moeten worden opgenomen in de Gebruikersdashboards met [Weergaveontwerper](../platform/view-designer.md).  Logboeken-query's leveren de gegevens die worden gebruikt door [tegels](../platform/view-designer-tiles.md) en [visualisatie delen](../platform/view-designer-parts.md) in elke weergave.  

- **Exporteren.**  Wanneer u logboekgegevens van Azure Monitor in Excel importeert of [Power BI](../platform/powerbi.md), maakt u een logboekquery voor het definiëren van de gegevens te exporteren.
- **PowerShell.** U kunt een PowerShell-script uitvoeren vanaf een opdrachtregel of een Azure Automation-runbook die gebruikmaakt van [Get-AzOperationalInsightsSearchResults](/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults) om op te halen van logboekgegevens van Azure Monitor.  Deze cmdlet is vereist voor een query om te bepalen van de gegevens moeten worden opgehaald.
- **API voor Azure Monitor-Logboeken.**  De [logboeken-API van Azure Monitor](../platform/alerts-overview.md) kan een client REST-API om op te halen van logboekgegevens uit de werkruimte.  De API-aanvraag bevat een query die wordt uitgevoerd op Azure Monitor om te bepalen van de gegevens moeten worden opgehaald.

![Zoekopdrachten in Logboeken](media/log-query-overview/queries-overview.png)

## <a name="write-a-query"></a>Een query schrijven
Azure Monitor maakt gebruik van [een versie van de querytaal Kusto](get-started-queries.md) ophalen en analyseren van logboekgegevens op verschillende manieren.  U doorgaans begint met eenvoudige query's en vervolgens de voortgang voor het gebruik van meer geavanceerde functies zoals de vereisten van uw steeds complexer.

De basisstructuur van een query is een brontabel gevolgd door een reeks van operators, gescheiden door een sluisteken `|`.  U kunt koppelen samen meerdere operators voor het verfijnen van de gegevens en geavanceerde functies uitvoeren.

Stel bijvoorbeeld dat u wilt de top tien van computers met de meeste gebeurtenissen op foutniveau vinden in de afgelopen dag.

```Kusto
Event
| where (EventLevelName == "Error")
| where (TimeGenerated > ago(1days))
| summarize ErrorCount = count() by Computer
| top 10 by ErrorCount desc
```

Of misschien wilt u computers waarop een heartbeat in de laatste dag dit nog niet hebt gevonden.

```Kusto
Heartbeat
| where TimeGenerated > ago(7d)
| summarize max(TimeGenerated) by Computer
| where max_TimeGenerated < ago(1d)  
```

Hoe zit een lijndiagram met het processorgebruik voor elke computer in de afgelopen week?

```Kusto
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
| render timechart    
```

U kunt in deze snelle voorbeelden zien dat, ongeacht het soort gegevens waarmee u werkt met de structuur van de query vergelijkbaar is.  U kunt splitsen in afzonderlijke stappen waarbij de resulterende gegevens uit één opdracht via de pijplijn wordt verzonden naar de volgende opdracht.

U kunt ook gegevens op te vragen over Log Analytics-werkruimten binnen uw abonnement.

```Kusto
union Update, workspace("contoso-workspace").Update
| where TimeGenerated >= ago(1h)
| summarize dcount(Computer) by Classification 
```

## <a name="how-azure-monitor-log-data-is-organized"></a>De rangschikking van logboekgegevens van Azure Monitor
Wanneer u een query bouwen, start u door te bepalen welke tabellen bevatten de gegevens die u zoekt. Verschillende soorten gegevens zijn onderverdeeld in specifieke tabellen in elk [Log Analytics-werkruimte](../learn/quick-create-workspace.md).  Documentatie voor verschillende gegevensbronnen bevat de naam van het gegevenstype dat wordt gemaakt en een beschrijving van elk van de bijbehorende eigenschappen.  Veel query's worden alleen gegevens uit één tabel vereist, maar andere gebruiken misschien een verscheidenheid aan opties om op te nemen van gegevens uit meerdere tabellen.

Terwijl [Application Insights](../app/app-insights-overview.md) winkels toepassingsgegevens, zoals aanvragen, uitzonderingen, traceringen, en gebruik in Azure Monitor-Logboeken, deze gegevens worden opgeslagen in een andere partitie dan de andere logboekgegevens. U dezelfde querytaal gebruiken voor toegang tot deze gegevens, maar moet gebruiken de [Application Insights-console](../app/analytics.md) of [Application Insights REST-API](https://dev.applicationinsights.io/) om deze te openen. U kunt [cross-resources query's](../log-query/cross-workspace-query.md) om Application Insights-gegevens met andere logboekgegevens in Azure Monitor te combineren.


![Tabellen](media/log-query-overview/queries-tables.png)




## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het gebruik van [Log Analytics maken en bewerken van zoekopdrachten](../log-query/portals.md).
- Bekijk een [zelfstudie over het schrijven van query's](../log-query/get-started-queries.md) met behulp van de nieuwe querytaal.
