---
title: Beveiligde web-API - configuratie van de app-code | Azure Active Directory
description: Informatie over het bouwen van een beveiligde web-API en de code van uw toepassing configureren.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
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
ms.openlocfilehash: 23ff03316a1f9409d4d6e4b7ddf52d0c8cc7a909
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551543"
---
# <a name="protected-web-api-adding-authorization-to-your-api"></a>Beveiligde web-API: Autorisatie toe te voegen aan uw API

Dit artikel wordt beschreven hoe u autorisatie kunt toevoegen aan uw web-API. Deze beveiliging zorgt ervoor dat de API alleen wordt genoemd:

- Toepassingen namens gebruikers die beschikken over de juiste bereiken.
- Daemon-apps met de juiste toepassingsrollen.

Als u wilt een ASP.NET/ASP.NET Core-web-API beveiligen, moet u om toe te voegen de `[Authorize]` kenmerk op een van de volgende:

- De controller zelf, als u wilt dat alle acties van de controller worden beveiligd
- De afzonderlijke controller-actie voor uw API

```CSharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

Maar deze beveiliging is niet voldoende. Het garandeert alleen ASP.NET/ASP.NET Core valideert het token. Uw API nodig heeft om te verifiëren dat het token gebruikt voor het aanroepen van uw web-API is aangevraagd met de claims wordt verwacht, met name:

- De *scopes*, als de API wordt aangeroepen namens een gebruiker.
- De *app-rollen*, als de API kan worden aangeroepen vanuit een daemon-app.

## <a name="verifying-scopes-in-apis-called-on-behalf-of-users"></a>Scopes in API's met de naam namens gebruikers verifiëren

Als uw API wordt aangeroepen door een client-app namens een gebruiker, moet deze de aanvraag een bearer-token met specifieke bereiken voor de API. (Zie [Code configuratie | Bearer-token](scenario-protected-web-api-app-configuration.md#bearer-token).)

```CSharp
[Authorize]
public class TodoListController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    const string scopeRequiredByAPI = "access_as_user";

    // GET: api/values
    [HttpGet]
    public IEnumerable<TodoItem> Get()
    {
        VerifyUserHasAnyAcceptedScope(scopeRequiredByAPI);
        // Do the work and return the result.
        ...
    }
...
}
```

De `VerifyUserHasAnyAcceptedScope` methode zou er ongeveer als het volgende doen:

- Controleer of er een claim met de naam `http://schemas.microsoft.com/identity/claims/scope` of `scp`.
- Controleer of de claim heeft een waarde met het bereik dat door de API verwacht.

```CSharp
    /// <summary>
    /// When applied to a <see cref="HttpContext"/>, verifies that the user authenticated in the 
    /// web API has any of the accepted scopes.
    /// If the authenticated user doesn't have any of these <paramref name="acceptedScopes"/>, the
    /// method throws an HTTP Unauthorized error with a message noting which scopes are expected in the token.
    /// </summary>
    /// <param name="acceptedScopes">Scopes accepted by this API</param>
    /// <exception cref="HttpRequestException"/> with a <see cref="HttpResponse.StatusCode"/> set to 
    /// <see cref="HttpStatusCode.Unauthorized"/>
    public static void VerifyUserHasAnyAcceptedScope(this HttpContext context,
                                                     params string[] acceptedScopes)
    {
        if (acceptedScopes == null)
        {
            throw new ArgumentNullException(nameof(acceptedScopes));
        }
        Claim scopeClaim = HttpContext?.User
                                      ?.FindFirst("http://schemas.microsoft.com/identity/claims/scope");
        if (scopeClaim == null || !scopeClaim.Value.Split(' ').Intersect(acceptedScopes).Any())
        {
            context.Response.StatusCode = (int)HttpStatusCode.Unauthorized;
            string message = $"The 'scope' claim does not contain scopes '{string.Join(",", acceptedScopes)}' or was not found";
            throw new HttpRequestException(message);
        }
    }
```

