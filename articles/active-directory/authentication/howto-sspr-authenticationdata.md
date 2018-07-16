---
title: Vereisten voor Azure AD SSPR-gegevens | Microsoft Docs
description: Eisen voor Azure AD Self-service voor wachtwoord opnieuw instellen en hoe u om te voldoen aan deze
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: ea6bf503eeba9e904c492a858139490b523a10cc
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044172"
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Wachtwoord opnieuw instellen zonder registratie door eindgebruikers implementeren

Gegevens van de verificatie moet aanwezig zijn voor het implementeren van Azure Active Directory (Azure AD) selfservice voor wachtwoordherstel (SSPR). Sommige organisaties hebben de gebruikers hun verificatiegegevens zelf opgeven. Maar in veel organisaties liever om te synchroniseren met gegevens die al in Active Directory. De gesynchroniseerde gegevens beschikbaar worden gesteld aan Azure AD en SSPR zonder dat tussenkomst van de gebruiker is vereist als u:
   * Juiste indeling van de gegevens in uw on-premises directory.
   * Configureer [Azure AD Connect met behulp van de express-instellingen](./../connect/active-directory-aadconnect-get-started-express.md).

Voor een goede werking telefoonnummers moet zich in de indeling *+ CountryCode PhoneNumber*, bijvoorbeeld: + 1 4255551234.

> [!NOTE]
> Er moet een spatie tussen de landcode en het telefoonnummer.
>
> Wachtwoord opnieuw instellen biedt geen ondersteuning voor extensies van de telefoon. Zelfs in de indeling van de 4255551234 + 1 X-12345, worden de extensies worden verwijderd voordat de oproep wordt gedaan.

## <a name="fields-populated"></a>Velden ingevuld

Als u de standaardinstellingen in Azure AD Connect gebruikt, worden de volgende toewijzingen gemaakt:

| On-premises Active Directory | Azure AD |
| --- | --- |
| telephoneNumber | Telefoon (werk) |
| mobiele | Mobiele telefoon |

Wanneer het mobiele telefoonnummer wordt gecontroleerd door een gebruiker, wordt het veld Phone onder de contactgegevens voor verificatie in Azure AD ook worden gevuld met dit getal.

## <a name="authentication-contact-info"></a>Contactgegevens voor verificatie

Een globale beheerder kan handmatig de contactgegevens voor verificatie instellen voor een gebruiker, zoals weergegeven in de volgende schermafbeelding.

![Neem contact op met][Contact]

Als het veld telefoon wordt ingevuld en mobiele telefoon in de SSPR-beleid is ingeschakeld, ziet de gebruiker dat nummer op de wachtwoordpagina opnieuw instellen van inschrijving en tijdens het wachtwoord opnieuw instellen van werkstroom. 

Het veld alternatief telefoonnummer wordt niet gebruikt voor wachtwoord opnieuw instellen.

Als het veld e-mailbericht wordt ingevuld en e-mailadres in de SSPR-beleid is ingeschakeld, ziet de gebruiker dat e-mailbericht op de wachtwoordpagina opnieuw instellen van inschrijving en tijdens het wachtwoord opnieuw instellen van werkstroom.

Als het veld alternatief e-mailadres wordt ingevuld en e-mailadres in de SSPR-beleid is ingeschakeld, wordt de gebruiker wordt **niet** zien dat e-mail op het wachtwoord opnieuw instellen van pagina voor de registratie, maar ze zien tijdens het wachtwoord opnieuw instellen van werkstroom. 


## <a name="security-questions-and-answers"></a>Beveiligingsvragen en antwoorden

De beveiligingsvragen en antwoorden zijn veilig opgeslagen in uw Azure AD-tenant en zijn alleen toegankelijk voor gebruikers via de [SSPR-registratieportal](https://aka.ms/ssprsetup). Beheerders kunnen bekijken of wijzigen van de inhoud van een andere gebruikers vragen en antwoorden.

### <a name="what-happens-when-a-user-registers"></a>Wat gebeurt er wanneer een gebruiker wordt geregistreerd

Wanneer een gebruiker registreert, wordt de registratiepagina van de volgende velden:

* **Telefoon voor authenticatie**
* **Verificatie-e-mailadres**
* **Beveiligingsvragen en antwoorden**

Als u hebt opgegeven een waarde voor **mobiele telefoon** of **alternatief e-mailadres**, gebruikers onmiddellijk deze waarden om hun wachtwoord opnieuw in te kunnen gebruiken, zelfs als ze nog niet hebt geregistreerd voor de service. Bovendien zien gebruikers deze waarden wanneer ze zich voor de eerste keer registreren, en ze deze wijzigen kunnen als ze willen. Nadat deze is geregistreerd, wordt deze waarden wordt bewaard in de **telefoon voor authenticatie** en **Verificatieadres** velden, respectievelijk.

## <a name="set-and-read-the-authentication-data-through-powershell"></a>Instellen en lezen van de gegevens van de verificatie via PowerShell

De volgende velden kunnen worden ingesteld via PowerShell:

* **Alternatief e-mailadres**
* **Mobiele telefoon**
* **Telefoon (werk)**: kan alleen worden ingesteld als u niet met een on-premises directory synchroniseert

### <a name="use-powershell-version-1"></a>Gebruik PowerShell versie 1

Als u wilt beginnen, moet u [downloaden en installeren van de Azure AD PowerShell-module](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Nadat u deze geïnstalleerd hebt, kunt u de volgende stappen voor het configureren van elk veld.

#### <a name="set-the-authentication-data-with-powershell-version-1"></a>Instellen dat de verificatiegegevens met PowerShell versie 1

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-1"></a>Lezen van de gegevens van de verificatie met PowerShell versie 1

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="read-the-authentication-phone-and-authentication-email-options"></a>De opties voor telefoon voor authenticatie en verificatie-e-mailbericht lezen

Om te lezen de **telefoon voor authenticatie** en **Verificatieadres** wanneer u PowerShell versie 1, gebruik de volgende opdrachten:

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="use-powershell-version-2"></a>Gebruik PowerShell versie 2

Als u wilt beginnen, moet u [downloaden en installeren van de Azure AD PowerShell-module voor versie 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0). Nadat u deze geïnstalleerd hebt, kunt u de volgende stappen voor het configureren van elk veld.

Voer de volgende opdrachten voor het snel van recente versies van PowerShell die ondersteuning bieden voor Install-Module installeren. (De eerste regel controleert of de module al is geïnstalleerd.)

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-the-authentication-data-with-powershell-version-2"></a>Instellen dat de verificatiegegevens met PowerShell versie 2

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-2"></a>Lezen van de gegevens van de verificatie met PowerShell versie 2

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Volgende stappen

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](howto-sspr-deployment.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](../user-help/active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](../user-help/active-directory-passwords-reset-register.md)
* [Hebt u een vraag over licenties?](concept-sspr-licensing.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](concept-sspr-howitworks.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](concept-sspr-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](howto-sspr-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](howto-sspr-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](concept-sspr-howitworks.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

[Contact]: ./media/howto-sspr-authenticationdata/user-authentication-contact-info.png "Globale beheerders kunnen de contactgegevens voor verificatie van een gebruiker wijzigen"
