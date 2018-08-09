---
title: 'Zelfstudie: toegang verlenen aan een web-API van ASP.NET Core vanuit een app met één pagina met behulp van Azure Active Directory B2C | Microsoft Docs'
description: Zelfstudie over het gebruik van Active Directory B2C om een .NET Core web-API te beveiligen en aan te roepen vanuit een app met één pagina.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 3/02/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: a2fbdebfc800c33a99b19b366209aeabb03fe115
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590830"
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-app-using-azure-active-directory-b2c"></a>Zelfstudie: toegang verlenen aan een web-API van ASP.NET Core vanuit een app met één pagina met behulp van Azure Active Directory B2C

Deze zelfstudie laat zien hoe u een ASP.NET Core web-API-resource, die wordt beveiligd door een Azure Active Directory (Azure AD) B2C, aanroept vanuit een app met één pagina.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een web-API registreren in uw Azure AD B2C-tenant
> * Bereiken voor een web-API definiëren en configureren
> * App-machtigingen verlenen aan de web-API
> * Voorbeeldcode bijwerken zodat Azure AD B2C wordt gebruikt om een web-API te beveiligen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Voltooi de zelfstudie [Verificatie van gebruikers met Azure Active Directory B2C in een toepassing met één pagina](active-directory-b2c-tutorials-spa.md).
* U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **ASP.NET- en webontwikkelworkload** installeren.
* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) of hoger
* [Node.js](https://nodejs.org/en/download/) installeren

## <a name="register-web-api"></a>Web-API registreren

Web-API-resources moeten worden geregistreerd in uw tenant voordat deze in staat zijn om [beveiligde resourceaanvragen](../active-directory/develop/developer-glossary.md#resource-server) door [clienttoepassingen](../active-directory/develop/developer-glossary.md#client-application) die een [toegangstoken](../active-directory/develop/developer-glossary.md#access-token) van Azure Active Directory aanbieden, kunnen accepteren en erop kunnen reageren. Tijdens de registratie wordt de [toepassing en het service-principal-object](../active-directory/develop/developer-glossary.md#application-object) in uw tenant opgezet. 

Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecteer **Azure AD B2C** in de lijst met services in Azure Portal.

2. Klik in de B2C-instellingen op **Toepassingen** en klik vervolgens op **Toevoegen**.

    Gebruik de volgende instellingen voor het registreren van de voorbeeld-web-API in uw tenant.
    
    ![Een nieuwe API toevoegen](media/active-directory-b2c-tutorials-spa-webapi/web-api-registration.png)
    
    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | Hello Core API | Voer een **naam** in die uw web-API aan ontwikkelaars beschrijft. |
    | **Web-app / web-API opnemen** | Ja | Selecteer **Ja** voor een web-API. |
    | **Impliciete stroom toestaan** | Ja | Selecteer **Ja** omdat de API gebruikmaakt van [aanmelding via OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **Antwoord-URL** | `http://localhost:44332` | Antwoord-URL's zijn eindpunten waarop Azure AD B2C tokens retourneert die worden aangevraagd door uw API. In deze zelfstudie wordt de voorbeeld-web-API lokaal (localhost) uitgevoerd en luistert deze op poort 5000. |
    | **App-id-URI** | HelloCoreAPI | De URI is een unieke identificatie van de API in de tenant. Hierdoor kunt u meerdere API's per tenant registreren. [Bereiken](../active-directory/develop/developer-glossary.md#scopes) bepalen de toegang tot de beveiligde API-resource en worden per app-id-URI gedefinieerd. |
    | **Systeemeigen client** | Nee | Aangezien dit een web-API en geen systeemeigen client is, moet u Nee selecteren. |
    
3. Klik op **Maken** om uw API te registreren.

Geregistreerde API's worden weergegeven in de lijst met toepassingen voor de Azure AD B2C-tenant. Selecteer de web-API in de lijst. Het eigenschappenvenster van de web-API wordt weergegeven.

![Eigenschappen van de web-API](./media/active-directory-b2c-tutorials-spa-webapi/b2c-web-api-properties.png)

Noteer de **client-id voor de toepassing**. Met de id wordt de API uniek aangeduid. Deze hebt u nodig als u later in deze zelfstudie de API gaat configureren.

Door de web-API bij Azure AD B2C te registeren, wordt een vertrouwensrelatie gedefinieerd. Omdat de API bij B2C is geregistreerd, kan de API nu de B2C-toegangstokens vertrouwen die van andere toepassingen worden ontvangen.

## <a name="define-and-configure-scopes"></a>Bereiken definiëren en configureren

[Bereiken](../active-directory/develop/developer-glossary.md#scopes) bieden een manier om toegang tot beveiligde resources te reguleren. Bereiken worden door de web-API gebruikt om toegangsbeheer op basis van een bereik te implementeren. Sommige gebruikers kunnen bijvoorbeeld zowel lees- als schrijftoegang hebben, terwijl andere gebruikers mogelijk alleen-lezen-machtigingen hebben. In deze zelfstudie gaat u lees- en schrijfmachtigingen definiëren voor de web-API.

### <a name="define-scopes-for-the-web-api"></a>Bereiken definiëren voor de web-API

Geregistreerde API's worden weergegeven in de lijst met toepassingen voor de Azure AD B2C-tenant. Selecteer de web-API in de lijst. Het eigenschappenvenster van de web-API wordt weergegeven.

Klik op **Gepubliceerde bereiken (preview-versie)**.

Voeg de volgende gegevens toe om bereiken voor de API te configureren. 

![bereiken die zijn gedefinieerd in een web-api](media/active-directory-b2c-tutorials-spa-webapi/scopes-web-api.png)

| Instelling      | Voorgestelde waarde  | Beschrijving                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Bereik** | demo.read | Leestoegang tot demo API |

Klik op **Opslaan**.

De gepubliceerde bereiken kunnen worden gebruikt om een client-appmachtiging te verlenen aan de web-API.

### <a name="grant-app-permissions-to-web-api"></a>App-machtigingen verlenen aan een web-API

Als u een beveiligde web-API wilt aanroepen vanuit een app, moet u uw app machtigingen verlenen tot de API. In deze zelfstudie gebruikt u de app met één pagina gemaakt in [Verificatie van gebruikers met Azure Active Directory B2C in een toepassing met één pagina (JavaScript)](active-directory-b2c-tutorials-spa.md).

1. Selecteer in Azure Portal **Azure AD B2C** in de lijst met services, en klik op **Toepassingen** om de lijst met geregistreerde apps weer te geven.

2. Selecteer **Mijn voorbeeld-app met één pagina** in de lijst met apps, klik op **API-toegang (preview)** en vervolgens op **Toevoegen**.

3. In de vervolgkeuzelijst **API selecteren** selecteert u uw geregistreerde web-API: **Hello Core API**.

4. In de vervolgkeuzelijst **Bereiken selecteren** selecteert u de bereiken die u hebt gedefinieerd toen u de web-API registreerde.

    ![bereiken voor app selecteren](media/active-directory-b2c-tutorials-spa-webapi/selecting-scopes-for-app.png)

5. Klik op **OK**.

Uw **Mijn voorbeeld-app met één pagina** is geregistreerd om de beveiligde **Hello Core API** te kunnen aanroepen. Een gebruiker [voert een verificatie uit](../active-directory/develop/developer-glossary.md#authentication) met Azure AD B2C om de WPF-desktop-app te kunnen gebruiken. De desktop-app verkrijgt een [authorisatietoekenning](../active-directory/develop/developer-glossary.md#authorization-grant) van Azure AD B2C voor toegang tot de beveiligde web-API.

## <a name="update-code"></a>Code bijwerken

Nu de web-API is geregistreerd en u bereiken hebt gedefinieerd, moet u de web-API-code configureren om uw Azure AD B2C-tenant te gebruiken. In deze zelfstudie gaat u een .NET Core web-app als voorbeeld configureren die u kunt downloaden uit GitHub. 

[Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi.git
```

### <a name="configure-the-web-api"></a>De web-API configureren

1. Open de oplossing **B2C-WebAPI-DotNet** in Visual Studio.

2. Open het bestand **appsettings.json**. Werk de volgende waarden bij om de web-API te configureren voor het gebruik van uw tenant:

    ```javascript
    "AzureAdB2C": 
      {
        "Tenant": "<your tenant name>.onmicrosoft.com", 
        "ClientId": "<The Application ID for your web API obtained from the Azure portal>",
        "Policy": "<Your sign up sign in policy e.g. B2C_1_SiUpIn>",
        "ScopeRead": "demo.read"  
      },
    ```

#### <a name="enable-cors"></a>CORS inschakelen

Om uw app met één pagina toe te staan de web-API van ASP.NET Core aan te roepen, moet u [CORS](https://docs.microsoft.com/aspnet/core/security/cors) inschakelen.

1. In **Startup.cs** voegt u CORS toe aan de methode `ConfigureServices()`.

    ```C#
    public void ConfigureServices(IServiceCollection services) {
      services.AddCors();
    ```

2. In **Startup.cs** configureert u CORS in de methode `Configure()`.

    ```C#
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory) {
      app.UseCors(builder =>
        builder.WithOrigins("http://localhost:6420").AllowAnyHeader().AllowAnyMethod());
    ```

3. Open het bestand **launchSettings.json** onder **Eigenschappen**, zoek de instelling voor *applicationURL* en registreer de waarde voor gebruik in de volgende sectie.

### <a name="configure-the-single-page-app"></a>Configureer de app met één pagina

De app met één pagina gebruikt Azure AD B2C voor gebruikersregistratie, aanmelding en roept de beveiligde ASP.NET Core web-API aan. U moet de app met één pagina bijwerken om de .NET Core web-API aan te roepen.
De app-instellingen wijzigen:

1. Open het bestand `index.html` in de Node.js-voorbeeld-app van één pagina.
2. Configureer het voorbeeld met de registratiegegevens voor Azure AD B2C-tenant. In de volgende code voegt u uw tenantnaam toe aan **b2cScopes** en wijzigt u de waarde van **webApi** in de waarde voor *applicationURL*, die u eerder hebt geregistreerd:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var applicationConfig = {
        clientID: '<Application ID for your SPA obtained from portal app registration>',
        authority: "https://login.microsoftonline.com/tfp/<your-tenant-name>.onmicrosoft.com/B2C_1_SiUpIn",
        b2cScopes: ["https://<Your tenant name>.onmicrosoft.com/HelloCoreAPI/demo.read"],
        webApi: 'http://localhost:64791/api/values',
    };
    ```

## <a name="run-the-spa-app-and-web-api"></a>Voer de SPA-app en web-API uit

U moet zowel de Node.js-app met één pagina als de web-API van .NET Core uitvoeren.

### <a name="run-the-aspnet-core-web-api"></a>De web-API van ASP.NET Core uitvoeren 

Druk op **F5** om fouten op te sporen in de oplossing **B2C-WebAPI.sln** in Visual Studio.

Wanneer het project is gestart, wordt een webpagina weergegeven in de standaardbrowser met de melding dat de web-API beschikbaar is voor aanvragen.

### <a name="run-the-single-page-app"></a>Voer de app met één pagina uit

1. Start een opdrachtprompt voor Node.js.
2. Ga naar de map met het Node.js-voorbeeld. Bijvoorbeeld `cd c:\active-directory-b2c-javascript-msal-singlepageapp`
3. Voer de volgende opdrachten uit:
    ```
    npm install && npm update
    node server.js
    ```

    Het consolevenster geeft het poortnummer van waar de app wordt gehost.
    
    ```
    Listening on port 6420...
    ```

4. Gebruik een browser om te navigeren naar het adres `http://localhost:6420` om de app weer te geven.
5. Meld u aan met het e-mailadres en het wachtwoord dat wordt gebruikt in [Verificatie van gebruikers met Azure Active Directory B2C in een toepassing met één pagina (JavaScript)](active-directory-b2c-tutorials-spa.md).
6. Klik op de knop **API aanroepen**.

Nadat u zich aanmeldt of inlogt met een gebruikersaccount, roept het voorbeeld de beveiligde web-API aan en retourneert een resultaat.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-tenant gebruiken voor andere zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-tenant verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u stapsgewijs geleerd hoe u een web-API kunt beveiligen door scopes in Azure AD B2C te registreren en te definiëren. U vindt meer informatie door te bladeren door de beschikbare Azure AD B2C-codevoorbeelden.

> [!div class="nextstepaction"]
> [Azure AD B2C-codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)
