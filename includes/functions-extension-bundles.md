---
ms.openlocfilehash: 0b0d2ead4a10d037b09c2a532eb21372ffd0cb82
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175816"
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