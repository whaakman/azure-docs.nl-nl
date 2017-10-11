---
title: Back-upcatalogus StorSimple Snapshot Manager | Microsoft Docs
description: Beschrijft hoe u het StorSimple Snapshot Manager MMC-module weergeven en beheren van de back-catalogus.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 6abdbfd2-22ce-45a5-aa15-38fae4c8f4ec
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: b97753e6f1b67e3c8d247281c5e5208033a56eca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Gebruik StorSimple Snapshot Manager voor het beheren van de back-upcatalogus

## <a name="overview"></a>Overzicht
De primaire functie van StorSimple Snapshot Manager is dat u toepassingsconsistente back-ups van StorSimple-volumes in de vorm van momentopnamen maken. Momentopnamen worden vermeld in een XML-bestand met de naam een *back-upcatalogus*. De back-upcatalogus ordent momentopnamen op volume groep en vervolgens op de lokale momentopname of cloudmomentopname van de.

Deze zelfstudie wordt beschreven hoe u kunt de **back-upcatalogus** knooppunt dat u wilt de volgende taken uitvoeren:

* Een volume herstellen
* Klonen van een volume of een volume-groep
* Verwijderen van een back-up
* Herstellen van een bestand
* De Storsimple Snapshot Manager-database herstellen

U kunt de back-upcatalogus weergeven door het uitbreiden van de **back-upcatalogus** knooppunt in de **bereik** deelvenster en vervolgens de groep volume uitbreiden.

* Als u de naam van het volume, klikt u op de **resultaten** deelvenster toont het aantal lokale momentopnamen en cloudmomentopnamen die beschikbaar zijn voor de groep volume. 
* Als u op **lokale momentopname** of **Cloudmomentopname**, wordt de **resultaten** deelvenster ziet u de volgende informatie over elke back-upmomentopname (afhankelijk van uw  **Weergeven** instellingen):
  
  * **Naam** – de tijd dat de momentopname werd gemaakt.
  * **Type** – of dit een momentopname van een lokale of een cloudmomentopname van de is.
  * **Eigenaar** – eigenaar van de inhoud. 
  * **Beschikbare** – of momentopname van de momenteel beschikbaar is. **De waarde True** geeft aan dat de momentopname beschikbaar is en kan worden hersteld; **False** geeft aan dat de momentopname niet langer beschikbaar. 
  * **Geïmporteerd** – of de back-up is geïmporteerd. **De waarde True** geeft aan dat de back-up is geïmporteerd uit de StorSimple-apparaat Manager-service op het moment dat het apparaat is geconfigureerd in StorSimple Snapshot Manager; **False** geeft aan dat het is niet geïmporteerd, maar door StorSimple Snapshot Manager is gemaakt. (U eenvoudig kunt herkennen een groep geïmporteerde volume omdat een achtervoegsel wordt toegevoegd die aangeeft dat het apparaat waarvan de groep volume is geïmporteerd.)
    
    ![Back-upcatalogus](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)
* Als u wilt uitbreiden **lokale momentopname** of **Cloudmomentopname**, en klik vervolgens op een afzonderlijke momentopname-naam, de **resultaten** deelvenster ziet u de volgende informatie over de momentopname die u hebt geselecteerd:
  
  * **Naam** – het volume dat wordt geïdentificeerd met een stationsletter. 
  * **Lokale naam** : de lokale naam van het station (indien beschikbaar). 
  * **Apparaat** : de naam van het apparaat waarop het volume zich bevindt. 
  * **Beschikbare** – of momentopname van de momenteel beschikbaar is. **De waarde True** geeft aan dat de momentopname beschikbaar is en kan worden hersteld; **False** geeft aan dat de momentopname niet langer beschikbaar. 

## <a name="restore-a-volume"></a>Een volume herstellen
Gebruik de volgende procedure een volume van de back-up herstellen.

#### <a name="prerequisites"></a>Vereisten
Als u dit nog niet hebt gedaan, maakt u een volume en volume-groep en verwijder vervolgens het volume. Standaard StorSimple Snapshot Manager back-ups van een volume voordat zodat het kan worden verwijderd. Deze voorzorgsmaatregel kan geen gegevens verloren gaan als het volume per ongeluk is verwijderd of als de gegevens moet worden hersteld om welke reden. 

StorSimple Snapshot Manager wordt het volgende bericht weergegeven bij het maken van de voorzorgsmaatregelen back-up.

