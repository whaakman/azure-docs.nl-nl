---
title: Azure AD-Node.js aan de slag | Microsoft Docs
description: "Het bouwen van een REST Node.js-web-API die kan worden geïntegreerd met Azure AD voor verificatie."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 4f58177f540c14172d7ece8b4bc8c8a2b9787f8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-web-apis-for-nodejs"></a>Aan de slag met web-API's voor Node.js
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

*Passport* is verificatiemiddleware voor Node.js. Flexibel en modulair, Passport kan worden onopvallend verwijderd voor een Express- of Restify-webtoepassing. Een uitgebreide set strategieën ondersteunt verificatie met een gebruikersnaam en wachtwoord, Facebook, Twitter en meer. We hebben een strategie ontwikkeld voor Microsoft Azure Active Directory (Azure AD). We installeert deze module en voegt u de Microsoft Azure Active Directory `passport-azure-ad` invoegtoepassing.

Hiervoor doet u het volgende:

1. U registreert een toepassing met Azure AD.
2. Uw app instellen voor het gebruik van de Passport `passport-azure-ad` invoegtoepassing.
3. Configureer een clienttoepassing de takenlijst-web-API niet aanroepen.

De code voor deze zelfstudie wordt onderhouden in [GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).

> [!NOTE]
> In dit artikel dekt niet het implementeren van aanmelding, registratie en Profielbeheer met Azure AD B2C. Dit artikel gaat over het aanroepen van web API's nadat de gebruiker al is geverifieerd.  Het is raadzaam dat u met begint [integreren met Azure Active Directory-document](active-directory-how-to-integrate.md) voor meer informatie over de basisprincipes van Azure Active Directory.
>
>

We de broncode bijvoorbeeld uitgevoerd in GitHub onder een MIT-licentie hebt gepubliceerd, dus kloon (of zelfs beter fork) gerust en feedback geven en pull-aanvragen.

## <a name="about-nodejs-modules"></a>Over Node.js-modules
We Node.js-modules gebruiken in dit scenario. Modules zijn geladen JavaScript-pakketten die specifieke functionaliteit voor uw toepassing bieden. U kunt gewoonlijk modules installeren met behulp van de Node.js een opdrachtregelprogramma NPM in de NPM-installatiemap. Sommige modules, zoals de HTTP-module worden echter opgenomen in het core Node.js-pakket.

Geïnstalleerde modules worden opgeslagen in de **node_modules** map in de hoofdmap van uw Node.js-installatiemap. Elke module in de **node_modules** directory onderhoudt een eigen **node_modules** map waarin zich geen modules die afhankelijk zijn van. Ook elke vereiste module heeft een **node_modules** directory. Deze directory recursieve structuur vertegenwoordigt de afhankelijkheidsketen.

Deze structuur van de keten afhankelijkheid resulteert in een grotere toepassing footprint. Maar ook wordt hiermee gegarandeerd dat alle afhankelijkheden is voldaan en de versie van de modules die wordt gebruikt in ontwikkeling wordt ook gebruikt in productie. Hierdoor wordt het gedrag van de app productie beter voorspelbaar en voorkomt dat versioning problemen die gevolgen voor gebruikers hebben mogelijk.

## <a name="step-1-register-an-azure-ad-tenant"></a>Stap 1: Een Azure AD-tenant registreren
Dit voorbeeld gebruiken, moet u een Azure Active Directory-tenant. Als u niet zeker weet welke tenant krijgen, raadpleegt u [een Azure AD-tenant verkrijgen](active-directory-howto-tenant.md).

## <a name="step-2-create-an-application"></a>Stap 2: Een toepassing maken
Vervolgens maakt u een app in uw directory waarmee informatie over het Azure AD die nodig zijn voor het veilig te communiceren met uw app.  De client-app en de web-API worden aangegeven met één **toepassings-ID** in dit geval omdat ze samen één logische app vormen.  Volg [deze instructies](active-directory-how-applications-are-added.md) om een app te maken. Als u een line-of-business-app bouwt [deze aanvullende instructies nuttig kunnen zijn](../active-directory-applications-guiding-developers-for-lob-applications.md).

Een toepassing maken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer in het bovenste menu uw account. Klik vervolgens onder de **Directory** kiest u de Active Directory-tenant waar u uw toepassing registreren.

