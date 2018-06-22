---
title: Wat is de beveiliging van een basislijn van voorwaardelijke toegang van Azure Active Directory? -preview | Microsoft Docs
description: Meer informatie over hoe basislijn beveiliging zorgt ervoor dat er ten minste de basisbeveiliging ingeschakeld in uw Azure Active Directory-omgeving.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/21/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 86b57a82573760ac73975e851b2bb4caf769845b
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36308557"
---
# <a name="what-is-baseline-protection---preview"></a>Wat is basislijn beveiliging? -preview  

In het afgelopen jaar zijn aanvallen identiteit met 300% verhoogd. Om te voorkomen dat uw omgeving de steeds groter wordende aanvallen, introduceert Azure Active Directory (Azure AD) een nieuwe functie voor basislijn beveiliging genoemd. Beveiliging van de basislijn is een reeks vooraf gedefinieerde [beleidsregels voor voorwaardelijke toegang](active-directory-conditional-access-azure-portal.md). Het doel van dit beleid is om ervoor te zorgen dat er ten minste het niveau van de basislijn van de beveiliging is ingeschakeld in alle edities van Azure AD. 

In dit artikel biedt een overzicht van basislijn-beveiliging in Azure Active Directory.


 
## <a name="require-mfa-for-admins"></a>MFA vereisen voor beheerders

Gebruikers met toegang tot beschermde accounts hebben onbeperkte toegang tot uw omgeving. Als gevolg van de stroom die deze accounts hebt, moet u ze behandelen speciale zorgvuldig. Een veelgebruikte methode voor het verbeteren van de beveiliging van bevoorrechte accounts is zodat een sterkere vorm van verificatie van account is vereist wanneer ze worden gebruikt om aan te melden. In Azure Active Directory krijgt u een sterkere accountverificatie doordat multi-factor authentication (MFA).  

**MFA vereisen voor beheerders** is een baseline-beleid dat MFA is vereist voor de functies van de volgende map: 

- Globale beheerder  

- SharePoint-beheerder  

- Exchange-beheerder  

- Voorwaardelijke toegang beheerder  

- Beveiligingsbeheerder  


![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/01.png)

Deze basislijn-beleid biedt u de optie om gebruikers en groepen te sluiten. U mogelijk wilt uitsluiten van een *[EMS-access-Administrator-account](active-directory-admin-manage-emergency-access-accounts.md)* zodat u niet de tenant zijn vergrendeld.


## <a name="enable-a-baseline-policy"></a>Een Basisbeleid inschakelen 

Basislijn beleidsregels zijn Preview-versie, zijn ze standaard niet geactiveerd. U moet een beleid handmatig inschakelen als u wilt activeren. Zodra deze functie is algemeen beschikbaar is bereikt, worden de beleidsregels zijn standaard ingeschakeld. De wijziging geplande gedrag is de reden waarom u bovendien activeren en deactiveren van een derde optie hebt om de status van een beleid ingesteld: **automatisch inschakelen in de toekomst beleid**. Als u deze optie selecteert, kunt u Microsoft bepalen wanneer een beleid activeren.      


**Een Basisbeleid inschakelen:**  

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder, beveiligingsbeheerder of beheerder voorwaardelijke toegang.

2. In de **Azure-portal**, klik op de navigatiebalk links, **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/02.png)

3. Op de **Azure Active Directory** pagina in de **beheren** sectie, klikt u op **voorwaardelijke toegang**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-baseline-protection/03.png)

4. Klik in de lijst met beleidsregels op een beleid dat met begint **Basisbeleid:**. 

5. Als het beleid, klikt u op **beleid direct gebruiken**.

6. Klik op **Opslaan**. 
 


  
 

## <a name="what-you-should-know"></a>Wat u moet weten 

Het beheren van beleidsregels voor voorwaardelijke toegang aangepaste beschikt over een Azure AD Premium-licentie, zijn basislijn beleidsregels beschikbaar in alle edities van Azure AD.     

De directory-functies die zijn opgenomen in het Basisbeleid zijn het meest bevoorrechte rollen voor Azure AD. 

Als u accounts die worden gebruikt in uw scripts uitgebreide, vervangt u deze met [beheerde Service identiteit (MSI)](./managed-service-identity/overview.md) of [service-principals met certificaten](../azure-resource-manager/resource-group-authenticate-service-principal.md). Als tijdelijke oplossing kunt u specifieke gebruikersaccounts uitsluiten van het Basisbeleid. 

Basislijn-beleid van toepassing op verouderde verificatie stroomt zoals POP, IMAP, oudere Office desktop client. 




## <a name="next-steps"></a>Volgende stappen

Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 
