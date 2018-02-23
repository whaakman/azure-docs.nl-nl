---
title: Azure SQL Analytics-oplossing in Log Analytics | Microsoft Docs
description: De Azure SQL Analytics-oplossing kunt u uw Azure SQL-databases beheren.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: magoedte
ms.openlocfilehash: 624c861db9bb318c368cef04965da0a73dd028d8
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a>Azure SQL Database met behulp van Azure SQL Analytics (Preview) in logboekanalyse bewaken

![Azure SQL Analytics symbool](./media/log-analytics-azure-sql/azure-sql-symbol.png)

De Azure SQL Analytics-oplossing in Azure-logboekanalyse verzamelt en visualiseren van belangrijke maatstaven voor prestaties van SQL Azure. Met behulp van de metrische gegevens die u verzamelt van de oplossing, kunt u aangepaste regels voor bewaking en waarschuwingen. U kunt Azure SQL Database bewaken en elastische pool metrische gegevens op meerdere Azure-abonnementen en elastische pools en ze visualiseren. De oplossing helpt u problemen bij elke laag van uw toepassing stack kunt identificeren.  Hierbij [diagnostische Azure metrische gegevens](log-analytics-azure-storage.md) samen met logboekanalyse weergaven om gegevens over uw Azure SQL-databases en elastische pools in een enkel Log Analytics-werkruimte te presenteren.

Deze preview-oplossing ondersteunt momenteel maximaal 150.000 Azure SQL-Databases en 5000 SQL elastische Pools per werkruimte.

De Azure SQL Analytics-oplossing, net zoals andere beschikbaar voor logboekanalyse, kunt u controleren en meldingen ontvangen over de status van uw Azure-resources: in dit geval, Azure SQL Database. Microsoft Azure SQL Database is een schaalbare relationele database-service die bekende SQL-Server-achtige mogelijkheden voor toepassingen die worden uitgevoerd in de Azure-cloud biedt. Log Analytics, helpt u bij het verzamelen, correleren en gestructureerde en ongestructureerde gegevens visualiseren.

Zie voor een praktische overzicht over het gebruik van Azure SQL Analytics-oplossing en typische gebruiksscenario's de ingesloten video:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Verbonden bronnen

De Azure SQL Analytics-oplossing gebruikt niet agents verbinding maken met de Log Analytics-service.

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing.

