---
title: Azure AD v2.0 OAuth 2.0 op namens-stroom | Microsoft Docs
description: In dit artikel wordt beschreven hoe u gebruik van HTTP-berichten voor het implementeren van service-to-service-verificatie met behulp van de OAuth 2.0 op namens-stroom.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: da13b7b7b9bd39692db422a315383e0f12aae453
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344873"
---
# <a name="azure-active-directory-v20-and-oauth-20-on-behalf-of-flow"></a>Azure Active Directory v2.0 en OAuth 2.0 namens-stroom
De OAuth 2.0 namens-stroom fungeert de use-case waar een toepassing wordt aangeroepen met een service of web-API, dat op zijn beurt vereist is voor het aanroepen van een andere service of web-API. Het idee is dat de gemachtigde gebruiker identiteits- en machtigingen via de aanvraagketen doorgegeven. Voor de middelste laag service voor geverifieerde aanvragen versturen naar de downstream-service, moet deze voor het beveiligen van een toegangstoken van Azure Active Directory (Azure AD), namens de gebruiker.

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen. Om te bepalen of het v2.0-eindpunt moet worden gebruikt, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).
>


> [!IMPORTANT]
> De [impliciete](v2-oauth2-implicit-grant-flow.md) kan niet worden gebruikt voor de stroom op-andere gebruikers-Of - kuuroorden hun toegangstoken (impliciete stroom) moeten doorgeven aan een vertrouwelijke client van de middelste laag om uit te voeren OBO stromen. Zie [beperkingen](#client-limitations) voor meer informatie waarop clients op-andere gebruikers-Of aanroepen kunnen uitvoeren.  

## <a name="protocol-diagram"></a>Protocol-diagram
Wordt ervan uitgegaan dat de gebruiker is geverifieerd op een toepassing met behulp van de [OAuth 2.0-autorisatiecode verlenen stroom](v2-oauth2-auth-code-flow.md). Op dit moment de toepassing heeft een toegangstoken *voor API A* (token A) met claims van de gebruiker en toestemming voor toegang tot de middelste laag web-API (A-API). Nu moet API A maken van een geverifieerde aanvraag naar de downstream web-API (API-B).

De volgende stappen de stroom op-andere gebruikers-Of vormen en met behulp van het volgende diagram worden uitgelegd.

![OAuth 2.0 op namens-stroom](./media/v1-oauth2-on-behalf-of-flow/active-directory-protocols-oauth-on-behalf-of-flow.png)


1. De clienttoepassing een aanvraag doet bij API A met het token A (met een `aud` claim van API-A).
2. API A wordt geverifieerd op het eindpunt van de Azure AD-token-uitgifte en vraagt een token voor toegang tot API B.
3. Het eindpunt van de Azure AD-token-uitgifte van de A-API-referenties met een token valideert en problemen met het toegangstoken voor API-B (token B).
4. De B-token is ingesteld in de autorisatie-header van de aanvraag voor API B.
5. Gegevens van de beveiligde resource wordt geretourneerd door API B.

> [!NOTE]
> In dit scenario heeft de middelste laag service geen tussenkomst van de gebruiker om op te halen van de gebruiker toestemming voor toegang tot de downstream-API. De optie voor het verlenen van toegang tot de downstream API daarom is vooraf verschijnt als een deel van de toestemming stap tijdens de verificatie is.
>

## <a name="service-to-service-access-token-request"></a>Service aan service-toegangstokenaanvraag
Een HTTP POST voor het aanvragen van een toegangstoken maken voor de tenant-specifieke Azure AD v2.0-eindpunt met de volgende parameters.

```
https://login.microsoftonline.com/<tenant>/oauth2/v2.0/token
```

Er zijn twee mogelijke situaties, afhankelijk van of de clienttoepassing moet worden beveiligd door een gedeeld geheim, of een certificaat kiest.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>Eerste geval: aanvraag voor een toegangstoken met een gedeeld geheim
Wanneer u een gedeeld geheim, bevat een tokenaanvraag voor de service-naar-service toegang tot de volgende parameters:

| Parameter |  | Beschrijving |
| --- | --- | --- |
| grant_type |vereist | Het type van het token aan te vragen. Voor een aanvraag met behulp van een JWT, de waarde moet **urn: ietf:params:oauth:grant-type: jwt-bearer**. |
| client_id |vereist | De aanvraag-ID die de [Portal voor Appregistratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) toegewezen aan uw app. |
| client_secret |vereist | Het geheim van de toepassing die u hebt gegenereerd voor uw app in de Portal voor Appregistratie. |
| bevestiging |vereist | De waarde van het token wordt gebruikt in de aanvraag. |
| scope |vereist | Een spatie gescheiden lijst met bereiken voor het token aan te vragen. Zie voor meer informatie, [scopes](v2-permissions-and-consent.md).|
| requested_token_use |vereist | Hiermee geeft u op hoe de aanvraag moet worden verwerkt. In de stroom op-andere gebruikers-Of de waarde moet **on_behalf_of**. |

#### <a name="example"></a>Voorbeeld
De volgende HTTP-POST vraagt een toegangstoken en een vernieuwingstoken met `user.read` voor het bereik van de https://graph.microsoft.com web-API.

```
//line breaks for legibility only

POST /oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer
&client_id=2846f71b-a7a4-4987-bab3-760035b2f389
&client_secret=BYyVnAt56JpLwUcyo47XODd
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3L3YyLjAiLCJpYXQiOjE0OTM5MjA5MTYsIm5iZiI6MTQ5MzkyMDkxNiwiZXhwIjoxNDkzOTI0ODE2LCJhaW8iOiJBU1FBMi84REFBQUFnZm8vNk9CR0NaaFV2NjJ6MFFYSEZKR0VVYUIwRUlIV3NhcGducndMMnVrPSIsIm5hbWUiOiJOYXZ5YSBDYW51bWFsbGEiLCJvaWQiOiJkNWU5NzljNy0zZDJkLTQyYWYtOGYzMC03MjdkZDRjMmQzODMiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJuYWNhbnVtYUBtaWNyb3NvZnQuY29tIiwic3ViIjoiZ1Q5a1FMN2hXRUpUUGg1OWJlX1l5dVZNRDFOTEdiREJFWFRhbEQzU3FZYyIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInV0aSI6IjN5U3F4UHJweUVPd0ZsTWFFMU1PQUEiLCJ2ZXIiOiIyLjAifQ.TPPJSvpNCSCyUeIiKQoLMixN1-M-Y5U0QxtxVkpepjyoWNG0i49YFAJC6ADdCs5nJXr6f-ozIRuaiPzy29yRUOdSz_8KqG42luCyC1c951HyeDgqUJSz91Ku150D9kP5B9-2R-jgCerD_VVuxXUdkuPFEl3VEADC_1qkGBiIg0AyLLbz7DTMp5DvmbC09DhrQQiouHQGFSk2TPmksqHm3-b3RgeNM1rJmpLThis2ZWBEIPx662pjxL6NJDmV08cPVIcGX4KkFo54Z3rfwiYg4YssiUc4w-w3NJUBQhnzfTl4_Mtq2d7cVlul9uDzras091vFy32tWkrpa970UvdVfQ
&scope=https://graph.microsoft.com/user.read+offline_access
&requested_token_use=on_behalf_of
```

### <a name="second-case-access-token-request-with-a-certificate"></a>Tweede geval: aanvraag voor een toegangstoken met een certificaat
Een service-naar-service toegangstokenaanvraag met een certificaat bevat de volgende parameters:

| Parameter |  | Beschrijving |
| --- | --- | --- |
| grant_type |vereist | Het type van het token aan te vragen. Voor een aanvraag met behulp van een JWT, de waarde moet **urn: ietf:params:oauth:grant-type: jwt-bearer**. |
| client_id |vereist | De aanvraag-ID die de [Portal voor Appregistratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) toegewezen aan uw app. |
| client_assertion_type |vereist |De waarde moet liggen `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |vereist | (Een JSON Web Token) een bewering die u wilt maken en te ondertekenen met het certificaat dat u geregistreerd als referenties voor uw toepassing. Meer informatie over [referenties van het certificaat](active-directory-certificate-credentials.md) voor informatie over het registreren van uw certificaat en de indeling van de verklaring.|
| bevestiging |vereist | De waarde van het token wordt gebruikt in de aanvraag. |
| requested_token_use |vereist | Hiermee geeft u op hoe de aanvraag moet worden verwerkt. In de stroom op-andere gebruikers-Of de waarde moet **on_behalf_of**. |
| scope |vereist | Een spatie gescheiden lijst met bereiken voor het token aan te vragen. Zie voor meer informatie, [scopes](v2-permissions-and-consent.md).|

U ziet dat de parameters bijna hetzelfde als in het geval van de aanvraag van het gedeelde geheim zijn, behalve dat de waarde voor client_secret-parameter is vervangen door twee parameters: een client_assertion_type en client_assertion.

#### <a name="example"></a>Voorbeeld
De volgende HTTP-POST vraagt een toegangstoken met `user.read` voor het bereik van de https://graph.microsoft.com web-API met een certificaat.

```
// line breaks for legibility only

POST /oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=https://graph.microsoft.com/user.read+offline_access
```

## <a name="service-to-service-access-token-response"></a>Service aan service access token antwoord
Een geslaagde reactie is een JSON OAuth 2.0-antwoord met de volgende parameters.

| Parameter | Beschrijving |
| --- | --- |
| token_type |Geeft aan dat de waarde van het token. Het enige type dat Azure AD ondersteunt **Bearer**. Zie voor meer informatie over bearer-tokens, de [OAuth 2.0 machtiging Framework: Bearer Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| scope |Het bereik van de toegang is verleend in het token. |
| expires_in |De hoeveelheid tijd die het toegangstoken ongeldig (in seconden is). |
| access_token |Het aangevraagde toegangstoken. De aanroepende service kunt u dit token gebruiken om te verifiëren bij de ontvangende service. |
| refresh_token |Het vernieuwingstoken dat voor de aangevraagde toegangstoken. De aanroepende service kunt u dit token gebruiken om aan te vragen van een andere toegangstoken nadat het huidige toegangstoken is verlopen. Het vernieuwingstoken dat is alleen beschikbaar als de `offline_access` bereik is aangevraagd.|

### <a name="success-response-example"></a>Voorbeeld van de reactie geslaagd
Het volgende voorbeeld toont een geslaagd antwoord op een verzoek om een token voor de https://graph.microsoft.com web-API.

```
{
  "token_type": "Bearer",
  "scope": "https://graph.microsoft.com/user.read",
  "expires_in": 3269,
  "ext_expires_in": 0,
  "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQTZOVGFlN0NkV1c3UWZkQ0NDYy0tY0hGa18wZE50MVEtc2loVzRMd2RwQVZISGpnTVdQZ0tQeVJIaGlDbUN2NkdyMEpmYmRfY1RmMUFxU21TcFJkVXVydVJqX3Nqd0JoN211eHlBQSIsImFsZyI6IlJTMjU2IiwieDV0IjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIiwia2lkIjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIn0.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaWF0IjoxNDkzOTMwMzA1LCJuYmYiOjE0OTM5MzAzMDUsImV4cCI6MTQ5MzkzMzg3NSwiYWNyIjoiMCIsImFpbyI6IkFTUUEyLzhEQUFBQU9KYnFFWlRNTnEyZFcxYXpKN1RZMDlYeDdOT29EMkJEUlRWMXJ3b2ZRc1k9IiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJUb2RvRG90bmV0T2JvIiwiYXBwaWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJhcHBpZGFjciI6IjEiLCJmYW1pbHlfbmFtZSI6IkNhbnVtYWxsYSIsImdpdmVuX25hbWUiOiJOYXZ5YSIsImlwYWRkciI6IjE2Ny4yMjAuMC4xOTkiLCJuYW1lIjoiTmF2eWEgQ2FudW1hbGxhIiwib2lkIjoiZDVlOTc5YzctM2QyZC00MmFmLThmMzAtNzI3ZGQ0YzJkMzgzIiwib25wcmVtX3NpZCI6IlMtMS01LTIxLTIxMjc1MjExODQtMTYwNDAxMjkyMC0xODg3OTI3NTI3LTI2MTE4NDg0IiwicGxhdGYiOiIxNCIsInB1aWQiOiIxMDAzM0ZGRkEwNkQxN0M5Iiwic2NwIjoiVXNlci5SZWFkIiwic3ViIjoibWtMMHBiLXlpMXQ1ckRGd2JTZ1JvTWxrZE52b3UzSjNWNm84UFE3alVCRSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInVuaXF1ZV9uYW1lIjoibmFjYW51bWFAbWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hY2FudW1hQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJWR1ItdmtEZlBFQ2M1dWFDaENRSkFBIiwidmVyIjoiMS4wIn0.cubh1L2VtruiiwF8ut1m9uNBmnUJeYx4x0G30F7CqSpzHj1Sv5DCgNZXyUz3pEiz77G8IfOF0_U5A_02k-xzwdYvtJUYGH3bFISzdqymiEGmdfCIRKl9KMeoo2llGv0ScCniIhr2U1yxTIkIpp092xcdaDt-2_2q_ql1Ha_HtjvTV1f9XR3t7_Id9bR5BqwVX5zPO7JMYDVhUZRx08eqZcC-F3wi0xd_5ND_mavMuxe2wrpF-EZviO3yg0QVRr59tE3AoWl8lSGpVc97vvRCnp4WVRk26jJhYXFPsdk4yWqOKZqzr3IFGyD08WizD_vPSrXcCPbZP3XWaoTUKZSNJg",
  "refresh_token": "OAQABAAAAAABnfiG-mA6NTae7CdWW7QfdAALzDWjw6qSn4GUDfxWzJDZ6lk9qRw4AnqPnvFqnzS3GiikHr5wBM1bV1YyjH3nUeIhKhqJWGwqJFRqs2sE_rqUfz7__3J92JDpi6gDdCZNNaXgreQsH89kLCVNYZeN6kGuFGZrjwxp1wS2JYc97E_3reXBxkHrA09K5aR-WsSKCEjf6WI23FhZMTLhk_ZKOe_nWvcvLj13FyvSrTMZV2cmzyCZDqEHtPVLJgSoASuQlD2NXrfmtcmgWfc3uJSrWLIDSn4FEmVDA63X6EikNp9cllH3Gp7Vzapjlnws1NQ1_Ff5QrmBHp_LKEIwfzVKnLLrQXN0EzP8f6AX6fdVTaeKzm7iw6nH0vkPRpUeLc3q_aNsPzqcTOnFfgng7t2CXUsMAGH5wclAyFCAwL_Cds7KnyDLL7kzOS5AVZ3Mqk2tsPlqopAiHijZaJumdTILDudwKYCFAMpUeUwEf9JmyFjl2eIWPmlbwU7cHKWNvuRCOYVqbsTTpJthwh4PvsL5ov5CawH_TaV8omG_tV6RkziHG9urk9yp2PH9gl7Cv9ATa3Vt3PJWUS8LszjRIAJmyw_EhgHBfYCvEZ8U9PYarvgqrtweLcnlO7BfnnXYEC18z_u5wemAzNBFUje2ttpGtRmRic4AzZ708tBHva2ePJWGX6pgQbiWF8esOrvWjfrrlfOvEn1h6YiBW291M022undMdXzum6t1Y1huwxHPHjCAA"
}
```

> [!NOTE]
> U ziet dat het bovenstaande toegangstoken een token V1-indeling is. Dit is omdat het token is opgegeven op basis van de bron waartoe toegang wordt verkregen. De Microsoft Graph-V1-tokens, aanvragen, zodat Azure AD V1-toegangstokens produceert wanneer een client een aanvraag tokens voor Microsoft Graph. Alleen toepassingen het beste toegangstokens - clients niet nodig om te onderzoeken. 


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
GET /v1.0/me HTTP/1.1
Host: graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQTZOVGFlN0NkV1c3UWZkSzdNN0RyNXlvUUdLNmFEc19vdDF3cEQyZjNqRkxiNlVrcm9PcXA2cXBJclAxZVV0QktzMHEza29HN3RzXzJpSkYtQjY1UV8zVGgzSnktUHZsMjkxaFNBQSIsImFsZyI6IlJTMjU2IiwieDV0IjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIiwia2lkIjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIn0.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaWF0IjoxNDkzOTMwMDE2LCJuYmYiOjE0OTM5MzAwMTYsImV4cCI6MTQ5MzkzMzg3NSwiYWNyIjoiMCIsImFpbyI6IkFTUUEyLzhEQUFBQUlzQjN5ZUljNkZ1aEhkd1YxckoxS1dlbzJPckZOUUQwN2FENTVjUVRtems9IiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJUb2RvRG90bmV0T2JvIiwiYXBwaWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJhcHBpZGFjciI6IjEiLCJmYW1pbHlfbmFtZSI6IkNhbnVtYWxsYSIsImdpdmVuX25hbWUiOiJOYXZ5YSIsImlwYWRkciI6IjE2Ny4yMjAuMC4xOTkiLCJuYW1lIjoiTmF2eWEgQ2FudW1hbGxhIiwib2lkIjoiZDVlOTc5YzctM2QyZC00MmFmLThmMzAtNzI3ZGQ0YzJkMzgzIiwib25wcmVtX3NpZCI6IlMtMS01LTIxLTIxMjc1MjExODQtMTYwNDAxMjkyMC0xODg3OTI3NTI3LTI2MTE4NDg0IiwicGxhdGYiOiIxNCIsInB1aWQiOiIxMDAzM0ZGRkEwNkQxN0M5Iiwic2NwIjoiVXNlci5SZWFkIiwic3ViIjoibWtMMHBiLXlpMXQ1ckRGd2JTZ1JvTWxrZE52b3UzSjNWNm84UFE3alVCRSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInVuaXF1ZV9uYW1lIjoibmFjYW51bWFAbWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hY2FudW1hQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJzUVlVekYxdUVVS0NQS0dRTVFVRkFBIiwidmVyIjoiMS4wIn0.Hrn__RGi-HMAzYRyCqX3kBGb6OS7z7y49XPVPpwK_7rJ6nik9E4s6PNY4XkIamJYn7tphpmsHdfM9lQ1gqeeFvFGhweIACsNBWhJ9Nx4dvQnGRkqZ17KnF_wf_QLcyOrOWpUxdSD_oPKcPS-Qr5AFkjw0t7GOKLY-Xw3QLJhzeKmYuuOkmMDJDAl0eNDbH0HiCh3g189a176BfyaR0MgK8wrXI_6MTnFSVfBePqklQeLhcr50YTBfWg3Svgl6MuK_g1hOuaO-XpjUxpdv5dZ0SvI47fAuVDdpCE48igCX5VMj4KUVytDIf6T78aIXMkYHGgW3-xAmuSyYH_Fr0yVAQ
```

## <a name="client-limitations"></a>Clientbeperkingen
Als een client de impliciete stroom gebruikt om op te halen van een id_token en dat de client ook jokertekens in een antwoord-URL heeft, kan de id_token kan niet worden gebruikt voor een OBO-stroom.  Toegangstokens die zijn verkregen via de impliciete stroom kunnen echter nog steeds worden ingewisseld door een vertrouwelijke client, zelfs als de initiërende client een antwoord-URL van het jokerteken geregistreerd heeft. 

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het OAuth 2.0-protocol en een andere manier voor het uitvoeren van service-to-service-verificatie met behulp van clientreferenties.
* [Referenties voor OAuth 2.0-client verlenen in Azure AD v2.0](v2-oauth2-client-creds-grant-flow.md)
* [OAuth 2.0 in Azure AD v2.0](v2-oauth2-auth-code-flow.md)
