---
title: Werken met de Server SDK voor Node.js back-end voor Mobile Apps | Microsoft Docs
description: Informatie over het werken met de Server SDK voor Node.js back-end voor Azure App Service Mobile Apps.
services: app-service\mobile
documentationcenter: 
author: elamalani
manager: elamalani
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: bd423d6fb62b2ace16832f665c8834b4aea7e26f
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-use-the-mobile-apps-nodejs-sdk"></a>Het gebruik van de Mobile Apps Node.js SDK
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

In dit artikel biedt gedetailleerde informatie en voorbeelden van het werken met een Node.js back-end in de functie Mobile Apps van Azure App Service.

## <a name="Introduction"></a>Inleiding
Mobiele Apps biedt de mogelijkheid een gegevens geoptimaliseerd voor mobiele toegang tot Web-API toevoegen aan een webtoepassing. De Mobile Apps SDK is beschikbaar voor ASP.NET- en Node.js-webtoepassingen. De SDK biedt de volgende bewerkingen:

* Tabel operations (lezen, invoegen, bijwerken en verwijderen) voor toegang tot gegevens
* Aangepaste API-bewerkingen

Beide bewerkingen kunnen worden gebruikt voor verificatie binnen alle id-providers waarmee Azure App Service. Deze providers bevatten sociale id-providers zoals Facebook, Twitter, Google, en Microsoft, evenals Azure Active Directory voor de identiteit van de onderneming.

Vindt u voorbeelden voor elk geval gebruik in de [directory voorbeelden op GitHub].

## <a name="supported-platforms"></a>Ondersteunde platforms
De Mobile Apps Node.js SDK biedt ondersteuning voor de huidige release TNS van knooppunt en hoger. De meest recente versie van de TNS is momenteel knooppunt v4.5.0. Andere versies van knooppunt werkt mogelijk, maar worden niet ondersteund.

De Mobile Apps Node.js SDK ondersteunt twee databasestuurprogramma: 

* Het knooppunt mssql-stuurprogramma ondersteunt Azure SQL Database en lokale exemplaren van SQL Server.  
* Het stuurprogramma sqlite3 ondersteunt SQLite-databases in één exemplaar.

### <a name="howto-cmdline-basicapp"></a>Een eenvoudige back-end voor Node.js maken met behulp van de opdrachtregel
Elke back-end van Mobile Apps Node.js wordt gestart als een toepassing ExpressJS. ExpressJS is het meest populaire web service-framework beschikbaar voor Node.js. U kunt een eenvoudige maken [Express] toepassing als volgt:

1. Maak een map voor uw project in een opdracht of een PowerShell-venster:

        mkdir basicapp
2. Voer `npm init` initialiseren van de pakketstructuur:

        cd basicapp
        npm init

   De `npm init` opdracht wordt gevraagd een reeks vragen initialiseren van het project. Zie het voorbeeld van uitvoer:

   ![De npm-init-uitvoer][0]
3. Installeer de `express` en `azure-mobile-apps` bibliotheken van de npm-opslagplaats:

        npm install --save express azure-mobile-apps
4. Maak een bestand app.js voor het implementeren van de basic mobiele server:

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table.
        mobile.tables.add('TodoItem');

        // Add the Mobile API so it is accessible as a Web API.
        app.use(mobile);

        // Start listening on HTTP.
        app.listen(process.env.PORT || 3000);

Deze toepassing maakt een geoptimaliseerd voor mobiele Web-API met één eindpunt (`/tables/TodoItem`) die niet-geverifieerde toegang biedt tot onderliggende SQL gegevensopslag met behulp van een dynamische schema. Het is geschikt is voor de client-bibliotheek snelstartgidsen te volgen:

* [Android client Quick Start]
* [Quick Start Apache Cordova-client]
* [iOS Client Quick Start]
* [Windows Store-client Quick Start]
* [Snelstartgids voor Xamarin.iOS-client]
* [Snelstartgids voor Xamarin.Android-client]
* [Quick Start Xamarin.Forms-client]

U vindt de code voor deze eenvoudige toepassing in de [basicapp voorbeeld op GitHub].

