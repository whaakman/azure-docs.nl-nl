---
title: 'Beleid: Azure AD SSPR | Microsoft Docs'
description: Azure AD selfservice voor wachtwoordherstel beleidsopties
services: active-directory
keywords: Wachtwoordbeheer Active directory, wachtwoordbeheer, Azure AD self service voor wachtwoordherstel
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 9d61f46070e6956c60f1135b98a9ebe71011b922
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Wachtwoordbeleid en -beperkingen in Azure Active Directory

In dit artikel beschrijft de wachtwoordbeleid en vereisten voor wachtwoordcomplexiteit die zijn gekoppeld aan de gebruikersaccounts die zijn opgeslagen in uw Azure AD-tenant.

## <a name="administrator-password-policy-differences"></a>Administrator-wachtwoord beleid verschillen

Microsoft dwingt een sterk standaardbeleid via **twee gates** af voor Wachtwoord opnieuw instellen voor elke Azure-beheerdersrol (zoals de hoofdbeheerder, Helpdesk-beheerder, wachtwoordbeheerder, enzovoort)

Dit wordt uitgeschakeld beheerders beveiligingsvragen te gebruiken en het volgende wordt afgedwongen.

Twee gate-beleid, vereisen van twee soorten verificatiegegevens (e-mailadres **en** telefoonnummer), is van toepassing in de volgende omstandigheden

* Alle Azure-beheerdersrollen
  * Helpdesk-beheerder
  * Serviceondersteuningsbeheerder
  * Financieel medewerker
  * Partner Tier1-ondersteuning
  * Partner Tier2-ondersteuning
  * De beheerder van Exchange
  * Lync servicebeheerder
  * De beheerder van gebruiker
  * Directory-schrijvers
  * Globale beheerder/Company Administrator
  * SharePoint-servicebeheerder
  * Naleving beheerder
  * Toepassingsbeheerder
  * Beveiligingsbeheerder
  * Beheerder met bevoorrechte rol
  * Intune-servicebeheerder
  * Toepassingsbeheerder Proxy-Service
  * CRM-servicebeheerder
  * Power BI-servicebeheerder
  
* in een proefversie van 30 dagen verstreken **of**
* Vanity domein aanwezig is (contoso.com) **of**
* Azure AD Connect synchroniseert identiteiten van uw on-premises directory

### <a name="exceptions"></a>Uitzonderingen
Beleid voor één poort, gegevenselementen verificatie vereisen (e-mailadres **of** telefoonnummer), is van toepassing in de volgende omstandigheden

* De eerste 30 dagen van een proef **of**
* Vanity domein is niet aanwezig (*. onmicrosoft.com) **en** Azure AD Connect identiteiten niet synchroniseren


## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName beleidsregels die van toepassing op alle gebruikersaccounts

Elke gebruikersaccount die behoeften aan te melden bij Azure AD, moet een unieke gebruikers UPN (User Principal Name)-kenmerkwaarde die is gekoppeld aan hun account hebben. De tabel hieronder overzichten de beleidsregels die van toepassing op zowel on-premises Active Directory-gebruikersaccounts gesynchroniseerd met de cloud als met alleen-gebruikersaccounts.

| Eigenschap | UserPrincipalName vereisten |
| --- | --- |
| Toegestane tekens |<ul> <li>A-Z</li> <li>a - z</li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
| Niet-toegestane tekens |<ul> <li>Een ' @' teken dat niet is scheiden van de gebruikersnaam van het domein.</li> <li>Een periode teken niet bevatten '.' direct vóór de ' @' symbool</li></ul> |
| Lengte-beperkingen |<ul> <li>De totale lengte mag niet groter zijn dan 113 tekens</li><li>64 tekens lang voordat de ' @' symbool</li><li>48 tekens na de ' @' symbool</li></ul> |

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Wachtwoordbeleid die alleen van toepassing op cloud-gebruikersaccounts

De volgende tabel beschrijft de instellingen voor wachtwoordbeleid beschikbaar die kunnen worden toegepast op gebruikersaccounts die zijn gemaakt en beheerd in Azure AD.

| Eigenschap | Vereisten |
| --- | --- |
| Toegestane tekens |<ul><li>A-Z</li><li>a - z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
| Niet-toegestane tekens |<ul><li>Unicode-tekens</li><li>Spaties</li><li> **Sterke wachtwoorden alleen**: een punt-teken niet bevatten '.' direct vóór de ' @' symbool</li></ul> |
| Wachtwoordbeperkingen |<ul><li>minimum van 8 tekens en maximaal 16 tekens</li><li>**Sterke wachtwoorden alleen**: 3 buiten 4 van de volgende vereist:<ul><li>Kleine letters</li><li>Hoofdletters</li><li>Cijfers (0-9)</li><li>Symbolen (Zie wachtwoordbeperkingen hierboven)</li></ul></li></ul> |
| Wachtwoordverval |<ul><li>Standaardwaarde: **90** dagen </li><li>Waarde kan worden geconfigureerd met de cmdlet Set-MsolPasswordPolicy van de Azure Active Directory-Module voor Windows PowerShell.</li></ul> |
| Meldingen verlopen van wachtwoorden |<ul><li>Standaardwaarde: **14** dagen (voordat wachtwoord is verlopen)</li><li>Waarde kan worden geconfigureerd met de cmdlet Set-MsolPasswordPolicy.</li></ul> |
| Wachtwoord verloopt |<ul><li>Standaardwaarde: **false** dagen (geeft aan dat totdat het wachtwoord afloopt is ingeschakeld) </li><li>Waarde kan worden geconfigureerd voor afzonderlijke gebruikersaccounts met de cmdlet Set-MsolUser. </li></ul> |
| Wachtwoord **wijzigen** geschiedenis |Laatste wachtwoordupdate **kan niet** opnieuw worden gebruikt wanneer **wijzigen** een wachtwoord. |
| Wachtwoord **opnieuw** geschiedenis | Laatste wachtwoordupdate **mogelijk** opnieuw worden gebruikt wanneer **opnieuw instellen van** een vergeten wachtwoord. |
| Accountvergrendeling |Na 10 mislukte aanmeldpogingen (onjuist wachtwoord), wordt de gebruiker gedurende een minuut worden vergrendeld. Verder aanmelden onjuist vergrendeling van de gebruiker voor de duur te verhogen. |

