---
title: Offlinesynchronisatie inschakelen voor uw Azure Mobile App (Cordova) | Microsoft Docs
description: Informatie over het gebruik van App Service Mobile App naar de cache en synchroniseren offlinesynchronisatie van gegevens in uw Cordova-toepassing
documentationcenter: cordova
author: conceptdev
manager: crdun
editor: ''
services: app-service\mobile
ms.assetid: 1a3f685d-f79d-4f8b-ae11-ff96e79e9de9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-cordova-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: 44c54b570a38eb1a3b9ca773893599d1d497dfa2
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52972147"
---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Offlinesynchronisatie inschakelen voor uw Cordova-app voor mobiel
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

In deze zelfstudie bevat de functie voor offlinesynchronisatie van Azure Mobile Apps voor Cordova. Offlinesynchronisatie kunnen eindgebruikers om te communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;, zelfs wanneer er geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database.  Zodra het apparaat weer online is, worden deze wijzigingen gesynchroniseerd met de externe service.

In deze zelfstudie is gebaseerd op de Cordova-snelstartgids-oplossing voor Mobile Apps die u maakt wanneer u de zelfstudie hebt voltooid [Snel starten met Apache Cordova]. In deze zelfstudie hebt bijwerken u de Quick Start-oplossing voor het toevoegen van de offline functies van Azure Mobile Apps.  Ook selecteren we de offline-specifieke code in de app.

