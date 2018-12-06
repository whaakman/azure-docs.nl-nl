---
title: Upgrade van Mobile Services naar Azure App Service
description: Leer hoe u eenvoudig uw Mobile Services-toepassing naar een App Service Mobile App upgraden
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 9c0ac353-afb6-462b-ab94-d91b8247322f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 4fd896e3b26ae66621ba49d1bb8a5d86c0dd63ee
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52963263"
---
# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Uw bestaande .NET Azure Mobile Service upgraden naar App Service
App Service Mobile is een nieuwe manier voor het bouwen van mobiele toepassingen met Microsoft Azure. Zie voor meer informatie, [Wat zijn mobiele apps?].

In dit onderwerp wordt beschreven hoe u het bijwerken van een bestaande .NET-back-endtoepassing met Azure Mobile Services naar een nieuwe App Service Mobile Apps. Hoewel u deze upgrade uitvoeren, moet uw bestaande Mobile Services-toepassing kan blijven functioneren.   Als u een Node.js-back-end-toepassing upgraden wilt, raadpleegt u [upgraden van uw Mobile Services van Node.js](app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Een mobiele back-end wordt bijgewerkt naar Azure App Service, wordt toegang heeft tot alle functies van App Service en worden in rekening gebracht volgens [prijzen van App Service], niet Mobile Services-prijzen.

## <a name="migrate-vs-upgrade"></a>Migreren upgraden
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Het is raadzaam dat u [een migratie uit te voeren](app-service-mobile-migrating-from-mobile-services.md) voordat er een upgrade voor aangebracht. Op deze manier kunt u beide versies van uw toepassing op de dezelfde App Service-Plan plaatsen en geen extra kosten in rekening gebracht.
>
>

### <a name="improvements-in-mobile-apps-net-server-sdk"></a>Verbeteringen in Mobile Apps .NET server SDK
Een upgrade naar de nieuwe [Mobile Apps SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) biedt de volgende voordelen:

* Meer flexibiliteit op NuGet-afhankelijkheden. De hosting-omgeving biedt niet langer een eigen versie van NuGet-pakketten, zodat u alternatieve compatibele versies kunt. Als er nieuwe belangrijke bugfixes of beveiligingsupdates op de Server-SDK voor Mobile of afhankelijkheden, moet u handmatig een uw service bijwerken.
* Meer flexibiliteit in de mobiele SDK. U kunt expliciet bepalen welke functies en routes zijn gedefinieerd, zoals verificatie, tabel-API's en het eindpunt van de registratie van pushmeldingen. Zie voor meer informatie, [over het gebruik van de server .NET-SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).
* Ondersteuning voor andere typen van ASP.NET-project en routes. U kunt nu MVC en Web-API-domeincontrollers in hetzelfde project gemaakt als uw mobiele back-endproject hosten.
* Ondersteuning voor nieuwe App Service-verificatiefuncties, waarmee u een algemene verificatieconfiguratie gebruiken voor uw web- en mobiele apps.

## <a name="overview"></a>Basisoverzicht van upgrade
In veel gevallen is een upgrade net zo eenvoudig als het overschakelen naar de server-nieuwe Mobile Apps SDK en uw code naar een nieuw exemplaar van de mobiele App opnieuw te publiceren. Er zijn echter enkele scenario's waarvoor u enkele aanvullende configuratiestappen, zoals geavanceerde verificatie-scenario's en het werken met geplande taken. Elk van deze wordt beschreven in latere secties.

> [!TIP]
> Het wordt aanbevolen dat u lees- en de rest van dit onderwerp volledig begrijpen voordat u een upgrade wordt gestart. Houd er rekening mee te maken van een van de functies gebruik die hieronder worden genoemd.
>
>

De Mobile Services-client-SDK's zijn **niet** compatibel is met de server-nieuwe Mobile Apps SDK. Zodat de continuïteit van de service voor uw app, moet u geen wijzigingen aan een site momenteel fungeert gepubliceerde clients publiceren. In plaats daarvan moet u een nieuwe mobiele app die als een duplicaat fungeert maken. U kunt deze toepassing op de dezelfde App Service-plan om te vermijden extra financiële kosten plaatsen.

U hebt dan twee versies van de toepassing: een die hetzelfde blijft en dient gepubliceerde apps in het gebruik en de andere die u kunt vervolgens een upgrade en een nieuwe versie van de client is gericht. U kunt verplaatsen en het testen van uw code in uw eigen tempo, maar moet u ervoor zorgen dat u eventuele oplossingen voor problemen op beide worden toegepast. Als u denkt dat een gewenste aantal client-apps in de natuur hebt bijgewerkt naar de nieuwste versie, u kunt de oorspronkelijke gemigreerde app verwijderen als u wilt.

Het volledige overzicht voor het upgradeproces is als volgt:

1. Maak een nieuwe mobiele App
2. Het project voor het gebruik van de nieuwe Server-SDK's bijwerken
3. Een nieuwe versie van de clienttoepassing
4. (Optioneel) Uw oorspronkelijke gemigreerde exemplaar verwijderen

## <a name="mobile-app-version"></a>Het maken van een tweede toepassingsexemplaar
De eerste stap bij het bijwerken is het maken van de mobiele App-resource die als voor de nieuwe versie van uw toepassing host fungeert. Als u al een bestaande mobiele service hebt gemigreerd, wilt u deze versie op het hostingabonnement maken. Open de [Azure Portal] en navigeer naar uw gemigreerde toepassing. Noteer de App Service-Plan wordt uitgevoerd.

Maak vervolgens de tweede toepassingsinstantie door de [.NET-back-end maken instructies](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Wanneer u wordt gevraagd om te selecteren dat u kiezen App Service-Plan of 'hostingabonnement' van het abonnement van uw gemigreerde toepassing.

U zult waarschijnlijk de dezelfde database en de Notification Hub gebruiken net als bij Mobile Services. U kunt deze waarden kopiëren door het openen van [Azure Portal] en te navigeren naar de oorspronkelijke toepassing, klikt u vervolgens op **instellingen** > **toepassingsinstellingen**. Onder **verbindingsreeksen**, kopie `MS_NotificationHubConnectionString` en `MS_TableConnectionString`. Navigeer naar uw nieuwe upgraden van site en plak deze in bestaande waarden overschreven. Herhaal dit proces voor alle andere toepassingsinstellingen behoeften voor uw app. Als een gemigreerde service niet gebruikt, kunt u lezen en verbindingsreeksen en appinstellingen van de **configureren** tabblad van de Mobile Services-sectie van de [klassieke Azure-portal].

Maak een kopie van de ASP.NET-project voor uw toepassing en deze publiceren naar de nieuwe site. Met behulp van een kopie van uw clienttoepassing bijgewerkt met de nieuwe URL, valideren dat alles werkt zoals verwacht.

## <a name="updating-the-server-project"></a>Bijwerken van het serverproject
Mobile Apps biedt een nieuwe [Mobiele App Server SDK] waarmee veel van dezelfde functionaliteit als de Mobile Services-runtime. Verwijder eerst alle verwijzingen naar de Mobile Services-pakketten. Zoek in het NuGet-Pakketbeheer `WindowsAzure.MobileServices.Backend`. De meeste apps ziet verschillende pakketten hier, met inbegrip van `WindowsAzure.MobileServices.Backend.Tables` en `WindowsAzure.MobileServices.Backend.Entity`. In dat geval moet beginnen met het laagste pakket in de structuur van afhankelijkheden, zoals `Entity`, en deze te verwijderen. Wanneer u hierom wordt gevraagd, niet alle afhankelijke pakketten verwijderen. Herhaal dit proces totdat u hebt verwijderd `WindowsAzure.MobileServices.Backend` zelf.

Op dit moment hebt u een project dat niet meer wordt verwezen naar Mobile Services SDK's.

Vervolgens voegt u verwijzingen naar de Mobile Apps SDK. Deze upgrade de meeste ontwikkelaars wilt downloaden en installeren van de `Microsoft.Azure.Mobile.Server.Quickstart` pakket, zoals deze wordt opgehaald in de volledige set vereist.

Er is een groot aantal compilatiefouten die voortvloeien uit de verschillen tussen de SDK's, maar deze zijn eenvoudig te verhelpen en worden behandeld in de rest van deze sectie.

### <a name="base-configuration"></a>Basisconfiguratie
Klik in WebApiConfig.cs, u het volgende vervangen kunt:

```csharp
// Use this class to set configuration options for your mobile service
ConfigOptions options = new ConfigOptions();

// Use this class to set WebAPI configuration options
HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));
```

met

```csharp
HttpConfiguration config = new HttpConfiguration();
new MobileAppConfiguration()
    .UseDefaultConfiguration()
.ApplyTo(config);

```

> [!NOTE]
> Als u meer informatie over de nieuwe server .NET-SDK en hoe u functies van uw app toevoegen wilt/verwijderen, raadpleegt u de [Het gebruik van de server .NET-SDK] onderwerp.
>
>

Als uw app maakt gebruik van de verificatiefuncties voor, moet u ook een OWIN-middleware registreren. In dit geval moet u de bovenstaande configuratiecode verplaatsen naar een nieuwe OWIN-Opstartklasse.

1. NuGet-pakket toevoegen `Microsoft.Owin.Host.SystemWeb` als deze nog niet zijn opgenomen in uw project.
2. In Visual Studio, klik met de rechtermuisknop op uw project en selecteer **toevoegen** -> **Nieuw Item**. Selecteer **Web** -> **algemene** -> **OWIN-Opstartklasse**.
3. Verplaatst u de bovenstaande code voor MobileAppConfiguration van `WebApiConfig.Register()` naar de `Configuration()` -methode van uw nieuwe Opstartklasse.

Zorg ervoor dat de `Configuration()` methode eindigt op:

```csharp
app.UseWebApi(config)
app.UseAppServiceAuthentication(config);
```

Er zijn aanvullende wijzigingen met betrekking tot verificatie die in de volledige verificatie hieronder worden besproken.

### <a name="working-with-data"></a>Werken met gegevens
In Mobile Services, wordt de naam van de mobiele app behandeld als de naam van het standaard schema in de instellingen van Entity Framework.

Om ervoor te zorgen dat u hetzelfde schema wordt verwezen als voorheen, gebruik het volgende om in te stellen van het schema in de DbContext voor uw toepassing:

```csharp
string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");
```

Zorg ervoor dat u hebt ingesteld als u de bovenstaande MS_MobileServiceName. Als uw toepassing eerder dit aangepast, kunt u de naam van een ander schema opgeven.

### <a name="system-properties"></a>Systeemeigenschappen
#### <a name="naming"></a>Naamgeving
In de Azure Mobile Services-server SDK Systeemeigenschappen altijd een dubbele onderstrepingstekens bevatten (`__`) voorvoegsel voor de eigenschappen:

* __createdAt
* __updatedAt
* __deleted
* __version

De Mobile Services-client-SDK's hebben speciale logica voor het parseren van Systeemeigenschappen in deze indeling.

In Azure Mobile Apps, niet meer Systeemeigenschappen hebben een speciale indeling en hebben de volgende namen:

* createdAt
* updatedAt
* verwijderd
* versie

De client-SDK's voor de mobiele Apps gebruiken de nieuwe namen in de eigenschappen van systeem, zodat er geen wijzigingen zijn vereist voor het clientcode. Echter, als u direct REST-aanroepen naar uw service vervolgens moet u uw query's dienovereenkomstig.

#### <a name="local-store"></a>Lokale archief
De wijzigingen in de namen van Systeemeigenschappen van het betekenen een lokale database offlinesynchronisatie voor Mobile Services is niet compatibel met Mobile Apps. Indien mogelijk moet u niet een upgrade van client-apps vanuit Mobile Services naar Mobile Apps pas na wijzigingen in behandeling zijn verzonden naar de server. De bijgewerkte app moet vervolgens gebruikt u de bestandsnaam van een nieuwe database.

Als een client-app is geüpgraded van Mobile Services naar Mobile Apps, terwijl er offline wijzigingen in behandeling in wachtrij voor de bewerking zijn, moet de database voor het gebruik van de nieuwe kolomnamen worden bijgewerkt. Op iOS-, kan dit worden bereikt met behulp van eenvoudige migraties te wijzigen van de kolomnamen. Op Android en de beheerde .NET-client, moet u aangepaste SQL om de naam van de kolommen voor het object gegevenstabellen te schrijven.

Op iOS-, moet u het schema van de belangrijkste gegevens voor uw gegevensentiteiten zodat deze overeenkomen met de volgende wijzigen. Houd er rekening mee dat de eigenschappen `createdAt`, `updatedAt` en `version` hoeft niet langer een `ms_` voorvoegsel:

| Kenmerk | Type | Opmerking |
| --- | --- | --- |
| id |Tekenreeks, gemarkeerd als vereist |primaire sleutel in de externe opslag |
| createdAt |Date |(optioneel) is toegewezen aan createdAt systeemeigenschap |
| updatedAt |Date |(optioneel) is toegewezen aan updatedAt systeemeigenschap |
| versie |Reeks |(optioneel) gebruikt voor het detecteren van conflicten, toegewezen aan versie |

#### <a name="querying-system-properties"></a>Eigenschappen van het uitvoeren van query 's
In Azure Mobile Services, Systeemeigenschappen worden niet verzonden standaard, maar alleen wanneer ze worden opgevraagd met behulp van de queryreeks `__systemProperties`. Daarentegen in Azure Mobile Apps-systeem eigenschappen zijn **altijd ingeschakeld** omdat ze deel van de server SDK-objectmodel uitmaken.

Deze wijziging voornamelijk heeft gevolgen voor aangepaste implementaties van domein managers, zoals de uitbreidingen van `MappedEntityDomainManager`. In Mobile Services, als een client nooit alle eigenschappen vraagt, is het mogelijk gebruik van een `MappedEntityDomainManager` die daadwerkelijk wijst niet alle eigenschappen. In Azure Mobile Apps veroorzaakt deze niet-toegewezen eigenschappen echter een fout in GET-query's.

De eenvoudigste manier om het probleem te verhelpen is aan de Leeskant wijzigen zodat ze van overnemen `ITableData` in plaats van `EntityData`. Voeg vervolgens de `[NotMapped]` kenmerk in de velden die moeten worden weggelaten.

Bijvoorbeeld, het volgende wordt gedefinieerd `TodoItem` zonder system-eigenschappen:

```csharp
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
```

Opmerking: als er fouten optreden op `NotMapped`, Voeg een verwijzing naar de assembly `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS
Sommige ondersteuning voor CORS Mobile Services opgenomen door de ASP.NET-CORS-oplossing. Deze laag wrapping is verwijderd als u wilt de ontwikkelaar geven meer controle wilt, zodat u kunt rechtstreeks gebruikmaken van [ASP.NET CORS-ondersteuning](https://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

De belangrijkste gebieden van belang als u met behulp van CORS zijn die de `eTag` en `Location` headers moeten worden toegestaan om de client-SDK's goed te laten werken.

### <a name="push-notifications"></a>Pushmeldingen
Voor pushmeldingen is het belangrijkste item dat u ontbreekt in de SDK-Server kan vinden de PushRegistrationHandler-klasse. Registraties net iets anders worden verwerkt in Mobile Apps en tagless registraties zijn standaard ingeschakeld. Beheren van labels kan worden bewerkstelligd met behulp van aangepaste API's. Raadpleeg de [registreren voor tags](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) instructies voor meer informatie.

### <a name="scheduled-jobs"></a>Geplande taken
Geplande taken zijn niet ingebouwd in Mobile Apps, zodat bestaande taken die u in uw .NET-back-end hebt moet afzonderlijk worden bijgewerkt. Een optie is het maken van een geplande [Web Job] op de mobiele App-code-site. U kunt ook instellen op een domeincontroller die de taakcode van uw en configureren de [Azure Scheduler] naar dat eindpunt op het verwachte schema bereikt.

### <a name="miscellaneous-changes"></a>Diverse wijzigingen
Alle ApiControllers die worden gebruikt door een mobiele client moet nu de `[MobileAppController]` kenmerk. Dit is niet meer opgenomen in de standaardinstelling zodat andere ApiControllers gaan niet beïnvloed door de mobiele bevatten.

De `ApiServices` object is niet langer deel uit van de SDK. Voor toegang tot de instellingen voor mobiele App, kunt u het volgende gebruiken:

```csharp
MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
```

Logboekregistratie wordt op dezelfde manier nu bereikt met behulp van de standaard ASP.NET-tracering schrijven:

```csharp
ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
traceWriter.Info("Hello, World");  
```

## <a name="authentication"></a>Overwegingen voor verificatie
De verificatieonderdelen van Mobile Services zijn nu in de functie App Service-verificatie/autorisatie verplaatst. U kunt meer informatie over dit inschakelen voor uw site door te lezen de [verificatie toevoegen aan uw mobiele app](app-service-mobile-ios-get-started-users.md) onderwerp.

Voor sommige providers, zoals AAD, Facebook en Google, moet u het volgende kunnen gebruikmaken van de bestaande registratie van uw toepassing kopiëren. U hoeft alleen gaat u naar de portal van de id-provider en een nieuwe Omleidings-URL toevoegen aan de registratie. Verificatie/autorisatie van App Service configureren met de client-ID en geheim.

### <a name="controller-action-authorization"></a>Controller actie autorisatie
Alle exemplaren van de `[AuthorizeLevel(AuthorizationLevel.User)]` kenmerk moet nu worden gewijzigd voor het gebruik van de standaard ASP.NET `[Authorize]` kenmerk. Bovendien domeincontrollers worden nu Anoniem standaard, zoals in andere ASP.NET-toepassingen.
Als u een van de andere AuthorizeLevel opties, zoals Admin of toepassing, houd er rekening mee dat deze verwijderd zijn. U kunt in plaats daarvan AuthorizationFilters instellen voor gedeelde geheimen of een AAD-Service-Principal om in te schakelen van service-naar-serviceaanroepen veilig configureren.

### <a name="getting-additional-user-information"></a>Het ophalen van aanvullende gegevens
U krijgt aanvullende informatie, met inbegrip van tokens voor toegang tot en met de `GetAppServiceIdentityAsync()` methode:

```csharp
FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();
```

Bovendien, als uw toepassing afhankelijkheden op gebruikers-id's, heeft zoals opslaat in een database, is het belangrijk te weten dat de gebruikers-id's tussen Mobile Services en App Service Mobile Apps verschillend zijn. U kunt echter nog steeds de Mobile Services gebruikers-ID ophalen. Alle van de subklassen ProviderCredentials hebben een gebruikers-id-eigenschap. Dus voortgezet uit het voorbeeld voor:

```csharp
string mobileServicesUserId = creds.Provider + ":" + creds.UserId;
```

Als uw app eventuele afhankelijkheden op gebruikers-id's neemt, is het belangrijk dat u gebruikmaken van dezelfde registratie met een id-provider indien mogelijk. Gebruikers-id's zijn doorgaans gericht op de registratie van de toepassing die is gebruikt, dus maak kennis met de registratie van een nieuwe problemen met overeenkomende gebruikers tot hun gegevens kan maken.

### <a name="custom-authentication"></a>Aangepaste verificatie
Als uw app van een oplossing voor aangepaste verificatie gebruikmaakt, wordt u om ervoor te zorgen dat de bijgewerkte site toegang tot het systeem heeft. De nieuwe instructies voor de aangepaste verificatie in de [Overzicht van de .NET server SDK] integreren van uw oplossing. Houd er rekening mee dat de aangepaste verificatie-onderdelen nog in preview zijn.

## <a name="updating-clients"></a>Clients bijwerken
Zodra u een operationele backend voor mobiele apps hebt, kunt u werken op een nieuwe versie van uw clienttoepassing die wordt verbruikt. Mobile Apps bevat ook een nieuwe versie van de client-SDK's en net als bij de upgrade van de server boven, u moet alle verwijzingen naar de Mobile Services SDK's verwijderen voordat u de installatie van de Mobile Apps-versies.

Een van de belangrijkste wijzigingen tussen de versies is dat de constructors niet langer vereist voor de sleutel van een toepassing. U nu doorgeven gewoon in de URL van uw mobiele App. Bijvoorbeeld: op de .NET-clients de `MobileServiceClient` constructor is nu:

```csharp
public static MobileServiceClient MobileService = new MobileServiceClient(
    "https://contoso.azurewebsites.net", // URL of the Mobile App
);
```

U kunt lezen over het installeren van de nieuwe SDK's en met behulp van de nieuwe structuur via de onderstaande koppelingen:

* [iOS-versie 3.0.0 of hoger](app-service-mobile-ios-how-to-use-client-library.md)
* [.NET (Windows/Xamarin) versie 2.0.0 of hoger](app-service-mobile-dotnet-how-to-use-client-library.md)

Als uw toepassing maakt gebruik van push-meldingen, noteert u de registratie van de specifieke instructies voor elk platform, als er zijn enkele wijzigingen er ook.

Wanneer u de nieuwe clientversie gereed, probeer het op basis van uw project bijgewerkte server. Nadat is gevalideerd dat het werkt, kunt u een nieuwe versie van uw toepassing op klanten vrijgeven. Uiteindelijk, nadat uw klanten hebben al een kans om deze updates te ontvangen, kunt u de versie van de Mobile Services van uw app verwijderen. U hebt op dit moment volledig bijgewerkt naar een App Service Mobile App met behulp van de meest recente Mobile Apps-server-SDK.

<!-- URLs. -->

[Azure Portal]: https://portal.azure.com/
[klassieke Azure-portal]: https://manage.windowsazure.com/
[Wat zijn mobiele apps?]: app-service-mobile-value-prop.md
[Mobiele App Server SDK]: https://www.nuget.org/packages/microsoft.azure.mobile.server
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /azure/scheduler/
[Web Job]: https://github.com/Azure/azure-webjobs-sdk/wiki
[Het gebruik van de server .NET-SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[prijzen van App Service]: https://azure.microsoft.com/pricing/details/app-service/
[Overzicht van de .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
