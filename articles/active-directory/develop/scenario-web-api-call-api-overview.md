---
title: Web-API die downstream Web-Api's (overzicht) aanroept-micro soft Identity-platform
description: Meer informatie over het bouwen van een web-API die downstream Web-Api's (overzicht) aanroept.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ef9fc121b16d81eed932d1ab55ca38d2a2f1057
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852503"
---
# <a name="scenario-web-api-that-calls-web-apis"></a>Scenario: Web-API die web-API's aanroept

Meer informatie over wat u nodig hebt om een web-API te maken die web-Api's aanroept.

## <a name="prerequisites"></a>Vereisten

In dit scenario wordt de beveiligde web-API voor het aanroepen van web-Api's gebaseerd op het scenario ' een web-API beveiligen '. Zie voor meer informatie over dit kern scenario de [beveiligde web API-scenario](scenario-protected-web-api-overview.md) .

## <a name="overview"></a>Overzicht

- Een client (Web-, Desktop-, mobiele of toepassing met één pagina)-wordt niet weer gegeven in het onderstaande diagram: roept een beveiligde web-API aan en biedt een JWT Bearer-token in de http-header autorisatie.
- De beveiligde web-API valideert het token en maakt gebruik `AcquireTokenOnBehalfOf` van de methode MSAL om een andere token te aanvragen (vanuit Azure AD), zodat het zelf een tweede Web-API (de downstream Web-API) kan aanroepen namens de gebruiker.
- De beveiligde web-API gebruikt dit token om een downstream API aan te roepen. Het kan ook later `AcquireTokenSilent`aanroepen om tokens aan te vragen voor andere downstream-api's (maar nog steeds namens dezelfde gebruiker). `AcquireTokenSilent`Hiermee wordt het token vernieuwd wanneer dit nodig is.

![Web-API die een web-API aanroept](media/scenarios/web-api.svg)

## <a name="specifics"></a>Opsporingsgegevens

Het deel van de registratie van de app met betrekking tot de API-machtigingen is klassiek. De configuratie van de toepassing omvat het gebruik van de OAuth 2,0-stroom om het JWT Bearer-token uit te wisselen op basis van een token voor een stroomafwaartse API. Dit token wordt toegevoegd aan de token cache, waar deze beschikbaar is in de controller van de Web-API, en kan een token op de achtergrond verkrijgen om downstream-Api's aan te roepen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Appregistratie](scenario-web-api-call-api-app-registration.md)
