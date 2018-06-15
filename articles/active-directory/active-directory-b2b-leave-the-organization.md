---
title: Laat een organisatie als gastgebruiker - Azure Active Directory | Microsoft Docs
description: Toont hoe een Azure AD B2B-gastgebruiker laat een organisatie met behulp van het toegangsvenster.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 942439da3e116415c77a28950df69d44744b2dd8
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077541"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Een organisatie als gastgebruiker verlaten

Een Azure Active Directory (Azure AD) B2B gastgebruiker kunt bepalen te verlaten van een organisatie op elk gewenst moment als ze niet meer nodig hebt voor het gebruik van apps van die organisatie of gekoppeld. Een gebruiker kunt een organisatie laten op hun eigen, zonder dat een beheerder.

## <a name="leave-an-organization"></a>Een organisatie verlaten

Te verlaten van een organisatie, als een gebruiker aangemeld bij de [Toegangspaneel](https://myapps.microsoft.com), het volgende doen:

1. Als u nog niet bent aangemeld bij de organisatie die u wilt verlaten, selecteer uw naam in de rechterbovenhoek en klik op de organisatie die u wilt verlaten.
2. Selecteer uw naam in de rechterbovenhoek.
3. Naast **organisaties**, selecteer het Instellingenpictogram (tandwielpictogram).
 
   ![Schermopname van gebruikersinstellingen in het deelvenster toegang](media/active-directory-b2b-leave-the-organization/UserSettings.png) 

3. Onder **organisaties**, vinden de organisatie die u wilt laten en selecteer **organisatie verlaten**.

   ![Schermopname van laat organisatie optie in de gebruikersinterface](media/active-directory-b2b-leave-the-organization/LeaveOrg.png)

4. Wanneer u wordt gevraagd om te bevestigen, selecteer **laat**. 

## <a name="account-removal"></a>Account is verwijderd

Wanneer een gebruiker een organisatie verlaat, het gebruikersaccount 'soft verwijderd' in de map. Standaard wordt het gebruikersobject verplaatst naar de **gebruikers verwijderd** gebied in Azure AD, maar is niet permanent verwijderd voor 30 dagen. Deze zachte verwijdering kan de beheerder om te herstellen van het gebruikersaccount (met inbegrip van groepen en machtigingen), als de gebruiker een aanvraag voor het herstellen van het account binnen de periode van 30 dagen.

Indien gewenst, kan een tenantbeheerder definitief verwijderen van het account op elk gewenst moment tijdens de periode van 30 dagen. Om dit te doen:

1. In de [Azure-portal](https://portal.azure.com), selecteer **Azure Active Directory**.
2. Onder **beheren**, selecteer **gebruikers**.
3. Selecteer **gebruikers verwijderd**.
4. Schakel het selectievakje in naast een verwijderde gebruiker en selecteer vervolgens **definitief verwijderen**.

Als u een gebruiker permanent verwijdert, kan deze actie is onherroepelijk.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure AD B2B [wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)



