---
title: Een Azure Active Directory v2.0-web-API beveiligen met behulp van Node.js | Microsoft Docs
description: Informatie over het bouwen van een Node.js-web-API die tokens van een persoonlijk Microsoft-account en van werk accepteert of school accounts.
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mtillman
editor: 
ms.assetid: 0b572fc1-2aaf-4cb6-82de-63010fb1941d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: f07e421feedf3c82da7be16434891cdbe6069038
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="secure-a-web-api-by-using-nodejs"></a>Een web-API beveiligen met behulp van Node.js
> [!NOTE]
> Niet alle Azure Active Directory-scenario's en onderdelen werken met het v2.0-eindpunt. Meer informatie over om te bepalen of u het v2.0-eindpunt of het eindpunt v1.0 moet gebruiken, [v2.0 beperkingen](active-directory-v2-limitations.md).
> 
> 

Wanneer u het v2.0-eindpunt voor Azure Active Directory (Azure AD) gebruikt, kunt u [OAuth 2.0](active-directory-v2-protocols.md) toegang tot tokens, voor het beveiligen van uw web-API. Met OAuth 2.0 toegang tokens, gebruikers die beschikken over een persoonlijk Microsoft-account en werk of schoolaccounts kunnen veilig toegang krijgen tot uw web-API.

*Passport* is verificatiemiddleware voor Node.js. Flexibel en modulair, Passport kan onopvallend worden verwijderd in een snelle gebaseerde of restify-webtoepassing. In Passport, een uitgebreide set strategieën ondersteuning voor verificatie met behulp van een gebruikersnaam en wachtwoord, Facebook, Twitter of andere opties. We hebben een strategie ontwikkeld voor Azure AD. In dit artikel wordt beschreven hoe u installeert de module en voegt u de Azure AD `passport-azure-ad` invoegtoepassing.

## <a name="download"></a>Downloaden
De code voor deze zelfstudie wordt onderhouden in [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs). Wilt u de zelfstudie, kunt u [basis van de app downloaden als ZIP-bestand](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), of het geraamte:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

U kunt ook de voltooide toepassing aan het einde van deze zelfstudie ophalen.

## <a name="1-register-an-app"></a>1: een app registreren
Maakt een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of voert u [deze gedetailleerde stappen](active-directory-v2-app-registration.md) om een app te registreren. Zorg ervoor dat u:

* Kopieer de **toepassings-Id** toegewezen aan uw app. U hebt deze nodig voor deze zelfstudie.
* Voeg de **Mobile** platform voor uw app.
* Kopieer de **omleidings-URI** vanuit de portal. Moet u de standaardwaarde van de URI van `urn:ietf:wg:oauth:2.0:oob`.

