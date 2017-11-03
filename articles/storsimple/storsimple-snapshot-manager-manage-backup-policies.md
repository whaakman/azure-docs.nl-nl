---
title: Back-upbeleid StorSimple Snapshot Manager | Microsoft Docs
description: Beschrijft hoe u het StorSimple Snapshot Manager MMC-module maken en beheren van de back-upbeleid dat geplande back-ups beheren.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 218c89e403673c16c72da95aa2c1d685bbed5a86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>Gebruik StorSimple Snapshot Manager maken en beheren van back-upbeleid
## <a name="overview"></a>Overzicht
Een back-upbeleid maakt een schema voor back-ups van gegevens op volume lokaal of in de cloud. Wanneer u een back-upbeleid maakt, kunt u ook een bewaarbeleid opgeven. (U kunt maximaal 64 momentopnamen behouden). Zie voor meer informatie over back-upbeleid [typen back-up](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) in [StorSimple 8000-serie: een oplossing voor hybride cloud](storsimple-overview.md).

Deze zelfstudie wordt uitgelegd hoe:

* Een back-upbeleid maken
* Een back-upbeleid bewerken
* Een back-upbeleid te verwijderen

## <a name="create-a-backup-policy"></a>Een back-upbeleid maken
Gebruik de volgende procedure een nieuwe back-upbeleid maken.

#### <a name="to-create-a-backup-policy"></a>Een back-upbeleid maken
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster met de rechtermuisknop op **back-upbeleid**, en klik op **back-up-beleid maken**.

    ![Een back-upbeleid maken](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    De **een beleid maken** dialoogvenster wordt weergegeven.

    ![Maak een beleid - tabblad Algemeen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. Op de **algemene** tabblad, voert u de volgende informatie:

   1. In de **naam** in het tekstvak, typ een naam voor het beleid.
   2. In de **Volume groep** in het tekstvak de naam van de volume-groep die is gekoppeld aan het beleid.
   3. Selecteer een **lokale momentopname** of **Cloud momentopname**.
   4. Selecteer het aantal momentopnamen wilt bewaren. Als u selecteert **alle**, 64 momentopnamen worden bewaard (maximum).
4. Klik op de **planning** tabblad.

    ![Maak een beleid - tabblad Planning](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. Op de **planning** tabblad, voert u de volgende informatie:

   1. Klik op de **inschakelen** selectievakje in de volgende back-up plannen.
   2. Onder **instellingen**, selecteer **één keer**, **dagelijkse**, **wekelijkse**, of **maandelijkse**.
   3. In de **Start** tekstvak, klik op het pictogram van de kalender en selecteer een begindatum.
   4. Onder **geavanceerde instellingen**, kunt u optioneel herhaaldelijk schema's en een einddatum instellen.
   5. Klik op **OK**.

Nadat u een back-upbeleid hebt gemaakt, de volgende informatie wordt weergegeven in de **resultaten** deelvenster:

* **Naam** : de naam van back-upbeleid.
* **Type** : lokale momentopname of cloudmomentopname van de.
* **Volume groep** – de volume-groep die is gekoppeld aan het beleid.
* **Bewaartermijn** : het aantal momentopnamen worden bewaard; het maximum is 64.
* **Gemaakt** – de datum waarop dit beleid is gemaakt.
* **Ingeschakeld** – of het beleid actief is: **True** geeft aan dat deze van kracht; **False** geeft aan of dit niet van kracht is.

## <a name="edit-a-backup-policy"></a>Een back-upbeleid bewerken
Gebruik de volgende procedure voor het bewerken van een bestaande back-upbeleid.

#### <a name="to-edit-a-backup-policy"></a>Een back-beleid bewerken
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, klikt u op de **back-upbeleid** knooppunt. Het back-upbeleid worden weergegeven in de **resultaten** deelvenster.
3. Met de rechtermuisknop op het beleid dat u wilt bewerken en klik vervolgens op **bewerken**.

    ![Een back-upbeleid bewerken](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. Wanneer de **een beleid maken** venster wordt weergegeven, typt u de wijzigingen en klik vervolgens op **OK**.

## <a name="delete-a-backup-policy"></a>Een back-upbeleid te verwijderen
Gebruik de volgende procedure om te verwijderen van een back-upbeleid.

#### <a name="to-delete-a-backup-policy"></a>Een back-upbeleid te verwijderen
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, klikt u op de **back-upbeleid** knooppunt. Het back-upbeleid worden weergegeven in de **resultaten** deelvenster.
3. Met de rechtermuisknop op de back-upbeleid dat u wilt verwijderen en klik vervolgens op **verwijderen**.
4. Wanneer het bevestigingsbericht wordt weergegeven, klikt u op **Ja**.

    ![Bevestiging van de back-upbeleid te verwijderen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over hoe [StorSimple Snapshot Manager weergeven en beheren van back-uptaken gebruiken](storsimple-snapshot-manager-manage-backup-jobs.md).
