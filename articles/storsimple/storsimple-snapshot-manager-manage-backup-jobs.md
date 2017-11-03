---
title: Back-uptaken StorSimple Snapshot Manager | Microsoft Docs
description: Beschrijft hoe u het StorSimple Snapshot Manager MMC-module weergeven en beheren van back-uptaken voor geplande, voltooide en die momenteel worden uitgevoerd.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 03e306b62250f2bb033cc14e856a59760b5406c3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>Gebruik StorSimple Snapshot Manager weergeven en beheren van back-uptaken

## <a name="overview"></a>Overzicht
De **taken** knooppunt in de **bereik** deelvenster toont de **geplande**, **afgelopen 24 uur**, en **met** back-up van taken die u hebt gestart, interactief of door een geconfigureerde beleid. 

Deze zelfstudie wordt uitgelegd hoe u kunt de **taken** knooppunt om weer te geven informatie over back-uptaken voor geplande, recente en die momenteel worden uitgevoerd. (De lijst met taken en de bijbehorende informatie wordt weergegeven in de **resultaten** deelvenster.) Bovendien kunt u met de rechtermuisknop op een vermelde taak en contextmenu met een lijst met beschikbare acties weergegeven.

## <a name="view-scheduled-jobs"></a>Geplande taken weergeven
Gebruik de volgende procedure om geplande back-uptaken weer te geven.

#### <a name="to-view-scheduled-jobs"></a>Geplande taken weergeven
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager. 
2. In de **bereik** deelvenster, vouw de **taken** knooppunt en klik op **geplande**. De volgende informatie wordt weergegeven in de **resultaten** deelvenster:
   
   * **Naam** : de naam van de geplande momentopname
   * **Voer vervolgens** – de datum en tijd van de volgende geplande momentopname
   * **Laatste uitvoering** – de datum en tijd van de meest recente geplande momentopname
     
     > [!NOTE]
     > Voor eenmalige alleen momentopnamen, de **volgende uitvoeren** en **laatste start** hetzelfde.
     
     ![Geplande back-uptaken](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. Als u wilt de volgende bewerkingen uitvoeren op een bepaalde taak, met de rechtermuisknop op de taaknaam van de in de **resultaten** en selecteer een van de opties in het venster.

## <a name="view-recent-jobs"></a>Recente taken weergeven
Gebruik de volgende procedure om te bekijken van de back-up en herstellen van taken die in de afgelopen 24 uur zijn voltooid.

#### <a name="to-view-recent-jobs"></a>Recente taken weergeven
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, vouw de **taken** knooppunt en klik op **afgelopen 24 uur**. De **resultaten** deelvenster ziet u back-uptaken voor de afgelopen 24 uur (met een maximum van 64-taken). De volgende informatie wordt weergegeven in de **resultaten** deelvenster, afhankelijk van de **weergave** opties die u opgeeft:
   
   * **Naam** : de naam van de geplande momentopname.
   * **Gestart** – de datum en tijd waarop de momentopname is begonnen.
   * **Gestopt** – de datum en tijd wanneer de momentopname is voltooid of is beëindigd.
   * **Verstreken** – de hoeveelheid tijd tussen de **gestart** en **gestopt** tijden.
   * **Status** – de status van de laatst voltooide taak. **Geslaagde** geeft aan dat de back-up is gemaakt. **Kan geen** geeft aan dat de taak niet met succes is uitgevoerd.
   * **Informatie** – de oorzaak van het probleem.
   * **Bytes verwerkt (MB)** – de hoeveelheid gegevens uit de volume-groep die is verwerkt (in MB). 
     
     ![Taken die zijn uitgevoerd in de afgelopen 24 uur](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. Als u wilt de volgende bewerkingen uitvoeren op een bepaalde taak, met de rechtermuisknop op de taaknaam van de in de **resultaten** en selecteer een van de opties in het venster.
   
    ![Een taak verwijderen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a>Actieve taken weergeven
Gebruik de volgende procedure om de taken weergeven die momenteel worden uitgevoerd.

#### <a name="to-view-currently-running-jobs"></a>Actieve taken weergeven
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, vouw de **taken** knooppunt en klik op **met**. Afhankelijk van de **weergave** opties die u opgeeft, de volgende informatie wordt weergegeven in de **resultaten** deelvenster:
   
   * **Naam** : de naam van de geplande momentopname.
   * **Gestart** – de datum en tijd waarop de momentopname is begonnen.
   * **Controlepunt** : de huidige actie van de back-up.
   * **Status** – het percentage van de voltooiing.
   * **Verstreken** – de hoeveelheid tijd is verstreken sinds het begin van de back-up. 
   * **Gemiddelde doorvoer (MB)** – verhouding van totaal aantal bytes van gegevens die worden verwerkt met die van de totale tijd voor het verwerken van (MB).
   * **Bytes verwerkt (MB)** : totaal aantal bytes van gegevens die zijn verwerkt (in MB).
   * **Bytes geschreven (MB)** : totaal aantal bytes van gegevens die zijn geschreven (in MB). Het bevat de gegevens, evenals de metagegevens en is daarom meestal groter is dan de Bytes verwerkt.
     
     ![Taken worden uitgevoerd](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. Als u wilt de volgende bewerkingen uitvoeren op een bepaalde taak, met de rechtermuisknop op de taaknaam van de in de **resultaten** en selecteer een van de opties in het venster.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over hoe [StorSimple Snapshot Manager gebruiken voor het beheren van de back-upcatalogus](storsimple-snapshot-manager-manage-backup-catalog.md).

