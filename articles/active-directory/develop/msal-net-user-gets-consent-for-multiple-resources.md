---
title: Er toestemming voor verschillende resources (Microsoft Authentication Library voor .NET) | Azure
description: Ontdek hoe een gebruiker vooraf toestemming voor verschillende resources met behulp van de Microsoft Authentication Library voor .NET (MSAL.NET) kan ontvangen.
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
ms.date: 04/30/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45bad9a03e8eff6f22ebb99fd2ef4bcd5fecf9b5
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65158818"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>Gebruiker toestemming voor verschillende resources met behulp van MSAL.NET opgehaald
Het eindpunt van de Microsoft identity platform staat niet toe dat u een token verkrijgen voor meerdere resources tegelijk. Wanneer u de Microsoft Authentication Library voor .NET (MSAL.NET), moet de parameter bereiken in de ophalen-tokenmethode bereiken voor één resource alleen bevatten. Echter, u kunt vooraf toestemming voor de verschillende resources vooraf door op te geven aanvullende bereiken met behulp van de `.WithExtraScopeToConsent` builder-methode.

> [!NOTE]
> Ophalen van toestemming voor verschillende bronnen werken voor Microsoft identity-platform, maar niet voor Azure AD B2C. Azure AD B2C ondersteunt alleen beheerderstoestemming, niet de toestemming van de gebruiker.

Bijvoorbeeld, hebt u twee resources met scopes 2 elk:

- https://mytenant.onmicrosoft.com/customerapi (met 2 scopes `customer.read` en `customer.write`)
- https://mytenant.onmicrosoft.com/vendorapi (met 2 scopes `vendor.read` en `vendor.write`)

Moet u de `.WithExtraScopeToConsent` modifier waarvoor de *extraScopesToConsent* parameter zoals wordt weergegeven in het volgende voorbeeld:

```csharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

Hiermee verkrijgt u een toegangstoken voor de eerste web-API. Wanneer u nodig hebt voor toegang tot de tweede web-API kunt u vervolgens op de achtergrond het token uit de cache-token verkrijgen:

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```
