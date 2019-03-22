---
title: HTTP-API's in duurzame functies - Azure
description: Informatie over het implementeren van HTTP APIs in de extensie duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: azfuncdf
ms.openlocfilehash: 5bd977826f489ca8452432babe6126b8553450fb
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58137705"
---
# <a name="http-apis-in-durable-functions-azure-functions"></a>HTTP-API's in duurzame functies (Azure Functions)

De extensie duurzame taak bevat een set HTTP-APIs die kan worden gebruikt om de volgende taken uitvoeren:

* Haal de status van een orchestration-exemplaar.
* Een gebeurtenis verzenden naar een exemplaar van de orchestration wachten.
* Een actief exemplaar van de orchestration beëindigen.

Elk van deze APIs HTTP is een webhook-bewerking die wordt verwerkt door de extensie duurzame taak direct. Ze zijn niet specifiek zijn voor een functie in de functie-app.

> [!NOTE]
> Deze bewerkingen kunnen ook worden aangeroepen rechtstreeks met behulp van de beheer-API's van het exemplaar op de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse. Zie voor meer informatie, [Instantiebeheer](durable-functions-instance-management.md).

## <a name="http-api-url-discovery"></a>De URL van de HTTP-API-detectie

De [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse toont een [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_) API die kan worden gebruikt voor het genereren van een nettolading van het HTTP-antwoord met koppelingen naar alle ondersteunde bewerkingen. Hier volgt een voorbeeld van de HTTP-trigger-functie die wordt gedemonstreerd hoe u deze API:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/HttpStart/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/index.js)]

Deze functies voorbeeld geven de volgende gegevens van de JSON-antwoord. Het gegevenstype van alle velden `string`.

| Veld                   |Description                           |
|-------------------------|--------------------------------------|
| **`id`**                |De ID van de orchestration-exemplaar. |
| **`statusQueryGetUri`** |De URL van de status van de orchestration-exemplaar. |
| **`sendEventPostUri`**  |De URL 'raise gebeurtenis' van de orchestration-exemplaar. |
| **`terminatePostUri`**  |De URL 'beëindigd' van de orchestration-exemplaar. |
| **`rewindPostUri`**     |De URL 'terugspoelen' van de orchestration-exemplaar. |

Hier volgt een voorbeeld van de reactie:

```http
HTTP/1.1 202 Accepted
Content-Length: 923
Content-Type: application/json; charset=utf-8
Location: https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX

{
    "id":"34ce9a28a6834d8492ce6a295f1a80e2",
    "statusQueryGetUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "sendEventPostUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "terminatePostUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "rewindPostUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2/rewind?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX"
}
```

> [!NOTE]
> De indeling van de webhook-URL's kan verschillen afhankelijk van welke versie van de Azure Functions-host die u uitvoert. Het bovenstaande voorbeeld is voor de Azure Functions 2.x-host.

## <a name="async-operation-tracking"></a>Asynchrone bewerking bijhouden

Het HTTP-antwoord dat eerder is vermeld is ontworpen voor het implementeren van langdurige HTTP asynchrone API's met duurzame functies. Dit wordt soms aangeduid als de *Polling consument patroon*. De client/server-stroom werkt als volgt:

1. De client verzendt een HTTP-aanvraag om een langlopende proces, zoals een orchestrator-functie te starten.
2. De-HTTP-doeltrigger retourneert een HTTP-202-antwoord met een `Location` -header met de `statusQueryGetUri` waarde.
3. De client controleert de URL in de `Location` header. Zie HTTP 202-antwoord met gaat door een `Location` header.
4. Wanneer het exemplaar is voltooid (of mislukt), het eindpunt in de `Location` header HTTP 200 wordt geretourneerd.

Met dit protocol kunnen coördineren van langlopende processen met externe clients of services die ondersteuning bieden voor een HTTP-eindpunt polling en te volgen de `Location` header. De fundamentele onderdelen zijn al ingebouwd in de duurzame functies HTTP-API's.

