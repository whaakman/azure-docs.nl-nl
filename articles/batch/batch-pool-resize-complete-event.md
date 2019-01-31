---
title: Azure Batch-pool vergroten of verkleinen van de gebeurtenis | Microsoft Docs
description: Naslaginformatie voor Batch-pool vergroten of verkleinen van de gebeurtenis.
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
ms.openlocfilehash: 87c98b89a49adbad88841dccbd4ba47d370b2be7
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474306"
---
# <a name="pool-resize-complete-event"></a>Gebeurtenis formaat pool wijzigen voltooid

 Deze gebeurtenis wordt verzonden wanneer de grootte van een groep van toepassingen is voltooid of mislukt.

 Het volgende voorbeeld ziet de hoofdtekst van een gebeurtenis formaat pool wijzigen voltooid voor een groep die vergroot en voltooid.

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
|id|String|De id van de groep.|
|nodeDeallocationOption|String|Hiermee geeft u als knooppunten kunnen worden verwijderd uit de groep als de poolgrootte afneemt.<br /><br /> Mogelijke waarden zijn:<br /><br /> **taak** : Beëindig actieve taken en plaats ze. De taken worden opnieuw uitgevoerd wanneer de taak is ingeschakeld. Knooppunten verwijderen zodra taken zijn beëindigd.<br /><br /> **beëindigen** : Beëindig actieve taken. De taken worden niet opnieuw uitgevoerd. Knooppunten verwijderen zodra taken zijn beëindigd.<br /><br /> **taskcompletion** : toestaan dat actieve taken om te voltooien. Plan geen nieuwe taken tijdens het wachten. Verwijder de knooppunten wanneer alle taken zijn voltooid.<br /><br /> **Retaineddata** -toestaan dat actieve taken uit om te voltooien en wacht vervolgens tot alle bewaarperioden om te verlopen. Plan geen nieuwe taken tijdens het wachten. Verwijder de knooppunten wanneer alle bewaarperioden voor taken zijn verlopen.<br /><br /> De standaardwaarde is de taak.<br /><br /> Als het verhogen van de grootte van de groep van toepassingen wordt de waarde is ingesteld op **ongeldig**.|
|currentDedicated|Int32|Het aantal rekenknooppunten dat momenteel is toegewezen aan de groep.|
|targetDedicated|Int32|Het aantal rekenknooppunten die zijn aangevraagd voor de pool.|
|enableAutoScale|Bool|Hiermee geeft u op of de groepsgrootte die automatisch wordt aangepast na verloop van tijd.|
|isAutoPool|Bool|Hiermee geeft u op of de groep is gemaakt via een job AutoPool mechanisme.|
|startTime|DateTime|De tijd dat de pool vergroten of verkleinen is gestart.|
|endTime|DateTime|Het tijdstip waarop de pool vergroten of verkleinen voltooid.|
|resultCode|String|Het resultaat van de grootte te wijzigen.|
|resultMessage|String|De fout formaat bevat de details van het resultaat.<br /><br /> Als de grootte te wijzigen is voltooid wordt aangegeven dat de bewerking is voltooid.|