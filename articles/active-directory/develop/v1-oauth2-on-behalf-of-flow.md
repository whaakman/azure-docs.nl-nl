---
title: Service-to-service verificatie met Azure AD met behulp van OAuth 2.0-conceptspecificatie op-andere gebruikers-Of-| Microsoft Docs
description: In dit artikel wordt beschreven hoe u gebruik van HTTP-berichten voor het implementeren van service-to-service-verificatie met behulp van de OAuth 2.0 op namens-stroom.
services: active-directory
documentationcenter: .net
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: celested
ms.reviewer: hirsin, nacanuma
ms.custom: aaddev
ms.openlocfilehash: a231b79bebd9684281edea48dfe7cf5f57ccdacb
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986012"
---
# <a name="service-to-service-calls-using-delegated-user-identity-in-the-on-behalf-of-flow"></a>Service-serviceaanroepen met behulp van gedelegeerde gebruikersidentiteit in de On-Behalf-Of-stroom

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

De OAuth 2.0 namens (OBO) stroom fungeert de use-case waar een toepassing wordt aangeroepen met een service of web-API, dat op zijn beurt vereist is voor het aanroepen van een andere service of web-API. Het idee is dat de gemachtigde gebruiker identiteits- en machtigingen via de aanvraagketen doorgegeven. Voor de middelste laag service voor geverifieerde aanvragen versturen naar de downstream-service, moet deze voor het beveiligen van een toegangstoken van Azure Active Directory (Azure AD), namens de gebruiker.

