---
title: Een Azure Cosmos DB-documentdatabase maken met Java
description: Biedt een voorbeeld van Java-code dat u kunt gebruiken om verbinding te maken met de SQL API van Azure Cosmos DB en er query’s op uit te voeren
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: quickstart
ms.date: 10/24/2018
ms.author: sngun
ms.openlocfilehash: 70aa5c62afb6706fa9084d1ed081f70517d8d89e
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54039432"
---
# <a name="create-and-manage-resources-of-an-azure-cosmos-db-sql-api-account-using-a-java-application"></a>Resources van een Azure Cosmos DB SQL API-account met behulp van een Java-toepassing maken en beheren

> [!div class="op_single_selector"]
> * [.NET](create-sql-api-dotnet.md)
> * [.NET (preview)](create-sql-api-dotnet-preview.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

In deze quickstart wordt getoond hoe u resources van een Azure Cosmos DB [SQL API](sql-api-introduction.md)-account met behulp van een Java-toepassing kunt maken en beheren. Eerst maakt u een Azure Cosmos DB SQL API-account via de Azure-portal, dan maakt u een Java-app met behulp van de [SQL Java SDK](sql-api-sdk-async-java.md) en voegt u met de Java-toepassing resources toe aan uw Cosmos DB-account. De instructies in deze snelstartgids kunnen worden uitgevoerd in elk besturingssysteem waarmee Java kan worden uitgevoerd. Als u deze quickstart hebt voltooid, weet u hoe u Cosmos DB-databases kunt maken en wijzigen, met behulp van verzamelingen in de UI of via een programma, afhankelijk van uw voorkeur.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Daarnaast doet u het volgende: 

* [Java Development Kit (JDK) 1.8+](https://aka.ms/azure-jdks)
    * Voer op Ubuntu `apt-get install default-jdk` uit om de JDK te installeren.
    * Zorg dat de omgevingsvariabele JAVA_HOME verwijst naar de map waarin de JDK is geïnstalleerd.
* [Download](https://maven.apache.org/download.cgi) en [installeer](https://maven.apache.org/install.html) een binair [Maven](https://maven.apache.org/)-archief
    * Op Ubuntu kunt u `apt-get install maven` uitvoeren om Maven te installeren.
* [Git](https://www.git-scm.com/)
    * Op Ubuntu kunt u `sudo apt-get install git` uitvoeren om Git te installeren.

## <a name="create-a-database-account"></a>Een databaseaccount maken

Voordat u een documentdatabase kunt maken, moet u een SQL-API-account maken met Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Een verzameling toevoegen

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Voorbeeldgegevens toevoegen

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Uw gegevens opvragen

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Nu gaan we werken met code. We gaan nu een SQL API-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt een kopie van de voorbeeld-app op uw computer.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started
    ```

## <a name="review-the-code"></a>De code bekijken

Deze stap is optioneel. Als u wilt weten hoe de databaseresources in de code worden gemaakt, kunt u de volgende codefragmenten bekijken. Sla dit anders over en ga naar [De app uitvoeren](#run-the-app). 

* Initialisatie van `AsyncDocumentClient`. De [AsyncDocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client) biedt een logische representatie aan de clientzijde voor de databaseservice van Azure Cosmos DB. Deze client wordt gebruikt om aanvragen aan de service te configureren en uitvoeren.

    ```java
    client = new AsyncDocumentClient.Builder()
             .withServiceEndpoint(YOUR_COSMOS_DB_ENDPOINT)
             .withMasterKeyOrResourceToken(YOUR_COSMOS_DB_MASTER_KEY)
             .withConnectionPolicy(ConnectionPolicy.GetDefault())
             .withConsistencyLevel(ConsistencyLevel.Eventual)
             .build();
    ```

* [Database](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._database) maken.

    ```java
    Database databaseDefinition = new Database();
    databaseDefinition.setId(databaseName);
    
    client.createDatabase(databaseDefinition, null)
            .toCompletable()
            .await();
    ```

* [DocumentCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._document_collection) maken.

    ```java
    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.setId(collectionName);

    //...

    client.createCollection(databaseLink, collectionDefinition, requestOptions)
            .toCompletable()
            .await();
    ```

* Documenten maken met behulp van de methode [createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._document).

    ```java
    // Any Java object within your code
    // can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    client.createDocument(collectionLink, family, null, true)
            .toCompletable()
            .await();

    ```

* SQL-query's via JSON worden uitgevoerd met behulp van de methode [queryDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querydocuments?view=azure-java-stable).

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);
    queryOptions.setMaxDegreeOfParallelism(-1);

    String collectionLink = String.format("/dbs/%s/colls/%s",
            databaseName,
            collectionName);
    Iterator<FeedResponse<Document>> it = client.queryDocuments(
            collectionLink,
            "SELECT * FROM Family WHERE Family.lastName = 'Andersen'",
            queryOptions).toBlocking().getIterator();

    System.out.println("Running SQL query...");
    while (it.hasNext()) {
        FeedResponse<Document> page = it.next();
        System.out.println(
                String.format("\tRead a page of results with %d items",
                        page.getResults().size()));
        for (Document doc : page.getResults()) {
            System.out.println(String.format("\t doc %s", doc));
        }
    }
    ```    

## <a name="run-the-app"></a>De app uitvoeren

Ga nu terug naar de Azure-portal om de verbindingsreeksgegevens op te halen en start de app met uw eindpuntgegevens. Hierdoor kan de app communiceren met de gehoste database.


1. Ga met de opdracht `cd` in het git-terminalvenster naar de map met voorbeeldcode.

    ```bash
    cd azure-cosmos-db-sql-api-async-java-getting-started/azure-cosmosdb-get-started
    ```

2. Gebruik in het git-terminalvenster de volgende opdracht om de vereiste Java-pakketten te installeren.

    ```bash
    mvn package
    ```

3. Start in het git-terminalvenster met de volgende opdracht de Java-toepassing (vervang YOUR_COSMOS_DB_HOSTNAME door de URI-waarde van de portal tussen aanhalingstekens, en vervang YOUR_COSMOS_DB_MASTER_KEY door de primaire sleutel van de portal tussen aanhalingstekens).

    ```bash
    mvn exec:java -DACCOUNT_HOST=YOUR_COSMOS_DB_HOSTNAME -DACCOUNT_KEY=YOUR_COSMOS_DB_MASTER_KEY

    ```

    Het terminalvenster geeft een melding dat de database FamilyDB is gemaakt. 
    
4. Druk op een toets om de database te maken en druk vervolgens op een andere toets om de verzameling te maken. 

    Ga terug naar Data Explorer in uw browser als u wilt controleren of het nu een FamilyDB-database en FamilyCollection-verzameling bevat.

5. Ga naar het consolevenster en druk op een toets om het eerste document te maken en druk vervolgens op een andere toets om het tweede document te maken. Ga vervolgens terug naar Data Explorer om ze weer te geven. 

6. Druk op een toets om een query uit te voeren en bekijk de uitvoer in het consolevenster. 

7. De resources die zijn gemaakt, worden niet verwijderd door de app. Ga terug naar de portal om [de resources te verwijderen](#clean-up-resources).  uit uw account zodat u daarvoor geen kosten in rekening worden gebracht.

    ![Console-uitvoer](./media/create-sql-api-java/rxjava-console-output.png)


## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u een Azure Cosmos DB-account, documentdatabase en verzameling kunt maken met behulp van Data Explorer, en hoe u een app kunt uitvoeren die hetzelfde doet via een programma. Nu kunt u aanvullende gegevens in uw Microsoft Azure Cosmos DB-verzameling importeren. 

> [!div class="nextstepaction"]
> [Gegevens importeren in Azure Cosmos DB](import-data.md)
