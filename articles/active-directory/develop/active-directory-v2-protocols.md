---
title: Meer informatie over de autorisatieprotocollen die worden ondersteund door Azure AD v2.0 | Microsoft Docs
description: Een handleiding voor protocollen die worden ondersteund door de Azure AD v2.0-eindpunt.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5fb4fa1b-8fc4-438e-b3b0-258d8c145f22
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 57a3d5fc50c2278b34fddbfba61b12b0d81a33ed
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52424524"
---
# <a name="v20-protocols---oauth-20--openid-connect"></a>v2.0-protocollen - OAuth 2.0 en OpenID Connect

Het v2.0-eindpunt kunt u Azure AD gebruiken voor identity-as-a-service met standaardprotocollen, OpenID Connect en OAuth 2.0. Terwijl de service compatibel met de standaarden is, kunnen er subtiele verschillen tussen de twee implementaties van deze protocollen. De informatie hier is handig als u ervoor kiest uw code schrijven door rechtstreeks verzenden en verwerken van HTTP-aanvragen of gebruik een 3e partij open source-bibliotheek, in plaats van met een van onze [open source-bibliotheken](reference-v2-libraries.md).

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt. Meer informatie over om te bepalen als u het v2.0-eindpunt moet gebruiken, [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="the-basics"></a>De basisbeginselen

In bijna alle OAuth en OpenID Connect-stromen zijn er vier partijen die betrokken zijn bij de uitwisseling van:

![OAuth 2.0-rollen](../../media/active-directory-v2-flows/protocols_roles.png)

* De **Autorisatieserver** is het v2.0-eindpunt. Het is verantwoordelijk voor het ervoor te zorgen dat de identiteit van de gebruiker, verlenen en intrekken van toegang tot bronnen en uitgeven van tokens. Het is ook wel bekend als de id-provider - niets te maken met gegevens van de gebruiker, de toegang en de vertrouwensrelaties tussen de partijen in een stroom veilig worden verwerkt.
* De **Resource-eigenaar** is doorgaans de eindgebruiker. Het is de partij die eigenaar is van de gegevens en het vermogen om toe te staan van derden voor toegang tot die gegevens of bron heeft.
* De **OAuth-Client** is uw app, geïdentificeerd door de toepassings-id. Meestal is dit de partij die de eindgebruiker de interactie met en tokens opgevraagd uit de autorisatie-server. De client moet worden gemachtigd voor toegang tot de resource door de resource-eigenaar.
* De **bronserver** is waarin de resource of de gegevens zich bevindt. Deze vertrouwt de Autorisatieserver veilig verifiëren en autoriseren van de OAuth-Client en Bearer access_tokens gebruikt om ervoor te zorgen dat toegang tot een resource kan worden verleend.

## <a name="app-registration"></a>App-registratie
Elke app die gebruikmaakt van het v2.0-eindpunt moet worden geregistreerd bij [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) voordat deze kan communiceren met behulp van OAuth of OpenID Connect. Het registratieproces app verzamelen & enkele waarden toewijzen aan uw app:

* Een **toepassings-Id** die uw app uniek wordt aangeduid
* Een **omleidings-URI** of **-pakket-id** die kunnen worden gebruikt om te leiden antwoorden naar uw app terug
* Enkele andere scenariospecifieke waarden.

Zie [Een app registreren](quickstart-v2-register-an-app.md) voor meer informatie.

## <a name="endpoints"></a>Eindpunten

Zodra geregistreerd, wordt de status van de app communiceert met Azure AD door verzoeken te sturen naar het v2.0-eindpunt:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Waar de `{tenant}` kan duren voordat een van de vier verschillende waarden:

| Waarde | Beschrijving |
| --- | --- |
| `common` |Kunnen gebruikers met persoonlijke Microsoft-accounts en werk-of schoolaccounts van Azure Active Directory aan te melden bij de toepassing. |
| `organizations` |Hiermee kunt u alleen gebruikers met een werk-of schoolaccounts van Azure Active Directory aan te melden bij de toepassing. |
| `consumers` |Hiermee kunnen alleen gebruikers met persoonlijke Microsoft-accounts (MSA) aan te melden bij de toepassing. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` |Hiermee kunt u alleen gebruikers met een werk-of schoolaccounts van een specifieke Azure Active Directory-tenant aan te melden bij de toepassing. Ofwel de beschrijvende domeinnaam van de Azure AD-tenant of guid-id van de tenant kan worden gebruikt. |

Kies een bepaalde app-type hieronder voor meer informatie over het communiceren met deze eindpunten.

## <a name="tokens"></a>Tokens

De implementatie v2.0 van OAuth 2.0 en OpenID Connect maken intensief gebruik van bearer-tokens, met inbegrip van weergegeven als JWTs bearer-tokens. Een bearer-token is een lichtgewicht beveiligingstoken die de "bearer" toegang tot een beveiligde bron verleent. In dit opzicht is de 'bearer' een partij die het token kan opleveren. Hoewel een partij moet eerst worden geverifieerd met Azure AD voor het ontvangen van het bearer-token, als de vereiste stappen zijn niet in gebruik voor het beveiligen van de token in overdracht en opslag, kan worden onderschept en die worden gebruikt door een onbedoelde partij. Hoewel sommige beveiligingstokens een ingebouwd mechanisme hebben voor zo wordt voorkomen dat onbevoegden gebruik ervan, wordt bearer-tokens geen dit mechanisme en moeten worden overgebracht in een beveiligd kanaal, zoals transport layer security (HTTPS). Als een bearer-token wordt verzonden in de wissen, kan een man-in de middelste aanval worden gebruikt door een schadelijke partij de token ophalen en deze gebruiken voor een niet-geautoriseerde toegang tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer op te slaan of bearer-tokens voor later gebruik opslaan in cache. Altijd voor zorgen dat uw app worden verzonden en bearer-tokens worden opgeslagen op een veilige manier. Zie voor meer beveiligingsoverwegingen op bearer-tokens, [RFC 6750 sectie 5](https://tools.ietf.org/html/rfc6750).

Meer informatie over de verschillende typen tokens die worden gebruikt in het v2.0-eindpunt is beschikbaar in [het v2.0-eindpunt-tokenverwijzing](v2-id-and-access-tokens.md).

## <a name="protocols"></a>Protocollen

Als u klaar om te zien van bepaalde voorbeeld-aanvragen bent, aan de slag met een van de onderstaande zelfstudies. Elke regel komt overeen met een bepaalde verificatiescenario. Als u hulp bij het vaststellen van de juiste stroom voor u nodig hebt, bekijk dan [de typen apps die u met het v2.0 maken kunt](v2-app-types.md).

* [Mobiele en systeemeigen toepassing met OAuth 2.0 bouwen](v2-oauth2-auth-code-flow.md)
* [Bouw WebApps met Open ID Connect](v2-protocols-oidc.md)
* [Apps van één pagina met het OAuth 2.0 impliciete stroom bouwen](v2-oauth2-implicit-grant-flow.md)
* [Daemons of processen op de server met het OAuth 2.0-Client referenties stromen bouwen](v2-oauth2-client-creds-grant-flow.md)
* [Tokens ophalen in een web-API met de OAuth 2.0-namens-stroom](v2-oauth2-on-behalf-of-flow.md)
