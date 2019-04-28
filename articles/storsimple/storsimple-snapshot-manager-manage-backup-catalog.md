---
title: StorSimple Snapshot Manager back-catalogus | Microsoft Docs
description: Beschrijft hoe u met de StorSimple Snapshot Manager MMC-module kunt weergeven en beheren van de back-catalogus.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: 6abdbfd2-22ce-45a5-aa15-38fae4c8f4ec
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: dc24ebd59fd977ef35766c304aec5824e2c7bb4c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127179"
---
# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Gebruik StorSimple Snapshot Manager voor het beheren van de back-catalogus

## <a name="overview"></a>Overzicht
Er is de primaire functie van de StorSimple Snapshot Manager kunt u toepassingsconsistente back-ups van StorSimple-volumes in de vorm van momentopnamen maken. Momentopnamen worden vermeld in een XML-bestand met de naam een *back-catalogus*. De back-catalogus organiseert momentopnamen per volumegroep en vervolgens op de lokale momentopname of cloud-momentopname.

Deze zelfstudie wordt beschreven hoe u kunt de **back-upcatalogus** knooppunt de volgende taken voltooien:

* Een volume herstellen
* Klonen van een volume of een volumegroep
* Een back-up verwijderen
* Een bestand herstellen
* De Storsimple Snapshot Manager-database herstellen

U kunt de back-catalogus weergeven door het uitbreiden van de **back-catalogus** knooppunt in de **bereik** deelvenster en vervolgens de volumegroep uitbreiden.

* Als u de naam van het volume, klikt u op de **resultaten** deelvenster ziet u het nummer van de lokale momentopnamen en cloudmomentopnamen die beschikbaar zijn voor de volumegroep. 
* Als u klikt op **lokale momentopname** of **Cloudmomentopname**, wordt de **resultaten** deelvenster ziet u de volgende informatie over elke back-upmomentopname (afhankelijk van uw  **Bekijk** instellingen):
  
  * **Naam** : de tijd die de momentopname werd gemaakt.
  * **Type** : bepaalt of dit een momentopname van een lokale of een cloudmomentopname van de is.
  * **De eigenaar van** – eigenaar van de inhoud. 
  * **Beschikbare** : bepaalt of de momentopname momenteel beschikbaar is is. **De waarde True** geeft aan dat de momentopname beschikbaar is en kan worden hersteld; **False** geeft aan dat de momentopname niet meer beschikbaar is. 
  * **Geïmporteerd** : bepaalt of de back-up is geïmporteerd. **De waarde True** geeft aan dat de back-up is geïmporteerd uit de StorSimple Device Manager-service op het moment dat het apparaat is geconfigureerd in StorSimple Snapshot Manager; **False** geeft aan dat deze niet is geïmporteerd, maar is gemaakt door StorSimple Snapshot Manager. (U kunt eenvoudig vaststellen een geïmporteerde volumegroep omdat een achtervoegsel wordt toegevoegd die aangeeft dat het apparaat van waaruit de volumegroep is geïmporteerd.)
    
    ![back-catalogus](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)
* Als u wilt uitbreiden **lokale momentopname** of **Cloudmomentopname**, en klik vervolgens op de naam van een afzonderlijke momentopname, de **resultaten** deelvenster ziet u de volgende informatie over de momentopname u hebt geselecteerd:
  
  * **Naam** : het volume dat is geïdentificeerd door de stationsletter. 
  * **Lokale naam** : de lokale naam van het station (indien beschikbaar). 
  * **Apparaat** : de naam van het apparaat waarop het volume zich bevindt. 
  * **Beschikbare** : bepaalt of de momentopname momenteel beschikbaar is is. **De waarde True** geeft aan dat de momentopname beschikbaar is en kan worden hersteld; **False** geeft aan dat de momentopname niet meer beschikbaar is. 

## <a name="restore-a-volume"></a>Een volume herstellen
Gebruik de volgende procedure een volume herstellen vanuit back-up.

#### <a name="prerequisites"></a>Vereisten
Als u hebt nog niet gedaan, maken van een volume en de volumegroep en verwijder vervolgens het volume. Standaard StorSimple Snapshot Manager back-ups van een volume voordat zodat het kan worden verwijderd. Deze voorzorgsmaatregel kan geen gegevens verloren gaan als het volume per ongeluk is verwijderd of als de gegevens moet worden hersteld voor een bepaalde reden. 

StorSimple Snapshot Manager het volgende bericht wordt weergegeven bij het maken van de voorzorgsmaatregelen back-up.