### <a name="howto-vs2015-basicapp"></a>Een back-end voor Node.js maken met behulp van Visual Studio 2015
Visual Studio 2015 is een uitbreiding voor het ontwikkelen van Node.js-toepassingen IDE vereist. Als u wilt starten, installeer de [1.1 van Node.js-hulpprogramma's voor Visual Studio]. Wanneer u de installatie hebt voltooid, moet u een snelle 4.x-toepassing maken:

1. Open de **nieuw Project** in het dialoogvenster (van **bestand** > **nieuw** > **Project**).
2. Vouw **sjablonen** > **JavaScript** > **Node.js**.
3. Selecteer **Basic Azure Node.js Express 4-toepassing**.
4. Vul in de naam van het project. Selecteer **OK**.

   ![Visual Studio 2015 nieuw project][1]
5. Met de rechtermuisknop op de **npm** uit en selecteer **installeren nieuwe npm pakketten**.
6. Mogelijk moet u de catalogus npm vernieuwen nadat u uw eerste Node.js-toepassing maken. Selecteer **vernieuwen** indien nodig.
7. Voer **apps van azure mobile** in het zoekvak. Selecteer de **azure mobile apps 2.0.0** van het pakket en selecteert u vervolgens **pakket installeren**.

   ![Nieuwe npm pakketten installeren][2]
8. Selecteer **sluiten**.
9. Open het bestand app.js om ondersteuning voor de Mobile Apps SDK. AT regel 6 at onder aan de bibliotheek `require` instructies, voeg de volgende code:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

   27 AT ongeveer regel achter elkaar `app.use` instructies, voeg de volgende code:

        app.use('/users', users);

        // Mobile Apps initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

   Sla het bestand op.
10. Voer de toepassing lokaal (de API wordt geleverd op http://localhost: 3000) of publiceren naar Azure.

### <a name="create-node-backend-portal"></a>Een back-end voor Node.js maken met behulp van de Azure-portal
U kunt een Mobile Apps back-end rechts maken in de [Azure-portal]. U kunt de volgende stappen uit of een client en server samen te maken door de [maken van een mobiele app](app-service-mobile-ios-get-started.md) zelfstudie. De zelfstudie bevat een vereenvoudigde versie van deze instructies en wordt aanbevolen voor bewijs van concept projecten.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Terug in de **aan de slag** deelvenster onder **maken van een tabel-API**, kies **Node.js** als uw back-end-taal.
Selecteer het selectievakje uit voor **ik erken dat hiermee alle site-inhoud wordt overschreven**, en selecteer vervolgens **takentabel maken**.

### <a name="download-quickstart"></a>Het CodeProject voor Node.js-back-end-snelstartgids downloaden met behulp van Git
Wanneer u een back-end voor Node.js Mobile Apps maakt met behulp van de portal **snel starten** deelvenster een Node.js-project wordt voor u gemaakt en geïmplementeerd voor uw site. U kunt in de portal, tabellen en API's toevoegen en bewerken van codebestanden voor de back-end voor Node.js. U kunt ook verschillende implementatieprogramma's gebruiken voor het downloaden van het back-end-project, zodat u kunt toevoegen of tabellen en API's wijzigen en vervolgens het project opnieuw te publiceren. Zie voor meer informatie de [Implementatiehandleiding voor Azure App Service]. 

De volgende procedure maakt gebruik van een Git-opslagplaats voor het downloaden van de Quick Start-projectcode:

1. Installeer Git als u dat nog niet hebt gedaan. De stappen die nodig zijn voor het installeren van Git variëren tussen besturingssystemen. Zie voor specifieke besturingssysteem-distributies en installatierichtlijnen [installeren Git](http://git-scm.com/book/en/Getting-Started-Installing-Git).
2. Volg de stappen in [inschakelen van de App Service-app-opslagplaats](../app-service/app-service-deploy-local-git.md#Step3) zodat de Git-opslagplaats voor uw back-end-site. Maak een notitie van implementatie-gebruikersnaam en wachtwoord.
3. In het deelvenster voor mobiele Apps van uw back-end, noteert u de **Git-kloon-URL** instelling.
4. Uitvoeren van de `git clone` opdracht met behulp van de Git-kloon-URL. Voer uw wachtwoord indien nodig, zoals in het volgende voorbeeld:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. Blader naar de lokale map (`/todolist` in het voorgaande voorbeeld), en u ziet dat project-bestanden zijn gedownload. Zoek het bestand todoitem.json in de `/tables` directory. Dit bestand definieert machtigingen voor de tabel. Ook het bestand todoitem.js niet vinden in dezelfde map. Hiermee definieert u de scripts CRUD bewerking voor de tabel.
6. Nadat u wijzigingen in de project-bestanden aanbrengt, voer de volgende opdrachten toe te voegen, doorvoeren en de wijzigingen vervolgens uploaden naar de site:

        $ git commit -m "updated the table script"
        $ git push origin master

   Wanneer u nieuwe bestanden aan het project toevoegt, moet u eerst om uit te voeren de `git add .` opdracht.

Telkens wanneer een nieuwe reeks doorvoeracties wordt doorgegeven voor de site opnieuw de site wordt gepubliceerd.

### <a name="howto-publish-to-azure"></a>Uw back-end voor Node.js publiceren naar Azure
Microsoft Azure biedt veel mechanismen voor het publiceren van uw mobiele Apps Node.js back-end met de Azure-service. Deze mechanismen zijn geïntegreerd in Visual Studio-hulpprogramma's voor opdrachtregelprogramma's en continue implementatie-opties op basis van broncodebeheer. Zie voor meer informatie de [Implementatiehandleiding voor Azure App Service].

Azure App Service biedt specifiek advies voor Node.js-toepassingen die u moet controleren voordat u de back-end publiceren:

* Hoe [Geef de versie van het knooppunt]
* Hoe [knooppunt-modules gebruiken]

### <a name="howto-enable-homepage"></a>Een startpagina van uw toepassing inschakelen
Veel toepassingen zijn een combinatie van web- en mobiele apps. Het framework ExpressJS kunt u de twee facetten combineren. Soms is echter mogelijk u alleen een mobiele-interface implementeren. Dit is handig voor het bieden van een startpagina om ervoor te zorgen dat de app-service is en wordt uitgevoerd. Geef uw eigen introductiepagina of de startpagina van een tijdelijke inschakelen. Gebruik de volgende code instantiëren Mobile Apps zodat de startpagina van een tijdelijke:

    var mobile = azureMobileApps({ homePage: true });

Als u deze optie beschikbaar wilt bij het ontwikkelen van lokaal, kunt u deze instelling kunt toevoegen aan het bestand azureMobile.js.

## <a name="TableOperations"></a>Tabelbewerkingen
De apps van azure mobile Node.js-SDK-Server biedt mechanismen om gegevenstabellen die zijn opgeslagen in Azure SQL Database als een Web-API weer te geven. Bevat vijf operations:

| Bewerking | Beschrijving |
| --- | --- |
| GET-/tables/*tablename* |Ophalen van alle records in de tabel. |
| GET-/tables/*tablename*/:id |Ophalen van een bepaalde record in de tabel. |
| POST /tables/*tablename* |Maak een record in de tabel. |
| PATCH /tables/*tablename*/:id |Een record in de tabel niet bijwerken. |
| DELETE /tables/*tablename*/:id |Een record in de tabel verwijderen. |

Deze Web-API ondersteunt [OData] en breidt het tabelschema ter ondersteuning van [offline gegevenssynchronisatie].

### <a name="howto-dynamicschema"></a>Tabellen met behulp van een dynamische schema definiëren
Voordat u een tabel gebruiken kunt, moet u deze definiëren. U kunt de tabellen definiëren met behulp van een statische schema (waar u de kolommen definiëren in het schema) of dynamisch (indien de SDK het schema op basis van binnenkomende aanvragen bepaalt). Bovendien kunt u bepaalde aspecten van de Web-API beheren door JavaScript-code toe te voegen aan de definitie.

Als een best practice moet u elke tabel definiëren in een JavaScript-bestand in de `tables` directory en gebruik vervolgens de `tables.import()` methode om de tabellen te importeren. Uitbreiden van het basic-app-voorbeeld zou u het bestand app.js aanpassen:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed.
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined.
    mobile.tables.initialize().then(function () {
        // Add the Mobile API so it is accessible as a Web API.
        app.use(mobile);

        // Start listening on HTTP.
        app.listen(process.env.PORT || 3000);
    });

Definieer de tabel in. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here.

    module.exports = table;

Tabellen wordt standaard een dynamische schema gebruikt. Als u globaal uitschakelen het dynamische schema, stel de `MS_DynamicSchema` app-instelling in op false in de Azure portal.

U vindt een compleet voorbeeld in de [todo-voorbeeld op GitHub].

### <a name="howto-staticschema"></a>Tabellen met behulp van een statische schema definiëren
U kunt de kolommen te kunnen stellen via de Web-API expliciet definiëren. De Node.js-SDK voor apps van azure mobile voegt automatisch eventuele extra kolommen die zijn vereist voor het offline synchroniseren van gegevens aan de lijst die u opgeeft. De Quick Start-clienttoepassingen vereisen bijvoorbeeld dat een tabel met twee kolommen: `text` (een tekenreeks) en `complete` (een Booleaanse waarde).  
De tabel kan worden gedefinieerd in de tabel definitie JavaScript-bestand (zich in de `tables` directory) als volgt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table.
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

    module.exports = table;

Als u tabellen statisch definieert, moet u ook aanroepen de `tables.initialize()` methode voor het maken van het databaseschema bij het opstarten. De `tables.initialize()` methode retourneert een [belofte] zodat de webservice geen aanvragen verwerken heeft voordat de database is geïnitialiseerd.

### <a name="howto-sqlexpress-setup"></a>SQL Server Express gebruiken als gegevensopslag ontwikkeling op uw lokale computer
De Mobile Apps Node.js SDK biedt drie opties voor die gegevens buiten het vak:

* Gebruik de **geheugen** -stuurprogramma voor een voorbeeld van de niet-permanente opslag.
* Gebruik de **mssql** -stuurprogramma voor SQL Server Express gegevensopslag voor ontwikkeling.
* Gebruik de **mssql** -stuurprogramma voor een Azure SQL Database-gegevensarchief voor productie.

De Mobile Apps Node.js SDK gebruikt de [mssql Node.js pakket] voor het maken van een verbinding met SQL Server Express en SQL-Database gebruiken. Dit pakket vereist dat u TCP-verbindingen op uw exemplaar van SQL Server Express inschakelen.

> [!TIP]
> Het stuurprogramma geheugen biedt geen een volledige set van opslagruimten voor het testen. Als u uw back-end lokaal testen wilt, raden wij het gebruik van een SQL Server Express-gegevensarchief en de mssql-stuurprogramma.
>
>

1. Download en installeer [Microsoft SQL Server 2014 Express]. Zorg ervoor dat u de SQL Server 2014 Express met hulpprogramma's-editie installeert. Tenzij u expliciet ondersteuning voor 64-bits vereist, verbruikt de 32-bits versie minder geheugen uitgevoerd.
2. Run SQL Server 2014 Configuration Manager:

   a. Vouw de **SQL Server-netwerkconfiguratie** knooppunt in het menu structuur.

   b. Selecteer **protocollen voor SQLEXPRESS**.

   c. Met de rechtermuisknop op **TCP/IP** en selecteer **inschakelen**. Selecteer **OK** in het pop-updialoogvenster.

   d. Met de rechtermuisknop op **TCP/IP** en selecteer **eigenschappen**.

   e. Selecteer de **IP-adressen** tabblad.

   f. Zoek de **IPAll** knooppunt. In de **TCP-poort** veld **1433**.

      ![SQL Server Express configureren voor de TCP/IP][3]

   g. Selecteer **OK**. Selecteer **OK** in het pop-updialoogvenster.

   h. Selecteer **SQL Server-Services** in het menu structuur.

   i. Met de rechtermuisknop op **SQL Server (SQLEXPRESS)** en selecteer **opnieuw**.

   j. Close SQL Server 2014 Configuration Manager.
3. Uitvoeren van SQL Server 2014 Management Studio en maak verbinding met uw lokale exemplaar van SQL Server Express:

   1. Met de rechtermuisknop op uw exemplaar in Object Explorer en selecteer **eigenschappen**.
   2. Selecteer de **beveiliging** pagina.
   3. Zorg ervoor dat **modus van SQL Server en Windows-verificatie** is geselecteerd.
   4. Selecteer **OK**.

      ![SQL Server Express-verificatie configureren][4]
   5. Vouw **beveiliging** > **aanmeldingen** in Object Explorer.
   6. Met de rechtermuisknop op **aanmeldingen** en selecteer **New Login**.
   7. Typ een aanmeldingsnaam. Selecteer **SQL Server-verificatie**. Geef een wachtwoord op en voer het wachtwoord in **wachtwoord bevestigen**. Het wachtwoord moet voldoen aan de complexiteitsvereisten van Windows.
   8. Selecteer **OK**.

      ![Een nieuwe gebruiker toevoegen aan SQL Server Express][5]
   9. Met de rechtermuisknop op uw nieuwe aanmelding en selecteer **eigenschappen**.
   10. Selecteer de **serverfuncties** pagina.
   11. Schakel het selectievakje in voor de **dbcreator** serverfunctie.
   12. Selecteer **OK**.
   13. Close SQL Server 2015 Management Studio.

Zorg ervoor dat de gebruikersnaam en wachtwoord die u hebt geselecteerd. Mogelijk moet u aanvullende serverfuncties of machtigingen, afhankelijk van uw database toewijzen.

De Node.js-toepassing leest de `SQLCONNSTR_MS_TableConnectionString` omgevingsvariabele voor de verbindingsreeks voor deze database. U kunt deze variabele instellen in uw omgeving. U kunt bijvoorbeeld PowerShell gebruiken voor het instellen van deze omgevingsvariabele:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Toegang tot de database via een TCP/IP-verbinding. Geef een gebruikersnaam en wachtwoord voor de verbinding.

### <a name="howto-config-localdev"></a>Uw project voor lokale ontwikkeling configureren
Mobile Apps leest een JavaScript-bestand aangeroepen *azureMobile.js* uit het lokale bestandssysteem. Gebruik dit bestand niet voor het configureren van de Mobile Apps SDK in de productieomgeving. Gebruik in plaats daarvan **appinstellingen** in de [Azure-portal]. 

Het bestand azureMobile.js moet een configuratieobject exporteren. De meest voorkomende instellingen zijn:

* Database-instellingen
* Instellingen voor diagnostische logboekregistratie
* Alternatieve CORS-instellingen

Dit voorbeeld azureMobile.js bestand implementeert de voorgaande database-instellingen:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Het is raadzaam dat u azureMobile.js aan uw bestand .gitignore toevoegen (of andere broncodebeheer bestand negeren) om te voorkomen dat wachtwoorden worden opgeslagen in de cloud. Configureer altijd productie-instellingen in **appinstellingen** binnen de [Azure-portal].

### <a name="howto-appsettings"></a>Appinstellingen voor uw mobiele app configureren
De meeste instellingen in het bestand azureMobile.js hebben een equivalent app-instelling in de [Azure-portal]. Gebruik de volgende lijst voor het configureren van uw app in **appinstellingen**:

| App-instelling | azureMobile.js instelling | Beschrijving | Geldige waarden |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |naam |Naam van de app |tekenreeks |
| **MS_MobileLoggingLevel** |logging.level |Minimale logboekniveau van berichten aan te melden |fout, waarschuwing, info, uitgebreid, foutopsporing, stom |
| **MS_DebugMode** |fouten opsporen |Hiermee schakelt foutopsporingsmodus of |True, false |
| **MS_TableSchema** |data.schema |Naam van de standaard schema voor de SQL-tabellen |tekenreeks (standaard: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Hiermee schakelt foutopsporingsmodus of |True, false |
| **MS_DisableVersionHeader** |versie (ingesteld op niet-gedefinieerde) |Schakelt de header X-ZUMO-Server-versie |True, false |
| **MS_SkipVersionCheck** |skipversioncheck |Hiermee schakelt de controle van de client-API-versie |True, false |

Instellen van een app-instelling:

1. Meld u aan bij de [Azure-portal].
2. Selecteer **alle resources** of **App Services**, en selecteer vervolgens de naam van uw mobiele app.
3. De **instellingen** deelvenster standaard wordt geopend. Als dit niet zo is, selecteert **instellingen**.
4. Op de **algemene** selecteert u **toepassingsinstellingen**.
5. Schuif naar de **appinstellingen** sectie.
6. Als uw app instelt, al bestaat, selecteert u de waarde van de app-instelling om de waarde te bewerken.
   Als uw app-instelling niet bestaat, voert u de app-instelling in de **sleutel** vak en de waarde in de **waarde** vak.
8. Selecteer **Opslaan**.

Het wijzigen van de meeste appinstellingen voor vereist service opnieuw opstarten.

### <a name="howto-use-sqlazure"></a>SQL-Database gebruiken als uw productiegegevens opslaan
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Met behulp van Azure SQL Database als gegevensopslag is identiek alle typen voor Azure App Service-toepassing. Als u nog niet gedaan hebt, volgt u deze stappen voor het maken van een back-end van Mobile Apps:

1. Meld u aan bij de [Azure-portal].
2. Selecteer in de linkerbovenhoek van het venster de **+ nieuw** knop > **Web en mobiel** > **mobiele App**, en geef vervolgens een naam voor uw mobiele Apps van back-end.
3. In de **resourcegroep** Voer dezelfde naam als uw app.
4. De standaardwaarde van App Service-abonnement is geselecteerd. Als u uw App Service-abonnement wijzigen wilt:

   a. Selecteer **App Service-abonnement** > **+ maken van nieuwe**. 
   
   b. Geef een naam op van de nieuwe App Service-abonnement en selecteer de juiste locatie. 
   
   c. Selecteer een geschikte prijscategorie voor de service. Selecteer **weergeven van alle** naar de weergave meer opties, zoals prijzen **vrije** en **gedeelde**. 
   
   d. Klik op de **Selecteer** knop. 
   
   e. Terug in de **App Service-abonnement** deelvenster **OK**.
5. Selecteer **Maken**. 

Inrichting van een mobiele Apps kan back-end van een paar minuten duren. Nadat de mobiele Apps van back-end is ingericht, in de portal geopend de **instellingen** deelvenster voor de back-end van Mobile Apps.

U kunt verbinding maken met een bestaande SQL-database van uw back-end van Mobile Apps of maak een nieuwe SQL-database. In deze sectie maken we een SQL-database.

> [!NOTE]
> Als er al een database op dezelfde locatie als de Mobile Apps back-end, kunt u in plaats daarvan selecteren **gebruik een bestaande database** en selecteer vervolgens die database. Het gebruik van een database in een andere locatie wordt niet aanbevolen vanwege de hogere latenties.
>
>

1. Selecteer in de nieuwe mobiele Apps van back-end **instellingen** > **mobiele App** > **gegevens** > **+ toevoegen**.
2. In de **gegevensverbinding toevoegen** deelvenster **SQL Database - vereiste instellingen configureren** > **een nieuwe database maken**. Voer de naam van de nieuwe database in de **naam** vak.
3. Selecteer **Server**. In de **nieuwe server** deelvenster, voer een unieke naam in de **servernaam** vak en geef een geschikte server aanmeldgegevens van serverbeheerder en het wachtwoord. Zorg ervoor dat **azure-services voor toegang tot server toestaan** is geselecteerd. Selecteer **OK**.

   ![Een Azure SQL-database maken][6]
4. In de **nieuwe database** deelvenster **OK**.
5. Terug in de **gegevensverbinding toevoegen** deelvenster **verbindingsreeks**, en voer de aanmeldingsnaam en het wachtwoord die u hebt opgegeven tijdens het maken van de database. Als u een bestaande database gebruiken, geeft u de aanmeldingsreferenties voor die database. Selecteer **OK**.
6. Terug in de **gegevensverbinding toevoegen** deelvenster opnieuw in en selecteer **OK** om de database te maken.

<!--- END OF ALTERNATE INCLUDE -->

Maken van de database kan enkele minuten duren. Gebruik de **meldingen** gebied de voortgang van de implementatie. Voortgang niet totdat de database wordt geïmplementeerd. Nadat de database is geïmplementeerd, wordt een verbindingsreeks voor de SQL Database-exemplaar in de instellingen van uw Mobile Apps-back-end-app gemaakt. U kunt deze appinstelling in zien **instellingen** > **toepassingsinstellingen** > **verbindingsreeksen**.

### <a name="howto-tables-auth"></a>Verificatie vereisen voor toegang tot tabellen
Als u wilt gebruiken van App Service-verificatie met de `tables` eindpunt, moet u App Service-verificatie in de [Azure-portal] eerste. Zie voor meer informatie de configuratiehandleiding voor de id-provider die u wilt gebruiken:

* [Azure Active Directory-verificatie configureren]
* [Facebook-verificatie configureren]
* [Google-verificatie configureren]
* [Microsoft-verificatie configureren]
* [Twitter-verificatie configureren]

Elke tabel heeft een access-eigenschap die u gebruiken kunt om toegang aan de tabel te beheren. Het volgende voorbeeld ziet u een statisch gedefinieerde tabel met de verificatie is vereist.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table.
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

    // Require authentication to access the table.
    table.access = 'authenticated';

    module.exports = table;

De eigenschap toegang kan duren voordat een van drie waarden:

* *anonieme* geeft aan dat de clienttoepassing lezen van gegevens zonder verificatie is toegestaan.
* *geverifieerde* geeft aan dat de clienttoepassing een verificatietoken geldig met de aanvraag moet verzenden.
* *uitgeschakeld* geeft aan dat deze tabel is uitgeschakeld.

Als de eigenschap toegang niet gedefinieerd is, is niet-geverifieerde toegang toegestaan.

### <a name="howto-tables-getidentity"></a>Claims voor verificatie met uw tabellen gebruiken
U kunt instellen wanneer er verschillende claims die worden aangevraagd wanneer verificatie is ingesteld. Deze claims zijn niet normaal gesproken beschikbaar via de `context.user` object. Echter, u ze kunt ophalen met behulp van de `context.user.getIdentity()` methode. De `getIdentity()` methode retourneert een voorraad die wordt omgezet in een object. Het object is ingevoerd met de methode voor verificatie (`facebook`, `google`, `twitter`, `microsoftaccount`, of `aad`).

Als u de verificatie van de Microsoft-account en de e-mailadressen claim aanvraag hebt ingesteld, kunt u bijvoorbeeld het e-mailadres toevoegen aan de record met de domeincontroller van de volgende tabel:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition.
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request.
    // READ: only return records that belong to the authenticated user.
    table.read(queryContextForEmail);

    // CREATE: add or overwrite the userId based on the authenticated user.
    table.insert(addEmailToContext);

    // UPDATE: only allow updating of records that belong to the authenticated user.
    table.update(queryContextForEmail);

    // DELETE: only allow deletion of records that belong to the authenticated user.
    table.delete(queryContextForEmail);

    module.exports = table;

Als u wilt zien welke claims beschikbaar zijn, kunt u een webbrowser gebruiken om weer te geven de `/.auth/me` eindpunt van uw site.

### <a name="howto-tables-disabled"></a>De toegang tot specifieke tabelbewerkingen uitschakelen
Naast het op de tabel wordt weergegeven, kan de eigenschap toegang worden gebruikt om afzonderlijke bewerkingen te controleren. Er zijn vier bewerkingen:

* `read` de RESTful-GET-bewerking op de tabel is.
* `insert` de RESTful POST-bewerking op de tabel is.
* `update` de PATCH van RESTful-bewerking op de tabel is.
* `delete` de bewerking RESTful verwijderen op de tabel is.

U wilt bijvoorbeeld een alleen-lezen niet-geverifieerde tabel bieden:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-only table. Only allow READ operations.
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>De query die wordt gebruikt met tabelbewerkingen aanpassen
Een algemene vereiste voor tabelbewerkingen wordt een beperkte weergave van de gegevens. U kunt bijvoorbeeld een tabel die is gecodeerd met de ID van de geverifieerde gebruiker, zodat u kunt alleen lezen of bijwerken van uw eigen records opgeven. De definitie van de volgende tabel biedt deze functionaliteit:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table.
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table.
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved.
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user.
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Bewerkingen die normaal een query gesproken hebben een queryeigenschap die u aanpassen met behulp van kunt een `where` component. De queryeigenschap is een [QueryJS] -object dat wordt gebruikt voor het converteren van een OData-query naar iets dat de gegevens back-end kan verwerken. Eenvoudige gelijkheid gevallen (zoals het vorige item), kunt u een kaart. U kunt ook specifieke SQL-componenten toevoegen:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Configureren van een voorlopige verwijderingen in een tabel
Een voorlopige verwijderingen worden records niet daadwerkelijk verwijderd. In plaats daarvan markeren het als verwijderd in de database door de verwijderde kolom in te stellen op true. De Mobile Apps SDK automatisch voorlopig verwijderde records verwijderd uit de resultaten tenzij de Client-SDK voor Mobile gebruikt `IncludeDeleted()`. Voor het configureren van een tabel voor een voorlopig verwijderen, stelt de `softDelete` eigenschap in het definitiebestand voor de tabel:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table.
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

    // Turn on soft delete.
    table.softDelete = true;

    // Require authentication to access the table.
    table.access = 'authenticated';

    module.exports = table;

U moet een mechanisme voor het verwijderen van records instellen: een clienttoepassing, een webtaak, een Azure-functie of een aangepaste API gebruiken.

### <a name="howto-tables-seeding"></a>De database met gegevens seed
Wanneer u een nieuwe toepassing maakt, wilt u mogelijk seed van een tabel met gegevens. U kunt dit doen in de tabel definitie JavaScript-bestand als volgt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table.
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

    // Require authentication to access the table.
    table.access = 'authenticated';

    module.exports = table;

Seeding van de gegevens gebeurt alleen als u de Mobile Apps SDK hebt gebruikt om de tabel te maken. Als de tabel al in de database bestaat, wordt er geen gegevens ingevoegd in de tabel. Als het dynamische schema is ingeschakeld, wordt het schema is afgeleid van de geseede gegevens.

Het is raadzaam dat u expliciet aanroepen de `tables.initialize()` methode voor het maken van de tabel wanneer de service wordt gestart.

### <a name="Swagger"></a>Swagger-ondersteuning inschakelen
Mobile Apps wordt geleverd met ingebouwde [Swagger] ondersteunen. Swagger als ondersteuning wilt inschakelen, moet u eerst swagger-ui installeren als een afhankelijkheid:

    npm install --save swagger-ui

Vervolgens kunt u Swagger-ondersteuning in de constructor Mobile Apps inschakelen:

    var mobile = azureMobileApps({ swagger: true });

U waarschijnlijk alleen wilt Swagger-ondersteuning inschakelen in ontwikkeling edities. U kunt dit doen met behulp van de `NODE_ENV` app-instelling:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

De `swagger` eindpunt bevindt zich op http://*uw site*.azurewebsites.net/swagger. U hebt toegang tot de Swagger-gebruikersinterface via de `/swagger/ui` eindpunt. Als u kiest voor authenticatie, inlezen in uw hele toepassing, Swagger, treedt een fout. Kies voor de beste resultaten niet-geverifieerde aanvragen dat is toegestaan in de Azure App Service-verificatie/autorisatie-instellingen en vervolgens verificatie beheren met behulp van de `table.access` eigenschap.

U kunt ook de Swagger-optie toevoegen aan het bestand azureMobile.js als u alleen Swagger-ondersteuning voor het ontwikkelen van lokaal.

## <a name="a-namepushpush-notifications"></a><a name="push">Pushmeldingen
Mobile Apps worden geïntegreerd met Azure Notification Hubs, zodat u gerichte pushmeldingen naar miljoenen apparaten voor alle primaire platforms verzenden kunt. U kunt met behulp van Notification Hubs pushmeldingen verzenden voor iOS, Android en Windows apparaten. Zie voor meer informatie over alle opties die u kunt doen met Notification Hubs, de [Notification Hubs-overzicht](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Pushmeldingen verzenden
De volgende code toont hoe u de `push` object voor het verzenden van een pushmelding op geregistreerde iOS-apparaten:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured.
    if (context.push) {
        // Send a push notification by using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Als u de push-registratie van een sjabloon van de client maakt, kunt u in plaats daarvan een sjabloon push-bericht verzenden naar apparaten op alle ondersteunde platforms. De volgende code laat zien hoe een sjabloon melding te verzenden:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured.
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


### <a name="push-user"></a>Pushmeldingen verzenden naar een geverifieerde gebruiker met behulp van tags
Wanneer een geverifieerde gebruiker geregistreerd voor pushmeldingen, wordt een gebruikers-ID-tag automatisch toegevoegd aan de registratie. Met deze code, kunt u pushmeldingen verzendt naar alle apparaten die zijn geregistreerd door een specifieke gebruiker. De volgende code haalt de SID van de gebruiker die de aanvraag wordt ingediend en verzendt een pushmelding sjabloon naar elke apparaatregistratie voor die gebruiker:

    // Only do the push if configured.
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Wanneer u bent geregistreerd voor pushmeldingen vanuit een geverifieerde client, zorg dat de verificatie is voltooid voordat u de registratie.

## <a name="CustomAPI"></a> Aangepaste API 's
### <a name="howto-customapi-basic"></a>Een aangepaste API definiëren
Naast de API van de toegang tot gegevens via de `/tables` eindpunt, Mobile Apps dekking van uw aangepaste API kan bieden. Aangepaste API's zijn gedefinieerd in een vergelijkbare manier als bij de tabeldefinities en toegang tot dezelfde mogelijkheden, zoals verificatie.

Als u wilt een App Service-verificatie met een aangepaste API gebruiken, moet u App Service-verificatie in de [Azure-portal] eerste. Zie voor meer informatie de configuratiehandleiding voor de id-provider die u wilt gebruiken:

* [Azure Active Directory-verificatie configureren]
* [Facebook-verificatie configureren]
* [Google-verificatie configureren]
* [Microsoft-verificatie configureren]
* [Twitter-verificatie configureren]

Aangepaste API's zijn gedefinieerd in ongeveer dezelfde manier als de API tabellen:

1. Maak een `api` directory.
2. Maak een API-definitie JavaScript-bestand in de `api` directory.
3. De methode importeren gebruiken voor het importeren van de `api` directory.

Hier volgt een prototype van de API-definitie op basis van de steekproef basic-app die we eerder gebruikt:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the custom API.
    mobile.api.import('./api');

    // Add the Mobile API so it is accessible as a Web API.
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Voorbeeld-API die de datum van de server met behulp van retourneert de `Date.now()` methode. Dit is het bestand api/date.js:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Elke parameter is een van de RESTful Standaardwerkwoorden: ophalen, POST, PATCH of verwijderen. De methode is een standaard [ExpressJS middleware] functie waarmee de vereiste uitvoer worden verzonden.

### <a name="howto-customapi-auth"></a>Verificatie vereisen voor toegang tot een aangepaste API gebruiken
De Mobile Apps SDK wordt de verificatie geïmplementeerd op dezelfde manier voor zowel de `tables` eindpunt en aangepaste API's. Als u wilt verificatie toevoegen aan de API die in de vorige sectie zijn ontwikkeld, een `access` eigenschap:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

U kunt ook verificatie op specifieke bewerkingen opgeven:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Hetzelfde token dat wordt gebruikt voor de `tables` eindpunt moet worden gebruikt voor aangepaste API's waarvoor verificatie vereist.

### <a name="howto-customapi-auth"></a>Verwerken van grote bestandsuploads
De Mobile Apps SDK gebruikt de [hoofdtekst parser middleware](https://github.com/expressjs/body-parser) om te accepteren en de inhoud van de hoofdtekst van de inzending decoderen. U kunt vooraf configureren hoofdtekst-parser voor het accepteren van groter bestand uploadt:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling.
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the custom API.
    mobile.api.import('./api');

    // Add the Mobile API so it is accessible as a Web API.
    app.use(mobile);

    // Start listening on HTTP.
    app.listen(process.env.PORT || 3000);

Het bestand is base 64-codering voor verzending. Met deze codering neemt de grootte van het uploaden van de werkelijke (en de grootte die u moet rekening houden met).

### <a name="howto-customapi-sql"></a>Uitvoeren van aangepaste SQL-instructies
De Mobile Apps SDK biedt toegang tot de volledige context via het request-object. U kunt eenvoudig geparameteriseerde SQL-instructies aan de gedefinieerde gegevensprovider uitvoeren:

    var api = {
        get: function (request, response, next) {
            // Check for parameters. If not there, pass on to a later API call.
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query. Anything that the mssql
            // driver can handle is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query. The context for Mobile Apps is available through
            // request.azureMobile. The data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Foutopsporing, gemakkelijk tabellen en eenvoudige API 's
### <a name="howto-diagnostic-logs"></a>Fouten opsporen, vaststellen en oplossen van Mobile Apps
Azure App Service biedt verschillende opsporen en oplossen van technieken voor Node.js-toepassingen.
Om te beginnen bij het oplossen van uw back-end voor Node.js Mobile Apps raadpleegt u de volgende artikelen:

* [Bewaking van de Azure App Service]
* [Diagnostische logboekregistratie inschakelen in Azure App Service]
* [In Visual Studio-Azure App Service oplossen]

Node.js-toepassingen hebben toegang tot een breed scala aan extra diagnostische logboeken. De Mobile Apps Node.js SDK gebruikt intern, [Winston] voor diagnostische logboekregistratie. Logboekregistratie is standaard ingeschakeld wanneer u foutopsporing inschakelt modus of stel de `MS_DebugMode` app-instelling in op true in de [Azure-portal]. Gegenereerde logboeken worden weergegeven in de diagnostische logboeken in de [Azure-portal].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Werken met gemakkelijk tabellen in de Azure portal
U kunt gemakkelijk tabellen maken en ermee rechts van de tabellen in de portal. U kunt de gegevensset uploaden naar gemakkelijk tabellen in CSV-indeling. Houd er rekening mee dat u eigenschapnamen (in uw gegevensset CSV) die strijdig zijn niet gebruiken met de eigenschapsnamen van de back-end van Mobile Apps. De eigenschap Systeemnamen zijn:
* CreatedAt
* updatedAt
* verwijderd
* versie

U kunt zelfs tabelbewerkingen bewerken met behulp van App Service-Editor. Wanneer u selecteert **gemakkelijk tabellen** in uw back-end-site-instellingen, kunt u toevoegen, wijzigen of verwijderen van een tabel. U ziet ook de gegevens in de tabel.

![Werken met gemakkelijk tabellen](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

De volgende opdrachten zijn beschikbaar op de opdrachtbalk voor een tabel:

* **Machtigingen wijzigen**: wijzigen van de machtiging voor lezen, invoegen, bijwerken en verwijderen van bewerkingen voor de tabel.
 Opties zijn voor het toestaan van anonieme toegang voor authenticatie, of alle toegang tot de bewerking uitschakelen.
* **Bewerk script**: het scriptbestand voor de tabel in App Service-Editor wordt geopend.
* **Schema beheren**: toevoegen of kolommen verwijderen of wijzigen van de tabelindex.
* **Tabel wissen**: afkappen van een bestaande tabel door rijen met alle gegevens te verwijderen, maar als u het schema niet worden gewijzigd.
* **Verwijderen van rijen**: verwijderen van afzonderlijke rijen met gegevens.
* **Weergave streaminglogboeken**: verbinding maken met de streaming log-service voor uw site.

### <a name="work-easy-apis"></a>Werken met eenvoudige API's in de Azure portal
U kunt eenvoudige API's maken en werken met aangepaste API's rechts in de portal. U kunt de API-scripts bewerken met behulp van App Service-Editor.

Wanneer u selecteert **eenvoudige API's** in uw back-end-site-instellingen, kunt u toevoegen, wijzigen of verwijderen van een aangepaste API-eindpunt.

![Werken met eenvoudige API 's](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

In de portal kunt u de toegangsmachtigingen voor een HTTP-actie wijzigen, het scriptbestand API in App Service-Editor bewerken of de streaminglogboeken weergeven.

### <a name="online-editor"></a>Code in App Service-Editor bewerken
U kunt uw Node.js back-end-scriptbestanden in App Service-Editor met behulp van de Azure-portal bewerken zonder te hoeven downloaden van het project op uw lokale computer. Scriptbestanden in de online-editor bewerken:

1. Selecteer in deelvenster voor mobiele Apps van uw back-end, **alle instellingen** > beide **gemakkelijk tabellen** of **eenvoudige API's**. Selecteer een tabel of de API en selecteer vervolgens **script bewerken**. Het scriptbestand wordt geopend in App Service-Editor.

   ![App Service-editor](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. Uw wijzigingen aanbrengen in het bestand met code in de online-editor. Wijzigingen worden automatisch opgeslagen terwijl u typt.

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Android client Quick Start]: app-service-mobile-android-get-started.md
[Apache Cordova Client Quick Start]: app-service-mobile-cordova-get-started.md
[iOS Client Quick Start]: app-service-mobile-ios-get-started.md
[Quick Start voor Xamarin.iOS-Client]: app-service-mobile-xamarin-ios-get-started.md
[Snelstartgids voor Xamarin.Android-Client]: app-service-mobile-xamarin-android-get-started.md
[Quick Start Xamarin.Forms-Client]: app-service-mobile-xamarin-forms-get-started.md
[Windows Store-Client Quick Start]: app-service-mobile-windows-store-dotnet-get-started.md
[offline gegevenssynchronisatie]: app-service-mobile-offline-data-sync.md
[Azure Active Directory-verificatie configureren]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook-verificatie configureren]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Google-verificatie configureren]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Microsoft-verificatie configureren]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter-verificatie configureren]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
[Implementatiehandleiding voor Azure App Service]: ../app-service/app-service-deploy-local-git.md
[Bewaking van de Azure App Service]: ../app-service/web-sites-monitor.md
[Diagnostische logboekregistratie inschakelen in Azure App Service]: ../app-service/web-sites-enable-diagnostic-log.md
[In Visual Studio-Azure App Service oplossen]: ../app-service/web-sites-dotnet-troubleshoot-visual-studio.md
[Geef de versie van het knooppunt]: ../nodejs-specify-node-version-azure-apps.md
[knooppunt-modules gebruiken]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure-portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp voorbeeld op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo-voorbeeld op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[directory voorbeelden op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[1.1 van Node.js-hulpprogramma's voor Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js pakket]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
