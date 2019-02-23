---
title: Met behulp van de geretourneerde waarde van een Azure-functie
description: Meer informatie over het beheren van de retourwaarden voor Azure Functions
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 01/14/2019
ms.author: cshoe
ms.openlocfilehash: 4ccfe192eaea94cb9b199bd3c6f0bdacf1685519
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56739679"
---
# <a name="using-the-azure-function-return-value"></a>Met behulp van de geretourneerde waarde van de Azure-functie

Dit artikel wordt uitgelegd hoe return werk waarden binnen een functie.

In de talen waarvoor de geretourneerde waarde, kunt u een functie binden [Uitvoerbinding](./functions-triggers-bindings.md#binding-direction) op de geretourneerde waarde:

* In een C#-klassenbibliotheek vormt, het kenmerk van de binding uitvoer van toepassing op de geretourneerde waarde van de methode.
* In andere talen, stelt u de `name` eigenschap in *function.json* naar `$return`.

Als er meerdere uitvoerbindingen, gebruikt u de geretourneerde waarde voor slechts één provider.

In C# en C#-script, andere manieren om gegevens te verzenden naar een Uitvoerbinding zijn `out` parameters en [collector objecten](functions-reference-csharp.md#writing-multiple-output-values).

Zie het gebruik van de geretourneerde waarde van taalspecifieke-voorbeeld:

* [C#](#c-example)
* [C# script (.csx)](#c-script-example)
* [F#](#f-example)
* [JavaScript](#javascript-example)
* [Python](#python-example)

## <a name="c-example"></a>C#-voorbeeld

Hier volgen enkele redenen C#-code die gebruikmaakt van de geretourneerde waarde voor een Uitvoerbinding, gevolgd door een asynchrone-voorbeeld:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

## <a name="c-script-example"></a>Voorbeeld van C#-script

Hier volgt de Uitvoerbinding de *function.json* bestand:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Dit is de C#-scriptcode, gevolgd door een voorbeeld van de asynchrone:

```cs
public static string Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

## <a name="f-example"></a>F#voorbeeld

Hier volgt de Uitvoerbinding de *function.json* bestand:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Hier volgt de F# code:

```fsharp
let Run(input: WorkItem, log: ILogger) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.LogInformation(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="javascript-example"></a>JavaScript-voorbeeld

Hier volgt de Uitvoerbinding de *function.json* bestand:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

De geretourneerde waarde in JavaScript, komt de tweede parameter voor `context.done`:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

## <a name="python-example"></a>Python-voorbeeld

Hier volgt de Uitvoerbinding de *function.json* bestand:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```
Hier volgt de Python-code:

```python
def main(input: azure.functions.InputStream) -> str:
    return json.dumps({
        'name': input.name,
        'length': input.length,
        'content': input.read().decode('utf-8')
    })
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Azure Functions-binding fouten afhandelen](./functions-bindings-errors.md)
