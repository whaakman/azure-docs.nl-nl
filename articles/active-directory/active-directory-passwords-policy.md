---
title: Selfservice voor wachtwoordherstel beleid - Azure Active Directory
description: Azure AD selfservice voor wachtwoordherstel beleidsopties
services: active-directory
keywords: Wachtwoordbeheer Active directory, wachtwoordbeheer, Azure AD self service voor wachtwoordherstel
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: ade7f1d3c868c2ce6ccedbbf11aaf7dc54706cff
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Wachtwoordbeleid en -beperkingen in Azure Active Directory

In dit artikel beschrijft de wachtwoordbeleid en vereisten voor wachtwoordcomplexiteit die zijn gekoppeld aan de gebruikersaccounts die zijn opgeslagen in uw tenant van Azure Active Directory (Azure AD).

## <a name="administrator-password-policy-differences"></a>Administrator-wachtwoord beleid verschillen

Microsoft dwingt een sterke standaard *twee gate* beleid voor elk Azure beheerdersrol voor wachtwoordherstel. 

Met een beleid twee gate hebben niet beheerders de mogelijkheid om met beveiligingsvragen.

 Een beleid twee gate vereist twee soorten verificatiegegevens, zoals een e-mailadres *en* een telefoonnummer. Een twee-gate-beleid van toepassing is in de volgende omstandigheden:

* De volgende Azure-beheerdersrollen worden beïnvloed:
  * Helpdesk-beheerder
  * Serviceondersteuningsbeheerder
  * Factureringsbeheerder
  * Laag1-ondersteuning voor partner
  * Laag2-ondersteuning voor partner
  * Exchange Service-beheerder
  * Lync Service-beheerder
  * De beheerder van gebruiker
  * Schrijvers van mappen
  * Globale beheerder of bedrijfsbeheerder
  * SharePoint-servicebeheerder
  * Beheerder voor naleving
  * Toepassingsbeheerder
  * Beveiligingsbeheerder
  * Beheerder met bevoorrechte rol
  * Microsoft Intune-servicebeheerder
  * Toepassingsbeheerder proxy-service
  * CRM-servicebeheerder
  * Power BI-servicebeheerder
  
* Als er 30 dagen zijn verstreken in een proefabonnement

  of

* Er is een vanity domein aanwezig is, zoals contoso.com

  of

* Azure AD Connect synchroniseert identiteiten van uw on-premises directory

### <a name="exceptions"></a>Uitzonderingen
Een beleid een gate vereist een stukje informatie, verificatie, zoals een e-mailadres *of* telefoonnummer. Een één-gate-beleid van toepassing is in de volgende omstandigheden:

* Het is in de eerste 30 dagen van een proefabonnement

  of

* Een vanity-domein is niet aanwezig (*. onmicrosoft.com) 

  en 

  Azure AD Connect is niet identiteiten synchroniseren


## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName beleidsregels die van toepassing op alle gebruikersaccounts

Elke gebruikersaccount die behoeften aan te melden bij Azure AD, moet een unieke gebruikers UPN (User Principal Name)-kenmerkwaarde die is gekoppeld aan hun account hebben. De volgende tabel die worden beschreven de beleidsregels die van toepassing op zowel lokale Active Directory-gebruikersaccounts die zijn gesynchroniseerd met de cloud en alleen in de cloud gebruikersaccounts:

| Eigenschap | UserPrincipalName vereisten |
| --- | --- |
| Toegestane tekens |<ul> <li>A – Z</li> <li>a - z</li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
| Niet-toegestane tekens |<ul> <li>Een ' @ ' teken dat niet de gebruikersnaam van het domein tussen.</li> <li>Een periode teken niet bevatten "." direct vóór de ' @ ' symbool</li></ul> |
| Lengte-beperkingen |<ul> <li>De totale lengte mag niet groter zijn dan 113 tekens</li><li>Er mag maximaal 64 tekens lang voordat de ' @ ' symbool</li><li>Er mag maximaal 48 tekens na de ' @ ' symbool</li></ul> |

## <a name="password-policies-that-only-apply-to-cloud-user-accounts"></a>Wachtwoordbeleid die alleen van toepassing op cloud-gebruikersaccounts

De volgende tabel beschrijft de instellingen voor wachtwoordbeleid beschikbaar die kunnen worden toegepast op gebruikersaccounts die zijn gemaakt en beheerd in Azure AD:

| Eigenschap | Vereisten |
| --- | --- |
| Toegestane tekens |<ul><li>A – Z</li><li>a - z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
| Niet-toegestane tekens |<ul><li>Unicode-tekens.</li><li>Spaties.</li><li> Sterke wachtwoorden alleen: een punt-teken niet bevatten "." direct vóór de ' @ ' symbool.</li></ul> |
| Wachtwoordbeperkingen |<ul><li>Ten minste 8 tekens en maximaal 16 tekens bestaan.</li><li>Sterke wachtwoorden alleen: drie op vier van de volgende vereist:<ul><li>Kleine letters.</li><li>Hoofdletters.</li><li>Cijfers (0-9).</li><li>Symbolen (Zie de vorige wachtwoordbeperkingen).</li></ul></li></ul> |
| Wachtwoordverval |<ul><li>Standaardwaarde: **90** dagen.</li><li>De waarde kan worden geconfigureerd met behulp van de `Set-MsolPasswordPolicy` cmdlet uit de Azure Active Directory-Module voor Windows PowerShell.</li></ul> |
| Meldingen verlopen van wachtwoorden |<ul><li>Standaardwaarde: **14** dagen (voordat wachtwoord is verlopen).</li><li>De waarde kan worden geconfigureerd met behulp van de `Set-MsolPasswordPolicy` cmdlet.</li></ul> |
| Wachtwoord verloopt |<ul><li>Standaardwaarde: **false** dagen (geeft aan dat totdat het wachtwoord afloopt is ingeschakeld).</li><li>De waarde kan worden geconfigureerd voor afzonderlijke gebruikersaccounts met behulp van de `Set-MsolUser` cmdlet.</li></ul> |
| Wachtwoordgeschiedenis wijzigen |Het laatste wachtwoord *kan niet* opnieuw worden gebruikt wanneer de gebruiker een wachtwoord wijzigt. |
| Geschiedenis-wachtwoord opnieuw instellen | Het laatste wachtwoord *kunt* opnieuw worden gebruikt wanneer de gebruiker een vergeten wachtwoord opnieuw instelt. |
| Accountvergrendeling |Na 10 mislukte aanmeldpogingen met een onjuist wachtwoord, de gebruiker gedurende een minuut vergrendeld. Verder aanmelden onjuist vergrendeling van de gebruiker om de duur van de tijd te vergroten. |

