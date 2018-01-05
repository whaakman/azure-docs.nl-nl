---
title: Offlinesynchronisatie voor uw app Universal Windows Platform (UWP) met Mobile Apps inschakelen | Microsoft Docs
description: Informatie over het gebruik van een Azure Mobile Apps-cache en sync offline gegevens in uw app Universal Windows Platform (UWP).
documentationcenter: windows
author: conceptdev
manager: crdun
editor: 
services: app-service\mobile
ms.assetid: 8fe51773-90de-4014-8a38-41544446d9b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: a16de4cef82c29f9b6becfae1901662ee1936934
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="enable-offline-sync-for-your-windows-app"></a>Offlinesynchronisatie voor uw Windows-app inschakelen
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie laat zien hoe u offline ondersteuning toevoegen aan een Universal Windows Platform (UWP)-app met behulp van een back-end voor mobiele Apps van Azure. Offlinesynchronisatie kan eindgebruikers werken met een mobiele app--weergeven, toevoegen of wijzigen van gegevens -, zelfs wanneer er geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe back-end.

In deze zelfstudie maakt u de UWP-app-project uit de handleiding bijwerken [maken van een Windows-app] ter ondersteuning van de offline functies van Azure Mobile Apps. Als u het gedownloade quick start-serverproject niet gebruikt, moet u de data access-extensiepakketten toevoegen aan uw project. Zie voor meer informatie over server extensiepakketten [werken met de .NET-back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie het onderwerp voor meer informatie over de functie offlinesynchronisatie [Offline synchroniseren van gegevens in Azure Mobile Apps].

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist de volgende vereisten:

* Visual Studio 2013 met Windows 8.1 of hoger.
* Voltooiing van [maken van een Windows-app][een windows-app maken].
* [Azure Mobile Services SQLite Store][sqlite store nuget]
* [SQLite voor de ontwikkeling van universele Windows-Platform](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>De clientapp ter ondersteuning van offlinefuncties bijwerken
Azure Mobile App offline functies kunt u communiceren met een lokale database als u zich in een offline-scenario. Als u deze functies in uw app, die u initialiseren een [SyncContext] [ synccontext] naar een lokale opslag. Vervolgens verwijzen naar de tabel via de [IMobileServiceSyncTable][IMobileServiceSyncTable] interface. SQLite wordt gebruikt als het lokale archief van het apparaat.

1. Installeer de [SQLite-runtime voor het Universal Windows Platform](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).
2. Open in Visual Studio de NuGet package manager voor het UWP-app-project dat u voltooid in de [maken van een Windows-app] zelfstudie.
    Zoek en installeer de **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-pakket.
3. Klik in Solution Explorer met de rechtermuisknop op **verwijzingen** > **verwijzing toevoegen...** >**Universeel Windows** > **extensies**, schakelt u beide **SQLite voor Universal Windows Platform** en **Visual C++-2015-Runtime voor universele Windows-Platform-apps**.

    ![SQLite UWP-verwijzing toevoegen][1]
4. Open het bestand MainPage.xaml.cs en opmerkingen bij de `#define OFFLINE_SYNC_ENABLED` definitie.
5. Druk in Visual Studio op de **F5** sleutel om te bouwen en uitvoeren van de client-app. De app werkt op dezelfde manier als toen u offlinesynchronisatie ingeschakeld. De lokale database is echter nu gevuld met gegevens die kunnen worden gebruikt in een offline-scenario.

## <a name="update-sync"></a>Update van de app verbreken van de back-end
In deze sectie kunt u de verbinding verbreekt met uw mobiele App back-end om te simuleren een offline situatie. Wanneer u gegevens toevoegt, ziet uitzonderings-handler u dat de app in de offlinemodus is. In deze status nieuwe items toegevoegd in het lokale archief en de back-end van mobiele app worden gesynchroniseerd wanneer push wordt uitgevoerd in een verbonden status.

1. App.xaml.cs bewerken in de gedeelde-project. Uitcommentarieer de initialisatie van de **MobileServiceClient** en voeg de volgende regel, dat gebruikmaakt van een ongeldige mobiele app-URL:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    U kunt ook offline gedrag demonstreren door Wi-Fi- en mobiele netwerken op het apparaat uit te schakelen of vliegtuigmodus gebruiken.
2. Druk op **F5** het bouwen en uitvoeren van de app. U ziet de synchronisatie is mislukt bij vernieuwen wanneer de app gestart.
3. Nieuwe items en Let op dat push is mislukt met een [CancelledByNetworkError] status telkens wanneer u klikt **opslaan**. De nieuwe todo-items bestaan echter in het lokale archief totdat ze naar de back-end voor de mobiele app kunnen worden geactiveerd.  In een productie-app als u deze uitzonderingen onderdrukken de client-app gedraagt zich alsof deze nog steeds verbonden met de back-end voor de mobiele app.
4. De toepassing sluiten en opnieuw om te controleren dat de nieuwe items die u hebt gemaakt met het lokale archief worden doorgevoerd.
5. (Optioneel) Open in Visual Studio **Server Explorer**. Navigeer naar de database in **Azure**->**SQL-Databases**. Met de rechtermuisknop op de database en selecteer **openen in SQL Server Object Explorer**. Nu kunt u bladeren naar de tabel van uw SQL-database en de inhoud ervan. Controleer of dat de gegevens in de back-end-database niet is gewijzigd.
6. (Optioneel) Gebruik een REST-hulpprogramma zoals Fiddler of Postman query uitvoeren op uw mobiele back-end, met een GET-query in de vorm `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Update de app opnieuw verbinding maken met uw back-end voor de mobiele App
In deze sectie kunt u de app om de back-end voor de mobiele app te herstellen. Deze wijzigingen simuleren opnieuw verbinden met een netwerk op de app.

Wanneer u de toepassing voor het eerst uitvoert de `OnNavigatedTo` aanroepen gebeurtenis-handler `InitLocalStoreAsync`. Deze methode aanroept op zijn beurt `SyncAsync` synchroniseren van het lokale archief met de back-end-database. De app probeert te synchroniseren bij het opstarten.

1. App.xaml.cs in het gedeelde project openen en verwijder de opmerkingen in de vorige initialisatie van `MobileServiceClient` de juiste de mobiele app-URL gebruiken.
2. Druk op de **F5** sleutel om te bouwen en uitvoeren van de app. De app uw lokale wijzigingen gesynchroniseerd met de mobiele Apps van Azure back-end push als pull-bewerkingen wanneer de `OnNavigatedTo` gebeurtenis-handler wordt uitgevoerd.
3. (Optioneel) De bijgewerkte gegevens met behulp van SQL Server Object Explorer of een REST-hulpprogramma zoals Fiddler weergeven. U ziet de gegevens is gesynchroniseerd tussen de database van de back-end voor mobiele Apps van Azure en het lokale archief.
4. Klik in de app op het selectievakje naast enkele items uit te voeren in het lokale archief.

   `UpdateCheckedTodoItem`aanroepen `SyncAsync` op synchronisatie elke voltooid object met de back-end voor de mobiele App. `SyncAsync`Zowel push als pull-aanroepen. Echter, **wanneer u een pull op basis van een tabel die de client heeft wijzigingen aangebracht in uitvoert, een push altijd automatisch wordt uitgevoerd**. Dit gedrag zorgt ervoor dat alle tabellen in het lokale archief samen met relaties consistent blijven. Dit gedrag kan resulteren in een onverwachte push.  Zie voor meer informatie over dit gedrag [Offline synchroniseren van gegevens in Azure Mobile Apps].

## <a name="api-summary"></a>API-overzicht
Ter ondersteuning van de offline functies van mobiele services, hebben we gebruikt de [IMobileServiceSyncTable] interface en geïnitialiseerd [MobileServiceClient.SyncContext] [ synccontext] met een lokale SQLite-database. Wanneer u offline bent, wordt de normale CRUD-bewerkingen voor mobiele Apps werken alsof de app nog steeds verbinding heeft terwijl de bewerkingen op basis van het lokale archief plaatsvinden. De volgende methoden worden gebruikt voor het lokale archief synchroniseren met de server:

* **[PushAsync]**  omdat deze methode deel uit van maakt [IMobileServicesSyncContext], wijzigingen in alle tabellen worden gepusht naar de back-end. Alleen records met lokale wijzigingen worden verzonden naar de server.
* **[PullAsync]**  een pull wordt gestart vanuit een [IMobileServiceSyncTable]. Wanneer er bijgehouden wijzigingen in de tabel zijn, wordt een impliciete push uitvoeren om ervoor te zorgen dat alle tabellen in het lokale archief samen met relaties consistent blijven. De *pushOtherTables* parameter besturingselementen in een impliciete push of andere tabellen in de context worden gepusht. De *query* parameter heeft een [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] of OData-query-tekenreeks voor het filteren van de geretourneerde gegevens. De *queryId* parameter wordt gebruikt voor het definiëren van incrementele synchronisatie. Zie voor meer informatie [Offline synchroniseren van gegevens in Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).
* **[PurgeAsync]**  uw app moet deze methode als u wilt verwijderen van verouderde gegevens uit het lokale archief periodiek aanroepen. Gebruik de *forceren* parameter als u wilt verwijderen van alle wijzigingen die nog niet zijn gesynchroniseerd.

Zie voor meer informatie over deze concepten [Offline synchroniseren van gegevens in Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Meer informatie
De volgende onderwerpen bevatten aanvullende achtergrondinformatie over de functie offline synchroniseren van Mobile Apps:

* [Offline synchroniseren van gegevens in Azure Mobile Apps]
* [Azure Mobile Apps .NET SDK procedure][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Offline synchroniseren van gegevens in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[een windows-app maken]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
