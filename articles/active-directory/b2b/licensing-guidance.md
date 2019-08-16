---
title: Hulp bij B2B-samen werking Licensing-Azure Active Directory | Microsoft Docs
description: Azure Active Directory B2B-samen werking vereist geen betaalde Azure AD-licenties, maar u kunt ook betaalde functies voor B2B-gast gebruikers ontvangen.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 08/15/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 081061eae07fa3765d032ad155e59ebf5aa3cbc9
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69512556"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Licentierichtlijnen voor B2B-samenwerking van Azure Active Directory

Met Azure Active Directory (Azure AD) Collaborate Business-to-Business (B2B)-samen werking kunt u externe gebruikers (of "gast gebruikers") uitnodigen om uw betaalde Azure AD-services te gebruiken. Sommige functies zijn gratis, maar voor elke betaalde Azure AD-functie kunt u Maxi maal vijf gast gebruikers uitnodigen voor elke Azure AD-licentie die u hebt voor een werk nemer of een niet-gast gebruiker in uw Tenant.

> [!NOTE]
> Raadpleeg de [Azure Active Directory prijzen](https://azure.microsoft.com/pricing/details/active-directory/) voor meer informatie over Azure ad-prijzen en B2B-samenwerkings functies.

B2B gast gebruikers licenties worden automatisch berekend en gerapporteerd op basis van de verhouding van 1:5. Op dit moment is het niet mogelijk om B2B-gebruikers licenties rechtstreeks aan gast gebruikers toe te wijzen.

Bovendien kunnen gast gebruikers gratis Azure AD-functies gebruiken zonder aanvullende licentie vereisten. Gast gebruikers hebben toegang tot gratis Azure AD-functies, zelfs als u geen betaalde Azure AD-licenties hebt. 

## <a name="examples-calculating-guest-user-licenses"></a>Voorbeelden: Licenties voor gast gebruikers worden berekend
Wanneer u hebt vastgesteld hoeveel gast gebruikers toegang nodig hebben tot uw betaalde Azure AD-Services, moet u ervoor zorgen dat u over voldoende betaalde licenties voor Azure AD beschikt om gast gebruikers te voorzien van de vereiste verhouding van 1:5. Hier volgen enkele voorbeelden:

- U wilt 100 gast gebruikers uitnodigen voor uw Azure AD-apps of-services en u wilt toegangs beheer en inrichting toewijzen aan alle gast gebruikers. U wilt ook MFA en voorwaardelijke toegang vereisen voor 50 van deze gast gebruikers. U hebt 10 Azure AD Basic licenties en 10 Azure AD Premium P1-licenties nodig om deze combi natie te kunnen behandelen. Als u van plan bent om functies voor identiteits beveiliging te gebruiken voor uw gast gebruikers, hebt u voor de gast gebruikers Azure AD Premium P2-licenties in dezelfde 1:5-verhouding nodig.
- U wilt 60 gast gebruikers uitnodigen die alle MFA vereisen, dus u moet ten minste 12 Azure AD Premium P1-licenties hebben. U hebt 10 werk nemers met Azure AD Premium P1-licenties, waardoor Maxi maal 50 gast gebruikers de 1:5-licentie verhouding kunnen hebben. U moet twee extra Premium P1-licenties aanschaffen om 10 extra gast gebruikers te kunnen behandelen.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende bronnen in azure AD B2B-samen werking:

* [Azure Active Directory prijzen](https://azure.microsoft.com/pricing/details/active-directory/)
* [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
* [Veelgestelde vragen over Azure Active Directory B2B-samenwerking](faq.md)
