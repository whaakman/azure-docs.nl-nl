---
title: Web-API die aanroepen van andere web-API's (een token voor de app ophalen) - Microsoft identity-platform
description: Meer informatie over het bouwen van een web-API die aanroepen van andere web-API's (ophalen van een token voor de app).
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
ms.openlocfilehash: ada4323b0e61c6dc9fb87af133c40ec9c35e3834
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074978"
---
# <a name="web-api-that-calls-web-apis---acquire-a-token-for-the-app"></a>Web-API die web-API's - roept een token verkrijgen voor de app.

Als u een clienttoepassing hebt gemaakt object, gebruiken voor het verkrijgen van een token dat u gebruiken kunt voor het aanroepen van een web-API.

## <a name="code-in-the-controller"></a>Code in de controller

Hier volgt een voorbeeld van code die wordt aangeroepen in de acties van de API-controllers, een downstream-API (met de naam todolist) aanroepen.

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
}
```

`BuildConfidentialClient()` is vergelijkbaar met wat u hebt gezien in het artikel [Web-API-aanroepen web-API's - app-configuratie](scenario-web-api-call-api-app-configuration.md). `BuildConfidentialClient()` instantieert `IConfidentialClientApplication` met een cache met alleen gegevens voor één account. Het account wordt geleverd door de `GetAccountIdentifier` methode.

De `GetAccountIdentifier` methode maakt gebruik van de claims die zijn gekoppeld aan de identiteit van de gebruiker waarvoor de web-API de JWT ontvangen:

```CSharp
public static string GetMsalAccountId(this ClaimsPrincipal claimsPrincipal)
{
 string userObjectId = GetObjectId(claimsPrincipal);
 string tenantId = GetTenantId(claimsPrincipal);

 if (    !string.IsNullOrWhiteSpace(userObjectId)
      && !string.IsNullOrWhiteSpace(tenantId))
 {
  return $"{userObjectId}.{tenantId}";
 }

 return null;
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een web-API aanroepen](scenario-web-api-call-api-call-api.md)
