---
title: Over het werken met de Node.js-back-end Server SDK voor Mobile Apps | Microsoft Docs
description: Informatie over het werken met de Node.js-back-end Server SDK voor Azure App Service Mobile Apps.
services: app-service\mobile
documentationcenter: ''
author: elamalani
manager: elamalani
editor: ''
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: e435baf4a5e3382e3e77195e5315299e3963ff5c
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409255"
---
# <a name="how-to-use-the-mobile-apps-nodejs-sdk"></a>Het gebruik van de Mobile Apps Node.js-SDK

[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

In dit artikel bevat gedetailleerde informatie en voorbeelden die laten zien hoe u werkt met een Node.js-back-end in de functie Mobile Apps van Azure App Service.

## <a name="Introduction"></a>Inleiding

Mobile Apps biedt de mogelijkheid een geoptimaliseerd voor mobiel toegang tot Web-API toevoegen aan een web-App. De Mobile Apps SDK wordt geboden voor ASP.NET- en Node.js-webtoepassingen. De SDK biedt de volgende bewerkingen:

* Tabelbewerkingen (lezen, invoegen, bijwerken en verwijderen) voor toegang tot gegevens
* Aangepaste API-bewerkingen

Beide bewerkingen opgeven voor verificatie voor alle id-providers waarmee Azure App Service. Deze providers zijn sociale id-providers, zoals Facebook, Twitter, Google en Microsoft, evenals Azure Active Directory voor de id van de onderneming.

U kunt ook voorbeelden vinden voor elk use-case in het [map van de voorbeelden op GitHub].

## <a name="supported-platforms"></a>Ondersteunde platforms

De Mobile Apps Node.js SDK biedt ondersteuning voor de huidige LTS-release van knooppunt en hoger. Op dit moment is de meest recente versie van de LTS knooppunt v4.5.0. Andere versies van knooppunt werken mogelijk, maar worden niet ondersteund.

De Mobile Apps Node.js SDK ondersteunt twee stuurprogramma's voor database: 

* Het knooppunt mssql-stuurprogramma biedt ondersteuning voor Azure SQL Database en de lokale SQL Server-exemplaren.  
* Het stuurprogramma sqlite3 ondersteunt SQLite-databases op slechts één exemplaar.

### <a name="howto-cmdline-basicapp"></a>Een eenvoudige back-end voor Node.js maken met behulp van de opdrachtregel

Elke back-end van Mobile Apps Node.js wordt gestart als een toepassing ExpressJS. ExpressJS is het meest populaire web service-framework beschikbaar voor Node.js. U kunt een eenvoudige maken [Express] toepassing als volgt te werk:

1. Maak een map voor uw project in een opdracht of het PowerShell-venster:

        mkdir basicapp

1. Voer `npm init` initialiseren van de pakketstructuur:

        cd basicapp
        npm init

   De `npm init` opdracht vraagt een reeks vragen om te initialiseren van het project. Zie het voorbeeld van uitvoer:

   ![De uitvoer van de init npm][0]

1. Installeer de `express` en `azure-mobile-apps` bibliotheken van de npm-opslagplaats:

        npm install --save express azure-mobile-apps

1. Maak een app.js-bestand voor het implementeren van de eenvoudige mobiele-server:

    ```javascript
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
    ```

Deze toepassing maakt een Web-API voor geoptimaliseerd voor mobiel met één eindpunt (`/tables/TodoItem`) dat niet-geverifieerde toegang biedt tot een onderliggende SQL-gegevensopslag met behulp van een dynamisch schema. Het is geschikt voor het volgen van de client-bibliotheek snelstartgidsen:

* [Snelstartgids voor android-client]
* [Snelstartgids voor Apache Cordova-client]
* [iOS Client Quick Start]
* [Snelstartgids voor Windows Store-client]
* [Snelstartgids voor Xamarin.iOS-client]
* [Snelstartgids voor Xamarin.Android-client]
* [Snelstartgids voor Xamarin.Forms-client]

U vindt de code voor deze eenvoudige toepassing in de [basicapp voorbeeld op GitHub].

### <a name="howto-vs2015-basicapp"></a>Een Node.js-back-end maken met behulp van Visual Studio 2015

Visual Studio 2015 is vereist voor het ontwikkelen van Node.js-toepassingen vanuit de IDE. Als u wilt starten, installeert de [Node.js-hulpprogramma's 1.1 voor Visual Studio]. Wanneer u klaar bent met de installatie, moet u een snelle 4.x-toepassing maken:

1. Open de **nieuw Project** in het dialoogvenster (van **bestand** > **nieuw** > **Project**).
1. Vouw **sjablonen** > **JavaScript** > **Node.js**.
1. Selecteer **Basic Azure Node.js Express 4-toepassing**.
1. Vul de projectnaam in. Selecteer **OK**.

   ![Nieuw project voor Visual Studio 2015][1]
1. Met de rechtermuisknop op de **npm** knooppunt en selecteert u **nieuwe installatie van npm-pakketten**.
1. Mogelijk moet u de catalogus npm vernieuwen nadat u uw eerste Node.js-toepassing maken. Selecteer **vernieuwen** indien nodig.
1. Voer **azure-mobile-apps** in het zoekvak in. Selecteer de **azure mobile apps 2.0.0** verpakt en selecteer vervolgens **pakket installeren**.

   ![Nieuwe npm-pakketten installeren][2]
1. Selecteer **sluiten**.
1. Open het bestand app.js om toe te voegen ondersteuning voor de Mobile Apps SDK. Op regel 6 at onder aan de bibliotheek `require` overzichten, voeg de volgende code:

    ```javascript
    var bodyParser = require('body-parser');
    var azureMobileApps = require('azure-mobile-apps');
    ```

    27 op ongeveer regel na de andere `app.use` overzichten, voeg de volgende code:

    ```javascript
    app.use('/users', users);

    // Mobile Apps initialization
    var mobile = azureMobileApps();
    mobile.tables.add('TodoItem');
    app.use(mobile);
    ```

    Sla het bestand op.

1. Ofwel de toepassing lokaal uitvoeren (de API wordt geleverd op http://localhost:3000) publiceren naar Azure.

### <a name="create-node-backend-portal"></a>Een Node.js-back-end maken met behulp van de Azure-portal

Kunt u een mobiele back-end Apps rechtstreeks in de [Azure Portal]. U kunt de volgende stappen uit of een client en server samen te maken door de volgende de [maken van een mobiele app](app-service-mobile-ios-get-started.md) zelfstudie. De zelfstudie bevat een vereenvoudigde versie van deze instructies en wordt aanbevolen voor proof-of-concept-projecten.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Terug in de **aan de slag** deelvenster onder **maken van een tabel-API**, kiest u **Node.js** als de taal voor uw back-end.
Selecteer het selectievakje in voor **ik erken dat hiermee alle site-inhoud wordt overschreven**, en selecteer vervolgens **takentabel maken**.

### <a name="download-quickstart"></a>Het CodeProject van de Node.js-back-end-snelstartgids downloaden met behulp van Git

Wanneer u een back-end voor Node.js mobiele Apps maakt met behulp van de portal **Quick start** in het deelvenster een Node.js-project is voor u gemaakt en geïmplementeerd op uw site. U kunt in de portal, tabellen en API's toevoegen en bewerken van codebestanden voor de Node.js-back-end. U kunt ook verschillende-hulpprogramma's gebruiken voor het downloaden van het back-end-project, zodat u kunt toevoegen of wijzigen van tabellen en API's en vervolgens het project opnieuw te publiceren. Zie voor meer informatie de [Azure App Service-Implementatiehandleiding].

De volgende procedure maakt gebruik van een Git-opslagplaats voor het downloaden van de snelstartcode project:

1. Installeer Git, als u dat nog niet hebt gedaan. De stappen die nodig zijn om Git te installeren variëren tussen besturingssystemen. Zie voor specifieke besturingssysteem-distributies en installatie [installeren van Git](https://git-scm.com/book/en/Getting-Started-Installing-Git).
1. Zie [voorbereiden van uw opslagplaats](../app-service/app-service-deploy-local-git.md#prepare-your-repository) om in te schakelen van de Git-opslagplaats voor uw back-end-site. Maak een notitie van de implementatie-gebruikersnaam en wachtwoord.
1. In het deelvenster voor uw Mobile Apps-back-end, maak een notitie van de **Git-kloon-URL** instelling.
1. Voer de `git clone` met behulp van de Git-kloon-URL. Voer uw wachtwoord indien nodig, zoals in het volgende voorbeeld:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

1. Blader naar de lokale map (`/todolist` in het voorgaande voorbeeld), en u ziet dat project-bestanden zijn gedownload. Ga naar het bestand todoitem.json in de `/tables` directory. Dit bestand definieert machtigingen voor de tabel. Ook vindt u het bestand todoitem.js in dezelfde map. Hiermee definieert u de scripts CRUD bewerking voor de tabel.
1. Nadat u wijzigingen in de project-bestanden aanbrengt, voer de volgende opdrachten om toe te voegen, doorvoeren en de wijzigingen vervolgens uploaden naar de site:

        $ git commit -m "updated the table script"
        $ git push origin master

   Wanneer u nieuwe bestanden aan het project toevoegt, moet u eerst om uit te voeren de `git add .` opdracht.

De site wordt gepubliceerd telkens wanneer een nieuwe set doorvoeracties naar de site wordt gepusht.

### <a name="howto-publish-to-azure"></a>Uw Node.js-back-end publiceren naar Azure

Microsoft Azure biedt veel mechanismen voor het publiceren van uw Mobile Apps Node.js-back-end voor de Azure-service. Implementatiehulpprogramma's geïntegreerd in Visual Studio, opdrachtregelprogramma's en continue implementatie-opties op basis van het besturingselement van deze mechanismen zijn. Zie voor meer informatie de [Azure App Service-Implementatiehandleiding].

Azure App Service heeft specifiek advies voor Node.js-toepassingen die u moet doorlezen voordat u de back-end publiceren:

* Hoe u [Geef de versie van het knooppunt]
* Hoe u [Node-modules gebruiken]

### <a name="howto-enable-homepage"></a>Een startpagina voor uw toepassing inschakelen

Veel toepassingen zijn een combinatie van web- en mobiele apps. Het framework ExpressJS kunt u de twee facetten combineren. Soms, maar mogelijk wilt u alleen een mobiele-interface te implementeren. Het is handig voor een startpagina om ervoor te zorgen dat de appservice ingeschakeld is en wordt uitgevoerd. U kunt uw eigen startpagina bieden of inschakelen van de startpagina van een tijdelijke. Als u wilt inschakelen op de startpagina van een tijdelijke, gebruik de volgende code exemplaar maken van Mobile Apps:

```javascript
var mobile = azureMobileApps({ homePage: true });
```

Als u wilt dat alleen deze optie beschikbaar bij het lokaal ontwikkelen, kunt u deze instelling toevoegen aan uw azureMobile.js-bestand.

## <a name="TableOperations"></a>Tabelbewerkingen

De azure-mobile-apps Node.js Server SDK biedt mechanismen als gegevenstabellen worden opgeslagen in Azure SQL Database als een Web-API beschikbaar wilt maken. Het biedt vijf bewerkingen:

| Bewerking | Description |
| --- | --- |
| GET-/tables/*tablename* |Haal alle records in de tabel. |
| GET-/tables/*tablename*/:id |Een specifieke record ophalen in de tabel. |
| POST /tables/*tablename* |Een record maken in de tabel. |
| PATCH /tables/*tablename*/:id |Een record in de tabel niet bijwerken. |
| VERWIJDEREN /tables/*tablename*/:id |Een record in de tabel verwijderen. |

Deze Web-API ondersteunt [OData] en breidt u het tabelschema ter ondersteuning van [offline gegevenssynchronisatie].

### <a name="howto-dynamicschema"></a>Tabellen met behulp van een dynamisch schema definiëren

Voordat u een tabel gebruiken kunt, moet u deze definiëren. U kunt tabellen definiëren met behulp van een statisch schema (waar u definieert de kolommen in het schema) of dynamisch (wanneer de SDK Hiermee bepaalt u het schema op basis van binnenkomende aanvragen). Bovendien kunt u specifieke aspecten van de Web-API beheren door JavaScript-code toe te voegen aan de definitie.

Als een best practice, moet u elke tabel definiëren in een JavaScript-bestand in de `tables` directory en gebruik vervolgens de `tables.import()` methode om de tabellen te importeren. Het voorbeeld van de basic-app is uitgebreid, zou u het app.js-bestand aanpassen:

```javascript
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
```

In de tabel in definiëren. / tables/TodoItem.js:

```javascript
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// Additional configuration for the table goes here.

module.exports = table;
```

Tabellen maken standaard gebruik van een dynamisch schema. Als u wilt uitschakelen het dynamische schema wereldwijd, stel de `MS_DynamicSchema` app-instelling op false in Azure portal.

U vindt een compleet voorbeeld in de [todo-voorbeeld op GitHub].

### <a name="howto-staticschema"></a>Tabellen met behulp van een statisch schema definiëren

U kunt expliciet aangeven welke kolommen om beschikbaar te stellen via de Web-API. De Node.js-SDK van azure-mobile-apps worden automatisch toegevoegd voor elke extra kolommen die vereist zijn voor de offline gegevenssynchronisatie aan de lijst die u opgeeft. Bijvoorbeeld een tabel met twee kolommen vereisen dat in de Quick Start-clienttoepassingen: `text` (een tekenreeks) en `complete` (een Booleaanse waarde).  
De tabel kan worden gedefinieerd in de tabel de definitie van JavaScript-bestand (zich in de `tables` directory) als volgt:

```javascript
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
```

Als u tabellen statisch definieert, moet u ook aanroepen de `tables.initialize()` methode voor het maken van het databaseschema bij het opstarten. De `tables.initialize()` methode retourneert een [belofte] zodat de webservice geen aanvragen dienen voordat de database is geïnitialiseerd.

### <a name="howto-sqlexpress-setup"></a>SQL Server Express gebruiken als een gegevensarchief ontwikkeling op uw lokale computer

De Mobile Apps Node.js SDK biedt drie opties voor die gegevens buiten het vak:

* Gebruik de **geheugen** -stuurprogramma voor een voorbeeld van de niet-permanente opslag.
* Gebruik de **mssql** stuurprogramma voor een SQL Server Express-gegevensarchief voor ontwikkeling.
* Gebruik de **mssql** stuurprogramma voor een Azure SQL Database-gegevensarchief voor productie.

De Mobile Apps Node.js-SDK gebruikt de [mssql Node.js-pakket] voor het maken van een verbinding met SQL Server Express en SQL-Database gebruiken. Dit pakket is vereist dat u TCP-verbindingen op uw exemplaar van SQL Server Express inschakelen.

> [!TIP]
> Het stuurprogramma geheugen biedt geen een volledige set faciliteiten voor het testen. Als u uw back-end lokaal testen wilt, raden wij het gebruik van een SQL Server Express-gegevensarchief en de mssql-stuurprogramma.

1. Download en installeer [Microsoft SQL Server 2014 Express]. Zorg ervoor dat u de SQL Server Express 2014 met hulpprogramma's-editie installeert. Tenzij u expliciet 64-bits-ondersteuning nodig hebt, neemt de 32-bits versie minder geheugen bij het uitvoeren van.
1. Voer SQL Server 2014 Configuration Manager:

   a. Vouw de **SQL Server-netwerkconfiguratie** knooppunt in het menu van de structuur.

   b. Selecteer **protocollen voor SQLEXPRESS**.

   c. Met de rechtermuisknop op **TCP/IP** en selecteer **inschakelen**. Selecteer **OK** in het pop-updialoogvenster.

   d. Met de rechtermuisknop op **TCP/IP** en selecteer **eigenschappen**.

   e. Selecteer de **IP-adressen** tabblad.

   f. Zoek de **IPAll** knooppunt. In de **TCP-poort** veld **1433**.

      ![SQL Server Express configureren voor TCP/IP][3]

   g. Selecteer **OK**. Selecteer **OK** in het pop-updialoogvenster.

   h. Selecteer **SQL Server Services** in het menu van de structuur.

   i. Met de rechtermuisknop op **SQL Server (SQLEXPRESS)** en selecteer **opnieuw**.

   j. Sluit SQL Server 2014 Configuration Manager.

1. Voer SQL Server 2014 Management Studio en maak verbinding met uw lokale exemplaar van SQL Server Express:

   1. Met de rechtermuisknop op het exemplaar in Object Explorer en selecteer **eigenschappen**.
   1. Selecteer de **Security** pagina.
   1. Zorg ervoor dat **modus van SQL Server en Windows-verificatie** is geselecteerd.
   1. Selecteer **OK**.

      ![SQL Server Express-verificatie configureren][4]
   1. Vouw **Security** > **aanmeldingen** in Object Explorer.
   1. Met de rechtermuisknop op **aanmeldingen** en selecteer **nieuwe aanmelding**.
   1. Voer een aanmeldingsnaam. Selecteer **SQL Server-verificatie**. Geef een wachtwoord op en voer vervolgens het wachtwoord in **wachtwoord bevestigen**. Het wachtwoord moet voldoen aan complexiteitsvereisten van Windows.
   1. Selecteer **OK**.

      ![Een nieuwe gebruiker toevoegen aan SQL Server Express][5]
   1. Met de rechtermuisknop op uw nieuwe aanmelding en selecteer **eigenschappen**.
   1. Selecteer de **serverfuncties** pagina.
   1. Schakel het selectievakje in voor de **dbcreator** serverfunctie.
   1. Selecteer **OK**.
   1. Sluit SQL Server 2015 Management Studio.

Zorg ervoor dat de gebruikersnaam en wachtwoord die u hebt geselecteerd. U moet mogelijk extra server-rollen of machtigingen, afhankelijk van uw vereisten voor de database toewijst.

De Node.js-toepassing leest de `SQLCONNSTR_MS_TableConnectionString` omgevingsvariabele voor de verbindingsreeks voor deze database. U kunt deze variabele instellen in uw omgeving. Bijvoorbeeld, kunt u PowerShell gebruiken om in te stellen deze omgevingsvariabele:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Toegang tot de database via een TCP/IP-verbinding. Geef een gebruikersnaam en wachtwoord voor de verbinding.

### <a name="howto-config-localdev"></a>Configureren van uw project voor lokale ontwikkeling

Mobile Apps leest een JavaScript-bestand met de naam *azureMobile.js* van het lokale bestandssysteem. Gebruik dit bestand niet naar de Mobile Apps SDK configureren in de productieomgeving. In plaats daarvan gebruik **App-instellingen** in de [Azure Portal].

Het bestand azureMobile.js moet een configuratieobject exporteren. De meest voorkomende instellingen zijn:

* Database-instellingen
* Instellingen voor diagnostische logboekregistratie
* Alternatieve CORS-instellingen

In dit voorbeeld **azureMobile.js** bestand implementeert de voorgaande database-instellingen:

```javascript
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
```

We raden u aan **azureMobile.js** aan uw **.gitignore** bestand (of andere broncodebeheer bestand negeren) om te voorkomen dat wachtwoorden worden opgeslagen in de cloud. Configureer altijd dat instellingen voor productie in **App-instellingen** binnen de [Azure Portal].

### <a name="howto-appsettings"></a>App-instellingen voor uw mobiele app configureren

De meeste instellingen in het bestand azureMobile.js hebben een gelijkwaardige app-instelling in de [Azure Portal]. Gebruik de volgende lijst om te configureren in uw app **App-instellingen**:

| App-instelling | azureMobile.js instelling | Description | Geldige waarden |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |naam |Naam van de app |string |
| **MS_MobileLoggingLevel** |Logging.level |Minimaal logboekniveau van berichten aan te melden |fout, waarschuwing, informatie, uitgebreid, foutopsporing, stom |
| **MS_DebugMode** |fouten opsporen |Hiermee schakelt de foutopsporingsmodus in of |waar of ONWAAR |
| **MS_TableSchema** |data.schema |De schemanaam standaard voor SQL-tabellen |tekenreeks (standaard: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Hiermee schakelt de foutopsporingsmodus in of |waar of ONWAAR |
| **MS_DisableVersionHeader** |versie (ingesteld op niet-gedefinieerde) |Schakelt de header X-ZUMO-Server-versie |waar of ONWAAR |
| **MS_SkipVersionCheck** |skipversioncheck |Hiermee schakelt de controle van de client-API-versie |waar of ONWAAR |

Instellen van een app-instelling:

1. Meld u aan bij [Azure Portal].
1. Selecteer **alle resources** of **App Services**, en selecteer vervolgens de naam van uw mobiele app.
1. De **instellingen** deelvenster wordt standaard geopend. Als dit niet zo is, selecteert u **instellingen**.
1. Op de **algemene** in het menu **toepassingsinstellingen**.
1. Schuif naar de **App-instellingen** sectie.
1. Als uw app-instelling al bestaat, selecteert u de waarde van de app-instelling om de waarde te bewerken.
   Als uw app-instelling niet bestaat, voert u de app-instelling in de **sleutel** vak en de waarde in de **waarde** vak.
1. Selecteer **Opslaan**.

Als u de meeste appinstellingen wijzigt, moet service opnieuw worden opgestart.

### <a name="howto-use-sqlazure"></a>SQL Database gebruiken als uw productiegegevens opslaan

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Met behulp van Azure SQL Database als een gegevensarchief is vrijwel identiek voor alle typen voor Azure App Service-toepassing. Als u niet hebt gedaan al, volgt u deze stappen voor het maken van een back-end van Mobile Apps:

1. Meld u aan bij [Azure Portal].
1. Selecteer in de linkerbovenhoek van het venster de **+ nieuw** knop > **Web en mobiel** > **mobiele App**, en geef vervolgens een naam voor uw Mobile Apps-back-end.
1. In de **resourcegroep** voert u dezelfde naam als uw app.
1. De standaardwaarde van App Service-plan is geselecteerd. Als u wijzigen van uw App Service-plan wilt:

   a. Selecteer **App Service-Plan** > **+ nieuwe maken**.

   b. Geef een naam op van de nieuwe App Service-plan en selecteer de juiste locatie.

   c. Selecteer een juiste prijscategorie voor de service. Selecteer **weergeven van alle** naar de weergave meer prijsopties, zoals **gratis** en **gedeelde**.

   d. Klik op de **Selecteer** knop.

   e. Klik in de **App Service-plan** venster **OK**.
1. Selecteer **Maken**.

Inrichting van een mobiele Apps kan-back-end enkele minuten duren. Nadat de Mobile Apps back-end is ingericht, de portal wordt geopend de **instellingen** deelvenster voor de back-end voor mobiele Apps.

U kunt een bestaande SQL-database verbinden met uw back-end van Mobile Apps of maak een nieuwe SQL-database. In deze sectie maken we een SQL-database.

> [!NOTE]
> Als u al een database op dezelfde locatie hebt als de Mobile Apps-back-end, kunt u in plaats daarvan selecteren **gebruik een bestaande database** en selecteer vervolgens die database. Het gebruik van een database in een andere locatie wordt niet aanbevolen vanwege de hogere latentie.

1. Selecteer in het nieuwe Mobile Apps back-end, **instellingen** > **mobiele App** > **gegevens** > **+ toevoegen**.
1. In de **gegevensverbinding toevoegen** venster **SQL Database - vereiste instellingen configureren** > **een nieuwe database maken**. Voer de naam van de nieuwe database in de **naam** vak.
1. Selecteer **Server**. In de **nieuwe server** deelvenster, voer een unieke naam in de **servernaam** in en geef een geschikte server beheerdersaanmelding bij en het wachtwoord. Zorg ervoor dat **azure-services tot server toestaan** is geselecteerd. Selecteer **OK**.

   ![Een Azure SQL-database maken][6]
1. In de **nieuwe database** venster **OK**.
1. Klik in de **gegevensverbinding toevoegen** venster **Connection string**, en voer de aanmeldingsnaam en het wachtwoord die u hebt opgegeven tijdens het maken van de database. Als u een bestaande database gebruikt, bieden u de aanmeldingsreferenties voor die database. Selecteer **OK**.
1. Klik in de **gegevensverbinding toevoegen** deelvenster opnieuw uit, selecteer **OK** om de database te maken.

<!--- END OF ALTERNATE INCLUDE -->

Het maken van de database kan enkele minuten duren. Gebruik de **meldingen** gebied voor het bewaken van de voortgang van de implementatie. Geen voortgang totdat de database is geïmplementeerd. Nadat de database is geïmplementeerd, wordt een verbindingsreeks voor de SQL Database-exemplaar in de instellingen van uw Mobile Apps-back-end-app gemaakt. U kunt zien dat deze app-instelling in **instellingen** > **toepassingsinstellingen** > **verbindingsreeksen**.

### <a name="howto-tables-auth"></a>Verificatie vereisen voor toegang tot tabellen

Als u wilt gebruiken van App Service-verificatie met de `tables` eindpunt, moet u App Service-verificatie in configureren de [Azure Portal] eerste. Zie voor meer informatie de configuratiehandleiding voor de id-provider die u wilt gebruiken:

* [Azure Active Directory-verificatie configureren]
* [Facebook-authenticatie configureren]
* [Google-verificatie configureren]
* [Microsoft-verificatie configureren]
* [Twitter-verificatie configureren]

Elke tabel heeft een access-eigenschap die u gebruiken kunt voor het beheren van toegang tot de tabel. Het volgende voorbeeld ziet u een statisch gedefinieerde tabel met de verificatie is vereist.

```javascript
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
```

De eigenschap access kan duren voordat een van drie waarden:

* *anonieme* geeft aan dat de clienttoepassing voor lezen van gegevens zonder verificatie is toegestaan.
* *geverifieerde* geeft aan dat de clienttoepassing een geldig verificatietoken met de aanvraag moet verzenden.
* *uitgeschakeld* geeft aan dat deze tabel is momenteel uitgeschakeld.

Als de eigenschap access gedefinieerd is, is niet-geverifieerde toegang toegestaan.

### <a name="howto-tables-getidentity"></a>Gebruik verificatie claims met tabellen
U kunt instellen wanneer er verschillende claims die worden aangevraagd als verificatie is ingesteld. Deze claims zijn niet normaal beschikbaar via de `context.user` object. Echter, u kunt ze wel ophalen met behulp van de `context.user.getIdentity()` methode. De `getIdentity()` methode retourneert een promise die wordt omgezet naar een object. Het object is opgegeven met de methode voor netwerkauthenticatie (`facebook`, `google`, `twitter`, `microsoftaccount`, of `aad`).

Als u de verificatie van de Microsoft-account en de e-mailadressen claim aanvraag hebt ingesteld, kunt u bijvoorbeeld het e-mailadres toevoegen aan de record met de volgende tabel controller:

```javascript
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
```

Als u wilt zien welke claims beschikbaar zijn, kunt u een webbrowser gebruiken om weer te geven de `/.auth/me` eindpunt van uw site.

### <a name="howto-tables-disabled"></a>De toegang tot specifieke tabelbewerkingen uitschakelen

Naast het op de tabel wordt weergegeven, kan de eigenschap access worden gebruikt voor het beheren van afzonderlijke bewerkingen. Er zijn vier bewerkingen:

* `read` de RESTful-GET-bewerking op de tabel is.
* `insert` de RESTful POST-bewerking op de tabel is.
* `update` de RESTful-PATCH-bewerking op de tabel is.
* `delete` de bewerking RESTful verwijderen op de tabel is.

U wilt bijvoorbeeld een alleen-lezen niet-geverifieerde tabel bieden:

```javascript
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// Read-only table. Only allow READ operations.
table.read.access = 'anonymous';
table.insert.access = 'disabled';
table.update.access = 'disabled';
table.delete.access = 'disabled';

module.exports = table;
```

### <a name="howto-tables-query"></a>De query die wordt gebruikt voor tabelbewerkingen aanpassen

Een algemene vereiste voor tabelbewerkingen wordt een beperkte weergave van de gegevens. U kunt bijvoorbeeld opgeven dat een tabel die is gecodeerd met de geverifieerde gebruikers-ID, zodat u kunt alleen lezen of uw eigen records bij te werken. De definitie van de volgende tabel biedt deze functionaliteit:

```javascript
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
```

Bewerkingen die normaal gesproken een query worden uitgevoerd, hebben de eigenschap van een query die u aanpassen met behulp van kunt een `where` component. De eigenschap van de query is een [QueryJS] object dat wordt gebruikt voor het converteren van een OData-query naar iets dat de gegevens back-end kan worden verwerkt. Voor eenvoudige gelijkheid gevallen (zoals de voorgaande afbeelding), kunt u een kaart. U kunt ook specifieke SQL-clausules toevoegen:

```javascript
context.query.where('myfield eq ?', 'value');
```

### <a name="howto-tables-softdelete"></a>Een voorlopige verwijdering configureren op een tabel

Een functie voor voorlopig verwijderen worden de records niet daadwerkelijk verwijderd. In plaats daarvan markeren het als verwijderd in de database door de verwijderde kolom instellen op true. De Mobile Apps SDK verwijdert automatisch voorlopig verwijderde records uit de resultaten, tenzij de mobiele Client-SDK gebruikt `IncludeDeleted()`. Voor het configureren van een tabel voor een functie voor voorlopig verwijderen, stelt de `softDelete` eigenschap in het definitiebestand van de tabel:

```javascript
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
```

U moet een mechanisme voor het verwijderen van records instellen: een clienttoepassing, een webtaak, een Azure-functie of een aangepaste API.

### <a name="howto-tables-seeding"></a>Seeden van uw database met gegevens

Wanneer u een nieuwe toepassing maakt, wilt u mogelijk seed-een tabel met gegevens. U kunt dit doen in de tabel de definitie van JavaScript-bestand als volgt:

```javascript
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
```

Seeding van de gegevens gebeurt alleen als u de Mobile Apps SDK hebt gebruikt om de tabel te maken. Als de tabel al in de database bestaat, wordt er geen gegevens opgenomen in de tabel. Als het dynamische schema is ingeschakeld, wordt het schema is afgeleid van de geseede gegevens.

Het is raadzaam dat u expliciet aanroepen de `tables.initialize()` methode voor het maken van de tabel wanneer de service wordt uitgevoerd.

### <a name="Swagger"></a>Ondersteuning voor Swagger inschakelen
Mobile Apps wordt geleverd met ingebouwde [Swagger] ondersteunen. Swagger als ondersteuning wilt inschakelen, moet u eerst swagger-ui installeren als een afhankelijkheid:

    npm install --save swagger-ui

Vervolgens kunt u Swagger-ondersteuning in de constructor van Mobile Apps inschakelen:

```javascript
var mobile = azureMobileApps({ swagger: true });
```

U waarschijnlijk slechts wilt inschakelen Swagger-ondersteuning in de ontwikkeling-edities. U kunt dit doen met behulp van de `NODE_ENV` app-instelling:

```javascript
var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });
```

De `swagger` eindpunt bevindt zich op http://*uw site*.azurewebsites.net/swagger. U hebt toegang tot de Swagger-gebruikersinterface via de `/swagger/ui` eindpunt. Als u ervoor verificatie vereist is voor de gehele toepassing kiest, Swagger, treedt een fout. Kies voor optimale resultaten, om toe te staan van niet-geverifieerde aanvragen in de Azure App Service-verificatie/autorisatie-instellingen en vervolgens verificatie beheren met behulp van de `table.access` eigenschap.

U kunt ook de Swagger-optie toe aan het bestand azureMobile.js toevoegen als u wilt dat alleen ondersteuning voor Swagger voor het lokaal ontwikkelen.

## <a name="a-namepushpush-notifications"></a><a name="push"/>Pushmeldingen verzenden

Mobile Apps kan worden geïntegreerd met Azure Notification Hubs, zodat u gerichte pushmeldingen naar miljoenen apparaten voor alle grote platformen verzenden kunt. U kunt met behulp van Notification Hubs, pushmeldingen verzenden naar iOS, Android en Windows apparaten. Zie voor meer informatie over alles wat u kunt doen met Notification Hubs, de [Notification Hubs-overzicht](../notification-hubs/notification-hubs-push-notification-overview.md).

### <a name="send-push"></a>Pushmeldingen verzenden

De volgende code ziet u hoe u de `push` -object voor verzending van een pushmelding op ingeschreven iOS-apparaten:

```javascript
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
```

Als u een sjabloon voor push-registratie van de client maakt, kunt u in plaats daarvan een sjabloon voor push-bericht verzenden naar apparaten op alle ondersteunde platforms. De volgende code laat zien hoe een sjabloon-melding te verzenden:

```javascript
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
```

### <a name="push-user"></a>Pushmeldingen verzenden naar een geverifieerde gebruiker met behulp van tags
Wanneer een geverifieerde gebruiker geregistreerd voor pushmeldingen, wordt automatisch een label-ID toegevoegd aan de registratie. Met behulp van deze tag kunt u pushmeldingen verzenden naar alle apparaten die zijn geregistreerd door een specifieke gebruiker. De volgende code wordt de SID van gebruiker door wie de aanvraag wordt uitgevoerd en verzendt een pushmelding sjabloon naar elke device Registration service voor die gebruiker:

```javascript
// Only do the push if configured.
if (context.push) {
    // Send a notification to the current user.
    context.push.send(context.user.id, payload, function (error) {
        if (error) {
            // Do something or log the error.
        }
    });
}
```

Wanneer u bent geregistreerd voor pushmeldingen vanuit een geverifieerde client, zorg ervoor dat de verificatie is voltooid voordat u inschrijving.

## <a name="CustomAPI"></a> Aangepaste API 's

### <a name="howto-customapi-basic"></a>Een aangepaste API definiëren

Naast de Data Access-API via de `/tables` eindpunt, Mobile Apps krijgt u een aangepaste API-dekking. Aangepaste API's in dezelfde manier als de tabeldefinities worden gedefinieerd en hebben toegang tot dezelfde faciliteiten, met inbegrip van verificatie.

Als u gebruiken van App Service-verificatie met een aangepaste API wilt, moet u App Service-verificatie in configureren de [Azure Portal] eerste. Zie voor meer informatie de configuratiehandleiding voor de id-provider die u wilt gebruiken:

* [Azure Active Directory-verificatie configureren]
* [Facebook-authenticatie configureren]
* [Google-verificatie configureren]
* [Microsoft-verificatie configureren]
* [Twitter-verificatie configureren]

Aangepaste API's zijn gedefinieerd op ongeveer dezelfde manier als de tabel-API:

1. Maak een `api` directory.
1. Maken van een API-definitie JavaScript-bestand in de `api` directory.
1. Gebruik van de importeermethode voor het importeren van de `api` directory.

Hier volgt een prototype van de API-definitie op basis van de steekproef van de basic-app die we eerder hebben gebruikt:

```javascript
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
```

We nemen een voorbeeld-API die de datum van de server met behulp van retourneert de `Date.now()` methode. Dit is het bestand api/date.js:

```javascript
var api = {
    get: function (req, res, next) {
        var date = { currentTime: Date.now() };
        res.status(200).type('application/json').send(date);
    });
};

module.exports = api;
```

Elke parameter is een van de standaard RESTful termen: OPHALEN, POST, PATCH of te verwijderen. De methode is een standaard [ExpressJS middleware] functie waarmee de vereiste uitvoer wordt verzonden.

### <a name="howto-customapi-auth"></a>Verificatie vereisen voor toegang tot een aangepaste API

De Mobile Apps SDK verificatie implementeert op dezelfde manier voor zowel de `tables` eindpunt en aangepaste API's. Verificatie toevoegen aan de API die is ontwikkeld in de vorige sectie, Voeg een `access` eigenschap:

```javascript
var api = {
    get: function (req, res, next) {
        var date = { currentTime: Date.now() };
        res.status(200).type('application/json').send(date);
    });
};
// All methods must be authenticated.
api.access = 'authenticated';

module.exports = api;
```

U kunt ook verificatie op specifieke bewerkingen opgeven:

```javascript
var api = {
    get: function (req, res, next) {
        var date = { currentTime: Date.now() };
        res.status(200).type('application/json').send(date);
    }
};
// The GET methods must be authenticated.
api.get.access = 'authenticated';

module.exports = api;
```

Het dezelfde token die wordt gebruikt voor de `tables` eindpunt moet worden gebruikt voor aangepaste API's waarvoor verificatie vereist is.

### <a name="howto-customapi-auth"></a>Het uploaden van grote bestanden verwerken

De Mobile Apps SDK gebruikt de [body-parser middleware](https://github.com/expressjs/body-parser) om te accepteren en decoderen van de van de hoofdtekstinhoud in uw inzending. U kunt vooraf configureren body-parser voor het accepteren van grotere bestand wordt geüpload:

```javascript
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
```

Het bestand is base-64 gecodeerd voor verzending. Deze codering neemt de omvang van het uploaden van de werkelijke (en de grootte die u moet rekening).

### <a name="howto-customapi-sql"></a>Aangepaste SQL-instructies uitvoeren

De Mobile Apps SDK biedt toegang tot de volledige context via het request-object. U kunt eenvoudig geparameteriseerde SQL-instructies toe aan de gedefinieerde gegevensprovider uitvoeren:

```javascript
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
```

## <a name="Debugging"></a>Foutopsporing, eenvoudige tabellen en eenvoudige API 's

### <a name="howto-diagnostic-logs"></a>Fouten opsporen, vaststellen en problemen oplossen met Mobile Apps

Azure App Service biedt verschillende opsporen en oplossen van technieken voor Node.js-toepassingen.
Om te beginnen bij het oplossen van uw back-end voor Node.js Mobile Apps, Zie de volgende artikelen:

* [Bewaking van Azure App Service]
* [Diagnostische logboekregistratie inschakelen in Azure App Service]
* [Azure App Service in Visual Studio oplossen]

Node.js-toepassingen hebben toegang tot een breed scala aan hulpprogramma's voor diagnostisch logboek. De Mobile Apps Node.js-SDK gebruikt intern, [Winston] voor registratie in diagnoselogboek. Logboekregistratie is standaard ingeschakeld wanneer u foutopsporing inschakelen modus of stel de `MS_DebugMode` app-instelling op ' True ' in de [Azure Portal]. Gegenereerde logboeken worden weergegeven in de diagnostische logboeken in de [Azure Portal].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Werken met eenvoudige tabellen in Azure portal

U kunt eenvoudige tabellen maken en werken met tabellen direct in de portal. U kunt de gegevensset uploaden naar eenvoudige tabellen in CSV-indeling. Houd er rekening mee dat u Eigenschapsnamen (in uw CSV-gegevensset) die conflicteren met de eigenschapsnamen system van de back-end van Mobile Apps niet gebruiken. De namen van de eigenschap system zijn:
* createdAt
* updatedAt
* verwijderd
* versie

U kunt zelfs tabelbewerkingen bewerken met behulp van App Service-Editor. Wanneer u selecteert **eenvoudige tabellen** in uw back-end-site-instellingen, kunt u toevoegen, wijzigen of verwijderen van een tabel. U ziet ook de gegevens in de tabel.

![Werken met eenvoudige tabellen](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

De volgende opdrachten zijn beschikbaar op de opdrachtbalk voor een tabel:

* **Machtigingen wijzigen**: Wijzigen van de machtiging voor lezen, invoegen, bijwerken en verwijderen van bewerkingen voor de tabel.
 Opties zijn voor het toestaan van anonieme toegang is verificatie vereist, of alle toegang tot de bewerking uitschakelen.
* **Script bewerken**: Het scriptbestand voor de tabel is in App Service-Editor geopend.
* **Schema beheren**: Toevoegen of kolommen verwijderen of wijzigen van de tabelindex.
* **Tabel wissen**: Een bestaande tabel afkappen door het verwijderen van alle rijen met gegevens, maar het schema ongewijzigd laten.
* **Rijen verwijderen**: Afzonderlijke rijen met gegevens verwijderen.
* **Weergave streaminglogboeken**: Verbinding maken met de streaming log-service voor uw site.

### <a name="work-easy-apis"></a>Werken met eenvoudige API's in Azure portal

U kunt eenvoudig API's gebruiken om te maken en werken met aangepaste API's direct in de portal. U kunt de API-scripts bewerken met behulp van App Service-Editor.

Wanneer u selecteert **eenvoudige API's** in uw back-end-site-instellingen, kunt u toevoegen, wijzigen of verwijderen van een aangepaste API-eindpunt.

![Werken met eenvoudige API 's](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

In de portal kunt u de toegangsmachtigingen voor een HTTP-actie wijzigen, bewerkt u het scriptbestand voor API in App Service-Editor of de streaminglogboeken bekijken.

### <a name="online-editor"></a>Bewerken van code in App Service-Editor

Met behulp van de Azure-portal, kunt u uw Node.js-back-end-script-bestanden in App Service-Editor zonder te downloaden van het project naar uw lokale computer. Om scriptbestanden te bewerken in de online-editor:

1. In het deelvenster voor uw Mobile Apps-back-end, selecteer **alle instellingen** > beide **eenvoudige tabellen** of **eenvoudige API's**. Selecteer een tabel of de API, en selecteer vervolgens **script bewerken**. Het scriptbestand wordt geopend in App Service-Editor.

   ![App Service-editor](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
1. Breng uw wijzigingen naar het codebestand in de online-editor. Wijzigingen worden automatisch opgeslagen terwijl u typt.

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Snelstartgids voor android-client]: app-service-mobile-android-get-started.md
[Snelstartgids voor Apache Cordova-Client]: app-service-mobile-cordova-get-started.md
[iOS Client Quick Start]: app-service-mobile-ios-get-started.md
[Snelstartgids voor Xamarin.iOS-Client]: app-service-mobile-xamarin-ios-get-started.md
[Snelstartgids voor Xamarin.Android-Client]: app-service-mobile-xamarin-android-get-started.md
[Snelstartgids voor Xamarin.Forms-Client]: app-service-mobile-xamarin-forms-get-started.md
[Snelstartgids voor Windows Store-Client]: app-service-mobile-windows-store-dotnet-get-started.md
[offline gegevenssynchronisatie]: app-service-mobile-offline-data-sync.md
[Azure Active Directory-verificatie configureren]: ../app-service/configure-authentication-provider-aad.md
[Facebook-authenticatie configureren]: ../app-service/configure-authentication-provider-facebook.md
[Google-verificatie configureren]: ../app-service/configure-authentication-provider-google.md
[Microsoft-verificatie configureren]: ../app-service/configure-authentication-provider-microsoft.md
[Twitter-verificatie configureren]: ../app-service/configure-authentication-provider-twitter.md
[Azure App Service-Implementatiehandleiding]: ../app-service/app-service-deploy-local-git.md
[Bewaking van Azure App Service]: ../app-service/web-sites-monitor.md
[Diagnostische logboekregistratie inschakelen in Azure App Service]: ../app-service/web-sites-enable-diagnostic-log.md
[Azure App Service in Visual Studio oplossen]: ../app-service/web-sites-dotnet-troubleshoot-visual-studio.md
[Geef de versie van het knooppunt]: ../nodejs-specify-node-version-azure-apps.md
[Node-modules gebruiken]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: https://expressjs.com/
[Swagger]: https://swagger.io/

[Azure Portal]: https://portal.azure.com/
[OData]: https://www.odata.org
[Belofte]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp voorbeeld op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[Todo-voorbeeld op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[map van de voorbeelden op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js-hulpprogramma's 1.1 voor Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[MSSQL Node.js-pakket]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: https://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS middleware]: https://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
