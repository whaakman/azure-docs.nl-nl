---
title: Configureren van Azure Active Directory zonder wachtwoord uitproberen aanmelden (preview)
description: De configuratie aanmelden bij Azure AD met behulp van beveiligingssleutels FIDO2 of de Microsoft Authenticator-app (preview) inschakelen
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ba2545467ebfbd032408aeee25b82b92a628f2a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712070"
---
# <a name="enable-passwordless-sign-in-for-azure-ad-preview"></a>De configuratie aanmelding inschakelen voor Azure AD (preview)

## <a name="requirements"></a>Vereisten

* Azure Multi-Factor Authentication
* Voorbeeld van registratie gecombineerd
* FIDO2 security key preview compatibel FIDO2 beveiligingssleutels is vereist
* WebAuthN Microsoft Edge op Windows 10 versie 1809 of hoger is vereist
* FIDO2 op basis van Windows-aanmelding in Azure AD vereist verbonden Windows 10 versie 1809 of hoger

## <a name="prepare-devices-for-preview"></a>Voorbereiden van apparaten voor de Preview-versie

Apparaten die u met zal worden valideert moeten worden uitgevoerd worden Windows 10 versie 1809 of hoger. De beste ervaring is op Windows 10 versie 1903 of hoger.

## <a name="enable-security-keys-for-windows-sign-in"></a>Meld u aan de beveiligingssleutels inschakelen voor Windows

Organisaties kunnen ervoor kiezen voor een of meer van de volgende methoden om in te schakelen van het gebruik van sleutels van de beveiliging voor Windows aanmelden.

### <a name="enable-credential-provider-via-intune"></a>Referentieprovider via Intune inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Blader naar **Microsoft Intune** > **apparaatinschrijving** > **Windows-inschrijving** > **Windows Hello voor bedrijven** > **eigenschappen**.
1. Onder **instellingen** ingesteld **sleutels gebruiken voor aanmelding bij** naar **ingeschakeld**.

Configuratie van beveiligingssleutels voor aanmelding, is niet afhankelijk van het configureren van Windows Hello voor bedrijven.

#### <a name="enable-targeted-intune-deployment"></a>Bepaalde Intune-implementatie inschakelen

Als u wilt richten op specifieke apparaatgroepen om in te schakelen van de Referentieprovider, gebruik de volgende aangepaste instellingen via Intune. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Blader naar **Microsoft Intune** > **apparaatconfiguratie** > **profielen** >  **-profielmaken**.
1. Het nieuwe profiel met de volgende instellingen configureren
   1. Naam: Sleutels voor aanmelding bij Windows
   1. Beschrijving: Hiermee kunt FIDO-beveiligingssleutels moet worden gebruikt tijdens Windows aanmelden
   1. Platform: Windows 10 en hoger
   1. Platformtype: Aangepast telefoonnummer
   1. Aangepaste OMA-URI-instellingen:
      1. Naam: FIDO-beveiligingssleutels inschakelen voor aanmelding bij Windows
      1. OMA-URI: ./Device/Vendor/MSFT/PassportForWork/SecurityKey/UseSecurityKeyForSignin
      1. Gegevenstype: Geheel getal
      1. Waarde: 1 
