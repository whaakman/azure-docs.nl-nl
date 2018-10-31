---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 92eb13165326f44432f09322ea97f3cee5ccec2b
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50251240"
---
Configuratie-instellingen voor [duurzame functies](../articles/azure-functions/durable-functions-overview.md).

```json
{
  "durableTask": {
    "HubName": "MyTaskHub",
    "ControlQueueBatchSize": 32,
    "PartitionCount": 4,
    "ControlQueueVisibilityTimeout": "00:05:00",
    "WorkItemQueueVisibilityTimeout": "00:05:00",
    "MaxConcurrentActivityFunctions": 10,
    "MaxConcurrentOrchestratorFunctions": 10,
    "AzureStorageConnectionStringName": "AzureWebJobsStorage",
    "TraceInputsAndOutputs": false,
    "LogReplayEvents": false,
    "EventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "EventGridKeySettingName":  "EventGridKey",
    "EventGridPublishRetryCount": 3,
    "EventGridPublishRetryInterval": "00:00:30"
  }
}
```

Namen van taken hub moeten beginnen met een letter en bestaan uit alleen letters en cijfers. Indien niet opgegeven, wordt de standaardnaam voor het hub van taak voor een functie-app is **DurableFunctionsHub**. Zie voor meer informatie, [taak hubs](../articles/azure-functions/durable-functions-task-hubs.md).

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------|
|HubName|DurableFunctionsHub|Alternatieve [taak hub](../articles/azure-functions/durable-functions-task-hubs.md) namen kunnen worden gebruikt voor het isoleren van meerdere duurzame functies toepassingen van elkaar worden verbonden, zelfs als theyre met behulp van de dezelfde opslag back-end.|
|ControlQueueBatchSize|32|Het aantal berichten om op te halen vanuit de besturingselement-wachtrij op een tijdstip.|
|PartitionCount |4|Het aantal partities voor de wachtrij van het besturingselement. Een positief geheel getal tussen 1 en 16 mogelijk.|
|ControlQueueVisibilityTimeout |5 minuten|De time-out voor zichtbaarheid van besturingselement voor uit wachtrij geplaatste berichten in wachtrij plaatsen.|
|WorkItemQueueVisibilityTimeout |5 minuten|De time-out voor zichtbaarheid van berichten in de wachtrij-item uit de wachtrij genomen werk.|
|MaxConcurrentActivityFunctions |10 x het aantal processors op de huidige computer|Het maximale aantal activiteitsfuncties mediataken tegelijk kunnen worden verwerkt op een afzonderlijke host-instantie.|
|MaxConcurrentOrchestratorFunctions |10 x het aantal processors op de huidige computer|Het maximale aantal orchestrator-functies die gelijktijdig kunnen worden verwerkt op een afzonderlijke host-instantie.|
|AzureStorageConnectionStringName |AzureWebJobsStorage|De naam van de appinstelling met de Azure Storage-verbindingsreeks die wordt gebruikt om de onderliggende Azure Storage-resources te beheren.|
|TraceInputsAndOutputs |false|Een waarde die aangeeft of de invoer en uitvoer van functieaanroepen traceren. Het standaardgedrag voor het traceren van gebeurtenissen voor de functie kan worden uitgevoerd, moet u het aantal bytes in de geserialiseerde invoer en uitvoer voor de functieaanroepen. Dit biedt minimale informatie over hoe de invoer en uitvoer eruit zien zonder aanzienlijk groter worden de logboeken of per ongeluk gevoelige informatie naar de logboeken om vrij te geven. Deze eigenschap instelt op true, zal de functie standaard logboekregistratie om aan te melden van de volledige inhoud van de functie-invoer en uitvoer.|
|LogReplayEvents|false|Een waarde die aangeeft of orchestration opnieuw afspelen gebeurtenissen schrijven naar Application Insights.|
|EventGridTopicEndpoint ||De URL van een Azure Event Grid-aangepast onderwerp-eindpunt. Als deze eigenschap is ingesteld, worden orchestration levenscyclus van de meldingsgebeurtenissen worden gepubliceerd naar dit eindpunt. Deze eigenschap ondersteunt resolutie van App-instellingen.|
|EventGridKeySettingName ||De naam van de app-instelling met de sleutel die wordt gebruikt voor verificatie met de aangepaste Azure Event Grid-onderwerp op `EventGridTopicEndpoint`.|
|EventGridPublishRetryCount|0|Het aantal nieuwe pogingen als publiceren naar de Event Grid-onderwerp is mislukt.|
|EventGridPublishRetryInterval|5 minuten|De Event Grid publiceert interval voor opnieuw proberen in de *uu: mm:* indeling.|

Veel van deze zijn voor het optimaliseren van prestaties. Zie voor meer informatie, [prestaties en schaal](../articles/azure-functions/durable-functions-perf-and-scale.md).