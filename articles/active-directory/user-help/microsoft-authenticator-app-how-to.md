---
title: Aan de slag met de Microsoft Authenticator-app - Azure Active Directory | Microsoft Docs
description: Leer hoe u een upgrade uitvoert naar de nieuwste versie van Microsoft Authenticator.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: active-directoary
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.reviewer: librown
ms.openlocfilehash: 8afd743f6010822709bf3d49f7a3bbcd51b19edf
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346484"
---
# <a name="get-started-with-the-microsoft-authenticator-app"></a>Aan de slag met de Microsoft Authenticator-app

De Microsoft Authenticator-app te voorkomen dat onbevoegde toegang tot accounts en frauduleuze transacties stoppen door te geven u een extra beveiligingsniveau voor uw werk- of schoolaccount (bijvoorbeeld alain@contoso.com) of uw persoonlijke Microsoft-account (voor bijvoorbeeld alain@outlook.com). U kunt gebruiken deze als een tweede verificatiemethode of als vervanging voor uw wachtwoord bij het gebruik van aanmelden via telefoon. Zie voor meer informatie over het instellen en gebruiken aanmelden via telefoon zonder wachtwoord uitproberen [Meld u aan met uw telefoon, niet uw wachtwoord](microsoft-authenticator-app-phone-signin-faq.md).

Wanneer u de app voor verificatie in twee stappen, kan dit werkt op twee manieren:

- **Melding over.** De app verzendt een melding naar uw apparaat. Zorg ervoor dat de melding is rechts en selecteer vervolgens **controleren**. Als u de melding niet herkent, selecteert u **weigeren**.

- **Verificatiecode.** Nadat u uw gebruikersnaam en wachtwoord hebt getypt, kunt u de app openen en kopieert u de verificatiecode hebt opgegeven op de **Accounts** scherm op het aanmeldingsscherm. De verificatiecode in die fungeert als een tweede vorm van verificatie.

## <a name="opt-in-for-two-step-verification"></a>Opt-in voor verificatie in twee stappen

Uw organisatie besluit of u verificatie in twee stappen met uw werk of school-account gebruiken. De beheerder laat u weten welke verificatiemethoden die moeten worden ingesteld en gebruikt. Zie voor meer informatie, [wat Azure multi-factor Authentication betekent voor mij](multi-factor-authentication-end-user.md).

