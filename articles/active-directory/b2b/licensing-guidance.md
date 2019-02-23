---
title: B2B-samenwerking richtlijnen - Azure Active Directory-licentieverlening | Microsoft Docs
description: Azure Active Directory B2B collaboration niet vereist-betaalde Azure AD-licenties, maar u kunt ook ophalen betaalde functies voor gastgebruikers voor B2B
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7fa01a6bf522061e54e9622cb9201f81c699a8ec
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56668449"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Licentierichtlijnen voor B2B-samenwerking van Azure Active Directory

Met business-to-business (B2B)-samenwerking van Azure Active Directory (Azure AD), kunt u uitnodigen externe gebruikers (of "gastgebruikers") met uw betaalde Azure AD-services. Voor elke Azure AD-licentie die u aan een gebruiker betaalde toewijst, kunt u maximaal vijf gastgebruikers ook kunnen onder de aftrek van de externe gebruiker uitnodigen.

Gebruikerslicenties voor B2B-Gast wordt automatisch berekend en gerapporteerd op basis van de hoogte-breedteverhouding 1:5. Het is momenteel niet mogelijk om toe te wijzen B2B Gast gebruikerslicenties rechtstreeks naar gastgebruikers.

Bovendien gratis Gast kunnen gebruikers met Azure AD-functies met geen aanvullende licentievereisten te voldoen. Gastgebruikers hebben toegang tot Azure AD-functies gratis, zelfs als u geen hebt betaalde Azure AD-licenties. 

## <a name="examples-calculating-guest-user-licenses"></a>Voorbeelden: Berekenen van de Gast-gebruikerslicenties
Zodra u hoeveel gastgebruikers nodig bepalen voor toegang tot uw betaalde Azure AD-services, zorg ervoor dat u hebt voldoende Azure AD betaalde licenties voor gastgebruikers ook kunnen in de vereiste 1:5-verhouding. Hier volgen enkele voorbeelden:

- U wilt uitnodigen 100 gastgebruikers ook kunnen aan uw Azure AD-apps of services, en u wilt toewijzen, toegangsbeheer en inrichting voor alle gastgebruikers. U wilt ook vereisen dat MFA en voorwaardelijke toegang voor deze gastgebruikers 50. Hebt u voor deze combinatie van 10 licenties voor Azure AD Basic en 10 licenties voor Azure AD Premium P1 nodig. Als u van plan bent te Identity Protection-functies met uw gastgebruikers ook kunnen gebruiken, moet u Azure AD Premium P2-licenties in de dezelfde 1:5-verhouding tussen het betrekking hebben op de gastgebruikers.
- U wilt uitnodigen 60 gastgebruikers die alle MFA, vereist dus u moet ten minste 12 Azure AD Premium P1-licenties hebt. Hebt u 10 werknemers met Azure AD Premium P1-licenties kunnen maximaal 50 gastgebruikers ook kunnen onder de 1:5 verhouding-licentieverlening. U moet twee extra Premium P1-licenties voor 10 extra Gastgebruikers ook kunnen kopen.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen op Azure AD B2B-samenwerking:

* [Prijzen voor Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
* [Veelgestelde vragen over Azure Active Directory B2B-samenwerking](faq.md)
