---
title: Wat is de beveiliging van een basislijn van voorwaardelijke toegang van Azure Active Directory | Microsoft Docs
description: Meer informatie over hoe basislijn beveiliging zorgt ervoor dat er ten minste de basisbeveiliging ingeschakeld in uw omgeving.
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
ms.date: 06/08/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 25ae4db2cd4f2a2cea74c428a272c6868acaa5c5
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35249269"
---
# <a name="what-is-baseline-protection"></a>Wat is basislijn beveiliging?  

In het afgelopen jaar zijn aanvallen identiteit met 300% verhoogd. Om te voorkomen dat uw omgeving de steeds groter wordende aanvallen, introduceert Azure Active Directory (Azure AD) een nieuwe functie voor basislijn beveiliging genoemd. Beveiliging van de basislijn is een reeks vooraf gedefinieerde voorwaardelijke toegangsbeleid. Het doel van dit beleid is om ervoor te zorgen dat er ten minste de basisbeveiliging ingeschakeld in uw omgeving. 

Tijdens de preview moet u een basislijn beleidsregels inschakelen als u wilt activeren. Na algemene beschikbaarheid deze beleidsregels zijn standaard ingeschakeld. 

Het eerste basislijn protection-beleid MFA is vereist voor bevoegde accounts. Aanvallers die controle van bevoorrechte accounts kunnen enorm veel schade doen, dus is het essentieel dat u eerst deze accounts beveiligen. De volgende bevoorrechte rollen zijn binnen het bereik van dit beleid: 

- Globale beheerder  

- SharePoint-beheerder  

- Exchange-beheerder  

- Voorwaardelijke toegang beheerder  

- Beveiligingsbeheerder  


![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/01.png)

## <a name="how-to-get-started"></a>Hoe gaat u aan de slag 

Basisbeleid inschakelen:  

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder, beveiligingsbeheerder of beheerder voorwaardelijke toegang.

2. In de **Azure-portal**, klik op de navigatiebalk links, **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/02.png)

3. Op de **Azure Active Directory** pagina in de **beheren** sectie, klikt u op **voorwaardelijke toegang**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-baseline-protection/03.png)

4. Klik in de lijst met beleidsregels op **Basisbeleid: MFA vereisen voor beheerders (Preview)**. 

5. Als het beleid, klikt u op **beleid direct gebruiken**.

6. Klik op **Opslaan**. 
 

De basislijn-beleid biedt u de optie om gebruikers en groepen te sluiten. U mogelijk wilt uitsluiten van een *[EMS-access-Administrator-account](active-directory-admin-manage-emergency-access-accounts.md)* zodat u niet de tenant zijn vergrendeld.
  
 

## <a name="what-you-should-know"></a>Wat u moet weten 

De directory-functies die zijn opgenomen in het Basisbeleid zijn het meest bevoorrechte rollen voor Azure AD. Op basis van feedback, kunnen anderen worden opgenomen in de toekomst. 

Als u accounts met administratorbevoegdheden in uw scripts hebt, moet u [beheerde Service identiteit (MSI)](managed-service-identity/overview.md) of [service-principals (met certificaten)](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal) in plaats daarvan. Als tijdelijke oplossing kunt u specifieke gebruikersaccounts uitsluiten van uw basislijn-beleid. 

Het beleid is van toepassing op verouderde verificatie stroomt zoals POP, IMAP, oudere Office desktop client. 

## <a name="next-steps"></a>Volgende stappen

Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 
