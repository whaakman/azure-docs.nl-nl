---
title: Naslaginformatie over Java-ontwikkelaars voor Azure Functions | Microsoft Docs
description: Lees hoe u voor het ontwikkelen van functies met behulp van Java.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Azure functions, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server, java
ms.service: azure-functions
ms.devlang: java
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: routlaw
ms.openlocfilehash: e4d96fa558e1122ef9e0fe0b265166757c45e678
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53321047"
---
# <a name="azure-functions-java-developer-guide"></a>Azure Functions Java-handleiding voor ontwikkelaars

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

## <a name="programming-model"></a>Programmeermodel 

De concepten van [triggers en bindingen](functions-triggers-bindings.md) zijn essentieel voor Azure Functions. Triggers start de uitvoering van code. Bindingen kunnen u een manier om gegevens aan doorgeven en als resultaat de gegevens van een functie zonder te hoeven schrijven van aangepaste code voor gegevenstoegang.

Een functie moet een stateless methode voor het verwerken van invoer en uitvoer produceren. De functie moet niet afhankelijk van de exemplaarvelden van een van de klasse. Alle methoden voor de functie moet `public` en de methode met aantekening @FunctionName moet uniek zijn als de naam van methode de vermelding voor een functie definieert.

## <a name="folder-structure"></a>mapstructuur

Hier volgt de mapstructuur van een Azure-functie Java-project:

```
FunctionsProject
 | - src
 | | - main
 | | | - java
 | | | | - FunctionApp
 | | | | | - MyFirstFunction.java
 | | | | | - MySecondFunction.java
 | - target
 | | - azure-functions
 | | | - FunctionApp
 | | | | - FunctionApp.jar
 | | | | - host.json
 | | | | - MyFirstFunction
 | | | | | - function.json
 | | | | - MySecondFunction
 | | | | | - function.json
 | | | | - bin
 | | | | - lib
 | - pom.xml
```

Er is een gedeelde [host.json](functions-host-json.md) -bestand dat kan worden gebruikt voor het configureren van de functie-app. Elke functie heeft een eigen codebestand (startactiviteit) en de binding-configuratiebestand (function.json).

U kunt meer dan één functie plaatsen in een project. Plaats uw functies in afzonderlijke JAR-bestanden. De FunctionApp in de doelmap is wat wordt geïmplementeerd naar uw functie-app in Azure.

## <a name="triggers-and-annotations"></a>Triggers en aantekeningen

 Azure functions worden aangeroepen door een trigger, zoals een HTTP-aanvraag, een timer of een update van gegevens. Uw functie heeft nodig om deze trigger en alle andere invoer voor het produceren van een of meer uitvoerwaarden te verwerken.

Gebruik de Java-aantekeningen opgenomen in de [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation) pakket invoer en uitvoer binden aan uw methoden. Zie voor meer informatie [Java-referentiedocumenten](/java/api/com.microsoft.azure.functions.annotation).

> [!IMPORTANT] 
> Moet u een Azure Storage-account in uw [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) Azure Storage-Blob, wachtrij of tabel triggers lokaal uitvoeren.

Voorbeeld:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

Dit is de gegenereerde bijbehorende `function.json` door de [azure-functions-maven-invoegtoepassing](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-maven-plugin):

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.Function.echo",
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

## <a name="jdk-runtime-availability-and-support"></a>JDK runtime-beschikbaarheid en ondersteuning 

