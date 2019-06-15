---
title: StorSimple Snapshot Manager en volumes | Microsoft Docs
description: Hierin wordt beschreven hoe u de StorSimple Snapshot Manager MMC-module weergeven en beheren van volumes en back-ups configureren.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: ''
ms.assetid: 78896323-e57c-431e-bbe2-0cbde1cf43a2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: 260dfdd4b8fe7c277358fa5773029ea9a532740a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61077523"
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>StorSimple Snapshot Manager weergeven en beheren van volumes gebruiken
## <a name="overview"></a>Overzicht
U kunt StorSimple Snapshot Manager **Volumes** knooppunt (op de **bereik** deelvenster) volumes selecteren en weergeven van informatie over deze. De volumes worden weergegeven als stations die overeenkomen met de volumes die zijn gekoppeld met de host. De **Volumes** knooppunt wordt weergegeven voor lokale volumes en typen die worden ondersteund door StorSimple, met inbegrip van volumes die zijn gedetecteerd door het gebruik van iSCSI- en een apparaat. 

Voor meer informatie over ondersteunde volumes, gaat u naar [ondersteuning voor meerdere typen](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Volumelijst met in het deelvenster met resultaten](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

De **Volumes** knooppunt ook kunt u opnieuw scannen of volumes verwijderen nadat het StorSimple Snapshot Manager ze detecteert. 

Deze zelfstudie wordt uitgelegd hoe u kunt koppelen, initialiseren, en formatteren van volumes en vervolgens StorSimple Snapshot Manager te gebruiken:

* Informatie over volumes weergeven 
* Volumes verwijderen
* Volumes opnieuw scannen 
* Configureren van een standaardvolume en back-up maken
* Configureren van een gespiegeld volume en back-up maken

> [!NOTE]
> Alle van de **Volume** knooppunt acties zijn ook beschikbaar in de **acties** deelvenster.
> 
> 

## <a name="mount-volumes"></a>Volumes koppelen
Gebruik de volgende procedure om te koppelen, initialiseren en formatteren van StorSimple-volumes. Deze procedure maakt gebruik van Schijfbeheer, een hulpprogramma voor het beheren van harde schijven en de bijbehorende volumes of partities. Voor meer informatie over Schijfbeheer, gaat u naar [Schijfbeheer](https://technet.microsoft.com/library/cc770943.aspx) op de website Microsoft TechNet.

#### <a name="to-mount-volumes"></a>Volumes koppelen
1. Start de Microsoft iSCSI-initiator op de hostcomputer.
2. Geef een van de interface IP-adressen als de doelportal of de detectie van IP-adres en maak verbinding met het apparaat. Nadat het apparaat is verbonden, zijn de volumes zijn toegankelijk is voor uw Windows-systeem. Voor meer informatie over het gebruik van de Microsoft iSCSI-initiator, gaat u naar de sectie "verbinding maken met een iSCSI-doelapparaat' [installeren en configureren van Microsoft iSCSI-Initiator][1].
3. Een van de volgende opties gebruiken om schijf Management te starten:
   
   * Typ Diskmgmt.msc in de **uitvoeren** vak.
   * Serverbeheer starten uit, vouw de **opslag** knooppunt en selecteert u vervolgens **Schijfbeheer**.
   * Start **Systeembeheer**, vouw de **Computerbeheer** knooppunt en selecteert u vervolgens **Schijfbeheer**. 
     
     > [!NOTE]
     > U moet administrator-bevoegdheden gebruiken om uit te voeren van Schijfbeheer.
     > 
     > 
4. De volumes online nemen:
   
   1. In Schijfbeheer met de rechtermuisknop op een volume dat is gemarkeerd als **Offline**.
   2. Klik op **schijf opnieuw activeren**. De schijf moet worden gemarkeerd **Online** nadat de schijf opnieuw wordt geactiveerd.
5. Initialiseren van de volumes:
   
   1. Met de rechtermuisknop op de gedetecteerde volumes.
   2. Selecteer in het menu **schijf initialiseren**.
   3. In de **schijf initialiseren** in het dialoogvenster, selecteer de schijven die u wilt initialiseren, en klik vervolgens op **OK**.
6. Eenvoudige volumes formatteren:
   
   1. Met de rechtermuisknop op een volume dat u wilt formatteren.
   2. Selecteer in het menu **Nieuw eenvoudig Volume**.
   3. Gebruik de wizard Nieuw eenvoudig Volume op het volume geformatteerd:
      
      * Geef de grootte van het volume.
      * Geef een stationsaanduiding is toegekend.
      * Selecteer het NTFS-bestandssysteem.
      * Geef een clustergrootte van 64 kB op.
      * Voer een snelle formattering uit.
7. Meerdere partitie volumes formatteren. Voor instructies gaat u naar de sectie "Partities en Volumes" in [Schijfbeheer implementeren](https://msdn.microsoft.com/library/dd163556.aspx).

## <a name="view-information-about-your-volumes"></a>Informatie over uw volumes weergeven
Gebruik de volgende procedure om informatie over de lokale en Azure StorSimple-volumes weer te geven.

#### <a name="to-view-volume-information"></a>Volume-informatie weergeven
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad. 
2. In de **bereik** deelvenster, klikt u op de **Volumes** knooppunt. Een lijst met lokale en gekoppelde volumes, met inbegrip van alle Azure StorSimple-volumes, wordt weergegeven in de **resultaten** deelvenster. De kolommen in de **resultaten** deelvenster worden geconfigureerd. (Met de rechtermuisknop op de **Volumes** knooppunt uit, selecteer **weergave**, en selecteer vervolgens **kolommen toevoegen/verwijderen**.)
   
    ![Configureren van de kolommen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)
   
   | Resultatenkolom | Description |
   |:--- |:--- |
   |  Name |De **naam** kolom bevat de stationsletter die is toegewezen aan elke gedetecteerde volume. |
   |  Apparaat |De **apparaat** kolom bevat het IP-adres van het apparaat is verbonden met de hostcomputer. |
   |  Volumenaam van apparaat |De **Volume apparaatnaam** kolom bevat de naam van het apparaat volume waarvan het geselecteerde volume deel uitmaakt. Dit is de naam van het volume gedefinieerd in de Azure-portal voor dat specifieke volume. |
   |  Toegangspaden |De **Toegangspaden** kolom ziet u de toegang tot het pad naar het volume. Dit is het station stationsletter of koppelpunt punt waarop het volume toegankelijk is op de host is. |

## <a name="delete-a-volume"></a>Een volume verwijderen
Gebruik de volgende procedure om te verwijderen van een volume van StorSimple Snapshot Manager.

> [!NOTE]
> U kunt een volume niet verwijderen als het is een onderdeel van een volumegroep. (De optie voor het verwijderen is niet beschikbaar voor volumes die lid van een volumegroep zijn.) U moet de groep van het gehele volume als u wilt verwijderen van het volume verwijderen.

#### <a name="to-delete-a-volume"></a>Een volume verwijderen
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, klikt u op de **Volumes** knooppunt. 
3. In de **resultaten** in het deelvenster met de rechtermuisknop op het volume dat u wilt verwijderen.
4. In het menu, klikt u op **verwijderen**. 
   
    ![Een volume verwijderen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 
5. De **Volume verwijderen** in het dialoogvenster wordt weergegeven. Type **bevestigen** in het tekstvak in en klik vervolgens op **OK**.
6. Standaard StorSimple Snapshot Manager back-ups van een volume voordat deze worden verwijderd. Deze voorzorgsmaatregel kunt beschermen tegen gegevensverlies als de verwijdering onbedoeld is verwijderd. StorSimple Snapshot Manager geeft een **automatische momentopnamen** voortgangsbericht tijdens het back-ups van het volume. 
   
    ![Automatische momentopnamen bericht](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Volumes opnieuw scannen
Gebruik de volgende procedure om opnieuw te scannen van de volumes die zijn verbonden aan StorSimple Snapshot Manager.

#### <a name="to-rescan-the-volumes"></a>Aan de volumes opnieuw scannen
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** in het deelvenster met de rechtermuisknop op **Volumes**, en klik vervolgens op **volumes opnieuw scannen**.
   
    ![Volumes opnieuw scannen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
   
    Deze procedure wordt de volumelijst met gesynchroniseerd met StorSimple Snapshot Manager. Eventuele wijzigingen, zoals nieuwe volumes of verwijderde volumes, wordt in de resultaten worden doorgevoerd.

## <a name="configure-and-back-up-a-basic-volume"></a>Configureren en back-up van een standaardvolume
Gebruik de volgende procedure voor het configureren van een back-up van een eenvoudige volume, en vervolgens onmiddellijk starten van een back-up of een beleid voor geplande back-ups maken.

### <a name="prerequisites"></a>Vereisten
Voordat u begint:

* Zorg ervoor dat de StorSimple-apparaat en de host computer correct zijn geconfigureerd. Ga voor meer informatie naar [uw on-premises StorSimple-apparaat implementeren](storsimple-deployment-walkthrough-u2.md).
* Installeer en configureer StorSimple Snapshot Manager. Ga voor meer informatie naar [implementeren StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).

#### <a name="to-configure-backup-of-a-basic-volume"></a>Back-up van een standaardvolume configureren
1. Maak een eenvoudige volume op het StorSimple-apparaat.
2. Koppelen, initialiseren en formatteren van het volume, zoals beschreven in [volumes koppelen](#mount-volumes). 
3. Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad. Het StorSimple Snapshot Manager-venster wordt weergegeven. 
4. In de **bereik** in het deelvenster met de rechtermuisknop op de **Volumes** knooppunt en selecteert u vervolgens **volumes opnieuw scannen**. Wanneer de scan is voltooid, een lijst van volumes moet worden weergegeven in de **resultaten** deelvenster. 
5. In de **resultaten** in het deelvenster met de rechtermuisknop op het volume en selecteer vervolgens **Volumegroep maken**. 
   
    ![Volumegroep maken](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 
6. In de **Volumegroep maken** in het dialoogvenster Typ een naam voor de volumegroep, volumes toewijzen aan en klik vervolgens op **OK**.
7. In de **bereik** deelvenster, vouw de **Volumegroepen** knooppunt. De nieuwe volumegroep moet worden weergegeven onder de **Volumegroepen** knooppunt. 
8. Met de rechtermuisknop op de naam van het volume-groep.
   
   * Als u wilt een interactieve (op aanvraag) back-uptaak starten, klikt u op **back-ups nemen**. 
   * Als u wilt een automatische back-up plannen, klikt u op **back-upbeleid maken**. Op de **algemene** pagina, selecteert u een volumegroep in de lijst. Op de **planning** pagina, voer de details van de planning. Wanneer u klaar bent, klikt u op **OK**. 
9. Om te bevestigen dat de back-uptaak is gestart, vouw de **taken** knooppunt in de **bereik** deelvenster en klik vervolgens op de **met** knooppunt. De lijst met actieve taken wordt weergegeven in de **resultaten** deelvenster. 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Configureren en back-up van een gespiegeld volume
De volgende stappen voor het configureren van back-up van een gespiegeld volume:

* Stap 1: Gebruik Schijfbeheer om een dynamische mirrored volume te maken. 
* Stap 2: Gebruik StorSimple Snapshot Manager back-up configureren.

### <a name="prerequisites"></a>Vereisten
Voordat u begint:

* Zorg ervoor dat de StorSimple-apparaat en de host computer correct zijn geconfigureerd. Ga voor meer informatie naar [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).
* Installeer en configureer StorSimple Snapshot Manager. Ga voor meer informatie naar [implementeren StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).
* Configureer twee volumes op het StorSimple-apparaat. (In de voorbeelden worden de beschikbare volumes zijn **schijf 1** en **schijf 2**.) 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>Stap 1: Gebruik Schijfbeheer om te maken van een gespiegeld volume
Schijfbeheer is een hulpprogramma voor het beheren van harde schijven en volumes of partities die ze bevatten. Voor meer informatie over Schijfbeheer, gaat u naar [Schijfbeheer](https://technet.microsoft.com/library/cc770943.aspx) op de website Microsoft TechNet.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Een dynamische mirrored volume maken
1. Een van de volgende opties gebruiken om schijf Management te starten: 
   
   * Open de **uitvoeren** in het vak **Diskmgmt.msc**, en druk op Enter.
   * Serverbeheer starten uit, vouw de **opslag** knooppunt en selecteert u vervolgens **Schijfbeheer**. 
   * Start **Systeembeheer**, vouw de **Computerbeheer** knooppunt en selecteert u vervolgens **Schijfbeheer**. 
2. Zorg ervoor dat u hebt twee volumes die beschikbaar zijn op het StorSimple-apparaat. (In het voorbeeld wordt de beschikbare volumes zijn **schijf 1** en **schijf 2**.) 
3. In het venster Schijfbeheer in de rechterkolom van het onderste deelvenster met de rechtermuisknop op **schijf 1** en selecteer **nieuw gespiegeld Volume**. 
   
    ![Nieuw Mirrored Volume](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 
4. Op de **nieuw gespiegeld Volume** pagina van de wizard, klikt u op **volgende**.
5. Op de **schijven selecteren** pagina, selecteert u **schijf 2** in de **geselecteerde** deelvenster, klikt u op **toevoegen**, en klik vervolgens op **volgende**. 
6. Op de **stationsletter toewijzen of het pad** pagina, accepteer de standaardwaarden en klik vervolgens op **volgende**. 
7. Op de **Opmaakvolume** pagina, in de **clustergrootte** Schakel **64 kB**. Selecteer de **Snelformatteren** selectievakje en klik vervolgens op **volgende**. 
8. Op de **voltooien van het nieuwe gespiegeld Volume** pagina, Controleer uw instellingen en klik vervolgens op **voltooien**. 
9. Een bericht weergegeven om aan te geven dat de standaardschijf worden geconverteerd naar een dynamische schijf. Klik op **Ja**.
   
    ![Dynamische schijf converteren bericht](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 
10. Controleer of schijf 1 en 2 van de schijf als dynamische gespiegelde volumes worden weergegeven in Schijfbeheer. (**Dynamische** moet worden weergegeven in de statuskolom en de kleur van de capaciteit-balk in rood, die wijzen op een gespiegeld volume moet wijzigen.) 
    
    ![Dynamische schijven gespiegeld Management](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 

### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>Stap 2: Gebruik StorSimple Snapshot Manager back-up configureren
Gebruik de volgende procedure voor het configureren van een dynamische gespiegeld volume, en vervolgens onmiddellijk starten van een back-up of een beleid voor geplande back-ups maken.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Back-up van een gespiegeld volume configureren
1. Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad. Het StorSimple Snapshot Manager-venster wordt weergegeven. 
2. In de **bereik** in het deelvenster met de rechtermuisknop op de **Volumes** knooppunt en selecteert u **volumes opnieuw scannen**. Wanneer de scan is voltooid, een lijst van volumes moet worden weergegeven in de **resultaten** deelvenster. Het dynamische mirrored volume wordt vermeld als één volume. 
3. In de **resultaten** in het deelvenster met de rechtermuisknop op de dynamische gespiegeld volume en klik vervolgens op **Volumegroep maken**. 
4. In de **Volumegroep maken** in het dialoogvenster Typ een naam voor de volumegroep, de dynamische mirrored volume toewijzen aan deze groep en klik vervolgens op **OK**. 
5. In de **bereik** deelvenster, vouw de **Volumegroepen** knooppunt. De nieuwe volumegroep moet worden weergegeven onder de **Volumegroepen** knooppunt. 
6. Met de rechtermuisknop op de naam van het volume-groep. 
   
   * Als u wilt een interactieve (op aanvraag) back-uptaak starten, klikt u op **back-ups nemen**. 
   * Als u wilt een automatische back-up plannen, klikt u op **back-upbeleid maken**. Op de **algemene** pagina, selecteert u de volumegroep in de lijst. Op de **planning** pagina, voer de details van de planning. Wanneer u klaar bent, klikt u op **OK**. 
7. U kunt de back-uptaak bewaken terwijl deze wordt uitgevoerd. In de **bereik** deelvenster, vouw de **taken** knooppunt en klik vervolgens op **met**, de taakdetails worden weergegeven in de **resultaten** deelvenster. Wanneer de back-uptaak is voltooid, de gegevens worden overgebracht naar de **afgelopen 24 uur per dag** uur taak lijst. 

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over het [StorSimple Snapshot Manager gebruiken voor het maken en beheren van volumegroepen](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
