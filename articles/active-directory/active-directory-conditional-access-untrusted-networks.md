---
title: Hoe - beleid voor voorwaardelijke toegang configureren van Azure Active Directory toegang tot pogingen van niet-vertrouwde netwerken | Microsoft Docs
description: Informatie over het configureren van beleid voor voorwaardelijke toegang in Azure Active Directory (Azure AD) aan voor toegang tot pogingen van niet-vertrouwde netwerken.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: protection
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: b37c9017d6c9a8b1d5f53141e28c170307206f3b
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225822"
---
# <a name="how-to-configure-conditional-access-policies-for-access-attempts-from-untrusted-networks"></a>Hoe: beleid voor voorwaardelijke toegang configureren voor toegang tot van niet-vertrouwde netwerken pogingen   

In een wereld mobiliteit, cloud-first kunt Azure Active Directory (Azure AD) single sign-on bij apparaten, apps en services vanaf elke locatie. Als gevolg van dit, kunnen uw gebruikers toegang tot uw cloud-apps niet alleen vanuit het netwerk van uw organisatie, maar ook vanaf elke internetlocatie met niet-vertrouwde. Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), u kunt bepalen hoe gemachtigde gebruikers hebben toegang tot uw cloudapps. Er is een algemene vereiste in deze context voor het beheren van toegang tot pogingen afkomstig zijn van niet-vertrouwde netwerken. In dit artikel biedt u de informatie die u wilt configureren van beleid voor voorwaardelijke toegang die verantwoordelijk is voor deze vereiste. 

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met bent: 

- De basisconcepten van Azure AD voor voorwaardelijke toegang 
- Configureren van beleid voor voorwaardelijke toegang in Azure portal

Zie:

- [Wat is voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md) : voor een overzicht van voorwaardelijke toegang 

- [Snelstartgids: MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-app-based-mfa.md) : als u wilt ophalen van enige ervaring met het configureren van beleid voor voorwaardelijke toegang. 


## <a name="scenario-description"></a>Scenariobeschrijving

Als u wilt de balans tussen beveiliging en productiviteit-master, is het mogelijk voldoende is voor u alleen uw gebruiker moeten worden geverifieerd met behulp van een wachtwoord vereist. Bij een poging tot toegang vanaf de locatie van een niet-vertrouwd netwerk is aangevraagd, is er echter een betere risico dat aanmeldingen niet worden uitgevoerd door legitieme gebruikers. Om dit probleem op te lossen, kunt u toegangspogingen om vanaf niet-vertrouwde netwerken blokkeren. U kunt ook kunt u multi-factor authentication (MFA) om te krijgen meer zekerheid dat er is een poging gedaan door de rechtmatige eigenaar van het account ook vereisen. 

Met Azure AD voor voorwaardelijke toegang, kunt u deze vereiste met één beleid waarmee toegang wordt verleend adres: 

- Aan de geselecteerde cloud-apps

- Voor de geselecteerde gebruikers en groepen  

- Meervoudige verificatie vereisen 

- Bij een poging tot toegang bestaat uit: 

    - Een locatie die wordt niet vertrouwd


## <a name="considerations"></a>Overwegingen

De uitdaging van dit scenario is om te vertalen *bij een poging tot toegang wordt uitgevoerd vanaf een locatie die wordt niet vertrouwd* in een voorwaarde voor voorwaardelijke toegang. In een beleid voor voorwaardelijke toegang configureert u de [locaties voorwaarde](active-directory-conditional-access-locations.md) soort scenario's die zijn gerelateerd aan de netwerklocaties. De voorwaarde locaties kunt u selecteren [benoemde locaties](active-directory-conditional-access-locations.md#named-locations), die staan voor logische groeperingen van IP-adresbereiken, landen en regio's.  

Normaal gesproken uw organisatie eigenaar is van een of meer-adresbereiken, bijvoorbeeld 199.30.16.0 - 199.30.16.24.
U kunt een benoemde locatie door te configureren:

- Dit adresbereik (199.30.16.0/24) op te geven 

- Toewijzen van een beschrijvende naam, zoals **bedrijfsnetwerk** 


In plaats van probeert te definiëren wat alle locaties zijn die niet vertrouwd zijn, kunt u het volgende doen:

- Opnemen 

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-untrusted-networks/02.png)

- Alle vertrouwde locaties uitsluiten 

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-untrusted-networks/01.png)



## <a name="implementation"></a>Implementatie

Met de methode die in dit artikel worden beschreven, kunt u nu een beleid voor voorwaardelijke toegang voor niet-vertrouwde locaties configureren. U moet uw beleid test voordat u implementeert in productie om ervoor te zorgen dat deze naar verwachting werkt. In het ideale geval moet u uw eerste tests in een testtenant indien mogelijk doen. Zie voor meer informatie, [hoe moet u een nieuw beleid implementeren](active-directory-conditional-access-best-practices.md#how-should-you-deploy-a-new-policy). 



## <a name="next-steps"></a>Volgende stappen

Als u meer informatie over voorwaardelijke toegang wilt, raadpleegt u [wat is voorwaardelijke toegang in Azure Active Directory?](active-directory-conditional-access-azure-portal.md)