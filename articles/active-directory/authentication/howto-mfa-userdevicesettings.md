---
title: 'Beheerders beheren van gebruikers en apparaten: Azure MFA | Microsoft Docs'
description: Dit wordt beschreven hoe u gebruikersinstellingen, zoals het forceren van de gebruikers het proces bewijs-up opnieuw te doen.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/23/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 363106421e75fa2e1b220d03a7d7cbed25447bcc
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098393"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Gebruikersinstellingen met Azure multi-factor Authentication in de cloud beheren

Als beheerder, kunt u de volgende instellingen voor de gebruiker en apparaat kunt beheren:

* Vereisen dat gebruikers opnieuw contactmethoden opgeven
* App-wachtwoorden verwijderen
* MFA is vereist op alle vertrouwde apparaten 

## <a name="require-users-to-provide-contact-methods-again"></a>Vereisen dat gebruikers opnieuw contactmethoden opgeven
Deze instelling zorgt ervoor dat de gebruiker opnieuw het registratieproces voltooid. Niet-browsertoepassingen blijven werken als de gebruiker heeft de app-wachtwoorden voor deze.  U kunt de app-wachtwoorden van gebruikers verwijderen doordat u tevens **verwijdert alle bestaande app-wachtwoorden die worden gegenereerd door de geselecteerde gebruikers**.

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
Deze instelling verwijdert alle app-wachtwoorden die een gebruiker heeft gemaakt. Niet-browsertoepassingen die gekoppeld aan deze app-wachtwoorden zijn uitgeschakeld totdat een nieuwe appwachtwoord is gemaakt.

### <a name="how-to-delete-users-existing-app-passwords"></a>Het verwijderen van gebruikers bestaande app-wachtwoorden
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**. De multi-factor authentication-pagina wordt geopend. 
6. Schakel het selectievakje naast de gebruiker of gebruikers die u wilt beheren. Een lijst met snelle stap opties worden weergegeven aan de rechterkant. 
7. Selecteer **gebruikersinstellingen beheren**.
8. Schakel het selectievakje voor **verwijdert alle bestaande app-wachtwoorden die worden gegenereerd door de geselecteerde gebruikers**.
   ![App-wachtwoorden verwijderen](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
9. Klik op **Opslaan**.
10. Klik op **sluiten**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>MFA op alle onthouden apparaten voor een gebruiker herstellen
Een van de configureerbare functies van Azure multi-factor Authentication geeft uw gebruikers de optie voor het markeren van apparaten als vertrouwd. Zie voor meer informatie [Azure multi-factor Authentication configureren instellingen](howto-mfa-mfasettings.md#remember-multi-factor-authentication-for-devices-that-users-trust).

Gebruikers kunnen de verificatie in twee stappen voor een configureerbare aantal dagen op hun apparaten reguliere afmelden. Als een account wordt aangetast of een vertrouwd apparaat verloren gaat, moet u mogelijk de status van de vertrouwde verwijderen en opnieuw verificatie in twee stappen vereist.

De **terugzetten meervoudige verificatie op alle onthouden apparaten** instelling houdt in dat de gebruiker gevraagd om uit te voeren verificatie in twee stappen de volgende keer dat ze zich aanmelden, ongeacht of ze hun apparaat als markeren hebt gekozen vertrouwd. 

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>MFA op alle onderbroken apparaten voor een gebruiker herstellen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**. De multi-factor authentication-pagina wordt geopend. 
6. Schakel het selectievakje naast de gebruiker of gebruikers die u wilt beheren. Een lijst met snelle stap opties worden weergegeven aan de rechterkant. 
7. Selecteer **gebruikersinstellingen beheren**.
8. Schakel het selectievakje voor **terugzetten meervoudige verificatie op alle onthouden apparaten**
   ![app-wachtwoorden verwijderen](./media/howto-mfa-userdevicesettings/rememberdevices.png)
9. Klik op **Opslaan**.
10. Klik op **sluiten**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [instellingen van Azure multi-factor Authentication configureren](howto-mfa-mfasettings.md)

- Als uw gebruikers hulp nodig hebt, wijst u ze naar de [gebruikershandleiding voor de verificatie in twee stappen](end-user/current/multi-factor-authentication-end-user.md)
