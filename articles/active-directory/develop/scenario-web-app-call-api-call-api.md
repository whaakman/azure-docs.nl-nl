---
title: Web-app dat aanroepen van web-API's (aanroepen van een web-API) - Microsoft identity-platform
description: Informatie over het bouwen van een Web-app die aanroepen van web-API's (aanroepen van een web-API)
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
ms.openlocfilehash: 3624f4e859081e53ee27b6f8415eb3f9b5a2a5fa
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67785460"
---
# <a name="web-app-that-calls-web-apis---call-a-web-api"></a>Web-app die aanroepen van web-API's - een web-API aanroepen

Nu dat u een token hebt, kunt u een beveiligde web-API kunt aanroepen.

## <a name="aspnet-core"></a>ASP.NET Core

Dit is een vereenvoudigde code van de actie van de `HomeController`. Deze code wordt een token voor het aanroepen van de Microsoft Graph. Deze code voor de tijd is toegevoegd, waarin wordt getoond hoe Microsoft Graph als een REST-API aanroepen. De URL van de graph API is beschikbaar in de `appsettings.json` bestands- en lees in een variabele met de naam `webOptions`:

```JSon
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    ...
  },
  ...
  "GraphApiUrl": "https://graph.microsoft.com"
}
```

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

 // Calls the web API (here the graph)
 HttpClient httpClient = new HttpClient();
 httpClient.DefaultRequestHeaders.Authorization =
     new AuthenticationHeaderValue(Constants.BearerAuthorizationScheme,accessToken);
 var response = await httpClient.GetAsync($"{webOptions.GraphApiUrl}/beta/me");

 if (response.StatusCode == HttpStatusCode.OK)
 {
  var content = await response.Content.ReadAsStringAsync();

  dynamic me = JsonConvert.DeserializeObject(content);
  return me;
 }

 ViewData["Me"] = me;
 return View();
}
```

> [!NOTE]
> De dezelfde methode kunt u een web-API aanroepen.
>
> De meeste Azure-web-API's bieden een SDK die vereenvoudigt u de methode aanroept. Dit is ook het geval van de Microsoft Graph. U leert in het volgende artikel waar vind ik een zelfstudie waarin u ziet deze aspecten.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verplaatsen naar productie](scenario-web-app-call-api-production.md)
