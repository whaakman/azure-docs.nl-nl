---
title: StorSimple Snapshot Manager en volumes | Microsoft Docs
description: Beschrijft hoe u het StorSimple Snapshot Manager MMC-module weergeven en beheren van volumes en back-ups configureren.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 78896323-e57c-431e-bbe2-0cbde1cf43a2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: 2c0b211bced99d272a73a7b018a22f99d8d58aa9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>Gebruik StorSimple Snapshot Manager weergeven en beheren van volumes
## <a name="overview"></a>Overzicht
U kunt het StorSimple Snapshot Manager **Volumes** knooppunt (op de **bereik** deelvenster) Selecteer volumes en informatie over deze weergeeft. De volumes worden weergegeven als de stations die overeenkomen met de volumes die zijn gekoppeld met de host. De **Volumes** knooppunt wordt de lokale volumes en typen van de volumes die worden ondersteund door StorSimple, met inbegrip van volumes die zijn gedetecteerd door het gebruik van iSCSI- en een apparaat. 

Voor meer informatie over ondersteunde volumes, gaat u naar [ondersteuning voor meerdere volumetypen](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Volumelijst in het deelvenster met resultaten](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

De **Volumes** knooppunt ook kunt u het opnieuw scannen of volumes verwijderen nadat ze in StorSimple Snapshot Manager worden gedetecteerd. 

Deze zelfstudie wordt uitgelegd hoe u kunt koppelen, initialiseren, en volumes formatteren en vervolgens StorSimple Snapshot Manager te gebruiken:

* Informatie over volumes weergeven 
* Volumes verwijderen
* Volumes opnieuw scannen 
* Configureer een standaardvolume en back-up maken
* Configureren van een gespiegeld volume en back-up maken

> [!NOTE]
> Alle van de **Volume** knooppunt acties zijn ook beschikbaar in de **acties** deelvenster.
> 
> 

## <a name="mount-volumes"></a>Koppelen van volumes
Gebruik de volgende procedure om te koppelen, initialiseren en formatteren van StorSimple-volumes. Deze procedure maakt gebruik van een systeemhulpprogramma voor het beheren van harde schijven en de bijbehorende volumes of partities Schijfbeheer. Voor meer informatie over Schijfbeheer, gaat u naar [Schijfbeheer](https://technet.microsoft.com/library/cc770943.aspx) op de website Microsoft TechNet.

#### <a name="to-mount-volumes"></a>Koppelen van volumes
1. Start de Microsoft iSCSI-initiator op de hostcomputer.
2. Geef een van de interface IP-adressen als de doelportal of detectie van IP-adres en verbinding maken met het apparaat. Nadat het apparaat is verbonden, zijn de volumes zijn toegankelijk voor uw Windows-systeem. Ga voor meer informatie over het gebruik van de Microsoft iSCSI-initiator aan de sectie "verbinding maken met een iSCSI-doelapparaat' [installeren en configureren van Microsoft iSCSI-Initiator][1].
3. Gebruik een van de volgende opties voor Schijfbeheer start:
   
   * Typ Diskmgmt.msc in de **uitvoeren** vak.
   * Start Serverbeheer uit, vouw de **opslag** knooppunt en selecteer vervolgens **Schijfbeheer**.
   * Start **Systeembeheer**, vouw de **Computerbeheer** knooppunt en selecteer vervolgens **Schijfbeheer**. 
     
     > [!NOTE]
     > Administrator-bevoegdheden moet u Schijfbeheer uitvoeren.
     > 
     > 
4. De volumes online nemen:
   
   1. In Schijfbeheer met de rechtermuisknop op een volume dat is gemarkeerd als **Offline**.
   2. Klik op **schijf opnieuw activeren**. De schijf moet worden gemarkeerd **Online** nadat de schijf opnieuw wordt geactiveerd.
5. Initialiseren van de volumes:
   
   1. Met de rechtermuisknop op de gedetecteerde volumes.
   2. Selecteer in het menu **schijf initialiseren**.
   3. In de **schijf initialiseren** in het dialoogvenster, selecteer de schijven die u wilt initialiseren en klik vervolgens op **OK**.
6. Eenvoudige volumes formatteren:
   
   1. Met de rechtermuisknop op een volume dat u de notatie wilt instellen.
   2. Selecteer in het menu **Nieuw eenvoudig Volume**.
   3. Gebruik de wizard Nieuw eenvoudig Volume op het volume formatteren:
      
      * Geef de grootte van het volume.
      * Geef een stationsletter.
      * Selecteer het NTFS-bestandssysteem.
      * Geef een clustergrootte van 64 kB op.
      * Voer een snelle formattering uit.
7. Meerdere partitie volumes formatteren. Voor instructies gaat u naar de sectie 'Partities en Volumes' [Schijfbeheer implementeren](https://msdn.microsoft.com/library/dd163556.aspx).

## <a name="view-information-about-your-volumes"></a>Informatie over uw volumes weergeven
Gebruik de volgende procedure om informatie over lokale en Azure StorSimple-volumes weer te geven.

#### <a name="to-view-volume-information"></a>Volume-informatie weergeven
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager. 
2. In de **bereik** deelvenster, klikt u op de **Volumes** knooppunt. Een lijst van lokale en gekoppelde volumes, met inbegrip van alle Azure StorSimple-volumes wordt weergegeven in de **resultaten** deelvenster. De kolommen in de **resultaten** deelvenster worden geconfigureerd. (Met de rechtermuisknop op de **Volumes** knooppunt **weergave**, en selecteer vervolgens **kolommen toevoegen/verwijderen**.)
   
    ![De kolommen configureren](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)
   
   | Kolom | Beschrijving |
   |:--- |:--- |
   |  Naam |De **naam** kolom bevat de stationsletter die is toegewezen aan elke gedetecteerde volume. |
   |  Apparaat |De **apparaat** kolom bevat het IP-adres van het apparaat is verbonden met de hostcomputer. |
   |  Volumenaam van apparaat |De **Volume apparaatnaam** kolom bevat de naam van het apparaat volume waarop het geselecteerde volume behoort. Dit is de naam van het volume gedefinieerd in de Azure-portal voor die specifieke volume. |
   |  Toegangspaden |De **Toegangspaden** kolom wordt de toegang tot het pad naar het volume weergegeven. Dit is het station stationsletter of koppelpunt punt waarop het volume toegankelijk op de hostcomputer is. |

## <a name="delete-a-volume"></a>Een volume verwijderen
Gebruik de volgende procedure een volume van StorSimple Snapshot Manager verwijderen.

> [!NOTE]
> U kunt een volume niet verwijderen als deze deel van een volume-groep uitmaakt. (De verwijderoptie is niet beschikbaar voor volumes die lid van een volume-groep zijn.) U moet de groep van het gehele volume voor het verwijderen van het volume verwijderen.

#### <a name="to-delete-a-volume"></a>Een volume verwijderen
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, klikt u op de **Volumes** knooppunt. 
3. In de **resultaten** deelvenster met de rechtermuisknop op het volume dat u wilt verwijderen.
4. Klik op het menu **verwijderen**. 
   
    ![Een volume verwijderen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 
5. De **Volume verwijderen** dialoogvenster wordt weergegeven. Type **bevestigen** in het tekstvak en klik vervolgens op **OK**.
6. Standaard StorSimple Snapshot Manager back-ups van een volume voordat u het verwijdert. Deze voorzorgsmaatregel kunt beschermen tegen gegevensverlies als de verwijdering onbedoeld is verwijderd. StorSimple Snapshot Manager geeft een **Automatische momentopname** voortgangsbericht terwijl deze back-ups van het volume. 
   
    ![Automatische momentopname bericht](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Volumes opnieuw scannen
Gebruik de volgende procedure om te scannen van volumes die zijn verbonden aan StorSimple Snapshot Manager.

#### <a name="to-rescan-the-volumes"></a>Om te scannen van de volumes
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster met de rechtermuisknop op **Volumes**, en klik vervolgens op **volumes opnieuw scannen**.
   
    ![Volumes opnieuw scannen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
   
    Deze procedure synchroniseert de volumelijst met StorSimple Snapshot Manager. Wijzigingen, zoals nieuwe volumes of verwijderde volumes worden, weergegeven in de resultaten.

## <a name="configure-and-back-up-a-basic-volume"></a>Configureren en back-up van een standaardvolume
Gebruik de volgende procedure voor het configureren van een back-up van een standaardvolume en vervolgens een back-up onmiddellijk te starten of een beleid voor geplande back-ups maken.

### <a name="prerequisites"></a>Vereisten
Voordat u begint:

* Zorg ervoor dat de StorSimple-apparaat en de host-computer juist zijn geconfigureerd. Ga voor meer informatie naar [uw on-premises StorSimple-apparaat implementeren](storsimple-deployment-walkthrough-u2.md).
* Installeer en configureer StorSimple Snapshot Manager. Ga voor meer informatie naar [implementeren StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).

#### <a name="to-configure-backup-of-a-basic-volume"></a>Back-up van een standaardvolume configureren
1. Maak een standaardvolume op het StorSimple-apparaat.
2. Koppelen, initialiseren en formatteren van het volume, zoals beschreven in [volumes koppelen](#mount-volumes). 
3. Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad. Het StorSimple Snapshot Manager-venster wordt weergegeven. 
4. In de **bereik** deelvenster met de rechtermuisknop op de **Volumes** knooppunt en selecteer vervolgens **volumes opnieuw scannen**. Wanneer de scan is voltooid, een lijst van volumes moet worden weergegeven in de **resultaten** deelvenster. 
5. In de **resultaten** deelvenster met de rechtermuisknop op het volume en selecteer vervolgens **Create Volume Group**. 
   
    ![Volume-groep maken](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 
6. In de **Create Volume Group** in het dialoogvenster Typ een naam voor de groep volume, volumes toe te kennen aan en klik vervolgens op **OK**.
7. In de **bereik** deelvenster, vouw de **Volume groepen** knooppunt. De nieuwe groep van volume moet worden weergegeven onder de **Volume groepen** knooppunt. 
8. Met de rechtermuisknop op de naam van het volume.
   
   * Voor het starten van een back-uptaak van interactieve (op aanvraag), klikt u op **back-ups nemen**. 
   * Als u een automatische back-ups plannen, klikt u op **back-upbeleid maken**. Op de **algemene** pagina, selecteert u een volume-groep in de lijst. Op de **planning** pagina, voert u de details van de planning. Wanneer u klaar bent, klikt u op **OK**. 
9. Om te bevestigen dat de back-uptaak is gestart, vouw de **taken** knooppunt in de **bereik** deelvenster en klik vervolgens op de **met** knooppunt. De lijst met actieve taken wordt weergegeven in de **resultaten** deelvenster. 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Configureren en back-up van een gespiegeld volume
De volgende stappen voor het configureren van de back-up van een gespiegeld volume:

* Stap 1: Gebruik Schijfbeheer om te maken van een gespiegeld volume. 
* Stap 2: StorSimple Snapshot Manager gebruiken voor het configureren van de back-up.

### <a name="prerequisites"></a>Vereisten
Voordat u begint:

* Zorg ervoor dat de StorSimple-apparaat en de host-computer juist zijn geconfigureerd. Ga voor meer informatie naar [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).
* Installeer en configureer StorSimple Snapshot Manager. Ga voor meer informatie naar [implementeren StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).
* Configureer twee volumes op het StorSimple-apparaat. (In de voorbeelden worden de beschikbare volumes zijn **schijf 1** en **schijf 2**.) 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>Stap 1: Gebruik Schijfbeheer om te maken van een gespiegeld volume
Schijfbeheer is een hulpprogramma voor het beheren van harde schijven en de volumes of partities die ze bevatten. Voor meer informatie over Schijfbeheer, gaat u naar [Schijfbeheer](https://technet.microsoft.com/library/cc770943.aspx) op de website Microsoft TechNet.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Maken van een gespiegeld volume
1. Gebruik een van de volgende opties voor Schijfbeheer start: 
   
   * Open de **uitvoeren** in het vak **Diskmgmt.msc**, en druk op Enter.
   * Start Serverbeheer uit, vouw de **opslag** knooppunt en selecteer vervolgens **Schijfbeheer**. 
   * Start **Systeembeheer**, vouw de **Computerbeheer** knooppunt en selecteer vervolgens **Schijfbeheer**. 
2. Zorg ervoor dat er twee volumes beschikbaar op het StorSimple-apparaat. (In het voorbeeld wordt de beschikbare volumes zijn **schijf 1** en **schijf 2**.) 
3. In het venster Schijfbeheer in de rechterkolom van het onderste deelvenster met de rechtermuisknop op **schijf 1** en selecteer **nieuw gespiegeld Volume**. 
   
    ![Nieuw Mirrored Volume](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 
4. Op de **nieuw gespiegeld Volume** pagina van de wizard, klikt u op **volgende**.
5. Op de **schijven selecteren** pagina **schijf 2** in de **geselecteerde** deelvenster, klikt u op **toevoegen**, en klik vervolgens op **volgende**. 
6. Op de **stationsaanduiding toewijzen of pad** pagina, accepteer de standaardinstellingen en klik vervolgens op **volgende**. 
7. Op de **Opmaakvolume** pagina in de **clustergrootte** de optie **64K**. Selecteer de **Snelformatteren** selectievakje en klik vervolgens op **volgende**. 
8. Op de **voltooien van het nieuwe gespiegeld Volume** pagina, Controleer uw instellingen en klik vervolgens op **voltooien**. 
9. Een bericht weergegeven om aan te geven dat de standaardschijf zal worden geconverteerd naar een dynamische schijf. Klik op **Ja**.
   
    ![Dynamische schijf converteren bericht](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 
10. Controleer of schijf 1 en 2 van de schijf worden als dynamische gespiegelde volumes weergegeven in Schijfbeheer. (**Dynamische** moet worden weergegeven in de statuskolom en de kleur van de capaciteit balk moet worden gewijzigd in rood, die een gespiegeld volume aangeeft.) 
    
    ![Schijf Management gespiegeld dynamische schijven](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 

### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>Stap 2: Gebruik StorSimple Snapshot Manager back-up configureren
Gebruik de volgende procedure voor het configureren van een dynamische mirrored volume en vervolgens een back-up onmiddellijk te starten of een beleid voor geplande back-ups maken.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Back-up van een gespiegeld volume configureren
1. Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad. Het StorSimple Snapshot Manager-venster wordt weergegeven. 
2. In de **bereik** deelvenster met de rechtermuisknop op de **Volumes** uit en selecteer **volumes opnieuw scannen**. Wanneer de scan is voltooid, een lijst van volumes moet worden weergegeven in de **resultaten** deelvenster. Het dynamische gespiegeld volume wordt vermeld als een enkel volume. 
3. In de **resultaten** deelvenster met de rechtermuisknop op de gespiegeld volume en klik vervolgens op **Create Volume Group**. 
4. In de **Create Volume Group** in het dialoogvenster Typ een naam voor de groep volume, de gespiegeld volume toewijzen aan deze groep en klik vervolgens op **OK**. 
5. In de **bereik** deelvenster, vouw de **Volume groepen** knooppunt. De nieuwe groep van volume moet worden weergegeven onder de **Volume groepen** knooppunt. 
6. Met de rechtermuisknop op de naam van het volume. 
   
   * Voor het starten van een back-uptaak van interactieve (op aanvraag), klikt u op **back-ups nemen**. 
   * Als u een automatische back-ups plannen, klikt u op **back-upbeleid maken**. Op de **algemene** pagina, selecteert u de groep volume in de lijst. Op de **planning** pagina, voert u de details van de planning. Wanneer u klaar bent, klikt u op **OK**. 
7. Als dit wordt uitgevoerd, kunt u de back-uptaak bewaken. In de **bereik** deelvenster, vouw de **taken** knooppunt en klik vervolgens op **met**, de taakdetails worden weergegeven in de **resultaten** deelvenster. Wanneer de back-uptaak is voltooid, de gegevens worden overgebracht naar de **afgelopen 24** uren taak lijst. 

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over hoe [StorSimple Snapshot Manager maken en beheren van volume groepen gebruiken](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
