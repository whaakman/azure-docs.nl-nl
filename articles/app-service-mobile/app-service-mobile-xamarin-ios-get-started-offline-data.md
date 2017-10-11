---
title: Offlinesynchronisatie voor uw mobiele Apps van Azure (Xamarin iOS) inschakelen
description: Informatie over het gebruik van App Service-mobiele App aan cache en sync offline gegevens in uw Xamarin iOS-toepassing
documentationcenter: xamarin
author: ggailey777
manager: cfowler
editor: 
services: app-service\mobile
ms.assetid: 828a287c-5d58-4540-9527-1309ebb0f32b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: b5878d8a2e18cf08b6e9074ecf40cd732624f0c0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Offlinesynchronisatie inschakelen voor uw mobiele Xamarin.iOS-app
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie maakt u kennis met de functie offline synchroniseren van Azure Mobile Apps voor Xamarin.iOS. Offlinesynchronisatie kan eindgebruikers werken met een mobiele app--weergeven, toevoegen of wijzigen van gegevens--, zelfs wanneer er geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe service.

In deze zelfstudie bijwerken Xamarin.iOS-app-project uit [een Xamarin iOS-app maken] ter ondersteuning van de offline functies van Azure Mobile Apps. Als u het gedownloade quick start-serverproject niet gebruikt, moet u de data access-extensiepakketten toevoegen aan uw project. Zie voor meer informatie over server extensiepakketten [werken met de .NET-back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie het onderwerp voor meer informatie over de functie offlinesynchronisatie [Offline synchroniseren van gegevens in Azure Mobile Apps].

## <a name="update-the-client-app-to-support-offline-features"></a>De clientapp ter ondersteuning van offlinefuncties bijwerken
Azure Mobile App offline functies kunt u communiceren met een lokale database als u zich in een offline-scenario. Als u deze functies in uw app, initialiseren een [SyncContext] naar een lokale opslag. Verwijzen naar de tabel via de interface [IMobileServiceSyncTable]. SQLite wordt gebruikt als het lokale archief van het apparaat.

1. Open de NuGet package manager in het project dat u voltooid in de [een Xamarin iOS-app maken] zelfstudie vervolgens zoeken naar en installeren de **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-pakket.
2. Open het bestand QSTodoService.cs en opmerkingen bij de `#define OFFLINE_SYNC_ENABLED` definitie.
3. Bouwen en uitvoeren van de client-app. De app werkt op dezelfde manier als toen u offlinesynchronisatie ingeschakeld. De lokale database is echter nu gevuld met gegevens die kunnen worden gebruikt in een offline-scenario.

## <a name="update-sync"></a>Update van de app verbreken van de back-end
In deze sectie kunt u de verbinding verbreekt met uw mobiele App back-end om te simuleren een offline situatie. Wanneer u gegevens toevoegt, ziet uitzonderings-handler u dat de app in de offlinemodus is. In deze status nieuwe items toegevoegd in het lokale archief en de back-end van mobiele app worden gesynchroniseerd wanneer push wordt uitgevoerd in een verbonden status.

1. QSToDoService.cs bewerken in de gedeelde-project. Wijzig de **applicationURL** om te verwijzen naar een ongeldige URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    U kunt ook offline gedrag met Wi-Fi- en mobiele netwerken op het apparaat uit te schakelen of met vliegtuigmodus aantonen.
2. Ontwikkel en voer de app. U ziet de synchronisatie is mislukt bij vernieuwen wanneer de app gestart.
3. Nieuwe items en Let op dat push is mislukt met de status [CancelledByNetworkError] telkens wanneer u klikt **opslaan**. De nieuwe todo-items bestaan echter in het lokale archief totdat ze naar de back-end voor de mobiele app kunnen worden geactiveerd.  In een productie-app als u deze uitzonderingen onderdrukken de client-app gedraagt zich alsof deze nog steeds verbonden met de back-end voor de mobiele app.
4. De toepassing sluiten en opnieuw om te controleren dat de nieuwe items die u hebt gemaakt met het lokale archief worden doorgevoerd.
5. (Optioneel) Als u Visual Studio geïnstalleerd op een PC is, open **Server Explorer**. Navigeer naar de database in **Azure**-> **SQL-Databases**. Met de rechtermuisknop op de database en selecteer **openen in SQL Server Object Explorer**. Nu kunt u bladeren naar de tabel van uw SQL-database en de inhoud ervan. Controleer of dat de gegevens in de back-end-database niet is gewijzigd.
6. (Optioneel) Gebruik een REST-hulpprogramma zoals Fiddler of Postman query uitvoeren op uw mobiele back-end, met een GET-query in de vorm `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Update de app opnieuw verbinding maken met uw back-end voor de mobiele App
In deze sectie opnieuw verbinding worden de back-end voor de mobiele app met de app. Hiermee wordt de app van de status offline heeft verplaatst naar een online status met de back-end voor de mobiele app gesimuleerd.   Als u de breuken netwerk gesimuleerd door het uitschakelen van verbinding met het netwerk, worden er geen codewijzigingen nodig.
Schakel in het netwerk opnieuw.  Wanneer u de toepassing voor het eerst uitvoert de `RefreshDataAsync` methode wordt aangeroepen. Dit wordt opgeroepen `SyncAsync` synchroniseren van het lokale archief met de back-end-database.

1. QSToDoService.cs in het gedeelde project openen en herstellen van de wijziging van de **applicationURL** eigenschap.
2. Bouwen en uitvoeren van de app. De app uw lokale wijzigingen gesynchroniseerd met de mobiele Apps van Azure back-end push als pull-bewerkingen wanneer de `OnRefreshItemsSelected` methode wordt uitgevoerd.
3. (Optioneel) De bijgewerkte gegevens met behulp van SQL Server Object Explorer of een REST-hulpprogramma zoals Fiddler weergeven. U ziet de gegevens is gesynchroniseerd tussen de database van de back-end voor mobiele Apps van Azure en het lokale archief.
4. Klik in de app op het selectievakje naast enkele items uit te voeren in het lokale archief.

   `CompleteItemAsync`aanroepen `SyncAsync` op synchronisatie elke voltooid object met de back-end voor de mobiele App. `SyncAsync`Zowel push als pull-aanroepen.
   **Wanneer u een pull op basis van een tabel die de client heeft wijzigingen aangebracht in uitvoert, een van de context van de synchronisatie client-push wordt altijd eerst uitgevoerd automatisch**. De impliciete push zorgt ervoor dat alle tabellen in het lokale archief samen met relaties consistent blijven. Zie voor meer informatie over dit gedrag [Offline synchroniseren van gegevens in Azure Mobile Apps].

## <a name="review-the-client-sync-code"></a>Bekijk de clientcode voor synchronisatie
De Xamarin-client-project dat u hebt gedownload wanneer u de zelfstudie voltooid [een Xamarin iOS-app maken] bevat al de code die offline synchronisatie met een lokale SQLite-database ondersteunt. Hier volgt een kort overzicht van wat al in de zelfstudie code opgenomen is. Zie voor een conceptueel overzicht van de functie [Offline synchroniseren van gegevens in Azure Mobile Apps].

* Voordat u tabelbewerkingen kunnen uitvoeren, moet het lokale archief worden geïnitialiseerd. De database van het lokale archief is geïnitialiseerd als `QSTodoListViewController.ViewDidLoad()` voert `QSTodoService.InitializeStoreAsync()`. Deze methode maakt u een nieuwe lokale SQLite database via de `MobileServiceSQLiteStore` klasse zoals opgegeven door de client voor mobiele Apps van Azure SDK.

    De `DefineTable` methode maakt een tabel in het lokale archief die overeenkomt met de velden in het opgegeven type `ToDoItem` in dit geval. Het type heeft geen om op te nemen van de kolommen die zich in de externe database. Het is mogelijk voor het opslaan van een subset kolommen.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }
* De `todoTable` lid is van `QSTodoService` is van de `IMobileServiceSyncTable` typt u in plaats van `IMobileServiceTable`. De IMobileServiceSyncTable zorgt ervoor dat alle maken, lezen, bijwerken en verwijderen (CRUD) tabel-bewerkingen op de lokale database.

    U besluit wanneer deze wijzigingen naar de back-end voor mobiele Apps van Azure worden gepusht door het aanroepen van `IMobileServiceSyncContext.PushAsync()`. De synchronisatie-context helpt tabelrelaties behouden door wijzigingen in alle tabellen die een client-app is gewijzigd wanneer worden gepusht en bij te houden `PushAsync` wordt aangeroepen.

    De opgegeven code aanroepen `QSTodoService.SyncAsync()` om te synchroniseren wanneer de lijst met taken wordt vernieuwd of een todoitem is toegevoegd of voltooid. De app wordt gesynchroniseerd na elke wijziging van de lokale. Als een pull wordt uitgevoerd op basis van een tabel die nog in behandeling zijnde lokale updates bijgehouden door de context, wordt die pullbewerking automatisch geactiveerd een context push eerst.

    In de opgegeven code alle records in de externe `TodoItem` tabel worden opgevraagd, maar het is ook mogelijk records filteren op het doorgeven van een query-id en opvragen voor `PushAsync`. Zie voor meer informatie de sectie *incrementele synchronisatie* in [Offline synchroniseren van gegevens in Azure Mobile Apps].

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }

## <a name="additional-resources"></a>Aanvullende resources
* [Offline synchroniseren van gegevens in Azure Mobile Apps]
* [Azure Mobile Apps .NET SDK procedure][8]

<!-- Images -->

<!-- URLs. -->
[een Xamarin iOS-app maken]: app-service-mobile-xamarin-ios-get-started.md
[Offline synchroniseren van gegevens in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
