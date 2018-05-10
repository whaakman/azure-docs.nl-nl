---
title: Werken in duurzame functies - Azure-koppeling
description: Informatie over het uitvoeren van een steekproef duurzame functies waarmee u een reeks functies uitvoert.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: e53b38bf336816ca670fad3ab70a43e5cc8b3437
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="function-chaining-in-durable-functions---hello-sequence-sample"></a>Functie chaining in duurzame functies - Hallo sequence-voorbeeld

Functie chaining verwijst naar het patroon van een reeks functies uitvoeren in een bepaalde volgorde. Vaak moet de uitvoer van een functie worden toegepast op de invoer van een andere functie. Dit artikel wordt uitgelegd voor een voorbeeldtoepassing die gebruikmaakt van [duurzame functies](durable-functions-overview.md) voor het implementeren van functie-koppeling.

## <a name="prerequisites"></a>Vereisten

* [Installeren van functies duurzame](durable-functions-install.md).

## <a name="the-functions"></a>De functies

Dit artikel wordt uitgelegd dat de volgende functies in de voorbeeld-app:

* `E1_HelloSequence`: Er is een orchestrator-functie die aanroept `E1_SayHello` meerdere keren in een reeks. Deze slaat de uitvoer van de `E1_SayHello` aangeroepen en registreert de resultaten.
* `E1_SayHello`: Er is een functie van het type activiteit die voegt toe aan elke tekenreeks met "Hallo".

De volgende secties worden de configuratie en code die worden gebruikt voor C# scripting- en JavaScript. De code voor het ontwikkelen van Visual Studio wordt weergegeven aan het einde van het artikel.

> [!NOTE]
> Duurzame functies is beschikbaar in JavaScript in de v2 runtime van Functions alleen.

## <a name="e1hellosequence"></a>E1_HelloSequence
### <a name="functionjson-file"></a>Function.JSON bestand

Als u Visual Studio Code of de Azure-portal voor ontwikkeling gebruikt, hier is de inhoud van de *function.json* -bestand voor de orchestrator-functie. De meeste orchestrator *function.json* bestanden moeten uitzien bijna hetzelfde.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/function.json)]

Het belangrijkste is dat de `orchestrationTrigger` bindingstype. Alle orchestrator-functies moeten dit triggertype te gebruiken.

> [!WARNING]
> Om door te gaan met de regel 'geen i/o-' van orchestrator-functies, niet gebruikmaken van een invoer of uitvoer bindingen bij gebruik van de `orchestrationTrigger` binding activeren.  Als andere invoer of uitvoer bindingen nodig zijn, ze in plaats daarvan moeten worden gebruikt in de context van `activityTrigger` functies die worden aangeroepen door de orchestrator.

### <a name="c-script-visual-studio-code-and-azure-portal-sample-code"></a>C# script (Visual Studio Code en de Azure portal voorbeeldcode) 

Dit is de broncode:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/run.csx)]

Alle C# orchestration functies moeten een parameter van het type hebben `DurableOrchestrationContext`, die bestaat in de `Microsoft.Azure.WebJobs.Extensions.DurableTask` assembly. Als u C# script, de assembly kan worden verwezen met de `#r` notatie. Dit contextobject kunt u andere aanroepen *activiteit* functies en invoerparameters op te geven met behulp van de [CallActivityAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_) methode.

Het aanroepen van de code `E1_SayHello` drie keer in de reeks met andere parameterwaarden. De geretourneerde waarde van elke aanroep wordt toegevoegd aan de `outputs` lijst, die aan het einde van de functie wordt geretourneerd.

### <a name="javascript"></a>Javascript

Dit is de broncode:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

Alle JavaScript-functies voor orchestration moeten bevatten de `durable-functions` module. Dit is een JavaScript-bibliotheek van de orchestration-functie worden handelingen omgezet in van een duurzame uitvoering protocol voor out-of-proc talen. Er zijn drie belangrijke verschillen tussen een orchestration-functie en andere JavaScript-functies:

