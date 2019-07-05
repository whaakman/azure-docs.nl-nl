---
title: Meer informatie over de autorisatieprotocollen die worden ondersteund door Microsoft identity-platform | Azure
description: Een handleiding voor OAuth 2.0 en OpenID Connect-protocollen die worden ondersteund door het eindpunt van Microsoft identity-platform.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 5fb4fa1b-8fc4-438e-b3b0-258d8c145f22
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7ffef5c3a7c8dd21654b6364013b1718bea1292
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67482990"
---
# <a name="microsoft-identity-platform-protocols"></a>Microsoft identity-platform-protocollen

De Microsoft identity-platform-eindpunt voor identity-as-a-service met standaardprotocollen, OpenID Connect en OAuth 2.0. Terwijl de service compatibel met de standaarden is, kunnen er subtiele verschillen tussen de twee implementaties van deze protocollen. De informatie hier is handig als u ervoor kiest uw code schrijven door rechtstreeks verzenden en verwerken van HTTP-aanvragen of een bibliotheek van derden open-source gebruiken in plaats van een van onze [open source-bibliotheken](reference-v2-libraries.md).

> [!NOTE]
> Niet alle Azure AD-scenario's en functies worden ondersteund door het eindpunt van de Microsoft identity-platform. Meer informatie over om te bepalen als u het eindpunt van de Microsoft identity-platform moet gebruiken, [beperkingen van het Microsoft identity platform](active-directory-v2-limitations.md).

## <a name="the-basics"></a>De basisbeginselen

In bijna alle stromen van OAuth 2.0 en OpenID Connect zijn er vier partijen die betrokken zijn bij de uitwisseling van:

![Diagram van de OAuth 2.0-rollen](./media/active-directory-v2-flows/protocols-roles.svg)

* De **Autorisatieserver** is het eindpunt van Microsoft identity-platform en is verantwoordelijk voor de identiteit van de gebruiker, verlenen en intrekken van toegang tot bronnen en uitgeven van tokens. De autorisatie-server ook wel bekend als de id-provider - veilig worden verwerkt niets te maken met gegevens van de gebruiker, de toegang en de vertrouwensrelaties tussen de partijen in een stroom.
* De **Resource-eigenaar** is doorgaans de eindgebruiker. Het is de partij die eigenaar is van de gegevens en het vermogen om toe te staan van derden voor toegang tot die gegevens of bron heeft.
* De **OAuth-Client** is uw app, geïdentificeerd door de toepassings-ID. De OAuth-client is meestal de partij die de gebruiker de interactie met en tokens opgevraagd uit de autorisatie-server. De client moet worden gemachtigd voor toegang tot de resource door de resource-eigenaar.
* De **bronserver** is waarin de resource of de gegevens zich bevindt. Deze vertrouwt de Autorisatieserver veilig verifiëren en autoriseren van de OAuth-Client en toegang-Bearer-tokens gebruikt om ervoor te zorgen dat toegang tot een resource kan worden verleend.

## <a name="app-registration"></a>App-registratie

