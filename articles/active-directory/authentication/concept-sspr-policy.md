---
title: Beleid voor Azure AD-selfservice voor wachtwoordherstel
description: Opties voor Azure AD Self-service voor wachtwoord opnieuw instellen configureren
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.openlocfilehash: 2b0e96186221908352771a80803c17b772d660be
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54431845"
---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Wachtwoordbeleid en beperkingen in Azure Active Directory

Dit artikel wordt beschreven voor het wachtwoordbeleid en -vereisten voor wachtwoordcomplexiteit die zijn gekoppeld met behulp van gebruikersaccounts in uw tenant Azure Active Directory (Azure AD).

## <a name="administrator-reset-policy-differences"></a>Beheerder opnieuw instellen van beleid verschillen

**Microsoft dwingt een sterk standaardbeleid *twee-gate* wachtwoord opnieuw instellen voor alle Azure-beheerdersrol** dit beleid kan afwijken van de waarde die u voor uw gebruikers hebt gedefinieerd en kan niet worden gewijzigd. U moet altijd functionaliteit voor het wachtwoord opnieuw instellen als een gebruiker testen zonder een Azure-beheerdersrollen toegewezen.

Met een twee-gate-beleid, **beheerders niet hebben de mogelijkheid om de beveiligingsvragen gebruiken als**.

Het beleid voor twee-gate vereist twee soorten verificatiegegevens, zoals een **e-mailadres**, **authenticator-app**, of een **telefoonnummer**. Een twee-gate-beleid van toepassing is in de volgende omstandigheden:

* De volgende Azure-beheerdersrollen zijn hierbij betrokken:
  * Helpdesk-beheerder
  * Serviceondersteuningsbeheerder
  * Factureringsbeheerder
  * Laag1-ondersteuning voor partner
  * Laag2-ondersteuning voor partner
  * Exchange Service-beheerder
  * Lync Service-beheerder
  * Beheerder van gebruikersaccounts
  * Schrijvers van mappen
  * Globale beheerder of bedrijfsbeheerder
  * SharePoint-servicebeheerder
  * Beheerder voor naleving
  * Toepassingsbeheerder
  * Beveiligingsbeheerder
  * Beheerder met bevoorrechte rol
  * Microsoft Intune-servicebeheerder
  * Servicebeheerder voor toepassingsproxy
  * CRM-servicebeheerder
  * Power BI-servicebeheerder

* Als de 30 dagen zijn verstreken in een proefabonnement; of
* Een aangepast domein is aanwezig, zoals contoso.com; of
* Azure AD Connect synchroniseert identiteiten van uw on-premises directory

### <a name="exceptions"></a>Uitzonderingen

Een beleid voor één gate vereist een stukje authenticatiegegevens, zoals een e-mailadres *of* telefoonnummer. Een één-gate-beleid van toepassing is in de volgende omstandigheden:

* Het is binnen de eerste 30 dagen van een proefabonnement; of
* Een aangepast domein niet aanwezig (*. onmicrosoft.com); en
* Azure AD Connect is niet identiteiten synchroniseren

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName-beleidsregels die van toepassing op alle gebruikersaccounts

Elke gebruikersaccount die u zich moet aanmelden bij Azure AD moet een unieke gebruikers principal name (UPN)-kenmerkwaarde die is gekoppeld aan hun account hebben. De volgende tabel geeft een overzicht van de beleidsregels die van toepassing op zowel on-premises Active Directory-gebruikersaccounts die zijn gesynchroniseerd naar de cloud en gebruikersaccounts die alleen in de cloud:

| Eigenschap | UserPrincipalName-vereisten |
| --- | --- |
| Toegestane tekens |<ul> <li>A – Z</li> <li>a - z</li><li>0 – 9</li> <li> ' \. - \_ ! \# ^ \~</li></ul> |
| Zijn niet toegestaan |<ul> <li>Een "\@ \" teken dat niet van de gebruikersnaam van het domein scheiden wordt.</li> <li>Mag een periode teken niet bevatten "." direct vóór de '\@ \" symbool</li></ul> |
| Lengtebeperkingen |<ul> <li>De totale lengte mag niet groter zijn dan 113 tekens</li><li>Er mag maximaal 64 tekens vóór de '\@ \" symbool</li><li>Er mag maximaal 48 tekens na het '\@ \" symbool</li></ul> |