3. Selecteer in het menu aan de linkerkant **meer Services**, en selecteer vervolgens **Azure Active Directory**.

4. Selecteer **App registraties**, en selecteer vervolgens **toevoegen**.

5. Volg de aanwijzingen voor het maken van een **webtoepassing en/of WebAPI**.

      * De **naam** beschrijving van de toepassing van uw toepassing aan eindgebruikers.

      * De **aanmeldings-URL** is de basis-URL van uw app.  Is de standaard-URL van de voorbeeldcode `https://localhost:8080`.

6. Nadat u hebt geregistreerd, wijst Azure AD uw app een unieke id U moet deze waarde in de volgende secties, dus kopiëren van de toepassingspagina.

7. Van de **instellingen** -> **eigenschappen** pagina voor uw toepassing, het bijwerken van de App ID URI. De **App ID URI** is de unieke id voor uw toepassing. De overeenkomst is met `https://<tenant-domain>/<app-name>`, bijvoorbeeld: `https://contoso.onmicrosoft.com/my-first-aad-app`.

8. Maak een **sleutel** voor uw toepassing uit de **instellingen** pagina en kopieer het ergens. U moet deze binnenkort.

## <a name="step-3-download-nodejs-for-your-platform"></a>Stap 3: Node.js voor uw platform downloaden
U moet een werkende implementatie van Node.js hebben om dit voorbeeld te kunnen gebruiken.

