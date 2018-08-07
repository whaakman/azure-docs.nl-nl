---
title: Verifiëren met Azure AD en ontvang een JWT-Token met behulp van OAuth 2.0
description: Voorbeeldcode waarin wordt getoond hoe om te verifiëren met Azure Active Directory met behulp van OAuth 2.0 voor toegang tot beveiligde webtoepassingen en web-API's in uw organisatie.
services: active-directory
author: rloutlaw
manager: angerobe
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: routlaw
ms.custom: aaddev
ms.openlocfilehash: eb26101229ad60abae7a8a84f8dfa496488e84ba
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579000"
---
# <a name="request-an-access-token-using-oauth-20-to-access-web-apis-and-applications-secured-by-azure-active-directory"></a>Aanvragen van een toegangstoken toegang tot web-API's en toepassingen beveiligd door Azure Active Directory met behulp van OAuth 2.0

In dit artikel laat zien hoe u een JSON Web Token (JWT) voor toegang tot resources die zijn beveiligd door Azure AD. Hierbij wordt ervan uitgegaan dat u hebt een [Autorisatietoken](/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code) van gebruiker verleende machtiging of via een [service-principal](/azure/active-directory/develop/active-directory-application-objects).

## <a name="build-the-request"></a>De aanvraag voor het samenstellen

Gebruik de volgende `POST` HTTP-aanvraag voor het ophalen van een JWT voor toegang tot een bepaalde toepassing of de API is beveiligd door Azure AD.

```http
POST https://{tenant}/oauth2/v2.0/token?client_id={client-id}
&scope={scope}
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh   
```

### <a name="request-headers"></a>Aanvraagheaders

De volgende headers zijn vereist:

|Aanvraagheader|Beschrijving|  
|--------------------|-----------------|  
| *Host:* | https://login.microsoftonline.com |
| *Content-Type:*| Application/x-www-form-urlencoded goedkeurt |
 

### <a name="uri-parameters"></a>URI-parameters

| Parameter     |                       | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                                                |
|---------------|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tenant        | vereist              | De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie zich bij de toepassing aanmelden kan. De toegestane waarden zijn `common`, `organizations`, `consumers`, en tenant-id's. Zie voor meer details [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints).                                                                                                                                                   |
| client_id     | vereist              | De aanvraag-ID die de portal voor wachtwoordregistratie ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uw app is toegewezen.                                                                                                                                                                                                                             |
| grant_type    | vereist              | Moet `authorization_code` voor de autorisatiecodestroom.                                                                                                                                                                                                                                                                                                                                                                            |
| scope         | vereist              | Een door spaties gescheiden lijst met bereiken. De bereiken die in dit gedeelte wordt aangevraagd moet gelijk aan of een subset van de bereiken in de aanroep naar `/authorize`. Als de bereiken die is opgegeven in deze aanvraag meerdere resource-server omvatten, wordt een token voor de resource die is opgegeven in de eerste scope geretourneerd door het v2.0-eindpunt. Raadpleeg voor een meer gedetailleerde uitleg van bereiken, [machtigingen en toestemming scopes](v2-permissions-and-consent.md). |
| Code          | vereist              | De authorization_code die u hebt verkregen in de aanroep naar `/authorize`                                                                                                                                                                                                                                                                                                                                                                |
| redirect_uri  | vereist              | Dezelfde redirect_uri-waarde die is gebruikt voor het verkrijgen van de authorization_code.                                                                                                                                                                                                                                                                                                                                                             |
| client_secret | vereist voor web-apps | Het toepassingsgeheim die u hebt gemaakt in de portal voor app-registratie voor uw app. Gebruik niet in een systeemeigen app omdat client_secrets op betrouwbare wijze kunnen niet worden opgeslagen op apparaten. Dit is vereist voor de web-apps en web-API's, waarvoor de mogelijkheid om op te slaan de waarde voor client_secret veilig op de server.  Client-geheimen moet URL gecodeerd voordat het wordt verzonden.                                                                                 |
| code_verifier | optioneel              | De dezelfde code_verifier die is gebruikt voor het verkrijgen van de authorization_code. Vereist als PKCE is gebruikt in de autorisatieaanvraag voor het verlenen van code. Zie voor meer informatie de [PKCE RFC](https://tools.ietf.org/html/rfc7636)                                                                                                                                                                                                                                                                                             |
## <a name="handle-the-response"></a>Het antwoord verwerkt

Een geslaagde respons van de token bevat een JWT-token en wordt er als volgt uitzien:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter     | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| access_token  | Het aangevraagde toegangstoken. De app kan dit token gebruikt voor verificatie bij de beveiligde resource, zoals een web-API.                                                                                                                                                                                                                                                                                                                                    |
| token_type    | Geeft aan dat de waarde van het token. Het enige type die ondersteuning biedt voor Azure AD is Bearer                                                                                                                                                                                                                                                                                                                                                                           |
| expires_in    | Hoe lang het toegangstoken is ongeldig (in seconden).                                                                                                                                                                                                                                                                                                                                                                                                       |
| scope         | De bereiken die de access_token is geldig voor.                                                                                                                                                                                                                                                                                                                                                                                                         |
| refresh_token | Een vernieuwingstoken OAuth 2.0. De app kan dit token gebruiken als u meer toegangstokens verkrijgen nadat het huidige toegangstoken is verlopen. Refresh_tokens worden lange levensduur hebben en kan worden gebruikt voor toegang tot resources behouden gedurende langere tijd wordt opgelost. Voor meer informatie raadpleegt u de [v2.0 tokenverwijzing](v2-id-and-access-tokens.md). <br> **Opmerking:** alleen opgegeven als `offline_access` bereik is aangevraagd.                                               |
| id_token      | Een niet-ondertekende JSON Web Token (JWT). De app kan worden gedecodeerd de segmenten van dit token informatie opvragen over de gebruiker die zijn aangemeld. De app kan de waarden in de cache en deze weer te geven, maar deze moet niet gebruiken voor autorisatie of grenzen voor netwerkbeveiliging. Zie voor meer informatie over id_tokens, de [v2.0-eindpunt tokenverwijzing](v2-id-and-access-tokens.md). <br> **Opmerking:** alleen opgegeven als `openid` bereik is aangevraagd. |



