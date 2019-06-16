---
title: StorSimple Snapshot Manager back-uptaken | Microsoft Docs
description: Beschrijft hoe u met de StorSimple Snapshot Manager MMC-module kunt weergeven en beheren van geplande, momenteel wordt uitgevoerd en voltooide back-uptaken.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: c34ff487f03d90b16b6660fbad77c3a16699e165
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64719874"
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>StorSimple Snapshot Manager gebruiken voor het weergeven en beheren van back-uptaken

## <a name="overview"></a>Overzicht
De **taken** knooppunt in de **bereik** deelvenster toont de **geplande**, **afgelopen 24 uur**, en **met** back-uptaken die u hebt gestart, interactief of door een geconfigureerde beleid. 

Deze zelfstudie wordt uitgelegd hoe u kunt de **taken** knooppunt informatie weergeven over geplande, momenteel wordt uitgevoerd en recente back-uptaken. (De lijst met taken en de bijbehorende informatie wordt weergegeven in de **resultaten** deelvenster.) Bovendien kunt u met de rechtermuisknop op een taak weergegeven en in het contextmenu met een lijst met beschikbare acties zien.

## <a name="view-scheduled-jobs"></a>Geplande taken weergeven
Gebruik de volgende procedure om geplande back-uptaken weer te geven.

#### <a name="to-view-scheduled-jobs"></a>Geplande taken weergeven
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad. 
2. In de **bereik** deelvenster, vouw de **taken** knooppunt en klik op **geplande**. De volgende informatie wordt weergegeven in de **resultaten** deelvenster:
   
   * **Naam** – de naam van de geplande momentopname
   * **Volgende uitvoering** – de datum en tijd van de volgende geplande momentopname
   * **Laatste uitvoering** – de datum en tijd van de meest recente geplande momentopname
     
     > [!NOTE]
     > Voor eenmalige alleen momentopnamen, de **volgende uitvoeren** en **laatste uitvoeren** zijn hetzelfde.
     
     ![Geplande back-uptaken](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. Als u wilt de volgende bewerkingen uitvoeren op een specifieke taak, met de rechtermuisknop op de taaknaam van de in de **resultaten** deelvenster en selecteer een van de menu-opties.

## <a name="view-recent-jobs"></a>Recente taken weergeven
Gebruik de volgende procedure om te bekijken van back-up en herstellen van taken die in de afgelopen 24 uur zijn voltooid.

#### <a name="to-view-recent-jobs"></a>Recente taken weergeven
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, vouw de **taken** knooppunt en klik op **afgelopen 24 uur**. De **resultaten** deelvenster ziet u back-uptaken voor de afgelopen 24 uur (tot een maximum van 64-taken). De volgende informatie wordt weergegeven in de **resultaten** deelvenster, afhankelijk van de **weergave** opties die u opgeeft:
   
   * **Naam** – de naam van de geplande momentopname.
   * **Aan de slag** – de datum en tijd waarop de momentopname is begonnen.
   * **Gestopt** – de datum en tijd wanneer de momentopname is voltooid of is beëindigd.
   * **Verstreken** – de hoeveelheid tijd tussen de **gestart** en **gestopt** tijden.
   * **Status** – de status van de laatst voltooide taak. **Succes** geeft aan dat de back-up is gemaakt. **Kan geen** geeft aan dat de taak niet met succes is uitgevoerd.
   * **Informatie** – de oorzaak van het probleem.
   * **Bytes verwerkt (MB)** – de hoeveelheid gegevens uit de volumegroep die is verwerkt (in MB/s). 
     
     ![Taken die zijn uitgevoerd in de afgelopen 24 uur](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. Als u wilt de volgende bewerkingen uitvoeren op een specifieke taak, met de rechtermuisknop op de taaknaam van de in de **resultaten** deelvenster en selecteer een van de menu-opties.
   
    ![Een taak verwijderen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a>Momenteel actieve taken weergeven
Gebruik de volgende procedure om de taken weergeven die momenteel worden uitgevoerd.

#### <a name="to-view-currently-running-jobs"></a>Momenteel actieve taken weergeven
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, vouw de **taken** knooppunt en klik op **met**. Afhankelijk van de **weergave** opties die u opgeeft, de volgende informatie wordt weergegeven in de **resultaten** deelvenster:
   
   * **Naam** – de naam van de geplande momentopname.
   * **Aan de slag** – de datum en tijd waarop de momentopname is begonnen.
   * **Controlepunt** : de huidige actie van de back-up.
   * **Status** – het percentage van de voltooiing.
   * **Verstreken** – de hoeveelheid tijd die is verstreken sinds het begin van de back-up. 
   * **Gemiddelde doorvoer (MB)** -verhouding van totaal aantal bytes aan gegevens verwerkt met die van de totale tijd voor het verwerken van (MB/s).
   * **Bytes verwerkt (MB)** : totaal aantal bytes aan gegevens verwerkt (in MB/s).
   * **(MB) geschreven bytes** : totaal aantal bytes van gegevens (in MB/s). Het bevat de gegevens, evenals de metagegevens en is daarom meestal groter is dan het aantal Bytes verwerkt.
     
     ![Taken die momenteel worden uitgevoerd](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. Als u wilt de volgende bewerkingen uitvoeren op een specifieke taak, met de rechtermuisknop op de taaknaam van de in de **resultaten** deelvenster en selecteer een van de menu-opties.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over het [StorSimple Snapshot Manager gebruiken voor het beheren van de back-catalogus](storsimple-snapshot-manager-manage-backup-catalog.md).

