---
title: Microsoft Authenticator-app voor mobiele telefoons | Microsoft Docs
description: Informatie over het upgraden naar de nieuwste versie van Azure Authenticator.
services: multi-factor-authentication
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: barlan
ms.reviewer: librown
ms.custom: H1Hack27Feb2017, end-user
ms.openlocfilehash: 0d293833b97b2a65d5377eef668696cb73ee3bd5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="get-started-with-the-microsoft-authenticator-app"></a>Aan de slag met de Microsoft Authenticator-app
De Microsoft Authenticator-app biedt een extra beveiligingsniveau in uw werk of school-account (bijvoorbeeld bsimon@contoso.com) of uw Microsoft-account (bijvoorbeeld bsimon@outlook.com).

De app werkt op twee manieren:

* **Melding**. De app kan helpen voorkomen dat onbevoegde toegang tot accounts en frauduleuze transacties stoppen door een melding naar uw smartphone of tablet worden gepusht. Gewoon de melding weergeven en als deze geldig is, selecteert u **controleren**. Anders kunt u **weigeren**.
* **Verificatiecode**. De app kan worden gebruikt als een Softwaretoken voor het genereren van een verificatiecode OAuth. Nadat u uw gebruikersnaam en wachtwoord hebt ingevoerd, voert u de code die wordt geleverd door de app in het aanmeldingsscherm. De verificatiecode biedt een tweede vorm van verificatie.

De Microsoft Authenticator-app wordt vervangen door de Azure Authenticator-app. De Azure Authenticator-app werkt nog altijd, maar als u besluit om naar de nieuwe Microsoft Authenticator-app te verplaatsen, in dit artikel kan u helpen.  

## <a name="opt-in-for-two-step-verification"></a>Opt-in voor verificatie in twee stappen

De Microsoft Authenticator-app werkt niet op zichzelf. Elk van uw accounts vraagt u om een tweede verificatiemethode nadat u zich met uw gebruikersnaam en wachtwoord aanmelden configureren.

Om een werk- of schoolaccount krijgt u niet normaal gesproken kiest u deze functie voor uzelf. In plaats daarvan een beveiligingsbeheerder kiest namens jou en verwittigt vervolgens registreren verificatiemethoden voor uw account. Meer informatie in dit scenario is van toepassing op u, [wat Azure multi-factor Authentication betekent voor mij](multi-factor-authentication-end-user.md).

