---
title: Exemplaar maken van een vertrouwelijke client-app met opties (Microsoft Authentication Library voor .NET) | Azure
description: Informatie over het instantiëren van een vertrouwelijke client-toepassing met configuratie-opties met behulp van de Microsoft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f935b1b2815501710444e3f921a157ba02e3215
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544090"
---
# <a name="instantiate-a-confidential-client-application-with-configuration-options-using-msalnet"></a>Het instantiëren van een vertrouwelijke client-toepassing met configuratie-opties met behulp van MSAL.NET

In dit artikel wordt beschreven hoe u exemplaar maken van een [vertrouwelijke clienttoepassing](msal-client-applications.md) met behulp van Microsoft Authentication Library voor .NET (MSAL.NET).  De toepassing wordt gestart met configuratie-opties die zijn gedefinieerd in een bestand met instellingen.

Voordat u het initialiseren van een toepassing, moet u eerst [registreren](quickstart-register-app.md) deze zodat uw app kan worden geïntegreerd met het Microsoft identity-platform. Na de registratie moet u mogelijk de volgende informatie (die kan worden gevonden in de Azure-portal):

- De client-ID (een tekenreeks die een GUID)
- De URL van de id-provider (met de naam van het exemplaar) en de doelgroep aanmelden voor uw toepassing. Deze twee parameters bekend staan als de instantie.
- De tenant-ID als u een line-of-business-toepassing uitsluitend voor uw organisatie (ook met de naam één tenant toepassing ontwikkelt).
- Het toepassingsgeheim (client geheime tekenreeks) of het certificaat (van het type X509Certificate2) als het een vertrouwelijke client-app.
- Voor web-apps, en soms voor openbare client-apps (met name wanneer uw app nodig heeft om te gebruiken een broker), hebt u instellen de redirectUri waar de id-provider contact op met back-uw toepassing met de beveiligingstokens.

## <a name="configure-the-application-from-the-config-file"></a>De toepassing uit het configuratiebestand configureren
De naam van de eigenschappen van de opties in MSAL.NET overeenkomen met de naam van de eigenschappen van de `AzureADOptions` in ASP.NET Core, dus u hoeft te schrijven geen glue-code.

De configuratie van een ASP.NET Core-toepassing wordt beschreven in een *appsettings.json* bestand:

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "[Enter the domain of your tenant, e.g. contoso.onmicrosoft.com]",
    "TenantId": "[Enter 'common', or 'organizations' or the Tenant Id (Obtained from the Azure portal. Select 'Endpoints' from the 'App registrations' blade and use the GUID in any of the URLs), e.g. da41245a5-11b3-996c-00a8-4d99re19f292]",
    "ClientId": "[Enter the Client Id (Application ID obtained from the Azure portal), e.g. ba74781c2-53c2-442a-97c2-3d60re42f403]",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc",

    "ClientSecret": "[Copy the client secret added to the app from the Azure portal]"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

U kunt uw toepassing vertrouwelijke client uit het configuratiebestand vanaf MSAL.NET v3.x biedt configureren. De klassen die betrekking hebben op de app-configuratie bevinden zich in de `Microsoft.Identity.Client.AppConfig` naamruimte.

In de klasse waar u wilt configureren en exemplaar maken van uw toepassing, moet u declareert een `ConfidentialClientApplicationOptions` object.  De configuratie van de bron (met inbegrip van het bestand appconfig.json) koppelen aan het exemplaar van de opties voor de toepassing:

```csharp
using Microsoft.Identity.Client.AppConfig;

private ConfidentialClientApplicationOptions _applicationOptions;
_applicationOptions = new ConfidentialClientApplicationOptions();
configuration.Bind("AzureAD", _applicationOptions);
```

Hierdoor kunnen de inhoud van de sectie 'AzureAD' van de *appsettings.json* bestand om te worden gekoppeld aan de bijbehorende eigenschappen van de `ConfidentialClientApplicationOptions` object.  Vervolgens maken een `ConfidentialClientApplication` object:

```csharp
IConfidentialClientApplication app;
app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
        .Build();
```

## <a name="add-runtime-configuration"></a>Runtimeconfiguratie toevoegen
In een vertrouwelijke client-toepassing hebt u meestal een cache per gebruiker. Daarom moet u de cache die is gekoppeld aan de gebruiker ophalen en de opbouwfunctie voor de toepassing te informeren dat u wilt gebruiken. Op dezelfde manier, u mogelijk een dynamisch berekende omleidings-URI. In dit geval is de code het volgende:

```csharp
IConfidentialClientApplication app;
var request = httpContext.Request;
var currentUri = UriHelper.BuildAbsolute(request.Scheme, request.Host, request.PathBase, _azureAdOptions.CallbackPath ?? string.Empty);
app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
       .WithRedirectUri(currentUri)
       .Build();
TokenCache userTokenCache = _tokenCacheProvider.SerializeCache(app.UserTokenCache,httpContext, claimsPrincipal);
```

