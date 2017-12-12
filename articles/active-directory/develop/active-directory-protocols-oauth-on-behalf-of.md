---
title: Azure AD Services auth met OAuth2.0 conceptspecificatie op-andere gebruikers-Of-| Microsoft Docs
description: In dit artikel wordt beschreven hoe het gebruik van HTTP-berichten voor het implementeren van services-verificatie met behulp van de OAuth2.0 op namens-stroom.
services: active-directory
documentationcenter: .net
author: navyasric
manager: mtillman
editor: 
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: bb3e01b1b8741253a459a41cfff27da558573551
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="service-to-service-calls-using-delegated-user-identity-in-the-on-behalf-of-flow"></a>Service naar serviceaanroepen met behulp van gedelegeerde gebruikersidentiteit in de On-namens-stroom
De OAuth 2.0 On-Behalf-Of stroom fungeert de gebruiksvoorbeeld waar een toepassing wordt aangeroepen met een service of web-API, die op zijn beurt moet aan te roepen op een andere service of web-API. Het idee is het doorgeven van de gedelegeerde gebruikersidentiteit en machtigingen via de aanvraagketen. Voor de middelste laag-service voor geverifieerde aanvragen naar de downstream-service maken, moet deze voor het beveiligen van een toegangstoken van Azure Active Directory (Azure AD), namens de gebruiker.

## <a name="on-behalf-of-flow-diagram"></a>Op-andere gebruikers-Of stroomdiagram
Wordt ervan uitgegaan dat de gebruiker is geverifieerd op een toepassing met behulp van de [autorisatiecodetoekenning OAuth 2.0](active-directory-protocols-oauth-code.md). De toepassing is op dit moment een toegangstoken (token A) met de claims van de gebruiker en toestemming voor toegang tot de web-API (A-API) voor de middelste laag. API A moet nu een geverifieerde aanvraag om aan te brengen de downstream web-API (API-B).

Welke stappen volgen vormen van de On-namens-stroom en met behulp van het volgende diagram worden uitgelegd.

![OAuth2.0 op namens-stroom](media/active-directory-protocols-oauth-on-behalf-of/active-directory-protocols-oauth-on-behalf-of-flow.png)


1. De clienttoepassing indient een aanvraag bij API A met het token A.
2. API-A met het eindpunt van de uitgifte van tokens Azure AD verifieert en vraagt een token voor toegang tot API B.
3. Het eindpunt van de uitgifte van tokens Azure AD API A van referenties met een token valideert en problemen met het toegangstoken voor de API-B (token B).
4. Het token B is ingesteld in de autorisatie-header van het verzoek om API B.
5. Gegevens van de beveiligde bron wordt geretourneerd door de API B.

## <a name="register-the-application-and-service-in-azure-ad"></a>Registreren van de toepassing en service in Azure AD
Registreer zowel de clienttoepassing en de middelste laag-service in Azure AD.
### <a name="register-the-middle-tier-service"></a>De service voor de middelste laag registreren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op de bovenste balk op je account en klikt u onder de **Directory** kiest u de Active Directory-tenant waar u wilt uw toepassing registreren.
3. Klik op **meer Services** in de nav linkerkant en kies **Azure Active Directory**.
4. Klik op **App registraties** en kies **registratie van de nieuwe toepassing**.
5. Geef een beschrijvende naam voor de toepassing en selecteer het toepassingstype. Op basis van de toepassing type set de aanmeldings-URL of Omleidings-URL naar de basis-URL. Klik op **maken** om de toepassing te maken.
6. Kies uw toepassing tijdens het nog steeds in de Azure portal en klik op **instellingen**. Kies in het menu instellingen **sleutels** en toevoegen van een sleutel - selecteert u een sleutel duur van 1 jaar of 2 jaar. Wanneer u deze pagina, de waarde van de sleutel wordt weergegeven, kopiëren en opslaan van de waarde in een veilige locatie - moet u deze sleutel later naar de toepassing configureren in uw implementatie - waarde van deze sleutel wordt niet meer weergegeven, noch worden opgehaald door een andere manier, dus neem record zodra deze is zichtbaar zijn vanaf de Azure-Portal.

