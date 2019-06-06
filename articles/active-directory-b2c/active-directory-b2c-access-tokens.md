---
title: Aanvragen van een toegangstoken - Azure Active Directory B2C | Microsoft Docs
description: Informatie over het aanvragen van een toegangstoken van Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 1a545f1e0fd1360d9147280454fb8b75bf216152
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66507396"
---
# <a name="request-an-access-token-in-azure-active-directory-b2c"></a>Aanvraag een toegangstoken in Azure Active Directory B2C

Een *toegangstoken* claims die u in Azure Active Directory (Azure AD) B2C gebruiken kunt voor het identificeren van de toegekende machtigingen aan uw API's bevat. Bij het aanroepen van een resource-server, moet een toegangstoken aanwezig zijn in de HTTP-aanvraag. Een toegangstoken wordt aangeduid als **access_token** in de antwoorden van Azure AD B2C. 

In dit artikel wordt beschreven hoe u aan te vragen van een toegangstoken voor een web-App en web-API. Zie voor meer informatie over de tokens in Azure AD B2C, de [overzicht van de tokens in Azure Active Directory B2C](active-directory-b2c-reference-tokens.md).

> [!NOTE]
> **Web-API-ketens (On-Behalf-Of) wordt niet ondersteund door Azure AD B2C.** -Veel architecturen bevatten een web-API die u moet een andere downstream web-API, zowel beveiligd door Azure AD B2C aanroepen. In dit scenario is gebruikelijk in clients met een web-API-backend, die op zijn beurt een andere service aanroept. In dit scenario gekoppelde web-API kan worden ondersteund met behulp van de toekenning van OAuth 2.0 JWT Bearer referentie, ook bekend als de On-Behalf-Of-stroom. De On-Behalf-Of-stroom is echter niet op dit moment geïmplementeerd in Azure AD B2C.

## <a name="prerequisites"></a>Vereisten

- [Een gebruikersstroom maken](tutorial-create-user-flows.md) zodat gebruikers kunnen zich registreren en aanmelden bij uw toepassing.
- Als u dit nog niet hebt gedaan, [toevoegen van een web-API-toepassing naar uw Azure Active Directory B2C-tenant](add-web-application.md).

## <a name="scopes"></a>Bereiken

Scopes bieden een manier om machtigingen tot beveiligde resources te beheren. Wanneer een toegangstoken wordt aangevraagd, de clienttoepassing nodig heeft om op te geven van de gewenste machtigingen in de **bereik** parameter van de aanvraag. Bijvoorbeeld, om op te geven de **bereikwaarde** van `read` voor de API waaraan de **App ID URI** van `https://contoso.onmicrosoft.com/api`, het bereik worden `https://contoso.onmicrosoft.com/api/read`.

Bereiken worden door de web-API gebruikt om toegangsbeheer op basis van een bereik te implementeren. Bijvoorbeeld: gebruikers van de web-API kunnen zowel lees- als schrijftoegang hebben of alleen leestoegang. Als u wilt meerdere machtigingen in dezelfde aanvraag aanschaft, kunt u meerdere vermeldingen toevoegen in de ene **bereik** parameter van de aanvraag, gescheiden door spaties.

Het volgende voorbeeld ziet u in een URL gedecodeerd bereiken:

```
scope=https://contoso.onmicrosoft.com/api/read openid offline_access
```

