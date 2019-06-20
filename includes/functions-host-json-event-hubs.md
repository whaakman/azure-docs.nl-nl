---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: b5d8f67a70961aab21312b6f241081dcb33f66fb
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175829"
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
