---
title: Gebruikerstoegang beheren met Azure AD-toegangsbeoordelingen | Microsoft Docs
description: Informatie over hoe u Azure Active Directory-toegangsbeoordelingen gebruikt om gebruikerstoegang te beheren als lidmaatschap van een groep of toewijzing aan een toepassing
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: 7b4d3baac791012e18e65c9e2e42272410e73fc1
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621654"
---
# <a name="manage-user-access-with-azure-ad-access-reviews"></a>Gebruikerstoegang beheren met Azure AD-toegangsbeoordelingen

Met Azure Active Directory (Azure AD) kunt u eenvoudig zorgen dat gebruikers de juiste toegang hebben. U kunt gebruikers zelf of een besluitvormer vragen deel te nemen aan een toegangsbeoordeling om de toegang van gebruikers te bevestigen. De beoordelaars kunnen de noodzaak van een gebruiker voor onafgebroken toegang becommentariëren op basis van suggesties van Azure AD. Wanneer een toegangsbeoordeling is voltooid, kunt u wijzigingen aanbrengen en de toegang verwijderen voor gebruikers die deze niet meer nodig hebben.

> [!NOTE]
> Als u alleen de toegang van gastgebruikers wilt beoordelen, en niet van alle typen gebruikers, raadpleeg dan [Manage guest user access with access reviews](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md) (De toegang van gastgebruikers beheren met toegangsbeoordelingen). Als u het lidmaatschap van een gebruiker in een beheerderrol, zoals Globale beheerder, wilt beoordelen, raadpleeg dan [Start an access review in Azure AD Privileged Identity Management](privileged-identity-management/pim-how-to-start-security-review.md) (Een toegangsbeoordeling starten in Azure AD Privileged Identity Management). 
>
>

## <a name="prerequisites"></a>Vereisten 


Toegangsbeoordelingen zijn beschikbaar met de Premium P2-editie van Azure AD, die deel uitmaakt van Microsoft Enterprise Mobility + Security, E5. Zie [Azure Active Directory-edities](active-directory-editions.md) voor meer informatie. Elke gebruiker die deze functie gebruikt (waaronder het maken van een beoordeling, het invullen van een beoordeling of het bevestigen van de toegang), heeft een licentie nodig. 

## <a name="create-and-perform-an-access-review"></a>Een toegangscontrole uitvoeren

Een toegangsbeoordeling kan een of meer gebruikers als beoordelaar hebben.  

1. Selecteer een groep in Azure AD met een of meer leden. Of selecteer een toepassing die is verbonden met Azure AD en waaraan een of meer gebruikers zijn toegewezen. 

2. Bepaal of elke gebruikersbeoordeling een eigen toegang moet hebben, of dat een of meer gebruikers de toegang van iedereen kunnen beoordelen.

3. Schakel toegangsbeoordelingen in zodat deze op de toegangsdeelvensters van de beoordelaars worden weergegeven. Ga als globale beheerder of gebruikersaccountbeheerder naar de [pagina Toegangsbeoordelingen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

4. Start de toegangsbeoordeling. Raadpleeg voor meer informatie [Create an access review](active-directory-azure-ad-controls-create-access-review.md) (Een toegangscontrole maken).

5. Vraag de beoordelaars feedback te geven. Standaard ontvangen ze allemaal een e-mail van Azure AD met een koppeling naar het toegangsdeelvenster waar ze [hun toegangsbeoordeling kunnen uitvoeren](active-directory-azure-ad-controls-perform-access-review.md).

6. Als de beoordelaars geen feedback hebben gegeven, kunt u Azure AD vragen hen een herinnering te sturen. Standaard ontvangen de beoordelaars die nog niet hebben gereageerd halverwege de einddatum automatisch een herinnering van Azure AD.

7. Wanneer alle beoordelaars feedback hebben gegeven, kunt u de toegangsbeoordeling stoppen en de wijzigingen toepassen. Raadpleeg voor meer informatie [Complete an access review](active-directory-azure-ad-controls-complete-access-review.md) (Een toegangscontrole voltooien).


## <a name="next-steps"></a>Volgende stappen

[Een toegangsbeoordeling maken voor leden van een groep of toegang tot een toepassing](active-directory-azure-ad-controls-create-access-review.md)




