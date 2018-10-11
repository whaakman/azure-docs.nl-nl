---
title: Een Java-toepassing ontwikkelen met behulp van Azure Cosmos DB Async Java SDK | Microsoft Docs
description: In deze zelfstudie leert u hoe u Azure Cosmos DB SQL API-accounts kunt gebruiken voor het opslaan van en toegang krijgen tot gegevens met behulp van een asynchrone Java-toepassing.
keywords: nosql zelfstudie, onlinedatabase, java-consoletoepassing
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: java
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: sngun
ms.openlocfilehash: aa2613f7cb73c2c338189aaaa48587c49a3093f5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46962183"
---
# <a name="build-a-java-application-by-using-azure-cosmos-db-async-java-sdk"></a>Een Java-toepassing ontwikkelen met behulp van Azure Cosmos DB Async Java SDK 

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Azure Cosmos DB is een wereldwijd gedistribueerde, multi-model database. In deze zelfstudie leert u hoe u Azure Cosmos DB SQL API-accounts kunt gebruiken voor het opslaan van en toegang krijgen tot gegevens met behulp van een asynchrone Java-toepassing. 

We behandelen de volgende onderwerpen:

* Een Azure Cosmos DB-account maken en er verbinding mee maken
* Uw oplossing configureren
* Een verzameling maken
* JSON-documenten maken
* Query's uitvoeren op de verzameling

