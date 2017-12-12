---
title: Meer informatie over de autorisatie protocollen die worden ondersteund door Azure AD v2.0 | Microsoft Docs
description: Een handleiding voor protocollen die worden ondersteund door het Azure AD v2.0-eindpunt.
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 5fb4fa1b-8fc4-438e-b3b0-258d8c145f22
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ce9a7cb14b933da23873d69e1f14a744d012a858
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="v20-protocols---oauth-20--openid-connect"></a>v2.0-protocollen - OAuth 2.0 & OpenID Connect
Het v2.0-eindpunt kunt Azure AD voor identity-as-a-service met de protocollen volgens de industrienorm, OpenID Connect en OAuth 2.0 gebruiken.  De service is compatibel met de standaarden, kunnen er subtiele verschillen tussen de twee implementaties van deze protocollen.  Hier de informatie is nuttig als u uw code te schrijven wilt door rechtstreeks verzenden & afhandeling van HTTP-aanvragen of gebruik een 3rd partij open source bibliotheek in plaats van met een van onze open-source bibliotheken.
<!-- TODO: Need link to libraries above -->

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt.  Meer informatie over om te bepalen of moet u het v2.0-eindpunt, [v2.0 beperkingen](active-directory-v2-limitations.md).
>
>

## <a name="the-basics"></a>De basisbeginselen
In bijna alle OAuth & OpenID Connect stromen zijn er vier partijen die zijn betrokken bij de exchange:

![OAuth 2.0-functies](../../media/active-directory-v2-flows/protocols_roles.png)

* De **autorisatie Server** is het v2.0-eindpunt.  Het is verantwoordelijk voor de identiteit van de gebruiker waarborgen, verlenen en intrekken van toegang tot bronnen en uitgeven van tokens.  Het is ook bekend als de id-provider - niets te maken met gegevens van de gebruiker, de toegang en de vertrouwensrelaties tussen partijen in een stroom veilig worden verwerkt.
* De **Resource-eigenaar** is meestal de eindgebruiker.  Het is de partij die eigenaar is van de gegevens en de bevoegdheid om toe te staan van derden voor toegang tot die gegevens of de resource heeft.
* De **OAuth Client** wordt uw app, geïdentificeerd door de toepassings-id.  Meestal is dit de partij die de eindgebruiker met communiceert en het tokens aanvraagt van de autorisatie-server.  De client moet worden gemachtigd voor toegang tot de bron van de resource-eigenaar.
* De **bronserver** is waar de resource of de gegevens zich bevindt.  Deze vertrouwt de autorisatie-Server veilig verifiëren en autoriseren van de OAuth-Client en Bearer-access_tokens gebruikt om ervoor te zorgen dat toegang tot een bron kan worden toegekend.

## <a name="app-registration"></a>App-registratie
Elke app die gebruikmaakt van het v2.0-eindpunt moet worden geregistreerd bij [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) voordat deze kan communiceren met OAuth of OpenID Connect.  Het registratieproces van de app wordt verzamelen & enkele waarden toewijzen aan uw app:

* Een **toepassings-Id** die wordt aangeduid uw app
* Een **omleidings-URI** of **-pakket-id** die kunnen worden gebruikt om een directe antwoorden terug naar uw app
* Enkele andere scenariospecifieke waarden.

Zie [Een app registreren](active-directory-v2-app-registration.md) voor meer informatie.

## <a name="endpoints"></a>Eindpunten
Na de registratie wordt de status van de app communiceert met Azure AD door verzoeken te sturen naar het v2.0-eindpunt:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Waar de `{tenant}` kan duren voordat een van de vier verschillende waarden:

| Waarde | Beschrijving |
| --- | --- |
| `common` |Kunnen de gebruikers met persoonlijke Microsoft-accounts en werk/schoolaccounts in Azure Active Directory voor aanmelding bij de toepassing. |
| `organizations` |Hiermee kunt u alleen gebruikers met werk/schoolaccounts van Azure Active Directory voor aanmelding bij de toepassing. |
| `consumers` |Hiermee kunnen alleen gebruikers met persoonlijke Microsoft-accounts (MSA) als u zich aanmeldt bij de toepassing. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` |Hiermee kunt u alleen gebruikers met een werk/school-account van een bepaald Azure Active Directory-tenant voor aanmelding bij de toepassing.  Ofwel de aangepaste domeinnaam van de Azure AD-tenant of guid-id van de tenant kan worden gebruikt. |

Kies een bepaalde app-type hieronder voor meer informatie over hoe u met deze eindpunten te werken.

## <a name="tokens"></a>Tokens
De implementatie v2.0 van OAuth 2.0 en OpenID Connect maken intensief gebruik van bearer-tokens, weergegeven als JWTs bearer-tokens. Een bearer-token is een lichtgewicht beveiligingstoken die de 'bearer' toegang tot een beveiligde bron verleent. In dit opzicht is het 'bearer' een partij die het token kan opleveren. Hoewel een partij moet eerst worden geverifieerd met Azure AD voor het ontvangen van de bearer-token als de vereiste stappen niet genomen worden voor het beveiligen van het token in overdracht en opslag, kunt u het probleem is onderschept en gebruikt door een onbedoelde partij. Sommige beveiligingstokens beschikken over een ingebouwde mechanisme om te voorkomen dat onbevoegden kunnen gebruiken, worden de bearer-tokens hebben geen dit mechanisme en moeten worden overgebracht in een beveiligd kanaal zoals transport layer security (HTTPS). Als een bearer-token in het wissen wordt verzonden, kan een man-in de middelste aanval worden gebruikt door een kwaadwillende partij het token verkrijgen en deze gebruiken voor een niet-geautoriseerde toegang tot een beveiligde bron. De dezelfde beveiligings-principals van toepassing wanneer caching bearer-tokens voor later gebruik of op te slaan. Altijd voor zorgen dat uw app verzendt en bearer-tokens worden opgeslagen op een veilige manier. Zie voor meer beveiligingsoverwegingen op bearer-tokens [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).

Meer informatie over de verschillende typen tokens die worden gebruikt in het v2.0-eindpunt is beschikbaar in [het v2.0-eindpunt-tokenverwijzing](active-directory-v2-tokens.md).

## <a name="protocols"></a>Protocollen
Als u klaar bent om te zien van bepaalde voorbeeld-aanvragen, aan de slag met een van de onderstaande zelfstudies.  Elke komt overeen met een bepaalde verificatiescenario.  Als u hulp bij het bepalen dat de juiste stroom voor u nodig hebt, kijk dan eens [de typen apps kunt u met het v2.0](active-directory-v2-flows.md).

* [Mobiele en systeemeigen toepassing met OAuth 2.0 bouwen](active-directory-v2-protocols-oauth-code.md)
* [Maken van Web Apps met Open ID Connect](active-directory-v2-protocols-oidc.md)
* [Apps van één pagina met de impliciete OAuth 2.0-stroom maken](active-directory-v2-protocols-implicit.md)
* [Build Daemons of processen op de Server met de referenties van de OAuth 2.0-Client stromen](active-directory-v2-protocols-oauth-client-creds.md)
* [Ophalen van tokens in een Web-API met het OAuth 2.0 namens-stroom](active-directory-v2-protocols-oauth-on-behalf-of.md)
