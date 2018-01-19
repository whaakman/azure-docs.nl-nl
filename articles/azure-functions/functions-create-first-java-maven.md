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
ms.openlocfilehash: c0984075cd8e372cce09ea100378dcd4e8cddabe
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>Uw eerste functie maken met Java en Maven (Preview)

> [!NOTE] 
> Java voor Azure Functions is momenteel in preview.

In deze snelstartgids vindt u instructies voor het maken van een [serverloos](https://azure.microsoft.com/overview/serverless-computing/) Functions-project met Maven. Daarnaast wordt uitgelegd hoe u het project lokaal kunt testen en vervolgens kunt implementeren naar Azure Functions. Wanneer u bent klaar, hebt u een door HTTP getriggerde functie-app die in Azure wordt uitgevoerd.

![Hello World-functie aanroepen vanaf de opdrachtregel met cURL](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten
Als u functie-apps wilt ontwikkelen met behulp van Java, moet het volgende zijn geïnstalleerd:

-  [.NET Core](https://www.microsoft.com/net/core), nieuwste versie.
-  [Java Developer Kit](https://www.azul.com/downloads/zulu/), versie 8.
-  [Azure-CLI](https://docs.microsoft.com/cli/azure)
-  [Apache Maven](https://maven.apache.org), versie 3.0 of hoger.
-  [Node.js](https://nodejs.org/download/), versie 8.6 of hoger.

> [!IMPORTANT] 
> De omgevingsvariabele JAVA_HOME moet zijn ingesteld op de installatielocatie van de JDK om deze snelstartgids te kunnen voltooien.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools installeren

[Azure Functions Core Tools 2.0](https://www.npmjs.com/package/azure-functions-core-tools) is een lokale ontwikkelingsomgeving voor het schrijven, uitvoeren en debuggen van Azure Functions. Installeer de met [npm](https://www.npmjs.com/), dat is opgenomen in [Node.js](https://nodejs.org/).

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



