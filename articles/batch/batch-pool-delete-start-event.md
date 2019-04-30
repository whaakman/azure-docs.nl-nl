---
title: Azure Batch gebeurtenis pool verwijderen starten | Microsoft Docs
description: Referentie voor gebeurtenis voor Batch pool verwijderen starten.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
origin.date: 04/20/2017
ms.date: 05/14/2018
ms.author: v-junlch
ms.openlocfilehash: 2352971af3844b56f93c16ebaf6cb23bd5fd8a5a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60774536"
---
# <a name="pool-delete-start-event"></a>Gebeurtenis pool verwijderen starten

 Deze gebeurtenis wordt verzonden wanneer een pool delete-bewerking is gestart. Aangezien het verwijderen van toepassingen een asynchrone is, kunt u verwachten dat een gebeurtenis pool verwijderen voltooid om te worden verzonden nadat de delete-bewerking is voltooid.

 Het volgende voorbeeld ziet de hoofdtekst van een gebeurtenis pool verwijderen starten.

```
{
    "id": "myPool1"
}
```

|Element|Type|Opmerkingen|
|-------------|----------|-----------|
|id|String|De id van de groep.|

<!-- Update_Description: update metedata properties -->