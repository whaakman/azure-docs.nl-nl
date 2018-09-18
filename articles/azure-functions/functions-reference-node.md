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
ms.date: 03/04/2018
ms.author: glenga
ms.openlocfilehash: a8ee92d117a416d638f62b573dfb155f67bf66e0
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983136"
---
# <a name="azure-functions-javascript-developer-guide"></a>Handleiding voor ontwikkelaars van Azure Functions-JavaScript

De JavaScript-ervaring voor Azure Functions kunt u eenvoudig voor het exporteren van een functie die wordt doorgegeven als een `context` -object om te communiceren met de runtime en voor het ontvangen en verzenden van gegevens via bindingen.

In dit artikel wordt ervan uitgegaan dat u al hebt gelezen de [referentie voor ontwikkelaars van Azure Functions](functions-reference.md).

## <a name="exporting-a-function"></a>Exporteren van een functie
Elke JavaScript-functie moet een enkel exporteren `function` via `module.exports` voor de runtime functie voor het vinden en voer deze uit. Altijd moet rekening houden met deze functie een `context` object als de eerste parameter.

```javascript
// You must include a context, other arguments are optional
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
    context.done();
};
// You can also use 'arguments' to dynamically handle inputs
module.exports = function(context) {
    context.log('Number of inputs: ' + arguments.length);
    // Iterates through trigger and input binding data
    for (i = 1; i < arguments.length; i++){
        context.log(arguments[i]);
    }
    context.done();
};
```

Invoer- en trigger-Bindingen (bindingen van `direction === "in"`) kunnen worden doorgegeven aan de functie als parameters. Ze worden doorgegeven aan de functie in dezelfde volgorde als waarin ze zijn gedefinieerd in *function.json*. U kunt dynamisch invoer met de JavaScript verwerken [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) object. Als u hebt bijvoorbeeld `function(context, a, b)` en wijzig deze in `function(context, a)`, krijgt u nog steeds de waarde van `b` in functiecode door te verwijzen naar `arguments[2]`.

Alle bindingen, ongeacht de richting, ook worden doorgegeven in de `context` object met de `context.bindings` eigenschap.

## <a name="context-object"></a>context-object
De runtime wordt gebruikt een `context` object om door te geven gegevens van en naar uw functie en u communiceren met de runtime te laten.

