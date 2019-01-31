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
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474289"
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