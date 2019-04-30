---
title: Uw StorSimple-wachtwoorden wijzigen | Microsoft Docs
description: Beschrijft hoe u de StorSimple Device Manager-service gebruiken om uw StorSimple Snapshot Manager en apparaat administrator-wachtwoord wijzigen.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: ab874bbdcd47a4bfa9abfba721afa46d0f23a338
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60637962"
---
# <a name="use-the-storsimple-device-manager-service-to-change-your-storsimple-passwords"></a>De StorSimple Device Manager-service gebruiken om uw StorSimple-wachtwoorden wijzigen

## <a name="overview"></a>Overzicht
De Azure-portal **apparaatinstellingen** optie bevat alle apparaatparameters die u kunt opnieuw configureren op een StorSimple-apparaat dat wordt beheerd door een StorSimple Device Manager-service. Deze zelfstudie wordt uitgelegd hoe u kunt de **Security** onder de optie **apparaatinstellingen** om de apparaatbeheerder van uw of StorSimple Snapshot Manager-wachtwoord te wijzigen.

## <a name="change-the-device-administrator-password"></a>Het beheerderswachtwoord voor het apparaat wijzigen
Wanneer u Windows PowerShell-interface gebruiken voor toegang tot het StorSimple-apparaat, moet u een beheerderswachtwoord voor het apparaat invoeren. Als de eerste StorSimple-apparaat is geregistreerd met een service, wordt het standaardwachtwoord voor deze interface is *Wachtwoord1*. U moet dit wachtwoord aan het einde van het registratieproces wijzigen voor de beveiliging van uw gegevens. U kunt niet afsluiten van het registratieproces zonder dat u dit wachtwoord wijzigt. Zie voor meer informatie, [stap 3: Configureren en registreren van het apparaat via Windows PowerShell voor StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Het wachtwoord dat eerst via de Windows PowerShell-interface is ingesteld tijdens de registratie kan later worden gewijzigd via de Azure-portal. De volgende stappen uitvoeren om het beheerderswachtwoord voor het apparaat wijzigen.

#### <a name="to-change-the-device-administrator-password"></a>Het beheerderswachtwoord voor het apparaat wijzigen
1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**.

2. Selecteer in de lijst in tabelvorm met apparaten, en klik op het apparaat waarvan het wachtwoord u van plan bent om te wijzigen.

    ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. In de **instellingen** blade, Ga naar **apparaatinstellingen > beveiliging**.

    ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. In de **beveiligingsinstellingen** blade, klikt u op **wachtwoord** te wijzigen van het beheerderswachtwoord voor het apparaat.

    ![](./media/storsimple-8000-change-passwords/changepwd3.png)

5. In de **wachtwoord** blade bieden een administrator-wachtwoord dat uit 8 tot 15 tekens bevat. Het wachtwoord moet bestaan uit een combinatie van 3 of meer van hoofdletters, kleine letters, cijfers en speciale tekens.

6. Bevestig het wachtwoord.

    ![](./media/storsimple-8000-change-passwords/changepwd4.png)

7. Klik op **opslaan** wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja**.

    ![](./media/storsimple-8000-change-passwords/changepwd6.png)

Het beheerderswachtwoord voor het apparaat moet nu worden bijgewerkt. U kunt deze gewijzigde wachtwoord gebruiken voor toegang tot de Windows PowerShell-interface.

## <a name="set-the-storsimple-snapshot-manager-password"></a>Stel het wachtwoord voor StorSimple Snapshot Manager
De StorSimple Snapshot Manager-software bevindt zich op uw Windows-host. Met deze software kunnen beheerders de back-ups van uw StorSimple-apparaat beheren. De back-ups zijn in feite momentopnamen die lokaal en in de cloud worden opgeslagen.

Wanneer u een apparaat in StorSimple Snapshot Manager configureert, wordt u gevraagd voor de IP-apparaatadres en het wachtwoord voor verificatie van uw opslagapparaat.

U kunt instellen of wijzigen van het wachtwoord voor StorSimple Snapshot Manager via de Azure-portal. De volgende stappen uitvoeren om het StorSimple Snapshot Manager-wachtwoord instellen of wijzigen.

#### <a name="to-set-the-storsimple-snapshot-manager-password"></a>De StorSimple Snapshot Manager-wachtwoord instellen
1. Ga naar de StorSimple-apparaatbeheerservice en klik op **Apparaten**.

2. Selecteer in de lijst in tabelvorm met apparaten, en klik op het apparaat waarvan u wilt instellen of wijzigen van het wachtwoord StorSimple Snapshot Manager.

     ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. In de **instellingen** blade, Ga naar **apparaatinstellingen > beveiliging**.

     ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. In de **beveiligingsinstellingen** blade, klikt u op **wachtwoord** naar de StorSimple Snapshot Manager-wachtwoord instellen of wijzigen.

     ![](./media/storsimple-8000-change-passwords/changepwd3.png) 

5. In de **wachtwoord** blade, Geef een wachtwoord op dat is 14 of 15 tekens. Zorg ervoor dat het wachtwoord een combinatie van 3 of meer hoofdletters, kleine letters, cijfers en speciale tekens bevat.

6. Bevestig het wachtwoord.

     ![](./media/storsimple-8000-change-passwords/changepwd5.png)

7. Klik op **opslaan** wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja**.

     ![](./media/storsimple-8000-change-passwords/changepwd6.png)

Het StorSimple Snapshot Manager-wachtwoord moet nu worden bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple security](storsimple-8000-security.md).
* Meer informatie over [wijzigen van de configuratie van uw apparaat](storsimple-8000-modify-device-config.md).
* Meer informatie over [met de StorSimple Device Manager-service voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

