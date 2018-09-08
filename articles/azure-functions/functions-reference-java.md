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
ms.date: 08/10/2018
ms.author: routlaw
ms.openlocfilehash: f0dc471e8875ad0d738fce10421c3586752148b9
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092306"
---
# <a name="azure-functions-java-developer-guide"></a>Azure Functions Java-handleiding voor ontwikkelaars

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

## <a name="programming-model"></a>Programmeermodel 

Uw Azure-functie moet een stateless klassenmethode waarmee invoer worden verwerkt en geeft een resultaat. Hoewel u instantiemethoden schrijven kunt, moet de exemplaarvelden van een van de klasse niet de functie afhankelijk. Alle methoden van de functie moet een `public` toegang modifier.

U kunt meer dan één functie plaatsen in een project. Plaats uw functies in afzonderlijke JAR-bestanden.

## <a name="triggers-and-annotations"></a>Triggers en aantekeningen

 Azure functions worden aangeroepen door een trigger, zoals een HTTP-aanvraag, een timer of een update van gegevens. Uw functie heeft nodig om deze trigger en alle andere invoer voor het produceren van een of meer uitvoerwaarden te verwerken.

Gebruik de Java-aantekeningen opgenomen in de [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation) pakket invoer en uitvoer binden aan uw methoden. De aantekeningen met voorbeeldcode is beschikbaar in de [Java-referentiedocumenten](/java/api/com.microsoft.azure.functions.annotation) voor elke aantekening en in de binding verwijzing documentatie voor Azure Functions, zoals die voor [HTTP-triggers](/azure/azure-functions/functions-bindings-http-webhook).

