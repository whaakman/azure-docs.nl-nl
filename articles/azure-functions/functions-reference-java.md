---
title: Naslaginformatie over Java-ontwikkelaars voor Azure Functions | Microsoft Docs
description: Begrijpen hoe werkt in combinatie met Java ontwikkelen.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Azure functions, functies, gebeurtenisverwerking webhooks, dynamische compute, zonder server architectuur, java
ms.service: functions
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/07/2017
ms.author: routlaw
ms.openlocfilehash: 09a48d61cb27b4db0778295565d167a0688cc99f
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="azure-functions-java-developer-guide"></a>Azure Functions Java-handleiding voor ontwikkelaars
> [!div class="op_single_selector"]
[!INCLUDE [functions-selector-languages](../../includes/functions-selector-languages.md)]

## <a name="programming-model"></a>-Programmeermodel 

Uw Azure-functie moet een stateless klassenmethode die verwerkt door de invoer en uitvoer produceert. Hoewel u Exemplaarmethoden schrijven zijn toegestaan, moet alle exemplaarvelden van de klasse niet uw functie afhankelijk. Alle methoden van de functie moet een `public` aanpassingsfunctie voor toegang.

## <a name="triggers-and-annotations"></a>Triggers en aantekeningen

Meestal wordt een Azure-functie opgeroepen vanwege een externe-trigger. De functie moet worden verwerkt die trigger en de bijbehorende invoer en uitvoer van een of meer produceren.

Java-aantekeningen zijn opgenomen in de `azure-functions-java-core` pakket invoer en uitvoer binden aan uw methoden. Triggers voor ondersteunde invoer en uitvoer aantekeningen binding zijn opgenomen in de volgende tabel:

Binding | Aantekening
---|---
CosmosDB | N/A
HTTP | <ul><li>`HttpTrigger`</li><li>`HttpOutput`</li></ul>
Mobile Apps | N/A
Notification Hubs | N/A
Storage-Blob | <ul><li>`BlobTrigger`</li><li>`BlobInput`</li><li>`BlobOutput`</li></ul>
Opslagwachtrij | <ul><li>`QueueTrigger`</li><li>`QueueOutput`</li></ul>
Table Storage | <ul><li>`TableInput`</li><li>`TableOutput`</li></ul>
Timer | <ul><li>`TimerTrigger`</li></ul>
Twilio | N/A

Trigger in- en uitgangen kunnen ook worden gedefinieerd in de [function.json](/azure/azure-functions/functions-reference#function-code) voor uw toepassing.

> [!IMPORTANT] 
> Moet u een Azure Storage-account in uw [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) Azure Storage-Blob, wachtrij of tabel triggers lokaal uitvoeren.

Voorbeeld met behulp van aantekeningen:

```java
import com.microsoft.azure.serverless.functions.annotation.HttpTrigger;
import com.microsoft.azure.serverless.functions.ExecutionContext;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

Dezelfde functie zonder aantekeningen geschreven:

```java
package com.example;

public class MyClass {
    public static String echo(String in) {
        return in;
    }
}
```

met de bijbehorende `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}

```

## <a name="data-types"></a>Gegevenstypen

U zijn kunt de gegevenstypen in Java gebruiken voor de invoer- en -gegevens, inclusief systeemeigen typen; aangepaste Java-typen en gespecialiseerde Azure-typen gedefinieerd in `azure-functions-java-core` pakket. De Azure Functions runtime probeert converteren de invoer in het type dat is aangevraagd door uw code ontvangen.

### <a name="strings"></a>Tekenreeksen

Waarden die zijn doorgegeven naar methoden van de functie wordt worden geconverteerd naar tekenreeksen als het bijbehorende type invoerparameter voor de functie van het type is `String`. 

### <a name="plain-old-java-objects-pojos"></a>Gewone oude Java-objecten (pojo's)

Tekenreeksen die zijn geformatteerd met JSON wordt naar Java-typen worden geconverteerd als de invoer van de methode van de functie dat Java-type verwacht. Deze conversie kunt u JSON-invoer doorgegeven aan uw functies en met Java-typen in uw code werken zonder de conversie implementeren in uw eigen code.

POJO typen gebruikt als invoer voor functies dezelfde moeten `public` aanpassingsfunctie voor toegang als de functie-methoden worden gebruikt in. U hoeft te declareren POJO klasse velden `public`. Bijvoorbeeld, een JSON-tekenreeks `{ "x": 3 }` kan worden geconverteerd naar het volgende POJO-type:

```Java
public class MyData {
    private int x;
}
```

### <a name="binary-data"></a>Binaire gegevens

Binaire gegevens wordt weergegeven als een `byte[]` in uw Azure functions-code. Binaire in- of uitgangen verbinding maken met uw functies door in te stellen de `dataType` veld in uw function.json naar `binary`:

```json
 {
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "blob",
      "name": "content",
      "direction": "in",
      "dataType": "binary",
      "path": "container/myfile.bin",
      "connection": "ExampleStorageAccount"
    },
  ]
}
```

Vervolgens worden gebruikt in uw functiecode:

```java
// Class definition and imports are omitted here
public static String echoLength(byte[] content) {
}
```

Gebruik `OutputBinding<byte[]>` type om een binding binaire uitvoer.


## <a name="function-method-overloading"></a>Overbelasting van de functie-methode

U mag de functie methoden met dezelfde naam maar met verschillende typen van de overbelasting. Bijvoorbeeld, hebben beide `String echo(String s)` en `String echo(MyType s)` in één klasse en Azure Functions runtime besluit welk account om aan te roepen door het werkelijke invoertype onderzoeken (voor HTTP-invoer, MIME-type `text/plain` leidt tot `String` terwijl `application/json` Hiermee geeft u `MyType`).

## <a name="inputs"></a>Invoer

Invoer zijn onderverdeeld in twee categorieën worden onderverdeeld in Azure Functions: een de trigger-invoer is en de andere is de aanvullende invoer. Hoewel deze verschillen in de `function.json`, het gebruik van Java-code identiek is. Laten we het volgende codefragment als voorbeeld:

```java
package com.example;

