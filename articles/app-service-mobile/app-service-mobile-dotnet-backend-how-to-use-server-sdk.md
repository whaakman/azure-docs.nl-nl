---
title: Over het werken met de .NET back-endserver SDK voor Mobile Apps | Microsoft Docs
description: Informatie over het werken met de .NET back-endserver SDK voor Azure App Service Mobile Apps.
keywords: appservice, azure appservice, mobiele app, mobiele service, schaal, schaalbaar, app-implementatie, azure app-implementatie
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 0620554f-9590-40a8-9f47-61c48c21076b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 33e968ac608c393d65f69bfd6abbc0d205fb9bd9
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53718874"
---
# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Werken met de .NET-back-endserver-SDK voor Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Dit onderwerp ziet u hoe u de .NET back-endserver SDK in belangrijke scenario's voor Azure App Service Mobile Apps te gebruiken. De Azure Mobile Apps SDK kunt u werken met mobiele clients van uw ASP.NET-toepassing.

> [!TIP]
> De [.NET server SDK voor Azure Mobile Apps] [ 2] is open source op GitHub. De opslagplaats bevat alle broncode, met inbegrip van de hele server SDK-eenheid-testpakket en sommige voorbeeldprojecten.
>
>

## <a name="reference-documentation"></a>Referentiedocumentatie
De referentiedocumentatie voor de SDK-server bevindt zich hier: [Naslaginformatie over Azure Mobile Apps .NET][1].

## <a name="create-app"></a>Procedures: De back-end van een .NET Mobile Apps maken
Als u een nieuw project start, kunt u een App Service-toepassing met behulp van de [Azure Portal] of Visual Studio. U kunt de App Service-toepassing lokaal uitvoeren of het project om uw cloud-gebaseerde mobiele App Service-app te publiceren.

