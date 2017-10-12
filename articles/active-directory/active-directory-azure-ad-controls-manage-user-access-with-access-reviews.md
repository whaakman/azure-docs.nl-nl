---
title: Gebruikerstoegang beheren met Azure AD-toegangsbeoordelingen | Microsoft Docs
description: Informatie over hoe u Azure Active Directory-toegangsbeoordelingen gebruikt om gebruikerstoegang te beheren als lidmaatschap van een groep of toewijzing aan een toepassing
services: active-directory
documentationcenter: 
author: markwahl-msft
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: 0459eb5cc71939202c8491f6b2714e28bd8e202d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-user-access-with-azure-ad-access-reviews"></a>Gebruikerstoegang beheren met Azure AD-toegangsbeoordelingen

Met Azure Active Directory kunt u er eenvoudig voor zorgen dat gebruikers passende toegang hebben door de gebruikers zelf of een beslisser te vragen deel te nemen aan een toegangsbeoordeling en de toegang van de gebruiker opnieuw te bevestigen (of hiermee 'akkoord te gaan').  De beoordelaars kunnen de noodzaak van een gebruiker voor onafgebroken toegang becommentariëren op basis van suggesties van Azure AD. Wanneer een toegangsbeoordeling is voltooid, kunt u wijzigingen aanbrengen en de toegang verwijderen voor gebruikers die deze niet meer nodig hebben.

> [!NOTE]
> Als u alleen de toegang van gastgebruikers wilt beoordelen, en niet van alle typen gebruikers, raadpleeg dan [Managing guest user access with access reviews](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md) (De toegang van gastgebruikers beheren met toegangsbeoordelingen).  En als u het lidmaatschap van een gebruiker in een beheerderrol, zoals Globale beheerder, wilt beoordelen, raadpleeg dan [How to start an access review in Azure AD PIM](active-directory-privileged-identity-management-how-to-start-security-review.md) (Een toegangsbeoordeling starten in Azure AD PIM). 
>
>

## <a name="prerequisites"></a>Vereisten 

Toegangsbeoordelingen zijn beschikbaar in de Premium P2-editie van Azure Active Directory, opgenomen in EMS E5. Zie [Azure Active Directory-edities](active-directory-editions.md) voor meer informatie.  Elke gebruiker die deze functie gebruikt - bijvoorbeeld voor het maken van een beoordeling, het controleren van toegang of het toepassen van een beoordeling - heeft een licentie nodig.


## <a name="creating-and-performing-an-access-review"></a>Een toegangsbeoordeling maken en uitvoeren

Een toegangsbeoordeling kan een of meer gebruikers als beoordelaar hebben.  

1. Selecteer in Azure Active Directory een groep die een of meer leden heeft, of een toepassing die is verbonden met Azure Active Directory en waaraan een of meer gebruikers zijn toegewezen. 
2. Bepaal of elke gebruikersbeoordeling een eigen toegang moet hebben, of dat een of meer gebruikers de toegang van iedereen kunnen beoordelen.
3. Schakel toegangsbeoordelingen in zodat deze op de toegangsdeelvensters van de beoordelaar worden weergegeven.  Ga als globale beheerder naar de [pagina Toegangsbeoordelingen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/). 
4. Start de toegangsbeoordeling. Zie [How to create an access review](active-directory-azure-ad-controls-create-access-review.md) (Een toegangsbeoordeling maken) voor meer informatie.
5. Vraag de beoordelaars feedback te geven. Standaard ontvangen ze allemaal een e-mail van Azure AD met een koppeling naar het toegangsdeelvenster waar ze [hun toegangsbeoordeling kunnen uitvoeren](active-directory-azure-ad-controls-perform-access-review.md).
6. Als de beoordelaars geen feedback hebben gegeven, kunt u Azure AD vragen hen een herinnering te sturen.  Standaard ontvangen de beoordelaars die nog niet hebben gereageerd halverwege de einddatum automatisch een herinnering van Azure AD.
7. Wanneer alle beoordelaars feedback hebben gegeven, kunt u de toegangsbeoordeling stoppen en de wijzigingen toepassen. Zie [How to complete an access review](active-directory-azure-ad-controls-complete-access-review.md) (Een toegangsbeoordeling voltooien) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

- [Een toegangsbeoordeling maken voor leden van een groep of toegang tot een toepassing](active-directory-azure-ad-controls-create-access-review.md)




