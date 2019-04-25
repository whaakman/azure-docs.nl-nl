---
title: Back-upbeleid van StorSimple Snapshot Manager | Microsoft Docs
description: Hierin wordt beschreven hoe u de StorSimple Snapshot Manager MMC-module om te maken en beheren van de back-upbeleid dat geplande back-ups beheren.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 7dac26b058b959011e38b4373369b8a1115d2705
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60303220"
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>StorSimple Snapshot Manager gebruiken om te maken en back-upbeleid beheren
## <a name="overview"></a>Overzicht
Een back-upbeleid maakt een schema voor back-ups van volumegegevens lokaal of in de cloud. Wanneer u een back-upbeleid maakt, kunt u ook een bewaarbeleid opgeven. (U kunt maximaal 64 momentopnamen behouden). Zie voor meer informatie over back-upbeleid [back-up van typen](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) in [StorSimple 8000-serie: een hybride cloudoplossing](storsimple-overview.md).

In deze zelfstudie wordt het volgende uitgelegd:

* Maak een back-upbeleid
* Een back-upbeleid bewerken
* Verwijderen van een back-upbeleid

## <a name="create-a-backup-policy"></a>Maak een back-upbeleid
Gebruik de volgende procedure om te maken van een nieuwe back-upbeleid.

#### <a name="to-create-a-backup-policy"></a>Een back-upbeleid maken
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** in het deelvenster met de rechtermuisknop op **back-upbeleid**, en klikt u op **maken back-upbeleid**.

    ![Maak een back-upbeleid](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    De **maken van een beleid** in het dialoogvenster wordt weergegeven.

    ![Maak een beleid - tabblad Algemeen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. Op de **algemene** tabblad, voert u de volgende informatie:

   1. In de **naam** in het tekstvak, typ een naam voor het beleid.
   2. In de **volumegroep** in het tekstvak de naam van de volumegroep die is gekoppeld aan het beleid.
   3. Selecteer een **lokale momentopname** of **Cloud-momentopname**.
   4. Selecteer het aantal momentopnamen te behouden. Als u selecteert **alle**, 64 momentopnamen worden bewaard (maximum).
4. Klik op de **planning** tabblad.

    ![Maak een beleid - tabblad Planning](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. Op de **planning** tabblad, voert u de volgende informatie:

   1. Klik op de **inschakelen** selectievakje in voor het plannen van de volgende back-up.
   2. Onder **instellingen**, selecteer **één keer**, **dagelijkse**, **wekelijkse**, of **maandelijkse**.
   3. In de **Start** tekstvak, klik op het kalenderpictogram en selecteer een begindatum.
   4. Onder **geavanceerde instellingen**, kunt u optioneel herhalingen schema's en een einddatum instellen.
   5. Klik op **OK**.

Nadat u een back-upbeleid hebt gemaakt, de volgende informatie wordt weergegeven in de **resultaten** deelvenster:

* **Naam** – de naam van de back-upbeleid.
* **Type** : lokale momentopname of cloud-momentopname.
* **Volumegroep** – de volumegroep die is gekoppeld aan het beleid.
* **Retentie** : het aantal momentopnamen worden bewaard; de maximumwaarde is 64.
* **Gemaakt** : de datum waarop dit beleid is gemaakt.
* **Ingeschakeld** : bepaalt of het beleid momenteel van kracht is: **De waarde True** aangeeft dat het van kracht; **False** geeft aan dat het is niet van kracht.

## <a name="edit-a-backup-policy"></a>Een back-upbeleid bewerken
Gebruik de volgende procedure om te bewerken van een bestaande back-upbeleid.

#### <a name="to-edit-a-backup-policy"></a>Een back-upbeleid bewerken
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, klikt u op de **back-upbeleid** knooppunt. Het back-upbeleid worden weergegeven in de **resultaten** deelvenster.
3. Met de rechtermuisknop op het beleid dat u wilt bewerken, en klik vervolgens op **bewerken**.

    ![Een back-upbeleid bewerken](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. Wanneer de **maken van een beleid** venster wordt weergegeven, typt u de wijzigingen, en klik vervolgens op **OK**.

## <a name="delete-a-backup-policy"></a>Verwijderen van een back-upbeleid
Gebruik de volgende procedure om te verwijderen van een back-upbeleid.

#### <a name="to-delete-a-backup-policy"></a>Verwijderen van een back-upbeleid
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, klikt u op de **back-upbeleid** knooppunt. Het back-upbeleid worden weergegeven in de **resultaten** deelvenster.
3. Met de rechtermuisknop op de back-upbeleid dat u wilt verwijderen, en klik vervolgens op **verwijderen**.
4. Wanneer het bevestigingsbericht wordt weergegeven, klikt u op **Ja**.

    ![Back-upbeleid bevestiging van verwijderen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over het [StorSimple Snapshot Manager gebruiken voor het weergeven en beheren van back-uptaken](storsimple-snapshot-manager-manage-backup-jobs.md).
