---
title: Desktop-app dat aanroepen van web-API's (code-configuratie) - Microsoft identity-platform
description: Informatie over het bouwen van een bureaublad-app dat aanroepen van web-API's (configuratie van de app-code)
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
ms.date: 05/o7/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c52f6fc66187d961dc93089a9f81f6de4d67fe41
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075938"
---
# <a name="desktop-app-that-calls-web-apis---code-configuration"></a>Desktop-app dat aanroepen van web-API's - code configureren

Nu dat u uw toepassing hebt gemaakt, leert u hoe u de code met de coördinaten van de toepassing configureren.

## <a name="msal-libraries"></a>MSAL-bibliotheken

De enige MSAL-bibliotheek bureaubladtoepassingen vandaag ondersteunen is MSAL.NET

## <a name="public-client-application"></a>Openbare clienttoepassing

Vanuit het oogpunt van code bureaubladtoepassingen openbare clienttoepassingen zijn en dat is de reden waarom u maken en bewerken van MSAL.NET `IPublicClientApplication`. Opnieuw zijn dingen enigszins verschillend of u interactieve verificatie gebruiken of niet.

![IPublicClientApplication](media/scenarios/public-client-application.png)

### <a name="exclusively-by-code"></a>Uitsluitend door code

De volgende code waarmee gebruikers aanmelden in de openbare cloud van Microsoft Azure met hun werk en schoolaccounts, of hun persoonlijke Microsoft-account wordt een openbare client-toepassing.

```CSharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId)
    .Build();
```

Als u gebruikmaken van interactieve verificatie, wilt zoals hierboven wordt weergegeven, die u wilt gebruiken de `.WithRedirectUri` modifier:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithRedirectUri(PublicClientApplicationBuilder.DefaultInteractiveDesktopRedirectUri)
         .Build();
```

### <a name="using-configuration-files"></a>Met behulp van configuratiebestanden

De volgende code wordt een openbare client-toepassing van een configuratieobject, dat kan worden ingevuld via een programma of gelezen uit een configuratiebestand

```CSharp
PublicClientApplicationOptions options = GetOptions(); // your own method
IPublicClientApplication app = PublicClientApplicationBuilder.CreateWithApplicationOptions(options)
    .Build();
```

### <a name="more-elaborated-configuration"></a>Meer uitgewerkt configuratie

U kunt de toepassing bouwen door een aantal parameters toe te voegen uitwerken. Bijvoorbeeld, als u wilt dat uw toepassing moet een toepassing met meerdere tenants in een nationale cloud (hier voor de Amerikaanse overheid), dat u kunt schrijven:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAadAuthority(AzureCloudInstance.AzureUsGovernment,
                         AadAuthorityAudience.AzureAdMultipleOrgs)
        .Build();
```

MSAL.NET bevat ook een optie voor AD FS 2019:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAdfsAuthority("https://consoso.com/adfs")
        .Build();
```

Ten slotte, als u verkrijgen van tokens voor een Azure AD B2C-tenant wilt, kunt uw tenant, zoals wordt weergegeven in het volgende codefragment opgeven:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithB2CAuthority("https://fabrikamb2c.b2clogin.com/tfp/{tenant}/{PolicySignInSignUp}")
        .Build();
```

### <a name="learn-more"></a>Meer informatie

Voor meer informatie over het configureren van een bureaubladtoepassing MSAL.NET:

- Voor een lijst van alle opties die beschikbaar is op `PublicClientApplicationBuilder`, Zie de referentiedocumentatie [PublicClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.appconfig.publicclientapplicationbuilder?view=azure-dotnet-preview#methods)
- Voor de beschrijving van alle opties die beschikbaar zijn in `PublicClientApplicationOptions` Zie [PublicClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.appconfig.publicclientapplicationoptions?view=azure-dotnet-preview), in de documentatie van verwijzing

## <a name="complete-example-with-configuration-options"></a>Compleet voorbeeld met configuratie-opties

Een .NET Core-consoletoepassing met de volgende Imagine `appsettings.json` configuratiebestand:

```JSon
{
  "Authentication": {
    "AzureCloudInstance": "AzurePublic",
    "AadAuthorityAudience": "AzureAdMultipleOrgs",
    "ClientId": "ebe2ab4d-12b3-4446-8480-5c3828d04c50"
  },

  "WebAPI": {
    "MicrosoftGraphBaseEndpoint": "https://graph.microsoft.com"
  }
}
```

U hebt weinig code voor het lezen van dit bestand met behulp van de opgegeven configuratie-framework; .NET

```CSharp
public class SampleConfiguration
{
 /// <summary>
 /// Authentication options
 /// </summary>
 public PublicClientApplicationOptions PublicClientApplicationOptions { get; set; }

 /// <summary>
 /// Base URL for Microsoft Graph (it varies depending on whether the application is ran
 /// in Microsoft Azure public clouds or national / sovereign clouds
 /// </summary>
 public string MicrosoftGraphBaseEndpoint { get; set; }

 /// <summary>
 /// Reads the configuration from a json file
 /// </summary>
 /// <param name="path">Path to the configuration json file</param>
 /// <returns>SampleConfiguration as read from the json file</returns>
 public static SampleConfiguration ReadFromJsonFile(string path)
 {
  // .NET configuration
  IConfigurationRoot Configuration;
  var builder = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile(path);
  Configuration = builder.Build();

  // Read the auth and graph endpoint config
  SampleConfiguration config = new SampleConfiguration()
  {
   PublicClientApplicationOptions = new PublicClientApplicationOptions()
  };
  Configuration.Bind("Authentication", config.PublicClientApplicationOptions);
  config.MicrosoftGraphBaseEndpoint =
  Configuration.GetValue<string>("WebAPI:MicrosoftGraphBaseEndpoint");
  return config;
 }
}
```

Voor het maken van uw toepassing, u hebt nu alleen nodig om de volgende code te schrijven:

```CSharp
SampleConfiguration config = SampleConfiguration.ReadFromJsonFile("appsettings.json");
var app = PublicClientApplicationBuilder.CreateWithApplicationOptions(config.PublicClientApplicationOptions)
           .Build();
```

en vóór de aanroep van de `.Build()` methode, kunt u uw configuratie met aanroepen naar overschrijven `.WithXXX` methoden, zoals eerder weergegeven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ophalen van een token voor een bureaublad-app](scenario-desktop-acquire-token.md)
