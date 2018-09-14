---
title: Metrische gegevens van Azure SQL database en diagnostische logboekregistratie | Microsoft Docs
description: Meer informatie over het configureren van Azure SQL Database voor het opslaan van Resourcegebruik, connectiviteit en Uitvoeringsstatistieken query.
services: sql-database
documentationcenter: ''
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: aa031b87df51bd9f7dec40a6c3e56023e2d82d96
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579493"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Metrische gegevens van Azure SQL-Database en logboekregistratie van diagnostische gegevens 
Azure SQL-Database kan metrische en diagnostische gegevens verzenden logboeken voor de bewaking vergemakkelijken. U kunt SQL Database configureren voor het opslaan van resourcegebruik, werkrollen en sessies, en connectiviteit in een van deze Azure-resources:

* **Azure Storage**: gebruikt voor het archiveren van grote hoeveelheden telemetriegegevens voor een lage prijs.
* **Azure Event Hubs**: gebruikt voor het integreren van SQL Database-telemetrie in uw eigen bewakingsoplossing of actieve pijplijnen.
* **Azure Log Analytics**: gebruikt voor een out-of-the-box-oplossing voor prestatiecontrole met rapportages, waarschuwingen en risicobeperking mogelijkheden. Dit is een functie van de [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md)

    ![Architectuur](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-logging"></a>Logboekregistratie inschakelen

Metrische en diagnostische gegevens logboekregistratie is standaard niet ingeschakeld. U kunt inschakelen en beheren van metrische en diagnostische gegevens logboekregistratie met behulp van een van de volgende methoden:

- Azure Portal
- PowerShell
- Azure-CLI
- Azure Monitor REST-API 
- Azure Resource Manager-sjabloon

Wanneer u metrische gegevens en logboekregistratie van diagnostische gegevens inschakelt, moet u om op te geven van de Azure-resource waar de geselecteerde gegevens worden verzameld. Beschikbare opties zijn onder andere:

- Log Analytics
- Event Hubs
- Storage 

U kunt inrichten van een nieuwe Azure-resource of Selecteer een bestaande resource. Na het selecteren van de storage-resource, moet u opgeven welke gegevens worden verzameld. Beschikbare opties zijn onder andere:

- [Alle metrische gegevens](sql-database-metrics-diag-logging.md#all-metrics): bevat DTU-percentage, DTU limiet, CPU-percentage, fysieke gegevens gelezen percentage, logboek schrijven percentage geslaagd/mislukt/geblokkeerd door firewallverbindingen, percentage van sessies, percentage van de werknemers, opslag, opslagpercentage , en het percentage van XTP-opslag.
- [QueryStoreRuntimeStatistics](sql-database-metrics-diag-logging.md#query-store-runtime-statistics): bevat informatie over de query-runtime-statistieken, zoals CPU-gebruik en de query duur.
- [QueryStoreWaitStatistics](sql-database-metrics-diag-logging.md#query-store-wait-statistics): bevat informatie over de query wait-statistieken, dit geeft aan wat uw query's wachtte op, zoals CPU, het logboek en VERGRENDELEN.
- [Fouten](sql-database-metrics-diag-logging.md#errors-dataset): bevat informatie over SQL-fouten die hebben plaatsgevonden voor deze database.
- [DatabaseWaitStatistics](sql-database-metrics-diag-logging.md#database-wait-statistics-dataset): bevat informatie over hoeveel tijd een database besteed aan het wachten op andere wacht typen.
- [Time-outs](sql-database-metrics-diag-logging.md#time-outs-dataset): bevat informatie over time-outs die hebben plaatsgevonden in een database.
- [Blokken](sql-database-metrics-diag-logging.md#blockings-dataset): bevat informatie over het blokkeren van gebeurtenissen die hebben plaatsgevonden in een database.
- [SQLInsights](sql-database-metrics-diag-logging.md#intelligent-insights-dataset): Intelligent Insights bevat. [Meer informatie over Intelligent Insights](sql-database-intelligent-insights.md).
- **Audit** / **SQLSecurityAuditEvents**: momenteel niet beschikbaar.

Als u Event Hubs of een storage-account selecteert, kunt u een bewaarbeleid opgeven. Dit beleid verwijdert de gegevens die ouder is dan een geselecteerde periode. Als u Log Analytics opgeeft, is het bewaarbeleid afhankelijk van de geselecteerde prijscategorie. Zie voor meer informatie, [Log Analytics-prijzen](https://azure.microsoft.com/pricing/details/log-analytics/). 

Voor meer informatie over het inschakelen van logboekregistratie en begrijpen van de metrische gegevens en logboekbestanden categorieën die worden ondersteund door de verschillende Azure-services, wordt u aangeraden dat u leest: 

* [Overzicht van metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
* [Overzicht van diagnostische logboeken van Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) 

### <a name="azure-portal"></a>Azure Portal

1. Om in te schakelen logboeken verzamelen van metrische en diagnostische gegevens in de portal, gaat u naar uw SQL-Database of elastische pool-pagina en selecteer vervolgens **diagnostische instellingen**.

   ![Inschakelen in Azure portal](./media/sql-database-metrics-diag-logging/enable-portal.png)

2. Maak een nieuwe of bestaande diagnostische instellingen bewerken door het doel en de telemetrie te selecteren.

   ![Diagnostische instellingen](./media/sql-database-metrics-diag-logging/diagnostics-portal.png)

### <a name="powershell"></a>PowerShell

Om in te schakelen metrische en diagnostische gegevens logboekregistratie met behulp van PowerShell, gebruikt u de volgende opdrachten:

- Om in te schakelen opslag van diagnostische logboeken in een opslagaccount, gebruikt u deze opdracht:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Het opslagaccount-ID is de resource-ID voor het opslagaccount waar u om de logboeken te verzenden.

- Als u wilt inschakelen voor streaming van diagnostische logboeken naar een event hub, gebruikt u deze opdracht:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   De regel-ID van Azure Service Bus is een tekenreeks zijn met deze indeling:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Als u wilt inschakelen verzenden van diagnostische logboeken naar Log Analytics-werkruimte, moet u deze opdracht gebruiken:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- U vindt de resource-ID van uw Log Analytics-werkruimte met behulp van de volgende opdracht uit:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

U kunt deze parameters voor het inschakelen van meerdere uitvoeropties combineren.

### <a name="to-configure-multiple-azure-resources"></a>Het configureren van meerdere Azure-resources

Ter ondersteuning van meerdere abonnementen, gebruikt u de PowerShell-script uit [inschakelen Azure resource metrische gegevens vastleggen met behulp van PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

De werkruimte resource-ID opgeven &lt;$WSID&gt; als parameter bij het uitvoeren van het script (inschakelen-AzureRMDiagnostics.ps1) voor het verzenden van diagnostische gegevens uit meerdere bronnen naar de werkruimte. Om op te halen van de werkruimte-ID &lt;$WSID&gt; naar die u verzenden van diagnostische gegevens wilt, Vervang &lt;subID&gt; vervangen door de abonnements-ID, &lt;RG_NAME&gt; met de naam van de resourcegroep, en vervang &lt;WS_NAME&gt; met de naam van de werkruimte in het volgende script.

- Voor het configureren van meerdere Azure-resources, gebruikt u de volgende opdrachten:

    ```powershell
    PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/<RG_NAME>/providers/microsoft.operationalinsights/workspaces/<WS_NAME>"
    PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
    ```

### <a name="azure-cli"></a>Azure-CLI

Om in te schakelen metrische en diagnostische gegevens logboekregistratie met behulp van de Azure CLI, gebruikt u de volgende opdrachten:

- Om in te schakelen opslag van diagnostische logboeken in een opslagaccount, gebruikt u deze opdracht:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   Het opslagaccount-ID is de resource-ID voor het opslagaccount waar u om de logboeken te verzenden.

- Als u wilt inschakelen voor streaming van diagnostische logboeken naar een event hub, gebruikt u deze opdracht:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   De regel-ID van Service Bus is een tekenreeks zijn met deze indeling:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Als u wilt inschakelen verzenden van diagnostische logboeken naar Log Analytics-werkruimte, moet u deze opdracht gebruiken:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

U kunt deze parameters voor het inschakelen van meerdere uitvoeropties combineren.

### <a name="rest-api"></a>REST-API

Meer informatie over hoe u [diagnostische instellingen wijzigen met behulp van de Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). 

### <a name="resource-manager-template"></a>Resource Manager-sjabloon

Meer informatie over hoe u [diagnostische instellingen bij het maken van resource inschakelen met behulp van Resource Manager-sjabloon](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-log-analytics"></a>Stream in Log Analytics 
De logboeken van de metrische gegevens en diagnostische gegevens van de SQL-Database kunnen worden gestreamd naar Log Analytics met behulp van de ingebouwde **verzenden naar Log Analytics** optie in de portal. U kunt ook Log Analytics inschakelen met behulp van een diagnostische instelling via PowerShell-cmdlets, de Azure CLI of de Azure Monitor REST API.

### <a name="installation-overview"></a>Installatie-overzicht

Bewaking van de vloot van een SQL-Database is heel eenvoudig met Log Analytics. Er zijn drie stappen vereist:

1. Een Log Analytics-resource maken.

2. Databases naar record metrische gegevens en diagnostische logboeken configureren in de Log Analytics-resource die u hebt gemaakt.

3. Installeer de **Azure SQL Analytics** oplossing uit de galerie in Log Analytics.

### <a name="create-a-log-analytics-resource"></a>Een Log Analytics-resource maken

1. Selecteer **een resource maken** in het menu aan de linkerkant.

2. Selecteer **bewaking en beheer**.

3. Selecteer **Log Analytics**.

4. Vul het formulier Log Analytics met de aanvullende informatie die is vereist: naam van de werkruimte, abonnement, resourcegroep, locatie en prijscategorie.

   ![Log Analytics](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>Databases naar Logboeken voor records metrische en diagnostische gegevens configureren

De eenvoudigste manier om te configureren waarin de hun metrische gegevens voor het registreren van databases is via de Azure-portal. In de portal, gaat u naar de resource van uw SQL-Database en selecteer **diagnostische instellingen**. 

### <a name="install-the-sql-analytics-solution-from-the-gallery"></a>De oplossing SQL Analytics installeren vanuit de galerie

1. Nadat u de Log Analytics-resource maakt en uw gegevens worden doorgestuurd naar het, installeert u de oplossing SQL Analytics. Selecteer op de startpagina op het menu aan de **Oplossingengalerie**. Selecteer in de galerie, de **Azure SQL Analytics** oplossing en selecteer **toevoegen**.

   ![Oplossing voor controle](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. Op uw startpagina, de **Azure SQL Analytics** tegel wordt weergegeven. Selecteer deze tegel om de SQL Analytics-dashboard te openen.

### <a name="use-the-sql-analytics-solution"></a>De oplossing SQL Analytics gebruiken

SQL-analyse is een hiërarchische dashboard waarmee u de hiërarchie van SQL Database-resources te doorlopen. Zie voor meer informatie over het gebruik van de oplossing SQL Analytics, [SQL-Database controleren met behulp van de oplossing SQL Analytics](../log-analytics/log-analytics-azure-sql.md).

## <a name="stream-into-event-hubs"></a>Streamen in Event Hubs

De logboeken van de metrische gegevens en diagnostische gegevens van de SQL-Database kunnen worden gestreamd naar Event Hubs met behulp van de ingebouwde **Stream naar een event hub** optie in de portal. U kunt de regel-ID van Service Bus ook inschakelen met behulp van een diagnostische instelling via PowerShell-cmdlets, de Azure CLI of de Azure Monitor REST API. 

### <a name="what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs"></a>Wat te doen met metrische en diagnostische gegevens in Event Hubs-Logboeken
Nadat u de geselecteerde gegevens worden gestreamd naar Event Hubs, bent u een stapje dichter bij het inschakelen van geavanceerde bewakingsscenario's. Eventhubs fungeert als de voordeur van een gebeurtenispijplijn. Nadat gegevens zijn verzameld in een event hub, kan worden omgezet en opgeslagen met behulp van elke gewenste realtime analyseprovider of batching/opslagadapters. Eventhubs worden losgekoppeld van de productie van een stroom gebeurtenissen van het gebruik van deze gebeurtenissen. Op deze manier kunnen gebeurtenisconsumers toegang tot de gebeurtenissen op hun eigen planning. Zie voor meer informatie over Event Hubs:

- [Wat zijn Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Aan de slag met Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


Hier volgen enkele manieren waarop u de streaming-mogelijkheden kan gebruiken:

* **Servicestatus weergeven door het streamen van gegevens naar Power BI hot pad**. Met behulp van Event Hubs, Stream Analytics en Power BI, kunt u eenvoudig uw metrische gegevens en diagnostische gegevens in bijna realtime inzichten in uw Azure-services te transformeren. Zie voor een overzicht van het instellen van een event hub, gegevens verwerken met Stream Analytics, en gebruik Power BI als uitvoer, [Stream Analytics en Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).

* **Stream logboeken naar derden logboekregistratie en telemetrie stromen**. Met behulp van Event Hubs streamen, krijgt u uw logboeken metrische en diagnostische gegevens in verschillende oplossingen van derden controleren en log analytics. 

* **Een aangepaste Telemetrie-en logboekregistratie platform**. Als u al beschikken over een platform op maat gemaakte telemetrie of van plan bent te maken van een, de zeer schaalbare publish-subscribe aard van Event Hubs kunt u logboeken met diagnostische gegevens flexibel opnemen. Zie [Dan Rosanova van handleiding voor het gebruik van Event Hubs in een wereldwijde schaal telemetrie-platform](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-storage"></a>Stream naar Storage

De logboeken van de metrische gegevens en diagnostische gegevens van de SQL-Database kunnen worden opgeslagen in opslag met behulp van de ingebouwde **archiveren naar een opslagaccount** optie in de portal. U kunt ook Storage inschakelen met behulp van een diagnostische instelling via PowerShell-cmdlets, de Azure CLI of de Azure Monitor REST API.

### <a name="schema-of-metrics-and-diagnostics-logs-in-the-storage-account"></a>Registreert het schema van de metrische en diagnostische gegevens in het storage-account

Na het instellen van metrische gegevens en diagnostische gegevens over het verzamelen van Logboeken, wordt een storage-container gemaakt in het opslagaccount dat u hebt geselecteerd tijdens de eerste rijen met gegevens beschikbaar zijn. De structuur van deze blobs is:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
Of eenvoudiger gezegd:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Bijvoorbeeld, kan een blobnaam voor alle metrische gegevens zijn:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Als u wilt om vast te leggen van de gegevens van de elastische pool, is de blobnaam van de is iets anders:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-storage"></a>Metrische gegevens en logboeken downloaden uit Storage

Meer informatie over het [metrische en diagnostische gegevens logboeken downloaden uit Storage](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application).

## <a name="metrics-and-logs-available"></a>Metrische gegevens en logboeken beschikbaar

### <a name="all-metrics"></a>Alle metrische gegevens

|**Resource**|**Metrische gegevens**|
|---|---|
|Database|DTU-percentage, DTU gebruikt, DTU limiet, CPU-percentage, fysieke gegevens lezen percentage logboek schrijven percentage, geslaagd/mislukt/geblokkeerd door firewallverbindingen, sessies percentage, percentage van de werknemers, opslag, opslagpercentage, percentage van XTP-opslag, en impassen |
|Elastische pool|eDTU-percentage, eDTU gebruikt, eDTU-limiet, CPU-percentage, fysieke gegevens lezen percentage logboek schrijven percentage, sessies percentage, percentage van de werknemers, opslag, opslagpercentage, limiet voor opslag, XTP-opslagpercentage |
|||

### <a name="logs"></a>Logboeken

### <a name="query-store-runtime-statistics"></a>Query Store runtime-statistieken

|Eigenschap|Beschrijving|
|---|---|
|TenantId|Uw tenant-ID.|
|SourceSystem|Altijd: Azure|
|TimeGenerated [UTC]|Tijdstempel waarop het logboek is vastgelegd.|
|Type|Altijd: AzureDiagnostics|
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT. SQL|
|Categorie|De naam van de categorie. Altijd: QueryStoreRuntimeStatistics|
|OperationName|Naam van de bewerking. Altijd: QueryStoreRuntimeStatisticsEvent|
|Resource|Naam van de resource.|
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES|
|SubscriptionId|Abonnement-GUID die de database deel uitmaakt.|
|ResourceGroup|De naam van de resourcegroep die de database behoort.|
|LogicalServerName_s|De naam van de server die de database behoort.|
|ElasticPoolName_s|De naam van de elastische groep die de database deel uitmaakt, indien van toepassing.|
|DatabaseName_s|Naam van de database.|
|ResourceId|Resource-URI.|
|query_hash_s|Query uitvoeren op hash.|
|query_plan_hash_s|Queryhash-plan.|
|statement_sql_handle_s|Overzicht sql-ingang.|
|interval_start_time_d|Start datetimeoffset van het interval in aantal tikken vanaf 1900-1-1.|
|interval_end_time_d|Einde datetimeoffset van het aantal tikken vanaf 1900-1-1-interval.|
|logical_io_writes_d|Totaal aantal logische i/o-schrijfbewerkingen.|
|max_logical_io_writes_d|Maximumaantal logische i/o-schrijfbewerkingen per uitvoering.|
|physical_io_reads_d|Totaal aantal fysieke i/o-leesbewerkingen.|
|max_physical_io_reads_d|Maximumaantal logische i/o-leesbewerkingen per uitvoering.|
|logical_io_reads_d|Totaal aantal logische i/o-leesbewerkingen.|
|max_logical_io_reads_d|Maximumaantal logische i/o-leesbewerkingen per uitvoering.|
|execution_type_d|Uitvoering van het type.|
|count_executions_d|Het aantal uitvoeringen van de query.|
|cpu_time_d|Totale CPU-tijd gebruikt door de query in microseconden.|
|max_cpu_time_d|Maximaal CPU tijd consumenten door één uitvoering in microseconden.|
|dop_d|Som van de mate van parallelle uitvoering.|
|max_dop_d|Maximale graad van parallelle uitvoering voor één uitvoering gebruikt.|
|rowcount_d|Totaal aantal rijen dat wordt geretourneerd.|
|max_rowcount_d|Maximumaantal rijen dat wordt geretourneerd in één uitvoering.|
|query_max_used_memory_d|Totale hoeveelheid geheugen die wordt gebruikt in KB.|
|max_query_max_used_memory_d|Maximale hoeveelheid geheugen die wordt gebruikt door één uitvoering in KB.|
|duration_d|Totale uitvoeringstijd in microseconden.|
|max_duration_d|Maximale uitvoeringstijd van één uitvoering.|
|num_physical_io_reads_d|Totaal aantal fysieke leesbewerkingen.|
|max_num_physical_io_reads_d|Maximumaantal fysieke leesbewerkingen per uitvoering.|
|log_bytes_used_d|Totale hoeveelheid log bytes dat wordt gebruikt.|
|max_log_bytes_used_d|Maximale hoeveelheid log bytes per uitvoering gebruikt.|
|query_id_d|ID van de query in Query Store.|
|plan_id_d|ID van het plan in Query Store.|

Meer informatie over [gegevens voor Query Store runtime-statistieken](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql).

### <a name="query-store-wait-statistics"></a>Query Store wacht statistieken

|Eigenschap|Beschrijving|
|---|---|
|TenantId|Uw tenant-ID.|
|SourceSystem|Altijd: Azure|
|TimeGenerated [UTC]|Tijdstempel waarop het logboek is vastgelegd.|
|Type|Altijd: AzureDiagnostics|
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT. SQL|
|Categorie|De naam van de categorie. Altijd: QueryStoreWaitStatistics|
|OperationName|Naam van de bewerking. Altijd: QueryStoreWaitStatisticsEvent|
|Resource|De naam van de resource|
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES|
|SubscriptionId|Abonnement-GUID die de database deel uitmaakt.|
|ResourceGroup|De naam van de resourcegroep die de database behoort.|
|LogicalServerName_s|De naam van de server die de database behoort.|
|ElasticPoolName_s|De naam van de elastische groep die de database deel uitmaakt, indien van toepassing.|
|DatabaseName_s|Naam van de database.|
|ResourceId|Resource-URI.|
|wait_category_s|De categorie van de wachttijd.|
|is_parameterizable_s|Is de query worden gebruikt als parameter.|
|statement_type_s|Het type van de instructie.|
|statement_key_hash_s|Instructie sleutel-hash.|
|exec_type_d|Het type kan worden uitgevoerd.|
|total_query_wait_time_ms_d|De totale wachttijd van de query op de specifieke wait-categorie.|
|max_query_wait_time_ms_d|Maximale wachttijd van de query in afzonderlijke uitvoering in de categorie specifieke wachten.|
|query_param_type_d|0|
|query_hash_s|Query uitvoeren op hash in Query Store.|
|query_plan_hash_s|Queryhash-plan in Query Store.|
|statement_sql_handle_s|Ingang voor de instructie in Query Store.|
|interval_start_time_d|Start datetimeoffset van het interval in aantal tikken vanaf 1900-1-1.|
|interval_end_time_d|Einde datetimeoffset van het aantal tikken vanaf 1900-1-1-interval.|
|count_executions_d|Het aantal uitvoeringen van de query.|
|query_id_d|ID van de query in Query Store.|
|plan_id_d|ID van het plan in Query Store.|

Meer informatie over [Query Store wacht statistiekgegevens](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql).

### <a name="errors-dataset"></a>Gegevensset voor fouten

|Eigenschap|Beschrijving|
|---|---|
|TenantId|Uw tenant-ID.|
|SourceSystem|Altijd: Azure|
|TimeGenerated [UTC]|Tijdstempel waarop het logboek is vastgelegd.|
|Type|Altijd: AzureDiagnostics|
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT. SQL|
|Categorie|De naam van de categorie. Altijd: fouten|
|OperationName|Naam van de bewerking. Altijd: ErrorEvent|
|Resource|De naam van de resource|
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES|
|SubscriptionId|Abonnement-GUID die de database deel uitmaakt.|
|ResourceGroup|De naam van de resourcegroep die de database behoort.|
|LogicalServerName_s|De naam van de server die de database behoort.|
|ElasticPoolName_s|De naam van de elastische groep die de database deel uitmaakt, indien van toepassing.|
|DatabaseName_s|Naam van de database.|
|ResourceId|Resource-URI.|
|Bericht|Foutbericht als tekst zonder opmaak.|
|user_defined_b|De gebruiker gedefinieerde fout bits is.|
|error_number_d|Foutcode.|
|Severity|Ernst van de fout.|
|state_d|Status van de fout.|
|query_hash_s|Queryhash van de mislukte query, indien beschikbaar.|
|query_plan_hash_s|Query-plan-hash van de mislukte query, indien beschikbaar.|

Meer informatie over [SQL Server-foutberichten](https://msdn.microsoft.com/library/cc645603.aspx).

### <a name="database-wait-statistics-dataset"></a>Database wacht statistieken gegevensset

|Eigenschap|Beschrijving|
|---|---|
|TenantId|Uw tenant-ID.|
|SourceSystem|Altijd: Azure|
|TimeGenerated [UTC]|Tijdstempel waarop het logboek is vastgelegd.|
|Type|Altijd: AzureDiagnostics|
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT. SQL|
|Categorie|De naam van de categorie. Altijd: DatabaseWaitStatistics|
|OperationName|Naam van de bewerking. Altijd: DatabaseWaitStatisticsEvent|
|Resource|De naam van de resource|
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES|
|SubscriptionId|Abonnement-GUID die de database deel uitmaakt.|
|ResourceGroup|De naam van de resourcegroep die de database behoort.|
|LogicalServerName_s|De naam van de server die de database behoort.|
|ElasticPoolName_s|De naam van de elastische groep die de database deel uitmaakt, indien van toepassing.|
|DatabaseName_s|Naam van de database.|
|ResourceId|Resource-URI.|
|wait_type_s|Naam van het type wachten.|
|start_utc_date_t [UTC]|Begintijd van de periode tijd wordt gemeten.|
|end_utc_date_t [UTC]|Einde van de periode tijd wordt gemeten.|
|delta_max_wait_time_ms_d|Maximum aantal keer per uitvoering gewacht|
|delta_signal_wait_time_ms_d|Totaal aantal signaal wachttijd.|
|delta_wait_time_ms_d|De totale wachttijd in de periode.|
|delta_waiting_tasks_count_d|Het aantal taken wachten.|

Meer informatie over [wacht statistieken van de database](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql).

### <a name="time-outs-dataset"></a>Time-outs gegevensset

|Eigenschap|Beschrijving|
|---|---|
|TenantId|Uw tenant-ID.|
|SourceSystem|Altijd: Azure|
|TimeGenerated [UTC]|Tijdstempel waarop het logboek is vastgelegd.|
|Type|Altijd: AzureDiagnostics|
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT. SQL|
|Categorie|De naam van de categorie. Altijd: time-outs|
|OperationName|Naam van de bewerking. Altijd: TimeoutEvent|
|Resource|De naam van de resource|
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES|
|SubscriptionId|Abonnement-GUID die de database deel uitmaakt.|
|ResourceGroup|De naam van de resourcegroep die de database behoort.|
|LogicalServerName_s|De naam van de server die de database behoort.|
|ElasticPoolName_s|De naam van de elastische groep die de database deel uitmaakt, indien van toepassing.|
|DatabaseName_s|Naam van de database.|
|ResourceId|Resource-URI.|
|error_state_d|Status van foutcode.|
|query_hash_s|Query-hash, indien beschikbaar.|
|query_plan_hash_s|Query uitvoeren op hash van de planning, indien beschikbaar.|

### <a name="blockings-dataset"></a>Blockings gegevensset

|Eigenschap|Beschrijving|
|---|---|
|TenantId|Uw tenant-ID.|
|SourceSystem|Altijd: Azure|
|TimeGenerated [UTC]|Tijdstempel waarop het logboek is vastgelegd.|
|Type|Altijd: AzureDiagnostics|
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT. SQL|
|Categorie|De naam van de categorie. Altijd: blokken|
|OperationName|Naam van de bewerking. Altijd: BlockEvent|
|Resource|De naam van de resource|
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES|
|SubscriptionId|Abonnement-GUID die de database deel uitmaakt.|
|ResourceGroup|De naam van de resourcegroep die de database behoort.|
|LogicalServerName_s|De naam van de server die de database behoort.|
|ElasticPoolName_s|De naam van de elastische groep die de database deel uitmaakt, indien van toepassing.|
|DatabaseName_s|Naam van de database.|
|ResourceId|Resource-URI.|
|lock_mode_s|LOCK-modus gebruikt door de query.|
|resource_owner_type_s|Eigenaar van de vergrendeling.|
|blocked_process_filtered_s|Proces rapport XML geblokkeerd.|
|duration_d|De duur van de vergrendeling in microseconden.|

### <a name="deadlocks-dataset"></a>Impassen gegevensset

|Eigenschap|Beschrijving|
|---|---|
|TenantId|Uw tenant-ID.|
|SourceSystem|Altijd: Azure|
|TimeGenerated [UTC] |Tijdstempel waarop het logboek is vastgelegd.|
|Type|Altijd: AzureDiagnostics|
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT. SQL|
|Categorie|De naam van de categorie. Altijd: impassen|
|OperationName|Naam van de bewerking. Altijd: DeadlockEvent|
|Resource|Naam van de resource.|
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES|
|SubscriptionId|Abonnement-GUID die de database deel uitmaakt.|
|ResourceGroup|De naam van de resourcegroep die de database behoort.|
|LogicalServerName_s|De naam van de server die de database behoort.|
|ElasticPoolName_s|De naam van de elastische groep die de database deel uitmaakt, indien van toepassing.|
|DatabaseName_s|Naam van de database. |
|ResourceId|Resource-URI.|
|deadlock_xml_s|Impasse rapport XML.|

### <a name="automatic-tuning-dataset"></a>Automatische afstemming gegevensset

|Eigenschap|Beschrijving|
|---|---|
|TenantId|Uw tenant-ID.|
|SourceSystem|Altijd: Azure|
|TimeGenerated [UTC]|Tijdstempel waarop het logboek is vastgelegd.|
|Type|Altijd: AzureDiagnostics|
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT. SQL|
|Categorie|De naam van de categorie. Altijd: AutomaticTuning|
|Resource|Naam van de resource.|
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES|
|SubscriptionId|Abonnement-GUID die de database deel uitmaakt.|
|ResourceGroup|De naam van de resourcegroep die de database behoort.|
|LogicalServerName_s|De naam van de server die de database behoort.|
|LogicalDatabaseName_s|Naam van de database.|
|ElasticPoolName_s|De naam van de elastische groep die de database deel uitmaakt, indien van toepassing.|
|DatabaseName_s|Naam van de database.|
|ResourceId|Resource-URI.|
|RecommendationHash_s|De unieke hash van de aanbeveling voor automatisch afstemmen.|
|OptionName_s|Bewerking voor automatisch afstemmen.|
|Schema_s|Databaseschema.|
|Table_s|De tabel is beïnvloed.|
|IndexName_s|Naam van de index.|
|IndexColumns_s|De naam van de kolom.|
|IncludedColumns_s|Kolommen die zijn opgenomen.|
|EstimatedImpact_s|Geschatte invloed van automatisch afstemmen aanbeveling JSON.|
|Event_s|Type gebeurtenis voor automatisch afstemmen.|
|Timestamp_t|De laatst bijgewerkte timestamp.|

### <a name="intelligent-insights-dataset"></a>Intelligent Insights-gegevensset
Meer informatie over de [Intelligent Insights-logboekindeling](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het inschakelen van logboekregistratie en begrijpen van de metrische gegevens en logboekbestanden categorieën ondersteund door de verschillende Azure-services, lezen:

 * [Overzicht van metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
 * [Overzicht van diagnostische logboeken van Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)

Voor meer informatie over Event Hubs, lezen:

* [Wat is Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
* [Aan de slag met Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Zie voor meer informatie over opslag, hoe u [metrische en diagnostische gegevens logboeken downloaden uit Storage](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application).
