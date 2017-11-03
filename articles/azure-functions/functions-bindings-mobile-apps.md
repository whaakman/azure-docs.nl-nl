---
title: Azure Functions Mobile Apps-bindingen | Microsoft Docs
description: Het gebruik van de bindingen van Azure Mobile Apps in Azure Functions begrijpen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: faad1263-0fa5-41a9-964f-aecbc0be706a
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/31/2016
ms.author: glenga
ms.openlocfilehash: d2c0e4e233761584bad2df05a8e702e4fc77e84f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-mobile-apps-bindings"></a>Azure Functions Mobile Apps-bindingen
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dit artikel wordt uitgelegd hoe u kunt configureren en code [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bindingen in de Azure Functions. Azure Functions ondersteunt invoer en uitvoer van de bindingen voor mobiele Apps.

De Mobile Apps-invoer en uitvoer bindingen kunnen u [lezen van en schrijven naar gegevenstabellen](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) in uw mobiele app.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a>Invoer Mobile Apps-binding
De invoer Mobile Apps-binding wordt een record van een eindpunt van de mobiele geladen en geeft deze door aan uw functie. In een C# en F # functies, worden wijzigingen in de record automatisch verzonden terug naar de tabel wanneer de functie is afgesloten.

De Mobile Apps-invoer voor een functie maakt gebruik van de volgende JSON-object in de `bindings` matrix van function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "id" : "<Id of the record to retrieve - see below>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "in"
}
```

Houd rekening met het volgende:

* `id`kan niet statisch of het kan zijn gebaseerd op de trigger die de functie activeert. Als u bijvoorbeeld een [wachtrij trigger]() voor de functie vervolgens `"id": "{queueTrigger}"` de tekenreekswaarde van het bericht uit de wachtrij als de record-ID gebruikt om op te halen.
* `connection`moet de naam van een appinstelling in de functie-app die op zijn beurt de URL van uw mobiele app bevat bevatten. De functie gebruikt deze URL om de REST-bewerkingen op basis van uw mobiele app samen te stellen. U [maken van een app-instelling in uw app functie]() die URL van uw mobiele app bevat (ziet eruit als `http://<appname>.azurewebsites.net`), geeft u de naam van de app-instelling in de `connection` eigenschap in de binding van uw invoer. 
* U moet opgeven `apiKey` als u [implementeren van een API-sleutel in uw back-end voor Node.js mobiele app](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), of [implementeren van een API-sleutel in uw back-end .NET mobiele app](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Hiervoor u [maken van een app-instelling in uw app functie]() die de API-sleutel bevat, voegt u de `apiKey` eigenschap in uw invoer binding met de naam van de app-instelling. 
  
  > [!IMPORTANT]
  > Deze API-sleutel moet niet worden gedeeld met uw mobiele app-clients. Deze moet alleen worden gedistribueerd veilig aan servicezijde clients, zoals Azure Functions. 
  > 
  > [!NOTE]
  > Azure Functions slaat uw verbindingsgegevens en API-sleutels als de app-instellingen zodat ze niet zijn ingeschakeld in uw resourcebeheerbibliotheek. Dit beschermt uw vertrouwelijke gegevens.
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a>Invoer-gebruik
Deze sectie wordt beschreven hoe u uw invoer binding Mobile Apps in uw functiecode. 

Wanneer de record met de opgegeven tabel en de record-ID is gevonden, wordt doorgegeven in de benoemde [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parameter (of in Node.js, doorgegeven aan de `context.bindings.<name>` object). Wanneer de record niet gevonden is, wordt de parameter is `null`. 

In C# en F # functies en wijzigingen die u aanbrengt op invoer van de record (invoerparameter) automatisch terug naar de tabel Mobile Apps wordt verzonden wanneer de functie is afgesloten. Gebruik in Node.js-functies, `context.bindings.<name>` voor toegang tot de invoerrecord. U kunt een record in Node.js niet wijzigen.

<a name="inputsample"></a>

## <a name="input-sample"></a>Voorbeeld van invoer
Stel dat u hebt de volgende function.json die een record van de mobiele App tabel met de id van het bericht uit de wachtrij trigger ophaalt:

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

Zie het voorbeeld taalspecifieke die gebruikmaakt van de invoerrecord van de binding. De C# en F # voorbeelden te wijzigen van de record `text` eigenschap.

* [C#](#inputcsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>Invoer voorbeeld in C# #

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

<!--
<a name="inputfsharp"></a>
### Input sample in F# ## 

```fsharp
#r "Newtonsoft.Json"    
open Newtonsoft.Json.Linq
let Run(myQueueItem: string, record: JObject) =
  inputDocument?text <- "This has changed."
```
-->

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>Invoer voorbeeld in Node.js

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a>Mobile Apps uitvoer binding
De Mobile Apps-uitvoer binden aan een nieuwe record schrijven naar een eindpunt van de Mobile Apps gebruiken.  

De Mobile Apps uitvoer voor een functie maakt gebruik van de volgende JSON-object in de `bindings` matrix van function.json:

```json
{
    "name": "<Name of output parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "out"
}
```

Houd rekening met het volgende:

* `connection`moet de naam van een appinstelling in de functie-app die op zijn beurt de URL van uw mobiele app bevat bevatten. De functie gebruikt deze URL om de REST-bewerkingen op basis van uw mobiele app samen te stellen. U [maken van een app-instelling in uw app functie]() die URL van uw mobiele app bevat (ziet eruit als `http://<appname>.azurewebsites.net`), geeft u de naam van de app-instelling in de `connection` eigenschap in de binding van uw invoer. 
* U moet opgeven `apiKey` als u [implementeren van een API-sleutel in uw back-end voor Node.js mobiele app](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), of [implementeren van een API-sleutel in uw back-end .NET mobiele app](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Hiervoor u [maken van een app-instelling in uw app functie]() die de API-sleutel bevat, voegt u de `apiKey` eigenschap in uw invoer binding met de naam van de app-instelling. 
  
  > [!IMPORTANT]
  > Deze API-sleutel moet niet worden gedeeld met uw mobiele app-clients. Deze moet alleen worden gedistribueerd veilig aan servicezijde clients, zoals Azure Functions. 
  > 
  > [!NOTE]
  > Azure Functions slaat uw verbindingsgegevens en API-sleutels als de app-instellingen zodat ze niet zijn ingeschakeld in uw resourcebeheerbibliotheek. Dit beschermt uw vertrouwelijke gegevens.
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a>Gebruik voor uitvoer
Deze sectie wordt beschreven hoe u de binding in uw functiecode Mobile Apps-uitvoer. 

Gebruik in C# functies, een benoemde output-parameter van het type `out object` de uitvoer-record te openen. Gebruik in Node.js-functies, `context.bindings.<name>` de uitvoer-record te openen.

<a name="outputsample"></a>

## <a name="output-sample"></a>Voorbeeld van uitvoer
Stel dat u hebt de volgende function.json die de trigger van een wachtrij en de uitvoer van een Mobile Apps definieert:

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

Zie de taalspecifieke-voorbeeldtoepassing die u een record in het eindpunt van de tabel Mobile Apps met de inhoud van het bericht uit de wachtrij maakt.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Voorbeeld van uitvoer in C# #

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

<!--
<a name="outfsharp"></a>
### Output sample in F# ## 
```fsharp

```
-->
<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Voorbeeld van uitvoer in Node.js

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

