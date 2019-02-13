---
title: 'Zelfstudie: toegang verlenen aan een web-API van ASP.NET Core vanuit een app met één pagina - Azure Active Directory B2C | Microsoft Docs'
description: Zelfstudie over het gebruik van Active Directory B2C om een .NET Core web-API te beveiligen en aan te roepen vanuit een app met één pagina.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 13cbf1e81e0d203c181efb0881ec2a437cbaef24
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55752174"
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-application-using-azure-active-directory-b2c"></a>Zelfstudie: Toegang verlenen aan een web-API van ASP.NET Core vanuit een app met één pagina met behulp van Azure Active Directory B2C

Deze zelfstudie laat zien hoe u een ASP.NET Core web-API-resource, die wordt beveiligd door een Azure Active Directory (Azure AD) B2C, aanroept vanuit een app met één pagina.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een web-API-toepassing toevoegen
> * Bereiken voor een web-API configureren
> * App-machtigingen verlenen aan de web-API
> * Het voorbeeld configureren voor gebruik van de toepassing

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Voltooi de stappen en de vereisten in [Zelfstudie: Verificatie van apps met één pagina inschakelen met behulp van Azure Active Directory B2C](active-directory-b2c-tutorials-spa.md).

## <a name="add-a-web-api-application"></a>Een web-API-toepassing toevoegen

Web-API-resources moeten worden geregistreerd in uw tenant voordat deze beveiligde resourceaanvragen door clienttoepassingen die een toegangstoken aanbieden, kunnen accepteren en erop kunnen reageren.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map gebruikt die uw Azure AD B2C-tenant bevat door te klikken op het **Map- en abonnementsfilter** in het bovenste menu en de map te kiezen waarin uw tenant zich bevindt.
3. Kies **Alle services** linksboven in de Microsoft Azure-portal en zoek en selecteer **Azure AD B2C**.
4. Selecteer **Toepassingen** en vervolgens **Toevoegen**.
5. Voer een naam in voor de toepassing. Bijvoorbeeld, *webapi1*.
6. Selecteer voor **Inclusief web-app/web-API** en **Impliciete stroom toestaan** de optie **Ja**.
7. Voer voor de **Antwoord-URL** een eindpunt in waarop Azure AD B2C tokens retourneert die door uw toepassing worden aangevraagd. In deze zelfstudie wordt het voorbeeld lokaal uitgevoerd en luistert dit op `https://localhost:5000`.
8. Voer voor **App ID URI** de id in die wordt gebruikt voor uw web-API. De volledige id-URI, inclusief het domein, wordt voor u gegenereerd. Bijvoorbeeld `https://contosotenant.onmicrosoft.com/api`.
9. Klik op **Create**.
10. Noteer op de eigenschappenpagina de toepassings-ID die u gebruikt wanneer u de web-app configureert.

## <a name="configure-scopes"></a>Bereiken configureren

Bereiken bieden een manier om toegang tot beveiligde resources te reguleren. Bereiken worden door de web-API gebruikt om toegangsbeheer op basis van een bereik te implementeren. Sommige gebruikers kunnen bijvoorbeeld zowel lees- als schrijftoegang hebben, terwijl andere gebruikers mogelijk alleen-lezen-machtigingen hebben. In deze zelfstudie gaat u lees- en schrijfmachtigingen definiëren voor de web-API.

1. Selecteer **Toepassingen** en vervolgens *webapi1*.
2. Selecteer **Gepubliceerde bereiken**.
3. Voer voor **Bereik** `Hello.Read` in en voor beschrijving `Read access to hello`.
4. Voer voor **Bereik** `Hello.Write` in en voor beschrijving `Write access to hello`.
5. Klik op **Opslaan**.

De gepubliceerde bereiken kunnen worden gebruikt om een client-appmachtiging te verlenen aan de web-API.

## <a name="grant-permissions"></a>Machtigingen verlenen

Als u een beveiligde web-API wilt aanroepen vanuit een app, moet u uw app machtigingen verlenen tot de API. In de vereiste zelfstudie hebt u een webtoepassing in Azure AD B2C gemaakt met de naam *webapp1*. Met deze toepassing kunt u de web-API aanroepen.

1. Selecteer **Toepassingen** en vervolgens *webapp1*.
2. Selecteer **API-toegang**, en selecteer vervolgens **Toevoegen**.
3. Selecteer in de vervolgkeuzelijst **API selecteren** *webapi1*.
4. Selecteer in de vervolgkeuzelijst **Bereiken selecteren** de bereiken **Hello.Read** en **Hello.Write** die u eerder hebt gedefinieerd.
5. Klik op **OK**.

Uw **Mijn voorbeeld-app met één pagina** is geregistreerd om de beveiligde **Hello Core API** te kunnen aanroepen. Een gebruiker voert een verificatie uit bij Azure AD B2C om de app met één pagina te kunnen gebruiken. De app met één pagina verkrijgt een autorisatietoekenning van Azure AD B2C voor toegang tot de beveiligde web-API.

## <a name="configure-the-sample"></a>Het voorbeeld configureren

Nu de web-API is geregistreerd en u bereiken hebt gedefinieerd, moet u de web-API-code configureren om uw Azure AD B2C-tenant te gebruiken. In deze zelfstudie gaat u een .NET Core web-app als voorbeeld configureren die u kunt downloaden uit GitHub. [Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

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
        "ClientId": "<application-ID>",
        "Policy": "B2C_1_signupsignin1>",
        "ScopeRead": "Hello.Read"  
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

3. Open het bestand **launchSettings.json** onder **Eigenschappen**, zoek de instelling **iisSettings** *applicationURL* en stel het poortnummer in op het nummer dat is geregistreerd voor antwoord-URL van de API`http://localhost:5000`.

### <a name="configure-the-single-page-application"></a>De app met één pagina configureren

De app met één pagina gebruikt Azure AD B2C voor gebruikersregistratie en -aanmelding en roept de beveiligde ASP.NET Core web-API aan. U werkt de app met één pagina bij om de .NET Core web-API aan te roepen.

De app-instellingen wijzigen:

1. Open het `index.html`-bestand.
2. Configureer het voorbeeld met de registratiegegevens voor Azure AD B2C-tenant. In de volgende code voegt u uw tenantnaam toe aan **b2cScopes** en wijzigt u de waarde van **webApi** in de waarde voor *applicationURL*, die u eerder hebt geregistreerd:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var applicationConfig = {
        clientID: '<application-ID>',
        authority: "https://<your-tenant-name>.b2clogin.com/tfp/<your-tenant-name>.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://<Your tenant name>.onmicrosoft.com/api/Hello.Read"],
        webApi: 'http://localhost:5000/api/values',
    };
    ```

## <a name="run-the-spa-application-and-web-api"></a>De SPA-app en web-API uitvoeren

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
6. Klik op **API aanroepen**.

Nadat u zich aanmeldt of inlogt met een gebruikersaccount, roept het voorbeeld de beveiligde web-API aan en retourneert een resultaat.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een web-API-toepassing toevoegen
> * Bereiken voor een web-API configureren
> * App-machtigingen verlenen aan de web-API
> * Het voorbeeld configureren voor gebruik van de toepassing

> [!div class="nextstepaction"]
> [Zelfstudie: Id-providers toevoegen aan uw toepassingen in Azure Active Directory B2C](tutorial-add-identity-providers.md)