Download en gebruik de [Azul Zulu Enterprise voor Azure](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf) Java 8-JDK van [Azul Systems](https://www.azul.com/downloads/azure-only/zulu/) voor lokale ontwikkeling van Java-functie-apps. Azure Functions maakt gebruik van de runtime Azul Java 8-JDK wanneer u uw functie-apps implementeren in de cloud.

[Ondersteuning van Azure](https://azure.microsoft.com/en-us/support/) voor problemen met de functie JDK en apps is beschikbaar met een [gekwalificeerde ondersteuningsplan](https://azure.microsoft.com/support/plans/).

## <a name="third-party-libraries"></a>Bibliotheken van derden 

Azure Functions ondersteunt het gebruik van bibliotheken van derden. Standaard alle afhankelijkheden die zijn opgegeven in uw project `pom.xml` bestand zal automatisch worden gebundeld tijdens de [ `mvn package` ](https://github.com/Microsoft/azure-maven-plugins/blob/master/azure-functions-maven-plugin/README.md#azure-functionspackage) doel. Voor bibliotheken niet is opgegeven als afhankelijkheden in de `pom.xml` bestand, plaats deze in een `lib` map in de hoofdmap van de functie. Afhankelijkheden worden geplaatst de `lib` directory worden toegevoegd aan het laadprogramma van de klasse system tijdens runtime.

De `com.microsoft.azure.functions:azure-functions-java-library` afhankelijkheid wordt standaard in het klassepad verstrekt en hoeft niet te worden opgenomen in de `lib` directory. Ook afhankelijkheden vermeld [hier](https://github.com/Azure/azure-functions-java-worker/wiki/Azure-Java-Functions-Worker-Dependencies) worden toegevoegd aan het klassepad door [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker).

## <a name="data-type-support"></a>Ondersteuning voor gegevenstype

U kunt gewoon oude Java-objecten (pojo's), typen die zijn gedefinieerd in `azure-functions-java-library` of primitieve gegevenstypen zoals tekenreeks, geheel getal en binden aan invoer/uitvoer-bindingen.

### <a name="plain-old-java-objects-pojos"></a>Plain oude Java-objecten (pojo's)

Voor het converteren van invoergegevens naar POJO, [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker) maakt gebruik van [gson](https://github.com/google/gson) bibliotheek. POJO typen gebruikt als invoer voor de functies moet `public`.

### <a name="binary-data"></a>Binaire gegevens

Binden binaire invoer of uitvoer naar `byte[]` door in te stellen de `dataType` veld in de function.json naar `binary`:

```java
   @FunctionName("BlobTrigger")
    @StorageAccount("AzureWebJobsStorage")
     public void blobTrigger(
        @BlobTrigger(name = "content", path = "myblob/{fileName}", dataType = "binary") byte[] content,
        @BindingName("fileName") String fileName,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Blob trigger function processed a blob.\n Name: " + fileName + "\n Size: " + content.length + " Bytes");
    }
```

Gebruik `Optional<T>` voor als null-waarden worden verwacht

## <a name="bindings"></a>Bindingen

Invoer- en uitvoerbindingen bevatten een verklarende manier om verbinding maken met gegevens vanuit uw code. Een functie verschillende invoer beschikken en uitvoerbindingen.

### <a name="example-input-binding"></a>Voorbeeld van de Invoerbinding

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String inputReq,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") TestInputData inputData
        @TableOutput(name = "myOutputTable", tableName = "Person", connection = "AzureWebJobsStorage") OutputBinding<Person> testOutputData,
    ) {
        testOutputData.setValue(new Person(httpbody + "Partition", httpbody + "Row", httpbody + "Name"));
        return "Hello, " + inputReq + " and " + inputData.getKey() + ".";
    }

    public static class TestInputData {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
    public static class Person {
        public String PartitionKey;
        public String RowKey;
        public String Name;

        public Person(String p, String r, String n) {
            this.PartitionKey = p;
            this.RowKey = r;
            this.Name = n;
        }
    }
}
```

Deze functie is aangeroepen met een HTTP-aanvraag. 
- De nettolading van de HTTP-aanvraag wordt doorgegeven als een `String` voor het argument `inputReq`
- Een vermelding wordt opgehaald uit de Azure-tabelopslag en wordt doorgegeven als `TestInputData` aan het argument `inputData`.

Voor het ontvangen van een batch van de invoer, kunt u binden aan `String[]`, `POJO[]`, `List<String>` of `List<POJO>`.

```java
@FunctionName("ProcessIotMessages")
    public void processIotMessages(
        @EventHubTrigger(name = "message", eventHubName = "%AzureWebJobsEventHubPath%", connection = "AzureWebJobsEventHubSender", cardinality = Cardinality.MANY) List<TestEventData> messages,
        final ExecutionContext context)
    {
        context.getLogger().info("Java Event Hub trigger received messages. Batch size: " + messages.size());
    }
    
    public class TestEventData {
    public String id;
}

```

Deze functie wordt geactiveerd wanneer er sprake is van nieuwe gegevens in de geconfigureerde event hub. Als de `cardinality` is ingesteld op `MANY`, functie ontvangt u een batch van berichten van event hub. EventData van event hub worden geconverteerd naar `TestEventData` voor de functie wordt uitgevoerd.

### <a name="example-output-binding"></a>Voorbeeld van de Uitvoerbinding

U kunt een Uitvoerbinding voor het gebruik van de geretourneerde waarde binden `$return` 

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Als er meerdere uitvoerbindingen, gebruikt u de geretourneerde waarde voor slechts één provider.

Gebruiken voor het verzenden van meerdere uitvoerwaarden `OutputBinding<T>` gedefinieerd in de `azure-functions-java-library` pakket. 

```java
@FunctionName("QueueOutputPOJOList")
    public HttpResponseMessage QueueOutputPOJOList(@HttpTrigger(name = "req", methods = { HttpMethod.GET,
            HttpMethod.POST }, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            @QueueOutput(name = "itemsOut", queueName = "test-output-java-pojo", connection = "AzureWebJobsStorage") OutputBinding<List<TestData>> itemsOut, 
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");
       
        String query = request.getQueryParameters().get("queueMessageId");
        String queueMessageId = request.getBody().orElse(query);
        itemsOut.setValue(new ArrayList<TestData>());
        if (queueMessageId != null) {
            TestData testData1 = new TestData();
            testData1.id = "msg1"+queueMessageId;
            TestData testData2 = new TestData();
            testData2.id = "msg2"+queueMessageId;

            itemsOut.getValue().add(testData1);
            itemsOut.getValue().add(testData2);

            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + queueMessageId).build();
        } else {
            return request.createResponseBuilder(HttpStatus.INTERNAL_SERVER_ERROR)
                    .body("Did not find expected items in CosmosDB input list").build();
        }
    }

     public static class TestData {
        public String id;
    }
```

Boven de functie wordt aangeroepen op een HttpRequest en schrijft u meerdere waarden naar de Azure-wachtrij

## <a name="httprequestmessage-and-httpresponsemessage"></a>Het HttpRequestMessage en HttpResponseMessage

 Het HttpRequestMessage en HttpResponseMessage typen zijn gedefinieerd in `azure-functions-java-library` helper typen om te werken met HttpTrigger-functies

| Gespecialiseerde Type      |       Doel        | Normaal gebruik                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    HTTP-trigger     | Methode, koptekst of query's ophalen |
| `HttpResponseMessage` | HTTP-Uitvoerbinding | Status van het resultaat dan 200   |

## <a name="metadata"></a>Metagegevens

Enkele triggers verzenden [activeren metagegevens](/azure/azure-functions/functions-triggers-bindings#trigger-metadata-properties) samen met invoergegevens. U kunt aantekening `@BindingName` binden aan het activeren van metagegevens


```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.functions.annotation.*;


public class Function {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```
In het bovenstaande voorbeeld de `queryValue` is gebonden aan de queryreeks-parameter `name` in de HTTP-aanvraag-URL `http://{example.host}/api/metadata?name=test`. Hieronder volgt een binding met een ander voorbeeld `Id` van metagegevens in de wachtrij trigger

```java
 @FunctionName("QueueTriggerMetadata")
    public void QueueTriggerMetadata(
        @QueueTrigger(name = "message", queueName = "test-input-java-metadata", connection = "AzureWebJobsStorage") String message,@BindingName("Id") String metadataId,
        @QueueOutput(name = "output", queueName = "test-output-java-metadata", connection = "AzureWebJobsStorage") OutputBinding<TestData> output,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Queue trigger function processed a message: " + message + " whith metadaId:" + metadataId );
        TestData testData = new TestData();
        testData.id = metadataId;
        output.setValue(testData);
    }
```

> [!NOTE]
> Naam in de aantekening moet overeenkomen met de eigenschap metadata

## <a name="execution-context"></a>Context voor uitvoering

`ExecutionContext` gedefinieerd in de `azure-functions-java-library` bevat methoden om te communiceren met de functions-runtime.

### <a name="logger"></a>Logger

Gebruik `getLogger` gedefinieerd in `ExecutionContext` om te schrijven logboeken van functiecode aan te geven.

Voorbeeld:

```java

import com.microsoft.azure.functions.*;
import com.microsoft.azure.functions.annotation.*;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="view-logs-and-trace"></a>Logboeken weergeven en tracering

U kunt de Azure CLI voor stream Java stdout en stderr-logboekregistratie, evenals de logboekregistratie van andere toepassingen. 

Uw functie toepassing configureren voor het schrijven van logboekregistratie van toepassingen met de Azure CLI:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

Om te streamen van uitvoer van de logboekregistratie voor uw functie-app met de Azure CLI, een nieuwe opdrachtprompt, Bash of Terminal-sessie openen en voer de volgende opdracht:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
De [az webapp log tail](/cli/azure/webapp/log) opdracht bevat opties voor het filteren van uitvoer met behulp van de `--provider` optie. 

De logboekbestanden downloaden als een ZIP-bestand met de Azure CLI, een nieuwe opdrachtprompt, Bash of Terminal-sessie openen en voer de volgende opdracht:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

U moet bestandssysteem logboekregistratie in de Azure Portal of Azure CLI voor het uitvoeren van deze opdracht hebt ingeschakeld.

## <a name="environment-variables"></a>Omgevingsvariabelen

In de functies, [app-instellingen](https://docs.microsoft.com/azure/azure-functions/functions-app-settings), zoals serviceverbinding tekenreeksen, worden weergegeven als omgevingsvariabelen tijdens de uitvoering. U hebt toegang tot deze instellingen gebruikt, `System.getenv("AzureWebJobsStorage")`

Voorbeeld:

Voeg [AppSetting](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings) met de naam testAppSetting en de waarde testAppSettingValue

```java

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        context.getLogger().info("testAppSetting "+ System.getenv("testAppSettingValue"));
        return String.format(req);
    }
}

```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over het ontwikkelen van Java voor Azure-functie:

* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
- Lokale ontwikkeling en foutopsporing met [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md), en [Eclipse](functions-create-maven-eclipse.md). 
* [Externe foutopsporing voor Java Azure Functions met Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Maven-invoegtoepassing voor Azure Functions](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md) -functie maken via stroomlijnen de `azure-functions:add` doel en een staging-map voor te bereiden [ZIP-bestandsimplementatie](deployment-zip-push.md).
