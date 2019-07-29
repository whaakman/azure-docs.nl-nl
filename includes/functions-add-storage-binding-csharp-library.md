---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: 4db460a5dcefb49de3ad2b594d3957cbcf7445c3
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592805"
---
In een C# klassen bibliotheek project worden de bindingen gedefinieerd als bindings kenmerken voor de functie methode. Het bestand *Function. json* wordt vervolgens automatisch gegenereerd op basis van deze kenmerken.

Open het project bestand *HttpTrigger.cs* en voeg de volgende `using` instructie toe:

```cs
using Microsoft.Azure.WebJobs.Extensions.Storage;
```

Voeg de volgende para meter toe `Run` aan de methode definitie:

```cs
[Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg
```

De `msg` para meter is `ICollector<T>` een type dat een verzameling berichten vertegenwoordigt die naar een uitvoer binding worden geschreven wanneer de functie is voltooid. In dit geval is de uitvoer een opslag wachtrij met de `outqueue`naam. De connection string voor het opslag account is ingesteld door de `StorageAccountAttribute`. Dit kenmerk geeft de instelling aan die het opslag account bevat connection string en kan worden toegepast op het niveau van de klasse, methode of para meter. In dit geval kunt u weglaten `StorageAccountAttribute` omdat u het standaard opslag account al gebruikt.

De definitie van de run-methode moet er nu als volgt uitzien:  

```cs
[FunctionName("HttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
    [Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, ILogger log)
```
