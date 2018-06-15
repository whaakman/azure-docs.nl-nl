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
ms.openlocfilehash: b16f7c040212db6d0dbeb7161a18f205cf524090
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33930432"
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Self-service portal voor Azure AD B2B-samenwerking aanmelding

Klanten kunnen veel doen met de ingebouwde functies die beschikbaar worden gesteld via de [Azure-portal](https://portal.azure.com) en de [toepassing Toegangsvenster](https://myapps.microsoft.com) voor eindgebruikers. Mogelijk moet u echter de werkstroom voorbereiden voor B2B-gebruikers aan de behoeften van uw organisatie aanpassen. U kunt dit doen met [de uitnodiging API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

Als een organisatie uitnodigen, mag u niet weet tevoren die de afzonderlijke externe deelnemers zijn die toegang tot uw resources nodig hebben. U moet een manier voor gebruikers van partnerbedrijven zich aanmelden met een reeks beleidsregels die u als de organisatie uitnodigen bepaalt. Dit scenario is het mogelijk via de API's. Er is een [voorbeeldproject op GitHub](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web) die wordt geregeld.

Dit project GitHub toont hoe organisaties de API's kunnen gebruiken om te voorzien in uw vertrouwde partners, met regels die bepalen van de apps die toegang te krijgen tot een aanmelding op basis van beleid, selfservice-functie. Gebruikers van de accountpartner kunnen toegang krijgen tot bronnen wanneer zij die nodig. Ze kunnen dit veilig, doen zonder de uitnodiging organisatie handmatig vrijgeven ze. U kunt gemakkelijk het project implementeren in een Azure-abonnement van uw keuze.

## <a name="as-is-code"></a>Als-code

Deze code wordt uitgevoerd als een voorbeeld ter illustratie van informatie over het gebruik van de Azure Active Directory B2B-uitnodiging API beschikbaar zijn. Deze moet worden aangepast door uw ontwikkelteam of een partner en moet worden gecontroleerd voordat u deze in een productiescenario implementeren.

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Azure AD B2B-samenwerking licentieverlening](active-directory-b2b-licensing.md)
* [Azure Active Directory B2B-samenwerking Veelgestelde vragen (FAQ)](active-directory-b2b-faq.md)