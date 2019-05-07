---
title: Initialiseren van de client-toepassingen (Microsoft Authentication Library voor .NET) | Azure
description: Meer informatie over het initialiseren van openbare client en vertrouwelijke client-toepassingen met behulp van de Microsoft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6197d472bdfaf03c9f99baa7691354e735cc91e
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075803"
---
# <a name="initialize-client-applications-using-msalnet"></a>Client-toepassingen met behulp van MSAL.NET initialiseren
Dit artikel wordt beschreven tijdens de initialisatie van openbare client en vertrouwelijke client-toepassingen met behulp van Microsoft Authentication Library voor .NET (MSAL.NET).  Lees voor meer informatie over de client toepassingstypen en opties voor toepassingsconfiguraties de [overzicht](msal-client-applications.md).

Met MSAL.NET 3.x, de aanbevolen manier om het instantiëren van een toepassing is met behulp van de toepassing builders: `PublicClientApplicationBuilder` en `ConfidentialClientApplicationBuilder`. Ze bieden een krachtig mechanisme voor het configureren van de toepassing van de code of uit een configuratiebestand of zelfs niet door een combinatie van beide methoden.

## <a name="prerequisites"></a>Vereisten
Voordat u het initialiseren van een toepassing, moet u eerst [registreren](quickstart-register-app.md) zodat uw app kan worden geïntegreerd met het Microsoft identity-platform.  Na de registratie moet u mogelijk de volgende informatie (die kan worden gevonden in de Azure-portal):

- De client-ID (een tekenreeks die een GUID)
- De URL van de id-provider (met de naam van het exemplaar) en de doelgroep aanmelden voor uw toepassing. Deze twee parameters bekend staan als de instantie.
- De tenant-ID als u een line-of-business-toepassing uitsluitend voor uw organisatie (ook met de naam één tenant toepassing ontwikkelt).
- Het toepassingsgeheim (client geheime tekenreeks) of het certificaat (van het type X509Certificate2) als het een vertrouwelijke client-app.
- Voor web-apps, en soms voor openbare client-apps (met name wanneer uw app nodig heeft om te gebruiken een broker), hebt u instellen de redirectUri waar de id-provider contact op met back-uw toepassing met de beveiligingstokens.

## <a name="ways-to-initialize-applications"></a>Manieren om te initialiseren van toepassingen
Er zijn veel verschillende manieren voor het starten van clienttoepassingen.

### <a name="initializing-a-public-client-application-from-code"></a>Tijdens de initialisatie van een openbare client-toepassing via code

De volgende code waarmee gebruikers aanmelden in de openbare cloud van Microsoft Azure met hun werk en schoolaccounts, of hun persoonlijke Microsoft-account wordt een openbare client-toepassing.

```csharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId)
    .Build();
```

### <a name="initializing-a-confidential-client-application-from-code"></a>Tijdens de initialisatie van een vertrouwelijke client-toepassing via code

Op dezelfde manier, de volgende code wordt een vertrouwelijk-toepassing (een Web-app zich bevindt in `https://myapp.azurewebsites.net`) verwerken van tokens van gebruikers in de openbare cloud van Microsoft Azure met hun werk en schoolaccounts, of hun persoonlijke Microsoft-account. De toepassing wordt geïdentificeerd met de id-provider door het delen van een clientgeheim:

```csharp
string redirectUri = "https://myapp.azurewebsites.net";
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithClientSecret(clientSecret)
    .WithRedirectUri(redirectUri )
    .Build();
```

Als u mogelijk kent, in de productieomgeving, in plaats van met een clientgeheim, kunt u Azure AD deelt met een certificaat. De code worden dan het volgende:

```csharp
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithCertificate(certificate)
    .WithRedirectUri(redirectUri )
    .Build();
```

### <a name="initializing-a-public-client-application-from-configuration-options"></a>Tijdens de initialisatie van een openbare clienttoepassing van configuratie-opties

De volgende code wordt een openbare client-toepassing van een configuratieobject, dat kan worden ingevuld via een programma of gelezen uit een configuratiebestand:

```csharp
PublicClientApplicationOptions options = GetOptions(); // your own method
IPublicClientApplication app = PublicClientApplicationBuilder.CreateWithApplicationOptions(options)
    .Build();
```

### <a name="initializing-a-confidential-client-application-from-configuration-options"></a>Tijdens de initialisatie van een vertrouwelijke client-toepassing van configuratie-opties

Hetzelfde type patroon is van toepassing op vertrouwelijke client-toepassingen. U kunt ook andere met behulp van parameters toevoegen `.WithXXX` parameters (hier een certificaat).

```csharp
ConfidentialClientApplicationOptions options = GetOptions(); // your own method
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(options)
    .WithCertificate(certificate)
    .Build();
```

## <a name="builder-modifiers"></a>Opbouwfunctie voor parameters

In de codefragmenten die met behulp van builders van toepassing, een aantal `.With` methoden kunnen worden toegepast als parameters (bijvoorbeeld `.WithCertificate` en `.WithRedirectUri`). 

