---
title: Laat een organisatie als gastgebruiker - Azure Active Directory | Microsoft Docs
description: Laat zien hoe een Azure AD B2B-gastgebruiker laat een organisatie met behulp van het toegangsvenster.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0f75f91c037a2f05c999d388ce7bb16ad2d0c9cd
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58005663"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Een organisatie als gastgebruiker verlaten

Een gastgebruiker van Azure Active Directory (Azure AD) B2B kunt bepalen voor een organisatie op elk gewenst moment verlaten als ze niet meer nodig hebt voor apps van die organisatie gebruiken of een koppeling onderhouden. Een gebruiker kan een organisatie verlaten op hun eigen, zonder dat een beheerder vragen.

## <a name="leave-an-organization"></a>Een organisatie verlaten

Volg deze stappen om een organisatie af.

1. Ga naar de pagina van uw profiel Toegangsvenster op een van de volgende manieren:
   
   - In de [Azure-portal](https://portal.azure.com), klikt u op uw naam in de rechterbovenhoek en selecteer **account weergeven**.
   - Open uw [Toegangsvenster](https://myapps.microsoft.com), klikt u op uw naam in de rechterbovenhoek, volgende om door te **organisaties**, selecteer het Instellingenpictogram (tandwiel).
 
   ![Schermopname van gebruikersinstellingen in Toegangsvenster](media/leave-the-organization/UserSettings.png) 

   > [!NOTE]
   > Als u nog niet bent aangemeld bij de organisatie die u wilt verlaten, onder **organisaties**, klikt u op de **aanmelden bij de organisatie verlaten** koppelen naast de naam van de organisatie. Nadat u bent aangemeld, klikt u op uw naam opnieuw in de rechterbovenhoek en naast **organisaties**, selecteer het Instellingenpictogram (tandwiel).

3. Onder **organisaties**, de organisatie die u wilt verlaten, en selecteer zoeken **organisatie verlaten**.

   ![Schermopname die laat zien laat organisatie optie in de gebruikersinterface](media/leave-the-organization/LeaveOrg.png)

4. Wanneer u wordt gevraagd om te bevestigen, selecteer **laat**. 

## <a name="account-removal"></a>Account is verwijderd

Wanneer een gebruiker een organisatie verlaat, het gebruikersaccount dat is 'soft verwijderd' in de map. Standaard wordt het gebruikersobject verplaatst naar de **verwijderde gebruikers** gebied in Azure AD, maar is niet permanent verwijderd voor 30 dagen. Deze voorlopig verwijderen kan de beheerder om te herstellen van het gebruikersaccount (met inbegrip van groepen en machtigingen), als de gebruiker een aanvraag voor het herstellen van het account binnen de periode van 30 dagen.

Indien gewenst, kunt een tenantbeheerder permanent verwijderen van het account op elk gewenst moment tijdens de periode van 30 dagen. Om dit te doen:

1. In de [Azure-portal](https://portal.azure.com), selecteer **Azure Active Directory**.
2. Onder **Beheren**, selecteer **Gebruikers**.
3. Selecteer **verwijderde gebruikers**.
4. Schakel het selectievakje in naast een verwijderde gebruiker en selecteer vervolgens **definitief verwijderen**.

Als u een gebruiker permanent verwijdert, kan deze actie is onherroepelijk.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure AD B2B [wat is Azure AD B2B-samenwerking?](what-is-b2b.md)



