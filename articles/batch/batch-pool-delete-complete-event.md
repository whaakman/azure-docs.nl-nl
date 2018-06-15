---
title: Azure Batch-pool verwijderen gebeurtenis | Microsoft Docs
description: Naslaginformatie voor Batch-pool gebeurtenis verwijderen.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: danlep
ms.openlocfilehash: bfcbcf40efc64ab1c79ee1a86e02502c68ad6d47
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
ms.locfileid: "30310311"
---
# <a name="pool-delete-complete-event"></a>Gebeurtenis pool verwijderen voltooid

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
|startTime|DateTime|De tijd dat de groep verwijderen is gestart.|
|endTime|DateTime|De tijd de groep verwijderen is voltooid.|

## <a name="remarks"></a>Opmerkingen
Zie voor meer informatie over statussen en foutcodes voor de bewerking formaat wijzigen van toepassingen [een adresgroep verwijderen van een account](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).