---
title: Naslaginformatie over Java-ontwikkelaars voor Azure Functions | Microsoft Docs
description: Lees hoe u voor het ontwikkelen van functies met behulp van Java.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Azure functions, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server, java
ms.service: functions
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/07/2017
ms.author: routlaw
ms.openlocfilehash: 65964372cf2a0aa42be967f7c93749c58a9f56dd
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621766"
---
# <a name="azure-functions-java-developer-guide"></a>Azure Functions Java-handleiding voor ontwikkelaars

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

## <a name="programming-model"></a>Programmeermodel 

Uw Azure-functie moet een stateless klassenmethode waarmee invoer worden verwerkt en geeft een resultaat. Hoewel u het schrijven van Exemplaarmethoden zijn toegestaan, moet de exemplaarvelden van een van de klasse niet de functie afhankelijk. Alle methoden van de functie moet een `public` toegang modifier.

## <a name="triggers-and-annotations"></a>Triggers en aantekeningen

Normaal gesproken wordt een Azure-functie aangeroepen vanwege een externe trigger. De functie moet verwerken deze trigger en de bijbehorende invoer en uitvoer van een of meer produceren.

Java-aantekeningen zijn opgenomen in de `azure-functions-java-core` pakket invoer en uitvoer binden aan uw methoden. De ondersteunde invoer triggers en aantekeningen-Uitvoerbinding zijn opgenomen in de volgende tabel:

Binding | Aantekening
---|---
CosmosDB | N/A
HTTP | <ul><li>`HttpTrigger`</li><li>`HttpOutput`</li></ul>
Mobile Apps | N/A
Notification Hubs | N/A
Storage Blob | <ul><li>`BlobTrigger`</li><li>`BlobInput`</li><li>`BlobOutput`</li></ul>
Opslagwachtrij | <ul><li>`QueueTrigger`</li><li>`QueueOutput`</li></ul>
Storage-tabel | <ul><li>`TableInput`</li><li>`TableOutput`</li></ul>
Timer | <ul><li>`TimerTrigger`</li></ul>
Twilio | N/A

