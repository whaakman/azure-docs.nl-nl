---
title: Microsoft Authenticator-app voor mobiele telefoons - Azure AD | Microsoft Docs
description: Leer hoe u een upgrade uitvoert naar de nieuwste versie van Azure Authenticator.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/17/2017
ms.author: lizross
ms.reviewer: librown
ms.custom: H1Hack27Feb2017, end-user
ms.openlocfilehash: 8241dcaaf5623a22f4fc485f021766276472fb51
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059821"
---
# <a name="get-started-with-the-microsoft-authenticator-app"></a>Aan de slag met de Microsoft Authenticator-app
De Microsoft Authenticator-app biedt een extra beveiligingsniveau van uw werk of school-account (bijvoorbeeld bsimon@contoso.com) of uw Microsoft-account (bijvoorbeeld bsimon@outlook.com).

De app werkt op twee manieren:

* **Melding**. De app kunt u voorkomt ongeoorloofde toegang tot accounts en frauduleuze transacties stoppen door het pushen van een melding naar uw smartphone of tablet. Bekijk de melding, en wanneer deze geldig is, selecteert u **controleren**. Anders kunt u **weigeren**.
* **Verificatiecode**. De app kan worden gebruikt als een Softwaretoken voor het genereren van een OAuth-verificatiecode. Nadat u uw gebruikersnaam en wachtwoord hebt ingevoerd, voert u de code die is geleverd door de app in het aanmeldingsscherm. De verificatiecode zorgt voor een tweede vorm van verificatie.

De Microsoft Authenticator-app wordt vervangen door de Azure Authenticator-app. De Azure Authenticator-app werkt nog steeds, maar als u besluit te verplaatsen naar de nieuwe Microsoft Authenticator-app, in dit artikel u kan helpen.  

## <a name="opt-in-for-two-step-verification"></a>Opt-in voor verificatie in twee stappen

De Microsoft Authenticator-app werkt niet op zichzelf. Elk van uw accounts aan u gevraagd om een tweede verificatiemethode, nadat u zich aanmelden met uw gebruikersnaam en wachtwoord configureren.

Voor een werk- of schoolaccount krijgt u niet normaal gesproken kiest u deze functie zelf. In plaats daarvan een beveiligingsbeheerder kan worden gebruikt in uw naam en vervolgens wordt geïnformeerd verificatiemethoden voor uw account te registreren. Meer informatie in dit scenario is van toepassing op u, [wat Azure multi-factor Authentication betekent voor mij](multi-factor-authentication-end-user.md).

