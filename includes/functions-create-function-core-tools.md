---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 26789a12053fa6275b09836e706c391e181c8efd
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175814"
---
## <a name="create-a-function"></a>Een functie maken

Met de volgende opdracht maakt u een functie die via HTTP is geactiveerd, met de naam `MyHttpTrigger`.

```bash
func new --name MyHttpTrigger --template "HttpTrigger"
```

Wanneer de opdracht wordt uitgevoerd, ziet u ongeveer de volgende uitvoer:

```output
The function "MyHttpTrigger" was created successfully from the "HttpTrigger" template.
```
