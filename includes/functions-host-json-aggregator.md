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
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50251258"
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

|Eigenschap |Standaard  | Beschrijving |
|---------|---------|---------| 
|batchSize|1000|Maximum aantal aanvragen om samen te voegen.| 
|flushTimeout|00:00:30|Maximale tijd periode om samen te voegen.| 

Functieaanroepen worden geaggregeerd als de eerste dag van de twee limieten zijn bereikt.