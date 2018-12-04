---
title: Gebruik Azure AD v2.0 aan te melden bij de gebruikers met behulp van ROPC | Microsoft Docs
description: Ondersteuning voor browser zonder verificatie van stromen met behulp van de resource-eigenaar wachtwoord-referentietoekenning.
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
ms.topic: article
ms.date: 11/28/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 4eb6850e4b6e267e0b4ef83f7639e90308cee989
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52841435"
---
# <a name="azure-active-directory-v20-and-the-oauth-20-resource-owner-password-credential"></a>Azure Active Directory v2.0 en de wachtwoordreferenties van OAuth 2.0-resource-eigenaar

Azure Active Directory (Azure AD) ondersteunt de [wachtwoordreferenties van resource-eigenaar (ROPC) verlenen](https://tools.ietf.org/html/rfc6749#section-4.3), waarmee een toepassing aan te melden bij de gebruikers hun wachtwoord direct kan verwerken. De stroom ROPC vereist een hoge mate van blootstelling van vertrouwen en de gebruiker en ontwikkelaars gebruik deze stroom alleen wanneer de andere, beter te beveiligen, stromen kunnen niet worden gebruikt.

> [!Important]
> * De Azure AD v2.0-eindpunt biedt alleen ondersteuning voor ROPC voor Azure AD-tenants, geen persoonlijke accounts. Dit betekent dat u een tenant-specifieke eindpunt moet gebruiken (`https://login.microsoftonline.com/{TenantId_or_Name}`) of de `organizations` eindpunt.
> * Persoonlijke accounts die worden uitgenodigd voor een Azure AD-tenant niet ROPC gebruiken.
> * Accounts waarvoor geen wachtwoorden kunnen niet aanmelden via ROPC. Voor dit scenario raden wij aan dat u een andere stroom voor uw app in plaats daarvan.
> * Als gebruikers multi-factor authentication (MFA) gebruiken moeten voor aanmelding bij de toepassing, wordt deze in plaats daarvan geblokkeerd.

## <a name="protocol-diagram"></a>Protocol-diagram

Het volgende diagram toont de ROPC-stroom.

![ROPC stroom](media/v2-oauth2-ropc/v2-oauth-ropc.png)

## <a name="authorization-request"></a>Autorisatie-aanvraag

De stroom ROPC is één aanvraag&mdash;deze stuurt de client-id en de referenties van gebruiker aan de id-provider en ontvangt u vervolgens de tokens in. De client moet het e-mailadres (UPN) van de gebruiker en het wachtwoord voordat u aanvragen. De client moet direct na de aanvraag is gelukt, veilig vrijgeven van de referenties van de gebruiker uit het geheugen. Het moet nooit opslaan.

```
// Line breaks and spaces are for legibility only.

POST https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token?

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=user.read%20openid%20profile%20offline_access
&client_secret=wkubdywbc2894u
&username=MyUsername@myTenant.com
&password=SuperS3cret
&grant_type=password
```

| Parameter | Voorwaarde | Beschrijving |
| --- | --- | --- |
| `tenant` | Vereist | De directory-tenant die u wilt vastleggen van de gebruiker in. Dit kan zijn in de beschrijvende naamindeling of GUID. Deze parameter kan niet worden ingesteld op `common` of `consumers`, maar kan worden ingesteld op `organizations`. |
| `grant_type` | Vereist | Moet worden ingesteld op `password`. |
| `username` | Vereist | E-mailadres van de gebruiker. |
| `password` | Vereist | Wachtwoord van de gebruiker. |
| `scope` | Aanbevolen | Een door spaties gescheiden lijst van [scopes](v2-permissions-and-consent.md), of de machtigingen die de app nodig heeft. Deze bereiken moeten worden gegeven vooraf door een beheerder of door de gebruiker in een interactieve stroom. |

### <a name="successful-authentication-response"></a>Verificatie is geslaagd antwoord

Hier volgt een voorbeeld van een geslaagde respons token:

```json
{
    "token_type": "Bearer",
    "scope": "User.Read profile openid email",
    "expires_in": 3599,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD..."
}
```

| Parameter | Indeling | Beschrijving |
| --------- | ------ | ----------- |
| `token_type` | Reeks | Altijd ingesteld op `Bearer`. |
| `scope` | Tekenreeksen gescheiden door spaties | Als een toegangstoken is geretourneerd, zijn deze parameter de scopes die het toegangstoken is ongeldig voor. |
| `expires_in`| int | Het aantal seconden dat de opgenomen toegangstoken is ongeldig voor. |
| `access_token`| Ondoorzichtige tekenreeks | Uitgegeven voor de [scopes](v2-permissions-and-consent.md) die zijn aangevraagd. |
| `id_token` | JWT | Uitgegeven als de oorspronkelijke `scope` parameter opgenomen de `openid` bereik. |
| `refresh_token` | Ondoorzichtige tekenreeks | Uitgegeven als de oorspronkelijke `scope` parameter opgenomen `offline_access`. |

U kunt het vernieuwingstoken dat nieuwe toegangstokens verkrijgen en vernieuwen met behulp van dezelfde stroom wordt beschreven in de [OAuth Code flow-documentatie](v2-oauth2-auth-code-flow.md#refresh-the-access-token).

### <a name="error-response"></a>Foutbericht

Als de gebruiker de juiste gebruikersnaam of wachtwoord is niet opgegeven of de client nog niet de aangevraagde toestemming verkregen, mislukt de verificatie.

| Fout | Beschrijving | Clientactie |
|------ | ----------- | -------------|
| `invalid_grant` | De verificatie is mislukt | De referenties zijn onjuist of de client geen toestemming voor de aangevraagde bereiken. Als de scopes niet zijn verleend, een `consent_required` suberror wordt geretourneerd. Als dit het geval is, moet de client de gebruiker doorsturen naar een interactieve prompt met behulp van een webweergave of in de browser. |
| `invalid_request` | De aanvraag is niet goed samengesteld. | Het machtigingstype wordt niet ondersteund op de `/common` of `/consumers` verificatie contexten.  Gebruik `/organizations` in plaats daarvan. |
| `invalid_client` | De app is niet goed ingesteld | Dit kan gebeuren als de `allowPublicClient` eigenschap niet is ingesteld op ' True ' in de [toepassingsmanifest](reference-app-manifest.md). De `allowPublicClient` eigenschap is nodig omdat de toekenning ROPC beschikt niet over een omleidings-URI. Azure AD kan niet vaststellen of de app een openbare client-toepassing of een vertrouwelijke client-toepassing, tenzij de eigenschap is ingesteld. Houd er rekening mee dat ROPC wordt alleen ondersteund voor openbare client-apps. |

## <a name="learn-more"></a>Meer informatie

* ROPC uitproberen voor zelf met behulp van de [console voorbeeldtoepassing](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2).
* Om te bepalen of het v2.0-eindpunt moet worden gebruikt, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).
