---
title: Analyseren van Log Analytics-gegevens in Azure Monitor | Microsoft Docs
description: U moet een zoeken in Logboeken om op te halen van alle gegevens van Log Analytics.  In dit artikel wordt beschreven hoe u met nieuwe log searches in Log Analytics worden gebruikt en concepten die u nodig hebt om te begrijpen voordat deze is gemaakt.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: bwren
ms.openlocfilehash: 47abb191383bd1ec1000c9fd1e0803a7d900c9bf
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54117636"
---
# <a name="analyze-log-analytics-data-in-azure-monitor"></a>Analyseren van Log Analytics-gegevens in Azure Monitor

Logboekgegevens die zijn verzameld door Azure Monitor is opgeslagen in een Log Analytics-werkruimte die is gebaseerd op [Azure Data Explorer](/azure/data-explorer). Het verzamelt telemetrie van een groot aantal bronnen en maakt gebruik van de [querytaal van Data Explorer](/azure/kusto/query) ophalen en analyseren van gegevens.

> [!NOTE]
> Log Analytics is eerder beschouwd als een eigen service in Azure. Het is nu beschouwd als een onderdeel van Azure Monitor en richt zich op de opslag en analyse van gegevens met behulp van de querytaal. Functies die werden beschouwd als onderdeel van Log Analytics, zoals Windows en Linux-agents voor het verzamelen van gegevens, weergaven om bestaande gegevens en waarschuwingen te visualiseren om proactief te waarschuwen u van problemen, niet zijn gewijzigd, maar worden nu beschouwd als onderdeel van Azure Monitor.



## <a name="log-queries"></a>Logboeken-query 's

U moet een logboekquery voor het ophalen van gegevens van Log Analytics.  Of u bent [analyseren van gegevens in de portal](../../azure-monitor/log-query/portals.md), [configureren van een waarschuwingsregel](../../azure-monitor/platform/alerts-metric.md) om te worden geïnformeerd over een bepaalde voorwaarde of bij het ophalen van gegevens met de [Log Analytics API](https://dev.loganalytics.io/), u wordt een query gebruiken om op te geven van de gegevens die u wilt.  In dit artikel wordt beschreven hoe de logboeken-query's worden gebruikt in Log Analytics en concepten die u weten moet voordat u een maakt.



## <a name="where-log-queries-are-used"></a>Wanneer logboeken-query's worden gebruikt

De verschillende manieren die u query's in Log Analytics gebruikt omvatten het volgende:

- **Portals.** U kunt uitvoeren interactieve analyses van logboekgegevens in de [Azure-portal](../../azure-monitor/log-query/portals.md).  Hiermee kunt u uw query bewerken en analyseer de resultaten in verschillende indelingen en visualisaties.  
- **Regels voor waarschuwingen.** [Waarschuwingsregels](../../azure-monitor/platform/alerts-overview.md) proactief problemen van gegevens in uw werkruimte te identificeren.  De waarschuwingsregel is gebaseerd op een logboekzoekopdracht die automatisch met regelmatige tussenpozen wordt uitgevoerd.  De resultaten worden gecontroleerd om te bepalen of een waarschuwing moet worden gemaakt.
- **Dashboards.** U kunt de resultaten van elke query in vastmaken een [Azure-dashboard](../../azure-monitor/platform/dashboards.md) waarmee u logboek- en metrische gegevens bij elkaar te visualiseren en (optioneel) delen met andere Azure-gebruikers. 
- **Weergaven.**  Kunt u visualisaties van gegevens moeten worden opgenomen in de Gebruikersdashboards met [Weergaveontwerper](../../azure-monitor/platform/view-designer.md).  Logboeken-query's leveren de gegevens die worden gebruikt door [tegels](../../azure-monitor/platform/view-designer-tiles.md) en [visualisatie delen](../../azure-monitor/platform/view-designer-parts.md) in elke weergave.  
- **Exporteren.**  Wanneer u gegevens importeren uit Log Analytics-werkruimte in Excel of [Power BI](../../azure-monitor/platform/powerbi.md), maakt u een logboekquery voor het definiëren van de gegevens te exporteren.
- **PowerShell.** U kunt een PowerShell-script uitvoeren vanaf een opdrachtregel of een Azure Automation-runbook die gebruikmaakt van [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0) voor het ophalen van gegevens van Log Analytics.  Deze cmdlet is vereist voor een query om te bepalen van de gegevens moeten worden opgehaald.
- **Log Analytics-API.**  De [Log Analytics search API melden](../../azure-monitor/platform/alerts-overview.md) kan een client REST-API om op te halen van logboekgegevens uit de werkruimte.  De API-aanvraag bevat een query die wordt uitgevoerd op basis van Log Analytics om te bepalen van de gegevens moeten worden opgehaald.

![Zoekopdrachten in Logboeken](media/log-query-overview/queries-overview.png)

## <a name="write-a-query"></a>Een query schrijven
Meld u Analytics gebruikt [een versie van de querytaal van Data Explorer](../../azure-monitor/log-query/get-started-queries.md) ophalen en analyseren van logboekgegevens op verschillende manieren.  U doorgaans begint met eenvoudige query's en vervolgens de voortgang voor het gebruik van meer geavanceerde functies zoals de vereisten van uw steeds complexer.

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

## <a name="how-log-analytics-data-is-organized"></a>De rangschikking van Log Analytics-gegevens
Wanneer u een query bouwen, start u door te bepalen welke tabellen bevatten de gegevens die u zoekt. Verschillende soorten gegevens zijn onderverdeeld in specifieke tabellen in elk [Log Analytics-werkruimte](../../azure-monitor/learn/quick-create-workspace.md).  Documentatie voor verschillende gegevensbronnen bevat de naam van het gegevenstype dat wordt gemaakt en een beschrijving van elk van de bijbehorende eigenschappen.  Veel query's worden alleen gegevens uit één tabel vereist, maar andere gebruiken misschien een verscheidenheid aan opties om op te nemen van gegevens uit meerdere tabellen.

Terwijl [Application Insights](../../azure-monitor/app/app-insights-overview.md) gegevens van de toepassing stores, zoals aanvragen, uitzonderingen, traceringen en gebruik in Log Analytics, deze gegevens worden opgeslagen in een andere partitie dan de andere logboekgegevens. U dezelfde querytaal gebruiken voor toegang tot deze gegevens, maar moet gebruiken de [Application Insights-console](../../azure-monitor/app/analytics.md) of [Application Insights REST-API](https://dev.applicationinsights.io/) om deze te openen. U kunt [cross-resources query's](../../azure-monitor/log-query/cross-workspace-query.md) om Application Insights-gegevens met andere gegevens in Log Analytics te combineren.


![Tabellen](media/log-query-overview/queries-tables.png)







## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [portals die u gebruiken om te maken en bewerken van zoekopdrachten](../../azure-monitor/log-query/portals.md).
- Bekijk een [zelfstudie over het schrijven van query's](../../azure-monitor/log-query/get-started-queries.md) met behulp van de nieuwe querytaal.
