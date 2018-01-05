---
title: Werken met de back-endserver Node.js SDK voor Mobile Apps | Microsoft Docs
description: Informatie over het werken met de back-endserver Node.js SDK voor Azure App Service Mobile Apps.
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
ms.openlocfilehash: 336da28bea7de313bced97e447fc6b7b1fb1390d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Het gebruik van de Azure Mobile Apps-SDK voor Node.js
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Dit artikel bevat gedetailleerde informatie over en voorbeelden waarin wordt getoond hoe werkt met een back-end voor Node.js in Azure App Service Mobile Apps.

## <a name="Introduction"></a>Inleiding
Azure App Service Mobile Apps biedt de mogelijkheid een gegevens geoptimaliseerd voor mobiele toegang tot Web-API toevoegen aan een webtoepassing.  De Azure App Service Mobile Apps SDK is beschikbaar voor ASP.NET- en Node.js-webtoepassingen.  De SDK biedt de volgende bewerkingen:

* Tabelbewerkingen (lezen, Insert, Update, Delete) voor toegang tot gegevens
* Aangepaste API-bewerkingen

Beide bewerkingen kunnen worden gebruikt voor verificatie binnen alle id-providers toegestaan door Azure App Service, met inbegrip van sociale id-providers zoals Facebook, Twitter, Google en Microsoft, evenals Azure Active Directory voor de identiteit van de onderneming.

Vindt u voorbeelden voor elk geval gebruik in de [directory voorbeelden op GitHub].

## <a name="supported-platforms"></a>Ondersteunde Platforms
De Azure Mobile Apps knooppunt SDK biedt ondersteuning voor de huidige release TNS van knooppunt en hoger.  Op het moment van schrijven is de meest recente versie van de TNS knooppunt v4.5.0.  Andere versies van knooppunt werkt, maar worden niet ondersteund.

De Azure Mobile Apps knooppunt SDK ondersteunt twee databasestuurprogramma - het knooppunt mssql-stuurprogramma ondersteunt SQL Azure en lokale exemplaren van SQL Server.  Het stuurprogramma sqlite3 ondersteunt SQLite-databases in één exemplaar.

### <a name="howto-cmdline-basicapp"></a>Hoe: maken van een eenvoudige Node.js back-end via de opdrachtregel
Elke back-end van Azure App Service Mobile App Node.js wordt gestart als een toepassing ExpressJS.  ExpressJS is het meest populaire web service-framework beschikbaar voor Node.js.  U kunt een eenvoudige maken [Express] toepassing als volgt:

1. Maak een map voor uw project in een opdracht of een PowerShell-venster.

        mkdir basicapp
2. Npm init om te initialiseren van de pakketstructuur worden uitgevoerd.

        cd basicapp
        npm init

    De npm-init-opdracht wordt u gevraagd een reeks vragen initialiseren van het project.  Zie het voorbeeld van uitvoer:

    ![De npm-init-uitvoer][0]
3. De snelle en apps van azure mobile bibliotheken installeren vanuit de npm-opslagplaats.

        npm install --save express azure-mobile-apps
4. Maak een bestand app.js voor het implementeren van de basic mobiele-server.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Deze toepassing maakt een WebAPI mobile geoptimaliseerd met één eindpunt (`/tables/TodoItem`) biedt geverifieerde toegang tot de onderliggende SQL gegevensopslag met behulp van een dynamische schema.  Is geschikt voor een snel starten van de client-bibliotheek te volgen:

* [Android Client Quick Start]
* [Apache Cordova-Client Quick Start]
* [iOS-Client Quick Start]
* [Windows Store-Client Quick Start]
* [Quick Start voor Xamarin.iOS-Client]
* [Snelstartgids voor Xamarin.Android-Client]
* [Quick Start Xamarin.Forms-Client]

U vindt de code voor deze eenvoudige toepassing in de [basicapp voorbeeld op GitHub].