import com.microsoft.azure.serverless.functions.annotation.BindingName;
import java.util.Optional;

public class MyClass {
    public static String echo(Optional<String> in, @BindingName("item") MyObject obj) {
        return "Hello, " + in.orElse("Azure") + " and " + obj.getKey() + ".";
    }

    private static class MyObject {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
}
```

De `@BindingName` aantekening accepteert een `String` eigenschap met de naam van de binding/trigger gedefinieerd in `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "put" ],
      "route": "items/{id}"
    },
    {
      "type": "table",
      "name": "item",
      "direction": "in",
      "tableName": "items",
      "partitionKey": "Example",
      "rowKey": "{id}",
      "connection": "ExampleStorageAccount"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}
```

Dus als u deze functie is aangeroepen, de nettolading van de HTTP-aanvraag een optionele doorgegeven `String` voor het argument `in` en een Azure-tabelopslag `MyObject` type doorgegeven naar argument `obj`. Gebruik de `Optional<T>` type voor het afhandelen van invoer in uw functies waarmee kunnen niet null zijn.

## <a name="outputs"></a>Uitvoer

Uitvoer kunnen worden uitgedrukt in retourwaarde of output-parameters. Als er slechts één uitvoer, wordt u aangeraden om de retourwaarde. U hebt voor meerdere uitgangen output-parameters gebruiken.

Geretourneerde waarde is de eenvoudigste vorm van uitvoer, u alleen de waarde van elk type terugkeren en Azure Functions-runtime probeert te marshallen deze terug naar het werkelijke type (zoals een HTTP-antwoord). In `functions.json`, u `$return` als de naam van de uitvoer-binding.

Als u wilt meerdere uitvoerwaarden produceren, gebruiken `OutputBinding<T>` dat is gedefinieerd in de `azure-functions-java-core` pakket. Als u wilt maken van een HTTP-antwoord en toepassen van een bericht naar een wachtrij ook, kunt u ongeveer schrijven:

```java
package com.example;

import com.microsoft.azure.serverless.functions.OutputBinding;
import com.microsoft.azure.serverless.functions.annotation.BindingName;

public class MyClass {
    public static String echo(String body, 
    @QueueOutput(queueName = "messages", connection = "AzureWebJobsStorage", name = "queue") OutputBinding<String> queue) {
        String result = "Hello, " + body + ".";
        queue.setValue(result);
        return result;
    }
}
```

waarvan de uitvoer-binding in moet definiëren `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "queue",
      "name": "queue",
      "direction": "out",
      "queueName": "messages",
      "connection": "AzureWebJobsStorage"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}
```
## <a name="specialized-types"></a>Speciale typen

Soms moet u een functie controle over in- en uitgangen hebt gedetailleerde. Specifieke typen in de `azure-functions-java-core` pakket vindt u aanvraaggegevens bewerken en aanpassen van de status van het resultaat van een HTTP-trigger:

| Speciaal Type      |       Doel        | Typische gebruiksscenario                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    HTTP-Trigger     | Methode, kopteksten of query's ophalen |
| `HttpResponseMessage<T>` | HTTP-uitvoer Binding | Geretourneerde status dan 200   |

> [!NOTE] 
> U kunt ook `@BindingName` aantekening ophalen van HTTP-headers en query's. Bijvoorbeeld: `@BindingName("name") String query` doorloopt de HTTP-aanvraagheaders en query's en wordt die waarde doorgegeven aan de methode. Bijvoorbeeld: `query` worden `"test"` als de aanvraag-URL is `http://example.org/api/echo?name=test`.

### <a name="metadata"></a>Metagegevens

Metagegevens afkomstig is van verschillende bronnen, zoals HTTP-headers, HTTP-query's en [activeren metagegevens](/azure/azure-functions/functions-triggers-bindings#trigger-metadata-properties). Gebruik de `@BindingName` aantekening samen met de naam van de metagegevens van de waarde op te halen.

Bijvoorbeeld, de `queryValue` worden in de volgende code codefragment `"test"` als de aangevraagde URL `http://{example.host}/api/metadata?name=test`.

```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.serverless.functions.annotation.*;

public class MyClass {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```

## <a name="functions-execution-context"></a>Uitvoeringscontext van functies

U kunt werken met Azure Functions uitvoeringsomgeving via de `ExecutionContext` object dat is gedefinieerd de `azure-functions-java-core` pakket. Gebruik de `ExecutionContext` -object om het aanroepen van informatie en functies runtime-gegevens in uw code.

### <a name="logging"></a>Logboekregistratie

Toegang tot het logboek van de runtime functies is beschikbaar via de `ExecutionContext` object. Dit logboek is gekoppeld aan de Azure monitor en kunt u de vlag-waarschuwingen en fouten opgetreden tijdens het uitvoeren van de functie.

De volgende voorbeeldcode bevat, wordt er een waarschuwingsbericht weergegeven registreert als de aanvraagtekst ontvangen leeg is.

```java
import com.microsoft.azure.serverless.functions.annotation.HttpTrigger;
import com.microsoft.azure.serverless.functions.ExecutionContext;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie:

* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
