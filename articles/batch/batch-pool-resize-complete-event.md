---
title: Azure Batch-pool vergroten of verkleinen van de gebeurtenis | Microsoft Docs
description: Naslaginformatie voor Batch-pool vergroten of verkleinen van de gebeurtenis.
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
ms.openlocfilehash: 7072293d98526812cb42ce9c2f8e33bfcafaa149
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="pool-resize-complete-event"></a>Gebeurtenis resize van toepassingen

 Deze gebeurtenis wordt verzonden wanneer het formaat van een groep van toepassingen is voltooid of mislukt.

 Het volgende voorbeeld ziet de hoofdtekst van de groep complete gebeurtenis resize voor een groep die vergroot en is voltooid.

```
{
    "id": "p_1_0_01503750-252d-4e57-bd96-d6aa05601ad8",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 4,
    "targetDedicated": 4,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "result": "Success",
    "resizeError": "The operation succeeded"
}
```

|Element|Type|Opmerkingen|
|-------------|----------|-----------|
|id|Tekenreeks|De id van de groep.|
|nodeDeallocationOption|Tekenreeks|Geeft aan wanneer knooppunten kunnen worden verwijderd uit de groep als de poolgrootte afneemt.<br /><br /> Mogelijke waarden zijn:<br /><br /> **requeue** : Beëindig actieve taken en requeue ze. De taken wordt opnieuw uitgevoerd wanneer de taak is ingeschakeld. Knooppunten verwijderen zodra taken zijn beëindigd.<br /><br /> **beëindigen** – actieve taken beëindigen. De taken worden niet opnieuw uitgevoerd. Knooppunten verwijderen zodra taken zijn beëindigd.<br /><br /> **taskcompletion** : toestaan dat actieve taken te voltooien. Er zijn geen nieuwe taken tijdens het wachten plannen. Verwijder de knooppunten wanneer alle taken zijn voltooid.<br /><br /> **Retaineddata** -Sta toe dat actieve taken uit om te voltooien en wacht vervolgens tot alle bewaarperioden om te verlopen. Er zijn geen nieuwe taken tijdens het wachten plannen. Verwijder de knooppunten wanneer alle bewaarperioden voor taken zijn verlopen.<br /><br /> De standaardwaarde is requeue.<br /><br /> Als de poolgrootte toeneemt, wordt de waarde is ingesteld op **ongeldig**.|
|currentDedicated|Int32|Het aantal rekenknooppunten momenteel toegewezen aan de groep.|
|targetDedicated|Int32|Het aantal rekenknooppunten die zijn aangevraagd voor de groep.|
|enableAutoScale|BOOL|Hiermee geeft u op of de poolgrootte automatisch wordt aangepast gedurende een bepaalde periode.|
|isAutoPool|BOOL|Hiermee geeft u op of de groep is gemaakt via een taak AutoPool mechanisme.|
|startTime|Datum/tijd|De tijd die het formaat van de groep van toepassingen is gestart.|
|Eindtijd|Datum/tijd|De tijd formaat van de groep van toepassingen is voltooid.|
|resultCode|Tekenreeks|Het resultaat van het formaat te wijzigen.|
|resultMessage|Tekenreeks|De fout formaat bevat de details van het resultaat.<br /><br /> Als het formaat is voltooid wordt aangegeven dat de bewerking is voltooid.|