Installeer Node.js vanuit [http://nodejs.org](http://nodejs.org).

## <a name="step-4-install-mongodb-on-your-platform"></a>Stap 4: Installatie MongoDB op uw platform
Om te kunnen gebruiken in dit voorbeeld, moet u een werkende implementatie van MongoDB hebben. U gebruikt MongoDB om de REST-API persistent ervoor in alle serverexemplaren.

Installeer MongoDB vanuit [http://mongodb.org](http://www.mongodb.org).

> [!NOTE]
> In dit scenario wordt ervan uitgegaan dat u de standaard- en -servereindpunten voor MongoDB, die op het moment van schrijven van dit mongodb://localhost is.
>
>

## <a name="step-5-install-the-restify-modules-in-your-web-api"></a>Stap 5: De Restify-modules installeren in uw web-API
We Restify gebruiken voor het bouwen van de REST-API. Restify is een minimaal en flexibel Node.js-toepassingsframework dat afgeleid van Express. Het bevat een set krachtige functies voor het ontwikkelen van REST-API's op Connect.

### <a name="install-restify"></a>Restify installeren
1. Vanaf de opdrachtregel, wijzig de mappen op de **azuread** directory. Als de **azuread** directory niet bestaat, maakt.

        `cd azuread - or- mkdir azuread; cd azuread`

2. Typ de volgende opdracht:

    `npm install restify`

    Met deze opdracht wordt Restify geïnstalleerd.

#### <a name="did-you-get-an-error"></a>Krijgt u een foutmelding?
Wanneer u NPM op sommige besturingssystemen gebruikt, wordt u er een foutmelding waarin wordt gemeld **fout: EPERM, type chmod ' / usr/lokale/bin /...'** en een suggestie die u probeert het account uitvoeren als beheerder. Als dit het geval is, moet u de sudo-opdracht gebruiken NPM op een hoger niveau van bevoegdheden wordt uitgevoerd.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Krijgt u een fout met betrekking tot DTRACE?
U ziet een fout als volgt wanneer u Restify installeert:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```
Restify biedt een krachtig mechanisme om REST-aanroepen te traceren met behulp van DTrace. Veel besturingssystemen hoeft echter geen DTrace. U kunt deze fouten negeren.

De uitvoer van deze opdracht moet er ongeveer als de volgende uitvoer:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)


## <a name="step-6-install-passportjs-in-your-web-api"></a>Stap 6: Installatie Passport.js in uw web-API
[Passport](http://passportjs.org/) is verificatiemiddleware voor Node.js. Flexibel en modulair, Passport kan worden onopvallend verwijderd voor een Express- of Restify-webtoepassing. Een uitgebreide set strategieën ondersteunt verificatie met een gebruikersnaam en wachtwoord, Facebook, Twitter en meer.

We hebben een strategie ontwikkeld voor Azure Active Directory. We installeert deze module en voeg vervolgens de invoegtoepassing van de strategie voor Azure Active Directory.

1. Vanaf de opdrachtregel, wijzig de mappen op de **azuread** directory.

2. Als u wilt installeren passport.js, voer de volgende opdracht:

    `npm install passport`

    De uitvoer van de opdracht moet er ongeveer als volgt uitzien:

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-to-your-web-api"></a>Stap 7: Passport-Azure-AD toevoegen aan uw web-API
Vervolgens wordt de OAuth-strategie toevoegen met behulp van `passport-azure-ad`, een reeks strategieën die Azure Active Directory met Passport verbinden. We gebruiken deze strategie voor bearer-tokens in dit voorbeeld REST-API.

> [!NOTE]
> Hoewel OAuth2 een kader waarin elk onbekend type token kan worden uitgegeven, worden alleen bepaalde typen worden vaak gebruikt. Bearer-tokens zijn de meest gebruikte tokens voor het beveiligen van eindpunten. Ze zijn het meest wordt uitgegeven token in OAuth2-type. Veel implementaties wordt ervan uitgegaan dat bearer-tokens het enige type tokens die zijn uitgegeven zijn.
>
>

Vanaf de opdrachtregel, wijzig de mappen op de **azuread** directory.

Typ de volgende opdracht voor het installeren van de Passport.js `passport-azure-ad module`:

`npm install passport-azure-ad`

De uitvoer van de opdracht moet er ongeveer als de volgende uitvoer:


    passport-azure-ad@1.0.0 node_modules/passport-azure-ad
    ├── xtend@4.0.0
    ├── xmldom@0.1.19
    ├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
    ├── underscore@1.8.3
    ├── async@1.3.0
    ├── jsonwebtoken@5.0.2
    ├── xml-crypto@0.5.27 (xpath.js@1.0.6)
    ├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
    ├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
    ├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    └── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)



## <a name="step-8-add-mongodb-modules-to-your-web-api"></a>Stap 8: MongoDB-modules toevoegen aan uw web-API
We gebruikt MongoDB als onze gegevensarchief. Daarom moeten we de veelgebruikte invoegtoepassing aangeroepen Mongoose voor het beheren van modellen en schema's te installeren. Ook moet het databasestuurprogramma voor MongoDB (dit wordt ook MongoDB genoemd) te installeren.

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a>Stap 9: Aanvullende modules installeren
Vervolgens wordt de overige vereiste modules installeren.

1. Vanaf de opdrachtregel, wijzig de mappen op de **azuread** map als u niet al er.

    `cd azuread`

2. Voer de volgende opdrachten voor het installeren van deze modules in uw **node_modules** directory:

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a>Stap 10: Een server.js met de afhankelijkheden maken
Het bestand server.js bevat de meeste van de functionaliteit voor onze web API-server. We de meeste code toevoegen aan dit bestand. Voor productiedoeleinden, is het raadzaam dat u de functionaliteit in kleinere bestanden, zoals afzonderlijke routes en controllers opsplitsen. In deze demonstratie gebruiken we server.js voor deze functionaliteit.

1. Vanaf de opdrachtregel, wijzig de mappen op de **azuread** map als u niet al er.

    `cd azuread`

2. Maak een `server.js` bestand in uw favoriete editor en voeg vervolgens de volgende informatie:

    ```Javascript
        'use strict';

        /**
         * Module dependencies.
         */

        var fs = require('fs');
        var path = require('path');
        var util = require('util');
        var assert = require('assert-plus');
        var bunyan = require('bunyan');
        var getopt = require('posix-getopt');
        var mongoose = require('mongoose/');
        var restify = require('restify');
        var passport = require('passport');
      var BearerStrategy = require('passport-azure-ad').BearerStrategy;
    ```

3. Sla het bestand op. We keert kort terug naar deze.

## <a name="step-11-create-a-config-file-to-store-your-azure-ad-settings"></a>Stap 11: Maak een configuratiebestand voor het opslaan van uw Azure AD-instellingen
Dit codebestand geeft de configuratieparameters van uw Azure Active Directory-portal aan Passport.js. U hebt deze configuratiewaarden gemaakt toen u de web-API toegevoegd aan de portal in het eerste deel van de procedure. Wanneer u de code hebt gekopieerd, wordt uitgelegd wat u in de waarden van deze parameters moet zetten.

1. Vanaf de opdrachtregel, wijzig de mappen op de **azuread** map als u niet al er.

    `cd azuread`

2. Maak een `config.js` bestand in uw favoriete editor en voeg vervolgens de volgende informatie:

    ```Javascript
         exports.creds = {
             mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
             clientID: 'your client ID',
             audience: 'your application URL',
            // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
          // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
             identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
             validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
             passReqToCallback: false,
             loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

         };
    ```
3. Sla het bestand op.

## <a name="step-12-add-configuration-values-to-your-serverjs-file"></a>Stap 12: Configuratiewaarden toevoegen aan het bestand server.js
We moeten deze waarden lezen uit het .config-bestand dat u hebt gemaakt in de toepassing. U doet dit door toevoegen we het .config-bestand als een vereiste bron in de toepassing. Vervolgens stelt u de globale variabelen moeten overeenkomen met de variabelen in het bestand config.js-document.

1. Vanaf de opdrachtregel, wijzig de mappen op de **azuread** map als u niet al er.

    `cd azuread`

2. Open uw `server.js` bestand in uw favoriete editor en voeg vervolgens de volgende informatie:

    ```Javascript
    var config = require('./config');
    ```
3. Voeg een nieuwe rubriek voor `server.js` met de volgende code:

    ```Javascript
    var options = {
        // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
        identityMetadata: config.creds.identityMetadata,
        clientID: config.creds.clientID,
        validateIssuer: config.creds.validateIssuer,
        audience: config.creds.audience,
        passReqToCallback: config.creds.passReqToCallback,
        loggingLevel: config.creds.loggingLevel

    };

    // Array to hold logged in users and the current logged in user (owner).
    var users = [];
    var owner = null;

    // Our logger.
    var log = bunyan.createLogger({
        name: 'Azure Active Directory Bearer Sample',
             streams: [
            {
                stream: process.stderr,
                level: "error",
                name: "error"
            },
            {
                stream: process.stdout,
                level: "warn",
                name: "console"
            }, ]
    });

      // If the logging level is specified, switch to it.
      if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    ```

4. Sla het bestand op.

## <a name="step-13-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Stap 13: De MongoDB-Model en de schemagegevens toevoegen met behulp van Mongoose
Alle deze voorbereiding gaat nu gaan betalen uitschakelen als we deze drie bestanden in een REST-API-service combineren.

Voor dit scenario we MongoDB gebruiken voor het opslaan van onze taken, zoals beschreven in stap 4.

In de `config.js` bestand dat wordt gemaakt in stap 11, we onze database aangeroepen `tasklist`, omdat die was we plaatsen aan het einde van onze **mogoose_auth_local** verbindings-URL. U hoeft deze database niet vooraf in MongoDB te maken. In plaats daarvan maakt MongoDB dit voor ons op de eerste uitvoering van onze servertoepassing (ervan uitgaande dat de database nog niet bestaat).

Nu we hebben u de server verteld welke MongoDB-database die we wilt gebruiken, moet er aanvullende code schrijven om het model en het schema voor onze server taken maken.

### <a name="discussion-of-the-model"></a>Beschrijving van het model
Onze schemamodel is eenvoudig. U uitbreiden het naar behoefte.

NAAM: De naam van de persoon die is toegewezen aan de taak. Een **tekenreeks**.

TAAK: De taak zelf. Een **tekenreeks**.

DATUM: De datum waarop de taak moet voltooid zijn. EEN **DATETIME**.

VOLTOOID: Als de taak is voltooid of niet. EEN **BOOLEAANSE**.

### <a name="creating-the-schema-in-the-code"></a>Het schema maken in de code
1. Vanaf de opdrachtregel, wijzig de mappen op de **azuread** map als u niet al er.

    `cd azuread`

2. Open uw `server.js` bestand in uw favoriete editor en voeg vervolgens de volgende informatie onder het configuratie-item:

    ```Javascript
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');

    // Here we create a schema to store our tasks and users. It's a fairly simple schema for now.
    var TaskSchema = new Schema({
        owner: String,
        task: String,
        completed: Boolean,
        date: Date
    });

    // Use the schema to register a model.
    mongoose.model('Task', TaskSchema);
    var Task = mongoose.model('Task');
    ```
Zoals u van de code ziet, maken we onze schema eerst. Vervolgens we een modelobject dat we gebruiken maken voor het opslaan van onze gegevens in de code bij we definiëren onze **Routes**.

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a>Step 14: Onze routes toevoegen voor onze REST-API-taakserver
Nu we hebben een databasemodel werken met, gaan we toevoegen de routes die we gaan gebruiken voor onze REST-API-server.

### <a name="about-routes-in-restify"></a>Routes in Restify
Routes werken in Restify op dezelfde manier als ze doen in de Express-stack. U definieert routes met behulp van de URI die de clienttoepassingen aanroepen. Meestal kunt definiëren u uw routes in een afzonderlijk bestand. Wij gebruiken we onze routes in het server.js-bestand geplaatst. Het is raadzaam dat u rekening te houden deze routes in een eigen bestand voor gebruik in productieomgevingen.

Een doorsnee patroon voor een Restify-route is als volgt:

```Javascript
function createObject(req, res, next) {

// Do work on object.

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // Keep the server going.
}

....

server.post('/service/:add/:object', createObject); // Calls createObject on routes that match this.

```


Dit is het patroon op het meest eenvoudige niveau. Restify (en Express) bieden veel diepere functionaliteit, zoals het definiëren van toepassingstypen en het geven van complexe routering tussen verschillende eindpunten. Voor onze toepassing, zijn we deze routes eenvoudig houden.

### <a name="add-default-routes-to-our-server"></a>Standaardroutes toevoegen aan onze server
We nu toevoegen de eenvoudige CRUD-routes van maken, ophalen, bijwerken en verwijderen.

1. Vanaf de opdrachtregel, wijzig de mappen op de **azuread** map als u niet al er:

    `cd azuread`

2. Open de `server.js` bestand in uw favoriete editor en voeg vervolgens de volgende informatie onder de vorige databasevermeldingen die u hebt aangebracht:

```Javascript

/**
 *
 * APIs for our REST Task server.
 */

// Create a task.

function createTask(req, res, next) {

    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it, and save it to Mongodb.
    var _task = new Task();

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.task = req.params.task;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}


// Delete a task by name.

function removeTask(req, res, next) {

    Task.remove({
        task: req.params.task,
        owner: owner
    }, function(err) {
        if (err) {
            req.log.warn(err,
                'removeTask: unable to delete %s',
                req.params.task);
            next(err);
        } else {
            log.info('Deleted task:', req.params.task);
            res.send(204);
            next();
        }
    });
}

// Delete all tasks.

function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
}


// Get a specific task based on name.

function getTask(req, res, next) {

    log.info('getTask was called for: ', owner);
    Task.find({
        owner: owner
    }, function(err, data) {
        if (err) {
            req.log.warn(err, 'get: unable to read %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Did you initialize the database as stated in the README?");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}

```

### <a name="add-error-handling-in-our-apis"></a>Fout tijdens verwerken van in onze API's toevoegen
```

///--- Errors for communicating something interesting back to the client.

function MissingTaskError() {
    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'MissingTask',
        message: '"task" is a required parameter',
        constructorOpt: MissingTaskError
    });

    this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);


function TaskExistsError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'TaskExists',
        message: owner + ' already exists',
        constructorOpt: TaskExistsError
    });

    this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);


function TaskNotFoundError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 404,
        restCode: 'TaskNotFound',
        message: owner + ' was not found',
        constructorOpt: TaskNotFoundError
    });

    this.name = 'TaskNotFoundError';
}