Triggerinvoer en uitvoer kunnen ook worden gedefinieerd de [function.json](/azure/azure-functions/functions-reference#function-code) voor uw functie in plaats van via aantekeningen. Met behulp van `function.json` in plaats van aantekeningen op deze manier wordt niet aanbevolen.

> [!IMPORTANT] 
> Moet u een Azure Storage-account in uw [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) Azure Storage-Blob, wachtrij of tabel triggers lokaal uitvoeren.

Voorbeeld met behulp van aantekeningen:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
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

## <a name="third-party-libraries"></a>Bibliotheken van derden 

Azure Functions ondersteunt het gebruik van bibliotheken van derden. Standaard alle afhankelijkheden die zijn opgegeven in uw project `pom.xml` bestand zal automatisch worden gebundeld tijdens de `mvn package` doel. Voor bibliotheken niet is opgegeven als afhankelijkheden in de `pom.xml` bestand, plaats deze in een `lib` map in de hoofdmap van de functie. Afhankelijkheden worden geplaatst de `lib` directory worden toegevoegd aan het laadprogramma van de klasse system tijdens runtime.

## <a name="data-type-support"></a>Ondersteuning voor gegevenstype

U kunt alle gegevenstypen die in Java gebruiken voor de invoer- en -gegevens, inclusief systeemeigen typen; aangepaste Java-typen en gespecialiseerde Azure typen die zijn gedefinieerd `azure-functions-java-library` pakket. De Azure Functions runtime probeert converteren de invoer die is ontvangen in het type dat is aangevraagd door uw code.

### <a name="strings"></a>Tekenreeksen

Waarden doorgegeven aan functie methoden zal worden geconverteerd naar tekenreeksen als het bijbehorende type invoerparameter voor de functie van het type is `String`. 

### <a name="plain-old-java-objects-pojos"></a>Plain oude Java-objecten (pojo's)

Tekenreeksen die zijn geformatteerd met JSON wordt naar Java-typen worden geconverteerd als de invoer handtekening van de functie dat Java-type verwacht. Deze conversie een hoeveelheid kunt u doorgeeft in JSON en werken met Java-typen.

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

Lege invoerwaarden mogelijk `null` als het argument functies, maar een aanbevolen manier om op te lossen met lege waarden is het gebruik van potentieel `Optional<T>`.


## <a name="function-method-overloading"></a>Overbelasting van de functie-methode

U mag functie methoden met dezelfde naam maar met verschillende typen overbelasten. Bijvoorbeeld, u kunt hebben beide `String echo(String s)` en `String echo(MyType s)` in een klasse. Azure Functions besluit welke methode u wilt aanroepen op basis van het invoertype (voor HTTP-invoer, MIME-type `text/plain` leidt tot `String` terwijl `application/json` vertegenwoordigt `MyType`).

## <a name="inputs"></a>Invoer

Invoer zijn onderverdeeld in twee categorieën in Azure Functions: de invoer voor de werkstroomtrigger is en de andere is de aanvullende invoer. Hoewel ze in verschillende zijn `function.json`, de syntaxis is vrijwel identiek in Java-code. We houden het volgende codefragment als voorbeeld:

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class MyClass {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String in,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") MyObject obj
    ) {
        return "Hello, " + in + " and " + obj.getKey() + ".";
    }

    public static class MyObject {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
}
```

Wanneer deze functie wordt geactiveerd, de HTTP-aanvraag wordt doorgegeven aan de functie door `String in`. Een vermelding wordt opgehaald uit de Azure Table Storage op basis van de ID in de URL van de route en aangebracht beschikbaar als `obj` in de hoofdtekst van de functie.

## <a name="outputs"></a>Uitvoer

Uitvoer kunnen worden uitgedrukt in de geretourneerde waarde of output-parameters. Als er slechts één uitvoer, u kunt het beste de geretourneerde waarde gebruiken. Voor meerdere uitvoer die u moet output-parameters gebruiken.

Geretourneerde waarde is de eenvoudigste vorm van uitvoer, u alleen de waarde van elk type geretourneerd en Azure Functions-runtime wordt geprobeerd het marshallen terug naar het werkelijke type (zoals een HTTP-antwoord).  U kunt aantekeningen uitvoer toepassen voor de functie-methode (de naameigenschap van de aantekening is $return) voor het definiëren van de uitvoer van de geretourneerde waarde.

Meerdere uitvoerwaarden produceren, gebruiken `OutputBinding<T>` dat is gedefinieerd in de `azure-functions-java-library` pakket. Als u wilt maken van een HTTP-antwoord en een bericht naar een wachtrij ook gepusht, kunt u er ongeveer als schrijven:

De functie voor het kopiëren inhoud van een blob kan bijvoorbeeld worden gedefinieerd als de volgende code. `@StorageAccount` aantekening wordt hier gebruikt om te voorkomen dat het dupliceren van de eigenschap connection voor zowel `@BlobTrigger` en `@BlobOutput`.

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class MyClass {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Gebruik `OutputBinding<byte[]`> om het maken van een binair bestand uitvoer (voor parameters); voor retourwaarden, gebruikt u alleen `byte[]`.

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
import com.microsoft.azure.functions.annotation.*;


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

## <a name="execution-context"></a>Context voor uitvoering

Interactie met Azure Functions-uitvoeringsomgeving via de `ExecutionContext` object dat is gedefinieerd de `azure-functions-java-library` pakket. Gebruik de `ExecutionContext` object aanroep informatie en functions runtime-gegevens gebruiken in uw code.

### <a name="custom-logging"></a>Aangepaste logboekregistratie

Toegang tot de logger Functions-runtime is beschikbaar via de `ExecutionContext` object. In dit logboek is gekoppeld aan de Azure monitor en kunt u met de vlag-waarschuwingen en fouten aangetroffen tijdens het uitvoeren van de functie.

De volgende voorbeeldcode, wordt er een waarschuwingsbericht weergegeven registreert als de hoofdtekst van de aanvraag ontvangen leeg is.

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

U kunt de Azure CLI voor stream van standaard Java en logboekregistratie van fouten, evenals de logboekregistratie van andere toepassingen. Eerst uw functie toepassing configureren voor het schrijven van logboekregistratie van toepassingen met de Azure CLI:

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

Geheime gegevens, zoals sleutels of tokens buiten uw broncode uit veiligheidsoverwegingen houden. Sleutels en tokens gebruiken in uw functiecode aan te geven door het lezen van omgevingsvariabelen.

Als u wilt instellen van omgevingsvariabelen bij het uitvoeren van Azure Functions lokaal, kunt u besluiten deze variabelen toevoegen aan het bestand local.settings.json. Indien deze nog niet aanwezig in de hoofdmap van uw functieproject, kunt u een kunt maken. Hier ziet u hoe het bestand eruit moet zien:

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
> Als deze benadering wordt overgenomen, worden zeker om toe te voegen van de local.settings.json bestand naar uw opslagplaats negeren-bestand, zodat deze niet is doorgevoerd.

Met de code nu, afhankelijk van deze omgevingsvariabelen, kunt u zich naar de Azure-portal voor het instellen van dezelfde sleutel / waarde-paren in de instellingen van uw functie-app zodat uw code oftewel werkt wanneer getest lokaal en wanneer geïmplementeerd op Azure.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over het ontwikkelen van Java voor Azure-functie:

* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
- Lokale ontwikkeling en foutopsporing met [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md), en [Eclipse](functions-create-maven-eclipse.md). 
* [Externe foutopsporing voor Java Azure Functions met Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Maven-invoegtoepassing voor Azure Functions](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md) -functie maken via stroomlijnen de `azure-functions:add` doel en een staging-map voor te bereiden [ZIP-bestandsimplementatie](deployment-zip-push.md).
