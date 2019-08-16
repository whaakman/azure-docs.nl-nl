---
title: Bereiken voor een v 1.0-toepassing (micro soft-verificatie bibliotheek) | Azure
description: Meer informatie over de bereiken voor een v 1.0-toepassing met behulp van de micro soft Authentication Library (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/23/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0590614e1c1bc7331246e76fa26a6567a05324e6
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532335"
---
# <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>Bereiken voor een web-API die v 1.0-tokens accepteert

OAuth2 machtigingen zijn machtigingen die een toepassing van Azure AD voor ontwikkel aars (v 1.0) Web-API (resource) beschikbaar maakt voor client toepassingen. Deze machtigings bereiken kunnen tijdens de toestemming aan client toepassingen worden verleend. Zie de sectie over `oauth2Permissions` de naslag informatie over het [Azure Active Directory-toepassings manifest](reference-app-manifest.md#manifest-reference).

## <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>Bereiken voor het aanvragen van toegang tot specifieke OAuth2-machtigingen van een v 1.0-toepassing
Als u tokens wilt verkrijgen voor specifieke bereiken van een v 1.0-toepassing (bijvoorbeeld de Azure AD-grafiek, die https:\//Graph.Windows.net is), moet u scopes maken door een gewenste resource-id met een gewenste OAuth2-machtiging samen te voegen. voor die bron.

Bijvoorbeeld voor toegang namens de gebruiker een v 1.0 Web API waarbij de URI van de App-ID is `ResourceId`:

```csharp
var scopes = new [] {  ResourceId+"/user_impersonation"};
```

```javascript
var scopes = [ ResourceId + "/user_impersonation"];
```

Als u wilt lezen en schrijven met MSAL.net Azure Active Directory met behulp van de Azure AD Graph API (\/https:/Graph.Windows.net/), maakt u een lijst met bereiken, zoals in het volgende:

```csharp
string ResourceId = "https://graph.windows.net/";
var scopes = new [] { ResourceId + "Directory.Read", ResourceID + "Directory.Write"}
```

```javascript
var ResourceId = "https://graph.windows.net/";
var scopes = [ ResourceId + "Directory.Read", ResourceID + "Directory.Write"];
```

Als u het bereik wilt schrijven dat overeenkomt met de Azure Resource Manager-API (\/https:/Management.core.Windows.net/), moet u het volgende bereik aanvragen (Let op de twee slashes):

```csharp
var scopes = new[] {"https://management.core.windows.net//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.azure.com/subscriptions?api-version=2016-09-01
```

> [!NOTE]
> U moet twee slashes gebruiken omdat de Azure Resource Manager-API een slash in de claim van de doel groep (AUD) verwacht en vervolgens een slash is om de API-naam van het bereik te scheiden.

De logica die door Azure AD wordt gebruikt, is als volgt:

- Voor ADAL (v 1.0)-eind punt met een v 1.0-toegangs token (alleen mogelijk), AUD = resource
- Voor MSAL (micro soft Identity platform (v 2.0)-eind punt) waarbij een toegangs token wordt gevraagd voor een resource die v 2.0-tokens accepteert, AUD = resource. AppId
- Voor MSAL (v 2.0-eind punt) waarbij een toegangs token wordt gevraagd voor een resource die een v 1.0-toegangs token accepteert (dit is het geval hierboven), parseert Azure AD de gewenste doel groep uit het aangevraagde bereik door alles vóór de laatste slash te nemen en deze als de resource-id te gebruiken. Als https:\//database.Windows.net verwacht dat het doel groep https:\//database.Windows.net/is, moet u daarom een scope van ' https:\//database.Windows.net//.default ' aanvragen. Zie ook github issue [#747: De afsluitende slash van de bron-URL wordt wegge laten, waardoor](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747)SQL-verificatie is mislukt.

## <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>Bereiken om toegang aan te vragen tot alle machtigingen van een v 1.0-toepassing
Als u een token wilt verkrijgen voor alle statische bereiken van een v 1.0-toepassing, voegt u '. default ' toe aan de URI van de App-ID van de API:

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

```javascript
var ResourceId = "someAppIDURI";
var scopes = [ ResourceId + "/.default"];
```

## <a name="scopes-to-request-for-client-credential-flow--daemon-app"></a>Bereiken voor het aanvragen van een client referentie stroom/daemon-app
In het geval van een client referentie stroom is `/.default`het bereik dat moet worden door gegeven ook. Dit geeft aan Azure AD: ' alle machtigingen op app-niveau die de beheerder heeft ingestemd in de registratie van de toepassing.
