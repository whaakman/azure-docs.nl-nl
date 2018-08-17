---
title: Uw werk-apparaat toevoegen aan het netwerk van uw organisatie - Azure Active Directory | Microsoft Docs
description: Leer hoe u uw werk-apparaat toevoegen aan het netwerk van uw organisatie.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: lizross
ms.reviewer: jairoc
ms.openlocfilehash: 34d3c12c83aeac7e92aa019abc38d9c4109883bd
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180776"
---
# <a name="join-your-work-device-to-your-organizations-network"></a>Uw werk-apparaat toevoegen aan het netwerk van uw organisatie
Toevoegen aan het netwerk van uw organisatie eigendom van het werk Windows 10-apparaat, zodat u toegang hebben tot mogelijk beperkte resources.

## <a name="what-happens-when-you-join-your-device"></a>Wat gebeurt er wanneer u uw apparaat
Terwijl u bent lid wordt van uw Windows 10-apparaat op het netwerk van uw organisatie, wordt de volgende acties uitgevoerd:

- Windows-registers uw apparaat op het netwerk van uw organisatie, zodat u toegang tot uw resources met behulp van uw persoonlijke account. Nadat het apparaat is geregistreerd, Windows vervolgens lid wordt van uw apparaat met het netwerk, zodat u de gebruikersnaam en het wachtwoord van uw organisatie gebruiken kunt om te melden en toegang tot beperkte resources.

- (Optioneel) op basis van de mogelijkheden van uw organisatie, u mogelijk gevraagd voor het instellen van verificatie via een [multi-Factor Authentication](multi-factor-authentication-end-user-first-time.md) of [beveiligingsgegevens](user-help-security-info-overview.md).

- (Optioneel) op basis van de mogelijkheden van uw organisatie, u mogelijk worden automatisch geregistreerd bij beheer van mobiele apparaten, zoals Microsoft Intune. Zie voor meer informatie over het inschrijven in Microsoft Intune [uw apparaat inschrijven bij Intune](https://docs.microsoft.com/intune-user-help/enroll-your-device-in-intune-all).

- Gaat u door het proces aanmelden met behulp van automatische aanmelding met uw organisatieaccount.

## <a name="to-join-a-brand-new-windows-10-device"></a>Een gloednieuwe Windows 10-apparaat
Als het apparaat is een gloednieuwe en nog niet is ingesteld, gaat u door het proces Windows Out of Box Experience (OOBE) op uw apparaat toevoegen aan het netwerk.

1. Het nieuwe apparaat starten en beginnen met het OOBE-proces.

2. Op de **aanmelden bij Microsoft** scherm, typt u uw werk of school-e-mailadres.

    ![Meld u aan het scherm met e-mailadres](./media/user-help-join-device-on-network/join-device-oobe-signin.png)

3. Op de **Voer uw wachtwoord** scherm, typt u uw wachtwoord.

    ![Voer uw wachtwoord scherm](./media/user-help-join-device-on-network/join-device-oobe-password.png)

4. Goedkeuren op uw mobiele apparaat, uw apparaat, zodat deze toegang heeft tot uw account. 

    ![Scherm van de mobiele melding ontvangen](./media/user-help-join-device-on-network/join-device-oobe-mobile.png)

5. Voltooi de OOBE-proces, inclusief het instellen van uw privacy-instellingen en het instellen van Windows Hello (indien nodig).

    Het apparaat is nu gekoppeld aan het netwerk van uw organisatie.

## <a name="to-make-sure-youre-joined"></a>Om te controleren of bent u toegevoegd
U kunt ervoor zorgen dat u lid geworden bent door te kijken op uw instellingen.

1. Open **instellingen**, en selecteer vervolgens **Accounts**.

    ![Accounts in het scherm instellingen](./media/user-help-join-device-on-network/join-device-settings-accounts.png)

2. Selecteer **toegang tot werk of school**, en zorg ervoor dat u tekst of iets dergelijks, Zie **verbonden met *< organisatie >* Azure AD**.

    ![Toegang tot werk- of schoolaccount scherm met verbonden contoso-account](./media/user-help-join-device-on-network/join-device-oobe-verify.png)


## <a name="to-join-an-already-configured-windows-10-device"></a>Lid worden van een reeds geconfigureerde Windows 10-apparaat
Als u hebt uw apparaat voor een tijdje hebt en er al ingesteld, u kunt deze stappen volgen om uw apparaat toevoegen aan het netwerk.

1. Open **instellingen**, en selecteer vervolgens **Accounts**.

2. Selecteer **toegang tot werk of school**, en selecteer vervolgens **Connect**.

    ![Toegang tot werk of school en verbinding maken met koppelingen](./media/user-help-join-device-on-network/join-device-access-work-school-connect.png)

3. Op de **instellen van een account voor werk of school** scherm, selecteer **dit apparaat toevoegen aan Azure Active Directory**.

    ![Instellen van een werk- of schoolaccount account scherm](./media/user-help-join-device-on-network/join-device-setup-join-aad.png)

4. Op de **je krijgt zo aangemeld** scherm, typt u uw e-mailadres (bijvoorbeeld alain@contoso.com), en selecteer vervolgens **volgende**.

    ![Je krijgt zo scherm aangemeld](./media/user-help-join-device-on-network/join-device-setup-get-signed-in.png)

5. Op de **wachtwoord opgeven** scherm, typt u het wachtwoord en selecteer vervolgens **aanmelden**.

    ![Wachtwoord invoeren](./media/user-help-join-device-on-network/join-device-setup-password.png)

6. Goedkeuren op uw mobiele apparaat, uw apparaat, zodat deze toegang heeft tot uw account. 

    ![Scherm van de mobiele melding ontvangen](./media/user-help-join-device-on-network/join-device-setup-mobile.png)

7. Op de **Zorg ervoor dat dit is uw organisatie** scherm, lees de informatie om te controleren of het juiste is, en selecteer vervolgens **Join**.

    ![Zorg ervoor dat dit is de verificatie-scherm van uw organisatie](./media/user-help-join-device-on-network/join-device-setup-confirm.png)

8. Op de **u bent klaar** scherm, klikt u op **gedaan**.

    ![U kunt alle set scherm](./media/user-help-join-device-on-network/join-device-setup-finish.png)

## <a name="to-make-sure-youre-joined"></a>Om te controleren of bent u toegevoegd
U kunt ervoor zorgen dat u lid geworden bent door te kijken op uw instellingen.

1. Open **instellingen**, en selecteer vervolgens **Accounts**.

    ![Accounts in het scherm instellingen](./media/user-help-join-device-on-network/join-device-settings-accounts.png)

2. Selecteer **toegang tot werk of school**, en zorg ervoor dat u tekst of iets dergelijks, Zie **verbonden met *< organisatie >* Azure AD**.

    ![Toegang tot werk- of schoolaccount scherm met verbonden contoso-account](./media/user-help-join-device-on-network/join-device-setup-verify.png)

## <a name="next-steps"></a>Volgende stappen
Nadat u uw apparaat aan het netwerk van uw organisatie toevoegen, moet u mogelijk zijn voor toegang tot al uw resources met uw werk of school-accountgegevens.

- Als uw organisatie wil u dat voor het registreren van uw persoonlijke apparaat, zoals uw telefoon, Zie [registreren van uw persoonlijke apparaat op het netwerk van uw organisatie](user-help-register-device-on-network.md).

