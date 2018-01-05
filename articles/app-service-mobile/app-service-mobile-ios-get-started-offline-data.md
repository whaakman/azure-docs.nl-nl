---
title: Offline synchronisatie met de mobiele iOS-apps inschakelen | Microsoft Docs
description: Informatie over het gebruik van mobiele apps van Azure App Service-cache en sync offline gegevens in de iOS-toepassingen.
documentationcenter: ios
author: conceptdev
manager: crdun
editor: 
services: app-service\mobile
ms.assetid: eb5b9520-0f39-4a09-940a-dadb6d940db8
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: b676b51241e4883fb1b4c40caba8e281bfa68a4c
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a>Offline synchronisatie met de mobiele iOS-apps inschakelen
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie bevat informatie over het offline synchroniseren met de functie Mobile Apps van Azure App Service voor iOS. Met offline synchroniseert eindgebruikers kunnen communiceren met een mobiele app weergeven, toevoegen of wijzigen van gegevens, zelfs wanneer er geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Nadat het apparaat weer online is, worden de wijzigingen zijn gesynchroniseerd met de externe back-end.

Als dit de eerste ervaring met Mobile Apps, moet u eerst Voltooi de zelfstudie [maken van een iOS-App]. Als u het gedownloade quick start-serverproject niet gebruikt, moet u de toegang tot gegevens extensiepakketten toevoegen aan uw project. Zie voor meer informatie over server extensiepakketten [werken met de .NET-back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie voor meer informatie over de functie offlinesynchronisatie, [Offline synchroniseren van gegevens in mobiele Apps].

## <a name="review-sync"></a>Bekijk de clientcode voor synchronisatie
De client-project dat u hebt gedownload voor de [maken van een iOS-App] zelfstudie bevat al de code die ondersteuning biedt voor offlinesynchronisatie met behulp van een lokale database op basis van gegevens van de Core. Deze sectie bevat een overzicht van wat is al opgenomen in de zelfstudie code. Zie voor een conceptueel overzicht van de functie [Offline synchroniseren van gegevens in mobiele Apps].

Met de functie offline synchroniseren van gegevens van mobiele Apps, kunnen eindgebruikers communiceren met een lokale database zelfs wanneer het netwerk niet toegankelijk is. Als u deze functies in uw app gebruikt, wilt u de context van de synchronisatie van initialiseren `MSClient` en verwijzen naar een lokaal archief. Vervolgens u verwijzen naar de tabel via de **MSSyncTable** interface.

In **QSTodoService.m** (Objective-C) of **ToDoTableViewController.swift** (Swift), u ziet dat het type van het lid **syncTable** is **MSSyncTable**. Offline synchronisatie maakt gebruik van deze synchronisatie tabel interface in plaats van **MSTable**. Wanneer een synchronisatietabel wordt gebruikt, worden alle bewerkingen gaat u naar het lokale archief en alleen met de externe back-end met expliciete push als pull-bewerkingen worden gesynchroniseerd.

 Als u een verwijzing naar een synchronisatietabel, gebruikt de **syncTableWithName** methode op `MSClient`. Gebruik te verwijderen van de functie offline synchroniseren van **tableWithName** in plaats daarvan.

Voordat u tabelbewerkingen kunnen uitvoeren, moet het lokale archief worden geïnitialiseerd. Dit is de relevante code:

* **Objective-C**. In de **QSTodoService.init** methode:

   ```objc
   MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
   self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
   ```    
* **SWIFT**. In de **ToDoTableViewController.viewDidLoad** methode:

   ```swift
   let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
   let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
   self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
   client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
   ```
   Deze methode maakt u een lokaal archief met behulp van de `MSCoreDataStore` interface, waardoor de Mobile Apps SDK biedt. U kunt ook een ander lokaal archief opgeven door het implementeren van de `MSSyncContextDataSource` protocol. Ook de eerste parameter van **MSSyncContext** wordt gebruikt om op te geven van een conflict-handler. Omdat we hebt doorgegeven `nil`, krijgen we de handler standaard conflict, die niet bij een conflict.

Nu gaan we de werkelijke synchronisatiebewerking uitvoeren en gegevens ophalen uit de externe back-end:

* **Objective-C**. `syncData`eerst pushes nieuwe wijzigingen en roept vervolgens **pullData** gegevens van de externe back-end ophalen. Op zijn beurt de **pullData** methode haalt nieuwe gegevens die overeenkomt met een query:

   ```objc
   -(void)syncData:(QSCompletionBlock)completion
   {
       // Push all changes in the sync context, and then pull new data.
       [self.client.syncContext pushWithCompletion:^(NSError *error) {
           [self logErrorIfNotNil:error];
           [self pullData:completion];
       }];
   }

   -(void)pullData:(QSCompletionBlock)completion
   {
       MSQuery *query = [self.syncTable query];

       // Pulls data from the remote server into the local table.
       // We're pulling all items and filtering in the view.
       // Query ID is used for incremental sync.
       [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
           [self logErrorIfNotNil:error];

           // Lets the caller know that we have finished.
           if (completion != nil) {
               dispatch_async(dispatch_get_main_queue(), completion);
           }
       }];
   }
   ```
* **SWIFT**:
   ```swift
   func onRefresh(sender: UIRefreshControl!) {
      UIApplication.sharedApplication().networkActivityIndicatorVisible = true

      self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
          (error) -> Void in

          UIApplication.sharedApplication().networkActivityIndicatorVisible = false

          if error != nil {
              // A real application would handle various errors like network conditions,
              // server conflicts, etc via the MSSyncContextDelegate
              print("Error: \(error!.description)")

              // We will discard our changes and keep the server's copy for simplicity
              if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                  for opError in opErrors {
                      print("Attempted operation to item \(opError.itemId)")
                      if (opError.operation == .Insert || opError.operation == .Delete) {
                          print("Insert/Delete, failed discarding changes")
                          opError.cancelOperationAndDiscardItemWithCompletion(nil)
                      } else {
                          print("Update failed, reverting to server's copy")
                          opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                      }
                  }
              }
          }
          self.refreshControl?.endRefreshing()
      }
   }
   ```

In de Objective-C-versie in `syncData`, noemen we eerst **pushWithCompletion** van de synchronisatie-context. Deze methode maakt deel uit van `MSSyncContext` (en niet de synchronisatietabel zelf) omdat deze wijzigingen pushes in alle tabellen. Alleen de records die zijn gewijzigd op een bepaalde manier lokaal (via CUD bewerkingen) worden verzonden naar de server. Klik op de helper **pullData** wordt genoemd, welke aanroepen **MSSyncTable.pullWithQuery** externe gegevens ophalen en opslaan in de lokale database.

In de Swift versie, omdat de push-bewerking niet strikt noodzakelijk is is, er is geen aanroep naar **pushWithCompletion**. Als er wijzigingen in behandeling in de context van de synchronisatie voor de tabel die een push-bewerking doet zijn, pull-altijd problemen met een push eerst. Als er meer dan één synchronisatietabel, is het echter het beste push om ervoor te zorgen dat alles consistent is voor alle gerelateerde tabellen niet expliciet aanroepen.

In de Objective-C- en Swift versies, kunt u de **pullWithQuery** methode om op te geven van een query voor het filteren van de records die u wilt ophalen. In dit voorbeeld wordt de query haalt alle records in de externe `TodoItem` tabel.

De tweede parameter van **pullWithQuery** een query-id die wordt gebruikt voor *incrementele synchronisatie*. Incrementele synchronisatie worden alleen de records die zijn gewijzigd sinds de laatste synchronisatie, met behulp van de record opgehaald `UpdatedAt` tijdstempel (aangeroepen `updatedAt` in het lokale archief.) De query-ID moet een beschrijvende tekenreeks die uniek is voor elke logische query in uw app. Als u wilt afmelden incrementele synchronisatie, doorgeven `nil` als de query-ID. Deze methode kan worden mogelijk inefficiënt, omdat het ophalen van alle records op elk pull-bewerking.

De Objective-C-app wordt gesynchroniseerd wanneer u wijzigen of toevoegen van gegevens, wanneer een gebruiker de gebaar vernieuwen uitvoert, en op starten.

De Swift-app wordt gesynchroniseerd wanneer de gebruiker de gebaar vernieuwen voert en op starten.

Omdat de app wordt gesynchroniseerd wanneer de gegevens zijn gewijzigd (Objective-C) of wanneer de app gestart (Objective-C en Swift), de app wordt ervan uitgegaan dat de gebruiker online is. In een volgende sectie wordt de app bijgewerkt, zodat gebruikers bewerken kunnen, zelfs als ze offline zijn.

## <a name="review-core-data"></a>Bekijk het gegevensmodel van Core
Wanneer u de belangrijkste offline gegevensopslag, moet u bepaalde tabellen en velden definiëren in het gegevensmodel. De voorbeeld-app bevat al een gegevensmodel met de juiste indeling. In deze sectie doorlopen we deze tabellen om weer te geven hoe ze worden gebruikt.

Open **QSDataModel.xcdatamodeld**. Er zijn vier tabellen gedefinieerd--drie die worden gebruikt door de SDK en één die wordt gebruikt voor de taak zelf items:
  * MS_TableOperations: Houdt de items die moeten worden gesynchroniseerd met de server.
  * MS_TableOperationErrors: Houdt eventuele fouten die tijdens het offline synchroniseren optreden.
  * MS_TableConfig: Houdt de laatste keer bijgewerkt voor de laatste synchronisatiebewerking voor alle pull-bewerkingen.
  * Takentabel: De taakitems worden opgeslagen. De systeemkolommen **createdAt**, **updatedAt**, en **versie** zijn optionele Systeemeigenschappen.

> [!NOTE]
> De Mobile Apps SDK reserveert kolomnamen die met beginnen '**``**'. Dit voorvoegsel niet gebruiken voor iets anders dan systeemkolommen. Anders wordt uw kolomnamen gewijzigd wanneer u de externe back-end.
>
>

Wanneer u de functie offlinesynchronisatie gebruikt, de drie systeemtabellen en de gegevenstabel definiëren.

### <a name="system-tables"></a>Systeemtabellen

**MS_TableOperations**  

![MS_TableOperations tabelkenmerken][defining-core-data-tableoperations-entity]

| Kenmerk | Type |
| --- | --- |
| id | Geheel getal 64 |
| itemId | Tekenreeks |
| properties | Binaire gegevens |
| tabel | Tekenreeks |
| tableKind | Geheel getal 16 |


**MS_TableOperationErrors**

 ![MS_TableOperationErrors tabelkenmerken][defining-core-data-tableoperationerrors-entity]

| Kenmerk | Type |
| --- | --- |
| id |Tekenreeks |
| operationId |Geheel getal 64 |
| properties |Binaire gegevens |
| tableKind |Geheel getal 16 |

 **MS_TableConfig**

 ![][defining-core-data-tableconfig-entity]

| Kenmerk | Type |
| --- | --- |
| id |Tekenreeks |
| sleutel |Tekenreeks |
| KeyType |Geheel getal 64 |
| tabel |Tekenreeks |
| waarde |Tekenreeks |

### <a name="data-table"></a>Gegevenstabel

**Takentabel**

| Kenmerk | Type | Opmerking |
| --- | --- | --- |
| id | Tekenreeks is gemarkeerd als vereist |primaire sleutel in externe opslag |
| Voltooien | Boole-waarde | Taak itemveld |
| Tekst |Tekenreeks |Taak itemveld |
| CreatedAt | Date | (optioneel) Toegewezen aan **createdAt** systeemeigenschap |
| updatedAt | Date | (optioneel) Toegewezen aan **updatedAt** systeemeigenschap |
| versie | Tekenreeks | (optioneel) Gebruikt voor het opsporen van conflicten, is toegewezen aan versie |

## <a name="setup-sync"></a>Het gedrag van de synchronisatie van de app wijzigen
In deze sectie maakt wijzigen u de app zodat deze wordt niet gesynchroniseerd op app starten of als u invoegen en items bijwerken. Synchronisatie alleen wanneer de knop Vernieuwen gebaar wordt uitgevoerd.

**Objective-C**:

1. In **QSTodoListViewController.m**, wijzig de **viewDidLoad** methode om te verwijderen van de aanroep van `[self refresh]` aan het einde van de methode. Nu de gegevens is niet gesynchroniseerd met de server in de app is gestart. In plaats daarvan wordt deze gesynchroniseerd met de inhoud van het lokale archief.
2. In **QSTodoService.m**, wijzig de definitie van `addItem` zodat deze niet synchroniseren nadat het item is ingevoegd. Verwijder de `self syncData` blokkeren en vervang deze door het volgende:

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. Wijzig de definitie van `completeItem` zoals eerder is vermeld. Verwijderen van het blok voor `self syncData` en vervang deze door het volgende:
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

**SWIFT**:

In `viewDidLoad`in **ToDoTableViewController.swift**, uitcommentarieer de twee regels die hier worden weergegeven voor het stoppen van de synchronisatie bij starten van de app. Op het moment van dit artikel biedt de Swift taken-app niet de service bijwerken wanneer iemand toevoegt of een item is voltooid. De service alleen op app starten wordt bijgewerkt.

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <a name="test-app"></a>De app testen
In deze sectie maakt verbinding u met een ongeldige URL om te simuleren een offline-scenario. Wanneer u gegevens toevoegt, zijn ondergebracht in de lokale Core gegevensarchief, maar ze zijn niet gesynchroniseerd met de back-end voor mobiele app.

1. Wijzig de URL van de mobiele app in **QSTodoService.m** naar een ongeldige URL en voer de app opnieuw:

   **Objective-C**. In QSTodoService.m:
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   **SWIFT**. In ToDoTableViewController.swift:
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. Sommige taakitems toevoegen. Sluit de simulator (of de app wordt geforceerd sluiten), en start deze opnieuw. Controleer of uw wijzigingen blijven behouden.

3. De inhoud van de externe **TodoItem** tabel:
   * Voor Node.js-back-end gaat u naar de [Azure-portal](https://portal.azure.com/) en op uw mobiele app back-end, **gemakkelijk tabellen** > **TodoItem**.  
   * Voor een .NET terug beëindigen, gebruikt u een SQL-hulpprogramma, zoals SQL Server Management Studio of een REST-client, zoals Fiddler of Postman.  

4. Controleer of de nieuwe items hebt *niet* is gesynchroniseerd met de server.

5. Wijzig de URL terug naar de juiste naam in **QSTodoService.m**, en voer de app opnieuw uit.

6. De vernieuwing gebaar uitvoeren met het binnenhalen van de lijst met items.  
Een spinner voortgang wordt weergegeven.

7. Weergave de **TodoItem** gegevens opnieuw. De nieuwe en gewijzigde taakitems moeten nu worden weergegeven.

## <a name="summary"></a>Samenvatting
Ter ondersteuning van de functie offlinesynchronisatie, hebben we gebruikt de `MSSyncTable` interface en geïnitialiseerd `MSClient.syncContext` met een lokaal archief. In dit geval wordt is het lokale archief een database op basis van gegevens van de Core.

Wanneer u een Core lokale gegevensarchief gebruikt, moet u verschillende tabellen met definiëren de [corrigeren Systeemeigenschappen](#review-core-data).

De normale maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen voor mobiele apps werken alsof de app nog steeds is verbonden, maar alle bewerkingen op basis van het lokale archief plaatsvinden.

Wanneer wij het lokale archief gesynchroniseerd met de server, hebben we gebruikt de **MSSyncTable.pullWithQuery** methode.

## <a name="additional-resources"></a>Aanvullende resources
* [Offline synchroniseren van gegevens in mobiele Apps]
* [Cloud behandeld: Offlinesynchronisatie in Azure Mobile Services] \(de video gaat over Mobile Services, maar Mobile Apps offline synchronisatie werkt op een vergelijkbare manier.\)

<!-- URLs. -->


[maken van een iOS-App]: app-service-mobile-ios-get-started.md
[Offline synchroniseren van gegevens in mobiele Apps]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Cloud behandeld: Offlinesynchronisatie in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