### <a name="register-the-client-application"></a>De clienttoepassing registreren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op de bovenste balk op je account en klikt u onder de **Directory** kiest u de Active Directory-tenant waar u wilt uw toepassing registreren.
3. Klik op **meer Services** in de nav linkerkant en kies **Azure Active Directory**.
4. Klik op **App registraties** en kies **registratie van de nieuwe toepassing**.
5. Geef een beschrijvende naam voor de toepassing en selecteer het toepassingstype. Op basis van de toepassing type set de aanmeldings-URL of Omleidings-URL naar de basis-URL. Klik op **maken** om de toepassing te maken.
6. Machtigingen configureren voor uw toepassing - in het menu instellingen, kiest u de **vereist machtigingen** sectie, klikt u op **toevoegen**, klikt u vervolgens **selecteert u een API**, en typ de naam van de service voor de middelste laag in het tekstvak. Klik vervolgens op **Selecteer machtigingen** en selecteer ' toegang *servicenaam*'.

### <a name="configure-known-client-applications"></a>Bekende clienttoepassingen configureren
In dit scenario heeft de middelste laag-service geen tussenkomst van de gebruiker om op te halen van de gebruiker toestemming voor toegang tot de downstream-API. Daarom moet de optie om toegang te verlenen aan de downstream-API worden gepresenteerd tevoren als onderdeel van de toestemming stap tijdens de verificatie.
Om dit te bereiken, volg onderstaande stappen voor het binden expliciet de registratie van de clientapp in Azure AD met de registratie van de middelste laag-service, die de toestemming vereist door de client en de middelste laag in een dialoogvenster met één worden samengevoegd.
1. Navigeer naar de middelste laag serviceregistratie en klik op **Manifest** om de manifest-editor te openen.
2. Zoek in het manifest de `knownClientApplications` eigenschap matrix en de Client-ID van de clienttoepassing toevoegen als een element.
3. Het manifest opslaan door te klikken op de opslagbewerking knop.

## <a name="service-to-service-access-token-request"></a>Token serviceaanvraag access-service
Om aan te vragen een toegangstoken, moet u een HTTP POST naar de tenant-specifieke Azure AD-eindpunt met de volgende parameters.

```
https://login.microsoftonline.com/<tenant>/oauth2/token
```
Er zijn twee gevallen, afhankelijk van of de clienttoepassing kiest om te worden beveiligd door een gedeeld geheim of een certificaat.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>Het eerste aanvraagnummer: aanvraag voor toegang tot token met een gedeeld geheim
Wanneer u een gedeeld geheim, bevat een tokenaanvraag voor service-naar-service toegang tot de volgende parameters:

| Parameter |  | Beschrijving |
| --- | --- | --- |
| grant_type |Vereist | Het type van de tokenaanvraag. Voor een aanvraag met behulp van een JWT, moet de waarde **urn: ietf:params:oauth:grant-type: jwt-bearer**. |
| Verklaring |Vereist | De waarde van het token dat wordt gebruikt in de aanvraag. |
| client_id |Vereist | De App-ID is toegewezen aan de service aanroepen tijdens de registratie met Azure AD. U kunt de App-ID vinden in de Azure-beheerportal op **Active Directory**, klikt u op de map en klik op de naam van de toepassing. |
| client_secret |Vereist | De sleutel wordt geregistreerd voor de service aanroepen in Azure AD. Deze waarde moet zijn vermeld op het moment van inschrijving. |
| Bron |Vereist | De App ID URI van de ontvangende service (beveiligde resource). Klik op de App ID URI, informatie in de Azure-beheerportal **Active Directory**, klikt u op de map, klik op de toepassingsnaam, **alle instellingen** en klik vervolgens op **eigenschappen**. |
| requested_token_use |Vereist | Hiermee geeft u op hoe de aanvraag moet worden verwerkt. In de On-namens-stroom, de waarde moet **on_behalf_of**. |
| Bereik |Vereist | Een spatie gescheiden lijst met bereiken voor de tokenaanvraag. Voor het OpenID Connect, het bereik **openid** moet worden opgegeven.|

#### <a name="example"></a>Voorbeeld
De volgende HTTP POST-aanvragen een toegangstoken voor de https://graph.windows.net web-API. De `client_id` de service identificeert die het toegangstoken aanvragen.

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

### <a name="second-case-access-token-request-with-a-certificate"></a>Tweede geval: aanvraag voor toegang tot token met een certificaat
Een service-naar-service toegang tokenaanvraag met een certificaat bevat de volgende parameters:

