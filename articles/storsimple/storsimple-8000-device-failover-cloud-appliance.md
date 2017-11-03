---
title: StorSimple-failover, herstel na noodgevallen op een StorSimple Cloud toestel | Microsoft Docs
description: Ontdek hoe u uw fysieke StorSimple 8000 serie-apparaat naar een cloud-apparaat een failover.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: ec8bebf2854e84a37e84b45564e80fc20b63d8d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="fail-over-to-your-storsimple-cloud-appliance"></a>Failover naar de Cloud van de StorSimple-apparaat

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt beschreven hoe u een StorSimple 8000 series fysiek apparaat op een StorSimple Cloud toestel failover als er een noodgeval. De failoverfunctie van de apparaten StorSimple gebruikt om gegevens te migreren vanaf een fysiek Bronapparaat in het datacenter naar een cloud-apparaat in Azure wordt uitgevoerd. De instructies in deze zelfstudie is van toepassing op StorSimple 8000 series fysieke apparaten en apparaten van cloud met versies van de software Update 3 en hoger.

Voor meer informatie over failover-apparaat en hoe deze wordt gebruikt voor het herstellen van een noodgeval, gaat u naar [Failover en herstel na noodgevallen voor StorSimple 8000 series apparaten](storsimple-8000-device-failover-disaster-recovery.md).

Failover van een fysieke StorSimple-apparaat aan een ander fysiek apparaat, gaat u naar [failover naar een fysiek StorSimple-apparaat](storsimple-8000-device-failover-physical-device.md). Failover van een apparaat op zichzelf, gaat u naar [failover naar de dezelfde fysieke StorSimple-apparaat](storsimple-8000-device-failover-same-device.md).

## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u de aandachtspunten voor failover-apparaat hebt bekeken. Ga voor meer informatie naar [algemene overwegingen voor het apparaat failover](storsimple-8000-device-failover-disaster-recovery.md).

- Een StorSimple Cloud toestel gemaakt en geconfigureerd voordat u deze procedure uitvoert, moet u hebben. Als uitgevoerd bijwerken van versie 3 of hoger, kunt u overwegen een 8020 cloud-apparaat voor het herstel na Noodgevallen. Het model 8020 64 TB gebruikt, en Premium-opslag. Ga voor meer informatie naar [implementeren en beheren van een StorSimple-apparaat met Cloud](storsimple-8000-cloud-appliance-u2.md).

## <a name="steps-to-fail-over-to-a-cloud-appliance"></a>Stappen failover naar een cloud-apparaat

De volgende stappen voor het herstellen van het apparaat aan een doel StorSimple Cloud toestel uitvoeren.

1.  Controleer of dat de volumecontainer die u failover wilt uitvoeren cloudmomentopnamen is gekoppeld. Ga voor meer informatie naar [gebruik StorSimple Apparaatbeheer service voor het maken van back-ups](storsimple-8000-manage-backup-policies-u2.md).
2. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**. In de **apparaten** blade, gaat u naar de lijst met apparaten die zijn verbonden met uw service.
    ![Selecteer het apparaat](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)
3. Selecteer en klik op het bronapparaat. Het bronapparaat heeft de volumecontainers die u failover wilt uitvoeren. Ga naar **instellingen > Volumecontainers**.

    ![Selecteer het apparaat](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. Selecteer een volumecontainer die u wilt een failover naar een ander apparaat. Klik op de volumecontainer lijst van volumes in deze container wilt weergeven. Selecteer een volume, klik met de rechtermuisknop en klik op **Offline nemen** offline te nemen het volume.

    ![Selecteer het apparaat](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. Herhaal dit proces voor de volumes in de volumecontainer.

     ![Selecteer het apparaat](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. Herhaal de vorige stap voor alle volumecontainers die u wilt een failover naar een ander apparaat.

7. Ga terug naar de **apparaten** blade. Klik in de opdrachtbalk op **failover**.

    ![Klik op mislukken via](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. In de **failover** blade, voer de volgende stappen uit:
   
    1. Klik op **bron**. Selecteer de volumecontainers failover. **De volumecontainers met momentopnamen van de gekoppelde cloud en offline volumes worden weergegeven.**
        ![Bron selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)
    2. Klik op **doel**. Selecteer een doel-cloud-apparaat in de vervolgkeuzelijst met beschikbare apparaten. **Alleen de apparaten waarvoor voldoende capaciteit voor volumecontainers bron worden weergegeven in de lijst.**

        ![Doel selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. Controleer de failover-instellingen onder **samenvatting** en schakel het selectievakje waarmee wordt aangegeven of de volumes in de geselecteerde volumecontainers offline zijn. 

        ![Failover-instellingen controleren](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. Er wordt een failover-taak gemaakt. Klik op de melding van de taak voor het controleren van de failover-taak.

    ![Monitor voor failover-taak](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. Nadat de failover is voltooid, gaat u terug naar de **apparaten** blade.

    1. Selecteer het apparaat dat is gebruikt als doel voor de failover.

       ![Selecteer het apparaat](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. Klik op **Volumecontainers**. Volumecontainers, samen met de volumes van het oude apparaat moeten worden vermeld.

       Als de volumecontainer waarvoor u failover lokaal volumes vastgemaakte heeft, zijn deze volumes als gelaagde volumes kan niet via. Lokaal vastgemaakte volumes worden niet ondersteund op een StorSimple-apparaat met Cloud.

       ![Doel-volumecontainers weergeven](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a>Volgende stappen

* Nadat u hebt een failover uitgevoerd, moet u mogelijk [deactiveren of verwijderen van uw StorSimple-apparaat](storsimple-8000-deactivate-and-delete-device.md).

* Voor informatie over het gebruik van de service Manager voor StorSimple-apparaat, gaat u naar [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