Elke app die wil accepteren zowel persoonlijke en werk-of schoolaccounts moet worden geregistreerd via de **App-registraties** -ervaring in de [Azure-portal](https://aka.ms/appregistrations) voordat deze zich voor deze gebruikers aanmelden kan in met behulp van OAuth 2.0 of OpenID Connect. Het registratieproces app verzamelt en enkele waarden toewijzen aan uw app:

* Een **toepassings-ID** die uw app uniek wordt aangeduid
* Een **omleidings-URI** (optioneel) die kunnen worden gebruikt om te leiden antwoorden naar uw app terug
* Enkele andere scenariospecifieke waarden.

Zie [Een app registreren](quickstart-register-app.md) voor meer informatie.

## <a name="endpoints"></a>Eindpunten

Zodra geregistreerd, wordt de status van de app communiceert met Microsoft identity-platform door verzoeken te sturen naar het eindpunt:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Waar de `{tenant}` kan duren voordat een van de vier verschillende waarden:

| Value | Description |
| --- | --- |
| `common` | Kunnen gebruikers met persoonlijke Microsoft-accounts en werk-of schoolaccounts van Azure AD aan te melden bij de toepassing. |
| `organizations` | Hiermee kunt u alleen gebruikers met een werk-of schoolaccounts van Azure AD aan te melden bij de toepassing. |
| `consumers` | Hiermee kunnen alleen gebruikers met persoonlijke Microsoft-accounts (MSA) aan te melden bij de toepassing. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` | Hiermee kunnen alleen gebruikers met een werk-of schoolaccounts van een specifieke Azure AD-tenant om aan te melden bij de toepassing. Ofwel de beschrijvende domeinnaam van de Azure AD-tenant of GUID-id van de tenant kan worden gebruikt. |

Als u wilt weten hoe om te communiceren met deze eindpunten, kiest u een bepaalde app-type in de [protocollen](#protocols) sectie en volg de koppelingen voor meer informatie.

> [!TIP]
> Elke app die is geregistreerd bij Azure AD kan het eindpunt van Microsoft identity-platform, kunt gebruiken, zelfs als ze zich niet in persoonlijke accounts.  Op deze manier kunt u bestaande toepassingen met Microsoft identity-platform migreren en [MSAL](reference-v2-libraries.md) zonder uw toepassing opnieuw te maken.  

## <a name="tokens"></a>Tokens

De Microsoft identity-platform-implementatie van OAuth 2.0 en OpenID Connect maken intensief gebruik van bearer-tokens, met inbegrip van weergegeven als JWTs bearer-tokens. Een bearer-token is een lichtgewicht beveiligingstoken die de "bearer" toegang tot een beveiligde bron verleent. In dit opzicht is de 'bearer' een partij die het token kan opleveren. Hoewel een partij moet eerst worden geverifieerd met Microsoft identity-platform voor het ontvangen van het bearer-token, als de vereiste stappen zijn niet in gebruik voor het beveiligen van de token in overdracht en opslag, kan worden onderschept en die worden gebruikt door een onbedoelde partij. Hoewel sommige beveiligingstokens een ingebouwd mechanisme hebben voor zo wordt voorkomen dat onbevoegden gebruik ervan, wordt bearer-tokens geen dit mechanisme en moeten worden overgebracht in een beveiligd kanaal, zoals transport layer security (HTTPS). Als een bearer-token wordt verzonden in de wissen, kunt een schadelijke partij een man-in-the-middle-aanval gebruiken om de token ophalen en deze gebruiken voor niet-geautoriseerde toegang tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer op te slaan of bearer-tokens voor later gebruik opslaan in cache. Altijd voor zorgen dat uw app worden verzonden en bearer-tokens worden opgeslagen op een veilige manier. Zie voor meer beveiligingsoverwegingen op bearer-tokens, [RFC 6750 sectie 5](https://tools.ietf.org/html/rfc6750).

Meer informatie van verschillende typen tokens die worden gebruikt in de Microsoft identity-platform-eindpunt is beschikbaar in [de Microsoft identity platform token eindpuntreferentie](v2-id-and-access-tokens.md).

## <a name="protocols"></a>Protocollen

Als u klaar om te zien van bepaalde voorbeeld-aanvragen bent, aan de slag met een van de onderstaande zelfstudies. Elke regel komt overeen met een bepaalde verificatiescenario. Als u hulp bij het vaststellen van de juiste stroom voor u nodig hebt, bekijk dan [de typen apps die u met Microsoft identity-platform maken kunt](v2-app-types.md).

* [Mobiele en systeemeigen toepassing met OAuth 2.0 bouwen](v2-oauth2-auth-code-flow.md)
* [Bouw web-apps met OpenID Connect](v2-protocols-oidc.md)
* [Apps van één pagina met het OAuth 2.0 impliciete stroom bouwen](v2-oauth2-implicit-grant-flow.md)
* [Bouw daemons of serverzijde processen met het OAuth 2.0-clientreferentiestroom](v2-oauth2-client-creds-grant-flow.md)
* [Tokens ophalen in een web-API met de OAuth 2.0-namens-stroom](v2-oauth2-on-behalf-of-flow.md)
