---
title: naslaginformatie over host.JSON voor Azure Functions 1.x
description: Referentiedocumentatie voor de Azure Functions host.json-bestand met de v1-runtime.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: glenga
ms.openlocfilehash: ee82aab37973117b0c1960d8b75a29bfad38b7c7
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50252164"
---
# <a name="hostjson-reference-for-azure-functions-1x"></a>naslaginformatie over host.JSON voor Azure Functions 1.x

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Versie 1:](functions-host-json-v1.md)
> * [Versie 2](functions-host-json.md)

De *host.json* metagegevensbestand globale configuratie-opties die invloed hebben op alle functies die voor een functie-app bevat. In dit artikel bevat de instellingen die beschikbaar voor de v1-runtime zijn. Het JSON-schema loopt http://json.schemastore.org/host.

> [!NOTE]
> In dit artikel is bedoeld voor Azure Functions 1.x.  Voor een verwijzing van host.json in functies 2.x gebruikt, Zie [naslaginformatie over host.json voor Azure Functions 2.x](functions-host-json.md).

Andere configuratieopties van de functie-app worden beheerd in uw [app-instellingen](functions-app-settings.md).

Sommige instellingen host.json worden alleen gebruikt bij het uitvoeren van lokaal in de [local.settings.json](functions-run-local.md#local-settings-file) bestand.

## <a name="sample-hostjson-file"></a>Voorbeeldbestand voor host.json

Het volgende voorbeeld *host.json* bestanden hebben alle mogelijke opties opgegeven.


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
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
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

De volgende secties van dit artikel wordt uitgelegd dat elke eigenschap op het hoogste niveau. Alle zijn optioneel tenzij anders aangegeven.

## <a name="aggregator"></a>aggregator

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>Application Insights

[!INCLUDE [applicationInsights](../../includes/functions-host-json-applicationinsights.md)]

## <a name="durabletask"></a>durableTask

[!INCLUDE [durabletask](../../includes/functions-host-json-durabletask.md)]

## <a name="eventhub"></a>eventHub

Configuratie-instellingen voor [Event Hub-triggers en bindingen](functions-bindings-event-hubs.md).

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="functions"></a>functions

Een lijst met functies die de host van de taak wordt uitgevoerd. Een lege matrix betekent dat alle functies uitvoeren. Bedoeld voor gebruik alleen wanneer [lokaal uitgevoerde](functions-run-local.md). In de functie-apps in Azure, moet u in plaats daarvan de stappen in volgen [het uitschakelen van de functies in Azure Functions](disable-function.md) specifieke functies in plaats van met deze instelling uitschakelen.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Geeft aan dat de duur van de time-out voor alle functies. Het geldige bereik is 1 seconde tot 10 minuten in een verbruiksabonnement zonder server en de standaardwaarde is 5 minuten. Er is geen algemene limiet in een App Service-plan en de standaardwaarde is afhankelijk van de runtimeversie.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

Configuratie-instellingen voor [health monitor voor de Host](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor).

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|ingeschakeld|true|Hiermee geeft u op of de functie is ingeschakeld. | 
|healthCheckInterval|10 seconden|Het tijdsinterval tussen de periodieke achtergrond-status wordt gecontroleerd. | 
|healthCheckWindow|2 minuten|Een verschuivend tijdvenster gebruikt in combinatie met de `healthCheckThreshold` instelling.| 
|healthCheckThreshold|6|Maximum aantal keren dat de statuscontrole kan mislukken voordat het recyclen van een host wordt gestart.| 
|counterThreshold|0,80|De drempelwaarde waarmee een prestatiemeteritem wordt beschouwd als niet in orde.| 

## <a name="http"></a>http

Configuratie-instellingen voor [http-triggers en bindingen](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="id"></a>id

*Versie 1.x alleen.*

De unieke ID voor de host van een taak. Een kleine letter GUID met streepjes u kunt verwijderen. Vereist wanneer die lokaal wordt uitgevoerd. Bij het uitvoeren in Azure, wordt u aangeraden dat u een id-waarde niet instellen. Een ID in Azure automatisch wordt gegenereerd wanneer `id` wordt weggelaten. 

Als u een opslagaccount voor meerdere functie-apps deelt, zorg ervoor dat elke functie-app een andere heeft `id`. U kunt weglaten de `id` eigenschap of elke functie-app handmatig in te stellen `id` op een andere waarde. De timertrigger gebruikt een opslag-vergrendeling om ervoor te zorgen dat er slechts één instantie van de timer als een functie-app wordt geschaald naar meerdere exemplaren. Als twee functie-apps dezelfde delen `id` en elk een timertrigger gebruikt, slechts één timer wordt uitgevoerd.

```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>Logger

Besturingselementen voor filteren voor logboeken die is geschreven door een [ILogger object](functions-monitoring.md#write-logs-in-c-functions) of door [context.log](functions-monitoring.md#write-logs-in-javascript-functions).

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
|defaultLevel|Informatie|Voor de categorieën die niet is opgegeven in de `categoryLevels` matrix kunnen logboeken op dit niveau en hoger verzenden naar Application Insights.| 
|categoryLevels|N.v.t.|Een matrix van categorieën die Hiermee geeft u het minimale logboek-niveau te verzenden naar Application Insights voor elke categorie. Alle categorieën die met dezelfde waarde beginnen Hiermee beheert u de categorie die u hier opgeeft, en meer waarden hebben voorrang. In het voorgaande voorbeeld *host.json* -bestand, alle categorieën die beginnen met "Host.Aggregator" log op `Information` niveau. Alle andere categorieën die beginnen met 'Host', zoals "Host.Executor", zich aanmelden `Error` niveau.| 

## <a name="queues"></a>wachtrijen

Configuratie-instellingen voor [Storage queue-triggers en bindingen](functions-bindings-storage-queue.md).

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="servicebus"></a>serviceBus

Configuratie-instelling voor [Service Bus-triggers en bindingen](functions-bindings-service-bus.md).

[!INCLUDE [functions-host-json-service-bus](../../includes/functions-host-json-service-bus.md)]

## <a name="singleton"></a>Singleton

Configuratie-instellingen voor het gedrag van Singleton vergrendelen. Zie voor meer informatie, [GitHub-probleem over de ondersteuning van singleton](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
{
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
|lockPeriod|00:00:15|De periode die functie niveau vergrendelingen voor worden uitgevoerd. De vergrendelingen voor automatisch verlengen.| 
|listenerLockPeriod|00:01:00|De periode die listener vergrendelingen voor worden uitgevoerd.| 
|listenerLockRecoveryPollingInterval|00:01:00|Het tijdsinterval voor listener-lock herstel moet worden gebruikt als een listener-vergrendeling kan niet worden verkregen bij het opstarten.| 
|lockAcquisitionTimeout|00:01:00|De maximale hoeveelheid tijd die de runtime wordt geprobeerd om een vergrendeling te verkrijgen.| 
|lockAcquisitionPollingInterval|N.v.t.|Het interval tussen pogingen van vergrendeling ophalen.| 

## <a name="tracing"></a>tracering

*Versie 1.x*

Configuratie-instellingen voor logboeken die u met behulp van maakt een `TraceWriter` object. Zie [C# logboekregistratie](functions-reference-csharp.md#logging) en [Node.js logboekregistratie](functions-reference-node.md#writing-trace-output-to-the-console).

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
|consoleLevel|informatie|Het traceringsniveau bepaald voor console-aanmelding. Opties zijn: `off`, `error`, `warning`, `info`, en `verbose`.|
|fileLoggingMode|debugOnly|Het traceringsniveau bepaald voor logboekregistratie. Opties zijn `never`, `always`, `debugOnly`.| 

## <a name="watchdirectories"></a>watchDirectories

Een set [gedeelde code mappen](functions-reference-csharp.md#watched-directories) die moet worden gecontroleerd op wijzigingen.  Zorgt ervoor dat wanneer de code in deze mappen wordt gewijzigd, de wijzigingen worden doorgevoerd door uw functies.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het bijwerken van het bestand host.json](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Zie de globale instellingen in omgevingsvariabelen](functions-app-settings.md)
