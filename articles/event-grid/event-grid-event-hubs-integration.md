---
title: Integratie van Azure Event Grid en Event Hubs
description: In dit artikel wordt beschreven hoe u met Azure Event Grid en Event Hubs gegevens kunt migreren naar een SQL Data Warehouse
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: tutorial
ms.date: 08/22/2018
ms.author: tomfitz
ms.openlocfilehash: aad7a24d8b0e0bc74815cad3604db1cc21a6db96
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44163223"
---
# <a name="stream-big-data-into-a-data-warehouse"></a>Big data streamen naar een datawarehouse

Azure [Event Grid](overview.md) is een intelligente service voor het routeren van gebeurtenissen waarmee u kunt reageren op meldingen van apps en services. Het kan bijvoorbeeld een Azure-functie activeren voor het verwerken van Event Hubs-gegevens die zijn opgenomen in een Azure Blob-opslag of Data Lake Store en de gegevens naar andere gegevensopslagplaatsen migreren. Dit [Event Hubs Capture en Event Grid-voorbeeld](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) laat zien hoe u Event Hubs Capture gebruikt met Event Grid voor het naadloos migreren van gegevens van Event Hubs uit blob-opslag naar een SQL Data Warehouse.

![Overzicht van toepassing](media/event-grid-event-hubs-integration/overview.png)

Op het moment dat er gegevens naar de gebeurtenishub worden verzonden, worden deze door Capture geëxtraheerd uit de stroom en worden er opslag-blobs met de gegevens gegenereerd in de Avro-indeling. Bij het genereren van de blob door Capture, wordt er een gebeurtenis geactiveerd. Event Grid verdeelt gegevens van de gebeurtenis naar abonnees. In dit geval worden de gebeurtenisgegevens verzonden naar het eindpunt Azure Functions. De gebeurtenisgegevens bestaan onder andere uit het pad van de gegenereerde blob. De functie gebruikt die URL voor het ophalen van het bestand, waarna dit wordt verzonden naar het datawarehouse.

In dit artikel leert u het volgende:

* De volgende infrastructuur implementeren:
  * Gebeurtenishub met Capture ingeschakeld
  * Opslagaccount voor de bestanden van Capture
  * Azure App Service-plan voor het hosten van de functie-app
  * Functie-app voor het verwerken van de gebeurtenis
  * SQL Server voor het hosten van het datawarehouse
  * SQL Data Warehouse voor het opslaan van de gemigreerde gegevens
* Een tabel maken in het datawarehouse
* Code toevoegen aan de functie-app
* Abonneren op de gebeurtenis
* App uitvoeren die gegevens naar de gebeurtenishub verzendt
* Gemigreerde gegevens weergeven in het datawarehouse

## <a name="about-the-event-data"></a>De gebeurtenisgegevens

Event Grid distribueert gebeurtenisgegevens naar de abonnees. In het volgende voorbeeld ziet u gebeurtenisgegevens voor het maken van een Capture-bestand. Let met name op de eigenschap `fileUrl` in het object `data`. De functie-app vraagt deze waarde op en gebruikt deze voor het ophalen van het Capture-bestand.

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

