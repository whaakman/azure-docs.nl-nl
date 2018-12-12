---
title: Metrische gegevens van Azure SQL database en diagnostische logboekregistratie | Microsoft Docs
description: Meer informatie over het configureren van Azure SQL Database voor het opslaan van Resourcegebruik, connectiviteit en Uitvoeringsstatistieken query.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: 19117803783047d8ddd9740c799b4cb81fd74c2c
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2018
ms.locfileid: "52890905"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Metrische gegevens van Azure SQL-Database en logboekregistratie van diagnostische gegevens 

Azure SQL Database, elastische pools Managed Instance en databases in Managed Instance kunnen verzenden van metrische gegevens en diagnostische logboeken voor prestatiebewaking van eenvoudiger. U kunt een database naar stream Resourcegebruik, werkrollen en sessies en connectiviteit in een van deze Azure-resources configureren:

* **Azure SQL Analytics**: gebruikt als intelligente geïntegreerde Azure-database-prestaties met rapportages, waarschuwingen en risicobeperking mogelijkheden voor controle.
* **Azure Event Hubs**: gebruikt voor het integreren van SQL Database-telemetrie in uw eigen bewakingsoplossing of actieve pijplijnen.
* **Azure Storage**: gebruikt voor het archiveren van grote hoeveelheden telemetriegegevens voor een fractie van de prijs.

    ![Architectuur](./media/sql-database-metrics-diag-logging/architecture.png)

Om te begrijpen van de metrische gegevens en meld u categorieën die worden ondersteund door de verschillende Azure-services, kunt u rekening houden met het lezen:

