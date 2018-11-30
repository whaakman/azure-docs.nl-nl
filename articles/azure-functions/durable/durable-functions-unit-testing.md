---
title: Azure Duurzame functies Eenheidstesten
description: Leer hoe u aan eenheid testen duurzame functies.
services: functions
author: kadimitr
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 02/28/2018
ms.author: kadimitr
ms.openlocfilehash: 159abb533bcc6211b4eca8b2c60f96bf9800db1a
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52642646"
---
# <a name="durable-functions-unit-testing"></a>Duurzame functies Eenheidstesten

Testen eenheid is een belangrijk onderdeel van moderne softwareontwikkelingsprocedures. Eenheidstests zakelijke logica gedrag controleren en beveiligen van de introductie van ongemerkt belangrijke wijzigingen in de toekomst. Duurzame functies kunnen eenvoudig toeneemt in complexiteit, zodat de introductie tot eenheidstests helpt om te voorkomen dat belangrijke wijzigingen. De volgende secties wordt uitgelegd hoe u aan eenheid test de drie functietypen - Orchestration-client, -Orchestrator en -activiteit functies. 

## <a name="prerequisites"></a>Vereisten

De voorbeelden in dit artikel is kennis nodig van de volgende concepten en frameworks: 

* Moduletests uitvoeren

* Durable Functions 

* [xUnit](https://xunit.github.io/) -testen-framework

* [moq](https://github.com/moq/moq4) -framework simuleren


## <a name="base-classes-for-mocking"></a>Basisklassen voor het simuleren 

Nabootsend wordt ondersteund via drie abstracte klassen in duurzame functies:

* [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html) 

* [DurableOrchestrationContextBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContextBase.html)

* [DurableActivityContextBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContextBase.html)

Deze klassen zijn basisklassen voor [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html), [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html), en [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) die de Orchestration-Client definiëren , Orchestrator en methoden van de activiteit. De mocks wordt verwacht gedrag voor de basisklasse methoden instellen zodat de eenheidstest kunt controleren of de bedrijfslogica. Er is een werkstroom in twee stappen voor het testen van de bedrijfslogica in de Orchestration-Client en de Orchestrator-eenheid:

1. Gebruik de basisklassen in plaats van de concrete implementatie bij het definiëren van de Orchestration-Client en de Orchestrator-handtekeningen.
2. In de eenheidstests model van het gedrag van de basisklassen en controleer of de bedrijfslogica. 

Meer informatie vinden in de volgende alinea's voor het testen van functies die gebruikmaken van de orchestration-client-binding en de orchestrator activeren binding.

## <a name="unit-testing-trigger-functions"></a>Triggerfuncties voor test-eenheid

In deze sectie wordt de test jednotky de logica van de volgende HTTP-trigger-functie voor het starten van nieuwe indelingen valideren.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

De taak van de testen eenheid is om te controleren of de waarde van de `Retry-After` koptekst opgegeven in de nettolading van de reactie. Zodat de eenheidstest gesimuleerde aantal een worden [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html) methoden voor het voorspelbare gedrag. 

Eerst een nagebootste van de basisklasse is vereist, [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html). De nagebootste mag een nieuwe klasse die [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html). Echter met behulp van een antwoordnabootsing framework zoals [moq](https://github.com/moq/moq4) vereenvoudigt het proces:    

```csharp
    // Mock DurableOrchestrationClientBase
    var durableOrchestrationClientBaseMock = new Mock<DurableOrchestrationClientBase>();
```

Vervolgens `StartNewAsync` methode is simulatie om terug te keren een bekende exemplaar-ID.

```csharp
    // Mock StartNewAsync method
    durableOrchestrationClientBaseMock.
        Setup(x => x.StartNewAsync(functionName, It.IsAny<object>())).
        ReturnsAsync(instanceId);
```

Volgende `CreateCheckStatusResponse` gesimuleerd altijd terug is een lege HTTP 200-respons.

```csharp
    // Mock CreateCheckStatusResponse method
    durableOrchestrationClientBaseMock
        .Setup(x => x.CreateCheckStatusResponse(It.IsAny<HttpRequestMessage>(), instanceId))
        .Returns(new HttpResponseMessage
        {
            StatusCode = HttpStatusCode.OK,
            Content = new StringContent(string.Empty),
            Headers = 
            { 
                RetryAfter = new RetryConditionHeaderValue(TimeSpan.FromSeconds(10))
            } 
        });
```

`TraceWriter` simulatie is ook mogelijk:

```csharp
    // Mock TraceWriter
    var traceWriterMock = new Mock<TraceWriter>(TraceLevel.Info);

```  

Nu de `Run` methode wordt aangeroepen vanuit de unit-test:

```csharp
    // Call Orchestration trigger function
    var result = await HttpStart.Run(
        new HttpRequestMessage()
        {
            Content = new StringContent("{}", Encoding.UTF8, "application/json"),
            RequestUri = new Uri("http://localhost:7071/orchestrators/E1_HelloSequence"),
        },
        durableOrchestrationClientBaseMock.Object, 
        functionName,
        traceWriterMock.Object);
 ``` 

 De laatste stap is om te vergelijken van de uitvoer met de verwachte waarde:

```csharp
    // Validate that output is not null
    Assert.NotNull(result.Headers.RetryAfter);

    // Validate output's Retry-After header value
    Assert.Equal(TimeSpan.FromSeconds(10), result.Headers.RetryAfter.Delta);
```

Nadat alle stappen worden gecombineerd, hebt het testen van eenheden de volgende code: 

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HttpStartTests.cs)]

## <a name="unit-testing-orchestrator-functions"></a>Eenheidstesten orchestrator-functies

Orchestrator-functies zijn nog interessanter voor eenheidstesten omdat ze meestal veel meer bedrijfslogica hebben.

In deze sectie de eenheid tests valideert de uitvoer van de `E1_HelloSequence` Orchestrator-functie:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

De code van de test eenheid wordt gestart met het maken van een nagebootste:

```csharp
    var durableOrchestrationContextMock = new Mock<DurableOrchestrationContextBase>();
```

Vervolgens wordt de activiteit methodeaanroepen worden simulatie:

```csharp
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Tokyo")).ReturnsAsync("Hello Tokyo!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Seattle")).ReturnsAsync("Hello Seattle!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "London")).ReturnsAsync("Hello London!");
```

Naast de eenheidstest worden gebeld `HelloSequence.Run` methode:

```csharp
    var result = await HelloSequence.Run(durableOrchestrationContextMock.Object);
```

En ten slotte de uitvoer worden gevalideerd:

```csharp
    Assert.Equal(3, result.Count);
    Assert.Equal("Hello Tokyo!", result[0]);
    Assert.Equal("Hello Seattle!", result[1]);
    Assert.Equal("Hello London!", result[2]);
```

Nadat alle stappen worden gecombineerd, hebt het testen van eenheden de volgende code:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceOrchestratorTests.cs)]

## <a name="unit-testing-activity-functions"></a>Activiteitsfuncties voor test-eenheid

Activiteitsfuncties kunnen worden getest op dezelfde manier als niet-duurzame functies eenheid. 

In deze sectie de eenheidstest valideert het gedrag van de `E1_SayHello` activiteit functie:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

En de eenheidstests controleert of de indeling van de uitvoer. De eenheidstests kunt de typen methodeparameters rechtstreeks of mock [DurableActivityContextBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContextBase.html) klasse:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceActivityTests.cs)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over xUnit](http://xunit.github.io/docs/getting-started-dotnet-core)

> [Meer informatie over moq](https://github.com/Moq/moq4/wiki/Quickstart)
