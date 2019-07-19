---
title: Daemon-app die web-Api's aanroept (app-configuratie)-micro soft Identity-platform
description: Meer informatie over het bouwen van een daemon-app die web-Api's aanroept (app-configuratie)
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
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 705545fd5167087be1a001c45f58907d6ff225e8
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68277828"
---
# <a name="daemon-app-that-calls-web-apis---code-configuration"></a>Daemon-app die web-Api's aanroept-code configuratie

Meer informatie over het configureren van de code voor uw daemon-toepassing die web-Api's aanroept.

## <a name="msal-libraries-supporting-daemon-apps"></a>MSAL-bibliotheken ondersteunen daemon-apps

De micro soft libraries ondersteunen daemon-apps zijn:

  MSAL-bibliotheek | Description
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Ondersteunde platforms voor het bouwen van een daemon-toepassing zijn .NET Framework en .NET Core-platformen (niet UWP, Xamarin. iOS en Xamarin. Android als deze platformen worden gebruikt voor het bouwen van open bare client toepassingen)
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | Ontwikkeling in uitvoering-in open bare preview
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | Ontwikkeling in uitvoering-in open bare preview

## <a name="configuration-of-the-authority"></a>Configuratie van de instantie

Omdat de daemon-toepassingen geen gedelegeerde machtigingen gebruiken, maar toepassings machtigingen, worden het *ondersteunde account type* niet *accounts in een organisatorische map en persoonlijke micro soft-accounts (bijvoorbeeld Skype, Xbox, Outlook.com)* . Er is inderdaad geen Tenant beheerder om toestemming te verlenen voor de daemon-toepassing voor persoonlijke micro soft-accounts. U moet *accounts in mijn organisatie* of *accounts in een organisatie*kiezen.

Daarom moet de in de toepassings configuratie opgegeven instantie Tenant-ED zijn (een Tenant-ID of een domein naam opgeven die is gekoppeld aan uw organisatie).

Als u een ISV bent en een multi tenant hulp programma wilt bieden, kunt u gebruiken `organizations`. Maar houd er rekening mee dat u ook moet uitleggen wat uw klanten zijn om toestemming van de beheerder te verlenen. Zie [toestemming vragen voor een volledige Tenant](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant) voor meer informatie. Er is ook een beperking in MSAL die `organizations` alleen is toegestaan wanneer de client referenties een toepassings geheim zijn (niet een certificaat). Zie [MSAL.net bug #891](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/891)

## <a name="application-configuration-and-instantiation"></a>Toepassings configuratie en instantiëring

In MSAL-bibliotheken worden de client referenties (geheim of certificaat) door gegeven als para meter van de client toepassings constructie.

> [!IMPORTANT]
> Zelfs als uw toepassing een console toepassing is die als een service wordt uitgevoerd, en als het een daemon-toepassing is, moet deze een vertrouwelijke client toepassing zijn.

### <a name="msalnet"></a>MSAL.NET

Voeg het [micro soft. IdentityClient](https://www.nuget.org/packages/Microsoft.Identity.Client) NuGet-pakket toe aan uw toepassing.

MSAL.NET-naam ruimte gebruiken

```CSharp
using Microsoft.Identity.Client;
```

De daemon-toepassing wordt weer gegeven door een`IConfidentialClientApplication`

```CSharp
IConfidentialClientApplication app;
```

Hier volgt de code voor het bouwen van een toepassing met een toepassings geheim:

```CSharp
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .WithAuthority(new Uri(config.Authority))
           .Build();
```

Hier volgt de code voor het bouwen van een toepassing met een certificaat:

```CSharp
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .WithAuthority(new Uri(config.Authority))
    .Build();
```

Ten slotte, in plaats van een client geheim of een certificaat, kan de vertrouwelijke client toepassing ook de identiteit bewijzen met behulp van client verklaringen. Dit geavanceerde scenario wordt beschreven in [client verklaringen](msal-net-client-assertions.md)


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
> [Daemon-app-tokens ophalen voor de app](./scenario-daemon-acquire-token.md)
