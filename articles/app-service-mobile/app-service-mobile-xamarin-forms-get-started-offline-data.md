---
title: Offlinesynchronisatie inschakelen voor uw Azure Mobile App (Xamarin.Forms) | Microsoft Docs
description: Informatie over het gebruik van App Service Mobile App naar de cache en synchroniseren offlinesynchronisatie van gegevens in uw Xamarin.Forms-toepassing
documentationcenter: xamarin
author: conceptdev
manager: yochayk
editor: ''
services: app-service\mobile
ms.assetid: acf0f874-3ea5-4410-bd22-b0e72140f3b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: crdun
ms.openlocfilehash: 506c59ca24aeafbac59b1508bb78142051302765
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "53001827"
---
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Offlinesynchronisatie inschakelen voor uw mobiele Xamarin.Forms-app
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie bevat de functie voor offlinesynchronisatie van Azure Mobile Apps voor Xamarin.Forms. Offlinesynchronisatie kunnen eindgebruikers om te communiceren met een mobiele app, weergeven, toevoegen of wijzigen van gegevens, zelfs als er geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe service.

In deze zelfstudie is gebaseerd op de Xamarin.Forms-snelstartgids-oplossing voor Mobile Apps die u maakt wanneer u de zelfstudie [een Xamarin iOS-app maken] voltooit. De Quick Start-oplossing voor Xamarin.Forms bevat de code voor de ondersteuning voor offlinesynchronisatie, die alleen moet worden ingeschakeld. In deze zelfstudie hebt bijwerken u de Quick Start-oplossing in te schakelen de offline functies van Azure Mobile Apps. Ook selecteren we de offline-specifieke code in de app. Als u het gedownloade Quick Start-oplossing niet gebruikt, moet u de data access-extensiepakketten toevoegen aan uw project. Zie voor meer informatie over het server-extensiepakketten [werken met de .NET back-endserver SDK voor Azure Mobile Apps][1].

