---
title: Wat is de beveiliging van een basislijn voor de voorwaardelijke toegang van Azure Active Directory? -preview | Microsoft Docs
description: Meer informatie over hoe basisbeveiliging zorgt ervoor dat u ten minste het niveau van de basislijn van de beveiliging is ingeschakeld in uw Azure Active Directory-omgeving hebt.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
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
ms.date: 07/02/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 70a0fc4236bc275cf9221f302e63f987a89af0d6
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39620345"
---
# <a name="what-is-baseline-protection-preview"></a>Wat is basislijn protection (preview)?  

In het afgelopen jaar hebben identiteitsaanvallen met 300% verhoogd. Ter bescherming van uw omgeving tegen de steeds groter wordende aanvallen, introduceert Azure Active Directory (Azure AD) een nieuwe functie voor basislijn beveiliging genoemd. Beveiliging van de basislijn is een set vooraf gedefinieerde [beleid voor voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md). Het doel van dit beleid is om ervoor te zorgen dat u ten minste het niveau van de basislijn van de beveiliging is ingeschakeld in alle edities van Azure AD. 

In dit artikel biedt een overzicht van de basislijn beveiliging in Azure Active Directory.


 
## <a name="require-mfa-for-admins"></a>MFA vereisen voor beheerders

Gebruikers met toegang tot beschermde accounts hebben onbeperkte toegang tot uw omgeving. Vanwege de stroom die deze accounts hebt, moet u ze behandelen met speciale aandacht. Een veelgebruikte methode voor het verbeteren van de beveiliging van bevoegde accounts is een sterkere vorm van verificatie-account vereist wanneer deze worden gebruikt om aan te melden. In Azure Active Directory krijgt u een sterkere accountverificatie doordat multi-factor authentication (MFA).  

**MFA vereisen voor beheerders** is een baseline-beleid dat MFA voor de volgende maprollen vereist: 

- Globale beheerder  

- SharePoint-beheerder  

- Exchange-beheerder  

- Voorwaardelijke-toegangsbeheerder  

- Beveiligingsbeheerder  


![Azure Active Directory](./media/baseline-protection/01.png)

Dit beleid biedt u de optie om gebruikers en groepen te sluiten. U mogelijk wilt uitsluiten van een *[EMS-access-Administrator-account](../users-groups-roles/directory-emergency-access.md)* om te controleren of u geen toegang tot de tenant zijn geblokkeerd.


## <a name="enable-a-baseline-policy"></a>Een beleid inschakelen 

Basislijn-beleidsregels zijn beschikbaar als preview, zijn ze standaard niet geactiveerd. U moet handmatig een beleid inschakelen als u wilt activeren. Zodra deze functie de algemene beschikbaarheid bereikt heeft, worden de beleidsregels zijn standaard ingeschakeld. De wijziging geplande gedrag is de reden waarom u bovendien activeren en deactiveren van een derde optie hebt om de status van een beleid ingesteld: **automatisch inschakelen van beleid in de toekomst**. Als u deze optie selecteert, kunt u Microsoft bepalen wanneer u een beleid activeren.      


**Een Basisbeleid inschakelen:**  

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als hoofdbeheerder, beveiligingsbeheerder of beheerder van voorwaardelijke toegang.

2. In de **Azure-portal**, klik op de navigatiebalk links **Azure Active Directory**.

    ![Azure Active Directory](./media/baseline-protection/02.png)

3. Op de **Azure Active Directory** pagina, in de **Security** sectie, klikt u op **voorwaardelijke toegang**.

    ![Voorwaardelijke toegang](./media/baseline-protection/05.png)

4. Klik in de lijst met beleidsregels op een beleid dat met begint **Basisbeleid:**. 

5. Als het beleid, klikt u op **beleid direct gebruiken**.

6. Klik op **Opslaan**. 
 
  
 

## <a name="what-you-should-know"></a>Wat u moet weten 

Tijdens het beheren van beleid voor aangepaste voorwaardelijke toegang vereist een Azure AD Premium-licentie, zijn basislijn beleidsregels beschikbaar in alle edities van Azure AD.     

De directory-rollen die zijn opgenomen in het Basisbeleid zijn de meeste bevoegdheden Azure AD-rollen. 

Als u accounts die worden gebruikt in uw scripts uitgebreide, vervangt u deze met [Managed Service Identity (MSI)](../managed-service-identity/overview.md) of [service-principals met certificaten](../../azure-resource-manager/resource-group-authenticate-service-principal.md). Als tijdelijke oplossing, kunt u specifieke gebruikersaccounts uitsluiten van het Basisbeleid. 

Basislijn-beleid van toepassing op verouderde verificatiestromen zoals POP, IMAP, oudere Office-bureaublad-client. 




## <a name="next-steps"></a>Volgende stappen

Als u weten hoe u een beleid voor voorwaardelijke toegang configureren wilt, Zie [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md).

Als u klaar om te configureren van beleid voor voorwaardelijke toegang voor uw omgeving bent, raadpleegt u de [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md). 
