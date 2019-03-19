---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 03/12/2019
ms.author: glenga
ms.custom: include file, fasttrack-edit
ms.openlocfilehash: 3b0be31afbce78c0de8dc919245e92bee7af04b0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57964122"
---
## <a name="update-the-function"></a>Functie bijwerken

Standaard maakt de sjabloon een functie die een functie-sleutel vereist bij het doen van aanvragen. Om het testen van de functie in Azure te vergemakkelijken, moet u de functie bijwerken voor het toestaan van anonieme toegang. De manier waarop u deze wijziging aanbrengt, is afhankelijk van de taal van uw functieproject.

### <a name="c"></a>C\#

Open het code-bestand MyHttpTrigger.cs dat uw nieuwe functie is en wijzig het kenmerk **AuthorizationLevel** in de functiedefinitie in de waarde van `Anonymous` en sla de wijzigingen op.

```csharp
[FunctionName("MyHttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req,
    ILogger log)
```

### <a name="javascript"></a>Javascript

Open het bestand function.json voor de nieuwe functie in een teksteditor, wijzig de eigenschap **authLevel** in **bindings.httpTrigger** in `anonymous` en sla de wijzigingen op.

```json
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
```

Nu kunt u de functie aanroepen in Azure zonder dat u de functiesleutel hoeft op te geven. De functiesleutel is nooit vereist wanneer de functie lokaal wordt uitgevoerd.
