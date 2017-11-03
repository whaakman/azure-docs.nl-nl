---
title: Azure Batch pool verwijderen start-gebeurtenis | Microsoft Docs
description: Verwijzing voor Batch-pool verwijderen start-gebeurtenis.
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
ms.openlocfilehash: f8a5241dce422e5c826ab428da6d7bc93284a1cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="pool-delete-start-event"></a>Gebeurtenissen voor het starten van toepassingen verwijderen

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