---
title: Mobile Apps-bindingen voor Azure Functions
description: Over het gebruik van Azure Mobile Apps-bindingen in Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: Azure functions, functies, gebeurtenisverwerking, dynamische Computing, serverloze architectuur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: 67adec7f30c8e4b24d0726ebdefa613fcefa7d3e
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38602514"
---
# <a name="mobile-apps-bindings-for-azure-functions"></a>Mobile Apps-bindingen voor Azure Functions 

Dit artikel wordt uitgelegd hoe u werkt met [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bindingen in Azure Functions. Azure Functions ondersteunt invoer- en uitvoerbindingen voor Mobile Apps.

De Mobile Apps-bindingen kunnen u lezen en bijwerken van gegevenstabellen in mobiele apps.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

Mobile Apps-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.MobileApps](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps) NuGet-pakket versie 1.x. Broncode voor het pakket is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.MobileApps/) GitHub-opslagplaats.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

Mobile Apps-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.MobileApps](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps) NuGet-pakket versie 3.x. Broncode voor het pakket is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="input"></a>Invoer

De Mobile Apps-Invoerbinding wordt een record van een tabeleindpunt mobiele geladen en geeft deze door aan uw functie. In C# en F #-functies, worden wijzigingen in de record automatisch verzonden terug naar de tabel wanneer de functie is afgesloten.

## <a name="input---example"></a>Invoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# script (.csx)](#input---c-script-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-script-example"></a>Invoer - voorbeeld van C#-script

Het volgende voorbeeld ziet u een Invoerbinding van Mobile Apps in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie wordt geactiveerd door een wachtrijbericht waarvoor een record-id. De functie leest de opgegeven record en wijzigt de `Text` eigenschap.

Hier volgt de binding-gegevens de *function.json* bestand:

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
De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

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

Het volgende voorbeeld ziet u een Invoerbinding van Mobile Apps in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie wordt geactiveerd door een wachtrijbericht waarvoor een record-id. De functie leest de opgegeven record en wijzigt de `Text` eigenschap.

Hier volgt de binding-gegevens de *function.json* bestand:

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
De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

## <a name="input---attributes"></a>Invoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [MobileTable](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) kenmerk.