| Parameter |  | Beschrijving |
| --- | --- | --- |
| grant_type |Vereist | Het type van de tokenaanvraag. Voor een aanvraag met behulp van een JWT, moet de waarde **urn: ietf:params:oauth:grant-type: jwt-bearer**. |
| Verklaring |Vereist | De waarde van het token dat wordt gebruikt in de aanvraag. |
| client_id |Vereist | De App-ID is toegewezen aan de service aanroepen tijdens de registratie met Azure AD. U kunt de App-ID vinden in de Azure-beheerportal op **Active Directory**, klikt u op de map en klik op de naam van de toepassing. |
| client_assertion_type |Vereist |De waarde moet liggen`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Vereist | Een bewering (een JSON Web Token) die u nodig hebt voor het maken en te ondertekenen met het certificaat u geregistreerd als referenties voor uw toepassing.  Meer informatie over [referenties van het certificaat](active-directory-certificate-credentials.md) voor informatie over het registreren van uw certificaat en de indeling van de bevestiging.|
| Bron |Vereist | De App ID URI van de ontvangende service (beveiligde resource). Klik op de App ID URI, informatie in de Azure-beheerportal **Active Directory**, klikt u op de map, klik op de toepassingsnaam, **alle instellingen** en klik vervolgens op **eigenschappen**. |
| requested_token_use |Vereist | Hiermee geeft u op hoe de aanvraag moet worden verwerkt. In de On-namens-stroom, de waarde moet **on_behalf_of**. |
| Bereik |Vereist | Een spatie gescheiden lijst met bereiken voor de tokenaanvraag. Voor het OpenID Connect, het bereik **openid** moet worden opgegeven.|

De parameters zijn bijna hetzelfde is in het geval van de aanvraag door een gedeeld geheim, behalve dat de parameter client_secret wordt vervangen door twee parameters: een client_assertion_type en client_assertion.

#### <a name="example"></a>Voorbeeld
De volgende HTTP POST-aanvragen een toegangstoken voor de web-https://graph.windows.net API met een certificaat. De `client_id` de service identificeert die het toegangstoken aanvragen.

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

## <a name="service-to-service-access-token-response"></a>Antwoord token service-access-service
Een geslaagde reactie is een JSON OAuth 2.0-antwoord met de volgende parameters.

| Parameter | Beschrijving |
| --- | --- |
| token_type |Geeft de waarde van het type token. Het enige type dat ondersteunt Azure AD is **Bearer**. Zie voor meer informatie over bearer-tokens, de [OAuth 2.0 autorisatie Framework: Bearer-Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| Bereik |Het bereik van toegang verleend in het token. |
| expires_in |Hoe lang het toegangstoken is ongeldig (in seconden). |
| expires_on |De tijd wanneer het toegangstoken is verlopen. De datum die wordt weergegeven als het aantal seconden van 1970-01-01T0:0:0Z UTC totdat de verlooptijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache. |
| Bron |De App ID URI van de ontvangende service (beveiligde resource). |
| access_token |Het aangevraagde toegangstoken. De aanroepende service kunt u dit token gebruiken om de ontvangende service te verifiëren. |
| id_token |De aangevraagde id-token. De service aanroepen kunt deze gebruiken om te controleren van de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. |
| refresh_token |Het vernieuwingstoken voor het aangevraagde toegangstoken. De aanroepende service kunt u dit token gebruiken om aan te vragen van een andere toegangstoken nadat het huidige toegangstoken is verlopen. |

### <a name="success-response-example"></a>Geslaagd antwoord voorbeeld
Het volgende voorbeeld toont een geslaagde reactie op een aanvraag voor een toegangstoken voor de https://graph.windows.net web-API.

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

### <a name="error-response-example"></a>Fout antwoord voorbeeld
Reactie op een fout is geretourneerd door Azure AD-tokeneindpunt als bij het verkrijgen van een toegangstoken voor de downstream-API als de downstream-API een beleid voor voorwaardelijke toegang zoals multi-factor authentication-server erop is ingesteld heeft. De service voor de middelste laag moet deze fout aan de clienttoepassing surface, zodat de clienttoepassing de interactie van de gebruiker om te voldoen aan het beleid voor voorwaardelijke toegang kunt bieden.

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
Nu de middelste laag-service kan het token verkregen hierboven geverifieerde aanvragen door het instellen van het token in aanbrengen de downstream web-API, gebruiken de `Authorization` header.

### <a name="example"></a>Voorbeeld
```
GET /me?api-version=2013-11-08 HTTP/1.1
Host: graph.windows.net
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het OAuth 2.0-protocol en een andere manier om uit te voeren services auth met clientreferenties.
* [Service-verificatie met OAuth 2.0-client referenties grant in Azure AD-service](active-directory-protocols-oauth-service-to-service.md)
* [OAuth 2.0 in Azure AD](active-directory-protocols-oauth-code.md)