Zie het onderwerp voor meer informatie over de functie voor offlinesynchronisatie, [Offline gegevenssynchronisatie in Azure Mobile Apps][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Schakel offlinesynchronisatie-functionaliteit in de Quick Start-oplossing
De offline synchronisatie-code is opgenomen in het project met behulp van C# preprocessor-instructies. Wanneer de **OFFLINE\_synchronisatie\_ingeschakeld** symbool is gedefinieerd, wordt deze codepaden zijn opgenomen in de build. Voor Windows-apps, moet u ook de SQLite-platform installeren.

1. In Visual Studio met de rechtermuisknop op de oplossing > **NuGet-pakketten beheren voor oplossing...** , zoek en installeer de **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-pakket voor alle projecten in de oplossing.
2. Open in Solution Explorer het bestand TodoItemManager.cs uit het project met **draagbare** vervolgens in de naam, die Portable Class Library-project, verwijder opmerkingen bij de volgende preprocessor-instructie:

        #define OFFLINE_SYNC_ENABLED
3. (Optioneel) Ter ondersteuning van Windows-apparaten, moet u een van de volgende pakketten van de SQLite-runtime installeren:

   * **Windows 8.1-Runtime:** installeren [SQLite voor Windows 8.1][3].
   * **Windows Phone 8.1:** installeren [SQLite voor Windows Phone 8.1][4].
   * **Universal Windows Platform** installeren [SQLite voor het Universal Windows Universal][5].

     Hoewel de Snelstartgids een universele Windows-project bevat, worden de universele Windows-platform wordt ondersteund met Xamarin Forms.
4. (Optioneel) In elke Windows-app-project met de rechtermuisknop op **verwijzingen** > **verwijzing toevoegen...** , vouw de **Windows** map > **extensies**.
    Schakel de juiste **SQLite voor Windows** SDK samen met de **Visual C++ 2013-Runtime voor Windows** SDK.
    De namen van de SQLite SDK verschillen met elke Windows-platform.

## <a name="review-the-client-sync-code"></a>Bekijk de code van de synchronisatie van client
Hier volgt een kort overzicht van wat is al opgenomen in de zelfstudie code binnen de `#if OFFLINE_SYNC_ENABLED` richtlijnen. De offline synchronisatie-functionaliteit is in het bestand TodoItemManager.cs project in het project draagbare klassenbibliotheek vormt. Zie voor een conceptueel overzicht van de functie [Offline gegevenssynchronisatie in Azure Mobile Apps][2].

* Voordat u een tabelbewerkingen kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd. De database van het lokale archief wordt geïnitialiseerd in het **TodoItemManager** klassenconstructor met behulp van de volgende code:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Deze code maakt een nieuwe lokale SQLite database met de **MobileServiceSQLiteStore** klasse.

    De **DefineTable** methode maakt u een tabel in het lokale archief dat overeenkomt met de velden in het opgegeven type.  Het type beschikt niet over te nemen van de kolommen die zich in de externe database. Het is mogelijk voor het opslaan van een subset van kolommen.
* De **todoTable** veld **TodoItemManager** is een **IMobileServiceSyncTable** typt u in plaats van **IMobileServiceTable**. Deze klasse maakt gebruik van de lokale database voor alle maken, lezen, bijwerken en verwijderen van de table-bewerkingen (CRUD). U besluit wanneer deze wijzigingen worden gepusht naar de back-end van de mobiele App door het aanroepen van **PushAsync** op de **IMobileServiceSyncContext**. De synchronisatie-context helpt u bij het behouden van relaties tussen tabellen bij te houden en pushen van wijzigingen in alle tabellen die een client-app is gewijzigd wanneer **PushAsync** wordt genoemd.

    De volgende **SyncAsync** methode wordt aangeroepen om te synchroniseren met de back-end van de mobiele App:

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

    In dit voorbeeld maakt gebruik van eenvoudige foutafhandeling met de standaard-handler voor synchronisatie. Een echte toepassing zou verwerken van de verschillende fouten, zoals netwerkomstandigheden en server veroorzaakt een conflict met een aangepaste **IMobileServiceSyncHandler** implementatie.

## <a name="offline-sync-considerations"></a>Overwegingen voor offlinesynchronisatie
In het voorbeeld de **SyncAsync** methode wordt alleen aangeroepen op opstarten en wanneer een synchronisatie is aangevraagd.  Als u wilt een synchronisatie in een Android- of iOS-app zich in de lijst met items; opgehaald voor Windows, gebruikt u de **synchronisatie** knop. In een echte toepassing kan u ook de synchronisatie-trigger maken wanneer de netwerk-status verandert.

Wanneer een pull wordt uitgevoerd op basis van een tabel die in behandeling heeft lokale updates bijgehouden door de context, die pull-bewerking automatisch triggers een vorige context-push. Bij het vernieuwen, toevoegen en het voltooien van items in dit voorbeeld kunt u weglaten de expliciete **PushAsync** aanroepen.

In de opgegeven code, alle records in de externe TodoItem-tabel worden opgevraagd, maar het is ook mogelijk om te filteren van records op te geven van een query-id en de query om toegang te **PushAsync**. Zie voor meer informatie de sectie *incrementele synchronisatie* in [Offline gegevenssynchronisatie in Azure Mobile Apps][2].

## <a name="run-the-client-app"></a>De client-app uitvoeren
Offline synchroniseren is nu ingeschakeld, voer de clienttoepassing uit ten minste één keer op elk platform voor het vullen van de lokale database. Later, simuleren van een offline-scenario en de gegevens in het lokale archief niet wijzigen terwijl de app offline is.

## <a name="update-the-sync-behavior-of-the-client-app"></a>Het gedrag van de synchronisatie van de client-app bijwerken
In deze sectie het clientproject voor het simuleren van een offline-scenario met behulp van een ongeldige toepassings-URL voor uw back-end te wijzigen. U kunt ook netwerkverbindingen uitschakelen door het verplaatsen van uw apparaat naar "Vliegtuigmodus."  Wanneer u toevoegen of wijzigen van gegevens, zijn deze wijzigingen ondergebracht in het lokale archief, maar niet is gesynchroniseerd met de back-end-gegevensarchief totdat de verbinding opnieuw tot stand gebracht.

1. Open in Solution Explorer het projectbestand Constants.cs uit de **draagbare** projecten en wijzig de waarde van `ApplicationURL` om te verwijzen naar een ongeldige URL:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. Open het bestand TodoItemManager.cs uit de **draagbare** project en voeg een **catch** voor de base **uitzondering** klasse aan de **try... catch**blokkeren **SyncAsync**. Dit **catch** blok de uitzonderingsbericht schrijft naar de console als volgt:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. Ontwikkel en voer de client-app.  Aantal nieuwe items toevoegen. U ziet dat er een uitzondering wordt geregistreerd in de console voor elke poging om te synchroniseren met de back-end. Deze nieuwe items bestaan alleen in het lokale archief totdat ze naar de mobiele back-end kunnen worden gepusht. De client-app gedraagt zich alsof het is verbonden met de back-end, ondersteunen die alle maken, lezen, bijwerken, (CRUD)-verwijderbewerkingen.
4. De app sluit en opnieuw starten om te controleren dat de nieuwe items die u hebt gemaakt naar het lokale archief worden opgeslagen.
5. (Optioneel) Visual Studio gebruiken om uw Azure SQL Database-tabel om te zien dat de gegevens in de back enddatabase niet is gewijzigd.

    Open in Visual Studio, **Server Explorer**. Navigeer naar uw database in **Azure**->**SQL-Databases**. Met de rechtermuisknop op uw database en selecteer **openen in SQL Server-Objectverkenner**. Nu kunt u bladeren naar de tabel van uw SQL-database en de inhoud ervan.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Update de client-app opnieuw verbinding maken met uw mobiele back-end
In deze sectie maakt opnieuw verbinding maken met de app naar de mobiele back-end die overeenkomt met de app die binnenkort terug om een online status. Als u het gebaar van de vernieuwing uitvoert, worden gegevens is gesynchroniseerd met uw mobiele back-end.

1. Open opnieuw Constants.cs. Corrigeer de `applicationURL` om te verwijzen naar de juiste URL.
2. Opnieuw opbouwen en uitvoeren van de client-app. De app probeert te synchroniseren met de mobiele app back-end na het starten. Controleer of dat er geen uitzonderingen worden geregistreerd in de console voor foutopsporing.
3. (Optioneel) De bijgewerkte gegevens met behulp van SQL Server Object Explorer of een REST-hulpprogramma zoals Fiddler bekijken of [Postman][6]. U ziet dat de gegevens is gesynchroniseerd tussen de back enddatabase en het lokale archief.

    U ziet dat de gegevens tussen de database en het lokale archief is gesynchroniseerd en bevat de items die u hebt toegevoegd, terwijl uw app is verbroken.

## <a name="additional-resources"></a>Aanvullende resources
* [Offline synchroniseren van gegevens in Azure Mobile Apps][2]
* [Azure Mobile Apps .NET SDK HOWTO][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: https://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: https://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: https://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: https://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
