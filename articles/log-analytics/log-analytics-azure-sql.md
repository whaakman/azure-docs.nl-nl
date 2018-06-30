---
title: Azure SQL Analytics-oplossing in Log Analytics | Microsoft Docs
description: Azure SQL Analytics-oplossing kunt u uw Azure SQL-databases beheren
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: f57a47677f752a644975a25fa746d78bced5d766
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132932"
---
# <a name="monitor-azure-sql-databases-using-azure-sql-analytics-preview"></a>Azure SQL Databases bewaken via Azure SQL Analytics (Preview)

![Azure SQL Analytics symbool](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Azure SQL-Analytics is een oplossing voor het bewaken van de prestaties van Azure SQL-Databases op grote schaal op meerdere elastische pools en abonnementen bewaking cloud. Deze verzamelt en visualiseren van belangrijke maatstaven voor prestaties van Azure SQL Database met ingebouwde intelligentie voor het oplossen van problemen op de voorgrond prestaties. 

Metrische gegevens die u hebt verzameld met de oplossing gebruikt, kunt u aangepaste regels voor bewaking en waarschuwingen. De oplossing helpt u bij het identificeren van problemen bij elke laag van de stack van uw toepassing. Diagnostische Azure metrische gegevens samen met logboekanalyse weergaven wordt gebruikt om gegevens over uw Azure SQL-databases en elastische pools in een enkel Log Analytics-werkruimte te presenteren. Log Analytics, helpt u bij het verzamelen, correleren en gestructureerde en ongestructureerde gegevens visualiseren.

Deze preview-oplossing ondersteunt momenteel maximaal 150.000 Azure SQL-Databases en 5000 SQL elastische Pools per werkruimte.

Zie voor een praktische overzicht over het gebruik van Azure SQL Analytics-oplossing en typische gebruiksscenario's de ingesloten video:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Verbonden bronnen

Azure SQL-Analytics is een cloud bewaking oplossing ondersteunende streaming van diagnostische gegevens telemetrie voor Azure SQL-Databases en elastische pools. Als u agents verbinding maken met de Log Analytics-service, de oplossing biedt geen ondersteuning voor connectiviteit met Windows, Linux of SCOM-resources, Zie de onderstaande tabel voor compatibiliteit.

| Verbonden bron | Ondersteuning | Beschrijving |
| --- | --- | --- |
| **[Azure Diagnostics](log-analytics-azure-storage.md)** | **Ja** | Azure metrische gegevens en logboekbestanden gegevens worden verzonden met logboekanalyse rechtstreeks door Azure. |
| [Azure Storage-account](log-analytics-azure-storage.md) | Nee | Log Analytics biedt de gegevens niet lezen uit een opslagaccount. |
| [Windows-agents](log-analytics-windows-agent.md) | Nee | Directe Windows-agents worden niet gebruikt door de oplossing. |
| [Linux-agents](log-analytics-linux-agents.md) | Nee | Directe Linux-agents worden niet gebruikt door de oplossing. |
| [SCOM-beheergroep](log-analytics-om-agents.md) | Nee | Een rechtstreekse verbinding tussen de SCOM-agents met logboekanalyse wordt niet gebruikt door de oplossing. |

## <a name="configuration"></a>Configuratie

Voer de volgende stappen uit om toe te voegen van de Azure SQL Analytics-oplossing naar de werkruimte.

1. De Azure SQL Analytics-oplossing toevoegen aan uw werkruimte van [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview).
2. Klik in de Azure-portal op **+ maken van een resource**, zoekt u naar **Azure SQL Analytics**.  
    ![Controle en beheer](./media/log-analytics-azure-sql/monitoring-management.png)
3. Selecteer **Azure SQL Analytics (Preview)** uit de lijst
4. In de **Azure SQL Analytics (Preview)** gebied, klikt u op **maken**.  
    ![Maken](./media/log-analytics-azure-sql/portal-create.png)
5. In de **nieuwe oplossing maken** gebied maken van nieuwe of Selecteer een bestaande werkruimte die u wilt toevoegen, de oplossing en klik vervolgens op **maken**.  
    ![toevoegen aan werkruimte](./media/log-analytics-azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-and-elastic-pools-to-stream-diagnostics-telemetry"></a>Azure SQL-Databases en elastische Pools configureren om diagnostische telemetrie van stroom

Als u Azure SQL Analytics-oplossing hebt gemaakt in uw werkruimte, om de prestaties van Azure SQL-Databases en/of elastische Pools controleren moet u **Configureer elke** van Azure SQL Database en de elastische groep resource die u wilt Als u de diagnostische gegevens telemetrie naar de oplossing stroomsgewijs wilt bewaken.

- Azure Diagnostics inschakelen voor uw Azure SQL-databases en elastische pools en [configureren zodat ze hun gegevens verzenden naar logboekanalyse](../sql-database/sql-database-metrics-diag-logging.md).

### <a name="to-configure-multiple-azure-subscriptions"></a>Voor het configureren van meerdere Azure-abonnementen

Gebruik de PowerShell-script uit ter ondersteuning van meerdere abonnementen [inschakelen Azure resource metrische gegevens logboekregistratie met behulp van PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/). Geef de werkruimte-ID als parameter bij het uitvoeren van het script voor het verzenden van diagnostische gegevens van resources in een Azure-abonnement met een werkruimte in een andere Azure-abonnement.

**Voorbeeld**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a>De oplossing gebruiken

Wanneer u de oplossing voor uw werkruimte toevoegt, is de tegel Azure SQL Analytics toegevoegd aan uw werkruimte en wordt deze weergegeven in het overzicht. De tegel toont het aantal Azure SQL-databases en elastische pools SQL Azure die de oplossing is verbonden met.

![Azure SQL-Analytics tegel](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL analytische gegevens weergeven

Klik op de **Azure SQL Analytics** tegel om de Azure SQL Analytics dashboard te openen. Het dashboard bevat het overzicht van alle databases die worden bewaakt via verschillende perspectieven. Voor verschillende perspectieven werken, moet u Logboeken of de juiste metrische gegevens op uw SQL-bronnen kunnen worden gestreamd naar Azure Log Analytics-werkruimte inschakelen.

![Overzicht van Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Als u een van de tegels selecteert, wordt een rapport zoomen geopend in de specifieke perspectief. Zodra het perspectief is geselecteerd, wordt het rapport zoomen geopend.

![Azure SQL Analytics time-outs](./media/log-analytics-azure-sql/azure-sql-sol-metrics.png)

Elk perspectief biedt samenvattingen op abonnement, server elastische pool en databaseniveau. Bovendien ziet elk perspectief u een perspectief specifiek zijn voor het rapport aan de rechterkant. Abonnement, server, groep of database selecteren in de lijst, blijft de inzoomen.

| Perspectief | Beschrijving |
| --- | --- |
| Resource per type | Perspectief waarmee alle resources bewaakt wordt geteld. Inzoomen biedt een overzicht van metrische gegevens voor DTU en GB. |
| Inzichten | Biedt hiërarchische inzoomen in Intelligent inzichten. Meer informatie over intelligent insights. |
| Fouten | Biedt hiërarchische inzoomen in SQL-fouten die zich op de databases voorgedaan. |
| Time-outs | Biedt hiërarchische inzoomen in SQL-outs die hebben plaatsgevonden van de databases. |
| Blokkeringen | Biedt hiërarchische inzoomen in SQL-blockings die hebben plaatsgevonden van de databases. |
| Wachten op database | Biedt hiërarchische inzoomen in SQL wacht statistieken op databaseniveau. Bevat overzichten van de totale wachttijd en de wachttijd per type wacht. |
| Duur van de query | Biedt hiërarchische inzoomen in de statistieken van de query kan worden uitgevoerd zoals duur van de query, CPU-gebruik, gegevens-IO-gebruik, logboek-i/o-gebruik. |
| Wachten op query | Biedt hiërarchische inzoomen in de query wacht statistieken per categorie wacht. |

### <a name="intelligent-insights-report"></a>Intelligent Insights-rapport

Azure SQL Database [Intelligent Insights](../sql-database/sql-database-intelligent-insights.md) kunt u weten wat er gebeurt met de databaseprestaties van uw. Alle Intelligent Insights verzameld worden weergegeven en toegankelijk is via het perspectief Insights.

![Azure SQL Analytics Insights](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Elastische Pool en Database-rapporten

Zowel elastische Pools en Databases hebben hun eigen specifieke rapporten waarin de gegevens die worden verzameld voor de resource in de opgegeven tijd.

![Azure SQL-Database voor Analytics](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Azure SQL Analytics elastische groep](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Query-rapporten

U kunt de prestaties van een query door het queryrapport correleren via de duur van de Query en query wacht perspectieven. Dit rapport vergelijkt de prestaties van query's over verschillende databases en kunt u gemakkelijk op de speldenpunt van databases waarmee de geselecteerde query ook versus die langzaam worden uitgevoerd.

![Azure SQL analysequery 's](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

### <a name="analyze-data-and-create-alerts"></a>Gegevens analyseren en waarschuwingen maken

U kunt gemakkelijk [waarschuwingen maken](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) met de gegevens die afkomstig zijn van Azure SQL Database-resources. Hier volgen enkele nuttige [logboek zoeken](log-analytics-log-searches.md) query's die u kunt gebruiken met een waarschuwing in een logboek:



*Hoge DTU voor Azure SQL Database*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/DATABASES/" and MetricName=="dtu_consumption_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

*Hoge DTU op elastische Pool in Azure SQL Database*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/ELASTICPOOLS/" and MetricName=="dtu_consumption_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```



## <a name="next-steps"></a>Volgende stappen

- Gebruik [logboek zoekopdrachten](log-analytics-log-searches.md) in Log Analytics om gedetailleerde Azure SQL-gegevens te bekijken.
- [Maak uw eigen dashboards](log-analytics-dashboards.md) Azure SQL-gegevens worden weergegeven.
- [Waarschuwingen maken](log-analytics-alerts.md) wanneer specifieke Azure SQL-gebeurtenissen plaatsvinden.