util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="step-15-create-your-server"></a>Stap 15: De server maken
We hebben onze database gedefinieerd en onze routes aanwezig zijn. Het laatste wat te doen is het toevoegen van het serverexemplaar waarmee onze aanroepen worden beheerd.

In Restify (en Express) kunt u een groot aantal aanpassing voor een REST-API-server uitvoeren, maar we gaan opnieuw de meest eenvoudige configuratie gebruiken voor onze doeleinden.

```Javascript
/**
 * Our server.
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads.
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());

// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in).
server.use(restify.requestLogger());

// Allow five requests per second by IP, and burst to 10.
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want.
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allow for JSON mapping to REST.
```

## <a name="step-16-add-the-routes-to-the-server-without-authentication-for-now"></a>Stap 16: De routes toevoegen aan de server (zonder verificatie nu)
```Javascript
/// Now the real handlers. Here we just CRUD.
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'.
/* In the pasport.authenticate() method. We set 'session: false' because REST is stateless and
/* we don't need to maintain session state. You can experiment with removing API protection
/* by removing the passport.authenticate() method as follows:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler.
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="step-17-run-the-server-before-adding-oauth-support"></a>Stap 17: Uitvoering van de server (vóór het OAuth-ondersteuning toe te voegen)
Testen van uw server voordat we verificatie toe te voegen.

Er is de eenvoudigste manier om uw server testen met behulp van curl in een opdrachtregel. Voordat we dat doen, moeten we een hulpprogramma waarmee we uitvoer kunt parseren als JSON.

1. Installeer de volgende JSON-hulpprogramma (dit hulpprogramma gebruiken in de volgende voorbeelden):

    `$npm install -g jsontool`

    Hiermee wordt het JSON-hulpprogramma op alle vereiste locaties geïnstalleerd. Nu dat we dat hebt gedaan, gaan we afspelen met de server:

2. Controleer eerst of dat het mongoDB-exemplaar wordt uitgevoerd:

    `$sudo mongod`

3. Vervolgens wijzigen naar de map en start curling:

    `$ cd azuread` `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 171
    Date: Tue, 14 Jul 2015 05:43:38 GMT
    [
    "GET /",
    "POST /tasks/:owner/:task",
    "POST /tasks (for JSON body)",
    "GET /tasks",
    "PUT /tasks/:owner",
    "GET /tasks/:owner",
    "DELETE /tasks/:owner/:task"
    ]
    ```

4. Vervolgens kunt we toevoegen een taak op deze manier:

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    Het antwoord moet zijn:

        ```Shell
        HTTP/1.1 201 Created
        Connection: close
        Access-Control-Allow-Origin: *
        Access-Control-Allow-Headers: X-Requested-With
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 5
        Date: Tue, 04 Feb 2014 01:02:26 GMT
        Hello
        ```
    En we kunnen een lijst met taken voor Brandon op deze manier:

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Als alle dit werkt, kunt u OAuth toevoegen aan de REST-API-server.

U hebt een REST-API-server met MongoDB!

## <a name="step-18-add-authentication-to-our-rest-api-server"></a>Stap 18: Verificatie toevoegen aan onze REST-API-server
Nu dat we een actieve REST-API hebben, begint het van groot belang met Azure AD.

Vanaf de opdrachtregel, wijzig de mappen op de **azuread** map als u niet al er.

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>De OIDCBearerStrategy gebruiken die is opgenomen in passport-azure-ad
Tot nu toe hebben we een typische REST-TODO-server zonder enige vorm van autorisatie gebouwd. Dit is waar u begint het samenstellen.

1. Eerst moet aangeven dat we Passport wilt gebruiken. Dit recht na de andere serverconfiguratie genomen:

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > Wanneer u API's schrijft, wordt u aangeraden altijd de gegevens koppelen aan een unieke van het token dat de gebruiker niet kan vervalsen. Wanneer deze server TODO-items opslaat, slaat deze op basis van de object-ID van de gebruiker in het token (aangeroepen via token.oid), die we in het veld 'eigenaar' plaatsen. Dit zorgt ervoor dat alleen die gebruiker toegang heeft tot hun TODOs. Er is geen blootstelling in de 'eigenaar'-API zodat een externe gebruiker de TODOs van anderen aanvragen kan, zelfs als ze zijn geverifieerd.                    

2. Volgende we gebruiken de bearer-strategie die wordt geleverd met `passport-azure-ad`. Bekijk de code nu en wordt de rest kort uitgelegd. Plaats deze achter u hebt geplakt hierboven:

```Javascript
    /**
    /*
    /* Calling the OIDCBearerStrategy and managing users.
    /*
    /* Passport pattern provides the need to manage users and info tokens
    /* with a FindorCreate() method that must be provided by the implementor.
    /* Here we just auto-register any user and implement a FindById().
    /* You'll want to do something smarter.
    **/

    var findById = function(id, fn) {
        for (var i = 0, len = users.length; i < len; i++) {
            var user = users[i];
            if (user.sub === id) {
                log.info('Found user: ', user);
                return fn(null, user);
            }
        }
        return fn(null, null);
    };


    var bearerStrategy = new BearerStrategy(options,
        function(token, done) {
            log.info('verifying the user');
            log.info(token, 'was the token retreived');
            findById(token.sub, function(err, user) {
                if (err) {
                    return done(err);
                }
                if (!user) {
                    // "Auto-registration"
                    log.info('User was added automatically as they were new. Their sub is: ', token.sub);
                    users.push(token);
                    owner = token.sub;
                    return done(null, token);
                }
                owner = token.sub;
                return done(null, user, token);
            });
        }
    );

    passport.use(bearerStrategy);