## <a name="2-install-nodejs"></a>2: Installeer Node.js
Voor het gebruik van het voorbeeld voor deze zelfstudie, moet u [Installeer Node.js](http://nodejs.org).

## <a name="3-install-mongodb"></a>3: Installeer MongoDB
Om te kunnen gebruiken in dit voorbeeld, moet u [MongoDB installeren](http://www.mongodb.org). In dit voorbeeld gebruikt u MongoDB om uw REST-API persistent ervoor in alle serverexemplaren.

> [!NOTE]
> In dit artikel gaan we ervan uit dat u de standaard- en -servereindpunten voor MongoDB: mongodb://localhost.
> 
> 

## <a name="4-install-the-restify-modules-in-your-web-api"></a>4: de restify-modules installeren in uw web-API
We Resitfy gebruiken voor het bouwen van de REST-API. Restify is een minimaal en flexibel Node.js-toepassingsframework dat afgeleid van Express. Restify is een set krachtige functies die u gebruiken kunt voor het bouwen van de REST-API's op Connect.

### <a name="install-restify"></a>Restify installeren
1.  Wijzig bij een opdrachtprompt de map in **azuread**:

    `cd azuread`

    Als de **azuread** directory niet bestaat, deze maken:

    `mkdir azuread`

2.  Restify installeren:

    `npm install restify`

    De uitvoer van deze opdracht moet er als volgt uitzien:

    ```
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
    └── bunyan@0.22.0(mv@0.0.5)
    ```

#### <a name="did-you-get-an-error"></a>Krijgt u een foutmelding?
In sommige besturingssystemen wanneer u de `npm` uitvoert, en u dit bericht ziet: `Error: EPERM, chmod '/usr/local/bin/..'`. De fout wordt gevolgd door een aanvraag die u probeert het account uitvoeren als beheerder. Als dit het geval is, gebruikt u de opdracht `sudo` om uit te voeren `npm` op een hoger niveau van bevoegdheden.

#### <a name="did-you-get-an-error-related-to-dtrace"></a>Krijgt u een fout met betrekking tot DTrace?
Wanneer u installeert restify, ziet u dit bericht:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: two
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

Restify is een krachtig mechanisme te traceren REST aanroept met behulp van DTrace. DTrace is echter niet beschikbaar is op veel besturingssystemen. U kunt dit foutbericht negeren.


## <a name="5-install-passportjs-in-your-web-api"></a>5: installatie Passport.js in uw web-API
1.  Wijzig bij de opdrachtprompt de map in **azuread**.

2.  Passport.js installeren:

    `npm install passport`

    De uitvoer van de opdracht er als volgt uit:

    ```
     passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3
    ```

## <a name="6-add-passport-azure-ad-to-your-web-api"></a>6: passport-azure-ad toevoegen aan uw web-API
Voeg vervolgens de OAuth-strategie toe met behulp van de passport-azuread. `passport-azuread`is een reeks strategieën die Azure AD met Passport verbinden. We gebruiken deze strategie voor bearer-tokens in dit voorbeeld REST-API.

> [!NOTE]
> Hoewel OAuth 2.0 een kader waarin elk onbekend type token kan worden uitgegeven biedt, worden bepaalde typen worden vaak gebruikt. Bearer-tokens worden vaak gebruikt voor het beveiligen van eindpunten. Bearer-tokens zijn het meest wordt uitgegeven token in OAuth 2.0-type. Veel OAuth 2.0-implementaties wordt ervan uitgegaan dat bearer-tokens het enige type token dat is uitgegeven zijn.
> 
> 

1.  Wijzig bij een opdrachtprompt de map in **azuread**.

    `cd azuread`

2.  Installeer de Passport.js `passport-azure-ad` module:

    `npm install passport-azure-ad`

    De uitvoer van de opdracht er als volgt uit:

    ```
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
    ```

## <a name="7-add-mongodb-modules-to-your-web-api"></a>7: MongoDB-modules toevoegen aan uw web-API
In dit voorbeeld gebruiken we MongoDB als onze gegevensarchief. 

1.  Installeren van Mongoose, een veelgebruikte invoegtoepassing voor het beheren van modellen en schema's: 

    `npm install mongoose`

2.  Installeer het databasestuurprogramma voor MongoDB, ook wel MongoDB genoemd:

    `npm install mongodb`

## <a name="8-install-additional-modules"></a>8: aanvullende modules installeren
Installeer de overige vereiste modules.

1.  Wijzig bij een opdrachtprompt de map in **azuread**:

    `cd azuread`

2.  Voer de volgende opdrachten. De opdrachten installeren de volgende modules in de map node_modules:

    *   `npm install crypto`
    *   `npm install assert-plus`
    *   `npm install posix-getopt`
    *   `npm install util`
    *   `npm install path`
    *   `npm install connect`
    *   `npm install xml-crypto`
    *   `npm install xml2js`
    *   `npm install xmldom`
    *   `npm install async`
    *   `npm install request`
    *   `npm install underscore`
    *   `npm install grunt-contrib-jshint@0.1.1`
    *   `npm install grunt-contrib-nodeunit@0.1.2`
    *   `npm install grunt-contrib-watch@0.2.0`
    *   `npm install grunt@0.4.1`
    *   `npm install xtend@2.0.3`
    *   `npm install bunyan`
    *   `npm update`

## <a name="9-create-a-serverjs-file-for-your-dependencies"></a>9: een bestand Server.js voor afhankelijkheden maken
Een bestand Server.js bevat het merendeel van de functies voor uw web-API-server. De meeste van uw code toevoegen aan dit bestand. Voor productiedoeleinden, kunt u de functionaliteit in kleinere bestanden opsplitsen zoals voor afzonderlijke routes en controllers. In dit artikel gebruiken we Server.js voor dit doel.

1.  Wijzig bij een opdrachtprompt de map in **azuread**:

    `cd azuread`

2.  Met een editor naar keuze, maak een Server.js-bestand. De volgende informatie toevoegen aan het bestand:

    ```Javascript
    'use strict';
    /**
    * Module dependencies.
    */
    var util = require('util');
    var assert = require('assert-plus');
    var mongoose = require('mongoose/');
    var bunyan = require('bunyan');
    var restify = require('restify');
    var config = require('./config');
    var passport = require('passport');
    var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
    ```

3.  Sla het bestand op. U gaat terug naar deze binnenkort.

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a>10: maken van een configuratiebestand voor het opslaan van uw Azure AD-instellingen
Dit codebestand geeft de configuratieparameters van uw Azure AD-portal aan Passport.js. U hebt deze configuratiewaarden gemaakt toen u de web-API toegevoegd aan de portal aan het begin van het artikel. Nadat u de code hebt gekopieerd, wordt uitgelegd wat er in de waarden van deze parameters.

1.  Wijzig bij een opdrachtprompt de map in **azuread**:

    `cd azuread`

2.  Maak een bestand Config.js in een teksteditor. Voeg de volgende informatie toe:

    ```Javascript
    // Don't commit this file to your public repos. This config is for first-run.
    exports.creds = {
    mongoose_auth_local: 'mongodb://localhost/tasklist', // Your Mongo auth URI goes here.
    issuer: 'https://sts.windows.net/**<your application id>**/',
    audience: '<your redirect URI>',
    identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For Microsoft, you should never need to change this.
    };

    ```



### <a name="required-values"></a>Vereiste waarden

*   **IdentityMetadata**: dit is wanneer `passport-azure-ad` wordt gezocht naar de configuratiegegevens voor de identiteitsprovider (IDP) en de sleutels voor het valideren van de JSON Web Tokens (JWTs). Als u Azure AD gebruikt, wilt u waarschijnlijk niet wijzigen.

*   **doelgroep**: uw omleidings-URI van de portal.

> [!NOTE]
> Uw sleutels rouleert met regelmatige tussenpozen. Zorg ervoor dat u altijd via de URL 'openid_keys' pull en of de app toegang hebt tot het Internet.
> 
> 

## <a name="11-add-the-configuration-to-your-serverjs-file"></a>11: de configuratie toevoegen aan het bestand Server.js
Uw toepassing moet lezen van de waarden uit het configuratiebestand dat u zojuist hebt gemaakt. Voeg het .config-bestand als een vereiste bron in uw toepassing. De globale variabelen ingesteld op apparaten die in Config.js.

1.  Wijzig bij de opdrachtprompt de map in **azuread**:

    `cd azuread`

2.  Open Server.js in een teksteditor. Voeg de volgende informatie toe:

    ```Javascript
    var config = require('./config');
    ```

3.  Een nieuwe sectie aan Server.js toevoegen:

    ```Javascript
    // Pass these options in to the ODICBearerStrategy.
    var options = {
    // The URL of the metadata document for your app. Put the keys for token validation from the URL found in the jwks_uri tag in the metadata.
    identityMetadata: config.creds.identityMetadata,
    issuer: config.creds.issuer,
    audience: config.creds.audience
    };
    // Array to hold signed-in users and the current signed-in user (owner).
    var users = [];
    var owner = null;
    // Your logger
    var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
    });
    ```

## <a name="12-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>12: de MongoDB-model en het schema-informatie toevoegen met behulp van Mongoose
Sluit deze drie bestanden in een REST-API-service.

In dit artikel gebruiken we MongoDB voor het opslaan van onze taken. Bespreken we dit op *stap 4*.

In het bestand Config.js file u hebt gemaakt in stap 11, uw database heet *tasklist*. Dat is wat u aan het einde van uw mongoose_auth_local verbindings-URL opnemen. U hoeft deze database niet vooraf in MongoDB te maken. De database wordt gemaakt op de eerste uitvoering van de servertoepassing (ervan uitgaande dat de database nog niet bestaat).

U hebt de-server opgegeven welke MongoDB-database moet worden gebruikt. Vervolgens moet u aanvullende code schrijven om het model en het schema voor de taken van de server maken.

### <a name="the-model"></a>Het model
De schemamodel is zeer eenvoudige. U kunt deze als u wilt uitbreiden. 

Het schemamodel heeft de volgende waarden:

*   **NAAM**. De persoon die is toegewezen aan de taak. Dit is een **tekenreeks** waarde.
*   **TAAK**. De naam van de taak. Dit is een **tekenreeks** waarde.
*   **DATUM**. De datum waarop de taak moet voltooid zijn. Dit is een **datetime** waarde.
*   **VOLTOOID**. Hiermee wordt aangegeven of de taak is voltooid. Dit is een **Booleaanse** waarde.

### <a name="create-the-schema-in-the-code"></a>Het schema in de code maken
1.  Wijzig bij een opdrachtprompt de map in **azuread**:

    `cd azuread`

2.  Open in uw editor Server.js. Onder het configuratie-item toevoegen de volgende informatie:

    ```Javascript
    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');
    ```

Deze code maakt verbinding met de MongoDB-server. Ook wordt een schemaobject.

#### <a name="using-the-schema-create-your-model-in-the-code"></a>Met het schema, het model in de code maken
Voeg onder de vorige code de volgende code:

```Javascript
// Create a basic schema to store your tasks and users.
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

Zoals u van de code ziet, maakt u eerst uw schema. Vervolgens maakt u een modelobject. Gebruik van het modelobject voor het opslaan van uw gegevens in de code bij het definiëren van uw **routes**.

## <a name="13-add-your-routes-for-your-task-rest-api-server"></a>13: uw routes toevoegen voor de REST-API-taakserver
Nu u een databasemodel werken met hebt, toevoegen de routes die u voor de REST-API-server gebruikt.

### <a name="about-routes-in-restify"></a>Routes in restify
Routes in restify werk precies dezelfde manier wanneer u de Express-stack gebruiken. U definieert routes met behulp van de URI die de clienttoepassingen aanroepen. Meestal kunt definiëren u uw routes in een afzonderlijk bestand. In deze zelfstudie we onze routes in Server.js geplaatst. Voor gebruik in productieomgevingen, is het raadzaam dat u rekening te houden routes in een eigen bestand.

Een doorsnee patroon voor een restify-route is:

```Javascript
function createObject(req, res, next) {
// Do work on object.
_object.name = req.params.object; // Passed value is in req.params under object.
///...
return next(); // Keep the server going.
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```


Dit is het patroon op de meest eenvoudige niveau. Restify (en Express) bieden veel diepere functionaliteit, zoals de mogelijkheid voor het definiëren van toepassingstypen en complexe routering tussen verschillende eindpunten.

#### <a name="add-default-routes-to-your-server"></a>Standaardroutes toevoegen aan een server
De eenvoudige CRUD-routes toevoegen: **maken**, **ophalen**, **bijwerken**, en **verwijderen**.

1.  Wijzig bij een opdrachtprompt de map in **azuread**:

    `cd azuread`

2.  Open Server.js in een teksteditor. De databasevermeldingen die u eerder hebt aangebracht toevoegen hieronder de volgende informatie:

    ```Javascript
    /**
    *
    * APIs for your REST task server
    */
    // Create a task.
    function createTask(req, res, next) {
    // Resitify currently has a bug that doesn't allow you to set default headers.
    // These headers comply with CORS, and allow you to use MongoDB Server as your response to any origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    // Create a new task model, fill it, and save it to MongoDB.
    var _task = new Task();
    if (!req.params.task) {
    req.log.warn({
    params: p
    }, 'createTodo: missing task');
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
    /// Returns the list of TODOs that were loaded.
    function listTasks(req, res, next) {
    // Resitify currently has a bug that doesn't allow you to set default headers.
    // These headers comply with CORS, and allow us to use MongoDB Server as our response to any origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    log.info("listTasks was called for: ", owner);
    Task.find({
    owner: owner
    }).limit(20).sort('date').exec(function(err, data) {
    if (err)
    return next(err);
    if (data.length > 0) {
    log.info(data);
    }
    if (!data.length) {
    log.warn(err, "There are no tasks in the database. Add one!");
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

### <a name="add-error-handling-for-the-routes"></a>Foutafhandeling voor de routes toevoegen
Foutafhandeling toevoegen zodat u terug naar de client over het opgetreden probleem kan communiceren.

Voeg de volgende code hieronder de code die u al hebt geschreven:

```Javascript
///--- Errors for communicating something more information back to the client.
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


## <a name="14-create-your-server"></a>14: de server maken
Het laatste wat te doen is het toevoegen van uw server-exemplaar. Het serverexemplaar beheert uw aanroepen.

Restify (en Express) mate aanpassen die u met een REST-API-server gebruiken kunt hebben. In deze zelfstudie gebruiken we de meest eenvoudige configuratie.

```Javascript
/**
* Your server
*/
var server = restify.createServer({
name: "Microsoft Azure Active Directory TODO Server",
version: "2.0.1"
});
// Ensure that you don't drop data on uploads.
server.pre(restify.pre.pause());
// Clean up imprecise paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());
// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());
// Set a per-request Bunyan logger (with requestid filled in).
server.use(restify.requestLogger());
// Allow 5 requests/second by IP address, and burst to 10.
server.use(restify.throttle({
burst: 10,
rate: 5,
ip: true,
}));
// Use common commands, such as:
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
mapParams: true
}));
```
## <a name="15-add-the-routes-without-authentication-for-now"></a>15: de routes (zonder verificatie nu) toevoegen
```Javascript
/// Use CRUD to add the real handlers.
/**
/*
/* Each of these handlers is protected by your Open ID Connect Bearer strategy. Invoke 'oidc-bearer'
/* in the pasport.authenticate() method. Because REST is stateless, set 'session: false'. You 
/* don't need to maintain session state. You can experiment with removing API protection.
/* To do this, remove the passport.authenticate() method:
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
// Register a default '/' handler
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
## <a name="16-run-the-server"></a>16: de server wordt uitgevoerd
Het is een goed idee om uw server te testen voordat u verificatie toevoegt.

Er is de eenvoudigste manier om uw server testen met behulp van curl bij een opdrachtprompt. Om dit te doen, moet u een eenvoudig hulpprogramma waarmee u kunt uitvoer kunt parseren als JSON. 

1.  Installeer het JSON-hulpprogramma we gebruiken in de volgende voorbeelden:

    `$npm install -g jsontool`

    Hiermee wordt het JSON-hulpprogramma op alle vereiste locaties geïnstalleerd.

2.  Controleer of het MongoDB-exemplaar is geactiveerd:

    `$sudo mongod`

3.  Wijzig de map in **azuread**, en voer vervolgens curl:

    `$ cd azuread`
    `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 2.0OK
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

4.  Een taak wilt toevoegen:

    `$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

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

5.  Lijst met taken voor Brandon:

    `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Als alle deze opdrachten worden uitgevoerd zonder fouten, bent u OAuth toevoegen aan de REST-API-server.

*U hebt nu een REST-API-server met MongoDB.*

## <a name="17-add-authentication-to-your-rest-api-server"></a>17: verificatie toevoegen aan de REST-API-server
Nu dat u een actieve REST-API hebt, instellen voor gebruik met Azure AD.

Wijzig bij een opdrachtprompt de map in **azuread**:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-thats-included-with-passport-azure-ad"></a>De oidcbearerstrategy gebruiken die is opgenomen in passport-azure-ad
U kunt een typische REST-TODO-server zonder enige vorm van autorisatie tot nu toe hebt gemaakt. Voeg nu verificatie.

Ten eerste kunt u aangeven dat u Passport wilt gebruiken. Dit recht na de configuratie van uw eerdere server genomen:

```Javascript
// Start using Passport.js.

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [!TIP]
> Wanneer u API's schrijft, is het een goed idee om de gegevens altijd koppelen aan een unieke van het token dat de gebruiker niet kan vervalsen. Wanneer deze server TODO-items opslaat, slaat deze op basis van de abonnements-ID van de gebruiker in het token (aangeroepen via token.sub). U kunt de token.sub plaatsen in het veld 'eigenaar'. Dit zorgt ervoor dat alleen die gebruiker toegang heeft tot de gebruiker TODOs. Niemand anders toegang tot de TODOs die zijn ingevoerd. Er is geen blootstelling in de API voor de 'eigenaar'. Een externe gebruiker kan andere gebruikers TODOs, aanvragen, zelfs als ze zijn geverifieerd.
> 
> 

Gebruik vervolgens de Open ID Connect Bearer-strategie die wordt geleverd met `passport-azure-ad`. Plaats deze achter eerder geplakt:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users.
/*
/* Because of the Passport pattern, you need to manage users and info tokens
/* with a FindorCreate() method. The method must be provided by the implementor.
/* In the following code, you autoregister any user and implement a FindById().
/* It's a good idea to do something more advanced.
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
var oidcStrategy = new OIDCBearerStrategy(options,
function(token, done) {
log.info('verifying the user');
log.info(token, 'was the token retrieved');
findById(token.sub, function(err, user) {
if (err) {
return done(err);
}
if (!user) {
// "Auto-registration"
log.info('User was added automatically, because they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

Passport wordt een vergelijkbaar patroon gebruikt voor alle strategieën (Twitter, Facebook, enzovoort). Alle schrijvers van strategieën voor het patroon. Geeft de strategie voor een `function()` die gebruikmaakt van een token en `done` als parameters. De strategie terug nadat al het werk wordt. Sla de gebruiker en het token niet initialiseren zodat u niet hoeft te vragen voor het opnieuw.

> [!IMPORTANT]
> De voorafgaande code geldt voor elke gebruiker die kan worden geverifieerd met de server. Dit wordt automatische registratie genoemd. Op een productieserver wilt u niet dat iedereen, kunnen zonder dat zij een registratieproces die u kiest doorlopen eerst. Dit is doorgaans het patroon die u in consumenten-apps ziet. De app kunt u mogelijk registreren met Facebook, maar vervolgens wordt u gevraagd om in te voeren als u meer informatie. Als u een opdrachtregelprogramma zijn niet voor deze zelfstudie gebruikt, kunt u het e-mailbericht kan extraheren uit het Tokenobject dat wordt geretourneerd. Vervolgens vraagt u mogelijk de gebruiker in te voeren als u meer informatie. Omdat dit een testserver is, moet u de gebruiker rechtstreeks naar de database in het geheugen toevoegen.
> 
> 

### <a name="protect-endpoints"></a>Eindpunten beveiligen
Eindpunten beveiligen door te geven de **passport.authenticate()** aanroepen met het protocol dat u wilt gebruiken.

U kunt uw route in uw servercode voor een meer geavanceerde optie bewerken:

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again"></a>18: de servertoepassing opnieuw uitvoeren
Curl opnieuw gebruiken om te zien als u OAuth 2.0-beveiliging voor uw eindpunten hebt. Doe dit voordat u een van uw client-SDK's uitvoeren met dit eindpunt. De geretourneerde headers moeten vertellen als de verificatie van uw correct werkt.

1.  Zorg ervoor dat uw isntance MongoDB wordt uitgevoerd:

    `$sudo mongod`

2.  Wijzig in de **azuread** directory en gebruik curl:

    `$ cd azuread`

    `$ node server.js`

3.  Probeer een basic POST:

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

Een 401-respons geeft aan dat de Passport-laag probeert om te leiden naar het geautoriseerde eindpunt, is precies wat u wilt.

*U hebt nu een REST-API-service die gebruikmaakt van OAuth 2.0.*

U bent gegaan zo ver mogelijk kunt u met deze server zonder met behulp van een OAuth 2.0-compatibele client. Daarvoor moet u een extra zelfstudie bekijken.

## <a name="next-steps"></a>Volgende stappen
Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) wordt geleverd als [een ZIP-bestand](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip). U kunt dit ook klonen vanuit GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Nu kunt u op verplaatsen met geavanceerdere onderwerpen. U wilt proberen [beveiligen van een Node.js-web-app met behulp van het v2.0-eindpunt](active-directory-v2-devquickstarts-node-web.md).

Hier volgen enkele aanvullende resources:

* [Handleiding voor Azure AD v2.0-ontwikkelaars](active-directory-appmodel-v2-overview.md)
* [Stack-overloop 'azure active directory' tag](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten downloaden
We raden u aan te melden om te worden geïnformeerd wanneer er beveiligingsincidenten optreden. Op de [Microsoft technische beveiligingsmeldingen](https://technet.microsoft.com/security/dd252948) pagina moet u zich abonneren op beveiligingswaarschuwingen aanbevelingen.