1. Dit beleid kan worden toegewezen aan specifieke gebruikers, apparaten of groepen. Meer informatie vindt u in het artikel [toewijzen van gebruikers- en apparaatprofielen in Microsoft Intune](https://docs.microsoft.com/intune/device-profile-assign).

![Het maken van Intune aangepast apparaat configuratie van beleid](./media/howto-authentication-passwordless-enable/intune-custom-profile.png)

### <a name="enable-credential-provider-via-provisioning-package"></a>Referentieprovider via inrichtingspakket inschakelen

Voor apparaten die niet worden beheerd door Intune, kan een inrichtingspakket waarmee de functionaliteit worden geïnstalleerd. De app Windows Configuration Designer kan worden geïnstalleerd vanaf de [Microsoft Store](https://www.microsoft.com/store/apps/9nblggh4tx22).

1. Start de Windows Configuration Designer.
1. Selecteer **bestand** > **nieuw project**.
1. Geef uw project een naam en noteer het pad waar het project is gemaakt.
1. Selecteer **Volgende**.
1. Laat **Provisioning pakket** geselecteerd als de **geselecteerde Projectwerkstroom** en selecteer **volgende**.
1. Selecteer **alle Windows-bureaubladtoepassingen** onder **kiezen welke instellingen u wilt weergeven en configureren** en selecteer **volgende**.
1. Selecteer **Finish**.
1. In het nieuwe project, bladert u naar **Runtime-instellingen** > **WindowsHelloForBusiness** > **SecurityKeys**  >  **UseSecurityKeyForSignIn**.
1. Stel **UseSecurityKeyForSignIn** naar **ingeschakeld**.
1. Selecteer **exporteren** > **Provisioning-pakket**
1. Laat de standaardwaarden in de **bouwen** venster onder **beschrijven het inrichtingspakket** en selecteer **volgende**.
1. Laat de standaardwaarden in de **bouwen** venster onder **Selecteer beveiligingsdetails voor het inrichtingspakket** en selecteer **volgende**.
1. Noteer of wijzig het pad in de **bouwen** windows onder **selecteert u waar om op te slaan van het inrichtingspakket** en selecteer **volgende**.
1. Selecteer **bouwen** op de **Build van het inrichtingspakket** pagina.
1. Sla de twee bestanden die zijn gemaakt (ppkg en cat) naar een locatie waar u deze toepassen op computers later opnieuw kunt.
1. Volg de instructies in het artikel [een inrichtingspakket toepassen](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-apply-package)om toe te passen van het inrichtingspakket die u hebt gemaakt.

## <a name="obtain-fido2-security-keys"></a>FIDO2 security-sleutels ophalen

Zie de sectie FIDO2 beveiligingssleutels, in het artikel [wat is de configuratie?](concept-authentication-passwordless.md) voor meer informatie over fabrikanten en ondersteunde sleutels.

> [!NOTE]
> Als u koopt en wilt gebruiken van sleutels voor NFC op basis van de beveiliging moet u een ondersteunde NFC-lezer.

## <a name="enable-passwordless-authentication-methods"></a>De configuratie verificatiemethoden inschakelen

### <a name="enable-the-combined-registration-experience"></a>De gecombineerde registratie-ervaring inschakelen

Registratie-functies voor FIDO2 beveiligingssleutels zijn afhankelijk van de gecombineerde registratie Preview-versie. Volg de stappen hieronder om de registratie van de gecombineerde Preview-functie inschakelen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com)
1. Blader naar **Azure Active Directory** > **gebruikersinstellingen**
   1. Klik op **beheren van instellingen voor toegang tot deelvenster preview-functies**
   1. Onder **gebruikers kunnen preview-functies gebruiken voor het registreren en beheren van beveiligingsgegevens - verbeterde**.
      1. Kies **geselecteerde** en kiest u een groep gebruikers die wordt opgenomen in de Preview-versie.
      1. Of kies **alle** om in te schakelen voor iedereen in uw directory.
1. Klik op **Opslaan**

### <a name="enable-new-passwordless-authentication-methods"></a>Nieuwe configuratie verificatiemethoden inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com)
1. Blader naar **Azure Active Directory** > **verificatiemethoden** > **methode verificatiebeleid (Preview)**
1. Onder elk **methode**, de volgende opties kiezen
   1. **Schakel** - Ja of Nee
   1. **Doel** -alle gebruikers of Selecteer gebruikers
1. **Sla** elke methode

> [!WARNING]
> De FIDO2 "Sleutel beleid voor softwarebeperking" niet nog werken. Deze functionaliteit is beschikbaar voor algemene beschikbaarheid, wijzig geen deze beleidsregels van de standaardwaarde.

> [!NOTE]
> U hoeft niet te meldt zich aan voor beide van de configuratie methoden (als u bekijken maar één voorbeeld van de configuratie wilt, kunt u deze methode inschakelen). U wordt aangeraden beide methoden proberen, aangezien ze beide hun eigen voordelen hebben.

## <a name="user-registration-and-management-of-fido2-security-keys"></a>Gebruikersregistratie en het beheer van beveiligingssleutels FIDO2

1. Ga naar [https://myprofile.microsoft.com](https://myprofile.microsoft.com)
1. Meld u aan in als dit nog niet
1. Klik op **beveiligingsgegevens**
   1. Als de gebruiker al ten minste één Azure multi-factor Authentication-methode geregistreerd heeft, kunnen ze onmiddellijk een beveiligingssleutel FIDO2 registreren.
   1. Als ze geen ten minste één Azure multi-factor Authentication-methode geregistreerd, moeten ze een toevoegen.
1. Een sleutel FIDO2 beveiliging toevoegen door te klikken op **-methode toevoegen** en het kiezen van **beveiligingssleutel**
1. Kies **USB-apparaat** of **NFC-apparaat**
1. Uw sleutel klaar en kies **volgende**
1. Een vak wordt weergegeven en wordt u gevraagd te maken/een PINCODE invoeren voor uw beveiligingssleutel vervolgens back-up gebaar van de vereiste voor uw sleutel biometrische of touch.
1. U wordt geretourneerd naar de gecombineerde registratie-ervaring en een beschrijvende naam op voor uw token opgeven, zodat u welke hebt u meerdere kunt identificeren. Klik op **Volgende**.
1. Klik op **gedaan** om het proces te voltooien

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Beveiligingssleutel biometrische, PINCODE, beheren of opnieuw instellen van beveiligingssleutel

* Windows 10 version 1809
   * Companion-software van de leverancier van de beveiliging is vereist
* Windows 10 versie 1903 of hoger
   * Gebruikers kunnen openen **Windows-instellingen** op hun apparaat > **Accounts** > **beveiligingssleutel**
   * Gebruikers kunnen hun PINCODE wijzigen, biometrie bijwerken of opnieuw instellen van hun beveiligingssleutel

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Gebruikersregistratie en het beheer van de Microsoft Authenticator-app

Voor het configureren van de Microsoft Authenticator-app voor aanmelding via telefoon, volg de instructies in het artikel [aanmelden bij uw accounts met behulp van de Microsoft Authenticator-app](../user-help/user-help-auth-app-sign-in.md).

## <a name="sign-in-with-passwordless-credentials"></a>Meld u aan met referenties voor de configuratie

### <a name="sign-in-at-the-lock-screen"></a>Meld u aan op het vergrendelingsscherm

In het voorbeeld hieronder een gebruiker Bala Sandhu al hun beveiligingssleutel FIDO2 ingericht. Bala kunt Kies de provider van de belangrijkste referentie beveiliging vanaf het vergrendelingsscherm van Windows 10 en voeg de beveiligingssleutel aan te melden bij Windows.

![Beveiligingssleutel zich aanmelden op het vergrendelingsscherm van Windows 10](./media/howto-authentication-passwordless-enable/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="sign-in-on-the-web"></a>Meld u op het web

In het voorbeeld hieronder een gebruiker heeft al hun beveiligingssleutel FIDO2 ingericht. De gebruiker kan kiezen om te ondertekenen op het web met hun beveiligingssleutel FIDO2 binnen de Microsoft Edge-browser op Windows 10 versie 1809 of hoger.

![Belangrijkste ondertekenen van de beveiliging in Microsoft Edge](./media/howto-authentication-passwordless-enable/fido2-windows-10-1903-edge-sign-in.png)

Zie het artikel voor informatie over het gebruik van de Microsoft Authenticator-app ondertekenen [aanmelden bij uw accounts met behulp van de Microsoft Authenticator-app](../user-help/user-help-auth-app-sign-in.md).

## <a name="known-issues"></a>Bekende problemen

### <a name="fido2-security-keys"></a>Beveiligingssleutels FIDO2

#### <a name="security-key-provisioning"></a>Security key inrichten

Beheerder inrichting en ongedaan maken inrichting van de sleutels van de beveiliging is niet beschikbaar in de openbare preview.

#### <a name="hybrid-azure-ad-join"></a>Hybride Azure AD-koppeling

WIA SSO afhankelijk zijn van gebruikers die gebruikmaken van beheerde referenties zoals FIDO2 beveiligingssleutels of zonder wachtwoord uitproberen aanmelden bij Microsoft Authenticator-app wilt hybride Join op Windows 10 om op te halen van de voordelen van eenmalige aanmelding. Echter beveiligingssleutels werken alleen voor Azure Active Directory worden gekoppeld machines nu. U wordt aangeraden dat u alleen FIDO2 sleutels voor het Windows-vergrendelingsscherm op pure Azure Active Directory worden gekoppeld machines uitproberen. Deze beperking geldt niet voor het web.

#### <a name="upn-changes"></a>UPN-wijzigingen

We werken op een functie waarmee u wijzigen in hybride toevoegen aan AAD UPN en toevoegen aan AAD-apparaten kunt ondersteunen. Als de UPN van een gebruiker wordt gewijzigd, kunt u de beveiligingssleutels FIDO2 voor die niet meer wijzigen. Dus de enige benadering is het apparaat opnieuw instelt en de gebruiker opnieuw registreren.

### <a name="authenticator-app"></a>Authenticator-app

#### <a name="ad-fs-integration"></a>AD FS-integratie

Wanneer een gebruiker heeft de Microsoft Authenticator zonder wachtwoord uitproberen referentie ingeschakeld, wordt altijd verificatie voor die gebruiker standaard een melding voor goedkeuring verzonden. Deze logica wordt voorkomen dat gebruikers in een hybride-tenant niet worden doorgestuurd naar AD FS voor verificatie van aanmelding zonder dat de gebruiker een extra stap te klikken op "Uw wachtwoord gebruiken in plaats daarvan." Dit proces wordt ook een on-premises-beleid voor voorwaardelijke toegang en stromen van Pass through-verificatie overslaan. De uitzondering op dit proces is als een login_hint is opgegeven, een gebruiker wordt automatisch doorgestuurd naar AD FS en overslaan van de optie voor het gebruik van de referentie op die de configuratie.

#### <a name="azure-mfa-server"></a>Azure MFA server

Eindgebruikers die zijn ingeschakeld voor MFA via on-premises Azure MFA-server van een organisatie kan nog steeds maken en een eenmalige configuratie phone teken in referentie gebruiken. Als de gebruiker probeert om meerdere installaties (5 +) van de Microsoft Authenticator met de referenties van een upgrade uitvoert, kan deze wijziging resulteert in een fout.  

#### <a name="device-registration"></a>Apparaatregistratie

Een van de vereisten voor het maken van deze referentie nieuw, sterk is dat het apparaat waarop deze zich bevindt in de Azure AD-tenant aan een afzonderlijke gebruiker is geregistreerd. Vanwege beperkingen van de registratie van apparaten, kan een apparaat slechts in één tenant worden geregistreerd. Deze limiet betekent dat er slechts één account voor werk of school in de Microsoft Authenticator-app kan worden ingeschakeld voor aanmelding via telefoon.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over het registreren van apparaten](../devices/overview.md)

[Meer informatie over Azure multi-factor Authentication](../authentication/howto-mfa-getstarted.md)
