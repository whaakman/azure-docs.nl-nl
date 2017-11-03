---
title: Azure Batch pool formaat start gebeurtenis | Microsoft Docs
description: Verwijzing voor Batch-pool formaat start-gebeurtenis.
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
ms.openlocfilehash: 826cd984d26b923ba38562e05a2e75c399be9121
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="pool-resize-start-event"></a>Gebeurtenissen voor het starten van toepassingen formaat wijzigen

 Deze gebeurtenis wordt verzonden wanneer het formaat van een groep van toepassingen is gestart. Aangezien het formaat van de groep van toepassingen een asynchrone gebeurtenis is, kunt u verwachten groep complete gebeurtenis resize om te worden verzonden wanneer de bewerking formaat is voltooid.

 Het volgende voorbeeld ziet de hoofdtekst van de groep van toepassingen startgebeurtenis resize voor een groep van toepassingen vergroten of verkleinen van 0 voor 2 knooppunten met een handmatige formaat.

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
|poolId|Tekenreeks|De id van de groep.|
|nodeDeallocationOption|Tekenreeks|Geeft aan wanneer knooppunten kunnen worden verwijderd uit de groep als de poolgrootte afneemt.<br /><br /> Mogelijke waarden zijn:<br /><br /> **requeue** : Beëindig actieve taken en requeue ze. De taken wordt opnieuw uitgevoerd wanneer de taak is ingeschakeld. Knooppunten verwijderen zodra taken zijn beëindigd.<br /><br /> **beëindigen** – actieve taken beëindigen. De taken worden niet opnieuw uitgevoerd. Knooppunten verwijderen zodra taken zijn beëindigd.<br /><br /> **taskcompletion** : toestaan dat actieve taken te voltooien. Er zijn geen nieuwe taken tijdens het wachten plannen. Verwijder de knooppunten wanneer alle taken zijn voltooid.<br /><br /> **Retaineddata** -Sta toe dat actieve taken uit om te voltooien en wacht vervolgens tot alle bewaarperioden om te verlopen. Er zijn geen nieuwe taken tijdens het wachten plannen. Verwijder de knooppunten wanneer alle bewaarperioden voor taken zijn verlopen.<br /><br /> De standaardwaarde is requeue.<br /><br /> Als de poolgrootte toeneemt, wordt de waarde is ingesteld op **ongeldig**.|
|currentDedicated|Int32|Het aantal rekenknooppunten momenteel toegewezen aan de groep.|
|targetDedicated|Int32|Het aantal rekenknooppunten die zijn aangevraagd voor de groep.|
|enableAutoScale|BOOL|Hiermee geeft u op of de poolgrootte automatisch wordt aangepast gedurende een bepaalde periode.|
|isAutoPool|BOOL|Speficies of de groep is gemaakt via een taak AutoPool mechanisme.|
