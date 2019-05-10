---
title: Web-API die downstream web-aanroepen van API's (de configuratie van app-code) - Microsoft identity-platform
description: Informatie over het bouwen van een web-API die aanroepen van web-API's (configuratie van de app-code)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
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
ms.openlocfilehash: 3dedef2d22df9c8c81410296bdb0c4814bd98b80
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507119"
---
# <a name="web-api-that-calls-web-apis---code-configuration"></a>Web-API die aanroepen van web-API's - code configureren

Nadat u uw web-API hebt geregistreerd, kunt u de code voor de toepassing configureren.

De code voor het configureren van uw web-API, zodat wordt de downstream web-API's voortbouwt op de code die wordt gebruikt om een web-API. Zie voor meer informatie, [beveiligde web-API - app-configuratie](scenario-protected-web-api-app-configuration.md).

## <a name="code-subscribed-to-ontokenvalidated"></a>Code die zijn geabonneerd op OnTokenValidated

Naast de configuratie van de code voor een beveiligde web-API's moet u zich abonneert op de validatie van het bearer-token dat ontvangen wanneer uw API wordt aangeroepen:

```CSharp
/// <summary>
/// Protects the web API with Microsoft Identity Platform v2.0 (AAD v2.0)
/// This supposes that the configuration files have a section named "AzureAD"
/// </summary>
/// <param name="services">Service collection to which to add authentication</param>
/// <param name="configuration">Configuration</param>
/// <returns></returns>
public static IServiceCollection AddProtectedApiCallsWebApis(this IServiceCollection services,
                                                             IConfiguration configuration,
                                                             IEnumerable<string> scopes)
{
    services.AddTokenAcquisition();
    services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
    {
        // When an access token for our own web API is validated, we add it 
        // to MSAL.NET's cache so that it can be used from the controllers.
        options.Events = new JwtBearerEvents();

        options.Events.OnTokenValidated = async context =>
        {
            context.Success();

            // Adds the token to the cache, and also handles the incremental consent 
            // and claim challenges
            AddAccountToCacheFromJwt(context, scopes);
            await Task.FromResult(0);
        };
    });
    return services;
}
```

## <a name="on-behalf-of-flow"></a>Namens-stroom

De methode AddAccountToCacheFromJwt() moet:

- Exemplaar maken van een vertrouwelijke client MSAL toepassing.
- Bel `AcquireTokenOnBehalf` voor het uitwisselen van het bearer-token dat is verkregen door de client voor de web-API, op basis van een bearer-token voor dezelfde gebruiker, maar voor onze API om aan te roepen een downstream-API.

### <a name="instantiate-a-confidential-client-application"></a>Exemplaar maken van een vertrouwelijke client-toepassing

Deze stroom is alleen beschikbaar in de vertrouwelijke clientstroom, zodat de beveiligde web-API biedt clientreferenties (clientgeheim of certificaat) voor de [ConfidentialClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.appconfig.confidentialclientapplicationbuilder?view=azure-dotnet-preview) via de `WithClientSecret` of `WithCertificate`methoden, respectievelijk.

![image](https://user-images.githubusercontent.com/13203188/55967244-3d8e1d00-5c7a-11e9-8285-a54b05597ec9.png)

```CSharp
IConfidentialClientApplication app;

#if !VariationWithCertificateCredentials
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .Build();
#else
// Building the client credentials from a certificate
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .Build();
#endif
```

### <a name="how-to-call-on-behalf-of"></a>Over het aanroepen van on-behalf-of

De on-behalf-of (OBO)-aanroep wordt gedaan door het aanroepen van de [AcquireTokenOnBehalf](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.apiconfig.acquiretokenonbehalfofparameterbuilder?view=azure-dotnet-preview) methode voor het `IConfidentialClientApplication` interface.

De `ClientAssertion` wordt samengesteld uit het bearer-token dat is ontvangen door de web-API van een eigen clients. Er zijn [twee constructors](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientcredential.-ctor?view=azure-dotnet), één waarmee een JWT bearer-token en één die met elk soort verklaring van de gebruiker (een ander soort beveiligingstoken, welk type vervolgens is opgegeven in een extra parameter met de naam `assertionType`).

![image](https://user-images.githubusercontent.com/13203188/37082180-afc4b708-21e3-11e8-8af8-a6dcbd2dfba8.png)

De stroom OBO wordt in de praktijk vaak gebruikt voor een token verkrijgen voor een downstream-API en op te slaan in de cache voor een token van de MSAL.NET gebruiker zodat andere onderdelen van de web-API kunnen later aanroepen op de [onderdrukkingen](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientapplicationbase.acquiretokensilent?view=azure-dotnet) van ``AcquireTokenOnSilent`` voor het aanroepen van de downstream-API's. Dit is het effect van het vernieuwen van de tokens, indien nodig.

```CSharp
private void AddAccountToCacheFromJwt(IEnumerable<string> scopes, JwtSecurityToken jwtToken, ClaimsPrincipal principal, HttpContext httpContext)
{
 try
 {
  UserAssertion userAssertion;
  IEnumerable<string> requestedScopes;
  if (jwtToken != null)
  {
   userAssertion = new UserAssertion(jwtToken.RawData, "urn:ietf:params:oauth:grant-type:jwt-bearer");
   requestedScopes = scopes ?? jwtToken.Audiences.Select(a => $"{a}/.default");
  }
  else
  {
   throw new ArgumentOutOfRangeException("tokenValidationContext.SecurityToken should be a JWT Token");
  }

  // Create the application
  var application = BuildConfidentialClientApplication(httpContext, principal);

  // .Result to make sure that the cache is filled-in before the controller tries to get access tokens
  var result = application.AcquireTokenOnBehalfOf(requestedScopes.Except(scopesRequestedByMsalNet),
                                                  userAssertion)
                                        .ExecuteAsync()
                                        .GetAwaiter().GetResult();
 }
 catch (MsalException ex)
 {
  Debug.WriteLine(ex.Message);
  throw;
 }
}
```

## <a name="protocol"></a>Protocol

Zie voor meer informatie over het protocol op-andere gebruikers-of [identiteitsplatform van Microsoft en OAuth 2.0 namens-stroom](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ophalen van een token voor de app.](scenario-web-api-call-api-acquire-token.md)
