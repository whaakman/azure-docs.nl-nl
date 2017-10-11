---
title: Offline synchroniseren van gegevens in Azure Mobile Apps | Microsoft Docs
description: Overzicht van de functie voor het synchroniseren van offline gegevens voor Azure Mobile Apps en voor conceptuele verwijzing
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 982fb683-8884-40da-96e6-77eeca2500e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 8e2bd755d14319f8c66f7ae7ec64fbd10801b39d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="offline-data-sync-in-azure-mobile-apps"></a>Offlinesynchronisatie van gegevens in Azure Mobile Apps
## <a name="what-is-offline-data-sync"></a>Wat is offline synchroniseren van gegevens?
Offline synchroniseren van gegevens is een client en server SDK-functie van Azure Mobile Apps waarmee u gemakkelijk voor ontwikkelaars om apps die functioneel zonder een netwerkverbinding zijn te maken.

Wanneer uw app in de offlinemodus bevindt, kunt u nog steeds maken en wijzigen van gegevens die zijn opgeslagen op een lokaal archief. Wanneer de app weer online is, kunnen lokale wijzigingen worden gesynchroniseerd met uw back-end voor mobiele Apps van Azure. De functie biedt tevens ondersteuning voor het opsporen van conflicten wanneer dezelfde record voor zowel de client als de back-end wordt gewijzigd. Conflicten kunnen vervolgens worden verwerkt op de server of op de client.

Offline synchronisatie heeft verschillende voordelen:

* De reactietijd van de app verbeteren door het in cache opslaan van servergegevens lokaal op het apparaat
* Robuuste apps die nuttig blijven wanneer er netwerkproblemen maken
* Eindgebruikers kunnen maken en gegevens wijzigen, zelfs wanneer er geen toegang tot het netwerk, ondersteuning van scenario's met weinig of geen verbinding toestaan
* Synchroniseren van gegevens op meerdere apparaten en het opsporen van conflicten wanneer dezelfde record is gewijzigd door de twee apparaten
* Netwerkgebruik op hoge latentie of gecontroleerde netwerken beperken

De volgende zelfstudies weergeven offlinesynchronisatie toevoegen aan uw mobiele clients met Azure Mobile Apps:

