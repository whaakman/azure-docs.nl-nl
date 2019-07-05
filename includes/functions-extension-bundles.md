---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/27/2019
ms.author: glenga
ms.openlocfilehash: 8110d0a9d574c6691322df2162ca877b031cbc59
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442285"
---
De eenvoudigste manier voor het installeren van de bindinguitbreidingen is om in te schakelen [extensie bundels](../articles/azure-functions/functions-bindings-register.md#extension-bundles). Pakketten worden automatisch geïnstalleerd met bundels is ingeschakeld, wordt een vooraf gedefinieerde set extensie.

Om in te schakelen extensie bundels, opent u de *host.json* bestands- en bijwerken van de inhoud zodat deze overeenkomt met de volgende code:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```