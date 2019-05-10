---
title: Web-app die zich aanmeldt gebruikers (code-configuratie) - Microsoft identity-platform
description: Informatie over het bouwen van een web-app die zich aanmeldt gebruikers (code-configuratie)
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
ms.openlocfilehash: b2204fe3e08b3c4b909ddc8b7ade4cec219d34fb
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406633"
---
# <a name="web-app-that-signs-in-users---code-configuration"></a>Web-app die zich aanmeldt-gebruikers - code configureren

Informatie over het configureren van de code voor uw Web-app waarmee gebruikers zich aanmeldt.

## <a name="libraries-used-to-protect-web-apps"></a>Bibliotheken die worden gebruikt voor het beveiligen van Web-Apps

<!-- This section can be in an include for Web App and Web APIs -->
De bibliotheken die wordt gebruikt voor het beveiligen van een Web-App (en een Web-API) zijn:

| Platform | Tapewisselaar | Description |
|----------|---------|-------------|
| ![.NET](media/sample-v2-code/logo_net.png) | [Identiteit modelextensies voor .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | Microsoft Identity-extensies voor .NET rechtstreeks door ASP.NET en ASP.NET Core gebruikt, wordt een set van DLL-bestanden met zowel op .NET Framework en .NET Core voorgesteld. Vanuit een ASP.NET/ASP.NET Core Web-app, kunt u beheren met behulp van validatie van tokens de **TokenValidationParameters** klasse (met name in sommige scenario ISV's) |

## <a name="aspnet-core-configuration"></a>Configuratie van ASP.NET Core

### <a name="application-configuration-files"></a>Configuratiebestanden voor toepassing

In de ASP.NET Core, een Web application aanmelden gebruikers met de Microsoft identity-platform wordt geconfigureerd via de `appsettings.json` bestand. De instellingen die u nodig hebt om in te vullen zijn:

- de cloud `Instance` als u wilt dat uw app wordt uitgevoerd in nationale clouds
- de doelgroep in `tenantId`
- de `clientId` voor uw toepassing, zoals gekopieerd vanuit Azure portal.

```JSon
{
  "AzureAd": {
    // Azure Cloud instance among:
    // "https://login.microsoftonline.com/" for Azure Public cloud.
    // "https://login.microsoftonline.us/" for Azure US government.
    // "https://login.microsoftonline.de/" for Azure AD Germany
    // "https://login.chinacloudapi.cn/" for Azure AD China operated by 21Vianet
    "Instance": "https://login.microsoftonline.com/",

    // Azure AD Audience among:
    // - the tenant Id as a a GUID obtained from the azure portal to sign-in users in your organization
    // - "organizations" to sign-in users in any work or school accounts
    // - "common" to sign-in users with any work and school account or Microsoft personal account
    // - "consumers" to sign-in users with Microsoft personal account only
    "TenantId": "[Enter the tenantId here]]",

    // Client Id (Application ID) obtained from the Azure portal
    "ClientId": "[Enter the Client Id]",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc"
  }
}
```

In de ASP.NET Core, er is een ander bestand met de URL (`applicationUrl`) en de SSL-poort (`sslPort`) voor uw toepassing, evenals verschillende profielen.

```JSon
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:3110/",
      "sslPort": 44321
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "webApp": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "http://localhost:3110/"
    }
  }
}
```

In de Azure-portal, het antwoord URI's die u nodig hebt om te registreren in de **verificatie** pagina voor uw toepassing moet overeenkomen met deze URL's; dat wil zeggen, voor de twee configuratiebestanden hierboven, zouden ze worden `https://localhost:44321/signin-oidc` als de applicationUrl is `http://localhost:3110` , maar de `sslPort` is opgegeven (44321), en de `CallbackPath` is `/signin-oidc` zoals gedefinieerd in de `appsettings.json`.
  
Op dezelfde manier, de afmeldings-URI zou worden ingesteld op `https://localhost:44321/signout-callback-oidc`.

### <a name="initialization-code"></a>Van initialisatiecode

In de ASP.NET Core Web-Apps (en Web-API's), de code doet de initialisatie van de toepassing bevindt zich in de `Startup.cs` -bestand, en om toe te voegen verificatie met de Microsoft Identity-platform (voorheen Azure AD) versie 2.0, moet u de volgende code toevoegen. De opmerkingen in de code moet spreken voor zich.

```CSharp
 services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
         .AddAzureAD(options => configuration.Bind("AzureAd", options));

 services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
 {
  // The ASP.NET core templates are currently using Azure AD v1.0, and compute
  // the authority (as {Instance}/{TenantID}). We want to use the Microsoft Identity Platform v2.0 endpoint
  options.Authority = options.Authority + "/v2.0/";

  // If you want to restrict the users that can sign-in to specific organizations
  // Set the tenant value in the appsettings.json file to 'organizations', and add the
  // issuers you want to accept to options.TokenValidationParameters.ValidIssuers collection.
  // Otherwise validate the issuer
  options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ForAadInstance(options.Authority).ValidateAadIssuer;

  // Set the nameClaimType to be preferred_username.
  // This change is needed because certain token claims from Azure AD v1.0 endpoint
  // (on which the original .NET core template is based) are different in Azure AD v2.0 endpoint.
  // For more details see [ID Tokens](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
  // and [Access Tokens](https://docs.microsoft.com/azure/active-directory/develop/access-tokens)
  options.TokenValidationParameters.NameClaimType = "preferred_username";
  ...
```

## <a name="aspnet-configuration"></a>ASP.NET-configuratie

In ASP.NET, de toepassing wordt geconfigureerd via de `Web.Config` bestand

```XML
<?xml version="1.0" encoding="utf-8"?>
<!--
  For more information on how to configure your ASP.NET application, please visit
  https://go.microsoft.com/fwlink/?LinkId=301880
  -->
<configuration>
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:ClientId" value="[Enter your client ID, as obtained from the app registration portal]" />
    <add key="ida:ClientSecret" value="[Enter your client secret, as obtained from the app registration portal]" />
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}{1}" />
    <add key="ida:RedirectUri" value="https://localhost:44326/" />
    <add key="vs:EnableBrowserLink" value="false" />
  </appSettings>
```

De code met betrekking tot verificatie in de ASP.NET-Web-app / Web-API's bevindt zich in de `App_Start/Startup.Auth.cs` bestand.

```CSharp
 public void ConfigureAuth(IAppBuilder app)
 {
  app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

  app.UseCookieAuthentication(new CookieAuthenticationOptions());

  app.UseOpenIdConnectAuthentication(
    new OpenIdConnectAuthenticationOptions
    {
     // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
     // The `Scope` describes the initial permissions that your app will need.
     //  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
     ClientId = clientId,
     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
     RedirectUri = redirectUri,
     Scope = "openid profile",
     PostLogoutRedirectUri = redirectUri,
    });
 }
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aanmelden en afmelden](scenario-web-app-sign-user-sign-in.md)
