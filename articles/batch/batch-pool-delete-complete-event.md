---
title: Azure Batch-pool verwijderen voltooid gebeurtenis | Microsoft Docs
description: Naslaginformatie voor Batch-pool verwijderen voltooid gebeurtenis.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: e715ccd0f5e79f9c640a3c060b0252b798748b4d
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474545"
---
# <a name="pool-delete-complete-event"></a>Gebeurtenis pool verwijderen voltooid

 Deze gebeurtenis wordt verzonden wanneer een pool delete-bewerking is voltooid.

 Het volgende voorbeeld ziet de hoofdtekst van een gebeurtenis pool verwijderen voltooid.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Element|Type|Opmerkingen|
|-------------|----------|-----------|
|id|String|De id van de groep.|
|startTime|DateTime|De tijd die de groep verwijderen is gestart.|
|endTime|DateTime|Het tijdstip waarop de pool verwijderen voltooid.|

## <a name="remarks"></a>Opmerkingen
Zie voor meer informatie over de Staten en in de foutcodes voor groep-/ verkleinbewerking [verwijderen van een groep van een account](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).