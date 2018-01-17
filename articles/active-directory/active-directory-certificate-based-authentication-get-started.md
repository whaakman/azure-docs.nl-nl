---
title: Azure Active Directory gebaseerde verificatie - aan de slag | Microsoft Docs
description: Informatie over het configureren van verificatie op basis van een certificaat in uw omgeving
author: MarkusVi
documentationcenter: na
manager: mtillman
ms.assetid: c6ad7640-8172-4541-9255-770f39ecce0e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 5c96f33b8f678155dc4b7a84718e5eadc541f441
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a>Aan de slag met verificatie op basis van certificaten in Azure Active Directory

Verificatie op basis van certificaten kunt u moeten worden geverifieerd door Azure Active Directory met een clientcertificaat op een Windows-, Android of iOS-apparaat verbinding te maken met uw Exchange online-account in:

- Mobiele Microsoft-toepassingen zoals Microsoft Outlook en Microsoft Word   

- Exchange ActiveSync (EAS)-clients

Configuratie van deze functie wordt voorkomen moet een combinatie van gebruikersnaam en wachtwoord invoeren in bepaalde e-mail en Microsoft Office-toepassingen op uw mobiele apparaat.

Dit onderwerp:

- Biedt u de stappen voor het configureren en gebruikmaken van verificatie op basis van certificaten voor gebruikers van tenants in Office 365 Enterprise, Business, Education en US Government-plan. Deze functie is beschikbaar in preview in Office 365 China US Government verdediging en US Government Federal plannen.