Als u mobiele mogelijkheden aan een bestaand project toevoegt, raadpleegt u de [downloaden en het initialiseren van de SDK](#install-sdk) sectie.

### <a name="create-a-net-backend-using-the-azure-portal"></a>Een .NET back-end met Azure portal maken
Voor het maken van een mobiele back-end voor App Service, volg de [beknopte zelfstudie] [ 3] of als volgt te werk:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Terug in de *aan de slag* blade onder **maken van een tabel-API**, kiest u **C#** als uw **back-endtaal**. Klik op **downloaden**, pak de gecomprimeerde projectbestanden op uw lokale computer en open de oplossing in Visual Studio.

### <a name="create-a-net-backend-using-visual-studio-2017"></a>Maak een .NET back-end met behulp van Visual Studio 2017

Installeer de Azure-workload via de Visual Studio Installer publiceren naar Azure Mobile Apps-project in Visual Studio. Wanneer u de SDK hebt geïnstalleerd, maakt u een ASP.NET-toepassing met behulp van de volgende stappen uit:

1. Open de **nieuw Project** dialoogvenster (van **bestand** > **nieuw** > **Project wordt gemaakt...** ).
2. Vouw **Visual C#** en selecteer **Web**.
3. Selecteer **ASP.NET-webtoepassing (.NET Framework)**.
4. Vul de projectnaam in. Klik vervolgens op **OK**.
5. Selecteer **Azure Mobile App** uit de lijst met sjablonen.
6. Klik op **OK** om de oplossing te maken.
7. Met de rechtermuisknop op het project in de **Solution Explorer** en kies **publiceren...** , en kies vervolgens **App Service** als het publicerende doel.
8. Volg de aanwijzingen om te verifiëren en kiest u een nieuwe of bestaande Azure App Service om te publiceren.

### <a name="create-a-net-backend-using-visual-studio-2015"></a>Maak een .NET back-end met behulp van Visual Studio 2015

Installeer de [Azure SDK voor .NET] [ 4] (versie 2.9.0 of hoger) om te maken van een Azure Mobile Apps-project in Visual Studio. Wanneer u de SDK hebt geïnstalleerd, maakt u een ASP.NET-toepassing met behulp van de volgende stappen uit:

1. Open de **nieuw Project** dialoogvenster (van **bestand** > **nieuw** > **Project wordt gemaakt...** ).
2. Vouw **sjablonen** > **Visual C#**, en selecteer **Web**.
3. Selecteer **ASP.NET-webtoepassing**.
4. Vul de projectnaam in. Klik vervolgens op **OK**.
5. Onder *ASP.NET 4.5.2-sjablonen sjablonen*, selecteer **Azure Mobile App**. Controleer **Host in de cloud** te maken van een mobiele back-end in de cloud waarmee u dit project kunt publiceren.
6. Klik op **OK**.

## <a name="install-sdk"></a>Procedures: Initialiseer de SDK te downloaden en
De SDK is beschikbaar op [NuGet.org]. Dit pakket bevat de functionaliteit aan de slag met de SDK vereist. Voor het initialiseren van de SDK, moet u acties uitvoeren op de **HttpConfiguration** object.

### <a name="install-the-sdk"></a>De SDK installeren
De SDK wilt installeren, met de rechtermuisknop op de server-project in Visual Studio, selecteer **NuGet-pakketten beheren**, zoek de [Microsoft.Azure.Mobile.Server] pakket en klik vervolgens op **installeren** .

### <a name="server-project-setup"></a> Het serverproject initialiseren
Een .NET-back-end-serverproject is die vergelijkbaar is met andere ASP.NET-projecten, door te nemen van een OWIN-Opstartklasse geïnitialiseerd. Zorg ervoor dat u verwijst naar het NuGet-pakket `Microsoft.Owin.Host.SystemWeb`. Deze klasse toevoegen in Visual Studio, met de rechtermuisknop op uw serverproject en selecteer **toevoegen** >
**Nieuw Item**, klikt u vervolgens **Web**  >  ** Algemene** > **OWIN-Opstartklasse**.  Een klasse wordt gegenereerd met het volgende kenmerk:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

In de `Configuration()` -methode van de OWIN-Opstartklasse, gebruik een **HttpConfiguration** object om de Azure Mobile Apps-omgeving te configureren.
Het volgende voorbeeld wordt het serverproject met geen extra functies geïnitialiseerd:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Om in te schakelen afzonderlijke functies, moet u uitbreidingsmethoden aanroepen op de **MobileAppConfiguration** object voordat **ApplyTo**. Bijvoorbeeld, de standaardroutes in de volgende code wordt toegevoegd aan alle API-domeincontrollers waarvoor het kenmerk `[MobileAppController]` tijdens de initialisatie:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

De server Quick Start van het aanroepen van de Azure portal **UseDefaultConfiguration()**. Dit is gelijk aan de volgende instellingen:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

De extensie-methoden die worden gebruikt zijn:

* `AddMobileAppHomeController()` biedt de standaardstartpagina voor Azure Mobile Apps.
* `MapApiControllers()` aangepaste API-functies biedt voor de WebAPI-controllers voorzien van de `[MobileAppController]` kenmerk.
* `AddTables()` bevat een toewijzing van de `/tables` eindpunten aan de tabel-controllers.
* `AddTablesWithEntityFramework()` is een korte hand voor de toewijzing van de `/tables` eindpunten met behulp van Entity Framework op basis van domeincontrollers.
* `AddPushNotifications()` biedt een eenvoudige methode voor het registreren van apparaten voor Notification Hubs.
* `MapLegacyCrossDomainController()` biedt standaard CORS-headers voor lokale ontwikkeling.

### <a name="sdk-extensions"></a>SDK-extensies
De volgende op basis van een NuGet-extensiepakketten bieden verschillende mobiele functies die kunnen worden gebruikt door uw toepassing. U met behulp van uitbreidingen inschakelen tijdens de initialisatie van de **MobileAppConfiguration** object.

* [Microsoft.Azure.Mobile.Server.Quickstart] biedt ondersteuning voor de basisconfiguratie van Mobile Apps. Toegevoegd aan de configuratie door het aanroepen van de **UseDefaultConfiguration** uitbreidingsmethode tijdens de initialisatie. Deze extensie bevat de volgende extensies: Meldingen, verificatie, entiteit, tabellen, tussen domeinen en Home-pakketten. Dit pakket wordt gebruikt door de Quickstart voor Mobile Apps beschikbaar in Azure portal.
* [Microsoft.Azure.Mobile.Server.Home](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) implementeert de standaard *deze mobiele app is actief en werkend pagina* voor de basis-website. Toevoegen aan de configuratie door het aanroepen van de **AddMobileAppHomeController** uitbreidingsmethode.
* [Microsoft.Azure.Mobile.Server.Tables](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) bevat klassen voor het werken met gegevens en de gegevenspijplijn sets-up. Toevoegen aan de configuratie door het aanroepen van de **AddTables** uitbreidingsmethode.
* [Microsoft.Azure.Mobile.Server.Entity](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) kunnen de Entity Framework voor toegang tot gegevens in de SQL-Database. Toevoegen aan de configuratie door het aanroepen van de **AddTablesWithEntityFramework** uitbreidingsmethode.
* [Microsoft.Azure.Mobile.Server.Authentication] Hiermee kunt u verificatie en sets-up de OWIN-middleware die wordt gebruikt om tokens te valideren. Toevoegen aan de configuratie door het aanroepen van de **AddAppServiceAuthentication** en **IAppBuilder**. **UseAppServiceAuthentication** uitbreidingsmethoden.
* [Microsoft.Azure.Mobile.Server.Notifications] kunt pushmeldingen verzenden en een push-eindpunt voor clientregistratie definieert. Toevoegen aan de configuratie door het aanroepen van de **AddPushNotifications** uitbreidingsmethode.
* [Microsoft.Azure.Mobile.Server.CrossDomain](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) maakt u een domeincontroller die gegevens naar oudere webbrowsers vanuit uw mobiele App verstuurt. Toevoegen aan de configuratie door het aanroepen van de **MapLegacyCrossDomainController** uitbreidingsmethode.
* [Microsoft.Azure.Mobile.Server.Login] bevat de methode AppServiceLoginHandler.CreateToken(), die een statische tijdens de scenario's voor aangepaste verificatie gebruikt methode.

## <a name="publish-server-project"></a>Procedures: Het serverproject publiceren
Deze sectie leest u hoe u uw .NET-back-end-project in Visual Studio publiceert. U kunt ook implementeren voor uw back-end-project met [Git](../app-service/deploy-local-git.md) of een van de andere methoden beschikbaar er zijn.

1. Opnieuw in Visual Studio het project voor het herstellen van NuGet-pakketten.
2. Klik in Solution Explorer met de rechtermuisknop op het project, klikt u op **publiceren**. De eerste keer dat u publiceert, moet u voor het definiëren van een profiel voor publiceren. Wanneer u al een profiel dat is gedefinieerd hebt, kunt u deze selecteren en klik op **publiceren**.
3. Als u wordt gevraagd om te selecteren van een doel publiceren, klikt u op **Microsoft Azure App Service** > **volgende**, en (indien nodig) aanmelden met uw Azure-referenties.
   Visual Studio downloadt en veilig slaat uw publicatie-instellingen rechtstreeks vanuit Azure.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. Kies uw **abonnement**, selecteer **resourcetype** van **weergave**, vouw **mobiele App**, en klik op de back-end van uw mobiele App en klik vervolgens op **OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. Controleer of de profielgegevens publiceren en op **publiceren**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Wanneer de back-end van uw mobiele App is gepubliceerd, ziet u een landingspagina die slagen aangeeft.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <a name="define-table-controller"></a> Procedures: Definieer een tabel-controller
Definieer een tabel-Controller om een SQL-tabel naar mobiele clients weer te geven.  Configureren van een tabel-Controller zijn drie stappen vereist:

1. Maak een klasse Data Transfer Object (DTO).
2. Configureer een tabelverwijzing in de mobiele DbContext-klasse.
3. Maak een tabel-controller.

Een Data Transfer Object (DTO) is een gewone C#-object die eigenschappen van overneemt `EntityData`.  Bijvoorbeeld:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

Het DTO wordt gebruikt voor het definiëren van de tabel in de SQL-database.  Voor het maken van de databasevermelding toevoegen een `DbSet<>` eigenschap in op de DbContext die u gebruikt.  In de standaard projectsjabloon, maken voor Azure Mobile Apps, de DbContext heet `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Als u de Azure-SDK geïnstalleerd hebt, kunt u een sjabloon kontroler tabulek nu als volgt maken:

1. Met de rechtermuisknop op de map Controllers en selecteer **toevoegen** > **Controller...** .
2. Selecteer de **Azure Mobile Apps Table Controller** optie en klik vervolgens op **toevoegen**.
3. In de **Controller toevoegen** dialoogvenster:
   * In de **Modelklasse** vervolgkeuzelijst, selecteer uw nieuwe DTO.
   * In de **DbContext** vervolgkeuzelijst, selecteert u de mobiele Service DbContext-klasse.
   * De naam van de domeincontroller is voor u gemaakt.
4. Klik op **Add**.

De Quick Start-serverproject bevat een voorbeeld van een eenvoudige **TodoItemController**.

### <a name="adjust-pagesize"></a>Procedures: Pas de grootte van de tabel paginering
Standaard retourneert Azure Mobile Apps 50 records per aanvraag.  Wisselbestand zorgt ervoor dat de client geen bezighouden heeft hun UI-thread noch de server te lang, ervoor te zorgen dat een goede gebruikerservaring. Als u wilt wijzigen van de grootte van de tabel paginering, de serverzijde 'toegestane grootte van de query' verhogen en het formaat van de client-side '-pagina de serverzijde 'toegestane grootte van de query' wordt aangepast met behulp van de `EnableQuery` kenmerk:

    [EnableQuery(PageSize = 500)]

Zorg ervoor dat de PageSize is hetzelfde of groter zijn dan de grootte die is aangevraagd door de client.  Verwijzen naar de specifieke client HOWTO-documentatie voor meer informatie over het wijzigen van het formaat van de client.

## <a name="how-to-define-a-custom-api-controller"></a>Procedure: Definieer een aangepaste API-controller
De aangepaste API-controller biedt de meest eenvoudige functionaliteit voor de back-end van uw mobiele App bij het blootstellen van een eindpunt. U kunt een API-controller van het mobiele-specifieke met behulp van het kenmerk [MobileAppController] registreren. De `MobileAppController` kenmerk registreert de route, stelt u de serializer van de Mobile Apps in JSON en Hiermee schakelt u [controle van de client versie](app-service-mobile-client-and-server-versioning.md).

1. In Visual Studio met de rechtermuisknop op de map Controllers, en klik vervolgens op **toevoegen** > **Controller**, selecteer **Web API 2-Controller&mdash;leeg** en Klik op **toevoegen**.
2. Geef een **controllernaam**, zoals `CustomController`, en klikt u op **toevoegen**.
3. In het nieuwe bestand in de klasse controller, voeg de volgende instructie:

        using Microsoft.Azure.Mobile.Server.Config;
4. Van toepassing de **[MobileAppController]** kenmerk aan de API-controller klassedefinitie, zoals in het volgende voorbeeld:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. In App_Start/Startup.MobileApp.cs-bestand, Voeg een aanroep naar de **MapApiControllers** uitbreidingsmethode, zoals in het volgende voorbeeld:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

U kunt ook de `UseDefaultConfiguration()` uitbreidingsmethode in plaats van `MapApiControllers()`. Elke domeincontroller die geen **MobileAppControllerAttribute** toegepast nog steeds toegankelijk door clients, maar deze kan niet worden correct gebruikt door clients met behulp van een client-SDK van de mobiele App.

## <a name="how-to-work-with-authentication"></a>Procedure: Werken met verificatie
Azure Mobile Apps maakt gebruik van App Service-verificatie / autorisatie voor het beveiligen van uw mobiele back-end.  Deze sectie leest u hoe u de volgende verificatie-gerelateerde taken in uw serverproject voor .NET-back-end uitvoert:

* [Procedures: Verificatie toevoegen aan een serverproject](#add-auth)
* [Procedures: Aangepaste verificatie voor uw toepassing gebruiken](#custom-auth)
* [Procedures: Gegevens van de geverifieerde gebruiker ophalen](#user-info)
* [Procedures: Beperken van toegang tot gegevens voor gemachtigde gebruikers](#authorize)

### <a name="add-auth"></a>Procedures: Verificatie toevoegen aan een serverproject
U kunt verificatie toevoegen aan uw serverproject door uit te breiden de **MobileAppConfiguration** object en het configureren van OWIN-middleware. Wanneer u installeert de [Microsoft.Azure.Mobile.Server.Quickstart] pakket en roep de **UseDefaultConfiguration** uitbreidingsmethode, kunt u doorgaan met stap 3.

1. In Visual Studio, installeert de [Microsoft.Azure.Mobile.Server.Authentication] pakket.
2. In het projectbestand Startup.cs voegt u de volgende coderegel toe aan het begin van de **configuratie** methode:

        app.UseAppServiceAuthentication(config);

    Dit onderdeel OWIN-middleware valideert tokens die zijn uitgegeven door de bijbehorende App Service-gateway.
3. Voeg de `[Authorize]` kenmerk aan een domeincontroller of de methode die moet worden geverifieerd.

Zie voor meer informatie over het verifiëren van clients naar uw back-end van Mobile Apps, [verificatie toevoegen aan uw app](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Procedures: Aangepaste verificatie voor uw toepassing gebruiken
> [!IMPORTANT]
> Om te kunnen aangepaste verificatie inschakelt, moet u eerst een App Service-verificatie inschakelen zonder het selecteren van een provider voor uw App Service in Azure portal. Hiermee schakelt u de omgevingsvariabele WEBSITE_AUTH_SIGNING_KEY wanneer deze wordt gehost.
> 
> 
Als u niet gebruiken op een van de App Service-verificatie/autorisatie-providers wilt, kunt u uw eigen systeem aanmelding kunt implementeren. Installeer de [Microsoft.Azure.Mobile.Server.Login] pakket om u te helpen bij het genereren van tokens van verificatie.  Geef uw eigen code voor het valideren van gebruikersreferenties. U kunt bijvoorbeeld controleren op basis van wachtwoorden voor gezouten en hash in een database. In onderstaand voorbeeld wordt de `isValidAssertion()` methode (elders gedefinieerd) is verantwoordelijk voor deze controles.

De aangepaste verificatie wordt weergegeven met het maken van een ApiController en het weergeven van `register` en `login` acties. De client moet een aangepaste UI gebruiken voor het verzamelen van de gegevens van de gebruiker.  De informatie wordt vervolgens verzonden naar de API met een standaard-HTTP POST-aanroep. Zodra de verklaring worden geverifieerd door de server, een token is verleend met behulp van de `AppServiceLoginHandler.CreateToken()` methode.  De ApiController **beter niet** gebruiken de `[MobileAppController]` kenmerk.

Een voorbeeld `login` actie:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

In het voorgaande voorbeeld zijn LoginResult en LoginResultUser serialiseerbare objecten blootstellen van vereiste eigenschappen. De client wordt verwacht dat de antwoorden van de aanmelding moet worden geretourneerd als JSON-objecten van het formulier:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

De `AppServiceLoginHandler.CreateToken()` methode bevat een *doelgroep* en een *verlener* parameter. Beide parameters zijn ingesteld op de URL van de toepassingshoofdmap van uw, met behulp van het HTTPS-schema. U moet op dezelfde manier ingesteld *secretKey* moet de waarde van uw toepassing de ondertekeningssleutel. De ondertekeningssleutel in een client niet distribueren als deze kan worden gebruikt om sleutels voorkomt en gebruikers te imiteren. U vindt de ondertekeningssleutel terwijl die wordt gehost in App Service door te verwijzen naar de *WEBSITE\_AUTH\_ondertekening\_sleutel* omgevingsvariabele. Indien nodig in de context van een lokale foutopsporing, volg de instructies in de [lokale foutopsporing met verificatie](#local-debug) sectie ophalen van de sleutel en deze opslaan als een toepassingsinstelling.

Het token verzonden, omvat mogelijk ook andere claims en een vervaldatum.  Minimaal, het token verzonden een onderwerp moet bevatten (**sub**) claim.

U kunt de standaard-client ondersteunen `loginAsync()` methode door overbelasting van de route voor verificatie.  Als de client roept `client.loginAsync('custom');` om aan te melden, uw route moet worden `/.auth/login/custom`.  U kunt de route voor het gebruik van aangepaste verificatie controller instellen `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> Met behulp van de `loginAsync()` aanpak zorgt ervoor dat het verificatietoken is gekoppeld aan elke volgende aanroep naar de service.
>
>

### <a name="user-info"></a>Procedures: Gegevens van de geverifieerde gebruiker ophalen
Wanneer een gebruiker is geverifieerd door App Service, kunt u de toegewezen gebruikers-ID en andere gegevens openen in uw .NET-back-end-code. De gebruikersgegevens kan worden gebruikt voor het maken van autorisatie beslissingen te nemen in de back-end. De volgende code haalt de gebruikers-ID die is gekoppeld aan een aanvraag:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

De SID is afgeleid van de provider-specifieke gebruikers-ID en statisch voor een bepaalde gebruiker en de provider voor de aanmelding.  De beveiligings-id is null voor ongeldige verificatietokens.

App Service kunt u specifieke claims aanvragen bij uw provider voor de aanmelding. De id-provider krijgt u meer informatie met behulp van de id-provider SDK.  Bijvoorbeeld, kunt u de Facebook Graph-API voor vrienden informatie.  In de provider-blade in Azure portal kunt u claims die worden aangevraagd. Sommige claims vereisen aanvullende configuratie met de id-provider.

De volgende code aanroepen de **GetAppServiceIdentityAsync** uitbreidingsmethode ophalen van de aanmeldingsreferenties, waaronder de toegang tot de token die nodig zijn om aan te vragen met de Facebook Graph-API:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Toevoegen van een met-instructie voor `System.Security.Principal` voor de **GetAppServiceIdentityAsync** uitbreidingsmethode.

### <a name="authorize"></a>Procedures: Beperken van toegang tot gegevens voor gemachtigde gebruikers
In de vorige sectie, we hebt u geleerd hoe u de gebruikers-ID van een geverifieerde gebruiker ophalen. U kunt de toegang beperken tot gegevens en andere bronnen op basis van deze waarde. Bijvoorbeeld, is een gebruikers-id-kolom toevoegen aan tabellen en filteren van de resultaten van de query door de gebruikers-ID een eenvoudige manier om te beperken van geretourneerde gegevens alleen voor gemachtigde gebruikers. De volgende code retourneert de rijen met gegevens alleen wanneer de beveiligings-id overeenkomt met de waarde in de kolom gebruikers-id aan de TodoItem-tabel:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

De `Query()` methode retourneert een `IQueryable` die kunnen worden bewerkt door LINQ om af te handelen filteren.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Procedure: Pushmeldingen toevoegen aan een serverproject
Pushmeldingen toevoegen aan uw serverproject door uit te breiden de **MobileAppConfiguration** object en het maken van een client Notification Hubs.

1. Met de rechtermuisknop op de serverproject in Visual Studio, en klikt u op **NuGet-pakketten beheren**, zoeken naar `Microsoft.Azure.Mobile.Server.Notifications`, klikt u vervolgens op **installeren**.
2. Herhaal deze stap voor het installeren van de `Microsoft.Azure.NotificationHubs` pakket, waaronder de clientbibliotheek voor Notification Hubs.
3. In App_Start/Startup.MobileApp.cs, en voeg een aanroep naar de **AddPushNotifications()** uitbreidingsmethode tijdens de initialisatie:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);
4. Voeg de volgende code die een client Notification Hubs maakt:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

