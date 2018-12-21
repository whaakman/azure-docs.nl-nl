---
title: Java-web-app ontwikkelen in Linux - Azure App Service
description: Ontwikkel, implementeer en schaal Spring Boot-Java-Web-apps met Azure App Service in Linux en Azure Cosmos DB.
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.service: app-service-web
ms.devlang: java
ms.topic: tutorial
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: 41f27782ab9bc6b0c72ef817472779ccca0e0d3e
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53337220"
---
# <a name="tutorial-build-a-java-web-app-using-spring-and-azure-cosmos-db"></a>Zelfstudie: Een Java-web-app bouwen met Spring en Azure Cosmos DB

In deze zelfstudie wordt u door het proces van het ontwikkelen, configureren, implementeren en schalen van Java-web-apps in Azure geleid. Wanneer u klaar bent, hebt u een [Spring Boot](https://projects.spring.io/spring-boot/)-toepassing die gegevens opslaat in [Azure Cosmos DB](/azure/cosmos-db) op [Azure App Service in Linux](/azure/app-service/containers).

![Java-app uitgevoerd in Azure App Service](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een Cosmos DB-database.
> * Een voorbeeld-app verbinden met de database en deze lokaal testen
> * De voorbeeld-app implementeren in Azure
> * Logboeken met diagnostische gegevens vanaf App Service streamen
> * Extra exemplaren toevoegen om de voorbeeld-app uit te breiden

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* [Azure CLI](https://docs.microsoft.com/cli/azure/overview), geïnstalleerd op uw eigen computer. 
* [Git](https://git-scm.com/)
* [Java JDK](https://aka.ms/azure-jdks)
* [Maven](https://maven.apache.org)

## <a name="clone-the-sample-todo-app-and-prepare-the-repo"></a>Kloon de voorbeeld-app voor taken en bereid de opslagplaats voor

In deze zelfstudie wordt een voorbeeld-app voor een takenlijst gebruikt met een web-UI die een Spring REST API aanroept met ondersteuning van [Spring Data voor Azure Cosmos DB](https://github.com/Microsoft/spring-data-cosmosdb). De code voor de app is beschikbaar [op GitHub](https://github.com/Microsoft/spring-todo-app). Zie voor meer informatie over het schrijven van Java-apps met Spring en Cosmos DB de zelfstudie [Spring Boot Starter met de Azure Cosmos DB SQL-API](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db ) en de snelstart [Spring Data voor Azure Cosmos DB](https://github.com/Microsoft/spring-data-cosmosdb#quick-start).


Voer de volgende opdrachten in de terminal uit om de voorbeeldopslagplaats te klonen en de omgeving van de voorbeeld-app in te stellen.

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
cd e2e-java-experience-in-app-service-linux-part-2
yes | cp -rf .prep/* .
```

## <a name="create-an-azure-cosmos-db"></a>Een Azure Cosmos DB maken

Volg deze stappen voor het maken van een Azure Cosmos DB-database in uw abonnement. De takenlijst-app maakt verbinding met deze database en slaat de gegevens op wanneer deze actief is, waarbij de status van de toepassing wordt behouden ongeacht waar u de toepassing uitvoert.

1. Meld uw Azure-CLI aan en stel eventueel uw abonnement in als er meer dan één abonnement verbonden is met uw aanmeldingsreferenties.

    ```bash
    az login
    az account set -s <your-subscription-id>
    ```   

2. Maak een Azure-resourcegroep, waarbij de naam van de resourcegroep noteert.

    ```bash
    az group create -n <your-azure-group-name> \
        -l <your-resource-group-region>
    ```

3. Maken Azure Cosmos DB met het `GlobalDocumentDB` type. Voor de Cosmos DB-naam mag u alleen kleine letters gebruiken. Noteer het veld `documentEndpoint` in de reactie van de opdracht.

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
    ```

4. Haal uw Azure Cosmos DB-sleutel op om verbinding te maken met de app. Houd de `primaryMasterKey`, `documentEndpoint` bij de hand, want u hebt ze in de volgende stap nodig.

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="configure-the-todo-app-properties"></a>De eigenschappen van de takenlijst-app configureren

Open een terminal op uw computer. Kopieer het bestand met het voorbeeldscript naar de gekloonde opslagplaats, zodat u deze kunt aanpassen voor de Cosmos DB-database die u zojuist hebt gemaakt.

```bash
cd initial/spring-todo-app
cp set-env-variables-template.sh .scripts/set-env-variables.sh
```
 
Bewerk `.scripts/set-env-variables.sh` in uw favoriete editor en voorziet deze van de Azure Cosmos DB-verbindingsgegevens. Gebruik voor de configuratie van de App Service Linux, dezelfde regio als voorheen (`your-resource-group-region`) en de resourcegroep (`your-azure-group-name`) die wordt gebruikt bij het maken van de Cosmos DB-database. Kies een WEBAPP_NAME die uniek is, omdat hiervoor niet de naam van een web-app in een Azure-implementatie kan worden gedupliceerd.

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>

// App Service Linux Configuration
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

Voer daarna het script uit:

```bash
source .scripts/set-env-variables.sh
```
   
Deze omgevingsvariabelen worden in `application.properties` in de takenlijst-app gebruikt. Met de velden in het eigenschappenbestand wordt een standaardconfiguratie voor de opslagplaats voor Spring Data ingesteld:

```properties
azure.cosmosdb.uri=${COSMOSDB_URI}
azure.cosmosdb.key=${COSMOSDB_KEY}
azure.cosmosdb.database=${COSMOSDB_DBNAME}
```

```java
@Repository
public interface TodoItemRepository extends DocumentDbRepository<TodoItem, String> {
}
```

De voorbeeld-app gebruikt vervolgens de aantekening `@Document` die is geïmporteerd uit `com.microsoft.azure.spring.data.cosmosdb.core.mapping.Document` om een entiteitstype in te stellen dat wordt opgeslagen en beheerd door Cosmos DB:

```java
@Document
public class TodoItem {
    private String id;
    private String description;
    private String owner;
    private boolean finished;
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Gebruik Maven om het voorbeeld uit te voeren.

```bash
mvn package spring-boot:run
```

De uitvoer moet er als volgt uitzien.

```bash
bash-3.2$ mvn package spring-boot:run
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 


[INFO] SimpleUrlHandlerMapping - Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] SimpleUrlHandlerMapping - Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] WelcomePageHandlerMapping - Adding welcome page: class path resource [static/index.html]
2018-10-28 15:04:32.101  INFO 7673 --- [           main] c.m.azure.documentdb.DocumentClient      : Initializing DocumentClient with serviceEndpoint [https://sample-cosmos-db-westus.documents.azure.com:443/], ConnectionPolicy [ConnectionPolicy [requestTimeout=60, mediaRequestTimeout=300, connectionMode=Gateway, mediaReadMode=Buffered, maxPoolSize=800, idleConnectionTimeout=60, userAgentSuffix=;spring-data/2.0.6;098063be661ab767976bd5a2ec350e978faba99348207e8627375e8033277cb2, retryOptions=com.microsoft.azure.documentdb.RetryOptions@6b9fb84d, enableEndpointDiscovery=true, preferredLocations=null]], ConsistencyLevel [null]
[INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
[INFO] TomcatWebServer - Tomcat started on port(s): 8080 (http) with context path ''
[INFO] TodoApplication - Started TodoApplication in 45.573 seconds (JVM running for 76.534)
```

U hebt lokaal toegang tot de Spring-takenlijst-app via deze koppeling zodra de app wordt gestart: [http://localhost:8080/](http://localhost:8080/).

 ![](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

Als u uitzonderingen ziet in plaats van het bericht 'TodoApplication gestart', controleert u of het script `bash` in de vorige stap de omgevingsvariabelen correct heeft geëxporteerd en of de waarden juist zijn voor de Azure Cosmos DB-database die u hebt gemaakt.

## <a name="configure-azure-deployment"></a>Azure-implementatie configureren

Open het `pom.xml`-bestand in de map `initial/spring-boot-todo` en voeg de volgende configuratie [Maven-invoegtoepassing voor Azure App Service](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) toe.

```xml    
<plugins> 

    <!--*************************************************-->
    <!-- Deploy to Java SE in App Service Linux           -->
    <!--*************************************************-->
       
    <plugin>
        <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.4.0</version>
            <configuration>
            <deploymentType>jar</deploymentType>
            
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>jre8</linuxRuntime>
            
            <appSettings>
                <property>
                    <name>COSMOSDB_URI</name>
                    <value>${COSMOSDB_URI}</value>
                </property>
                <property>
                    <name>COSMOSDB_KEY</name>
                    <value>${COSMOSDB_KEY}</value>
                </property>
                <property>
                    <name>COSMOSDB_DBNAME</name>
                    <value>${COSMOSDB_DBNAME}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Dserver.port=80</value>
                </property>
            </appSettings>
            
        </configuration>
    </plugin>            
    ...
</plugins>
```

## <a name="deploy-to-app-service-on-linux"></a>Implementeren naar App Service in Linux

Gebruik het Maven-doel `azure-webapp:deploy` om de takenlijst-app naar Azure App Service te implementeren in Linux.

```bash

// Deploy
bash-3.2$ mvn azure-webapp:deploy
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- azure-webapp-maven-plugin:1.4.0:deploy (default-cli) @ spring-todo-app ---
[INFO] Authenticate with Azure CLI 2.0
[INFO] Target Web App doesn't exist. Creating a new one...
[INFO] Creating App Service Plan 'ServicePlanb6ba8178-5bbb-49e7'...
[INFO] Successfully created App Service Plan.
[INFO] Successfully created Web App.
[INFO] Trying to deploy artifact to spring-todo-app...
[INFO] Successfully deployed the artifact to https://spring-todo-app.azurewebsites.net
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:19 min
[INFO] Finished at: 2018-10-28T15:32:03-07:00
[INFO] Final Memory: 50M/574M
[INFO] ------------------------------------------------------------------------
```

De uitvoer bevat de URL naar uw geïmplementeerde toepassing (in dit voorbeeld `https://spring-todo-app.azurewebsites.net` ). U kunt deze URL kopiëren naar uw webbrowser of de volgende opdracht in uw Terminal-venster uitvoeren om uw app te laden.

```bash
open https://spring-todo-app.azurewebsites.net
```

U moet de app zien wanneer die wordt uitgevoerd met de externe URL in de adresbalk:

 ![](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-in-app-service.jpg)

## <a name="view-logs-to-troubleshoot-the-app"></a>Logboeken weergeven om problemen met de app op te lossen

Logboekregistratie inschakelen voor de naar App Service geïmplementeerde Java web-app in Linux:

```bash
az webapp log config --name ${WEBAPP_NAME} \
 --resource-group ${RESOURCEGROUP_NAME} \
  --web-server-logging filesystem
```

Stream vervolgens de logboeken van de web-app naar uw terminal:

```bash
az webapp log tail --name ${WEBAPP_NAME} \
 --resource-group ${RESOURCEGROUP_NAME}
```

U ziet de meest recente uitvoerregels, en als er nieuwe aanvragen worden gedaan naar de taken-app, worden ze naar de console gestreamd. Gebruik CTRL + C om de console af te sluiten.

```bash
bash-3.2$ az webapp log tail --name ${WEBAPP_NAME}  --resource-group ${RESOURCEGROUP_NAME}
2018-10-28T22:50:17  Welcome, you are now connected to log-streaming service.
2018-10-28T22:44:56.265890407Z   _____                               
2018-10-28T22:44:56.265930308Z   /  _  \ __________ _________   ____  
2018-10-28T22:44:56.265936008Z  /  /_\  \___   /  |  \_  __ \_/ __ \ 
2018-10-28T22:44:56.265940308Z /    |    \/    /|  |  /|  | \/\  ___/ 
2018-10-28T22:44:56.265944408Z \____|__  /_____ \____/ |__|    \___  >
2018-10-28T22:44:56.265948508Z         \/      \/                  \/ 
2018-10-28T22:44:56.265952508Z A P P   S E R V I C E   O N   L I N U X
2018-10-28T22:44:56.265956408Z Documentation: http://aka.ms/webapp-linux
2018-10-28T22:44:56.266260910Z Setup openrc ...
2018-10-28T22:44:57.396926506Z Service `hwdrivers' needs non existent service `dev'
2018-10-28T22:44:57.397294409Z  * Caching service dependencies ... [ ok ]
2018-10-28T22:44:57.474152273Z Starting ssh service...
...
...
2018-10-28T22:46:13.432160734Z [INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
2018-10-28T22:46:13.744859424Z [INFO] TomcatWebServer - Tomcat started on port(s): 80 (http) with context path ''
2018-10-28T22:46:13.783230205Z [INFO] TodoApplication - Started TodoApplication in 57.209 seconds (JVM running for 70.815)
2018-10-28T22:46:14.887366993Z 2018-10-28 22:46:14.887  INFO 198 --- [p-nio-80-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
2018-10-28T22:46:14.887637695Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization started
2018-10-28T22:46:14.998479907Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization completed in 111 ms

2018-10-28T22:49:20.572059062Z Sun Oct 28 22:49:20 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:25.850543080Z Sun Oct 28 22:49:25 GMT 2018 DELETE ======= /api/todolist/{4f41ab03-1b12-4131-a920-fe5dfec106ca} ======= 
2018-10-28T22:49:26.047126614Z Sun Oct 28 22:49:26 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:30.201740227Z Sun Oct 28 22:49:30 GMT 2018 POST ======= /api/todolist ======= Milk
2018-10-28T22:49:30.413468872Z Sun Oct 28 22:49:30 GMT 2018 GET ======= /api/todolist =======
```

## <a name="scale-out-the-todo-app"></a>De takenlijst-app uitbreiden

Breid de toepassing uit door een andere werknemer toe te voegen:

```bash
az appservice plan update --number-of-workers 2 \
   --name ${WEBAPP_PLAN_NAME} \
   --resource-group <your-azure-group-name>
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze resources niet voor een andere zelfstudie nodig hebt (zie [Volgende stappen](#next)), kunt u ze verwijderen door de volgende opdracht in de Cloud Shell uit te voeren: 
  
```bash
az group delete --name your-azure-group-name
``` 

<a name="next"></a>

## Next steps

[Azure for Java Developers](/java/azure/)
[Spring Boot](https://spring.io/projects/spring-boot), 
[Spring Data for Cosmos DB](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable), 
[Azure Cosmos DB](/azure/cosmos-db/sql-api-introduction)
and
[App Service Linux](/azure/app-service/containers/app-service-linux-intro).

Learn more about running Java apps on App Service on Linux in the developer guide.

> [!div class="nextstepaction"] 
> [Java in App Service Linux dev guide](/azure/app-service/containers/app-service-linux-java)
