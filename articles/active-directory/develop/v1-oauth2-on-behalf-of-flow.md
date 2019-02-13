---
title: Azure Active Directory service-naar-service-verificatie die gebruikmaakt van de OAuth 2.0-conceptspecificatie op-andere gebruikers-Of-| Microsoft Docs
description: In dit artikel wordt beschreven hoe u gebruik van HTTP-berichten voor het implementeren van service-naar-serviceverificatie met het OAuth 2.0 op namens-stroom.
services: active-directory
documentationcenter: .net
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: celested
ms.reviewer: hirsin, nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd5d724583dd4682fb6c0b01e8ec11196936289b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56211900"
---
# <a name="service-to-service-calls-that-use-delegated-user-identity-in-the-on-behalf-of-flow"></a>Service-naar-service aanroepen die gebruik gedelegeerde gebruikersidentiteit in de On-Behalf-Of-stroom

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

De stroom voor OAuth 2.0 namens (OBO) kan een toepassing die een service of een web-API om door te geven van de verificatie van de gebruiker op een andere service of web-API aanroept. De stroom OBO geeft de gemachtigde gebruiker identiteits- en machtigingen via de aanvraagketen. Voor de middelste laag service voor geverifieerde aanvragen versturen naar de downstream-service, moet het toegangstoken op uit Azure Active Directory (Azure AD) beveiligen namens de gebruiker.

