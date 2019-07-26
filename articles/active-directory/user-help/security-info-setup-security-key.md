---
title: Beveiligings gegevens instellen voor het gebruik van een beveiligings sleutel (preview)-Azure Active Directory | Microsoft Docs
description: Uw beveiligings gegevens instellen om uw identiteit te verifiëren met behulp van een snelle FIDO2-beveiligings sleutel (Identity online).
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: librown
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d2f06b054e433c0320019548c56539d102beaad
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68386497"
---
# <a name="set-up-security-info-to-use-a-security-key-preview"></a>Beveiligings gegevens voor het gebruik van een beveiligings sleutel instellen (preview-versie)

U kunt beveiligings sleutels gebruiken als een aanmeldings methode zonder wacht woord binnen uw organisatie. Een beveiligings sleutel is een fysiek apparaat dat wordt gebruikt met een unieke pincode om u aan te melden bij uw werk-of school account. Omdat voor beveiligings sleutels het fysieke apparaat moet worden gebruikt en wat alleen u weet, wordt dit beschouwd als een sterkere verificatie methode dan een gebruikers naam en wacht woord.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-key.md)]

>[!Important]
>Deze inhoud is bedoeld voor gebruikers. Als u een beheerder bent, kunt u meer informatie vinden over hoe u uw Azure Active Directory-omgeving (Azure AD) instelt en beheert in de [documentatie voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory).

## <a name="what-is-a-security-key"></a>Wat is een beveiligings sleutel?