U hebt het volgende nodig om deze zelfstudie te voltooien:

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
* [Visual studio 2017 versie 15.3.2 of hoger](https://www.visualstudio.com/vs/) met werkbelastingen voor: .NET-desktopontwikkeling, Azure-ontwikkeling, ASP.NET- en webontwikkeling, Node.js-ontwikkeling en Python-ontwikkeling.
* Het voorbeeldproject [EventHubsCaptureEventGridDemo](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) gedownload op uw computer.

## <a name="deploy-the-infrastructure"></a>De infrastructuur implementeren

Om dit artikel te vereenvoudigen, gaat u de vereiste infrastructuur implementeren met behulp van een Resource Manager-sjabloon. Bekijk de [sjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/EventHubsDataMigration.json) om te zien welke resources er worden geïmplementeerd.

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

Geef een wachtwoord op als dat wordt gevraagd.

## <a name="create-a-table-in-sql-data-warehouse"></a>Een tabel maken in SQL Data Warehouse

Voeg een tabel toe aan uw datawarehouse door het script [CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql) uit te voeren. Gebruik hiervoor Visual Studio of de query-editor in de portal.

Dit is het script dat u moet uitvoeren:

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

## <a name="publish-the-azure-functions-app"></a>De Azure Functions-app publiceren

1. Open het voorbeeldproject [EventHubsCaptureEventGridDemo](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) in Visual Studio 2017 (15.3.2 of hoger).

1. Klik in Solution Explorer met de rechtermuisknop op **FunctionEGDWDumper** en selecteer **Publish**.

   ![Functie-app publiceren](media/event-grid-event-hubs-integration/publish-function-app.png)

1. Selecteer **Azure Function App** en **Select Existing**. Selecteer **Publiceren**.

   ![Bestaande functie-app](media/event-grid-event-hubs-integration/pick-target.png)

1. Selecteer de functie-app die u via de sjabloon hebt geïmplementeerd. Selecteer **OK**.

   ![Functie-app selecteren](media/event-grid-event-hubs-integration/select-function-app.png)

1. Als Visual Studio het profiel heeft geconfigureerd, selecteert u **Publish**.

   ![Selecteer Publish](media/event-grid-event-hubs-integration/select-publish.png)

Nadat de functie is gepubliceerd, kunt u zich abonneren op de gebeurtenis.

## <a name="subscribe-to-the-event"></a>Abonneren op de gebeurtenis

1. Ga naar de [Azure-portal](https://portal.azure.com/). Selecteer de resourcegroep en de functie-app.

   ![Functie-app weergeven](media/event-grid-event-hubs-integration/view-function-app.png)

1. Selecteer de functie.

   ![Functie selecteren](media/event-grid-event-hubs-integration/select-function.png)

1. Selecteer **Add Event Grid subscription**.

   ![Abonnement toevoegen](media/event-grid-event-hubs-integration/add-event-grid-subscription.png)

9. Geef een naam op voor het Event Grid-abonnement. Gebruik **Event Hubs Namespaces** als het gebeurtenistype. Geef waarden op om uw exemplaar van de Event Hubs-naamruimte te selecteren. Laat de waarde voor het eindpunt ongewijzigd (Subscriber Endpoint). Selecteer **Maken**.

   ![Abonnement maken](media/event-grid-event-hubs-integration/set-subscription-values.png)

## <a name="run-the-app-to-generate-data"></a>De app uitvoeren om gegevens te genereren

U bent nu klaar met het instellen van uw gebeurtenishub, SQL-datawarehouse, Azure-functie-app en gebeurtenisabonnement. De oplossing kan gegevens migreren van de gebeurtenishub naar het datawarehouse. Voordat u een toepassing gaat uitvoeren die gegevens voor Event Hub genereert, moet u nog enkele waarden configureren.

1. Selecteer in de portal de naamruimte van uw gebeurtenishub. Selecteer **Verbindingsreeksen**.

   ![Selecteer Verbindingsreeksen](media/event-grid-event-hubs-integration/event-hub-connection.png)

2. Selecteer **RootManageSharedAccessKey**.

   ![Sleutel selecteren](media/event-grid-event-hubs-integration/show-root-key.png)

3. Kopieer **Verbindingsreeks-primaire sleutel**.

   ![Sleutel kopiëren](media/event-grid-event-hubs-integration/copy-key.png)

4. Ga terug naar het Visual Studio-project. Open **program.cs** in het project WindTurbineDataGenerator.

5. Vervang de twee constante waarden. Gebruik de gekopieerde waarde voor **EventHubConnectionString**. Gebruik **hubdatamigration** als de naam van de gebeurtenishub.

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://demomigrationnamespace.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. Bouw de oplossing. Voer de toepassing WindTurbineGenerator.exe uit. Voer na een paar minuten een query voor de gemigreerde gegevens uit op de tabel in het datawarehouse.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de verschillen in de Azure-berichtenservices [Kiezen tussen Azure-services die berichten bezorgen](compare-messaging-services.md).
* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Zie [Event Hubs Capture inschakelen met behulp van de Azure-portal](../event-hubs/event-hubs-capture-enable-through-portal.md) voor een inleiding tot Event Hubs Capture.
* Zie het Engelstalige artikel [An overview, how Event Hubs Capture integrates with Event Grid](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) voor meer informatie over het instellen en uitvoeren van het voorbeeld.