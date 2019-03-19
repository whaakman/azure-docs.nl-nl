---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 03/14/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d79d1bd5ec244ad4399a02c349e2504516d06ccd
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58137647"
---
Configuratie-instellingen voor [duurzame functies](../articles/azure-functions/durable-functions-overview.md).

```json
{
  "durableTask": {
    "hubName": "MyTaskHub",
    "controlQueueBatchSize": 32,
    "partitionCount": 4,
    "controlQueueVisibilityTimeout": "00:05:00",
    "workItemQueueVisibilityTimeout": "00:05:00",
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
    "maxQueuePollingInterval": "00:00:30",
    "azureStorageConnectionStringName": "AzureWebJobsStorage",
    "trackingStoreConnectionStringName": "TrackingStorage",
    "trackingStoreNamePrefix": "DurableTask",
    "traceInputsAndOutputs": false,
    "logReplayEvents": false,
    "eventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "eventGridKeySettingName":  "EventGridKey",
    "eventGridPublishRetryCount": 3,
    "eventGridPublishRetryInterval": "00:00:30",
    "eventGridPublishEventTypes": ["Started", "Pending", "Failed", "Terminated"]
  }
}
```

Namen van taken hub moeten beginnen met een letter en bestaan uit alleen letters en cijfers. Indien niet opgegeven, wordt de standaardnaam voor het hub van taak voor een functie-app is **DurableFunctionsHub**. Zie voor meer informatie, [taak hubs](../articles/azure-functions/durable-functions-task-hubs.md).

|Eigenschap  |Standaard | Description |
|---------|---------|---------|
|hubName|DurableFunctionsHub|Alternatieve [taak hub](../articles/azure-functions/durable-functions-task-hubs.md) namen kunnen worden gebruikt voor het isoleren van meerdere duurzame functies toepassingen van elkaar worden verbonden, zelfs als ze de dezelfde opslag back-end gebruiken.|
|controlQueueBatchSize|32|Het aantal berichten om op te halen vanuit de besturingselement-wachtrij op een tijdstip.|
|partitionCount |4|Het aantal partities voor de wachtrij van het besturingselement. Een positief geheel getal tussen 1 en 16 mogelijk.|
|controlQueueVisibilityTimeout |5 minuten|De time-out voor zichtbaarheid van besturingselement voor uit wachtrij geplaatste berichten in wachtrij plaatsen.|
|workItemQueueVisibilityTimeout |5 minuten|De time-out voor zichtbaarheid van berichten in de wachtrij-item uit de wachtrij genomen werk.|
|maxConcurrentActivityFunctions |10 x het aantal processors op de huidige computer|Het maximale aantal activiteitsfuncties mediataken tegelijk kunnen worden verwerkt op een afzonderlijke host-instantie.|
|maxConcurrentOrchestratorFunctions |10 x het aantal processors op de huidige computer|Het maximale aantal orchestrator-functies die gelijktijdig kunnen worden verwerkt op een afzonderlijke host-instantie.|
|maxQueuePollingInterval|30 seconden|De maximale controle en de polling-interval van werkitem wachtrij in de *uu: mm:* indeling. Hoe hoger de waarde kunnen leiden tot hogere latenties berichtverwerking. Lagere waarden kunnen vanwege toegenomen opslagtransacties leiden tot hogere kosten voor opslag.|
|azureStorageConnectionStringName |AzureWebJobsStorage|De naam van de appinstelling met de Azure Storage-verbindingsreeks die wordt gebruikt om de onderliggende Azure Storage-resources te beheren.|
|trackingStoreConnectionStringName||De naam van een verbindingsreeks te gebruiken voor de tabellen geschiedenis en instanties. Indien niet opgegeven, de `azureStorageConnectionStringName` verbinding wordt gebruikt.|
|trackingStoreNamePrefix||Het voorvoegsel moet worden gebruikt voor de geschiedenis en exemplaren tabellen wanneer `trackingStoreConnectionStringName` is opgegeven. Als niet is ingesteld, de standaardwaarde van het voorvoegsel zich `DurableTask`. Als `trackingStoreConnectionStringName` niet is opgegeven, worden de tabellen geschiedenis en exemplaren gebruikt de `hubName` waarde als het voorvoegsel en alle instellingen voor `trackingStoreNamePrefix` worden genegeerd.|
|traceInputsAndOutputs |false|Een waarde die aangeeft of de invoer en uitvoer van functieaanroepen traceren. Het standaardgedrag voor het traceren van gebeurtenissen voor de functie kan worden uitgevoerd, moet u het aantal bytes in de geserialiseerde invoer en uitvoer voor de functieaanroepen. Dit gedrag biedt minimale informatie over hoe de invoer en uitvoer eruit zien zonder aanzienlijk groter worden de logboeken of per ongeluk gevoelige informatie om vrij te geven. Deze eigenschap instelt op true, zal de functie standaard logboekregistratie om aan te melden van de volledige inhoud van de functie-invoer en uitvoer.|
|LogReplayEvents|false|Een waarde die aangeeft of orchestration opnieuw afspelen gebeurtenissen schrijven naar Application Insights.|
|eventGridTopicEndpoint ||De URL van een Azure Event Grid-aangepast onderwerp-eindpunt. Als deze eigenschap is ingesteld, worden orchestration levenscyclus meldingsgebeurtenissen worden gepubliceerd naar dit eindpunt. Deze eigenschap ondersteunt resolutie van App-instellingen.|
|eventGridKeySettingName ||De naam van de app-instelling met de sleutel die wordt gebruikt voor verificatie met de aangepaste Azure Event Grid-onderwerp op `EventGridTopicEndpoint`.|
|eventGridPublishRetryCount|0|Het aantal nieuwe pogingen als publiceren naar de Event Grid-onderwerp is mislukt.|
|eventGridPublishRetryInterval|5 minuten|De Event Grid publiceert interval voor opnieuw proberen in de *uu: mm:* indeling.|
|eventGridPublishEventTypes||Een lijst met typen gebeurtenissen om te publiceren naar Event Grid. Indien niet opgegeven, worden alle gebeurtenistypen wordt gepubliceerd. Toegestane waarden zijn onder andere `Started`, `Completed`, `Failed`, `Terminated`.|

Veel van deze instellingen zijn voor het optimaliseren van prestaties. Zie voor meer informatie, [prestaties en schaal](../articles/azure-functions/durable-functions-perf-and-scale.md).