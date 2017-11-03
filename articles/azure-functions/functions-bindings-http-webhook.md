---
title: Azure Functions HTTP- en webhook bindingen | Microsoft Docs
description: Het gebruik van HTTP- en webhook triggers en bindingen in de Azure Functions begrijpen.
services: functions
documentationcenter: na
author: mattchenderson
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, gebeurtenis verwerking, webhooks, dynamische Reken-, zonder server-architectuur, HTTP-API, REST
ms.assetid: 2b12200d-63d8-4ec1-9da8-39831d5a51b1
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: mahender
ms.openlocfilehash: 3c3247592cbe2bc382d220264b0c646ee566b8a7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-http-and-webhook-bindings"></a>Azure Functions HTTP- en webhook bindingen
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe configureren en werken met HTTP-triggers en bindingen in de Azure Functions.
Met deze, kunt u Azure Functions te bouwen zonder Server API's en reageren op webhooks.

Azure Functions bevat de volgende bindingen:
- Een [HTTP-trigger](#httptrigger) kunt u een functie met een HTTP-aanvraag worden aangeroepen. Dit kan worden aangepast om te reageren op [webhooks](#hooktrigger).
- Een [HTTP uitvoer binding](#output) kunt u om te reageren op de aanvraag.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a>HTTP-trigger
De HTTP-trigger voert de functie in reactie op een HTTP-aanvraag. U kunt aanpassen om te reageren op een bepaalde URL of set HTTP-methoden. Een HTTP-trigger kan ook worden geconfigureerd om te reageren op webhooks. 

Als u de functies portal gebruikt, u kunt ook aan de slag meteen met een vooraf gemaakte sjabloon. Selecteer **nieuwe functie** en kiest u 'API & Webhooks' in de **Scenario** vervolgkeuzelijst. Selecteer een van de sjablonen en klik op **maken**.

Standaard wordt een HTTP-trigger op de aanvraag met een HTTP 200 OK-statuscode en een lege hoofdtekst reageert. Configureren voor het wijzigen van het antwoord een [HTTP uitvoer binding](#output)

### <a name="configuring-an-http-trigger"></a>Configureren van een HTTP-trigger
Een HTTP-trigger wordt gedefinieerd door een JSON-object in de `bindings` matrix van function.json, zoals wordt weergegeven in het volgende voorbeeld:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function",
    "methods": [ "get" ],
    "route": "values/{id}"
},
```
De binding ondersteunt de volgende eigenschappen:

|Eigenschap  |Beschrijving  |
|---------|---------|
| **naam** | Vereist: de naam van de variabele in functiecode gebruikt voor de aanvraag of de hoofdtekst van de aanvraag. Zie [werken met een HTTP-trigger vanuit code](#httptriggerusage). |
| **type** | Vereist - moet worden ingesteld op `httpTrigger`. |
| **richting** | Vereist - moet worden ingesteld op `in`. |
| **authLevel** | Hiermee bepaalt u wat sleutels, indien van toepassing, aanwezig zijn op de aanvraag moeten om de functie aanroepen. De waarde kan een van de volgende waarden zijn: <ul><li><code>anonymous</code>&mdash;Er is geen API-sleutel is vereist.</li><li><code>function</code>&mdash;Een specifieke functies API-sleutel is vereist. Dit is de standaardwaarde als niets wordt opgegeven.</li><li><code>admin</code>&mdash;De hoofdsleutel is vereist.</li></ul> Zie voor meer informatie [werken met sleutels](#keys). |
| **methoden** | Een matrix met de HTTP-methoden waarop de functie reageert. Als niet wordt opgegeven, wordt de functie reageert op alle HTTP-methoden. Zie [aanpassen van het HTTP-eindpunt](#url). |
| **route** | Definieert het Routesjabloon aanvragen waarmee uw functie reageert URL's beheren. De standaardwaarde als niets wordt opgegeven is `<functionname>`. Zie voor meer informatie [aanpassen van het HTTP-eindpunt](#url). |
| **webHookType** | Hiermee wordt de HTTP-trigger om te fungeren als de ontvanger van een webhook voor de opgegeven provider. Gebruik niet de _methoden_ eigenschap bij gebruik van deze instelling. De waarde kan een van de volgende waarden zijn:<ul><li><code>genericJson</code>&mdash;Een eindpunt voor algemene doeleinden webhook zonder logica voor een specifieke provider.</li><li><code>github</code>&mdash;De functie reageert op GitHub webhooks. Gebruik niet de _authLevel_ eigenschap als u deze waarde.</li><li><code>slack</code>&mdash;De functie reageert op toegestane webhooks. Gebruik niet de _authLevel_ eigenschap als u deze waarde.</li></ul> Zie voor meer informatie [reageert op webhooks](#hooktrigger). |

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a>Werken met een HTTP-trigger van code
Voor C# en F #, kunt u het type van de invoer voor deze trigger declareren `HttpRequestMessage` of een aangepaste .NET-type. Als u ervoor kiest `HttpRequestMessage`, krijgt u volledige toegang tot het request-object. Voor een aangepaste .NET-type probeert functies parseren van de JSON-aanvraagtekst om in te stellen van eigenschappen van het object. 

De runtime van Functions biedt voor Node.js-functies, de aanvraagtekst in plaats van het request-object. Zie voor meer informatie [voorbeelden van HTTP-trigger](#httptriggersample).


<a name="output"></a>
## <a name="http-response-output-binding"></a>Binding voor de HTTP-antwoord van de uitvoer
Gebruik de uitvoer van de HTTP-binding om te reageren op de afzender van HTTP-aanvraag. Deze binding vereist een HTTP-trigger en Hiermee kunt u het antwoord is gekoppeld aan de aanvraag van de trigger aanpassen. Als een HTTP-uitvoer binding is niet opgegeven, wordt een HTTP-trigger retourneert HTTP 200 OK met een lege hoofdtekst. 

### <a name="configuring-an-http-output-binding"></a>Binding uitvoer voor het configureren van een HTTP
Een HTTP-uitvoer binding wordt gedefinieerd, een JSON-object in de `bindings` matrix van function.json, zoals wordt weergegeven in het volgende voorbeeld:

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
De binding ondersteunt de volgende vereiste eigenschappen:

|Eigenschap  |Beschrijving  |
|---------|---------|
|**naam** | De naam van de variabele in functiecode gebruikt voor het antwoord. Zie [werken met een HTTP-uitvoer binding vanuit code](#outputusage). |
| **type** |moet worden ingesteld op `http`. |
| **richting** | moet worden ingesteld op `out`. |

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a>Werken met een HTTP-uitvoer binding van code
U kunt de uitvoerparameter gebruiken om te reageren op de aanroeper HTTP- of webhook. U kunt ook de patronen language standaard antwoord gebruiken. Bijvoorbeeld reacties, Zie [voorbeelden van HTTP-trigger](#httptriggersample) en [Webhook trigger voorbeelden](#hooktriggersample).


<a name="hooktrigger"></a>
## <a name="responding-to-webhooks"></a>Reageren op webhooks.
Een HTTP-trigger met de _webHookType_ eigenschap is geconfigureerd om te reageren op [webhooks](https://en.wikipedia.org/wiki/Webhook). De instelling 'genericJson' maakt gebruik van de basisconfiguratie. Hiermee beperkt u aanvragen tot alleen degenen met behulp van HTTP POST en met de `application/json` inhoudstype.

De trigger kan ook worden aangepast aan een specifieke webhook-provider, zoals [GitHub](https://developer.github.com/webhooks/) of [Slack](https://api.slack.com/outgoing-webhooks). Wanneer een provider wordt opgegeven, kan de runtime van Functions validatielogica van de provider voor u verwerken.  

### <a name="configuring-github-as-a-webhook-provider"></a>GitHub als een webhook-provider configureren
Om te reageren op GitHub webhooks, eerst uw functie te maken met een HTTP-Trigger en stel de **webHookType** eigenschap `github`. Kopieer de [URL](#url) en [API-sleutel](#keys) in de **webhook toevoegen** pagina van de GitHub-opslagplaats. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

Zie voor een voorbeeld [maken van een functie die wordt geactiveerd door een GitHub webhook](functions-create-github-webhook-triggered-function.md).

### <a name="configuring-slack-as-a-webhook-provider"></a>Vertraging configureren als een webhook-provider
De toegestane webhook genereert een token voor u in plaats van waarin u kunt opgeven, dus u een sleutel functiespecifieke met het token van de toegestane vertraging configureren moet. Zie [werken met sleutels](#keys).

<a name="url"></a>
## <a name="customizing-the-http-endpoint"></a>Het HTTP-eindpunt aanpassen
Wanneer u een functie voor een HTTP-trigger of WebHook, is de functie standaard adresseerbare met een route van het formulier:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

U kunt deze route met de optionele `route` eigenschap voor de HTTP-trigger binding de invoer. Als u bijvoorbeeld de volgende *function.json* -bestand definieert een `route` eigenschap voor een HTTP-trigger:

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

Met behulp van deze configuratie de functie is nu adresseerbare met de volgende route in plaats van de oorspronkelijke route.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Hierdoor kan de functiecode ter ondersteuning van twee parameters in het adres _categorie_ en _id_. U kunt een [Web API Route beperking](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) met de parameters. De volgende C#-functiecode maakt gebruik van beide parameters.

```csharp
public static Task<HttpResponseMessage> Run(HttpRequestMessage req, string category, int? id, 
                                                TraceWriter log)
{
    if (id == null)
        return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
    else
        return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
}
```

Hier volgt een Node.js-code voor de functie dezelfde Routeparameters gebruiken.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;

    if (!id) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: category + " item with id = " + id + " was requested."
        };
    }

    context.done();
} 
```

Standaard worden alle routes van de functie voorafgegaan door *api*. U kunt ook aanpassen of verwijderen van het voorvoegsel met behulp van de `http.routePrefix` eigenschap in uw *host.json* bestand. Het volgende voorbeeld verwijdert u de *api* routeprefix met behulp van een lege tekenreeks voor het voorvoegsel op in de *host.json* bestand.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

Voor gedetailleerde informatie over het bijwerken van de *host.json* -bestand voor de functie, Zie [het bijwerken van de functie app-bestanden](functions-reference.md#fileupdate). 

Voor informatie over andere eigenschappen kunt u configureren uw *host.json* bestand, Zie [host.json verwijzing](functions-host-json.md).


<a name="keys"></a>
## <a name="working-with-keys"></a>Werken met sleutels
HTTP-triggers kunnen u sleutels gebruiken voor extra beveiliging. Een standaard HTTP-trigger kunt gebruiken dat deze als een API-sleutel, vereisen de sleutel moet aanwezig zijn op de aanvraag. Webhooks kunt sleutels gebruiken voor het toestaan van aanvragen in verschillende manieren, afhankelijk van wat de provider ondersteunt.

Sleutels worden opgeslagen als onderdeel van de functie-app in Azure en zijn versleuteld in rust. Als u wilt uw sleutels weergeven, nieuwe te maken of sleutels rouleert met nieuwe waarden, navigeer naar een van uw functies in de portal en selecteer 'Beheren'. 

Er zijn twee soorten sleutels:
- **Hostsleutels**: deze sleutels worden gedeeld door alle functies in de functie-app. Wanneer als een API-sleutel gebruikt, kunnen toegang tot een functie binnen de functie-app.
- **Functietoetsen**: deze sleutels alleen van toepassing op de specifieke functies waarin ze zijn gedefinieerd. Wanneer als een API-sleutel gebruikt, kan deze alleen toegang tot deze functie.

Elke sleutel met de naam voor de verwijzing en er is een standaard-sleutel (met de naam 'standaard') op het niveau van de functie en de host. De **hoofdsleutel** is een standaard host-sleutel met de naam '_master' die is gedefinieerd voor elke functie-app. Deze sleutel kan niet worden ingetrokken. Het biedt beheerderstoegang tot de runtime-API's. Met behulp van `"authLevel": "admin"` in de binding JSON is vereist voor deze sleutel moeten worden weergegeven in de aanvraag; een andere toets resulteert in het autorisatiefouten.

> [!IMPORTANT]  
> Vanwege de verhoogde machtigingen verleend door de hoofdsleutel, moet u geen deze sleutel delen met derden of distribueren in systeemeigen clienttoepassingen. Wees voorzichtig bij het kiezen van de beheerder autorisatieniveau.

### <a name="api-key-authorization"></a>API-sleutel autorisatie
Een HTTP-trigger moet standaard een API-sleutel in de HTTP-aanvraag. Dus de HTTP-aanvraag normaal ziet er als volgt:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

De sleutel kan worden opgenomen in een queryreeks-variabele met de naam `code`, zoals hierboven, of kan worden opgenomen in een `x-functions-key` HTTP-header. De waarde van de sleutel mag een gedefinieerd voor de functie functietoets of op een host-toets.

U kunt anonieme aanvragen waarvoor geen sleutels toestaan. U kunt ook vereisen dat de hoofdsleutel worden gebruikt. Wijzigen van het standaardniveau voor autorisatie via de `authLevel` eigenschap in de JSON van de binding. Zie voor meer informatie [HTTP-trigger](#httptrigger).

### <a name="keys-and-webhooks"></a>Sleutels en webhooks.
Webhook autorisatie wordt verwerkt door het onderdeel van de ontvanger webhook, onderdeel van de HTTP-trigger en het mechanisme voor varieert op basis van de webhook-type. Elke mechanisme komt, maar afhankelijk zijn van een sleutel. De functie-sleutel met de naam 'standaard' wordt standaard gebruikt. Configureer de webhook-provider voor het verzenden van de naam van de sleutel met de aanvraag in een van de volgende manieren voor het gebruik van een andere sleutel:

- **Querytekenreeks**: de provider geeft de naam van de sleutel in de `clientid` querytekenreeksparameter, zoals `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
- **Aanvraagheader**: de provider geeft de naam van de sleutel in de `x-functions-clientid` header.

> [!NOTE]
> Functietoetsen hebben voorrang op de hostsleutels. Wanneer twee sleutels zijn gedefinieerd met dezelfde naam, wordt de functietoets altijd gebruikt.


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a>Voorbeelden van HTTP-trigger
Stel dat u hebt de volgende HTTP-trigger in de `bindings` matrix van function.json:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

Zie de taalspecifieke-voorbeeldtoepassing die u zoekt een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag.

* [C#](#httptriggercsharp)
* [F#](#httptriggerfsharp)
* [Node.js](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a>Voorbeeld van de HTTP-trigger in C# #
```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

U kunt ook verbinden met een .NET-object in plaats van **HttpRequestMessage**. Dit object wordt gemaakt van de hoofdtekst van de aanvraag kan worden geparseerd als JSON. Op deze manier kan een type worden doorgegeven aan de HTTP-antwoord uitvoer binding en geretourneerd als de antwoordtekst, samen met een statuscode 200.

```csharp
using System.Net;
using System.Threading.Tasks;

public static string Run(CustomObject req, TraceWriter log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public String name {get; set;}
}
}
```

<a name="httptriggerfsharp"></a>
### <a name="http-trigger-sample-in-f"></a>Voorbeeld van de HTTP-trigger in F # #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

U moet een `project.json` bestand met NuGet om te verwijzen naar de `FSharp.Interop.Dynamic` en `Dynamitey` assembly's, als volgt:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Dit NuGet gebruikt voor het ophalen van de afhankelijkheden en verwijst naar deze in uw script.

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a>Voorbeeld van de HTTP-trigger in Node.JS
```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```
     
<a name="hooktriggersample"></a>
## <a name="webhook-samples"></a>Webhook-voorbeelden
Stel dat u hebt de volgende webhook trigger in de `bindings` matrix van function.json:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

Zie de taalspecifieke-voorbeeldtoepassing die u GitHub probleem opmerkingen registreert.

* [C#](#hooktriggercsharp)
* [F#](#hooktriggerfsharp)
* [Node.js](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a>Voorbeeld van de Webhook in C# #
```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

<a name="hooktriggerfsharp"></a>

### <a name="webhook-sample-in-f"></a>Voorbeeld van de Webhook in F # #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

<a name="hooktriggernodejs"></a>

### <a name="webhook-sample-in-nodejs"></a>Voorbeeld van de Webhook in Node.JS
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