## <a name="password-policies-that-only-apply-to-cloud-user-accounts"></a>Wachtwoordbeleid die alleen van toepassing op cloud-gebruikersaccounts

De volgende tabel beschrijft de beschikbare wachtwoord-beleidsinstellingen die kunnen worden toegepast op gebruikersaccounts die zijn gemaakt en beheerd in Azure AD:

| Eigenschap | Vereisten |
| --- | --- |
| Toegestane tekens |<ul><li>A – Z</li><li>a - z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ " ( ) ;</li></ul> |
| Zijn niet toegestaan |<ul><li>Unicode-tekens.</li><li>Spaties.</li><li> Alleen sterke wachtwoorden: Mag een punt-teken niet bevatten "." Direct vóór de '\@ \" symbool ".</li></ul> |
| Wachtwoordbeperkingen |<ul><li>Ten minste 8 tekens en maximaal 16 tekens bestaan.</li><li>Alleen sterke wachtwoorden: Moet drie van vier van de volgende opties:<ul><li>Kleine letters.</li><li>Hoofdletters.</li><li>Cijfers (0-9).</li><li>De symbolen (Zie de vorige wachtwoordbeperkingen).</li></ul></li></ul> |
| Duur van de wachtwoord-vervaldatum |<ul><li>Standaardwaarde: **90** dagen.</li><li>De waarde kan worden geconfigureerd met behulp van de `Set-MsolPasswordPolicy` cmdlet uit de Azure Active Directory-Module voor Windows PowerShell.</li></ul> |
| Melding van wachtwoord-vervaldatum |<ul><li>Standaardwaarde: **14** dagen (voordat wachtwoord is verlopen).</li><li>De waarde kan worden geconfigureerd met behulp van de `Set-MsolPasswordPolicy` cmdlet.</li></ul> |
| Wachtwoord verloopt |<ul><li>Standaardwaarde: **false** dagen (geeft aan dat wachtwoord-vervaldatum is ingeschakeld).</li><li>De waarde voor afzonderlijke gebruikersaccounts kan worden geconfigureerd met behulp van de `Set-MsolUser` cmdlet.</li></ul> |
| Geschiedenis van apparaatwachtwoorden wijzigen |Het laatste wachtwoord *kan geen* opnieuw worden gebruikt wanneer de gebruiker een wachtwoord wijzigt. |
| Geschiedenis voor wachtwoord opnieuw instellen | Het laatste wachtwoord *kunt* opnieuw worden gebruikt wanneer de gebruiker een vergeten wachtwoord opnieuw instellen. |
| Accountvergrendeling |Na 10 mislukte aanmeldpogingen met een onjuist wachtwoord, kunnen de gebruiker is vergrendeld voor één minuut. Verder aanmeldingspogingen onjuiste uitsluiting van de gebruiker voor het verhogen van de duur van de tijd. |

## <a name="set-password-expiration-policies-in-azure-ad"></a>Verloopbeleid voor wachtwoorden instellen in Azure AD

Een globale beheerder voor een Microsoft-cloudservice kunt u de Microsoft Azure AD-Module voor Windows PowerShell gebruiken om in te stellen wachtwoorden van gebruikers niet om te verlopen. U kunt ook Windows PowerShell-cmdlets gebruiken om te verwijderen de-verloopt nooit configuratie of om te zien welke gebruiker wachtwoorden zijn ingesteld op nooit verlopen. 

Deze handleiding is van toepassing op andere providers, zoals Intune en Office 365, die ook afhankelijk van Azure AD voor identiteits- en directory services zijn. Wachtwoord verloopt, is het enige deel van het beleid dat kan worden gewijzigd.

