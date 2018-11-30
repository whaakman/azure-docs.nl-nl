---
title: Beheerders beheren van gebruikers en apparaten - Azure MFA | Microsoft Docs
description: Dit wordt beschreven hoe u gebruikersinstellingen, zoals het afdwingen van de gebruikers het proces proof-up opnieuw te doen.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: cf470ad14c27fd36cf60bb540e823b15fc58eded
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52496901"
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
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**. De multi-factor authentication-pagina wordt geopend. 
4. Schakel het selectievakje naast de gebruiker of gebruikers die u wilt beheren. Een lijst met snelle stap opties worden weergegeven aan de rechterkant. 
5. Selecteer **gebruikersinstellingen beheren**.
6. Schakel het selectievakje voor **vereisen dat geselecteerde gebruikers opnieuw contactmethoden opgeven**.
   ![Contactmethoden opgeven](./media/howto-mfa-userdevicesettings/reproofup.png)
7. Klik op **Opslaan**.
8. Klik op **sluiten**.

## <a name="delete-users-existing-app-passwords"></a>App-wachtwoorden bestaande gebruikers verwijderen
Deze instelling verwijdert alle app-wachtwoorden die een gebruiker heeft gemaakt. Niet-browsertoepassingen die gekoppeld aan deze app-wachtwoorden zijn niet meer werken totdat er een nieuw app-wachtwoord is gemaakt.

### <a name="how-to-delete-users-existing-app-passwords"></a>App-wachtwoorden bestaande gebruikers verwijderen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**. De multi-factor authentication-pagina wordt geopend. 
6. Schakel het selectievakje naast de gebruiker of gebruikers die u wilt beheren. Een lijst met snelle stap opties worden weergegeven aan de rechterkant. 
7. Selecteer **gebruikersinstellingen beheren**.
8. Schakel het selectievakje voor **verwijderen van alle bestaande app-wachtwoorden die worden gegenereerd door de geselecteerde gebruikers**.
   ![App-wachtwoorden verwijderen](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
9. Klik op **Opslaan**.
10. Klik op **sluiten**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>MFA op alle onthouden apparaten voor een gebruiker herstellen
Een van de configureerbare functies van Azure multi-factor Authentication biedt uw gebruikers de optie voor het markeren van apparaten als vertrouwd. Zie voor meer informatie, [Azure multi-factor Authentication configureren instellingen](howto-mfa-mfasettings.md#remember-multi-factor-authentication).

Gebruikers kunnen kiezen uit de verificatie in twee stappen voor een configureerbaar aantal dagen op hun reguliere apparaten. Als een account is aangetast of een vertrouwd apparaat verloren gegaan is, moet u mogelijk de status van de vertrouwde verwijderen en opnieuw verificatie in twee stappen vereist.

De **terugzetten multi-factor authentication op alle onthouden apparaten** betekent dat de gebruiker instellen uitgedaagd om het uitvoeren van de volgende keer dat ze zich aanmelden, ongeacht of ze te markeren van hun apparaat hebben gekozen voor verificatie vertrouwd. 

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>MFA op alle onderbroken apparaten voor een gebruiker herstellen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**. De multi-factor authentication-pagina wordt geopend. 
6. Schakel het selectievakje naast de gebruiker of gebruikers die u wilt beheren. Een lijst met snelle stap opties worden weergegeven aan de rechterkant. 
7. Selecteer **gebruikersinstellingen beheren**.
8. Schakel het selectievakje voor **terugzetten multi-factor authentication op alle onthouden apparaten**
   ![app-wachtwoorden verwijderen](./media/howto-mfa-userdevicesettings/rememberdevices.png)
9. Klik op **Opslaan**.
10. Klik op **sluiten**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [instellingen van de Azure multi-factor Authentication configureren](howto-mfa-mfasettings.md)

- Als uw gebruikers hulp nodig hebt, wijst u ze naar de [de gebruikershandleiding voor verificatie in twee stappen](../user-help/multi-factor-authentication-end-user.md)
