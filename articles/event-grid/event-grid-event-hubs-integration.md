---
title: Integratie van Azure Event raster en Event Hubs
description: Hierin wordt beschreven hoe u Azure gebeurtenis raster en Event Hubs gebruiken om gegevens te migreren naar een SQL Data Warehouse
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/19/2018
ms.author: tomfitz
ms.openlocfilehash: b315bd77a47a6f106c5768da56828a5169de5fe9
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="stream-big-data-into-a-data-warehouse"></a>Stroom big data in een datawarehouse

Azure [gebeurtenis raster](overview.md) is een intelligente gebeurtenis routing-service waarmee u om te reageren op meldingen van apps en services. De [Event Hubs vastleggen en gebeurtenis raster voorbeeld](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) laat zien hoe Azure Event Hubs vastleggen gebruiken met Azure Event raster naadloos om gegevens te migreren van een event hub naar een SQL Data Warehouse.

![Toepassingsoverzicht van de](media/event-grid-event-hubs-integration/overview.png)

Wanneer gegevens worden verzonden naar de event hub, wordt het vastleggen gegevens ophaalt uit de stroom en storage-blobs met de gegevens worden gegenereerd in het Avro-indeling. Bij het vastleggen van de blob genereert, wordt een gebeurtenis geactiveerd. Gebeurtenis raster verdeelt gegevens over de gebeurtenis naar abonnees. In dit geval wordt de gebeurtenisgegevens verzonden naar de Azure Functions-eindpunt. De gebeurtenisgegevens bevat het pad van de gegenereerde blob. De URL wordt gebruikt voor het ophalen van het bestand en dit te verzenden naar het datawarehouse.

In dit artikel leert u:

* De volgende infrastructuur implementeren:
  * Event hub, met vastleggen is ingeschakeld
  * Storage-account voor de bestanden van vastleggen
  * Azure app service-plan voor het hosten van de functie-app
  * Functie-app voor het verwerken van de gebeurtenis
  * SQL Server voor het hosten van het datawarehouse
  * SQL Data Warehouse voor het opslaan van de gemigreerde gegevens
* Een tabel maken in het datawarehouse
* Voeg code toe aan de functie-app
* Abonneren op de gebeurtenis
* App uitvoert die gegevens naar de event hub verzendt
* Gemigreerde gegevens weergeven in het datawarehouse

## <a name="about-the-event-data"></a>Over de gebeurtenisgegevens

Gebeurtenis raster distribueert gebeurtenisgegevens naar de abonnees. Het volgende voorbeeld ziet gegevens van gebeurtenissen voor het maken van een bestand opnemen. In het bijzonder, zoals u ziet de `fileUrl` eigenschap in de `data` object. De functie-app deze waarde wordt opgehaald en wordt gebruikt voor het ophalen van het bestand vastleggen.

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        }
    }
]
```

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt voltooid, moet u het volgende hebben:

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
* [Visual studio 2017 versie 15.3.2 of hoger](https://www.visualstudio.com/vs/) met werkbelastingen voor: bureaublad .NET-ontwikkeling, ontwikkelen van Azure, ASP.NET en web ontwikkeling, Node.js-ontwikkeling en ontwikkeling van een Python.
* De [EventHubsCaptureEventGridDemo voorbeeldproject](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) gedownload op uw computer.

## <a name="deploy-the-infrastructure"></a>De infrastructuur implementeren

Om te vereenvoudigen in dit artikel, moet u de vereiste infrastructuur met Resource Manager-sjabloon implementeren. Geef weer voor de resources die zijn geïmplementeerd, de [sjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/EventHubsDataMigration.json). Voor de preview-versie gebeurtenis raster ondersteunt **westus2** en **westcentralus** regio's. Gebruik een van deze gebieden voor de locatie van de resourcegroep.

Gebruik voor Azure CLI:

```azurecli-interactive
az group create -l westcentralus -n rgDataMigrationSample

az group deployment create \
  --resource-group rgDataMigrationSample \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json \
  --parameters eventHubNamespaceName=<event-hub-namespace> eventHubName=hubdatamigration sqlServerName=<sql-server-name> sqlServerUserName=<user-name> sqlServerPassword=<password> sqlServerDatabaseName=<database-name> storageName=<unique-storage-name> functionAppName=<app-name>
```

Gebruik voor PowerShell:

```powershell
New-AzureRmResourceGroup -Name rgDataMigration -Location westcentralus

