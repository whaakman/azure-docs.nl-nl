---
title: Offlinesynchronisatie inschakelen voor uw Azure Mobile App (Xamarin Android)
description: Informatie over het gebruik van App Service Mobile App naar de cache en synchroniseren offlinesynchronisatie van gegevens in uw Xamarin Android-toepassing
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
services: app-service\mobile
ms.assetid: 91d59e4b-abaa-41f4-80cf-ee7933b32568
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 33b95f117908fc0d7028a85defd148fdd821c391
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52963603"
---
# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Offlinesynchronisatie inschakelen voor uw mobiele app voor Xamarin.Android

[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie bevat de functie voor offlinesynchronisatie van Azure Mobile Apps voor Xamarin.Android. Offlinesynchronisatie kunnen eindgebruikers om te communiceren met een mobiele app, weergeven, toevoegen of wijzigen van gegevens, zelfs als er geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database.
Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe service.

In deze zelfstudie hebt u het clientproject uit de zelfstudie bijwerken [een Xamarin Android-app maken] ter ondersteuning van de offline functies van Azure Mobile Apps. Als u het gedownloade quick start-serverproject niet gebruikt, moet u de data access-extensiepakketten toevoegen aan uw project. Zie voor meer informatie over het server-extensiepakketten [werken met de .NET back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie het onderwerp voor meer informatie over de functie voor offlinesynchronisatie, [Offlinesynchronisatie van gegevens in Azure Mobile Apps].

## <a name="update-the-client-app-to-support-offline-features"></a>Update de client-app voor de ondersteuning van offline functies

Offlinefuncties van Azure Mobile App kunnen u met een lokale database werken, wanneer u zich in een offline-scenario. Voor het gebruik van deze functies in uw app, die u initialiseert een [SyncContext] naar een lokale opslag. Vervolgens verwijzen naar de tabel via de [IMobileServiceSyncTable](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable?view=azure-dotnet) interface. SQLite wordt gebruikt als het lokale archief van het apparaat.

1. Open in Visual Studio de NuGet-Pakketbeheer in het project dat u in voltooid de [een Xamarin Android-app maken] zelfstudie.  Zoek en installeer de **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-pakket.
2. Open het bestand ToDoActivity.cs en verwijder de opmerking bij de `#define OFFLINE_SYNC_ENABLED` definitie.
3. Druk in Visual Studio, op de **F5** sleutel om te bouwen en uitvoeren van de client-app. De app werkt op dezelfde manier als toen u offline synchronisatie ingeschakeld. De lokale database wordt echter nu gevuld met gegevens die kunnen worden gebruikt in een offline-scenario.

## <a name="update-sync"></a>Het verbreken van de back-end-app bijwerken

In deze sectie maakt verbreekt u de verbinding met uw mobiele App back-end voor het simuleren van een offline situatie. Wanneer u gegevensitems toevoegt, uw uitzonderingshandler geeft aan dat de app in de offlinemodus bevindt is. In deze status kunnen nieuwe items toegevoegd in het lokale archief en worden gesynchroniseerd met de back-end van de mobiele app wanneer een push wordt uitgevoerd in een verbonden status heeft.

1. ToDoActivity.cs bewerken in de gedeelde-project. Wijzig de **applicationURL** om te verwijzen naar een ongeldige URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    U kunt ook offline gedrag door Wi-Fi- en mobiele netwerken op het apparaat uit te schakelen of met behulp van vliegtuigmodus aantonen.
2. Druk op **F5** wilt bouwen en uitvoeren van de app. U ziet de synchronisatie is mislukt voor vernieuwen wanneer de app gestart.
3. Voer nieuwe artikelen en u ziet dat push is mislukt met de status [CancelledByNetworkError] telkens wanneer u klikt op **opslaan**. De nieuwe items bestaan echter in het lokale archief totdat ze naar de back-end van mobiele Apps kunnen worden gepusht.  In een productie-app, als u deze uitzonderingen onderdrukken de client-app gedraagt zich alsof het nog steeds verbonden met de back-end van de mobiele app.
4. De app sluit en opnieuw starten om te controleren dat de nieuwe items die u hebt gemaakt naar het lokale archief worden opgeslagen.
5. (Optioneel) Open in Visual Studio, **Server Explorer**. Navigeer naar uw database in **Azure**->**SQL-Databases**. Met de rechtermuisknop op uw database en selecteer **openen in SQL Server-Objectverkenner**. Nu kunt u bladeren naar de tabel van uw SQL-database en de inhoud ervan. Controleer of dat de gegevens in de back enddatabase niet is gewijzigd.
6. (Optioneel) Een REST-hulpprogramma zoals Fiddler of Postman gebruiken om te vragen van uw mobiele back-end, met een GET-query in het formulier `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Update de app opnieuw verbinding maken met de back-end van uw mobiele App

In deze sectie maakt opnieuw verbinding maken met de app naar de mobiele app back-end. Wanneer u de toepassing, voor het eerst uitvoert de `OnCreate` aanroepen gebeurtenis-handler `OnRefreshItemsSelected`. Deze methode roept `SyncAsync` om te synchroniseren van het lokale archief met de back-enddatabase.

1. ToDoActivity.cs in het gedeelde project openen en herstellen van de wijziging van de **applicationURL** eigenschap.
2. Druk op de **F5** sleutel om te bouwen en uitvoeren van de app. De app worden gesynchroniseerd met uw lokale wijzigingen met de back-end van mobiele Apps van Azure met behulp van push- en pull-bewerkingen wanneer de `OnRefreshItemsSelected` methode wordt uitgevoerd.
3. (Optioneel) De bijgewerkte gegevens met behulp van SQL Server Object Explorer of een REST-hulpprogramma zoals Fiddler bekijken. U ziet dat de gegevens is gesynchroniseerd tussen de back-end-database van Azure Mobile App en het lokale archief.
4. Klik in de app op het selectievakje in naast een paar items uit te voeren in het lokale archief.

   `CheckItem` aanroepen `SyncAsync` synchroniseren elke voltooid item met de mobiele App back-end. `SyncAsync` Zowel push als pull-aanroepen. **Wanneer u een pull op basis van een tabel die de client is gewijzigd om te worden uitgevoerd, een push altijd automatisch wordt uitgevoerd**. Dit zorgt ervoor dat alle tabellen in het lokale archief samen met relaties consistent blijven. Dit gedrag kan leiden tot een onverwachte push. Zie voor meer informatie over dit gedrag [Offlinesynchronisatie van gegevens in Azure Mobile Apps].

## <a name="review-the-client-sync-code"></a>Bekijk de code van de synchronisatie van client

De Xamarin-client-project dat u hebt gedownload wanneer u de zelfstudie voltooid [een Xamarin Android-app maken] bevat al de code die offline synchronisatie met behulp van een lokale SQLite-database ondersteunt. Hier volgt een kort overzicht van wat al in de zelfstudie code opgenomen is. Zie voor een conceptueel overzicht van de functie [Offlinesynchronisatie van gegevens in Azure Mobile Apps].

* Voordat u een tabelbewerkingen kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd. De database van het lokale archief wordt geïnitialiseerd wanneer `ToDoActivity.OnCreate()` voert `ToDoActivity.InitLocalStoreAsync()`. Deze methode maakt u een lokale SQLite database met de `MobileServiceSQLiteStore` klasse geleverd door de client-SDK van Azure Mobile Apps.

    De `DefineTable` methode maakt u een tabel in het lokale archief dat overeenkomt met de velden in het opgegeven type `ToDoItem` in dit geval. Het type beschikt niet over te nemen van de kolommen die zich in de externe database. Het is mogelijk om op te slaan slechts een subset van kolommen.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync.
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }
* De `toDoTable` lid zijn van `ToDoActivity` is van de `IMobileServiceSyncTable` typt u in plaats van `IMobileServiceTable`. De IMobileServiceSyncTable zorgt ervoor dat alle maken, lezen, bijwerken en verwijderen van de table-bewerkingen (CRUD) met het lokale archief-database.

    U besluit wanneer wijzigingen worden gepusht naar de back-end van mobiele Apps van Azure door het aanroepen van `IMobileServiceSyncContext.PushAsync()`. De synchronisatie-context helpt u bij het behouden van relaties tussen tabellen bij te houden en pushen van wijzigingen in alle tabellen die een client-app is gewijzigd wanneer `PushAsync` wordt genoemd.

    De opgegeven code roept `ToDoActivity.SyncAsync()` om te synchroniseren wanneer de todoitem-lijst wordt vernieuwd of een todoitem wordt toegevoegd aan of voltooid. De code wordt gesynchroniseerd na elke wijziging van de lokale.

    In de opgegeven code, die alle records in de externe `TodoItem` tabel zijn opgevraagd, maar het is ook mogelijk om te filteren van records op te geven van een query-id en de query om toegang te `PushAsync`. Zie voor meer informatie de sectie *incrementele synchronisatie* in [Offlinesynchronisatie van gegevens in Azure Mobile Apps].

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>Aanvullende resources

* [Offlinesynchronisatie van gegevens in Azure Mobile Apps]
* [Azure Mobile Apps .NET SDK HOWTO][8]

<!-- URLs. -->
[Een Xamarin Android-app maken]: ./app-service-mobile-xamarin-android-get-started.md
[Offlinesynchronisatie van gegevens in Azure Mobile Apps]: ./app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Een Xamarin Android-app maken]: app-service-mobile-xamarin-android-get-started.md
[Offlinesynchronisatie van gegevens in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