Tijd om aan de slag te gaan.

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u over de volgende zaken beschikt:

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Git](https://git-scm.com/downloads).
* [Java Development Kit (JDK) 8+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Stap 1: een Azure Cosmos DB-account maken
Begin met het maken van een Azure Cosmos DB-account. Als u al een account hebt dat u wilt gebruiken, kunt u verder naar de stap [Het GitHub-project klonen](#GitClone). Als u de Azure Cosmos DB-emulator gebruikt, volgt u de stappen in [Azure Cosmos DB-emulator](local-emulator.md) om de emulator in te stellen en meteen naar [Het GitHub-project klonen](#GitClone) te gaan.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>Stap 2: het GitHub-project klonen
Kloon eerst de GitHub-opslagplaats voor [Get Started with Azure Cosmos DB and Java](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started) (Aan de slag met Azure Cosmos DB en Java). Voer bijvoorbeeld vanuit een lokale map het volgende uit als u het voorbeeldproject lokaal wilt ophalen.

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started.git

cd azure-cosmos-db-sql-api-async-java-getting-started
cd azure-cosmosdb-get-started

```
De map bevat een `pom.xml` voor het project en een `src/main/java/com/microsoft/azure/cosmosdb/sample`-map met Java-broncode, waaronder `Main.java`. Deze toont hoe u eenvoudige bewerkingen kunt uitvoeren met Azure Cosmos DB, zoals het maken van documenten en het uitvoeren van query's op gegevens binnen een verzameling. De `pom.xml` bevat een afhankelijkheid voor de [Azure Cosmos DB Java SDK in Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-documentdb</artifactId>
  <version>2.0.0</version>
</dependency>
```

## <a id="Connect"></a>Stap 3: verbinding maken met een Azure Cosmos DB-account
Ga vervolgens terug naar [Azure Portal](https://portal.azure.com) om uw eindpunt en primaire hoofdsleutel op te halen. Uw toepassing heeft het Azure Cosmos DB-eindpunt en de primaire sleutel nodig om te bepalen waarmee verbinding moet worden gemaakt en om ervoor te zorgen dat Azure Cosmos DB de verbinding van uw toepassing vertrouwt. Het bestand `AccountSettings.java` bevat de primaire sleutel en de URI-waarden. 

Navigeer in Azure Portal naar uw Azure Cosmos DB-account en klik daarna op **Sleutels**. Kopieer de URI en de PRIMAIRE SLEUTEL van de portal en plak deze in het bestand `AccountSettings.java`. 

```java
public class AccountSettings 
{
  // Replace MASTER_KEY and HOST with values from your Azure Cosmos DB account.
    
  // <!--[SuppressMessage("Microsoft.Security", "CS002:SecretInNextLine")]-->
  public static String MASTER_KEY = System.getProperty("ACCOUNT_KEY", 
          StringUtils.defaultString(StringUtils.trimToNull(
          System.getenv().get("ACCOUNT_KEY")), "<Fill your Azure Cosmos DB account key>"));

  public static String HOST = System.getProperty("ACCOUNT_HOST",
           StringUtils.defaultString(StringUtils.trimToNull(
           System.getenv().get("ACCOUNT_HOST")),"<Fill your Azure Cosmos DB URI>"));
}
```

![Schermopname van Azure Portal die voor de NoSQL-zelfstudie wordt gebruikt om een Java-consoletoepassing te maken. Schermopname van Azure Portal waarin een Azure Cosmos DB-account wordt weergegeven met de hub ACTIEF gemarkeerd. Verder is de knop SLEUTELS gemarkeerd op de Azure Cosmos DB-accountblade en zijn de waarden URI, PRIMAIRE SLEUTEL en SECUNDAIRE SLEUTEL gemarkeerd op de blade Sleutels][keys]

## <a name="step-4-initialize-the-client-object"></a>Stap 4: het clientobject initialiseren
Initialiseer het clientobject met behulp van de waarden van de host-URI en van de primaire sleutel die zijn gedefinieerd in het bestand AccountSettings.java

```java
client = new AsyncDocumentClient.Builder()
         .withServiceEndpoint(AccountSettings.HOST)
         .withMasterKey(AccountSettings.MASTER_KEY)
         .withConnectionPolicy(ConnectionPolicy.GetDefault())
         .withConsistencyLevel(ConsistencyLevel.Session)
         .build();
```

## <a id="CreateDatabase"></a>Stap 5: een database maken

Uw Azure Cosmos DB-[database](sql-api-resources.md#databases) kan worden gemaakt met de methode createDatabaseIfNotExists() van de klasse DocumentClient. Een database is een logische container voor een JSON-documentopslag, gepartitioneerd in verzamelingen.

```java
private void createDatabaseIfNotExists() throws Exception 
{
    
    writeToConsoleAndPromptToContinue("Check if database " + databaseName + " exists.");

    String databaseLink = String.format("/dbs/%s", databaseName);

    Observable<ResourceResponse<Database>> databaseReadObs = client.readDatabase(databaseLink, null);

    Observable<ResourceResponse<Database>> databaseExistenceObs = databaseReadObs
                .doOnNext(x -> {System.out.println("database " + databaseName + " already exists.");}).onErrorResumeNext(e -> {
   // if the database doesn't already exists
   // readDatabase() will result in 404 error
   if (e instanceof DocumentClientException) 
   {
       DocumentClientException de = (DocumentClientException) e;
       // if database 
       if (de.getStatusCode() == 404) {
       // if the database doesn't exist, create it.
       System.out.println("database " + databaseName + " doesn't existed," + " creating it...");
       Database dbDefinition = new Database();
       dbDefinition.setId(databaseName);
       return client.createDatabase(dbDefinition, null);
     }
     }

    // some unexpected failure in reading database happened.
    // pass the error up.
    System.err.println("Reading database " + databaseName + " failed.");
        return Observable.error(e);     
    });

   // wait for completion
   databaseExistenceObs.toCompletable().await();

   System.out.println("Checking database " + databaseName + " completed!\n");
}
```

## <a id="CreateColl"></a>Stap 6: een verzameling maken

> [!WARNING]
> Met **createCollection** maakt u een nieuwe verzameling met gereserveerde doorvoer, wat gevolgen heeft voor de kosten. Zie onze [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie.
> 
> 
U kunt een verzameling maken met de methode createDocumentCollectionIfNotExists() van de klasse DocumentClient. Een verzameling is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica.

```java
private void createDocumentCollectionIfNotExists() throws Exception 
{
    
    writeToConsoleAndPromptToContinue("Check if collection " + collectionName + " exists.");

    // query for a collection with a given id
    // if it exists nothing else to be done
    // if the collection doesn't exist, create it.

    String databaseLink = String.format("/dbs/%s", databaseName);

    // we know there is only single page of result (empty or with a match)
    client.queryCollections(databaseLink, new SqlQuerySpec("SELECT * FROM r where r.id = @id", 
            new SqlParameterCollection(new SqlParameter("@id", collectionName))), null).single() 
            .flatMap(page -> {
            if (page.getResults().isEmpty()) {
             // if there is no matching collection create the collection.
             DocumentCollection collection = new DocumentCollection();
             collection.setId(collectionName);
             System.out.println("Creating collection " + collectionName);

             return client.createCollection(databaseLink, collection, null);
            } 
            else {
              // collection already exists, nothing else to be done.
              System.out.println("Collection " + collectionName + "already exists");
              return Observable.empty();
            }
      }).toCompletable().await();

 System.out.println("Checking collection " + collectionName + " completed!\n");
    }
```

## <a id="CreateDoc"></a>Stap 7: JSON-documenten maken

U kunt een [document](sql-api-resources.md#documents) maken met de methode createDocument van de klasse DocumentClient. Documenten bestaan uit door gebruikers gedefinieerde (willekeurige) JSON-inhoud. U kunt nu een of meer documenten invoegen. Het bestand src/main/java/com/microsoft/azure/cosmosdb/sample/Families.java definieert de familie waartoe de JSON-documenten behoren 

```java
public static Family getJohnsonFamilyDocument() {
     Family andersenFamily = new Family();
     andersenFamily.setId("Johnson" + System.currentTimeMillis());
     andersenFamily.setLastName("Johnson");

      Parent parent1 = new Parent();
      parent1.setFirstName("John");

      Parent parent2 = new Parent();
      parent2.setFirstName("Lili");

      return andersenFamily;
    }
```

## <a id="Query"></a>Stap 8: query's uitvoeren op Azure Cosmos DB-resources

Azure Cosmos DB biedt ondersteuning voor uitgebreide [query's](sql-api-sql-query.md) op de JSON-documenten die zijn opgeslagen in elke verzameling. De volgende voorbeeldcode laat zien hoe u query's kunt toepassen op documenten in Azure Cosmos DB met behulp van de SQL-syntaxis met de methode [queryDocuments](/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querydocuments).

```java
private void executeSimpleQueryAsyncAndRegisterListenerForResult(CountDownLatch completionLatch) 
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setMaxItemCount(100);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    Observable<FeedResponse<Document>> queryObservable = client.queryDocuments(collectionLink,
           "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    queryObservable.subscribe(queryResultPage -> {
            System.out.println("Got a page of query result with " +
            queryResultPage.getResults().size() + " document(s)"
            + " and request charge of " + queryResultPage.getRequestCharge());
    },
   // terminal error signal
        e -> {
          e.printStackTrace();
          completionLatch.countDown();
        },

   // terminal completion signal
         () -> {
            completionLatch.countDown();
         });
}
```

## <a id="Run"></a>Stap 9: uw Java-consoletoepassing volledig uitvoeren
Als u de toepassing in de console wilt uitvoeren, gaat u naar de projectmap en compileert u de code met Maven:

```bash
mvn package
```

Als u `mvn package` uitvoert, wordt de nieuwste Azure Cosmos DB-bibliotheek vanuit Maven gedownload en wordt `GetStarted-0.0.1-SNAPSHOT.jar` geproduceerd. Voer vervolgens de app uit door het volgende uit te voeren:

```bash
mvn exec:java -DACCOUNT_HOST=<YOUR_COSMOS_DB_HOSTNAME> -DACCOUNT_KEY= <YOUR_COSMOS_DB_MASTER_KEY>
```
Gefeliciteerd! U hebt de NoSQL-zelfstudie voltooid en beschikt nu over een werkende Java-consoletoepassing.

## <a name="next-steps"></a>Volgende stappen
* Wilt u een zelfstudie voor Java-web-apps volgen? Zie [Build a web application with Java using Azure Cosmos DB](sql-api-java-application.md) (Een Java-web-app maken met Azure Cosmos DB).
* Leer hoe het [bewaken van een Azure Cosmos DB-account](monitor-accounts.md) werkt.
* Voer query's uit op onze voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png