New-AzureRmResourceGroupDeployment -ResourceGroupName rgDataMigration -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json -eventHubNamespaceName <event-hub-namespace> -eventHubName hubdatamigration -sqlServerName <sql-server-name> -sqlServerUserName <user-name> -sqlServerDatabaseName <database-name> -storageName <unique-storage-name> -functionAppName <app-name>
```

Geef een wachtwoordwaarde als u wordt gevraagd.

## <a name="create-a-table-in-sql-data-warehouse"></a>Een tabel in SQL Data Warehouse maken

Een tabel toevoegen aan uw datawarehouse door het uitvoeren van de [CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql) script. Het script wordt uitgevoerd, moet u Visual Studio of de Query-Editor gebruiken in de portal.

Het script uit te voeren is:

```sql
CREATE TABLE [dbo].[Fact_WindTurbineMetrics] (
    [DeviceId] nvarchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [MeasureTime] datetime NULL, 
    [GeneratedPower] float NULL, 
    [WindSpeed] float NULL, 
    [TurbineSpeed] float NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
```

## <a name="publish-the-azure-functions-app"></a>Publiceer de app Azure Functions

1. Open de [EventHubsCaptureEventGridDemo voorbeeldproject](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) in Visual Studio 2017 (15.3.2 of hoger).

2. Klik in Solution Explorer met de rechtermuisknop op **FunctionDWDumper**, en selecteer **publiceren**.

   ![Functie-app publiceren](media/event-grid-event-hubs-integration/publish-function-app.png)

3. Selecteer **Azure functie-App** en **Selecteer een bestaande**. Selecteer **OK**.

   ![Doel-functie-app](media/event-grid-event-hubs-integration/pick-target.png)

4. Selecteer de functie-app die u via de sjabloon hebt geïmplementeerd. Selecteer **OK**.

   ![Functie-app selecteren](media/event-grid-event-hubs-integration/select-function-app.png)

5. Wanneer Visual Studio kan het profiel is geconfigureerd, selecteer **publiceren**.

   ![Selecteer publiceren](media/event-grid-event-hubs-integration/select-publish.png)

6. Na het publiceren van de functie, gaat u naar de [Azure-portal](https://portal.azure.com/). Selecteer uw resource en de functie-app.

   ![Functie-app weergeven](media/event-grid-event-hubs-integration/view-function-app.png)

7. Selecteer de functie.

   ![Selecteer een functie](media/event-grid-event-hubs-integration/select-function.png)

8. De URL voor de functie niet ophalen. Deze URL moet u bij het maken van het gebeurtenisabonnement.

   ![Functie-URL ophalen](media/event-grid-event-hubs-integration/get-function-url.png)

9. De waarde kopiëren.

   ![URL kopiëren](media/event-grid-event-hubs-integration/copy-url.png)

## <a name="subscribe-to-the-event"></a>Abonneren op de gebeurtenis

U kunt Azure CLI of de portal gebruiken om u te abonneren op de gebeurtenis. Dit artikel ziet beide benaderingen.

### <a name="portal"></a>Portal

1. Selecteer in de naamruimte Event Hubs **gebeurtenis raster** aan de linkerkant.

   ![Selecteer gebeurtenis raster](media/event-grid-event-hubs-integration/select-event-grid.png)

2. Een gebeurtenisabonnement toevoegen.

   ![Gebeurtenisabonnement toevoegen](media/event-grid-event-hubs-integration/add-event-subscription.png)

3. Geef waarden op voor het gebeurtenisabonnement. Gebruik de Azure Functions-URL die u hebt gekopieerd. Selecteer **Maken**.

   ![Abonnement-waarden opgeven](media/event-grid-event-hubs-integration/provide-values.png)

### <a name="azure-cli"></a>Azure-CLI

Abonneren op de gebeurtenis, voer de volgende opdrachten (waarvoor versie 2.0.24 of hoger van Azure CLI):

```azurecli-interactive
namespaceid=$(az resource show --namespace Microsoft.EventHub --resource-type namespaces --name <your-EventHubs-namespace> --resource-group rgDataMigrationSample --query id --output tsv)
az eventgrid event-subscription create \
  --resource-id $namespaceid \
  --name captureEventSub \
  --endpoint <your-function-endpoint>
```

## <a name="run-the-app-to-generate-data"></a>Voer de app voor het genereren van gegevens

U kunt instellen van uw event hub, SQL datawarehouse, Azure functie-app en voor een gebeurtenisabonnement hebt voltooid. De oplossing is gereed voor het migreren van gegevens van de event hub naar het datawarehouse. Voordat u een toepassing die gegevens voor event hub genereert, moet u enkele waarden configureren.

1. Selecteer in de portal voor uw event hub-naamruimte. Selecteer **verbindingsreeksen**.

   ![Selecteer verbindingsreeksen](media/event-grid-event-hubs-integration/event-hub-connection.png)

2. Selecteer **RootManageSharedAccessKey**

   ![Sleutel selecteren](media/event-grid-event-hubs-integration/show-root-key.png)

3. Kopiëren **verbindingsreeks - primaire sleutel**

   ![Kopieer sleutel](media/event-grid-event-hubs-integration/copy-key.png)

4. Ga terug naar Visual Studio-project. Open in het project WindTurbineDataGenerator **program.cs**.

5. Vervang de twee constante waarden. Gebruik de gekopieerde waarde voor **EventHubConnectionString**. Gebruik de naam van de event hub voor **EventHubName**.

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://tfdatamigratens.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. De oplossing bouwen. Voer de toepassing WindTurbineGenerator.exe. Query uitvoeren op de tabel in het datawarehouse voor de gemigreerde gegevens na een paar minuten.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot gebeurtenis raster, [over gebeurtenis raster](overview.md).
* Zie voor een inleiding tot het vastleggen van Event Hubs, [inschakelen Event Hubs vastleggen met behulp van de Azure-portal](../event-hubs/event-hubs-capture-enable-through-portal.md).
* Zie voor meer informatie over het instellen en uitvoeren van de steekproef [Event Hubs vastleggen en gebeurtenis raster voorbeeld](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo).