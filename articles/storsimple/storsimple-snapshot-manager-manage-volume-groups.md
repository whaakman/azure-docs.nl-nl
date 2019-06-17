---
title: StorSimple Snapshot Manager volumegroepen | Microsoft Docs
description: Beschrijft hoe u met de StorSimple Snapshot Manager MMC-module volumegroepen maken en beheren.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: ''
ms.assetid: 7a232414-6a28-4b81-bd7b-cf61e28b33d7
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: e84bc790ac577796e91be010deecc8c5cea1b010
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64699717"
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>StorSimple Snapshot Manager gebruiken om te maken en volumegroepen beheren
## <a name="overview"></a>Overzicht
U kunt de **Volumegroepen** knooppunt op de **bereik** deelvenster volumes toewijzen aan volumegroepen, informatie weergeven over een volumegroep, een back-ups te plannen en volumegroepen bewerken.

Volumegroepen zijn groepen van gerelateerde volumes om ervoor te zorgen dat de back-ups toepassingsconsistente worden gebruikt. Zie voor meer informatie, [Volumes en volumegroepen](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) en [integratie met Windows Volume Shadow Copy Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

> [!IMPORTANT]
> * Alle volumes in de volumegroep van een moeten afkomstig zijn van een enkele cloudserviceprovider.
> * Wanneer u volumegroepen configureert, niet door elkaar gedeelde clustervolumes (CSV's) en niet-CSV-'s in hetzelfde volume. StorSimple Snapshot Manager biedt geen ondersteuning voor een combinatie van CSV's en niet-CSV's in de dezelfde momentopname.

![Knooppunt voor volume-groepen](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Afbeelding 1: StorSimple Snapshot Manager Volumegroepen knooppunt** 

Deze zelfstudie wordt uitgelegd hoe u StorSimple Snapshot Manager kunt gebruiken:

* Informatie over uw volumegroepen weergeven
* Een volumegroep maken
* Back-up van een volumegroep
* Een volumegroep bewerken
* Een volumegroep verwijderen

Al deze acties zijn beschikbaar in de **acties** deelvenster.

## <a name="view-volume-groups"></a>Volume-groepen weergeven
Als u klikt op de **Volumegroepen** knooppunt, de **resultaten** deelvenster ziet u de volgende informatie over elke volumegroep, afhankelijk van de kolomselecties die u aanbrengt. (De kolommen in de **resultaten** deelvenster worden geconfigureerd. Met de rechtermuisknop op de **Volumes** knooppunt uit, selecteer **weergave**, en selecteer vervolgens **kolommen toevoegen/verwijderen**.)

| Resultatenkolom | Description |
|:--- |:--- |
| Name |De **naam** kolom bevat de naam van de volumegroep. |
| Toepassing |De **toepassingen** kolom bevat het nummer van de VSS Writer-services die momenteel is geïnstalleerd en uitgevoerd op de Windows-host. |
| Geselecteerd |De **geselecteerde** kolom ziet u het aantal volumes die zijn opgenomen in de volumegroep. Een nul (0) geeft aan dat er geen toepassing gekoppeld aan de volumes in de volumegroep is. |
| Geïmporteerd |De **geïmporteerd** kolom ziet u het aantal geïmporteerde volumes. Als de waarde **waar**, deze kolom wordt aangegeven dat de volumegroep van een is geïmporteerd uit de Azure-portal en niet in StorSimple Snapshot Manager gemaakt is. |

> [!NOTE]
> StorSimple Snapshot Manager volumegroepen worden ook weergegeven op de **back-upbeleid** tabblad in de Azure-portal.
> 
> 

## <a name="create-a-volume-group"></a>Een volumegroep maken
Gebruik de volgende procedure om een volumegroep te maken.

#### <a name="to-create-a-volume-group"></a>Een volumegroep maken
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** in het deelvenster met de rechtermuisknop op **Volumegroepen**, en klik vervolgens op **Volumegroep maken**.
   
    ![Volumegroep maken](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
   
    De **maken van een volumegroep** in het dialoogvenster wordt weergegeven.
   
    ![Een volume-groepsdialoogvenster maken](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png)
3. Voer de volgende informatie in:
   
   1. In de **naam** typt u een unieke naam voor het nieuwe volumegroep.
   2. In de **toepassingen** vak, selecteer toepassingen die zijn gekoppeld aan de volumes die u aan de volumegroep toevoegen wilt.
      
       De **toepassingen** alleen de toepassingen die gebruikmaken van StorSimple-volumes en VSS-schrijvers zijn ingeschakeld voor deze in een lijst weergegeven. Een VSS-schrijver is alleen ingeschakeld als alle volumes die de writer op de hoogte van de is StorSimple-volumes zijn. Als de toepassingen vak leeg is, zijn geen toepassingen die gebruikmaken van Azure StorSimple-volumes en er een ondersteunde VSS-schrijvers geïnstalleerd. (Op dit moment Azure StorSimple biedt ondersteuning voor Microsoft Exchange en SQL Server.) Zie voor meer informatie over VSS-schrijvers [integratie met Windows Volume Shadow Copy Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).
      
       Als u een toepassing selecteert, worden alle volumes die zijn gekoppeld aan deze automatisch geselecteerd. Daarentegen als u volumes die zijn gekoppeld aan een specifieke toepassing selecteert, de toepassing wordt automatisch geselecteerd de **toepassingen** vak. 
   3. In de **Volumes** Selecteer StorSimple-volumes om toe te voegen aan de volumegroep. 
      
      * U kunt volumes met één of meerdere partities opnemen. (Meerdere volumes van de partitie kunnen worden dynamische schijven of standaardschijven met meerdere partities.) Een volume dat meerdere partities wordt beschouwd als één eenheid. Als gevolg hiervan, als u slechts één van de partities aan de volumegroep van een toevoegt, alle andere partities worden automatisch toegevoegd aan die volumegroep op hetzelfde moment. Nadat u een volume met meerdere partitie aan een volumegroep toevoegen, blijft de meerdere partitie volume worden behandeld als één eenheid.
      * U kunt lege volumegroepen maken door toe te wijzen niet alle volumes aan hen. 
      * Geen verschillende gedeelde clustervolumes (CSV's) en niet-CSV-'s in hetzelfde volume. StorSimple Snapshot Manager biedt geen ondersteuning voor een combinatie van CSV-volumes en niet-CSV-volumes in de dezelfde momentopname.
4. Klik op **OK** om op te slaan van de volumegroep.

## <a name="back-up-a-volume-group"></a>Back-up van een volumegroep
Gebruik de volgende procedure voor back-up van een volumegroep.

#### <a name="to-back-up-a-volume-group"></a>Back-up van een volumegroep
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, vouw de **Volumegroepen** knooppunt, met de rechtermuisknop op de naam van een volume-groep en klik vervolgens op **duren back-up**.
   
    ![Direct een back-up volumegroep](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)
3. In de **duren back-up** in het dialoogvenster, selecteer **lokale momentopname** of **Cloudmomentopname**, en klik vervolgens op **maken**.
   
    ![Back-dialoogvenster nemen](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png)
4. Om te bevestigen dat de back-up wordt uitgevoerd, vouw de **taken** knooppunt en klik vervolgens op **met**. De back-up moet worden weergegeven.
5. Als u wilt weergeven van de momentopname van het voltooide, vouw de **back-upcatalogus** knooppunt, vouw de naam van het volume-groep en klik vervolgens op **lokale momentopname** of **Cloudmomentopname**. De back-up wordt weergegeven als deze is voltooid.

## <a name="edit-a-volume-group"></a>Een volumegroep bewerken
Gebruik de volgende procedure een volumegroep wilt bewerken.

#### <a name="to-edit-a-volume-group"></a>Een volumegroep bewerken
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, vouw de **Volumegroepen** knooppunt, met de rechtermuisknop op de naam van een volume-groep en klik vervolgens op **bewerken**.
3. De ** maken van een volumegroep ** in het dialoogvenster wordt weergegeven. U kunt wijzigen de **naam**, **toepassingen**, en **Volumes** vermeldingen.
4. Klik op **OK** om uw wijzigingen op te slaan.

## <a name="delete-a-volume-group"></a>Een volumegroep verwijderen
Gebruik de volgende procedure om een volumegroep te verwijderen. 

> [!WARNING]
> Hiermee verwijdert u ook alle de back-ups die zijn gekoppeld aan de volumegroep.
> 
> 

#### <a name="to-delete-a-volume-group"></a>Een volumegroep verwijderen
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, vouw de **Volumegroepen** knooppunt, met de rechtermuisknop op de naam van een volume-groep en klik vervolgens op **verwijderen**.
3. De **Volumegroep verwijderen** in het dialoogvenster wordt weergegeven. Type **bevestigen** in het tekstvak in en klik vervolgens op **OK**.
   
    De groep verwijderde volume verdwijnt uit de lijst in de **resultaten** deelvenster en alle back-ups die gekoppeld aan die volumegroep zijn zijn verwijderd uit de back-catalogus.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over het [StorSimple Snapshot Manager gebruiken voor het maken en beheren van back-upbeleid](storsimple-snapshot-manager-manage-backup-policies.md).

