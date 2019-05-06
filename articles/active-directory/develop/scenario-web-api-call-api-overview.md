---
title: Web-API die downstream aanroept web-API's (overzicht) - Microsoft identity-platform
description: Meer informatie over het bouwen van een web-API die downstream web-aanroepen van API's (overzicht).
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
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 497134b7f3cc535f7b3f180db13cd04ef56787db
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075398"
---
# <a name="scenario-web-api-that-calls-web-apis"></a>Scenario: Web-API die web-API's aanroepen

Meer informatie over alle moet u een web-API bouwen die worden aangeroepen web-API's.

## <a name="prerequisites"></a>Vereisten

Dit scenario, beveiligde web-API aanroepen van web-API's, dat voortbouwt op het scenario 'Een web-API beveiligen'. Zie voor meer informatie over deze fundamentele scenario, [beveiligd Web API - Scenario](scenario-protected-web-api-overview.md) eerste.

## <a name="overview"></a>Overzicht

- Een client (web, desktop, mobiel apparaat, of één pagina application) - niet weergegeven op het onderstaande diagram - een beveiligde web-API-aanroepen en biedt een JWT bearer-token in de "Autorisatie" Http-header.
- De beveiligde web-API valideert het token en maakt gebruik van de MSAL `AcquireTokenOnBehalfOf` methode om aan te vragen (vanuit Azure AD) een ander token zodat deze kunnen zelf, aanroepen van een tweede web-API (met de naam van de downstream web-API) namens de gebruiker.
- De beveiligde web-API maakt gebruik van dit token een downstream-API aan te roepen. Kan ook worden aangeroepen `AcquireTokenSilent`later aanvragen van tokens voor andere downstream-API's (maar nog steeds namens de gebruiker). `AcquireTokenSilent` Hiermee vernieuwt u de token wanneer dat nodig is.

![Web-API aanroepen van een web-API](media/scenarios/web-api.svg)

## <a name="specifics"></a>Specifieke informatie

Het deel van de app-registratie met betrekking tot de API-machtigingen is klassieke. Configuratie van de toepassing omvat het gebruik van de OAuth 2.0-namens-stroom voor het uitwisselen van het JWT bearer-token op basis van een token voor een downstream-API. Dit token wordt toegevoegd aan de tokencache, waar het is beschikbaar in de web-API van domeincontrollers en een token op de achtergrond om aan te roepen downstream API's kunt ophalen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Appregistratie](scenario-web-api-call-api-app-registration.md)