> [!NOTE]
> Alleen de wachtwoorden voor gebruikersaccounts die niet zijn gesynchroniseerd via adreslijstsynchronisatie kunnen worden geconfigureerd voor het niet is verlopen. Zie voor meer informatie over synchronisatie van directory [AD met Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>

## <a name="set-or-check-the-password-policies-by-using-powershell"></a>Instellen of het wachtwoordbeleid controleren met behulp van PowerShell

Als u wilt beginnen, moet u [downloaden en installeren van de Azure AD PowerShell-module](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). Nadat u deze geïnstalleerd hebt, kunt u de volgende stappen uit om te configureren van elk veld.

### <a name="check-the-expiration-policy-for-a-password"></a>Het vervalbeleid voor wachtwoord controleren

1. Verbinding maken met Windows PowerShell met behulp van de administrator-referenties van uw bedrijf.
1. Voer een van de volgende opdrachten:

   * Als u wilt zien als het wachtwoord van een enkele gebruiker is ingesteld op nooit verlopen, kunt u de volgende cmdlet uitvoeren met behulp van de UPN (bijvoorbeeld *aprilr@contoso.onmicrosoft.com*) of de gebruikers-ID van de gebruiker die u wilt controleren: `Get-AzureADUser -ObjectId <user ID> | Select-Object @{N="PasswordNeverExpires";E={$_.PasswordPolicies -contains "DisablePasswordExpiration"}}`
   * Om te zien de **wachtwoord verloopt nooit** instellen voor alle gebruikers, voert u de volgende cmdlet: `Get-AzureADUser -All $true | Select-Object UserPrincipalName, @{N="PasswordNeverExpires";E={$_.PasswordPolicies -contains "DisablePasswordExpiration"}}`

### <a name="set-a-password-to-expire"></a>Stel een wachtwoord verlopen

1. Verbinding maken met Windows PowerShell met behulp van de administrator-referenties van uw bedrijf.
1. Voer een van de volgende opdrachten:

   * Als u wilt het wachtwoord van een gebruiker zo instellen dat het wachtwoord is verlopen, moet u de volgende cmdlet uitvoeren met behulp van de UPN of de gebruikers-ID van de gebruiker: `Set-AzureADUser -ObjectId <user ID> -PasswordPolicies None`
   * Om in te stellen de wachtwoorden voor alle gebruikers in de organisatie, zodat ze zijn verlopen, gebruikt u de volgende cmdlet: `Get-AzureADUser -All $true | Set-AzureADUser -PasswordPolicies None`

### <a name="set-a-password-to-never-expire"></a>Stel een wachtwoord op nooit verlopen

1. Verbinding maken met Windows PowerShell met behulp van de administrator-referenties van uw bedrijf.
1. Voer een van de volgende opdrachten:

   * Om het wachtwoord van een gebruiker op nooit verlopen, moet u de volgende cmdlet uitvoeren met behulp van de UPN of de gebruikers-ID van de gebruiker: `Set-AzureADUser -ObjectId <user ID> -PasswordPolicies DisablePasswordExpiration`
   * Om in te stellen de wachtwoorden voor alle gebruikers in een organisatie op nooit verlopen, moet u de volgende cmdlet uitvoeren: `Get-AzureADUser -All $true | Set-AzureADUser -PasswordPolicies DisablePasswordExpiration`

   > [!WARNING]
   > Wachtwoorden is ingesteld op `-PasswordPolicies DisablePasswordExpiration` nog steeds leeftijd op basis van de `pwdLastSet` kenmerk. Als u de wachtwoorden nooit verlopen en gaat u 90 dagen door, de wachtwoorden zijn verlopen. Op basis van de `pwdLastSet` kenmerk, als u de vervaldatum om te wijzigen `-PasswordPolicies None`, alle wachtwoorden die u hebt een `pwdLastSet` ouder dan 90 dagen de gebruiker moet te wijzigen van de volgende keer dat ze zich aanmelden. Deze wijziging kan invloed hebben op een groot aantal gebruikers. 

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen bevatten aanvullende informatie over wachtwoordherstel via Azure AD:

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](howto-sspr-deployment.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](../user-help/active-directory-passwords-update-your-own-password.md).
* [Registreer u voor selfservice voor wachtwoordherstel](../user-help/active-directory-passwords-reset-register.md).
* [Hebt u een vraag over licenties?](concept-sspr-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](howto-sspr-authenticationdata.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](concept-sspr-howitworks.md#authentication-methods)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](howto-sspr-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](howto-sspr-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](concept-sspr-howitworks.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)
