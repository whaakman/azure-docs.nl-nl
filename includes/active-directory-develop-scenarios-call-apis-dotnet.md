---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 0196d39f5b131bc54e00412beb7fdf10b7352336
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176218"
---
### <a name="authenticationresult-properties-in-msalnet"></a>Eigenschappen van AuthenticationResult in MSAL.NET

Methoden voor het verkrijgen van tokens retourneren een `AuthenticationResult` (of, voor het async-methoden een `Task<AuthenticationResult>`.

In MSAL.NET, `AuthenticationResult` exposes:

- `AccessToken` voor de Web-API voor toegang tot resources. Deze parameter een tekenreeks is, meestal een met base64 gecodeerde JWT maar de client moet nooit zoeken binnen het toegangstoken. De indeling wordt niet gegarandeerd blijft stabiel en deze kan worden versleuteld voor de resource. Gebruikers schrijven van code, afhankelijk van de token toegang tot inhoud op de client is een van de grootste bronnen van fouten en client logische onderbrekingen. Zie ook [toegangstokens](../articles/active-directory/develop/access-tokens.md)
- `IdToken` voor de gebruiker (deze parameter is een gecodeerde JWT). Zie [ID-Tokens](../articles/active-directory/develop/id-tokens.md)
- `ExpiresOn` Geeft de datum en tijd wanneer het token is verlopen
- `TenantId` bevat de tenant waarin de gebruiker is gevonden. Gastgebruikers (Azure AD B2B-scenario's) is de Tenant-ID de Gast-tenant, niet de unieke tenant.
Wanneer het token wordt doorgegeven voor een gebruiker `AuthenticationResult` bevat ook informatie over deze gebruiker. Voor vertrouwelijke client stromen waar tokens worden aangevraagd zonder gebruiker (voor de toepassing), is deze gebruikersgegevens null.
- De `Scopes` voor het token is uitgegeven.
- De unieke Id voor de gebruiker.

### <a name="iaccount"></a>IAccount

MSAL.NET definieert het concept van Account (via de `IAccount` interface). Deze belangrijke wijziging biedt de juiste semantiek: het feit dat dezelfde gebruiker diverse accounts in verschillende Azure hebben kunt AD-mappen. MSAL.NET biedt ook betere informatie in het geval van Gast-scenario's, zoals thuis accountinformatie wordt verstrekt.
Het volgende diagram toont de structuur van de `IAccount` interface:

![image](https://user-images.githubusercontent.com/13203188/44657759-4f2df780-a9fe-11e8-97d1-1abbffade340.png)

De `AccountId` klasse identificeert een account in een specifieke tenant. Het heeft de volgende eigenschappen:

| Eigenschap | Description |
|----------|-------------|
| `TenantId` | Een tekenreeksweergave van een GUID, is de ID van de tenant waarin het account zich bevindt. |
| `ObjectId` | Een tekenreeksweergave van een GUID, is de ID van de gebruiker die eigenaar is van het account in de tenant. |
| `Identifier` | De unieke id voor het account. `Identifier` is van de samenvoeging van `ObjectId` en `TenantId` gescheiden door een komma en zijn niet base64-gecodeerd. |

De `IAccount` interface Hiermee geeft u informatie over één account. Dezelfde gebruiker kan worden gebruikt in verschillende tenants, dat wil zeggen, een gebruiker meerdere accounts kan hebben. De leden zijn:

| Eigenschap | Description |
|----------|-------------|
| `Username` | Een tekenreeks met de waarde weer te geven in de UserPrincipalName (UPN)-indeling, bijvoorbeeld john.doe@contoso.com. Deze tekenreeks mag null is, terwijl de HomeAccountId en HomeAccountId.Identifier niet null zijn. Deze eigenschap wordt vervangen door de `DisplayableId` eigenschap van `IUser` in eerdere versies van MSAL.NET. |
| `Environment` | Een tekenreeks met de id-provider voor dit account, bijvoorbeeld `login.microsoftonline.com`. Deze eigenschap wordt vervangen door de `IdentityProvider` eigenschap van `IUser`, behalve dat `IdentityProvider` ook informatie over de tenant (naast de cloudomgeving), was dat hier de waarde alleen de host is. |
| `HomeAccountId` | AccountId van het oorspronkelijke account voor de gebruiker. Deze eigenschap identificatie unieke van de gebruiker in Azure AD-tenants. |

### <a name="using-the-token-to-call-a-protected-api"></a>Een beveiligde API aan te roepen met behulp van het token

Zodra de `AuthenticationResult` is geretourneerd door MSAL (in `result`), moet u deze toevoegen aan de HTTP-autorisatie-header, voordat u de aanroep voor toegang tot de beveiligde Web-API.

```CSharp
httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```