## <a name="set-password-expiration-policies-in-azure-ad"></a>Beleidsregels voor het verlopen van wachtwoord instellen in Azure AD

Een globale beheerder voor een cloudservice van Microsoft kunt de Microsoft Azure AD-Module voor Windows PowerShell gebruiken om in te stellen wachtwoorden niet te laten verlopen. U kunt ook Windows PowerShell-cmdlets gebruiken om te verwijderen de-verloopt nooit configuratie of om te zien welke gebruiker de wachtwoorden nooit verlopen zijn ingesteld. 

In deze richtlijnen is van toepassing op andere providers, zoals Intune en Office 365, die ook afhankelijk van Azure AD voor identiteits- en directory services zijn. Verlopen van wachtwoorden is het enige deel van het beleid dat kan worden gewijzigd.

> [!NOTE]
> Alleen de wachtwoorden voor gebruikersaccounts die niet zijn gesynchroniseerd door middel van directory-synchronisatie kunnen worden geconfigureerd voor het niet is verlopen. Zie voor meer informatie over adreslijstsynchronisatie [AD met Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>
>

## <a name="set-or-check-the-password-policies-by-using-powershell"></a>Instellen of het wachtwoordbeleid controleren met behulp van PowerShell

Om te beginnen, moet u [downloaden en installeren van de Azure AD PowerShell-module](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). Nadat u geïnstalleerd hebt, kunt u de volgende stappen uit voor het configureren van elk veld.

### <a name="how-to-check-the-expiration-policy-for-a-password"></a>Het controleren van het vervalbeleid voor wachtwoord
1. Verbinding maken met Windows PowerShell met de beheerdersreferenties van uw bedrijf.
2. Voer een van de volgende opdrachten:

   * Als u wilt zien als een enkele gebruiker-wachtwoord is ingesteld op nooit verlopen, kunt u de volgende cmdlet uitvoeren met behulp van de UPN (bijvoorbeeld  *aprilr@contoso.onmicrosoft.com* ) of de gebruikers-ID van de gebruiker die u wilt controleren:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`
   * Om te zien de **wachtwoord verloopt nooit** instellen voor alle gebruikers, voer de volgende cmdlet:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

### <a name="set-a-password-to-expire"></a>Een wachtwoord verloopt instellen

1. Verbinding maken met Windows PowerShell met de beheerdersreferenties van uw bedrijf.
2. Voer een van de volgende opdrachten:

   * Om het wachtwoord van een gebruiker zo instellen dat het wachtwoord is verlopen, moet u de volgende cmdlet uitvoeren met behulp van de UPN of de gebruikers-ID van de gebruiker:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`
   * Om in te stellen de wachtwoorden van alle gebruikers in de organisatie, zodat ze zijn verlopen, gebruikt u de volgende cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

### <a name="set-a-password-to-never-expire"></a>Een wachtwoord nooit verloopt instellen

1. Verbinding maken met Windows PowerShell met de beheerdersreferenties van uw bedrijf.
2. Voer een van de volgende opdrachten:

   * Als u wilt dat het wachtwoord van een gebruiker nooit verloopt, moet u de volgende cmdlet uitvoeren met behulp van de UPN of de gebruikers-ID van de gebruiker:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`
   * Om in te stellen de wachtwoorden van alle gebruikers in een organisatie nooit verloopt, moet u de volgende cmdlet uitvoeren:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

   > [!WARNING]
   > Wachtwoorden die zijn ingesteld op `-PasswordNeverExpires $true` nog leeftijd op basis van de `pwdLastSet` kenmerk. Als u de wachtwoorden nooit verlopen instellen en 90 dagen gaat door, de wachtwoorden zijn verlopen. Op basis van de `pwdLastSet` kenmerk, als u de vervaldatum voor wijzigt `-PasswordNeverExpires $false`, alle wachtwoorden die u hebt een `pwdLastSet` ouder is dan 90 dagen de gebruiker moet te wijzigen van de volgende keer dat ze zich aanmelden. Deze wijziging kan invloed hebben op een groot aantal gebruikers. 

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen bevatten aanvullende informatie over het wachtwoord opnieuw instellen via Azure AD:

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](active-directory-passwords-update-your-own-password.md).
* [Registreer u voor selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md).
* [Hebt u een vraag over licenties?](active-directory-passwords-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](active-directory-passwords-data.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](active-directory-passwords-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](active-directory-passwords-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)
