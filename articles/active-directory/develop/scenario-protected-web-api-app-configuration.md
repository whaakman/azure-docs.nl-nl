---
title: Beveiligde web-API - configuratie van de app-code | Azure
description: Informatie over het bouwen van een beveiligde web-API en de code van uw toepassing configureren.
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
ms.openlocfilehash: fa262c1c6a091575a70c5670b1c7a7c96e8e2128
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536889"
---
# <a name="protected-web-api-code-configuration"></a>Beveiligde web-API: Codeconfiguratie

Voor het configureren van de code voor uw beveiligde web-API, moet u weten wat API's definieert als beveiligd, het configureren van een bearer-token en het valideren van het token.

## <a name="what-defines-aspnetaspnet-core-apis-as-protected"></a>Wat ASP.NET/ASP.NET Core API's definieert als beveiligd?

Zoals web-apps, de ASP.NET/ASP.NET Core-web-API's zijn 'beveiligd' vanwege de handelingen van deze domeincontroller worden voorafgegaan door de `[Authorize]` kenmerk. De controller-acties kunnen dus worden aangeroepen alleen als de API wordt aangeroepen met een identiteit die gemachtigd.

Houd rekening met de volgende vragen:

- Hoe weet de web-API de identiteit van de app die wordt aangeroepen (U kunt een web-API aanroepen voor alleen een app.)
- Als de app namens een gebruiker de web-API wordt aangeroepen, wat is de identiteit van de gebruiker?

## <a name="bearer-token"></a>Bearer-token

De informatie over de identiteit van de app, en de gebruiker (tenzij de web-app aanroepen vanuit een daemon-app service-naar-service accepteert), worden bewaard in het bearer-token dat ingesteld in de header wanneer de app wordt aangeroepen.

Hier volgt een C# codevoorbeeld ziet u een client die de API wordt aangeroepen nadat deze een token met Microsoft Authentication Library voor .NET (MSAL.NET krijgt):

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
> Het bearer-token is aangevraagd door een clienttoepassing naar het eindpunt van Microsoft identity-platform *voor de web-API*. De web-API is de enige toepassing die u moet controleren of het token en weergeven van de claims die deze bevat. Client-apps moeten nooit proberen om te controleren van de claims in tokens. (De web-API nodig zijn, in de toekomst, dat het token wordt gecodeerd. Deze vereiste wordt voorkomen dat toegang voor client-apps die toegangstokens kunt bekijken.)

## <a name="jwtbearer-configuration"></a>JwtBearer configuratie

In deze sectie wordt beschreven hoe u een bearer-token configureren.

### <a name="config-file"></a>Configuratiebestand

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line-of-business app).
      Otherwise, you can leave them set to common.
      This can be:
      - A GUID (Tenant ID = Directory ID)
      - 'common' (any organization and personal accounts)
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

Wanneer een app wordt aangeroepen op een domeincontroller actie waarin een `[Authorize]` kenmerk ASP.NET/ASP.NET Core kijkt naar het bearer-token in de autorisatie-header van de aanroepende aanvraag en haalt het toegangstoken. Het token wordt vervolgens doorgestuurd naar de JwtBearer-middleware die Microsoft IdentityModel-extensies voor .NET aanroept.

In ASP.NET Core, wordt deze middleware geïnitialiseerd in het bestand Startup.cs:

```CSharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

De middleware wordt toegevoegd aan de web-API door deze instructie:

```CSharp
 services.AddAzureAdBearer(options => Configuration.Bind("AzureAd", options));
```

 De ASP.NET Core-sjablonen maken op dit moment Azure Active Directory (Azure AD) web-API's die zich in gebruikers binnen uw organisatie of een organisatie, niet met persoonlijke accounts. Maar u kunt ze voor het gebruik van het eindpunt van de Microsoft identity-platform door deze code toe te voegen aan het bestand Startup.cs eenvoudig wijzigen:

```CSharp
services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
{
    // This is a Microsoft identity platform v2.0 web API.
    options.Authority += "/v2.0";

    // The web API accepts as audiences both the Client ID (options.Audience) and api://{ClientID}.
    options.TokenValidationParameters.ValidAudiences = new []
    {
     options.Audience,
     $"api://{options.Audience}"
    };

    // Instead of using the default validation (validating against a single tenant,
    // as we do in line-of-business apps),
    // we inject our own multitenant validation logic (which even accepts both v1 and v2 tokens).
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ValidateAadIssuer;
});
```

## <a name="token-validation"></a>Validatie van tokens

De JwtBearer-middleware, zoals de middleware OpenID Connect in web-apps is gericht op `TokenValidationParameters` voor het valideren van het token. Het token wordt ontsleuteld (indien nodig), de claims zijn uitgepakt en de handtekening wordt geverifieerd. De middleware valideert het token vervolgens door te zoeken naar deze gegevens:

- Het bedoeld voor de web-API (doelgroep).
- Het is uitgegeven voor een app die is toegestaan voor het aanroepen van de web-API (sub).
- Het is uitgegeven door een vertrouwde beveiligingstokenservice (STS) (verlener).
- De levensduur is binnen het bereik (verlopen).
- Het is niet gewijzigd (handtekening).

Er kan ook worden speciale bevestigingen. Het is bijvoorbeeld mogelijk om te valideren dat ondersteuningssleutels (wanneer de ingesloten in een token) vertrouwd worden en dat het token is niet opnieuw worden afgespeeld. Ten slotte vereisen sommige protocollen specifieke bevestigingen.

### <a name="validators"></a>Systeemstatuscontrolepunten

De validatiestappen worden vastgelegd in validators die bevinden zich allemaal in de [Microsoft IdentityModel-extensies voor .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) open-source-bibliotheek in een bronbestand: [Microsoft.IdentityModel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs).

De validators worden beschreven in deze tabel:

| Validator | Description |
|---------|---------|
| `ValidateAudience` | Zorgt ervoor dat het token is voor de toepassing die het token (voor me valideert). |
| `ValidateIssuer` | Zorgt ervoor dat het token is uitgegeven door een vertrouwde STS (van iemand die ik vertrouw). |
| `ValidateIssuerSigningKey` | Zorgt ervoor dat de toepassing die de token vertrouwensrelaties valideren de sleutel die is gebruikt voor het ondertekenen van het token. (Speciaal geval waarin de sleutel is ingesloten in het token. Meestal niet vereist.) |
| `ValidateLifetime` | Zorgt ervoor dat het token nog steeds (of al) geldig is. De validatie wordt gecontroleerd of de levensduur van het token (`notbefore` en `expires` claims) binnen het bereik. |
| `ValidateSignature` | Zorgt ervoor dat het token nog niet is geknoeid. |
| `ValidateTokenReplay` | Zorgt ervoor dat het token is niet opnieuw afgespeeld. (Speciaal geval voor sommige protocollen voor eenmalig gebruik). |

De validators worden allemaal gekoppeld aan de eigenschappen van de `TokenValidationParameters` klasse, zelf geïnitialiseerd vanuit de configuratie ASP.NET/ASP.NET Core. In de meeste gevallen kunt u geen om de parameters te wijzigen. Er is één uitzondering, voor apps die niet één tenants. (Dat wil zeggen, web-apps die gebruikers van elke organisatie of van persoonlijke Microsoft-accounts te accepteren.) In dit geval wordt moet de verlener worden gevalideerd.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Controleer of bereiken en app-rollen in uw code](scenario-protected-web-api-verification-scope-app-roles.md)
