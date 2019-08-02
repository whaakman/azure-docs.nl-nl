---
title: Web-app die gebruikers aanmeldt (code configuratie)-micro soft Identity-platform
description: Meer informatie over het bouwen van een web-app die wordt aangemeld bij gebruikers (code configuratie)
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
ms.openlocfilehash: a96d17ae7fbe94877032e7b4b2aacb63f6e070ca
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562257"
---
# <a name="web-app-that-signs-in-users---code-configuration"></a>Web-app die gebruikers code configuratie aanmeldt

Meer informatie over het configureren van de code voor uw web-app die gebruikers aanmeldt.

## <a name="libraries-used-to-protect-web-apps"></a>Bibliotheken die worden gebruikt voor het beveiligen van Web Apps

<!-- This section can be in an include for Web App and Web APIs -->
De bibliotheken die worden gebruikt voor het beveiligen van een web-app (en een web-API) zijn:

| Platform | Bibliotheek | Description |
|----------|---------|-------------|
| ![.NET](media/sample-v2-code/logo_net.png) | [Identiteits model-uitbrei dingen voor .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | Micro soft Identity Extensions voor .NET maakt rechtstreeks gebruik van ASP.NET en ASP.NET Core en stelt een set Dll's voor die beide worden uitgevoerd op .NET Framework en .NET core. Vanuit een ASP.NET/ASP.NET core-web-app kunt u token validatie beheren met de **TokenValidationParameters** -klasse (met name bij sommige ISV-scenario's) |

## <a name="aspnet-core-configuration"></a>ASP.NET Core Configuratie

Code fragmenten in dit artikel en het volgende worden geëxtraheerd uit de [Stapsgewijze zelf studie voor de ASP.net core-web-app, hoofd stuk 1](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg). Mogelijk wilt u deze zelf studie raadplegen voor volledige implementatie details.

### <a name="application-configuration-files"></a>Configuratie bestanden voor toepassingen

In ASP.net core wordt een webtoepassing die gebruikers met het micro soft Identity-platform aanmeldt via `appsettings.json` het bestand geconfigureerd. De instellingen die u moet invullen zijn:

- de Cloud `Instance` als u wilt dat uw app in nationale Clouds wordt uitgevoerd
- de doel groep in`tenantId`
- de `clientId` voor uw toepassing, zoals gekopieerd van de Azure Portal.

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
    // - the tenant Id as a GUID obtained from the azure portal to sign-in users in your organization
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

In ASP.net Core is er een ander bestand met de URL (`applicationUrl`) en de SSL-poort (`sslPort`) voor uw toepassing, evenals verschillende profielen.

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

In de Azure Portal moeten de antwoord-Uri's die u moet registreren op de **verificatie** pagina voor uw toepassing, overeenkomen met deze url's. dat wil zeggen dat voor `https://localhost:44321/signin-oidc` de twee hierboven genoemde configuratie bestanden de applicationUrl is `http://localhost:3110` , maar de `sslPort` is opgegeven (44321) en de `CallbackPath` is `/signin-oidc` gedefinieerd in de `appsettings.json`.
  
Op dezelfde manier wordt de afmeldings-URI ingesteld op `https://localhost:44321/signout-callback-oidc`.

### <a name="initialization-code"></a>Initialisatie code

In ASP.net core web apps (en Web-api's) wordt de code die de initialisatie van de toepassing uitvoert in `Startup.cs` het bestand opgeslagen en, om verificatie toe te voegen met het micro soft Identity platform (voorheen Azure AD v 2.0), moet u de volgende code toevoegen. De opmerkingen in de code moeten zichzelf verklaren.

  > [!NOTE]
  > Als u het project in Visual Studio start met het standaard ASP.net-hoofd project of `dotnet new mvc` als u `AddAzureAD` de methode gebruikt, is standaard beschikbaar omdat de gerelateerde pakketten automatisch worden geladen. Als u echter een volledig nieuw project bouwt en probeert de onderstaande code te gebruiken, raden we u aan het NuGet-pakket **' micro soft. AspNetCore. Authentication. AzureAD. UI '** toe te voegen aan `AddAzureAD` uw project om de methode beschikbaar te maken.
  
```CSharp
 services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
         .AddAzureAD(options => configuration.Bind("AzureAd", options));

 services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
 {
  // The ASP.NET core templates are currently using Azure AD v1.0, and compute
  // the authority (as {Instance}/{TenantID}). We want to use the Microsoft identity platform endpoint
  options.Authority = options.Authority + "/v2.0/";

  // If you want to restrict the users that can sign-in to specific organizations
  // Set the tenant value in the appsettings.json file to 'organizations', and add the
  // issuers you want to accept to options.TokenValidationParameters.ValidIssuers collection.
  // Otherwise validate the issuer
  options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ForAadInstance(options.Authority).ValidateAadIssuer;

  // Set the nameClaimType to be preferred_username.
  // This change is needed because certain token claims from Azure AD v1.0 endpoint
  // (on which the original .NET core template is based) are different in Microsoft identity platform endpoint.
  // For more details see [ID Tokens](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
  // and [Access Tokens](https://docs.microsoft.com/azure/active-directory/develop/access-tokens)
  options.TokenValidationParameters.NameClaimType = "preferred_username";
  ...
```

## <a name="aspnet-configuration"></a>ASP.NET-configuratie

In ASP.net wordt de toepassing via het `Web.Config` bestand geconfigureerd

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

De code die betrekking heeft op verificatie in ASP.net Web app/Web-api's bevindt zich in het `App_Start/Startup.Auth.cs` bestand.

```CSharp
 public void ConfigureAuth(IAppBuilder app)
 {
  app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

  app.UseCookieAuthentication(new CookieAuthenticationOptions());

  app.UseOpenIdConnectAuthentication(
    new OpenIdConnectAuthenticationOptions
    {
     // The `Authority` represents the identity platform endpoint - https://login.microsoftonline.com/common/v2.0
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
> [Meld u aan en af](scenario-web-app-sign-user-sign-in.md)