> [!IMPORTANT]
> Vanaf mei 2018, een `id_token` kan niet worden gebruikt voor de On-Behalf-Of-stroom.  Apps van één pagina (kuuroorden) moeten een toegangstoken doorgeven aan een vertrouwelijke client van de middelste laag om uit te voeren OBO stromen. Zie voor meer informatie over de clients die kunnen worden uitgevoerd op-andere gebruikers-Of aanroepen [beperkingen](#client-limitations).

## <a name="on-behalf-of-flow-diagram"></a>On-Behalf-Of stroomdiagram

De OBO-stroom wordt gestart nadat de gebruiker is geverifieerd in een toepassing die gebruikmaakt van de [OAuth 2.0-autorisatiecode verlenen stroom](v1-protocols-oauth-code.md). Op dat moment een toegangstoken (token A) in de toepassing wordt verzonden op de middelste laag web API (A-API) met claims van de gebruiker en de toestemming voor toegang tot API A. API A maakt vervolgens een geverifieerde aanvraag voor de downstream web-API (API-B).

De stroom op-andere gebruikers-Of deel uitmaken van deze stappen: ![OAuth 2.0 op namens-stroom](./media/v1-oauth2-on-behalf-of-flow/active-directory-protocols-oauth-on-behalf-of-flow.png)

1. De clienttoepassing doet een aanvraag bij API A met het token A.
1. API A wordt geverifieerd op het eindpunt van de Azure AD-token-uitgifte en vraagt een token voor toegang tot API B.
1. Het eindpunt van de Azure AD-token-uitgifte van de A-API-referenties met een token valideert en problemen met het toegangstoken voor API-B (token B).
1. De aanvraag voor API B bevat token B in de autorisatie-header.
1. B-API retourneert gegevens van de beveiligde resource.

>[!NOTE]
>De claim doelgroep in een toegangstoken dat wordt gebruikt voor het aanvragen van een token voor een downstream-service moet de ID van de service die de OBO-aanvraag. Het token ook moet zijn ondertekend met de globale ondertekeningssleutel van Azure Active Directory (dit is de standaardinstelling voor toepassingen die zijn geregistreerd via **App-registraties** in de portal).

## <a name="register-the-application-and-service-in-azure-ad"></a>De toepassing en de service in Azure AD registreren

Registreer de middelste laag en de clienttoepassing in Azure AD.

### <a name="register-the-middle-tier-service"></a>De middelste laag service registreren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Op de bovenste balk, selecteert u uw account en kijk onder de **Directory** om te selecteren van een Active Directory-tenant voor uw toepassing.
1. Selecteer **meer Services** in het linkerdeelvenster en kies **Azure Active Directory**.
1. Selecteer **App-registraties** en vervolgens **nieuwe toepassing registreren**.
1. Geef een beschrijvende naam voor de toepassing en selecteer het toepassingstype.
    1. De aanmeldings-URL of de omleidings-URL, afhankelijk van het type ingesteld op de basis-URL.
    1. Selecteer **maken** om de toepassing te maken.
1. Het genereren van een clientgeheim vóór het afsluiten van de Azure-portal.
    1. In de Azure-portal, kiest u uw toepassing en selecteer **instellingen**.
    1. Selecteer **sleutels** in het menu instellingen en voeg een sleutel met een duur van de sleutel van één jaar of twee jaar.
    1. Als u deze pagina opslaat, wordt de sleutelwaarde in de Azure-portal weergegeven. Kopieer en bewaar de sleutelwaarde op een veilige locatie.

    > [!IMPORTANT]
    > U moet de sleutel voor het configureren van de toepassingsinstellingen in uw implementatie. De waarde van deze sleutel niet opnieuw wordt weergegeven en is het niet worden opgehaald door een andere manier. Noteer deze zodra deze zichtbaar in de Azure portal is.

### <a name="register-the-client-application"></a>De clienttoepassing registreren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Op de bovenste balk, selecteert u uw account en kijk onder de **Directory** om te selecteren van een Active Directory-tenant voor uw toepassing.
1. Selecteer **meer Services** in het linkerdeelvenster en kies **Azure Active Directory**.
1. Selecteer **App-registraties** en vervolgens **nieuwe toepassing registreren**.
1. Geef een beschrijvende naam voor de toepassing en selecteer het toepassingstype.
   1. De aanmeldings-URL of de omleidings-URL, afhankelijk van het type ingesteld op de basis-URL.
   1. Selecteer **maken** om de toepassing te maken.
1. Machtigingen configureren voor uw toepassing.
   1. Kies in het menu instellingen de **vereiste machtigingen** uit en selecteer vervolgens **toevoegen** en **Select an API**.
   1. Typ de naam van de middelste laag service in het tekstveld.
   1. Kies **Selecteer machtigingen** en selecteer vervolgens **de naam van de Access-service**.

### <a name="configure-known-client-applications"></a>Bekende clienttoepassingen configureren

In dit scenario moet de middelste laag service om op te halen van de gebruiker toestemming voor toegang tot de downstream-API zonder tussenkomst van de gebruiker. De optie om toegang te verlenen aan de downstream-API moet tijdens de verificatie van voorgrond als onderdeel van de stap toestemming worden aangeboden.

Volg de stappen hieronder om expliciet verbindt de clientapp-registratie in Azure AD met registratie van de middelste laag service. Met deze bewerking worden de toestemming vereist door de client en de middelste laag in een enkel dialoogvenster samengevoegd.

1. Ga naar de registratie van de middelste laag service en selecteer **Manifest** om het manifest editor te openen.
1. Zoek de `knownClientApplications` eigenschap matrix en de client-ID van de clienttoepassing toevoegen als een element.
1. Het manifest opslaan door **opslaan**.

## <a name="service-to-service-access-token-request"></a>Service-naar-service toegangstokenaanvraag

Een HTTP POST voor het aanvragen van een toegangstoken maken voor de tenant-specifieke Azure AD-eindpunt met de volgende parameters:

```
https://login.microsoftonline.com/<tenant>/oauth2/token
```

De clienttoepassing wordt beveiligd door een gedeeld geheim of door een certificaat.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>Eerste geval: Aanvraag voor een toegangstoken met een gedeeld geheim

Wanneer u een gedeeld geheim, bevat een tokenaanvraag voor de service-naar-service toegang tot de volgende parameters:

| Parameter |  | Description |
| --- | --- | --- |
| grant_type |vereist | Het type van het token aan te vragen. Een aanvraag OBO maakt gebruik van een JSON Web Token (JWT), zodat de waarde moet **urn: ietf:params:oauth:grant-type: jwt-bearer**. |
| bevestiging |vereist | De waarde van het toegangstoken wordt gebruikt in de aanvraag. |
| client_id |vereist | De app-ID die wordt toegewezen aan de aanroepende service tijdens de registratie met Azure AD. Ga voor de app-ID in Azure portal, selecteert u **Active Directory**, kiest u de map en selecteer vervolgens de naam van de toepassing. |
| client_secret |vereist | De sleutel geregistreerd voor de aanroepende service in Azure AD. Deze waarde moet zijn vermeld op het moment van inschrijving. |
| Bron |vereist | De app-ID-URI van de ontvangende service (beveiligde resource). Als u wilt de app-ID-URI vinden in de Azure-portal, selecteert u **Active Directory** en kiest u de map. Naam van de toepassing selecteert, kiest u **alle instellingen**, en selecteer vervolgens **eigenschappen**. |
| requested_token_use |vereist | Hiermee geeft u op hoe de aanvraag moet worden verwerkt. In de stroom op-andere gebruikers-Of de waarde moet **on_behalf_of**. |
| scope |vereist | Een spatie gescheiden lijst met bereiken voor het token aan te vragen. Voor de OpenID Connect, het bereik **openid** moet worden opgegeven.|

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

### <a name="second-case-access-token-request-with-a-certificate"></a>Tweede geval: Aanvraag voor een toegangstoken met een certificaat

Een service-naar-service toegangstokenaanvraag met een certificaat bevat de volgende parameters:

| Parameter |  | Description |
| --- | --- | --- |
| grant_type |vereist | Het type van het token aan te vragen. Een aanvraag OBO maakt gebruik van een JWT-toegangstoken, zodat de waarde moet **urn: ietf:params:oauth:grant-type: jwt-bearer**. |
| bevestiging |vereist | De waarde van het token wordt gebruikt in de aanvraag. |
| client_id |vereist | De app-ID die wordt toegewezen aan de aanroepende service tijdens de registratie met Azure AD. Ga voor de app-ID in Azure portal, selecteert u **Active Directory**, kiest u de map en selecteer vervolgens de naam van de toepassing. |
| client_assertion_type |vereist |De waarde moet liggen `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |vereist | Een JSON Web Token die u maakt en meld u aan met het certificaat dat u geregistreerd als referenties voor uw toepassing. Zie [referenties van het certificaat](active-directory-certificate-credentials.md) voor meer informatie over assertion indeling en over het registreren van uw certificaat.|
| Bron |vereist | De app-ID-URI van de ontvangende service (beveiligde resource). Als u wilt de app-ID-URI vinden in de Azure-portal, selecteert u **Active Directory** en kiest u de map. Naam van de toepassing selecteert, kiest u **alle instellingen**, en selecteer vervolgens **eigenschappen**. |
| requested_token_use |vereist | Hiermee geeft u op hoe de aanvraag moet worden verwerkt. In de stroom op-andere gebruikers-Of de waarde moet **on_behalf_of**. |
| scope |vereist | Een spatie gescheiden lijst met bereiken voor het token aan te vragen. Voor de OpenID Connect, het bereik **openid** moet worden opgegeven.|

Deze parameters zijn bijna hetzelfde net als bij de aanvraag van het gedeelde geheim, behalve dat de `client_secret parameter` wordt vervangen door twee parameters: `client_assertion_type` en `client_assertion`.

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

## <a name="service-to-service-access-token-response"></a>Service-naar-service toegang token antwoord

Een geslaagd antwoord is een JSON OAuth 2.0-antwoord met de volgende parameters:

| Parameter | Description |
| --- | --- |
| token_type |Geeft aan dat de waarde van het token. Het enige type dat Azure AD ondersteunt **Bearer**. Zie voor meer informatie over bearer-tokens, de [OAuth 2.0 machtiging Framework: Bearer Token gebruik (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |
| scope |Het bereik van de toegang is verleend in het token. |
| expires_in |De hoeveelheid tijd die het toegangstoken ongeldig (in seconden is). |
| expires_on |De tijd wanneer het toegangstoken is verlopen. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC tot de vervaltijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache. |
| Bron |De app-ID-URI van de ontvangende service (beveiligde resource). |
| access_token |Het aangevraagde toegangstoken. De aanroepende service kunt u dit token gebruiken om te verifiëren bij de ontvangende service. |
| id_token |De aangevraagde ID-token. De aanroepende service kunt u dit token gebruiken om te controleren of de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. |
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

Het eindpunt van de Azure AD-token retourneert een foutbericht als er wordt geprobeerd een toegangstoken verkrijgen voor een downstream-API die is ingesteld met een beleid voor voorwaardelijke toegang (bijvoorbeeld, meervoudige verificatie). De middelste laag service moet deze fout naar de clienttoepassing surface, zodat de clienttoepassing kan de tussenkomst van de gebruiker om te voldoen aan het beleid voor voorwaardelijke toegang verlenen.

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

De middelste laag service het toegangstoken verkregen kunt gebruiken voor geverifieerde aanvragen versturen naar de downstream web-API door in te stellen van het token in de `Authorization` header.

### <a name="example"></a>Voorbeeld

```
GET /me?api-version=2013-11-08 HTTP/1.1
Host: graph.windows.net
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ
```

## <a name="saml-assertions-obtained-with-an-oauth20-obo-flow"></a>SAML-asserties ondertekend met een stroom voor OAuth 2.0 OBO verkregen

Sommige OAuth gebaseerde webservices nodig voor toegang tot andere webservice-API's die SAML-asserties ondertekend in niet-interactieve stromen accepteren. Azure Active Directory biedt een SAML-verklaring in reactie op een On-Behalf-Of-stroom die gebruikmaakt van een SAML gebaseerde webservice als een doelbron.

>[!NOTE]
>Dit is een uitbreiding van de OAuth 2.0 namens-stroom waarmee een OAuth2-toepassing voor toegang tot web service API-eindpunten die SAML-tokens gebruiken die niet-standaard.

> [!TIP]
> Wanneer u een SAML-beveiligde webservice vanuit een front-end-web-App aanroepen, kunt u gewoon de API aanroepen en een stroom normale interactieve verificatie met de bestaande sessie van de gebruiker te initiëren. U hoeft alleen het gebruik van een flow OBO wanneer de aanroep van een service-naar-service is vereist voor een SAML-token voor gebruikerscontext.

### <a name="obtain-a-saml-token-by-using-an-obo-request-with-a-shared-secret"></a>Een SAML-token ophalen met behulp van een OBO-aanvraag met een gedeeld geheim

Een service-naar-service-aanvraag voor een SAML-verklaring bevat de volgende parameters:

| Parameter |  | Description |
| --- | --- | --- |
| grant_type |vereist | Het type van het token aan te vragen. Voor een aanvraag die gebruikmaakt van een JWT, de waarde moet **urn: ietf:params:oauth:grant-type: jwt-bearer**. |
| bevestiging |vereist | De waarde van het toegangstoken wordt gebruikt in de aanvraag.|
| client_id |vereist | De app-ID die wordt toegewezen aan de aanroepende service tijdens de registratie met Azure AD. Ga voor de app-ID in Azure portal, selecteert u **Active Directory**, kiest u de map en selecteer vervolgens de naam van de toepassing. |
| client_secret |vereist | De sleutel geregistreerd voor de aanroepende service in Azure AD. Deze waarde moet zijn vermeld op het moment van inschrijving. |
| Bron |vereist | De app-ID-URI van de ontvangende service (beveiligde resource). Dit is de resource die de doelgroep van het SAML-token. Als u wilt de app-ID-URI vinden in de Azure-portal, selecteert u **Active Directory** en kiest u de map. Naam van de toepassing selecteert, kiest u **alle instellingen**, en selecteer vervolgens **eigenschappen**. |
| requested_token_use |vereist | Hiermee geeft u op hoe de aanvraag moet worden verwerkt. In de stroom op-andere gebruikers-Of de waarde moet **on_behalf_of**. |
| requested_token_type | vereist | Hiermee geeft u het type token dat is aangevraagd. De waarde kan zijn **urn: ietf:params:oauth:token-type: saml2** of **urn: ietf:params:oauth:token-type: saml1** afhankelijk van de vereisten van de gebruikte resource. |

Het antwoord bevat een SAML-token dat is gecodeerd in UTF8 en Base64url.

- **SubjectConfirmationData voor een SAML-verklaring afkomstig is van een aanroep van OBO**: Als de doeltoepassing moet een waarde van de ontvanger in **SubjectConfirmationData**, en vervolgens de waarde een niet-jokertekens antwoord-URL in de configuratie van de resource-toepassing moet.
- **Het knooppunt SubjectConfirmationData**: Het knooppunt mag niet een **InResponseTo** omdat het geen deel uit van een SAML-antwoord van het kenmerk. De toepassing die is ontvangen van het SAML-token moet accepteren van de SAML-verklaring zonder een **InResponseTo** kenmerk.

- **Toestemming geven**: Toestemming moet hebben gekregen voor het ontvangen van een SAML-token met gebruikersgegevens op een OAuth-stroom. Zie voor meer informatie over machtigingen en toestemming voor het verkrijgen van beheerder [machtigingen en toestemming in het eindpunt van de Azure Active Directory v1.0](https://docs.microsoft.com/azure/active-directory/develop/v1-permissions-and-consent).

### <a name="response-with-saml-assertion"></a>Antwoord met SAML-verklaring

| Parameter | Description |
| --- | --- |
| token_type |Geeft aan dat de waarde van het token. Het enige type dat Azure AD ondersteunt **Bearer**. Zie voor meer informatie over het bearer-tokens [OAuth 2.0 machtiging Framework: Bearer Token gebruik (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |
| scope |Het bereik van de toegang is verleend in het token. |
| expires_in |De hoeveelheid tijd die het toegangstoken ongeldig (in seconden is). |
| expires_on |De tijd wanneer het toegangstoken is verlopen. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC tot de vervaltijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache. |
| Bron |De app-ID-URI van de ontvangende service (beveiligde resource). |
| access_token |De parameter die het SAML-verklaring retourneert. |
| refresh_token |Het vernieuwingstoken. De aanroepende service kunt u dit token gebruiken om aan te vragen van een andere toegangstoken nadat de huidige SAML-verklaring is verlopen. |

- token_type: Bearer
- expires_in: 3296
- ext_expires_in: 0
- expires_on: 1529627844
- resource: `https://api.contoso.com`
- access_token: \<SAML-verklaring\>
- issued_token_type: urn: ietf:params:oauth:token-type: saml2
- refresh_token: \<Token vernieuwen\>

## <a name="client-limitations"></a>Clientbeperkingen

Openbare clients met jokertekens antwoord-URL's niet gebruiken een `id_token` voor OBO stromen. Echter nog steeds een vertrouwelijke client kunt inwisselen **toegang** tokens die zijn verkregen via de impliciete stroom, zelfs als de openbare-client een jokerteken heeft omleidings-URI die zijn geregistreerd.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het OAuth 2.0-protocol en een andere manier voor het uitvoeren van service-naar-service-verificatie die gebruikmaakt van clientreferenties:

* [Service-verificatie met behulp van OAuth 2.0-clientreferenties in Azure AD-service](v1-oauth2-client-creds-grant-flow.md)
* [OAuth 2.0 in Azure AD](v1-protocols-oauth-code.md)