* [Overzicht van metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
* [Overzicht van diagnostische logboeken van Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) 

## <a name="enable-logging-of-diagnostics-telemetry"></a>Logboekregistratie van diagnostische gegevens telemetrie inschakelen

Gebruik de eerste sectie van dit document om in te schakelen van diagnostische gegevens telemetrie voor databases en het tweede gedeelte van het document om in te schakelen van diagnostische gegevens telemetrie voor elastische pools of beheerde instanties. Gebruik de latere secties van dit document Azure SQL Analytics configureren als een hulpprogramma voor bewaking voor het weergeven van de telemetrie van de diagnostische gegevens gestreamde-database.

> [!NOTE]
> Als u gebruikmaakt van elastische pools of beheerde instanties, naast het inschakelen van diagnostische gegevens telemetrie voor databases, kunt u wordt ook aangeraden om in te schakelen van diagnostische gegevens telemetrie voor deze resources ook. Dit komt doordat elastische pools en beheerde instanties in de rol van de database containers hebben een eigen diagnostische gegevens telemetrie die gescheiden van een afzonderlijke database-telemetrie voor diagnostische gegevens is. 
>

U kunt inschakelen en metrische gegevens en logboekregistratie van diagnostische gegevens telemetrie beheren met behulp van een van de volgende methoden:

- Azure Portal
- PowerShell
- Azure-CLI
- Azure Monitor REST-API 
- Azure Resource Manager-sjabloon

Wanneer u metrische gegevens en logboekregistratie van diagnostische gegevens inschakelt, moet u de Azure-resource opgeven waar de geselecteerde gegevens worden verzameld. Beschikbare opties zijn onder andere:

- Azure SQL-analyse
- Azure Event Hubs
- Azure Storage

U kunt inrichten van een nieuwe Azure-resource of Selecteer een bestaande resource. Na het selecteren van een resource, met behulp van de optie diagnostische instellingen, moet u opgeven welke gegevens worden verzameld.

## <a name="enable-logging-for-azure-sql-database-or-databases-in-managed-instance"></a>Logboekregistratie inschakelen voor Azure SQL Database of databases in het beheerde exemplaar

Metrische en diagnostische gegevens die zich aanmelden op de SQL-Database en -databases in het beheerde exemplaar zijn niet standaard ingeschakeld.

De volgende diagnostische gegevens telemetrie is beschikbaar voor de verzameling voor Azure SQL-Databases en databases in het beheerde exemplaar:

| Telemetrie voor databases bewaken | Ondersteuning van Azure SQL-Database | Database in de Managed Instance-ondersteuning |
| :------------------- | ------------------- | ------------------- |
| [Alle metrische gegevens](sql-database-metrics-diag-logging.md#all-metrics): bevat DTU-/ CPU-percentage, DTU/CPU beperken, fysieke gegevens gelezen percentage logboek schrijven percentage geslaagd/mislukt/geblokkeerd door firewallverbindingen, sessies percentage, percentage van de werknemers, opslag, opslagpercentage, en Percentage van XTP-opslag. | Ja | Nee |
| [QueryStoreRuntimeStatistics](sql-database-metrics-diag-logging.md#query-store-runtime-statistics): bevat informatie over de query-runtime-statistieken, zoals CPU-gebruik zijn en duur van de statistieken op te vragen. | Ja | Ja |
| [QueryStoreWaitStatistics](sql-database-metrics-diag-logging.md#query-store-wait-statistics): bevat informatie over de query wait-statistieken, dit geeft aan wat uw query's wachtte op, zoals CPU, het logboek en VERGRENDELEN. | Ja | Ja |
| [Fouten](sql-database-metrics-diag-logging.md#errors-dataset): bevat informatie over SQL-fouten die hebben plaatsgevonden voor deze database. | Ja | Nee |
| [DatabaseWaitStatistics](sql-database-metrics-diag-logging.md#database-wait-statistics-dataset): bevat informatie over hoeveel tijd een database besteed aan het wachten op andere wacht typen. | Ja | Nee |
| [Time-outs](sql-database-metrics-diag-logging.md#time-outs-dataset): bevat informatie over time-outs die hebben plaatsgevonden in een database. | Ja | Nee |
| [Blokken](sql-database-metrics-diag-logging.md#blockings-dataset): bevat informatie over het blokkeren van gebeurtenissen die hebben plaatsgevonden in een database. | Ja | Nee |
| [SQLInsights](sql-database-metrics-diag-logging.md#intelligent-insights-dataset): Intelligent Insights bevat in prestaties. [Meer informatie over Intelligent Insights](sql-database-intelligent-insights.md). | Ja | Ja |

### <a name="azure-portal"></a>Azure Portal

Streaming van diagnostische gegevens telemetrie voor Azure SQL Database en -databases in het beheerde exemplaar naar bestemmingen van Azure storage, wordt eventhubs of Log Analytics geconfigureerd via menu van de instellingen voor diagnostische gegevens voor elk van de databases in Azure portal.

### <a name="configure-streaming-of-diagnostics-telemetry-for-azure-sql-database"></a>Streaming van diagnostische gegevens telemetrie voor Azure SQL Database configureren

   ![Pictogram van een SQL-Database](./media/sql-database-metrics-diag-logging/icon-sql-database-text.png)

Om in te schakelen van diagnostische gegevens telemetrie voor streaming **Azure SQL Database**, als volgt te werk:

1. Ga naar uw Azure SQL Database-bron
2. Selecteer **diagnostische instellingen**
3. Selecteer **diagnostische gegevens inschakelen** als er geen vorige instellingen bestaat, of selecteer **instelling bewerken** een vorige instelling bewerken
- Maximaal drie (3) parallelle verbindingen met stream diagnostische gegevens telemetrie kan worden gemaakt. Selecteer voor het configureren van meerdere parallelle streaming van diagnostische gegevens naar meerdere resources, **+ diagnostische instelling toevoegen** om een extra instelling te maken.

   ![Diagnostische gegevens inschakelen voor SQL-Database](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-enable.png)

4. Typ de naam voor de instelling: voor eigen referentie
5. Selecteer naar welke resource moet worden stream diagnostische gegevens uit de database: **archiveren naar opslagaccount**, **Stream naar een event hub**, of **verzenden naar Log Analytics**
6. Voor standard bewakingservaring, schakel selectievakjes in voor databasetelemetrie diagnostics log: **SQLInsights**, **AutomaticTuning**, **QueryStoreRuntimeStatistics** , **QueryStoreWaitStatistics**, **fouten**, **DatabaseWaitStatistics**, **time-outs**, **blokken** , **Impassen**. Deze telemetrische gegevens is een gebeurtenis op basis van en de standaard controle-ervaring biedt.
7. Voor geavanceerde bewakingservaring selectievakje voor **AllMetrics**. Dit is een 1 minuut op basis van telemetrie voor de database-telemetrie voor diagnostische gegevens, zoals hierboven beschreven. 
8. Klik op **Opslaan**

   ![Diagnostische gegevens voor SQL Database configureren](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-selection.png)

> [!NOTE]
> Beveiligingscontrole logboeken zijn kan niet worden ingeschakeld vanuit database diagnostische instellingen. Inschakelen van controle logboekstreaming [controle voor uw database instellen](sql-database-auditing.md#subheading-2), en ziet ook [SQL auditlogboeken beschikbaar zijn in Azure Log Analytics en Azure Event Hubs](https://blogs.msdn.microsoft.com/sqlsecurity/2018/09/13/sql-audit-logs-in-azure-log-analytics-and-azure-event-hubs/).
>

> [!TIP]
> Herhaal de bovenstaande stappen voor elke Azure SQL-Database die u wilt bewaken. 
>

### <a name="configure-streaming-of-diagnostics-telemetry-for-databases-in-managed-instance"></a>Configureer streaming van diagnostische gegevens telemetrie voor databases in het beheerde exemplaar

   ![Database in de Managed Instance-pictogram](./media/sql-database-metrics-diag-logging/icon-mi-database-text.png)

Om in te schakelen van diagnostische gegevens telemetrie voor streaming **databases in het beheerde exemplaar**, als volgt te werk:

1. Ga naar de database in het beheerde exemplaar
2. Selecteer **diagnostische instellingen**
3. Selecteer **diagnostische gegevens inschakelen** als er geen vorige instellingen bestaat, of selecteer **instelling bewerken** een vorige instelling bewerken
- Maximaal drie (3) parallelle verbindingen met stream diagnostische gegevens telemetrie kan worden gemaakt. Selecteer voor het configureren van meerdere parallelle streaming van diagnostische gegevens naar meerdere resources, **+ diagnostische instelling toevoegen** om een extra instelling te maken.

   ![Diagnostische gegevens voor beheerd exemplaar van database inschakelen](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-enable.png)

4. Typ de naam voor de instelling: voor eigen referentie
5. Selecteer naar welke resource moet worden stream diagnostische gegevens uit de database: **archiveren naar opslagaccount**, **Stream naar een event hub**, of **verzenden naar Log Analytics**
6. Schakel de selectievakjes voor databasetelemetrie diagnostische gegevens: **SQLInsights**, **QueryStoreRuntimeStatistics**, **QueryStoreWaitStatistics** en **fouten**
7. Klik op **Opslaan**

   ![Diagnostische gegevens voor beheerd exemplaar in de database configureren](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-selection.png)

> [!TIP]
> Herhaal de bovenstaande stappen voor elke database in het beheerde exemplaar u wilt bewaken.
>

## <a name="enable-logging-for-elastic-pools-or-managed-instance"></a>Logboekregistratie inschakelen voor elastische pools of Managed Instance

Elastische pools en beheerde instanties als database containers hebben een eigen diagnostische telemetrie van afzonderlijke uit databases. Deze diagnostische gegevens telemetrie is niet standaard ingeschakeld. 

### <a name="configure-streaming-of-diagnostics-telemetry-for-elastic-pools"></a>Streaming van diagnostische gegevens telemetrie voor elastische pools configureren

   ![Elastische pool-pictogram](./media/sql-database-metrics-diag-logging/icon-elastic-pool-text.png)

De volgende diagnostische gegevens telemetrie is beschikbaar voor de verzameling voor elastische pools resource:

| Resource | Bewaking van telemetrie |
| :------------------- | ------------------- |
| **Elastische pool** | [Alle metrische gegevens](sql-database-metrics-diag-logging.md#all-metrics) bevat eDTU/CPU-percentage, eDTU/CPU-limiet, fysieke gegevens gelezen percentage, logboek schrijven percentage, sessies percentage, percentage van de werknemers, opslag, opslagpercentage, limiet voor opslag en XTP-opslagpercentage. |

Om in te schakelen van diagnostische gegevens telemetrie voor streaming **elastische groep**, als volgt te werk:

1. Ga naar de elastische pool-resource in Azure portal
2. Selecteer **diagnostische instellingen**
3. Selecteer **diagnostische gegevens inschakelen** als er geen vorige instellingen bestaat, of selecteer **instelling bewerken** een vorige instelling bewerken

   ![Diagnostische gegevens voor elastische pools inschakelen](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-enable.png)

4. Typ de naam voor de instelling: voor eigen referentie
5. Selecteer naar welke resource moet worden stream diagnostische gegevens van de elastische pool: **archiveren naar opslagaccount**, **Stream naar een event hub**, of **verzenden naar Log Analytics**
6. Selecteer in het geval Log Analytics is ingeschakeld, **configureren** en een nieuwe werkruimte maken door te selecteren **+ nieuwe werkruimte maken**, of Selecteer een bestaande werkruimte
7. Schakel het selectievakje voor diagnostische gegevens telemetrie voor elastische Pools **AllMetrics**
8. Klik op **Opslaan**.

   ![Diagnostische gegevens voor elastische pools configureren](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-selection.png)

> [!TIP]
> Herhaal de bovenstaande stappen voor elke elastische groep die u wilt bewaken.
>

### <a name="configure-streaming-of-diagnostics-telemetry-for-managed-instance"></a>Configureer streaming van diagnostische gegevens telemetrie voor beheerd exemplaar

   ![Pictogram van het beheerde exemplaar](./media/sql-database-metrics-diag-logging/icon-managed-instance-text.png)

De volgende diagnostische gegevens telemetrie is beschikbaar voor de verzameling voor de Managed Instance-resource:

| Resource | Bewaking van telemetrie |
| :------------------- | ------------------- |
| **Beheerd exemplaar** | [ResourceUsageStats](sql-database-metrics-diag-logging.md#resource-usage-stats) vCores aantal, gemiddelde CPU-percentage, i/o-aanvragen, lezen/geschreven bytes bevat, gereserveerde opslagruimte, opslagruimte gebruikt. |

Om in te schakelen van diagnostische gegevens telemetrie voor streaming **Managed Instance resource**, als volgt te werk:

1. Ga naar de Managed Instance-resource in Azure portal
2. Selecteer **diagnostische instellingen**
3. Selecteer **diagnostische gegevens inschakelen** als er geen vorige instellingen bestaat, of selecteer **instelling bewerken** een vorige instelling bewerken

   ![Diagnostische gegevens inschakelen voor beheerd exemplaar](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-enable.png)

4. Typ de naam voor de instelling: voor eigen referentie
5. Selecteer naar welke resource moet worden stream diagnostische gegevens van de elastische pool: **archiveren naar opslagaccount**, **Stream naar een event hub**, of **verzenden naar Log Analytics**
6. In het geval Log Analytics is ingeschakeld, maken of een bestaande werkruimte gebruiken
7. Schakel het selectievakje in voor diagnostische gegevens telemetrie **ResourceUsageStats**
8. Klik op **Opslaan**.

   ![Diagnostische gegevens configureren voor Managed Instance](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-selection.png)

> [!TIP]
> Herhaal de bovenstaande stappen voor elk beheerd exemplaar u wilt bewaken.
>

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

## <a name="stream-into-azure-sql-analytics"></a>Stream in Azure SQL-analyse 

Azure SQL Analytics is een cloud voor controle voor het bewaken van de prestaties van Azure SQL-databases, elastische pools en beheerde instanties op schaal en voor meerdere abonnementen via één venster inzicht bedrijfsanalyses. Er worden verzameld en worden gevisualiseerd met belangrijke metrische gegevens voor Azure SQL Database-prestaties met ingebouwde intelligentie voor het oplossen van prestaties.

![Overzicht van Azure SQL Analytics](../azure-monitor/insights/media/azure-sql/azure-sql-sol-overview.png)

De logboeken van de metrische gegevens en diagnostische gegevens van de SQL-Database kunnen worden gestreamd naar Azure SQL Analytics met behulp van de ingebouwde **verzenden naar Log Analytics** optie in de instellingenblade van diagnostische gegevens in de portal. U kunt ook Log Analytics inschakelen met behulp van een diagnostische instelling via PowerShell-cmdlets, de Azure CLI of de Azure Monitor REST API.

### <a name="installation-overview"></a>Installatie-overzicht

Bewaking van de vloot van een SQL-Database is heel eenvoudig met Azure SQL Analytics. Er zijn drie stappen vereist:

1. Azure SQL Analytics-oplossing maken vanuit Azure Marketplace
2. Werkruimte voor bewaking in de oplossing maken
3. Configureer databases naar stream diagnostische gegevens telemetrie in de werkruimte die u hebt gemaakt.

Als u van elastische pools gebruikmaakt of beheerde instanties, naast het database-telemetrie voor diagnostische gegevens, configureren van diagnostische gegevens telemetrie van deze resources ook streaming.

### <a name="create-azure-sql-analytics-resource"></a>Azure SQL Analytics-resource maken

1. Zoeken naar Azure SQL Analytics in Azure Marketplace en selecteert u deze

   ![Zoeken naar Azure SQL Analytics in portal](./media/sql-database-metrics-diag-logging/sql-analytics-in-marketplace.png)
   
2. Selecteer **maken** op het scherm van de oplossing-overzicht

3. Vul het formulier Azure SQL-analyse met de aanvullende informatie die is vereist: naam van de werkruimte, abonnement, resourcegroep, locatie en prijscategorie.
 
   ![Configureren van Azure SQL Analytics in portal](./media/sql-database-metrics-diag-logging/sql-analytics-configuration-blade.png)

4. Om te bevestigen **OK**, en voltooien door het selecteren van **maken**

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>Databases naar Logboeken voor records metrische en diagnostische gegevens configureren

De eenvoudigste manier om te configureren waarin de hun metrische gegevens voor het registreren van databases is via de Azure-portal - zoals hierboven is beschreven. In de portal, gaat u naar de resource van uw SQL-Database en selecteer **diagnostische instellingen**.

In het geval u Elastische pools of beheerde instanties worden gebruikt, moet u ook het configureren van instellingen voor diagnostische gegevens van deze resources ook aan hun eigen telemetrie van Azure diagnostics streamen naar de werkruimte die u hebt gemaakt.

### <a name="use-the-sql-analytics-solution"></a>De oplossing SQL Analytics gebruiken

SQL-analyse is een hiërarchische dashboard waarmee u de hiërarchie van SQL Database-resources te doorlopen. Zie voor meer informatie over het gebruik van de oplossing SQL Analytics, [SQL-Database controleren met behulp van de oplossing SQL Analytics](../azure-monitor/insights/azure-sql.md).

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

## <a name="data-retention-policy-and-pricing"></a>Bewaarbeleid voor gegevens en prijzen

Als u Event Hubs of een storage-account selecteert, kunt u een bewaarbeleid opgeven. Dit beleid verwijdert de gegevens die ouder is dan een geselecteerde periode. Als u Log Analytics opgeeft, is het bewaarbeleid afhankelijk van de geselecteerde prijscategorie. Verbruik van diagnostische gegevens telemetrie boven de gratis eenheden van gegevensopname die elke maand toegewezen is van toepassing. Gratis eenheden van de gegevensopname opgegeven gratis bewaking inschakelen van meerdere databases per maand. Houd er rekening mee dat meer actieve databases met zwaardere werkbelastingen zal worden gebruikt voor het opnemen van meer gegevens ten opzichte van niet-actieve databases. Zie voor meer informatie, [Log Analytics-prijzen](https://azure.microsoft.com/pricing/details/monitor/). 

Als u van Azure SQL Analytics gebruikmaakt, kunt u eenvoudig uw gegevensverbruik voor opname in de oplossing bewaken met OMS-werkruimte in het navigatiemenu van Azure SQL Analytics selecteren en vervolgens de optie gebruik en geschatte kosten.

## <a name="metrics-and-logs-available"></a>Metrische gegevens en logboeken beschikbaar

Verzamelde bewaking telemetrie kan worden gebruikt voor uw eigen **aangepaste analyse** en **toepassingsontwikkeling** met behulp van [SQL Analytics-taal](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries). Structuur van de verzamelde gegevens, metrische gegevens en zich aanmeldt, wordt hieronder weergegeven.

## <a name="all-metrics"></a>Alle metrische gegevens

### <a name="all-metrics-for-elastic-pools"></a>Alle metrische gegevens voor elastische pools

|**Resource**|**Metrische gegevens**|
|---|---|
|Elastische pool|eDTU-percentage, eDTU gebruikt, eDTU-limiet, CPU-percentage, fysieke gegevens lezen percentage logboek schrijven percentage, sessies percentage, percentage van de werknemers, opslag, opslagpercentage, limiet voor opslag, XTP-opslagpercentage |

### <a name="all-metrics-for-azure-sql-database"></a>Alle metrische gegevens voor Azure SQL Database

|**Resource**|**Metrische gegevens**|
|---|---|
|Azure SQL Database|DTU-percentage, DTU gebruikt, DTU limiet, CPU-percentage, fysieke gegevens lezen percentage logboek schrijven percentage, geslaagd/mislukt/geblokkeerd door firewallverbindingen, sessies percentage, percentage van de werknemers, opslag, opslagpercentage, percentage van XTP-opslag, en impassen |

## <a name="logs"></a>Logboeken

### <a name="logs-for-managed-instance"></a>Logboeken voor het beheerde exemplaar

### <a name="resource-usage-stats"></a>Statistieken voor het gebruik van resource

|Eigenschap|Beschrijving|
|---|---|
|TenantId|Uw tenant-ID.|
|SourceSystem|Altijd: Azure|
|TimeGenerated [UTC]|Tijdstempel waarop het logboek is vastgelegd.|
|Type|Altijd: AzureDiagnostics|
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT. SQL|
|Categorie|De naam van de categorie. Altijd: ResourceUsageStats|
|Resource|Naam van de resource.|
|ResourceType|De naam van het resourcetype. Altijd: MANAGEDINSTANCES|
|SubscriptionId|Abonnement-GUID die de database deel uitmaakt.|
|ResourceGroup|De naam van de resourcegroep die de database behoort.|
|LogicalServerName_s|Naam van het beheerde exemplaar.|
|ResourceId|Resource-URI.|
|SKU_s|Beheerde exemplaar product-SKU|
|virtual_core_count_s|Numver van vCores beschikbaar|
|avg_cpu_percent_s|Gemiddelde CPU-percentage|
|reserved_storage_mb_s|Gereserveerde opslagcapaciteit op Managed Instance|
|storage_space_used_mb_s|Gebruikte opslag op Managed Instance|
|io_requests_s|Aantal IOPS|
|io_bytes_read_s|Gelezen IOP's bytes|
|io_bytes_written_s|Aantal geschreven bytes van IOPS|

### <a name="logs-for-azure-sql-database-and-managed-instance-database"></a>Logboeken voor Azure SQL Database en het beheerde exemplaar van database

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
|IndexColumns_s|Kolomnaam.|
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
