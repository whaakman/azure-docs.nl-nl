---
title: Offlinesynchronisatie inschakelen voor uw Azure Mobile App (Android)
description: Informatie over het gebruik van App Service Mobile Apps aan de cache en synchroniseren offlinesynchronisatie van gegevens in uw Android-toepassing
documentationcenter: android
author: conceptdev
manager: crdun
services: app-service\mobile
ms.assetid: 32a8a079-9b3c-4faf-8588-ccff02097224
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: a20c79acce8c9dc9051651a0473fd07b8e62f5de
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52960342"
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Offlinesynchronisatie inschakelen voor uw mobiele Android-app
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie bevat informatie over de functie voor offlinesynchronisatie van Azure Mobile Apps voor Android. Offlinesynchronisatie kunnen eindgebruikers om te communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;, zelfs wanneer er geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe back-end.

Als dit de eerste ervaring met Azure Mobile Apps, moet u eerst de zelfstudie voltooien [Een Android-App maken]. Als u het gedownloade quick start-serverproject niet gebruikt, moet u de data access-extensiepakketten toevoegen aan uw project. Zie voor meer informatie over het server-extensiepakketten [werken met de .NET back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie het onderwerp voor meer informatie over de functie voor offlinesynchronisatie, [Offlinesynchronisatie van gegevens in Azure Mobile Apps].

## <a name="update-the-app-to-support-offline-sync"></a>De app ondersteuning voor offlinesynchronisatie bijwerken
Offline synchroniseren zorgt u om te lezen en schrijven van een *synchronisatietabel* (met behulp van de *IMobileServiceSyncTable* interface), die deel uitmaakt van een **SQLite** database op uw apparaat.

Als u wilt pushen en ophalen van wijzigingen tussen het apparaat en Azure Mobile Services, gebruikt u een *synchronisatiecontext* (*MobileServiceClient.SyncContext*), die u met de lokale database voor het opslaan van initialiseren gegevens lokaal.

1. In `TodoActivity.java`, een opmerking bij de bestaande definitie van `mToDoTable` en verwijder opmerkingen bij de versie van de tabel synchroniseren:
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. In de `onCreate` methode, een opmerking bij de initialisatie van de bestaande van `mToDoTable` en verwijder de opmerkingen in deze definitie:
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. In `refreshItemsFromTable` opmerking uit de definitie van `results` en verwijder de opmerkingen in deze definitie:
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. Een opmerking bij de definitie van `refreshItemsFromMobileServiceTable`.
5. Verwijder de opmerkingen in de definitie van `refreshItemsFromMobileServiceTableSyncTable`:
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. Verwijder de opmerkingen in de definitie van `sync`:
   
        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>De app testen
In deze sectie kunt u testen het gedrag met Wi-Fi op, en schakelt u vervolgens Wi-Fi te maken van een offline-scenario.

Wanneer u gegevensitems toevoegt, worden ze die zijn ondergebracht in het lokale archief van de SQLite, maar niet gesynchroniseerd met de mobiele service, drukt u op de **vernieuwen** knop. Andere apps mogelijk verschillende vereisten met betrekking tot wanneer gegevens moeten worden gesynchroniseerd, maar voor deze demo in deze zelfstudie heeft de gebruiker deze aanvragen.

Wanneer u op die knop drukt, wordt er een nieuwe achtergrondtaak gestart. Deze eerst pushes alle wijzigingen in het lokale archief met behulp van synchronisatiecontext en vervolgens de gegevens worden gewijzigd van Azure naar de lokale tabel.

### <a name="offline-testing"></a>Offline testen
1. Plaats het apparaat of de simulator in *vliegtuigmodus*. Hiermee maakt u een offline-scenario.
2. Voegt u enkele *ToDo* items of bepaalde items als voltooid markeren. Sluit het apparaat of de simulator (of de app wordt geforceerd sluiten) en start opnieuw op. Controleer of dat uw wijzigingen zijn opgeslagen op het apparaat omdat ze zijn ondergebracht in het lokale archief van de SQLite.
3. De inhoud van de Azure *TodoItem* tabel met een SQL-hulpprogramma, zoals *SQL Server Management Studio*, of een REST-client, zoals *Fiddler* of  *Postman*. Controleer of de nieuwe items hebt *niet* is gesynchroniseerd met de server
   
       + Voor een Node.js-back-end, gaat u naar de [Azure-portal](https://portal.azure.com/), en in uw mobiele App back-end op **eenvoudige tabellen** > **TodoItem** om weer te geven van de inhoud van de `TodoItem`tabel.
       + Bekijk voor een .NET back-end, de inhoud van de tabel met een SQL-hulpprogramma, zoals *SQL Server Management Studio*, of een REST-client, zoals *Fiddler* of *Postman*.
4. Wi-Fi inschakelen in het apparaat of de simulator. Vervolgens drukt u op de **vernieuwen** knop.
5. De TodoItem-gegevens weer in de Azure-portal. De nieuwe en gewijzigde TodoItems wordt nu weergegeven.

## <a name="additional-resources"></a>Aanvullende resources
* [Offlinesynchronisatie van gegevens in Azure Mobile Apps]
* [Cloud Cover: Offline synchronisatie in Azure Mobile Services] \(Opmerking: de video zich in Mobile Services, maar offline synchronisatie werkt op dezelfde manier in Azure Mobile Apps\)

<!-- URLs. -->

[Offlinesynchronisatie van gegevens in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md

[Een Android-App maken]: app-service-mobile-android-get-started.md

[Cloud Cover: Offline synchronisatie in Azure Mobile Services]: https://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: https://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

