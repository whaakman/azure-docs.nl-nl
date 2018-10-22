---
title: Offline synchroniseren van gegevens in Azure Mobile Apps | Microsoft Docs
description: Overzicht van de functie voor offlinesynchronisatie van gegevens voor Azure Mobile Apps en voor conceptuele verwijzing in
documentationcenter: windows
author: conceptdev
manager: crdun
editor: ''
services: app-service\mobile
ms.assetid: 982fb683-8884-40da-96e6-77eeca2500e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: ab8fb4a567e4c4a7bf1e884999a4e403a98547a0
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471030"
---
# <a name="offline-data-sync-in-azure-mobile-apps"></a>Offlinesynchronisatie van gegevens in Azure Mobile Apps
## <a name="what-is-offline-data-sync"></a>Wat is offline synchroniseren van gegevens?
Offline synchroniseren van gegevens is een client en server SDK-functie van Azure Mobile Apps waarmee u eenvoudig voor ontwikkelaars voor het maken van apps die functioneel zonder een netwerkverbinding zijn.

Wanneer uw app in de offlinemodus bevindt, kunt u nog steeds maken en wijzigen van gegevens die zijn opgeslagen in een lokale opslag. Wanneer de app weer online is, kunnen lokale wijzigingen worden gesynchroniseerd met uw mobiele Apps van Azure back-end. De functie biedt ook ondersteuning voor het detecteren van conflicten wanneer dezelfde record wordt gewijzigd op de client en de back-end. Conflicten kunnen vervolgens worden verwerkt op de server of de client.

Offline synchronisatie heeft verschillende voordelen:

* Verbeter de responsiviteit door servergegevens lokaal op het apparaat opslaan in cache
* Maak robuuste apps die bruikbaar blijven wanneer er netwerkproblemen zijn
* Eindgebruikers kunnen gegevens maken en wijzigen, zelfs wanneer er geen toegang tot het netwerk, ondersteuning van scenario's met weinig of geen verbinding toestaan
* Gegevens synchroniseren tussen meerdere apparaten en conflicten worden gedetecteerd wanneer dezelfde record wordt gewijzigd door twee apparaten
* Netwerkgebruik in netwerken met hoge latentie of met een datalimiet beperken

De volgende zelfstudies laten zien hoe voeg offlinesynchronisatie toe aan uw mobiele clients met behulp van Azure Mobile Apps:

* [Android: Offline synchroniseren inschakelen]
* [Apache Cordova: Offline synchroniseren inschakelen](app-service-mobile-cordova-get-started-offline-data.md)
* [iOS: offline synchroniseren inschakelen]
* [Xamarin iOS: offline synchroniseren inschakelen]
* [Xamarin Android: Offline synchroniseren inschakelen]
* [Xamarin.Forms: Enable-offlinesynchronisatie](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Universal Windows Platform: Offline synchroniseren inschakelen]

## <a name="what-is-a-sync-table"></a>Wat is een synchronisatietabel?
Voor toegang tot het eindpunt '/ tabellen', geef de Azure Mobile-client-SDK's interfaces, zoals `IMobileServiceTable` (.NET client SDK) of `MSTable` (iOS-client). Deze API's rechtstreeks verbinding maken met de mobiele Apps van Azure back-end en mislukken als de clientapparaat beschikt niet over een netwerkverbinding.

Ter ondersteuning van offline gebruiken, moet uw app in plaats daarvan gebruiken de *synchronisatietabel* API's, zoals `IMobileServiceSyncTable` (.NET client SDK) of `MSSyncTable` (iOS-client). Alle de dezelfde CRUD-bewerkingen (maken, lezen, Update, Delete) worden toegepast op synchronisatie tabel-API's, met uitzondering van nu ze lezen uit of schrijven naar een *lokale archief*. Voordat elke tabel synchronisatiebewerkingen kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd.

## <a name="what-is-a-local-store"></a>Wat is een lokale store?
Een lokale opslag is de laag voor gegevenspersistentie op het clientapparaat. De client-SDK's voor Azure Mobile Apps biedt een standaardimplementatie van het lokale archief. Op Windows, Xamarin en Android, is gebaseerd op SQLite. Op iOS-, is deze gebaseerd op de belangrijkste gegevens.

Voor het gebruik van de SQLite-gebaseerde implementatie op Windows Phone of Microsoft Store, moet u een SQLite-extensie installeren. Zie voor meer informatie, [Universal Windows Platform: Offline synchroniseren inschakelen]. Android en iOS verzenden met een versie van SQLite in het besturingssysteem van het apparaat zelf en dus is het niet nodig om te verwijzen naar uw eigen versie van SQLite.

Ontwikkelaars kunnen ook hun eigen lokale archief implementeren. Als u opslaan van gegevens in een versleutelde indeling op de mobiele client wilt, kunt u bijvoorbeeld een lokale opslag die gebruikmaakt van SQLCipher voor versleuteling definiëren.

## <a name="what-is-a-sync-context"></a>Wat is er een context synchroniseren?
Een *synchronisatie context* is gekoppeld aan een mobiele client-object (zoals `IMobileServiceClient` of `MSClient`) en bijgehouden wijzigingen die zijn aangebracht met sync-tabellen. De synchronisatie-context onderhoudt een *bewerking wachtrij*, later naar de server die houdt een geordende lijst met CUD bewerkingen (Create, Update, Delete) die is verzonden.

