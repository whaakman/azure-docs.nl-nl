---
title: Een token op de achtergrond ophalen (Microsoft Authentication Library voor .NET) | Azure
description: Informatie over het verkrijgen van een toegangstoken (van de token cache) op de achtergrond met behulp van de Microsoft Authentication Library voor .NET (MSAL.NET).
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
ms.openlocfilehash: 6331407067a39550d866d7c293a92fac9184b54e
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544240"
---
# <a name="get-a-token-from-the-token-cache-using-msalnet"></a>Een token van de token cache met behulp van MSAL.NET verkrijgen

Wanneer u een toegangstoken met behulp van Microsoft Authentication Library voor .NET (MSAL.NET) aanschaft, wordt het token in cache opgeslagen. Wanneer de toepassing een token moet, moet deze eerst aanroepen de `AcquireTokenSilent` methode om te controleren of als een acceptabele token in de cache. In veel gevallen is het mogelijk om toegang te verkrijgen van een ander token met meer bereiken op basis van een token in de cache. Het is ook mogelijk om te vernieuwen van een token wanneer dit het ophalen van verloopt bijna (zoals de tokencache ook een vernieuwingstoken bevat).

De aanbevolen patroon is om aan te roepen de `AcquireTokenSilent` methode eerste.  Als `AcquireTokenSilent` mislukt, klikt u vervolgens een token met andere methoden verkrijgen.

In het volgende voorbeeld wordt de toepassing eerst geprobeerd om een token uit de cache-token te verkrijgen.  Als een `MsalUiRequiredException` uitzondering is opgetreden, de toepassing een token interactief verkrijgt. 

```csharp
AuthenticationResult result = null;
var accounts = await app.GetAccountsAsync();

try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
        .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
 // A MsalUiRequiredException happened on AcquireTokenSilentAsync.
 // This indicates you need to call AcquireTokenAsync to acquire a token
 System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

 try
 {
    result = await app.AcquireTokenInteractive(scopes)
          .ExecuteAsync();
 }
 catch (MsalException msalex)
 {
    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
 }
}
catch (Exception ex)
{
 ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
 return;
}

if (result != null)
{
 string accessToken = result.AccessToken;
 // Use the token
}
```