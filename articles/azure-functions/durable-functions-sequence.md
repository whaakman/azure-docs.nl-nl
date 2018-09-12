---
title: Werken in duurzame functies - Azure-koppeling
description: Leer hoe u een voorbeeld van duurzame functies die wordt uitgevoerd een reeks functies uitvoeren.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: azfuncdf
ms.openlocfilehash: c84977dacddcf9ccca7fde735ad4acb8a1523fa9
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44378700"
---
# <a name="function-chaining-in-durable-functions---hello-sequence-sample"></a>Werken in duurzame functies - Hallo reeks voorbeeld-koppeling

Koppeling van de functie verwijst naar het patroon van het uitvoeren van een reeks functies in een bepaalde volgorde. De uitvoer van een functie moet vaak worden toegepast op de invoer van een andere functie. Dit artikel wordt uitgelegd voor een voorbeeld dat gebruikmaakt van [duurzame functies](durable-functions-overview.md) voor het implementeren van functie-koppeling.

## <a name="prerequisites"></a>Vereisten

* [Duurzame functies installeren](durable-functions-install.md).

## <a name="the-functions"></a>De functies

Dit artikel wordt uitgelegd dat de volgende functies in de voorbeeld-app:

* `E1_HelloSequence`: Een orchestrator-functie die worden aangeroepen `E1_SayHello` meerdere keren in een reeks. Slaat de uitvoer van de `E1_SayHello` aanroept en registreert de resultaten.
* `E1_SayHello`: Een activiteit-functie die een tekenreeks zijn met "Hallo" voegt toe.

De volgende secties worden de configuratie en de code die worden gebruikt voor C#-scripts en JavaScript. De code voor het ontwikkelen van Visual Studio wordt weergegeven aan het einde van het artikel.

> [!NOTE]
> Duurzame functies is beschikbaar in JavaScript in de alleen v2 Functions-runtime.

## <a name="e1hellosequence"></a>E1_HelloSequence
### <a name="functionjson-file"></a>Function.JSON bestand

Als u Visual Studio Code of de Azure-portal voor ontwikkeling gebruiken, hier is de inhoud van de *function.json* -bestand voor de orchestrator-functie. De meeste orchestrator *function.json* bestanden er bijna hetzelfde als volgt uit.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/function.json)]

Het belangrijkste is dat de `orchestrationTrigger` type binding. Alle orchestrator-functies moeten dit triggertype gebruiken.

> [!WARNING]
> Als u wilt houden aan de regel 'geen i/o' van de orchestrator-functies, niet gebruikt voor elke invoer of uitvoerbindingen bij het gebruik van de `orchestrationTrigger` binding activeren.  Als andere invoer of uitvoerbindingen nodig zijn, ze in plaats daarvan moeten worden gebruikt in de context van `activityTrigger` functies, die worden aangeroepen door de orchestrator.

### <a name="c-script-visual-studio-code-and-azure-portal-sample-code"></a>C#-script (Visual Studio Code en Azure portal voorbeeldcode) 

Dit is de broncode:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/run.csx)]

Alle C# orchestration functies moet een parameter van het type `DurableOrchestrationContext`, waar zich bevindt de `Microsoft.Azure.WebJobs.Extensions.DurableTask` assembly. Als u C#-script, de assembly kan worden verwezen met behulp van de `#r` notatie. Dit contextobject kunt u andere aanroepen *activiteit* functies en pass invoerparameters die zijn opgegeven met behulp van de [CallActivityAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_) methode.

De code roept `E1_SayHello` drie keer in de reeks met andere parameterwaarden. De geretourneerde waarde van elke aanroep wordt toegevoegd aan de `outputs` lijst, die aan het einde van de functie wordt geretourneerd.

### <a name="javascript"></a>Javascript

Dit is de broncode:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

Alle JavaScript-functies voor orchestration moeten bevatten de `durable-functions` module. Dit is een JavaScript-bibliotheek die de orchestration-functie-acties in de duurzame uitvoering protocol voor out-of-proc talen vertaalt. Er zijn drie belangrijke verschillen tussen een orchestration-functie en andere JavaScript-functies:

