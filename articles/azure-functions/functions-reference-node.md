---
title: JavaScript-referentie voor ontwikkelaars voor Azure Functions | Microsoft Docs
description: Begrijpen hoe functies ontwikkelen met behulp van JavaScript.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.service: functions
ms.devlang: nodejs
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: tdykstra
ms.openlocfilehash: f613e480f6699b323c18402f01873e565768f10f
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="azure-functions-javascript-developer-guide"></a>Azure Functions JavaScript-handleiding voor ontwikkelaars
[!INCLUDE [functions-selector-languages](../../includes/functions-selector-languages.md)]

De JavaScript-ervaring voor Azure Functions maakt het gemakkelijk te exporteren van een functie die wordt doorgegeven als een `context` voor communicatie met de runtime en voor het ontvangen en verzenden van gegevens via bindingen-object.

In dit artikel wordt ervan uitgegaan dat u al hebt gelezen de [naslaginformatie voor ontwikkelaars van Azure Functions](functions-reference.md).

## <a name="exporting-a-function"></a>Exporteren van een functie
Alle JavaScript-functies moeten één exporteren `function` via `module.exports` voor de runtime vinden van de functie en voer deze uit. Deze functie moet altijd bevatten een `context` object.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Bindingen van `direction === "in"` worden doorgegeven als argument, wat betekent dat u kunt gebruiken [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) dynamisch afhandelen nieuwe invoer (bijvoorbeeld met behulp van `arguments.length` worden herhaald voor de ingevoerde gegevens). Deze functie is handig wanneer u alleen een trigger en er zijn geen extra invoergegevens omdat u voorspelbare toegang uw triggergegevens zonder die verwijzen naar tot uw `context` object.

De argumenten worden altijd doorgegeven aan de functie in de volgorde waarin ze plaatsvinden in *function.json*, zelfs als u ze niet in de uitvoer-instructie opgeven. Als u hebt bijvoorbeeld `function(context, a, b)` en wijzig dit in `function(context, a)`, kunt u de waarde van krijgen `b` in de functiecode door te verwijzen naar `arguments[2]`.

Alle bindingen, ongeacht de richting, worden ook doorgegeven op de `context` object (Zie het volgende script). 

## <a name="context-object"></a>context-object
De runtime gebruikt een `context` object voor het doorgeven van gegevens naar en van de functie en kunt u communiceren met de runtime.