Triggerinvoer en uitvoer kunnen ook worden gedefinieerd de [function.json](/azure/azure-functions/functions-reference#function-code) voor uw toepassing.

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

U zijn kunt de gegevenstypen in Java gebruiken voor de invoer- en -gegevens, met inbegrip van systeemeigen typen; aangepaste Java-typen en gespecialiseerde Azure typen die zijn gedefinieerd `azure-functions-java-core` pakket. De Azure Functions runtime probeert converteren de invoer die is ontvangen in het type dat is aangevraagd door uw code.

### <a name="strings"></a>Tekenreeksen

Waarden doorgegeven aan functie methoden zal worden geconverteerd naar tekenreeksen als het bijbehorende type invoerparameter voor de functie van het type is `String`. 

### <a name="plain-old-java-objects-pojos"></a>Plain oude Java-objecten (pojo's)

Tekenreeksen die zijn geformatteerd met JSON wordt naar Java-typen worden geconverteerd als de invoer van de functie-methode wordt verwacht dat Java-type. Deze conversie een hoeveelheid kunt u het doorgeven van JSON-invoer in uw functies en werken met Java-typen in uw code zonder dat de conversie implementeren in uw eigen code.

POJO typen gebruikt als invoer voor de functies dezelfde moeten `public` modifier toegang als de functie-methoden worden gebruikt in. U hoeft te declareren POJO klasse velden `public`. Bijvoorbeeld, een JSON-tekenreeks `{ "x": 3 }` kan worden geconverteerd naar de volgende POJO-type:

```Java
public class MyData {
    private int x;
}
```

### <a name="binary-data"></a>Binaire gegevens

Binaire gegevens worden weergegeven als een `byte[]` in uw Azure functions-code. Binaire invoer of uitvoer verbinden aan uw functions door in te stellen de `dataType` veld in de function.json naar `binary`:

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

Vervolgens worden gebruikt in uw functiecode aan te geven:

```java
// Class definition and imports are omitted here
public static String echoLength(byte[] content) {
}
```

Gebruik `OutputBinding<byte[]>` type om te maken van een binaire Uitvoerbinding.


## <a name="function-method-overloading"></a>Overbelasting van de functie-methode

U mag functie methoden met dezelfde naam maar met verschillende typen overbelasten. Bijvoorbeeld, u kunt hebben beide `String echo(String s)` en `String echo(MyType s)` in één klasse en Azure Functions runtime besluit welke regel moet worden aangeroepen door de werkelijke invoertype controleren (voor HTTP-invoer, MIME-type `text/plain` leidt tot `String` terwijl `application/json` Hiermee geeft u `MyType`).

## <a name="inputs"></a>Invoer

Invoer zijn onderverdeeld in twee categorieën in Azure Functions: de invoer voor de werkstroomtrigger is en de andere is de aanvullende invoer. Hoewel ze in verschillende zijn `function.json`, de syntaxis is vrijwel identiek in Java-code. We houden het volgende codefragment als voorbeeld:

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

Dus wanneer deze functie is aangeroepen, de nettolading van de HTTP-aanvraag wordt doorgegeven een optionele `String` voor het argument `in` en een Azure-tabelopslag `MyObject` type doorgegeven aan argument `obj`. Gebruik de `Optional<T>` type om af te handelen invoer in uw functies die kunnen niet null zijn.

## <a name="outputs"></a>Uitvoer

Uitvoer kunnen worden uitgedrukt in de geretourneerde waarde of output-parameters. Als er slechts één uitvoer, u kunt het beste de geretourneerde waarde gebruiken. Voor meerdere uitvoer die u moet output-parameters gebruiken.

Geretourneerde waarde is de eenvoudigste vorm van uitvoer, u alleen de waarde van elk type geretourneerd en Azure Functions-runtime wordt geprobeerd het marshallen terug naar het werkelijke type (zoals een HTTP-antwoord). In `functions.json`, gebruikt u `$return` als de naam van de Uitvoerbinding.

Meerdere uitvoerwaarden produceren, gebruiken `OutputBinding<T>` dat is gedefinieerd in de `azure-functions-java-core` pakket. Als u wilt maken van een HTTP-antwoord en een bericht naar een wachtrij ook gepusht, kunt u er ongeveer als schrijven:

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

welke moet definiëren de Uitvoerbinding in `function.json`:

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

Soms moet u een functie controle over de invoer en uitvoer hebt gedetailleerde. Speciale typen in de `azure-functions-java-core` pakket zijn al voor u aanvraaggegevens bewerken en aanpassen van de status van het resultaat van een HTTP-trigger:

| Gespecialiseerde Type      |       Doel        | Normaal gebruik                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    HTTP-Trigger     | Methode, koptekst of query's ophalen |
| `HttpResponseMessage<T>` | HTTP-Uitvoerbinding | Status van het resultaat dan 200   |

> [!NOTE] 
> U kunt ook `@BindingName` aantekening om HTTP-headers en query's te verkrijgen. Bijvoorbeeld, `@BindingName("name") String query` gegevensbrontabellen loopt, de HTTP-aanvraagheaders en query's en die waarde doorgegeven aan de methode. Bijvoorbeeld, `query` worden `"test"` als de aanvraag-URL is `http://example.org/api/echo?name=test`.

### <a name="metadata"></a>Metagegevens

Metagegevens is afkomstig uit verschillende bronnen, zoals HTTP-headers, HTTP-query's, en [activeren metagegevens](/azure/azure-functions/functions-triggers-bindings#trigger-metadata-properties). Gebruik de `@BindingName` aantekening samen met de naam van de metagegevens van de waarde op te halen.

Bijvoorbeeld, de `queryValue` in de volgende code codefragment worden `"test"` als de aangevraagde URL `http://{example.host}/api/metadata?name=test`.

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

## <a name="functions-execution-context"></a>Uitvoeringscontext functies

U communiceert met Azure Functions-uitvoeringsomgeving via de `ExecutionContext` object dat is gedefinieerd de `azure-functions-java-core` pakket. Gebruik de `ExecutionContext` object aanroep informatie en functions runtime-gegevens gebruiken in uw code.

### <a name="logging"></a>Logboekregistratie

Toegang tot de logger Functions-runtime is beschikbaar via de `ExecutionContext` object. In dit logboek is gekoppeld aan de Azure monitor en kunt u met de vlag-waarschuwingen en fouten aangetroffen tijdens het uitvoeren van de functie.

De volgende voorbeeldcode, wordt er een waarschuwingsbericht weergegeven registreert als de hoofdtekst van de aanvraag ontvangen leeg is.

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

## <a name="environment-variables"></a>Omgevingsvariabelen

Vaak is het wenselijk om op te halen van geheime gegevens uit de broncode voor opmaaktalen wordt om beveiligingsredenen. Hiermee wordt de code worden gepubliceerd naar bron codeopslag zonder dat u per ongeluk referenties met andere ontwikkelaars. Dit kan worden bereikt door gebruik van omgevingsvariabelen, zowel bij het uitvoeren van Azure Functions lokaal en bij het implementeren van uw functies naar Azure.

Gemakkelijk omgevingsvariabelen instellen bij het uitvoeren van Azure Functions lokaal, kunt u besluiten deze variabelen toevoegen aan het bestand local.settings.json. Indien deze nog niet aanwezig in de hoofdmap van uw functieproject, gerust een te maken. Hier ziet u hoe het bestand eruit moet zien:

```xml
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "AzureWebJobsDashboard": ""
  }
}
```

Elke sleutel / waarde van de toewijzing van in de `values` kaart wordt beschikbaar gesteld tijdens runtime als een omgevingsvariabele die toegankelijk is door het aanroepen van `System.getenv("<keyname>")`, bijvoorbeeld `System.getenv("AzureWebJobsStorage")`. Toevoegen van extra sleutel / waarde-paren is geaccepteerd en aanbevolen praktijk.

> [!NOTE]
> Als deze benadering wordt overgenomen, worden zorg ervoor dat rekening houden met het toevoegen van de local.settings.json bestand naar uw opslagplaats negeren-bestand, zodat deze niet is doorgevoerd.

Met de code nu, afhankelijk van deze omgevingsvariabelen, kunt u aanmelden bij de Azure-Portal voor het instellen van dezelfde sleutel / waarde-paren in de instellingen van uw functie-app, zodat uw code oftewel werkt wanneer getest lokaal en wanneer geïmplementeerd op Azure.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie:

* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
* [Externe foutopsporing voor Java Azure Functions met Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
