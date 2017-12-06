---
title: Stateful singletons in duurzame functies - Azure
description: Informatie over het implementeren van een stateful singleton in de uitbreiding duurzame functies voor Azure Functions.
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
ms.openlocfilehash: 05099e868e62f612be0a3354eb8b339507ac7e4a
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="stateful-singletons-in-durable-functions---counter-sample"></a>Stateful singletons in duurzame functies - teller voorbeeld

Stateful singletons zijn langlopende (mogelijk eeuwige) orchestrator-functies die kunnen worden opgeslagen status en worden aangeroepen en opgevraagd met andere functies. Stateful singletons zijn vergelijkbaar met de [Actor model](https://en.wikipedia.org/wiki/Actor_model) in gedistribueerde computeromgevingen.

Tijdens niet een juiste 'actor'-implementatie hebben orchestrator-functies veel van dezelfde runtime-kenmerken. Ze zijn bijvoorbeeld stateful, betrouwbare, één thread, locatie-transparante en globaal adresseerbare. Dit zijn de kenmerken die vooral handig maar zonder de noodzaak van een afzonderlijke framework echte actor-implementaties maken.

Dit artikel laat zien dat het uitvoeren van de *teller* voorbeeld. Het voorbeeld laat zien dat een singleton-object dat ondersteunt *verhoging* en *verlagen* operations en de interne status dienovereenkomstig bijgewerkt.

## <a name="prerequisites"></a>Vereisten

* Volg de instructies in [duurzame functies installeren](durable-functions-install.md) voor het instellen van het voorbeeld.
* In dit artikel wordt ervan uitgegaan dat u al hebt doorlopen de [Hello Sequence](durable-functions-sequence.md) voorbeeld scenario.

## <a name="scenario-overview"></a>Overzicht van scenario's

Het prestatiemeteritem-scenario is verrassend moeilijk te implementeren met de reguliere staatloze functies. Een van de belangrijkste uitdagingen die u hebt het beheren van **gelijktijdigheid**. Bewerkingen, zoals *verhoging* en *verlagen* moet atomaire of anders kan er racetoestanden die ertoe leiden dat de bewerkingen die elkaar overschrijven.

Een optie om het gebruik van één VM voor het hosten van de gegevens van prestatiemeteritems is, maar dit is kostbaar en beheren van **betrouwbaarheid** een uitdaging kan zijn omdat één VM periodiek kan worden opgestart. U kunt ook een gedistribueerde platform met synchronisatie hulpmiddelen zoals blob-leases voor het beheer van gelijktijdigheid van taken, maar dit introduceert een grote hoeveelheid **complexiteit**.

Duurzame functies kunt u dit soort scenario trivial te implementeren, omdat de orchestration-exemplaren zijn wachtrijen op een enkele virtuele machine en de uitvoering van de orchestrator-functie is altijd één thread. Niet alleen die, maar ze zijn langlopende en stateful, en op externe gebeurtenissen kunnen reageren. De volgende voorbeeldcode laat zien hoe teller als een orchestrator-functie langlopende implementeren.

## <a name="the-sample-function"></a>De functie sample

Dit artikel begeleidt u bij de **E3_Counter** functie in de voorbeeld-app.



## <a name="the-counter-orchestration"></a>De teller orchestration

De volgende secties worden de code die wordt gebruikt voor het ontwikkelen van Visual Studio Code en Azure-Portal.

### <a name="c-script"></a>C#-Script

Het bestand function.json:

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_Counter/function.json)]

Het bestand run.csx:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_Counter/run.csx)]

### <a name="precompiled-c"></a>Vooraf gecompileerde C# 

De volgende secties worden de code die wordt gebruikt voor het ontwikkelen van Visual Studio.

Dit is de code die de orchestrator-functie implementeert:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Counter.cs)]

### <a name="explanation-of-the-code"></a>Uitleg van de code

Deze functie voor orchestrator in wezen doet het volgende:

1. Luistert naar een externe gebeurtenis met de naam *bewerking* met [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_).
2. Wordt verhoogd of verlaagd de `counterState` lokale variabele, afhankelijk van de aangevraagde bewerking.
3. Opnieuw is opgestart de orchestrator met behulp van de [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) methode, de meest recente waarde van `counterState` als de nieuwe invoer.
4. Wordt verder uitgevoerd, permanent verloren of totdat een *end* bericht wordt ontvangen.

Dit is een voorbeeld van een *eeuwige orchestration* &mdash; dat wil zeggen een mogelijk nooit wordt beëindigd. Reageren op berichten die zijn verzonden door de [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) methode, die kan worden aangeroepen door een niet-orchestrator-functie.

