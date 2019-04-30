---
title: Beheerderswachtwoord voor het apparaat StorSimple Virtual Array wijzigen | Microsoft Docs
description: Beschrijft hoe u de Azure portal of de StorSimple Virtual Array web-UI gebruiken om te wijzigen van het beheerderswachtwoord voor het apparaat.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 11490814-d9fd-4dc7-9c3b-55dd2c23eaf1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5308badf439254062a8aefca1840eb21bc234ace
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60580365"
---
# <a name="change-the-storsimple-virtual-array-device-administrator-password-via-storsimple-device-manager"></a>Wijzigen van de StorSimple Virtual Array beheerderswachtwoord voor het apparaat via de StorSimple-Apparaatbeheerfunctie

## <a name="overview"></a>Overzicht

Wanneer u de Windows PowerShell-interface gebruiken voor toegang tot de StorSimple Virtual Array, moet u een beheerderswachtwoord voor het apparaat invoeren. Wanneer het StorSimple-apparaat eerst is ingericht en is gestart, is het standaardwachtwoord *Wachtwoord1*. De standaardwachtwoord is verlopen, is de eerste keer is dat u zich aanmeldt en u moet dit wachtwoord wijzigen voor de beveiliging van uw gegevens.

U kunt ook de lokale web-UI of de Azure-portal gebruiken om te wijzigen van het beheerderswachtwoord op elk gewenst moment nadat het apparaat is geïmplementeerd in uw productieomgeving. Elk van deze procedures wordt beschreven in dit artikel.

 ![Blade voor apparaten](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-the-azure-portal-to-change-the-password"></a>De Azure portal gebruiken voor het wachtwoord wijzigen

De volgende stappen uitvoeren om het beheerderswachtwoord voor het apparaat via de Azure-portal wijzigen.

#### <a name="to-change-the-device-administrator-password-via-the-azure-portal"></a>Het beheerderswachtwoord voor het apparaat via de Azure-portal wijzigen

1. Selecteer uw service op de startpagina van de service, dubbelklikt u op de naam van de service, en klik vervolgens in de **Management** sectie, klikt u op **apparaten**. Hiermee opent u de **apparaten** blade met een lijst met alle apparaten van uw StorSimple Virtual Array.

2. In de **apparaten** blade, dubbelklik op het apparaat waarvoor u een wijziging van het wachtwoord in.

3. In de **instellingen** blade voor uw apparaat, klikt u op **Security**.

4. In de **beveiligingsinstellingen** blade, doet u het volgende:
   
   1. Schuif omlaag naar de **apparaat beheerderswachtwoord** sectie. Geef een administrator-wachtwoord dat uit 8 tot 15 tekens bevat.
   2. Bevestig het wachtwoord.
   3. Klik op **Opslaan** boven aan de blade.

Het beheerderswachtwoord voor het apparaat is nu bijgewerkt. U kunt deze gewijzigde wachtwoord gebruiken voor toegang tot het apparaat lokaal.

![Blade Security-instellingen](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-the-local-web-ui-to-change-the-password"></a>De lokale webgebruikersinterface gebruiken om het wachtwoord te wijzigen

Voer de volgende stappen uit als u wilt wijzigen van het beheerderswachtwoord voor het apparaat via de lokale webgebruikersinterface.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Het beheerderswachtwoord voor het apparaat via de lokale webgebruikersinterface wijzigen

1. Klik in de lokale webgebruikersinterface op **onderhoud** > **wachtwoordwijziging** voor uw apparaat.
   
    ![Wachtwoord1 wijzigen](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. Voer de **huidige wachtwoord**.
3. Geef een **nieuw wachtwoord**. Het wachtwoord moet ten minste 8 tekens lang zijn. Deze moet 3 van 4 van de volgende waarden bevatten: hoofdletters, kleine letters, cijfers en speciale tekens.
   
    Houd er rekening mee dat uw wachtwoord mag niet gelijk zijn aan de laatste 24 wachtwoorden.
4. Voer het wachtwoord opnieuw in ter bevestiging.
   
    ![Wachtwoord2 wijzigen](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. Aan de onderkant van de pagina, klikt u op **toepassen**. Het nieuwe wachtwoord wordt nu toegepast. Als het wijzigen van het wachtwoord niet is gelukt, ziet u de volgende fout:
   
    ![Wachtwoordfout](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    Nadat het wachtwoord is bijgewerkt, wordt u gewaarschuwd. U kunt vervolgens deze gewijzigde wachtwoord gebruiken voor toegang tot het apparaat lokaal.


## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [beheren van uw StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).

