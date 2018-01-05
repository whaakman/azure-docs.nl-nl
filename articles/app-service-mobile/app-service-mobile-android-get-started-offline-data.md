---
title: Offlinesynchronisatie voor uw Azure Mobile Apps (Android) inschakelen
description: Informatie over het gebruik van App Service Mobile Apps aan cache en sync offline gegevens in uw Android-toepassing
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
ms.openlocfilehash: 152702bed0ea061c3cb86e2ff6f88bf204f9d243
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Offlinesynchronisatie voor uw mobiele Android-app inschakelen
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie bevat de functie offline synchroniseren van Azure Mobile Apps voor Android. Offlinesynchronisatie kunnen eindgebruikers werken met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs wanneer er geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe back-end.

Als dit de eerste ervaring met Azure Mobile Apps, moet u eerst Voltooi de zelfstudie [maken van een Android-App]. Als u het gedownloade quick start-serverproject niet gebruikt, moet u de data access-extensiepakketten toevoegen aan uw project. Zie voor meer informatie over server extensiepakketten [werken met de .NET-back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie het onderwerp voor meer informatie over de functie offlinesynchronisatie [Offline synchroniseren van gegevens in Azure Mobile Apps].

## <a name="update-the-app-to-support-offline-sync"></a>De app ter ondersteuning van offlinesynchronisatie bijwerken
Met het offline synchroniseren u om te lezen en schrijven van een *synchronisatietabel* (met behulp van de *IMobileServiceSyncTable* interface), die deel uitmaakt van een **SQLite** database op het apparaat.

Als u wilt push als pull wijzigingen tussen het apparaat en Azure Mobile Services, die u gebruikt een *synchronisatiecontext* (*MobileServiceClient.SyncContext*), die u met de lokale database voor het opslaan van lokale gegevens initialiseren.

1. In `TodoActivity.java`, uitcommentarieer de bestaande definitie van `mToDoTable` en het commentaar verwijderen van de versie van de tabel synchronisatie:
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. In de `onCreate` methode uitcommentarieer de initialisatie van de bestaande van `mToDoTable` en het commentaar verwijderen van deze definitie voor:
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. In `refreshItemsFromTable` uitcommentarieer de definitie van `results` en het commentaar verwijderen van deze definitie voor:
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. De definitie van commentaar `refreshItemsFromMobileServiceTable`.
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
In deze sectie kunt u testen van het gedrag met Wi-Fi op en schakel vervolgens uit Wi-Fi voor het maken van een offline-scenario.

Wanneer u gegevens toevoegt, worden ze ondergebracht in het lokale archief van SQLite, maar niet gesynchroniseerd met de mobiele service totdat u op de **vernieuwen** knop. Andere apps kunnen verschillende vereisten met betrekking tot wanneer gegevens moeten worden gesynchroniseerd, maar voor demonstratiedoeleinden in deze zelfstudie de aanvragen van deze gebruiker heeft.

Wanneer u op deze knop klikt, wordt er een nieuwe achtergrondtaak gestart. Deze duwt eerst alle wijzigingen in het lokale archief met synchronisatiecontext, wordt de gegevens worden gewijzigd van Azure naar de lokale tabel.

### <a name="offline-testing"></a>Offline testen
1. Plaats het apparaat of emulator in *vliegtuigmodus*. Hiermee maakt u een offline-scenario.
2. Voeg enkele toe *ToDo* items of bepaalde items als voltooid markeren. Sluit het apparaat of de simulator (of de app wordt geforceerd sluiten) en opnieuw te starten. Controleer of dat uw wijzigingen zijn opgeslagen op het apparaat omdat ze worden beheerd in het lokale archief van SQLite.
3. De inhoud van de Azure *TodoItem* tabel met een SQL-hulpprogramma zoals *SQL Server Management Studio*, of een REST-client, zoals *Fiddler* of *Postman*. Controleer of de nieuwe items hebt *niet* is gesynchroniseerd met de server
   
       + Voor een Node.js-back-end, gaat u naar de [Azure-portal](https://portal.azure.com/), en in uw Mobile App back-end op **gemakkelijk tabellen** > **TodoItem** om weer te geven van de inhoud van de `TodoItem` tabel.
       + Voor een .NET-backend bekijkt u de inhoud van de tabel met een SQL zoals hulpprogramma *SQL Server Management Studio*, of een REST-client, zoals *Fiddler* of *Postman*.
4. Wi-Fi inschakelen in het apparaat of de simulator. Klik vervolgens op de **vernieuwen** knop.
5. De gegevens van de taken weer in de Azure portal. De nieuwe en gewijzigde taken wordt nu weergegeven.

## <a name="additional-resources"></a>Aanvullende resources
* [Offline synchroniseren van gegevens in Azure Mobile Apps]
* [Cloud behandeld: Offlinesynchronisatie in Azure Mobile Services] \(Opmerking: de video zich in Mobile Services, maar offlinesynchronisatie werkt op een vergelijkbare manier als in Azure Mobile Apps\)

<!-- URLs. -->

[Offline synchroniseren van gegevens in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md

[maken van een Android-App]: app-service-mobile-android-get-started.md

[Cloud behandeld: Offlinesynchronisatie in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

