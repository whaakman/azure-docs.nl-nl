---
title: StorSimple-failover, herstel na noodgevallen naar een StorSimple Cloud Appliance | Microsoft Docs
description: Leer hoe u uw fysieke StorSimple 8000-serie-apparaat met een cloudapparaat failover.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 45c521fd044fa258b8052a3f0de48784cf4160e8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60584324"
---
# <a name="fail-over-to-your-storsimple-cloud-appliance"></a>Een failover uitvoeren naar uw StorSimple-Cloudapparaat

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u failover wilt uitvoeren voor een fysiek StorSimple 8000-serie-apparaat met een StorSimple-Cloudapparaat als er zich een noodgeval voordoet. StorSimple maakt gebruik van de functie van de failover-apparaat om gegevens te migreren vanaf een fysiek apparaat van de bron in het datacenter naar een cloudapparaat in Azure wordt uitgevoerd. De instructies in deze zelfstudie is van toepassing op StorSimple 8000-serie fysieke apparaten en -cloudapparaten met versies van de software Update 3 en hoger.

Voor meer informatie over de failover van het apparaat en hoe deze wordt gebruikt om te herstellen na een noodgeval, gaat u naar [Failover en herstel na noodgevallen voor apparaten uit de StorSimple 8000-serie](storsimple-8000-device-failover-disaster-recovery.md).

Als u wilt een fysiek StorSimple-apparaat naar een ander fysiek apparaat een failover, gaat u naar [failover naar een fysiek StorSimple-apparaat](storsimple-8000-device-failover-physical-device.md). Als u wilt een apparaat naar zichzelf failover, gaat u naar [fungeren als failover voor de dezelfde fysieke StorSimple-apparaat](storsimple-8000-device-failover-same-device.md).

## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u de aandachtspunten voor failover van het apparaat hebt bekeken. Ga voor meer informatie naar [algemene overwegingen voor failover van het apparaat](storsimple-8000-device-failover-disaster-recovery.md).

- Een StorSimple-Cloudapparaat hebt gemaakt en geconfigureerd voordat u deze procedure uitvoert, moet u hebben. Als actief bijwerken van versie 3 of hoger, kunt u overwegen een 8020-cloudapparaat voor het herstel na Noodgevallen. Het model 8020 64 TB heeft en Premium-opslag gebruikt. Ga voor meer informatie naar [implementeren en beheren van een StorSimple Cloud Appliance](storsimple-8000-cloud-appliance-u2.md).

## <a name="steps-to-fail-over-to-a-cloud-appliance"></a>Stappen om een failover uitvoeren naar een cloudapparaat

De volgende stappen uitvoeren om de apparaat herstellen naar een doel voor StorSimple-Cloudapparaat.

1.  Controleer of dat de volumecontainer die u failover wilt uitvoeren, cloudmomentopnamen is gekoppeld. Ga voor meer informatie naar [gebruik StorSimple Device Manager-service voor het maken van back-ups](storsimple-8000-manage-backup-policies-u2.md).
2. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**. In de **apparaten** blade, Ga naar de lijst met apparaten die zijn verbonden met uw service.
    ![Apparaat selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)
3. Selecteer en klik op uw Bronapparaat. Het bronapparaat heeft de volumecontainers die u wilt een failover. Ga naar **instellingen > Volumecontainers**.

    ![Apparaat selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. Selecteer een volumecontainer die u een failover wilt uitvoeren naar een ander apparaat. Klik op de volumecontainer om de lijst van volumes in deze container weer te geven. Selecteer een volume, klik met de rechtermuisknop en klikt u op **Offline nemen** offline te nemen het volume.

    ![Apparaat selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. Herhaal dit proces voor alle volumes in de volumecontainer.

     ![Apparaat selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. Herhaal de vorige stap voor alle volumecontainers die u een failover wilt uitvoeren naar een ander apparaat.

7. Ga terug naar de **apparaten** blade. Klik in de opdrachtbalk op **failover**.

    ![Klik op mislukt via](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. In de **failover** blade, voer de volgende stappen uit:
   
    1. Klik op **bron**. Selecteer de volumecontainers voor failover. **Alleen de volumecontainers met gekoppelde cloud-momentopnamen en offline volumes worden weergegeven.**
        ![Bron selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)
    2. Klik op **doel**. Selecteer een doel-cloudapparaat in de vervolgkeuzelijst met beschikbare apparaten. **Alleen de apparaten die onvoldoende capaciteit voor volumecontainers bron worden weergegeven in de lijst.**

        ![Doel selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. Controleer de failover-instellingen onder **samenvatting** en schakel het selectievakje waarmee wordt aangegeven dat de volumes in de geselecteerde volumecontainers offline zijn. 

        ![Controleer de instellingen van de failover](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. Er wordt een failovertaak gemaakt. Klik op de melding van de taak voor het controleren van de failover-taak.

    ![Failover-taak controleren](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. Nadat de failover is voltooid, gaat u terug naar de **apparaten** blade.

    1. Selecteer het apparaat dat is gebruikt als het doel voor de failover.

       ![Apparaat selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. Klik op **Volumecontainers**. Alle volumecontainers, samen met de volumes van het oude apparaat moeten worden vermeld.

       Als de volumecontainer waarvoor u een failover lokaal volumes vastgemaakt is, die volumes failover is uitgevoerd als gelaagde volumes. Lokaal vastgemaakte volumes worden niet ondersteund op een StorSimple Cloud Appliance.

       ![Weergave doel volumecontainers](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a>Volgende stappen

* Nadat u een failover hebt uitgevoerd, moet u mogelijk [deactiveren of verwijderen van uw StorSimple-apparaat](storsimple-8000-deactivate-and-delete-device.md).

* Voor informatie over het gebruik van de StorSimple Device Manager-service, gaat u naar [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