Deze voorbeeldcode is voor ASP.NET Core. Voor ASP.NET, Vervang `HttpContext.User` met `ClaimsPrincipal.Current`, en vervang het claimtype `"http://schemas.microsoft.com/identity/claims/scope"` met `"scp"`. (Zie ook het codefragment verderop in dit artikel.)

## <a name="verifying-app-roles-in-apis-called-by-daemon-apps"></a>App-rollen in API's die worden aangeroepen door daemon-apps controleren

Als uw web-API wordt aangeroepen door een [daemon app](scenario-daemon-overview.md), van de app moet de machtiging voor een toepassing naar uw web-API. We hebben gezien [blootstellen van machtigingen van de toepassing (app-rollen)](https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-app-registration#exposing-application-permissions-app-roles) dat uw API wordt aangegeven dat deze machtigingen (bijvoorbeeld de `access_as_application` app rol).
Nu moet u beschikken over uw API's controleren of het token ontvangen bevat de `roles` claim en dat deze claim heeft de waarde die wordt verwacht. De code doet deze verificatie is vergelijkbaar met de code die controleert of gedelegeerde machtigingen, met dien verstande dat, in plaats van de tests voor `scopes`, de actie van de controller wordt getest voor `roles`:

```CSharp
[Authorize]
public class TodoListController : ApiController
{
    public IEnumerable<TodoItem> Get()
    {
        ValidateAppRole("access_as_application");
        ...
    }
```

De `ValidateAppRole()` methode kan er ongeveer als volgt zijn:

```CSharp
private void ValidateAppRole(string appRole)
{
    //
    // The `role` claim tells you what permissions the client application has in the service.
    // In this case, we look for a `role` value of `access_as_application`.
    //
    Claim roleClaim = ClaimsPrincipal.Current.FindFirst("roles");
    if (roleClaim == null || !roleClaim.Value.Split(' ').Contains(appRole))
    {
        throw new HttpResponseException(new HttpResponseMessage 
        { StatusCode = HttpStatusCode.Unauthorized, 
            ReasonPhrase = $"The 'roles' claim does not contain '{appRole}' or was not found" 
        });
    }
}
}
```

Deze voorbeeldcode is voor ASP.NET. Voor ASP.NET Core, Vervang `ClaimsPrincipal.Current` met `HttpContext.User`, en vervang de `"roles"` met de naam van claim `"http://schemas.microsoft.com/identity/claims/roles"`. (Zie ook het codefragment eerder in dit artikel.)

### <a name="accepting-app-only-tokens-if-the-web-api-should-be-called-only-by-daemon-apps"></a>Alleen app-tokens accepteren als de web-API moet alleen worden aangeroepen door daemon-apps

De `roles` claim wordt ook gebruikt voor gebruikers in gebruiker toewijzing patronen. (Zie [het: App-rollen in uw toepassing toevoegen en deze ontvangen in het token](howto-add-app-roles-in-azure-ad-apps.md).) Alleen controleren rollen kunnen apps aan te melden als de gebruikers en andersom, als de rollen toegewezen aan beide zijn. Het is raadzaam dat u verschillende rollen voor gebruikers en apps om te voorkomen dat deze verwarring declareren.

Als u toestaan dat alleen daemon-apps wilt naar uw web-API aanroepen, een voorwaarde toevoegen, bij het valideren van de functie app, die de token een alleen-app-token is:

```CSharp
string oid = ClaimsPrincipal.Current.FindFirst("oid");
string sub = ClaimsPrincipal.Current.FindFirst("sub");
bool isAppOnlyToken = oid == sub;
```

Controle van de inverse voorwaarde wordt toegestaan voor alleen apps die een gebruiker aanmelden bij het aanroepen van uw API.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verplaatsen naar productie](scenario-protected-web-api-production.md)
