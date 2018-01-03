---
title: Mobile Apps-bindingen voor Azure Functions
description: Het gebruik van de bindingen van Azure Mobile Apps in Azure Functions begrijpen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: a1e4f15747031ba75ba5ae589557750919a71853
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="mobile-apps-bindings-for-azure-functions"></a>Mobile Apps-bindingen voor Azure Functions 

Dit artikel wordt uitgelegd hoe u werkt met [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bindingen in de Azure Functions. Azure Functions ondersteunt invoer en uitvoer van de bindingen voor mobiele Apps.

De Mobile Apps-bindingen kunnen u lezen en bijwerken van gegevenstabellen in mobiele apps.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="input"></a>Invoer

De invoer Mobile Apps-binding wordt een record van een eindpunt van de mobiele geladen en geeft deze door aan uw functie. In C# en F # functies, worden wijzigingen in de record automatisch verzonden terug naar de tabel wanneer de functie is afgesloten.

## <a name="input---example"></a>Invoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#input---c-script-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-script-example"></a>Invoer - voorbeeld van C#-script

Het volgende voorbeeld ziet u een invoer Mobile Apps-binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie wordt geactiveerd door een wachtrijbericht die een record-id heeft. De functie de opgegeven record leest en Hiermee wijzigt u de `Text` eigenschap.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
],
"disabled": false
}
```
De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de C#-scriptcode:

```cs
#r "Newtonsoft.Json"    
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, JObject record)
{
    if (record != null)
    {
        record["Text"] = "This has changed.";
    }    
}
```

### <a name="input---javascript"></a>Invoer - JavaScript

Het volgende voorbeeld ziet u een invoer Mobile Apps-binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie wordt geactiveerd door een wachtrijbericht die een record-id heeft. De functie de opgegeven record leest en Hiermee wijzigt u de `Text` eigenschap.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
],
"disabled": false
}
```
De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

## <a name="input---attributes"></a>Invoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [MobileTable](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.MobileApps](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps).

Zie voor meer informatie over de kenmerkeigenschappen die u kunt configureren, [de volgende configuratiesectie](#input---configuration).

## <a name="input---configuration"></a>Invoer - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `MobileTable` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
| **type**|| Moet worden ingesteld op 'mobileTable'|
| **richting**||Moet worden ingesteld op 'in'|
| **naam**|| De naam van de invoerparameter in functiehandtekening.|
|**tableName** |**TableName**|Naam van de tabel met de mobiele app|
| **ID**| **ID** | De id van de record om op te halen. Kan niet statisch of op basis van de trigger die de functie activeert. Bijvoorbeeld, als u een wachtrij-trigger voor de functie vervolgens `"id": "{queueTrigger}"` de tekenreekswaarde van het bericht uit de wachtrij als de record-ID gebruikt om op te halen.|
|**verbinding**|**Verbinding**|De naam van een app-instelling met de mobiele app-URL. De functie gebruikt deze URL om de REST-bewerkingen op basis van uw mobiele app samen te stellen. Maken van een app-instelling in uw app in de functie die de mobiele app-URL bevat, geeft u vervolgens de naam van de app-instelling in de `connection` eigenschap in de binding van uw invoer. De URL ziet eruit als `http://<appname>.azurewebsites.net`.
|**apiKey**|**ApiKey**|De naam van een appinstelling met uw mobiele app API-sleutel. Geef de API-sleutel als u [implementeren van een API-sleutel in uw mobiele app Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), of [een API-sleutel in uw mobiele app voor .NET implementeren](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Als u de sleutel, het maken van een app-instelling in uw app in de functie die de API-sleutel bevat, en voeg vervolgens de `apiKey` eigenschap in uw invoer binding met de naam van de app-instelling. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> De API-sleutel niet worden delen met uw mobiele app-clients. Deze moet alleen worden gedistribueerd veilig aan servicezijde clients, zoals Azure Functions. Azure Functions slaat uw verbindingsgegevens en API-sleutels als de app-instellingen zodat ze niet zijn ingeschakeld in uw resourcebeheerbibliotheek. Dit beschermt uw vertrouwelijke gegevens.

## <a name="input---usage"></a>Invoer - gebruik

In C#-functies, wanneer de record met de opgegeven ID is gevonden, wordt doorgegeven in de benoemde [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parameter. Wanneer de record niet gevonden is, de parameterwaarde is `null`. 

In de JavaScript-functies, de record is doorgegeven aan de `context.bindings.<name>` object. Wanneer de record niet gevonden is, de parameterwaarde is `null`. 

In C# en F # functies en wijzigingen die u aanbrengt op invoer van de record (invoerparameter) automatisch terug naar de tabel worden verzonden wanneer de functie is afgesloten. U kunt een record in JavaScript-functies niet wijzigen.

## <a name="output"></a>Uitvoer

De Mobile Apps-uitvoer binden aan een nieuwe record schrijven naar een tabel met Mobile Apps gebruiken.  

## <a name="output---example"></a>Output - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Output - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die wordt veroorzaakt door een wachtrijbericht en maakt u een record in de tabel van een mobiele app.

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
    TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

### <a name="output---c-script-example"></a>Output - voorbeeld van C#-script

Het volgende voorbeeld ziet u de uitvoer van een Mobile Apps binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie wordt geactiveerd door een wachtrijbericht en een nieuw record gemaakt met de vastgelegde waarde voor de `Text` eigenschap.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
],
"disabled": false
}
```

De [configuratie](#output---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de C#-scriptcode:

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

### <a name="output---javascript-example"></a>Output - JavaScript-voorbeeld

Het volgende voorbeeld ziet u de uitvoer van een Mobile Apps binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie wordt geactiveerd door een wachtrijbericht en een nieuw record gemaakt met de vastgelegde waarde voor de `Text` eigenschap.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
],
"disabled": false
}
```

