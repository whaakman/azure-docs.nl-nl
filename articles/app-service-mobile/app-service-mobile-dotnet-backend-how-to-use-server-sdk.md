---
title: Werken met de .NET-back-endserver SDK voor Mobile Apps | Microsoft Docs
description: Informatie over het werken met de .NET-back-endserver SDK voor Azure App Service Mobile Apps.
keywords: App service, azure app service, mobiele app, mobiele service, schaal, schaalbaar, app-implementatie, azure app-implementatie
services: app-service\mobile
documentationcenter: 
author: conceptdev
manager: crdun
editor: 
ms.assetid: 0620554f-9590-40a8-9f47-61c48c21076b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: a1a29d87864bff8cb2ecda70d8a0a7833c70d481
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Werken met de .NET-back-endserver-SDK voor Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Dit onderwerp leest u hoe u de .NET back-endserver SDK in de belangrijkste scenario's voor Azure App Service Mobile Apps. De Azure Mobile Apps SDK kunt u werken met mobiele clients van uw ASP.NET-toepassing.

> [!TIP]
> De [server .NET SDK voor Azure Mobile Apps] [ 2] is open-source op GitHub. De opslagplaats bevat alle broncode, met inbegrip van de hele server SDK-eenheid test suite en sommige-voorbeeldprojecten.
>
>

## <a name="reference-documentation"></a>Referentiedocumentatie
De documentatie bij de SDK-server bevindt zich hier: [Azure Mobile Apps .NET Reference][1].

## <a name="create-app"></a>Procedure: een mobiele App voor .NET-back-end maken
Als u een nieuw project begint, kunt u een App Service-toepassing met behulp van de [Azure-portal] of Visual Studio. U kunt de App Service-toepassing lokaal uitvoeren of het project om uw cloud-gebaseerde mobiele App Service-app te publiceren.

