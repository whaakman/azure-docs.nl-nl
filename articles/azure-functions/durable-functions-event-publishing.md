---
title: Duurzame fungeert publiceren naar Azure gebeurtenis raster (preview)
description: Informatie over het configureren van automatische Azure gebeurtenis raster publiceren voor duurzame functies.
services: functions
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/20/2018
ms.author: tdykstra
ms.openlocfilehash: 50e517e5719fb102fd91072abe59d3908176278e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Duurzame fungeert publiceren naar Azure gebeurtenis raster (preview)

In dit artikel laat zien hoe voor het instellen van Azure Duurzame Functions orchestration lifecycle om gebeurtenissen te publiceren (zoals gemaakt, voltooide en mislukte) aan een aangepaste [Azure gebeurtenis raster onderwerp](https://docs.microsoft.com/en-us/azure/event-grid/overview). 

Hieronder volgen enkele scenario's waarbij deze functie handig is:

* **DevOps-scenario's zoals implementaties van blauw/groen**: U wilt weten of er taken worden uitgevoerd voordat u implementeert de [side-by-side implementatiestrategie](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-versioning#side-by-side-deployments).

* **Controle en diagnostische gegevens ondersteuning voor geavanceerde**: U kunt bijhouden van orchestration statusgegevens in een externe winkel geoptimaliseerd voor query's, zoals SQL-database of CosmosDB.

* **Activiteit op de achtergrond langlopende**: als u duurzame functies voor een activiteit op de achtergrond langlopende, deze functie kunt u de huidige status weten.

## <a name="prerequisites"></a>Vereisten

* Installeer [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) 1.3.0-rc of later in uw project duurzame functies.
* Installeer [Azure-Opslagemulator](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-emulator).
* Installeer [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest) of gebruik [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview)

## <a name="create-a-custom-event-grid-topic"></a>Maken van een aangepaste gebeurtenis raster onderwerp

Maak een onderwerp gebeurtenis raster voor het verzenden van gebeurtenissen van duurzame functies. De volgende instructies wordt aangegeven hoe u een onderwerp maakt met behulp van Azure CLI. Raadpleeg voor informatie over hoe u kunt dit doen met behulp van PowerShell of Azure portal, de volgende artikelen:

* [EventGrid Quickstarts: Aangepaste gebeurtenis - PowerShell maken](https://docs.microsoft.com/en-us/azure/event-grid/custom-event-quickstart-powershell)
* [EventGrid Quickstarts: Aangepaste gebeurtenis - Azure-portal maken](https://docs.microsoft.com/en-us/azure/event-grid/custom-event-quickstart-portal)

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de `az group create` opdracht. Op dit moment ondersteunt gebeurtenis raster niet alle regio's. Zie voor meer informatie over welke regio's worden ondersteund, de [gebeurtenis raster overzicht](https://docs.microsoft.com/en-us/azure/event-grid/overview). 

```bash
az group create --name eventResourceGroup --location westus2
```

### <a name="create-a-custom-topic"></a>Een aangepast onderwerp maken

Een gebeurtenis raster-onderwerp vindt u een gebruiker gedefinieerde eindpunt dat u uw gebeurtenis te plaatsen. Vervang `<topic_name>` door een unieke naam voor het onderwerp. Naam van het onderwerp moet uniek zijn omdat deze een DNS-vermelding is.

```bash
az eventgrid topic create --name <topic_name> -l westus2 -g eventResourceGroup 
```

## <a name="get-the-endpoint-and-key"></a>Het eindpunt en sleutel ophalen

Het eindpunt van het onderwerp worden opgehaald. Vervang `<topic_name>` met de naam die u hebt gekozen.

```bash
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

De sleutel van het onderwerp ophalen. Vervang `<topic_name>` met de naam die u hebt gekozen.

```bash
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

Nu kunt u gebeurtenissen verzenden naar het onderwerp.

## <a name="configure-azure-event-grid-publishing"></a>Azure Event raster publicatie configureren

Zoeken in uw project duurzame functies de `host.json` bestand.

Voeg `EventGridTopicEndpoint` en `EventGridKeySettingName` in een `durableTask` eigenschap.

```json
{
    "durableTask": {
        "EventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
        "EventGridKeySettingName": "EventGridKey"
    }
}
```

* **EventGridTopicEndpoint** -het eindpunt van het onderwerp raster.
* **EventGridKeySettingName** -de sleutel van de toepassingsinstelling op uw Azure-functie. Duurzame functies wordt de gebeurtenis raster onderwerp sleutel ophalen van de waarde.

Zodra u configureert de `host.json` -bestand, uw duurzame functies project begint met het lifecycle gebeurtenissen verzenden naar het onderwerp gebeurtenis raster. Dit werkt wanneer u uitvoert in de functie-App en wanneer u lokaal uitvoeren.

De app-instelling voor de sleutel van het onderwerp in de functie-App instellen en `local.setting.json`. De volgende JSON is een voorbeeld van de `local.settings.json` voor lokale foutopsporing. Vervang `<topic_key>` met de sleutel van het onderwerp.  

```json
{
    "IsEncrypted": false,
    "Values": {
        "AzureWebJobsStorage": "UseDevelopmentStorage=true",
        "AzureWebJobsDashboard": "UseDevelopmentStorage=true",
        "EventGridKey": "<topic_key>"
    }
}
```

Zorg ervoor dat [Opslagemulator](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-emulator) werkt. Het is een goed idee om uit te voeren de `AzureStorageEmulator.exe clear all` opdracht voordat wordt uitgevoerd.

## <a name="create-functions-that-listen-for-events"></a>Functies die naar gebeurtenissen luisteren maken

Een functie-App maken. Het is raadzaam terug te vinden in dezelfde regio bevinden als het onderwerp raster.

### <a name="create-an-event-grid-trigger-function"></a>Een gebeurtenis raster trigger-functie maken

Maak een functie voor het ontvangen van de levenscyclus van gebeurtenissen. Selecteer **aangepaste functie**. 

![Selecteer een Maak een aangepaste functie.](media/durable-functions-event-publishing/functions-portal.png)

Gebeurtenis raster Trigger kiest en selecteert u `C#`.

![Selecteer de gebeurtenis raster Trigger.](media/durable-functions-event-publishing/eventgrid-trigger.png)

Voer de naam van de functie en selecteer vervolgens `Create`.

![De gebeurtenis raster Trigger maken.](media/durable-functions-event-publishing/eventgrid-trigger-creation.png)

Een functie met de volgende code wordt gemaakt: 

```csharp
#r "Newtonsoft.Json"
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

Selecteer `Add Event Grid Subscription`. Deze bewerking wordt toegevoegd een raster gebeurtenis abonnement voor de gebeurtenis raster onderwerp dat u hebt gemaakt. Zie voor meer informatie [concepten in Azure gebeurtenis raster](https://docs.microsoft.com/en-us/azure/event-grid/concepts)

![Selecteer de koppeling gebeurtenis raster Trigger.](media/durable-functions-event-publishing/eventgrid-trigger-link.png)

Selecteer `Event Grid Topics` voor **Onderwerptype**. Selecteer de resourcegroep die u hebt gemaakt voor het onderwerp gebeurtenis raster. Selecteer vervolgens het exemplaar van het onderwerp gebeurtenis raster. Druk op `Create`.

![Hiermee wordt een Event Grid-abonnement gemaakt.](media/durable-functions-event-publishing/eventsubscription.png)

U kunt nu klaar voor het ontvangen van de levenscyclus van gebeurtenissen. 

## <a name="create-durable-functions-to-send-the-events"></a>Duurzame functies voor het verzenden van de gebeurtenissen maken.

Start de foutopsporing op uw lokale computer in uw project duurzame functies.  De volgende code is hetzelfde als de sjabloon voor de duurzame functies. U hebt al geconfigureerd `host.json` en `local.settings.json` op uw lokale machine. 

```csharp
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;

namespace LifeCycleEventSpike
{
    public static class Sample
    {
    {
        [FunctionName("Sample")]
        public static async Task<List<string>> RunOrchestrator(
            [OrchestrationTrigger] DurableOrchestrationContext context)
        {
            var outputs = new List<string>();

            // Replace "hello" with the name of your Durable Activity Function.
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Tokyo"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Seattle"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "London"));

            // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
            return outputs;
        }

        [FunctionName("Sample_Hello")]
        public static string SayHello([ActivityTrigger] string name, TraceWriter log)
        {
            log.Info($"Saying hello to {name}.");
            return $"Hello {name}!";
        }

        [FunctionName("Sample_HttpStart")]
        public static async Task<HttpResponseMessage> HttpStart(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
            [OrchestrationClient]DurableOrchestrationClient starter,
            TraceWriter log)
        {
            // Function input comes from the request content.
            string instanceId = await starter.StartNewAsync("Sample", null);
            log.Info($"Started orchestration with ID = '{instanceId}'.");
            return starter.CreateCheckStatusResponse(req, instanceId);
        }
    }
}
```

Als u de `Sample_HttpStart` met Postman of uw browser, duurzame functie voor het verzenden van de levenscyclus van gebeurtenissen wordt gestart. Het eindpunt is meestal `http://localhost:7071/api/Sample_HttpStart` voor lokale foutopsporing.

Raadpleeg de logboeken van de functie die u hebt gemaakt in de Azure portal.

```
2018-04-20T09:28:21.041 [Info] Function started (Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d)
2018-04-20T09:28:21.104 [Info] {
    "id": "054fe385-c017-4ce3-b38a-052ac970c39d",    
    "subject": "durable/orchestrator/Running",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Running"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2018-04-20T09:28:19.6492068Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}

2018-04-20T09:28:21.104 [Info] Function completed (Success, Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d, Duration=65ms)
2018-04-20T09:28:37.098 [Info] Function started (Id=36fadea5-198b-4345-bb8e-2837febb89a2)
2018-04-20T09:28:37.098 [Info] {
    "id": "8cf17246-fa9c-4dad-b32a-5a868104f17b",
    "subject": "durable/orchestrator/Completed",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Completed"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2018-04-20T09:28:36.5061317Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}
2018-04-20T09:28:37.098 [Info] Function completed (Success, Id=36fadea5-198b-4345-bb8e-2837febb89a2, Duration=0ms)
```

## <a name="event-schema"></a>Gebeurtenis-Schema

De volgende lijst wordt uitgelegd dat het schema van de levenscyclus van gebeurtenissen:

* **id**: de unieke id voor de gebeurtenis met gebeurtenis raster.
* **onderwerp**: pad naar het onderwerp van de gebeurtenis. `durable/orchestrator/{orchestrationRuntimeStatus}`. `{orchestrationRuntimeStatus}` worden `Running`, `Completed`, `Failed`, en `Terminated`.  
* **gegevens**: duurzame specifieke Parameters van functies.
    * **hubName**: [TaskHub](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-task-hubs) naam.
    * **Functienaam**: de naam van de Orchestrator-functie.
    * **instanceId**: instanceId duurzame functies.
    * **reden**: aanvullende gegevens die zijn gekoppeld aan de gebeurtenis bijhouden. Zie voor meer informatie [diagnostische gegevens in duurzame functies (Azure-functies)](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-diagnostics)
    * **runtimeStatus**: Status van de Orchestration-Runtime. Actief is, voltooid, is mislukt, geannuleerd. 
* **eventType**: 'orchestratorEvent'
* **eventTime**: gebeurtenis-tijd (UTC).
* **dataVersion**: versie van het schema van de levenscyclus van de gebeurtenis.
* **metadataVersion**: versie van de metagegevens.
* **onderwerp**: EventGrid onderwerp resource.

## <a name="how-to-test-locally"></a>Lokaal testen

U kunt lokaal testen met [ngrok](functions-bindings-event-grid.md#local-testing-with-ngrok).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over instantiebeheer in duurzame functies](durable-functions-instance-management.md)

> [!div class="nextstepaction"]
> [Meer informatie over versiebeheer in duurzame functies](durable-functions-versioning.md)
