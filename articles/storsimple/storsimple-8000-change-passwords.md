---
title: Uw StorSimple-wachtwoorden wijzigen | Microsoft Docs
description: Beschrijft hoe de Apparaatbeheer StorSimple-service gebruiken om uw StorSimple Snapshot Manager en apparaat administrator-wachtwoord wijzigen.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 7762f8499c67672f0a2ffed99e98baea4c940fa0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-change-your-storsimple-passwords"></a>De service StorSimple Apparaatbeheer gebruiken om uw StorSimple-wachtwoorden wijzigen

## <a name="overview"></a>Overzicht
De Azure-portal **apparaatinstellingen** optie bevat alle apparaatparameters die u kunt opnieuw configureren op een StorSimple-apparaat dat wordt beheerd door een StorSimple-apparaat Manager-service. Deze zelfstudie wordt uitgelegd hoe u kunt de **beveiliging** onder de optie **apparaatinstellingen** om de apparaatbeheerder van uw of StorSimple Snapshot Manager-wachtwoord te wijzigen.

## <a name="change-the-device-administrator-password"></a>Het beheerderswachtwoord voor het apparaat wijzigen
Wanneer u Windows PowerShell-interface gebruiken voor toegang tot het StorSimple-apparaat, hoeft u een apparaat administrator-wachtwoord moeten invoeren. Als de eerste StorSimple-apparaat is geregistreerd met een service, wordt het standaardwachtwoord voor deze interface is *Wachtwoord1*. Voor de beveiliging van uw gegevens moet u dit wachtwoord aan het einde van het registratieproces wijzigen. U kunt niet afsluiten van het registratieproces zonder dat u dit wachtwoord wijzigt. Zie voor meer informatie [stap 3: configureren en registreren van het apparaat via Windows PowerShell voor StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Het wachtwoord dat u eerst via de Windows PowerShell-interface is ingesteld tijdens de registratie kan later worden gewijzigd via de Azure-portal. De volgende stappen uitvoeren om te wijzigen van het beheerderswachtwoord van het apparaat.

#### <a name="to-change-the-device-administrator-password"></a>Het beheerderswachtwoord van het apparaat wijzigen
1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**.

2. Selecteer in de lijst in tabelvorm van apparaten, en klik op het apparaat waarvan u wilt wijzigen wachtwoord.

    ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. In de **instellingen** blade, gaat u naar **apparaatinstellingen > beveiliging**.

    ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. In de **beveiligingsinstellingen** blade, klikt u op **wachtwoord** wijzigen van het beheerderswachtwoord van het apparaat.

    ![](./media/storsimple-8000-change-passwords/changepwd3.png)

5. In de **wachtwoord** blade bieden een administrator-wachtwoord tussen 8 en 15 tekens bevat. Het wachtwoord moet een combinatie van 3 of meer van hoofdletters, kleine letters, numerieke en speciale tekens.

6. Bevestig het wachtwoord.

    ![](./media/storsimple-8000-change-passwords/changepwd4.png)

7. Klik op **opslaan** wanneer u om bevestiging gevraagd, klikt u op **Ja**.

    ![](./media/storsimple-8000-change-passwords/changepwd6.png)

Het beheerderswachtwoord van het apparaat moet nu worden bijgewerkt. U kunt dit gewijzigde wachtwoord gebruiken voor toegang tot de Windows PowerShell-interface.

## <a name="set-the-storsimple-snapshot-manager-password"></a>Het wachtwoord voor StorSimple Snapshot Manager instellen
De StorSimple Snapshot Manager-software bevindt zich op uw Windows-host. Met deze software kunnen beheerders de back-ups van uw StorSimple-apparaat beheren. De back-ups zijn in feite momentopnamen die lokaal en in de cloud worden opgeslagen.

Wanneer u een apparaat in StorSimple Snapshot Manager configureert, wordt u gevraagd de IP-adres en het wachtwoord voor verificatie van uw opslagapparaat opgeven.

U kunt instellen of het wachtwoord voor StorSimple Snapshot Manager wijzigen via de Azure-portal. De volgende stappen wilt instellen of wijzigen van het wachtwoord voor StorSimple Snapshot Manager uitvoeren.

#### <a name="to-set-the-storsimple-snapshot-manager-password"></a>De StorSimple Snapshot Manager-wachtwoord instellen
1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**.

2. Selecteer in de lijst in tabelvorm van apparaten, en klik op het apparaat waarvan u wilt instellen of wijzigen van het wachtwoord StorSimple Snapshot Manager.

     ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. In de **instellingen** blade, gaat u naar **apparaatinstellingen > beveiliging**.

     ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. In de **beveiligingsinstellingen** blade, klikt u op **wachtwoord** instellen of wijzigen van het StorSimple Snapshot Manager-wachtwoord.

     ![](./media/storsimple-8000-change-passwords/changepwd3.png) 

5. In de **wachtwoord** blade, voer een wachtwoord 14 of 15 tekens. Zorg ervoor dat het wachtwoord een combinatie van 3 of meer van hoofdletters, kleine letters, numerieke en speciale tekens bevat.

6. Bevestig het wachtwoord.

     ![](./media/storsimple-8000-change-passwords/changepwd5.png)

7. Klik op **opslaan** wanneer u om bevestiging gevraagd, klikt u op **Ja**.

     ![](./media/storsimple-8000-change-passwords/changepwd6.png)

Het wachtwoord voor StorSimple Snapshot Manager moet nu worden bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple security](storsimple-8000-security.md).
* Meer informatie over [wijzigen van de configuratie van uw apparaat](storsimple-8000-modify-device-config.md).
* Meer informatie over [de service Manager voor StorSimple-apparaat gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

