---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: b5d8f67a70961aab21312b6f241081dcb33f66fb
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66131715"
---
```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Eigenschap  |Standaard | Description |
|---------|---------|---------| 
|maxBatchSize|64|Het maximale aantal ontvangen gebeurtenissen per ontvangen lus.|
|prefetchCount|N.v.t.|De standaard PrefetchCount die wordt gebruikt door de onderliggende EventProcessorHost.| 
|batchCheckpointFrequency|1|Het aantal batches van gebeurtenissen te verwerken voordat u een controlepunt van de cursor Event hub maakt.| 