| Verbonden bron | Ondersteuning | Beschrijving |
| --- | --- | --- |
| [Windows-agents](log-analytics-windows-agent.md) | Nee | Directe Windows-agents worden niet gebruikt door de oplossing. |
| [Linux-agents](log-analytics-linux-agents.md) | Nee | Directe Linux-agents worden niet gebruikt door de oplossing. |
| [SCOM-beheergroep](log-analytics-om-agents.md) | Nee | Een rechtstreekse verbinding tussen de SCOM-agents met logboekanalyse wordt niet gebruikt door de oplossing. |
| [Azure Storage-account](log-analytics-azure-storage.md) | Nee | Log Analytics biedt de gegevens niet lezen uit een opslagaccount. |
| [Azure Diagnostics](log-analytics-azure-storage.md) | Ja | Azure metrische gegevens en logboekbestanden gegevens worden verzonden met logboekanalyse rechtstreeks door Azure. |

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u niet hebt, kunt u één voor [gratis](https://azure.microsoft.com/free/).
- Een werkruimte voor logboekanalyse. U kunt een bestaande of kunt u [Maak een nieuwe](log-analytics-quick-create-workspace.md) voordat u begint met behulp van deze oplossing.
- Azure Diagnostics inschakelen voor uw Azure SQL-databases en elastische pools en [configureren zodat ze hun gegevens verzenden naar logboekanalyse](../sql-database/sql-database-metrics-diag-logging.md).

## <a name="configuration"></a>Configuratie

Voer de volgende stappen uit om toe te voegen van de Azure SQL Analytics-oplossing naar de werkruimte.

1. De Azure SQL Analytics-oplossing toevoegen aan uw werkruimte van [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).
2. Klik in de Azure-portal op **maken van een resource** > **bewaking + Management**.  
    ![Controle en beheer](./media/log-analytics-azure-sql/monitoring-management.png)
3. In de **bewaking + Management** Klik **alle**.
4. In de **aanbevolen** lijst, klikt u op **meer**, en klik vervolgens in de nieuwe lijst vinden **Azure SQL Analytics (Preview)** en selecteert u vervolgens.  
    ![Azure SQL Analytics-oplossing](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)
5. In de **Azure SQL Analytics (Preview)** gebied, klikt u op **maken**.  
    ![Maken](./media/log-analytics-azure-sql/portal-create.png)
6. In de **nieuwe oplossing maken** gebied, selecteer de werkruimte die u wilt toevoegen, de oplossing en klik vervolgens op **maken**.  
    ![toevoegen aan werkruimte](./media/log-analytics-azure-sql/add-to-workspace.png)


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

>[!NOTE]
> Werk uw logboekanalyse als u de nieuwste versie van Azure SQL Analytics.
>

Wanneer u de oplossing voor uw werkruimte toevoegt, is de tegel Azure SQL Analytics toegevoegd aan uw werkruimte en wordt deze weergegeven in het overzicht. De tegel toont het aantal Azure SQL-databases en elastische pools SQL Azure die de oplossing is verbonden met.

![Azure SQL-Analytics tegel](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL analytische gegevens weergeven

Klik op de **Azure SQL Analytics** tegel om de Azure SQL Analytics dashboard te openen. Het dashboard bevat het overzicht van alle databases die worden bewaakt via verschillende perspectieven. Voor verschillende perspectieven werken, moet u Logboeken of de juiste metrische gegevens op uw SQL-bronnen kunnen worden gestreamd naar Azure Log Analytics-werkruimte inschakelen.

![Overzicht van Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Als u een van de tegels selecteert, wordt een rapport zoomen geopend in de specifieke perspectief. Zodra het perspectief is geselecteerd, wordt het rapport zoomen geopend.

![Azure SQL Analytics time-outs](./media/log-analytics-azure-sql/azure-sql-sol-timeouts.png)

Elk perspectief biedt samenvattingen op abonnement, server elastische pool en databaseniveau. Bovendien ziet elk perspectief u een perspectief specifiek zijn voor het rapport aan de rechterkant. Abonnement, server, groep of database selecteren in de lijst, blijft de inzoomen.

| Perspectief | Beschrijving |
| --- | --- |
| Resource per type | Perspectief waarmee alle resources bewaakt wordt geteld. Inzoomen biedt een overzicht van metrische gegevens voor DTU en GB. |
| Inzichten | Biedt hiërarchische inzoomen in Intelligent inzichten. Meer informatie over intelligent insights. |
| Fouten | Biedt hiërarchische inzoomen in SQL-fouten die zich op de databases voorgedaan. |
| Time-outs | Biedt hiërarchische inzoomen in SQL-outs die hebben plaatsgevonden van de databases. |
| Blockings | Biedt hiërarchische inzoomen in SQL-blockings die hebben plaatsgevonden van de databases. |
| Wachten op database | Biedt hiërarchische inzoomen in SQL wacht statistieken op databaseniveau. Bevat overzichten van de totale wachttijd en de wachttijd per type wacht. |
| duur van de query | Biedt hiërarchische inzoomen in de statistieken van de query kan worden uitgevoerd zoals duur van de query, CPU-gebruik, gegevens-IO-gebruik, logboek-i/o-gebruik. |
| Query moet wachten | Biedt hiërarchische inzoomen in de query wacht statistieken per categorie wacht. |

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

U kunt eenvoudig waarschuwingen maken met de gegevens die afkomstig zijn van Azure SQL Database-resources. Hier volgen enkele nuttige [logboek zoeken](log-analytics-log-searches.md) query's die u voor waarschuwingen gebruiken kunt:

[!INCLUDE[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


*Hoge DTU voor Azure SQL Database*

```
AzureMetrics | where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/DATABASES/" and MetricName=="dtu_consumption_percent" | summarize avg(Maximum) by ResourceId
```

*Hoge DTU op elastische Pool in Azure SQL Database*

```
AzureMetrics | where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/ELASTICPOOLS/" and MetricName=="dtu_consumption_percent" | summarize avg(Maximum) by ResourceId
```

U kunt deze waarschuwing-query's gebruiken om te attenderen op specifieke drempelwaarden voor Azure SQL Database en elastische pools. Een waarschuwing voor uw werkruimte voor logboekanalyse configureren:

#### <a name="to-configure-an-alert-for-your-workspace"></a>Een waarschuwing voor uw werkruimte configureren

1. Ga naar de [OMS-portal](http://mms.microsoft.com/) en meld u aan.
2. Open de werkruimte die u hebt geconfigureerd voor de oplossing.
3. Klik op de pagina overzicht de **Azure SQL Analytics (Preview)** tegel.
4. Voer een van de voorbeelden van query's.
5. In logboek zoekopdracht, klikt u op **waarschuwing**.  
![waarschuwing in de zoekopdracht maken](./media/log-analytics-azure-sql/create-alert01.png)
6. Op de **waarschuwingsregel toevoegen** pagina, configureert de toepasselijke eigenschappen en de specifieke drempelwaarden die u wilt en klik vervolgens op **opslaan**.  
![toevoegen van waarschuwingsregel](./media/log-analytics-azure-sql/create-alert02.png)

## <a name="next-steps"></a>Volgende stappen

- Gebruik [logboek zoekopdrachten](log-analytics-log-searches.md) in Log Analytics om gedetailleerde Azure SQL-gegevens te bekijken.
- [Maak uw eigen dashboards](log-analytics-dashboards.md) Azure SQL-gegevens worden weergegeven.
- [Waarschuwingen maken](log-analytics-alerts.md) wanneer specifieke Azure SQL-gebeurtenissen plaatsvinden.
