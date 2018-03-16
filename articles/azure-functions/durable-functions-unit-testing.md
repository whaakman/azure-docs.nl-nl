---
title: Azure Functions duurzame eenheid testen
description: Meer informatie over hoe eenheid test duurzame functies.
services: functions
author: kadimitr
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/28/2018
ms.author: kadimitr
ms.openlocfilehash: a8d2a2281dcaf6e5e308ad4a2aafb167f9ba9121
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="durable-functions-unit-testing"></a>Duurzame functies eenheid testen

Testen van de eenheden is een belangrijk onderdeel van de moderne software development procedures. Controles voor zakelijke logica gedrag controleren en beschermen tegen introducing onopgemerkte grote wijzigingen in de toekomst. Duurzame functies kunnen gemakkelijk worden uitgebreid in complexiteit zodat introducing eenheidstests helpt om te voorkomen dat de wijzigingen op te splitsen. De volgende secties wordt uitgelegd hoe eenheid test de functietypen drie - client, Orchestrator en activiteit Orchestration functies. 

## <a name="prerequisites"></a>Vereisten

De voorbeelden in dit artikel vereisen kennis van de volgende concepten en frameworks: 

* Testen van de eenheden

* Durable Functions 

* [xUnit](https://xunit.github.io/) -framework testen

* [moq](https://github.com/moq/moq4) -framework Mocking


## <a name="base-classes-for-mocking"></a>Basisklassen voor mocking 

Mocking wordt ondersteund via twee abstracte klassen in duurzame functies:

* [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html) 

* [DurableOrchestrationContextBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContextBase.html)

Deze klassen zijn basisklassen voor [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) en [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) die de Orchestration-Client en de Orchestrator-methoden definiëren. De mocks wordt verwacht gedrag voor basisklasse methoden instellen, zodat het testen van eenheden kunt controleren of de zakelijke logica. Er is een werkstroom in twee stappen voor het testen van de zakelijke logica in de Orchestration-Client en de Orchestrator-eenheid:

1. Gebruik in plaats van de concrete implementatie op de basisklassen behoren bij het definiëren van de Orchestration-Client en de Orchestrator-handtekeningen.
2. In de eenheidstests model van het gedrag van de basisklassen en controleer of de zakelijke logica. 

Meer informatie vinden in de volgende leden voor het testen van functies die gebruikmaken van de orchestration-client-binding en de orchestrator-binding activeren.

## <a name="unit-testing-trigger-functions"></a>Eenheid testen trigger-functies

In deze sectie wordt het testen van eenheden de logica van de volgende HTTP-trigger-functie voor het starten van nieuwe integraties valideren.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

De taak eenheid testen worden om te controleren of de waarde van de `Retry-After` header is opgegeven in de nettolading van de reactie. Zodat het testen van eenheden wordt een aantal model [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html) methoden om ervoor te zorgen voorspelbaar gedrag. 

Eerst een nagebootste van de basisklasse is vereist, [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html). De nagebootste mag een nieuwe klasse die wordt geïmplementeerd [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html). Echter, met behulp van een mocking framework zoals [moq](https://github.com/moq/moq4) vereenvoudigt het proces:    

```csharp
    // Mock DurableOrchestrationClientBase
    var durableOrchestrationClientBaseMock = new Mock<DurableOrchestrationClientBase>();
```

Vervolgens `StartNewAsync` methode is mocked om te retourneren van een bekende exemplaar-ID.

```csharp
    // Mock StartNewAsync method
    durableOrchestrationClientBaseMock.
        Setup(x => x.StartNewAsync(functionName, It.IsAny<object>())).
        ReturnsAsync(instanceId);
```

Volgende `CreateCheckStatusResponse` mocked altijd return is een leeg HTTP 200-antwoord.

```csharp
    // Mock CreateCheckStatusResponse method
    durableOrchestrationClientBaseMock
        .Setup(x => x.CreateCheckStatusResponse(It.IsAny<HttpRequestMessage>(), instanceId))
        .Returns(new HttpResponseMessage
        {
            StatusCode = HttpStatusCode.OK,
            Content = new StringContent(string.Empty),
        });
```

`TraceWriter` ook mocked is:

```csharp
    // Mock TraceWriter
    var traceWriterMock = new Mock<TraceWriter>(TraceLevel.Info);

```  

Nu de `Run` methode aangeroepen vanuit de unit-test:

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

 De laatste stap is het vergelijken van de uitvoer met de verwachte waarde:

```csharp
    // Validate that output is not null
    Assert.NotNull(result.Headers.RetryAfter);

    // Validate output's Retry-After header value
    Assert.Equal(TimeSpan.FromSeconds(10), result.Headers.RetryAfter.Delta);
```

Na het combineren van alle stappen heeft de unit-test de volgende code: 

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HttpStartTests.cs)]

## <a name="unit-testing-orchestrator-functions"></a>Testen van orchestrator-functies van de eenheden

Orchestrator-functies zijn nog meer interessante voor het testen aangezien meestal veel meer bedrijfslogica hebben eenheid. Orchestrator-functies kunnen op dit moment wordt alleen in C# worden geïmplementeerd.

In dit gedeelte de eenheid tests valideert de uitvoer van de `E1_HelloSequence` Orchestrator-functie:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

De code van de test eenheid wordt gestart met het maken van een nagebootste:

```csharp
    var durableOrchestrationContextMock = new Mock<DurableOrchestrationContextBase>();
```

Vervolgens wordt de activiteit methodeaanroepen worden mocked:

```csharp
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Tokyo")).ReturnsAsync("Hello Tokyo!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Seattle")).ReturnsAsync("Hello Seattle!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "London")).ReturnsAsync("Hello London!");
```

Naast het testen van eenheden belt `HelloSequence.Run` methode:

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

Na het combineren van alle stappen heeft de unit-test de volgende code:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceOrchestratorTests.cs)]

## <a name="unit-testing-activity-functions"></a>Eenheid testen activiteit functies

Functies van de activiteit kunnen worden getest op dezelfde manier als niet-duurzame functies eenheid. Functies van de activiteit beschikt niet over een basisklasse voor mocking. Dus de eenheidstests de parametertypen rechtstreeks gebruiken.

In deze sectie valideert het testen van eenheden het gedrag van de `E1_SayHello` activiteit functie:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

En het testen van eenheden controleert of de indeling van de uitvoer:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceActivityTests.cs)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over xUnit](http://xunit.github.io/docs/getting-started-dotnet-core)

> [Meer informatie over moq](https://github.com/Moq/moq4/wiki/Quickstart)