---
title: HTTP-API's in duurzame functies - Azure
description: Informatie over het implementeren van HTTP APIs in de uitbreiding duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: bb5361022e4c9693812753ae33df5aeb037b5aaa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="http-apis-in-durable-functions-azure-functions"></a>HTTP-API's in duurzame functies (Azure-functies)

De extensie duurzame taak beschrijft een reeks HTTP APIs die kunnen worden gebruikt voor de volgende taken uitvoeren:

* De status van een exemplaar van de orchestration ophalen.
* Een gebeurtenis verzenden naar een exemplaar van de orchestration wachten.
* Een actief exemplaar van de orchestration worden beëindigd.

Elk van deze APIs HTTP zijn webhook-bewerkingen die rechtstreeks door de uitbreiding voor duurzame taak worden verwerkt. Ze zijn niet specifiek zijn voor een functie in de functie-app.

> [!NOTE]
> Deze bewerkingen kunnen ook worden aangeroepen rechtstreeks met de exemplaar-management API's op de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse. Zie voor meer informatie [Instantiebeheer](durable-functions-instance-management.md).

## <a name="http-api-url-discovery"></a>HTTP-URL van de API-detectie

De [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse zichtbaar gemaakt een [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_) API die kan worden gebruikt voor het genereren van een HTTP-antwoord nettolading met koppelingen naar alle ondersteunde bewerkingen. Hier volgt een voorbeeld van HTTP-trigger-functie die laat zien hoe u deze API:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/HttpStart/run.csx)]

Deze functie bijvoorbeeld produceert de volgende JSON-antwoordgegevens. Het gegevenstype van alle velden `string`.

| Veld             |Beschrijving                           |
|-------------------|--------------------------------------|
| id                |De ID van de orchestration-exemplaar. |
| statusQueryGetUri |De URL van de status van de orchestration-exemplaar. |
| sendEventPostUri  |De URL 'gebeurtenis activeren' van de orchestration-exemplaar. |
| terminatePostUri  |De URL 'is beëindigd' van de orchestration-exemplaar. |

Hier volgt een voorbeeld van antwoord:

```http
HTTP/1.1 202 Accepted
Content-Length: 923
Content-Type: application/json; charset=utf-8
Location: https://{host}/webhookextensions/handler/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX

{
    "id":"34ce9a28a6834d8492ce6a295f1a80e2",
    "statusQueryGetUri":"https://{host}/webhookextensions/handler/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "sendEventPostUri":"https://{host}/webhookextensions/handler/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "terminatePostUri":"https://{host}/webhookextensions/handler/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX"
}
```
> [!NOTE]
> De indeling van de webhook-URL's kan verschillen, afhankelijk van welke versie van de Azure Functions-host die u uitvoert. Het bovenstaande voorbeeld is voor de Azure Functions 2.0-host.

## <a name="async-operation-tracking"></a>Asynchrone bewerking bijhouden

Het HTTP-antwoord dat eerder is ontworpen om u te helpen langlopende HTTP async API's met duurzame functies implementeren. Dit wordt soms aangeduid als de *gebruikspatroon Polling*. De client/server-stroom werkt als volgt:

1. De client verzendt een HTTP-aanvraag om een langdurige proces, zoals een orchestrator-functie te starten.
2. De doel-HTTP-trigger retourneert een 202 HTTP-antwoord met een `Location` header met de `statusQueryGetUri` waarde.
3. De client controleert of de URL in de `Location` header. Zie 202 HTTP-antwoorden met gaat door een `Location` header.
4. Wanneer het exemplaar is voltooid (of mislukt), het eindpunt in de `Location` header retourneert HTTP 200.

Met dit protocol kunnen coördineren langlopende processen met externe clients of services die ondersteuning bieden voor polling van een HTTP-eindpunt Volg de `Location` header. De fundamentele onderdelen zijn al ingebouwd in duurzame functies HTTP API's.