U kunt nu de client Notification Hubs gebruiken om pushmeldingen te verzenden naar geregistreerde apparaten. Zie voor meer informatie, [pushmeldingen toevoegen aan uw app](app-service-mobile-ios-get-started-push.md). Zie voor meer informatie over Notification Hubs, [overzicht van Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

## <a name="tags"></a>Procedures: Gerichte pushmeldingen met Tags inschakelen
Notification Hubs kunt u gerichte meldingen verzenden naar specifieke rapporten met behulp van tags. Verschillende tags worden automatisch gemaakt:

* De installatie-ID identificeert een specifiek apparaat.
* De gebruikers-Id op basis van de geverifieerde SID identificeert een specifieke gebruiker.

De installatie van de ID is toegankelijk vanuit de **installationId** eigenschap op de **MobileServiceClient**.  Het volgende voorbeeld ziet u hoe u een label toevoegen aan een specifieke installatie in Notification Hubs met een installatie-ID:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Geleverd door de client tijdens de registratie voor pushberichten labels worden genegeerd door de back-end bij het maken van de installatie. Als u wilt inschakelen op een client labels toevoegen aan de installatie, moet u een aangepaste API die wordt toegevoegd met behulp van het bovenstaande patroon labels maken.

Zie [toegevoegd Client push notification-tags] [ 5] in het App Service Mobile Apps quickstart voltooide voorbeeld voor een voorbeeld.

## <a name="push-user"></a>Procedures: Pushmeldingen verzenden naar een geverifieerde gebruiker
Wanneer een geverifieerde gebruiker geregistreerd voor pushmeldingen, wordt automatisch een label-ID toegevoegd aan de registratie. Met behulp van deze tag kunt u pushmeldingen verzenden naar alle apparaten die zijn geregistreerd door die persoon. De volgende code wordt de SID van gebruiker die de aanvraag en verzendt een pushmelding sjabloon naar elke device Registration service voor die persoon:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Tijdens het registreren van pushmeldingen in vanuit een geverifieerde client, zorg ervoor dat de verificatie is voltooid voordat u de registratie. Zie voor meer informatie, [Pushmeldingen naar gebruikers] [ 6] in het voorbeeld van de App Service Mobile Apps voltooide Snelstartgids voor .NET-back-end.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Procedure: Fouten opsporen en oplossen van de SDK voor .NET-Server
Azure App Service biedt verschillende opsporen en oplossen van technieken voor ASP.NET-toepassingen:

* [Bewaking van een Azure App Service](../app-service/web-sites-monitor.md)
* [Diagnostische logboekregistratie in Azure App Service inschakelen](../app-service/troubleshoot-diagnostic-logs.md)
* [Een Azure App Service in Visual Studio oplossen](../app-service/troubleshoot-dotnet-visual-studio.md)

### <a name="logging"></a>Logboekregistratie
U kunt schrijven naar diagnostische logboeken voor App Service met behulp van de standaard ASP.NET-tracering schrijven. Voordat u naar de logboeken schrijven kan, moet u diagnostische gegevens inschakelen in de back-end van uw mobiele App.

Diagnostische gegevens inschakelen en schrijven naar de logboeken:

1. Volg de stappen in [diagnostische gegevens inschakelen](../app-service/troubleshoot-diagnostic-logs.md#enablediag).
2. Voeg de volgende gebruiksinstructie in uw codebestand:

        using System.Web.Http.Tracing;
3. Maak een trace-schrijver van de .NET back-end schrijven naar de diagnostische logboeken als volgt:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. Uw serverproject te publiceren en toegang tot de backend voor mobiele Apps voor het uitvoeren van het codepad met de logboekregistratie.
5. Downloaden en beoordelen van de logboeken, zoals beschreven in [het: Logboeken downloaden die u](../app-service/troubleshoot-diagnostic-logs.md#download).

### <a name="local-debug"></a>Lokale foutopsporing met verificatie
U kunt uw toepassing lokaal om wijzigingen te testen voordat u deze publiceert naar de cloud uitvoeren. Druk op voor de meeste Azure Mobile Apps-back-ends *F5* tijdens het in Visual Studio. Er zijn echter enkele aanvullende overwegingen bij het gebruik van verificatie.

Hebt u een cloud-gebaseerde mobiele app met App Service-verificatie/autorisatie geconfigureerd moet, en de client het cloudeindpunt dat is opgegeven als de alternatieve aanmeldings-host. Zie de documentatie voor uw clientplatform voor de specifieke stappen die nodig zijn.

Zorg ervoor dat uw mobiele back-end [Microsoft.Azure.Mobile.Server.Authentication] geïnstalleerd. Voeg in van uw toepassing OWIN-Opstartklasse, het volgende na `MobileAppConfiguration` is toegepast op uw `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

In het voorgaande voorbeeld, moet u configureren de *authAudience* en *authIssuer* toepassingsinstellingen binnen uw Web.config-bestand op elke worden de URL van de toepassingshoofdmap van uw, met behulp van het HTTPS-schema. U moet op dezelfde manier ingesteld *authSigningKey* moet de waarde van uw toepassing de ondertekeningssleutel.
Als u de ondertekeningssleutel:

1. Ga naar de app binnen de [Azure Portal]
2. Klik op **extra**, **Kudu**, **gaat**.
3. Klik op de site Kudu Management **omgeving**.
4. Zoek de waarde voor *WEBSITE\_AUTH\_ondertekening\_sleutel*.

Gebruik de sleutel voor de *authSigningKey* parameter in de configuratie van uw lokale toepassing.  Uw mobiele back-end is nu uitgerust om tokens bij lokale uitvoering die de client het token van het eindpunt van de cloud gebaseerde verkrijgt te valideren.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure Portal]: https://portal.azure.com
[NuGet.org]: https://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx
