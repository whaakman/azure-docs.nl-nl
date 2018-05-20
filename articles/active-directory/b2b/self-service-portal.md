---
title: Self-service portal voor registratie voor Azure Active Directory B2B-samenwerking | Microsoft Docs
description: Azure Active Directory B2B-samenwerking ondersteunt uw externe bedrijfsrelaties door zakelijke partners selectief toegang te verlenen tot uw zakelijke toepassingen
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/08/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 5ee55034e84fe09484a2f7613cc2224be70fdebb
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Self-service portal voor Azure AD B2B-samenwerking aanmelding

Klanten kunnen veel doen met de ingebouwde functies die beschikbaar worden gesteld via de [Azure-portal](https://portal.azure.com) en de [toepassing Toegangsvenster](https://myapps.microsoft.com) voor eindgebruikers. Mogelijk moet u echter de werkstroom voorbereiden voor B2B-gebruikers aan de behoeften van uw organisatie aanpassen. U kunt dit doen met [de uitnodiging API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

Als een organisatie uitnodigen, mag u niet weet tevoren die de afzonderlijke externe deelnemers zijn die toegang tot uw resources nodig hebben. U moet een manier voor gebruikers van partnerbedrijven zich aanmelden met een reeks beleidsregels die u als de organisatie uitnodigen bepaalt. Dit scenario is het mogelijk via de API's. Er is een [voorbeeldproject op GitHub](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web) die wordt geregeld.

Dit project GitHub toont hoe organisaties de API's kunnen gebruiken om te voorzien in uw vertrouwde partners, met regels die bepalen van de apps die toegang te krijgen tot een aanmelding op basis van beleid, selfservice-functie. Gebruikers van de accountpartner kunnen toegang krijgen tot bronnen wanneer zij die nodig. Ze kunnen dit veilig, doen zonder de uitnodiging organisatie handmatig vrijgeven ze. U kunt gemakkelijk het project implementeren in een Azure-abonnement van uw keuze.

## <a name="as-is-code"></a>Als-code

Deze code wordt uitgevoerd als een voorbeeld ter illustratie van informatie over het gebruik van de Azure Active Directory B2B-uitnodiging API beschikbaar zijn. Deze moet worden aangepast door uw ontwikkelteam of een partner en moet worden gecontroleerd voordat u deze in een productiescenario implementeren.

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
* [Azure AD B2B-samenwerking licentieverlening](licensing-guidance.md)
* [Azure Active Directory B2B-samenwerking Veelgestelde vragen (FAQ)](faq.md)