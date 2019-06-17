---
title: Het token cache wissen met behulp van Microsoft Authentication Library voor .NET - Azure
description: Informatie over het wissen van de token cache met behulp van de Microsoft Authentication Library voor .NET (MSAL.NET).
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
ms.date: 05/07/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6763c6b2b1f9b4de7d8669a50a4979a7aac00c7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65544122"
---
# <a name="clear-the-token-cache-using-msalnet"></a>Wis de cache van het token met behulp van MSAL.NET

Wanneer u [een toegangstoken verkrijgen](msal-acquire-cache-tokens.md) met behulp van Microsoft Authentication Library voor .NET (MSAL.NET), het token is opgeslagen in de cache. Wanneer de toepassing een token moet, moet deze eerst aanroepen de `AcquireTokenSilent` methode om te controleren of als een acceptabele token in de cache. 

De cache wissen wordt bereikt door het verwijderen van de accounts uit de cache. De sessiecookie die zich in de browser, maar wordt niet verwijderd.  In het volgende voorbeeld wordt een openbare clienttoepassing, Hiermee haalt u de accounts voor de toepassing en verwijdert u de accounts.

```csharp
private readonly IPublicClientApplication _app;
private static readonly string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
private static readonly string Authority = string.Format(CultureInfo.InvariantCulture, AadInstance, Tenant);

_app = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(Authority)
                .Build();

var accounts = (await _app.GetAccountsAsync()).ToList();

// clear the cache
while (accounts.Any())
{
   await _app.RemoveAsync(accounts.First());
   accounts = (await _app.GetAccountsAsync()).ToList();
}

```

Lees voor meer informatie over het ophalen en opslaan in cache tokens [een toegangstoken verkrijgen](msal-acquire-cache-tokens.md).