1. De functie is een [functie voor het genereren.](https://docs.microsoft.com/en-us/scripting/javascript/advanced/iterators-and-generators-javascript)
2. De functie wordt verpakt in een aanroep naar de `durable-functions` module (hier `df`).
3. De functie wordt beëindigd door het aanroepen van `return`, niet `context.done`.

De `context` object bevat een `df` object kunt u andere aanroepen *activiteit* functies en invoerparameters op te geven met behulp van de `callActivityAsync` methode. Het aanroepen van de code `E1_SayHello` drie keer in de reeks met verschillende parameterwaarden met `yield` om aan te geven tijdens de uitvoering moet wachten op de functieaanroepen async-activiteit moet worden geretourneerd. De geretourneerde waarde van elke aanroep wordt toegevoegd aan de `outputs` lijst, die aan het einde van de functie wordt geretourneerd.

## <a name="e1sayhello"></a>E1_SayHello
### <a name="functionjson-file"></a>Function.JSON bestand

De *function.json* -bestand voor de functie activiteit `E1_SayHello` is vergelijkbaar met die van `E1_HelloSequence` , behalve dat gebruikmaakt van een `activityTrigger` bindingstype in plaats van een `orchestrationTrigger` bindingstype.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_SayHello/function.json)]

> [!NOTE]
> Een functie die wordt aangeroepen door een functie orchestration moet gebruiken de `activityTrigger` binding.

De implementatie van `E1_SayHello` is een relatief trivial tekenreeks bewerking opmaak.

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_SayHello/run.csx)]

Deze functie heeft een parameter van het type [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html), die wordt gebruikt om op te halen van de ingang die is doorgegeven door de aanroep van de orchestrator-functie [ `CallActivityAsync<T>` ](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_).

### <a name="javascript"></a>Javascript

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

In tegenstelling tot een JavaScript orchestration-functie moet een JavaScript-functie activiteit geen speciale instellingen. De invoer doorgegeven door de orchestrator-functie bevindt zich op de `context.bindings` object onder de naam van de `activitytrigger` binding - in dit geval `context.bindings.name`. De naam van de binding kan worden ingesteld als een parameter van de geëxporteerde functie en toegankelijk rechtstreeks, dit is wat de voorbeeldcode doet.

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Uit te voeren de `E1_HelloSequence` orchestration, de volgende HTTP POST-aanvraag verzenden.

```
POST http://{host}/orchestrators/E1_HelloSequence
```

Bijvoorbeeld, als u het voorbeeld uitvoert in een functie-app met de naam 'myfunctionapp', vervangen door '{host}' 'myfunctionapp.azurewebsites.net'.

Het resultaat is een antwoord HTTP 202 als volgt (afgekapte als beknopt alternatief bevat):

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Op dit moment de orchestration is in de wachtrij geplaatst en begint onmiddellijk uitvoeren. De URL in de `Location` header kan worden gebruikt om de status van de uitvoering te controleren.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

Het resultaat is de status van de orchestration. Deze wordt uitgevoerd en voltooit snel, zodat deze weergegeven in de *voltooid* status aan een antwoord dat op dit lijkt (afgekapte als beknopt alternatief bevat):

```
HTTP/1.1 200 OK
Content-Length: 179
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":null,"output":["Hello Tokyo!","Hello Seattle!","Hello London!"],"createdTime":"2017-06-29T05:24:57Z","lastUpdatedTime":"2017-06-29T05:24:59Z"}
```

Zoals u ziet de `runtimeStatus` van het exemplaar is *voltooid* en de `output` bevat het JSON-geserialiseerd resultaat van de uitvoering van de orchestrator-functie.

> [!NOTE]
> Het HTTP POST-eindpunt dat de orchestrator-functie gestart is geïmplementeerd in de voorbeeld-app als een HTTP-functie met de naam 'HttpStart' activeren. U kunt vergelijkbare starter logica voor andere triggertypen zoals implementeren `queueTrigger`, `eventHubTrigger`, of `timerTrigger`.

Bekijk de logboeken van de functie kan worden uitgevoerd. De `E1_HelloSequence` functie gestart en meerdere keren voltooid wegens het replay-gedrag dat wordt beschreven in de [overzicht](durable-functions-overview.md). Er zijn aan de andere kant slechts drie uitvoeringen van `E1_SayHello` omdat die functies wordt weergegeven, komen niet ophalen cookies.

## <a name="visual-studio-sample-code"></a>Voorbeeldcode voor Visual Studio

Dit is de orchestration als één C#-bestand in Visual Studio-project:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

## <a name="next-steps"></a>Volgende stappen

Dit voorbeeld is een eenvoudige functie chaining orchestration gedemonstreerd. Het volgende voorbeeld laat zien hoe het fan-uitgaand/fan-in patroon implementeren. 

> [!div class="nextstepaction"]
> [De Fan-uitgaand/fan-in voorbeeld uitvoeren](durable-functions-cloud-backup.md)
