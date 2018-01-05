---
title: Offlinesynchronisatie inschakelen voor uw mobiele Apps van Azure (Xamarin.Forms) | Microsoft Docs
description: Informatie over het gebruik van App Service-mobiele App aan cache en sync offline gegevens in uw toepassing Xamarin.Forms
documentationcenter: xamarin
author: conceptdev
manager: yochayk
editor: 
services: app-service\mobile
ms.assetid: acf0f874-3ea5-4410-bd22-b0e72140f3b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: crdun
ms.openlocfilehash: f88e6a4037bcca54982359742cdc6021f020882d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Offlinesynchronisatie inschakelen voor uw mobiele Xamarin.Forms-app
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie maakt u kennis met de functie offline synchroniseren van Azure Mobile Apps voor Xamarin.Forms. Offlinesynchronisatie kan eindgebruikers werken met een mobiele app--weergeven, toevoegen of wijzigen van gegevens--, zelfs wanneer er geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe service.

Deze zelfstudie is gebaseerd op de Xamarin.Forms-Quick Start-oplossing voor Mobile Apps die u maakt wanneer u de zelfstudie [een Xamarin iOS-app maken] voltooit. De Quick Start-oplossing voor Xamarin.Forms bevat de code ter ondersteuning van offlinesynchronisatie die moet worden ingeschakeld. In deze zelfstudie maakt bijwerken u de Quick Start-oplossing de offline functies van Azure Mobile Apps inschakelen. We markeren de offline-specifieke code in de app ook. Als u de gedownloade Quick Start-oplossing niet gebruikt, moet u de data access-extensiepakketten toevoegen aan uw project. Zie voor meer informatie over server extensiepakketten [werken met de .NET-back-endserver SDK voor Azure Mobile Apps][1].

