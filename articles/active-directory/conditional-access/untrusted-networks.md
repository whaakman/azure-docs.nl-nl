---
title: Hoe u multi-factor authentication (MFA) vereist voor toegang van niet-vertrouwde netwerken met voorwaardelijke toegang van Azure Active Directory (Azure AD) | Microsoft Docs
description: Informatie over het configureren van beleid voor voorwaardelijke toegang in Azure Active Directory (Azure AD) aan voor toegang tot pogingen van niet-vertrouwde netwerken.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.component: conditional-access
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/10/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 7ffb3bfb4985f56e7e2e81a2a6d08a6ff7469fdb
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54445402"
---
# <a name="how-to-require-mfa-for-access-from-untrusted-networks-with-conditional-access"></a>Procedure: MFA vereisen voor toegang van niet-vertrouwde netwerken met voorwaardelijke toegang   

Azure Active Directory (Azure AD) maakt eenmalige aanmelding aan apparaten, apps en services vanaf elke locatie. Uw gebruikers kunnen toegang krijgen tot uw cloud-apps niet alleen vanuit het netwerk van uw organisatie, maar ook vanaf elke internetlocatie met niet-vertrouwde. Voor toegang via niet-vertrouwde netwerken, kunt u een algemene best practice is het vereisen van multi-factor authentication (MFA).

Dit artikel vindt u de informatie die u wilt configureren van beleid voor voorwaardelijke toegang die MFA voor toegang vanuit niet-vertrouwde netwerken vereist. 

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met bent: 

- De [basisconcepten](overview.md) van voorwaardelijke toegang van Azure AD 
- De [aanbevolen procedures](best-practices.md) voor het configureren van beleid voor voorwaardelijke toegang in Azure portal



## <a name="scenario-description"></a>Scenariobeschrijving

Als u wilt de balans tussen beveiliging en productiviteit-master, is het mogelijk voldoende is voor u alleen een wachtwoord vereisen voor aanmeldingen vanaf het netwerk van uw organisatie. Voor toegang vanaf de locatie van een niet-vertrouwd netwerk, is er echter een betere risico dat aanmeldingen niet worden uitgevoerd door legitieme gebruikers. Om dit probleem op te lossen, kunt u de toegang van niet-vertrouwde netwerken blokkeren. U kunt ook kunt u multi-factor authentication (MFA) om te krijgen meer zekerheid dat er is een poging gedaan door de rechtmatige eigenaar van het account ook vereisen. 

Met Azure AD voor voorwaardelijke toegang, kunt u deze vereiste met één beleid waarmee toegang wordt verleend adres: 

- Aan de geselecteerde cloud-apps

- Voor de geselecteerde gebruikers en groepen  

- Meervoudige verificatie vereisen 

- Wanneer toegang afkomstig is van: 

    - Een locatie die wordt niet vertrouwd


## <a name="implementation"></a>Implementatie

De uitdaging van dit scenario is om te vertalen *toegang vanaf een niet-vertrouwd netwerklocatie* in een voorwaarde voor voorwaardelijke toegang. In een beleid voor voorwaardelijke toegang configureert u de [locaties voorwaarde](location-condition.md) soort scenario's die zijn gerelateerd aan de netwerklocaties. De voorwaarde locaties kunt u benoemde locaties, logische groeperingen van IP-adresbereiken, landen en regio's selecteren.  

Normaal gesproken uw organisatie eigenaar is van een of meer-adresbereiken, bijvoorbeeld 199.30.16.0 - 199.30.16.24.
U kunt een benoemde locatie door te configureren:

- Dit adresbereik (199.30.16.0/24) op te geven 

- Toewijzen van een beschrijvende naam, zoals **bedrijfsnetwerk** 


In plaats van probeert te definiëren wat alle locaties zijn die niet vertrouwd zijn, kunt u het volgende doen:

- Elke locatie opnemen 

    ![Voorwaardelijke toegang](./media/untrusted-networks/02.png)

- Alle vertrouwde locaties uitsluiten 

    ![Voorwaardelijke toegang](./media/untrusted-networks/01.png)



## <a name="policy-deployment"></a>Implementatie van beleid

Met de methode die in dit artikel worden beschreven, kunt u nu een beleid voor voorwaardelijke toegang voor niet-vertrouwde locaties configureren. Om ervoor te zorgen dat uw beleid werkt zoals verwacht, is het aanbevolen om deze te testen voordat u implementeert in productie. In het ideale geval gebruik een testtenant om te controleren of het nieuwe beleid werkt zoals bedoeld. Zie voor meer informatie, [over het implementeren van een nieuw beleid](best-practices.md#how-should-you-deploy-a-new-policy). 



## <a name="next-steps"></a>Volgende stappen

Als u meer informatie over voorwaardelijke toegang wilt, raadpleegt u [wat is voorwaardelijke toegang in Azure Active Directory?](../active-directory-conditional-access-azure-portal.md)