Momenteel worden verschillende ontwerpen en providers van beveiligings sleutels ondersteund met behulp van de [Fido (Fast Identity online](https://fidoalliance.org/fido2/) ) (FIDO2)-verificatie methode met een wacht woord. Met deze methode kunt u zich één keer aanmelden bij uw werk-of school account om toegang te krijgen tot alle cloud resources en ondersteunde browsers van uw organisatie.

Uw beheerder of uw organisatie verstrekt u een beveiligings sleutel als deze nodig is voor uw werk-of school account. Er zijn verschillende soorten beveiligings sleutels die u kunt gebruiken, bijvoorbeeld een USB-sleutel die u aansluit op uw apparaat of een NFC-sleutel die u op een NFC-lezer tikt. Raadpleeg de documentatie van de fabrikant voor meer informatie over de beveiligings sleutel, zoals wat het geval is.

> [!Note]
> Als u geen FIDO2-beveiligings sleutel kunt gebruiken, zijn er andere verificatie methoden met een wacht woord, zoals de Microsoft Authenticator app of Windows hello. Zie [Wat is de Microsoft Authenticator-app?](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-overview)voor meer informatie over de app Microsoft Authenticator. Zie [overzicht van Windows hello](https://www.microsoft.com/windows/windows-hello)voor meer informatie over Windows hello.

## <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u begint met het registreren van uw beveiligings sleutel of:

- Uw beheerder heeft deze functie ingeschakeld voor gebruik binnen uw organisatie.

- U bevindt zich op een apparaat met ten minste Windows 10, versie 1903 en gebruikt de micro soft Edge-browser.

- U hebt een fysieke beveiligings sleutel van uw beheerder of uw organisatie ontvangen. Uw beveiligings sleutel moet zowel FIDO2 als micro soft-compatibel zijn. Als u vragen hebt over uw beveiligings sleutel en of deze compatibel zijn, neemt u contact op met de Help Desk van uw organisatie.

## <a name="register-your-security-key"></a>Uw beveiligingssleutel registreren

U moet uw beveiligings sleutel voorbereiden om te werken met Windows en een unieke pincode voordat u zich met de sleutel kunt aanmelden bij uw werk-of school account.

1. Ga naar de pagina **Mijn profiel** op https://myprofile.microsoft.com en meld u aan als u dat nog niet hebt gedaan.

2. Selecteer **beveiligings gegevens**, selecteer **methode toevoegen**en selecteer vervolgens **beveiligings sleutel** in de vervolg keuzelijst **een methode toevoegen** .

    ![Het vak methode toevoegen, waarbij de beveiligings sleutel is geselecteerd](media/security-info/security-info-security-key-add-method.png)

3. Selecteer **toevoegen**en selecteer vervolgens het type van de beveiligings sleutel die u hebt, hetzij **USB-apparaat** of **NFC-apparaat**.

    ![Kies of u een USB-of NFC-type van de beveiligings sleutel hebt](media/security-info/security-info-security-key-choose-type.png)

    > [!Note]
    > Als u niet zeker weet welk type beveiligings sleutel u hebt, raadpleegt u de documentatie van de fabrikant. Als u niet zeker weet wat de fabrikant is, neemt u contact op met de Help Desk van uw organisatie.

4. Laat uw beveiligings sleutel fysiek beschikbaar en selecteer **volgende**in het vak **beveiligings sleutel** .

    ![Vak registratie van beveiligings sleutel starten](media/security-info/security-info-security-key-start-setup.png)

    Er wordt een nieuw vak weer gegeven om u te helpen bij het instellen van uw nieuwe aanmeldings methode.

5. Selecteer in het vak **uw nieuwe aanmeldings methode instellen** de optie **volgende**en klik vervolgens op:

    - Als uw beveiligings sleutel een USB-apparaat is, plaatst u de beveiligings sleutel in de USB-poort van uw apparaat.

    - Als uw beveiligings sleutel een NFC-apparaat is, tikt u op uw beveiligings sleutel voor uw lezer.

6. Typ uw unieke pincode voor de beveiligings sleutel in het vak **Windows-beveiliging** en selecteer vervolgens **OK**.

    U keert terug naar het vak **uw nieuwe aanmeldings methode instellen** .

7. Selecteer **Volgende**.

8. Ga terug naar de pagina **beveiligings gegevens** , typ een naam die u later voor uw nieuwe beveiligings sleutel gaat herkennen en selecteer **volgende**.

    ![Pagina beveiligings informatie, met de naam van de beveiligings sleutel](media/security-info/security-info-security-key-name.png)

    Uw beveiligings sleutel is geregistreerd en klaar voor gebruik om u aan te melden bij uw werk-of school account.

9. Selecteer **gereed** om het vak **beveiligings sleutel** te sluiten.

    De pagina **beveiligings gegevens** wordt bijgewerkt met de informatie over de beveiligings sleutel.

    ![Pagina met beveiligings gegevens, waarbij alle geregistreerde methoden worden weer gegeven](media/security-info/security-info-security-key-configured.png)

## <a name="delete-a-security-key-from-your-security-info"></a>Een beveiligings sleutel verwijderen uit uw beveiligings gegevens

Als je je beveiligings sleutel niet meer wilt gebruiken, kun je de sleutel uit je beveiligings gegevens verwijderen. Hierdoor wordt de beveiligings sleutel niet meer gebruikt voor uw werk-of school account, dan blijft de beveiligings sleutel uw gegevens en referentie gegevens opslaan. Als u uw gegevens en referentie gegevens uit de beveiligings sleutel zelf wilt verwijderen, moet u de instructies in de sectie [een micro soft-compatibele beveiligings sleutel opnieuw instellen](#reset-your-security-key) in dit artikel volgen.

1. Selecteer de koppeling **verwijderen** uit de beveiligings sleutel die u wilt verwijderen.

2. Selecteer **OK** in het vak **beveiligings sleutel verwijderen** .

    Uw beveiligings sleutel is verwijderd en u kunt deze niet meer gebruiken om u aan te melden bij uw werk-of school account.

>[!Important]
>Als u deze beveiligings sleutel per ongeluk hebt verwijderd, moet u deze opnieuw registreren met behulp van de instructies in de sectie [uw beveiligings sleutel registreren](#register-your-security-key) in dit artikel.

## <a name="manage-your-security-key-settings-from-windows-settings"></a>Uw beveiligings sleutel instellingen beheren vanuit Windows-instellingen

U kunt de instellingen van uw beveiligings sleutel beheren via de **Windows-instellingen** -app, waaronder het opnieuw instellen van uw beveiligings sleutel en het maken van een nieuwe pincode voor de beveiligings sleutel.

### <a name="reset-your-security-key"></a>Uw beveiligings sleutel opnieuw instellen

Als u alle account gegevens wilt verwijderen die zijn opgeslagen in uw fysieke beveiligings sleutel, moet u de sleutel herstellen naar de fabrieks instellingen. Als u de beveiligings sleutel opnieuw instelt, wordt alles uit de sleutel verwijderd, zodat u opnieuw kunt beginnen.

>[!IMPORTANT]
>Als u de beveiligings sleutel opnieuw instelt, wordt alles uit de sleutel verwijderd, zodat de fabrieks instellingen worden hersteld.
>
> **Alle gegevens en referenties worden gewist.**

#### <a name="to-reset-your-security-key"></a>Uw beveiligings sleutel opnieuw instellen

1. Open de Windows-instellingen-app, selecteer **accounts**, selecteer **aanmeldings opties**, selecteer **beveiligings sleutel**en selecteer vervolgens **beheren**.

2. Plaats uw beveiligings sleutel in de USB-poort of tik op uw NFC-lezer om uw identiteit te verifiëren.

3. Volg de instructies op het scherm op basis van de fabrikant van uw specifieke beveiligings sleutel. Als uw sleutel fabrikant niet wordt vermeld in de instructies op het scherm, raadpleegt u de site van de fabrikant voor meer informatie.

4. Selecteer **sluiten** om het scherm **beheren** te sluiten.

### <a name="create-a-new-security-key-pin"></a>Een nieuwe pincode voor de beveiligings sleutel maken

U kunt een nieuwe pincode voor de beveiligings sleutel maken voor uw beveiligings sleutel.

#### <a name="to-create-a-new-security-key-pin"></a>Een nieuwe pincode voor de beveiligings sleutel maken

1. Open de Windows-instellingen-app, selecteer **accounts**, selecteer **aanmeldings opties**, selecteer **beveiligings sleutel**en selecteer vervolgens **beheren**.

2. Plaats uw beveiligings sleutel in de USB-poort of tik op uw NFC-lezer om uw identiteit te verifiëren. 5
3. Selecteer **toevoegen** in het gebied **pincode van beveiligings sleutel** , typ en bevestig uw nieuwe pincode voor de beveiligings sleutel en selecteer vervolgens **OK**.

    De beveiligings sleutel wordt bijgewerkt met de nieuwe pincode van de beveiligings sleutel voor gebruik met uw werk-of school account. Als u besluit om uw pincode opnieuw te wijzigen, kunt u de knop **wijzigen** selecteren. 6
4. Selecteer **sluiten** om het scherm **beheren** te sluiten.

## <a name="additional-security-info-methods"></a>Aanvullende beveiligings gegevens methoden

U hebt extra opties voor het controleren van uw identiteit door uw organisatie, op basis van wat you're u probeert te doen. De opties zijn:

- **Verificator-app.** Down load en gebruik een verificator-app om een goedkeurings melding of een wille keurig gegenereerde goedkeurings code te verkrijgen voor verificatie in twee stappen of het opnieuw instellen van wacht woorden. Zie [beveiligings informatie instellen voor het gebruik van een verificator-app](security-info-setup-auth-app.md)voor stapsgewijze instructies voor het instellen en gebruiken van de app Microsoft Authenticator.

- **Tekst van mobiel apparaat.** Voer het nummer van uw mobiele apparaat in en ontvang een tekst die u gebruikt voor verificatie in twee stappen of het opnieuw instellen van wacht woorden. Voor stapsgewijze instructies over het verifiëren van uw identiteit met een SMS-bericht, Zie [beveiligings informatie instellen voor het gebruik van tekst berichten (SMS)](security-info-setup-text-msg.md).

- **Mobiel apparaat of telefoon nummer van werk.** Voer het nummer van uw mobiele apparaat in en ontvang een telefoon oproep voor verificatie in twee stappen of het opnieuw instellen van wacht woorden. Zie [beveiligings informatie instellen voor het gebruik van telefoon gesprekken](security-info-setup-phone-number.md)voor stapsgewijze instructies over het verifiëren van uw identiteit met een telefoon nummer.

- **E-mail adres.** Voer het e-mail adres van uw werk of school in om een e-mail te ontvangen voor het opnieuw instellen van wacht woorden. Deze optie is niet beschikbaar voor verificatie in twee stappen. Zie [beveiligings informatie instellen voor het gebruik van e-mail](security-info-setup-email.md)voor stapsgewijze instructies voor het instellen van uw e-mail adres.

- **Beveiligings vragen.** Beantwoord enkele beveiligings vragen die door uw beheerder voor uw organisatie zijn gemaakt. Deze optie is alleen beschikbaar voor het opnieuw instellen van wacht woorden en niet voor verificatie in twee stappen. Voor stapsgewijze instructies over het instellen van uw beveiligings vragen raadpleegt u het artikel [beveiligings vragen instellen voor het gebruik](security-info-setup-questions.md) van beveiligings problemen.

## <a name="next-steps"></a>Volgende stappen

- Lees voor meer informatie over verificatie methoden met een wacht woord de [open bare preview-versie van de FIDO2-beveiligings sleutels van micro soft, het inschakelen van blogs met een wacht woord](https://www.onmsft.com/news/microsofts-azure-ad-begins-public-preview-of-fido2-security-keys-enabling-passwordless-logins) en het lezen van [wat is de Microsoft Authenticator-app?](https://docs.microsoft.com/azure/active-directory/user-help8user-help-auth-app-overview) en [ Overzichts artikelen van Windows hello](https://www.microsoft.com/windows/windows-hello) .

- Voor meer informatie over [beveiligings sleutels die compatibel zijn met micro soft](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/microsoft-compatible-security-key).

- Stel uw wachtwoord opnieuw in als u dit hebt verloren of bent vergeten, uit het [Portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com/) of volg de stappen in het artikel [Uw werk- of schoolaccount wachtwoord opnieuw instellen](user-help-reset-password.md).

- Krijg tips voor het oplossen van problemen en hulp met problemen met aanmelden in het artikel [U kunt niet aanmelden bij uw Microsoft-account](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).
