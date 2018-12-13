---
title: JavaScript-referentie voor ontwikkelaars voor Azure Functions | Microsoft Docs
description: Lees hoe u voor het ontwikkelen van functies met behulp van JavaScript.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.service: azure-functions
ms.devlang: nodejs
ms.topic: reference
ms.date: 10/26/2018
ms.author: glenga
ms.openlocfilehash: 17e0cf170197b99037e2892d1b74a699a3a9eef5
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275326"
---
# <a name="azure-functions-javascript-developer-guide"></a>Handleiding voor ontwikkelaars van Azure Functions-JavaScript

Deze handleiding bevat informatie over de complexiteit van het schrijven van Azure Functions met JavaScript.

Een JavaScript-functie is een geëxporteerde `function` die wordt uitgevoerd wanneer geactiveerd ([triggers zijn geconfigureerd in de function.json](functions-triggers-bindings.md)). Het eerste argument elke functie wordt doorgegeven is een `context` -object dat wordt gebruikt voor het ontvangen en verzenden die gegevens bindt, logboekregistratie en communiceert met de runtime.

In dit artikel wordt ervan uitgegaan dat u al hebt gelezen de [referentie voor ontwikkelaars van Azure Functions](functions-reference.md). U moet ook uitvoeren met de Snelstartgids voor Functions voor het maken van uw eerste functie, met behulp van [Visual Studio Code](functions-create-first-function-vs-code.md) of [in de portal](functions-create-first-azure-function.md).

## <a name="folder-structure"></a>mapstructuur