> [!NOTE]
> Standaard worden alle acties die HTTP-gebaseerde geleverd door [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) ondersteunen het patroon standaard asynchrone bewerking. Dit maakt het mogelijk voor het insluiten van een duurzame functie langlopende als onderdeel van een werkstroom Logic Apps. Meer informatie over Logic Apps ondersteuning voor asynchrone HTTP patronen vindt u in de [Azure Logic Apps werkstroom acties en triggers documentatie](../logic-apps/logic-apps-workflow-actions-triggers.md#asynchronous-patterns).

## <a name="http-api-reference"></a>HTTP-API-referentiemateriaal

Alle HTTP APIs geïmplementeerd door de uitbreiding nemen de volgende parameters. Het gegevenstype van alle parameters `string`.

| Parameter  | Parametertype  | Beschrijving |
|------------|-----------------|-------------|
| Exemplaar-id | URL             | De ID van de orchestration-exemplaar. |
| taskHub    | Queryreeks    | De naam van de [taak hub](durable-functions-task-hubs.md). Als niet wordt opgegeven, wordt de taaknaam hub van de huidige functie-app gebruikt. |
| verbinding | Queryreeks    | De **naam** van de verbindingsreeks voor de storage-account. Als niet wordt opgegeven, wordt de standaard-verbindingsreeks voor de functie-app gebruikt. |
| systemKey  | Queryreeks    | De autorisatiesleutel is vereist voor het aanroepen van de API. |

`systemKey`een autorisatiesleutel is automatisch gegenereerd door de Azure Functions-host. Deze specifiek verleent toegang tot de extensie duurzame taak API's en kunnen worden beheerd dezelfde manier als [andere sleutels autorisatie](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Key-management-API). De eenvoudigste manier om te detecteren de `systemKey` waarde is met behulp van de `CreateCheckStatusResponse` API eerder is vermeld.

De volgende secties hebben betrekking op de specifieke HTTP APIs ondersteund door de uitbreiding en vindt u voorbeelden van hoe ze kunnen worden gebruikt.

### <a name="get-instance-status"></a>Exemplaar status ophalen

Hiermee haalt u de status van een opgegeven orchestration-exemplaar.

#### <a name="request"></a>Aanvraag

Voor functies 1.0 is indeling van de aanvraag als volgt uit:

```http
GET /admin/extensions/DurableTaskExtension/instances/{instanceId}?taskHub={taskHub}&connection={connection}&code={systemKey}
```

De functies 2.0-indeling heeft dezelfde parameters, maar heeft een iets andere URL-voorvoegsel:

```http
GET /webhookextensions/handler/DurableTaskExtension/instances/{instanceId}?taskHub={taskHub}&connection={connection}&code={systemKey}
```

#### <a name="response"></a>Antwoord

Verschillende mogelijke status code-waarden kunnen worden geretourneerd.

* **HTTP 200 (OK)**: het opgegeven exemplaar is een voltooide status.
* **HTTP 202 (geaccepteerde)**: het opgegeven exemplaar is uitgevoerd.
* **HTTP-fout 400 (ongeldige aanvraag)**: het opgegeven exemplaar is mislukt of is beëindigd.
* **HTTP 404 (niet gevonden)**: het opgegeven exemplaar bestaat niet of is niet gestart.

De nettolading van de reactie voor de **HTTP 200** en **HTTP 202** gevallen is een JSON-object met de volgende velden.

| Veld           | Gegevenstype | Beschrijving |
|-----------------|-----------|-------------|
| runtimeStatus   | Tekenreeks    | De runtimestatus van het exemplaar. Mogelijke waarden zijn *met*, *in behandeling*, *mislukt*, *geannuleerd*, *beëindigd*, *Voltooid*. |
| Invoer           | JSON      | De JSON-gegevens die wordt gebruikt voor het initialiseren van het exemplaar. |
| Uitvoer          | JSON      | De JSON-uitvoer van het exemplaar. Dit veld is `null` als het exemplaar niet in een voltooide status is. |
| createdTime     | Tekenreeks    | De tijd waarop het exemplaar is gemaakt. Maakt gebruik van ISO 8601-notatie wordt uitgebreid. |
| LastUpdatedTime | Tekenreeks    | De tijd waarop het exemplaar is opgeslagen. Maakt gebruik van ISO 8601-notatie wordt uitgebreid. |

Hier volgt een voorbeeld antwoord nettolading (indeling voor de leesbaarheid):

```json
{
  "runtimeStatus": "Completed",
  "input": null,
  "output": [
    "Hello Tokyo!",
    "Hello Seattle!",
    "Hello London!"
  ],
  "createdTime": "2017-10-06T18:30:24Z",
  "lastUpdatedTime": "2017-10-06T18:30:30Z"
}
```

De **HTTP 202** antwoord bevat ook een **locatie** antwoordheader die verwijst naar dezelfde URL als voor de `statusQueryGetUri` veld eerder is vermeld.

### <a name="raise-event"></a>Genereren van gebeurtenis

Verzendt een Meldingsbericht naar een actief exemplaar van de orchestration.

#### <a name="request"></a>Aanvraag

Voor functies 1.0 is indeling van de aanvraag als volgt uit:

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection={connection}&code={systemKey}
```

De functies 2.0-indeling heeft dezelfde parameters, maar heeft een iets andere URL-voorvoegsel:

```http
POST /webhookextensions/handler/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection={connection}&code={systemKey}
```

Aanvraag parameters voor deze API bevatten de standaardset die eerder is vermeld en de volgende unieke parameters.

| Veld       | Parametertype  | Gegevens tType | Beschrijving |
|-------------|-----------------|-----------|-------------|
| EventName   | URL             | Tekenreeks    | De naam van de gebeurtenis die het doelexemplaar orchestration wacht op. |
| {inhoud}   | Inhoud opvragen | JSON      | De JSON-indeling nettolading. |

#### <a name="response"></a>Antwoord

Verschillende mogelijke status code-waarden kunnen worden geretourneerd.

* **HTTP 202 (geaccepteerde)**: de gebeurtenis verhoogd is geaccepteerd voor verwerking.
* **HTTP-fout 400 (ongeldige aanvraag)**: inhoud van de aanvraag is niet van het type `application/json` of is geen geldige JSON.
* **HTTP 404 (niet gevonden)**: het opgegeven exemplaar is niet gevonden.
* **HTTP 410 (Gone)**: het opgegeven exemplaar is voltooid of mislukt en de verhoogde gebeurtenissen kan verwerken.

Hier volgt een voorbeeld van de aanvraag die worden verzonden in de JSON-tekenreeks `"incr"` naar een exemplaar wachten op een gebeurtenis met de naam **bewerking** (die afkomstig zijn uit de [teller](durable-functions-counter.md) voorbeeld):

```
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code=XXX
Content-Type: application/json
Content-Length: 6

"incr"
```

De antwoorden voor deze API bevatten niet alle inhoud.

### <a name="terminate-instance"></a>Sessie is beëindigd

Een actief exemplaar van de orchestration beëindigt.

#### <a name="request"></a>Aanvraag

Voor functies 1.0 is indeling van de aanvraag als volgt uit:

```http
DELETE /admin/extensions/DurableTaskExtension/instances/{instanceId}/terminate?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

De functies 2.0-indeling heeft dezelfde parameters, maar heeft een iets andere URL-voorvoegsel:

```http
DELETE /webhookextensions/handler/DurableTaskExtension/instances/{instanceId}/terminate?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

Aanvraag parameters voor deze API bevatten de standaardset die eerder is vermeld en de volgende unieke parameter.

| Veld       | Parametertype  | Gegevenstype | Beschrijving |
|-------------|-----------------|-----------|-------------|
| Reden      | Queryreeks    | Tekenreeks    | Optioneel. De reden voor de orchestration-exemplaar wordt beëindigd. |

#### <a name="response"></a>Antwoord

Verschillende mogelijke status code-waarden kunnen worden geretourneerd.

* **HTTP 202 (geaccepteerde)**: de terminate-aanvraag is geaccepteerd voor verwerking.
* **HTTP 404 (niet gevonden)**: het opgegeven exemplaar is niet gevonden.
* **HTTP 410 (Gone)**: het opgegeven exemplaar is voltooid of mislukt.

Hier volgt een voorbeeld van de aanvraag die beëindigt een actief exemplaar en de reden van een opgegeven **buggy**:

```
DELETE /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason=buggy&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

De antwoorden voor deze API bevatten niet alle inhoud.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het afhandelen van fouten](durable-functions-error-handling.md)
