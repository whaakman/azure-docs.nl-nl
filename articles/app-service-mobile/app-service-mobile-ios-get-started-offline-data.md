---
title: Offline synchroniseren met mobiele iOS-apps inschakelen | Microsoft Docs
description: Informatie over het gebruik van mobiele apps van Azure App Service naar de cache en de synchronisatie van offlinegegevens in iOS-toepassingen.
documentationcenter: ios
author: conceptdev
manager: crdun
editor: ''
services: app-service\mobile
ms.assetid: eb5b9520-0f39-4a09-940a-dadb6d940db8
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: bc0afcf1ac7d9e7a777d850e1b6df7b915837f3a
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956871"
---
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a>Offline synchroniseren met mobiele iOS-apps inschakelen
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie bevat informatie over het offline synchroniseren met de functie Mobile Apps van Azure App Service voor iOS. Met het offline synchroniseren eindgebruikers kunnen communiceren met een mobiele app weergeven, toevoegen of wijzigen van gegevens, zelfs als ze geen netwerkverbinding hebt. Wijzigingen worden opgeslagen in een lokale database. Nadat het apparaat weer online is, worden de wijzigingen worden gesynchroniseerd met de externe back-end.

Als dit de eerste ervaring met Mobile Apps, moet u eerst de zelfstudie voltooien [Een iOS-App maken]. Als u het gedownloade quick start-serverproject niet gebruikt, moet u de toegang tot gegevens extensiepakketten toevoegen aan uw project. Zie voor meer informatie over het server-extensiepakketten [werken met de .NET back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie voor meer informatie over de functie voor offlinesynchronisatie, [Offlinegegevens synchroniseren in mobiele Apps].

## <a name="review-sync"></a>Bekijk de code van de synchronisatie van client
De client-project dat u hebt gedownload voor de [Een iOS-App maken] zelfstudie al-code die ondersteuning biedt voor offline synchronisatie met behulp van een lokale database op basis van de belangrijkste gegevens bevat. In deze sectie bevat een overzicht van wat is al opgenomen in de zelfstudie code. Zie voor een conceptueel overzicht van de functie [Offlinegegevens synchroniseren in mobiele Apps].

Met de functie offline gegevenssynchronisatie van Mobile Apps, kunnen eindgebruikers communiceren met een lokale database zelfs wanneer het netwerk niet toegankelijk is. Voor het gebruik van deze functies in uw app, u de context van de synchronisatie van initialiseren `MSClient` en verwijzen naar een lokale opslag. En vervolgens u verwijzen naar de tabel via de **MSSyncTable** interface.

In **QSTodoService.m** (Objective-C) of **ToDoTableViewController.swift** (Swift), ziet u het type van het lid **syncTable** is **MSSyncTable** . Offline synchronisatie maakt gebruik van deze interface van de tabel synchronisatie in plaats van **MSTable**. Als een synchronisatietabel wordt gebruikt, worden alle bewerkingen gaat u naar het lokale archief en alleen met de externe back-end met expliciete push- en pull-bewerkingen worden gesynchroniseerd.

 Als u een verwijzing naar een tabel synchroniseren, gebruikt de **syncTableWithName** methode voor `MSClient`. Als u wilt verwijderen offlinesynchronisatie functionaliteit, gebruik **tableWithName** in plaats daarvan.

Voordat u een tabelbewerkingen kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd. Dit is de relevante code:

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
   Deze methode maakt u een lokale opslag met behulp van de `MSCoreDataStore` interface, waardoor de SDK voor Mobile Apps biedt. U kunt ook een andere lokale archief opgeven door het implementeren van de `MSSyncContextDataSource` protocol. Ook de eerste parameter van **MSSyncContext** wordt gebruikt om op te geven van een conflict-handler. Omdat we hebt doorgegeven `nil`, krijgen we de standaard conflict handler, die niet op een conflict.

Nu gaan we de werkelijke synchronisatiebewerking uitvoeren en gegevens ophalen uit de externe back-end:

* **Objective-C**. `syncData` nieuwe wijzigingen eerst pushes en roept daarna **pullData** gegevens ophalen uit de externe back-end. Op zijn beurt de **pullData** methode haalt nieuwe gegevens die overeenkomt met een query:

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

In de Objective-C-versie in `syncData`, noemen we eerst **pushWithCompletion** in de context van de synchronisatie. Deze methode is lid van `MSSyncContext` (en niet de synchronisatietabel zelf) omdat wijzigingen in alle tabellen worden gepusht. Alleen de records die mogelijk zijn gewijzigd op een bepaalde manier lokaal (via CUD bewerkingen) worden verzonden naar de server. Vervolgens het hulpprogramma **pullData** wordt aangeroepen, welke aanroepen **MSSyncTable.pullWithQuery** externe gegevens ophalen en opslaan in de lokale database.

In de versie van het Swift, omdat de push-bewerking niet strikt noodzakelijk is is, er is geen aanroep naar **pushWithCompletion**. Als er wijzigingen in behandeling in de context van de synchronisatie voor de tabel die een push-bewerking doet zijn, op te halen altijd problemen met een push eerst. Hebt u meer dan één synchronisatietabel, is het echter aanbevolen om aan te roepen expliciet push om ervoor te zorgen dat alles consistent voor gerelateerde tabellen is.

In zowel de Objective-C en Swift versies, kunt u de **pullWithQuery** methode om op te geven van een query voor het filteren van de records die u wilt ophalen. In dit voorbeeld wordt de query haalt u alle records in de externe `TodoItem` tabel.

De tweede parameter van **pullWithQuery** is een query-ID die wordt gebruikt voor *incrementele synchronisatie*. Incrementele synchronisatie worden alleen de records die zijn gewijzigd sinds de laatste synchronisatie, met behulp van de record opgehaald `UpdatedAt` tijdstempel (met de naam `updatedAt` in het lokale archief.) De query-ID moet een beschrijvende tekenreeks die uniek is voor elke logische query in uw app. Als u wilt afmelden voor incrementele synchronisatie, doorgeven `nil` als de query-ID. Deze methode kan zijn mogelijk niet efficiënt, omdat het ophalen van alle records voor elke pullbewerking.

De Objective-C-app wordt gesynchroniseerd wanneer u wijzigen of toevoegen van gegevens, wanneer een gebruiker het gebaar van de vernieuwing uitvoert, en bij het starten.

De Swift-app wordt gesynchroniseerd wanneer de gebruiker het gebaar van de vernieuwing uitvoert en bij het starten.

Omdat de app wordt gesynchroniseerd wanneer de gegevens zijn gewijzigd (Objective-C) of wanneer de app wordt gestart (Objective-C en Swift), de app wordt ervan uitgegaan dat de gebruiker online is. In een volgende sectie werkt u de app zodat gebruikers bewerken kunnen, zelfs als ze offline zijn.

## <a name="review-core-data"></a>Bekijk de belangrijkste gegevens-model
Wanneer u de belangrijkste gegevens offline store gebruikt, moet u bepaalde tabellen en velden definiëren in het gegevensmodel. De voorbeeld-app bevat al een gegevensmodel met de juiste indeling. In deze sectie behandelen we deze tabellen om weer te geven hoe ze worden gebruikt.

Open **QSDataModel.xcdatamodeld**. Vier tabellen zijn gedefinieerd--drie die worden gebruikt door de SDK en één die wordt gebruikt voor de taak zelf items:
  * MS_TableOperations: Houdt bij of de items die moeten worden gesynchroniseerd met de server.
  * MS_TableOperationErrors: Houdt bij of eventuele fouten die tijdens het offline synchroniseren optreden.
  * MS_TableConfig: Sporen te wissen in de laatste bijgewerkt voor de laatste synchronisatiebewerking voor alle pull-bewerkingen.
  * TodoItem: Slaat de to-do-items. De systeemkolommen **createdAt**, **updatedAt**, en **versie** zijn optioneel Systeemeigenschappen.

> [!NOTE]
> De Mobile Apps SDK reserveert kolomnamen die met beginnen '**``**'. Gebruik dit voorvoegsel niet met iets anders dan systeemkolommen. De kolomnamen zijn anders gewijzigd wanneer u de externe back-end gebruikt.
>
>

Wanneer u de functie voor offlinesynchronisatie, definieert u de drie tabellen en de tabel.

### <a name="system-tables"></a>Systeemtabellen

**MS_TableOperations**  

![MS_TableOperations tabelkenmerken][defining-core-data-tableoperations-entity]

| Kenmerk | Type |
| --- | --- |
| id | Geheel getal 64 |
| artikel-id | Reeks |
| properties | Binaire gegevens |
| tabel | Reeks |
| tableKind | Geheel getal 16 |


**MS_TableOperationErrors**

 ![MS_TableOperationErrors tabelkenmerken][defining-core-data-tableoperationerrors-entity]

| Kenmerk | Type |
| --- | --- |
| id |Reeks |
| operationId |Geheel getal 64 |
| properties |Binaire gegevens |
| tableKind |Geheel getal 16 |

 **MS_TableConfig**

 ![][defining-core-data-tableconfig-entity]

| Kenmerk | Type |
| --- | --- |
| id |Reeks |
| sleutel |Reeks |
| Sleuteltype |Geheel getal 64 |
| tabel |Reeks |
| waarde |Reeks |

### <a name="data-table"></a>Gegevenstabel

**TodoItem**

| Kenmerk | Type | Opmerking |
| --- | --- | --- |
| id | Tekenreeks, gemarkeerd als vereist |primaire sleutel in de externe opslag |
| Voltooien | Booleaans | Veld to-do-item |
| tekst |Reeks |Veld to-do-item |
| createdAt | Date | (optioneel) Toegewezen aan **createdAt** systeemeigenschap |
| updatedAt | Date | (optioneel) Toegewezen aan **updatedAt** systeemeigenschap |
| versie | Reeks | (optioneel) Voor het detecteren van conflicten, toegewezen aan versie |

## <a name="setup-sync"></a>Het gedrag van de synchronisatie van de app wijzigen
In deze sectie maakt wijzigen u de app zodat deze wordt niet gesynchroniseerd op de app start of als u invoegen en bijwerken van items. Deze synchroniseert alleen als de knop Vernieuwen gebaar wordt uitgevoerd.

**Objective-C**:

1. In **QSTodoListViewController.m**, wijzigt de **viewDidLoad** methode om te verwijderen van de aanroep van `[self refresh]` aan het einde van de methode. Nu dat de gegevens niet is gesynchroniseerd met de server op het startscherm van de app. In plaats daarvan het gesynchroniseerd met de inhoud van het lokale archief.
2. In **QSTodoService.m**, wijzigen van de definitie van `addItem` zodat deze niet gesynchroniseerd nadat het item wordt ingevoegd. Verwijder de `self syncData` blokkeren en vervang deze door het volgende:

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. Wijzigen van de definitie van `completeItem` zoals eerder is vermeld. Verwijderen van het blok voor `self syncData` en vervang deze door het volgende:
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

**SWIFT**:

In `viewDidLoad`in **ToDoTableViewController.swift**, een opmerking bij de twee regels die hieronder om te synchroniseren op het startscherm van de app stoppen. Op het moment van dit artikel is geschreven, komt de Swift-taken-app niet de service bijwerken wanneer iemand toevoegt of een item is voltooid. De service alleen op het startscherm van de app worden bijgewerkt.

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <a name="test-app"></a>De app testen
In deze sectie maakt verbinding u met een ongeldige URL voor het simuleren van een offline-scenario. Wanneer u gegevensitems toevoegt, zijn ondergebracht in de lokale belangrijkste gegevens opslaan, maar ze zijn niet gesynchroniseerd met de back-end voor mobiele Apps.

1. Wijzig de URL van de mobiele apps in **QSTodoService.m** naar een ongeldige URL en voer de app opnieuw uit:

   **Objective-C**. In QSTodoService.m:
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   **SWIFT**. In ToDoTableViewController.swift:
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. Voeg enkele taakitems toe. Sluit de simulator (of de app wordt geforceerd sluiten), en vervolgens opnieuw te starten. Controleer of dat uw wijzigingen behouden blijven.

3. De inhoud van de externe **TodoItem** tabel:
   * Voor Node.js-back-end gaat u naar de [Azure-portal](https://portal.azure.com/) en in uw mobiele Apps back-end, klikt u op **eenvoudige tabellen** > **TodoItem**.  
   * Voor een .NET back end, gebruikt u een SQL-hulpprogramma, zoals SQL Server Management Studio of een REST-client, zoals Fiddler of Postman.  

4. Controleer of de nieuwe items hebt *niet* is gesynchroniseerd met de server.

5. Wijzig de URL terug naar de juiste naam in **QSTodoService.m**, en voer de app opnieuw uit.

6. Het gebaar van de vernieuwing uitvoeren door het omlaag in de lijst met items te schuiven.  
Een spinner voortgang wordt weergegeven.

7. Weergave de **TodoItem** gegevens opnieuw. De nieuwe en gewijzigde to-do-items moeten nu worden weergegeven.

## <a name="summary"></a>Samenvatting
Ter ondersteuning van de functie voor offlinesynchronisatie, hebben we gebruikt de `MSSyncTable` interface en geïnitialiseerd `MSClient.syncContext` met een lokale opslag. In dit geval is het lokale archief een database op basis van de belangrijkste gegevens.

Als u een lokaal archief van de belangrijkste gegevens gebruikt, moet u meerdere tabellen met definiëren de [corrigeren Systeemeigenschappen](#review-core-data).

De normale maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen voor mobiele apps werken als de app nog steeds is verbonden, maar alle bewerkingen op basis van het lokale archief plaatsvinden.

Wanneer we het lokale archief gesynchroniseerd met de server, we hebben gebruikt de **MSSyncTable.pullWithQuery** methode.

## <a name="additional-resources"></a>Aanvullende resources
* [Offlinegegevens synchroniseren in mobiele Apps]
* [Cloud Cover: Offline synchronisatie in Azure Mobile Services] \(de video gaat over Mobile Services, maar Mobile Apps offline synchronisatie werkt op een soortgelijke manier.\)

<!-- URLs. -->


[Een iOS-App maken]: app-service-mobile-ios-get-started.md
[Offlinegegevens synchroniseren in mobiele Apps]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Cloud Cover: Offline synchronisatie in Azure Mobile Services]: https://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: https://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
