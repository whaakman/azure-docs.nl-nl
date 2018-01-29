---
title: Azure Functions HTTP- en webhook bindingen
description: Het gebruik van HTTP- en webhook triggers en bindingen in de Azure Functions begrijpen.
services: functions
documentationcenter: na
author: mattchenderson
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, gebeurtenis verwerking, webhooks, dynamische Reken-, zonder server-architectuur, HTTP-API, REST
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: mahender
ms.openlocfilehash: 5fe981b96725917b9cf567ded9ff38a8055fdb4d
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="azure-functions-http-and-webhook-bindings"></a>Azure Functions HTTP- en webhook bindingen

Dit artikel wordt uitgelegd hoe u werkt met HTTP-bindingen in de Azure Functions. Azure Functions ondersteunt het HTTP-triggers en bindingen van de uitvoer.

Een HTTP-trigger kan worden aangepast om te reageren op [webhooks](https://en.wikipedia.org/wiki/Webhook). Een trigger webhook accepteert alleen een JSON-nettolading en valideert de JSON. Er zijn speciale versies van de webhook-trigger waarmee het eenvoudiger om af te handelen webhooks van bepaalde providers, zoals GitHub en vertraging.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="trigger"></a>Trigger

De HTTP-trigger kunt u een functie met een HTTP-aanvraag worden aangeroepen. U kunt een HTTP-trigger gebruiken om te bouwen zonder Server API's en reageren op webhooks. 

Standaard wordt een HTTP-trigger op de aanvraag met een HTTP 200 OK-statuscode en een lege hoofdtekst reageert. Configureren voor het wijzigen van het antwoord een [HTTP uitvoer binding](#http-output-binding).

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die zoekt naar een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag.

```cs
[FunctionName("HttpTriggerCSharp")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)]HttpRequestMessage req, 
    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

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

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een trigger-binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie zoekt een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt C# script-code die wordt gebonden aan `HttpRequestMessage`:

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

U kunt verbinden met een aangepast object in plaats van `HttpRequestMessage`. Dit object wordt gemaakt van de hoofdtekst van de aanvraag kan worden geparseerd als JSON. Op deze manier kan een type worden doorgegeven aan de HTTP-antwoord uitvoer binding en geretourneerd als de antwoordtekst, samen met een statuscode 200.

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

### <a name="trigger---f-example"></a>Trigger - F #-voorbeeld

Het volgende voorbeeld ziet u een trigger-binding in een *function.json* bestand en een [F # functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie zoekt een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de F #-code:

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

U moet een `project.json` bestand met NuGet om te verwijzen naar de `FSharp.Interop.Dynamic` en `Dynamitey` assembly's, zoals wordt weergegeven in het volgende voorbeeld:

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

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een trigger-binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie zoekt een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

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
     
## <a name="trigger---webhook-example"></a>Trigger - webhook-voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#webhook---c-example)
* [C# script (.csx)](#webhook---c-script-example)
* [F#](#webhook---f-example)
* [JavaScript](#webhook---javascript-example)

### <a name="webhook---c-example"></a>Webhook - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een HTTP 200 in reactie op een algemene JSON-aanvraag verzendt.

```cs
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

### <a name="webhook---c-script-example"></a>Webhook - voorbeeld van C#-script

Het volgende voorbeeld ziet u een webhook trigger binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie Logboeken GitHub probleem opmerkingen.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de C#-scriptcode:

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

### <a name="webhook---f-example"></a>Webhook - F #-voorbeeld

Het volgende voorbeeld ziet u een webhook trigger binding in een *function.json* bestand en een [F # functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie Logboeken GitHub probleem opmerkingen.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de F #-code:

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

### <a name="webhook---javascript-example"></a>Webhook - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een webhook trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie Logboeken GitHub probleem opmerkingen.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="trigger---attributes"></a>Trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs) kenmerk, gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http).

U kunt de autorisatie niveau en toegestane HTTP-methoden instellen in kenmerk constructorparameters en er zijn eigenschappen van het type en de route sjabloon webhook. Zie voor meer informatie over deze instellingen [Trigger - configuratie](#trigger---configuration). Hier volgt een `HttpTrigger` kenmerk in een handtekening voor methode:

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    ...
}
 ```

Zie voor een compleet voorbeeld [Trigger - C#-voorbeeld](#trigger---c-example).

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `HttpTrigger` kenmerk.


|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
| **type** | n.v.t.| Vereist - moet worden ingesteld op `httpTrigger`. |
| **direction** | n.v.t.| Vereist - moet worden ingesteld op `in`. |
| **naam** | n.v.t.| Vereist: de naam van de variabele in functiecode gebruikt voor de aanvraag of de hoofdtekst van de aanvraag. |
| <a name="http-auth"></a>**authLevel** |  **AuthLevel** |Hiermee bepaalt u wat sleutels, indien van toepassing, aanwezig zijn op de aanvraag moeten om de functie aanroepen. Het machtigingsniveau kan een van de volgende waarden zijn: <ul><li><code>anonymous</code>&mdash;Er is geen API-sleutel is vereist.</li><li><code>function</code>&mdash;Een specifieke functies API-sleutel is vereist. Dit is de standaardwaarde als niets wordt opgegeven.</li><li><code>admin</code>&mdash;De hoofdsleutel is vereist.</li></ul> Zie de sectie voor meer informatie over [autorisatie sleutels](#authorization-keys). |
| **methoden** |**Methoden** | Een matrix met de HTTP-methoden waarop de functie reageert. Als niet wordt opgegeven, wordt de functie reageert op alle HTTP-methoden. Zie [aanpassen van het http-eindpunt](#trigger---customize-the-http-endpoint). |
| **route** | **Route** | Definieert het Routesjabloon aanvragen waarmee uw functie reageert URL's beheren. De standaardwaarde als niets wordt opgegeven is `<functionname>`. Zie voor meer informatie [aanpassen van het http-eindpunt](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** |Hiermee wordt de HTTP-trigger om te fungeren als een [webhook](https://en.wikipedia.org/wiki/Webhook) ontvanger voor de opgegeven provider. Stelt niet de `methods` eigenschap als u deze eigenschap instellen. Het type webhook kan een van de volgende waarden zijn:<ul><li><code>genericJson</code>&mdash;Een eindpunt voor algemene doeleinden webhook zonder logica voor een specifieke provider. Deze instelling voorkomt dat aanvragen met alleen die via HTTP POST en met de `application/json` inhoudstype.</li><li><code>github</code>&mdash;De functie reageert op [GitHub webhooks](https://developer.github.com/webhooks/). Gebruik niet de _authLevel_ eigenschap met de GitHub-webhooks. Zie de sectie van de webhooks GitHub verderop in dit artikel voor meer informatie.</li><li><code>slack</code>&mdash;De functie reageert op [webhooks vertraging](https://api.slack.com/outgoing-webhooks). Gebruik niet de _authLevel_ eigenschap met de toegestane webhooks. Zie de sectie toegestane webhooks verderop in dit artikel voor meer informatie.</li></ul>|

## <a name="trigger---usage"></a>Trigger - usage

Voor C# en F #, kunt u het type van de invoer voor deze trigger declareren `HttpRequestMessage` of een aangepast type. Als u ervoor kiest `HttpRequestMessage`, krijgt u volledige toegang tot het request-object. Voor een aangepast type probeert functies parseren van de JSON-aanvraagtekst om in te stellen van eigenschappen van het object. 

De runtime van Functions biedt voor JavaScript-functies, de aanvraagtekst in plaats van het request-object. Zie voor meer informatie de [JavaScript trigger voorbeeld](#trigger---javascript-example).

### <a name="github-webhooks"></a>GitHub webhooks.

Om te reageren op GitHub webhooks, eerst uw functie te maken met een HTTP-Trigger en stel de **webHookType** eigenschap `github`. Kopieer de URL en API-sleutel in de **webhook toevoegen** pagina van de GitHub-opslagplaats. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

Zie [Een functie maken die wordt geactiveerd door een GitHub-webhook](functions-create-github-webhook-triggered-function.md) voor een voorbeeld.

### <a name="slack-webhooks"></a>Toegestane webhooks.

De toegestane webhook genereert een token voor u in plaats van waarin u kunt opgeven, dus u een sleutel functiespecifieke met het token van de toegestane vertraging configureren moet. Zie [autorisatie sleutels](#authorization-keys).

### <a name="customize-the-http-endpoint"></a>Het HTTP-eindpunt aanpassen

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

```
http://<yourapp>.azurewebsites.net/api/products/electronics/357
```

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

Hier volgt een Node.js-code voor de functie met dezelfde Routeparameters.

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

Standaard worden alle routes van de functie voorafgegaan door *api*. U kunt ook aanpassen of verwijderen van het voorvoegsel met behulp van de `http.routePrefix` eigenschap in uw [host.json](functions-host-json.md) bestand. Het volgende voorbeeld verwijdert u de *api* routeprefix met behulp van een lege tekenreeks voor het voorvoegsel op in de *host.json* bestand.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

### <a name="authorization-keys"></a>Autorisatie-sleutels

HTTP-triggers kunnen u sleutels gebruiken voor extra beveiliging. Een standaard HTTP-trigger kunt gebruiken dat deze als een API-sleutel, vereisen de sleutel moet aanwezig zijn op de aanvraag. Webhooks kunt sleutels gebruiken voor het toestaan van aanvragen in verschillende manieren, afhankelijk van wat de provider ondersteunt.

Sleutels worden opgeslagen als onderdeel van de functie-app in Azure en zijn versleuteld in rust. Als u wilt uw sleutels weergeven, nieuwe te maken of sleutels rouleert met nieuwe waarden, navigeer naar een van uw functies in de portal en selecteer 'Beheren'. 

Er zijn twee soorten sleutels:

- **Hostsleutels**: deze sleutels worden gedeeld door alle functies in de functie-app. Wanneer als een API-sleutel gebruikt, kunnen toegang tot een functie binnen de functie-app.
- **Functietoetsen**: deze sleutels alleen van toepassing op de specifieke functies waarin ze zijn gedefinieerd. Wanneer als een API-sleutel gebruikt, kan deze alleen toegang tot deze functie.

Elke sleutel met de naam voor de verwijzing en er is een standaard-sleutel (met de naam 'standaard') op het niveau van de functie en de host. Functietoetsen hebben voorrang op de hostsleutels. Wanneer twee sleutels zijn gedefinieerd met dezelfde naam, wordt de functietoets altijd gebruikt.

De **hoofdsleutel** is een standaard host-sleutel met de naam '_master' die is gedefinieerd voor elke functie-app. Deze sleutel kan niet worden ingetrokken. Het biedt beheerderstoegang tot de runtime-API's. Met behulp van `"authLevel": "admin"` in de binding JSON is vereist voor deze sleutel moeten worden weergegeven in de aanvraag; een andere toets resulteert in het autorisatiefouten.

> [!IMPORTANT]  
> Vanwege de verhoogde machtigingen verleend door de hoofdsleutel, moet u geen deze sleutel delen met derden of distribueren in systeemeigen clienttoepassingen. Wees voorzichtig bij het kiezen van de beheerder autorisatieniveau.

### <a name="api-key-authorization"></a>API-sleutel autorisatie

Een HTTP-trigger moet standaard een API-sleutel in de HTTP-aanvraag. Dus de HTTP-aanvraag normaal ziet er als volgt:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

De sleutel kan worden opgenomen in een queryreeks-variabele met de naam `code`, zoals hierboven, of kan worden opgenomen in een `x-functions-key` HTTP-header. De waarde van de sleutel mag een gedefinieerd voor de functie functietoets of op een host-toets.

U kunt anonieme aanvragen waarvoor geen sleutels toestaan. U kunt ook vereisen dat de hoofdsleutel worden gebruikt. Wijzigen van het standaardniveau voor autorisatie via de `authLevel` eigenschap in de JSON van de binding. Zie voor meer informatie [Trigger - configuratie](#trigger---configuration).

### <a name="keys-and-webhooks"></a>Sleutels en webhooks.

Webhook autorisatie wordt verwerkt door het onderdeel van de ontvanger webhook, onderdeel van de HTTP-trigger en het mechanisme voor varieert op basis van de webhook-type. Elke mechanisme komt, maar afhankelijk zijn van een sleutel. De functie-sleutel met de naam 'standaard' wordt standaard gebruikt. Configureer de webhook-provider voor het verzenden van de naam van de sleutel met de aanvraag in een van de volgende manieren voor het gebruik van een andere sleutel:

- **Querytekenreeks**: de provider geeft de naam van de sleutel in de `clientid` querytekenreeksparameter, zoals `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
- **Aanvraagheader**: de provider geeft de naam van de sleutel in de `x-functions-clientid` header.

## <a name="trigger---limits"></a>Trigger - limieten

De lengte van de HTTP-aanvraag is beperkt tot 100 kB (102,400) en de URL-lengte is beperkt tot 4 kB (4.096). Deze limieten worden opgegeven door de `httpRuntime` element van de runtime [Web.config-bestand](https://github.com/Azure/azure-webjobs-sdk-script/blob/v1.x/src/WebJobs.Script.WebHost/Web.config).

## <a name="trigger---hostjson-properties"></a>Trigger - eigenschappen host.json

De [host.json](functions-host-json.md) bestand bevat instellingen voor HTTP-trigger gedrag.

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="output"></a>Uitvoer

Gebruik de uitvoer van de HTTP-binding om te reageren op de afzender van HTTP-aanvraag. Deze binding vereist een HTTP-trigger en Hiermee kunt u het antwoord is gekoppeld aan de aanvraag van de trigger aanpassen. Als een HTTP-uitvoer binding is niet opgegeven, wordt een HTTP-trigger retourneert HTTP 200 OK met een lege hoofdtekst. 

## <a name="output---configuration"></a>Output - configuratie

Voor klasse C#-bibliotheken zijn er geen binding van de uitvoer-specifieke configuratie-eigenschappen. Voor het verzenden van een HTTP-antwoord moet de functie retourtype `HttpResponseMessage` of `Task<HttpResponseMessage>`.

Voor andere talen uitvoer een HTTP binding wordt gedefinieerd als een JSON-object in de `bindings` matrix van function.json, zoals wordt weergegeven in het volgende voorbeeld:

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand.

|Eigenschap  |Beschrijving  |
|---------|---------|
| **type** |moet worden ingesteld op `http`. |
| **direction** | moet worden ingesteld op `out`. |
|**naam** | De naam van de variabele in functiecode gebruikt voor het antwoord. |

## <a name="output---usage"></a>Output - gebruik

U kunt de uitvoerparameter gebruiken om te reageren op de aanroeper HTTP- of webhook. U kunt ook de patronen language standaard antwoord gebruiken. Bijvoorbeeld reacties, Zie de [trigger voorbeeld](#trigger---example) en de [webhook voorbeeld](#trigger---webhook-example).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
