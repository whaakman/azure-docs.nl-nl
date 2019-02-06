---
title: Taakhubs in duurzame functies - Azure
description: Leer wat een hub voor de taak is in de extensie duurzame functies voor Azure Functions. Meer informatie over het configureren van taakhubs configureren.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: azfuncdf
ms.openlocfilehash: 596eedab39ff926fcdc880c82c49ac464b7ff23b
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55753463"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Taakhubs in duurzame functies (Azure Functions)

Een *taak hub* in [duurzame functies](durable-functions-overview.md) is een logische container voor Azure Storage-resources die worden gebruikt voor indelingen. Orchestrator en activiteit functies kunnen alleen met elkaar communiceren wanneer ze deel uitmaken van dezelfde taak hub.

Als meerdere functie-apps delen met een opslagaccount, elke functie-app *moet* worden geconfigureerd met een afzonderlijke taak hubnaam op. Een storage-account kan meerdere taakhubs bevatten. Het volgende diagram ziet u een taak hub per functie-app in de gedeelde en toegewijde storage-accounts.

![Diagram van gedeeld en storage-accounts toegewezen.](./media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Azure Storage-resources

Een hub taak bestaat uit de volgende opslagresources:

* Een of meer controle-wachtrijen.
* Een werkitem wachtrij.
* Van één geschiedenistabel.
* Tabel met één exemplaren.
* Een storage-container met een of meer lease-blobs.

Al deze resources worden automatisch gemaakt in de standaard Azure Storage-account als orchestrator of activiteitsfuncties zijn gepland uitvoeren. De [prestaties en schaal](durable-functions-perf-and-scale.md) artikel wordt uitgelegd hoe deze resources worden gebruikt.

## <a name="task-hub-names"></a>Namen van de hub taken

Taakhubs worden aangeduid met een naam die is gedeclareerd in de *host.json* bestand, zoals weergegeven in het volgende voorbeeld:

### <a name="hostjson-functions-1x"></a>host.JSON (1.x werkt)

```json
{
  "durableTask": {
    "hubName": "MyTaskHub"
  }
}
```

### <a name="hostjson-functions-2x"></a>host.JSON (werkt 2.x)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "MyTaskHub"
    }
  }
}
```

Taakhubs kunnen ook worden geconfigureerd met behulp van app-instellingen, zoals wordt weergegeven in de volgende *host.json* voorbeeld van een bestand:

### <a name="hostjson-functions-1x"></a>host.JSON (1.x werkt)

```json
{
  "durableTask": {
    "hubName": "%MyTaskHub%"
  }
}
```

### <a name="hostjson-functions-2x"></a>host.JSON (werkt 2.x)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "%MyTaskHub%"
    }
  }
}
```

De naam van de taak hub wordt ingesteld op de waarde van de `MyTaskHub` app-instelling. De volgende `local.settings.json` ziet u hoe u voor het definiëren van de `MyTaskHub` instellen als `samplehubname`:

```json
{
  "IsEncrypted": false,
  "Values": {
    "MyTaskHub" : "samplehubname"
  }
}
```

Hier volgt een vooraf gecompileerde C# voorbeeld van het schrijven van een functie die wordt gebruikt een [OrchestrationClientBinding](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) om te werken met een taak-hub die is geconfigureerd als een App-instelling:

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
    [OrchestrationClient(TaskHub = "%MyTaskHub%")] DurableOrchestrationClientBase starter,
    string functionName,
    ILogger log)
{
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

En hieronder ziet u de vereiste configuratie voor JavaScript. De eigenschap van de hub taak in de `function.json` bestand is ingesteld via App-instelling:

```json
{
    "name": "input",
    "taskHub": "%MyTaskHub%",
    "type": "orchestrationClient",
    "direction": "in"
}
```

Namen van taken hub moeten beginnen met een letter en bestaan uit alleen letters en cijfers. Indien niet opgegeven, wordt de standaardnaam is **DurableFunctionsHub**.

> [!NOTE]
> De naam is wat wordt onderscheid gemaakt tussen een taak hub vanuit een ander wanneer er meerdere taakhubs in een gedeelde storage-account. Als u meerdere functie-apps delen van een gedeelde storage-account hebt, moet u expliciet verschillende namen voor elke taak hub in configureren de *host.json* bestanden. De meerdere functie-apps wordt anders met elkaar concurreren om berichten, hetgeen in niet-gedefinieerde gedrag resulteren kunnen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het afhandelen van versiebeheer](durable-functions-versioning.md)
