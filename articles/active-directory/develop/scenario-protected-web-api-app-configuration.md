---
title: Beveiligde web-API - configuratie van de app-code | Azure
description: Informatie over het bouwen van een beveiligde Web-API en de code van uw toepassing configureren.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdd68d9ec5d0dd83df4628f39785ce255482245d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111154"
---
# <a name="protected-web-api---code-configuration"></a>Beveiligde web-API - code configureren

Als u wilt configureren van de code voor uw beveiligde web-API, moet u begrijpen wat de API's beveiligd maakt, wat u moet het bearer-token configureren en valideren van het token.

## <a name="what-makes-aspnetaspnet-core-apis-protected"></a>Wat is ASP.NET/ASP.NET Core API's beveiligd?

Net als in web-Apps, de ASP.NET/ASP.NET core web API's zijn 'beveiliging gebruiken' omdat de acties van de controller worden voorafgegaan door de `[Authorize]` kenmerk. De acties van de domeincontroller kunnen daarom alleen worden aangeroepen als de API wordt aangeroepen met een identiteit die is gemachtigd.

Houd rekening met de volgende vragen:

- Hoe weet de web-API de identiteit van de toepassing die wordt aangeroepen (alleen voor een toepassing kan een web-API aanroepen)
- Als de toepassing namens een gebruiker de web-API wordt aangeroepen, wat is de identiteit van de gebruiker?

## <a name="bearer-token"></a>Bearer-token

De informatie over de identiteit van de toepassing, en de gebruiker (tenzij de web-app service-naar-service aanroepen vanuit een daemontoepassing accepteert), worden bewaard in het bearer-token dat ingesteld in de header bij het aanroepen van de toepassing.

Hier volgt een C# codevoorbeeld ziet u een client die de API wordt aangeroepen na het aanschaffen van een token met MSAL.NET.

```CSharp
var scopes = new[] {$"api://.../access_as_user}";
var result = await app.AcquireToken(scopes)
                      .ExecuteAsync();

httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
```

> [!IMPORTANT]
> Het bearer-token is aangevraagd door een clienttoepassing naar het eindpunt van Microsoft identity-platform **voor de web-API**. De web-API is de enige toepassing die u moet controleren of het token en bekijk de claims die deze bevat. De clienttoepassingen nooit de claims in de tokens moeten kijken (de web-API kan bepalen, in de toekomst, die vereist dat het token wordt gecodeerd en hierdoor wordt de clienttoepassing die noten kraken opent u de toegangstokens te geven).

## <a name="jwtbearer-configuration"></a>JwtBearer configuratie

In deze sectie bevat informatie over wat u moet het bearer-token configureren.

### <a name="config-file"></a>Configuratiebestand

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line of business app)
      Otherwise you can leave them set to common
      This can be:
      - A guid (Tenant ID = Directory ID)
      - 'common' (Any organization and personal accounts)
      - 'organizations' (any organization)
      - 'consumers' (Microsoft personal accounts)
    */
    "TenantId": "common"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="code-initialization"></a>Initialisatie van code

Wanneer de toepassing wordt aangeroepen op het bedrijf van de actie controller een `[Authorize]` kenmerk ASP.NET/ASP.NET core kijkt naar het bearer-token in de autorisatie-header van de aanroepende aanvraag en haalt het toegangstoken, dit wordt vervolgens doorgestuurd naar de JwtBearer middleware, die de extensies van Microsoft Identity Model voor .NET aanroept.

In de ASP.NET Core, deze middleware wordt geïnitialiseerd in het `Startup.cs` bestand.

```CSharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

De middleware wordt toegevoegd aan de web-API door de volgende instructie:

```CSharp
 services.AddAzureAdBearer(options => Configuration.Bind("AzureAd", options));
```

 Op dit moment maken de ASP.NET Core-sjablonen voor Azure AD v1.0 web-API's. Echter, kunt u eenvoudig wijzigen ze voor het gebruik van het eindpunt van de Microsoft identity-platform door toe te voegen van de volgende code in de `Startup.cs` bestand.

```CSharp
services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
{
    // This is a Microsoft identity platform v2.0 web API
    options.Authority += "/v2.0";

    // The web API accepts as audiences are both the Client ID (options.Audience) and api://{ClientID}
    options.TokenValidationParameters.ValidAudiences = new []
    {
     options.Audience,
     $"api://{options.Audience}"
    };

    // Instead of using the default validation (validating against a single tenant,
    // as we do in line of business apps),
    // we inject our own multi-tenant validation logic (which even accepts both V1 and V2 tokens)
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ValidateAadIssuer;
});
```

## <a name="token-validation"></a>Validatie van tokens

De JwtBearer-middleware, zoals de middleware OpenID Connect in web-apps, wordt geleid door de `TokenValidationParameters` voor het valideren van het token. Het token wordt ontsleuteld (indien nodig), de claims zijn uitgepakt en de handtekening wordt geverifieerd. Vervolgens is het token gevalideerd door te zoeken naar de volgende gegevens:

- Het bedoeld voor de web-API (doelgroep)
- Het is uitgegeven voor een toepassing die is toegestaan voor het aanroepen van de web-API (sub)
- Het is uitgegeven door een betrouwbaar Security Token Server (STS) (verlener)
- Levensduur van het token is binnen het bereik (verlopen)
- Er is geen geknoeid met (handtekening)

Er kan ook worden speciale bevestigingen. Het is bijvoorbeeld mogelijk om te valideren dat ondersteuningssleutels (wanneer de ingesloten in een token) vertrouwd worden en dat het token is niet opnieuw worden afgespeeld. Ten slotte vereisen sommige protocollen specifieke bevestigingen.

### <a name="validators"></a>Systeemstatuscontrolepunten

De validatiestappen worden vastgelegd in validators die bevinden zich allemaal in de [extensie voor de Microsoft Identity-Model voor .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) open-source-bibliotheek in een bronbestand: [Microsoft.IdentityModel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs)

De validators worden beschreven in de volgende tabel:

| Validator | Description |
|---------|---------|
| `ValidateAudience` | Zorgt ervoor dat het token voor de toepassing die het token (voor me valideert). |
| `ValidateIssuer` | Zorgt ervoor dat het token is uitgegeven door een vertrouwde STS (van iemand die ik vertrouw). |
| `ValidateIssuerSigningKey` | Zorgt ervoor dat de toepassing die de token vertrouwensrelaties valideren de sleutel die is gebruikt voor het ondertekenen van het token (speciale gevallen waarbij de sleutel is ingesloten in het token wordt normaal gesproken niet nodig). |
| `ValidateLifetime` | Zorgt ervoor dat het token nog steeds (of al) geldig is. Gedaan door te controleren dat de levensduur van het token (`notbefore`, `expires` claims) binnen het bereik. |
| `ValidateSignature` | Zorgt ervoor dat het token niet is geknoeid. |
| `ValidateTokenReplay` | Zorgt ervoor dat het token is niet opnieuw afgespeeld (speciaal geval voor sommige protocollen voor eenmalig gebruik). |

De validators worden allemaal gekoppeld aan de eigenschappen van de `TokenValidationParameters` klasse, zelf geïnitialiseerd vanuit de configuratie ASP.NET/ASP.NET Core. In de meeste gevallen kunt u geen om de parameters te wijzigen. Er is een uitzondering voor toepassingen die niet één tenants (dat wil zeggen web-apps die gebruikers vanaf elke organisatie of persoonlijke Microsoft-accounts accepteren)--in dit geval wordt de verlener moet worden gevalideerd.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Controleer of bereiken en app-rollen in uw code](scenario-protected-web-api-verification-scope-app-roles.md)