* [Android: Offlinesynchronisatie inschakelen]
* [Apache Cordova: Offlinesynchronisatie inschakelen](app-service-mobile-cordova-get-started-offline-data.md)
* [iOS: offlinesynchronisatie inschakelen]
* [Xamarin iOS: offlinesynchronisatie inschakelen]
* [Xamarin Android: Offlinesynchronisatie inschakelen]
* [Xamarin.Forms: Offlinesynchronisatie inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [universele Windows-Platform: inschakelen offlinesynchronisatie]

## <a name="what-is-a-sync-table"></a>Wat is een tabel synchroniseren?
Voor toegang tot het eindpunt (/ tabellen), geef de client-SDK's van Azure Mobile interfaces, zoals `IMobileServiceTable` (.NET client SDK) of `MSTable` (iOS-client). Deze API's direct verbinding maken met de back-end voor mobiele Apps van Azure en mislukken als de clientapparaat beschikt niet over een netwerkverbinding.

Ter ondersteuning van offlinegebruik, moet uw app in plaats daarvan gebruiken de *synchronisatietabel* API's, zoals `IMobileServiceSyncTable` (.NET client SDK) of `MSSyncTable` (iOS-client). Alle de dezelfde CRUD-bewerkingen (maken, lezen, bijwerken, verwijderen) in combinatie met synchronisatie tabel API's, behalve nu ze lezen uit of schrijven naar een *lokale archief*. Voordat u synchronisatie tabelbewerkingen kunnen uitvoeren, moet het lokale archief worden geïnitialiseerd.

## <a name="what-is-a-local-store"></a>Wat is een lokaal archief?
Een lokaal archief is de laag voor gegevenspersistentie op het clientapparaat. De Azure Mobile Apps-client-SDK's bieden een standaardimplementatie van het lokale archief. Op Windows, Xamarin en Android, is gebaseerd op SQLite. In iOS, is gebaseerd op gegevens van de Core.

Als u wilt de SQLite-gebaseerde implementatie van Windows Phone of Windows Store 8.1 gebruikt, moet u een SQLite-uitbreiding te installeren. Zie voor meer informatie [universele Windows-Platform: inschakelen offlinesynchronisatie]. Android en iOS worden geleverd met een versie van SQLite in het besturingssysteem van het apparaat zelf, zodat deze niet nodig om te verwijzen naar uw eigen versie van SQLite.

Ontwikkelaars kunnen ook hun eigen lokale store implementeren. Als u gegevens opslaan in een versleutelde indeling op de mobiele-client wenst, kunt u bijvoorbeeld een lokaal archief die gebruikmaakt van SQLCipher voor versleuteling definiëren.

## <a name="what-is-a-sync-context"></a>Wat is er een synchronisatie-context?
Een *sync context* is gekoppeld aan een object mobiele clients (zoals `IMobileServiceClient` of `MSClient`) en bijgehouden wijzigingen die zijn aangebracht met synchronisatie tabellen. De synchronisatie-context onderhoudt een *bewerking wachtrij*, waarvan een geordende lijst met CUD bewerkingen (maken, bijwerken, verwijderen) die later valt houdt worden verzonden naar de server.

Een lokaal archief is gekoppeld aan de context van de synchronisatie met een initialisatiemethode, zoals `IMobileServicesSyncContext.InitializeAsync(localstore)` in de [SDK voor .NET-clients].

## <a name="how-sync-works"></a>Hoe offline synchronisatie werkt
Wanneer u de synchronisatie-tabellen, bepaalt de clientcode als lokale wijzigingen worden gesynchroniseerd met een back-end voor mobiele Apps van Azure. Niets wordt verzonden naar de back-end tot er een aanroep van is *push* lokale wijzigingen. Daarnaast het lokale archief is gevuld met nieuwe gegevens alleen wanneer er een aanroep van *pull* gegevens.

* **Push**: Push is een bewerking van de synchronisatie-context en stuurt alle CUD wijzigingen sinds de laatste push. Houd er rekening mee dat het is niet mogelijk alleen een afzonderlijke tabel wijzigingen verzenden omdat anders operations kunnen worden verzonden volgorde. Push voert een reeks van REST-aanroepen naar uw mobiele Apps van Azure back-end die op zijn beurt Hiermee wijzigt u de server-database.
* **Pull-**: Pull wordt uitgevoerd op basis van per tabel en kan worden aangepast met een query voor het ophalen van slechts een subset van de server-gegevens. De Azure Mobile client SDK's worden vervolgens de resulterende gegevens in het lokale archief invoegen.
* **Impliciete Pushes**: als een pull wordt uitgevoerd op basis van een tabel met wachtende lokale updates, de pull eerst voert een `push()` van de synchronisatie-context. Deze push helpt te beperken van conflicten tussen wijzigingen die al in de wachtrij en nieuwe gegevens van de server.
* **Incrementele synchronisatie**: de eerste parameter voor het pullbewerking is een *querynaam* die alleen op de client wordt gebruikt. Als u een naam voor de niet-null gebruikt, de Azure Mobile SDK wordt uitgevoerd een *incrementele synchronisatie*. Telkens wanneer een pullbewerking retourneert een set resultaten, de meest recente `updatedAt` tijdstempel van die resultaatset wordt opgeslagen in de lokale SDK-systeemtabellen. Alleen records ophalen opeenvolgende pull-bewerkingen na tijdstempel.

  Voor het gebruik van incrementele synchronisatie, moet uw server zinvolle retourneren `updatedAt` waarden en moet ook ondersteuning voor sortering op dit veld. Echter, aangezien de SDK een eigen sorteren op het veld updatedAt voegt, u geen gebruik een pull-query zijn eigen heeft `orderBy` component.

  Naam van de query kan een willekeurige tekenreeks die u kiest zijn, maar het moet uniek zijn voor elke logische query in uw app.
  Anders verschillende pull-bewerkingen, kunnen u de dezelfde incrementele synchronisatie tijdstempel overschrijven en uw query's kunnen onjuiste resultaten retourneren.

  Als de query een parameter heeft, wordt er een manier voor het maken van een unieke naam is de waarde van parameter opnemen.
  Als u op gebruikers-id filtert, kan de querynaam van uw als volgt (in C#) zijn:

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Als u afmelden incrementele synchronisatie wilt, doorgeven `null` als de query-ID. In dit geval alle records worden opgehaald bij elke aanroep naar `PullAsync`, dit is mogelijk niet efficiënt.
* **Opschonen van**: U kunt de inhoud van het gebruik van het lokale archief wissen `IMobileServiceSyncTable.PurgeAsync`.
  Opschonen van mogelijk nodig als u verouderde gegevens in de client-database hebt of als u wilt alle in behandeling zijnde wijzigingen negeren.

  Een opschonen wordt een tabel uit het lokale archief gewist. Als er bewerkingen in afwachting van synchronisatie met de server-database, de leegmaken als er een uitzondering gegenereerd tenzij de *forceren opschonen* parameter is ingesteld.

  Als een voorbeeld van verouderde gegevens op de client, Stel dat in het voorbeeld 'todo list' Device1 haalt alleen items die niet zijn voltooid. Een stukje 'Melk koopt' is gemarkeerd als voltooid op de server door een ander apparaat. Echter heeft Device1 nog steeds de takentabel 'Kopen melk' in het lokale archief omdat deze is alleen binnenhalen van items die zijn niet gemarkeerd als voltooid. Een opschonen wordt dit item verouderde gewist.

## <a name="next-steps"></a>Volgende stappen
* [iOS: offlinesynchronisatie inschakelen]
* [Xamarin iOS: offlinesynchronisatie inschakelen]
* [Xamarin Android: Offlinesynchronisatie inschakelen]
* [universele Windows-Platform: inschakelen offlinesynchronisatie]

<!-- Links -->
[SDK voor .NET-clients]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Offlinesynchronisatie inschakelen]: app-service-mobile-android-get-started-offline-data.md
[iOS: offlinesynchronisatie inschakelen]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: offlinesynchronisatie inschakelen]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Offlinesynchronisatie inschakelen]: app-service-mobile-xamarin-android-get-started-offline-data.md
[universele Windows-Platform: inschakelen offlinesynchronisatie]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
