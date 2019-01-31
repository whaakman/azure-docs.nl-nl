---
title: Azure Batch pool resize gebeurtenis starten | Microsoft Docs
description: Naslaginformatie voor Batch gebeurtenis formaat pool wijzigen starten.
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
ms.openlocfilehash: 63abeaca5a9c87945671e79a9c8d7a2512d0d777
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55471331"
---
# <a name="pool-resize-start-event"></a>Gebeurtenis formaat pool wijzigen starten

 Deze gebeurtenis wordt verzonden wanneer de grootte van een groep van toepassingen is gestart. Aangezien de grootte van de pool een asynchrone is, kunt u verwachten dat een gebeurtenis formaat pool wijzigen voltooid om te worden verzonden nadat de bewerking formaat is voltooid.

 Het volgende voorbeeld ziet de hoofdtekst van een gebeurtenis formaat pool wijzigen starten voor een pool vergroten of verkleinen van 0 aan 2 knooppunten met een handmatige formaat wijzigen.

```
{
    "poolId": "myPool1",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 0,
    "targetDedicated": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|Element|Type|Opmerkingen|
|-------------|----------|-----------|
|poolId|String|De id van de groep.|
|nodeDeallocationOption|String|Hiermee geeft u als knooppunten kunnen worden verwijderd uit de groep als de poolgrootte afneemt.<br /><br /> Mogelijke waarden zijn:<br /><br /> **taak** : Beëindig actieve taken en plaats ze. De taken worden opnieuw uitgevoerd wanneer de taak is ingeschakeld. Knooppunten verwijderen zodra taken zijn beëindigd.<br /><br /> **beëindigen** : Beëindig actieve taken. De taken worden niet opnieuw uitgevoerd. Knooppunten verwijderen zodra taken zijn beëindigd.<br /><br /> **taskcompletion** : toestaan dat actieve taken om te voltooien. Plan geen nieuwe taken tijdens het wachten. Verwijder de knooppunten wanneer alle taken zijn voltooid.<br /><br /> **Retaineddata** -toestaan dat actieve taken uit om te voltooien en wacht vervolgens tot alle bewaarperioden om te verlopen. Plan geen nieuwe taken tijdens het wachten. Verwijder de knooppunten wanneer alle bewaarperioden voor taken zijn verlopen.<br /><br /> De standaardwaarde is de taak.<br /><br /> Als het verhogen van de grootte van de groep van toepassingen wordt de waarde is ingesteld op **ongeldig**.|
|currentDedicated|Int32|Het aantal rekenknooppunten dat momenteel is toegewezen aan de groep.|
|targetDedicated|Int32|Het aantal rekenknooppunten die zijn aangevraagd voor de pool.|
|enableAutoScale|Bool|Hiermee geeft u op of de groepsgrootte die automatisch wordt aangepast na verloop van tijd.|
|isAutoPool|Bool|Hiermee geeft u op of de groep is gemaakt via een job AutoPool mechanisme.|
