---
title: Upgrade uitvoeren in Mobile Services in Azure App Service
description: Meer informatie over het eenvoudige upgrade van uw Mobile Services-toepassing in een App Service-mobiele App
services: app-service\mobile
documentationcenter: 
author: conceptdev
manager: crdun
editor: 
ms.assetid: 9c0ac353-afb6-462b-ab94-d91b8247322f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: f07b1d6037ff8ca16b673e6a1a235769355a9993
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Uw bestaande .NET Azure Mobile Service bijwerken naar App Service
App Service Mobile is een nieuwe manier om mobiele toepassingen bouwen met Microsoft Azure. Zie voor meer informatie, [wat zijn Mobile Apps?].

Dit onderwerp wordt beschreven hoe u een bestaande toepassing van de .NET-back-end van Azure Mobile Services bijwerken naar een nieuwe App Service Mobile Apps. Terwijl u deze upgrade uitvoert, moet uw bestaande Mobile Services-toepassing kunt blijven werken.   Als u upgraden van een back-end voor Node.js-toepassing wilt, raadpleegt u [upgraden van uw Node.js Mobile Services](app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Wanneer een mobiele back-end wordt bijgewerkt naar Azure App Service, wordt het toegang heeft tot alle functies van de App Service en worden in rekening gebracht volgens [App Service-prijzen], niet Mobile Services-prijzen.

## <a name="migrate-vs-upgrade"></a>Migreren upgraden
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Het is raadzaam dat u [uitvoeren van een migratie](app-service-mobile-migrating-from-mobile-services.md) voordat u doorgaat door middel van een upgrade. Op deze manier kunt u beide versies van uw toepassing op de dezelfde App Service-Plan geplaatst en er zijn geen extra kosten in rekening gebracht.
>
>

### <a name="improvements-in-mobile-apps-net-server-sdk"></a>Verbeteringen in Mobile Apps .NET server SDK
Een upgrade naar de nieuwe [Mobile Apps SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) biedt de volgende voordelen:

* Meer flexibiliteit in NuGet-afhankelijkheden. De hosting-omgeving biedt niet langer een eigen versies van het NuGet-pakketten, zodat u alternatieve compatibel versies kunt gebruiken. Als er nieuwe bugfixes voor essentiële of beveiligingsupdates naar de Mobile Server SDK of de afhankelijkheden, moet u handmatig een uw service bijwerken.
* Meer flexibiliteit in de mobiele SDK. U kunt expliciet bepalen welke functies en routes worden gedefinieerd, zoals verificatie, tabel-API's en het eindpunt van de registratie push. Zie voor meer informatie, [het gebruik van de server .NET SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).
* Ondersteuning voor andere typen van ASP.NET-project en routes. U kunt nu MVC en Web-API-domeincontrollers in hetzelfde project als uw mobiele back-endproject hosten.
* Ondersteuning voor nieuwe App Service-verificatie-functies waarmee u kunt een algemene authentication-configuratie gebruiken in uw web- en mobiele apps.

## <a name="overview"></a>Upgrade basisoverzicht
In veel gevallen is upgraden net zo eenvoudig als het overschakelen naar de nieuwe server voor Mobile Apps SDK en uw code naar een nieuw exemplaar van de mobiele App opnieuw te publiceren. Er zijn echter enkele scenario's waarvoor u enkele aanvullende configuratiestappen, zoals geavanceerde verificatie-scenario's en werken met geplande taken. Elk van deze wordt beschreven in latere secties.

> [!TIP]
> Het wordt aanbevolen dat u lees- en volledig inzicht in de rest van dit onderwerp voordat u een upgrade start. Noteer van alle onderdelen gebruik die hieronder worden genoemd.
>
>

De Mobile Services-client-SDK's zijn **niet** compatibel is met de nieuwe server voor Mobile Apps SDK. U moet wijzigingen niet aan een site die momenteel bedient gepubliceerde clients publiceren, zodat de continuïteit van de service voor uw app. In plaats daarvan moet u een nieuwe mobiele app die als een duplicaat fungeert maken. U kunt deze toepassing op de dezelfde App Service-abonnement om te vermijden extra financiële kosten plaatsen.

U moet dan twee versies van de toepassing: één blijft hetzelfde, en fungeert gepubliceerde apps in het wild en de andere die u kunt vervolgens een upgrade en doel met een nieuwe client-release. U kunt verplaatsen en testen van uw code in uw tempo, maar moet u ervoor zorgen dat u alle oplossingen voor problemen op beide toegepast. Als u denkt dat een gewenste aantal client-apps in het wild hebt bijgewerkt naar de nieuwste versie, u de oorspronkelijke gemigreerde app verwijderen kunt als u wenst.

Het volledige overzicht voor het upgradeproces is als volgt:

1. Een nieuwe mobiele App maken
2. Het project voor het gebruik van de nieuwe Server SDK's bijwerken
3. Een nieuwe versie van de clienttoepassing
4. (Optioneel) Verwijderen van uw oorspronkelijke gemigreerde exemplaar

## <a name="mobile-app-version"></a>Een tweede toepassingsexemplaar maken
De eerste stap bij het bijwerken is het maken van de resource voor de mobiele App die als voor de nieuwe versie van uw toepassing host fungeert. Als u al een bestaande mobiele service hebt gemigreerd, wilt u deze versie op hetzelfde hosting plan maken. Open de [Azure-portal] en navigeer naar uw gemigreerde toepassing. Noteer de App Service-Plan wordt uitgevoerd.

Maak vervolgens de tweede instantie van de toepassing door de [.NET backend maken instructies](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Kies het plan van uw gemigreerde toepassing wanneer u wordt gevraagd om te selecteren dat u App Service-abonnement of 'hosten plan'.

U zult waarschijnlijk dezelfde database en Notification Hub gebruiken zoals in Mobile Services. U kunt deze waarden kopiëren door het openen van [Azure-portal] en navigeren naar de oorspronkelijke toepassing, klikt u vervolgens op **instellingen** > **toepassingsinstellingen**. Onder **verbindingsreeksen**, kopie `MS_NotificationHubConnectionString` en `MS_TableConnectionString`. Navigeer naar uw nieuwe upgraden van site en plak deze in een bestaande waarden wordt overschreven. Herhaal dit proces voor andere toepassingsinstellingen de behoeften van uw app. Als een gemigreerde service niet gebruikt, kunt u lezen verbindingsreeksen en appinstellingen van de **configureren** tabblad van de Mobile Services-sectie van de [klassieke Azure-portal].

Maak een kopie van de ASP.NET-project voor uw toepassing en deze publiceren naar de nieuwe site. Met behulp van een kopie van de clienttoepassing bijgewerkt met de nieuwe URL controleren of alles werkt zoals verwacht.

## <a name="updating-the-server-project"></a>Bijwerken van het serverproject
Mobiele Apps biedt een nieuwe [Mobile App Server SDK] waarmee u veel van dezelfde functionaliteit als de Mobile Services-runtime. U moet eerst alle verwijzingen naar de Mobile Services-pakketten verwijderen. Zoek in de NuGet-Pakketbeheer voor `WindowsAzure.MobileServices.Backend`. De meeste apps ziet verschillende pakketten hier, met inbegrip van `WindowsAzure.MobileServices.Backend.Tables` en `WindowsAzure.MobileServices.Backend.Entity`. In dat geval moet beginnen met het laagste pakket in de afhankelijkheidsstructuur van zoals `Entity`, en deze te verwijderen. Wanneer u wordt gevraagd, verwijder alle afhankelijke pakketten niet. Herhaal dit proces totdat u hebt verwijderd `WindowsAzure.MobileServices.Backend` zelf.

Op dit moment hebt u een project dat verwijst niet meer naar Mobile Services SDK's.

Vervolgens voegt u verwijzingen naar de Mobile Apps SDK. Voor deze upgrade de meeste ontwikkelaars wilt downloaden en installeren van de `Microsoft.Azure.Mobile.Server.Quickstart` inpakken, zoals dit, in de hele set vereist klikt wordt.

Er zijn een aantal compilatiefouten ten gevolge van verschillen tussen de SDK's, maar deze zijn gemakkelijk te adres en in de rest van deze sectie worden behandeld.

### <a name="base-configuration"></a>Basisconfiguratie
Klik, in WebApiConfig.cs, kunt u vervangen:

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

met

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

> [!NOTE]
> Als u meer informatie over de nieuwe server met .NET SDK en hoe u functies van uw app toevoegen of verwijderen wilt, raadpleegt u de [het gebruik van de server .NET SDK] onderwerp.
>
>

Als uw app maakt gebruik van de verificatiefuncties, moet u ook een OWIN-middleware registreren. In dit geval moet u de bovenstaande configuratiecode verplaatsen naar een nieuwe OWIN-Opstartklasse.

1. Het NuGet-pakket toevoegen `Microsoft.Owin.Host.SystemWeb` als deze niet in uw project opgenomen wordt.
2. In Visual Studio, klik met de rechtermuisknop op uw project en selecteer **toevoegen** -> **Nieuw Item**. Selecteer **Web** -> **algemene** -> **OWIN-Opstartklasse**.
3. Verplaatsen van de bovenstaande code voor MobileAppConfiguration van `WebApiConfig.Register()` naar de `Configuration()` methode van uw nieuwe starten van de klasse.

Zorg ervoor dat de `Configuration()` methode eindigt op:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Er zijn aanvullende wijzigingen die verband houden met verificatie, die in de onderstaande sectie van volledige authenticatie worden besproken.

### <a name="working-with-data"></a>Werken met gegevens
In Mobile Services, wordt de naam van de mobiele app behandeld als de naam van het standaard schema in de Entity Framework-installatie.

Om ervoor te zorgen dat u hetzelfde schema wordt verwezen als voorheen, gebruik de volgende instellen van het schema in de DbContext voor uw toepassing hebt:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Controleer of dat u hebt ingesteld als u de bovenstaande doen MS_MobileServiceName. Als uw toepassing dit eerder hebt aangepast, kunt u de naam van een ander schema opgeven.

### <a name="system-properties"></a>Systeemeigenschappen
#### <a name="naming"></a>Naamgeving
In de Azure Mobile Services-server SDK Systeemeigenschappen altijd een dubbele onderstrepingsteken bevatten (`__`) voorvoegsel voor de eigenschappen:

* __createdAt
* __updatedAt
* __deleted
* __version

De Mobile Services-client-SDK's hebben speciale logica voor het parseren van Systeemeigenschappen in deze indeling.

In Azure Mobile Apps niet langer Systeemeigenschappen hebben een speciale indeling en hebben de volgende namen:

* CreatedAt
* updatedAt
* verwijderd
* versie

De client-Mobile Apps SDK's gebruiken de nieuwe system eigenschappen namen, zodat er geen wijzigingen zijn vereist voor de clientcode. Echter, als u rechtstreeks REST-aanroepen naar de service vervolgens moet u uw query's dienovereenkomstig.

#### <a name="local-store"></a>Lokale archief
De wijzigingen in de namen van eigenschappen betekent dat een lokale database offlinesynchronisatie voor Mobile Services is niet compatibel met Mobile Apps. Indien mogelijk, dient u geen upgrade van client-apps Mobile Services voor mobiele Apps pas na wijzigingen in behandeling zijn verzonden naar de server. De bijgewerkte app moet vervolgens gebruikt u de bestandsnaam van een nieuwe database.

Als een client-app is geüpgraded van Mobile Services voor mobiele Apps wanneer er een offline wijzigingen in behandeling in de wachtrij bewerking, moet de systeemdatabase voor het gebruik van de nieuwe kolomnamen worden bijgewerkt. In iOS, kan dit worden bereikt met lightweight migraties om de kolomnamen te wijzigen. Op Android- en de beheerde .NET-client, moet u aangepaste SQL om de naam van de kolommen voor de object-gegevenstabellen te schrijven.

In iOS, moet u uw schema Core gegevens voor uw gegevensentiteiten overeenkomen met het volgende wijzigen. Houd er rekening mee dat de eigenschappen `createdAt`, `updatedAt` en `version` niet langer een `ms_` voorvoegsel:

| Kenmerk | Type | Opmerking |
| --- | --- | --- |
| id |Tekenreeks is gemarkeerd als vereist |primaire sleutel in externe opslag |
| CreatedAt |Date |(optioneel) is toegewezen aan de eigenschap system createdAt |
| updatedAt |Date |(optioneel) is toegewezen aan de eigenschap system updatedAt |
| versie |Tekenreeks |(optioneel) gebruikt voor het opsporen van conflicten, is toegewezen aan versie |

#### <a name="querying-system-properties"></a>Eigenschappen van het uitvoeren van query 's
In Azure Mobile Services, Systeemeigenschappen worden niet verzonden standaard, maar alleen wanneer ze worden aangevraagd met behulp van de queryreeks `__systemProperties`. Daarentegen in Azure Mobile Apps-systeem eigenschappen zijn **altijd ingeschakeld** omdat ze deel van de server SDK-objectmodel uitmaken.

Deze wijziging hoofdzakelijk van invloed is op aangepaste implementaties van domein managers, zoals de uitbreidingen van `MappedEntityDomainManager`. In Mobile Services, als een client nooit vraagt om alle Systeemeigenschappen is het mogelijk gebruik van een `MappedEntityDomainManager` die daadwerkelijk wijst niet alle eigenschappen. In Azure Mobile Apps veroorzaakt deze niet-toegewezen eigenschappen echter een fout in GET-query's.

De eenvoudigste manier om het probleem te verhelpen, is uw DTOs wijzigen zodat ze van overnemen `ITableData` in plaats van `EntityData`. Voeg vervolgens de `[NotMapped]` kenmerk in de velden die moeten worden weggelaten.

De volgende definieert bijvoorbeeld `TodoItem` zonder system-eigenschappen:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Opmerking: als er fouten optreden op `NotMapped`, Voeg een verwijzing naar de assembly `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS
Sommige ondersteuning voor CORS Mobile Services opgenomen door de ASP.NET-CORS-oplossing. Deze laag wrapping is verwijderd als u de ontwikkelaar geven meer controle, zodat u rechtstreeks gebruikmaken van kunt [ASP.NET CORS-ondersteuning](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

De belangrijkste gebieden van belang als u met behulp van CORS zijn die de `eTag` en `Location` headers moeten worden toegestaan om de client-SDK's goed te laten werken.

### <a name="push-notifications"></a>Pushmeldingen
Voor pushmeldingen is het belangrijkste item dat u ontbreekt in de SDK-Server kan vinden in de PushRegistrationHandler-klasse. Registraties iets anders in mobiele Apps worden verwerkt en tagless registraties zijn standaard ingeschakeld. Het beheren van labels kan worden bewerkstelligd met behulp van aangepaste API's. Zie de [registreren voor labels](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) instructies voor meer informatie.

### <a name="scheduled-jobs"></a>Geplande taken
Geplande taken worden niet is ingebouwd in Mobile Apps, zodat eventuele bestaande taken die u in uw .NET-back-end hebt moet afzonderlijk worden bijgewerkt. Een mogelijkheid is het maken van een geplande [Web taak] op de mobiele App-code-site. Kan ook een domeincontroller die uw taakcode instellen en configureren de [Azure Scheduler] naar dat eindpunt van het verwachte schema bereikt.

### <a name="miscellaneous-changes"></a>Diverse wijzigingen
Alle ApiControllers die worden gebruikt door een mobiele client moet nu de `[MobileAppController]` kenmerk. Dit is niet langer zodat andere ApiControllers gaan hierdoor niet beïnvloed door de mobiele formatters standaard opgenomen.

De `ApiServices` object is niet langer deel uitmaakt van de SDK. Mobiele App om instellingen te openen, kunt u het volgende:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Logboekregistratie wordt op dezelfde manier nu bereikt met het standaard ASP.NET-tracering schrijven:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

## <a name="authentication"></a>Overwegingen voor verificatie
De verificatieonderdelen van Mobile Services zijn nu in de App Service-verificatie/autorisatie-functie verplaatst. U kunt meer informatie over het inschakelen van dit voor uw site door te lezen de [verificatie toevoegen aan uw mobiele app](app-service-mobile-ios-get-started-users.md) onderwerp.

Voor sommige providers, zoals AAD, Facebook en Google, moet u het volgende kunnen gebruikmaken van de bestaande registratie van uw toepassing kopiëren. U hoeft te navigeren naar de portal van de identiteitsprovider en een nieuwe Omleidings-URL toevoegen aan de registratie. App Service-verificatie/autorisatie configureren met het client-ID en het geheim.

### <a name="controller-action-authorization"></a>Controller actie autorisatie
Alle geopende exemplaren van de `[AuthorizeLevel(AuthorizationLevel.User)]` kenmerk moet nu worden gewijzigd voor het gebruik van de standaard ASP.NET `[Authorize]` kenmerk. Bovendien zijn domeincontrollers nu Anoniem standaard, zoals in andere ASP.NET-toepassingen.
Als u een van de andere AuthorizeLevel opties, zoals Admin of toepassing, houd er rekening mee dat deze verwijderd zijn. U kunt in plaats daarvan AuthorizationFilters voor gedeelde geheimen instellen of configureren van een AAD-Service-Principal veilig zodat service-naar-service aanroepen.

### <a name="getting-additional-user-information"></a>Aanvullende informatie ophalen
Krijgt u extra gebruikersgegevens, inclusief toegangstokens via de `GetAppServiceIdentityAsync()` methode:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Bovendien als uw toepassing afhankelijkheden op gebruikers-id's, duurt zoals ze opslaan in een database, is het belangrijk te weten dat de gebruikers-id's tussen Mobile Services en App Service Mobile Apps niet identiek zijn. U kunt echter nog steeds de Mobile Services gebruikers-ID ophalen. Alle ProviderCredentials subklassen hebben een gebruikers-id-eigenschap. Zodat u verder gaat in het voorbeeld voordat:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Als uw app eventuele afhankelijkheden op gebruikers-id's neemt, is het belangrijk dat u gebruikmaken van dezelfde registratie met een id-provider indien mogelijk. Gebruikers-id's worden doorgaans binnen het bereik van de registratie van de toepassing die is gebruikt, zodat u problemen met de overeenkomende gebruikers tot hun gegevens introductie van een nieuwe registratie kan maken.

### <a name="custom-authentication"></a>Aangepaste verificatie
Als uw app van een aangepaste verificatie-oplossing gebruikmaakt, wordt u om ervoor te zorgen dat de bijgewerkte site toegang tot het systeem heeft. De nieuwe instructies voor de aangepaste verificatie in de [.NET SDK overzicht] voor het integreren van uw oplossing. Houd er rekening mee dat de aangepaste verificatie-onderdelen nog steeds in preview zijn.

## <a name="updating-clients"></a>Clients bijwerken
Zodra u een operationele back-end voor mobiele apps hebt, kunt u werken op een nieuwe versie van uw clienttoepassing waarin deze worden verbruikt. Mobile Apps bevat ook een nieuwe versie van de client-SDK's en net als bij de upgrade van de server hierboven, moet u alle verwijzingen naar de Mobile Services SDK's verwijderen voordat u de Mobile Apps-versie installeert.

Een van de belangrijkste wijzigingen tussen de versies is dat de constructors zijn niet langer vereist voor de sleutel van een toepassing. U nu doorgeven gewoon in de URL van uw mobiele App. Bijvoorbeeld: op de .NET-clients de `MobileServiceClient` constructor is nu:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

U kunt lezen over het installeren van de nieuwe SDK's en met behulp van de nieuwe structuur via de onderstaande koppelingen:

* [iOS-versie 3.0.0 of hoger](app-service-mobile-ios-how-to-use-client-library.md)
* [.NET (Windows/Xamarin) versie 2.0.0 of hoger](app-service-mobile-dotnet-how-to-use-client-library.md)

Als uw toepassing maakt gebruik van pushmeldingen, noteer de specifieke registratie-instructies voor elk platform, zijn omdat er er enkele wijzigingen ook.

Wanneer u de nieuwe clientversie gereed hebt, try it out in op basis van uw project bijgewerkte server. Na de validatie of deze werkt, kunt u een nieuwe versie van uw toepassing op klanten vrijgeven. Zodra uw klanten hebt om deze updates te ontvangen, kunt u uiteindelijk de Mobile Services-versie van uw app verwijderen. U hebt op dit moment volledig bijgewerkt naar een App Service Mobile App met behulp van de meest recente server-Mobile Apps SDK.

<!-- URLs. -->

[Azure-portal]: https://portal.azure.com/
[klassieke Azure-portal]: https://manage.windowsazure.com/
[wat zijn Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web taak]: https://github.com/Azure/azure-webjobs-sdk/wiki
[het gebruik van de server .NET SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[App Service-prijzen]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET SDK overzicht]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
