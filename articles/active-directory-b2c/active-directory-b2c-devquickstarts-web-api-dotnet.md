---
title: Aanroepen van een beveiligde web-api ASP.NET Azure Active Directory B2C | Microsoft Docs
description: Het maken van een .NET-Web-app en het aanroepen van een web api met Azure Active Directory B2C en OAuth 2.0-toegangstokens.
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: mtillman
editor: 
ms.assetid: d3888556-2647-4a42-b068-027f9374aa61
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.custom: seohack1
ms.openlocfilehash: d81976988a26ce264dd7b9ed24f43aed21d4ee99
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-b2c-call-a-net-web-api-from-a-net-web-app"></a>Azure AD B2C: Een .NET web API aanroepen vanuit een .NET-web-app

Met behulp van Azure AD B2C, kunt u krachtige identity management-functies kunt toevoegen aan uw web-apps en web-API's. Dit artikel wordt beschreven hoe u kunt aanvragen toegangstokens en controleer aanroepen vanuit een .NET 'takenlijst' web-app naar een .NET web-api.

In dit artikel wordt niet uitgelegd hoe implementeert aanmelding, registratie en Profielbeheer met Azure AD B2C. Dit artikel gaat over het aanroepen van web API's nadat de gebruiker al is geverifieerd. Als u nog niet gedaan hebt, moet u het volgende doen:

* Aan de slag met een [.NET-web-app](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
* Aan de slag met een [.NET web-api](active-directory-b2c-devquickstarts-api-dotnet.md)

## <a name="prerequisite"></a>Vereiste

Voor het bouwen van een webtoepassing die een web aanroept-api, moet u:

1. [Een Azure AD B2C-tenant maken](active-directory-b2c-get-started.md).
2. [Registreren van een web api](active-directory-b2c-app-registration.md#register-a-web-api).
3. [Een web-app registreren](active-directory-b2c-app-registration.md#register-a-web-app).
4. [Instellen van het beleid](active-directory-b2c-reference-policies.md).
5. [Het web app machtigingen toewijzen aan via het web api](active-directory-b2c-access-tokens.md#publishing-permissions).

> [!IMPORTANT]
> De clienttoepassing en web-API moeten dezelfde Azure AD B2C-directory gebruiken.
>

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie wordt bewaard in [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). U kunt het voorbeeld klonen door de volgende opdracht uit te voeren:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Nadat u de voorbeeldcode hebt gedownload, opent u het SLN-bestand in Visual Studio om aan de slag te gaan. Het oplossingsbestand bevat twee projecten: `TaskWebApp` en `TaskService`. `TaskWebApp`is een MVC-webtoepassing die de gebruiker werkt. `TaskService` is de web-API voor de back-end van de app waarin elke takenlijst van de gebruiker wordt opgeslagen. In dit artikel omvat niet gebouw de `TaskWebApp` web-app of de `TaskService` web-api. Zie voor meer informatie over het bouwen van de .NET-web-app met behulp van Azure AD B2C, onze [.NET web app-zelfstudie](active-directory-b2c-devquickstarts-web-dotnet-susi.md). Zie voor meer informatie over het bouwen van de .NET-web-API die zijn beveiligd met Azure AD B2C, onze [.NET-web-API-zelfstudie](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="update-the-azure-ad-b2c-configuration"></a>De Azure AD B2C-configuratie bijwerken

Ons voorbeeld is geconfigureerd voor gebruik van het beleid en de client-id van onze demo-tenant. Als u gebruiken van uw eigen tenant wilt:

1. Open `web.config` in het `TaskService`-project en vervang de waarden voor

    * `ida:Tenant` door de naam van uw tenant
    * `ida:ClientId`met de toepassings-ID van uw web-api
    * `ida:SignUpSignInPolicyId` door de naam van uw 'Aanmelden/registreren'-beleid

2. Open `web.config` in het `TaskWebApp`-project en vervang de waarden voor

    * `ida:Tenant` door de naam van uw tenant
    * `ida:ClientId` door de id van uw web-app-toepassing
    * `ida:ClientSecret` door de geheime sleutel voor uw web-app
    * `ida:SignUpSignInPolicyId` door de naam van uw 'Aanmelden/registreren'-beleid
    * `ida:EditProfilePolicyId` door de naam van uw 'Profiel bewerken'-beleid
    * `ida:ResetPasswordPolicyId` door de naam van uw 'Wachtwoord opnieuw instellen'-beleid



## <a name="requesting-and-saving-an-access-token"></a>Aanvragen en het opslaan van een toegangstoken

### <a name="specify-the-permissions"></a>Geef de machtigingen

Als u wilt de aanroep van de web-API maken, moet u verifiëren van de gebruiker (met behulp van uw beleid sign-up-to-date/aanmelden) en [ontvangen van een toegangstoken](active-directory-b2c-access-tokens.md) van Azure AD B2C. Om een toegangstoken ontvangt, moet u eerst de machtigingen die u het toegangstoken dat wilt verlenen opgeven. De machtigingen zijn opgegeven in de `scope` parameter bij het maken van de aanvraag voor de `/authorize` eindpunt. Om bijvoorbeeld te verkrijgen van een toegangstoken met de machtiging 'lezen' aan de resource-toepassing met de App ID URI van `https://contoso.onmicrosoft.com/tasks`, het bereik zijn `https://contoso.onmicrosoft.com/tasks/read`.

Open het bestand om het bereik opgeven in ons voorbeeld, `App_Start\Startup.Auth.cs` en definieer de `Scope` variabele in OpenIdConnectAuthenticationOptions.

```CSharp
// App_Start\Startup.Auth.cs

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ...

            // Specify the scope by appending all of the scopes requested into one string (seperated by a blank space)
            Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
        }
    );
}
```

### <a name="exchange-the-authorization-code-for-an-access-token"></a>Exchange de autorisatiecode voor een toegangstoken

Nadat een gebruiker heeft de ervaring registreren of aanmelden voltooid, ontvangt de app een autorisatiecode van Azure AD B2C. De middleware OWIN OpenID Connect, de code wordt opgeslagen, maar niet voor een toegangstoken exchange. U kunt de [MSAL bibliotheek](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) waarmee de exchange. In ons voorbeeld geconfigureerde een callback van querymelding in het OpenID Connect middleware wanneer een autorisatiecode wordt ontvangen. In de callback we MSAL gebruiken voor het uitwisselen van de code voor een token en het token in de cache op te slaan.

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

Deze sectie wordt beschreven hoe u de token ontvangen tijdens sign-up-to-date/aanmelden met Azure AD B2C om toegang tot de web-API.

### <a name="retrieve-the-saved-token-in-the-controllers"></a>Het token opgeslagen in de controllers opgehaald

De `TasksController` verantwoordelijk is voor communicatie met de web-API en voor het verzenden van HTTP-aanvragen naar de API om te lezen, maken en verwijderen van taken. Omdat de API is beveiligd door Azure AD B2C, moet u eerst ophalen van de token die u in de vorige stap hebt opgeslagen.

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

### <a name="read-tasks-from-the-web-api"></a>Lezen van de taken van de web-API

Wanneer u een token hebt, kunt u het koppelen aan HTTP `GET` aanvragen in de `Authorization` header om aan te roepen veilig `TaskService`:

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

Ga als volgt hetzelfde patroon bij het verzenden van `POST` en `DELETE` aanvragen naar de web-API MSAL met het toegangstoken ophalen uit de cache.

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Ten slotte bouwen en uitvoeren van zowel de apps. Aanmelden en meld u aan en maak taken voor de aangemelde gebruiker. Meld u af en meld u aan als een andere gebruiker. Taken voor die gebruiker maken. U ziet hoe de taken worden opgeslagen per gebruiker op de API, omdat de API haalt de identiteit van de gebruiker uit het token dat wordt ontvangen. U kunt ook met de scopes speelt. Verwijder de machtiging 'schrijven' en probeer het toevoegen van een taak. Zorg ervoor dat elke keer dat u het bereik wijzigen afmelden.

