---
title: Toepassingen met één pagina in Azure Active Directory
description: Hierin wordt beschreven welke toepassingen met één pagina (kuuroorden) zijn en de basisprincipes van stroom protocol, registratie en verlopen van het token voor dit apptype.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.openlocfilehash: 8b06f43522cd9c93be16de19036de65b69b07941
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967775"
---
# <a name="single-page-applications"></a>Toepassingen met één pagina

Toepassingen met één pagina (kuuroorden) worden doorgaans gestructureerd als een JavaScript-presentatie-laag (front-end) die in de browser wordt uitgevoerd en een web-API back-end die wordt uitgevoerd op een server en implementeert de bedrijfslogica van de toepassing. Zie voor meer informatie over de impliciete autorisatietoekenning en kunt u bepalen of deze geschikt is voor uw toepassingsscenario, [inzicht in de OAuth2-impliciete stroom in Azure Active Directory verlenen](v1-oauth2-implicit-grant-flow.md).

In dit scenario is wanneer de gebruiker zich aanmeldt, JavaScript front-end gebruikt [Active Directory Authentication Library voor JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) en de impliciete autorisatietoekenning verkrijgen van een ID-token (id_token) van Azure AD. Het token is opgeslagen in de cache en de client gekoppeld aan de aanvraag als het bearer-token bij het maken van aanroepen naar de Web-API-back-end, dat is beveiligd met behulp van de OWIN-middleware.

## <a name="diagram"></a>Diagram

![Diagram van de toepassing met één pagina](./media/authentication-scenarios/single_page_app.png)

## <a name="protocol-flow"></a>Stroom protocol

1. De gebruiker navigeert naar de webtoepassing.
1. De toepassing retourneert de JavaScript-front-end (presentatielaag) naar de browser.
1. De gebruiker een aanmelding initieert, bijvoorbeeld door te klikken op een koppeling aanmelden. De browser verzendt een GET naar het autorisatie-eindpunt van de Azure AD om aan te vragen van een ID-token. Deze aanvraag bevat de toepassings-ID en antwoord-URL de queryparameters.
1. Azure AD valideert de antwoord-URL op basis van de geregistreerde antwoord-URL die is geconfigureerd in Azure portal.
1. De gebruiker meldt zich aan op de pagina aanmelden.
1. Als verificatie geslaagd is, wordt Azure AD maakt u een ID-token en stuurt deze als een URL-fragment (#) naar antwoord-URL van de toepassing. Voor een productietoepassing moet deze antwoord-URL HTTPS. Het geretourneerde token bevat claims over de gebruiker en de Azure AD die nodig zijn voor de toepassing voor het valideren van het token.
1. De JavaScript-client-code die wordt uitgevoerd in de browser extraheert het token uit het antwoord om te gebruiken bij het beveiligen van aanroepen van de toepassing web dat API terug beëindigen.
1. De browser aanroept van de toepassing-web-API terug eindigen met de ID-token in de autorisatie-header. De Azure AD-verificatieservice problemen met een ID-token die kan worden gebruikt als een bearer-token als de resource hetzelfde als de client-ID is (in dit geval, dit geldt als de web-API de app zelf back-end is).

## <a name="code-samples"></a>Codevoorbeelden

Zie de [codevoorbeelden voor scenario's met één pagina toepassing](sample-v1-code.md#single-page-applications). Zorg ervoor dat u terugkomen vaak als nieuwe voorbeelden worden regelmatig toegevoegd.

## <a name="app-registration"></a>App-registratie

* Één tenant - als u een toepassing voor uw organisatie bouwt, moet deze worden geregistreerd in de directory van uw bedrijf met behulp van de Azure-portal.
* Multitenant - als u het bouwen van een toepassing die kan worden gebruikt door gebruikers buiten uw organisatie, deze moet worden geregistreerd in de directory van uw bedrijf, maar ook moet worden geregistreerd in de adreslijst van elke organisatie die van de toepassing gebruikmaakt. Als u uw toepassing in de directory, kunt u een aanmeldingsproces opnemen voor uw klanten waarmee ze akkoord gaan met uw toepassing. Wanneer ze zich registreren voor uw toepassing, wordt deze weergegeven met een dialoogvenster waarin de machtigingen die voor de toepassing moet worden weergegeven, en vervolgens de optie om in te stemmen. Afhankelijk van de vereiste machtigingen kan een beheerder in de andere organisatie worden vereist om toestemming te geven. Wanneer de gebruiker of beheerder hiermee akkoord gaat, wordt de toepassing is geregistreerd in de directory.

Na de registratie van de toepassing, moet deze worden geconfigureerd voor het gebruik van OAuth 2.0-impliciete-protocol. Dit protocol is standaard uitgeschakeld voor toepassingen. Schakel het protocol van de impliciete goedkeuring voor OAuth2 voor uw toepassing, het manifest van de toepassing vanuit Azure portal bewerken en de waarde 'oauth2AllowImplicitFlow' ingesteld op true. Zie voor meer informatie, [toepassingsmanifest](reference-app-manifest.md).

## <a name="token-expiration"></a>Geldigheidsduur van het token

Met behulp van ADAL.js helpt met het volgende:

* vernieuwen van een token verlopen
* aanvragen van een toegangstoken voor het aanroepen van een web API-resource

Na een geslaagde verificatie wordt in Azure AD een cookie schrijft in de browser van de gebruiker een sessie tot stand brengen. Houd er rekening mee dat de sessie bestaat tussen de gebruiker en de Azure AD (niet tussen de gebruiker en de web-App). Wanneer een token is verlopen, ADAL.js maakt gebruik van deze sessie op de achtergrond verkrijgen van een ander token. ADAL.js maakt gebruik van een verborgen iFrame te verzenden en ontvangen van de aanvraag met behulp van de impliciete goedkeuring voor OAuth-protocol. ADAL.js kunt ook hetzelfde mechanisme gebruiken op de achtergrond toegangstokens verkrijgen voor andere web-API-resources die de toepassing wordt aangeroepen, zolang deze resources ondersteuning voor cross-origin resource sharing (CORS), zijn geregistreerd in de map van de gebruiker en eventuele vereiste toestemming is opgegeven door de gebruiker tijdens het aanmelden.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [toepassingstypen en scenario's](app-types.md)
* Meer informatie over de Azure AD [de basisbeginselen van verificatie](authentication-scenarios.md)
