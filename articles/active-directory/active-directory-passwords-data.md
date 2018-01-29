---
title: Vereisten voor Azure AD SSPR-gegevens | Microsoft Docs
description: Eisen voor Azure AD zelf uw wachtwoord opnieuw instellen en hoe ze voldoen aan
services: active-directory
keywords: 
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
ms.custom: it-pro
ms.openlocfilehash: 504316a7d09595d24318d1f574b0f8927914d424
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Wachtwoord opnieuw instellen zonder dat eindgebruikers registratie implementeren

Verificatiegegevens moet aanwezig zijn voor het implementeren van Azure Active Directory (Azure AD) selfservice voor wachtwoordherstel (SSPR). Sommige organisaties hebben de gebruikers hun verificatiegegevens zelf invoeren. Maar in veel organisaties wilt synchroniseren met gegevens die al in Active Directory. De gesynchroniseerde gegevens beschikbaar worden gesteld aan Azure AD en SSPR zonder dat tussenkomst van de gebruiker is vereist als u:
   * De gegevens in uw on-premises directory goed opmaken.
   * Configureer [Azure AD Connect met behulp van de snelle instellingen](./connect/active-directory-aadconnect-get-started-express.md).

Werkt alleen goed als telefoonnummers moet de indeling *+ CountryCode PhoneNumber*, bijvoorbeeld: + 1 4255551234.

> [!NOTE]
> Er moet een spatie staan tussen de landcode en het telefoonnummer.
>
> Wachtwoordherstel biedt geen ondersteuning voor toestelnummers. Zelfs in de indeling van de 4255551234 + 1 X-12345 extensies verwijderd voordat de oproep wordt gedaan.

## <a name="fields-populated"></a>Velden ingevuld

Als u de standaardinstellingen in Azure AD Connect gebruikt, worden de volgende toewijzingen worden gemaakt:

| On-premises Active Directory | Azure AD | Azure AD authentication contactgegevens |
| --- | --- | --- |
| telephoneNumber | Telefoon (werk) | Alternatief telefoonnummer |
| mobiele | Mobiel telefoonnummer | Telefoon |


## <a name="security-questions-and-answers"></a>Beveiligingsvragen en antwoorden

De beveiligingsvragen en antwoorden worden veilig opgeslagen in uw Azure AD-tenant en zijn alleen toegankelijk voor gebruikers via de [SSPR-registratieportal](https://aka.ms/ssprsetup). Beheerders kunnen zien of aanpassen van de inhoud van een andere gebruikers vragen en antwoorden.

### <a name="what-happens-when-a-user-registers"></a>Wat gebeurt er wanneer een gebruiker registreert

Wanneer een gebruiker registreert, stelt de registratiepagina van de volgende velden:

* **Telefoon voor authenticatie**
* **Verificatie-e-mailadres**
* **Beveiligingsvragen en antwoorden**

Als u hebt een waarde voor opgegeven **mobiele telefoon** of **alternatief e-mailadres**, kunnen gebruikers direct gebruiken deze waarden om hun wachtwoord opnieuw instellen zelfs als ze nog niet hebt geregistreerd voor de service. Daarnaast kunt u gebruikers deze waarden wanneer ze zich voor de eerste keer registreren, en ze kunnen ze als ze willen aanpassen. Nadat ze zijn geregistreerd, wordt deze waarden wordt bewaard in de **telefoon voor authenticatie** en **verificatie e** velden, respectievelijk.

## <a name="set-and-read-the-authentication-data-through-powershell"></a>Ingesteld en de verificatiegegevens via PowerShell lezen

De volgende velden kunnen worden ingesteld via PowerShell:

* **Alternatieve e-mailadres**
* **Mobiele telefoon**
* **Telefoon (werk)**: kan alleen worden ingesteld als u niet met een on-premises directory synchroniseert

### <a name="use-powershell-version-1"></a>Gebruik PowerShell versie 1

Om te beginnen, moet u [downloaden en installeren van de Azure AD PowerShell-module](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Nadat u geïnstalleerd hebt, kunt u de stappen volgen voor het configureren van elk veld.

#### <a name="set-the-authentication-data-with-powershell-version-1"></a>De verificatiegegevens met PowerShell versie 1 instellen

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-1"></a>Lezen van de verificatiegegevens met PowerShell versie 1

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="read-the-authentication-phone-and-authentication-email-options"></a>Lezen van de telefoon voor authenticatie en E-mail van de verificatie-opties

Lezen van de **telefoon voor authenticatie** en **verificatie e** wanneer u PowerShell versie 1, gebruik de volgende opdrachten:

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="use-powershell-version-2"></a>Gebruik PowerShell versie 2

Om te beginnen, moet u [downloaden en installeren van de Azure AD PowerShell-module voor versie 2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md). Nadat u geïnstalleerd hebt, kunt u de stappen volgen voor het configureren van elk veld.

Als u wilt snel installeren van recente versies die ondersteuning bieden voor installatie-Module van PowerShell, voer de volgende opdrachten. (De eerste regel controleert of de module al is geïnstalleerd.)

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-the-authentication-data-with-powershell-version-2"></a>Stel de verificatiegegevens met PowerShell versie 2

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-2"></a>Lezen van de verificatiegegevens met PowerShell versie 2

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Volgende stappen

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md)
* [Hebt u een vraag over licenties?](active-directory-passwords-licensing.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](active-directory-passwords-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](active-directory-passwords-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](active-directory-passwords-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)
