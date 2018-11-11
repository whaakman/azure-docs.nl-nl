---
title: Een .NET-web-API aanroepen vanuit een .NET-web-app in Azure Active Directory B2C | Microsoft Docs
description: Over het bouwen van een .NET-Web-app en aanroepen van een web-api met behulp van Azure Active Directory B2C en OAuth 2.0-toegangstokens.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/17/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7296954a17b21183eb8be2744b42289522cf7f57
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51012493"
---
# <a name="call-a-net-web-api-from-a-net-web-app-in-azure-active-directory-b2c"></a>Een .NET-web-API aanroepen vanuit een .NET-web-app in Azure Active Directory B2C

Met behulp van Azure AD B2C, kunt u beheer van krachtige identiteitsfuncties toevoegen aan uw web-apps en web-API's. In dit artikel wordt beschreven hoe u aanvragen toegangstokens en aanroepen voor het maken van een 'Takenlijst'.NET-web-app naar een .NET web-api.

In dit artikel omvat niet het implementeren van aanmelding, registratie en Profielbeheer met Azure AD B2C. Dit artikel gaat over het aanroepen van web API's nadat de gebruiker is al geverifieerd. Als u niet hebt gedaan, moet u het:

* Aan de slag met een [.NET-web-app](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
* Aan de slag met een [.NET web-api](active-directory-b2c-devquickstarts-api-dotnet.md)

## <a name="prerequisite"></a>Vereiste

Voor het bouwen van een webtoepassing die een web-aanroept api, moet u naar:

1. [Een Azure AD B2C-tenant maken](active-directory-b2c-get-started.md).
2. [Registreren van een web api](active-directory-b2c-app-registration.md).
3. [Een web-app registreren](active-directory-b2c-app-registration.md).
4. [Instellen van het beleid](active-directory-b2c-reference-policies.md).
5. [Verlenen van de web app-machtigingen voor het gebruik van de web api](active-directory-b2c-access-tokens.md).

> [!IMPORTANT]
> De clienttoepassing en web-API moeten dezelfde Azure AD B2C-directory gebruiken.
>

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie wordt bewaard in [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). U kunt het voorbeeld klonen door de volgende opdracht uit te voeren:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Nadat u de voorbeeldcode hebt gedownload, opent u het SLN-bestand in Visual Studio om aan de slag te gaan. Het oplossingsbestand bevat twee projecten: `TaskWebApp` en `TaskService`. `TaskWebApp` is een MVC-webtoepassing waarmee de gebruiker werkt. `TaskService` is de web-API voor de back-end van de app waarin elke takenlijst van de gebruiker wordt opgeslagen. In dit artikel omvat niet gebouw de `TaskWebApp` web-app of de `TaskService` web-api. Zie voor meer informatie over het bouwen van de .NET-web-app met behulp van Azure AD B2C, onze [zelfstudie voor .NET web-apps](active-directory-b2c-devquickstarts-web-dotnet-susi.md). Zie voor meer informatie over het bouwen van de .NET-web-API is beveiligd met behulp van Azure AD B2C, onze [.NET web API zelfstudie](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="update-the-azure-ad-b2c-configuration"></a>De Azure AD B2C-configuratie bijwerken

Ons voorbeeld is geconfigureerd voor gebruik van het beleid en de client-id van onze demo-tenant. Als u wilt het gebruik van uw eigen tenant:

1. Open `web.config` in het `TaskService`-project en vervang de waarden voor

    * `ida:Tenant` door de naam van uw tenant
    * `ida:ClientId` met de toepassings-ID van uw web-api
    * `ida:SignUpSignInPolicyId` door de naam van uw 'Aanmelden/registreren'-beleid

2. Open `web.config` in het `TaskWebApp`-project en vervang de waarden voor

    * `ida:Tenant` door de naam van uw tenant
    * `ida:ClientId` door de id van uw web-app-toepassing
    * `ida:ClientSecret` door de geheime sleutel voor uw web-app
    * `ida:SignUpSignInPolicyId` door de naam van uw 'Aanmelden/registreren'-beleid
    * `ida:EditProfilePolicyId` door de naam van uw 'Profiel bewerken'-beleid
    * `ida:ResetPasswordPolicyId` door de naam van uw 'Wachtwoord opnieuw instellen'-beleid



## <a name="requesting-and-saving-an-access-token"></a>Aanvragen en opslaan van een toegangstoken

### <a name="specify-the-permissions"></a>De machtigingen opgeven

Als u wilt de aanroep naar de web-API maakt, moet u verifiëren van de gebruiker (met behulp van uw aanmelden-up-to-date/aanmeldingsbeleid) en [ontvangen van een toegangstoken](active-directory-b2c-access-tokens.md) van Azure AD B2C. Als u wilt een toegangstoken ontvangt, moet u eerst de machtigingen die u wilt dat het toegangstoken om toegang te verlenen opgeven. De machtigingen zijn opgegeven in de `scope` parameter bij het maken van de aanvraag voor de `/authorize` eindpunt. Bijvoorbeeld, om te verkrijgen van een toegangstoken met de machtiging 'lezen' aan de resourcetoepassing met de URI van de App-ID van `https://contoso.onmicrosoft.com/tasks`, het bereik worden `https://contoso.onmicrosoft.com/tasks/read`.

Als u wilt het bereik opgeven in ons voorbeeld, open het bestand `App_Start\Startup.Auth.cs` en definieer de `Scope` variabele in OpenIdConnectAuthenticationOptions.

```CSharp
// App_Start\Startup.Auth.cs

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ...

            // Specify the scope by appending all of the scopes requested into one string (separated by a blank space)
            Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
        }
    );
}
```

### <a name="exchange-the-authorization-code-for-an-access-token"></a>De autorisatiecode voor een toegangstoken uitwisselen

Nadat een gebruiker is de ervaring voor registreren of aanmelden voltooid, ontvangt uw app een autorisatiecode van Azure AD B2C. De OpenID Connect OWIN-middleware de code worden opgeslagen, maar niet voor een toegangstoken exchange. U kunt de [MSAL bibliotheek](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) waarmee de exchange. In ons voorbeeld hebben we geconfigureerd een callback van querymelding in de middleware OpenID Connect wanneer een autorisatiecode is ontvangen. In de callback wij MSAL gebruiken voor het uitwisselen van de code voor een token en het token in de cache opslaan.

```CSharp
/*
* Callback function when an authorization code is received
*/
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
    // Extract the code from the response notification
    var code = notification.Code;

    var userObjectId = notification.AuthenticationTicket.Identity.FindFirst(ObjectIdElement).Value;
    var authority = String.Format(AadInstance, Tenant, DefaultPolicy);
    var httpContext = notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase;

    // Exchange the code for a token. Make sure to specify the necessary scopes
    ClientCredential cred = new ClientCredential(ClientSecret);
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                            RedirectUri, cred, new NaiveSessionCache(userObjectId, httpContext));
    var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { ReadTasksScope, WriteTasksScope }, code, DefaultPolicy);
}
```

## <a name="calling-the-web-api"></a>De web-API aanroepen

In deze sectie wordt beschreven hoe u de token ontvangen tijdens het aanmelden-up-to-date/aanmelden met Azure AD B2C voor toegang tot de web-API.

### <a name="retrieve-the-saved-token-in-the-controllers"></a>De opgeslagen token in de controllers ophalen

De `TasksController` verantwoordelijk is voor communicatie met de web-API en voor het verzenden van HTTP-aanvragen naar de API om te lezen, maken en verwijderen van taken. Omdat de API wordt beveiligd door Azure AD B2C, moet u eerst ophalen van het token dat u in de vorige stap hebt opgeslagen.

```CSharp
// Controllers\TasksController.cs

/*
* Uses MSAL to retrieve the token from the cache
*/
private async void acquireToken(String[] scope)
{
    string userObjectID = ClaimsPrincipal.Current.FindFirst(Startup.ObjectIdElement).Value;
    string authority = String.Format(Startup.AadInstance, Startup.Tenant, Startup.DefaultPolicy);

    ClientCredential credential = new ClientCredential(Startup.ClientSecret);

    // Retrieve the token using the provided scopes
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                        Startup.RedirectUri, credential,
                                        new NaiveSessionCache(userObjectID, this.HttpContext));
    AuthenticationResult result = await app.AcquireTokenSilentAsync(scope);

    accessToken = result.Token;
}
```

### <a name="read-tasks-from-the-web-api"></a>Taken lezen van de web-API

Wanneer u een token hebt, kunt u deze koppelen aan de HTTP `GET` aanvragen in de `Authorization` header om aan te roepen veilig `TaskService`:

```CSharp
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try {

        // Retrieve the token with the specified scopes
        acquireToken(new string[] { Startup.ReadTasksScope });

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, apiEndpoint);

        // Add token to the Authorization header and make the request
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
        HttpResponseMessage response = await client.SendAsync(request);

        // Handle response ...
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Maken en verwijderen van taken op de web-API

Ga als volgt hetzelfde patroon bij het verzenden van `POST` en `DELETE` aanvragen naar de web-API, met MSAL het toegangstoken ophalen uit de cache.

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Ten slotte, bouwen en uitvoeren van zowel de apps. Aanmelden en meld u aan en taken maken voor de aangemelde gebruiker. Meld u af en meld u aan als een andere gebruiker. Maak taken voor die gebruiker. U ziet hoe de taken zijn opgeslagen per gebruiker op de API, omdat de API haalt de identiteit van de gebruiker uit het token dat wordt ontvangen. U kunt ook spelen met de scopes. Verwijder de machtiging 'schrijven' en probeer het vervolgens een taak toe te voegen. Zorg ervoor dat u zich afmelden telkens wanneer die u het bereik wijzigen.

