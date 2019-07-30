---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/27/2019
ms.author: glenga
ms.openlocfilehash: d697334fe56fb9133a06cee79067c60bc3a37281
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639121"
---
De eenvoudigste manier om bindings uitbreidingen te installeren is door [uitbreidings bundels](../articles/azure-functions/functions-bindings-register.md#extension-bundles)in te scha kelen. Wanneer u bundels inschakelt, wordt automatisch een vooraf gedefinieerde set uitbreidings pakketten geïnstalleerd.

Als u uitbreidings bundels wilt inschakelen, opent u het bestand host. json en werkt u de inhoud bij zodat deze overeenkomt met de volgende code:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```