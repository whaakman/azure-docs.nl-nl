---
title: Bereiken voor een toepassing v1.0 (Microsoft Authentication Library) | Azure
description: Meer informatie over de bereiken voor een v1.0-toepassing met behulp van de Microsoft Authentication Library (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/23/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 44aad6b2fab7e0ab2ff11d8469782b001b1f4d18
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545903"
---
# <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>Bereiken voor een Web-API v1.0 tokens accepteren

OAuth2-machtigingen zijn machtigingsbereiken die een Azure Active Directory voor ontwikkelaars (v1.0) web-API (resource)-toepassing beschikbaar aan clienttoepassingen stelt. Dit bereik aan machtigingen kunnen tijdens toestemming worden verleend aan clienttoepassingen. Zie de sectie over `oauth2Permissions` in de [Azure Active Directory application manifest verwijzing](reference-app-manifest.md#manifest-reference).

## <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>Bereiken voor het aanvragen van toegang tot specifieke OAuth2-machtigingen van een toepassing v1.0
Als u wilt verkrijgen van tokens voor specifieke scopes van een toepassing v1.0 (bijvoorbeeld de Azure AD graph, die is https://graph.windows.net), moet u bereiken door het samenvoegen van een gewenste resource-id met een gewenste OAuth2-machtiging voor deze resource te maken.

Bijvoorbeeld, voor toegang tot namens de gebruiker een web-API van v1.0 waar de app-ID-URI is `ResourceId`:

```csharp
var scopes = new [] {  ResourceId+"/user_impersonation"};
```

```javascript
var scopes = [ ResourceId + "/user_impersonation"];
```

Als u wilt lezen en schrijven met MSAL.NET Azure Active Directory met behulp van de Azure AD graph API (https://graph.windows.net/), maakt u een lijst met scopes zoals in het volgende:

```csharp
string ResourceId = "https://graph.windows.net/";
var scopes = new [] { ResourceId + "Directory.Read", ResourceID + "Directory.Write"}
```

```javascript
var ResourceId = "https://graph.windows.net/";
var scopes = [ ResourceId + "Directory.Read", ResourceID + "Directory.Write"];
```

Als u wilt schrijven van het bereik dat overeenkomt met de Azure Resource Manager-API (https://management.core.windows.net/), moet u het volgende bereik (Houd er rekening mee de twee slashes) van aanvraag:

```csharp
var scopes = new[] {"https://management.core.windows.net//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.azure.com/subscriptions?api-version=2016-09-01
```

> [!NOTE]
> U moet twee slashes gebruiken omdat de API van Azure Resource Manager een schuine streep in de doelgroep-claim (aud verwacht) en vervolgens er een slash is voor het scheiden van de naam van de API van het bereik.

De logica die wordt gebruikt door Azure AD is het volgende:

- Voor het eindpunt van de ADAL (v1.0) met een v1.0 access token (de enige mogelijke), aud resource =
- Voor MSAL (Microsoft identity-platform (v2.0) eindpunt), waarin wordt gevraagd een toegangstoken voor een resource accepteren v2.0-tokens, aud = resource. Toepassings-id
- Azure AD wordt de gewenste doelgroep van het aangevraagde bereik voor MSAL (v2.0-eindpunt) waarin wordt gevraagd een toegangstoken voor een resource een toegangstoken v1.0 (dit is de bovenstaande aanvraag) accepteren, geparseerd door te nemen alles vóór de laatste slash en als de resource-id gebruiken. Dus als https://database.windows.net wordt verwacht dat een een publiek van 'https://database.windows.net/', moet u een bereik van de aanvraag'https://database.windows.net//.default'. Zie ook GitHub probleem [#747: Bron-url afsluitende schuine streep wordt weggelaten, waardoor sql-verificatie mislukt](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747).

## <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>Bereiken voor het aanvragen van toegang tot de machtigingen van een toepassing v1.0
Als u een token verkrijgen voor de statische bereiken van een toepassing v1.0 wilt, moet u '.standaard' toevoegen aan de ID-URI van de API-app:

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

```javascript
var ResourceId = "someAppIDURI";
var scopes = [ ResourceId + "/.default"];
```

## <a name="scopes-to-request-for-client-credential-flow--daemon-app"></a>Bereiken om aan te vragen voor client referentie-gegevensstroom / daemon-app
In het geval van een client referentie-gegevensstroom, het bereik om door te geven ook worden `/.default`. Hiermee wordt aangegeven met Azure AD: "alle app-niveau machtigingen die de beheerder heeft ingestemd met de in de registratie van de toepassing.