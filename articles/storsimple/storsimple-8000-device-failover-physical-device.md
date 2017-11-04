---
title: StorSimple-failover, herstel na noodgevallen naar een fysiek apparaat voor StorSimple 8000 series | Microsoft Docs
description: Ontdek hoe u uw fysieke StorSimple 8000 serie-apparaat aan een ander fysiek apparaat failover.
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
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: f3ac9545a341fc24ca12c9f2547805d6956cd98a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="fail-over-to-a-storsimple-8000-series-physical-device"></a>Failover naar een fysiek apparaat voor StorSimple 8000-serie

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt beschreven hoe u via een fysiek StorSimple 8000 serie-apparaat aan een andere fysieke StorSimple-apparaat mislukken als er een noodgeval. De failoverfunctie van de apparaten StorSimple gebruikt om gegevens te migreren vanaf een fysiek Bronapparaat in het datacenter naar een andere fysieke apparaat. De instructies in deze zelfstudie is van toepassing op StorSimple 8000-serie fysieke apparaten met versies van de software Update 3 en hoger.

Voor meer informatie over failover-apparaat en hoe deze wordt gebruikt voor het herstellen van een noodgeval, gaat u naar [Failover en herstel na noodgevallen voor StorSimple 8000 series apparaten](storsimple-8000-device-failover-disaster-recovery.md).

Failover van een fysieke StorSimple-apparaat naar een StorSimple-Cloud-apparaat, gaat u naar [failover naar een Cloud StorSimple toestel](storsimple-8000-device-failover-cloud-appliance.md). Een fysiek apparaat naar zichzelf failover, gaat u naar [failover naar de dezelfde fysieke StorSimple-apparaat](storsimple-8000-device-failover-same-device.md).


## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u de aandachtspunten voor failover-apparaat hebt bekeken. Ga voor meer informatie naar [algemene overwegingen voor het apparaat failover](storsimple-8000-device-failover-disaster-recovery.md).

- U moet een StorSimple 8000 series fysiek apparaat geïmplementeerd in het datacenter hebben. Update 3 of hoger, software, moet het apparaat worden uitgevoerd. Ga voor meer informatie naar [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).


## <a name="steps-to-fail-over-to-a-physical-device"></a>Stappen failover naar een fysiek apparaat

De volgende stappen voor het herstellen van uw apparaat op een doelapparaat fysieke uitvoeren.

1. Controleer of dat de volumecontainer die u failover wilt uitvoeren cloudmomentopnamen is gekoppeld. Ga voor meer informatie naar [gebruik StorSimple Apparaatbeheer service voor het maken van back-ups](storsimple-8000-manage-backup-policies-u2.md).
2. Ga naar uw StorSimple-Apparaatbeheer en klik vervolgens op **apparaten**. In de **apparaten** blade, gaat u naar de lijst met apparaten die zijn verbonden met uw service.
    ![Selecteer het apparaat](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)
3. Selecteer en klik op het bronapparaat. Het bronapparaat heeft de volumecontainers die u failover wilt uitvoeren. Ga naar **instellingen > Volumecontainers**.
4. Selecteer een volumecontainer die u wilt een failover naar een ander apparaat. Klik op de volumecontainer lijst van volumes in deze container wilt weergeven. Selecteer een volume, klik met de rechtermuisknop en klik op **Offline nemen** offline te nemen het volume. Herhaal dit proces voor de volumes in de volumecontainer.
5. Herhaal de vorige stap voor alle volumecontainers die u wilt een failover naar een ander apparaat.
6. Ga terug naar de **apparaten** blade. Klik in de opdrachtbalk op **failover**.
    ![Klik op mislukken via](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)
    
7. In de **failover** blade, voer de volgende stappen uit:
   
   1. Klik op **bron**. De volumecontainers met volumes die zijn gekoppeld aan cloudmomentopnamen worden weergegeven. Alleen de containers weergegeven, komen in aanmerking voor failover. Selecteer in de lijst met volumecontainers, de volumecontainers die u wilt een failover. **De volumecontainers met momentopnamen van de gekoppelde cloud en offline volumes worden weergegeven.**

       ![Bron selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev5.png)
   2. Klik op **doel**. Voor de volumecontainers in de vorige stap hebt geselecteerd, selecteert u een doelapparaat uit de vervolgkeuzelijst met beschikbare apparaten. Alleen de apparaten waarvoor voldoende capaciteit voor volumecontainers bron worden weergegeven in de lijst.

        ![Doel selecteren](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev6.png)

   3. Controleer ten slotte de failover-instellingen onder **samenvatting**. Nadat u de instellingen hebt doorgenomen, selecteer het selectievakje waarmee wordt aangegeven of de volumes in de geselecteerde volumecontainers offline zijn. Klik op **OK**.

       ![Failover-instellingen controleren](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev8.png)
  
8. StorSimple wordt een failover-taak gemaakt. Klik op de melding van de taak voor het bewaken van de failover-taak via de **taken** blade.

    Als de volumecontainer waarvoor u failover lokale volumes heeft, raadpleegt u het herstellen van de afzonderlijke taken voor alle lokale volumes (en niet voor gelaagde volumes) in de container. Deze terugzetten taken kunnen geruime tijd duren om te voltooien. Is het waarschijnlijk dat de failover-taak eerder kan voltooien. Deze volumes hebben lokale garanties alleen nadat het herstellen van taken voltooid zijn.

    ![Monitor voor failover-taak](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

9. Nadat de failover is voltooid, gaat u naar de **apparaten** blade.
   
   1. Selecteer het apparaat dat is gebruikt voor het doelapparaat voor de failoverproces.

       ![Selecteer het apparaat](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

   2. Ga naar de **Volumecontainers** blade. Volumecontainers, samen met de volumes van het oude apparaat moeten worden vermeld.

       ![Doel-volumecontainers weergeven](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev16.png)


## <a name="next-steps"></a>Volgende stappen

* Nadat u hebt een failover uitgevoerd, moet u mogelijk [deactiveren of verwijderen van uw StorSimple-apparaat](storsimple-8000-deactivate-and-delete-device.md).
* Voor informatie over het gebruik van de service Manager voor StorSimple-apparaat, gaat u naar [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

