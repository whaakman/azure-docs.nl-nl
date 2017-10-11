---
title: 'Beheerders beheren van gebruikers en apparaten: Azure MFA | Microsoft Docs'
description: Dit wordt beschreven hoe u gebruikersinstellingen, zoals het forceren van de gebruikers het proces bewijs-up opnieuw te doen.
documentationcenter: 
services: multi-factor-authentication
author: kgremban
manager: femila
ms.assetid: aac3b922-7cc1-428c-9044-273579aa7b5a
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: e9b8504d4a59cf0fae69a4e975d6f834028066d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
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
   ![Contactmethoden opgeven](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
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
   ![App-wachtwoorden verwijderen](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
9. Klik op **Opslaan**.
10. Klik op **sluiten**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>MFA op alle onthouden apparaten voor een gebruiker herstellen
Een van de configureerbare functies van Azure multi-factor Authentication geeft uw gebruikers de optie voor het markeren van apparaten als vertrouwd. Zie voor meer informatie [Azure multi-factor Authentication configureren instellingen](multi-factor-authentication-whats-next.md#remember-multi-factor-authentication-for-devices-that-users-trust).

Gebruikers kunnen de verificatie in twee stappen voor een configureerbare aantal dagen op hun apparaten reguliere afmelden. Als een account wordt aangetast of een vertrouwd apparaat verloren gaat, moet u mogelijk de status van de vertrouwde verwijderen en opnieuw verificatie in twee stappen vereist.

De **terugzetten meervoudige verificatie op alle onthouden apparaten** instelling houdt in dat de gebruiker gevraagd om uit te voeren verificatie in twee stappen de volgende keer dat ze zich aanmelden, ongeacht of ze hun apparaat als markeren hebt gekozen vertrouwd. 

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>MFA op alle onderbroken apparaten voor een gebruiker herstellen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**. De multi-factor authentication-pagina wordt geopend. 
6. Schakel het selectievakje naast de gebruiker of gebruikers die u wilt beheren. Een lijst met snelle stap opties worden weergegeven aan de rechterkant. 
7. Selecteer **gebruikersinstellingen beheren**.
8. Schakel het selectievakje voor **terugzetten meervoudige verificatie op alle onthouden apparaten**
   ![app-wachtwoorden verwijderen](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Klik op **Opslaan**.
10. Klik op **sluiten**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [instellingen van Azure multi-factor Authentication configureren](multi-factor-authentication-whats-next.md)

- Als uw gebruikers hulp nodig hebt, wijst u ze naar de [gebruikershandleiding voor de verificatie in twee stappen](./end-user/multi-factor-authentication-end-user.md)