Zie het onderwerp voor meer informatie over de functie offlinesynchronisatie [Offline synchroniseren van gegevens in Azure Mobile Apps][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Schakel offlinesynchronisatie-functionaliteit in de Quick Start-oplossing
De code offlinesynchronisatie is opgenomen in het project met behulp van C# preprocessor-instructies. Wanneer de **OFFLINE\_SYNC\_ingeschakeld** symbool is gedefinieerd, wordt deze codepaden zijn opgenomen in de build. Voor Windows-apps, moet u ook de SQLite-platform installeren.

1. Met de rechtermuisknop op de oplossing in Visual Studio > **NuGet-pakketten beheren voor oplossing...** , zoek vervolgens naar en installeren de **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-pakket voor alle projecten in de oplossing.
2. Open in Solution Explorer het TodoItemManager.cs-bestand uit het project met **draagbare** in de naam die Portable Class Library-project, klikt u vervolgens Opmerkingen bij de volgende preprocessor-instructie:

        #define OFFLINE_SYNC_ENABLED
3. (Optioneel) Ter ondersteuning van Windows-apparaten, een van de volgende SQLite-runtime-pakketten te installeren:

   * **Windows 8.1 Runtime:** installeren [SQLite voor Windows 8.1][3].
   * **Windows Phone 8.1:** installeren [SQLite voor Windows Phone 8.1][4].
   * **Universal Windows Platform** installeren [SQLite voor de Universal Windows universele][5].

     Hoewel de Quick Start geen een universele Windows-project bevat, wordt het universele Windows-platform ondersteund met Xamarin Forms.
4. (Optioneel) Elke Windows-app-project met de rechtermuisknop op **verwijzingen** > **verwijzing toevoegen...** , vouw de **Windows** map > **extensies**.
    Schakel de juiste **SQLite voor Windows** SDK samen met de **Visual C++ 2013-Runtime voor Windows** SDK.
    De namen van SQLite SDK verschillen met elke Windows-platform.

## <a name="review-the-client-sync-code"></a>Bekijk de clientcode voor synchronisatie
Hier volgt een kort overzicht van wat is al opgenomen in de zelfstudie code binnen de `#if OFFLINE_SYNC_ENABLED` richtlijnen. De offlinesynchronisatie-functionaliteit is in het projectbestand TodoItemManager.cs in het project Portable Class Library. Zie voor een conceptueel overzicht van de functie [Offline synchroniseren van gegevens in Azure Mobile Apps][2].

* Voordat u tabelbewerkingen kunnen uitvoeren, moet het lokale archief worden geïnitialiseerd. De database van het lokale archief is geïnitialiseerd de **TodoItemManager** klassen-constructor met de volgende code:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Deze code maakt een nieuwe lokale SQLite database met de **MobileServiceSQLiteStore** klasse.

    De **DefineTable** methode maakt een tabel in het lokale archief die overeenkomt met de velden in het opgegeven type.  Het type heeft geen om op te nemen van de kolommen die zich in de externe database. Het is mogelijk voor het opslaan van een subset kolommen.
* De **todoTable** veld **TodoItemManager** is een **IMobileServiceSyncTable** typt u in plaats van **IMobileServiceTable**. Deze klasse maakt gebruik van de lokale database voor alle maken, lezen, bijwerken en verwijderen (CRUD) tabel-bewerkingen. U besluit wanneer deze wijzigingen worden gepusht back-end van de mobiele App door aan te roepen **PushAsync** op de **IMobileServiceSyncContext**. De synchronisatie-context helpt tabelrelaties behouden door wijzigingen in alle tabellen die een client-app is gewijzigd wanneer worden gepusht en bij te houden **PushAsync** wordt aangeroepen.

    De volgende **SyncAsync** methode wordt aangeroepen om te synchroniseren met de back-end voor de mobiele App:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    Dit voorbeeld maakt gebruik van eenvoudige foutafhandeling met de standaard synchronisatie-handler. Het afhandelen van een echte toepassing zou de verschillende fouten zoals netwerkomstandigheden en server conflicten met behulp van een aangepaste **IMobileServiceSyncHandler** implementatie.

## <a name="offline-sync-considerations"></a>Offlinesynchronisatie overwegingen
In het voorbeeld de **SyncAsync** methode alleen wordt aangeroepen voor opstarten en waarop een synchronisatie wordt aangevraagd.  Voor het initiëren van een synchronisatie in een Android- of iOS-app trek omlaag in de lijst met items; voor Windows, gebruikt de **Sync** knop. In een echte toepassing kan u de trigger sync wanneer de status van het netwerk wordt gewijzigd.

Wanneer een pull wordt uitgevoerd op basis van een tabel met wachtende lokale updates bijgehouden door de context die pull-bewerking automatisch triggers een voorgaande context push. Bij het vernieuwen, toe te voegen en het voltooien van de items in dit voorbeeld kunt u weglaten de expliciete **PushAsync** aanroepen.

In de opgegeven code alle records in de externe takentabel worden opgevraagd, maar het is ook mogelijk records filteren op het doorgeven van een query-id en opvragen voor **PushAsync**. Zie voor meer informatie de sectie *incrementele synchronisatie* in [Offline synchroniseren van gegevens in Azure Mobile Apps][2].

## <a name="run-the-client-app"></a>De clientapp uitvoeren
Met offlinesynchronisatie is nu ingeschakeld, uitgevoerd bij de clienttoepassing ten minste eenmaal voor elk platform voor het vullen van de lokale database. Later een offline-scenario te simuleren en de gegevens in het lokale archief niet wijzigen terwijl de app offline is.

## <a name="update-the-sync-behavior-of-the-client-app"></a>Het gedrag van de synchronisatie van de client-app bijwerken
In deze sectie wijzigt het clientproject te simuleren van een offline-scenario met een ongeldige toepassings-URL voor uw back-end. U kunt ook netwerkverbindingen uitschakelen door te verplaatsen van het apparaat naar 'Vliegtuigmodus'.  Wanneer u toevoegen of wijzigen van gegevensitems, zijn deze wijzigingen ondergebracht in het lokale archief, maar niet gesynchroniseerd met de back-end-gegevensopslag totdat de verbinding opnieuw tot stand gebracht.

1. Open het projectbestand Constants.cs uit in de Solution Explorer de **draagbare** project en wijzig de waarde van `ApplicationURL` om te verwijzen naar een ongeldige URL:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. Open het bestand TodoItemManager.cs uit de **draagbare** project en voeg een **catch** voor de base **uitzondering** klasse naar de **try... catch**blokkeren in **SyncAsync**. Dit **catch** blok het uitzonderingsbericht schrijft naar de console als volgt:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. Ontwikkel en voer de client-app.  Aantal nieuwe items toevoegen. U ziet dat er een uitzondering wordt geregistreerd in de console voor elke poging om te synchroniseren met de back-end. Deze nieuwe items bestaan alleen in het lokale archief totdat ze naar de mobiele back-end kunnen worden geactiveerd. De clientapp gedraagt zich als deze is verbonden met de back-end, ondersteunen die alle maken, lezen, update, delete (CRUD)-bewerkingen.
4. De toepassing sluiten en opnieuw om te controleren dat de nieuwe items die u hebt gemaakt met het lokale archief worden doorgevoerd.
5. (Optioneel) Met Visual Studio kunt u uw Azure SQL Database-tabel om te zien dat de gegevens in de back-end-database niet is gewijzigd.

    Open in Visual Studio **Server Explorer**. Navigeer naar de database in **Azure**->**SQL-Databases**. Met de rechtermuisknop op de database en selecteer **openen in SQL Server Object Explorer**. Nu kunt u bladeren naar de tabel van uw SQL-database en de inhoud ervan.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Update de client-app opnieuw verbinding maken met uw mobiele back-end
In deze sectie opnieuw verbinding worden de mobiele back-end die de app een online status terugkomst simuleert met de app. Wanneer u de gebaar vernieuwen uitvoert, worden gegevens wordt gesynchroniseerd met uw mobiele back-end.

1. Open Constants.cs. Corrigeer de `applicationURL` om te verwijzen naar de juiste URL.
2. Bouwen en uitvoeren van de client-app. De app probeert te synchroniseren met de back-end voor de mobiele app nadat u hebt. Controleer of dat er geen uitzonderingen worden geregistreerd in de console voor foutopsporing.
3. (Optioneel) De bijgewerkte gegevens met behulp van SQL Server Object Explorer of een REST-hulpprogramma zoals Fiddler weergeven of [Postman][6]. U ziet dat de gegevens tussen de back-end-database en het lokale archief is gesynchroniseerd.

    U ziet de gegevens tussen de database en het lokale archief is gesynchroniseerd en bevat de items die u hebt toegevoegd terwijl de verbinding van uw app is verbroken.

## <a name="additional-resources"></a>Aanvullende resources
* [Offline synchroniseren van gegevens in Azure Mobile Apps][2]
* [Azure Mobile Apps .NET SDK procedure][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
