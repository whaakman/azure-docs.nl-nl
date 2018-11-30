---
title: naslaginformatie over host.JSON voor Azure Functions 2.x
description: Referentiedocumentatie voor de Azure Functions host.json-bestand met de v2-runtime.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/08/2018
ms.author: glenga
ms.openlocfilehash: b429f9dab0411dba599fd696dff3d424916007e0
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52635166"
---
# <a name="hostjson-reference-for-azure-functions-2x"></a>naslaginformatie over host.JSON voor Azure Functions 2.x  

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Versie 1:](functions-host-json-v1.md)
> * [Versie 2](functions-host-json.md)

De *host.json* metagegevensbestand globale configuratie-opties die invloed hebben op alle functies die voor een functie-app bevat. In dit artikel bevat de instellingen die beschikbaar voor de v2-runtime zijn.  

> [!NOTE]
> In dit artikel is bedoeld voor Azure Functions 2.x.  Voor een verwijzing van host.json in functies 1.x, Zie [naslaginformatie over host.json voor Azure Functions 1.x](functions-host-json-v1.md).

Andere configuratieopties van de functie-app worden beheerd in uw [app-instellingen](functions-app-settings.md).

Sommige instellingen host.json worden alleen gebruikt bij het uitvoeren van lokaal in de [local.settings.json](functions-run-local.md#local-settings-file) bestand.

## <a name="sample-hostjson-file"></a>Voorbeeldbestand voor host.json

Het volgende voorbeeld *host.json* bestanden hebben alle mogelijke opties opgegeven.


```json
{
    "version": "2.0",
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "extensions": {
        "cosmosDb": {},
        "durableTask": {},
        "eventHubs": {},
        "http": {},
        "queues": {},
        "sendGrid": {},
        "serviceBus": {}
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
    "logging": {
        "fileLoggingMode": "debugOnly",
        "logLevel": {
          "Function.MyFunction": "Information",
          "default": "None"
        },
        "applicationInsights": {
            "samplingSettings": {
              "isEnabled": true,
              "maxTelemetryItemsPerSecond" : 5
            }
        }
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "watchDirectories": [ "Shared", "Test" ]
}
```

De volgende secties van dit artikel wordt uitgelegd dat elke eigenschap op het hoogste niveau. Alle zijn optioneel tenzij anders aangegeven.

## <a name="aggregator"></a>aggregator

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>Application Insights

Deze instelling is een onderliggend element van [logboekregistratie](#logging).

[!INCLUDE [applicationInsights](../../includes/functions-host-json-applicationinsights.md)]

## <a name="cosmosdb"></a>cosmos DB

Configuratie-instelling kunt u vinden in [Cosmos DB-triggers en bindingen](functions-bindings-cosmosdb-v2.md#host-json).

## <a name="durabletask"></a>durableTask

Configuratie-instelling kunt u vinden in [bindingen voor duurzame functies](durable/durable-functions-bindings.md#host-json).

## <a name="eventhub"></a>eventHub

Configuratie-instellingen kunnen u vinden in [Event Hub-triggers en bindingen](functions-bindings-event-hubs.md#host-json). 

## <a name="extensions"></a>Extensies

Eigenschap retourneert een object dat alle van de binding-specifieke instellingen, zoals bevat [http](#http) en [eventHub](#eventhub).

## <a name="functions"></a>functions

Een lijst met functies die de host van de taak wordt uitgevoerd. Een lege matrix betekent dat alle functies uitvoeren. Bedoeld voor gebruik alleen wanneer [lokaal uitgevoerde](functions-run-local.md). In de functie-apps in Azure, moet u in plaats daarvan de stappen in volgen [het uitschakelen van de functies in Azure Functions](disable-function.md) specifieke functies in plaats van met deze instelling uitschakelen.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Geeft aan dat de duur van de time-out voor alle functies. Het geldige bereik is 1 seconde tot 10 minuten in een verbruiksabonnement zonder server en de standaardwaarde is 5 minuten. Er is geen algemene limiet in een App Service-plan en de standaardwaarde is afhankelijk van de runtimeversie. In versie 2.x, de standaardwaarde voor een App Service-plan 30 minuten is. In versie 1.x, is het *null*, wat aangeeft dat het geen time-out.

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

Configuratie-instellingen kunnen u vinden in [http-triggers en bindingen](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="logging"></a>Logboekregistratie

Hiermee bepaalt u het gedrag van de registratie van de functie-app, met inbegrip van Application Insights.

```json
"logging": {
    "fileLoggingMode": "debugOnly",
    "logLevel": {
      "Function.MyFunction": "Information",
      "default": "None"
    },
    "applicationInsights": {
        ...
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------|
|fileLoggingMode|debugOnly|Hiermee definieert u welk niveau van logboekregistratie is ingeschakeld.  Opties zijn `never`, `always`, `debugOnly`. |
|LogLevel|N.v.t.|Object dat definieert de logboekcategorie filteren voor functies in de app. Versie 2.x volgt de ASP.NET Core-indeling voor het filteren van logboek-categorie. Hiermee kunt u filteren van logboekregistratie voor specifieke functies. Zie voor meer informatie, [logboek filteren](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering) in de documentatie van ASP.NET Core. |
|Application Insights|N.v.t.| De [applicationInsights](#applicationinsights) instelling. |

## <a name="queues"></a>wachtrijen

Configuratie-instellingen kunnen u vinden in [Storage queue-triggers en bindingen](functions-bindings-storage-queue.md#host-json).  

## <a name="sendgrid"></a>SendGrid

Configuratie-instelling kunt u vinden in [SendGrid triggers en bindingen](functions-bindings-sendgrid.md#host-json).

## <a name="servicebus"></a>serviceBus

Configuratie-instelling kunt u vinden in [Service Bus-triggers en bindingen](functions-bindings-service-bus.md#host-json).

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

## <a name="version"></a>versie

De tekenreeks voor de `"version": "2.0"` is vereist voor een functie-app die gericht is op de v2-runtime.

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
