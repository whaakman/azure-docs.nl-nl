---
title: Azure SQL Analytics-oplossing in Log Analytics | Microsoft Docs
description: Azure SQL Analytics-oplossing helpt u bij het beheren van uw Azure SQL-databases
services: log-analytics
documentationcenter: ''
author: danimir
manager: carmonm
ms.reviewer: carlrab
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: v-daljep
ms.component: na
ms.openlocfilehash: 47069f0af7409d87cb2d4fbbbce9dda0b1c2056e
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886557"
---
# <a name="monitor-azure-sql-databases-using-azure-sql-analytics-preview"></a>Monitor voor Azure SQL-Databases met behulp van Azure SQL Analytics (Preview)

![Azure SQL Analytics-symbool](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Azure SQL Analytics is een cloud voor controle voor het bewaken van de prestaties van Azure SQL-Databases op schaal in meerdere elastische pools en abonnementen. Er worden verzameld en worden gevisualiseerd met belangrijke metrische gegevens voor Azure SQL Database-prestaties met ingebouwde intelligentie voor bovenaan op problemen met prestaties. 

Metrische gegevens die u hebt verzameld met de oplossing gebruikt, kunt u aangepaste regels voor bewaking en waarschuwingen kunt maken. De oplossing helpt u bij het identificeren van problemen in elke laag van uw toepassingsstack. Metrische gegevens voor Azure-diagnose en weergaven van Log Analytics wordt gebruikt om gegevens over uw Azure SQL-databases en elastische pools in één Log Analytics-werkruimte te presenteren. Log Analytics helpt u bij het verzamelen, afstemmen en visualiseren van gestructureerde en ongestructureerde gegevens.

Deze preview-oplossing ondersteunt momenteel maximaal 150.000 Azure SQL-Databases en 5000 elastische SQL-Pools per werkruimte.

Zie de ingesloten video voor een praktische overzicht over het gebruik van Azure SQL Analytics-oplossing en voor typische gebruiksscenario's:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Verbonden bronnen

Azure SQL Analytics is een cloud solution ondersteunende streaming van diagnostische gegevens telemetrie voor Azure SQL-Databases en elastische pools controleren. Verbinding maken met de service Log Analytics agents niet gebruikt, de oplossing biedt geen ondersteuning voor connectiviteit met Windows, Linux of SCOM-resources, Zie de onderstaande Compatibiliteitstabel.

| Verbonden bron | Ondersteuning | Beschrijving |
| --- | --- | --- |
| **[Azure Diagnostics](log-analytics-azure-storage.md)** | **Ja** | Azure-logboeken en metrische gegevens gegevens worden verzonden naar Log Analytics rechtstreeks door Azure. |
| [Azure Storage-account](log-analytics-azure-storage.md) | Nee | Log Analytics lezen niet van de gegevens van een storage-account. |
| [Windows-agents](log-analytics-windows-agent.md) | Nee | Direct Windows-agents niet worden gebruikt door de oplossing. |
| [Linux-agents](log-analytics-linux-agents.md) | Nee | Directe Linux-agents niet worden gebruikt door de oplossing. |
| [SCOM-beheergroep](log-analytics-om-agents.md) | Nee | Een directe verbinding van de SCOM-agent naar Log Analytics wordt niet gebruikt door de oplossing. |

## <a name="configuration"></a>Configuratie

De volgende stappen uitvoeren om de Azure SQL Analytics-oplossing toevoegen aan uw werkruimte.

1. De Azure SQL Analytics-oplossing toevoegen aan uw werkruimte van [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview).
2. Klik in de Azure-portal op **+ een resource maken**, zoek vervolgens naar **Azure SQL Analytics**.  
    ![Controle en beheer](./media/log-analytics-azure-sql/monitoring-management.png)
3. Selecteer **Azure SQL Analytics (Preview)** in de lijst
4. In de **Azure SQL Analytics (Preview)** gebied, klikt u op **maken**.  
    ![Maken](./media/log-analytics-azure-sql/portal-create.png)
5. In de **nieuwe oplossing maken** gebied, maak een nieuwe of Selecteer een bestaande werkruimte die u wilt toevoegen van de oplossing en klik vervolgens op **maken**.  
    ![aan werkruimte toevoegen](./media/log-analytics-azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-and-elastic-pools-to-stream-diagnostics-telemetry"></a>Azure SQL-Databases en elastische Pools tot stream diagnostische gegevens telemetrie configureren

Nadat u Azure SQL Analytics-oplossing in uw werkruimte hebt gemaakt, als u wilt bewaken van prestaties van Azure SQL-Databases en/of elastische Pools, moet u **elk configureren** van Azure SQL Database en elastische pool-resource die u wilt om te controleren om te streamen van de diagnostische gegevens telemetrie aan de oplossing.

- Azure Diagnostics inschakelt voor uw Azure SQL-databases en elastische pools en [configureren zodat ze hun gegevens verzenden naar Log Analytics](../sql-database/sql-database-metrics-diag-logging.md).

### <a name="to-configure-multiple-azure-subscriptions"></a>Het configureren van meerdere Azure-abonnementen

Ter ondersteuning van meerdere abonnementen, gebruikt u de PowerShell-script uit [inschakelen Azure resource metrische gegevens vastleggen met behulp van PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/). Geef de werkruimte-ID als parameter bij het uitvoeren van het script voor het verzenden van diagnostische gegevens van resources in één Azure-abonnement aan een werkruimte in een andere Azure-abonnement.

**Voorbeeld**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a>De oplossing gebruiken

Wanneer u de oplossing aan uw werkruimte toevoegt, de tegel Azure SQL Analytics wordt toegevoegd aan uw werkruimte en deze wordt weergegeven in het overzicht. De tegel ziet u het aantal Azure SQL-databases en elastische Azure SQL-groepen die de oplossing is verbonden met.

![Azure SQL Analytics-tegel](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL Analytics-gegevens weergeven

Klik op de **Azure SQL Analytics** tegel om de Azure SQL Analytics-dashboard te openen. Het dashboard bevat een overzicht van alle databases die via verschillende perspectieven worden bewaakt. Voor verschillende perspectieven te werken, moet u juiste metrische gegevens of Logboeken inschakelen voor uw SQL-resources kunnen worden gestreamd naar Azure Log Analytics-werkruimte.

![Overzicht van Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Het selecteren van de tegels, wordt een rapport zoomen geopend in de specifieke perspectief. Nadat het perspectief is geselecteerd, wordt het rapport zoomen geopend.

![Azure SQL Analytics time-outs](./media/log-analytics-azure-sql/azure-sql-sol-metrics.png)

Elk perspectief biedt samenvattingen op abonnement, server, elastische pool en databaseniveau. Bovendien ziet elk perspectief u een perspectief specifiek zijn voor het rapport aan de rechterkant. Abonnement, server, groep of database selecteren in de lijst, blijft de inzoomen.

| Perspectief | Beschrijving |
| --- | --- |
| Resource per type | Perspectief dat alle resources die worden bewaakt telt. Inzoom biedt een overzicht van metrische gegevens over DTU en GB. |
| Inzichten | Biedt hiërarchische inzoomen naar intelligente inzichten. Meer informatie over intelligente inzichten. |
| Fouten | Biedt hiërarchische inzoomen in SQL-fouten die hebben plaatsgevonden voor de databases. |
| Time-outs | Biedt hiërarchische inzoomen in SQL-outs die hebben plaatsgevonden voor de databases. |
| Blokkeringen | Biedt hiërarchische inzoomen in SQL-blockings die hebben plaatsgevonden voor de databases. |
| Wachten op database | Biedt hiërarchische inzoomen in SQL wacht statistieken op het databaseniveau van de. Bevat overzichten van de totale wachttijd en de wachttijd per type wachten. |
| Queryduur | Biedt hiërarchische inzoomen in de uitvoering van querystatistieken, zoals queryduur van de, CPU-gebruik, gegevens-IO en logboek-IO-gebruik. |
| Wachten op query | Biedt hiërarchische inzoomen in de wacht querystatistieken op categorie wachten. |

### <a name="intelligent-insights-report"></a>Intelligent Insights-rapport

Azure SQL Database [Intelligent Insights](../sql-database/sql-database-intelligent-insights.md) kunt u weten wat er gebeurt met de databaseprestaties van uw. Alle Intelligent Insights verzamelde kan worden gevisualiseerd en toegankelijk is via de Insights-perspectief.

![Azure SQL Analytics-inzichten](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Elastische Pool- en -rapporten

Zowel elastische Pools en Databases hebben hun eigen specifieke rapporten waarin alle gegevens die worden verzameld voor de resource in de opgegeven tijd.

![Azure SQL Analytics-Database](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Azure SQL Analytics elastische Pool](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Query-rapporten

Via de duur van de Query en de query wacht perspectieven, kunt u de prestaties van elke query door de queryrapport correleren. Dit rapport worden de prestaties van query's voor verschillende databases met elkaar vergeleken en kunt u gemakkelijk de databases die de geselecteerde query ook ten opzichte van die langzaam worden uitgevoerd met deze functie.

![Azure SQL Analytics-query 's](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

### <a name="analyze-data-and-create-alerts"></a>Gegevens analyseren en waarschuwingen te maken

U kunt eenvoudig [waarschuwingen maken](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) met de gegevens die afkomstig zijn van Azure SQL Database-resources. Hier volgen enkele nuttige [zoeken in logboeken](log-analytics-log-searches.md) query's die u met een waarschuwing gebruiken kunt:

*Hoge CPU-capaciteit op Azure SQL Database*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/DATABASES/"
| where MetricName=="cpu_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - Vooraf vereiste van het instellen van deze waarschuwing is die stream metrische gegevens gecontroleerde databases (optie 'Alle metrische gegevens') op de oplossing.
> - Vervang de waarde MetricName cpu_percent door dtu_consumption_percent hoog DTU om resultaten te verkrijgen in plaats daarvan.

*Hoge CPU-capaciteit op Azure SQL Database elastische Pools*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/ELASTICPOOLS/"
| where MetricName=="cpu_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - Vooraf vereiste van het instellen van deze waarschuwing is die stream metrische gegevens gecontroleerde databases (optie 'Alle metrische gegevens') op de oplossing.
> - Vervang de waarde MetricName cpu_percent door dtu_consumption_percent hoog DTU om resultaten te verkrijgen in plaats daarvan.

*Azure SQL Database-opslag in gemiddelde meer dan 95% in de afgelopen 1 uur*

```
let time_range = 1h;
let storage_threshold = 95;
AzureMetrics
| where ResourceId contains "/DATABASES/"
| where MetricName == "storage_percent"
| summarize max_storage = max(Average) by ResourceId, bin(TimeGenerated, time_range)
| where max_storage > storage_threshold
| distinct ResourceId
```

> [!NOTE]
> - Vooraf vereiste van het instellen van deze waarschuwing is die stream metrische gegevens gecontroleerde databases (optie 'Alle metrische gegevens') op de oplossing.
> - Deze query vereist een waarschuwingsregel om te worden ingesteld voor het starten een waarschuwing als er resultaten (> 0 resultaten) van de query, die aangeeft of de voorwaarde op sommige databases bestaat. De uitvoer is een lijst van databaseresources die zich boven de storage_threshold binnen de time_range gedefinieerd.
> - De uitvoer is een lijst van databaseresources die zich boven de storage_threshold binnen de time_range gedefinieerd.

*Waarschuwing bij Intelligent insights*

```
let alert_run_interval = 1h;
let insights_string = "hitting its CPU limits";
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active" 
| where TimeGenerated > ago(alert_run_interval)
| where rootCauseAnalysis_s contains insights_string
| distinct ResourceId
```

> [!NOTE]
> - Vooraf vereiste van het instellen van deze waarschuwing is die diagnoselogboek voor gecontroleerde databases stream SQLInsights aan de oplossing.
> - Deze query vereist een waarschuwingsregel om te worden ingesteld om uit te voeren met dezelfde frequentie als alert_run_interval om te voorkomen dat dubbele resultaten. De regel moet worden ingesteld om te worden geactiveerd uit de waarschuwing als er resultaten (> 0 resultaten) van de query.
> - De alert_run_interval om op te geven van het tijdsbereik om te controleren als de voorwaarde is opgetreden op de databases die zijn geconfigureerd in stream SQLInsights logboek op de oplossing aanpassen.
> - De insights_string om vast te leggen van de uitvoer van de inzichten hoofdmap oorzaak analysis tekst aanpassen. Dit is dezelfde tekst weergegeven in de gebruikersinterface van de oplossing die u uit de bestaande inzichten gebruiken kunt. U kunt ook de onderstaande query gebruiken om te zien van de tekst van alle inzichten die zijn gegenereerd voor uw abonnement. De uitvoer van de query gebruiken om de afzonderlijke tekenreeksen voor het instellen van waarschuwingen voor inzichten.

```
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active" 
| distinct rootCauseAnalysis_s
```

## <a name="next-steps"></a>Volgende stappen

- Gebruik [zoekopdrachten](log-analytics-log-searches.md) in Log Analytics om gedetailleerde Azure SQL-gegevens weer te geven.
- [Maak uw eigen dashboards](log-analytics-dashboards.md) met Azure SQL-gegevens.
- [Waarschuwingen maken](log-analytics-alerts.md) wanneer specifieke Azure SQL-gebeurtenissen plaatsvinden.
