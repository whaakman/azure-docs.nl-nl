---
title: Azure Batch gebeurtenis pool verwijderen starten | Microsoft Docs
description: Referentie voor gebeurtenis voor Batch pool verwijderen starten.
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
ms.openlocfilehash: 2352971af3844b56f93c16ebaf6cb23bd5fd8a5a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
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