### <a name="modifiers-common-to-public-and-confidential-client-applications"></a>Parameters voor openbare en vertrouwelijke client-toepassingen

De parameters die u op een openbare client of een vertrouwelijke client toepassing builder instellen kunt zijn:

|Parameter | Description|
|--------- | --------- |
|`.WithAuthority()` 7 onderdrukkingen | Hiermee stelt u de standaard-instantie van toepassing op een Azure AD-instantie met de mogelijkheid voor het kiezen van de Azure-Cloud, de doelgroep, de tenant (tenant-ID of domain name), of door rechtstreeks de instantie-URI.|
|`.WithAdfsAuthority(string)` | Hiermee stelt u de standaard-instantie van de toepassing moet een AD FS-instantie.|
|`.WithB2CAuthority(string)` | Hiermee stelt u de standaard-instantie van de toepassing moet een Azure AD B2C-instantie.|
|`.WithClientId(string)` | Onderdrukt de client-ID.|
|`.WithComponent(string)` | Hiermee stelt u de naam van de bibliotheek met behulp van MSAL.NET (omwille van de telemetrie). |
|`.WithDebugLoggingCallback()` | Als met de naam, de toepassing moet worden gebeld `Debug.Write` gewoon inschakelen foutopsporingsgegevens. Zie [logboekregistratie](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/logging) voor meer informatie.|
|`.WithExtraQueryParameters(IDictionary<string,string> eqp)` | De parameters van toepassing op extra query die wordt verzonden in alle verificatieaanvraag instellen. Dit is overschrijfbare op elk niveau van de methode ophalen van tokens (met dezelfde `.WithExtraQueryParameters pattern`).|
|`.WithHttpClientFactory(IMsalHttpClientFactory httpClientFactory)` | Hiermee schakelt u geavanceerde scenario's zoals het configureren van een HTTP-proxy of als u wilt afdwingen dat MSAL het gebruik van een bepaalde HttpClient (bijvoorbeeld in ASP.NET Core web-apps /-API's).|
|`.WithLogging()` | Als ze worden aangeroepen, wordt de toepassing een callback bij foutopsporing in traceringen aanroepen. Zie [logboekregistratie](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/logging) voor meer informatie.|
|`.WithRedirectUri(string redirectUri)` | Onderdrukt de standaard-omleidings-URI. In het geval van openbare clienttoepassingen is dit handig voor scenario's met betrekking tot de broker.|
|`.WithTelemetry(TelemetryCallback telemetryCallback)` | Hiermee stelt u de gemachtigde die wordt gebruikt om telemetrie te verzenden.|
|`.WithTenantId(string tenantId)` | Onderdrukt de tenant-ID of beschrijving van de tenant.|

### <a name="modifiers-specific-to-xamarinios-applications"></a>Parameters die specifiek zijn voor Xamarin.iOS-toepassingen

De parameters die u voor een openbare client toepassing builder op Xamarin.iOS instellen kunt zijn:

|Parameter | Description|
|--------- | --------- |
|`.WithIosKeychainSecurityGroup()` | **Alleen Xamarin.iOS**: Hiermee stelt u de beveiligingsgroep van de iOS-sleutelhanger (voor de cachepersistentie).|

### <a name="modifiers-specific-to-confidential-client-applications"></a>Parameters die specifiek zijn voor vertrouwelijke client-toepassingen

De parameters die u voor een toepassing vertrouwelijke client builder instellen kunt zijn:

|Parameter | Description|
|--------- | --------- |
|`.WithCertificate(X509Certificate2 certificate)` | Hiermee stelt u het certificaat voor het identificeren van de toepassing met Azure AD.|
|`.WithClientSecret(string clientSecret)` | Hiermee stelt u het identificeren van de toepassing met Azure AD clientgeheim (app-wachtwoord).|

Deze parameters zijn sluiten elkaar wederzijds uit. Als u beide opgeeft, genereert MSAL een zinvolle uitzondering.

### <a name="example-of-usage-of-modifiers"></a>Voorbeeld van het gebruik van parameters

We gaan ervan uit dat uw toepassing een line-of-business-toepassing alleen bestemd voor uw organisatie is.  U kunt vervolgens het volgende schrijven:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAadAuthority(AzureCloudInstance.AzurePublic, tenantId)
        .Build();
```

Waar wordt deze interessante is dat er nu programmeren voor nationale clouds is vereenvoudigd. Als u wilt dat uw toepassing moet een toepassing met meerdere tenants in een nationale cloud, u kunt schrijven, bijvoorbeeld:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAadAuthority(AzureCloudInstance.AzureUsGovernment, AadAuthorityAudience.AzureAdMultipleOrgs)
        .Build();
```

Er is ook een onderdrukking voor de AD FS (AD FS 2019 wordt momenteel niet ondersteund):
```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAdfsAuthority("https://consoso.com/adfs")
        .Build();
```

Ten slotte, als u een Azure AD B2C-ontwikkelaar bent, kunt u uw tenant als volgt:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithB2CAuthority("https://fabrikamb2c.b2clogin.com/tfp/{tenant}/{PolicySignInSignUp}")
        .Build();
```
