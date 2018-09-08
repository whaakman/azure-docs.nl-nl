---
title: Status van de aangepaste indeling in duurzame functies - Azure
description: Informatie over het configureren en gebruiken van de status van aangepaste indeling voor duurzame functies.
services: functions
author: kadimitr
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: azfuncdf
ms.openlocfilehash: c8eb2be6836e11ddbaed81970024ea7200ea819d
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093088"
---
# <a name="custom-orchestration-status-in-durable-functions-azure-functions"></a>Status van de aangepaste indeling in duurzame functies (Azure Functions)

Status van aangepaste indeling kunt u een aangepaste statuswaarde instellen voor de orchestrator-functie. Deze status wordt geboden via de GetStatus HTTP-API of de `DurableOrchestrationClient.GetStatusAsync` API.

## <a name="sample-use-cases"></a>Voorbeelden van use cases 

### <a name="visualize-progress"></a>Voortgang visualiseren

Clients kunnen het eindpunt van de status controleren en een voortgang gebruikersinterface die worden gevisualiseerd met de huidige uitvoeringsfase weergeven. Het volgende voorbeeld ziet u voortgang delen:

```csharp
[FunctionName("E1_HelloSequence")]
public static async Task<List<string>> Run(
  [OrchestrationTrigger] DurableOrchestrationContextBase context)
{
  var outputs = new List<string>();

  outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Tokyo"));
  context.SetCustomStatus("Tokyo");
  outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Seattle"));
  context.SetCustomStatus("Seattle");
  outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "London"));
  context.SetCustomStatus("London");

  // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
  return outputs;
}

[FunctionName("E1_SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
  return $"Hello {name}!";
}
```

En vervolgens ontvangt de client de uitvoer van de orchestration alleen wanneer `CustomStatus` veld is ingesteld op "Londen":

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
  [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
  [OrchestrationClient] DurableOrchestrationClientBase starter,
  string functionName,
  ILogger log)
{
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    DurableOrchestrationStatus durableOrchestrationStatus = await starter.GetStatusAsync(instanceId);
    while (durableOrchestrationStatus.CustomStatus.ToString() != "London")
    {
      await Task.Delay(200);
      durableOrchestrationStatus = await starter.GetStatusAsync(instanceId);
    }

    HttpResponseMessage httpResponseMessage = new HttpResponseMessage(HttpStatusCode.OK)
    {
      Content = new StringContent(JsonConvert.SerializeObject(durableOrchestrationStatus))
    };

    return httpResponseMessage;
  }
}
```

### <a name="output-customization"></a>Aanpassing van uitvoer 

Een andere interessante scenario is gebruikers segmenteren door aangepaste uitvoer op basis van unieke kenmerken of interacties. Met behulp van de status van aangepaste indeling blijft de client-side-code algemene. Alle wijzigingen van de belangrijkste gebeurt op de server, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("CityRecommender")]
public static void Run(
  [OrchestrationTrigger] DurableOrchestrationContextBase context)
{
  int userChoice = context.GetInput<int>();

  switch (userChoice)
  {
    case 1:
    context.SetCustomStatus(new
    {
      recommendedCities = new[] {"Tokyo", "Seattle"},
      recommendedSeasons = new[] {"Spring", "Summer"}
     });
      break;
    case 2:
      context.SetCustomStatus(new
      {
        recommendedCity = new[] {"Seattle, London"},
        recommendedSeasons = new[] {"Summer"}
      });
        break;
      case 3:
      context.SetCustomStatus(new
      {
        recommendedCity = new[] {"Tokyo, London"},
        recommendedSeasons = new[] {"Spring", "Summer"}
      });
        break;
  }

  // Wait for user selection and refine the recommendation
} 
```

### <a name="instruction-specification"></a>Instructie-specificatie 

De orchestrator kan unieke instructies bieden aan de clients via de aangepaste status. De status van aangepaste instructies worden toegewezen aan de stappen in de orchestration-code:

```csharp
[FunctionName("ReserveTicket")]
public static async Task<bool> Run(
  [OrchestrationTrigger] DurableOrchestrationContextBase context)
{
  string userId = context.GetInput<string>();

  int discount = await context.CallActivityAsync<int>("CalculateDiscount", userId);

  context.SetCustomStatus(new
  {
    discount = discount,
    discountTimeout = 60,
    bookingUrl = "https://www.myawesomebookingweb.com",
  });

  bool isBookingConfirmed = await context.WaitForExternalEvent<bool>("BookingConfirmed");

  context.SetCustomStatus(isBookingConfirmed
    ? new {message = "Thank you for confirming your booking."}
    : new {message = "The booking was not confirmed on time. Please try again."});

  return isBookingConfirmed;
}
```

## <a name="sample"></a>Voorbeeld

In het volgende voorbeeld wordt is de status van de aangepaste eerst; ingesteld

```csharp
public static async Task SetStatusTest([OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    var customStatus = new { nextActions = new [] {"A", "B", "C"}, foo = 2, };
    ctx.SetCustomStatus(customStatus);

    // ...do more work...
}
```

Terwijl de indeling wordt uitgevoerd, kunnen deze aangepaste status ophalen van externe clients:

```http
GET /admin/extensions/DurableTaskExtension/instances/instance123

```

Clients ontvangt het volgende antwoord: 

```http
{
  "runtimeStatus": "Running",
  "input": null,
  "customStatus": { "nextActions": ["A", "B", "C"], "foo": 2 },
  "output": null,
  "createdTime": "2017-10-06T18:30:24Z",
  "lastUpdatedTime": "2017-10-06T19:40:30Z"
}
```

> [!WARNING]
>  De status van de aangepaste nettolading is beperkt tot 16 KB van UTF-16-JSON-tekst omdat deze kunnen zijn moet voor de kolom van een Azure-tabelopslag. Ontwikkelaars kunnen externe opslag gebruiken als ze nodig hebben grotere nettolading.


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over HTTP-API's in duurzame functies](durable-functions-http-api.md)


