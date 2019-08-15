---
title: Testen van Azure Durable Functions-eenheid
description: Meer informatie over het testen van de eenheids Durable Functions.
author: ggailey777
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: glenga
ms.openlocfilehash: 0080365853e7a9c74d3ba0e5efb06ce5a3af2a21
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967101"
---
# <a name="durable-functions-unit-testing"></a>Testen van Durable Functions eenheid

Eenheids tests is een belang rijk onderdeel van moderne software ontwikkelings procedures. Eenheids tests verifiëren het gedrag van bedrijfs logica en beveiligen tegen het introduceren van onopgemerkte wijzigingen in de toekomst. Durable Functions kan gemakkelijk groeien in complexiteit zodat er door het introduceren van de eenheids tests geen wijzigingen kunnen worden opgesplitst. In de volgende secties wordt uitgelegd hoe u de drie functie typen-Orchestration-client, Orchestrator en activiteit functies test eenheid testen.

## <a name="prerequisites"></a>Vereisten

Voor de voor beelden in dit artikel is kennis van de volgende concepten en frameworks vereist:

* Moduletests uitvoeren

* Durable Functions

* [xUnit](https://xunit.github.io/) -test framework

* [MOQ](https://github.com/moq/moq4) : Framework model

## <a name="base-classes-for-mocking"></a>Basis klassen voor het aftrekken

De deprototypen wordt ondersteund via drie abstracte klassen in Durable Functions:

* [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html)

* [DurableOrchestrationContextBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContextBase.html)

* [DurableActivityContextBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContextBase.html)

Deze klassen zijn basis klassen voor [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html), [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html)en [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) die de Orchestration-client, Orchestrator en activiteiten methoden definiëren. Met de-modellen wordt het verwachte gedrag voor basis klassen methoden ingesteld, zodat de eenheids test de bedrijfs logica kan controleren. Er is een werk stroom met twee stappen voor het testen van de bedrijfs logica in de Orchestration-client en Orchestrator:

1. Gebruik de basis klassen in plaats van de concrete implementatie bij het definiëren van de hand tekeningen van de Orchestration-client en Orchestrator.
2. In de eenheids tests wordt het gedrag van de basis klassen gesimuleerd en wordt de bedrijfs logica gecontroleerd.

Meer informatie vindt u in de volgende alinea's voor het testen van functies die gebruikmaken van de Orchestration-client binding en de Orchestrator-trigger binding.

## <a name="unit-testing-trigger-functions"></a>Activerings functies voor eenheids tests

In deze sectie valideert de eenheids test de logica van de volgende HTTP-activerings functie voor het starten van nieuwe integraties.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

De eenheids test taak wordt uitgevoerd om de waarde van de `Retry-After` header te controleren die is opgenomen in de nettolading van de reactie. De eenheids test zal dus een aantal [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html) -methoden betrekken om voorspelbaar gedrag te garanderen.

Eerst is een model van de basis klasse vereist, [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html). De Modeler kan een nieuwe klasse zijn die [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html)implementeert. Het gebruik van een model raamwerk zoals [MOQ](https://github.com/moq/moq4) vereenvoudigt echter het proces:

```csharp
    // Mock DurableOrchestrationClientBase
    var durableOrchestrationClientBaseMock = new Mock<DurableOrchestrationClientBase>();
```

Vervolgens `StartNewAsync` wordt de methode gebruikt om een bekende exemplaar-id te retour neren.

```csharp
    // Mock StartNewAsync method
    durableOrchestrationClientBaseMock.
        Setup(x => x.StartNewAsync(functionName, It.IsAny<object>())).
        ReturnsAsync(instanceId);
```

De `CreateCheckStatusResponse` volgende is gesimuleerd om altijd een leeg http 200-antwoord te retour neren.

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

`ILogger`wordt ook gemodeleerd:

```csharp
    // Mock ILogger
    var loggerMock = new Mock<ILogger>();

```  

De `Run` methode wordt nu aangeroepen vanuit de eenheids test:

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
        loggerMock.Object);
 ```

 De laatste stap bestaat uit het vergelijken van de uitvoer met de verwachte waarde:

```csharp
    // Validate that output is not null
    Assert.NotNull(result.Headers.RetryAfter);

    // Validate output's Retry-After header value
    Assert.Equal(TimeSpan.FromSeconds(10), result.Headers.RetryAfter.Delta);
```

Na het combi neren van alle stappen, heeft de eenheids test de volgende code:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HttpStartTests.cs)]

## <a name="unit-testing-orchestrator-functions"></a>Orchestrator-functies voor het testen van eenheden

Orchestrator-functies zijn nog interessanter voor het testen van de eenheid omdat ze meestal veel meer bedrijfs logica hebben.

In dit gedeelte wordt de uitvoer van de `E1_HelloSequence` Orchestrator-functie door de eenheids tests gevalideerd:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

De code voor de eenheids test wordt gestart met het maken van een model:

```csharp
    var durableOrchestrationContextMock = new Mock<DurableOrchestrationContextBase>();
```

Vervolgens worden de aanroepen van de activiteit methode gesimuleerd:

```csharp
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Tokyo")).ReturnsAsync("Hello Tokyo!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Seattle")).ReturnsAsync("Hello Seattle!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "London")).ReturnsAsync("Hello London!");
```

De volgende methode wordt aangeroepen `HelloSequence.Run` door de eenheids test:

```csharp
    var result = await HelloSequence.Run(durableOrchestrationContextMock.Object);
```

En ten slotte wordt de uitvoer gevalideerd:

```csharp
    Assert.Equal(3, result.Count);
    Assert.Equal("Hello Tokyo!", result[0]);
    Assert.Equal("Hello Seattle!", result[1]);
    Assert.Equal("Hello London!", result[2]);
```

Na het combi neren van alle stappen, heeft de eenheids test de volgende code:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceOrchestratorTests.cs)]

## <a name="unit-testing-activity-functions"></a>Activiteiten functies voor eenheids tests

Activiteit functies kunnen als eenheid worden getest op dezelfde manier als niet-duurzame functies.

In deze sectie wordt met de eenheids test het gedrag van `E1_SayHello` de activiteit functie gevalideerd:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

En de eenheids tests controleren de indeling van de uitvoer. De eenheids tests kunnen de parameter typen rechtstreeks of model [DurableActivityContextBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContextBase.html) -klasse gebruiken:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceActivityTests.cs)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over xUnit](https://xunit.github.io/docs/getting-started-dotnet-core)
> 
> [Meer informatie over MOQ](https://github.com/Moq/moq4/wiki/Quickstart)