> [!NOTE]
> Standaard worden alle acties voor HTTP-gebaseerde geleverd door [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) ondersteunen het patroon standaard asynchrone bewerking. Deze mogelijkheid maakt het mogelijk om te sluiten van een langlopende duurzame functie als onderdeel van een Logic Apps-werkstroom. Meer informatie over Logic Apps ondersteuning voor asynchrone HTTP patronen te vinden in de [Azure Logic Apps-werkstroom acties en triggers-documentatie](../../logic-apps/logic-apps-workflow-actions-triggers.md#asynchronous-patterns).

## <a name="http-api-reference"></a>HTTP-API-verwijzing

Alle HTTP APIs geïmplementeerd door de extensie voor toets maken de volgende parameters. Het gegevenstype van alle parameters `string`.

| Parameter        | Parametertype  | Description |
|------------------|-----------------|-------------|
| **`taskHub`**    | Querytekenreeks    | De naam van de [taak hub](durable-functions-task-hubs.md). Als niet is opgegeven, wordt de taaknaam hub van de huidige functie-app gebruikt. |
| **`connection`** | Querytekenreeks    | De **naam** van de verbindingsreeks voor de storage-account. Als niet is opgegeven, wordt de standaard-verbindingsreeks voor de functie-app gebruikt. |
| **`systemKey`**  | Querytekenreeks    | De autorisatiesleutel is vereist voor het aanroepen van de API. |

`systemKey` een autorisatiesleutel wordt automatisch gegenereerd door de Azure Functions-host. Het speciaal verleent toegang tot de extensie duurzame taak API's en kunnen worden beheerd als dezelfde manier als [andere sleutels voor de verificatieregel](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Key-management-API). De eenvoudigste manier voor het detecteren van de `systemKey` waarde is met behulp van de `CreateCheckStatusResponse` API eerder is vermeld.

In de volgende secties gaan over de specifieke HTTP APIs ondersteund door de extensie en vindt u voorbeelden van hoe ze kunnen worden gebruikt.

### <a name="get-instance-status"></a>Status van het exemplaar ophalen

Hiermee haalt u de status van een opgegeven orchestration-exemplaar.

#### <a name="request"></a>Aanvraag

Voor versie 1.x van de Functions-runtime, de aanvraag is opgemaakt als volgt te werk (meerdere regels worden weergegeven voor de duidelijkheid):

```http
GET /admin/extensions/DurableTaskExtension/instances/{instanceId}
    ?taskHub={taskHub
    &connection={connectionName}
    &code={systemKey}
    &showHistory=[true|false]
    &showHistoryOutput=[true|false]
    &showInput=[true|false]
```

In versie 2.x van de Functions-runtime, de URL-indeling heeft dezelfde parameters, maar met een iets ander voorvoegsel:

```http
GET /runtime/webhooks/durabletask/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &showHistory=[true|false]
    &showHistoryOutput=[true|false]
    &showInput=[true|false]
```

Aanvraag-parameters voor deze API bevatten de standaardset die eerder is vermeld en de volgende unieke parameters:

| Veld                   | Parametertype  | Description |
|-------------------------|-----------------|-------------|
| **`instanceId`**        | URL             | De ID van de orchestration-exemplaar. |
| **`showInput`**         | Querytekenreeks    | Een optionele parameter. Indien ingesteld op `false`, de functie de invoer niet worden opgenomen in de nettolading van de reactie.|
| **`showHistory`**       | Querytekenreeks    | Een optionele parameter. Indien ingesteld op `true`, de orchestration-uitvoeringsgeschiedenis worden opgenomen in de nettolading van de reactie.|
| **`showHistoryOutput`** | Querytekenreeks    | Een optionele parameter. Indien ingesteld op `true`, de functie levert worden opgenomen in de orchestration-uitvoeringsgeschiedenis.|
| **`createdTimeFrom`**   | Querytekenreeks    | Een optionele parameter. Als u opgeeft, filtert de lijst met geretourneerde exemplaren die zijn gemaakt op of na de opgegeven ISO8601-timestamp.|
| **`createdTimeTo`**     | Querytekenreeks    | Een optionele parameter. Als u opgeeft, filtert de lijst met geretourneerde exemplaren die zijn gemaakt op of voor de opgegeven ISO8601-timestamp.|
| **`runtimeStatus`**     | Querytekenreeks    | Een optionele parameter. Als u opgeeft, filters de lijst met geretourneerde exemplaren op basis van hun runtimestatus. Zie de lijst met waarden voor mogelijke runtime status, de [uitvoeren van query's exemplaren](durable-functions-instance-management.md) onderwerp. |

#### <a name="response"></a>Antwoord

Verschillende mogelijke status codewaarden kunnen worden geretourneerd.

* **HTTP 200 (OK)**: Het opgegeven exemplaar is met een onvoltooide status.
* **HTTP 202 (aanvaard)**: Het opgegeven exemplaar wordt uitgevoerd.
* **HTTP 400 (foute aanvraag)**: Het opgegeven exemplaar is mislukt of is beëindigd.
* **HTTP 404 (Not Found)**: Het opgegeven exemplaar bestaat niet of is niet gestart.
* **HTTP 500 (interne serverfout)**: Het opgegeven exemplaar is mislukt met een onverwerkte uitzondering.

De nettolading van de reactie voor de **HTTP 200** en **HTTP 202** gevallen is een JSON-object met de volgende velden:

| Veld                 | Gegevenstype | Description |
|-----------------------|-----------|-------------|
| **`runtimeStatus`**   | string    | De runtimestatus van het exemplaar. Mogelijke waarden zijn *met*, *in behandeling*, *mislukt*, *geannuleerd*, *beëindigd*, *Voltooid*. |
| **`input`**           | JSON      | De JSON-gegevens die wordt gebruikt voor het initialiseren van het exemplaar. Dit veld is `null` als de `showInput` queryreeks-parameter is ingesteld op `false`.|
| **`customStatus`**    | JSON      | De JSON-gegevens die worden gebruikt voor de status van de aangepaste indeling. Dit veld is `null` niet ingesteld. |
| **`output`**          | JSON      | De JSON-uitvoer van het exemplaar. Dit veld is `null` als het exemplaar niet met een onvoltooide status is. |
| **`createdTime`**     | string    | De tijd waarop het exemplaar is gemaakt. Maakt gebruik van ISO 8601-notatie uitgebreid. |
| **`lastUpdatedTime`** | string    | De tijd waarop het exemplaar is opgeslagen. Maakt gebruik van ISO 8601-notatie uitgebreid. |
| **`historyEvents`**   | JSON      | Een JSON-matrix met de orchestration-uitvoeringsgeschiedenis. Dit veld is `null` , tenzij de `showHistory` queryreeks-parameter is ingesteld op `true`. |

Hier volgt een voorbeeld van payload van he antwoord met inbegrip van de orchestration uitvoer geschiedenis en activiteit van uitvoerbewerkingen (indeling voor de leesbaarheid):

```json
{
  "createdTime": "2018-02-28T05:18:49Z",
  "historyEvents": [
      {
          "EventType": "ExecutionStarted",
          "FunctionName": "E1_HelloSequence",
          "Timestamp": "2018-02-28T05:18:49.3452372Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello Tokyo!",
          "ScheduledTime": "2018-02-28T05:18:51.3939873Z",
          "Timestamp": "2018-02-28T05:18:52.2895622Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello Seattle!",
          "ScheduledTime": "2018-02-28T05:18:52.8755705Z",
          "Timestamp": "2018-02-28T05:18:53.1765771Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello London!",
          "ScheduledTime": "2018-02-28T05:18:53.5170791Z",
          "Timestamp": "2018-02-28T05:18:53.891081Z"
      },
      {
          "EventType": "ExecutionCompleted",
          "OrchestrationStatus": "Completed",
          "Result": [
              "Hello Tokyo!",
              "Hello Seattle!",
              "Hello London!"
          ],
          "Timestamp": "2018-02-28T05:18:54.3660895Z"
      }
  ],
  "input": null,
  "customStatus": { "nextActions": ["A", "B", "C"], "foo": 2 },
  "lastUpdatedTime": "2018-02-28T05:18:54Z",
  "output": [
      "Hello Tokyo!",
      "Hello Seattle!",
      "Hello London!"
  ],
  "runtimeStatus": "Completed"
}
```

De **HTTP 202** antwoord bevat ook een **locatie** response-header die verwijst naar dezelfde URL als de `statusQueryGetUri` veld eerder is vermeld.

### <a name="get-all-instances-status"></a>Status van alle exemplaren ophalen

U kunt ook de status van alle exemplaren van de query door het verwijderen van de `instanceId` van de aanvraag 'Get status exemplaar'. In dit geval zijn de eenvoudige parameters hetzelfde als de 'Get-exemplaar status'. Queryreeksparameters voor het filteren worden ook ondersteund.

Eén ding te onthouden is dat `connection` en `code` zijn optioneel. Als u anonieme verificatie op de functie hebt en vervolgens de code is niet vereist.
Als u niet wilt om de verbindingsreeks van een andere opslag dan gedefinieerd in de app-instelling van het AzureWebJobsStorage te gebruiken, kunt u de verbinding queryreeks-parameter veilig negeren.

#### <a name="request"></a>Aanvraag

Voor versie 1.x van de Functions-runtime, de aanvraag is opgemaakt als volgt te werk (meerdere regels worden weergegeven voor de duidelijkheid):

```http
GET /admin/extensions/DurableTaskExtension/instances
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
    &showInput=[true|false]
    &top={integer}
```

In versie 2.x van de Functions-runtime, de URL-indeling heeft dezelfde parameters, maar met een iets ander voorvoegsel:

```http
GET /runtime/webhooks/durableTask/instances?
    taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
    &showInput=[true|false]
    &top={integer}
```

Aanvraag-parameters voor deze API bevatten de standaardset die eerder is vermeld en de volgende unieke parameters:

| Veld                   | Parametertype  | Description |
|-------------------------|-----------------|-------------|
| **`instanceId`**        | URL             | De ID van de orchestration-exemplaar. |
| **`showInput`**         | Querytekenreeks    | Een optionele parameter. Indien ingesteld op `false`, de functie de invoer niet worden opgenomen in de nettolading van de reactie.|
| **`showHistory`**       | Querytekenreeks    | Een optionele parameter. Indien ingesteld op `true`, de orchestration-uitvoeringsgeschiedenis worden opgenomen in de nettolading van de reactie.|
| **`showHistoryOutput`** | Querytekenreeks    | Een optionele parameter. Indien ingesteld op `true`, de functie levert worden opgenomen in de orchestration-uitvoeringsgeschiedenis.|
| **`createdTimeFrom`**   | Querytekenreeks    | Een optionele parameter. Als u opgeeft, filtert de lijst met geretourneerde exemplaren die zijn gemaakt op of na de opgegeven ISO8601-timestamp.|
| **`createdTimeTo`**     | Querytekenreeks    | Een optionele parameter. Als u opgeeft, filtert de lijst met geretourneerde exemplaren die zijn gemaakt op of voor de opgegeven ISO8601-timestamp.|
| **`runtimeStatus`**     | Querytekenreeks    | Een optionele parameter. Als u opgeeft, filters de lijst met geretourneerde exemplaren op basis van hun runtimestatus. Zie de lijst met waarden voor mogelijke runtime status, de [uitvoeren van query's exemplaren](durable-functions-instance-management.md) onderwerp. |
| **`top`**               | Querytekenreeks    | Een optionele parameter. Als u opgeeft, beperkt het aantal exemplaren geretourneerd door de query. |

#### <a name="response"></a>Antwoord

Hier volgt een voorbeeld van antwoordpayloads, met inbegrip van de orchestration-status (indeling voor de leesbaarheid):

```json
[
    {
        "instanceId": "7af46ff000564c65aafbfe99d07c32a5",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-06-04T10:46:39Z",
        "lastUpdatedTime": "2018-06-04T10:46:47Z"
    },
    {
        "instanceId": "80eb7dd5c22f4eeba9f42b062794321e",
        "runtimeStatus": "Running",
        "input": null,
        "customStatus": null,
        "output": null,
        "createdTime": "2018-06-04T15:18:28Z",
        "lastUpdatedTime": "2018-06-04T15:18:38Z"
    },
    {
        "instanceId": "9124518926db408ab8dfe84822aba2b1",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-06-04T10:46:54Z",
        "lastUpdatedTime": "2018-06-04T10:47:03Z"
    },
    {
        "instanceId": "d100b90b903c4009ba1a90868331b11b",
        "runtimeStatus": "Pending",
        "input": null,
        "customStatus": null,
        "output": null,
        "createdTime": "2018-06-04T15:18:39Z",
        "lastUpdatedTime": "2018-06-04T15:18:39Z"
    }
]
```

> [!NOTE]
> Met deze bewerking kan zeer kostbaar in termen van Azure-opslag i/o zijn als er een groot aantal rijen in de tabel exemplaren. Meer informatie over exemplaar tabel vindt u de [prestaties en schaalbaarheid in duurzame functies (Azure Functions)](durable-functions-perf-and-scale.md#instances-table) documentatie.
>

Als meer resultaten bestaat, wordt een vervolgtoken geretourneerd in de antwoordkop.  De naam van de koptekst is `x-ms-continuation-token`.

Als u in de volgende aanvraagheader voortzetting van token waarde hebt ingesteld, krijgt u de volgende pagina van de resultaten. Deze naam van de aanvraagheader is ook `x-ms-continuation-token`.

### <a name="purge-single-instance-history"></a>Geschiedenis van één exemplaar leegmaken

Hiermee verwijdert u de geschiedenis en verwante artefacts voor een opgegeven orchestration-exemplaar.

#### <a name="request"></a>Aanvraag

Voor versie 1.x van de Functions-runtime, de aanvraag is opgemaakt als volgt te werk (meerdere regels worden weergegeven voor de duidelijkheid):

```http
DELETE /admin/extensions/DurableTaskExtension/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connection}
    &code={systemKey}
```

In versie 2.x van de Functions-runtime, de URL-indeling heeft dezelfde parameters, maar met een iets ander voorvoegsel:

```http
DELETE /runtime/webhooks/durabletask/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connection}
    &code={systemKey}
```

Aanvraag-parameters voor deze API bevatten de standaardset die eerder is vermeld en de volgende unieke parameters:

| Veld             | Parametertype  | Description |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL             | De ID van de orchestration-exemplaar. |

#### <a name="response"></a>Antwoord

De volgende HTTP-status codewaarden kunnen worden geretourneerd.

* **HTTP 200 (OK)**: De exemplaar-geschiedenis is is leeggemaakt.
* **HTTP 404 (Not Found)**: Het opgegeven exemplaar bestaat niet.

De nettolading van de reactie voor de **HTTP 200** geval wordt een JSON-object met het volgende veld:

| Veld                  | Gegevenstype | Description |
|------------------------|-----------|-------------|
| **`instancesDeleted`** | geheel getal   | Het aantal exemplaren verwijderd. Voor het geval één exemplaar zijn deze waarde altijd `1`. |

Hier volgt een voorbeeld van payload van he antwoord (indeling voor de leesbaarheid):

```json
{
    "instancesDeleted": 1
}
```

### <a name="purge-multiple-instance-history"></a>Meerdere exemplaar Geschiedenis leegmaken

U kunt de geschiedenis en verwante artefacts voor meerdere exemplaren binnen een hub taak ook verwijderen door het verwijderen van de `{instanceId}` van de aanvraag voor het opschonen van geschiedenis van één exemplaar. Als u wilt leegmaken selectief exemplaar geschiedenis, gebruiken dezelfde filters die worden beschreven in de aanvraag 'Get status van alle instanties'.

#### <a name="request"></a>Aanvraag

Voor versie 1.x van de Functions-runtime, de aanvraag is opgemaakt als volgt te werk (meerdere regels worden weergegeven voor de duidelijkheid):

```http
DELETE /admin/extensions/DurableTaskExtension/instances
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
```

In versie 2.x van de Functions-runtime, de URL-indeling heeft dezelfde parameters, maar met een iets ander voorvoegsel:

```http
DELETE /runtime/webhooks/durabletask/instances
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
```

Aanvraag-parameters voor deze API bevatten de standaardset die eerder is vermeld en de volgende unieke parameters:

| Veld                 | Parametertype  | Description |
|-----------------------|-----------------|-------------|
| **`createdTimeFrom`** | Querytekenreeks    | Een optionele parameter. Als u opgeeft, filtert de lijst opgeschoonde exemplaren die zijn gemaakt op of na de opgegeven ISO8601-timestamp.|
| **`createdTimeTo`**   | Querytekenreeks    | Een optionele parameter. Als u opgeeft, filtert de lijst opgeschoonde exemplaren die zijn gemaakt op of voor de opgegeven ISO8601-timestamp.|
| **`runtimeStatus`**   | Querytekenreeks    | Een optionele parameter. Als u opgeeft, filters de lijst met opgeschoonde exemplaren op basis van hun runtimestatus. Zie de lijst met waarden voor mogelijke runtime status, de [uitvoeren van query's exemplaren](durable-functions-instance-management.md) onderwerp. |

Als er geen parameters zijn opgegeven, worden alle exemplaren in de hub taak worden opgeschoond.

> [!NOTE]
> Met deze bewerking zeer kostbaar in termen van Azure-opslag i/o kan zijn als er veel van de rijen in de instanties en/of de geschiedenis van tabellen. Meer informatie over deze tabellen kunnen worden gevonden in de [prestaties en schaalbaarheid in duurzame functies (Azure Functions)](durable-functions-perf-and-scale.md#instances-table) documentatie.

#### <a name="response"></a>Antwoord

De volgende HTTP-status codewaarden kunnen worden geretourneerd.

* **HTTP 200 (OK)**: De exemplaar-geschiedenis is is leeggemaakt.
* **HTTP 404 (Not Found)**: Er zijn geen exemplaren gevonden die overeenkomen met het filterexpressie.

De nettolading van de reactie voor de **HTTP 200** geval wordt een JSON-object met het volgende veld:

| Veld                   | Gegevenstype | Description |
|-------------------------|-----------|-------------|
| **`instancesDeleted`**  | geheel getal   | Het aantal exemplaren verwijderd. |

Hier volgt een voorbeeld van payload van he antwoord (indeling voor de leesbaarheid):

```json
{
    "instancesDeleted": 250
}
```

### <a name="raise-event"></a>Gebeurtenis

Verzendt een melding van de gebeurtenis naar een actief exemplaar van de indeling.

#### <a name="request"></a>Aanvraag

Voor versie 1.x van de Functions-runtime, de aanvraag is opgemaakt als volgt te werk (meerdere regels worden weergegeven voor de duidelijkheid):

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
```

In versie 2.x van de Functions-runtime, de URL-indeling heeft dezelfde parameters, maar met een iets ander voorvoegsel:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/raiseEvent/{eventName}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
```

Aanvraag-parameters voor deze API bevatten de standaardset die eerder is vermeld en de volgende unieke parameters:

| Veld             | Parametertype  | Description |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL             | De ID van de orchestration-exemplaar. |
| **`eventName`**   | URL             | De naam van de gebeurtenis die de doelinstantie orchestration wacht op. |
| **`{content}`**   | De inhoud van aanvraag | De JSON-indeling nettolading. |

#### <a name="response"></a>Antwoord

Verschillende mogelijke status codewaarden kunnen worden geretourneerd.

* **HTTP 202 (aanvaard)**: De gegeven gebeurtenis is geaccepteerd voor verwerking.
* **HTTP 400 (foute aanvraag)**: Inhoud van de aanvraag is niet van het type `application/json` of is geen geldige JSON.
* **HTTP 404 (Not Found)**: Het opgegeven exemplaar is niet gevonden.
* **HTTP-fout 410 (voltooid)**: Het opgegeven exemplaar is voltooid of mislukt en kan de verhoogde gebeurtenissen niet verwerken.

Hier volgt een voorbeeldaanvraag die de JSON-tekenreeks verzendt `"incr"` naar een exemplaar wachten op een gebeurtenis met de naam **bewerking**:

```http
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code=XXX
Content-Type: application/json
Content-Length: 6

"incr"
```

De antwoorden voor deze API bevatten niet alle inhoud.

### <a name="terminate-instance"></a>Sessie beëindigen

Een actief exemplaar van de orchestration beëindigt.

#### <a name="request"></a>Aanvraag

Voor versie 1.x van de Functions-runtime, de aanvraag is opgemaakt als volgt te werk (meerdere regels worden weergegeven voor de duidelijkheid):

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/terminate
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

In versie 2.x van de Functions-runtime, de URL-indeling heeft dezelfde parameters, maar met een iets ander voorvoegsel:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/terminate
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

Vragen om parameters voor deze API de standaardset die eerder is vermeld en de volgende unieke parameter bevatten.

| Veld             | Parametertype  | Description |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL             | De ID van de orchestration-exemplaar. |
| **`reason`**      | Querytekenreeks    | Optioneel. De reden voor de orchestration-sessie wordt beëindigd. |

#### <a name="response"></a>Antwoord

Verschillende mogelijke status codewaarden kunnen worden geretourneerd.

* **HTTP 202 (aanvaard)**: De aanvraag beëindigen is geaccepteerd voor verwerking.
* **HTTP 404 (Not Found)**: Het opgegeven exemplaar is niet gevonden.
* **HTTP-fout 410 (voltooid)**: Het opgegeven exemplaar is voltooid of mislukt.

Hier volgt een voorbeeldaanvraag die een actief exemplaar wordt beëindigd en Hiermee geeft u een reden van de **buggy**:

```
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason=buggy&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

De antwoorden voor deze API bevatten niet alle inhoud.

## <a name="rewind-instance-preview"></a>Terugspoelen exemplaar (preview)

Herstelt een mislukte orchestration-instantie in een status running doorbrengt door af te spelen de meest recente mislukte bewerkingen.

### <a name="request"></a>Aanvraag

Voor versie 1.x van de Functions-runtime, de aanvraag is opgemaakt als volgt te werk (meerdere regels worden weergegeven voor de duidelijkheid):

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/rewind
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

In versie 2.x van de Functions-runtime, de URL-indeling heeft dezelfde parameters, maar met een iets ander voorvoegsel:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/rewind
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

Vragen om parameters voor deze API de standaardset die eerder is vermeld en de volgende unieke parameter bevatten.

| Veld             | Parametertype  | Description |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL             | De ID van de orchestration-exemplaar. |
| **`reason`**      | Querytekenreeks    | Optioneel. De reden voor de orchestration-instantie terugspoelen. |

### <a name="response"></a>Antwoord

Verschillende mogelijke status codewaarden kunnen worden geretourneerd.

* **HTTP 202 (aanvaard)**: De terugspoelen-aanvraag is geaccepteerd voor verwerking.
* **HTTP 404 (Not Found)**: Het opgegeven exemplaar is niet gevonden.
* **HTTP-fout 410 (voltooid)**: Het opgegeven exemplaar is voltooid of is beëindigd.

Hier volgt een voorbeeldaanvraag die een mislukt exemplaar teruggespoeld en Hiermee geeft u een reden van de **vaste**:

```http
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/rewind?reason=fixed&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

De antwoorden voor deze API bevatten niet alle inhoud.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het afhandelen van fouten](durable-functions-error-handling.md)