### <a name="howto-vs2015-basicapp"></a>Procedure: een back-end knooppunt maken met Visual Studio 2015
Visual Studio 2015 is een uitbreiding voor het ontwikkelen van Node.js-toepassingen IDE vereist.  Als u wilt starten, installeer de [1.1 van Node.js-hulpprogramma's voor Visual Studio].  Zodra de Node.js-Tools voor Visual Studio zijn geïnstalleerd, moet u een snelle 4.x-toepassing maken:

1. Open de **nieuw Project** dialoogvenster (van **bestand** > **nieuw** > **Project wordt gemaakt...** ).
2. Vouw **sjablonen** > **JavaScript** > **Node.js**.
3. Selecteer de **Basic Azure Node.js Express 4-toepassing**.
4. Vul in de naam van het project.  Klik op *OK*.

    ![Nieuw Project voor Visual Studio 2015][1]
5. Met de rechtermuisknop op de **npm** uit en selecteer **installeren nieuwe npm pakketten...** .
6. Mogelijk moet u de npm-catalogus over het maken van uw eerste Node.js-toepassing te vernieuwen.  Klik op **vernieuwen** indien nodig.
7. Voer *apps van azure mobile* in het zoekvak.  Klik op de **azure mobile apps 2.0.0** van het pakket en klik vervolgens op **pakket installeren**.

    ![Nieuwe npm pakketten installeren][2]
8. Klik op **Sluiten**.
9. Open de *app.js* bestand ondersteuning toevoegen voor de Azure Mobile Apps SDK.  Op de regel 6 at onder aan de bibliotheek instructies nodig, voeg de volgende code:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Op de regel ongeveer 27 na de app.use-instructies, voeg de volgende code:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Sla het bestand op.
10. Voer de toepassing lokaal (de API wordt geleverd op http://localhost: 3000) of publiceren naar Azure.

### <a name="create-node-backend-portal"></a>Procedure: een Node.js-back-end met de Azure portal maken
Kunt u een rechts van de back-end voor mobiele App in de [Azure-portal]. U kunt de volgende stappen of een client en server samen te maken door de [maken van een mobiele app](app-service-mobile-ios-get-started.md) zelfstudie. De zelfstudie bevat een vereenvoudigde versie van deze instructies en wordt aanbevolen voor bewijs van concept projecten.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Terug in de *aan de slag* blade onder **maken van een tabel-API**, kies **Node.js** als uw **back-endtaal**.
Schakel het selectievakje voor '**ik erken dat hiermee alle inhoud van site overschreven wordt.**', klikt u vervolgens op **takentabel maken**.

### <a name="download-quickstart"></a>Procedure: de Node.js-back-end Quick Start code-project hebt gedownload met Git
Wanneer u een back-end voor Node.js mobiele App maakt met behulp van de portal **snel starten** blade een Node.js-project wordt voor u gemaakt en geïmplementeerd voor uw site. U kunt de tabellen en API's toevoegen en bewerken van codebestanden voor de Node.js-back-end in de portal. U kunt ook verschillende implementatieprogramma's gebruiken voor het downloaden van de back-endproject zodat u kunt toevoegen of wijzigen van de API's en tabellen en vervolgens opnieuw publiceren van het project. Zie voor meer informatie de [Implementatiehandleiding voor Azure App Service]. de volgende procedure maakt gebruik van een Git-opslagplaats voor het downloaden van de Quick Start-project-code.

1. Installeer Git als u dat nog niet hebt gedaan. De stappen die nodig zijn voor het installeren van Git variëren tussen besturingssystemen. Zie [installeren Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) voor besturingssysteem-specifieke distributies en installatie-instructies.
2. Volg de stappen in [inschakelen van de App Service-app-opslagplaats](../app-service/app-service-deploy-local-git.md#Step3) zodat de Git-opslagplaats voor uw back-end-site, waardoor een notitie van de implementatie van gebruikersnaam en wachtwoord.
3. In de blade voor uw back-end voor de mobiele App, noteert u de **Git-kloon-URL** instelling.
4. Uitvoeren van de `git clone` opdracht met de Git-kloon-URL invoeren van het wachtwoord indien nodig, zoals in het volgende voorbeeld:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. Blader naar de lokale map, die in het voorgaande voorbeeld /todolist, en u ziet dat project-bestanden zijn gedownload. Zoek de `todoitem.json` bestand de `/tables` directory.  Dit bestand definieert machtigingen voor de tabel.  Ook de `todoitem.js` bestand in dezelfde map, waarmee wordt gedefinieerd die CRUD-bewerking scripts voor de tabel.
6. Nadat u wijzigingen in de project-bestanden aangebracht hebt, voert u de volgende opdrachten kunt toevoegen, doorvoeren en uploadt u de wijzigingen naar de site uit:

        $ git commit -m "updated the table script"
        $ git push origin master

    Wanneer u nieuwe bestanden aan het project toevoegt, moet u eerst uit te voeren de `git add .` opdracht.

Telkens wanneer een nieuwe reeks doorvoeracties wordt doorgegeven voor de site opnieuw de site wordt gepubliceerd.

### <a name="howto-publish-to-azure"></a>Hoe: uw Node.js-back-end publiceren naar Azure
Microsoft Azure biedt veel mechanismen voor het publiceren van uw Azure App Service Mobile Apps Node.js-back-end naar de Azure-service.  Deze omvatten gebruik geïntegreerd in Visual Studio-hulpprogramma's voor, opdrachtregelprogramma's en continue implementatie-opties op basis van broncodebeheer.  Zie voor meer informatie over dit onderwerp de [Implementatiehandleiding voor Azure App Service].

Azure App Service biedt specifiek advies voor Node.js-toepassing die u vóór de implementatie controleren moet:

* Hoe [Geef de versie van het knooppunt]
* Hoe [knooppunt-modules gebruiken]

### <a name="howto-enable-homepage"></a>Procedure: een startpagina van uw toepassing inschakelen
Veel toepassingen zijn een combinatie van web- en mobiele apps en het framework ExpressJS kunt u de twee facetten combineren.  Soms is echter mogelijk wilt u alleen een mobiele-interface implementeren.  Dit is handig om te bieden een startpagina om te controleren of de app-service actief is.  Geef uw eigen introductiepagina of de startpagina van een tijdelijke inschakelen.  Als u wilt inschakelen op de startpagina van een tijdelijke, gebruikt u de volgende instantiëren van Azure Mobile Apps:

    var mobile = azureMobileApps({ homePage: true });

Als u deze optie beschikbaar wilt bij het ontwikkelen van lokaal, kunt u toevoegen met deze instelling om uw `azureMobile.js` bestand.

## <a name="TableOperations"></a>Tabelbewerkingen
De apps van azure mobile Node.js-SDK-Server biedt mechanismen om gegevenstabellen die zijn opgeslagen in Azure SQL Database als een WebAPI weer te geven.  Er zijn vijf operations beschikbaar.

| Bewerking | Beschrijving |
| --- | --- |
| GET-/tables/*tablename* |Ophalen van alle records in de tabel |
| GET-/tables/*tablename*/:id |Ophalen van een bepaalde record in de tabel |
| POST /tables/*tablename* |Een record in de tabel maken |
| PATCH /tables/*tablename*/:id |Een record in de tabel niet bijwerken |
| DELETE /tables/*tablename*/:id |Een record in de tabel verwijderen |

Biedt ondersteuning voor deze WebAPI [OData] en breidt het tabelschema ter ondersteuning van [offline gegevenssynchronisatie].

### <a name="howto-dynamicschema"></a>Hoe: tabellen met behulp van een dynamische schema definiëren
Voordat een tabel kan worden gebruikt, moet worden gedefinieerd.  Tabellen kunnen worden gedefinieerd met een statische schema (de ontwikkelaar definieert waar de kolommen in het schema) of dynamisch (de SDK bepaalt waar het schema op basis van binnenkomende aanvragen). De ontwikkelaar kan bovendien specifieke aspecten van de WebAPI beheren door Javascript-code toe te voegen aan de definitie.

Als een best practice moet u elke tabel in een Javascript-bestand in de map tabellen definiëren en vervolgens de methode tables.import() gebruiken om de tabellen te importeren.  Uitbreiden van de basic-app, zou het bestand app.js worden aangepast:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Definieer de tabel in. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Tabellen dynamische schema standaard gebruikt.  Als wilt uitschakelen dynamische schema globaal in, stelt u de App-instelling **MS_DynamicSchema** op false in de Azure-portal.

U vindt een compleet voorbeeld in de [todo-voorbeeld op GitHub].

### <a name="howto-staticschema"></a>Hoe: tabellen met behulp van een statische schema definiëren
U kunt de kolommen te kunnen stellen via de WebAPI expliciet definiëren.  De Node.js-SDK voor apps van azure mobile voegt automatisch eventuele extra kolommen die zijn vereist voor het offline synchroniseren van gegevens aan de lijst die u opgeeft.  De Quick Start-clienttoepassingen vereisen bijvoorbeeld dat een tabel met twee kolommen: tekst (tekenreeks) en (een Booleaanse waarde) te voltooien.  
De tabel kan worden gedefinieerd in de tabel definitie JavaScript-bestand (te vinden in de map tabellen) als volgt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Als u tabellen statisch definieert, moet u ook de tables.initialize()-methode voor het maken van het databaseschema bij het opstarten aanroepen.  De methode tables.initialize() retourneert een [belofte] zodat de webservice dienen geen aanvragen voor de database wordt geïnitialiseerd.

### <a name="howto-sqlexpress-setup"></a>Hoe: Gebruik SQL Express als gegevensopslag ontwikkeling op uw lokale computer
De Azure Mobile Apps de AzureMobile Apps knooppunt SDK biedt drie opties voor die gegevens buiten het vak: SDK biedt drie opties voor die gegevens buiten het vak:

* Gebruik de **geheugen** -stuurprogramma voor een voorbeeld van de niet-permanente archief
* Gebruik de **mssql** -stuurprogramma voor SQL Express gegevensopslag voor ontwikkeling
* Gebruik de **mssql** -stuurprogramma voor een Azure SQL Database-gegevensarchief voor productie

De Azure Mobile Apps Node.js SDK gebruikt de [mssql Node.js pakket] voor het maken van een verbinding met SQL Express en SQL-Database gebruiken.  Dit pakket vereist dat u TCP-verbindingen op uw exemplaar van SQL Express inschakelen.

> [!TIP]
> Het stuurprogramma geheugen biedt geen een volledige set van opslagruimten voor het testen.  Als u uw back-end lokaal testen wilt, raden wij het gebruik van een gegevensarchief SQL Express en de mssql-stuurprogramma.
>
>

1. Download en installeer [Microsoft SQL Server 2014 Express].  Zorg ervoor dat u de SQL Server 2014 Express installeren met hulpprogramma's-editie.  Tenzij u expliciet ondersteuning voor 64-bits vereist, verbruikt de 32-bits versie minder geheugen uitgevoerd.
2. Voer de SQL Server 2014 Configuration Manager.

   1. Vouw de **SQL Server-netwerkconfiguratie** knooppunt in het menu links structuur.
   2. Klik op **protocollen voor SQLEXPRESS**.
   3. Met de rechtermuisknop op **TCP/IP** en selecteer **inschakelen**.  Klik op **OK** in het pop-upvenster.
   4. Met de rechtermuisknop op **TCP/IP** en selecteer **eigenschappen**.
   5. Klik op de **IP-adressen** tabblad.
   6. Zoek de **IPAll** knooppunt.  In de **TCP-poort** veld **1433**.

          ![Configure SQL Express for TCP/IP][3]
   7. Klik op **OK**.  Klik op **OK** in het pop-upvenster.
   8. Klik op **SQL Server-Services** in het menu links structuur.
   9. Met de rechtermuisknop op **SQL Server (SQLEXPRESS)** en selecteer **opnieuw starten**
   10. Sluit de SQL Server 2014 Configuration Manager.
3. Voer de SQL Server 2014 Management Studio en maak verbinding met uw lokale exemplaar van SQL Express

   1. Met de rechtermuisknop op uw exemplaar in Object Explorer en selecteer **eigenschappen**
   2. Selecteer de **beveiliging** pagina.
   3. Zorg ervoor dat de **modus van SQL Server en Windows-verificatie** is geselecteerd
   4. Klik op **OK**

          ![Configure SQL Express Authentication][4]
   5. Vouw **beveiliging** > **aanmeldingen** in Object Explorer
   6. Met de rechtermuisknop op **aanmeldingen** en selecteer **New Login...**
   7. Typ een aanmeldingsnaam.  Selecteer **SQL Server-verificatie**.  Voer een wachtwoord, voert u het wachtwoord in **wachtwoord bevestigen**.  Het wachtwoord moet voldoen aan de complexiteitsvereisten van Windows.
   8. Klik op **OK**

          ![Add a new user to SQL Express][5]
   9. Met de rechtermuisknop op uw nieuwe aanmelding en selecteer **eigenschappen**
   10. Selecteer de **serverfuncties** pagina
   11. Schakel het selectievakje in naast de **dbcreator** serverfunctie
   12. Klik op **OK**
   13. De SQL Server 2015 Management Studio sluiten

Zorg ervoor dat u vastlegt gebruikersnaam en wachtwoord die u hebt geselecteerd.  U wilt toewijzen van aanvullende serverfuncties of machtigingen, afhankelijk van de vereisten van uw specifieke database.

De Node.js-toepassing leest de **SQLCONNSTR_MS_TableConnectionString** omgevingsvariabele voor de verbindingsreeks voor deze database.  U kunt deze variabele instellen in uw omgeving.  U kunt bijvoorbeeld PowerShell gebruiken voor het instellen van deze omgevingsvariabele:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Toegang tot de database via een TCP/IP-verbinding en geef een gebruikersnaam en wachtwoord voor de verbinding.

### <a name="howto-config-localdev"></a>Hoe: uw project voor lokale ontwikkeling configureren
Azure Mobile Apps leest een JavaScript-bestand aangeroepen *azureMobile.js* uit het lokale bestandssysteem.  Dit bestand geen gebruiken voor het configureren van de Azure Mobile Apps SDK in productie - App-instellingen binnen de [Azure-portal] in plaats daarvan.  De *azureMobile.js* bestand moet een configuratieobject exporteren.  De meest voorkomende instellingen zijn:

* Database-instellingen
* Instellingen voor diagnostische logboekregistratie
* Alternatieve CORS-instellingen

Een voorbeeld *azureMobile.js* bestand uitvoering van de voorgaande database-instellingen wordt gevolgd:

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

Het is raadzaam dat u toevoegt, *azureMobile.js* naar uw *.gitignore* bestand (of andere broncodebeheer bestand negeren) om te voorkomen dat wachtwoorden worden opgeslagen in de cloud.  Altijd productie-instellingen configureren in de instellingen van de App in de [Azure-portal].

### <a name="howto-appsettings"></a>Procedure: App-instellingen voor uw mobiele App configureren
De meeste instellingen in de *azureMobile.js* bestand hebben een equivalente App-instelling de [Azure-portal].  Gebruik de volgende lijst voor het configureren van uw app in App-instellingen:

| App-instelling | *azureMobile.js* instelling | Beschrijving | Geldige waarden |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |naam |De naam van de app |tekenreeks |
| **MS_MobileLoggingLevel** |Logging.level |Minimale logboekniveau van berichten aan te melden |fout, waarschuwing, info, uitgebreid, foutopsporing, stom |
| **MS_DebugMode** |fouten opsporen |In- of de foutopsporingsmodus uitschakelen |True, false |
| **MS_TableSchema** |Data.schema |Naam van de standaard schema voor de SQL-tabellen |tekenreeks (standaard: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |In- of de foutopsporingsmodus uitschakelen |True, false |
| **MS_DisableVersionHeader** |versie (ingesteld op niet-gedefinieerde) |Schakelt de header X-ZUMO-Server-versie |True, false |
| **MS_SkipVersionCheck** |skipversioncheck |Hiermee schakelt de controle van de client-API-versie |True, false |

Instellen van een App-instelling:

1. Meld u aan bij [Azure-portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van uw mobiele App.
3. Standaard wordt de blade instellingen geopend. Als dit niet zo is, klikt u op **instellingen**.
4. Klik op **toepassingsinstellingen** in het menu Algemeen.
5. Blader naar de sectie met App-instellingen.
6. Als uw app instelt, al bestaat, klikt u op de waarde van de app-instelling om de waarde te bewerken.
7. Als uw app-instelling niet bestaat, voert u de App-instelling in het vak sleutel en de waarde in het vak waarde.
8. Nadat u voltooid zijn, klikt u op **opslaan**.

Het wijzigen van de meeste appinstellingen voor vereist service opnieuw opstarten.

### <a name="howto-use-sqlazure"></a>Procedure: SQL-Database gebruiken als uw gegevensarchief productie
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Met behulp van Azure SQL Database als gegevensopslag is identiek alle typen voor Azure App Service-toepassing. Als u dit nog niet hebt gedaan, volgt u deze stappen voor het maken van een back-end voor de mobiele App.

1. Meld u aan bij [Azure-portal].
2. In de rechterbovenhoek van het venster links, klikt u op de **+ nieuw** knop > **Web en mobiel** > **mobiele App**, Geef een naam op voor uw back-end voor de mobiele App.
3. In de **resourcegroep** Voer dezelfde naam als uw app.
4. Het standaard-App Service-abonnement is geselecteerd.  Als u wijzigen van uw App Service-abonnement wilt, u kunt dit doen door te klikken op de App Service-Plan > **+ maken van nieuwe**.  Geef een naam op van de nieuwe App Service-abonnement en selecteer de juiste locatie.  Klik op de prijscategorie en selecteer een geschikte prijscategorie voor de service. Selecteer **weergeven van alle** naar de weergave meer opties, zoals prijzen **vrije** en **gedeelde**.  Nadat u de prijscategorie hebt geselecteerd, klikt u op de **Selecteer** knop.  Terug in de **App Service-abonnement** blade, klikt u op **OK**.
5. Klik op **Create**. Inrichting van een back-end voor de mobiele App kan een paar minuten duren.  Zodra de back-end voor de mobiele App is geconfigureerd, de portal wordt geopend de **instellingen** blade voor de back-end voor de mobiele App.

Zodra de back-end voor de mobiele App is gemaakt, kunt u een verbinding maken met een bestaande SQL-database back-end van uw mobiele App of maak een nieuwe SQL-database.  In deze sectie maken we een SQL-database.

> [!NOTE]
> Als u al een database op dezelfde locatie bevinden als de back-end voor de mobiele app hebt, kunt u in plaats daarvan kiezen **gebruik een bestaande database** en selecteer vervolgens die database. Het gebruik van een database in een andere locatie wordt niet aanbevolen vanwege de hogere latenties.
>
>

1. Klik in de nieuwe mobiele App back-end op **instellingen** > **mobiele App** > **gegevens** > **+ toevoegen**.
2. In de **gegevensverbinding toevoegen** blade, klikt u op **SQL Database - vereiste instellingen configureren** > **een nieuwe database maken**.  Voer de naam van de nieuwe database in de **naam** veld.
3. Klik op **Server**.  In de **nieuwe server** blade, voer een unieke naam in de **servernaam** veld en geef een geschikte **aanmeldgegevens van serverbeheerder** en **wachtwoord**.  Zorg ervoor dat **azure-services voor toegang tot server toestaan** is ingeschakeld.  Klik op **OK**.

    ![Een Azure SQL-Database maken][6]
4. Op de **nieuwe database** blade, klikt u op **OK**.
5. Terug op de **gegevensverbinding toevoegen** blade Selecteer **verbindingsreeks**, voer de aanmeldingsnaam en het wachtwoord die u hebt opgegeven bij het maken van de database.  Als u een bestaande database gebruiken, geeft u de aanmeldingsreferenties voor die database.  Nadat u hebt ingevoerd, klikt u op **OK**.
6. Terug op de **gegevensverbinding toevoegen** blade opnieuw in en klikt u op **OK** om de database te maken.

<!--- END OF ALTERNATE INCLUDE -->

Maken van de database kan enkele minuten duren.  Gebruik de **meldingen** gebied de voortgang van de implementatie.  Voortgang niet totdat de database is geïmplementeerd.  Zodra met succes is geïmplementeerd, wordt een verbindingsreeks voor de SQL-Database in uw mobiele back-end App-instellingen gemaakt.  U ziet dit app-instelling in de **instellingen** > **toepassingsinstellingen** > **verbindingsreeksen**.

### <a name="howto-tables-auth"></a>Procedure: verificatie vereisen voor toegang tot tabellen
Als u wenst voor het gebruik van App Service-verificatie met het eindpunt tabellen, moet u App Service-verificatie in de [Azure-portal] eerste.  Raadpleeg de configuratiehandleiding voor de id-provider die u wilt gebruiken voor meer informatie over het configureren van verificatie in een Azure App Service:

* [Het configureren van Azure Active Directory-verificatie]
* [Facebook-verificatie configureren]
* [Google-verificatie configureren]
* [Het configureren van Microsoft Authentication]
* [Twitter-verificatie configureren]

Elke tabel heeft een access-eigenschap die kan worden gebruikt voor het beheren van toegang tot de tabel.  Het volgende voorbeeld ziet u een statisch gedefinieerde tabel met de verificatie is vereist.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

De eigenschap toegang kan duren voordat een van drie waarden

* *anonieme* geeft aan dat de clienttoepassing lezen van gegevens zonder verificatie is toegestaan
* *geverifieerde* geeft aan dat de clienttoepassing een verificatietoken geldig met de aanvraag moet verzenden
* *uitgeschakeld* geeft aan dat deze tabel is uitgeschakeld

Als de eigenschap toegang niet gedefinieerd is, is niet-geverifieerde toegang toegestaan.

### <a name="howto-tables-getidentity"></a>Procedure: verificatie claims gebruiken met uw tabellen
U kunt instellen wanneer er verschillende claims die worden aangevraagd wanneer verificatie is ingesteld.  Deze claims zijn niet normaal gesproken beschikbaar via de `context.user` object.  Maar ze kunnen worden opgehaald met de `context.user.getIdentity()` methode.  De `getIdentity()` methode retourneert een voorraad die wordt omgezet in een object.  Het object is ingevoerd met de verificatiemethode (facebook, google, twitter, microsoftaccount of aad).

Als u verificatie van de Microsoft-Account en het verzoek claimtypen die de e-mailadressen hebt ingesteld, kunt u bijvoorbeeld het e-mailadres toevoegen aan de record met de domeincontroller van de volgende tabel:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
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

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Als u wilt zien welke claims beschikbaar zijn, kunt u een webbrowser gebruiken om weer te geven de `/.auth/me` eindpunt van uw site.

### <a name="howto-tables-disabled"></a>Procedure: de toegang tot specifieke tabelbewerkingen uitschakelen
Naast het op de tabel wordt weergegeven, kan de eigenschap toegang worden gebruikt om afzonderlijke bewerkingen te controleren.  Er zijn vier bewerkingen:

* *Lees* is van de RESTful-GET-bewerking op de tabel
* *invoegen* is de RESTful POST-bewerking in de tabel
* *bijwerken* is de bewerking RESTful PATCH in de tabel
* *Verwijder* is de bewerking RESTful verwijderen in de tabel

U wilt bijvoorbeeld een alleen-lezen niet-geverifieerde tabel bieden:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Procedure: de query die wordt gebruikt met tabelbewerkingen aanpassen
Een algemene vereiste voor tabelbewerkingen wordt een beperkte weergave van de gegevens.  U kunt bijvoorbeeld een tabel die is gecodeerd met de ID van de geverifieerde gebruiker, zodat u kunt alleen lezen of bijwerken van uw eigen records opgeven.  De definitie van de volgende tabel biedt deze functionaliteit:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Bewerkingen die normaal gesproken een query uitvoeren hebben een queryeigenschap die u met een waar aanpassen kunt component. De queryeigenschap is een [QueryJS] -object dat wordt gebruikt voor het converteren van een OData-query naar iets dat de gegevens back-end kan verwerken.  Eenvoudige gelijkheid gevallen (zoals het vorige item), kan een kaart worden gebruikt. U kunt ook specifieke SQL-componenten toevoegen:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Hoe: voorlopig verwijderen configureren in een tabel
Voorlopige verwijderingen worden records niet daadwerkelijk verwijderd.  In plaats daarvan markeren het als verwijderd in de database door de verwijderde kolom in te stellen op true.  De Azure Mobile Apps SDK automatisch voorlopig verwijderde records worden verwijderd uit de resultaten tenzij de Client-SDK voor Mobile IncludeDeleted() gebruikt.  Voor het configureren van een tabel voor voorlopig verwijderen, stelt de `softDelete` eigenschap in het definitiebestand voor de tabel:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Een mechanisme voor het opschonen van records - vanuit een clienttoepassing, via een webtaak, Azure-functie of via een aangepaste API gebruiken, moet u instellen.

### <a name="howto-tables-seeding"></a>Procedure: de database met gegevens Seed
Bij het maken van een nieuwe toepassing mogelijk wilt u een tabel met gegevens is ingevuld.  Dit kan worden uitgevoerd binnen de tabel definitie JavaScript-bestand als volgt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Seeding van de gegevens wordt alleen uitgevoerd wanneer de tabel is gemaakt door de Azure Mobile Apps SDK.  Als de tabel al in de database bestaat, wordt er geen gegevens ingevoegd in de tabel.  Als u dynamische schema is ingeschakeld, klikt u vervolgens het schema afgeleid van de geseede gegevens.

Het is raadzaam dat u expliciet aanroepen de `tables.initialize()` methode voor het maken van de tabel wanneer de service wordt gestart.

### <a name="Swagger"></a>Hoe: Swagger-ondersteuning inschakelen
Azure App Service Mobile Apps wordt geleverd met ingebouwde [Swagger] ondersteunen.  Swagger als ondersteuning wilt inschakelen, moet u eerst de swagger-ui installeren als een afhankelijkheid:

    npm install --save swagger-ui

Zodra u hebt geïnstalleerd, kunt u Swagger-ondersteuning in de Azure Mobile Apps-constructor inschakelen:

    var mobile = azureMobileApps({ swagger: true });

U waarschijnlijk alleen wilt Swagger-ondersteuning inschakelen in ontwikkeling edities.  U kunt dit doen door het gebruik van de `NODE_ENV` app-instelling:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Het swagger-eindpunt bevindt zich op http://*uw site*.azurewebsites.net/swagger.  U hebt toegang tot de Swagger-gebruikersinterface via de `/swagger/ui` eindpunt.  Als u kiest voor authenticatie, inlezen in uw hele toepassing, Swagger, treedt een fout.  Kies voor de beste resultaten om toe te staan van niet-geverifieerde aanvragen via de Azure App Service-verificatie / autorisatie-instellingen beheren vervolgens verificatie met behulp van de `table.access` eigenschap.

U kunt ook de Swagger-optie voor het toevoegen uw `azureMobile.js` als u alleen Swagger ondersteuning bij het ontwikkelen van lokaal bestand.

## <a name="a-namepushpush-notifications"></a><a name="push">Pushmeldingen
Mobiele Apps kan worden geïntegreerd met Azure Notification Hubs waarmee u kunt gerichte pushmeldingen verzenden naar miljoenen apparaten voor alle primaire platforms. U kunt via Notification Hubs pushmeldingen verzenden naar iOS, Android en Windows-apparaten. Zie voor meer informatie over alle opties die u kunt doen met Notification Hubs, [overzicht van Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Hoe: pushmeldingen verzenden
De volgende code toont hoe u de push-object gebruiken voor het verzenden van een pushmelding naar geregistreerde iOS-apparaten:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Als u de push-registratie van een sjabloon van de client maakt, kunt u in plaats daarvan een sjabloon push-bericht verzenden naar apparaten op alle ondersteunde platforms. De volgende code laat zien hoe een sjabloon melding te verzenden:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


### <a name="push-user"></a>Hoe: pushmeldingen verzenden naar een geverifieerde gebruiker met tags
Wanneer een geverifieerde gebruiker geregistreerd voor pushmeldingen, wordt een gebruikers-ID-tag automatisch toegevoegd aan de registratie. Met deze code, kunt u pushmeldingen verzendt naar alle apparaten die zijn geregistreerd door een specifieke gebruiker. De volgende code wordt de SID van de gebruiker die de aanvraag en verzendt een pushmelding sjabloon naar elke apparaatregistratie voor die gebruiker:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Wanneer u registreert voor pushmeldingen vanuit een geverifieerde client, moet u ervoor zorgen dat de verificatie is voltooid voordat u de registratie.

## <a name="CustomAPI"></a>Aangepaste API 's
### <a name="howto-customapi-basic"></a>Procedure: een aangepaste API definiëren
Naast de API voor gegevenstoegang via het eindpunt /tables krijgt u Azure Mobile Apps dekking van uw aangepaste API.  Aangepaste API's zijn gedefinieerd in een vergelijkbare manier als bij de tabeldefinities en toegang tot dezelfde mogelijkheden, zoals verificatie.

Als u wilt een App Service-verificatie met een aangepaste API gebruiken, moet u App Service-verificatie in de [Azure-portal] eerste.  Raadpleeg de configuratiehandleiding voor de id-provider die u wilt gebruiken voor meer informatie over het configureren van verificatie in een Azure App Service:

* [Het configureren van Azure Active Directory-verificatie]
* [Facebook-verificatie configureren]
* [Google-verificatie configureren]
* [Het configureren van Microsoft Authentication]
* [Twitter-verificatie configureren]

Aangepaste API's zijn gedefinieerd in ongeveer dezelfde manier als de API-tabellen.

1. Maak een **api** directory
2. Maak een API-definitie JavaScript-bestand in de **api** directory.
3. De methode importeren gebruiken voor het importeren van de **api** directory.

Hier volgt de prototype api-definitie op basis van het basic-app-voorbeeld dat hebben we eerder hebt gebruikt.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Voorbeeld-API die de server datum met retourneert de *Date.now()* methode.  Dit is het bestand api/date.js:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Elke parameter is een van de RESTful Standaardwerkwoorden - GET, POST, PATCH of verwijderen.  De methode is een standaard [ExpressJS Middleware] functie waarmee de vereiste uitvoer worden verzonden.

### <a name="howto-customapi-auth"></a>Procedure: verificatie vereisen voor toegang tot een aangepaste API gebruiken
Azure Mobile Apps SDK wordt de verificatie geïmplementeerd op dezelfde manier voor het eindpunt van de tabellen en de aangepaste API's.  Als u wilt verificatie toevoegen aan de API die in de vorige sectie zijn ontwikkeld, een **toegang** eigenschap:

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

Hetzelfde token dat wordt gebruikt voor het eindpunt tabellen moet worden gebruikt voor aangepaste API's waarvoor verificatie is vereist.

### <a name="howto-customapi-auth"></a>Hoe: grote bestandsuploads verwerken
Azure Mobile Apps SDK gebruikt de [hoofdtekst parser middleware](https://github.com/expressjs/body-parser) om te accepteren en de inhoud van de hoofdtekst van de inzending decoderen.  Instantie-parser voor het accepteren van groter bestand uploadt, kunt u vooraf configureren:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Het bestand is base 64-codering voor verzending.  Dit verhoogt de grootte van het uploaden van de werkelijke (en daarom de grootte u moet rekening houden met).

### <a name="howto-customapi-sql"></a>Hoe: uitvoeren van aangepaste SQL-instructies
De Azure Mobile Apps SDK biedt toegang tot de volledige Context via het request-object, zodat u eenvoudig met parameters SQL-instructies aan de gedefinieerde gegevensprovider uitvoeren:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Foutopsporing, gemakkelijk tabellen en eenvoudige API 's
### <a name="howto-diagnostic-logs"></a>Procedure: fouten opsporen, diagnoses stellen en problemen met Azure Mobile apps
De Azure App Service biedt verschillende opsporen en oplossen van technieken voor Node.js-toepassingen.
Raadpleeg de volgende artikelen om te beginnen bij het oplossen van uw back-end voor Node.js Mobile:

* [Bewaking van een Azure App Service]
* [Diagnostische logboekregistratie in Azure App Service inschakelen]
* [Een Azure App Service in Visual Studio oplossen]

Node.js-toepassingen hebben toegang tot een breed scala aan extra diagnostische logboeken.  De Azure Mobile Apps Node.js SDK gebruikt intern, [Winston] voor diagnostische logboekregistratie.  Logboekregistratie is standaard ingeschakeld door in te schakelen foutopsporingsmodus of door de **MS_DebugMode** app-instelling in op true in de [Azure-portal]. Gegenereerde logboeken worden weergegeven in de logboeken met diagnostische gegevens op de [Azure-portal].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Procedure: werken met gemakkelijk tabellen in de Azure portal
Eenvoudig tabellen in de portal kunnen u maken en werken met tabellen rechts in de portal. U kunt de gegevensset uploaden naar gemakkelijk tabellen in CSV-indeling. Houd er rekening mee dat u namen van eigenschappen (in uw gegevensset CSV) die strijdig zijn met system eigenschappen namen van back-end van Azure Mobile Apps niet gebruiken. De namen van de systeem-eigenschappen zijn:
* CreatedAt
* updatedAt
* verwijderd
* versie

U kunt zelfs tabelbewerkingen met de App Service-Editor bewerken. Wanneer u klikt op **gemakkelijk tabellen** in uw back-end voor site-instellingen kunt u toevoegen, wijzigen of verwijderen van een tabel. U ziet ook de gegevens in de tabel.

![Werken met gemakkelijk tabellen](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

De volgende opdrachten zijn beschikbaar op de opdrachtbalk voor een tabel:

* **Machtigingen wijzigen** - wijzigen van de machtiging voor lezen, invoegen, bijwerken en verwijderen van bewerkingen op de tabel.
  Opties zijn voor het toestaan van anonieme toegang voor authenticatie, of alle toegang tot de bewerking uitschakelen.
* **Bewerk script** -het scriptbestand voor de tabel in de App Service-Editor wordt geopend.
* **Schema beheren** - toevoegen of kolommen verwijderen of wijzigen van de tabelindex.
* **Tabel wissen** -afgekapt van een bestaande tabel verwijderen van alle gegevensrijen, maar als u het schema ongewijzigd.
* **Verwijderen van rijen** -afzonderlijke rijen met gegevens te verwijderen.
* **Weergave streaminglogboeken** -Hiermee gaat u naar de streaming log-service voor uw site.

### <a name="work-easy-apis"></a>Procedure: werken met eenvoudige API's in de Azure portal
Eenvoudige API's in de portal kunt u maken en werken met aangepaste API's rechts in de portal. U kunt de API-scripts met de App Service-Editor bewerken.

Wanneer u klikt op **eenvoudige API's** in uw back-end voor site-instellingen kunt u toevoegen, wijzigen of verwijderen van een aangepaste API-eindpunt.

![Werken met eenvoudige API 's](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

In de portal kunt u de toegangsmachtigingen voor een bepaalde HTTP-actie wijzigen, het scriptbestand API in App Service Editor bewerken of de streaminglogboeken weergeven.

### <a name="online-editor"></a>Hoe: code in de App Service Editor bewerken
De Azure-portal kunt u uw Node.js-back-end-scriptbestanden in de App Service Editor bewerken zonder te hoeven downloaden van het project op uw lokale computer. Scriptbestanden in de online-editor bewerken:

1. Klik op de blade van uw mobiele App-back-end **alle instellingen** > beide **gemakkelijk tabellen** of **eenvoudige API's**, klikt u op een tabel of de API en klik vervolgens op **script bewerken**. Het scriptbestand wordt geopend in de App Service-Editor.

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
[Android Client Quick Start]: app-service-mobile-android-get-started.md
[Apache Cordova-Client Quick Start]: app-service-mobile-cordova-get-started.md
[iOS-Client Quick Start]: app-service-mobile-ios-get-started.md
[Quick Start voor Xamarin.iOS-Client]: app-service-mobile-xamarin-ios-get-started.md
[Snelstartgids voor Xamarin.Android-Client]: app-service-mobile-xamarin-android-get-started.md
[Quick Start Xamarin.Forms-Client]: app-service-mobile-xamarin-forms-get-started.md
[Windows Store-Client Quick Start]: app-service-mobile-windows-store-dotnet-get-started.md
[offline gegevenssynchronisatie]: app-service-mobile-offline-data-sync.md
[Het configureren van Azure Active Directory-verificatie]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook-verificatie configureren]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Google-verificatie configureren]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Het configureren van Microsoft Authentication]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter-verificatie configureren]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
[Implementatiehandleiding voor Azure App Service]: ../app-service/app-service-deploy-local-git.md
[Bewaking van een Azure App Service]: ../app-service/web-sites-monitor.md
[Diagnostische logboekregistratie in Azure App Service inschakelen]: ../app-service/web-sites-enable-diagnostic-log.md
[Een Azure App Service in Visual Studio oplossen]: ../app-service/web-sites-dotnet-troubleshoot-visual-studio.md
[Geef de versie van het knooppunt]: ../nodejs-specify-node-version-azure-apps.md
[knooppunt-modules gebruiken]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure-portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[belofte]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp voorbeeld op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo-voorbeeld op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[directory voorbeelden op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[1.1 van Node.js-hulpprogramma's voor Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js pakket]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