> [!IMPORTANT]
> Vanaf mei 2018, een `id_token` kan niet worden gebruikt voor de stroom op-andere gebruikers-Of - kuuroorden moeten slagen voor een **toegang** token gebruikt voor een middelste laag vertrouwelijke client om uit te voeren OBO stromen. Zie [beperkingen](#client-limitations) voor meer informatie waarop clients op-andere gebruikers-Of aanroepen kunnen uitvoeren.

## <a name="on-behalf-of-flow-diagram"></a>On-Behalf-Of stroomdiagram
Wordt ervan uitgegaan dat de gebruiker is geverifieerd op een toepassing met behulp van de [OAuth 2.0-autorisatiecode verlenen stroom](v1-protocols-oauth-code.md). De toepassing is op dit moment een toegangstoken (token A) met claims van de gebruiker en toestemming voor toegang tot de middelste laag web-API (A-API). Nu moet API A maken van een geverifieerde aanvraag naar de downstream web-API (API-B).

De volgende stappen de stroom op-andere gebruikers-Of vormen en met behulp van het volgende diagram worden uitgelegd.

![OAuth 2.0 op namens-stroom](./media/v1-oauth2-on-behalf-of-flow/active-directory-protocols-oauth-on-behalf-of-flow.png)


1. De clienttoepassing doet een aanvraag bij API A met het token A.
2. API A wordt geverifieerd op het eindpunt van de Azure AD-token-uitgifte en vraagt een token voor toegang tot API B.
3. Het eindpunt van de Azure AD-token-uitgifte van de A-API-referenties met een token valideert en problemen met het toegangstoken voor API-B (token B).
4. De B-token is ingesteld in de autorisatie-header van de aanvraag voor API B.
5. Gegevens van de beveiligde resource wordt geretourneerd door API B.

>[!NOTE]
>De claim doelgroep in een toegangstoken dat wordt gebruikt voor het aanvragen van een token voor een downstream-service moet de id van de service die de aanvraag OBO en het token moet worden ondertekend met de Azure Active Directory algemene ondertekeningssleutel (dit is de standaardinstelling voor toepassingen die zijn geregistreerd via **App-registraties** in de portal)

## <a name="register-the-application-and-service-in-azure-ad"></a>De toepassing en de service in Azure AD registreren
Zowel de clienttoepassing en de middelste laag service registreren in Azure AD.
### <a name="register-the-middle-tier-service"></a>De middelste laag service registreren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op de bovenste balk van uw account en klikt u onder de **Directory** kiest u de Active Directory-tenant waar u wilt registreren van uw toepassing.
3. Klik op **meer Services** in het navigatievenster aan de linkerkant, en kies **Azure Active Directory**.
4. Klik op **App-registraties** en kies **nieuwe toepassing registreren**.
5. Geef een beschrijvende naam voor de toepassing en selecteer het toepassingstype. Op basis van het type toepassing set de aanmeldings-URL- of Omleidings-URL aan de basis-URL. Klik op **maken** om de toepassing te maken.
6. Terwijl u nog in de Azure-portal, kiest u uw toepassing en klik op **instellingen**. Kies in het menu instellingen **sleutels** en voeg een sleutel - Selecteer een duur van de sleutel van 1 jaar of twee jaar. Wanneer u deze pagina opslaat, waarde van de sleutel wordt weergegeven, kopiëren en opslaan van de waarde in een veilige locatie - moet u deze sleutel later naar de toepassingsinstellingen configureren in uw implementatie - waarde van deze sleutel, worden niet opnieuw wordt weergegeven, noch worden opgehaald door een andere manier , dus neem vastleggen zodra deze zichtbaar zijn vanaf de Azure-Portal is.

### <a name="register-the-client-application"></a>De clienttoepassing registreren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op de bovenste balk van uw account en klikt u onder de **Directory** kiest u de Active Directory-tenant waar u wilt registreren van uw toepassing.
3. Klik op **meer Services** in het navigatievenster aan de linkerkant, en kies **Azure Active Directory**.
4. Klik op **App-registraties** en kies **nieuwe toepassing registreren**.
5. Geef een beschrijvende naam voor de toepassing en selecteer het toepassingstype. Op basis van het type toepassing set de aanmeldings-URL- of Omleidings-URL aan de basis-URL. Klik op **maken** om de toepassing te maken.
6. Machtigingen configureren voor uw toepassing - in het menu instellingen, kiest u de **vereiste machtigingen** sectie, klikt u op **toevoegen**, klikt u vervolgens **Select an API**, en typ de naam van de middelste laag service in het tekstvak. Klik vervolgens op **Selecteer machtigingen** en selecteer ' toegang *servicenaam*'.

### <a name="configure-known-client-applications"></a>Bekende clienttoepassingen configureren
In dit scenario heeft de middelste laag service geen tussenkomst van de gebruiker om op te halen van de gebruiker toestemming voor toegang tot de downstream-API. Daarom moet de optie om toegang te verlenen aan de downstream-API worden gepresenteerd vooraf als onderdeel van de toestemming stap tijdens de verificatie.
Om dit te bereiken, volgt u de stappen hieronder om expliciet verbindt de clientapp-registratie in Azure AD met de registratie van de middelste laag service, die de toestemming vereist door de client en de middelste laag in een enkel dialoogvenster worden samengevoegd.
1. Navigeer naar de registratie van de middelste laag service, en klik op **Manifest** om het manifest editor te openen.
2. Zoek in het manifest, het `knownClientApplications` eigenschap matrix, en de Client-ID van de clienttoepassing toevoegen als een element.
3. Het manifest opslaan door te klikken op de opslagbewerking knop.

## <a name="service-to-service-access-token-request"></a>Service aan service-toegangstokenaanvraag
Een HTTP POST voor het aanvragen van een toegangstoken maken voor de tenant-specifieke Azure AD-eindpunt met de volgende parameters.

```
https://login.microsoftonline.com/<tenant>/oauth2/token
```
Er zijn twee mogelijke situaties, afhankelijk van of de clienttoepassing moet worden beveiligd door een gedeeld geheim, of een certificaat kiest.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>Eerste geval: aanvraag voor een toegangstoken met een gedeeld geheim
Wanneer u een gedeeld geheim, bevat een tokenaanvraag voor de service-naar-service toegang tot de volgende parameters:

| Parameter |  | Beschrijving |
| --- | --- | --- |
| grant_type |Vereist | Het type van het token aan te vragen. Omdat een OBO-aanvraag maakt gebruik van een JWT-toegangstoken, de waarde moet **urn: ietf:params:oauth:grant-type: jwt-bearer**. |
| bevestiging |Vereist | De waarde van het toegangstoken wordt gebruikt in de aanvraag. |
| client_id |Vereist | De App-ID die wordt toegewezen aan de aanroepende service tijdens de registratie met Azure AD. U kunt de App-ID vinden in de Azure Management Portal op **Active Directory**, klikt u op de map en klik vervolgens op de naam van de toepassing. |
| client_secret |Vereist | De sleutel geregistreerd voor de aanroepende service in Azure AD. Deze waarde moet zijn vermeld op het moment van inschrijving. |
| Bron |Vereist | De App-ID-URI van de ontvangende service (beveiligde resource). U kunt de URI van de App-ID vinden in de Azure Management Portal op **Active Directory**, klikt u op de map, klikt u op de naam van de toepassing, klikt u op **alle instellingen** en klik vervolgens op **eigenschappen**. |
| requested_token_use |Vereist | Hiermee geeft u op hoe de aanvraag moet worden verwerkt. In de stroom op-andere gebruikers-Of de waarde moet **on_behalf_of**. |
| scope |Vereist | Een spatie gescheiden lijst met bereiken voor het token aan te vragen. Voor de OpenID Connect, het bereik **openid** moet worden opgegeven.|

#### <a name="example"></a>Voorbeeld
De volgende HTTP-POST vraagt een toegangstoken voor de https://graph.windows.net web-API. De `client_id` identificeert de service die door het toegangstoken worden aangevraagd.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_secret=0Y1W%2BY3yYb3d9N8vSjvm8WrGzVZaAaHbHHcGbcgG%2BoI%3D
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

### <a name="second-case-access-token-request-with-a-certificate"></a>Tweede geval: aanvraag voor een toegangstoken met een certificaat
Een service-naar-service toegangstokenaanvraag met een certificaat bevat de volgende parameters:

| Parameter |  | Beschrijving |
| --- | --- | --- |
| grant_type |Vereist | Het type van het token aan te vragen. Omdat een OBO-aanvraag maakt gebruik van een JWT-toegangstoken, de waarde moet **urn: ietf:params:oauth:grant-type: jwt-bearer**. |
| bevestiging |Vereist | De waarde van het token wordt gebruikt in de aanvraag. |
| client_id |Vereist | De App-ID die wordt toegewezen aan de aanroepende service tijdens de registratie met Azure AD. U kunt de App-ID vinden in de Azure Management Portal op **Active Directory**, klikt u op de map en klik vervolgens op de naam van de toepassing. |
| client_assertion_type |Vereist |De waarde moet liggen `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Vereist | (Een JSON Web Token) een bewering die u wilt maken en te ondertekenen met het certificaat dat u geregistreerd als referenties voor uw toepassing. Meer informatie over [referenties van het certificaat](active-directory-certificate-credentials.md) voor informatie over het registreren van uw certificaat en de indeling van de verklaring.|
| Bron |Vereist | De App-ID-URI van de ontvangende service (beveiligde resource). U kunt de URI van de App-ID vinden in de Azure Management Portal op **Active Directory**, klikt u op de map, klikt u op de naam van de toepassing, klikt u op **alle instellingen** en klik vervolgens op **eigenschappen**. |
| requested_token_use |Vereist | Hiermee geeft u op hoe de aanvraag moet worden verwerkt. In de stroom op-andere gebruikers-Of de waarde moet **on_behalf_of**. |
| scope |Vereist | Een spatie gescheiden lijst met bereiken voor het token aan te vragen. Voor de OpenID Connect, het bereik **openid** moet worden opgegeven.|

U ziet dat de parameters bijna hetzelfde als in het geval van de aanvraag van het gedeelde geheim zijn, behalve dat de waarde voor client_secret-parameter is vervangen door twee parameters: een client_assertion_type en client_assertion.

#### <a name="example"></a>Voorbeeld
De volgende HTTP-POST vraagt een toegangstoken voor de https://graph.windows.net web-API met een certificaat. De `client_id` identificeert de service die door het toegangstoken worden aangevraagd.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

## <a name="service-to-service-access-token-response"></a>Service aan service access token antwoord
Een geslaagde reactie is een JSON OAuth 2.0-antwoord met de volgende parameters.

| Parameter | Beschrijving |
| --- | --- |
| token_type |Geeft aan dat de waarde van het token. Het enige type dat Azure AD ondersteunt **Bearer**. Zie voor meer informatie over bearer-tokens, de [OAuth 2.0 machtiging Framework: Bearer Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| scope |Het bereik van de toegang is verleend in het token. |
| expires_in |De hoeveelheid tijd die het toegangstoken ongeldig (in seconden is). |
| expires_on |De tijd wanneer het toegangstoken is verlopen. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC tot de vervaltijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache. |
| Bron |De App-ID-URI van de ontvangende service (beveiligde resource). |
| access_token |Het aangevraagde toegangstoken. De aanroepende service kunt u dit token gebruiken om te verifiëren bij de ontvangende service. |
| id_token |De aangevraagde id-token. De aanroepende service kunt dit gebruiken om te controleren of de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. |
| refresh_token |Het vernieuwingstoken dat voor de aangevraagde toegangstoken. De aanroepende service kunt u dit token gebruiken om aan te vragen van een andere toegangstoken nadat het huidige toegangstoken is verlopen. |

### <a name="success-response-example"></a>Voorbeeld van de reactie geslaagd
Het volgende voorbeeld toont een geslaagd antwoord op een verzoek om een token voor de https://graph.windows.net web-API.

```
{
    "token_type":"Bearer",
    "scope":"User.Read",
    "expires_in":"43482",
    "ext_expires_in":"302683",
    "expires_on":"1493466951",
    "not_before":"1493423168",
    "resource":"https://graph.windows.net",
    "access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ",
    "refresh_token":"AQABAAAAAABnfiG-mA6NTae7CdWW7QfdjKGu9-t1scy_TDEmLi4eLQMjJGt_nAoVu6A4oSu1KsRiz8XyQIPKQxSGfbf2FoSK-hm2K8TYzbJuswYusQpJaHUQnSqEvdaCeFuqXHBv84wjFhuanzF9dQZB_Ng5za9xKlUENrNtlq9XuLNVKzxEyeUM7JyxzdY7JiEphWImwgOYf6II316d0Z6-H3oYsFezf4Xsjz-MOBYEov0P64UaB5nJMvDyApV-NWpgklLASfNoSPGb67Bc02aFRZrm4kLk-xTl6eKE6hSo0XU2z2t70stFJDxvNQobnvNHrAmBaHWPAcC3FGwFnBOojpZB2tzG1gLEbmdROVDp8kHEYAwnRK947Py12fJNKExUdN0njmXrKxNZ_fEM33LHW1Tf4kMX_GvNmbWHtBnIyG0w5emb-b54ef5AwV5_tGUeivTCCysgucEc-S7G8Cz0xNJ_BOiM_4bAv9iFmrm9STkltpz0-Tftg8WKmaJiC0xXj6uTf4ZkX79mJJIuuM7XP4ARIcLpkktyg2Iym9jcZqymRkGH2Rm9sxBwC4eeZXM7M5a7TJ-5CqOdfuE3sBPq40RdEWMFLcrAzFvP0VDR8NKHIrPR1AcUruat9DETmTNJukdlJN3O41nWdZOVoJM-uKN3uz2wQ2Ld1z0Mb9_6YfMox9KTJNzRzcL52r4V_y3kB6ekaOZ9wQ3HxGBQ4zFt-2U0mSszIAA",
    "id_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC8yNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IvIiwiaWF0IjoxNDkzNDIzMTY4LCJuYmYiOjE0OTM0MjMxNjgsImV4cCI6MTQ5MzQ2Njk1MSwiYW1yIjpbInB3ZCJdLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXRpIjoieEN3ZnpoYS1QMFdKUU9MeENHZ0tBQSIsInZlciI6IjEuMCJ9."
}
```

### <a name="error-response-example"></a>Foutvoorbeeld antwoord
Een foutbericht wordt geretourneerd door Azure AD-token-eindpunt bij het verkrijgen van een toegangstoken voor de downstream-API, als de downstream-API een beleid voor voorwaardelijke toegang zoals multi-factor authentication instellen op het heeft. De middelste laag service moet deze fout naar de clienttoepassing surface, zodat de clienttoepassing kan de tussenkomst van de gebruiker om te voldoen aan het beleid voor voorwaardelijke toegang verlenen.

```
{
    "error":"interaction_required",
    "error_description":"AADSTS50079: Due to a configuration change made by your administrator, or because you moved to a new location, you must enroll in multi-factor authentication to access 'bf8d80f9-9098-4972-b203-500f535113b1'.\r\nTrace ID: b72a68c3-0926-4b8e-bc35-3150069c2800\r\nCorrelation ID: 73d656cf-54b1-4eb2-b429-26d8165a52d7\r\nTimestamp: 2017-05-01 22:43:20Z",
    "error_codes":[50079],
    "timestamp":"2017-05-01 22:43:20Z",
    "trace_id":"b72a68c3-0926-4b8e-bc35-3150069c2800",
    "correlation_id":"73d656cf-54b1-4eb2-b429-26d8165a52d7",
    "claims":"{\"access_token\":{\"polids\":{\"essential\":true,\"values\":[\"9ab03e19-ed42-4168-b6b7-7001fb3e933a\"]}}}"
}
```

## <a name="use-the-access-token-to-access-the-secured-resource"></a>Gebruik het toegangstoken voor toegang tot de beveiligde bron
Nu de middelste laag service het token verkregen gebruiken kunt boven aan de geverifieerde aanvragen versturen naar de downstream web-API, door het instellen van het token in de `Authorization` header.

### <a name="example"></a>Voorbeeld
```
GET /me?api-version=2013-11-08 HTTP/1.1
Host: graph.windows.net
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ
```
## <a name="service-to-service-calls-using-a-saml-assertion-obtained-with-an-oauth20-on-behalf-of-flow"></a>Service-to-Service aanroepen met behulp van een SAML-verklaring verkregen met een OAuth 2.0 op namens-stroom

Sommige OAuth op basis van web services nodig voor toegang tot andere webservice-API's die SAML-asserties ondertekend in niet-interactieve stromen accepteren.  Azure Active Directory biedt een SAML-verklaring in reactie op een stroom op-andere gebruikers-of met een op SAML gebaseerde webservice als een doelbron. 

>[!NOTE] 
>Dit is een uitbreiding van de OAuth 2.0-namens-stroom waarmee op basis van OAuth2-toepassing met toegang tot web service API-eindpunten die SAML-tokens gebruiken die niet-standaard.  

>[!TIP]
>Als u een beveiligd SAML-webservice uit een front-endwebtoepassing aanroept, kunt u gewoon de API aanroepen en starten van een stroom normale interactieve verificatie dat door de gebruikers wordt gebruikt bestaande sessie.  U hoeft alleen te overwegen om een stroom OBO wanneer de aanroep van een service-to-service is vereist voor een SAML-token voor gebruikerscontext.

### <a name="obtain-a-saml-token-using-an-obo-request-with-a-shared-secret"></a>Een SAML-token met behulp van een OBO-aanvraag met een gedeeld geheim ophalen
Een service-naar-service-aanvraag voor het verkrijgen van een SAML-verklaring bevat de volgende parameters:

| Parameter |  | Beschrijving |
| --- | --- | --- |
| grant_type |Vereist | Het type van het token aan te vragen. Voor een aanvraag met behulp van een JWT, de waarde moet **urn: ietf:params:oauth:grant-type: jwt-bearer**. |
| bevestiging |Vereist | De waarde van het toegangstoken wordt gebruikt in de aanvraag.|
| client_id |Vereist | De App-ID die wordt toegewezen aan de aanroepende service tijdens de registratie met Azure AD. U kunt de App-ID vinden in de Azure Management Portal op **Active Directory**, klikt u op de map en klik vervolgens op de naam van de toepassing. |
| client_secret |Vereist | De sleutel geregistreerd voor de aanroepende service in Azure AD. Deze waarde moet zijn vermeld op het moment van inschrijving. |
| Bron |Vereist | De App-ID-URI van de ontvangende service (beveiligde resource). Dit is de resource die de doelgroep van het SAML-token.  U kunt de URI van de App-ID vinden in de Azure Management Portal op **Active Directory**, klikt u op de map, klikt u op de naam van de toepassing, klikt u op **alle instellingen** en klik vervolgens op **eigenschappen**. |
| requested_token_use |Vereist | Hiermee geeft u op hoe de aanvraag moet worden verwerkt. In de stroom op-andere gebruikers-Of de waarde moet **on_behalf_of**. |
| requested_token_type | Vereist | Hiermee geeft u het type token dat is aangevraagd.  De waarde kan zijn ' urn: ietf:params:oauth:token-type: saml2 "of" urn: ietf:params:oauth:token-type: saml1 ", afhankelijk van de vereisten van de bron waartoe toegang wordt verkregen. |


Het antwoord bevat een UTF8 en Base64url gecodeerd SAML token. 

SubjectConfirmationData voor een SAML-verklaring afkomstig is van een aanroep van OBO: als de doeltoepassing is een waarde van de ontvanger in SubjectConfirmationData vereist, wordt deze moet worden ingesteld als een niet-jokertekens antwoord-URL in de configuratie van de resource-toepassing.

Het knooppunt SubjectConfirmationData mag niet een kenmerk InResponseTo bevatten, omdat het geen deel uit van een SAML-antwoord.  De toepassing die is ontvangen van de SAML-token moet worden geaccepteerd van de SAML-verklaring zonder een kenmerk InResponseTo.

Toestemming: Als u wilt een SAML-token met gebruikersgegevens op een OAuth-stroom ontvangen, toestemming moet hebben gekregen.  Raadpleeg https://docs.microsoft.com/azure/active-directory/develop/v1-permissions-and-consent voor informatie over machtigingen en het verkrijgen van goedgekeurd door een beheerder.

### <a name="response-with-saml-assertion"></a>Antwoord met SAML-verklaring

| Parameter | Beschrijving |
| --- | --- |
| token_type |Geeft aan dat de waarde van het token. Het enige type dat Azure AD ondersteunt **Bearer**. Zie voor meer informatie over bearer-tokens, de [OAuth 2.0 machtiging Framework: Bearer Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| scope |Het bereik van de toegang is verleend in het token. |
| expires_in |De hoeveelheid tijd die het toegangstoken ongeldig (in seconden is). |
| expires_on |De tijd wanneer het toegangstoken is verlopen. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC tot de vervaltijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache. |
| Bron |De App-ID-URI van de ontvangende service (beveiligde resource). |
| access_token |De SAML-verklaring wordt geretourneerd in de access_token-parameter. |
| refresh_token |Het vernieuwingstoken. De aanroepende service kunt u dit token gebruiken om aan te vragen van een andere toegangstoken nadat de huidige SAML-verklaring is verlopen. |

token_type: Bearer expires_in:3296 ext_expires_in:0 expires_on:1529627844 resource:https://api.contoso.com access_token: <Saml assertion> issued_token_type:urn:ietf:params:oauth:token-type: saml2 refresh_token: <Refresh token>

## <a name="client-limitations"></a>Clientbeperkingen
Openbare clients met jokertekens antwoord-URL's niet gebruiken een `id_token` voor OBO stromen. Een vertrouwelijke client kan echter nog steeds inwisselen toegangstokens die zijn verkregen via de impliciete stroom, zelfs als de openbare-client heeft een jokerteken redirect die URI geregistreerd.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het OAuth 2.0-protocol en een andere manier voor het uitvoeren van service-to-service-verificatie met behulp van clientreferenties.
* [Serviceverificatie met behulp van OAuth 2.0-clientreferenties in Azure AD-service](v1-oauth2-client-creds-grant-flow.md)
* [OAuth 2.0 in Azure AD](v1-protocols-oauth-code.md)