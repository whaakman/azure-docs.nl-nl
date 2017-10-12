---
title: 'Azure AD B2C: Een web-API beveiligen met behulp van Node.js | Microsoft Docs'
description: Een Node.js web-API ontwikkelen die tokens accepteert van een B2C-tenant
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: fc2b9af8-fbda-44e0-962a-8b963449106a
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: xerners
ms.openlocfilehash: 2315f5d0ebb142561c65224cd9d06299466a7ee4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C: Een web-API ontwikkelen met behulp van Node.js
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Met Azure Active Directory (Azure AD) B2C kunt u een web-API beveiligen met OAuth 2.0-toegangstokens. Met deze tokens kunnen client-apps die gebruikmaken van Azure AD B2C, worden geverifieerd bij de API. In dit artikel wordt uitgelegd hoe u een takenlijst-API maakt, waarmee gebruikers taken kunnen toevoegen en vermelden. De web-API is beveiligd met Azure AD B2C en biedt geverifieerde gebruikers alleen de mogelijkheid om hun takenlijst te beheren.

> [!NOTE]
> Dit voorbeeld is bedoeld voor gebruik met de [iOS B2C-voorbeeldtoepassing](active-directory-b2c-devquickstarts-ios.md). Voer eerst deze procedure uit en volg daarna dat voorbeeld.
>
>

**Passport** is verificatiemiddleware voor Node.js. Passport is flexibel en modulair, en kan onopvallend worden geïnstalleerd in een Express- of Restify-webtoepassing. Een uitgebreide set strategieën ondersteunt verificatie met een gebruikersnaam en wachtwoord, Facebook, Twitter en meer. We hebben een strategie ontwikkeld voor Azure Active Directory (Azure AD). U installeert deze module en voegt vervolgens de `passport-azure-ad`-invoegtoepassing van Azure AD toe.

Hiervoor doet u het volgende:

1. U registreert een toepassing met Azure AD.
2. U stelt de toepassing in voor het gebruik van de Passport-invoegtoepassing van `passport-azure-ad`.
3. U configureert een client om de web-API 'takenlijst' aan te roepen.

## <a name="get-an-azure-ad-b2c-directory"></a>Een Azure AD B2C-directory maken
Voordat u Azure AD B2C kunt gebruiken, moet u een directory, of tenant, maken.  Een directory is een container voor alle gebruikers, apps, groepen en meer.  Als u nog geen directory hebt, [maakt u een B2C-directory](active-directory-b2c-get-started.md) voordat u verdergaat.

## <a name="create-an-application"></a>Een app maken
Vervolgens moet u in de B2C-directory een app maken die Azure AD de informatie geeft die nodig is om veilig te communiceren met uw app. In dit geval worden zowel de client-app als de web-API vertegenwoordigd door één **toepassings-id** omdat ze samen één logische app vormen. Volg [deze instructies](active-directory-b2c-app-registration.md) om een app te maken. Zorg ervoor dat:

* U een **web-app of web-API** in de toepassing opneemt.
* U `http://localhost/TodoListService` invoert als **antwoord-URL**. Dit is de standaard-URL voor dit codevoorbeeld.
* U een **toepassingsgeheim** maakt voor uw toepassing en dit kopieert. U hebt deze gegevens later nodig. Deze waarde moet [een escape-teken voor XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) bevatten voordat u deze kunt gebruiken.
* U de **toepassings-id** kopieert die is toegewezen aan uw app. U hebt deze gegevens later nodig.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Het beleid maken
In Azure AD B2C wordt elke gebruikerservaring gedefinieerd door [beleid](active-directory-b2c-reference-policies.md). Deze app bevat twee identiteitservaringen: registreren en aanmelden. U moet één beleidsregel maken voor elk type, zoals wordt beschreven in het [naslagartikel voor beleid](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).  Wanneer u uw drie beleidsregels maakt:

* Kiest u **Weergavenaam** en andere registratiekenmerken in het registratiebeleid.
* Kiest u **Weergavenaam**- en **Object-id**-toepassingsclaims voor elk beleid.  U kunt ook andere claims kiezen.
* Noteert u de **naam** van elk beleid nadat u dit hebt gemaakt. Deze moet het voorvoegsel `b2c_1_` bevatten.  U hebt deze beleidsnamen later nodig.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u de drie beleidsregels hebt gemaakt, kunt u uw app maken.

Voor meer informatie over de werking van beleid in Azure AD B2C, leest u eerst de [zelfstudie Aan de slag met .NET-web-app](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>De code downloaden
De code voor deze zelfstudie [wordt bewaard in GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS). Als u het voorbeeld wilt maken terwijl u aan het werk bent, kunt u [een basisproject downloaden als zip-bestand](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). U kunt het basisproject ook klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

De voltooide app is ook [beschikbaar als zip-bestand](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) of in de `complete`-vertakking van dezelfde opslagplaats.

## <a name="download-nodejs-for-your-platform"></a>Node.js voor uw platform downloaden
U moet een werkende installatie van Node.js hebben om dit voorbeeld te kunnen gebruiken.

Installeer Node.js vanuit [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>MongoDB installeren voor uw platform
U moet een werkende installatie van MongoDB hebben om dit voorbeeld te kunnen gebruiken. U gebruikt MongoDB om uw REST-API permanent te maken in alle serverexemplaren.

Installeer MongoDB vanuit [mongodb.org](http://www.mongodb.org).

> [!NOTE]
> In deze procedure wordt ervan uitgegaan dat u gebruikmaakt van de standaardinstallatie- en -servereindpunten voor MongoDB. Op het moment waarop dit artikel is geschreven, zijn dat `mongodb://localhost`.
>
>

## <a name="install-the-restify-modules-in-your-web-api"></a>De Restify-modules installeren in uw web-API
U gebruikt Restify om de REST-API te ontwikkelen. Restify is een minimaal en flexibel Node.js-toepassingsframework dat is afgeleid van Express. Het bevat een set krachtige functies voor het ontwikkelen van REST-API's op Connect.

### <a name="install-restify"></a>Restify installeren
Wijzig de directory vanaf de opdrachtregel in `azuread`. Als de directory `azuread` niet bestaat, maakt u deze.

`cd azuread` of `mkdir azuread;`

Voer de volgende opdracht in:

`npm install restify`

Met deze opdracht wordt Restify geïnstalleerd.

#### <a name="did-you-get-an-error"></a>Krijgt u een foutmelding?
Wanneer u `npm` in sommige besturingssystemen gebruikt, wordt het foutbericht `Error: EPERM, chmod '/usr/local/bin/..'` weergegeven, plus het verzoek om het account uit te voeren als beheerder. In dat geval gebruikt u de opdracht `sudo` om `npm` uit te voeren op een hoger niveau van bevoegdheden.

#### <a name="did-you-get-a-dtrace-error"></a>Wordt er een DTrace-fout weergegeven?
Mogelijk wordt er iets in de trant van het volgende weergegeven wanneer u Restify installeert:

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

Restify biedt een krachtig mechanisme om REST-aanroepen te traceren met behulp van DTrace. Veel besturingssystemen beschikken echter niet over DTrace. U kunt deze fouten negeren.

De uitvoer van de opdracht ziet er ongeveer als volgt uit:

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

## <a name="install-passport-in-your-web-api"></a>Passport installeren in uw web-API
Wijzig de directory vanaf de opdrachtregel in `azuread`, als dat nog niet is gebeurd.

Installeer Passport met de volgende opdracht:

`npm install passport`

De uitvoer van de opdracht ziet er ongeveer als volgt uit:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>Passport-azuread toevoegen aan uw web-API
Voeg vervolgens de OAuth-strategie toe met behulp van `passport-azuread`. Dit is een reeks strategieën die Azure AD verbinden met Passport. Gebruik deze strategie voor bearer-tokens in het REST-API-voorbeeld.

> [!NOTE]
> Hoewel OAuth2 een kader biedt waarin elk onbekend type token kan worden uitgegeven, worden alleen bepaalde typen tokens wijdverbreid gebruikt. De tokens voor het beveiligen van eindpunten zijn bearer-tokens. Dit is het type token dat het meest wordt uitgegeven in OAuth2. In veel implementaties wordt ervan uitgegaan dat bearer-tokens het enige type token zijn dat wordt uitgegeven.
>
>

Wijzig de directory vanaf de opdrachtregel in `azuread`, als dat nog niet is gebeurd.

Voer de volgende opdracht in om de Passport-module `passport-azure-ad` te installeren:

`npm install passport-azure-ad`

De uitvoer van de opdracht ziet er ongeveer als volgt uit:

``
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
``

## <a name="add-mongodb-modules-to-your-web-api"></a>MongoDB-modules toevoegen aan uw web-API
In dit voorbeeld wordt MongoDB gebruikt als gegevensarchief. Installeer daarvoor Mongoose, een veel gebruikte invoegtoepassing voor het beheren van modellen en schema’s.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Aanvullende modules installeren
Vervolgens installeert u de overige vereiste modules.

Wijzig de directory vanaf de opdrachtregel in `azuread`, als dat nog niet is gebeurd:

`cd azuread`

Installeer de modules in uw `node_modules`-directory:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`

## <a name="create-a-serverjs-file-with-your-dependencies"></a>Een bestand server.js met de afhankelijkheden maken
Het bestand `server.js` verstrekt het merendeel van de functionaliteit voor uw web-API-server.

Wijzig de directory vanaf de opdrachtregel in `azuread`, als dat nog niet is gebeurd:

`cd azuread`

Maak een bestand `server.js` in een teksteditor. Voeg de volgende informatie toe:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Sla het bestand op. U hebt dit bestand later nodig.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Een bestand config.js maken om de Azure AD-instellingen op te slaan
Dit codebestand geeft de configuratieparameters van de Azure AD Portal door aan het bestand `Passport.js`. U hebt deze configuratiewaarden gemaakt toen u de web-API aan de portal toevoegde in het eerste deel van de procedure. Wanneer u de code hebt gekopieerd, wordt uitgelegd wat u in de waarden van deze parameters moet zetten.

Wijzig de directory vanaf de opdrachtregel in `azuread`, als dat nog niet is gebeurd:

`cd azuread`

Maak een bestand `config.js` in een teksteditor. Voeg de volgende informatie toe:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Vereiste waarden
`clientID` : de client-id van uw web-API-toepassing.

`IdentityMetadata` : dit is de locatie waar `passport-azure-ad` de configuratiegegevens voor de id-provider zoekt. Er wordt ook gezocht naar de sleutels om de JSON-webtokens te valideren.

`audience`: de uniform resource identifier (URI) van de portal die de aanroepende toepassing identificeert.

`tenantName`: de tenantnaam, bijvoorbeeld **contoso.onmicrosoft.com**.

`policyName`: het beleid waarmee u de tokens wilt valideren die bij de server binnenkomen. Dit beleid moet hetzelfde zijn als het beleid dat u op de clienttoepassing gebruikt om u aan te melden.

> [!NOTE]
> Gebruik nu hetzelfde beleid in de client- en serverconfiguratie. Als u al een procedure hebt doorlopen waarin u deze beleidsregels hebt gemaakt, hoeft u dit niet opnieuw te doen. Omdat u de procedure hebt voltooid, hoeft u op de site geen nieuw beleid voor clientprocedures te maken.
>
>

## <a name="add-configuration-to-your-serverjs-file"></a>Configuratie toevoegen aan het bestand server.js
Als u de waarden wilt lezen van het `config.js`-bestand dat u hebt gemaakt, voegt u het `.config`-bestand als een vereiste resource toe in uw toepassing en stelt u de globale variabelen in op de variabelen in het `config.js`-document.

Wijzig de directory vanaf de opdrachtregel in `azuread`, als dat nog niet is gebeurd:

`cd azuread`

Open het bestand `server.js` in een teksteditor. Voeg de volgende informatie toe:

```Javascript
var config = require('./config');
```
Voeg een nieuwe sectie aan `server.js` toe die de volgende code bevat:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Daarna voegt u tijdelijke aanduidingen toe voor de gebruikers die van de aanroepende toepassingen worden ontvangen.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

U gaat ook een logger maken.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Het MongoDB-model en de schemagegevens toevoegen met behulp van Mongoose
De eerdere voorbereiding loont de moeite wanneer u deze drie bestanden samenbrengt in een REST-API-service.

Voor deze procedure gebruikt u MongoDB om uw taken op te slaan, zoals eerder is besproken.

In het bestand `config.js` hebt u de **takenlijst** van uw database aangeroepen. De naam van de takenlijst is aan het einde van de verbindings-URL van `mongoose_auth_local` geplaatst. U hoeft deze database niet vooraf in MongoDB te maken. De database wordt voor u gemaakt wanneer u de servertoepassing voor de eerste keer uitvoert.

Nadat u de server hebt laten weten welke MongoDB-database u wilt gebruiken, moet u aanvullende code schrijven om het model en het schema voor de servertaken te maken.

### <a name="expand-the-model"></a>Het model uitbreiden
Dit schemamodel is eenvoudig. U kunt het naar behoefte uitbreiden.

`owner`: de persoon die aan de taak is toegewezen. Dit object is een **tekenreeks**.  

`Text`: de taak zelf. Dit object is een **tekenreeks**.

`date`: de datum waarop de taak moet zijn voltooid. Dit object is een **datum/tijd**.

`completed`: of de taak is voltooid. Dit object is een **Booleaanse waarde**.

### <a name="create-the-schema-in-the-code"></a>Het schema in de code maken
Wijzig de directory vanaf de opdrachtregel in `azuread`, als dat nog niet is gebeurd:

`cd azuread`

Open het bestand `server.js` in een teksteditor. Voeg de volgende informatie toe onder het configuratie-item:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Eerst maakt u het schema en vervolgens maakt u een modelobject dat u gebruikt om uw gegevens in de code op te slaan wanneer u de **routes** definieert.

## <a name="add-routes-for-your-rest-api-task-server"></a>Routes toevoegen voor de REST-API-taakserver
Nu u een databasemodel hebt om mee te werken, voegt u de routes toe die u voor de REST-API-server wilt toevoegen.

### <a name="about-routes-in-restify"></a>Routes in Restify
Routes werken in Restify op precies dezelfde manier als wanneer ze de Express-stack gebruiken. U definieert routes met behulp van de URI die de clienttoepassingen aanroepen.

Een doorsnee patroon voor een Restify-route is:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

U kunt Restify en Express gebruiken voor een veel diepere functionaliteit, zoals voor het definiëren van toepassingstypen en de uitvoering van complexe routering tussen verschillende eindpunten. Voor deze zelfstudie houden we de routes eenvoudig.

#### <a name="add-default-routes-to-your-server"></a>Standaardroutes toevoegen aan een server
U voegt nu de CRUD-basisroutes voor **maken** en **vermelden** toe aan de REST-API. Andere routes vindt u in het gedeelte `complete` van het voorbeeld.

Wijzig de directory vanaf de opdrachtregel in `azuread`, als dat nog niet is gebeurd:

`cd azuread`

Open het bestand `server.js` in een teksteditor. Voeg de volgende informatie toe onder de databasevermeldingen die u hierboven hebt gemaakt:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
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
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

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
            log.warn(err, "There is no tasks in the database. Add one!");
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


#### <a name="add-error-handling-for-the-routes"></a>Foutafhandeling voor de routes toevoegen
Voeg een vorm van foutafhandeling toe, zodat u eventuele problemen op een begrijpelijke manier kunt terugkoppelen naar de client.

Voeg de volgende code toe:

```Javascript
///--- Errors for communicating something interesting back to the client
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


## <a name="create-your-server"></a>De server maken
U hebt nu een database gedefinieerd en de routes geconfigureerd. Nu hoeft u alleen nog het serverexemplaar toe te voegen waarmee uw aanroepen worden beheerd.

Met Restify en Express kunt u de REST-API-server in grote mate aanpassen, maar hier gebruikt u de meest eenvoudige configuratie.

```Javascript

/**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>De routes toevoegen aan de server (zonder verificatie)
```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

```

## <a name="add-authentication-to-your-rest-api-server"></a>Verificatie toevoegen aan een REST-API-app
Nu u een actieve REST-API-server hebt, kunt u deze gaan gebruiken met Azure AD.

Wijzig de directory vanaf de opdrachtregel in `azuread`, als dat nog niet is gebeurd:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>De OIDCBearerStrategy gebruiken die is opgenomen in passport-azure-ad
> [!TIP]
> Wanneer u API's schrijft, moet u de gegevens altijd koppelen aan iets unieks in het token, iets dat de gebruiker niet kan vervalsen. Wanneer de server ToDo-items opslaat, gebeurt dit op basis van de **oid** van de gebruiker in het token (aangeroepen via token.oid); deze komt in het eigenaarsveld te staan. Met deze waarde zorgt u ervoor dat alleen die gebruiker toegang heeft tot zijn/haar eigen ToDo-items. In de API wordt de 'eigenaar' niet weergegeven. Dat betekent dat een externe gebruiker ToDo-items van anderen kan aanvragen, ook al zijn ze geverifieerd.
>
>

Vervolgens gebruikt u de bearer-strategie die bij `passport-azure-ad` is geleverd.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Passport maakt gebruik van hetzelfde patroon voor alle strategieën. U geeft hieraan een `function()` door die `token` en `done` als parameters heeft. De strategie komt weer naar u terug nadat al het werk is uitgevoerd. Sla de gebruiker op en sla ook het token op, zodat u het niet opnieuw hoeft op te vragen.

> [!IMPORTANT]
> In bovenstaande code wordt elke gebruiker gebruikt die zich bij de server kan verifiëren. Dit proces wordt automatische registratie genoemd. In productieservers geeft u gebruikers geen toegang tot de API zonder ze een registratieproces te laten doorlopen. Dit proces is doorgaans het patroon dat u ziet in consumentenapps, waarbij u zich kunt registreren via Facebook, maar waarvoor u vervolgens aanvullende informatie moet invullen. Als dit geen opdrachtregelprogramma was, had u het e-mailadres kunnen verkrijgen uit het tokenobject dat wordt geretourneerd en had u gebruikers daarna niet kunnen vragen aanvullende gegevens in te vullen. Omdat dit een voorbeeld is, voegt u ze toe aan een database in het geheugen.
>
>

## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>De servertoepassing uitvoeren om te verifiëren dat u wordt geweigerd
U kunt `curl` gebruiken om na te gaan of u nu OAuth2-bescherming voor uw eindpunten hebt. De geretourneerde headers moeten voldoende zijn om aan te geven dat u op de juiste weg bent.

Controleer of het MongoDB-exemplaar is geactiveerd:

    $sudo mongodb

Schakel naar de directory en voer de server uit:

    $ cd azuread
    $ node server.js

Voer `curl` uit in een nieuw terminalvenster

Probeer een basic POST:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Een 401-fout is het gewenste antwoord. Hiermee wordt aangegeven dat de Passport-laag probeert om te leiden naar het geautoriseerde eindpunt.

## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>U hebt nu een REST-API-service die gebruikmaakt van OAuth2
U hebt een REST-API geïmplementeerd met behulp van Restify en OAuth! U hebt nu voldoende code om door te gaan met het ontwikkelen van uw service. U kunt voortbouwen op dit voorbeeld. U bent nu met deze server zo ver mogelijk gegaan zonder gebruik te maken van een OAuth2-compatibele client. Gebruik voor de volgende stap een aanvullende walkthrough, zoals [Een web-API verbinden met behulp van iOS met B2C](active-directory-b2c-devquickstarts-ios.md).

## <a name="next-steps"></a>Volgende stappen
U kunt nu verdergaan met geavanceerdere onderwerpen, te weten:

[Een web-API verbinden met behulp van iOS met B2C](active-directory-b2c-devquickstarts-ios.md)