Voor een persoonlijk account is moet u voor het instellen van verificatie in twee stappen voor uzelf. Als u een Microsoft-account hebt, deze stappen zijn beschikbaar in [over verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

U kunt ook de Microsoft Authenticator gebruiken met niet-Microsoft-accounts. Ze kunnen de functie dan verificatie in twee stappen aanroepen, maar u moet kunnen Ga hiervoor naar de beveiligings- of instellingen voor aanmelding.

## <a name="install-the-app"></a>De app installeren
De Microsoft Authenticator-app is beschikbaar voor [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) en [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

## <a name="add-accounts-to-the-app"></a>Accounts toevoegen aan de app.
Voor elk account dat u wilt toevoegen aan de Microsoft Authenticator-app, gebruikt u een van de volgende procedures:

### <a name="add-a-personal-microsoft-account-to-the-app"></a>Een persoonlijk Microsoft-account toevoegen aan de app

Voor een persoonlijk Microsoft-account (één waarmee u zich aanmeldt bij Outlook.com, Xbox, Skype, enzovoort), is hoeft u zich aanmeldt bij uw account in de Microsoft Authenticator-app.

### <a name="add-a-work-or-school-account-to-the-app-using-the-qr-code-scanner"></a>Een account voor werk of school toevoegen aan de app met behulp van de QR-codescanner
1. Ga naar het scherm beveiliging verificatie-instellingen.  Zie voor meer informatie over het verkrijgen op dit scherm [wijzigen van uw beveiligingsinstellingen](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).
2. Schakel het selectievakje in naast **Authenticator-app** Selecteer **configureren**.

    ![De knop configureren op het scherm beveiliging verificatie-instellingen](./media/microsoft-authenticator-app-how-to/azureauthe.png)

    Hiermee wordt een scherm met een QR-code erop.

    ![Scherm waarmee de QR-code](./media/microsoft-authenticator-app-how-to/barcode2.png)
3. Open de Microsoft Authenticator-app. Op de **accounts** scherm, selecteer **+**, en vervolgens opgeven dat u een account voor werk- of schoolaccount toe te voegen.
4. Gebruik van de camera aan de QR-code scannen en selecteer vervolgens **gedaan** te sluiten van het scherm QR-code.

    Als uw camera niet goed werkt is, kunt u [handmatig invoeren van de QR-code en URL](#add-an-account-to-the-app-manually).

5. Als de app wordt de naam van uw account met een code van zes cijfers eronder weergegeven, bent u klaar.

    ![Scherm accounts](./media/microsoft-authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Een account handmatig toevoegen aan de app.
1. Ga naar het scherm beveiliging verificatie-instellingen.  Zie voor meer informatie over het verkrijgen op dit scherm [wijzigen van uw beveiligingsinstellingen](multi-factor-authentication-end-user-manage-settings.md).
2. Selecteer **configureren**.

    ![De knop configureren op het scherm beveiliging verificatie-instellingen](./media/microsoft-authenticator-app-how-to/azureauthe.png)

    Hiermee wordt een scherm met een QR-code erop.  Houd er rekening mee de code en URL.

    ![Scherm waarmee de QR-code en URL](./media/microsoft-authenticator-app-how-to/barcode2.png)
3. Open de Microsoft Authenticator-app. Op de **accounts** scherm, selecteer **+**, en vervolgens opgeven dat u een account voor werk- of schoolaccount toe te voegen.

4. Selecteer in de scanner **handmatig invoeren van code**.

    ![Scherm voor het scannen van een QR-code](./media/microsoft-authenticator-app-how-to/scan2.png)
5. Voer de code en de URL in de desbetreffende vakken in de app en selecteer vervolgens **voltooien**.

    ![Scherm voor het invoeren van code en URL in](./media/microsoft-authenticator-app-how-to/manual.png)

6. Als de app wordt de naam van uw account met een code van zes cijfers eronder weergegeven, bent u klaar.

    ![Scherm accounts](./media/microsoft-authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-using-your-devices-fingerprint-or-facial-recognition-capabilities"></a>Een account toevoegen aan de app met de vingerafdruk of gezichtsherkenning-mogelijkheden van uw apparaat
Uw organisatie mogelijk een PINCODE vereisen voor de verificatie-uitdaging voltooien. De Microsoft Authenticator-app kunt de vingerafdruk of gezichtsherkenning-mogelijkheden van uw apparaat in plaats van een PINCODE gebruiken. Om dit op uw eerste verificatie in de app, ziet u een optie voor het gebruik van Touch ID (voor iOS) of id van vingerafdruk in plaats daarvan. 

Als u Touch ID voor de Microsoft Authenticator instelt, moet u een uitdaging normale verificatie met een PINCODE te voltooien. Microsoft Authenticator wordt deze automatisch ingesteld voor apparaten die ondersteuning bieden voor Touch-ID. 

![Verificatie van de Touch ID instellen](./media/microsoft-authenticator-app-how-to/touchid1.png)

Vanaf dat punt doorsturen, wanneer u bent verplicht om te controleren of de aanmelding, u selecteert de pushmelding ontvangen en scant uw vingerafdruk in plaats van uw pincode.

![Pushmelding](./media/microsoft-authenticator-app-how-to/touchid2.png)

## <a name="use-the-app-when-you-sign-in"></a>De app gebruiken wanneer u zich aanmeldt

Nadat uw account is toegevoegd aan de app, u mogelijk gevraagd om te doen van een testverificatie om te controleren of dat alles juist is geconfigureerd. Hierna kunt bent u klaar. U hoeft te doen niets totdat de volgende keer dat u zich aanmeldt.

Als u ervoor kiest verificatiecodes gebruiken in de app, start u deze op de startpagina te zien. Ze wijzigen elke 30 seconden zodat u altijd een nieuwe code hebt wanneer u nodig hebt. Maar u hoeft verder niets te doen met hen totdat u zich aanmelden en wordt gevraagd een verificatiecode invoeren.  
