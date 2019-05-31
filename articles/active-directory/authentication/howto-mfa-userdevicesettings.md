---
title: Beheerders beheren van gebruikers en apparaten - Azure MFA - Azure Active Directory
description: Hoe kunnen beheerders gebruikersinstellingen, zoals het afdwingen van de gebruikers het proces proof-up opnieuw wijzigen.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 04d4848a00fd645bcf23342f27fe820ccf034a8b
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66298846"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Gebruikersinstellingen met Azure multi-factor Authentication in de cloud beheren

Als een beheerder, kunt u de volgende gebruikers- en -instellingen beheren:

* Vereisen dat gebruikers opnieuw contactmethoden opgeven
* App-wachtwoorden verwijderen
* MFA vereisen voor alle vertrouwde apparaten

## <a name="require-users-to-provide-contact-methods-again"></a>Vereisen dat gebruikers opnieuw contactmethoden opgeven

Deze instelling zorgt ervoor dat de gebruiker opnieuw het registratieproces te voltooien. Niet-browsertoepassingen blijven werken als de gebruiker heeft de app-wachtwoorden voor hen.  U kunt de app-wachtwoorden van gebruikers verwijderen doordat u tevens **verwijderen van alle bestaande app-wachtwoorden die worden gegenereerd door de geselecteerde gebruikers**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Hoe gebruikers moeten opnieuw contactmethoden opgeven

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers** > **alle gebruikers**.
3. Selecteer aan de rechterkant **multi-Factor Authentication** op de werkbalk. De multi-factor authentication-pagina wordt geopend.
4. Schakel het selectievakje naast de gebruiker of gebruikers die u wilt beheren. Een lijst met opties voor snelle stap wordt weergegeven aan de rechterkant.
5. Selecteer **gebruikersinstellingen beheren**.
6. Schakel het selectievakje voor **vereisen dat geselecteerde gebruikers opnieuw contactmethoden opgeven**.
   ![Vereisen dat gebruikers opnieuw contactmethoden opgeven](./media/howto-mfa-userdevicesettings/reproofup.png)
7. Klik op **Opslaan**.
8. Klik op **sluiten**.

Organisaties kunnen deze stappen voltooien met PowerShell het volgende als richtlijn om te wissen met de `StrongAuthenticationMethods` kenmerk:

```PowerShell
$Upn = "theuser@domain.com"
$noMfaConfig = @()
Set-MsolUser -UserPrincipalName $Upn -StrongAuthenticationMethods $noMfaConfig
```

## <a name="delete-users-existing-app-passwords"></a>App-wachtwoorden bestaande gebruikers verwijderen

Deze instelling verwijdert alle app-wachtwoorden die een gebruiker heeft gemaakt. Niet-browsertoepassingen die gekoppeld aan deze app-wachtwoorden zijn niet meer werken totdat er een nieuw app-wachtwoord is gemaakt.

### <a name="how-to-delete-users-existing-app-passwords"></a>App-wachtwoorden bestaande gebruikers verwijderen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers** > **alle gebruikers**.
3. Selecteer aan de rechterkant **multi-Factor Authentication** op de werkbalk. De multi-factor authentication-pagina wordt geopend.
4. Schakel het selectievakje naast de gebruiker of gebruikers die u wilt beheren. Een lijst met opties voor snelle stap wordt weergegeven aan de rechterkant.
5. Selecteer **gebruikersinstellingen beheren**.
6. Schakel het selectievakje voor **verwijderen van alle bestaande app-wachtwoorden die worden gegenereerd door de geselecteerde gebruikers**.
   ![Alle bestaande appwachtwoorden verwijderen](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
7. Klik op **Opslaan**.
8. Klik op **sluiten**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>MFA op alle onthouden apparaten voor een gebruiker herstellen

Een van de configureerbare functies van Azure multi-factor Authentication biedt uw gebruikers de optie voor het markeren van apparaten als vertrouwd. Zie voor meer informatie, [Azure multi-factor Authentication configureren instellingen](howto-mfa-mfasettings.md#remember-multi-factor-authentication).

Gebruikers kunnen kiezen uit de verificatie in twee stappen voor een configureerbaar aantal dagen op hun reguliere apparaten. Als een account is aangetast of een vertrouwd apparaat verloren gegaan is, moet u mogelijk de status van de vertrouwde verwijderen en opnieuw verificatie in twee stappen vereist.

Wanneer dit selectievakje inschakelt, **terugzetten multi-factor authentication op alle onthouden apparaten** gebruikers zijn vereist voor het uitvoeren van verificatie in twee stappen de volgende keer dat ze zich aanmelden, zelfs als ze hun apparaat als vertrouwd gemarkeerd.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>MFA op alle onderbroken apparaten voor een gebruiker herstellen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers** > **alle gebruikers**.
3. Selecteer aan de rechterkant **multi-Factor Authentication** op de werkbalk. De multi-factor authentication-pagina wordt geopend.
4. Schakel het selectievakje naast de gebruiker of gebruikers die u wilt beheren. Een lijst met opties voor snelle stap wordt weergegeven aan de rechterkant.
5. Selecteer **gebruikersinstellingen beheren**.
6. Schakel het selectievakje voor **terugzetten multi-factor authentication op alle onthouden apparaten**
   ![multi-factor Authentication-verificatie op alle onthouden apparaten herstellen](./media/howto-mfa-userdevicesettings/rememberdevices.png)
7. Klik op **Opslaan**.
8. Klik op **sluiten**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [instellingen van de Azure multi-factor Authentication configureren](howto-mfa-mfasettings.md)
- Als uw gebruikers hulp nodig hebt, wijst u ze naar de [de gebruikershandleiding voor verificatie in twee stappen](../user-help/multi-factor-authentication-end-user.md)
