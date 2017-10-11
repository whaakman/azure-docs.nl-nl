---
title: Azure Backup gebruiken ter vervanging van uw tape-infrastructuur | Microsoft Docs
description: Meer informatie over hoe Azure Backup biedt tape-achtige-semantiek waardoor u back-up en herstellen van gegevens in Azure
services: backup
documentationcenter: 
author: trinadhk
manager: vijayts
editor: 
ms.assetid: 2e1bb67d-986c-4437-8056-3a63169b4214
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 1/10/2017
ms.author: saurse;trinadhk;markgal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0f3152daf5f91f7c9e540797bf09b21969d2d33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="move-your-long-term-storage-from-tape-to-the-azure-cloud"></a>Uw langetermijnopslag van tape verplaatsen naar de Azure-cloud
Azure back-up- en System Center Data Protection Manager-klanten kunnen:

* Back-up van gegevens in de schema's die het beste behoeften van de organisatie.
* De back-upgegevens voor langere tijd behouden
* Controleer Azure een deel van hun lange bewaartermijn (in plaats van tape moet).

Dit artikel wordt uitgelegd hoe klanten back-up en retentie beleidsregels kunnen inschakelen. Klanten die tapes gebruiken voor het oplossen van hun lange-termijn-bewaren moeten nu een krachtige en levensvatbaar alternatief met de beschikbaarheid van deze functie hebben. De functie is ingeschakeld in de nieuwste versie van de Azure Backup (die beschikbaar is [hier](http://aka.ms/azurebackup_agent)). Klanten van System Center DPM moeten ten minste bijwerken, DPM 2012 R2 UR5 voordat u DPM met de Azure Backup-service.

## <a name="what-is-the-backup-schedule"></a>Wat is de back-upschema?
Het back-upschema geeft de frequentie van de back-upbewerking. De instellingen in het volgende scherm bijvoorbeeld aangeven dat de back-ups dagelijks om 18: 00 uur en om middernacht worden genomen.

![Dagelijks schema](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

Klanten kunnen ook een wekelijkse back-up plannen. De instellingen in het volgende scherm geven bijvoorbeeld back-ups elke alternatieve zondag & woensdag op 9:30:00 uur en 1:00 uur zijn gemaakt.

![Wekelijkse planning](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-the-retention-policy"></a>Wat is het bewaarbeleid?
Het bewaarbeleid geeft de duur waarvoor de back-up moet worden opgeslagen. In plaats van alleen 'plat beleid' voor alle back-uppunten opgeeft, kunnen klanten verschillende Bewaarbeleidsregels op basis van wanneer de back-up is gemaakt opgeven. Bijvoorbeeld blijft dagelijks, uitgevoerde back-uppunt die als een operationele herstelpunt maakt fungeert, behouden gedurende 90 dagen. Het back-punt aan het einde van elk kwartaal voor auditdoeleinden genomen blijft behouden gedurende een langere periode.

![Bewaarbeleid](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

Het totale aantal 'bewaarpunten' opgegeven in dit beleid is 90 (punten per dag) + 40 (elk kwartaal tien jaar) = 130.

## <a name="example--putting-both-together"></a>Voorbeeld: het samenstellen van beide
![Voorbeeldscherm](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **Dagelijkse bewaarbeleid**: back-ups dagelijks die zijn opgeslagen voor de zeven dagen.
2. **Wekelijkse bewaarbeleid**: back-ups die elke dag om middernacht en 18: 00 uur zaterdag bewaard voor vier weken
3. **Maandelijkse bewaarbeleid**: back-ups die op middernacht en 18: 00 uur op de laatste zaterdag van elke maand worden bewaard gedurende 12 maanden
4. **Jaarlijks bewaarbeleid**: back-ups die op de laatste zaterdag van elke maart om middernacht tien jaar worden bewaard

Het totale aantal 'bewaarpunten' (punten waaruit een klant kunt gegevens herstellen) in het voorgaande diagram wordt berekend als volgt:

* twee beheerpunten herstelpunten per dag voor de zeven dagen = 14
* twee beheerpunten per week voor vier weken = 8 herstelpunten
* twee beheerpunten per maand gedurende 12 maanden = 24 herstelpunten
* één punt per jaar per 10 jaar = 10 herstel verwijst

Het totale aantal herstelpunten is 56.

> [!NOTE]
> Azure backup heeft geen beperking op het aantal herstelpunten.
>
>

## <a name="advanced-configuration"></a>Geavanceerde configuratie
Door te klikken op **wijzigen** in het vorige scherm klanten hebben meer flexibiliteit bij het bewaarschema opgeven.

![Wijzigen](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure Backup:

* [Kennismaking met Azure Backup](backup-introduction-to-azure-backup.md)
* [Azure Backup proberen](backup-try-azure-backup-in-10-mins.md)
