---
title: StorSimple-failover, herstel na noodgevallen naar een fysiek apparaat voor StorSimple 8000-reeks | Microsoft Docs
description: Leer hoe u uw fysieke StorSimple 8000-serie-apparaat naar een ander fysiek apparaat een failover.
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
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 5fcf95a1a3033a5150945dbd841f12d50ebb023b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60577180"
---
# <a name="fail-over-to-a-storsimple-8000-series-physical-device"></a>Failover naar een fysiek apparaat voor StorSimple 8000-serie

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u failover wilt uitvoeren voor een fysiek StorSimple 8000-serie-apparaat met een andere fysieke StorSimple-apparaat als er zich een noodgeval voordoet. StorSimple maakt gebruik van de functie van de failover-apparaat om gegevens te migreren vanaf een fysiek apparaat van de bron in het datacenter naar een ander fysiek apparaat. De instructies in deze zelfstudie is van toepassing op StorSimple 8000-serie fysieke apparaten met versies van de software Update 3 en hoger.

Voor meer informatie over de failover van het apparaat en hoe deze wordt gebruikt om te herstellen na een noodgeval, gaat u naar [Failover en herstel na noodgevallen voor apparaten uit de StorSimple 8000-serie](storsimple-8000-device-failover-disaster-recovery.md).

Voor een fysieke StorSimple-apparaat met een StorSimple-Cloudapparaat failovers, gaat u naar [failover naar een StorSimple Cloud Appliance](storsimple-8000-device-failover-cloud-appliance.md). Als u wilt een fysiek apparaat naar zichzelf failover, gaat u naar [fungeren als failover voor de dezelfde fysieke StorSimple-apparaat](storsimple-8000-device-failover-same-device.md).


## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u de aandachtspunten voor failover van het apparaat hebt bekeken. Ga voor meer informatie naar [algemene overwegingen voor failover van het apparaat](storsimple-8000-device-failover-disaster-recovery.md).

- Hebt u een StorSimple 8000-serie fysiek apparaat in het datacenter zijn geïmplementeerd. Het apparaat moet uitvoeren Update 3 of hoger, software. Ga voor meer informatie naar [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).


## <a name="steps-to-fail-over-to-a-physical-device"></a>Stappen om een failover uitvoeren naar een fysiek apparaat

De volgende stappen uitvoeren om uw apparaat herstellen naar een fysiek apparaat.

1. Controleer of dat de volumecontainer die u failover wilt uitvoeren, cloudmomentopnamen is gekoppeld. Ga voor meer informatie naar [gebruik StorSimple Device Manager-service voor het maken van back-ups](storsimple-8000-manage-backup-policies-u2.md).
2. Ga naar uw StorSimple Device Manager en klik vervolgens op **apparaten**. In de **apparaten** blade, Ga naar de lijst met apparaten die zijn verbonden met uw service.
    ![Apparaat selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)
3. Selecteer en klik op uw Bronapparaat. Het bronapparaat heeft de volumecontainers die u wilt een failover. Ga naar **instellingen > Volumecontainers**.
4. Selecteer een volumecontainer die u een failover wilt uitvoeren naar een ander apparaat. Klik op de volumecontainer om de lijst van volumes in deze container weer te geven. Selecteer een volume, klik met de rechtermuisknop en klikt u op **Offline nemen** offline te nemen het volume. Herhaal dit proces voor alle volumes in de volumecontainer.
5. Herhaal de vorige stap voor alle volumecontainers die u een failover wilt uitvoeren naar een ander apparaat.
6. Ga terug naar de **apparaten** blade. Klik in de opdrachtbalk op **failover**.
    ![Klik op mislukt via](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)
    
7. In de **failover** blade, voer de volgende stappen uit:
   
   1. Klik op **bron**. De volumecontainers met volumes die zijn gekoppeld met behulp van cloudmomentopnamen worden weergegeven. Alleen de containers die worden weergegeven, komen in aanmerking voor failover. Selecteer in de lijst met volumecontainers, de volumecontainers die u wilt een failover. **Alleen de volumecontainers met gekoppelde cloud-momentopnamen en offline volumes worden weergegeven.**

       ![Bron selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev5.png)
   2. Klik op **doel**. Voor de volumecontainers die in de vorige stap hebt geselecteerd, selecteert u een apparaat uit de vervolgkeuzelijst met beschikbare apparaten. Alleen de apparaten die onvoldoende capaciteit voor volumecontainers bron worden weergegeven in de lijst.

        ![Doel selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev6.png)

   3. Controleer ten slotte de failover-instellingen onder **samenvatting**. Nadat u de instellingen hebt doorgenomen, selecteert u het selectievakje waarmee wordt aangegeven dat de volumes in de geselecteerde volumecontainers offline zijn. Klik op **OK**.

       ![Controleer de instellingen van de failover](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev8.png)
  
8. StorSimple maakt een failovertaak. Klik op de melding van de taak voor het bewaken van de failovertaak via de **taken** blade.

    Als de volumecontainer waarvoor u een failover lokale volumes heeft, ziet u aparte herstelbewerking taken voor alle lokale volumes (niet voor gelaagde volumes) in de container. Deze herstellen taken kunnen geruime tijd duren om uit te voeren. Is het waarschijnlijk dat de failovertaak eerder kan voltooien. Deze volumes moet lokale garanties alleen nadat het herstellen van taken voltooid zijn.

    ![Failover-taak controleren](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

9. Nadat de failover is voltooid, gaat u naar de **apparaten** blade.
   
   1. Selecteer het apparaat dat is gebruikt als het doelapparaat voor de failoverproces.

       ![Apparaat selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

   2. Ga naar de **Volumecontainers** blade. Alle volumecontainers, samen met de volumes van het oude apparaat moeten worden vermeld.

       ![Weergave doel volumecontainers](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev16.png)


## <a name="next-steps"></a>Volgende stappen

* Nadat u een failover hebt uitgevoerd, moet u mogelijk [deactiveren of verwijderen van uw StorSimple-apparaat](storsimple-8000-deactivate-and-delete-device.md).
* Voor informatie over het gebruik van de StorSimple Device Manager-service, gaat u naar [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

