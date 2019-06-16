---
title: Web-API die worden aangeroepen web-API's (aanroep van API's) - Microsoft identity-platform
description: Meer informatie over het bouwen van een web-API die downstream web-aanroepen van API's (aanroepen van een web-API).
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
ms.openlocfilehash: 1cd093cc68a21558dc326221eeaa8c034c24f1c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074723"
---
# <a name="web-api-that-calls-web-apis---call-an-api"></a>Web-API die web-API's - roept een API aanroepen

Zodra u een token hebt, kunt u een beveiligde web-API kunt aanroepen. Dit wordt gedaan via de controller van de web-API voor uw ASP.NET/ASP.NET Core.

## <a name="controller-code"></a>Controllercode

Hier is de voortzetting van de voorbeeldcode die wordt weergegeven [beveiligde web-API-aanroepen web-API's - ophalen van een token](scenario-web-api-call-api-acquire-token.md), met de naam in de acties van de API-controllers, aanroepen van een downstream-API (met de naam todolist).

Nadat u het token verkregen, gebruiken als een bearer-token van de downstream-API aan te roepen.

```CSharp
private async Task GetTodoList(bool isAppStarting)
{
 ...
 //
 // Get an access token to call the To Do service.
 //
 AuthenticationResult result = null;
 try
 {
  app = BuildConfidentialClient(HttpContext, HttpContext.User);
  result = await app.AcquireTokenSilent(Scopes, account)
                     .ExecuteAsync()
                     .ConfigureAwait(false);
 }
...

// Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
_httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the To Do list service.
HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
...
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verplaatsen naar productie](scenario-web-api-call-api-production.md)