![Automatische momentopname bericht](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

> [!IMPORTANT]
> U kunt een volume dat deel uitmaakt van een volume-groep niet verwijderen. De verwijderoptie is niet beschikbaar. <br>
> 
> 

#### <a name="to-restore-a-volume"></a>Een volume herstellen
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager. 
2. In de **bereik** deelvenster, vouw de **back-upcatalogus** knooppunt, een volume uitvouwen en klik vervolgens op **lokale momentopnamen** of **Cloudmomentopnamen**. Een lijst met back-upmomentopnamen wordt weergegeven in de **resultaten** deelvenster.
3. Zoeken naar de back-up die u herstellen wilt, klik met de rechtermuisknop en klik vervolgens op **herstellen**.
   
    ![Terugzetten van back-upcatalogus](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 
4. Controleer de details op de bevestigingspagina type **bevestigen**, en klik vervolgens op **OK**. StorSimple Snapshot Manager maakt gebruik van de back-up te herstellen van het volume.
   
    ![Bevestigingsbericht herstellen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 
5. Als dit wordt uitgevoerd, kunt u de herstelbewerking bewaken. In de **bereik** deelvenster, vouw de **taken** knooppunt en klik vervolgens op **met**. De taakgegevens worden weergegeven in de **resultaten** deelvenster. Wanneer de hersteltaak is voltooid, de taakdetails worden overgebracht naar de **afgelopen 24 uur** lijst.

## <a name="clone-a-volume-or-volume-group"></a>Klonen van een volume of een volume-groep
Gebruik de volgende procedure een kopie (klonen) van een volume of de volume-groep te maken.

#### <a name="to-clone-a-volume-or-volume-group"></a>Voor het klonen van een volume of een volume-groep
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, vouw de **back-upcatalogus** knooppunt, een volume uitvouwen en klik vervolgens op **Cloudmomentopnamen**. Een lijst van back-ups wordt weergegeven in de **resultaten** deelvenster.
3. Het volume of de volume-groep die u wilt klonen, met de rechtermuisknop op het volume of de groepsnaam volume en klik op Zoeken **kloon**. De **Cloudmomentopname kloon** dialoogvenster wordt weergegeven.
   
    ![Klonen van een cloudmomentopname](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Voltooi de **kloon Cloudmomentopname** in het dialoogvenster als volgt: 
   
   1. In de **naam** in het tekstvak, typ een naam voor de gekloonde volume. Deze naam wordt weergegeven in de **Volumes** knooppunt. 
   2. (Optioneel) Selecteer **station**, en selecteer vervolgens een stationsletter in de vervolgkeuzelijst.
   3. (Optioneel) Selecteer **map (NTFS)**, en typt u een pad naar de map of klik op Bladeren en selecteer een locatie voor de map. 
   4. Klik op **Create**.
5. Wanneer het proces voor het klonen is voltooid, moet u het gekloonde volume initialiseren. Start Serverbeheer en start Schijfbeheer. Zie voor gedetailleerde instructies [volumes koppelen](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Nadat deze is geïnitialiseerd, het volume wordt vermeld onder de **Volumes** knooppunt in de **bereik** deelvenster. Als u het volume dat wordt vermeld niet ziet, vernieuwt u de lijst van volumes (met de rechtermuisknop op de **Volumes** knooppunt en klik vervolgens op **vernieuwen**).

## <a name="delete-a-backup"></a>Verwijderen van een back-up
Gebruik de volgende procedure een momentopname van de back-upcatalogus verwijderen. 

> [!NOTE]
> Een momentopname verwijdert, wordt de back-ups van gegevens die zijn gekoppeld aan de momentopname. Het proces van het opschonen van gegevens vanuit de cloud kan echter enige tijd duren.<br>


#### <a name="to-delete-a-backup"></a>Verwijderen van een back-up
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, vouw de **back-upcatalogus** knooppunt, een volume uitvouwen en klik vervolgens op **lokale momentopnamen** of **Cloudmomentopnamen**. Een lijst van momentopnamen wordt weergegeven in de **resultaten** deelvenster.
3. Met de rechtermuisknop op de momentopname die u wilt verwijderen en klik vervolgens op **verwijderen**.
4. Wanneer het bevestigingsbericht wordt weergegeven, klikt u op **OK**.

## <a name="recover-a-file"></a>Herstellen van een bestand
Als een bestand per ongeluk worden verwijderd van een volume, kunt u het bestand herstellen door bij het ophalen van een momentopname die vóór de verwijdering datums, met behulp van de momentopname maken van een kloon van het volume en vervolgens het bestand uit de gekloonde volume te kopiëren naar het oorspronkelijke volume.

#### <a name="prerequisites"></a>Vereisten
Voordat u begint, zorg ervoor dat er een recente back-up van de volume-groep. Verwijder het bestand opgeslagen op een van de volumes in de groep volume. Gebruik tot slot de volgende stappen uit het bestand is verwijderd uit uw back-up hersteld. 

#### <a name="to-recover-a-deleted-file"></a>Voor het herstellen van een bestand is verwijderd
1. Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad. Het StorSimple Snapshot Manager consolevenster wordt weergegeven. 
2. In de **bereik** deelvenster, vouw de **back-upcatalogus** knooppunt en blader naar een momentopname met het verwijderde bestand. Normaal gesproken moet u een momentopname die net vóór de verwijdering is gemaakt.
3. Het volume dat u klonen wilt, vinden met de rechtermuisknop en klik op **kloon**. De **Cloudmomentopname kloon** dialoogvenster wordt weergegeven.
   
    ![Klonen van een cloudmomentopname](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Voltooi de **kloon Cloudmomentopname** in het dialoogvenster als volgt: 
   
   1. In de **naam** in het tekstvak, typ een naam voor de gekloonde volume. Deze naam wordt weergegeven in de **Volumes** knooppunt. 
   2. (Optioneel) Selecteer **station**, en selecteer vervolgens een stationsletter in de vervolgkeuzelijst. 
   3. (Optioneel) Selecteer **map (NTFS)**, en typ het pad naar een map of klik op **Bladeren** en selecteer een locatie voor de map. 
   4. Klik op **Create**. 
5. Wanneer het proces voor het klonen is voltooid, moet u het gekloonde volume initialiseren. Start Serverbeheer en start Schijfbeheer. Zie voor gedetailleerde instructies [volumes koppelen](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Nadat deze is geïnitialiseerd, het volume wordt vermeld onder de **Volumes** knooppunt in de **bereik** deelvenster. 
   
    Als u het volume dat wordt vermeld niet ziet, vernieuwt u de lijst van volumes (met de rechtermuisknop op de **Volumes** knooppunt en klik vervolgens op **vernieuwen**).
6. Open de NTFS-map met het gekloonde volume, vouw de **Volumes** knooppunt en open vervolgens het gekloonde volume. Zoek het bestand dat u wilt herstellen en kopieer het naar de primaire volume.
7. Nadat u het bestand te herstellen, kunt u de NTFS-map met het gekloonde volume verwijderen.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>De StorSimple Snapshot Manager-database herstellen
U moet regelmatig back-up van de StorSimple Snapshot Manager-database op de hostcomputer. Als een noodsituatie voordoet of de hostcomputer voor een of andere reden mislukt, kunt u het herstellen van de back-up. Maken van de databaseback-up is een handmatig proces.

#### <a name="to-back-up-and-restore-the-database"></a>Back-up en herstellen van de database
1. De Microsoft StorSimple Management-Service stoppen:
   
   1. Start Serverbeheer.
   2. Op het dashboard Serverbeheer op de **extra** selecteert u **Services**.
   3. Op de **Services** Selecteer de **Microsoft StorSimple Management Service**.
   4. In het rechterdeelvenster onder **Microsoft StorSimple Management Service**, klikt u op **Stop de service**.
2. Blader naar C:\ProgramData\Microsoft\StorSimple\BACatalog op de hostcomputer. 
   
   > [!NOTE]
   > ProgramData is een verborgen map.
   > 
   > 
3. Het catalogus XML-bestand niet vinden, Kopieer het bestand en de kopie opslaan op een veilige locatie of in de cloud. Als de host is mislukt, kunt u deze back-upbestand voor het herstellen van de back-upbeleid dat u hebt gemaakt in StorSimple Snapshot Manager.
   
    ![Azure StorSimple-bestand voor back-upcatalogus](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)
4. De Microsoft StorSimple Management-Service opnieuw starten: 
   
   1. Op het dashboard Serverbeheer op de **extra** selecteert u **Services**.
   2. Op de **Services** Selecteer de **Microsoft StorSimple Management Service**.
   3. In het rechterdeelvenster onder **Microsoft StorSimple Management Service**, klikt u op **Start de service opnieuw**.
5. Blader naar C:\ProgramData\Microsoft\StorSimple\BACatalog op de hostcomputer. 
6. Verwijder het bestand van de XML-catalogus en vervang deze door de back-up die u hebt gemaakt. 
7. Klik op het bureaubladpictogram StorSimple Snapshot Manager voor het starten van StorSimple Snapshot Manager. 

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple Snapshot Manager gebruiken voor het beheer van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over [StorSimple Snapshot Manager-taken en werkstromen](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).

