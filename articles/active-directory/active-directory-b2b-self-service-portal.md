---
title: Self-service portal voor registratie voor Azure Active Directory B2B-samenwerking | Microsoft Docs
description: Azure Active Directory B2B-samenwerking ondersteunt uw externe bedrijfsrelaties door zakelijke partners selectief toegang te verlenen tot uw zakelijke toepassingen
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: 307373c75bbb87cec683f7a3097f8f159c9d5e61
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Self-service portal voor Azure AD B2B-samenwerking aanmelding

Klanten kunnen veel doen met de ingebouwde functies die beschikbaar worden gesteld via onze IT-beheerder [Azure-portal](https://portal.azure.com) en onze [toepassing Toegangsvenster](https://myapps.microsoft.com) voor eindgebruikers. Maar er zijn ook op de hoogte dat bedrijven nodig hebben voor het aanpassen van de werkstroom voorbereiden voor B2B-gebruikers aan de behoeften van hun organisatie. Ze kunnen doen dat met [onze API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

In als u discussies met onze klanten zien we een gemeenschappelijke nodig toename up boven alle andere gebruikers. De uitnodiging organisatie kan niet van tevoren weet die de afzonderlijke externe deelnemers zijn die toegang tot hun bronnen nodig hebben. Ze wilden een manier voor gebruikers van partnerbedrijven zich aanmelden met een reeks beleidsregels die u de organisatie uitnodigen beheert. Dit scenario is mogelijk via onze API's, zodat we een project op Github die u zojuist hebt die is gepubliceerd: [Github-voorbeeldproject](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

Onze Github-project wordt gedemonstreerd hoe organisaties kunnen onze API's gebruiken en bieden een aanmelding op basis van beleid, selfservice-functie voor hun vertrouwde partners, met regels die bepalen van de toegang te krijgen tot apps. Gebruikers van de accountpartner kunnen toegang krijgen tot resources wanneer ze nodig hebben, veilig, zonder dat de organisatie van uitnodigen om handmatig vrijgeven ze. U kunt gemakkelijk het project implementeren in een Azure-abonnement van uw keuze.

## <a name="as-is-code"></a>Als-code

Houd er rekening mee dat deze code als voorbeeld voor het demonstreren van informatie over het gebruik van de Azure Active Directory B2B-uitnodiging API beschikbaar wordt gemaakt. Deze moet worden aangepast door uw team dev of een partner en moet worden gecontroleerd voordat ze worden geïmplementeerd in een productiescenario.

## <a name="next-steps"></a>Volgende stappen

Lees ook onze andere artikelen over Azure AD B2B-samenwerking:
* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hoe voeg beheerders van Azure Active Directory B2B-samenwerking gebruikers?](active-directory-b2b-admin-add-users.md)
* [Hoe kunnen IT-medewerkers B2B-samenwerking gebruikers toevoegen](active-directory-b2b-iw-add-users.md)
* [De elementen van de uitnodigingsmail voor B2B-samenwerking](active-directory-b2b-invitation-email.md)
* [B2B-samenwerking uitnodiging inwisseling](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B-samenwerking licentieverlening](active-directory-b2b-licensing.md)
* [Het oplossen van Azure Active Directory B2B-samenwerking](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B-samenwerking Veelgestelde vragen (FAQ)](active-directory-b2b-faq.md)
* [Multi-Factor Authentication voor gebruikers van B2B-samenwerking](active-directory-b2b-mfa-instructions.md)
* [B2B-samenwerking gebruikers zonder een uitnodiging toevoegen](active-directory-b2b-add-user-without-invite.md)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)