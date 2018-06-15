---
title: Status van de aangepaste orchestration in duurzame functies - Azure
description: Informatie over het configureren en gebruiken van aangepaste orchestration status voor duurzame functies.
services: functions
author: kadimitr
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/24/2018
ms.author: azfuncdf
ms.openlocfilehash: 840b96b9cfdb28ca1b17f54698677f4d491342c8
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
ms.locfileid: "32310349"
---
# <a name="custom-orchestration-status-in-durable-functions-azure-functions"></a>Status van de aangepaste orchestration in duurzame functies (Azure-functies)

Aangepaste orchestration status kunt u een aangepaste statuswaarde instellen voor uw orchestrator-functie. Deze status wordt opgegeven via de HTTP-GetStatus API of de `DurableOrchestrationClient.GetStatusAsync` API.

## <a name="sample-use-cases"></a>Voorbeeld gebruiksvoorbeelden 

### <a name="visualize-progress"></a>Voortgang visualiseren

Clients kunnen pollen van het eindpunt van de status en wordt weergegeven een gebruikersinterface voor het visualiseren van de huidige uitvoeringsfase. Het volgende voorbeeld toont de voortgang delen:

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

En vervolgens ontvangt de client de uitvoer van de orchestration alleen wanneer `CustomStatus` veld is ingesteld op 'Londen':

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

### <a name="output-customization"></a>Aanpassing van de uitvoer 

Een ander interessante scenario is gebruikers segmenteren door aangepaste uitvoer op basis van unieke kenmerken of interacties terug te keren. Met behulp van aangepaste orchestration status blijft de clientcode algemene. Alle wijzigingen van de belangrijkste gebeurt aan de serverzijde, zoals wordt weergegeven in het volgende voorbeeld:

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

De orchestrator kan unieke instructies bieden aan de clients via de aangepaste status. De instructies van de aangepaste status worden toegewezen aan de stappen in de orchestration-code:

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

Terwijl de orchestration wordt uitgevoerd, kunnen externe clients deze aangepaste status ophalen:

```http
GET /admin/extensions/DurableTaskExtension/instances/instance123

```

Clients krijgen het volgende antwoord: 

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
>  De status van de aangepaste nettolading is beperkt tot 16 KB aan UTF-16 JSON-tekst omdat het moet een kolom met Azure Table Storage. Ontwikkelaars kunnen externe opslag gebruiken als ze nodig hebben groter nettolading.


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over HTTP-API's in duurzame functies](durable-functions-http-api.md)


