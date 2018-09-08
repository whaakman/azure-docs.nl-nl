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
ms.openlocfilehash: 36307c86332ac331e444d65ba27c044585379e68
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093394"
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
Retourneert een benoemde object dat al uw invoer- en gegevens bevat. Bijvoorbeeld, volgende definitie van de binding in uw *function.json* hebt u toegang de inhoud van de wachtrij van tot de `context.bindings.myInput` object. 

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

### <a name="contextdone-method"></a>methode context.Done
```
context.done([err],[propertyBag])
```

Informeert de runtime die uw code is voltooid. Als uw functie maakt gebruik van de `async function` declaratie (beschikbaar met behulp van knooppunt 8 + in functies versie 2.x), u niet wilt gebruiken `context.done()`. De `context.done` callback impliciet wordt genoemd.

Als uw functie niet een functie asynchrone is **moet worden aangeroepen `context.done`**  om te informeren over de runtime die uw functie voltooid is. Als deze ontbreekt, wordt de uitvoering time-out.

De `context.done` methode kunt u weer zowel een gebruiker gedefinieerde fout doorgeven aan de runtime en een eigenschappenverzameling van eigenschappen die de eigenschappen worden overschreven op het `context.bindings` object.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a>methode context.log  

```
context.log(message)
```
Kunt u schrijven naar de streaminglogboeken console op het standaardniveau voor tracering. Op `context.log`, extra logboekregistratie methoden zijn beschikbaar waarmee u naar het consolelogboek op andere traceringsniveaus schrijven:


| Methode                 | Beschrijving                                |
| ---------------------- | ------------------------------------------ |
| **fout (_bericht_)**   | Schrijft naar foutniveau logboekregistratie of lager.   |
| **waarschuwing (_bericht_)**    | Schrijft naar waarschuwingsniveau logboekregistratie of lager. |
| **Info (_bericht_)**    | Schrijft naar info-niveau logboekregistratie of lager.    |
| **uitgebreide (_bericht_)** | Schrijft gegevens naar uitgebreide logboekregistratie op.           |

Het volgende voorbeeld schrijft naar de console het traceerniveau waarschuwing:

```javascript
context.log.warn("Something has happened."); 
```
U kunt het traceerniveau drempelwaarde voor het registreren in het bestand host.json in te stellen of uitschakelen.  Zie de volgende sectie voor meer informatie over het schrijven naar de logboeken.

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

Andere opties voor `dataType` zijn `stream` en `string`.

## <a name="writing-trace-output-to-the-console"></a>Trace-uitvoer schrijven naar de console 

In de functies, gebruikt u de `context.log` methoden trace-uitvoer schrijven naar de console. Op dit moment kunt u niet gebruiken `console.log` te schrijven naar de console.

Als u aanroept `context.log()`, het bericht is geschreven naar de console op het standaardniveau van trace die is de _info_ traceerniveau. De volgende code schrijft naar de console op het traceerniveau informatie:

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


Alle `context.log` methoden ondersteunen dezelfde parameter-indeling die wordt ondersteund door de op Node.js [util.format methode](https://nodejs.org/api/util.html#util_util_format_format). Houd rekening met de volgende code schrijft naar de console met behulp van het traceerniveau dat is standaard:

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

De `request` object heeft de volgende eigenschappen:

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

De `response` object heeft de volgende eigenschappen:

| Eigenschap  | Beschrijving                                               |
| --------- | --------------------------------------------------------- |
| _De hoofdtekst_    | Een object dat de hoofdtekst van het antwoord bevat.         |
| _Headers_ | Een object met de antwoordheaders.             |
| _isRaw_   | Geeft aan dat de opmaak is overgeslagen voor het antwoord.    |
| _status_  | De HTTP-statuscode van het antwoord.                     |

### <a name="accessing-the-request-and-response"></a>Toegang tot de aanvraag en respons 

Wanneer u met HTTP-triggers werkt, kunt u de HTTP-aanvraag en respons objecten in een van drie manieren openen:

+ Van de benoemde invoer- en uitvoerbindingen. Op deze manier kunnen werken de HTTP-trigger en bindingen op dezelfde manier als andere bindingen. Het volgende voorbeeld wordt het antwoordobject met behulp van een benoemde `response` binding: 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ Van `req` en `res` eigenschappen op de `context` object. Op deze manier kunt u het gebruikelijke patroon voor toegang tot HTTP gegevens uit het contextobject, de volledige gebruiken in plaats van `context.bindings.name` patroon. Het volgende voorbeeld ziet u hoe u toegang tot de `req` en `res` objecten op de `context`:

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ Door het aanroepen van `context.done()`. Een speciaal soort HTTP-binding retourneert het antwoord dat is doorgegeven aan de `context.done()` methode. De volgende HTTP-Uitvoerbinding definieert een `$return` uitvoerparameter:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    Deze Uitvoerbinding wordt verwacht dat u om op te geven van het antwoord bij het aanroepen van `done()`, als volgt:

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a>Knooppunt-versie en package management

De volgende tabel ziet u de Node.js-versie die wordt gebruikt door elke primaire versie van de runtime van Functions:

| Versie van de functies | Versie van Node.js | 
|---|---|
| 1.x | 6.11.2 (vergrendeld door de runtime) |
| 2.x  | _Actieve LTS_ en _huidige_ Node.js-versies (8.11.1 en 10.6.0 aanbevolen). De-versie instellen met behulp van de WEBSITE_NODE_DEFAULT_VERSION [app-instelling](functions-how-to-use-azure-function-app-settings.md#settings).|

U kunt zien dat de huidige versie die door de runtime wordt gebruikt door af te drukken `process.version` van elke functie.

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

