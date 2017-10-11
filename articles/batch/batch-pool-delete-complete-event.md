---
title: Azure Batch-pool verwijderen gebeurtenis | Microsoft Docs
description: Naslaginformatie voor Batch-pool gebeurtenis verwijderen.
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 890f2ba7fda37060c56177868d6214d517d91831
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="pool-delete-complete-event"></a>Gebeurtenis groep verwijderen

 Deze gebeurtenis wordt verzonden wanneer een groep delete-bewerking is voltooid.

 Het volgende voorbeeld ziet de hoofdtekst van een groep verwijderingsgebeurtenis.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Element|Type|Opmerkingen|
|-------------|----------|-----------|
|id|Tekenreeks|De id van de groep.|
|startTime|Datum/tijd|De tijd dat de groep verwijderen is gestart.|
|Eindtijd|Datum/tijd|De tijd de groep verwijderen is voltooid.|

## <a name="remarks"></a>Opmerkingen
Zie voor meer informatie over statussen en foutcodes voor de bewerking formaat wijzigen van toepassingen [een adresgroep verwijderen van een account](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).