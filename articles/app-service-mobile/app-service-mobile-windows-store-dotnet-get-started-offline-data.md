---
title: Offlinesynchronisatie inschakelen voor uw app Universal Windows Platform (UWP) met Mobile Apps | Microsoft Docs
description: Informatie over het gebruik van een Azure Mobile App naar de cache en synchroniseren offlinesynchronisatie van gegevens in uw app Universal Windows Platform (UWP).
documentationcenter: windows
author: conceptdev
manager: crdun
editor: ''
services: app-service\mobile
ms.assetid: 8fe51773-90de-4014-8a38-41544446d9b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 69ee9e7101a2b7337e1e42ff5ae09954fbfd50b2
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52994925"
---
# <a name="enable-offline-sync-for-your-windows-app"></a>Offlinesynchronisatie voor uw Windows-app inschakelen
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie leert u hoe u Offlineondersteuning toevoegt aan een Universal Windows Platform (UWP)-app met behulp van een back-end van mobiele Apps van Azure. Offlinesynchronisatie kunnen eindgebruikers om te communiceren met een mobiele app, weergeven, toevoegen of wijzigen van gegevens -, zelfs wanneer er geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe back-end.

In deze zelfstudie hebt u de UWP-app-project uit de zelfstudie bijwerken [maken van een Windows-app] ter ondersteuning van de offline functies van Azure Mobile Apps. Als u het gedownloade quick start-serverproject niet gebruikt, moet u de data access-extensiepakketten toevoegen aan uw project. Zie voor meer informatie over het server-extensiepakketten [werken met de .NET back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie het onderwerp voor meer informatie over de functie voor offlinesynchronisatie, [Offlinesynchronisatie van gegevens in Azure Mobile Apps].

## <a name="requirements"></a>Vereisten  
In deze zelfstudie vereist de volgende vereisten:

* Visual Studio 2013 wordt uitgevoerd op Windows 8.1 of hoger.
* Voltooiing van [maken van een Windows-app][maken van een windows-app].
* [Azure Mobile Services SQLite Store][sqlite store nuget]
* [SQLite voor Universal Windows Platform-ontwikkeling](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform) 

## <a name="update-the-client-app-to-support-offline-features"></a>Update de client-app voor de ondersteuning van offline functies
Offlinefuncties van Azure Mobile App kunnen u met een lokale database werken, wanneer u zich in een offline-scenario. Voor het gebruik van deze functies in uw app, die u initialiseert een [SyncContext] [ synccontext] naar een lokale opslag. Vervolgens verwijzen naar de tabel via de [IMobileServiceSyncTable][IMobileServiceSyncTable] interface. SQLite wordt gebruikt als het lokale archief van het apparaat.

1. Installeer de [SQLite runtime voor het Universal Windows Platform](https://sqlite.org/2016/sqlite-uwp-3120200.vsix).
2. Open in Visual Studio de NuGet package manager voor de UWP-app-project dat u voltooid in de [maken van een Windows-app] zelfstudie.
    Zoek en installeer de **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-pakket.
3. Klik in Solution Explorer met de rechtermuisknop op **verwijzingen** > **verwijzing toevoegen...** > **Universal Windows** > **extensies**, schakelt u beide **SQLite voor Universal Windows Platform** en **Visual C++ 2015-Runtime voor Universal Windows Platform-apps**.

    ![SQLite UWP verwijzing toevoegen][1]
4. Open het bestand MainPage.xaml.cs en verwijder de opmerking bij de `#define OFFLINE_SYNC_ENABLED` definitie.
5. Druk in Visual Studio, op de **F5** sleutel om te bouwen en uitvoeren van de client-app. De app werkt op dezelfde manier als toen u offline synchronisatie ingeschakeld. De lokale database wordt echter nu gevuld met gegevens die kunnen worden gebruikt in een offline-scenario.

## <a name="update-sync"></a>Het verbreken van de back-end-app bijwerken
In deze sectie maakt verbreekt u de verbinding met uw mobiele App back-end voor het simuleren van een offline situatie. Wanneer u gegevensitems toevoegt, uw uitzonderingshandler geeft aan dat de app in de offlinemodus bevindt is. In deze status kunnen nieuwe items toegevoegd in het lokale archief en de back-end van de mobiele app worden gesynchroniseerd wanneer push wordt vervolgens uitgevoerd in een verbonden status heeft.

1. App.xaml.cs bewerken in de gedeelde-project. Een opmerking bij de initialisatie van de **MobileServiceClient** en voeg de volgende regel, dat gebruikmaakt van een ongeldige mobiele app-URL:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    U kunt ook offline gedrag demonstreren door Wi-Fi- en mobiele netwerken op het apparaat uit te schakelen of vliegtuigmodus.
2. Druk op **F5** wilt bouwen en uitvoeren van de app. U ziet de synchronisatie is mislukt voor vernieuwen wanneer de app gestart.
3. Voer nieuwe artikelen en u ziet dat push is mislukt met een [CancelledByNetworkError] telkens wanneer u klikt op de status **opslaan**. De nieuwe items bestaan echter in het lokale archief totdat ze naar de back-end van mobiele Apps kunnen worden gepusht.  In een productie-app, als u deze uitzonderingen onderdrukken de client-app gedraagt zich alsof het nog steeds verbonden met de back-end van de mobiele app.
4. De app sluit en opnieuw starten om te controleren dat de nieuwe items die u hebt gemaakt naar het lokale archief worden opgeslagen.
5. (Optioneel) Open in Visual Studio, **Server Explorer**. Navigeer naar uw database in **Azure**->**SQL-Databases**. Met de rechtermuisknop op uw database en selecteer **openen in SQL Server-Objectverkenner**. Nu kunt u bladeren naar de tabel van uw SQL-database en de inhoud ervan. Controleer of dat de gegevens in de back enddatabase niet is gewijzigd.
6. (Optioneel) Een REST-hulpprogramma zoals Fiddler of Postman gebruiken om te vragen van uw mobiele back-end, met een GET-query in het formulier `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Update de app opnieuw verbinding maken met de back-end van uw mobiele App
In deze sectie maakt u opnieuw verbinding maakt met de app de back-end van de mobiele app. Deze wijzigingen simuleren opnieuw verbinden met een netwerk op de app.

Wanneer u de toepassing, voor het eerst uitvoert de `OnNavigatedTo` aanroepen gebeurtenis-handler `InitLocalStoreAsync`. Deze methode aanroepen op zijn beurt `SyncAsync` om te synchroniseren van het lokale archief met de back-enddatabase. De app probeert te synchroniseren bij het opstarten.

1. App.xaml.cs in het gedeelde project openen en verwijder de opmerkingen in de vorige initialisatie van `MobileServiceClient` gebruiken de juiste URL van de mobiele app.
2. Druk op de **F5** sleutel om te bouwen en uitvoeren van de app. De app worden gesynchroniseerd met uw lokale wijzigingen met de back-end van mobiele Apps van Azure met behulp van push- en pull-bewerkingen als het `OnNavigatedTo` gebeurtenis-handler wordt uitgevoerd.
3. (Optioneel) De bijgewerkte gegevens met behulp van SQL Server Object Explorer of een REST-hulpprogramma zoals Fiddler bekijken. U ziet dat de gegevens is gesynchroniseerd tussen de back-end-database van Azure Mobile App en het lokale archief.
4. Klik in de app op het selectievakje in naast een paar items uit te voeren in het lokale archief.

   `UpdateCheckedTodoItem` aanroepen `SyncAsync` synchroniseren elke voltooid item met de mobiele App back-end. `SyncAsync` Zowel push als pull-aanroepen. Echter, **wanneer u een pull op basis van een tabel die de client is gewijzigd om te worden uitgevoerd, een push altijd automatisch wordt uitgevoerd**. Dit gedrag zorgt ervoor dat alle tabellen in het lokale archief samen met relaties consistent blijven. Dit gedrag kan leiden tot een onverwachte push.  Zie voor meer informatie over dit gedrag [Offlinesynchronisatie van gegevens in Azure Mobile Apps].

## <a name="api-summary"></a>API-overzicht
Ter ondersteuning van de offline functies van mobiele services, die we hebben gebruikt de [IMobileServiceSyncTable] interface en geïnitialiseerd [MobileServiceClient.SyncContext] [ synccontext] met een lokale SQLite-database. Wanneer u offline bent, wordt de normale CRUD-bewerkingen voor mobiele Apps werken alsof de app nog steeds is verbonden terwijl de bewerkingen op basis van het lokale archief plaatsvinden. De volgende methoden worden gebruikt voor het synchroniseren van het lokale archief met de server:

* **[PushAsync]**  omdat deze methode lid van is [IMobileServicesSyncContext], wijzigingen in alle tabellen worden gepusht naar de back-end. Alleen records met lokale wijzigingen worden verzonden naar de server.
* **[PullAsync]**  een pull wordt gestart vanuit een [IMobileServiceSyncTable]. Wanneer er wijzigingen in de tabel, is een impliciete push wordt uitgevoerd om ervoor te zorgen dat alle tabellen in het lokale archief samen met relaties consistent blijven. De *pushOtherTables* parameter besturingselementen in een impliciete push of andere tabellen in de context worden gepusht. De *query* parameter heeft een [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] of OData-query-tekenreeks voor het filteren van de geretourneerde gegevens. De *queryId* parameter wordt gebruikt voor het definiëren van incrementele synchronisatie. Zie voor meer informatie, [Offline gegevenssynchronisatie in Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).
* **[PurgeAsync]**  moet deze methode voor het opschonen van verouderde gegevens uit het lokale archief regelmatig in uw app aanroepen. Gebruik de *forceren* parameter als u wilt leegmaken van alle wijzigingen die nog niet zijn gesynchroniseerd.

Zie voor meer informatie over deze concepten [Offline gegevenssynchronisatie in Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Meer informatie
De volgende onderwerpen bevatten aanvullende achtergrondinformatie over de functie voor offlinesynchronisatie van Mobile Apps:

* [Offlinesynchronisatie van gegevens in Azure Mobile Apps]
* [Azure Mobile Apps .NET SDK HOWTO][8]

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
[Offlinesynchronisatie van gegevens in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[maken van een Windows-app]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: https://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: https://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: https://go.microsoft.com/fwlink/?LinkID=716921
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