Een lokale opslag is gekoppeld aan de context van de synchronisatie met behulp van een initialisatiemethode zoals `IMobileServicesSyncContext.InitializeAsync(localstore)` in de [SDK voor .NET-client].

## <a name="how-sync-works"></a>Hoe offline synchronisatie werkt
Wanneer u de synchronisatie-tabellen, bepaalt de clientcode wanneer lokale wijzigingen worden gesynchroniseerd met de back-end van een Azure Mobile Apps. Niets verzonden naar de back-end voordat er een aanroep van wordt *push* lokale wijzigingen. Op dezelfde manier het lokale archief wordt gevuld met nieuwe gegevens alleen wanneer er een aanroep van wordt *pull* gegevens.

* **Push**: Push is een bewerking in de context van de synchronisatie en alle CUD wijzigingen sinds de laatste push verzendt. Houd er rekening mee dat het is niet mogelijk voor het verzenden van alleen de wijzigingen van een afzonderlijke tabel, omdat anders bewerkingen kunnen worden verzonden niet de juiste volgorde. Push voert een reeks van REST-aanroepen naar uw mobiele Apps van Azure back-end, die op zijn beurt Hiermee wijzigt u de server-database.
* **Pull-**: Pull-abonnementen op basis van per tabel is uitgevoerd en kunnen worden aangepast met een query voor het ophalen van slechts een subset van de server-gegevens. De client-SDK's voor Azure Mobile wordt vervolgens de resulterende gegevens invoegen in het lokale archief.
* **Impliciete Pushes**: als een pull op basis van een tabel met lokale updates die nog niet wordt uitgevoerd, voert de pull eerst een `push()` in de context van de synchronisatie. Deze push helpt bij het minimaliseren van conflicten tussen de wijzigingen die al in de wachtrij staan en nieuwe gegevens van de server.
* **Incrementele synchronisatie**: de eerste parameter voor de pullbewerking wordt een *querynaam* die alleen op de client wordt gebruikt. Als u de naam van een niet-null-query gebruikt, de Azure Mobile SDK wordt uitgevoerd een *incrementele synchronisatie*. Telkens wanneer een pullbewerking retourneert een set met resultaten, de meest recente `updatedAt` timestamp van die resultatenset wordt opgeslagen in de lokale SDK-systeemtabellen. Volgende pull-bewerkingen worden alleen records ophalen na deze timestamp.

  Voor het gebruik van incrementele synchronisatie, de server moet retourneren zinvolle `updatedAt` waarden en moet ook ondersteuning voor sorteren op dit veld. Echter, omdat de SDK een eigen sorteren in het veld updatedAt voegt, niet kan u een pull-query een eigen heeft `orderBy` component.

  De querynaam mag een willekeurige tekenreeks die u kiest, maar deze naam moet uniek zijn voor elke logische query in uw app.
  Anders verschillende pull-bewerkingen kunnen het dezelfde tijdstempel voor incrementele synchronisatie overschrijven en uw query's kunnen onjuiste resultaten retourneren.

  Als de query een parameter heeft, is één manier om te maken van een unieke naam op te nemen van de waarde van parameter.
  Als u op gebruikers-id filtert, kan de querynaam van uw als volgt (in C#) worden:

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Als u afmelden voor incrementele synchronisatie wilt, doorgeven `null` als de query-ID. In dit geval alle records worden opgehaald op voor elke aanroep naar `PullAsync`, dit is mogelijk niet efficiënt.
* **Opschonen van**: U kunt de inhoud van het gebruik van het lokale archief wissen `IMobileServiceSyncTable.PurgeAsync`.
  Verwijderen kan nodig zijn als u verouderde gegevens in de client-database hebt, of als u wilt verwijderen van alle wijzigingen in behandeling.

  Een opschonen wordt een tabel uit het lokale archief gewist. Als er bewerkingen in afwachting van synchronisatie met de server-database, het opschonen van een uitzondering genereert, tenzij de *forceren opschonen* parameter is ingesteld.

  Stel in het voorbeeld 'takenlijst' Device1 haalt alleen items die niet zijn voltooid als een voorbeeld van verouderde gegevens op de client. Een todoitem 'Melk koopt' is gemarkeerd als voltooid op de server door een ander apparaat. Echter heeft Device1 nog steeds de todoitem 'Kopen melk' in het lokale archief omdat deze is alleen binnenhalen van items die zijn niet gemarkeerd als voltooid. Een opschonen Hiermee schakelt u dit item verlopen.

## <a name="next-steps"></a>Volgende stappen
* [iOS: offline synchroniseren inschakelen]
* [Xamarin iOS: offline synchroniseren inschakelen]
* [Xamarin Android: Offline synchroniseren inschakelen]
* [Universal Windows Platform: Offline synchroniseren inschakelen]

<!-- Links -->
[SDK voor .NET-client]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Offline synchroniseren inschakelen]: app-service-mobile-android-get-started-offline-data.md
[iOS: offline synchroniseren inschakelen]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: offline synchroniseren inschakelen]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Offline synchroniseren inschakelen]: app-service-mobile-xamarin-android-get-started-offline-data.md
[Universal Windows Platform: Offline synchroniseren inschakelen]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
