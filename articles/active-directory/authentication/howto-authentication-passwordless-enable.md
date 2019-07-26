---
title: Azure Active Directory aanmelding zonder wacht woord configureren (preview-versie)
description: Schakel wacht woordloze aanmelding bij Azure AD in met behulp van FIDO2-beveiligings sleutels of de Microsoft Authenticator-app (preview)
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
ms.openlocfilehash: ad2f25aac7f74c74eb63fd4666c5184ae751ec1f
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68499932"
---
# <a name="enable-passwordless-sign-in-for-azure-ad-preview"></a>Aanmelden zonder wacht woord voor Azure AD inschakelen (preview)

## <a name="requirements"></a>Vereisten

* Azure Multi-Factor Authentication
* Gecombineerde registratie Preview met ingeschakelde gebruikers voor SSPR
* Voor beeld van FIDO2-beveiligings sleutel zijn compatibele FIDO2-beveiligings sleutels vereist
* Webauthn vereist micro soft Edge op Windows 10 versie 1809 of hoger
* Voor FIDO2 gebaseerde Windows-aanmelding moet Azure AD zijn toegevoegd aan Windows 10 versie 1809 of hoger

## <a name="prepare-devices-for-preview"></a>Apparaten voorbereiden voor de preview-versie

Op apparaten waarop u wilt testen, moet Windows 10 versie 1809 of hoger worden uitgevoerd. De beste ervaring is met Windows 10 versie 1903 of hoger.

## <a name="enable-security-keys-for-windows-sign-in"></a>Beveiligings sleutels voor Windows-aanmelding inschakelen

Organisaties kunnen ervoor kiezen om een of meer van de volgende methoden te gebruiken om het gebruik van beveiligings sleutels voor Windows-aanmelding in te scha kelen.

### <a name="enable-credential-provider-via-intune"></a>Referentie provider inschakelen via intune

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Blader naar **Microsoft intune** > **registratie van Windows** > **Hello voor bedrijven** > -inschrijving > voor het apparaat.
1. Onder **instellingen** instellen **beveiligings sleutels gebruiken voor aanmelden** bij **ingeschakeld**.

Configuratie van beveiligings sleutels voor aanmelden is niet afhankelijk van het configureren van Windows hello voor bedrijven.

#### <a name="enable-targeted-intune-deployment"></a>Beoogde intune-implementatie inschakelen

Als u specifieke apparaatgroepen wilt instellen om de referentie provider in te scha kelen, gebruikt u de volgende aangepaste instellingen via intune. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Ga naar **Microsoft intune** > **apparaatconfiguratie** > profielenprofiel > **maken**.
1. Configureer het nieuwe profiel met de volgende instellingen
   1. Naam: Beveiligings sleutels voor Windows-aanmelding
   1. Beschrijving: Hiermee kunnen FIDO-beveiligings sleutels worden gebruikt tijdens Windows-aanmelding
   1. Platform: Windows 10 en hoger
   1. Platform type: Aangepast
   1. Aangepaste OMA-URI-instellingen:
      1. Naam: FIDO-beveiligings sleutels inschakelen voor Windows-aanmelding
      1. OMA-URI: ./Device/Vendor/MSFT/PassportForWork/SecurityKey/UseSecurityKeyForSignin
      1. Gegevens type: Geheel getal
      1. Waarde: 1 
