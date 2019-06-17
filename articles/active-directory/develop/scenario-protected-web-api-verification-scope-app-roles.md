---
title: Beveiligde web-API - configuratie van de app-code | Azure Active Directory
description: Informatie over het bouwen van een beveiligde Web-API en de code van uw toepassing configureren.
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
ms.openlocfilehash: b700825be9a7fe23fe4b50a2d69d4de71f7dc038
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67116452"
---
# <a name="protected-web-api---adding-authorization-to-your-api"></a>Beveiligde web-API - verificatie toe te voegen aan uw API

Dit artikel wordt beschreven hoe u autorisatie kunt toevoegen aan uw Web-API. Deze beveiliging zorgt ervoor dat alleen wordt aangeroepen door:

- toepassingen namens gebruikers met de juiste bereiken 
- of daemon-apps met de juiste toepassingsrollen.

Voor een ASP.NET / ASP.NET Core Web-API moet worden beveiligd, u moet toevoegen de `[Authorize]` kenmerk op:

- de Controller zelf als u wilt dat alle acties van de controller worden beveiligd
- of de afzonderlijke controller-actie voor uw API.

```CSharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

Maar deze beveiliging is niet voldoende. Deze alleen dat ASP.NET vermelde / ASP.NET Core valideert het token. Uw API nodig heeft om te verifiëren dat het token gebruikt voor het aanroepen van uw Web-API is aangevraagd met de claims, met name verwacht:

- de **scopes** als de API wordt aangeroepen namens een gebruiker
- de **app-rollen** als de API kan worden aangeroepen vanuit een daemon-app.

## <a name="verifying-scopes-in-apis-called-on-behalf-of-users"></a>Scopes in API's met de naam namens gebruikers verifiëren

Als uw API wordt aangeroepen door een client-app namens een gebruiker, wordt deze nodig heeft om aan te vragen een bearer-token met een specifiek bereik voor de API (Zie [Code configuratie | Bearer-token](scenario-protected-web-api-app-configuration.md#bearer-token))

```CSharp
[Authorize]
public class TodoListController : Controller
{
    /// <summary>
    /// The Web API will only accept tokens 1) for users, 2) having the `access_as_user` scope for
    /// this API
    /// </summary>
    const string scopeRequiredByAPI = "access_as_user";

    // GET: api/values
    [HttpGet]
    public IEnumerable<TodoItem> Get()
    {
        VerifyUserHasAnyAcceptedScope(scopeRequiredByAPI);
        // Do the work and return the result
        ...
    }
...
}
```

De `VerifyUserHasAnyAcceptedScope` methode zou er ongeveer als het volgende doen:

- Controleer of er een claims met de naam `http://schemas.microsoft.com/identity/claims/scope` of `scp`
- Controleer of de claim heeft een waarde met het bereik dat door de API verwacht.

```CSharp
    /// <summary>
    /// When applied to an <see cref="HttpContext"/>, verifies that the user authenticated in the 
    /// Web API has any of the accepted scopes.
    /// If the authenticated user does not have any of these <paramref name="acceptedScopes"/>, the
    /// method throws an HTTP Unauthorized with the message telling which scopes are expected in the token
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

Deze voorbeeldcode is voor ASP.NET Core. Voor ASP.NET alleen vervangen `HttpContext.User` door `ClaimsPrincipal.Current`, en het claimtype `"http://schemas.microsoft.com/identity/claims/scope"` door `"scp"` (Zie ook het onderstaande codefragment)

## <a name="verifying-app-roles-in-apis-called-by-daemon-apps"></a>App-rollen in API's die worden aangeroepen door daemon-apps controleren

Als uw Web-API wordt aangeroepen door een [Daemon-toepassing](scenario-daemon-overview.md), en vervolgens de toepassing moet de machtiging voor een toepassing naar uw Web-API. We hebben gezien in [scenario-protected-web-api-app-registration.md#how-to-expose-application-permissions--app-roles-] dat uw API wordt aangegeven dat deze machtigingen (bijvoorbeeld als de `access_as_application` app rol).
Nu moet u beschikken over uw API's controleren of het token ontvangen bevat de `roles` claims en dat deze claim heeft de waarde die wordt verwacht. De code doet deze verificatie is vergelijkbaar met de code die controleert of gedelegeerde machtigingen, met dien verstande dat, in plaats van de tests voor `scopes`, de actie van de controller wordt getest voor `roles`:

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
    // In this case we look for a `role` value of `access_as_application`
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

Deze voorbeeldcode is voor ASP.NET. Voor ASP.NET Core, Vervang `ClaimsPrincipal.Current` door `HttpContext.User` en de `"roles"` door de naam van claim `"http://schemas.microsoft.com/identity/claims/roles"` (Zie ook het bovenstaande codefragment)

### <a name="accepting-app-only-tokens-if-the-web-api-should-only-be-called-by-daemon-apps"></a>Alleen app-tokens accepteren als de Web-API moet alleen worden aangeroepen door daemon-apps

De `roles` claim wordt ook gebruikt voor gebruikers in gebruiker toewijzing patronen (Zie [het: App-rollen in uw toepassing toevoegen en deze ontvangen in het token](howto-add-app-roles-in-azure-ad-apps.md)). Alleen controleren rollen kunnen apps aan te melden als de gebruikers en andersom, als de rollen toegewezen aan beide zijn. We raden u aan verschillende rollen voor gebruikers en apps om te voorkomen dat deze verwarring aangegeven.

Als u dat alleen daemon toepassingen uw Web-API aan te roepen wilt, moet u een voorwaarde toevoegen, bij het valideren van de functie app, die de token een alleen-app-token is:

```CSharp
string oid = ClaimsPrincipal.Current.FindFirst("oid");
string sub = ClaimsPrincipal.Current.FindFirst("sub");
bool isAppOnlyToken = oid == sub;
```

Controle van de inverse voorwaarde wordt toegestaan voor alleen apps die een gebruiker, meld u aan uw API aan te roepen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verplaatsen naar productie](scenario-protected-web-api-production.md)
