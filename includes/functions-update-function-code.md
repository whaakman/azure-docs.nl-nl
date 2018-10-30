---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 09/16/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d264477693458ff4132c1f69704a480eed2756e0
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49987970"
---
## <a name="update-the-function"></a>Functie bijwerken

Standaard maakt de sjabloon een functie die een functie-sleutel vereist bij het doen van aanvragen. Om het testen van de functie in Azure te vergemakkelijken, moet u de functie bijwerken voor het toestaan van anonieme toegang. De manier waarop u deze wijziging aanbrengt, is afhankelijk van de taal van uw functieproject.

### <a name="c"></a>C\#

Open het code-bestand MyHttpTrigger.cs dat uw nieuwe functie is en wijzig het kenmerk **AuthorizationLevel** in de functiedefinitie in de waarde van `anonymous` en sla de wijzigingen op.

```csharp
[FunctionName("MyHttpTrigger")]
        public static IActionResult Run([HttpTrigger(AuthorizationLevel.Anonymous, 
            "get", "post", Route = null)]HttpRequest req, ILogger log)
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