Zie het onderwerp voor meer informatie over de functie voor offlinesynchronisatie, [Offlinesynchronisatie van gegevens in Azure Mobile Apps]. Zie voor meer informatie over het gebruik van API, de [API-documentatie](https://azure.github.io/azure-mobile-apps-js-client).

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Voeg offlinesynchronisatie toe aan de Quick Start-oplossing
De code offline synchronisatie moet worden toegevoegd aan de app. Offline synchronisatie is vereist voor de invoegtoepassing cordova-sqlite-opslag, die automatisch wordt toegevoegd aan uw app wanneer de Azure Mobile Apps-invoegtoepassing is opgenomen in het project. De Quick Start-project bevat beide van deze invoegtoepassingen.

1. Index.js open in Visual Studio Solution Explorer en vervang de volgende code

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

    De bovenstaande code toevoegingen initialiseren van het lokale archief en definiëren van een lokale tabel die overeenkomt met de kolomwaarden gebruikt in uw Azure-back-end. (U hoeft niet alle waarden van de kolom in deze code opnemen.)  De `version` veld wordt beheerd door de mobiele back-end en wordt gebruikt voor het oplossen van conflicten.

    Krijgt u een verwijzing naar de synchronisatie-context door aan te roepen **getSyncContext**. De synchronisatie-context helpt u bij het behouden van relaties tussen tabellen bij te houden en pushen van wijzigingen in alle tabellen die een client-app is gewijzigd wanneer `.push()` wordt genoemd.

3. De URL van de toepassing bijwerken naar de URL van uw mobiele App-toepassing.

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

    De bovenstaande code wordt de context van de synchronisatie wordt geïnitialiseerd en roept vervolgens getSyncTable (in plaats van een getTable) als u een verwijzing naar de lokale tabel.

    Deze code maakt gebruik van de lokale database voor alle maken, lezen, bijwerken en verwijderen van de table-bewerkingen (CRUD).

    In dit voorbeeld voert een eenvoudige foutafhandeling op synchronisatieconflicten. Een echte toepassing zou verwerken van de verschillende fouten zoals netwerkomstandigheden, server conflicten en anderen. Zie voor meer codevoorbeelden, de [offlinesynchronisatie voorbeeld].

5. Vervolgens voegt u deze functie om uit te voeren van de werkelijke synchronisatie toe.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    U bepalen wanneer u de wijzigingen naar de back-end van Mobile App forceren door het aanroepen van **syncContext.push()**. Bijvoorbeeld, u kunt aanroepen **syncBackend** in de gebeurtenis-handler van een knop die is gekoppeld aan een knop synchroniseren.

## <a name="offline-sync-considerations"></a>Overwegingen voor offlinesynchronisatie

In het voorbeeld de **push** -methode van **syncContext** alleen voor het starten van de app in de callbackfunctie voor de aanmelding wordt aangeroepen.  In een echte toepassing kan u ook deze synchronisatie-functionaliteit handmatig wordt geactiveerd of wanneer de netwerk-status wijzigingen aanbrengen.

Wanneer een pull wordt uitgevoerd op basis van een tabel die in behandeling heeft lokale updates bijgehouden door de context, die pull-bewerking automatisch triggers een push. Bij het vernieuwen, toevoegen en het voltooien van items in dit voorbeeld kunt u weglaten de expliciete **push** aanroepen, omdat dit overbodig kan zijn.

In de opgegeven code, alle records in de externe todoItem-tabel worden opgevraagd, maar het is ook mogelijk om te filteren van records op te geven van een query-id en de query om toegang te **push**. Zie voor meer informatie de sectie *incrementele synchronisatie* in [Offlinesynchronisatie van gegevens in Azure Mobile Apps].

## <a name="optional-disable-authentication"></a>(Optioneel) Verificatie uitschakelen

Als u niet wilt instellen van verificatie voordat testen offline synchronisatie, opmerkingen bij de callback-functie voor aanmelding, maar laat u de code binnen commentaarteken de callback-functie.  Na de opmerkingen bij de regels van de aanmelding, met de code de volgende:

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Nu de app wordt gesynchroniseerd met de Azure back-end wanneer u de app uitvoeren.

## <a name="run-the-client-app"></a>De client-app uitvoeren
Offline synchroniseren is nu ingeschakeld, kunt u de clienttoepassing ten minste één keer uitvoeren op elk platform voor het vullen van de lokale database. Later, simuleren van een offline-scenario en de gegevens in het lokale archief niet wijzigen terwijl de app offline is.

## <a name="optional-test-the-sync-behavior"></a>(Optioneel) Test de sync-gedrag
In deze sectie maakt wijzigen u het clientproject als u wilt simuleren van een offline-scenario met behulp van een ongeldige toepassings-URL voor uw back-end. Wanneer u toevoegen of wijzigen van gegevensitems, worden deze wijzigingen worden opgeslagen in het lokale archief, maar zijn niet gesynchroniseerd met de back-end-gegevensarchief, totdat de verbinding opnieuw tot stand gebracht.

1. Open het bestand index.js-project in Solution Explorer en wijzigen van de URL van de toepassing om te verwijzen naar een ongeldige URL, zoals de volgende code:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. Werk de CSP in index.html, `<meta>` element met dezelfde ongeldige URL.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Bouwen en uitvoeren van de client-app en u ziet dat er een uitzondering in de console wordt geregistreerd wanneer de app probeert te synchroniseren met de back-end na aanmelding. Alle nieuwe items die u toevoegt bestaan alleen in het lokale archief totdat ze worden gepusht naar de mobiele back-end. De client-app gedraagt zich alsof het is verbonden met de back-end.

4. De app sluit en opnieuw starten om te controleren dat de nieuwe items die u hebt gemaakt naar het lokale archief worden opgeslagen.

5. (Optioneel) Visual Studio gebruiken om uw Azure SQL Database-tabel om te zien dat de gegevens in de back enddatabase niet is gewijzigd.

    Open in Visual Studio, **Server Explorer**. Navigeer naar uw database in **Azure**->**SQL-Databases**. Met de rechtermuisknop op uw database en selecteer **openen in SQL Server-Objectverkenner**. Nu kunt u bladeren naar de tabel van uw SQL-database en de inhoud ervan.

## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Optioneel) Test de opnieuw verbinding maken met uw mobiele back-end

In deze sectie maakt u opnieuw verbinding maakt met de app de mobiele back-end die overeenkomt met de app die binnenkort terug om een online status. Wanneer u zich aanmeldt, worden gegevens is gesynchroniseerd met uw mobiele back-end.

1. Index.js openen en herstellen van de URL van de toepassing.
2. Open opnieuw index.html en corrigeer de URL van de toepassing in de CSP `<meta>` element.
3. Opnieuw opbouwen en uitvoeren van de client-app. De app probeert te synchroniseren met de mobiele app back-end na aanmelding. Controleer of dat er geen uitzonderingen worden geregistreerd in de console voor foutopsporing.
4. (Optioneel) De bijgewerkte gegevens met behulp van SQL Server Object Explorer of een REST-hulpprogramma zoals Fiddler bekijken. U ziet dat de gegevens is gesynchroniseerd tussen de back enddatabase en het lokale archief.

    U ziet dat de gegevens tussen de database en het lokale archief is gesynchroniseerd en bevat de items die u hebt toegevoegd, terwijl uw app is verbroken.

## <a name="additional-resources"></a>Aanvullende resources
* [Offlinesynchronisatie van gegevens in Azure Mobile Apps]
* [Visual Studio Tools for Apache Cordova]

## <a name="next-steps"></a>Volgende stappen
* Lees meer geavanceerde functies zoals conflictoplossing in offline synchronisatie de [offlinesynchronisatie voorbeeld]
* Bekijk de offlinesynchronisatie API-verwijzing in de [API-documentatie](https://azure.github.io/azure-mobile-apps-js-client).

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Snel starten met Apache Cordova]: app-service-mobile-cordova-get-started.md
[offlinesynchronisatie voorbeeld]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling
[Offlinesynchronisatie van gegevens in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[Cloud Cover: Offline Sync in Azure Mobile Services]: https://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: https://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
