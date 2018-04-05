---
title: Uw eerste functie maken in Azure met behulp van Java en Maven | Microsoft Docs
description: Lees hier hoe u een eenvoudige, door HTTP getriggerde functie maakt en publiceert naar Azure met Java en Maven.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: azure-functies, functies, gebeurtenisverwerking, berekenen, architectuur zonder server
ms.service: functions
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/07/2017
ms.author: routlaw, glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 81d9d8790a750f34133f3f00dafc15c56185d7b1
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>Uw eerste functie maken met Java en Maven (Preview)

> [!NOTE] 
> Java voor Azure Functions is momenteel in preview.

In deze snelstart vindt u instructies voor het maken van een [serverloos](https://azure.microsoft.com/overview/serverless-computing/) Functions-project met Maven. Daarnaast wordt uitgelegd hoe u het project lokaal kunt testen en vervolgens kunt implementeren naar Azure Functions. Wanneer u bent klaar, hebt u een door HTTP getriggerde functie-app die in Azure wordt uitgevoerd.

![Hello World-functie aanroepen vanaf de opdrachtregel met cURL](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten
Als u functie-apps wilt ontwikkelen met behulp van Java, moet het volgende zijn geïnstalleerd:

-  [Java Developer Kit](https://www.azul.com/downloads/zulu/), versie 8.
-  [Apache Maven](https://maven.apache.org), versie 3.0 of hoger.
-  [Azure-CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> De omgevingsvariabele JAVA_HOME moet zijn ingesteld op de installatielocatie van de JDK om deze quickstart te kunnen voltooien.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools installeren

[Azure Functions Core Tools 2.0](https://www.npmjs.com/package/azure-functions-core-tools) is een lokale ontwikkelingsomgeving voor het schrijven, uitvoeren en debuggen van Azure Functions. 

Als u deze wilt installeren, gaat u naar het gedeelte [Installeren](https://github.com/azure/azure-functions-core-tools#installing) en zoekt u naar de specifieke instructies voor uw besturingssysteem (Windows, Linux, Mac).

U kunt deze ook handmatig installeren met [npm](https://www.npmjs.com/), opgenomen in [Node.js](https://nodejs.org/), na de volgende vereiste installaties:

-  [.NET Core](https://www.microsoft.com/net/core), nieuwste versie.
-  [Node.js](https://nodejs.org/download/), versie 8.6 of hoger.

Voer het volgende uit om door te gaan met installeren op basis van npm:

```
npm install -g azure-functions-core-tools@core
```

> [!NOTE]
> Als er problemen zijn bij het installeren van Azure Functions Core Tools versie 2.0, raadpleegt u [Runtime versie 2.x](/azure/azure-functions/functions-run-local#version-2x-runtime).

## <a name="generate-a-new-functions-project"></a>Een nieuw Functions-project genereren

Voer in een lege map de volgende opdracht uit om het Functions-project te genereren op basis van een [Maven-archetype](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html).

### <a name="linuxmacos"></a>Linux/Mac OS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

### <a name="windows-cmd"></a>Windows (CMD)
```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-archetype
```

U wordt door Maven gevraagd om de waarden die nodig zijn om het project te kunnen genereren. Informatie over de waarden voor _groupId_, _artifactId_ en _version_ kunt u vinden in de Engelstalige [naslag van Maven over naamconventies](https://maven.apache.org/guides/mini/guide-naming-conventions.html). De waarde voor _appName_ moet uniek zijn binnen Azure. Om die reden genereert Maven standaard een app-naam op basis van de eerder opgegeven waarde voor _artifactId_. De waarde voor _packageName_ bepaalt het Java-pakket voor de gegenereerde functiecode.

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Confirm properties configuration: Y
```

Maven maakt de projectbestanden in een nieuwe map met de naam _artifactId_. De gegenereerde code in het project is een eenvoudige, [door HTTP getriggerde](/azure/azure-functions/functions-bindings-http-webhook) functie die de hoofdtekst van de aanvraag weergeeft:

```java
public class Function {
    /**
     * This function listens at endpoint "/api/hello". Two ways to invoke it using "curl" command in bash:
     * 1. curl -d "HTTP Body" {your host}/api/hello
     * 2. curl {your host}/api/hello?name=HTTP%20Query
     */
    @FunctionName("hello")
    public HttpResponseMessage<String> hello(
            @HttpTrigger(name = "req", methods = {"get", "post"}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponse(400, "Please pass a name on the query string or in the request body");
        } else {
            return request.createResponse(200, "Hello, " + name);
        }
    }
}

```

## <a name="run-the-function-locally"></a>De functie lokaal uitvoeren

Ga naar de zojuist gemaakte projectmap en gebruik Maven om de functie te bouwen en uit te voeren:

```
cd fabrikam-function
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]
> Als u deze uitzondering krijgt: `javax.xml.bind.JAXBException` met Java 9, bekijkt u de tijdelijke oplossing op [GitHub](https://github.com/jOOQ/jOOQ/issues/6477).

U ziet deze uitvoer terwijl de functie wordt uitgevoerd:

```Output
Listening on http://localhost:7071
Hit CTRL-C to exit...

Http Functions:

   hello: http://localhost:7071/api/hello
```

Activeer de functie vanaf de opdrachtregel met de opdracht curl in een nieuwe terminal:

```
curl -w '\n' -d LocalFunction http://localhost:7071/api/hello
```

```Output
Hello LocalFunction!
```

Gebruik `Ctrl-C` in de terminal om de functiecode te stoppen.

## <a name="deploy-the-function-to-azure"></a>De functie implementeren in Azure

Bij het implementeren naar Azure Functions worden accountreferenties uit de Azure CLI gebruikt. [Meld u daarom aan met de Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) en implementeer de code vervolgens in een nieuwe functie app via de Maven-target `azure-functions:deploy`.

```
az login
mvn azure-functions:deploy
```

Als het implementeren is voltooid, ziet u de URL die u kunt gebruiken voor toegang tot de Azure-functie-app:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

Test de functie-app in Azure met behulp van de opdracht curl:

```
curl -w '\n' https://fabrikam-function-20170920120101928.azurewebsites.net/api/hello -d AzureFunctions
```

```Output
Hello AzureFunctions!
```

## <a name="next-steps"></a>Volgende stappen

U hebt een Java-functie-app gemaakt met een eenvoudige HTTP-trigger en deze geïmplementeerd naar Azure Functions.

- Neem de [Azure Functions Java-handleiding voor ontwikkelaars](functions-reference-java.md) door voor meer informatie over het ontwikkelen van Java-functies. Deze handleiding is vooralsnog door een vertaalmachine vertaald.
- U kunt extra functies met verschillende triggers toevoegen aan uw project met behulp van de Maven-target `azure-functions:add`.
- Gebruik Visual Studio Code om functies lokaal te debuggen. Als u het [Java Extension Pack](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack) hebt geïnstalleerd en het Functions-project hebt geopend in Visual Studio Code, [kunt u de debugger via attach koppelen](https://code.visualstudio.com/Docs/editor/debugging#_launch-configurations) aan poort 5005. Vervolgens stelt u een onderbrekingspunt in de editor in en activeert u de functie terwijl deze lokaal wordt uitgevoerd: ![Functies debuggen in Visual Studio Code](media/functions-create-java-maven/vscode-debug.png)
- Gebruik Visual Studio Code om op afstand fouten in functies op te sporen. Raadpleeg de documentatie [Writing serverless Java Applications](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud) (Serverloze Java-apps schrijven) voor meer informatie.