## <a name="set-password-expiration-policies-in-azure-active-directory"></a>Beleidsregels voor het verlopen van wachtwoord instellen in Azure Active Directory

Een globale beheerder voor een cloudservice van Microsoft kunt u de Microsoft Azure Active Directory-Module voor Windows PowerShell gebruiken voor het instellen van wachtwoorden niet te laten verlopen. U kunt ook Windows PowerShell-cmdlets gebruiken om te verwijderen de-verloopt nooit configuratie of om te zien welke gebruiker wachtwoorden zijn ingesteld op niet verlopen. In deze richtlijnen is van toepassing op andere providers zoals Microsoft Intune en Office 365, die ook afhankelijk van Microsoft Azure Active Directory voor identiteits- en directory services zijn. Dit is het enige deel van het beleid dat kan worden gewijzigd.

> [!NOTE]
> Alleen de wachtwoorden voor gebruikersaccounts die niet zijn gesynchroniseerd door middel van directory-synchronisatie kunnen worden geconfigureerd voor het niet is verlopen. Zie voor meer informatie over adreslijstsynchronisatie[AD met Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>
>

## <a name="set-or-check-password-policies-using-powershell"></a>Instellen of wachtwoordbeleid met behulp van PowerShell controleren

Om te beginnen, moet u [downloaden en installeren van de Azure AD PowerShell-module](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). Nadat u geïnstalleerd hebt, kunt u de onderstaande stappen voor het configureren van elk veld volgen.

### <a name="how-to-check-expiration-policy-for-a-password"></a>Het vervalbeleid voor wachtwoord controleren
1. Verbinding maken met Windows PowerShell met de administrator-referenties van uw bedrijf.
2. Voer een van de volgende opdrachten:

   * Om te zien of het wachtwoord van een enkele gebruiker is ingesteld op nooit verlopen, kunt u de volgende cmdlet uitvoeren met behulp van de UPN (user Principal name) (bijvoorbeeld aprilr@contoso.onmicrosoft.com) of de gebruikers-ID van de gebruiker die u wilt controleren:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`
   * Overzicht van de instelling 'Wachtwoord verloopt nooit' voor alle gebruikers, voer de volgende cmdlet:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

### <a name="set-a-password-to-expire"></a>Een wachtwoord verloopt instellen

1. Verbinding maken met Windows PowerShell met de administrator-referenties van uw bedrijf.
2. Voer een van de volgende opdrachten:

   * Om het wachtwoord van een gebruiker zo instellen dat het wachtwoord is verlopen, kunt u de volgende cmdlet uitvoeren met behulp van de UPN (user Principal name) of de gebruikers-ID van de gebruiker:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`
   * Om in te stellen de wachtwoorden van alle gebruikers in de organisatie, zodat ze zijn verlopen, gebruikt u de volgende cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

### <a name="set-a-password-to-never-expire"></a>Een wachtwoord nooit verloopt instellen

1. Verbinding maken met Windows PowerShell met de administrator-referenties van uw bedrijf.
2. Voer een van de volgende opdrachten:

   * Stel het wachtwoord van een gebruiker nooit verlopen door de volgende cmdlet uitvoeren met behulp van de UPN (user Principal name) of de gebruikers-ID van de gebruiker:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`
   * Om in te stellen de wachtwoorden van alle gebruikers in een organisatie nooit verloopt, moet u de volgende cmdlet uitvoeren:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

   > [!WARNING]
   > Als u instelt `-PasswordNeverExpires $true` het wachtwoord wordt nog steeds leeftijd op basis van de `pwdLastSet` kenmerk. Dit betekent dat als u instellen dat wachtwoorden nooit verlopen en 90 dagen door op basis van gaat `pwdLastSet` en u kunt wijzigen `-PasswordNeverExpires $false` alle wachtwoorden die u hebt een `pwdLastSet` ouder is dan 90 dagen moet wijzigen bij volgende aanmelding. Deze wijziging kan invloed op een groot aantal gebruikers. 

## <a name="next-steps"></a>Volgende stappen

De volgende koppelingen bieden aanvullende informatie over wachtwoordherstel met behulp van Azure AD

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