De `context` object is altijd de eerste parameter voor een functie en moet worden opgenomen omdat deze methoden, zoals `context.done` en `context.log`, die vereist zijn correct gebruik van de runtime. U kunt het object naam wat u graag (bijvoorbeeld `ctx` of `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
    context.done();
};
```

### <a name="contextbindings-property"></a>de eigenschap context.bindings

```
context.bindings
```
Retourneert een benoemde object dat al uw invoer- en gegevens bevat. Bijvoorbeeld, de volgende bindingsdefinities in uw *function.json* hebt u toegang de inhoud van een wachtrij van tot `context.bindings.myInput` en uitvoer toewijzen aan een wachtrij met `context.bindings.myOutput`.

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

Houd er rekening mee dat u kiezen kunt voor het definiëren van uitvoer binding gegevens met de `context.done` methode in plaats van de `context.binding` object (Zie hieronder).

### <a name="contextbindingdata-property"></a>de eigenschap context.bindingData

```
context.bindingData
```
Retourneert een benoemde-object dat gegevens van trigger metagegevens en de functie aanroepen bevat (`invocationId`, `sys.methodName`, `sys.utcNow`, `sys.randGuid`). Zie voor een voorbeeld van de metagegevens van de trigger, [event hubs voorbeeld](functions-bindings-event-hubs.md#trigger---javascript-example).

### <a name="contextdone-method"></a>methode context.Done
```
context.done([err],[propertyBag])
```

Informeert de runtime die uw code is voltooid. Als uw functie gebruikmaakt van de JavaScript [ `async function` ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) declaratie (beschikbaar met behulp van knooppunt 8 + in functies versie 2.x), u hoeft niet te gebruiken `context.done()`. De `context.done` callback impliciet wordt genoemd.

Als uw functie niet een functie asynchrone is **moet worden aangeroepen `context.done`**  om te informeren over de runtime die uw functie voltooid is. Als deze ontbreekt, wordt de uitvoering time-out.

De `context.done` methode kunt u weer zowel een gebruiker gedefinieerde fout doorgeven aan de runtime- en uitvoergegevens van de binding met een JSON-object. Eigenschappen doorgegeven aan `context.done` overschrijft alles instellen op de `context.bindings` object.

```javascript
// Even though we set myOutput to have:
//  -> text: 'hello world', number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: 'hello there, world', noNumber: true
```

### <a name="contextlog-method"></a>methode context.log  

```
context.log(message)
```
Kunt u schrijven naar de streaminglogboeken functie op het standaardniveau voor tracering. Op `context.log`, extra logboekregistratie methoden zijn beschikbaar waarmee u de functie Logboeken op andere traceringsniveaus:


| Methode                 | Beschrijving                                |
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

## <a name="binding-data-type"></a>Binding-gegevenstype

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

## <a name="writing-trace-output-to-the-console"></a>Trace-uitvoer schrijven naar de console 

In de functies, gebruikt u de `context.log` methoden trace-uitvoer schrijven naar de console. In functies v1.x, u niet gebruiken `console.log` te schrijven naar de console. Traceren in v2.x van functies, ouputs via `console.log` zijn vastgelegd op het niveau van de functie-App. Dit betekent dat de uitvoer van `console.log` zijn niet gekoppeld aan een specifieke functie-aanroep.

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

Functions kunt u bij het definiëren van het traceerniveau drempelwaarde voor het schrijven van de console, Hierdoor is het eenvoudig om te bepalen de traceringen manier worden geschreven naar de console van uw functies. Als u wilt dat de drempelwaarde voor alle traceringen geschreven naar de console, gebruikt u de `tracing.consoleLevel` eigenschap in het bestand host.json. Deze instelling geldt voor alle functies in uw functie-app. Het volgende voorbeeld wordt de tracering drempelwaarde uitgebreide logboekregistratie inschakelen:

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

| Eigenschap      | Beschrijving                                                    |
| ------------- | -------------------------------------------------------------- |
| _De hoofdtekst_        | Een object dat de hoofdtekst van de aanvraag bevat.               |
| _Headers_     | Een object dat de aanvraagheaders bevat.                   |
| _Methode_      | De HTTP-methode van de aanvraag.                                |
| _originalUrl_ | De URL van de aanvraag.                                        |
| _params_      | Een object dat de routering parameters van de aanvraag bevat. |
| _Query_       | Een object met de queryparameters.                  |
| _rawBody_     | De hoofdtekst van het bericht als een tekenreeks.                           |


### <a name="response-object"></a>Antwoordobject

De `context.res` (antwoord)-object heeft de volgende eigenschappen:

| Eigenschap  | Beschrijving                                               |
| --------- | --------------------------------------------------------- |
| _De hoofdtekst_    | Een object dat de hoofdtekst van het antwoord bevat.         |
| _Headers_ | Een object met de antwoordheaders.             |
| _isRaw_   | Geeft aan dat de opmaak is overgeslagen voor het antwoord.    |
| _status_  | De HTTP-statuscode van het antwoord.                     |

### <a name="accessing-the-request-and-response"></a>Toegang tot de aanvraag en respons 

Wanneer u met HTTP-triggers werkt, kunt u de HTTP-aanvraag en respons objecten in een aantal manieren openen:

+ Van `req` en `res` eigenschappen op de `context` object. Op deze manier kunt u het gebruikelijke patroon voor toegang tot HTTP gegevens uit het contextobject, de volledige gebruiken in plaats van `context.bindings.name` patroon. Het volgende voorbeeld ziet u hoe u toegang tot de `req` en `res` objecten op de `context`:

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ Van de benoemde invoer- en uitvoerbindingen. Op deze manier kunnen werken de HTTP-trigger en bindingen op dezelfde manier als andere bindingen. Het volgende voorbeeld wordt het antwoordobject met behulp van een benoemde `response` binding: 

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
+ _[Alleen-antwoord]_  Door het aanroepen van `context.res.send(body?: any)`. Een HTTP-antwoord wordt gemaakt met invoer `body` als hoofdtekst van het antwoord. `context.done()` impliciet wordt genoemd.

+ _[Alleen-antwoord]_  Door het aanroepen van `context.done()`. Een speciaal soort HTTP-binding retourneert het antwoord dat is doorgegeven aan de `context.done()` methode. De volgende HTTP-Uitvoerbinding definieert een `$return` uitvoerparameter:

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
| 2.x  | _Actieve LTS_ en _huidige_ Node.js-versies (8.11.1 en 10.6.0 aanbevolen). De-versie instellen met behulp van de WEBSITE_NODE_DEFAULT_VERSION [app-instelling](functions-how-to-use-azure-function-app-settings.md#settings).|

U kunt zien dat de huidige versie die door de runtime wordt gebruikt door af te drukken `process.version` van elke functie.

## <a name="package-management"></a>Pakketbeheer
De volgende stappen kunnen u pakketten opnemen in uw functie-app: 

1. Ga naar `https://<function_app_name>.scm.azurewebsites.net`.

2. Klik op **Foutopsporingsconsole** > **CMD**.

3. Ga naar `D:\home\site\wwwroot`, en sleep vervolgens uw package.json-bestand naar de **wwwroot** map in het bovenste gedeelte van de pagina.  
    U kunt ook bestanden uploaden naar uw functie-app op andere manieren. Zie voor meer informatie, [het bijwerken van de functie app-bestanden](functions-reference.md#fileupdate). 

4. Nadat het package.json-bestand is geüpload, voert u de `npm install` opdracht in de **Kudu-console voor uitvoering op afstand**.  
    Met deze actie wordt gedownload van de pakketten die zijn aangegeven in het package.json-bestand en de functie-app opnieuw wordt opgestart.

Nadat de pakketten die u moet zijn geïnstalleerd, u deze importeren naar de functie door het aanroepen van `require('packagename')`, zoals in het volgende voorbeeld:

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

U moet definiëren een `package.json` bestand in de hoofdmap van uw functie-app. Het bestand te definiëren, kunt alle functies in de app delen de dezelfde in de cache-pakketten, waardoor de beste prestaties. Als een conflict zich voordoet, kunt u deze oplossen door toe te voegen een `package.json` bestand in de map van een specifieke functie.  

## <a name="environment-variables"></a>Omgevingsvariabelen
Als u een omgevingsvariabele of een app-instelling waarde, gebruikt u `process.env`, zoals hier wordt weergegeven de `GetEnvironmentVariable` functie:

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
## <a name="considerations-for-javascript-functions"></a>Overwegingen voor JavaScript-functies

Wanneer u met JavaScript-functies werkt, worden op de hoogte van de overwegingen voor in de volgende twee secties.

### <a name="choose-single-vcpu-app-service-plans"></a>Kies één vCPU Appservice-plannen

Wanneer u een functie-app die gebruikmaakt van de App Service-plan maakt, wordt u aangeraden dat u een één-vCPU-plan in plaats van een abonnement met meerdere vcpu's selecteert. Vandaag de dag functies uitgevoerd JavaScript-functies efficiënter op één vCPU-VM's en grotere VM's niet de verwachte prestatieverbeteringen produceren. Indien nodig, kunt u handmatig uitschalen door meer VM-instanties van één vCPU toe te voegen of kunt u automatisch schalen inschakelen. Zie voor meer informatie, [aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).    

### <a name="typescript-and-coffeescript-support"></a>Ondersteuning voor typeScript en CoffeeScript
Omdat direct-ondersteuning nog niet voor het compileren van automatische TypeScript of CoffeeScript via de runtime bestaat, moet deze ondersteuning buiten de runtime worden verwerkt tijdens de implementatie. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie:

* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)

