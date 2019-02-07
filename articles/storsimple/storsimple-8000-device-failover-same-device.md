---
title: StorSimple-failover, herstel na noodgevallen voor apparaten uit de 8000-serie | Microsoft Docs
description: Leer hoe u uw StorSimple-apparaat op hetzelfde apparaat een failover.
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
ms.date: 06/23/2017
ms.author: alkohli
ms.openlocfilehash: dd207eaad1a3e821724d51a890d0882bfffda131
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55809058"
---
# <a name="fail-over-your-storsimple-physical-device-to-same-device"></a>Uw fysieke StorSimple-apparaat op hetzelfde apparaat een failover

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u failover wilt uitvoeren voor een fysiek StorSimple 8000-serie-apparaat naar zichzelf als er zich een noodgeval voordoet. StorSimple maakt gebruik van de functie van de failover-apparaat om gegevens te migreren vanaf een fysiek apparaat van de bron in het datacenter naar een ander fysiek apparaat. De instructies in deze zelfstudie is van toepassing op StorSimple 8000-serie fysieke apparaten met versies van de software Update 3 en hoger.

Voor meer informatie over de failover van het apparaat en hoe deze wordt gebruikt om te herstellen na een noodgeval, gaat u naar [Failover en herstel na noodgevallen voor apparaten uit de StorSimple 8000-serie](storsimple-8000-device-failover-disaster-recovery.md).

Als u wilt een fysiek apparaat met een ander fysiek apparaat een failover, gaat u naar [fungeren als failover voor de dezelfde fysieke StorSimple-apparaat](storsimple-8000-device-failover-physical-device.md). Voor een fysieke StorSimple-apparaat met een StorSimple-Cloudapparaat failovers, gaat u naar [failover naar een StorSimple Cloud Appliance](storsimple-8000-device-failover-cloud-appliance.md).


## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u de aandachtspunten voor failover van het apparaat hebt bekeken. Ga voor meer informatie naar [algemene overwegingen voor failover van het apparaat](storsimple-8000-device-failover-disaster-recovery.md).


## <a name="steps-to-fail-over-to-the-same-device"></a>Stappen voor het op hetzelfde apparaat een failover

De volgende stappen uitvoeren als u wilt een failover naar hetzelfde apparaat.

1. Maak cloud Momentopnames van alle volumes op uw apparaat. Ga voor meer informatie naar [gebruik StorSimple Device Manager-service voor het maken van back-ups](storsimple-8000-manage-backup-policies-u2.md).
2. Uw apparaat terugzetten op fabrieksinstellingen. Volg de gedetailleerde instructies in [opnieuw instellen van een StorSimple-apparaat fabrieksinstellingen](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. Ga naar de StorSimple Device Manager-service en selecteer vervolgens **apparaten**. In de **apparaten** blade het oude apparaat moet worden weergegeven als **Offline**.

    ![Bronapparaat offline](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. Uw apparaat configureren en registreer deze opnieuw met uw StorSimple Device Manager-service. Het nieuw ingeschreven apparaat moet worden weergegeven als **klaar om in te stellen**. De naam van het apparaat voor het nieuwe apparaat is hetzelfde als het oude apparaat maar toegevoegd met een cijfer om aan te geven dat het apparaat is opnieuw ingesteld op de standaardinstelling en opnieuw geregistreerd.

    ![Nieuw ingeschreven apparaat gereed voor configuratie](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. Voltooi de installatie van het apparaat voor het nieuwe apparaat. Ga voor meer informatie naar [stap 4: Minimale apparaatconfiguratie voltooien](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup). Op de **apparaten** blade, de status van het apparaat gewijzigd in **Online**.

   > [!IMPORTANT]
   > **De minimale configuratie eerst voltooien of herstel na Noodgeval mislukken.**

    ![Nieuw ingeschreven apparaat online](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. Selecteer het oude apparaat (offline status) en in de opdrachtbalk op **failover**. In de **failover** blade oude apparaat selecteren als de bron en het doelapparaat opgeven als het nieuw ingeschreven apparaat.

    ![Overzicht van failover](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    Voor gedetailleerde instructies Raadpleeg niet worden uitgevoerd via een ander fysiek apparaat.

7. Een hersteltaak van het apparaat wordt gemaakt dat u van bewaken kunt de **taken** blade.

8. Nadat de taak is voltooid, toegang tot het nieuwe apparaat en navigeer naar de **volumecontainers** blade. Controleren of de volumecontainers van het oude apparaat hebt gemigreerd naar het nieuwe apparaat.

   ![Gemigreerde volumecontainers](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. Nadat de failover voltooid is, kunt u deactiveren en verwijderen van het oude apparaat vanuit de portal. Selecteer het oude apparaat (offline), klik met de rechtermuisknop en selecteer vervolgens **deactiveren**. Nadat het apparaat is gedeactiveerd, wordt de status van het apparaat wordt bijgewerkt.

     ![Bronapparaat zijn gedeactiveerd](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. Selecteer het gedeactiveerde apparaat, klik met de rechtermuisknop en selecteer vervolgens **verwijderen**. Hiermee verwijdert u het apparaat uit de lijst met apparaten.

    ![Bronapparaat verwijderd](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a>Volgende stappen

* Nadat u een failover hebt uitgevoerd, moet u mogelijk [deactiveren of verwijderen van uw StorSimple-apparaat](storsimple-8000-deactivate-and-delete-device.md).
* Voor informatie over het gebruik van de StorSimple Device Manager-service, gaat u naar [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