1. Dit beleid kan worden toegewezen aan specifieke gebruikers, apparaten of groepen. Meer informatie vindt u in het artikel [gebruikers-en apparaatprofielen toewijzen in Microsoft intune](https://docs.microsoft.com/intune/device-profile-assign).

![Beleid voor aangepaste configuratie van intune-apparaten maken](./media/howto-authentication-passwordless-enable/intune-custom-profile.png)

### <a name="enable-credential-provider-via-provisioning-package"></a>Referentie provider inschakelen via inrichtings pakket

Voor apparaten die niet worden beheerd door intune, kan een inrichtings pakket worden geïnstalleerd om de functionaliteit in te scha kelen. De app Windows Configuration Designer kan worden geïnstalleerd vanuit de [Microsoft Store](https://www.microsoft.com/store/apps/9nblggh4tx22).

1. Start de ontwerp functie voor Windows-configuratie.
1. Selecteer **bestand** > **Nieuw project**.
1. Geef uw project een naam en noteer het pad waar het project is gemaakt.
1. Selecteer **Volgende**.
1. Verlaat het inrichtings **pakket** dat is geselecteerd als de **geselecteerde project werk stroom** en selecteer **volgende**.
1. Selecteer **alle Windows Desktop-edities** onder **Kies welke instellingen u wilt weer geven en configureren** en selecteer **volgende**.
1. Selecteer **Finish**.
1. Blader in het zojuist gemaakte project naar **runtime-instellingen** > **WindowsHelloForBusiness** > **SecurityKeys** > **UseSecurityKeyForSignIn**.
1. Stel **UseSecurityKeyForSignIn** in op **ingeschakeld**.
1. Inrichtings**pakket** **exporteren** > selecteren
1. Accepteer de standaard waarden in het venster **Build** onder **Beschrijving van het inrichtings pakket** en selecteer **volgende**.
1. Accepteer de standaard waarden in het venster **bouwen** onder **Selecteer beveiligings Details voor het inrichtings pakket** en selecteer **volgende**.
1. Noteer of wijzig het pad in de **Build** -Vensters onder **selecteren waar het inrichtings pakket moet worden opgeslagen** en selecteer **volgende**.
1. Selecteer **samen stellen** op de pagina **het inrichtings pakket bouwen** .
1. Sla de twee gemaakte bestanden (ppkg en Cat) op een locatie op waar u ze op een later tijdstip op de computers kunt Toep assen.
1. Volg de instructies in het artikel [een inrichtings pakket Toep assen](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-apply-package)om het inrichtings pakket toe te passen dat u hebt gemaakt.

## <a name="obtain-fido2-security-keys"></a>FIDO2-beveiligings sleutels verkrijgen

Zie de sectie FIDO2-beveiligings sleutels in het artikel [Wat is een wacht woord?](concept-authentication-passwordless.md) voor meer informatie over ondersteunde sleutels en fabrikanten.

> [!NOTE]
> Als u voor het kopen en plannen van NFC-beveiligings sleutels wilt gebruiken, hebt u een ondersteunde NFC-lezer nodig.

## <a name="enable-passwordless-authentication-methods"></a>Verificatie methoden met een wacht woord inschakelen

### <a name="enable-the-combined-registration-experience"></a>De gecombineerde registratie-ervaring inschakelen

Registratie functies voor FIDO2-beveiligings sleutels zijn afhankelijk van de gecombineerde registratie preview. Volg de onderstaande stappen om de gecombineerde registratie preview in te scha kelen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com)
1. Bladeren naar **Azure Active Directory** > **gebruikers instellingen**
   1. Klik op **instellingen voor de preview-versie van gebruikers onderdelen beheren**
   1. Onder **gebruikers kunnen preview-functies gebruiken voor het registreren en beheren van beveiligings gegevens-verbeterd**.
      1. Kies **geselecteerd** en kies een groep gebruikers die deel nemen aan de preview-versie.
      1. Of kies **Alles** om in te scha kelen voor iedereen in uw Directory.
1. Klik op **Opslaan**

### <a name="enable-new-passwordless-authentication-methods"></a>Nieuwe authenticatie methoden met een wacht woord inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com)
1. Blader naar **Azure Active Directory** > verificatie methode beleid voor verificatie**methoden** >  **(preview-versie)**
1. Onder elke **methode**kiest u de volgende opties:
   1. **Inschakelen** -ja of Nee
   1. **Doel** -alle gebruikers of Selecteer gebruikers
1. Elke methode **Opslaan**

> [!WARNING]
> Het FIDO2 ' sleutel restrictie beleid ' werkt nog niet. Deze functionaliteit is beschikbaar voor algemene Beschik baarheid. Wijzig deze beleids regels niet standaard.

> [!NOTE]
> U hoeft niet aan te melden bij beide methoden zonder wacht woord (als u slechts één methode met een wacht woord wilt bekijken, kunt u alleen die methode inschakelen). We raden u aan beide methoden uit te proberen, want ze hebben hun eigen voor delen.

## <a name="user-registration-and-management-of-fido2-security-keys"></a>Gebruikers registratie en het beheer van FIDO2-beveiligings sleutels

1. Ga naar [https://myprofile.microsoft.com](https://myprofile.microsoft.com)
1. Meld u aan als dat nog niet is gebeurd
1. Klik op **beveiligings gegevens**
   1. Als de gebruiker al ten minste één multi-factor Authentication-methode voor Azure heeft geregistreerd, kunnen ze onmiddellijk een FIDO2-beveiligings sleutel registreren.
   1. Als er niet ten minste één Azure multi-factor Authentication-methode is geregistreerd, moeten ze er een toevoegen.
1. Voeg een FIDO2-beveiligings sleutel toe door te klikken op **methode toevoegen** en **beveiligings sleutel** kiezen
1. **USB-apparaat** of **NFC-apparaat** kiezen
1. Laat uw sleutel gereed en kies **volgende**
1. Er wordt een vak weer gegeven waarin u wordt gevraagd om een pincode voor uw beveiligings sleutel te maken of in te voeren. vervolgens voert u de vereiste penbeweging voor uw sleutel biometrisch of Touch uit.
1. U wordt teruggekeerd naar de gecombineerde registratie-ervaring en u hebt gevraagd om een duidelijke naam op te geven voor uw token, zodat u kunt herkennen wat er allemaal zijn. Klik op **Volgende**.
1. Klik op **gereed** om het proces te volt ooien

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Beveiligings sleutel biometrisch, pincode of opnieuw instellen beveiligings sleutel beheren

* Windows 10 versie 1809
   * Er is een aanvullende software van de leverancier van de beveiligings sleutel vereist
* Windows 10 versie 1903 of hoger
   * Gebruikers kunnen **Windows-instellingen** op de**beveiligings sleutel** van hun apparaat > **accounts** > openen
   * Gebruikers kunnen hun pincode wijzigen, biometrie bijwerken of hun beveiligings sleutel opnieuw instellen

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Gebruikers registratie en het beheer van Microsoft Authenticator-app

Volg de instructies in het artikel [Aanmelden bij uw accounts met behulp van de Microsoft Authenticator-app](../user-help/user-help-auth-app-sign-in.md)om de Microsoft Authenticator-app te configureren voor aanmelden bij de telefoon.

## <a name="sign-in-with-passwordless-credentials"></a>Aanmelden met referenties voor een wacht woord

### <a name="sign-in-at-the-lock-screen"></a>Meld u aan op het vergrendelings scherm

In het voor beeld onder een gebruiker Bala Sandhu is de beveiligings sleutel FIDO2 al ingericht. Bala kan de referentie provider voor de beveiligings sleutel kiezen in het vergrendelings scherm van Windows 10 en de beveiligings sleutel voor aanmelding bij Windows invoegen.

![De beveiligings sleutel meldt zich aan bij het vergrendelings scherm van Windows 10](./media/howto-authentication-passwordless-enable/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="sign-in-on-the-web"></a>Aanmelden op het web

In het voor beeld onder een gebruiker heeft de FIDO2-beveiligings sleutel al ingericht. De gebruiker kan zich aanmelden op het web met de beveiligings sleutel FIDO2 in de micro soft Edge-browser van Windows 10 versie 1809 of hoger.

![Aanmeldings sleutel voor de beveiliging van micro soft Edge](./media/howto-authentication-passwordless-enable/fido2-windows-10-1903-edge-sign-in.png)

Voor informatie over het aanmelden met behulp van de app Microsoft Authenticator raadpleegt u het artikel en [meldt u zich aan bij uw accounts met behulp van de Microsoft Authenticator-app](../user-help/user-help-auth-app-sign-in.md).

## <a name="known-issues"></a>Bekende problemen

### <a name="fido2-security-keys"></a>FIDO2-beveiligings sleutels

#### <a name="security-key-provisioning"></a>Inrichten van beveiligings sleutel

Het inrichten van de beheerder en het ongedaan maken van de inrichting van beveiligings sleutels is niet beschikbaar in de open bare preview.

#### <a name="hybrid-azure-ad-join"></a>Hybride Azure AD-koppeling

Gebruikers die gebruikmaken van WIA SSO die beheerde referenties gebruiken, zoals FIDO2-beveiligings sleutels of aanmelden met een wacht woord waarbij Microsoft Authenticator app moet worden aangemeld bij een hybride verbinding van Windows 10, kunt u profiteren van de voor delen van SSO. Beveiligings sleutels werken echter alleen voor Azure Active Directory gekoppelde machines. We raden u aan om FIDO2-beveiligings sleutels voor het Windows-vergrendelings scherm op zuivere Azure Active Directory gekoppelde computers alleen uit te proberen. Deze beperking is niet van toepassing op het web.

#### <a name="upn-changes"></a>UPN-wijzigingen

Er wordt gewerkt aan de ondersteuning van een functie waarmee UPN kan worden gewijzigd op hybride AADJ-en AADJ-apparaten. Als de UPN van een gebruiker wordt gewijzigd, kunt u FIDO2-beveiligings sleutels niet meer wijzigen. De enige benadering is het opnieuw instellen van het apparaat en de gebruiker moet zich opnieuw registreren.

### <a name="authenticator-app"></a>Authenticator-app

#### <a name="ad-fs-integration"></a>Integratie van AD FS

Wanneer een gebruiker de Microsoft Authenticator wacht woordloze referentie heeft ingeschakeld, zal verificatie voor die gebruiker altijd standaard een melding verzenden voor goed keuring. Deze logica voor komt dat gebruikers in een hybride Tenant worden doorgestuurd naar ADFS voor aanmeldings verificatie zonder dat de gebruiker een extra stap hoeft te doen met het klikken op uw wacht woord gebruiken. Met dit proces worden ook alle on-premises beleids regels voor voorwaardelijke toegang en Pass Through-verificatie stromen genegeerd. De uitzonde ring op dit proces is als er een login_hint is opgegeven, een gebruiker wordt automatisch doorgestuurd naar AD FS en de optie voor het gebruik van wacht woordloze referenties overs Laan.

#### <a name="azure-mfa-server"></a>Azure MFA-server

Eind gebruikers die zijn ingeschakeld voor MFA via de on-premises Azure MFA-server van een organisatie, kunnen nog steeds één wacht woord voor eenmalige aanmelding op hetzelfde telefoon nummer maken en gebruiken. Als de gebruiker meerdere installaties (5 +) van de Microsoft Authenticator met de referentie probeert bij te werken, kan deze wijziging een fout veroorzaken.  

#### <a name="device-registration"></a>Apparaatregistratie

Een van de vereisten om deze nieuwe, sterke referentie te maken, is dat het apparaat waar het zich bevindt, is geregistreerd in de Azure AD-Tenant, naar een afzonderlijke gebruiker. Als gevolg van beperkingen voor apparaatregistratie, kan een apparaat alleen worden geregistreerd in één Tenant. Deze limiet betekent dat er slechts één werk-of school account in de Microsoft Authenticator-app kan worden ingeschakeld voor aanmelding via de telefoon.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over apparaatregistratie](../devices/overview.md)

[Meer informatie over Azure multi-factor Authentication](../authentication/howto-mfa-getstarted.md)