Voor een persoonlijke account moet u de verificatie in twee stappen voor uzelf instellen. Als u een Microsoft-account hebt, deze stappen zijn beschikbaar in [over verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

U kunt ook de Microsoft-Authenticator gebruiken met niet-Microsoft-accounts. Ze kunnen de functie iets anders dan verificatie in twee stappen aanroepen, maar moet u kunnen vinden onder beveiligings- of instellingen.

## <a name="install-the-app"></a>De app installeren
De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), en [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="add-accounts-to-the-app"></a>Accounts toevoegen aan de app.
Voor elke account die u wilt toevoegen aan de Microsoft Authenticator-app, gebruikt u een van de volgende procedures:

### <a name="add-a-personal-microsoft-account-to-the-app"></a>Een persoonlijk Microsoft-account toevoegen aan de app.

Voor een persoonlijk Microsoft-account (één waarmee u zich aanmeldt bij Outlook.com, Xbox, Skype, enzovoort), is hoeft u zich aanmelden bij uw account in de Microsoft Authenticator-app.

### <a name="add-a-work-or-school-account-to-the-app-using-the-qr-code-scanner"></a>Een account voor werk of school toevoegen aan de app via de scanner QR-code
1. Ga naar het scherm beveiliging verificatie-instellingen.  Zie voor informatie over hoe u naar dit scherm, [wijzigen van uw beveiligingsinstellingen](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).
2. Schakel het selectievakje in naast **verificator-app** Selecteer **configureren**.

    ![De knop configureren op het scherm beveiliging verificatie-instellingen](./media/authenticator-app-how-to/azureauthe.png)

    Hiermee wordt een scherm met een QR-code erop.

    ![Scherm waarmee de QR-code](./media/authenticator-app-how-to/barcode2.png)
3. Open de Microsoft Authenticator-app. Op de **accounts** Schakel in het scherm  **+** , en geef vervolgens dat u wilt een werk- of schoolaccount toevoegen.
4. Gebruik van de camera moet scan de QR-code en selecteer vervolgens **gedaan** te sluiten van het scherm QR-code.

    Als uw camera niet goed werkt is, kunt u [de QR-code en URL handmatig invoeren](#add-an-account-to-the-app-manually).

5. Als de app wordt de naam van uw account een 6-cijferige code eronder weergegeven, kunt u klaar bent.

    ![Accounts scherm](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Een account handmatig toevoegen aan de app.
1. Ga naar het scherm beveiliging verificatie-instellingen.  Zie voor informatie over hoe u naar dit scherm, [wijzigen van uw beveiligingsinstellingen](multi-factor-authentication-end-user-manage-settings.md).
2. Selecteer **configureren**.

    ![De knop configureren op het scherm beveiliging verificatie-instellingen](./media/authenticator-app-how-to/azureauthe.png)

    Hiermee wordt een scherm met een QR-code erop.  Noteer de code en URL.

    ![Scherm waarmee de QR-code en URL](./media/authenticator-app-how-to/barcode2.png)
3. Open de Microsoft Authenticator-app. Op de **accounts** Schakel in het scherm  **+** , en geef vervolgens dat u wilt een werk- of schoolaccount toevoegen.

4. Selecteer in de scanner **code handmatig invoeren**.

    ![Scherm voor het scannen van QR-code](./media/multi-factor-authentication-end-user-first-time/scan2.png)
5. Voer de code en de URL in de desbetreffende vakken in de app en selecteer **voltooien**.

    ![Scherm voor het invoeren van code en URL](./media/authenticator-app-how-to/manual.png)

6. Als de app wordt de naam van uw account een 6-cijferige code eronder weergegeven, kunt u klaar bent.

    ![Accounts scherm](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-using-your-devices-fingerprint-or-facial-recognition-capabilities"></a>Een account toevoegen aan de app met behulp van de vingerafdruk of gezichtsherkenning mogelijkheden van uw apparaat
Uw organisatie mogelijk een PINCODE vereisen voor de uitdaging verificatie te voltooien. De Microsoft Authenticator-app kunt gebruiken de vingerafdruk of gezichtsherkenning mogelijkheden van uw apparaat in plaats van een PINCODE. Als u wilt dit instellen op uw eerste verificatie in de app, ziet u een optie voor het gebruik van Touch ID (voor iOS) of identificatie in plaats daarvan vingerafdruk. 

Als u Touch ID voor Microsoft Authenticator instelt, moet u een challenge normale verificatie met een PINCODE te voltooien. Microsoft Authenticator wordt automatisch instellen voor apparaten die ondersteuning bieden voor Touch-ID. 

![Verificatie van Touch ID setup](./media/authenticator-app-how-to/touchid1.png)

Vanaf dat punt doorsturen, wanneer u klaar om te controleren of uw aanmelden vereist, u de pushmelding ontvangen selecteren en scannen van uw vingerafdruk in plaats van uw pincode.

![Push-melding](./media/authenticator-app-how-to/touchid2.png)

## <a name="use-the-app-when-you-sign-in"></a>De app gebruiken wanneer u zich aanmeldt

Wanneer uw account is toegevoegd aan de app, wordt u mogelijk gevraagd te doen een testverificatie om te controleren of dat alles correct is geconfigureerd. Daarna kunt bent nu u klaar! U hoeft niet te iets anders doen totdat de volgende keer dat u zich aanmeldt.

Als u ervoor verificatie te gebruiken in de app kiest, start u deze op de startpagina te zien. Ze wijzigen elke 30 seconden zodat u altijd een nieuwe code hebt wanneer u een nodig. Maar u hoeft verder niets te doen met hen totdat u zich aanmeldt, wordt gevraagd een verificatiecode invoeren.  
