---
title: Offlinesynchronisatie inschakelen voor uw mobiele Apps van Azure (Cordova) | Microsoft Docs
description: Informatie over het gebruik van App Service-mobiele App aan cache en sync offline gegevens in uw Cordova-toepassing
documentationcenter: cordova
author: conceptdev
manager: crdun
editor: 
services: app-service\mobile
ms.assetid: 1a3f685d-f79d-4f8b-ae11-ff96e79e9de9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-cordova-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: c12328a441a8cc438fa3e974863cc8adf8651b50
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Offlinesynchronisatie voor uw Cordova mobile app inschakelen
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Deze zelfstudie maakt u kennis met de functie offline synchroniseren van Azure Mobile Apps voor Cordova. Offlinesynchronisatie kunnen eindgebruikers werken met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs wanneer er geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database.  Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe service.

Deze zelfstudie is gebaseerd op de Cordova-Quick Start-oplossing voor Mobile Apps die u maakt wanneer u de zelfstudie hebt voltooid [snel starten van Apache Cordova]. In deze zelfstudie maakt bijwerken u de Quick Start-oplossing voor het toevoegen van offline functies van Azure Mobile Apps.  We markeren de offline-specifieke code in de app ook.

Zie het onderwerp voor meer informatie over de functie offlinesynchronisatie [Offline synchroniseren van gegevens in Azure Mobile Apps]. Zie voor meer informatie over het gebruik van de API van de [API-documentatie](https://azure.github.io/azure-mobile-apps-js-client).

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Offlinesynchronisatie toevoegen aan de Quick Start-oplossing
De code offlinesynchronisatie moet worden toegevoegd aan de app. Offline synchronisatie is vereist voor de invoegtoepassing cordova-sqlite-opslag, die automatisch wordt toegevoegd aan uw app wanneer de Azure Mobile Apps-invoegtoepassing is opgenomen in het project. De Quick Start-project bevat zowel van deze invoegtoepassingen.

1. Open index.js in Visual Studio Solution Explorer en vervang de volgende code

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable;      // Reference to a table endpoint on backend

    Met deze code:

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable,      // Reference to a table endpoint on backend
           syncContext;        // Reference to offline data sync context

2. Vervang vervolgens de volgende code:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    Met deze code:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              complete: 'boolean',
              version: 'string'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    De voorgaande code toevoegingen initialiseren van het lokale archief en definiëren van een lokale tabel die overeenkomt met de kolomwaarden gebruikt in uw Azure back-end. (U hoeft niet alle kolomwaarden wilt opnemen in deze code.)  De `version` veld wordt beheerd door de mobiele back-end en wordt gebruikt voor het oplossen van conflicten.

    Ontvangt u een verwijzing naar de synchronisatie-context door aan te roepen **getSyncContext**. De synchronisatie-context helpt tabelrelaties behouden door wijzigingen in alle tabellen die een client-app is gewijzigd wanneer worden gepusht en bij te houden `.push()` wordt aangeroepen.

3. De URL van de toepassing worden bijgewerkt naar de URL van uw mobiele App-toepassing.

4. Vervang vervolgens deze code:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    Met deze code:

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    De bovenstaande code initialiseert de synchronisatie-context en roept vervolgens getSyncTable (in plaats van een getTable) als u verwijst naar de lokale tabel.

    Deze code maakt gebruik van de lokale database voor alle maken, lezen, bijwerken en verwijderen (CRUD) tabel-bewerkingen.

    Dit voorbeeld voert eenvoudige foutafhandeling op synchronisatieconflicten. Een echte toepassing zou de verschillende fouten zoals netwerkomstandigheden, server conflicten en anderen verwerken. Zie voor codevoorbeelden van de [offlinesynchronisatie voorbeeld].

5. Vervolgens voegt u deze functie om uit te voeren van de werkelijke synchronisatie.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    U bepalen wanneer wijzigingen naar de back-end voor de mobiele App forceren door aan te roepen **syncContext.push()**. U kunt bijvoorbeeld aanroepen **syncBackend** in een knop gebeurtenis-handler gekoppeld aan een knop synchronisatie.

## <a name="offline-sync-considerations"></a>Offlinesynchronisatie overwegingen

In het voorbeeld de **push** methode van **syncContext** alleen wordt aangeroepen voor het starten van de app in de callback-functie voor aanmelding.  In een echte toepassing kan u de functionaliteit van deze synchronisatie handmatig geactiveerd of wanneer de netwerk-status is gewijzigd.

Wanneer een pull wordt uitgevoerd op basis van een tabel met wachtende lokale updates bijgehouden door de context die pull-bewerking automatisch triggers een push. Bij het vernieuwen, toe te voegen en het voltooien van de items in dit voorbeeld kunt u weglaten de expliciete **push** aanroepen, omdat deze is mogelijk overbodig.

In de opgegeven code alle records in de externe takentabel worden opgevraagd, maar het is ook mogelijk records filteren op het doorgeven van een query-id en opvragen voor **push**. Zie voor meer informatie de sectie *incrementele synchronisatie* in [Offline synchroniseren van gegevens in Azure Mobile Apps].

## <a name="optional-disable-authentication"></a>(Optioneel) Verificatie uit te schakelen

Als u niet wilt testen offlinesynchronisatie na verificatie instellen, uitcommentariëren de callback-functie voor aanmelding, maar laat u de code in de functie voor retouraanroepen zonder opmerkingen.  Na de aanmelding regels commentaarteken, volgt de code:

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Nu de app wordt gesynchroniseerd met de Azure back-end wanneer u de app uitvoeren.

## <a name="run-the-client-app"></a>De clientapp uitvoeren
Met offlinesynchronisatie is nu ingeschakeld, kunt u de clienttoepassing ten minste eenmaal uitvoeren voor elk platform voor het vullen van de lokale database. Later een offline-scenario te simuleren en de gegevens in het lokale archief niet wijzigen terwijl de app offline is.

## <a name="optional-test-the-sync-behavior"></a>(Optioneel) Het gedrag van de synchronisatie te testen
In deze sectie kunt u het clientproject om te simuleren van een offline-scenario met een ongeldige toepassings-URL voor uw back-end wijzigen. Wanneer u toevoegen of wijzigen van gegevensitems, worden deze wijzigingen in het lokale archief worden bewaard, maar niet is gesynchroniseerd met de back-end-gegevensopslag totdat de verbinding opnieuw tot stand gebracht.

1. Open het projectbestand index.js in Solution Explorer en wijzig de URL van de toepassing om te verwijzen naar een ongeldige URL, zoals de volgende code:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. Werk de CSP in index.html, `<meta>` element met dezelfde URL ongeldig.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Bouwen en uitvoeren van de client-app en u ziet dat er een uitzondering wordt geregistreerd in de console wanneer de app probeert te synchroniseren met de back-end na aanmelding. Geen nieuwe objecten die u toevoegt bestaan alleen in het lokale archief totdat ze naar de mobiele back-end worden gepusht. De clientapp gedraagt zich als deze is verbonden met de back-end.

4. De toepassing sluiten en opnieuw om te controleren dat de nieuwe items die u hebt gemaakt met het lokale archief worden doorgevoerd.

5. (Optioneel) Met Visual Studio kunt u uw Azure SQL Database-tabel om te zien dat de gegevens in de back-end-database niet is gewijzigd.

    Open in Visual Studio **Server Explorer**. Navigeer naar de database in **Azure**->**SQL-Databases**. Met de rechtermuisknop op de database en selecteer **openen in SQL Server Object Explorer**. Nu kunt u bladeren naar de tabel van uw SQL-database en de inhoud ervan.

## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Optioneel) Het opnieuw verbinden met uw mobiele back-end testen

In deze sectie kunt u de app naar de mobiele back-end die de app een online status terugkomst simuleert herstellen. Wanneer u zich aanmeldt, worden gegevens wordt gesynchroniseerd met uw mobiele back-end.

1. Open index.js en herstellen van de URL van de toepassing.
2. Open index.html en corrigeer de URL van de toepassing in de CSP `<meta>` element.
3. Bouwen en uitvoeren van de client-app. De app probeert te synchroniseren met de back-end voor de mobiele app na aanmelding. Controleer of dat er geen uitzonderingen worden geregistreerd in de console voor foutopsporing.
4. (Optioneel) De bijgewerkte gegevens met behulp van SQL Server Object Explorer of een REST-hulpprogramma zoals Fiddler weergeven. U ziet dat de gegevens tussen de back-end-database en het lokale archief is gesynchroniseerd.

    U ziet de gegevens tussen de database en het lokale archief is gesynchroniseerd en bevat de items die u hebt toegevoegd terwijl de verbinding van uw app is verbroken.

## <a name="additional-resources"></a>Aanvullende resources
* [Offline synchroniseren van gegevens in Azure Mobile Apps]
* [Visual Studio Tools for Apache Cordova]

## <a name="next-steps"></a>Volgende stappen
* Bekijk meer geavanceerde functies zoals conflictoplossing in offlinesynchronisatie de [offlinesynchronisatie voorbeeld]
* Bekijk de offlinesynchronisatie API-verwijzing in de [API-documentatie](https://azure.github.io/azure-mobile-apps-js-client).

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[snel starten van Apache Cordova]: app-service-mobile-cordova-get-started.md
[offlinesynchronisatie voorbeeld]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling
[Offline synchroniseren van gegevens in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