![Automatische momentopnamen bericht](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

> [!IMPORTANT]
> U kunt een volume dat deel uitmaakt van een volumegroep niet verwijderen. De optie voor het verwijderen is niet beschikbaar. <br>
> 
> 

#### <a name="to-restore-a-volume"></a>Een volume herstellen
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad. 
2. In de **bereik** deelvenster, vouw de **back-upcatalogus** knooppunt, een volumegroep uitvouwen, en klik vervolgens op **lokale momentopnamen** of **Cloudmomentopnamen**. Een lijst van de Backup-momentopnamen wordt weergegeven in de **resultaten** deelvenster.
3. De back-up die u herstellen wilt, vinden met de rechtermuisknop en klik vervolgens op **herstellen**.
   
    ![Back-catalogus herstellen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 
4. Bekijk de details op de bevestigingspagina type **bevestigen**, en klik vervolgens op **OK**. StorSimple Snapshot Manager maakt gebruik van de back-up te herstellen van het volume.
   
    ![Bevestigingsbericht herstellen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 
5. U kunt de herstelbewerking bewaken terwijl deze wordt uitgevoerd. In de **bereik** deelvenster, vouw de **taken** knooppunt en klik vervolgens op **met**. De taakdetails worden weergegeven in de **resultaten** deelvenster. Wanneer de hersteltaak is voltooid, de taakdetails worden overgebracht naar de **afgelopen 24 uur** lijst.

## <a name="clone-a-volume-or-volume-group"></a>Klonen van een volume of een volumegroep
Gebruik de volgende procedure een kopie (gekloond) van een volume of een volumegroep te maken.

#### <a name="to-clone-a-volume-or-volume-group"></a>Voor het klonen van een volume of een volumegroep
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, vouw de **back-upcatalogus** knooppunt, een volumegroep uitvouwen, en klik vervolgens op **Cloudmomentopnamen**. Een lijst met back-ups wordt weergegeven in de **resultaten** deelvenster.
3. Het volume of de volumegroep die u wilt klonen, met de rechtermuisknop op het volume of de naam van de volume-groep en klikt u op zoek **kloon**. De **kloon Cloud-momentopname** in het dialoogvenster wordt weergegeven.
   
    ![Klonen van een cloud-momentopname](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Voltooi de **kloon Cloud-momentopname** in het dialoogvenster als volgt te werk: 
   
   1. In de **naam** in het tekstvak, typ een naam voor de gekloonde volume. Deze naam wordt weergegeven in de **Volumes** knooppunt. 
   2. (Optioneel) Selecteer **station**, en selecteer vervolgens een stationsletter in de vervolgkeuzelijst.
   3. (Optioneel) Selecteer **map (NTFS)**, en typt u een pad of klik op Bladeren en selecteer een locatie voor de map. 
   4. Klik op **Create**.
5. Wanneer het proces voor het klonen is voltooid, moet u de gekloonde volume initialiseren. Start Serverbeheer en start vervolgens Schijfbeheer. Zie voor gedetailleerde instructies [volumes koppelen](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Nadat deze is geïnitialiseerd, het volume worden vermeld in de **Volumes** knooppunt in de **bereik** deelvenster. Als u het volume dat wordt vermeld niet ziet, vernieuwt u de lijst van volumes (met de rechtermuisknop op de **Volumes** knooppunt en klik vervolgens op **vernieuwen**).

## <a name="delete-a-backup"></a>Een back-up verwijderen
Gebruik de volgende procedure voor het verwijderen van een momentopname van de back-catalogus. 

> [!NOTE]
> Een momentopname verwijdert, wordt de back-ups van gegevens die zijn gekoppeld aan de momentopname. Het proces van het opschonen van gegevens vanuit de cloud kan echter enige tijd duren.<br>


#### <a name="to-delete-a-backup"></a>Een back-up verwijderen
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, vouw de **back-upcatalogus** knooppunt, een volumegroep uitvouwen, en klik vervolgens op **lokale momentopnamen** of **Cloudmomentopnamen**. Een lijst met momentopnamen wordt weergegeven in de **resultaten** deelvenster.
3. Met de rechtermuisknop op de momentopname die u wilt verwijderen, en klik vervolgens op **verwijderen**.
4. Wanneer het bevestigingsbericht wordt weergegeven, klikt u op **OK**.

## <a name="recover-a-file"></a>Een bestand herstellen
Als een bestand per ongeluk is verwijderd van een volume, kunt u het bestand door bij het ophalen van een momentopname die vóór de verwijdering datums, met behulp van de momentopname te maken van een kloon van het volume en vervolgens het bestand van de gekloonde volume te kopiëren naar het oorspronkelijke volume herstellen.

#### <a name="prerequisites"></a>Vereisten
Voordat u begint, zorg ervoor dat u een recente back-up van de volumegroep hebt. Verwijder vervolgens een bestand dat is opgeslagen op een van de volumes in die volumegroep. Gebruik tot slot de volgende stappen uit het verwijderde bestand herstellen vanuit uw back-up. 

#### <a name="to-recover-a-deleted-file"></a>Om te herstellen van een bestand is verwijderd
1. Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad. Het StorSimple Snapshot Manager consolevenster wordt weergegeven. 
2. In de **bereik** deelvenster, vouw de **back-upcatalogus** knooppunt en blader naar een momentopname die het verwijderde bestand bevat. Normaal gesproken moet u een momentopname die net vóór de verwijdering is gemaakt.
3. Het volume dat u klonen wilt, niet vinden met de rechtermuisknop en klikt u op **kloon**. De **kloon Cloud-momentopname** in het dialoogvenster wordt weergegeven.
   
    ![Klonen van een cloud-momentopname](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Voltooi de **kloon Cloud-momentopname** in het dialoogvenster als volgt te werk: 
   
   1. In de **naam** in het tekstvak, typ een naam voor de gekloonde volume. Deze naam wordt weergegeven in de **Volumes** knooppunt. 
   2. (Optioneel) Selecteer **station**, en selecteer vervolgens een stationsletter in de vervolgkeuzelijst. 
   3. (Optioneel) Selecteer **map (NTFS)**, en typ het pad naar een map of klik op **Bladeren** en selecteer een locatie voor de map. 
   4. Klik op **Create**. 
5. Wanneer het proces voor het klonen is voltooid, moet u de gekloonde volume initialiseren. Start Serverbeheer en start vervolgens Schijfbeheer. Zie voor gedetailleerde instructies [volumes koppelen](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Nadat deze is geïnitialiseerd, het volume worden vermeld in de **Volumes** knooppunt in de **bereik** deelvenster. 
   
    Als u het volume dat wordt vermeld niet ziet, vernieuwt u de lijst van volumes (met de rechtermuisknop op de **Volumes** knooppunt en klik vervolgens op **vernieuwen**).
6. Open de NTFS-map met de gekloonde volume uit, vouw de **Volumes** knooppunt en open vervolgens het gekloonde volume. Zoek het bestand dat u wilt herstellen en kopieer het naar het primaire volume.
7. Nadat u het bestand hebt hersteld, kunt u de NTFS-map met de gekloonde volume verwijderen.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>De StorSimple Snapshot Manager-database herstellen
U moet regelmatig back-up van de StorSimple Snapshot Manager-database op de hostcomputer. Als zich een noodgeval voordoet of de computer om welke reden dan ook mislukt, kunt u deze herstellen vanuit de back-up. Het maken van de databaseback-up is een handmatig proces.

#### <a name="to-back-up-and-restore-the-database"></a>Aan de back-up en herstel de database
1. Stop de Microsoft StorSimple Management-Service:
   
   1. Start Serverbeheer.
   2. Op het dashboard Serverbeheer op de **extra** in het menu **Services**.
   3. Op de **Services** venster de **Microsoft StorSimple-beheerservice**.
   4. In het rechterdeelvenster onder **Microsoft StorSimple-beheerservice**, klikt u op **Stop de service**.
2. Blader naar C:\ProgramData\Microsoft\StorSimple\BACatalog op de hostcomputer. 
   
   > [!NOTE]
   > ProgramData is een verborgen map.
   > 
   > 
3. Het XML-bestand van catalogus zoeken, Kopieer het bestand en de kopie opslaan op een veilige locatie of in de cloud. Als de host mislukt, kunt u deze back-upbestand voor het herstellen van de back-upbeleid dat u hebt gemaakt in StorSimple Snapshot Manager.
   
    ![Azure StorSimple-back-catalogus-bestand](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)
4. De Microsoft StorSimple Management-Service opnieuw starten: 
   
   1. Op het dashboard Serverbeheer op de **extra** in het menu **Services**.
   2. Op de **Services** venster de **Microsoft StorSimple-beheerservice**.
   3. In het rechterdeelvenster onder **Microsoft StorSimple-beheerservice**, klikt u op **Start de service opnieuw**.
5. Blader naar C:\ProgramData\Microsoft\StorSimple\BACatalog op de hostcomputer. 
6. Verwijderen van de catalogus XML-bestand en vervang deze door de back-up die u hebt gemaakt. 
7. Klik op het pictogram op het bureaublad StorSimple Snapshot Manager voor het starten van StorSimple Snapshot Manager. 

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over [StorSimple Snapshot Manager-taken en werkstromen](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).

