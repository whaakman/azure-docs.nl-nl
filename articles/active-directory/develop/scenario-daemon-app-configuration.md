---
title: Daemon-app aanroepen van web API's (app-configuratie) - Microsoft identity-platform
description: Informatie over het bouwen van een daemon-app dat aanroepen van web-API's (app-configuratie)
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
ms.openlocfilehash: fd2da6baecdce3ab85a45347f27f573bf814445d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055766"
---
# <a name="daemon-app-that-calls-web-apis---code-configuration"></a>Daemon-app dat aanroepen van web-API's - code configureren

Informatie over het configureren van de code voor uw daemontoepassing die aanroepen van web-API's.

## <a name="msal-libraries-supporting-daemon-apps"></a>MSAL bibliotheken ondersteunende daemon-apps

De Microsoft-bibliotheken ondersteunende daemon-apps zijn:

  Bibliotheek met MSAL | Description
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Ondersteunde platforms voor het bouwen van een daemon-toepassing .NET Framework en .NET Core-platforms zijn (niet UWP Xamarin.iOS en Xamarin.Android als deze platforms worden gebruikt om openbare client-toepassingen te bouwen)
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | Ontwikkeling in voortgang: in openbare preview
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | Ontwikkeling in voortgang: in openbare preview

## <a name="configuration-of-the-authority"></a>Configuratie van de instantie

Gezien het feit dat de toepassingen die daemon gebruik geen gedelegeerde machtigingen, maar de machtigingen van de toepassing, hun *ondersteund accounttype* kan niet worden *Accounts in een organisatie-map en persoonlijke Microsoft-accounts () bijvoorbeeld, Skype, Xbox, Outlook.com)* . Er is namelijk geen tenantbeheerder toestemming om de daemon-toepassing voor persoonlijke Microsoft-accounts te verlenen. U moet kiezen *accounts in mijn organisatie* of *accounts in elke organisatie*.

Daarom moet de instantie die is opgegeven in de configuratie van de toepassing de tenant-ed (voor het opgeven van een Tenant-ID of een domeinnaam die is gekoppeld aan uw organisatie).

Als u een ISV en wilt u een hulpprogramma voor meerdere tenants bieden, kunt u `organizations`. Maar houd er rekening mee dat u moet ook uitleggen aan uw klanten het verlenen van toestemming van een beheerder. Zie [aanvragen van toestemming voor een hele tenant](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant) voor meer informatie. Er is ook op dat moment een beperking in MSAL die `organizations` is alleen toegestaan als de referenties van de client een toepassingsgeheim (niet een certificaat). Zie [MSAL.NET fout #891](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/891)

## <a name="application-configuration-and-instantiation"></a>Toepassingsconfiguratie en instantiëring

In de MSAL-bibliotheken, worden de referenties van de client (geheim of certificaat) doorgegeven als een parameter van het samenstellen van de toepassing vertrouwelijke client.

> [!IMPORTANT]
> Zelfs als uw toepassing een consoletoepassing is moet uitgevoerd als een service als een daemon-toepassing is deze een vertrouwelijke client-toepassing.

### <a name="msalnet"></a>MSAL.NET

Voeg de [Microsoft.IdentityClient](https://www.nuget.org/packages/Microsoft.Identity.Client) NuGet-pakket aan uw toepassing.

Gebruik van de naamruimte van MSAL.NET

```CSharp
using Microsoft.Identity.Client;
```

De daemontoepassing worden weergegeven door een `IConfidentialClientApplication`

```CSharp
IConfidentialClientApplication app;
```

Dit is de code voor het maken van een toepassing met een toepassingsgeheim:

```CSharp
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .WithAuthority(new Uri(config.Authority))
           .Build();
```

Dit is de code voor het maken van een toepassing met een certificaat:

```CSharp
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .WithAuthority(new Uri(config.Authority))
    .Build();
```

### <a name="msalpython"></a>MSAL.Python

```Python
# Create a preferably long-lived app instance which maintains a token cache.

app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache

    )
```

### <a name="msaljava"></a>MSAL.Java

```Java
PrivateKey key = getPrivateKey();
X509Certificate publicCertificate = getPublicCertificate();

// create clientCredential with public and private key
IClientCredential credential = ClientCredentialFactory.create(key, publicCertificate);

ConfidentialClientApplication cca = ConfidentialClientApplication
  .builder(CLIENT_ID, credential)
  .authority(AUTHORITY_MICROSOFT)
  .build();
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Daemon-app - verkrijgen van tokens voor de app.](./scenario-daemon-acquire-token.md)