De `context` object is altijd de eerste parameter aan een functie en moet worden opgenomen omdat deze methoden, zoals `context.done` en `context.log`, die vereist zijn juist gebruik van de runtime. U kunt het object naam wat u wilt dat (bijvoorbeeld `ctx` of `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

### <a name="contextbindings-property"></a>de eigenschap context.bindings

```
context.bindings
```
Retourneert een benoemde object dat al uw invoer en uitvoer gegevens bevat. Bijvoorbeeld de volgende bindingsdefinitie in uw *function.json* hebt u toegang de inhoud van de wachtrij van tot de `context.bindings.myInput` object. 

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
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

### <a name="contextdone-method"></a>context.Done methode
```
context.done([err],[propertyBag])
```

Informeert de runtime die uw code is voltooid. U moet aanroepen `context.done`, of anders de runtime nooit weet dat uw functie voltooid is en de uitvoering wordt time-out. 

De `context.done` methode kunt u weer zowel een gebruiker gedefinieerde fout doorgeven aan de runtime en een eigenschappenverzameling van eigenschappen die het overschrijven van de eigenschappen op de `context.bindings` object.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a>context.log methode  

```
context.log(message)
```
Hiermee kunt u om te schrijven naar de logboeken van de streaming-console op het standaardniveau voor tracering. Op `context.log`, extra logboekregistratie methoden zijn beschikbaar waarmee u naar het consolelogboek op andere traceringsniveaus schrijven:


| Methode                 | Beschrijving                                |
| ---------------------- | ------------------------------------------ |
| **fout (_bericht_)**   | Schrijft naar foutniveau registreren of lager.   |
| **waarschuwen (_bericht_)**    | Schrijft naar waarschuwingsniveau registreren of lager. |
| **Info (_bericht_)**    | Schrijft naar info-niveau registreren of lager.    |
| **uitgebreide (_bericht_)** | Schrijft naar de uitgebreide logboekregistratie.           |

Het volgende voorbeeld wordt geschreven naar de console op het niveau voor het traceren van waarschuwing:

```javascript
context.log.warn("Something has happened."); 
```
U kunt stellen voor het registreren in het bestand host.json traceerniveau of uitschakelen.  Zie de volgende sectie voor meer informatie over het schrijven naar de logboeken.

## <a name="binding-data-type"></a>Binding-gegevenstype

Gebruik het definiëren van het gegevenstype voor een invoer-binding het `dataType` eigenschap in de definitie van de binding. Gebruik bijvoorbeeld om te lezen van de inhoud van een HTTP-aanvraag in binaire indeling, het type `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Andere opties voor `dataType` zijn `stream` en `string`.

## <a name="writing-trace-output-to-the-console"></a>Trace-uitvoer schrijven naar de console 

In de functies, gebruikt u de `context.log` methoden trace-uitvoer schrijven naar de console. Op dit moment kunt u niet gebruiken `console.log` schrijven naar de console.

Als u aanroept `context.log()`, het bericht is geschreven naar de console op het standaardniveau van trace die de _info_ traceerniveau. De volgende code schrijft naar de console op het info-Traceerniveau:

```javascript
context.log({hello: 'world'});  
```

De bovenstaande code is gelijk aan de volgende code:

```javascript
context.log.info({hello: 'world'});  
```

De volgende code schrijft naar de console op het foutniveau van de:

```javascript
context.log.error("An error has occurred.");  
```

Omdat _fout_ is de hoogste niveau, deze tracering wordt geschreven naar de uitvoer op alle traceringsniveaus als logboekregistratie is ingeschakeld.  


Alle `context.log` methoden ondersteunen dezelfde indeling voor parameter die wordt ondersteund door de Node.js [util.format methode](https://nodejs.org/api/util.html#util_util_format_format). Houd rekening met de volgende code, naar de console schrijft met behulp van het standaard Traceerniveau:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

U kunt ook de dezelfde code schrijven in de volgende indeling:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-the-trace-level-for-console-logging"></a>Het traceerniveau voor console-logboekregistratie configureren

Functies kunt u het traceerniveau drempelwaarde voor het schrijven naar de console kunt u gemakkelijk om te bepalen die anders de traceringen manier worden geschreven naar de console van uw functies definiëren. U stelt de drempelwaarde voor alle traces geschreven naar de console met de `tracing.consoleLevel` eigenschap in het bestand host.json. Deze instelling geldt voor alle functies in uw app in de functie. Het volgende voorbeeld wordt de tracering drempelwaarde uitgebreide logboekregistratie inschakelen:

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

Waarden van **consoleLevel** overeenkomen met de namen van de `context.log` methoden. Schakel alle traceerlogboek in de console ingesteld **consoleLevel** naar _uit_. Zie voor meer informatie [host.json verwijzing](functions-host-json.md).

## <a name="http-triggers-and-bindings"></a>HTTP-triggers en bindingen

HTTP- en webhook triggers en HTTP-uitvoer bindingen met de aanvraag en -antwoord objecten vertegenwoordigen de HTTP-berichten.  

### <a name="request-object"></a>Request-object

De `request` object heeft de volgende eigenschappen:

| Eigenschap      | Beschrijving                                                    |
| ------------- | -------------------------------------------------------------- |
| _hoofdtekst_        | Een object dat de hoofdtekst van de aanvraag bevat.               |
| _headers_     | Een object dat de aanvraagheaders bevat.                   |
| _methode_      | De HTTP-methode van de aanvraag.                                |
| _originalUrl_ | De URL van de aanvraag.                                        |
| _parameters voor_      | Een object dat de routering parameters van de aanvraag bevat. |
| _query_       | Een object dat de queryparameters bevat.                  |
| _rawBody_     | De hoofdtekst van het bericht als een tekenreeks.                           |


### <a name="response-object"></a>Response-object

De `response` object heeft de volgende eigenschappen:

| Eigenschap  | Beschrijving                                               |
| --------- | --------------------------------------------------------- |
| _hoofdtekst_    | Een object dat de hoofdtekst van het antwoord bevat.         |
| _headers_ | Een object met de antwoordheaders.             |
| _isRaw_   | Hiermee wordt aangegeven dat opmaak voor het antwoord is overgeslagen.    |
| _status_  | De HTTP-statuscode van het antwoord.                     |

### <a name="accessing-the-request-and-response"></a>Toegang tot de aanvraag en -antwoord 

Wanneer u met HTTP-triggers werkt, kunt u de HTTP-aanvraag en antwoord-objecten in een van drie manieren openen:

+ Van de benoemde invoer en uitvoer bindingen. Op deze manier kunnen werken de HTTP-trigger en bindingen op dezelfde manier als andere bindingen. Het volgende voorbeeld wordt het antwoordobject met behulp van een benoemde `response` binding: 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ Van `req` en `res` eigenschappen op de `context` object. Op deze manier kunt u het conventionele patroon voor toegang tot HTTP gegevens uit het contextobject de volledige gebruiken in plaats van `context.bindings.name` patroon. Het volgende voorbeeld laat zien hoe voor toegang tot de `req` en `res` objecten op de `context`:

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ Door het aanroepen van `context.done()`. Een speciaal soort HTTP-binding retourneert de reactie die wordt doorgegeven aan de `context.done()` methode. De volgende HTTP-uitvoer binding definieert een `$return` uitvoerparameter:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    Deze binding uitvoer verwacht op te geven van het antwoord bij het aanroepen van `done()`, als volgt:

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a>Knooppunt-versie en pakket-management
De versie van het knooppunt is momenteel vergrendeld op `6.5.0`. We bent voor het onderzoeken van het toevoegen van ondersteuning voor meer versies en waardoor het worden geconfigureerd.

De volgende stappen kunt u pakketten opnemen in uw app in functie: 

1. Ga naar `https://<function_app_name>.scm.azurewebsites.net`.

2. Klik op **Console voor foutopsporing** > **CMD**.

3. Ga naar `D:\home\site\wwwroot`, en sleep uw package.json-bestand naar de **wwwroot** map in het bovenste gedeelte van de pagina.  
    U kunt ook bestanden uploaden naar uw app in de functie op andere manieren. Zie voor meer informatie [het bijwerken van de functie app-bestanden](functions-reference.md#fileupdate). 

4. Nadat de package.json-bestand is geüpload, voert u de `npm install` opdracht in de **Kudu-console voor uitvoering op afstand**.  
    Deze actie de pakketten die zijn aangegeven in de package.json-bestand wordt gedownload en de functie-app opnieuw wordt opgestart.

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

U moet definiëren een `package.json` bestand in de hoofdmap van uw app functie. Het definiëren van het bestand, kunt alle functies in de app delen de dezelfde in de cache pakketten, waardoor de beste prestaties. Als een conflict ontstaat, u het kunt oplossen door toe te voegen een `package.json` bestand in de map van een specifieke functie.  

## <a name="environment-variables"></a>Omgevingsvariabelen
Als u een omgevingsvariabele of een app die waarde instellen, gebruikt `process.env`, zoals weergegeven in het volgende voorbeeld:

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

Wanneer u met JavaScript-functies werkt, rekening houden met het in de volgende twee secties worden.

### <a name="choose-single-vcpu-app-service-plans"></a>Kies één vCPU App Service-abonnementen

Wanneer u een functie-app die gebruikmaakt van de App Service-abonnement maakt, wordt u aangeraden dat u een plan één vCPU in plaats van een plan met meerdere vcpu's selecteert. Vandaag de dag functies JavaScript-functies efficiënter uitgevoerd op één vCPU VM's en met behulp van grotere virtuele machines niet de verwachte prestatieverbeteringen produceren. Indien nodig, zodat u handmatig kunt uitbreiden door meer VM-instanties van één vCPU toe te voegen of u automatisch schalen kunt inschakelen. Zie voor meer informatie [aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).    

### <a name="typescript-and-coffeescript-support"></a>Ondersteuning voor machineschrift en CoffeeScript
Omdat rechtstreekse ondersteuning nog niet voor het compileren van automatische machineschrift of CoffeeScript via de runtime bestaat, moet deze ondersteuning buiten de runtime worden verwerkt tijdens de implementatie. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie:

* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)

