---
title: Self-service portal voor registratie voor Azure Active Directory B2B-samenwerking | Microsoft Docs
description: Azure Active Directory B2B-samenwerking ondersteunt uw externe bedrijfsrelaties door zakelijke partners selectief toegang te verlenen tot uw zakelijke toepassingen
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/08/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 94001b005a883c172cab279029b47ac1ad0c0de5
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643824"
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Self-service portal voor aanmelding bij Azure AD B2B-samenwerking

Klanten kunnen veel doen met de ingebouwde functies die worden weergegeven via de [Azure-portal](https://portal.azure.com) en de [Toegangsvenster](https://myapps.microsoft.com) voor eindgebruikers. U moet echter mogelijk om aan te passen van de werkstroom onboarding voor B2B-gebruikers aan de behoeften van uw organisatie. U kunt dit doen met [de uitnodiging API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

Als een organisatie kan u niet weet tevoren die de afzonderlijke externe deelnemers zijn die toegang tot uw resources nodig hebben. U moet een manier voor gebruikers van partnerbedrijven zich aanmelden met een set beleidsregels die u als de uitnodigende organisatie bepaalt. In dit scenario is het mogelijk via de API's. Er is een [voorbeeldproject op GitHub](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web) die precies dat heeft.

Deze GitHub-project laat zien hoe organisaties de API's kunnen gebruiken voor een aanmelding op basis van beleid, selfservice-mogelijkheid van uw vertrouwde partners, met regels die bepalen de toegang te krijgen tot apps. Gebruikers van partnerorganisaties kunnen toegang krijgen tot bronnen wanneer ze deze nodig hebt. Ze kunnen dit veilig, doen zonder dat de uitnodigende organisatie handmatig onboarding ze. U kunt gemakkelijk het project implementeren in een Azure-abonnement van uw keuze.

## <a name="as-is-code"></a>As-is-code

Deze code is beschikbaar als een voorbeeld ter illustratie van het gebruik van de Azure Active Directory B2B-uitnodiging API. Het moet worden aangepast door uw ontwikkelingsteam of een partner, en moet worden gecontroleerd voordat u deze in een productiescenario voor implementeert.

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
* [Licentieverlening voor Azure AD B2B collaboration](licensing-guidance.md)
* [Azure Active Directory B2B-samenwerking Veelgestelde vragen (FAQ)](faq.md)