De vereiste mapstructuur voor een JavaScript-project ziet er als volgt uit. Deze standaardinstelling kan worden gewijzigd. Zie voor meer informatie de [scriptbestand](#using-scriptfile) onderstaande sectie.

```
FunctionsProject
 | - MyFirstFunction
 | | - index.js
 | | - function.json
 | - MySecondFunction
 | | - index.js
 | | - function.json
 | - SharedCode
 | | - myFirstHelperFunction.js
 | | - mySecondHelperFunction.js
 | - node_modules
 | - host.json
 | - package.json
 | - extensions.csproj
 | - bin
```

In de hoofdmap van het project, er is een gedeelde [host.json](functions-host-json.md) -bestand dat kan worden gebruikt voor het configureren van de functie-app. Elke functie heeft een map met een eigen codebestand (.js) en de binding-configuratiebestand (function.json). De naam van `function.json`van bovenliggende map is altijd de naam van uw functie.

De binding-extensies vereist in [versie 2.x](functions-versions.md) van de functies runtime zijn gedefinieerd in de `extensions.csproj` bestand met de werkelijke dll-bestanden in de `bin` map. Als u lokaal ontwikkelt, moet u [bindinguitbreidingen registreren](functions-triggers-bindings.md#local-development-azure-functions-core-tools). Bij het ontwikkelen van functies in Azure portal, geldt deze registratie voor u.

## <a name="exporting-a-function"></a>Exporteren van een functie

JavaScript-functies moeten worden geëxporteerd [ `module.exports` ](https://nodejs.org/api/modules.html#modules_module_exports) (of [ `exports` ](https://nodejs.org/api/modules.html#modules_exports)). De geëxporteerde functie moet een JavaScript-functie die wordt uitgevoerd wanneer ze worden geactiveerd.

Standaard de Functions-runtime zoekt de functie in `index.js`, waarbij `index.js` deelt dezelfde bovenliggende map als het bijbehorende `function.json`. In het geval standaard uw geëxporteerde functie moet het enige exporteren uit het bestand of de uitvoer met de naam `run` of `index`. Meer informatie over het configureren van de locatie van de en naam van de functie exporteren, [configureren van uw functie-ingangspunt](functions-reference-node.md#configure-function-entry-point) hieronder.

De geëxporteerde functie wordt een aantal argumenten doorgegeven op worden uitgevoerd. Het eerste argument duurt is altijd een `context` object. Als uw functie synchroon is (resulteert niet in een Promise), moet u doorgeven de `context` -object, als het aanroepen `context.done` is vereist voor gebruik.

```javascript
// You should include context, other arguments are optional
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
    context.done();
};
```

### <a name="exporting-an-async-function"></a>Exporteren van een asynchrone-functie
Wanneer u de [ `async function` ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) declaratie of gewoon JavaScript [beloften](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) in versie 2.x van de Functions-runtime, hoeft u niet expliciet aan te roepen de [ `context.done` ](#contextdone-method) callback gebruikt om op te geven dat de functie is voltooid. De functie is voltooid wanneer de geëxporteerde async-functie/belofte is voltooid. Voor de functies die gericht is op de versie 1.x-runtime, moet u nog steeds aanroepen [ `context.done` ](#contextdone-method) wanneer uw code wordt uitgevoerd wordt uitgevoerd.

Het volgende voorbeeld wordt een eenvoudige functie die registreert dat hij is geactiveerd en is onmiddellijk voltooid.

```javascript
module.exports = async function (context) {
    context.log('JavaScript trigger function processed a request.');
};
```

Bij het exporteren van een asynchrone-functie, kunt u ook een Uitvoerbinding om te configureren de `return` waarde. Dit wordt aanbevolen als u slechts één Uitvoerbinding hebt.

Om toe te wijzen een uitvoer met `return`, wijzigt de `name` eigenschap `$return` in `function.json`.

```json
{
  "type": "http",
  "direction": "out",
  "name": "$return"
}
```

In dit geval wordt moet de functie eruitzien als in het volgende voorbeeld:

```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');
    // You can call and await an async method here
    return {
        body: "Hello, world!"
    };
}
```

## <a name="bindings"></a>Bindingen 
In JavaScript, [bindingen](functions-triggers-bindings.md) zijn geconfigureerd en gedefinieerd in de function.json van een functie. Functies werken met bindingen een aantal manieren.

### <a name="inputs"></a>Invoer
Invoer zijn onderverdeeld in twee categorieën in Azure Functions: de invoer voor de werkstroomtrigger is en de andere is de aanvullende invoer. Trigger en andere invoer Bindingen (bindingen van `direction === "in"`) kunnen worden gelezen door een functie op drie manieren:
 - **_(Aanbevolen)_  Als parameters aan uw functie doorgegeven.** Ze worden doorgegeven aan de functie in dezelfde volgorde als waarin ze zijn gedefinieerd in *function.json*. Houd er rekening mee dat de `name` -eigenschap worden gedefinieerd *function.json* hoeft niet te overeenkomen met de naam van de parameter, maar het moet.
 
   ```javascript
   module.exports = async function(context, myTrigger, myInput, myOtherInput) { ... };
   ```
   
 - **Als leden van de [ `context.bindings` ](#contextbindings-property) object.** Elk lid met de naam van de `name` -eigenschap worden gedefinieerd *function.json*.
 
   ```javascript
   module.exports = async function(context) { 
       context.log("This is myTrigger: " + context.bindings.myTrigger);
       context.log("This is myInput: " + context.bindings.myInput);
       context.log("This is myOtherInput: " + context.bindings.myOtherInput);
   };
   ```
   
 - **Als invoer met de JavaScript [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) object.** Dit is in wezen hetzelfde als invoer wordt doorgegeven als parameters, maar kunt u voor het afhandelen van dynamisch invoer.
 
   ```javascript
   module.exports = async function(context) { 
       context.log("This is myTrigger: " + arguments[1]);
       context.log("This is myInput: " + arguments[2]);
       context.log("This is myOtherInput: " + arguments[3]);
   };
   ```

### <a name="outputs"></a>Uitvoer
Uitvoer (bindingen van `direction === "out"`) door een functie in een aantal manieren om te kunnen worden geschreven. In alle gevallen moet de `name` eigenschap van de binding zoals gedefinieerd in *function.json* komt overeen met de naam van het Objectlid naar worden geschreven in de functie. 

U kunt gegevens toewijzen aan uitvoerbindingen in een van de volgende manieren. Deze methoden moeten niet worden gecombineerd.
- **_[Aanbevolen voor meerdere uitvoer]_  Retourneren een object.** Als u van een asynchrone/Promise functie retourneren gebruikmaakt, kunt u een object met een toegewezen uitvoergegevens retourneren. In het volgende voorbeeld wordt de uitvoerbindingen zijn met de naam "httpResponse" en "queueOutput" in *function.json*.

  ```javascript
  module.exports = async function(context) {
      let retMsg = 'Hello, world!';
      return {
          httpResponse: {
              body: retMsg
          },
          queueOutput: retMsg
      };
  };
  ```
  
  Als u van een synchrone functie gebruikmaakt, kunt u terugkeren dit object met [ `context.done` ](#contextdone-method) (Zie het voorbeeld).
- **_[Aanbevolen voor één uitvoer]_  Rechtstreeks een waarde retourneren en het gebruik van de naam van de binding $return.** Dit werkt alleen voor asynchrone/belofte functies retourneren. Zie het voorbeeld in [exporteren van een functie asynchrone](#exporting-an-async-function). 
- **Toewijzen van waarden die moeten worden `context.bindings`**  kunt u waarden rechtstreeks aan context.bindings toewijzen.

  ```javascript
  module.exports = async function(context) {
      let retMsg = 'Hello, world!';
      context.bindings.httpResponse = {
          body: retMsg
      };
      context.bindings.queueOutput = retMsg;
      return;
  };
  ```
 
### <a name="bindings-data-type"></a>Bindings-gegevenstype

Voor het definiëren van het gegevenstype voor een Invoerbinding, de `dataType` eigenschap in het bindingsdefinitie van de. Bijvoorbeeld, om te lezen van de inhoud van een HTTP-aanvraag in binaire indeling, gebruikt het type `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Opties voor `dataType` zijn: `binary`, `stream`, en `string`.

## <a name="context-object"></a>context-object
De runtime wordt gebruikt een `context` object om door te geven gegevens van en naar uw functie en u communiceren met de runtime te laten. Het contextobject kan worden gebruikt voor het lezen van en instellen van gegevens uit de bindingen schrijven logboeken en met behulp van de `context.done` retouraanroep wanneer de geëxporteerde functie synchroon is.

De `context` -object is altijd de eerste parameter voor een functie. Deze moet worden toegevoegd omdat er belangrijke methoden, zoals `context.done` en `context.log`. U kunt het object naam wat u graag (bijvoorbeeld `ctx` of `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(ctx) {
    // function logic goes here :)
    ctx.done();
};
```

### <a name="contextbindings-property"></a>de eigenschap context.bindings

```js
context.bindings
```

Retourneert een benoemde object dat al uw invoer- en gegevens bevat. Bijvoorbeeld, de volgende bindingsdefinities in uw function.json kunnen u toegang tot de inhoud van een wachtrij van `context.bindings.myInput` en uitvoer toewijzen aan een wachtrij met `context.bindings.myOutput`.

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
},
{
    "type":"queue",
    "direction":"out",
    "name":"myOutput"
    ...
}
```

```javascript
// myInput contains the input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

U kunt kiezen voor het definiëren van uitvoer binding gegevens met de `context.done` methode in plaats van de `context.binding` object (Zie hieronder).

### <a name="contextbindingdata-property"></a>de eigenschap context.bindingData

```js
context.bindingData
```

Retourneert een benoemde-object dat gegevens van trigger metagegevens en de functie aanroepen bevat (`invocationId`, `sys.methodName`, `sys.utcNow`, `sys.randGuid`). Zie voor een voorbeeld van de metagegevens van de trigger, [event hubs voorbeeld](functions-bindings-event-hubs.md#trigger---javascript-example).

### <a name="contextdone-method"></a>methode context.Done

```js
context.done([err],[propertyBag])
```

Hiermee kunt de runtime weet dat uw code is voltooid. Als uw functie wordt gebruikt de [ `async function` ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) declaratie, hoeft u niet te gebruiken `context.done()`. De `context.done` callback impliciet wordt genoemd. Async-functies zijn beschikbaar in het knooppunt 8 of een latere versie, hiervoor is versie 2.x van de Functions-runtime.

Als uw functie niet een functie asynchrone is **moet worden aangeroepen** `context.done` om te informeren over de runtime die uw functie voltooid is. De time-out op als deze ontbreekt.

De `context.done` methode kunt u weer zowel een gebruiker gedefinieerde fout doorgeven aan de runtime- en uitvoergegevens van de binding met een JSON-object. Eigenschappen doorgegeven aan `context.done` overschrijven alles instellen op de `context.bindings` object.

```javascript
// Even though we set myOutput to have:
//  -> text: 'hello world', number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method overwrites the myOutput binding to be: 
//  -> text: 'hello there, world', noNumber: true
```

### <a name="contextlog-method"></a>methode context.log  

```js
context.log(message)
```

Kunt u schrijven naar de streaminglogboeken functie op het standaardniveau voor tracering. Op `context.log`, extra logboekregistratie methoden zijn beschikbaar waarmee u de functie Logboeken op andere traceringsniveaus:


| Methode                 | Description                                |
| ---------------------- | ------------------------------------------ |
| **fout (_bericht_)**   | Schrijft naar foutniveau logboekregistratie of lager.   |
| **waarschuwing (_bericht_)**    | Schrijft naar waarschuwingsniveau logboekregistratie of lager. |
| **Info (_bericht_)**    | Schrijft naar info-niveau logboekregistratie of lager.    |
| **uitgebreide (_bericht_)** | Schrijft gegevens naar uitgebreide logboekregistratie op.           |

Het volgende voorbeeld schrijft u een logboek tijdens het traceerniveau waarschuwing:

```javascript
context.log.warn("Something has happened."); 
```

U kunt [de drempelwaarde trace-niveau voor logboekregistratie configureren](#configure-the-trace-level-for-console-logging) in het bestand host.json. Zie voor meer informatie over het schrijven van Logboeken, [trace-uitvoer schrijven](#writing-trace-output-to-the-console) hieronder.

Lezen [controlefuncties van Azure](functions-monitoring.md) voor meer informatie over het weergeven en uitvoeren van query's functielogboeken.

## <a name="writing-trace-output-to-the-console"></a>Trace-uitvoer schrijven naar de console 

In de functies, gebruikt u de `context.log` methoden trace-uitvoer schrijven naar de console. In de Functions-v2.x trace uitvoer met behulp van `console.log` zijn vastgelegd op het niveau van de functie-App. Dit betekent dat de uitvoer van `console.log` zijn niet gekoppeld aan een specifieke functieaanroepen en kan daarom niet worden weergegeven in een specifieke functie Logboeken. Ze doen, echter doorgeven naar Application Insights. In functies v1.x, u niet gebruiken `console.log` te schrijven naar de console.

Als u aanroept `context.log()`, het bericht is geschreven naar de console op het standaardniveau van trace die is de _info_ traceerniveau. De volgende code schrijft naar de console op het traceerniveau informatie:

```javascript
context.log({hello: 'world'});  
```

Deze code is gelijk aan de bovenstaande code:

```javascript
context.log.info({hello: 'world'});  
```

Deze code schrijft naar de console op het foutniveau van de:

```javascript
context.log.error("An error has occurred.");  
```

Omdat _fout_ is de hoogste niveau, deze tracering wordt geschreven naar de uitvoer op alle traceringsniveaus als logboekregistratie is ingeschakeld.

Alle `context.log` methoden ondersteunen dezelfde parameter-indeling die wordt ondersteund door de op Node.js [util.format methode](https://nodejs.org/api/util.html#util_util_format_format). Houd rekening met de volgende code, die schrijft de functielogboeken met behulp van het traceerniveau dat is standaard:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

U kunt ook de dezelfde code schrijven in de volgende indeling:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-the-trace-level-for-console-logging"></a>Het traceerniveau voor console-aanmelding configureren

Functions kunt u bij het definiëren van het traceerniveau drempelwaarde voor het schrijven van de console, Hierdoor is het eenvoudig om te bepalen de traceringen manier worden geschreven naar de console van uw functie. Als u wilt dat de drempelwaarde voor alle traceringen geschreven naar de console, gebruikt u de `tracing.consoleLevel` eigenschap in het bestand host.json. Deze instelling geldt voor alle functies in uw functie-app. Het volgende voorbeeld wordt de tracering drempelwaarde uitgebreide logboekregistratie inschakelen:

```json
{
    "tracing": {
        "consoleLevel": "verbose"
    }
}  
```

Waarden van **consoleLevel** komen overeen met de namen van de `context.log` methoden. Als u wilt uitschakelen op alle traceerlogboekregistratie naar de console, stelt **consoleLevel** naar _uit_. Zie voor meer informatie, [naslaginformatie over host.json](functions-host-json.md).

## <a name="http-triggers-and-bindings"></a>HTTP-triggers en bindingen

HTTP- en webhook-triggers en HTTP-output bindingen met de aanvraag en respons objecten vertegenwoordigen de HTTP-berichten.  

### <a name="request-object"></a>Request-object

De `context.req` (aanvraag)-object heeft de volgende eigenschappen:

| Eigenschap      | Description                                                    |
| ------------- | -------------------------------------------------------------- |
| _De hoofdtekst_        | Een object dat de hoofdtekst van de aanvraag bevat.               |
| _Headers_     | Een object dat de aanvraagheaders bevat.                   |
| _Methode_      | De HTTP-methode van de aanvraag.                                |
| _originalUrl_ | De URL van de aanvraag.                                        |
| _params_      | Een object dat de routering parameters van de aanvraag bevat. |
| _Query_       | Een object met de queryparameters.                  |
| _rawBody_     | De hoofdtekst van het bericht als een tekenreeks.                           |


### <a name="response-object"></a>Responsobject

De `context.res` (antwoord)-object heeft de volgende eigenschappen:

| Eigenschap  | Description                                               |
| --------- | --------------------------------------------------------- |
| _De hoofdtekst_    | Een object dat de hoofdtekst van het antwoord bevat.         |
| _Headers_ | Een object met de antwoordheaders.             |
| _isRaw_   | Geeft aan dat de opmaak is overgeslagen voor het antwoord.    |
| _status_  | De HTTP-statuscode van het antwoord.                     |

### <a name="accessing-the-request-and-response"></a>Toegang tot de aanvraag en respons 

Wanneer u met HTTP-triggers werkt, kunt u de HTTP-aanvraag en respons objecten in een aantal manieren openen:

+ **Van `req` en `res` eigenschappen op de `context` object.** Op deze manier kunt u het gebruikelijke patroon voor toegang tot HTTP gegevens uit het contextobject, de volledige gebruiken in plaats van `context.bindings.name` patroon. Het volgende voorbeeld ziet u hoe u toegang tot de `req` en `res` objecten op de `context`:

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ **Van de benoemde invoer- en uitvoerbindingen.** Op deze manier kunnen werken de HTTP-trigger en bindingen op dezelfde manier als andere bindingen. Het volgende voorbeeld wordt het antwoordobject met behulp van een benoemde `response` binding: 

    ```json
    {
        "type": "http",
        "direction": "out",
        "name": "response"
    }
    ```
    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```
+ **_[Alleen-antwoord]_  Door het aanroepen van `context.res.send(body?: any)`.** Een HTTP-antwoord wordt gemaakt met invoer `body` als hoofdtekst van het antwoord. `context.done()` impliciet wordt genoemd.

+ **_[Alleen-antwoord]_  Door het aanroepen van `context.done()`.** Een speciaal soort HTTP-binding retourneert het antwoord dat is doorgegeven aan de `context.done()` methode. De volgende HTTP-Uitvoerbinding definieert een `$return` uitvoerparameter:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version"></a>De versie van knooppunt

De volgende tabel ziet u de Node.js-versie die wordt gebruikt door elke primaire versie van de runtime van Functions:

| Versie van de functies | Versie van Node.js | 
|---|---|
| 1.x | 6.11.2 (vergrendeld door de runtime) |
| 2.x  | _Actieve LTS_ en even _huidige_ Node.js-versies (8.11.1 en 10.14.1 aanbevolen). De-versie instellen met behulp van de WEBSITE_NODE_DEFAULT_VERSION [app-instelling](functions-how-to-use-azure-function-app-settings.md#settings).|

U kunt zien dat de huidige versie die door de runtime wordt gebruikt door de bovenstaande appinstelling te controleren of door te drukken `process.version` van elke functie.

## <a name="dependency-management"></a>Beheer van afhankelijkheden
Als u wilt gebruiken van community-bibliotheken in uw JavaScript-code, zoals wordt weergegeven in het onderstaande voorbeeld, moet u ervoor zorgen dat alle afhankelijkheden zijn geïnstalleerd op uw functie-App in Azure.

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

> [!NOTE]
> U moet definiëren een `package.json` bestand in de hoofdmap van uw functie-App. Het bestand te definiëren, kunt alle functies in de app delen de dezelfde in de cache-pakketten, waardoor de beste prestaties. Als een conflict zich voordoet, kunt u deze oplossen door toe te voegen een `package.json` bestand in de map van een specifieke functie.  

Wanneer u een functie-Apps implementeert vanuit broncodebeheer, `package.json` bestand in uw opslagplaats aanwezig is, activeert een `npm install` in de map tijdens de implementatie. Maar wanneer u implementeert via de Portal of de CLI, moet u handmatig de om pakketten te installeren.

Er zijn twee manieren om pakketten te installeren op uw functie-App: 

### <a name="deploying-with-dependencies"></a>Implementeren met afhankelijkheden
1. Alle vereiste pakketten installeren door lokaal uit te voeren `npm install`.

2. Implementeer uw code en zorg ervoor dat de `node_modules` map is opgenomen in de implementatie. 


### <a name="using-kudu"></a>Kudu gebruiken
1. Ga naar `https://<function_app_name>.scm.azurewebsites.net`.

2. Klik op **Foutopsporingsconsole** > **CMD**.

3. Ga naar `D:\home\site\wwwroot`, en sleep vervolgens uw package.json-bestand naar de **wwwroot** map in het bovenste gedeelte van de pagina.  
    U kunt ook bestanden uploaden naar uw functie-app op andere manieren. Zie voor meer informatie, [het bijwerken van de functie app-bestanden](functions-reference.md#fileupdate). 

4. Nadat het package.json-bestand is geüpload, voert u de `npm install` opdracht in de **Kudu-console voor uitvoering op afstand**.  
    Met deze actie wordt gedownload van de pakketten die zijn aangegeven in het package.json-bestand en de functie-app opnieuw wordt opgestart.

## <a name="environment-variables"></a>Omgevingsvariabelen

In de functies, [app-instellingen](functions-app-settings.md), zoals serviceverbinding tekenreeksen, worden weergegeven als omgevingsvariabelen tijdens de uitvoering. U hebt toegang tot deze instellingen met behulp van `process.env`, zoals hier wordt weergegeven de `GetEnvironmentVariable` functie:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.log('Node.js timer trigger function ran!', timeStamp);
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));

    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Bij lokale uitvoering, app-instellingen worden gelezen uit de [local.settings.json](functions-run-local.md#local-settings-file) projectbestand.

## <a name="configure-function-entry-point"></a>Functie-ingangspunt configureren

De `function.json` eigenschappen `scriptFile` en `entryPoint` kan worden gebruikt om de locatie en naam van de geëxporteerde functie configureren. Deze eigenschappen kunnen belangrijk zijn wanneer uw JavaScript transpiled is.

### <a name="using-scriptfile"></a>Met behulp van `scriptFile`

Een JavaScript-functie wordt standaard uitgevoerd vanuit `index.js`, een bestand met dezelfde bovenliggende map als het bijbehorende gedeelde `function.json`.

`scriptFile` kan worden gebruikt voor het ophalen van een mapstructuur die lijkt op het volgende voorbeeld:

```
FunctionApp
 | - host.json
 | - myNodeFunction
 | | - function.json
 | - lib
 | | - nodeFunction.js
 | - node_modules
 | | - ... packages ...
 | - package.json
```

De `function.json` voor `myNodeFunction` moet bevatten een `scriptFile` eigenschap die verwijst naar het bestand met de geëxporteerde functie om uit te voeren.

```json
{
  "scriptFile": "../lib/nodeFunction.js",
  "bindings": [
    ...
  ]
}
```

### <a name="using-entrypoint"></a>Met behulp van `entryPoint`

In `scriptFile` (of `index.js`), een functie moet worden geëxporteerd met behulp van `module.exports` om te kunnen worden gevonden en worden uitgevoerd. De functie die wordt uitgevoerd wanneer geactiveerd is standaard de enige exporteren vanuit dat bestand, de uitvoer met de naam `run`, of het exporteren met de naam `index`.

Dit kan worden geconfigureerd met behulp van `entryPoint` in `function.json`, zoals in het volgende voorbeeld:

```json
{
  "entryPoint": "logFoo",
  "bindings": [
    ...
  ]
}
```

In v2.x van functies, die ondersteuning biedt voor de `this` parameter in de gebruikersfuncties de functiecode kan vervolgens worden zoals in het volgende voorbeeld:

```javascript
class MyObj {
    constructor() {
        this.foo = 1;
    };

    function logFoo(context) { 
        context.log("Foo is " + this.foo); 
        context.done(); 
    }
}

const myObj = new MyObj();
module.exports = myObj;
```

In dit voorbeeld is het belangrijk te weten dat hoewel een object wordt geëxporteerd, er geen garandeert dat zijn rond staat tussen uitvoerbewerkingen te behouden.

## <a name="considerations-for-javascript-functions"></a>Overwegingen voor JavaScript-functies

Wanneer u met JavaScript-functies werkt, worden op de hoogte van de overwegingen met betrekking tot in de volgende secties.

### <a name="choose-single-vcpu-app-service-plans"></a>Kies één vCPU Appservice-plannen

Wanneer u een functie-app die gebruikmaakt van de App Service-plan maakt, wordt u aangeraden dat u een één-vCPU-plan in plaats van een abonnement met meerdere vcpu's selecteert. Vandaag de dag functies uitgevoerd JavaScript-functies efficiënter op één vCPU-VM's en grotere VM's niet de verwachte prestatieverbeteringen produceren. Indien nodig, kunt u handmatig uitschalen door meer VM-instanties van één vCPU toe te voegen of kunt u automatisch schalen inschakelen. Zie voor meer informatie, [aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).    

### <a name="typescript-and-coffeescript-support"></a>Ondersteuning voor typeScript en CoffeeScript

Omdat direct-ondersteuning nog niet voor het compileren van automatische TypeScript of CoffeeScript via de runtime bestaat, moet deze ondersteuning buiten de runtime worden verwerkt tijdens de implementatie. 

### <a name="cold-start"></a>Koude Start

Bij het ontwikkelen van Azure Functions in de serverloze hostingmodel, koude start worden realiteit. *Koude start* verwijst naar het feit dat wanneer uw functie-app wordt gestart voor het eerst na een periode van inactiviteit, duurt het langer om opnieuw te starten. Voor JavaScript-functies met grote afhankelijkheidsstructuren in het bijzonder kan koude start aanzienlijk zijn. Het proces koude start sneller [uw functies worden uitgevoerd als een pakketbestand](run-functions-from-deployment-package.md) indien mogelijk. Veel implementatiemethoden gebruiken de uitvoering van pakket model standaard, maar als u grote koude starts ondervindt en niet op deze manier worden uitgevoerd, een aanzienlijke verbetering vormt door deze wijziging kan bieden.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

+ [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
+ [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
+ [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
