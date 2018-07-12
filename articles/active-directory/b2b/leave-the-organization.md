---
title: Laat een organisatie als gastgebruiker - Azure Active Directory | Microsoft Docs
description: Laat zien hoe een Azure AD B2B-gastgebruiker laat een organisatie met behulp van het toegangsvenster.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 17b34b173a10a355817fee0f5928b7fb478125e3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38590420"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Een organisatie als gastgebruiker verlaten

Een gastgebruiker van Azure Active Directory (Azure AD) B2B kunt bepalen voor een organisatie op elk gewenst moment verlaten als ze niet meer nodig hebt voor apps van die organisatie gebruiken of een koppeling onderhouden. Een gebruiker kan een organisatie verlaten op hun eigen, zonder dat een beheerder vragen.

## <a name="leave-an-organization"></a>Een organisatie verlaten

Laat een organisatie, als een gebruiker aangemeld bij de [Toegangsvenster](https://myapps.microsoft.com), doet u het volgende:

1. Als u nog niet bent aangemeld bij de organisatie die u wilt laten staan, selecteert u uw naam in de rechterbovenhoek en klik op de organisatie die u wilt laten staan.
2. Selecteer uw naam in de rechterbovenhoek.
3. Naast **organisaties**, selecteer het Instellingenpictogram (tandwiel).
 
   ![Schermopname van gebruikersinstellingen in Toegangsvenster](media/leave-the-organization/UserSettings.png) 

3. Onder **organisaties**, de organisatie die u wilt verlaten, en selecteer zoeken **organisatie verlaten**.

   ![Schermopname die laat zien laat organisatie optie in de gebruikersinterface](media/leave-the-organization/LeaveOrg.png)

4. Wanneer u wordt gevraagd om te bevestigen, selecteer **laat**. 

## <a name="account-removal"></a>Account is verwijderd

Wanneer een gebruiker een organisatie verlaat, het gebruikersaccount dat is 'soft verwijderd' in de map. Standaard wordt het gebruikersobject verplaatst naar de **verwijderde gebruikers** gebied in Azure AD, maar is niet permanent verwijderd voor 30 dagen. Deze voorlopig verwijderen kan de beheerder om te herstellen van het gebruikersaccount (met inbegrip van groepen en machtigingen), als de gebruiker een aanvraag voor het herstellen van het account binnen de periode van 30 dagen.

Indien gewenst, kunt een tenantbeheerder permanent verwijderen van het account op elk gewenst moment tijdens de periode van 30 dagen. Om dit te doen:

1. In de [Azure-portal](https://portal.azure.com), selecteer **Azure Active Directory**.
2. Onder **beheren**, selecteer **gebruikers**.
3. Selecteer **verwijderde gebruikers**.
4. Schakel het selectievakje in naast een verwijderde gebruiker en selecteer vervolgens **definitief verwijderen**.

Als u een gebruiker permanent verwijdert, kan deze actie is onherroepelijk.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure AD B2B [wat is Azure AD B2B-samenwerking?](what-is-b2b.md)



