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
ms.openlocfilehash: d88fda62c59d01a3703fdb583e0881aa8478a6cd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050763"
---
# <a name="azure-functions-java-developer-guide"></a>Azure Functions Java-handleiding voor ontwikkelaars

Biedt ondersteuning voor de Azure Functions-runtime [Java SE 8 LTS (zulu8.31.0.2 jre8.0.181 win_x64)](https://repos.azul.com/azure-only/zulu/packages/zulu-8/8u181/). Deze handleiding bevat informatie over de complexiteit van het schrijven van Azure Functions met behulp van Java.

Een Java-functie is een `public` methode, voorzien van de aantekening `@FunctionName`. Deze methode de vermelding voor een Java-functie definieert en moet uniek zijn in een bepaald pakket. 

In dit artikel wordt ervan uitgegaan dat u al hebt gelezen de [referentie voor ontwikkelaars van Azure Functions](functions-reference.md). U moet ook uitvoeren met de Snelstartgids voor Functions voor het maken van uw eerste functie met behulp van [Visual Studio Code](functions-create-first-function-vs-code.md) of [Maven](functions-create-first-java-maven.md).

## <a name="programming-model"></a>Programmeermodel 

De concepten van [triggers en bindingen](functions-triggers-bindings.md) zijn essentieel voor Azure Functions. Triggers start de uitvoering van code. Bindingen kunnen u een manier om gegevens aan doorgeven en als resultaat de gegevens van een functie zonder te hoeven schrijven van aangepaste code voor gegevenstoegang.

## <a name="folder-structure"></a>mapstructuur

Hier volgt de mapstructuur van een Azure Functions Java-project:

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

U kunt een gedeelde [host.json](functions-host-json.md) -bestand naar de functie-app configureren. Elke functie heeft een eigen codebestand (startactiviteit) en de binding-configuratiebestand (function.json).

U kunt meer dan één functie plaatsen in een project. Plaats uw functies in afzonderlijke JAR-bestanden. De `FunctionApp` in de doel-directory is wat wordt geïmplementeerd naar uw functie-app in Azure.

## <a name="triggers-and-annotations"></a>Triggers en aantekeningen

 Functies worden aangeroepen door een trigger, zoals een HTTP-aanvraag, een timer of een update van gegevens. Uw functie heeft nodig om deze trigger en alle andere invoer voor het produceren van een of meer uitvoerwaarden te verwerken.

Gebruik de Java-aantekeningen opgenomen in de [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation) pakket invoer en uitvoer binden aan uw methoden. Zie voor meer informatie de [Java-referentiedocumenten](/java/api/com.microsoft.azure.functions.annotation).

> [!IMPORTANT] 
> Moet u een Azure Storage-account in uw [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) Azure Blob storage, Azure Queue storage of Azure Table storage triggers lokaal uitvoeren.

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

Voor lokale ontwikkeling van Java-functie-apps downloaden en gebruiken van de [Azul Zulu Enterprise voor Azure](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf) Java 8-JDK van [Azul Systems](https://www.azul.com/downloads/azure-only/zulu/). Azure Functions maakt gebruik van de runtime Azul Java 8-JDK wanneer u uw functie-apps implementeren in de cloud.

[Ondersteuning van Azure](https://azure.microsoft.com/support/) voor problemen met de functie JDK en apps is beschikbaar met een [gekwalificeerde ondersteuningsplan](https://azure.microsoft.com/support/plans/).

## <a name="customize-jvm"></a>JVM aanpassen

Functions kunt u de Java virtual machine (JVM) gebruikt voor het uitvoeren van uw Java-functies aanpassen. De [volgende JVM-opties](https://github.com/Azure/azure-functions-java-worker/blob/master/worker.config.json#L7) worden standaard gebruikt:

* `-XX:+TieredCompilation`
* `-XX:TieredStopAtLevel=1`
* `-noverify` 
* `-Djava.net.preferIPv4Stack=true`
* `-jar`

U krijgt u een extra argumenten in een app-instelling met de naam `JAVA_OPTS`. U kunt app-instellingen toevoegen aan uw functie-app in Azure worden geïmplementeerd in Azure portal of Azure CLI.

### <a name="azure-portal"></a>Azure Portal

In de [Azure-portal](https://portal.azure.com), gebruikt u de [toepassingsinstellingen tabblad](functions-how-to-use-azure-function-app-settings.md#settings) om toe te voegen de `JAVA_OPTS` instelling.

### <a name="azure-cli"></a>Azure-CLI

U kunt de [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) -opdracht uit om `JAVA_OPTS`, zoals in het volgende voorbeeld:

```azurecli-interactive
az functionapp config appsettings set --name <APP_NAME> \
--resource-group <RESOURCE_GROUP> \
--settings "JAVA_OPTS=-Djava.awt.headless=true"
```
In dit voorbeeld wordt een ' headless '-modus. Vervang `<APP_NAME>` met de naam van uw functie-app en `<RESOURCE_GROUP> ` met de resourcegroep.

> [!WARNING]  
> In de [verbruiksabonnement](functions-scale.md#consumption-plan), u moet toevoegen de `WEBSITE_USE_PLACEHOLDER` instellen met een waarde van `0`.  
Deze instelling neemt de tijden koude start voor Java-functies.

## <a name="third-party-libraries"></a>Bibliotheken van derden 

Azure Functions ondersteunt het gebruik van bibliotheken van derden. Standaard alle afhankelijkheden die zijn opgegeven in uw project `pom.xml` bestand automatisch worden gebundeld tijdens de [ `mvn package` ](https://github.com/Microsoft/azure-maven-plugins/blob/master/azure-functions-maven-plugin/README.md#azure-functionspackage) doel. Voor bibliotheken niet is opgegeven als afhankelijkheden in de `pom.xml` bestand, plaats deze in een `lib` map in de hoofdmap van de functie. Afhankelijkheden worden geplaatst de `lib` directory worden toegevoegd aan het laadprogramma van de klasse system tijdens runtime.

De `com.microsoft.azure.functions:azure-functions-java-library` afhankelijkheid wordt standaard in het klassepad verstrekt en hoeft niet te worden opgenomen in de `lib` directory. Bovendien [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker) toegevoegd, worden alle afhankelijkheden [hier](https://github.com/Azure/azure-functions-java-worker/wiki/Azure-Java-Functions-Worker-Dependencies) aan het klassepad.

## <a name="data-type-support"></a>Ondersteuning voor gegevenstype

U kunt gewoon oude Java-objecten (pojo's), typen die zijn gedefinieerd in `azure-functions-java-library`, of primitieve typen zoals tekenreeks en geheel getal te binden aan invoer- of uitvoerbindingen.

### <a name="pojos"></a>Pojo 's

Voor het converteren van invoergegevens naar POJO, [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker) maakt gebruik van de [gson](https://github.com/google/gson) bibliotheek. POJO typen gebruikt als invoer voor de functies moet `public`.

### <a name="binary-data"></a>Binaire gegevens

Binden binaire invoer of uitvoer naar `byte[]`, door in te stellen de `dataType` veld in de function.json naar `binary`:

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

Als u verwacht null-waarden dat, gebruikt u `Optional<T>`.

## <a name="bindings"></a>Bindingen

Invoer- en uitvoerbindingen bevatten een verklarende manier om verbinding maken met gegevens vanuit uw code. Een functie verschillende invoer beschikken en uitvoerbindingen.

### <a name="input-binding-example"></a>Voorbeeld van de Invoerbinding

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

U deze functie aanroepen met een HTTP-aanvraag. 
- De nettolading van de HTTP-aanvraag wordt doorgegeven als een `String` voor het argument `inputReq`.
- Een vermelding wordt opgehaald uit Table storage en wordt doorgegeven als `TestInputData` aan het argument `inputData`.

Voor het ontvangen van een batch van de invoer, kunt u binden aan `String[]`, `POJO[]`, `List<String>`, of `List<POJO>`.

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

Deze functie wordt geactiveerd wanneer er sprake is van nieuwe gegevens in de geconfigureerde event hub. Omdat de `cardinality` is ingesteld op `MANY`, de functie een batch van berichten ontvangt van de event hub. `EventData` uit de event hub worden geconverteerd naar `TestEventData` voor de functie wordt uitgevoerd.

### <a name="output-binding-example"></a>Voorbeeld van uitvoer-binding

U kunt een Uitvoerbinding aan de geretourneerde waarde koppelen met behulp van `$return`. 

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

U aanroepen deze functie op een HttpRequest. Deze schrijft meerdere waarden naar Queue storage.

## <a name="httprequestmessage-and-httpresponsemessage"></a>Het HttpRequestMessage en HttpResponseMessage

 Deze worden gedefinieerd in `azure-functions-java-library`. Ze zijn helper typen om te werken met HttpTrigger-functies.

| Gespecialiseerde type      |       Doel        | Normaal gebruik                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    HTTP-trigger     | Methode, koptekst of query's opgehaald |
| `HttpResponseMessage` | HTTP Output Binding | Opgevraagd dan 200   |

## <a name="metadata"></a>Metagegevens

Enkele triggers verzenden [activeren metagegevens](/azure/azure-functions/functions-triggers-bindings) samen met invoergegevens. U kunt aantekening `@BindingName` binden aan het activeren van metagegevens.


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
In het voorgaande voorbeeld wordt de `queryValue` is gebonden aan de queryreeks-parameter `name` in de HTTP-aanvraag-URL `http://{example.host}/api/metadata?name=test`. Hier volgt een voorbeeld, waarin wordt beschreven hoe u verbinding maken met `Id` van metagegevens in de wachtrij trigger.

```java
 @FunctionName("QueueTriggerMetadata")
    public void QueueTriggerMetadata(
        @QueueTrigger(name = "message", queueName = "test-input-java-metadata", connection = "AzureWebJobsStorage") String message,@BindingName("Id") String metadataId,
        @QueueOutput(name = "output", queueName = "test-output-java-metadata", connection = "AzureWebJobsStorage") OutputBinding<TestData> output,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Queue trigger function processed a message: " + message + " with metadaId:" + metadataId );
        TestData testData = new TestData();
        testData.id = metadataId;
        output.setValue(testData);
    }
```

> [!NOTE]
> De naam die is opgegeven in de aantekening moet overeenkomen met de metagegevenseigenschap.

## <a name="execution-context"></a>Context voor uitvoering

`ExecutionContext`, gedefinieerd in de `azure-functions-java-library`, bevat de methoden om te communiceren met de functions-runtime.

### <a name="logger"></a>Logger

Gebruik `getLogger`, die is gedefinieerd in `ExecutionContext`, om te schrijven logboeken van functiecode aan te geven.

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

Dit is het configureren van uw functie-app voor het schrijven van logboekregistratie van toepassingen met behulp van de Azure CLI:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

Aan de uitvoer van de logboekregistratie voor uw functie-app streamen met behulp van de Azure CLI, een nieuwe opdrachtprompt, Bash of Terminal-sessie openen en voer de volgende opdracht:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
De [az webapp log tail](/cli/azure/webapp/log) opdracht bevat opties voor het filteren van uitvoer met behulp van de `--provider` optie. 

Voor het downloaden van de logboekbestanden als een ZIP-bestand met behulp van de Azure CLI, een nieuwe opdrachtprompt, Bash of Terminal-sessie openen en voer de volgende opdracht:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

U moet bestandssysteem logboekregistratie in Azure portal of Azure CLI voor het uitvoeren van deze opdracht hebt ingeschakeld.

## <a name="environment-variables"></a>Omgevingsvariabelen

In de functies, [app-instellingen](https://docs.microsoft.com/azure/azure-functions/functions-app-settings), zoals serviceverbinding tekenreeksen, worden weergegeven als omgevingsvariabelen tijdens de uitvoering. U hebt toegang tot deze instellingen via, `System.getenv("AzureWebJobsStorage")`.

Bijvoorbeeld, u kunt toevoegen [AppSetting](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings), met de naam van de `testAppSetting` en de waarde `testAppSettingValue`:

```java

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        context.getLogger().info("testAppSetting "+ System.getenv("testAppSettingValue"));
        return String.format(req);
    }
}

```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over Azure Functions Java-ontwikkeling:

* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
* Lokale ontwikkeling en foutopsporing met [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md), en [Eclipse](functions-create-maven-eclipse.md)
* [Externe foutopsporing voor Java Azure Functions met Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Maven-invoegtoepassing voor Azure Functions](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md) 
* Functie maken via stroomlijnen de `azure-functions:add` doel, en een staging-map voor te bereiden [ZIP-bestandsimplementatie](deployment-zip-push.md).
