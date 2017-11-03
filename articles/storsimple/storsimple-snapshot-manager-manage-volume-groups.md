---
title: StorSimple Snapshot Manager volume groepen | Microsoft Docs
description: Hierin wordt beschreven hoe u met de StorSimple Snapshot Manager MMC-module volume groepen maken en beheren.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 7a232414-6a28-4b81-bd7b-cf61e28b33d7
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 6067a88cd42d29c3d2f4b74580095424de77561e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>StorSimple Snapshot Manager gebruiken voor het volume-groepen maken en beheren
## <a name="overview"></a>Overzicht
U kunt de **Volume groepen** knooppunt op de **bereik** deelvenster volumes toewijzen aan groepen volume, informatie weergeven over een groep volume back-ups plannen en volume groepen bewerken.

Volume-groepen zijn groepen van gerelateerde volumes om ervoor te zorgen dat de back-ups toepassingsconsistente worden gebruikt. Zie voor meer informatie [Volumes en volume groepen](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) en [integratie met Windows Volume Shadow Copy Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

> [!IMPORTANT]
> * Alle volumes in een groep volume moeten afkomstig zijn van een serviceprovider één cloud.
> * Wanneer u een volume-groepen configureert, niet door elkaar op gedeelde clustervolumes (CSV's) en niet-CSV in dezelfde groep volume. StorSimple Snapshot Manager biedt geen ondersteuning voor een combinatie van CSV's en niet-CSV's in de dezelfde momentopname.

![Knooppunt voor volume-groepen](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Afbeelding 1: StorSimple Snapshot Manager Volume groepen knooppunt** 

Deze zelfstudie wordt uitgelegd hoe u StorSimple Snapshot Manager om te kan gebruiken:

* Informatie over de volume-groepen weergeven
* De groep van een volume maken
* Back-up van een volume-groep
* Een volume-groep bewerken
* Een volume-groep verwijderen

Al deze acties zijn ook beschikbaar in de **acties** deelvenster.

## <a name="view-volume-groups"></a>Volume-groepen weergeven
Als u op de **Volume groepen** knooppunt de **resultaten** deelvenster ziet u de volgende informatie over elke groep volume, afhankelijk van de kolomselecties voor de die u aanbrengt. (De kolommen in de **resultaten** deelvenster worden geconfigureerd. Met de rechtermuisknop op de **Volumes** knooppunt **weergave**, en selecteer vervolgens **kolommen toevoegen/verwijderen**.)

| Kolom | Beschrijving |
|:--- |:--- |
| Naam |De **naam** kolom bevat de naam van de volume-groep. |
| Toepassing |De **toepassingen** kolom ziet u het nummer van de VSS-schrijvers die momenteel is geïnstalleerd en actief op de Windows-host. |
| Geselecteerd |De **geselecteerde** kolom ziet u het aantal volumes die zijn opgenomen in de groep volume. Een nul (0) geeft aan dat er geen toepassing gekoppeld aan de volumes in de groep volume. |
| Geïmporteerd |De **geïmporteerde** kolom ziet u het aantal geïmporteerde volumes. Als de waarde **True**, deze kolom wordt aangegeven dat een volume-groep is geïmporteerd uit de Azure-portal en in StorSimple Snapshot Manager niet is gemaakt. |

> [!NOTE]
> StorSimple Snapshot Manager volume groepen worden ook weergegeven op de **back-upbeleid** tabblad in de Azure-portal.
> 
> 

## <a name="create-a-volume-group"></a>De groep van een volume maken
Gebruik de volgende procedure om een volume-groep te maken.

#### <a name="to-create-a-volume-group"></a>Een volume-groep maken
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster met de rechtermuisknop op **Volume groepen**, en klik vervolgens op **Create Volume Group**.
   
    ![Volume-groep maken](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
   
    De **maakt u een groep volume** dialoogvenster wordt weergegeven.
   
    ![Het groepsdialoogvenster van een volume maken](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png)
3. Voer de volgende informatie in:
   
   1. In de **naam** typt u een unieke naam voor de nieuwe volume.
   2. In de **toepassingen** vak, bepaalde toepassingen die zijn gekoppeld aan de volumes die u aan de groep volume toevoegen wilt.
      
       De **toepassingen** alleen die toepassingen die gebruikmaken van StorSimple-volumes en VSS-schrijvers zijn ingeschakeld voor deze in een lijst weergegeven. Een VSS-schrijver is alleen beschikbaar als alle volumes die de writer op de hoogte van is StorSimple-volumes zijn. Als de toepassingen vak leeg is, zijn geen toepassingen die gebruikmaken van Azure StorSimple-volumes en er een ondersteunde VSS-schrijvers van geïnstalleerd zijn. (Momenteel Azure StorSimple ondersteunt Microsoft Exchange en SQL Server.) Zie voor meer informatie over VSS-schrijvers [integratie met Windows Volume Shadow Copy Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).
      
       Als u een toepassing selecteert, worden alle volumes die zijn gekoppeld aan deze automatisch geselecteerd. Als u daarentegen als u volumes die zijn gekoppeld aan een specifieke toepassing selecteert, de toepassing wordt automatisch geselecteerd in de **toepassingen** vak. 
   3. In de **Volumes** Selecteer StorSimple-volumes om toe te voegen aan de groep volume. 
      
      * U kunt volumes met één of meerdere partities opnemen. (Meerdere volumes van de partitie kunnen worden dynamische schijven of standaardschijven met meerdere partities.) Een volume dat meerdere partities bevat wordt behandeld als één eenheid. Dus als u slechts één van de partities aan een groep volume toevoegt, worden alle andere partities automatisch toegevoegd aan die groep volume op hetzelfde moment. Nadat u een volume met meerdere partitie aan een groep volume toevoegen, blijft de meerdere partitie volume worden behandeld als één eenheid.
      * U kunt lege volume groepen maken door toe te wijzen niet alle volumes toe. 
      * Niet door elkaar op gedeelde clustervolumes (CSV's) en niet-CSV in dezelfde groep volume. StorSimple Snapshot Manager biedt geen ondersteuning voor een combinatie van CSV-volumes en niet-CSV-volumes in de dezelfde momentopname.
4. Klik op **OK** om op te slaan van de volume-groep.

## <a name="back-up-a-volume-group"></a>Back-up van een volume-groep
Gebruik de volgende procedure back-up van een volume-groep.

#### <a name="to-back-up-a-volume-group"></a>Back-up van een volume-groep
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, vouw de **Volume groepen** knooppunt met de rechtermuisknop op de naam van een volume-groep en klik vervolgens op **duren back-up**.
   
    ![Back-up van volume-groep onmiddellijk](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)
3. In de **duren back-up** dialoogvenster, **lokale momentopname** of **Cloudmomentopname**, en klik vervolgens op **maken**.
   
    ![Back-dialoogvenster](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png)
4. Om te bevestigen dat de back-up wordt uitgevoerd, vouw de **taken** knooppunt en klik vervolgens op **met**. De back-up moet worden weergegeven.
5. Als u wilt weergeven van de momentopname van de voltooide, vouw de **back-upcatalogus** knooppunt, vouw de naam van het volume en klik vervolgens op **lokale momentopname** of **Cloudmomentopname**. De back-up wordt weergegeven als deze voltooid is.

## <a name="edit-a-volume-group"></a>Een volume-groep bewerken
Gebruik de volgende procedure om een volume-groep te bewerken.

#### <a name="to-edit-a-volume-group"></a>Een volume-groep wilt bewerken
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, vouw de **Volume groepen** knooppunt met de rechtermuisknop op de naam van een volume-groep en klik vervolgens op **bewerken**.
3. De ** maakt u een groep volume ** in het dialoogvenster wordt weergegeven. U kunt wijzigen de **naam**, **toepassingen**, en **Volumes** vermeldingen.
4. Klik op **OK** om uw wijzigingen op te slaan.

## <a name="delete-a-volume-group"></a>Een volume-groep verwijderen
Gebruik de volgende procedure om een volume-groep te verwijderen. 

> [!WARNING]
> Hiermee verwijdert u ook alle back-ups die zijn gekoppeld aan de groep volume.
> 
> 

#### <a name="to-delete-a-volume-group"></a>Een volume-groep verwijderen
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, vouw de **Volume groepen** knooppunt met de rechtermuisknop op de naam van een volume-groep en klik vervolgens op **verwijderen**.
3. De **Volume groep verwijderen** dialoogvenster wordt weergegeven. Type **bevestigen** in het tekstvak en klik vervolgens op **OK**.
   
    De groep verwijderde volume verdwijnt uit de lijst in de **resultaten** deelvenster en alle back-ups die gekoppeld aan die groep volume zijn worden verwijderd uit de back-catalogus.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over hoe [StorSimple Snapshot Manager gebruiken voor het maken en beheren van back-upbeleid](storsimple-snapshot-manager-manage-backup-policies.md).

