---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: da0cbab59d9c801419ac4b3704fee3275f337fd9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66131632"
---
Hiermee geeft u het aantal functieaanroepen zijn samengevoegd wanneer [berekenen van de metrische gegevens voor Application Insights](../articles/azure-functions/functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Eigenschap |Standaard  | Description |
|---------|---------|---------| 
|batchSize|1000|Maximum aantal aanvragen om samen te voegen.| 
|flushTimeout|00:00:30|Maximale tijd periode om samen te voegen.| 

Functieaanroepen worden geaggregeerd als de eerste dag van de twee limieten zijn bereikt.