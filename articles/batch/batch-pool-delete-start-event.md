---
title: Azure Batch pool verwijderen start-gebeurtenis | Microsoft Docs
description: Verwijzing voor Batch-pool verwijderen start-gebeurtenis.
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
ms.openlocfilehash: 8737b9ff6452730ff5a55fa7324e37f0fe715433
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
ms.locfileid: "30312056"
---
# <a name="pool-delete-start-event"></a>Gebeurtenis pool verwijderen starten

 Deze gebeurtenis wordt verzonden wanneer een groep delete-bewerking is gestart. Aangezien het verwijderen van toepassingen een asynchrone gebeurtenis is, kunt u verwachten groep complete verwijderingsgebeurtenis om te worden verzonden nadat de delete-bewerking is voltooid.

 Het volgende voorbeeld ziet de hoofdtekst van een groep verwijderen start-gebeurtenis.

```
{
    "id": "myPool1"
}
```

|Element|Type|Opmerkingen|
|-------------|----------|-----------|
|id|Tekenreeks|De id van de groep.|