```

Passport wordt een vergelijkbaar patroon gebruikt voor alle strategieën (Twitter, Facebook, enzovoort) die alle schrijvers van strategieën voor. De strategie bekijkt, ziet u geven we een functie die een token en een gereed als parameters heeft. De strategie komt weer naar ons nadat deze zijn werk doet. Nadat het geval is, we opslaan van de gebruiker en het token niet initialiseren zodat we niet opnieuw hoeft te vragen voor het opnieuw.

> [!IMPORTANT]
> De vorige code wordt elke gebruiker die plaatsvindt om te verifiëren met onze server. Dit wordt automatische registratie genoemd. In productieservers doorlopen u die wordt aangeraden dat u niet toestaan dat iedereen zonder dat zij eerst een registratieproces die u kiest. Dit is doorgaans het patroon die u ziet in consumenten-apps, waarmee u kunt registreren met Facebook, maar vervolgens vraagt u om aanvullende informatie in te vullen. Als dit niet een opdrachtregelprogramma, kan hebben we het e-mailbericht opgehaald uit het Tokenobject dat wordt geretourneerd en wordt vervolgens gevraagd de gebruiker om aanvullende informatie in te vullen. Omdat dit een testserver is, voegen we deze gewoon toe aan de database in het geheugen.
>
>

### <a name="protect-some-endpoints"></a>Sommige eindpunten beveiligen
U beveiligt de eindpunten door te geven de `passport.authenticate()` aanroepen met het protocol dat u wilt gebruiken.

Als u ons servercode iets interessanters te doen, gaan we de route voor bewerken.

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a>Stap 19: De servertoepassing opnieuw uitvoeren en zorg ervoor dat u wordt geweigerd
We gebruiken `curl` opnieuw uit om te controleren of we nu OAuth2-bescherming tegen onze eindpunten. We doen deze test voordat u een van onze client-SDK's met dit eindpunt uitvoert. De geretourneerde headers moeten voldoende om ons te laten als gaan we u het juiste pad.

1. Controleer eerst of dat het mongoDB-exemplaar wordt uitgevoerd:

    `$sudo mongod`

2. Vervolgens wijzigen naar de map en start curling.

      `$ cd azuread` `$ node server.js`

3. Probeer een basic POST.

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

Een 401 is de reactie die u hier zoekt. Dit antwoord geeft aan dat de Passport-laag probeert omleiden naar het geautoriseerde eindpunt, is precies wat u wilt.

## <a name="next-steps"></a>Volgende stappen
U bent gegaan zo ver mogelijk kunt u met deze server zonder met behulp van een OAuth2-compatibele client. U moet een extra scenario doorlopen.

U hebt nu geleerd hoe u een REST-API implementeert met Restify en OAuth2. Hebt u ook meer dan voldoende code te houden voor het ontwikkelen van uw service en leren hoe u kunt in dit voorbeeld is opgebouwd.

Als u geïnteresseerd in de volgende stappen in de ADAL reis bent, vindt hier u enkele ondersteunde ADAL clients het is raadzaam dat u met blijven werken.

Klonen naar beneden op uw machine ontwikkelaars en configureren zoals beschreven in deze stapsgewijze kennismaking.

[ADAL voor iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[ADAL voor Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