- Wordt ervan uitgegaan dat er al een [openbare-sleutelinfrastructuur (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) en [AD FS](connect/active-directory-aadconnectfed-whatis.md) geconfigureerd.    


## <a name="requirements"></a>Vereisten

Voor het configureren van verificatie op basis van certificaten, moet het volgende worden voldaan:  

- Certificaat gebaseerde verificatie (CBA) wordt alleen ondersteund voor een federatieve omgeving voor browser- of systeemeigen clients met moderne authenticatie (ADAL). De enige uitzondering hierop is Exchange Active Sync (EAS) voor EXO die kan worden gebruikt voor zowel federatieve als beheerde accounts.

- De basiscertificeringsinstantie en tussenliggende certificeringsinstanties moeten worden geconfigureerd in Azure Active Directory.  

- Elke CA moet een certificaatintrekkingslijst (CRL) die kan worden verwezen via de URL van een internetverbinding hebben.  

- U moet ten minste één certificeringsinstantie geconfigureerd in Azure Active Directory hebben. U vindt gerelateerde stappen in de [configureren van de certificeringsinstanties](#step-2-configure-the-certificate-authorities) sectie.  

- Voor Exchange ActiveSync-clients moet het certificaat hebben routeerbaar e-mailadres van de gebruiker in Exchange online in de Principal-naam of de waarde RFC822-naam van het veld alternatieve naam voor onderwerp. Azure Active Directory wordt de waarde RFC822 toegewezen aan het proxyadres-kenmerk in de map.  

- Het clientapparaat moet toegang hebben tot ten minste één certificeringsinstantie die certificaten uitgeeft.  

- Een clientcertificaat voor clientverificatie moet zijn verleend aan de client.  




## <a name="step-1-select-your-device-platform"></a>Stap 1: Selecteer uw apparaatplatform

U moet als eerste stap voor het apparaatplatform dat u belangrijk vindt, controleert u het volgende:

- Ondersteuning voor de mobiele Office-toepassingen
- De specifieke implementatie-vereisten  

Verwante informatie bestaat voor de volgende apparaatplatforms:

- [Android](active-directory-certificate-based-authentication-android.md)
- [iOS](active-directory-certificate-based-authentication-ios.md)


## <a name="step-2-configure-the-certificate-authorities"></a>Stap 2: De certificeringsinstanties configureren

Voor het configureren van uw certificaat uploaden instanties in Azure Active Directory, voor elke certificeringsinstantie in het volgende:

* Het openbare deel van het certificaat in *.cer* indeling
* De Internetgericht URL's waar de certificaatintrekkingslijsten (CRL's) zich bevinden

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

1. Windows PowerShell starten met administratorbevoegdheden.
2. Installeer de Azure AD-module. U moet versie installeren [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) of hoger.  

        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33

Als een eerste stap in de configuratie moet u een verbinding maken met uw tenant. Zodra een verbinding met uw tenant bestaat, kunt u bekijken, toevoegen, verwijderen en wijzigen van de vertrouwde certificeringsinstanties die zijn gedefinieerd in uw directory.

### <a name="connect"></a>Verbinding maken

Gebruiken om een verbinding maken met uw tenant, het [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet:

    Connect-AzureAD


### <a name="retrieve"></a>Ophalen

Voor het ophalen van de vertrouwde certificeringsinstanties die zijn gedefinieerd in uw directory, gebruiken de [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet.

    Get-AzureADTrustedCertificateAuthority


### <a name="add"></a>Toevoegen

Gebruik voor het maken van een vertrouwde certificeringsinstantie de [nieuw AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet en stel de **crlDistributionPoint** kenmerk voor een correcte waarde:

    $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]"
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation
    $new_ca.AuthorityType=0
    $new_ca.TrustedCertificate=$cert
    $new_ca.crlDistributionPoint=”<CRL Distribution URL>”
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca


### <a name="remove"></a>Verwijderen

Voor het verwijderen van een vertrouwde certificeringsinstantie gebruiken de [verwijderen AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:

    $c=Get-AzureADTrustedCertificateAuthority
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2]


### <a name="modfiy"></a>Modfiy

Voor het wijzigen van een vertrouwde certificeringsinstantie gebruiken de [Set AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:

    $c=Get-AzureADTrustedCertificateAuthority
    $c[0].AuthorityType=1
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0]


## <a name="step-3-configure-revocation"></a>Stap 3: Configureer intrekken

Als u wilt een certificaat intrekken, Azure Active Directory haalt de certificaatintrekkingslijst (CRL) uit de URL's als onderdeel van de informatie over certificeringsinstantie geüpload en opgeslagen in het cachegeheugen. De laatste publiceren tijdstempel (**ingangsdatum** eigenschap) in de CRL wordt gebruikt om te controleren of de CRL is nog geldig. De CRL om in te trekken van de toegang tot de certificaten die deel van de lijst uitmaken periodiek naar wordt verwezen.

Als een meer instant intrekking is vereist (bijvoorbeeld als een gebruiker verliest een apparaat), kan het verificatietoken van de gebruiker ongeldig worden gemaakt. Zo het verificatietoken, stel de **StsRefreshTokenValidFrom** veld voor deze bepaalde gebruiker met behulp van Windows PowerShell. U moet bijwerken de **StsRefreshTokenValidFrom** veld voor elke gebruiker die u wilt intrekken van toegang voor.

Om ervoor te zorgen dat de intrekking zich blijft voordoen, moet u instellen de **ingangsdatum** van de CRL naar een datum na de waarde die is ingesteld door **StsRefreshTokenValidFrom** en zorg ervoor dat het betreffende certificaat is in de CRL.

De volgende stappen vatten het proces voor het bijwerken en het verificatietoken ongeldig te maken door in te stellen de **StsRefreshTokenValidFrom** veld.

**Intrekking configureren:**

1. Verbinding maken met beheerdersreferenties voor de service MSOL:

        $msolcred = get-credential
        connect-msolservice -credential $msolcred

2. De huidige StsRefreshTokensValidFrom-waarde voor een gebruiker opgehaald:

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com`
        $user.StsRefreshTokensValidFrom

3. Een nieuwe StsRefreshTokensValidFrom-waarde voor de gebruiker gelijk is aan de huidige tijdstempel configureert:

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

De datum die u instelt, moet in de toekomst zijn. Als de datum niet in de toekomst is de **StsRefreshTokensValidFrom** eigenschap is niet ingesteld. Als de datum in de toekomst **StsRefreshTokensValidFrom** is ingesteld op de huidige tijd (niet de datum aangegeven door de opdracht Set-MsolUser).


## <a name="step-4-test-your-configuration"></a>Stap 4: De configuratie van de testen

### <a name="testing-your-certificate"></a>Testen van uw certificaat

Als een eerste configuratietest, moet u proberen aan te melden bij [Outlook Web Access](https://outlook.office365.com) of [SharePoint Online](https://microsoft.sharepoint.com) met behulp van uw **browser op het apparaat**.

Als het aanmelden geslaagd is, weet u dat:

- Het gebruikerscertificaat is ingericht op uw testapparaat
- AD FS is juist geconfigureerd  


### <a name="testing-office-mobile-applications"></a>Mobiele Office-toepassingen testen

**Testen op certificaten gebaseerde verificatie op uw mobiele Office-toepassing:**

1. Installeer een mobiele Office-toepassing (zoals OneDrive) op uw testapparaat.
3. Start de toepassing.
4. Voer uw gebruikersnaam en selecteer vervolgens het gebruikerscertificaat dat u wilt gebruiken.

U moet worden aangemeld.

### <a name="testing-exchange-activesync-client-applications"></a>Exchange ActiveSync clienttoepassingen testen

Voor toegang tot Exchange ActiveSync (EAS) via op certificaten gebaseerde verificatie, moet een EAS-profiel met het clientcertificaat voor de toepassing beschikbaar.

De EAS-profiel, moet de volgende informatie bevatten:

- Het certificaat moet worden gebruikt voor verificatie

- Het EAS-eindpunt (bijvoorbeeld outlook.office365.com)

EAS-profiel kan worden geconfigureerd en worden geplaatst op het apparaat via het gebruik van beheer van mobiele apparaten (MDM) zoals Intune, of door het plaatsen van het certificaat handmatig in het EAS-profiel op het apparaat.  

### <a name="testing-eas-client-applications-on-android"></a>Testen van toepassingen voor EAS-client op Android

**Verificatie testen:**  

1. Een EAS-profiel configureren in de toepassing die voldoet aan de bovenstaande vereisten.  
2. Open de toepassing en controleer of dat e-mail wordt gesynchroniseerd.
