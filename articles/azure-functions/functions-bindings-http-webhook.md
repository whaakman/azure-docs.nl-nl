---
title: Azure Functions-HTTP- en webhook-bindingen
description: Over het gebruik van HTTP- en webhook-triggers en bindingen in Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: Azure functions, functies, gebeurtenissen verwerken, webhooks, dynamisch berekenen, serverloze architectuur, HTTP-API, REST
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: 5f6538c69139b8cd254b44cb9875e18a14c8fa8b
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39344144"
---
# <a name="azure-functions-http-and-webhook-bindings"></a>Azure Functions-HTTP- en webhook-bindingen

In dit artikel wordt uitgelegd hoe u werkt met Azure Functions-HTTP-bindingen. Azure Functions ondersteunt HTTP-triggers en uitvoerbindingen.

Een HTTP-trigger kan worden aangepast om te reageren op [webhooks](https://en.wikipedia.org/wiki/Webhook). Een webhook-trigger accepteert alleen een JSON-nettolading en valideert de JSON. Er zijn speciale versies van de webhook-trigger die het eenvoudiger om af te handelen webhooks van bepaalde providers, zoals GitHub en Slack.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De HTTP-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet-pakket versie 1.x. Broncode voor het pakket is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.Http) GitHub-opslagplaats.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De HTTP-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet-pakket versie 3.x. Broncode voor het pakket is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/) GitHub-opslagplaats.

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Trigger

De HTTP-trigger kunt u een functie met een HTTP-aanvraag aanroepen. U kunt een HTTP-trigger gebruiken om te bouwen serverloze API's en webhooks reageren. 

Standaard retourneert een HTTP-trigger HTTP 200 OK met een lege hoofdtekst in functies 1.x of HTTP 204 geen inhoud met een lege hoofdtekst in functies 2.x. Configureren voor het wijzigen van het antwoord een [HTTP-Uitvoerbinding](#http-output-binding).

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die zoekt naar een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag. U ziet dat de geretourneerde waarde wordt gebruikt voor de Uitvoerbinding, maar een kenmerk van de geretourneerde waarde is niet vereist.

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

Het volgende voorbeeld ziet u de binding van een trigger in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie zoekt naar een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag.

Hier volgt de *function.json* bestand:

```json
{
    "disabled": false,
    "bindings": [
        {
            "authLevel": "function",
            "name": "req",
            "type": "httpTrigger",
            "direction": "in",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "name": "$return",
            "type": "http",
            "direction": "out"
        }
    ]
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt C#-script-code die wordt gebonden aan `HttpRequestMessage`:

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

U kunt koppelen aan een aangepast object in plaats van `HttpRequestMessage`. Dit object wordt gemaakt van de hoofdtekst van de aanvraag, geparseerd als JSON. Op deze manier kan een type worden doorgegeven aan het antwoord van de HTTP-output-binding en geretourneerd als de antwoordtekst, samen met een 200-statuscode.

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
```

### <a name="trigger---f-example"></a>Trigger - F #-voorbeeld

Het volgende voorbeeld ziet u de binding van een trigger in een *function.json* bestand en een [F #-functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie zoekt naar een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

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

U moet een `project.json` bestand dat gebruikmaakt van NuGet om te verwijzen naar de `FSharp.Interop.Dynamic` en `Dynamitey` assembly's, zoals wordt weergegeven in het volgende voorbeeld:

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

Het volgende voorbeeld ziet u de binding van een trigger in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie zoekt naar een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag.

Hier volgt de *function.json* bestand:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status defaults to 200 */
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

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een HTTP-200 in reactie op een generieke JSON-aanvraag verzendt.

```cs
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

### <a name="webhook---c-script-example"></a>Webhook - C#-scriptvoorbeeld

Het volgende voorbeeld ziet u een webhook-trigger binding in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie registreert opmerkingen van GitHub-probleem.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "github",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

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

Het volgende voorbeeld ziet u een webhook-trigger binding in een *function.json* bestand en een [F #-functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie registreert opmerkingen van GitHub-probleem.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "github",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

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

Het volgende voorbeeld ziet u een webhook-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie registreert opmerkingen van GitHub-probleem.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "github",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="trigger---attributes"></a>Trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs) kenmerk.

U kunt de autorisatie niveau en toegestane HTTP-methoden instellen in kenmerk constructor parameters en er zijn eigenschappen van het type en de route sjabloon webhook. Zie voor meer informatie over deze instellingen [Trigger - configuratie](#trigger---configuration). Hier volgt een `HttpTrigger` kenmerk in een handtekeningmethode:

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    ...
}
 ```

Zie voor een compleet voorbeeld [Trigger - voorbeeld met C#](#trigger---c-example).

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `HttpTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
| **type** | N.v.t.| Vereist: moet worden ingesteld op `httpTrigger`. |
| **direction** | N.v.t.| Vereist: moet worden ingesteld op `in`. |
| **De naam** | N.v.t.| Vereist: de naam van de variabele die wordt gebruikt in de functiecode voor de aanvraag of de hoofdtekst van de aanvraag. |
| <a name="http-auth"></a>**authLevel** |  **AuthLevel** |Hiermee bepaalt u wat sleutels, indien van toepassing, aanwezig zijn op de aanvraag moeten om de functie aanroepen. Het autorisatieniveau kan een van de volgende waarden zijn: <ul><li><code>anonymous</code>&mdash;Er zijn geen API-sleutel is vereist.</li><li><code>function</code>&mdash;Een functie-specifieke API-sleutel is vereist. Dit is de standaardwaarde als niets wordt opgegeven.</li><li><code>admin</code>&mdash;De hoofdsleutel is vereist.</li></ul> Zie de sectie voor meer informatie over [sleutels voor de verificatieregel](#authorization-keys). |
| **Methoden** |**Methoden** | Een matrix met de HTTP-methoden waarop de functie reageert. Indien niet opgegeven, wordt de functie reageert op alle HTTP-methoden. Zie [aanpassen van het http-eindpunt](#customize-the-http-endpoint). |
| **route** | **route** | Hiermee definieert u de Routesjabloon beheren waarop aanvraag-URL's die uw functie reageert. De standaardwaarde als niets wordt opgegeven is `<functionname>`. Zie voor meer informatie, [aanpassen van het http-eindpunt](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** |Hiermee wordt de HTTP-trigger om te fungeren als een [webhook](https://en.wikipedia.org/wiki/Webhook) ontvanger voor de opgegeven provider. Stel de `methods` eigenschap als u deze eigenschap is ingesteld. Het webhooktype kan een van de volgende waarden zijn:<ul><li><code>genericJson</code>&mdash;Een algemene webhook-eindpunt zonder logica voor een specifieke provider. Deze instelling aanvragen beperkt tot alleen die via HTTP POST en met de `application/json` type inhoud.</li><li><code>github</code>&mdash;De functie reageert op [GitHub webhooks](https://developer.github.com/webhooks/). Gebruik niet de _authLevel_ eigenschap met GitHub webhooks. Zie de sectie GitHub webhooks verderop in dit artikel voor meer informatie.</li><li><code>slack</code>&mdash;De functie reageert op [webhooks Slack](https://api.slack.com/outgoing-webhooks). Gebruik niet de _authLevel_ eigenschap met de Slack webhooks. Zie de sectie Slack webhooks verderop in dit artikel voor meer informatie.</li></ul>|

## <a name="trigger---usage"></a>Trigger - gebruik

Voor C# en F #-functies, kunt u het type van de invoer voor een trigger declareren `HttpRequestMessage` of een aangepast type. Als u ervoor kiest `HttpRequestMessage`, krijgt u volledige toegang tot het request-object. Voor een type aangepaste functies probeert parseren van de hoofdtekst van de JSON-aanvraag voor het instellen van eigenschappen van het object. 

De Functions-runtime biedt voor JavaScript-functies, de hoofdtekst van de aanvraag in plaats van het request-object. Zie voor meer informatie de [JavaScript trigger voorbeeld](#trigger---javascript-example).

### <a name="github-webhooks"></a>GitHub webhooks

Om te reageren op GitHub webhooks, eerst uw functie maken met een HTTP-Trigger en stel de **webHookType** eigenschap `github`. Kopieer de URL en API-sleutel in de **webhook toevoegen** pagina van uw GitHub-opslagplaats. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

Zie [Een functie maken die wordt geactiveerd door een GitHub-webhook](functions-create-github-webhook-triggered-function.md) voor een voorbeeld.

### <a name="slack-webhooks"></a>Slack-webhooks

De Slack-webhook genereert een token voor u in plaats van waarin u kunt opgeven, dus u een sleutel functiespecifieke met het token van Slack configureren moet. Zie [sleutels voor de verificatieregel](#authorization-keys).

### <a name="customize-the-http-endpoint"></a>Aanpassen van het HTTP-eindpunt

Wanneer u een functie voor een HTTP-trigger of een WebHook, maakt is de functie standaard beschikbaar met een route van het formulier:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

U kunt deze route met de optionele `route` eigenschap van de HTTP-trigger invoer van de binding. Als u bijvoorbeeld de volgende *function.json* -bestand definieert een `route` eigenschap voor een HTTP-trigger:

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

Met behulp van deze configuratie de functie is nu beschikbaar met de volgende route in plaats van de oorspronkelijke route.

```
http://<yourapp>.azurewebsites.net/api/products/electronics/357
```

Hiermee wordt de functiecode voor de ondersteuning van twee parameters in het adres _categorie_ en _id_. U kunt een [Web API Route beperking](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) met de parameters. De volgende C#-functiecode maakt gebruik van beide parameters.

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

Hier volgt een Node.js-functiecode die gebruikmaakt van de Routeparameters die.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;

    if (!id) {
        context.res = {
            // status defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status defaults to 200 */
            body: category + " item with id = " + id + " was requested."
        };
    }

    context.done();
} 
```

Standaard alle functie-routes worden voorafgegaan door *api*. U kunt ook aanpassen of verwijderen van het voorvoegsel via de `http.routePrefix` eigenschap in uw [host.json](functions-host-json.md) bestand. Het volgende voorbeeld verwijdert u de *api* route voorvoegsel met behulp van een lege tekenreeks voor het voorvoegsel in de *host.json* bestand.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

### <a name="authorization-keys"></a>Sleutels voor de verificatieregel

HTTP-triggers kunnen u sleutels gebruiken voor extra beveiliging. Een standaard HTTP-trigger kunt gebruiken als een API-sleutel, die de sleutel moet aanwezig zijn op de aanvraag. Webhooks kunt sleutels gebruiken voor het toestaan van aanvragen in verschillende manieren, afhankelijk van wat de provider ondersteunt.

> [!NOTE]
> Als functies lokaal uitvoert, de autorisatie is uitgeschakeld ongeacht de `authLevel` instellen in `function.json`. Als u op Azure Functions publiceren, de `authLevel` onmiddellijk van kracht.

Sleutels worden opgeslagen als onderdeel van uw functie-app in Azure en in rust worden versleuteld. Als u uw sleutels, nieuwe te maken of sleutels Ga naar de nieuwe waarden, gaat u naar een van uw functies in de portal en selecteer 'Beheren'. 

Er zijn twee soorten sleutels:

- **Hostsleutels**: deze sleutels worden gedeeld door alle functies in de functie-app. Wanneer gebruikt als een API-sleutel, geven deze toegang tot een functie binnen de functie-app.
- **Functietoetsen**: deze sleutels alleen van toepassing op de specifieke functies waarin ze zijn gedefinieerd. Wanneer gebruikt als een API-sleutel, geven alleen toegang tot die functie.

Elke sleutel is met de naam van verwijzing en er is een standaard-sleutel (met de naam 'standaard') op het niveau van de functie en de host. Functietoetsen hebben voorrang op de hostsleutels. Wanneer er twee sleutels zijn gedefinieerd met dezelfde naam, wordt de sleutel van de functie altijd gebruikt.

De **hoofdsleutel** is een standaard host-sleutel met de naam '_master' die is gedefinieerd voor elke functie-app. Deze sleutel kan niet worden ingetrokken. Het biedt beheerderstoegang tot de runtime-API's. Met behulp van `"authLevel": "admin"` in de binding JSON is deze sleutel kan worden weergegeven in de aanvraag; vereist een andere toets naar Autorisatiefout leidt.

> [!IMPORTANT]  
> Vanwege de verhoogde machtigingen verleend door de hoofdsleutel, moet u niet met derden delen van deze sleutel of deze in native clienttoepassingen te distribueren. Wees voorzichtig bij het kiezen van het autorisatieniveau van de beheerder.

### <a name="api-key-authorization"></a>Autorisatiebeleid API

Een HTTP-trigger moet standaard een API-sleutel in de HTTP-aanvraag. Dus lijkt de HTTP-aanvraag normaal op het volgende:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

De sleutel kan worden opgenomen in een queryreeks-variabele met de naam `code`, zoals hierboven, of kunnen worden opgenomen in een `x-functions-key` HTTP-header. De waarde van de sleutel kan zijn een functie-sleutel gedefinieerd voor de functie of een willekeurige toets host.

U kunt anonieme aanvragen, waarvoor geen sleutels. U kunt ook vereisen dat de hoofdsleutel worden gebruikt. U het standaardniveau voor autorisatie wijzigen met behulp van de `authLevel` eigenschap in de JSON van de binding. Zie voor meer informatie, [Trigger - configuratie](#trigger---configuration).

### <a name="keys-and-webhooks"></a>Sleutels en webhooks

Autorisatie van de Webhook wordt verwerkt door het onderdeel van de ontvanger webhook, onderdeel van de HTTP-trigger, en het mechanisme is afhankelijk van het webhooktype. Elke mechanisme heeft, maar afhankelijk zijn van een sleutel. De functie-sleutel met de naam 'standaard' wordt standaard gebruikt. Voor het gebruik van een andere productcode, de webhook-provider voor het verzenden van de naam van de sleutel met de aanvraag in een van de volgende manieren te configureren:

- **Querytekenreeks**: de provider geeft de naam van de sleutel in de `clientid` query-tekenreeksparameter, zoals `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
- **Aanvraagkoptekst**: de provider geeft de naam van de sleutel in de `x-functions-clientid` header.

## <a name="trigger---limits"></a>Trigger - limieten

De lengte van de HTTP-aanvraag is beperkt tot 100MB (104,857,600 bytes) en de URL-lengte is beperkt tot 4KB (4096 bytes). Deze limieten zijn opgegeven door de `httpRuntime` element van de runtime van [Web.config-bestand](https://github.com/Azure/azure-webjobs-sdk-script/blob/v1.x/src/WebJobs.Script.WebHost/Web.config).

Als een functie die wordt gebruikt niet de HTTP-trigger binnen ongeveer 2,5 minuten, de time-out van de gateway wordt voltooid en retourneert een HTTP 502-fout. De functie blijft actief, maar kan niet naar een HTTP-antwoord retourneren. Voor langlopende functies, wordt aangeraden dat u gaat u als volgt de asynchrone patronen en retourneert een locatie waar u de status van de aanvraag kunt pingen. Zie voor informatie over hoe lang een functie kunt uitvoeren, [schaal en hosting - verbruiksabonnement](functions-scale.md#consumption-plan). 

## <a name="trigger---hostjson-properties"></a>Trigger - eigenschappen voor host.json

De [host.json](functions-host-json.md) bestand bevat instellingen die het HTTP-trigger gedrag beheren.

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="output"></a>Uitvoer

Gebruik de HTTP-Uitvoerbinding om te reageren op de afzender van HTTP-aanvraag. Deze binding vereist een HTTP-trigger en Hiermee kunt u het antwoord dat is gekoppeld aan de aanvraag van de trigger aanpassen. Als een HTTP-Uitvoerbinding is niet opgegeven, wordt een HTTP-trigger HTTP 200 OK geretourneerd met een lege hoofdtekst in functies 1.x of HTTP 204 geen inhoud met een lege hoofdtekst in functies 2.x.

## <a name="output---configuration"></a>Uitvoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand. Voor C#-klasse bibliotheken er zijn geen kenmerkeigenschappen die met deze overeenkomen *function.json* eigenschappen. 

|Eigenschap  |Beschrijving  |
|---------|---------|
| **type** |Moet worden ingesteld op `http`. |
| **direction** | Moet worden ingesteld op `out`. |
|**De naam** | De naam van de variabele in functiecode gebruikt voor het antwoord of `$return` de geretourneerde waarde gebruiken. |

## <a name="output---usage"></a>Uitvoer - gebruik

Gebruik de taal-standaard antwoord patronen voor het verzenden van een HTTP-antwoord. In C# of C#-script, moet u de functie retourtype `HttpResponseMessage` of `Task<HttpResponseMessage>`. In C#, is een kenmerk van de geretourneerde waarde niet vereist.

Bijvoorbeeld antwoorden, Zie de [trigger voorbeeld](#trigger---example) en de [webhook voorbeeld](#trigger---webhook-example).

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)
