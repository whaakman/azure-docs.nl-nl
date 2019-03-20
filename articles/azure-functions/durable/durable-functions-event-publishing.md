---
title: Duurzame functies publiceren naar Azure Event Grid (preview)
description: Informatie over het configureren van automatische Azure Event Grid-publicatie voor duurzame functies.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: glenga
ms.openlocfilehash: c07a42349fbd81a46b1b7cd9bcad1978f891a6b2
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58136358"
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Duurzame functies publiceren naar Azure Event Grid (preview)

In dit artikel laat zien hoe het instellen van duurzame functies voor het publiceren van gebeurtenissen in de orchestration levensduur (zoals gemaakt, voltooide en mislukte) op een aangepast [Azure Event Grid-onderwerp](https://docs.microsoft.com/azure/event-grid/overview).

Hier volgen enkele scenario's waarin deze functie handig is:

* **DevOps-scenario's, zoals blue/green implementaties**: U wilt weten of er taken worden uitgevoerd voordat u implementeert de [side-by-side-implementatiestrategie](durable-functions-versioning.md#side-by-side-deployments).

* **Geavanceerde ondersteuning voor controle en diagnose**: U kunt bijhouden van orchestration statusinformatie in een externe opslag die is geoptimaliseerd voor query's, zoals SQL-database of cosmos DB.

* **Activiteit op de achtergrond langlopende**: Als u duurzame functies voor een activiteit op de achtergrond langlopende, wordt deze functie helpt u om te weten de huidige status.

## <a name="prerequisites"></a>Vereisten

* Installeer [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) 1.3.0-rc of later in uw project duurzame functies.
* Installeer [Azure-Opslagemulator](https://docs.microsoft.com/azure/storage/common/storage-use-emulator).
* Installeer [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) of gebruik [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)

## <a name="create-a-custom-event-grid-topic"></a>Een aangepast event grid-onderwerp maken

Maak een event grid-onderwerp voor het verzenden van gebeurtenissen van duurzame functies. De volgende instructies laten zien hoe u een onderwerp maakt met behulp van Azure CLI. Raadpleeg de volgende artikelen voor meer informatie over hoe u doet dit met behulp van PowerShell of Azure portal:

* [EventGrid snelstartgidsen: Maken van aangepaste gebeurtenissen - PowerShell](https://docs.microsoft.com/azure/event-grid/custom-event-quickstart-powershell)
* [EventGrid snelstartgidsen: Een aangepaste gebeurtenis - Azure portal maken](https://docs.microsoft.com/azure/event-grid/custom-event-quickstart-portal)

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht `az group create`. Op dit moment ondersteunt Azure Event Grid niet alle regio's. Zie voor meer informatie over welke regio's worden ondersteund, de [overzicht van Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview).

```bash
az group create --name eventResourceGroup --location westus2
```

### <a name="create-a-custom-topic"></a>Een aangepast onderwerp maken

Een event grid-onderwerp biedt een door de gebruiker gedefinieerd eindpunt waarop u de gebeurtenis te posten. Vervang `<topic_name>` door een unieke naam voor het onderwerp. Naam van het onderwerp moet uniek zijn omdat deze niet langer een DNS-vermelding.

```bash
az eventgrid topic create --name <topic_name> -l westus2 -g eventResourceGroup
```

## <a name="get-the-endpoint-and-key"></a>Het eindpunt en de sleutel ophalen

Het eindpunt van het onderwerp ophalen. Vervang `<topic_name>` met de naam die u hebt gekozen.

```bash
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

De sleutel van het onderwerp ophalen. Vervang `<topic_name>` met de naam die u hebt gekozen.

```bash
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

Nu kunt u gebeurtenissen verzenden naar het onderwerp.

## <a name="configure-azure-event-grid-publishing"></a>Configureer Azure Event Grid-publicatie

Zoek in uw project duurzame functies de `host.json` bestand.

Voeg `eventGridTopicEndpoint` en `eventGridKeySettingName` in een `durableTask` eigenschap.

```json
{
    "durableTask": {
        "eventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
        "eventGridKeySettingName": "EventGridKey"
    }
}
```

De mogelijke configuratie-eigenschappen van Azure Event Grid kunnen u vinden in de [host.json documentatie](../functions-host-json.md#durabletask). Na het configureren van de `host.json` -bestand, de functie-app verzendt lifecycle gebeurtenissen naar event grid-onderwerp. Dit werkt wanneer u uw functie-app, zowel lokaal als in Azure uitvoeren.'' '

De app-instelling voor de sleutel voor onderwerp instellen in de functie-App en `local.setting.json`. De volgende JSON wordt een voorbeeld van de `local.settings.json` voor lokale foutopsporing. Vervang `<topic_key>` met de sleutel van het onderwerp.  

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

Zorg ervoor dat [Opslagemulator](https://docs.microsoft.com/azure/storage/common/storage-use-emulator) werkt. Is het een goed idee om uit te voeren de `AzureStorageEmulator.exe clear all` opdracht voordat u uitvoert.

## <a name="create-functions-that-listen-for-events"></a>Functies maken die naar gebeurtenissen luisteren

Een functie-App maken. Het is raadzaam te zoeken in dezelfde regio als de event grid-onderwerp.

### <a name="create-an-event-grid-trigger-function"></a>Een activeringsfunctie voor event grid maken

Maak een functie voor het ontvangen van de van levenscyclusgebeurtenissen. Selecteer **aangepaste functie**.

![Selecteer een maken een aangepaste functie aan.](./media/durable-functions-event-publishing/functions-portal.png)

Kies de Trigger Gebeurtenisraster en selecteer `C#`.

![De Trigger Gebeurtenisraster selecteren.](./media/durable-functions-event-publishing/eventgrid-trigger.png)

Voer de naam van de functie en selecteer vervolgens `Create`.

![De Trigger Gebeurtenisraster maken.](./media/durable-functions-event-publishing/eventgrid-trigger-creation.png)

Een functie met de volgende code wordt gemaakt:

```csharp
#r "Newtonsoft.Json"
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Microsoft.Extensions.Logging;

public static void Run(JObject eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
}
```

Selecteer `Add Event Grid Subscription`. Met deze bewerking wordt een event grid-abonnement voor de event grid-onderwerp dat u hebt gemaakt. Zie voor meer informatie, [concepten in Azure Event Grid](https://docs.microsoft.com/azure/event-grid/concepts)

![Selecteer de Trigger Gebeurtenisraster-koppeling.](./media/durable-functions-event-publishing/eventgrid-trigger-link.png)

Selecteer `Event Grid Topics` voor **Onderwerptype**. Selecteer de resourcegroep die u hebt gemaakt voor de event grid-onderwerp. Selecteer vervolgens het exemplaar van de event grid-onderwerp. Druk op `Create`.

![Hiermee wordt een Event Grid-abonnement gemaakt.](./media/durable-functions-event-publishing/eventsubscription.png)

U bent nu klaar om te ontvangen van gebeurtenissen in de levensduur.

## <a name="create-durable-functions-to-send-the-events"></a>Duurzame functies voor het verzenden van de gebeurtenissen maken

Start de foutopsporing op uw lokale computer in uw project duurzame functies.  De volgende code is hetzelfde als de sjabloon voor de duurzame functies. U hebt al geconfigureerd `host.json` en `local.settings.json` op uw lokale computer.

```csharp
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace LifeCycleEventSpike
{
    public static class Sample
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
        public static string SayHello([ActivityTrigger] string name, ILogger log)
        {
            log.LogInformation($"Saying hello to {name}.");
            return $"Hello {name}!";
        }

        [FunctionName("Sample_HttpStart")]
        public static async Task<HttpResponseMessage> HttpStart(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
            [OrchestrationClient]DurableOrchestrationClient starter,
            ILogger log)
        {
            // Function input comes from the request content.
            string instanceId = await starter.StartNewAsync("Sample", null);
            log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
            return starter.CreateCheckStatusResponse(req, instanceId);
        }
    }
}
```

Als u de `Sample_HttpStart` met Postman of in uw browser, duurzame functie voor het verzenden van gebeurtenissen in de levensduur wordt gestart. Het eindpunt is meestal `http://localhost:7071/api/Sample_HttpStart` voor lokale foutopsporing.

Zie de logboeken van de functie die u hebt gemaakt in Azure portal.

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

## <a name="event-schema"></a>Gebeurtenisschema

De volgende lijst wordt het schema van de levenscyclus van gebeurtenissen beschreven:

* **`id`**: De unieke id voor de event grid-gebeurtenis.
* **`subject`**: Pad naar het onderwerp van de gebeurtenis. `durable/orchestrator/{orchestrationRuntimeStatus}`. `{orchestrationRuntimeStatus}` worden `Running`, `Completed`, `Failed`, en `Terminated`.  
* **`data`**: Duurzame functies specifieke Parameters.
  * **`hubName`**: [TaskHub](durable-functions-task-hubs.md) naam.
  * **`functionName`**: Naam van de orchestrator-functie.
  * **`instanceId`**: Duurzame functies instanceId.
  * **`reason`**: Aanvullende gegevens die zijn gekoppeld aan de traceringsgebeurtenis. Zie voor meer informatie, [diagnostische gegevens in duurzame functies (Azure Functions)](durable-functions-diagnostics.md)
  * **`runtimeStatus`**: Status van de Orchestration-Runtime. Actieve, voltooid, is mislukt, geannuleerd.
* **`eventType`**: "orchestratorEvent"
* **`eventTime`**: Tijd van de gebeurtenis (UTC).
* **`dataVersion`**: De versie van het schema van de gebeurtenis levenscyclus.
* **`metadataVersion`**:  De versie van de metagegevens.
* **`topic`**: Event grid-onderwerp resource.

## <a name="how-to-test-locally"></a>Lokaal testen

U kunt lokaal testen met [ngrok](../functions-bindings-event-grid.md#local-testing-with-ngrok).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over instantiebeheer in duurzame functies](durable-functions-instance-management.md)

> [!div class="nextstepaction"]
> [Informatie over versiebeheer in duurzame functies](durable-functions-versioning.md)
