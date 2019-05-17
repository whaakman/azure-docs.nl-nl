---
ms.openlocfilehash: 0b0d2ead4a10d037b09c2a532eb21372ffd0cb82
ms.sourcegitcommit: e729629331ae10097a081a03029398525f4147a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/25/2019
ms.locfileid: "64511091"
---
Als u wilt verwijzen naar de Azure Functions-bindingen 2.x standaard, opent u de *host.json* bestands- en update-inhoud zodat deze overeenkomen met de volgende code.

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```