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
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132207"
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
