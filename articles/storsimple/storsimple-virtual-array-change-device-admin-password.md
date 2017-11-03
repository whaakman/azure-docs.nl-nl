---
title: Wijziging virtuele StorSimple-matrix apparaatwachtwoord admin | Microsoft Docs
description: Beschrijft hoe de Azure portal of het virtuele StorSimple-matrix webgebruikersinterface gebruiken om te wijzigen van het beheerderswachtwoord van het apparaat.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 11490814-d9fd-4dc7-9c3b-55dd2c23eaf1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 260a23003d705e6598da8c51bb5a96f2539a0014
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="change-the-storsimple-virtual-array-device-administrator-password-via-storsimple-device-manager"></a>Wijzig het beheerderswachtwoord voor virtuele StorSimple-matrix apparaat via de StorSimple-Apparaatbeheer

## <a name="overview"></a>Overzicht

Wanneer u de Windows PowerShell-interface gebruiken voor toegang tot de virtuele StorSimple-matrix, hoeft u een apparaat administrator-wachtwoord moeten invoeren. Wanneer het StorSimple-apparaat eerst is ingericht en is gestart, is het standaardwachtwoord *Wachtwoord1*. De standaardwachtwoord is verlopen, is de eerste keer dat u zich aanmelden als u dit wachtwoord wijzigen zijn vereist voor de beveiliging van uw gegevens.

U kunt ook de lokale webgebruikersinterface of Azure portal gebruiken om te wijzigen van het beheerderswachtwoord van het apparaat op elk gewenst moment nadat het apparaat is geïmplementeerd in uw productieomgeving. Elk van deze procedures wordt beschreven in dit artikel.

 ![Blade voor apparaten](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-the-azure-portal-to-change-the-password"></a>De Azure portal gebruiken om het wachtwoord te wijzigen

De volgende stappen uitvoeren om te wijzigen van het beheerderswachtwoord van het apparaat via de Azure portal.

#### <a name="to-change-the-device-administrator-password-via-the-azure-portal"></a>Het beheerderswachtwoord van het apparaat via de Azure-portal wijzigen

1. Selecteer uw service op de startpagina van de service, dubbelklikt u op de servicenaam en klik vervolgens in de **Management** sectie, klikt u op **apparaten**. Hiermee opent u de **apparaten** blade met een lijst met alle apparaten in uw virtuele StorSimple-matrix.

2. In de **apparaten** blade, dubbelklik op het apparaat waarvoor u een wachtwoord is gewijzigd.

3. In de **instellingen** blade voor uw apparaat, klikt u op **beveiliging**.

4. In de **beveiligingsinstellingen** blade het volgende doen:
   
   1. Schuif omlaag naar de **apparaat beheerderswachtwoord** sectie. Geef een administrator-wachtwoord tussen 8 en 15 tekens bevat.
   2. Bevestig het wachtwoord.
   3. Klik op **opslaan** boven aan de blade.

Het beheerderswachtwoord van het apparaat is nu bijgewerkt. U kunt dit gewijzigde wachtwoord gebruiken voor toegang tot het apparaat lokaal.

![Blade Security-instellingen](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-the-local-web-ui-to-change-the-password"></a>De lokale webgebruikersinterface gebruiken om het wachtwoord te wijzigen

De volgende stappen uitvoeren om te wijzigen van het beheerderswachtwoord van het apparaat via de lokale webgebruikersinterface.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Het beheerderswachtwoord van het apparaat via de lokale webgebruikersinterface wijzigen

1. Klik in de lokale webgebruikersinterface op **onderhoud** > **wachtwoordwijziging** voor uw apparaat.
   
    ![Wachtwoord1 wijzigen](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. Voer de **huidige wachtwoord**.
3. Geef een **nieuw wachtwoord**. Het wachtwoord moet ten minste 8 tekens lang zijn. Deze moet 3 van 4 van de volgende waarden bevatten: hoofdletters, kleine letters, numerieke en speciale tekens.
   
    Houd er rekening mee dat uw wachtwoord mag niet hetzelfde zijn als de laatste 24 wachtwoorden.
4. Geef het wachtwoord ter bevestiging opnieuw.
   
    ![Wachtwoord2 wijzigen](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. Klik onder aan de pagina op **toepassen**. Het nieuwe wachtwoord is nu toegepast. Als het wijzigen van het wachtwoord is niet geslaagd, ziet u de volgende fout:
   
    ![Fout bij het](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    Nadat het wachtwoord is bijgewerkt, wordt u gewaarschuwd. U kunt vervolgens dit gewijzigde wachtwoord gebruiken voor toegang tot het apparaat lokaal.


## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe [beheren van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md).