Een uniek kenmerk van deze functie orchestrator is dat de effectief geen geschiedenis: de `ContinueAsNew` methode stelt u de geschiedenis na elke verwerkte gebeurtenis. Dit is de beste manier voor het implementeren van een orchestrator waarvoor een willekeurige levensduur. Met behulp van een `while` lus kan ervoor zorgen dat de orchestrator-functie Geschiedenis groeien unbounded, wat resulteert in onnodig hoog geheugengebruik.

> [!NOTE]
> De `ContinueAsNew` -methode heeft een andere gebruiksvoorbeeld naast eeuwige integraties. Zie voor meer informatie [eeuwige integraties](durable-functions-eternal-orchestrations.md).

## <a name="run-the-sample"></a>Het voorbeeld uitvoert

U kunt de orchestration starten door de volgende HTTP POST-aanvraag te verzenden. Om toe te staan `counterState` om te beginnen bij nul (de standaardwaarde voor `int`), er is geen inhoud in deze aanvraag.

```
POST http://{host}/orchestrators/E3_Counter
Content-Length: 0
```

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{
  "id":"bcf6fb5067b046fbb021b52ba7deae5a",
  "statusQueryGetUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}",
  "sendEventPostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}",
  "terminatePostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}"}
```

De **E3_Counter** exemplaar wordt gestart en vervolgens onmiddellijk wordt gewacht op een gebeurtenis moet worden verzonden met de `RaiseEventAsync` of met behulp van de **sendEventUrl** HTTP POST-webhook waarnaar wordt verwezen in de 202 reactie. Geldige `eventName` waarden zijn *incr*, *afname in voorraden*, en *end*.

```
POST http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
Content-Type: application/json
Content-Length: 6

"incr"
```

U ziet de resultaten van de 'incr'-bewerking op de functie Logboeken in de Azure Functions-portal.

```
2017-06-29T18:54:53.998 Function started (Id=34e34a61-38b3-4eac-b6e2-98b85e32eec8)
2017-06-29T18:54:53.998 Current counter state is 0. Waiting for next operation.
2017-06-29T18:58:01.458 Function started (Id=b45d6c2f-39f3-42a2-b904-7761b2614232)
2017-06-29T18:58:01.458 Current counter state is 0. Waiting for next operation.
2017-06-29T18:58:01.458 Received 'incr' operation.
2017-06-29T18:58:01.458 Function completed (Success, Id=b45d6c2f-39f3-42a2-b904-7761b2614232, Duration=8ms)
2017-06-29T18:58:11.518 Function started (Id=e1f38cb2-546a-404d-ab22-1ac8f81a93d9)
2017-06-29T18:58:11.518 Current counter state is 1. Waiting for next operation.
```

Op dezelfde manier als u de orchestrator-status controleren, ziet u de `input` veld is ingesteld op de bijgewerkte waarde (1).

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey} HTTP/1.1
```

```
HTTP/1.1 202 Accepted
Content-Length: 129
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"runtimeStatus":"Running","input":1,"output":null,"createdTime":"2017-06-29T18:58:01Z","lastUpdatedTime":"2017-06-29T18:58:11Z"}
```

U kunt doorgaan met het verzenden van nieuwe bewerkingen voor dit exemplaar en zien dat de status dienovereenkomstig bijgewerkt. Als u afsluiten van het exemplaar wilt, u kunt dit doen door te sturen een *end* bewerking.

> [!WARNING]
> Op het moment van schrijven, er zijn bekende racetoestanden bij het aanroepen van `ContinueAsNew` tijdens het verwerken van berichten, zoals externe gebeurtenissen of beëindiging aanvragen gelijktijdig. Zie voor de meest recente informatie op deze racetoestanden [GitHub probleem](https://github.com/Azure/azure-functions-durable-extension/issues/67).

## <a name="next-steps"></a>Volgende stappen

Dit voorbeeld heeft aangetoond hoe moet worden verwerkt [externe gebeurtenissen](durable-functions-external-events.md) en implementeren van [eeuwige integraties](durable-functions-eternal-orchestrations.md) in [stateful singletons](durable-functions-singletons.md). Het volgende voorbeeld laat zien hoe u externe gebeurtenissen en [duurzame timers](durable-functions-timers.md) voor het afhandelen van menselijke tussenkomst.

> [!div class="nextstepaction"]
> [De menselijke tussenkomst-voorbeeld uitvoeren](durable-functions-phone-verification.md)
