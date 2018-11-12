---
title: Azure Backup gebruiken om uw tape-infrastructuur te vervangen
description: Informatie over hoe Azure Backup biedt tape-achtige semantiek waarmee u back-up en herstellen van gegevens in Azure
services: backup
author: trinadhk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 1/10/2017
ms.author: saurse
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 59236774f98af927082c78f4b75a1f5880a7cac4
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259596"
---
# <a name="move-your-long-term-storage-from-tape-to-the-azure-cloud"></a>Uw langdurige opslag van tape verplaatsen naar de Azure-cloud
Azure Backup en System Center Data Protection Manager-klanten kunnen:

* Back-up van gegevens in de schema's die het best past bij de behoeften van de organisatie.
* De back-upgegevens behouden voor langere tijd
* Controleer Azure een deel van hun langetermijnretentie (in plaats van tape moet).

In dit artikel wordt uitgelegd hoe klanten een back-up-en bewaarbeleid kunnen inschakelen. Klanten die gebruikmaken van tapes voor de lange-termijn-bewaarperiode moeten beschikt nu over een krachtige en levensvatbaar alternatief met de beschikbaarheid van deze functie. De functie is ingeschakeld in de nieuwste versie van de Azure Backup (die beschikbaar is [hier](https://aka.ms/azurebackup_agent)). System Center DPM moeten klanten bijwerken naar, ten minste DPM 2012 R2 UR5 voordat u DPM met de Azure Backup-service.

## <a name="what-is-the-backup-schedule"></a>Wat is de back-upschema?
De back-upschema geeft aan dat de frequentie van de back-upbewerking. Bijvoorbeeld, de instellingen in het volgende scherm geven aan dat de back-ups dagelijks om 18: 00 uur en om middernacht worden genomen.

![Dagelijks schema](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

Klanten kunnen ook een wekelijkse back-up plannen. Bijvoorbeeld, de instellingen in het volgende scherm geven aan dat back-ups elke alternatieve zondag & woensdag worden genomen om 9:30 uur en 1:00 uur.

![Wekelijks schema](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-the-retention-policy"></a>Wat is het bewaarbeleid?
Het bewaarbeleid geeft de periode waarvoor de back-up moet worden opgeslagen. In plaats van alleen 'plat beleid' voor alle back-uppunten opgeeft, kunnen klanten opgeven ander retentiebeleid op basis van wanneer de back-up is gemaakt. De back-uppunt worden genomen, dagelijks, dat als een operationele herstelpunt fungeert, is bijvoorbeeld 90 dagen bewaard. De back-uppunt genomen aan het einde van elk kwartaal voor auditdoeleinden blijft behouden gedurende een langere periode.

![Bewaarbeleid](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

Het totale aantal 'bewaarpunten' opgegeven in dit beleid is 90 (punten per dag) + 40 (een elk kwartaal 10 jaar) = 130.

## <a name="example--putting-both-together"></a>Voorbeeld: het samenstellen van beide
![Voorbeeldscherm](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **Dagelijks bewaarbeleid**: back-ups dagelijks die zijn opgeslagen voor de zeven dagen.
2. **Wekelijks bewaarbeleid**: back-ups die elke dag om middernacht en 18: 00 uur zaterdag bewaard voor vier weken
3. **Maandelijks bewaarbeleid**: back-ups genomen om middernacht en 18: 00 uur op de laatste zaterdag van elke maand worden bewaard gedurende 12 maanden
4. **Jaarlijks bewaarbeleid**: back-ups die op de laatste zaterdag van elke maart om middernacht worden bewaard gedurende tien jaar

Het totale aantal 'bewaarpunten' (punt van waaruit een klant kunt gegevens herstellen) in het voorgaande diagram wordt berekend als volgt:

* twee punten herstelpunten per dag voor zeven dagen = 14
* twee punten per week voor vier weken = 8 herstelpunten
* twee punten herstelpunten per maand gedurende 12 maanden = 24
* een punt per jaar per 10 jaar = 10 recovery verwijst

Het totale aantal herstelpunten is 56.

> [!NOTE]
> Azure backup geen beperking op aantal herstelpunten.
>
>

## <a name="advanced-configuration"></a>Geavanceerde configuratie
Door te klikken op **wijzigen** in het vorige scherm, hebben klanten meer flexibiliteit bij het bewaarschema op te geven.

![Wijzigen](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure Backup:

* [Kennismaking met Azure Backup](backup-introduction-to-azure-backup.md)
* [Probeer Azure Backup](backup-try-azure-backup-in-10-mins.md)
