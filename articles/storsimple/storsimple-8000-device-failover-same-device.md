---
title: StorSimple-failover, herstel na noodgevallen voor apparaten 8000 serie | Microsoft Docs
description: Ontdek hoe u uw StorSimple-apparaat op hetzelfde apparaat een failover.
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
ms.date: 06/23/2017
ms.author: alkohli
ms.openlocfilehash: acc8929dc3476e9590e8e4d9526b38b7c0719570
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="fail-over-your-storsimple-physical-device-to-same-device"></a>Failover van uw fysieke StorSimple-apparaat op hetzelfde apparaat

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt beschreven hoe u via een fysiek StorSimple 8000 serie-apparaat naar zichzelf mislukken als er een noodgeval. De failoverfunctie van de apparaten StorSimple gebruikt om gegevens te migreren vanaf een fysiek Bronapparaat in het datacenter naar een andere fysieke apparaat. De instructies in deze zelfstudie is van toepassing op StorSimple 8000-serie fysieke apparaten met versies van de software Update 3 en hoger.

Voor meer informatie over failover-apparaat en hoe deze wordt gebruikt voor het herstellen van een noodgeval, gaat u naar [Failover en herstel na noodgevallen voor StorSimple 8000 series apparaten](storsimple-8000-device-failover-disaster-recovery.md).

Failover van een fysiek apparaat met een andere fysieke apparaat, gaat u naar [failover naar de dezelfde fysieke StorSimple-apparaat](storsimple-8000-device-failover-physical-device.md). Failover van een fysieke StorSimple-apparaat naar een StorSimple-Cloud-apparaat, gaat u naar [failover naar een Cloud StorSimple toestel](storsimple-8000-device-failover-cloud-appliance.md).


## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u de aandachtspunten voor failover-apparaat hebt bekeken. Ga voor meer informatie naar [algemene overwegingen voor het apparaat failover](storsimple-8000-device-failover-disaster-recovery.md).


## <a name="steps-to-fail-over-to-the-same-device"></a>Stappen failover naar hetzelfde apparaat

De volgende stappen uitvoeren als u failover naar hetzelfde apparaat nodig.

1. Momentopnamen cloud van alle volumes op uw apparaat. Ga voor meer informatie naar [gebruik StorSimple Apparaatbeheer service voor het maken van back-ups](storsimple-8000-manage-backup-policies-u2.md).
2. Uw apparaat terugzetten op fabrieksinstellingen. Volg de gedetailleerde instructies in [hoe een StorSimple-apparaat opnieuw instelt fabrieksinstellingen](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. Ga naar de service Manager voor StorSimple-apparaat en selecteer vervolgens **apparaten**. In de **apparaten** blade het oude apparaat moet worden weergegeven als **Offline**.

    ![Offline Bronapparaat](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. Uw apparaat configureren en het opnieuw registreren met uw StorSimple-apparaat Manager-service. Het nieuw ingeschreven apparaat moet worden weergegeven als **gereed is voor het instellen van**. De naam van het apparaat voor het nieuwe apparaat is hetzelfde als het oude apparaat maar toegevoegd met een cijfer om aan te geven dat het apparaat is opnieuw ingesteld op de standaardinstelling en opnieuw geregistreerd.

    ![Nieuw ingeschreven apparaat gereed is om in te stellen](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. Voltooi de installatie van het apparaat voor het nieuwe apparaat. Ga voor meer informatie naar [stap 4: minimale apparaatconfiguratie voltooien](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup). Op de **apparaten** blade, de status van het apparaat gewijzigd in **Online**.

   > [!IMPORTANT]
   > **De minimale configuratie eerst te voltooien of uw DR mislukken.**

    ![Nieuw ingeschreven apparaat online is](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. Selecteer het oude apparaat (offline status) en klik op de opdrachtbalk **failover**. In de **failover** blade oude apparaat selecteren als bron en het doelapparaat opgeven als het nieuw ingeschreven apparaat.

    ![Overzicht van failover](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    Raadpleeg voor gedetailleerde instructies [failover naar een ander fysiek apparaat](#fail-over-to-another-physical-device).

7. Een hersteltaak van het apparaat wordt gemaakt dat u vanaf bewaken kunt de **taken** blade.

8. Nadat de taak is voltooid, toegang tot het nieuwe apparaat en navigeer naar de **volumecontainers** blade. Controleren of de volumecontainers van het oude apparaat hebt gemigreerd naar het nieuwe apparaat.

   ![Volumecontainers gemigreerd](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. Nadat de failover voltooid is, kunt u deze kunt deactiveren en verwijderen van het oude apparaat via de portal. Selecteer de oude apparaat (offline), klik met de rechtermuisknop en selecteer vervolgens **deactiveren**. Nadat het apparaat is gedeactiveerd, wordt de status van het apparaat is bijgewerkt.

     ![Het bronvolume is gedeactiveerd](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. Selecteer de gedeactiveerde apparaat, klik met de rechtermuisknop en selecteer vervolgens **verwijderen**. Hiermee verwijdert u het apparaat uit de lijst met apparaten.

    ![Het bronvolume is verwijderd](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a>Volgende stappen

* Nadat u hebt een failover uitgevoerd, moet u mogelijk [deactiveren of verwijderen van uw StorSimple-apparaat](storsimple-8000-deactivate-and-delete-device.md).
* Voor informatie over het gebruik van de service Manager voor StorSimple-apparaat, gaat u naar [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