1. De functie is een [generator-functie.](https://docs.microsoft.com/scripting/javascript/advanced/iterators-and-generators-javascript)
2. De functie is verpakt in een aanroep naar de `durable-functions` module (hier `df`).
3. De functie wordt beëindigd door het aanroepen van `return`, niet `context.done`.

De `context` -object bevat een `df` object kunt u andere aanroepen *activiteit* functies en pass invoerparameters die zijn opgegeven met behulp van de `callActivityAsync` methode. De code roept `E1_SayHello` drie keer in de reeks met andere parameterwaarden, met behulp van `yield` om aan te geven van de uitvoering moet wachten op de functieaanroepen die asynchrone activiteit moet worden geretourneerd. De geretourneerde waarde van elke aanroep wordt toegevoegd aan de `outputs` lijst, die aan het einde van de functie wordt geretourneerd.

## <a name="e1sayhello"></a>E1_SayHello
### <a name="functionjson-file"></a>Function.JSON bestand

De *function.json* -bestand voor de functie activiteit `E1_SayHello` is vergelijkbaar met die van `E1_HelloSequence` , behalve dat gebruikmaakt van een `activityTrigger` bindingstype in plaats van een `orchestrationTrigger` type binding.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_SayHello/function.json)]

> [!NOTE]
> Een functie die wordt aangeroepen door een orchestration-functie moet gebruiken de `activityTrigger` binding.

De implementatie van `E1_SayHello` is een relatief eenvoudig tekenreeks bewerking opmaak.

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_SayHello/run.csx)]

Deze functie heeft een parameter van het type [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html), die wordt gebruikt om op te halen van de invoer die is doorgegeven aan het door de orchestrator-functie-aanroep naar [ `CallActivityAsync<T>` ](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_).

### <a name="javascript"></a>Javascript

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

In tegenstelling tot een orchestration JavaScript-functie moet een JavaScript-functie voor activiteit geen speciale configuratie. De invoer die is doorgegeven door de orchestrator-functie bevindt zich op de `context.bindings` object onder de naam van de `activitytrigger` binding - in dit geval `context.bindings.name`. De naam van de binding kan worden ingesteld als een parameter van de geëxporteerde functie en rechtstreeks worden geopend dat is wat de voorbeeldcode doet.

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Om uit te voeren de `E1_HelloSequence` orchestration, verzenden de volgende HTTP POST-aanvraag.

```
POST http://{host}/orchestrators/E1_HelloSequence
```

> [!NOTE]
> Het vorige HTTP-fragment wordt ervan uitgegaan dat er is een item in de `host.json` -bestand dat Hiermee verwijdert u de standaard `api/` voorvoegsel van alle functies URL van de HTTP-trigger. U vindt de opmaak voor deze configuratie in de `host.json` bestand in de voorbeelden.

Bijvoorbeeld, als u het voorbeeld uitvoert in een functie-app met de naam "myfunctionapp", vervangen door '{host}' 'myfunctionapp.azurewebsites.net'.

Het resultaat is een HTTP-202-antwoord wordt als volgt (bijgesneden voor beknoptheid):

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Op dit moment de indeling is in de wachtrij geplaatst en begint onmiddellijk wilt uitvoeren. De URL in de `Location` header kan worden gebruikt om de status van de uitvoering te controleren.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

Het resultaat is de status van de indeling. Deze wordt uitgevoerd en is voltooid, ziet u in de *voltooid* staat met een antwoord er als volgt uitzien (bijgesneden voor beknoptheid):

```
HTTP/1.1 200 OK
Content-Length: 179
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":null,"output":["Hello Tokyo!","Hello Seattle!","Hello London!"],"createdTime":"2017-06-29T05:24:57Z","lastUpdatedTime":"2017-06-29T05:24:59Z"}
```

Zoals u ziet de `runtimeStatus` van het exemplaar is *voltooid* en de `output` bevat het JSON-geserialiseerd resultaat van de orchestrator-functie-uitvoering.

> [!NOTE]
> De HTTP POST-eindpunt dat aan de slag van de orchestrator-functie is geïmplementeerd in de voorbeeld-app als een HTTP-functie met de naam 'HttpStart' activeren. U kunt vergelijkbare starter logica voor andere triggers van het type, zoals implementeren `queueTrigger`, `eventHubTrigger`, of `timerTrigger`.

Bekijk de logboeken van de uitvoering van de functie. De `E1_HelloSequence` functie aan de slag en meerdere keren voltooid vanwege de replay-gedrag dat wordt beschreven in de [overzicht](durable-functions-overview.md). Er zijn aan de andere kant, alleen voor de drie uitvoeringen van `E1_SayHello` omdat deze functie-uitvoeringen niet opnieuw doen ophalen afgespeeld.

## <a name="visual-studio-sample-code"></a>Visual Studio-voorbeeldcode

Hier volgt de indeling als één C#-bestand in een Visual Studio-project:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

## <a name="next-steps"></a>Volgende stappen

In dit voorbeeld is een eenvoudige indeling voor het koppelen van de functie geïllustreerd. Het volgende voorbeeld laat zien hoe de fan-uitgaand/fan-in patroon wilt implementeren. 

> [!div class="nextstepaction"]
> [De Fan-uitgaand/fan-in het voorbeeld uitvoeren](durable-functions-cloud-backup.md)