Het volgende voorbeeld ziet u bereiken in een URL gecodeerd:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fapi%2Fread%20openid%20offline_access
```

Als u meer bereiken dan wat wordt verleend voor uw clienttoepassing aangevraagd, wordt de aanroep slaagt als ten minste één machtiging is verleend. De **scp** claim in het resulterende toegangstoken is gevuld met alleen de machtigingen die met succes zijn verleend. De OpenID Connect-standaard bevat verschillende speciale bereikwaarden. De machtiging voor toegang tot het gebruikersprofiel, gelden voor de volgende bereiken:

- **openid** -aanvragen van een ID-token.
- **offline_access** -vraagt een vernieuwing token met [autorisatiecode stromen](active-directory-b2c-reference-oauth-code.md).

Als de **response_type** parameter in een `/authorize` verzoek bevat `token`, wordt de **bereik** parameter moet ten minste één resource bereik dan bevatten `openid` en `offline_access`die wordt verleend. Anders wordt de `/authorize` aanvraag mislukt.

## <a name="request-a-token"></a>Een token aanvragen

Om aan te vragen een toegangstoken, moet u een autorisatiecode. Hieronder volgt een voorbeeld van een aanvraag naar de `/authorize` -eindpunt voor een autorisatiecode. Aangepaste domeinen worden niet ondersteund voor gebruik met toegangstokens. Gebruik uw tenant-name.onmicrosoft.com domein in de aanvraag-URL.

In het volgende voorbeeld vervangt u deze waarden:

- `<tenant-name>` -De naam van uw Azure AD B2C-tenant.
- `<policy-name>` -De naam van uw aangepaste beleid of een gebruiker-stroom.
- `<application-ID>` -De toepassings-id van de webtoepassing die u ter ondersteuning van de gebruikersstroom geregistreerd.
- `<redirect-uri>` -De **omleidings-URI** dat u hebt ingevoerd toen u de clienttoepassing geregistreerd.

```
GET https://<tenant-name>.b2clogin.com/tfp/<tenant-name>.onmicrosoft.com/<policy-name>/oauth2/v2.0/authorize?
client_id=<application-ID>
&nonce=anyRandomValue
&redirect_uri=https://jwt.ms
&scope=https://tenant-name>.onmicrosoft.com/api/read
&response_type=code 
```

Het antwoord met de autorisatiecode moet zijn vergelijkbaar met het volgende voorbeeld:

```
https://jwt.ms/?code=eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMC...
```

Na de autorisatiecode ontvangen, kunt u deze gebruiken om aan te vragen van een toegangstoken:

```
POST <tenant-name>.onmicrosoft.com/oauth2/v2.0/token?p=<policy-name> HTTP/1.1
Host: https://<tenant-name>.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&client_id=<application-ID>
&scope=https://<tenant-name>.onmicrosoft.com/api/read
&code=eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMC...
&redirect_uri=https://jwt.ms
&client_secret=2hMG2-_:y12n10vwH...
```

U ziet er ongeveer uitzien als het volgende antwoord:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrN...",
    "token_type": "Bearer",
    "not_before": 1549647431,
    "expires_in": 3600,
    "expires_on": 1549651031,
    "resource": "f2a76e08-93f2-4350-833c-965c02483b11",
    "profile_info": "eyJ2ZXIiOiIxLjAiLCJ0aWQiOiJjNjRhNGY3ZC0zMDkxLTRjNzMtYTcyMi1hM2YwNjk0Z..."
}
```

Bij het gebruik van https://jwt.ms voor het onderzoeken van het toegangstoken dat is geretourneerd, moet u er ongeveer uitzien als in het volgende voorbeeld:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dl..."
}.{
  "iss": "https://contoso0926tenant.b2clogin.com/c64a4f7d-3091-4c73-a7.../v2.0/",
  "exp": 1549651031,
  "nbf": 1549647431,
  "aud": "f2a76e08-93f2-4350-833c-965...",
  "oid": "1558f87f-452b-4757-bcd1-883...",
  "sub": "1558f87f-452b-4757-bcd1-883...",
  "name": "David",
  "tfp": "B2C_1_signupsignin1",
  "nonce": "anyRandomValue",
  "scp": "read",
  "azp": "38307aee-303c-4fff-8087-d8d2...",
  "ver": "1.0",
  "iat": 1549647431
}.[Signature]
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [tokens configureren in Azure AD B2C](configure-tokens.md)
