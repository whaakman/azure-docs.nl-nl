---
title: Hoe-configureren van Azure Active Directory-beleid voor voorwaardelijke toegang voor toegangspogingen van niet-vertrouwde netwerken | Microsoft Docs
description: Informatie over het beleid voor voorwaardelijke toegang in Azure Active Directory (Azure AD) te configureren voor toegangspogingen van niet-vertrouwde netwerken.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 952a3a3952a96c7125e0b0dbe770b72c17a57101
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232399"
---
# <a name="how-to-configure-conditional-access-policies-for-access-attempts-from-untrusted-networks"></a>Procedure: beleid voor voorwaardelijke toegang voor van niet-vertrouwde netwerken toegangspogingen configureren   

In een wereld mobiel eerste, cloud eerste kunnen Azure Active Directory (Azure AD) eenmalige aanmelding aan apparaten, apps en services vanaf elke locatie. Hierdoor kan uw gebruikers hebben toegang tot uw cloud-apps niet alleen vanaf het netwerk van uw organisatie, maar ook vanuit een niet-vertrouwde internetlocatie. Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), kunt u bepalen hoe gemachtigde gebruikers hebben toegang tot uw cloud-apps. Een van de algemene vereisten in deze context is gestart vanaf een niet-vertrouwde netwerken toegangspogingen beheren. In dit artikel biedt u informatie die u nodig hebt voor het configureren van beleid voor voorwaardelijke toegang die verantwoordelijk is voor deze vereiste. 

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met bent: 

- De basisconcepten van voorwaardelijke toegang van Azure AD 
- Beleid voor voorwaardelijke toegang configureren in de Azure-portal

Zie:

- [Wat is er voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md) : voor een overzicht van voorwaardelijke toegang 

- [Snelstartgids: MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-app-based-mfa.md) - ophalen enige ervaring met het beleid voor voorwaardelijke toegang configureren. 


## <a name="scenario-description"></a>Scenariobeschrijving

Als u wilt de hoofdsleutel van de evenwicht tussen beveiliging en productiviteit, mogelijk voldoende zijn om te vereisen dat alleen de gebruiker kunnen worden geverifieerd met een wachtwoord. Wanneer toegang wordt geprobeerd vanaf de locatie van een niet-vertrouwd netwerk, is er echter een grotere kans verkleind dat aanmeldingen niet zijn uitgevoerd door legitieme gebruikers. Om dit probleem op te lossen, kunt u toegangspogingen van niet-vertrouwde netwerken te blokkeren. Ook kunt u multi-factor authentication (MFA) meer zekerheid dat door de legitieme eigenaar van het account is een poging gedaan te krijgen, ook verplichten. 

U kunt deze vereiste met een enkele beleidsregel die toegang verleent oplossen met voorwaardelijke toegang van Azure AD: 

- Aan de geselecteerde cloud-apps

- Voor de geselecteerde gebruikers en groepen  

- Meervoudige verificatie vereisen 

- Wanneer toegang wordt geprobeerd uit: 

    - Een locatie die wordt niet vertrouwd


## <a name="considerations"></a>Overwegingen

De uitdaging van dit scenario is het vertalen *wanneer toegang wordt geprobeerd vanaf een locatie die wordt niet vertrouwd* in een voorwaarde voor voorwaardelijke toegang. In een beleid voor voorwaardelijke toegang configureert u de [locaties voorwaarde](active-directory-conditional-access-locations.md) soort scenario's die zijn gerelateerd aan de netwerklocaties. De locaties voorwaarde kunt u selecteren [locaties met de naam](active-directory-conditional-access-locations.md#named-locations), die staan voor logische groeperingen van IP-adresbereiken, landen en regio's.  

Normaal gesproken uw organisatie eigenaar is van een of meer adresbereiken, bijvoorbeeld 199.30.16.0 - 199.30.16.24.
U kunt een benoemde locatie door configureren:

- Dit bereik (199.30.16.0/24) opgeven 

- Een beschrijvende naam zoals toewijzen **bedrijfsnetwerk** 


In plaats van probeert te definiëren wat alle locaties zijn die niet vertrouwd zijn, kunt u het volgende doen:

- Opnemen 

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-untrusted-networks/02.png)

- Alle vertrouwde locaties uitsluiten 

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-untrusted-networks/01.png)



## <a name="implementation"></a>Implementatie

Met de methode die in dit artikel wordt beschreven, kunt u nu een beleid voor voorwaardelijke toegang voor niet-vertrouwde locaties configureren. Test uw beleid altijd voordat dit uit te rollen in productie om ervoor te zorgen dat deze werkt zoals verwacht. In het ideale geval moet u uw eerste tests in een testtenant indien mogelijk doen. Zie voor meer informatie [hoe moet u een nieuw beleid implementeren](active-directory-conditional-access-best-practices.md#how-should-you-deploy-a-new-policy). 



## <a name="next-steps"></a>Volgende stappen

Als u meer informatie over voorwaardelijke toegang wilt, raadpleegt u [wat is er voorwaardelijke toegang in Azure Active Directory?](active-directory-conditional-access-azure-portal.md)