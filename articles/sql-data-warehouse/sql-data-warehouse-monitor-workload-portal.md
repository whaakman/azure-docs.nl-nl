---
title: Workload monitor - Azure portal | Microsoft Docs
description: Azure SQL Data Warehouse met behulp van de Azure portal controleren
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 03/22/2019
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: 6c8ce090039e3d5cc85c86d920710294de2165f9
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58405780"
---
# <a name="monitor-workload---azure-portal"></a>Workload monitor - Azure portal

Dit artikel wordt beschreven hoe u uw workload controleren met de Azure-portal. Dit omvat het instellen van Azure Monitor-logboeken voor het onderzoeken van query's uitvoeren en werkbelasting trends met behulp van log analytics voor [Azure SQL Data Warehouse](https://azure.microsoft.com/blog/workload-insights-with-sql-data-warehouse-delivered-through-azure-monitor-diagnostic-logs-pass/).

## <a name="prerequisites"></a>Vereisten

- Azure-abonnement: Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
- Azure SQL Data Warehouse: We verzamelen logboeken voor een SQL datawarehouse. Als u een SQL datawarehouse ingericht hebt, raadpleegt u de instructies in [maken van een SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-tutorial).

## <a name="create-a-log-analytics-workspace"></a>Een Log Analytics-werkruimte maken

Navigeer naar de blade bladeren voor Log Analytics-werkruimten en een werkruimte maken 

![Log Analytics-werkruimten](media/sql-data-warehouse-monitor/log_analytics_workspaces.png)

![Analytics-werkruimte toevoegen](media/sql-data-warehouse-monitor/add_analytics_workspace.png)

![Analytics-werkruimte toevoegen](media/sql-data-warehouse-monitor/add_analytics_workspace_2.png)

Ga naar de volgende voor meer informatie over werkruimten [documentatie](https://docs.microsoft.com/azure/azure-monitor/platform/manage-access#create-a-workspace).

## <a name="turn-on-diagnostic-logs"></a>Logboeken met diagnostische gegevens inschakelen 

Diagnostische instellingen voor het verzenden van Logboeken van uw SQL datawarehouse configureren. Logboeken bestaan uit de weergaven van de telemetrie van uw datawarehouse gelijk is aan de meest gebruikte prestaties DMV's voor probleemoplossing voor SQL Data Warehouse. De volgende weergaven worden momenteel ondersteund:

- [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_dms_workers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_sql_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?view=aps-pdw-2016-au7)


![Logboeken met diagnostische gegevens inschakelen](media/sql-data-warehouse-monitor/enable_diagnostic_logs.png)

Logboeken kunnen worden verzonden naar Azure Storage, Stream Analytics of Log Analytics. Selecteer voor deze zelfstudie Log Analytics.

![Logboeken opgeven](media/sql-data-warehouse-monitor/specify_logs.png)

## <a name="run-queries-against-log-analytics"></a>Query's uitvoeren op Log Analytics

Navigeer naar uw Log Analytics-werkruimte waar u het volgende kunt doen:

- Logboeken met behulp van Logboeken-query's analyseren en query's voor hergebruik opslaan
- Query's voor hergebruik opslaan
- Waarschuwingen maken
- De queryresultaten vastmaken aan een dashboard

Voor meer informatie over de mogelijkheden van Logboeken-query's, gaat u naar de volgende [documentatie](https://docs.microsoft.com/azure/azure-monitor/log-query/query-language).

![Log Analytics-werkruimte editor](media/sql-data-warehouse-monitor/log_analytics_workspace_editor.png)



![Meld u Analytics-werkruimte query 's](media/sql-data-warehouse-monitor/log_analytics_workspace_queries.png)

## <a name="sample-log-queries"></a>Voorbeeld van Logboeken-query 's



```Kusto
//List all queries 
AzureDiagnostics
| where Category contains "ExecRequests"
| project TimeGenerated, StartTime_t, EndTime_t, Status_s, Command_s, ResourceClass_s, duration=datetime_diff('millisecond',EndTime_t, StartTime_t)
```
```Kusto
//Chart the most active resource classes
AzureDiagnostics
| where Category contains "ExecRequests"
| where Status_s == "Completed"
| summarize totalQueries = dcount(RequestId_s) by ResourceClass_s
| render barchart 
```
```Kusto
//Count of all queued queries
AzureDiagnostics
| where Category contains "waits" 
| where Type_s == "UserConcurrencyResourceType"
| summarize totalQueuedQueries = dcount(RequestId_s)
```
## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt ingesteld en geconfigureerd met Azure monitor-Logboeken, [aanpassen van de Azure-dashboards](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards) om te delen met uw team.
