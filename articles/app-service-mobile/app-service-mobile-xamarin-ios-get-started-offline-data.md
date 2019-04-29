---
title: Offlinesynchronisatie inschakelen voor uw Azure Mobile App (Xamarin iOS)
description: Informatie over het gebruik van App Service Mobile App naar de cache en synchroniseren offlinesynchronisatie van gegevens in uw Xamarin iOS-toepassing
documentationcenter: xamarin
author: conceptdev
manager: cfowler
editor: ''
services: app-service\mobile
ms.assetid: 828a287c-5d58-4540-9527-1309ebb0f32b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 6a43ed0a50082cc37587752631c707bf9b5059ab
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097445"
---
# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Offlinesynchronisatie inschakelen voor uw mobiele Xamarin.iOS-app
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie bevat de functie voor offlinesynchronisatie van Azure Mobile Apps voor Xamarin.iOS. Offlinesynchronisatie kunnen eindgebruikers om te communiceren met een mobiele app, weergeven, toevoegen of wijzigen van gegevens, zelfs als er geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe service.

In deze zelfstudie, werken de Xamarin.iOS-app-project van [Een Xamarin iOS-app maken] ter ondersteuning van de offline functies van Azure Mobile Apps. Als u het gedownloade quick start-serverproject niet gebruikt, moet u de data access-extensiepakketten toevoegen aan uw project. Zie voor meer informatie over het server-extensiepakketten [werken met de .NET back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie het onderwerp voor meer informatie over de functie voor offlinesynchronisatie, [Offlinesynchronisatie van gegevens in Azure Mobile Apps].

## <a name="update-the-client-app-to-support-offline-features"></a>Update de client-app voor de ondersteuning van offline functies
Offlinefuncties van Azure Mobile App kunnen u met een lokale database werken, wanneer u zich in een offline-scenario. Initialiseren voor het gebruik van deze functies in uw app, een [SyncContext] naar een lokale opslag. Verwijzen naar de tabel via de interface [IMobileServiceSyncTable]. SQLite wordt gebruikt als het lokale archief van het apparaat.

1. Open de NuGet package manager in het project dat u voltooid in de [Een Xamarin iOS-app maken] zelfstudie, en vervolgens zoeken naar en installeren de **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-pakket.
2. Open het bestand QSTodoService.cs en verwijder de opmerking bij de `#define OFFLINE_SYNC_ENABLED` definitie.
3. Opnieuw opbouwen en uitvoeren van de client-app. De app werkt op dezelfde manier als toen u offline synchronisatie ingeschakeld. De lokale database wordt echter nu gevuld met gegevens die kunnen worden gebruikt in een offline-scenario.

## <a name="update-sync"></a>Het verbreken van de back-end-app bijwerken
In deze sectie maakt verbreekt u de verbinding met uw mobiele App back-end voor het simuleren van een offline situatie. Wanneer u gegevensitems toevoegt, uw uitzonderingshandler geeft aan dat de app in de offlinemodus bevindt is. In deze status kunnen nieuwe items toegevoegd in het lokale archief en de back-end van de mobiele app worden gesynchroniseerd wanneer push wordt vervolgens uitgevoerd in een verbonden status heeft.

1. QSToDoService.cs bewerken in de gedeelde-project. Wijzig de **applicationURL** om te verwijzen naar een ongeldige URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    U kunt ook offline gedrag door Wi-Fi- en mobiele netwerken op het apparaat uit te schakelen of met behulp van vliegtuigmodus aantonen.
2. Bouw de app en voer deze uit. U ziet de synchronisatie is mislukt voor vernieuwen wanneer de app gestart.
3. Voer nieuwe artikelen en u ziet dat push is mislukt met de status [CancelledByNetworkError] telkens wanneer u klikt op **opslaan**. De nieuwe items bestaan echter in het lokale archief totdat ze naar de back-end van mobiele Apps kunnen worden gepusht.  In een productie-app, als u deze uitzonderingen onderdrukken de client-app gedraagt zich alsof het nog steeds verbonden met de back-end van de mobiele app.
4. De app sluit en opnieuw starten om te controleren dat de nieuwe items die u hebt gemaakt naar het lokale archief worden opgeslagen.
5. (Optioneel) Als u Visual Studio is geïnstalleerd op een PC hebt, opent u **Server Explorer**. Navigeer naar uw database in **Azure**-> **SQL-Databases**. Met de rechtermuisknop op uw database en selecteer **openen in SQL Server-Objectverkenner**. Nu kunt u bladeren naar de tabel van uw SQL-database en de inhoud ervan. Controleer of dat de gegevens in de back enddatabase niet is gewijzigd.
6. (Optioneel) Een REST-hulpprogramma zoals Fiddler of Postman gebruiken om te vragen van uw mobiele back-end, met een GET-query in het formulier `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Update de app opnieuw verbinding maken met de back-end van uw mobiele App
In deze sectie maakt opnieuw verbinding maken met de app naar de mobiele app back-end. Dit simuleert de verplaatsing van een offline status naar een online status met de mobiele app back-end-app.   Als u de breuk netwerk gesimuleerd door het uitschakelen van verbinding met het netwerk, zijn zonder codewijzigingen nodig.
Schakel in het netwerk opnieuw.  Wanneer u de toepassing, voor het eerst uitvoert de `RefreshDataAsync` methode wordt aangeroepen. Hiermee op zijn beurt wordt `SyncAsync` om te synchroniseren van het lokale archief met de back-enddatabase.

1. QSToDoService.cs in het gedeelde project openen en herstellen van de wijziging van de **applicationURL** eigenschap.
2. Opnieuw opbouwen en uitvoeren van de app. De app worden gesynchroniseerd met uw lokale wijzigingen met de back-end van mobiele Apps van Azure met behulp van push- en pull-bewerkingen wanneer de `OnRefreshItemsSelected` methode wordt uitgevoerd.
3. (Optioneel) De bijgewerkte gegevens met behulp van SQL Server Object Explorer of een REST-hulpprogramma zoals Fiddler bekijken. U ziet dat de gegevens is gesynchroniseerd tussen de back-end-database van Azure Mobile App en het lokale archief.
4. Klik in de app op het selectievakje in naast een paar items uit te voeren in het lokale archief.

   `CompleteItemAsync` aanroepen `SyncAsync` synchroniseren elke voltooid item met de mobiele App back-end. `SyncAsync` Zowel push als pull-aanroepen.
   **Wanneer u een pull op basis van een tabel die de client is gewijzigd om te worden uitgevoerd, een van de context van de synchronisatie van client-push is altijd het eerst uitgevoerd automatisch**. De impliciete push zorgt ervoor dat alle tabellen in het lokale archief samen met relaties consistent blijven. Zie voor meer informatie over dit gedrag [Offlinesynchronisatie van gegevens in Azure Mobile Apps].

## <a name="review-the-client-sync-code"></a>Bekijk de code van de synchronisatie van client
De Xamarin-client-project dat u hebt gedownload wanneer u de zelfstudie voltooid [Een Xamarin iOS-app maken] bevat al de code die offline synchronisatie met behulp van een lokale SQLite-database ondersteunt. Hier volgt een kort overzicht van wat al in de zelfstudie code opgenomen is. Zie voor een conceptueel overzicht van de functie [Offlinesynchronisatie van gegevens in Azure Mobile Apps].

* Voordat u een tabelbewerkingen kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd. De database van het lokale archief wordt geïnitialiseerd wanneer `QSTodoListViewController.ViewDidLoad()` voert `QSTodoService.InitializeStoreAsync()`. Deze methode maakt u een nieuwe lokale SQLite database via de `MobileServiceSQLiteStore` klasse geleverd door de client-SDK van Azure Mobile App.

    De `DefineTable` methode maakt u een tabel in het lokale archief dat overeenkomt met de velden in het opgegeven type `ToDoItem` in dit geval. Het type beschikt niet over te nemen van de kolommen die zich in de externe database. Het is mogelijk om op te slaan slechts een subset van kolommen.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }
* De `todoTable` lid zijn van `QSTodoService` is van de `IMobileServiceSyncTable` typt u in plaats van `IMobileServiceTable`. De IMobileServiceSyncTable zorgt ervoor dat alle maken, lezen, bijwerken en verwijderen van de table-bewerkingen (CRUD) met het lokale archief-database.

    U besluit wanneer deze wijzigingen worden gepusht naar de back-end van mobiele Apps van Azure door het aanroepen van `IMobileServiceSyncContext.PushAsync()`. De synchronisatie-context helpt u bij het behouden van relaties tussen tabellen bij te houden en pushen van wijzigingen in alle tabellen die een client-app is gewijzigd wanneer `PushAsync` wordt genoemd.

    De opgegeven code roept `QSTodoService.SyncAsync()` om te synchroniseren wanneer de todoitem-lijst wordt vernieuwd of een todoitem wordt toegevoegd aan of voltooid. De app wordt gesynchroniseerd na elke wijziging van de lokale. Als een pull wordt uitgevoerd op basis van een tabel die nog in behandeling zijnde lokale updates bijgehouden door de context, wordt die pullbewerking automatisch geactiveerd een context-push eerst.

    In de opgegeven code, die alle records in de externe `TodoItem` tabel zijn opgevraagd, maar het is ook mogelijk om te filteren van records op te geven van een query-id en de query om toegang te `PushAsync`. Zie voor meer informatie de sectie *incrementele synchronisatie* in [Offlinesynchronisatie van gegevens in Azure Mobile Apps].

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
* [Offlinesynchronisatie van gegevens in Azure Mobile Apps]
* [Azure Mobile Apps .NET SDK HOWTO][8]

<!-- Images -->

<!-- URLs. -->
[Een Xamarin iOS-app maken]: app-service-mobile-xamarin-ios-get-started.md
[Offlinesynchronisatie van gegevens in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
