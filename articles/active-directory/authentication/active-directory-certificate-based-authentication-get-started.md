---
title: Aan de slag met Azure Active Directory-verificatie op basis van certificaten
description: Informatie over het configureren van verificatie op basis van het certificaat in uw omgeving
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: annaba
ms.openlocfilehash: 37b0067b0879953ca4fbb16af146ba447f29e794
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55081565"
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a>Aan de slag met verificatie op basis van certificaten in Azure Active Directory

Verificatie op basis van certificaten kunt u moeten worden geverifieerd door Azure Active Directory met een clientcertificaat op een Windows-, Android- of iOS-apparaat verbinding te maken met uw Exchange online-account aan:

- Mobiele toepassingen van Microsoft, zoals Microsoft Outlook en Microsoft Word
- Exchange ActiveSync (EAS) clients

Configuratie van deze functie elimineert de noodzaak om een combinatie van gebruikersnaam en wachtwoord bepaalde e-mail en Microsoft Office-toepassingen in uw mobiele apparaat.

In dit onderwerp:

- Biedt u de stappen voor het configureren en gebruikmaken van verificatie op basis van certificaten voor gebruikers van tenants in Office 365 Enterprise, Business, onderwijs en US Government-abonnementen. Deze functie is beschikbaar in preview in Office 365 China, US Government Defense en Amerikaanse overheid federale overheid plannen.
- Wordt ervan uitgegaan dat u al een [openbare-sleutelinfrastructuur (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) en [AD FS](../hybrid/how-to-connect-fed-whatis.md) geconfigureerd.

## <a name="requirements"></a>Vereisten

Als u wilt configureren op certificaten gebaseerde verificatie, moeten de volgende instructies waar zijn:

- Certificaten gebaseerde verificatie wordt alleen ondersteund voor federatieve omgevingen voor browser- of systeemeigen clients met moderne verificatie (ADAL). De enige uitzondering hierop is Exchange Active Sync (EAS) voor Exchange Online (EXO), die kan worden gebruikt voor federatieve en beheerde accounts.
- De basiscertificeringsinstantie en tussenliggende certificeringsinstanties moeten worden geconfigureerd in Azure Active Directory.
- Elke CA moet een certificaatintrekkingslijst (CRL) die kan worden verwezen via de URL van een internetverbinding hebben.
- U moet ten minste één certificeringsinstantie geconfigureerd in Azure Active Directory hebben. U vindt gerelateerde stappen in de [configureren van de certificeringsinstanties](#step-2-configure-the-certificate-authorities) sectie.
- Voor Exchange ActiveSync-clients moet het clientcertificaat routeerbaar e-mailadres van de gebruiker in Exchange online in de Principal-naam of de waarde RFC822-naam van het veld alternatieve naam voor onderwerp. Azure Active Directory wordt de waarde RFC822 toegewezen aan het kenmerk Proxy-adres in de map.
- Uw client-apparaat moet toegang hebben tot ten minste één certificeringsinstantie die certificaten uitgeeft.
- Een clientcertificaat voor clientverificatie moet zijn uitgegeven aan de client.

## <a name="step-1-select-your-device-platform"></a>Stap 1: Selecteer uw apparaatplatform

U moet als eerste stap voor het apparaatplatform dat u geïnteresseerd bent, controleert u het volgende:

- Ondersteuning voor de mobiele Office-toepassingen
- De specifieke implementatie-vereisten

De gerelateerde gegevens bestaat voor de volgende apparaatplatformen:

- [Android](active-directory-certificate-based-authentication-android.md)
- [iOS](active-directory-certificate-based-authentication-ios.md)

## <a name="step-2-configure-the-certificate-authorities"></a>Stap 2: De certificeringsinstanties configureren

Als u wilt configureren van uw certificaat uploaden instanties in Azure Active Directory, voor elke certificeringsinstantie, het volgende:

* Het openbare gedeelte van het certificaat in *.cer* indeling
* De internetgerichte-URL's waarop de certificaatintrekkingslijsten (CRL's) zich bevinden

Het schema voor een certificeringsinstantie ziet er als volgt uit:

    class TrustedCAsForPasswordlessAuth
    {
       CertificateAuthorityInformation[] certificateAuthorities;
    }

    class CertificateAuthorityInformation

    {
        CertAuthorityType authorityType;
        X509Certificate trustedCertificate;
        string crlDistributionPoint;
        string deltaCrlDistributionPoint;
        string trustedIssuer;
        string trustedIssuerSKI;
    }

    enum CertAuthorityType
    {
        RootAuthority = 0,
        IntermediateAuthority = 1
    }

Voor de configuratie, kunt u de [Azure Active Directory PowerShell versie 2](/powershell/azure/install-adv2?view=azureadps-2.0):

1. Start Windows PowerShell met beheerdersbevoegdheden.
2. Installeer de Azure AD-moduleversie [2.0.0.33](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) of hoger.

        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33

Als een eerste configuratiestap moet u een verbinding maken met uw tenant. Zodra een verbinding met uw tenant bestaat, kunt u bekijken, toevoegen, verwijderen en wijzigen van de vertrouwde certificeringsinstanties die zijn gedefinieerd in uw directory.

### <a name="connect"></a>Verbinding maken

Als u wilt een verbinding maken met uw tenant, gebruikt u de [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet:

    Connect-AzureAD

### <a name="retrieve"></a>Ophalen

Als u wilt ophalen van de vertrouwde certificeringsinstanties die zijn gedefinieerd in uw directory, gebruiken de [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet.

    Get-AzureADTrustedCertificateAuthority

### <a name="add"></a>Toevoegen

Voor het maken van een vertrouwde certificeringsinstantie gebruikt de [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet en stel de **crlDistributionPoint** kenmerk voor een correcte waarde:

    $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]"
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation
    $new_ca.AuthorityType=0
    $new_ca.TrustedCertificate=$cert
    $new_ca.crlDistributionPoint="<CRL Distribution URL>"
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca

### <a name="remove"></a>Verwijderen

Als u wilt verwijderen van een vertrouwde certificeringsinstantie, gebruikt u de [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:

    $c=Get-AzureADTrustedCertificateAuthority
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2]

### <a name="modify"></a>Wijzigen

Als u wilt wijzigen van een vertrouwde certificeringsinstantie, gebruikt u de [Set AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:

    $c=Get-AzureADTrustedCertificateAuthority
    $c[0].AuthorityType=1
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0]

## <a name="step-3-configure-revocation"></a>Stap 3: Intrekking configureren

Als u wilt een clientcertificaat intrekt, Azure Active Directory worden opgehaald van de certificaatintrekkingslijst (CRL) via de URL's geüpload als onderdeel van de informatie over certificeringsinstantie en opgeslagen in het cachegeheugen. De laatste tijdstempel publiceren (**ingangsdatum** eigenschap) in de CRL wordt gebruikt om te controleren of de CRL is nog geldig. De CRL wordt periodiek verwezen om in te trekken van toegang tot de certificaten die deel van de lijst uitmaken.

Als een meer directe intrekken is vereist (bijvoorbeeld als een gebruiker verliest een apparaat), het Autorisatietoken van de gebruiker kan ongeldig worden gemaakt. Als u wilt het Autorisatietoken ongeldig te maken, stelt de **StsRefreshTokenValidFrom** veld voor deze bepaalde gebruiker met behulp van Windows PowerShell. U moet bijwerken de **StsRefreshTokenValidFrom** veld voor elke gebruiker die u wilt intrekken van toegang voor.

Om ervoor te zorgen dat de intrekking zich blijft voordoen, moet u instellen de **ingangsdatum** van de CRL naar een datum na de ingestelde waarde bij **StsRefreshTokenValidFrom** en zorg ervoor dat de betreffende certificaat bevindt zich in de CRL.

De volgende stappen beschrijven het proces voor het bijwerken en het Autorisatietoken ongeldig door in te stellen de **StsRefreshTokenValidFrom** veld.

**Het configureren van intrekken:**

1. Verbinding maken met Administrator-referenties voor de MSOL-service:

        $msolcred = get-credential
        connect-msolservice -credential $msolcred

2. De huidige StsRefreshTokensValidFrom-waarde voor een gebruiker ophalen:

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com`
        $user.StsRefreshTokensValidFrom

3. Configureer een nieuwe StsRefreshTokensValidFrom-waarde voor de gebruiker gelijk is aan de huidige tijdstempel:

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

De datum die u instelt moet in de toekomst worden. Als de datum niet in de toekomst wordt is de **StsRefreshTokensValidFrom** eigenschap niet is ingesteld. Als de datum in de toekomst **StsRefreshTokensValidFrom** is ingesteld op de huidige tijd (niet de datum die is aangegeven door de opdracht Set-MsolUser).

## <a name="step-4-test-your-configuration"></a>Stap 4: Configuratie van de testen

### <a name="testing-your-certificate"></a>Testen van uw certificaat

Als een eerste configuratietest, moet u proberen zich aanmeldt bij [Outlook Web Access](https://outlook.office365.com) of [SharePoint Online](https://microsoft.sharepoint.com) met behulp van uw **browser op het apparaat**.

Als de aanmelding geslaagd is, weet u dat:

- Het gebruikerscertificaat is ingericht voor uw testapparaat
- AD FS is juist geconfigureerd

### <a name="testing-office-mobile-applications"></a>Testen van mobiele Office-toepassingen

**Om te testen op certificaten gebaseerde verificatie op uw mobiele Office-toepassing:**

1. Installeer een mobiele Office-toepassing (bijvoorbeeld OneDrive) op uw testapparaat.
3. Start de toepassing.
4. Voer uw gebruikersnaam en selecteer vervolgens het gebruikerscertificaat dat u wilt gebruiken.

U moet worden aangemeld.

### <a name="testing-exchange-activesync-client-applications"></a>Testen van toepassingen voor Exchange ActiveSync-client

Voor toegang tot Exchange ActiveSync (EAS) via verificatie op basis van certificaten, moet een EAS-profiel met het clientcertificaat voor de toepassing beschikbaar.

De EAS-profiel moet de volgende informatie bevatten:

- Het certificaat van de gebruiker moet worden gebruikt voor verificatie

- De EAS-eindpunt (bijvoorbeeld outlook.office365.com)

Een EAS-profiel kan worden geconfigureerd en worden geplaatst op het apparaat via het gebruik van beheer van mobiele apparaten (MDM) zoals Intune, of door handmatig het certificaat in de EAS-profiel op het apparaat.

### <a name="testing-eas-client-applications-on-android"></a>Testen van toepassingen voor EAS-client op Android

**Voor het testen van verificatie via certificaat:**

1. Een EAS-profiel configureren in de toepassing die voldoet aan de vereisten in de vorige sectie.
2. Open de toepassing en controleer of dat e-mail wordt gesynchroniseerd.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over verificatie op basis van certificaten op Android-apparaten.](active-directory-certificate-based-authentication-android.md)

[Meer informatie over verificatie op basis van certificaten op iOS-apparaten.](active-directory-certificate-based-authentication-ios.md)