De [configuratie](#output---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="output---attributes"></a>Output - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [MobileTable](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.MobileApps](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps).

Zie voor meer informatie over de kenmerkeigenschappen die u kunt configureren, [Output - configuratie](#output---configuration). Hier volgt een `MobileTable` kenmerk voorbeeld in een handtekening voor methode:

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
    TraceWriter log)
{
    ...
}
```

Zie voor een compleet voorbeeld [uitvoer - C#-voorbeeld](#output---c-example).

## <a name="output---configuration"></a>Output - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `MobileTable` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
| **type**|| Moet worden ingesteld op 'mobileTable'|
| **richting**||Moet worden ingesteld op 'out'|
| **naam**|| Naam van de uitvoerparameter bij functiehandtekening.|
|**tableName** |**TableName**|Naam van de tabel met de mobiele app|
|**verbinding**|**MobileAppUriSetting**|De naam van een app-instelling met de mobiele app-URL. De functie gebruikt deze URL om de REST-bewerkingen op basis van uw mobiele app samen te stellen. Maken van een app-instelling in uw app in de functie die de mobiele app-URL bevat, geeft u vervolgens de naam van de app-instelling in de `connection` eigenschap in de binding van uw invoer. De URL ziet eruit als `http://<appname>.azurewebsites.net`.
|**apiKey**|**ApiKeySetting**|De naam van een appinstelling met uw mobiele app API-sleutel. Geef de API-sleutel als u [implementeren van een API-sleutel in uw back-end voor Node.js mobiele app](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), of [implementeren van een API-sleutel in uw back-end .NET mobiele app](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Als u de sleutel, het maken van een app-instelling in uw app in de functie die de API-sleutel bevat, en voeg vervolgens de `apiKey` eigenschap in uw invoer binding met de naam van de app-instelling. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> De API-sleutel niet worden delen met uw mobiele app-clients. Deze moet alleen worden gedistribueerd veilig aan servicezijde clients, zoals Azure Functions. Azure Functions slaat uw verbindingsgegevens en API-sleutels als de app-instellingen zodat ze niet zijn ingeschakeld in uw resourcebeheerbibliotheek. Dit beschermt uw vertrouwelijke gegevens.

## <a name="output---usage"></a>Output - gebruik

Gebruik in C# scriptfuncties, een benoemde output-parameter van het type `out object` de uitvoer-record te openen. In C# klassenbibliotheken de `MobileTable` kenmerk kan worden gebruikt met een van de volgende typen:

* `ICollector<T>`of `IAsyncCollector<T>`, waarbij `T` is `JObject` of een type met een `public string Id` eigenschap.
* `out JObject`
* `out T`of `out T[]`, waarbij `T` is een Type met een `public string Id` eigenschap.

Gebruik in Node.js-functies, `context.bindings.<name>` de uitvoer-record te openen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