U kunt de verificatie in twee stappen voor uzelf instellen voor uw persoonlijke Microsoft-account. Zie voor meer informatie en instructies, [over verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

U kunt ook de Microsoft Authenticator-app gebruiken met niet-Microsoft-accounts. Deze accounts kunnen aanroepen de functie dan verificatie in twee stappen, maar u zou het mogelijk te vinden binnen de beveiligings- of instellingen voor aanmelding. Zie voor meer informatie over het instellen van deze niet-Microsoft-accounts, het [klantondersteuning van Microsoft-video's](https://www.youtube.com/playlist?list=PLyhj1WZ29G65QdD9NxTOAm8HwOS-OBUrX).

## <a name="install-the-app"></a>De app installeren

De Microsoft Authenticator-app is beschikbaar voor [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) en [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071). Als u de beste ervaring, moet u toestaan dat de app meldingen te ontvangen wanneer u wordt gevraagd. 

## <a name="add-accounts-to-the-app"></a>Accounts toevoegen aan de app.

U kunt werk of schoolaccounts of persoonlijke accounts toevoegen aan de Microsoft Authenticator-app. 

### <a name="add-a-personal-microsoft-account"></a>Een persoonlijk Microsoft-account toevoegen

Voor een persoonlijk Microsoft-account (één waarmee u zich aanmeldt bij Outlook.com, Xbox, Skype, enzovoort), is hoeft u zich aanmeldt bij uw account in de Microsoft Authenticator-app.

### <a name="add-a-work-or-school-account"></a>Een werk- of schoolaccount toevoegen

1. Indien mogelijk, gaat u naar de [aanvullende beveiligingsverificatie](http://aka.ms/mfasetup) scherm op een andere computer of apparaat. Zie voor meer informatie over het registreren op dit scherm [wijzigen van uw beveiligingsinstellingen](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page) of neem contact op met uw beheerder.

    >[!Note]
    >Als uw beheerder heeft ingeschakeld op de beveiliging info preview-ervaring, kunt u de instructies in de [beveiligingsgegevens ingesteld voor het gebruik van een verificator-app](security-info-setup-auth-app.md) sectie.

2. Schakel het selectievakje in naast **Authenticator-app**, en selecteer vervolgens **configureren**.

    ![De knop configureren op het scherm beveiliging verificatie-instellingen](./media/microsoft-authenticator-app-how-to/auth-app-configure.png)

    De **mobiele app configureren** scherm wordt weergegeven met een QR-code voor u om te scannen met de authenticator-app.

    ![Scherm waarmee de QR-code](./media/microsoft-authenticator-app-how-to/auth-app-barcode.png)

3. Open de Microsoft Authenticator-app. Op de **Accounts** scherm, selecteer **account toevoegen**, en selecteer vervolgens **werk- of schoolaccount**.

4. Voor het gebruik van de camera van het apparaat moet de QR-code scannen en selecteer vervolgens **gedaan** te sluiten van het scherm QR-code.

    >[!Note]
    >Als uw camera niet goed werkt, kunt u [handmatig invoeren van de QR-code en URL](#add-an-account-to-the-app-manually).

    De **Accounts** scherm van de app ziet u de accountnaam van uw en een 6-cijferige verificatiecode. Voor extra beveiliging verandert de verificatiecode in die elke 30 seconden voorkomt dat u met de dezelfde code twee keer.  

    ![Scherm accounts](./media/microsoft-authenticator-app-how-to/auth-app-accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Een account handmatig toevoegen aan de app.

1. Ga naar de **aanvullende beveiligingsverificatie** scherm. Zie voor meer informatie over het verkrijgen op dit scherm [wijzigen van uw beveiligingsinstellingen](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).

2. Schakel het selectievakje in naast **Authenticator-app**, en selecteer vervolgens **configureren**.

    De **mobiele app configureren** scherm wordt weergegeven.

3. Kopieer de code en URL-gegevens van de **mobiele app configureren** scherm, zodat u ze handmatig in de QR-scanner kan typen.

4. Open de Microsoft Authenticator-app. Op de **Accounts** scherm, selecteer **account toevoegen**, en selecteer vervolgens **werk- of schoolaccount**.

5. Selecteer in het scherm QR-scanner **handmatig invoeren van code**.

    ![Scherm voor het scannen van een QR-code](./media/microsoft-authenticator-app-how-to/auth-app-manual-code.png)
   
6. Typ de code en URL van het scherm met de QR-code in de **een account toevoegen** scherm en selecteer vervolgens **voltooien**.

    ![Scherm voor het invoeren van code en URL in](./media/microsoft-authenticator-app-how-to/auth-app-code-url.png)

    De **Accounts** scherm van de app ziet u de accountnaam van uw en een 6-cijferige verificatiecode. Voor extra beveiliging verandert de verificatiecode in die elke 30 seconden voorkomt dat u met de dezelfde code twee keer.

### <a name="using-your-devices-fingerprint-or-facial-recognition-capabilities"></a>Met behulp van de vingerafdruk of gezichtsherkenning-mogelijkheden van uw apparaat

Uw organisatie vereist mogelijk een PINCODE voor het voltooien van uw identiteit te verifiëren. U kunt de Microsoft Authenticator-app van uw apparaat vingerafdruk of gezichtsherkenning mogelijkheden gebruiken in plaats van een PINCODE instellen. U kunt dit instellen van de eerste keer dat u de authenticator-app gebruiken om te controleren of uw account door te selecteren de optie voor het gebruik van uw apparaat biometrische mogelijkheden als identificatie in plaats van uw PINCODE.

## <a name="use-the-app-when-you-sign-in"></a>De app gebruiken wanneer u zich aanmeldt

Nadat u uw accounts aan de app toevoegen, gebruikt u de app te melden bij uw accounts.

Als u ervoor kiest verificatiecodes gebruiken in de app, zult u start in de **Accounts** pagina. De codes wijzigen elke 30 seconden, zodat u altijd een nieuwe code hebt wanneer u nodig hebt. Maar u hoeft verder niets te doen met hen totdat u zich aanmelden en wordt gevraagd een verificatiecode invoeren.

## <a name="next-steps"></a>Volgende stappen

- Als u meer algemene vragen over de app hebt, raadpleegt u [Veelgestelde vragen over de Microsoft Authenticator](microsoft-authenticator-app-faq.md)

- Als u vragen over hoe u aanmelding via telefoon hebt, raadpleegt u [Meld u aan met uw telefoon, niet uw wachtwoord](microsoft-authenticator-app-phone-signin-faq.md)

- Als u meer informatie over verificatie in twee stappen wilt, Zie [Mijn account voor verificatie in twee stappen instellen](multi-factor-authentication-end-user-first-time.md)

- Als u meer informatie over de beveiligingsgegevens wilt, Zie [je beveiligingsgegevens beheren](security-info-manage-settings.md)