Zie voor meer informatie over de kenmerkeigenschappen die u kunt configureren, [de volgende configuratiesectie](#input---configuration).

## <a name="input---configuration"></a>Invoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `MobileTable` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
| **type**|| Moet worden ingesteld op "mobileTable"|
| **direction**||Moet worden ingesteld op 'in'|
| **De naam**|| De naam van de invoerparameter in de functiehandtekening.|
|**Tabelnaam** |**Tabelnaam**|Naam van de mobiele app gegevenstabel|
| **id**| **Id** | De id van de record om op te halen. Kan zijn statisch of op basis van de trigger die de functie activeert. Bijvoorbeeld, als u een wachtrijtrigger vervolgens gebruiken voor uw functie `"id": "{queueTrigger}"` maakt gebruik van de tekenreekswaarde van bericht uit de wachtrij als de record-ID om op te halen.|
|**verbinding**|**verbinding**|De naam van een appinstelling met de URL van de mobiele app. De functie maakt gebruik van deze URL om de vereiste REST-bewerkingen op uw mobiele app samen te stellen. Maken van een app-instelling in uw functie-app die van de mobiele app-URL bevat, geeft u vervolgens de naam van de app-instelling in de `connection` eigenschap in uw Invoerbinding. De URL lijkt `http://<appname>.azurewebsites.net`.
|**apiKey**|**ApiKey**|De naam van een appinstelling met de API-sleutel van uw mobiele app. Geef de API-sleutel als u [implementeren van een API-sleutel in uw mobiele app voor Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), of [een API-sleutel in uw mobiele .NET-app implementeren](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Maken van een app-instelling in uw functie-app met de API-sleutel voor de sleutel, en vervolgens toevoegen de `apiKey` eigenschap in uw Invoerbinding met de naam van de app-instelling. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> Niet de API-sleutel niet delen met uw mobiele app-clients. Het moet alleen worden gedistribueerd veilig aan servicezijde-clients, zoals Azure Functions. Azure Functions wordt uw verbindingsgegevens en API-sleutels als app-instellingen opgeslagen zodat ze niet worden gecontroleerd in uw resourcebeheerbibliotheek. Dit beschermt uw vertrouwelijke gegevens.

## <a name="input---usage"></a>Invoer - gebruik

In C#-functies, wanneer de record met de opgegeven ID is gevonden, wordt doorgegeven in de benoemde [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parameter. Wanneer de record niet wordt gevonden, de parameterwaarde is `null`. 

In JavaScript-functies, de record is doorgegeven aan de `context.bindings.<name>` object. Wanneer de record niet wordt gevonden, de parameterwaarde is `null`. 

In C# en F #-functies, wijzigingen die u aanbrengt aan de invoerpoort record (invoerparameter) automatisch terug naar de tabel worden verzonden wanneer de functie is afgesloten. U kunt een record in JavaScript-functies niet wijzigen.

## <a name="output"></a>Uitvoer

Gebruik de Mobile Apps-Uitvoerbinding schrijven van een nieuwe record in een tabel met Mobile Apps.  

## <a name="output---example"></a>Uitvoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Uitvoer - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die wordt geactiveerd door een wachtrijbericht en maakt u een record in een tabel van de mobiele app.

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

### <a name="output---c-script-example"></a>Uitvoer - voorbeeld van C#-script

Het volgende voorbeeld ziet u een Uitvoerbinding voor Mobile Apps in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie wordt geactiveerd door een wachtrijbericht en maakt u een nieuwe record met vastgelegde waarde voor de `Text` eigenschap.

Hier volgt de binding-gegevens de *function.json* bestand:

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

De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de C#-scriptcode:

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

### <a name="output---javascript-example"></a>Uitvoer - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een Uitvoerbinding voor Mobile Apps in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie wordt geactiveerd door een wachtrijbericht en maakt u een nieuwe record met vastgelegde waarde voor de `Text` eigenschap.

Hier volgt de binding-gegevens de *function.json* bestand:

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

De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="output---attributes"></a>Uitvoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [MobileTable](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) kenmerk.

Zie voor meer informatie over de kenmerkeigenschappen die u kunt configureren, [uitvoer - configuratie](#output---configuration). Hier volgt een `MobileTable` kenmerk voorbeeld in een handtekeningmethode:

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

Zie voor een compleet voorbeeld [uitvoer - voorbeeld met C#](#output---c-example).

## <a name="output---configuration"></a>Uitvoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `MobileTable` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
| **type**|| Moet worden ingesteld op "mobileTable"|
| **direction**||Moet worden ingesteld op 'out'|
| **De naam**|| De naam van de uitvoerparameter bij functiehandtekening.|
|**Tabelnaam** |**Tabelnaam**|Naam van de mobiele app gegevenstabel|
|**verbinding**|**MobileAppUriSetting**|De naam van een appinstelling met de URL van de mobiele app. De functie maakt gebruik van deze URL om de vereiste REST-bewerkingen op uw mobiele app samen te stellen. Maken van een app-instelling in uw functie-app die van de mobiele app-URL bevat, geeft u vervolgens de naam van de app-instelling in de `connection` eigenschap in uw Invoerbinding. De URL lijkt `http://<appname>.azurewebsites.net`.
|**apiKey**|**ApiKeySetting**|De naam van een appinstelling met de API-sleutel van uw mobiele app. Geef de API-sleutel als u [implementeren van een API-sleutel in uw back-end mobiele app Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), of [implementeren van een API-sleutel in uw mobiele app .NET back-end](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Maken van een app-instelling in uw functie-app met de API-sleutel voor de sleutel, en vervolgens toevoegen de `apiKey` eigenschap in uw Invoerbinding met de naam van de app-instelling. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> Niet de API-sleutel niet delen met uw mobiele app-clients. Het moet alleen worden gedistribueerd veilig aan servicezijde-clients, zoals Azure Functions. Azure Functions wordt uw verbindingsgegevens en API-sleutels als app-instellingen opgeslagen zodat ze niet worden gecontroleerd in uw resourcebeheerbibliotheek. Dit beschermt uw vertrouwelijke gegevens.

## <a name="output---usage"></a>Uitvoer - gebruik

In C#-script-functies, gebruikt u een benoemde output-parameter van het type `out object` voor toegang tot de uitvoer-record. In C#-klassebibliotheken, de `MobileTable` kenmerk kan worden gebruikt met een van de volgende typen:

* `ICollector<T>` of `IAsyncCollector<T>`, waarbij `T` is `JObject` of een type met een `public string Id` eigenschap.
* `out JObject`
* `out T` of `out T[]`, waarbij `T` is een Type met een `public string Id` eigenschap.

Gebruik in Node.js-functies, `context.bindings.<name>` voor toegang tot de uitvoer-record.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)
