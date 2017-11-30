---
title: host.JSON-documentatie voor Azure Functions
description: Documentatie voor de Azure Functions host.json-bestand.
services: functions
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/09/2017
ms.author: tdykstra
ms.openlocfilehash: 522d0590595b0fc0fef503599f1677658f223bd8
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="hostjson-reference-for-azure-functions"></a>host.JSON-documentatie voor Azure Functions

De *host.json* metagegevensbestand van de globale configuratie-opties die invloed hebben op alle functies die voor een functie-app bevat. In dit artikel bevat de instellingen die beschikbaar zijn. De JSON-schema is op http://json.schemastore.org/host.

Er zijn andere globale configuratie-opties in [appinstellingen](functions-app-settings.md) en in de [local.settings.json](functions-run-local.md#local-settings-file) bestand.

## <a name="sample-hostjson-file"></a>Voorbeeldbestand host.json

Het volgende voorbeeld *host.json* bestand heeft alle mogelijke opties opgegeven.

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    },
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    },
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    },
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    },
    "watchDirectories": [ "Shared" ],
}
```

De volgende secties van dit artikel wordt uitgelegd voor elke eigenschap op het hoogste niveau. Zijn optioneel tenzij anders aangegeven.

## <a name="aggregator"></a>Aggregator

Geeft aan hoeveel functie aanroepen zijn geaggregeerd wanneer [berekenen van de metrische gegevens voor Application Insights](functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|batchSize|1000|Maximum aantal aanvragen kunnen worden geaggregeerd.| 
|flushTimeout|00:00:30|Maximumtijd periode kunnen worden geaggregeerd.| 

Functie aanroepen worden geaggregeerd als de eerste dag van de twee beperkt zijn bereikt.

## <a name="applicationinsights"></a>ApplicationInsights

Bepaalt de [steekproeven functie in Application Insights](functions-monitoring.md#configure-sampling).

```json
{
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|IsEnabled|onwaar|Hiermee of steekproeven uitgeschakeld.| 
|maxTelemetryItemsPerSecond|5|De drempelwaarde op welke steekproeven begint.| 

## <a name="eventhub"></a>EventHub

Configuratie-instellingen voor [Event Hub-triggers en bindingen](functions-bindings-event-hubs.md).

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="functions"></a>functies

Een lijst met functies die de host van de taak wordt uitgevoerd.  Een lege matrix betekent dat alle functies uitvoeren.  Bedoeld voor gebruik alleen wanneer [lokaal uitgevoerd](functions-run-local.md). In functie-apps gebruiken de *function.json* `disabled` eigenschap in plaats van deze eigenschap in *host.json*.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Hiermee geeft u de time-outduur voor alle functies. Het geldige bereik is van 1 seconde tot 10 minuten in verbruik plannen en de standaardwaarde is 5 minuten. Er is geen limiet in App Service-abonnementen en de standaardwaarde is null, wat aangeeft dat er geen time-out.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="http"></a>http

Configuratie-instellingen voor [HTTP-triggers en bindingen](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="id"></a>id

De unieke ID voor een taak host. Een kleine letter GUID met streepjes u kunt verwijderen. Vereist bij lokale uitvoering. Wanneer dit wordt uitgevoerd in Azure Functions, wordt automatisch een ID gegenereerd als `id` wordt weggelaten.

```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>Logboek

Besturingselementen voor het filteren voor Logboeken geschreven door een [ILogger object](functions-monitoring.md#write-logs-in-c-functions) of door [context.log](functions-monitoring.md#write-logs-in-javascript-functions).

```json
{
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|categoryFilter|N.v.t.|Hiermee geeft u filteren op categorie| 
|defaultLevel|Informatie|Voor andere categorieën die niet is opgegeven in de `categoryLevels` matrix kunnen logboeken op dit niveau en hoger naar Application Insights verzenden.| 
|categoryLevels|N.v.t.|Een matrix van categorieën die Hiermee geeft u het minimale logboekniveau verzenden naar Application Insights voor elke categorie. De hier opgegeven categorie alle categorieën die met dezelfde waarde beginnen bepaalt en langer waarden voorrang. In het voorgaande voorbeeld *host.json* -bestand, alle categorieën die beginnen met 'Host.Aggregator' logbestand op `Information` niveau. Alle andere categorieën die met 'Host', zoals 'Host.Executor beginnen' melden bij `Error` niveau.| 

## <a name="queues"></a>Wachtrijen

Configuratie-instellingen voor [opslag queue-triggers en bindingen](functions-bindings-storage-queue.md).

```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|maxPollingInterval|60000|Het maximale interval in milliseconden tussen polls van de wachtrij.| 
|visibilityTimeout|0|Het interval tussen nieuwe pogingen bij het verwerken van een bericht is mislukt.| 
|batchSize|16|Het aantal berichten op te halen en parallel worden verwerkt. Het maximum is 32.| 
|maxDequeueCount|5|Het aantal pogingen voor het verwerken van een bericht voordat u deze verplaatst naar de wachtrij verontreinigd.| 
|newBatchThreshold|batchSize/2|De drempelwaarde waarmee een nieuwe batch van berichten worden opgehaald.| 

## <a name="servicebus"></a>Service Bus

Configuratie-instelling voor [Service Bus-triggers en bindingen](functions-bindings-service-bus.md).

[!INCLUDE [functions-host-json-service-bus](../../includes/functions-host-json-service-bus.md)]

## <a name="singleton"></a>Singleton

Configuratie-instellingen voor Singleton vergrendelen gedrag. Zie voor meer informatie [GitHub probleem over singleton ondersteuning](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|lockPeriod|00:00:15|De periode die functie niveau vergrendelingen voor worden uitgevoerd. De vergrendelingen wordt automatisch verlengen.| 
|listenerLockPeriod|00:01:00|De periode die listener vergrendelingen voor worden uitgevoerd.| 
|listenerLockRecoveryPollingInterval|00:01:00|Het tijdsinterval voor listener vergrendeling herstel moet worden gebruikt als een listener-vergrendeling kan niet worden verkregen bij het opstarten.| 
|lockAcquisitionTimeout|00:01:00|De maximale hoeveelheid tijd die de runtime probeert te verkrijgen van een vergrendeling.| 
|lockAcquisitionPollingInterval|N.v.t.|Het interval tussen pogingen voor vergrendeling verkrijgen.| 

## <a name="tracing"></a>tracering

Configuratie-instellingen voor de logboeken die u met behulp van maakt een `TraceWriter` object. Zie [C# logboekregistratie](functions-reference-csharp.md#logging) en [Node.js logboekregistratie](functions-reference-node.md#writing-trace-output-to-the-console). 

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|consoleLevel|Info|Het niveau van de tracering voor console-aanmelding. Opties zijn: `off`, `error`, `warning`, `info`, en `verbose`.|
|fileLoggingMode|debugOnly|De tracering niveau voor logboekregistratie. Opties zijn `never`, `always`, `debugOnly`.| 

## <a name="watchdirectories"></a>watchDirectories

Een set [gedeelde mappen code](functions-reference-csharp.md#watched-directories) die moeten worden gecontroleerd op wijzigingen.  Zorgt ervoor dat wanneer de code in deze mappen is gewijzigd, de wijzigingen die zijn opgepikt door uw functies.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="durabletask"></a>durableTask

[Taak hub](durable-functions-task-hubs.md) naam in voor [duurzame functies](durable-functions-overview.md).

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

Taak hub namen moeten beginnen met een letter en bestaan uit alleen letters en cijfers. Als u niet opgeeft, wordt de standaardnaam voor het hub van taak voor een functie-app is **DurableFunctionsHub**. Zie voor meer informatie [taak hubs](durable-functions-task-hubs.md).


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het bijwerken van het bestand host.json](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Zie globale instellingen in omgevingsvariabelen](functions-app-settings.md)