Als u mobiele mogelijkheden aan een bestaand project toevoegt, raadpleegt u de [downloaden en het initialiseren van de SDK](#install-sdk) sectie.

### <a name="create-a-net-backend-using-the-azure-portal"></a>Maken van een .NET-backend met de Azure portal
Voor het maken van een App Service mobiele back-end, volg de [Quick Start-zelfstudie] [ 3] of als volgt te werk:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Terug in de *aan de slag* blade onder **maken van een tabel-API**, kies **C#** als uw **back-endtaal**. Klik op **downloaden**, pak de gecomprimeerde projectbestanden op uw lokale computer en open de oplossing in Visual Studio.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Maken van een .NET-backend met behulp van Visual Studio 2013 en Visual Studio 2015
Installeer de [Azure SDK voor .NET] [ 4] (versie 2.9.0 of hoger) voor het maken van een Azure Mobile Apps-project in Visual Studio. Als u de SDK hebt geïnstalleerd, maakt u een ASP.NET-toepassing met behulp van de volgende stappen uit:

1. Open de **nieuw Project** dialoogvenster (van *bestand* > **nieuw** > **Project wordt gemaakt...** ).
2. Vouw **sjablonen** > **Visual C#**, en selecteer **Web**.
3. Selecteer **ASP.NET-webtoepassing**.
4. Vul in de naam van het project. Klik vervolgens op **OK**.
5. Onder *ASP.NET 4.5.2-sjabloon sjablonen*, selecteer **mobiele Apps van Azure**. Controleer **Host in de cloud** voor het maken van een mobiele back-end in de cloud waaraan u dit project kunt publiceren.
6. Klik op **OK**.

## <a name="install-sdk"></a>Hoe: downloaden en het initialiseren van de SDK
De SDK is beschikbaar op [NuGet.org]. Dit pakket bevat de basisfunctionaliteit vereist om te beginnen met de SDK. Voor het initialiseren van de SDK, moet u acties uitvoeren op de **HttpConfiguration** object.

### <a name="install-the-sdk"></a>De SDK installeren
De SDK wilt installeren, met de rechtermuisknop op de serverproject in Visual Studio, selecteer **NuGet-pakketten beheren**, zoekt de [Microsoft.Azure.Mobile.Server] van het pakket en klik vervolgens op **installeren**.

### <a name="server-project-setup"></a>Het serverproject initialiseren
Een .NET-back-end-serverproject is net als bij andere projecten ASP.NET door een OWIN-Opstartklasse geïnitialiseerd. Zorg ervoor dat u verwijst naar het NuGet-pakket `Microsoft.Owin.Host.SystemWeb`. Deze klasse toevoegen in Visual Studio, met de rechtermuisknop op uw serverproject en selecteer **toevoegen** >
**Nieuw Item**, klikt u vervolgens **Web** > **algemene** > **OWIN-Opstartklasse**.  Een klasse wordt gegenereerd met het volgende kenmerk:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

In de `Configuration()` methode van uw OWIN-Opstartklasse gebruik een **HttpConfiguration** object om de Azure Mobile Apps-omgeving te configureren.
Het volgende voorbeeld initialiseert het serverproject met geen extra functies:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Om afzonderlijke functies inschakelen, moet u uitbreidingsmethoden aanroepen voor de **MobileAppConfiguration** object voordat u **ApplyTo**. Bijvoorbeeld, de standaardroutes in de volgende code wordt toegevoegd aan alle API-domeincontrollers waarvoor het kenmerk `[MobileAppController]` tijdens de initialisatie:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

De Quick Start de server van de Azure portal aanroepen **UseDefaultConfiguration()**. Dit is gelijk aan de volgende instellingen:

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

De extensiemethoden zijn:

* `AddMobileAppHomeController()`biedt de standaardstartpagina voor Azure Mobile Apps.
* `MapApiControllers()`aangepaste API mogelijkheden biedt voor WebAPI-controllers gedecoreerd worden met de `[MobileAppController]` kenmerk.
* `AddTables()`bevat een toewijzing van de `/tables` eindpunten aan tabel-controllers.
* `AddTablesWithEntityFramework()`is een korte voorhanden voor het toewijzen van de `/tables` eindpunten met behulp van Entity Framework op basis van domeincontrollers.
* `AddPushNotifications()`biedt een eenvoudige methode voor het registreren van apparaten voor Notification Hubs.
* `MapLegacyCrossDomainController()`biedt standaard CORS-headers voor lokale ontwikkeling.

### <a name="sdk-extensions"></a>SDK-extensies
De volgende op basis van het NuGet-extensiepakketten bieden verschillende mobiele functies die kunnen worden gebruikt door uw toepassing. U uitbreidingen inschakelen tijdens de initialisatie met behulp van de **MobileAppConfiguration** object.

* [Microsoft.Azure.Mobile.Server.Quickstart] ondersteunt de basisinstellingen voor mobiele Apps. Toegevoegd aan de configuratie door het aanroepen van de **UseDefaultConfiguration** extensiemethode tijdens de initialisatie. Deze extensie bevat de volgende extensies: meldingen, verificatie, entiteit, tabellen, verschillende domeinen en thuis-pakketten. Dit pakket wordt gebruikt door de Mobile Apps-snelstartgids beschikbaar is op de Azure-portal.
* [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) implementeert de standaard *deze mobiele app is actief en werkend pagina* voor de hoofdmap van de website. Toevoegen aan de configuratie door het aanroepen van de **AddMobileAppHomeController** extensiemethode.
* [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) bevat klassen voor het werken met gegevens en de pijplijn data sets-up. Toevoegen aan de configuratie door het aanroepen van de **AddTables** extensiemethode.
* [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) kunnen de Entity Framework voor toegang tot gegevens in de SQL-Database. Toevoegen aan de configuratie door het aanroepen van de **AddTablesWithEntityFramework** extensiemethode.
* [Microsoft.Azure.Mobile.Server.Authentication] verificatie ingeschakeld en wordt uit de OWIN-middleware gebruikt om tokens te valideren. Toevoegen aan de configuratie door het aanroepen van de **AddAppServiceAuthentication** en **IAppBuilder**. **UseAppServiceAuthentication** uitbreidingsmethoden.
* [Microsoft.Azure.Mobile.Server.Notifications] maakt het mogelijk pushmeldingen en definieert een push-eindpunt voor registratie. Toevoegen aan de configuratie door het aanroepen van de **AddPushNotifications** extensiemethode.
* [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) maakt u een domeincontroller die gegevens naar oudere webbrowsers vanuit uw mobiele App fungeert. Toevoegen aan de configuratie door het aanroepen van de **MapLegacyCrossDomainController** extensiemethode.
* [Microsoft.Azure.Mobile.Server.Login] bevat de methode AppServiceLoginHandler.CreateToken(), die een statische methode die wordt gebruikt tijdens de scenario's voor aangepaste verificatie.

## <a name="publish-server-project"></a>Procedure: het serverproject publiceren
Deze sectie wordt beschreven hoe u uw .NET-back-endproject vanuit Visual Studio publiceert. U kunt ook implementeren voor uw back-end-project met [Git](../app-service/app-service-deploy-local-git.md) of een andere methoden beschikbaar er.

1. Bouw het project voor het herstellen van NuGet-pakketten in Visual Studio opnieuw op.
2. Klik in Solution Explorer met de rechtermuisknop op het project, klikt u op **publiceren**. De eerste keer dat u publiceert, moet u een publicatieprofiel definiëren. Wanneer u al een profiel dat is gedefinieerd hebt, kunt u selecteren en op **publiceren**.
3. Als u wordt gevraagd om te selecteren van een doel publiceren, klikt u op **Microsoft Azure App Service** > **volgende**, en (indien nodig) aanmelden met uw Azure-referenties.
   Visual Studio downloadt en veilig opgeslagen uw publicatie-instellingen rechtstreeks uit Azure.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. Kies uw **abonnement**, selecteer **brontype** van **weergave**, vouw **mobiele App**, en klikt u op back-end van uw mobiele App en klik vervolgens op **OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. Controleer of de profielgegevens publiceren en op **publiceren**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Als uw back-end voor de mobiele App is gepubliceerd, ziet u een startpagina die slagen aangeeft.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <a name="define-table-controller"></a>Procedure: een tabel controller definiëren
Definieer een Controller tabel om een SQL-tabel aan mobiele clients weer te geven.  Een tabel Controller configureren, moet drie stappen:

1. Maak een klasse gegevens overbrengen Object (DTO).
2. Configureer een tabelverwijzing in de mobiele DbContext-klasse.
3. Maak een tabel-domeincontroller.

Een Data Transfer Object (DTO) is een gewone C#-object dat eigenschappen van overneemt `EntityData`.  Bijvoorbeeld:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

De DTO wordt gebruikt voor het definiëren van de tabel in de SQL-database.  Voor het maken van de databasevermelding toevoegen een `DbSet<>` eigenschap in op de DbContext die u gebruikt.  In het project standaardsjabloon voor Azure Mobile Apps, de DbContext heet `Models\MobileServiceContext.cs`:

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

Als u de Azure-SDK geïnstalleerd hebt, kunt u een sjabloon tabel controller nu als volgt maken:

1. Met de rechtermuisknop op de map Controllers en selecteer **toevoegen** > **Controller...** .
2. Selecteer de **Azure Mobile Apps tabel Controller** optie en klik vervolgens op **toevoegen**.
3. In de **Controller toevoegen** dialoogvenster:
   * In de **Modelklasse** vervolgkeuzelijst, selecteer uw nieuwe DTO.
   * In de **DbContext** vervolgkeuzelijst selecteert u de mobiele Service DbContext-klasse.
   * Naam van de domeincontroller is voor u gemaakt.
4. Klik op **Add**.

Het serverproject Quick Start bevat een voorbeeld van een eenvoudige **TodoItemController**.

### <a name="adjust-pagesize"></a>Procedure: de grootte van de tabel paginering aanpassen
Azure Mobile Apps retourneert standaard 50 records per aanvraag.  Paginering zorgt ervoor dat de client geen bezighouden heeft hun UI-thread of op de server te lang gezorgd een goede gebruikerservaring. Als u wilt wijzigen de grootte van de tabel paginering, aan de serverzijde 'toegestane grootte van de query' verhogen en de client-side paginagrootte aan de serverzijde 'toegestane grootte van de query' wordt aangepast met behulp van de `EnableQuery` kenmerk:

    [EnableQuery(PageSize = 500)]

Zorg ervoor dat de PageSize is gelijk aan of groter zijn dan de grootte die is aangevraagd door de client.  Raadpleeg de specifieke client procedure-documentatie voor meer informatie over het wijzigen van het formaat van de client.

## <a name="how-to-define-a-custom-api-controller"></a>Procedure: een aangepaste API-controller definiëren
De aangepaste API-controller biedt basisfuncties back-end van uw mobiele App bij het blootstellen van een eindpunt. U kunt een API-controller van het mobiele-specifieke met het kenmerk [MobileAppController] registreren. De `MobileAppController` kenmerk registreert de route, stelt u de serialisatiefunctie Mobile Apps JSON en Hiermee schakelt u [controle van de client versie](app-service-mobile-client-and-server-versioning.md).

1. In Visual Studio met de rechtermuisknop op de map Controllers, en klik vervolgens op **toevoegen** > **Controller**, selecteer **Web API 2-Controller&mdash;leeg** en klik op **toevoegen**.
2. Geef een **controllernaam**, zoals `CustomController`, en klik op **toevoegen**.
3. Voeg het volgende toe in de nieuwe domeincontroller klassebestand met de instructie:

        using Microsoft.Azure.Mobile.Server.Config;
4. Van toepassing de **[MobileAppController]** kenmerk naar de API-controller klassendefinitie, zoals in het volgende voorbeeld:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. In het bestand App_Start/Startup.MobileApp.cs, Voeg een aanroep naar de **MapApiControllers** uitbreidingsmethode, zoals in het volgende voorbeeld:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

U kunt ook de `UseDefaultConfiguration()` extensiemethode in plaats van `MapApiControllers()`. Elke domeincontroller die geen **MobileAppControllerAttribute** toegepast nog steeds toegankelijk zijn voor clients, maar deze kan niet worden correct gebruikt door clients met behulp van een client voor mobiele App SDK.

## <a name="how-to-work-with-authentication"></a>Procedure: werken met verificatie
Mobiele Apps van Azure maakt gebruik van App Service-verificatie / autorisatie voor het beveiligen van uw mobiele back-end.  Deze sectie wordt beschreven hoe u de volgende verificatie-gerelateerde taken uitvoert in uw project met .NET back-end-server:

* [Procedure: verificatie toevoegen aan een serverproject](#add-auth)
* [Procedure: aangepaste verificatie voor uw toepassing gebruiken](#custom-auth)
* [Hoe: ophalen geverifieerde gebruikersgegevens](#user-info)
* [Hoe: beperken van toegang tot gegevens voor gemachtigde gebruikers](#authorize)

### <a name="add-auth"></a>Procedure: verificatie toevoegen aan een serverproject
U kunt verificatie toevoegen aan uw serverproject door het uitbreiden van de **MobileAppConfiguration** object en OWIN middleware configureren. Wanneer u installeert de [Microsoft.Azure.Mobile.Server.Quickstart] pakket en de aanroep van de **UseDefaultConfiguration** uitbreidingsmethode, kunt u doorgaan met stap 3.

1. In Visual Studio, installeert de [Microsoft.Azure.Mobile.Server.Authentication] pakket.
2. In het projectbestand Startup.cs voegt u de volgende coderegel toe aan het begin van de **configuratie** methode:

        app.UseAppServiceAuthentication(config);

    Dit onderdeel OWIN-middleware valideert tokens die zijn uitgegeven door de bijbehorende App Service-gateway.
3. Voeg de `[Authorize]` kenmerk met een domeincontroller of de methode die verificatie vereist.

Zie voor meer informatie over het verifiëren van clients in uw back-end van Mobile Apps, [verificatie toevoegen aan uw app](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Procedure: aangepaste verificatie voor uw toepassing gebruiken
> [!IMPORTANT]
> Om aangepaste verificatie inschakelt, moet u eerst een App Service-verificatie inschakelen zonder het selecteren van een serviceprovider voor uw App-Service in de Azure portal. Hiermee schakelt u de omgevingsvariabele WEBSITE_AUTH_SIGNING_KEY wanneer deze wordt gehost.
> 
> 
Als u niet gebruiken een van de App Service-verificatie/autorisatie-providers wilt, kunt u uw eigen systeem aanmelding te implementeren. Installeer de [Microsoft.Azure.Mobile.Server.Login] pakket om u te helpen bij het genereren van de verificatie-tokens.  Geef uw eigen code voor het valideren van gebruikersreferenties. U kunt bijvoorbeeld controleren tegen gezouten en hash-wachtwoorden in een database. In het onderstaande voorbeeld de `isValidAssertion()` methode (elders gedefinieerd) is verantwoordelijk voor deze controles.

De aangepaste verificatie wordt blootgelegd door een ApiController maken en blootstellen `register` en `login` acties. De client moet een aangepaste gebruikersinterface gebruiken voor het verzamelen van de informatie van de gebruiker.  De informatie wordt vervolgens verzonden naar de API met een standaard HTTP POST-aanroep. Zodra de verklaring worden geverifieerd door de server een token is verleend met behulp van de `AppServiceLoginHandler.CreateToken()` methode.  De ApiController **beter niet** gebruiken de `[MobileAppController]` kenmerk.

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

In het voorgaande voorbeeld zijn LoginResult en LoginResultUser serialiseerbaar objecten blootstellen vereiste eigenschappen. De client verwacht aanmelding antwoorden moeten worden geretourneerd als JSON-objecten van het formulier:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

De `AppServiceLoginHandler.CreateToken()` methode bevat een *doelgroep* en een *verlener* parameter. Beide parameters zijn ingesteld op de URL van de hoofdmap van uw toepassing met behulp van het HTTPS-schema. U moet op dezelfde manier instellen *secretKey* moet de waarde van uw toepassing de handtekeningsleutel. De handtekeningsleutel in een client niet distribueren als kan worden gebruikt om te sleutels voorkomt en gebruikers te imiteren. U vindt de ondertekeningssleutel terwijl die in App Service wordt gehost door te verwijzen naar de *WEBSITE\_AUTH\_ondertekening\_sleutel* omgevingsvariabele. Indien nodig in de context van een lokale foutopsporing, volg de instructies in de [lokale foutopsporing met verificatie](#local-debug) sectie op te halen van de sleutel en deze opslaan als een toepassingsinstelling.

Het gepubliceerde token omvatten mogelijk ook andere claims en een vervaldatum.  Minimaal het gepubliceerde token een onderwerp moet bevatten (**sub**) claim.

U kunt de standaard client ondersteunen `loginAsync()` methode door overbelasting van de route voor verificatie.  Als de client roept `client.loginAsync('custom');` Meld u bij uw route moet `/.auth/login/custom`.  U kunt instellen dat de route voor de aangepaste verificatie controller met `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> Met behulp van de `loginAsync()` aanpak zorgt ervoor dat het verificatietoken is gekoppeld aan elke volgende aanroep naar de service.
>
>

### <a name="user-info"></a>Hoe: ophalen geverifieerde gebruikersgegevens
Wanneer een gebruiker is geverifieerd door App Service, kunt u de toegewezen gebruikers-ID en andere informatie openen in uw .NET-back-end-code. De gebruikersgegevens kan worden gebruikt voor het nemen van autorisatiebeslissingen in de back-end. De volgende code haalt de gebruikers-ID die is gekoppeld aan een aanvraag:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

De SID is afgeleid van de provider-specifieke gebruikers-ID en statisch voor een bepaalde gebruiker en de aanmeldingsprovider is.  De SID is null voor ongeldige verificatietokens.

App Service kunt u bepaalde claims aanvragen bij uw aanmeldingsprovider. Elke identiteitsprovider krijgt u meer informatie, met behulp van de identiteitsprovider SDK.  U kunt bijvoorbeeld de Facebook Graph-API voor vrienden informatie.  In de blade provider in de Azure portal kunt u claims die worden aangevraagd. Sommige claims vereisen aanvullende configuratie met de id-provider.

De volgende code aanroepen de **GetAppServiceIdentityAsync** uitbreidingsmethode de aanmeldingsreferenties, waaronder de toegang token ophalen die nodig is voor aanvragen op basis van de Facebook Graph-API:

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

Toevoegen van een via-instructie voor `System.Security.Principal` waarmee de **GetAppServiceIdentityAsync** extensiemethode.

### <a name="authorize"></a>Hoe: beperken van toegang tot gegevens voor gemachtigde gebruikers
In de vorige sectie we hebt u geleerd hoe op te halen van de gebruikers-ID van een geverifieerde gebruiker. U kunt de toegang beperken tot gegevens en andere bronnen op basis van deze waarde. Bijvoorbeeld, is een gebruikers-id-kolom toevoegen aan tabellen en filteren van resultaten van de query door de gebruikers-ID een eenvoudige manier om geretourneerde gegevens alleen tot gemachtigde gebruikers te beperken. De volgende code retourneert rijen met gegevens alleen als de beveiligings-id overeenkomt met de waarde in de kolom gebruikers-id in de takentabel:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

De `Query()` methode retourneert een `IQueryable` die kan worden bewerkt door LINQ om af te handelen filteren.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Hoe: toevoegen push notifications op een serverproject
Pushmeldingen toevoegen aan uw serverproject door het uitbreiden van de **MobileAppConfiguration** object en het maken van een client Notification Hubs.

1. In Visual Studio met de rechtermuisknop op het serverproject en klik op **NuGet-pakketten beheren**, zoeken naar `Microsoft.Azure.Mobile.Server.Notifications`, klikt u vervolgens op **installeren**.
2. Herhaal deze stap voor het installeren van de `Microsoft.Azure.NotificationHubs` pakket, waaronder de clientbibliotheek van Notification Hubs.
3. In App_Start/Startup.MobileApp.cs, en voeg een aanroep naar de **AddPushNotifications()** extensiemethode tijdens de initialisatie:

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

U kunt nu de client Notification Hubs gebruiken om pushmeldingen te verzenden naar geregistreerde apparaten. Zie voor meer informatie [pushmeldingen toevoegen aan uw app](app-service-mobile-ios-get-started-push.md). Zie voor meer informatie over Notification Hubs, [overzicht van Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

## <a name="tags"></a>Hoe: Enable gericht push met Tags
Notification Hubs kunt u gerichte meldingen verzenden naar specifieke registraties met behulp van codes. Verschillende labels worden automatisch gemaakt:

* De installatie-ID identificeert een specifiek apparaat.
* De gebruikers-Id op basis van de geverifieerde SID identificeert een specifieke gebruiker.

De installatie van de ID toegankelijk is vanaf de **omwille van** -eigenschap op de **MobileServiceClient**.  Het volgende voorbeeld ziet u hoe u een label toevoegen aan de installatie van een specifieke in Notification Hubs met een installatie-ID:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Alle codes die is verstrekt door de client tijdens de registratie voor pushberichten worden genegeerd door de back-end bij het maken van de installatie. Om een client labels toevoegen aan de installatie, moet u een aangepaste API die wordt toegevoegd met behulp van het vorige patroon labels maken.

Zie [Client toegevoegd push notification-tags] [ 5] in het voorbeeld van App Service Mobile Apps voltooide Quick Start voor een voorbeeld.

## <a name="push-user"></a>Hoe: pushmeldingen verzenden naar een geverifieerde gebruiker
Wanneer een geverifieerde gebruiker geregistreerd voor pushmeldingen, wordt een gebruikers-ID-tag automatisch toegevoegd aan de registratie. Met deze code, kunt u pushmeldingen verzendt naar alle apparaten die zijn geregistreerd door deze persoon. De volgende code wordt de SID van de gebruiker die de aanvraag en verzendt een pushmelding sjabloon naar elke apparaatregistratie voor die persoon:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Wanneer u registreert voor pushmeldingen vanuit een geverifieerde client, moet u ervoor zorgen dat de verificatie is voltooid voordat u de registratie. Zie voor meer informatie [doorgeven aan gebruikers] [ 6] in het voorbeeld van de voltooide Quick Start-App Service Mobile Apps voor .NET-back-end.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Procedure: fouten opsporen en oplossen van de Server .NET SDK
Azure App Service biedt verschillende opsporen en oplossen van technieken voor ASP.NET-toepassingen:

* [Bewaking van een Azure App Service](../app-service/web-sites-monitor.md)
* [Diagnostische logboekregistratie in Azure App Service inschakelen](../app-service/web-sites-enable-diagnostic-log.md)
* [Een Azure App Service in Visual Studio oplossen](../app-service/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Logboekregistratie
U kunt schrijven naar App Service diagnostische logboeken met behulp van de standaard ASP.NET-tracering schrijven. Voordat u naar Logboeken schrijven kan, moet u diagnostische gegevens inschakelen in uw back-end voor de mobiele App.

Diagnostische gegevens inschakelen en schrijven naar de logboeken:

1. Volg de stappen in [diagnostische gegevens inschakelen](../app-service/web-sites-enable-diagnostic-log.md#enablediag).
2. Voeg de volgende gebruiksinstructie in uw codebestand:

        using System.Web.Http.Tracing;
3. Maak een trace-schrijver van de .NET-back-end schrijven naar de diagnostische logboeken als volgt:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. Uw serverproject publiceren en toegang tot de mobiele App back-end voor het uitvoeren van het codepad met de logboekregistratie.
5. Download en evalueren van de logboeken, zoals beschreven in [hoe: Logboeken downloaden](../app-service/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Lokale foutopsporing met verificatie
U kunt uw toepassing lokaal om wijzigingen te testen voordat u ze naar de cloud publiceert uitvoeren. Druk op voor de meeste Azure Mobile Apps back-ends, *F5* tijdens het in Visual Studio. Er zijn echter enkele aanvullende overwegingen bij het gebruik van verificatie.

U moet een cloud-gebaseerde mobiele app met App Service-verificatie/autorisatie geconfigureerd hebben en de client het cloudeindpunt dat is opgegeven als de alternatieve aanmeldings-host moet hebben. Raadpleeg de documentatie voor uw clientplatform voor de specifieke stappen vereist.

Zorg ervoor dat uw mobiele back-end [Microsoft.Azure.Mobile.Server.Authentication] geïnstalleerd. Voeg in uw toepassing OWIN-Opstartklasse, het volgende na `MobileAppConfiguration` is toegepast op uw `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

In het voorgaande voorbeeld, configureert u moet de *authAudience* en *authIssuer* toepassingsinstellingen binnen uw Web.config-bestand op elke worden de URL van de hoofdmap van uw toepassing met behulp van het HTTPS-schema. U moet op dezelfde manier instellen *authSigningKey* moet de waarde van uw toepassing de handtekeningsleutel.
De handtekeningsleutel verkrijgen:

1. Navigeer naar uw app binnen de [Azure-portal]
2. Klik op **extra**, **Kudu**, **gaat**.
3. Klik op de site Kudu Management **omgeving**.
4. De waarde voor *WEBSITE\_AUTH\_ondertekening\_sleutel*.

Gebruik de sleutel voor de *authSigningKey* parameter in de configuratie van uw lokale toepassingen.  Uw mobiele back-end is nu ingericht om tokens bij lokale uitvoering waarmee de client de token van het eindpunt cloud-gebaseerde te valideren.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure-portal]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx
