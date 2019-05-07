---
title: Web-app die aanroepen van web-API's (een token voor de app ophalen) - Microsoft identity-platform
description: Informatie over het bouwen van een Web-app die aanroepen van web-API's (ophalen van een token voor de app)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
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
ms.openlocfilehash: 653db995000308bb3ef78a9183696cd9d8ed1056
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074798"
---
# <a name="web-app-that-calls-web-apis---acquire-a-token-for-the-app"></a>Web-app die web-API's aanroept: een token verkrijgen voor de app.

Nu u hebt u gebouwd toepassingsobject client, gebruikt u deze om een token dat u vervolgens gebruiken voor het aanroepen van een web-API's te verkrijgen. In ASP.NET of ASP.NET Core, moet u een web-API wordt vervolgens uitgevoerd in de netwerkcontroller aanroept. Dit gaat over:

- Ophalen van een token voor de web-API met behulp van de tokencache. Voor deze, die u aanroept `AcquireTokenSilent`
- Aanroepen van de beveiligde API met het toegangstoken

## <a name="aspnet-core"></a>ASP.NET Core

De controllermethoden worden beveiligd door een `[Authorize]` kenmerk die ervoor zorgt gebruikers dat uit voor het gebruik van de Web-App die wordt geverifieerd. Hier is de code die Microsoft Graph aanroepen

```CSharp
[Authorize]
public class HomeController : Controller
{
 ...
}
```

Dit is een vereenvoudigde code van de actie van de HomeController Hiermee haalt u een token voor het aanroepen van de Microsoft Graph.

```CSharp
public async Task<IActionResult> Profile()
{
 var application = BuildConfidentialClientApplication(HttpContext, HttpContext.User);
 string accountIdentifier = claimsPrincipal.GetMsalAccountId();
 string loginHint = claimsPrincipal.GetLoginHint();

 // Get the account
 IAccount account = await application.GetAccountAsync(accountIdentifier);

 // Special case for guest users as the Guest iod / tenant id are not surfaced.
 if (account == null)
 {
  var accounts = await application.GetAccountsAsync();
  account = accounts.FirstOrDefault(a => a.Username == loginHint);
 }

 AuthenticationResult result;
 result = await application.AcquireTokenSilent(new []{"user.read"}, account)
                            .ExecuteAsync();
 var accessToken = result.AccessToken;
 ...
 // use the access token to call a web API
}
```

Als u weten in details van het totaal van de code die is vereist voor dit scenario wilt, raadpleegt u de fase 2 [2-1-Web App aanroepen van Microsoft Graph](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph) stap van de [ms-identity-aspnetcore-Web-App-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) zelfstudie

Er zijn talrijke extra complexiteit zoals:

- implementeren van een token cache voor de Web-App (de zelfstudie presenteren verschillende implementaties)
- Het account is verwijderd uit de cache als de gebruiker zich aanmeldt-out
- Aanroepen van verschillende API's, met inbegrip van wie incrementele toestemming heeft gegeven

## <a name="aspnet"></a>ASP.NET

Wat zijn vergelijkbaar in ASP.NET:

- Een controller actie beveiligd door een kenmerk [autoriseren] wordt opgehaald door de tenant-ID en gebruikers-ID van de `ClaimsPrincipal` lid van de controller (maakt gebruik van ASP.NET `HttpContext.User`)
- van daaruit bouwt Hiermee u een MSAL.NET `IConfidentialClientApplication`
- IT-aanroep de `AcquireTokenSilent` -methode van de toepassing vertrouwelijke client 

de code is vergelijkbaar met de code die wordt weergegeven voor ASP.NET Core

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een web-API aanroepen](scenario-web-app-call-api-call-api.md)
