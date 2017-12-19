---
title: 'NoSQL-zelfstudie: SQL-API voor Azure Cosmos DB Java SDK | Microsoft Docs'
description: Een NoSQL-zelfstudie waarmee u maakt een online database en Java-consoletoepassing met behulp van de SQL-API voor Azure Cosmos DB. Azure SQL is een NoSQL-database voor JSON.
keywords: nosql zelfstudie, onlinedatabase, java-consoletoepassing
services: cosmos-db
documentationcenter: Java
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 75a9efa1-7edd-4fed-9882-c0177274cbb2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 05/22/2017
ms.author: arramac
ms.openlocfilehash: 9714234411e96074daae17b4711a52991768bd7b
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="nosql-tutorial-build-a-sql-api-java-console-application"></a>NoSQL-zelfstudie: een SQL-API-Java-consoletoepassing bouwen
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Node.js voor MongoDB](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [C++](sql-api-cpp-get-started.md)
>  
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Welkom bij de NoSQL-zelfstudie voor de SQL-API voor Azure Cosmos DB Java SDK. Wanneer u deze zelfstudie hebt voltooid, beschikt u over een consoletoepassing waarmee u Azure Cosmos DB-resources kunt maken en er query's op kunt uitvoeren.

We behandelen de volgende onderwerpen:

* Een Azure Cosmos DB-account maken en er verbinding mee maken
* Uw Visual Studio-oplossing configureren
* Een online-database maken
* Een verzameling maken
* JSON-documenten maken
* Query's uitvoeren op de verzameling
* JSON-documenten maken
* Query's uitvoeren op de verzameling
* Een document vervangen
* Een document verwijderen
* De database verwijderen

Tijd om aan de slag te gaan.

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u over de volgende zaken beschikt:

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Git](https://git-scm.com/downloads).
* [Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Stap 1: een Azure Cosmos DB-account maken
Begin met het maken van een Azure Cosmos DB-account. Als u al een account hebt dat u wilt gebruiken, kunt u verder naar de stap [Het GitHub-project klonen](#GitClone). Als u de Azure Cosmos DB-emulator gebruikt, volgt u de stappen in [Azure Cosmos DB-emulator](local-emulator.md) om de emulator in te stellen en meteen naar [Het GitHub-project klonen](#GitClone) te gaan.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>Stap 2: het GitHub-project klonen
Kloon eerst de GitHub-opslagplaats voor [Get Started with Azure Cosmos DB and Java](https://github.com/Azure-Samples/documentdb-java-getting-started) (Aan de slag met Azure Cosmos DB en Java). Voer bijvoorbeeld vanuit een lokale map het volgende uit als u het voorbeeldproject lokaal wilt ophalen.

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

De map bevat een `pom.xml` voor het project en een `src` map met inbegrip van Java source code `Program.java` waaruit blijkt hoe eenvoudig bewerkingen uitvoeren met Azure Cosmos DB als het maken van documenten en gegevens binnen een verzameling opvragen . De `pom.xml` bevat een afhankelijkheid op de [Azure Cosmos DB Java SDK op Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a id="Connect"></a>Stap 3: verbinding maken met een Azure Cosmos DB-account
Ga vervolgens terug naar [Azure Portal](https://portal.azure.com) om uw eindpunt en primaire hoofdsleutel op te halen. Uw toepassing heeft het Azure Cosmos DB-eindpunt en de primaire sleutel nodig om te bepalen waarmee verbinding moet worden gemaakt en om ervoor te zorgen dat Azure Cosmos DB de verbinding van uw toepassing vertrouwt.

Navigeer in Azure Portal naar uw Azure Cosmos DB-account en klik daarna op **Sleutels**. Kopieer in de portal de URI en plak deze in `https://FILLME.documents.azure.com` in het bestand Program.java. Kopieer vervolgens de PRIMAIRE SLEUTEL van de portal en plak deze in `FILLME`.

    this.client = new DocumentClient(
        "https://FILLME.documents.azure.com",
        "FILLME"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Schermopname van Azure Portal die voor de NoSQL-zelfstudie wordt gebruikt om een Java-consoletoepassing te maken. Schermopname van Azure Portal waarin een Azure Cosmos DB-account wordt weergegeven met de hub ACTIEF gemarkeerd. Verder is de knop SLEUTELS gemarkeerd op de Azure Cosmos DB-accountblade en zijn de waarden URI, PRIMAIRE SLEUTEL en SECUNDAIRE SLEUTEL gemarkeerd op de blade Sleutels][keys]

## <a name="step-4-create-a-database"></a>Stap 4: een database maken
Uw Azure Cosmos DB-[database](sql-api-resources.md#databases) kan worden gemaakt met de methode [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) van de klasse **DocumentClient**. Een database is een logische container voor een JSON-documentopslag, gepartitioneerd in verzamelingen.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a id="CreateColl"></a>Stap 5: een verzameling maken
> [!WARNING]
> Met **createCollection** maakt u een nieuwe verzameling met gereserveerde doorvoer, wat gevolgen heeft voor de kosten. Zie onze [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie.
> 
> 

U kunt een [verzameling](sql-api-resources.md#collections) maken met de methode [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) van de klasse **DocumentClient**. Een verzameling is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a id="CreateDoc"></a>Stap 6: JSON-documenten maken
U kunt een [document](sql-api-resources.md#documents) maken met de methode [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) van de klasse **DocumentClient**. Documenten bestaan uit door gebruikers gedefinieerde (willekeurige) JSON-inhoud. U kunt nu een of meer documenten invoegen. Als u gegevens die u wilt opslaan in de database al hebt, kunt u Azure Cosmos DB [hulpprogramma voor gegevensmigratie](import-data.md) importeren van de gegevens in een database.

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![Diagram waarin u de hiërarchische relatie ziet tussen het account, de online database, de verzameling en de documenten die in de NoSQL-zelfstudie worden gebruikt om een Java-consoletoepassing te maken](./media/sql-api-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Stap 7: query's uitvoeren op Azure Cosmos DB-resources
Azure Cosmos DB biedt ondersteuning voor uitgebreide [query's](sql-api-sql-query.md) op de JSON-documenten die zijn opgeslagen in elke verzameling.  De volgende voorbeeldcode laat zien hoe u query's kunt toepassen op documenten in Azure Cosmos DB met behulp van de SQL-syntaxis met de methode [queryDocuments](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments).

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a id="ReplaceDocument"></a>Stap 8: JSON-document vervangen
Azure Cosmos DB ondersteunt het bijwerken van JSON-documenten met behulp van de methode [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument).

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a id="DeleteDocument"></a>Stap 9: JSON-document verwijderen
Azure Cosmos DB ondersteunt ook het verwijderen van JSON-documenten met behulp van de methode [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument).  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a id="DeleteDatabase"></a>Stap 10: de database verwijderen
Als u de gemaakte database verwijdert, worden de database en alle onderliggende resources (verzamelingen, documenten enz.) verwijderd.

    this.client.deleteDatabase("/dbs/familydb", null);

## <a id="Run"></a>Stap 11: uw Java-consoletoepassing volledig uitvoeren
Voor de toepassing uitvoeren vanaf de console, gaat u naar de projectmap en gecompileerd met behulp van Maven:
    
    mvn package

Als u `mvn package` uitvoert, wordt de nieuwste Azure Cosmos DB-bibliotheek vanuit Maven gedownload en wordt `GetStarted-0.0.1-SNAPSHOT.jar` geproduceerd. Voer vervolgens de app uit door het volgende uit te voeren:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Gefeliciteerd. U hebt de NoSQL-zelfstudie voltooid en beschikt nu over een werkende Java-consoletoepassing.

## <a name="next-steps"></a>Volgende stappen
* Wilt u een zelfstudie voor Java-web-apps volgen? Zie [Build a web application with Java using Azure Cosmos DB](sql-api-java-application.md) (Een Java-web-app maken met Azure Cosmos DB).
* Leer hoe het [bewaken van een Azure Cosmos DB-account](monitor-accounts.md) werkt.
* Voer query's uit op onze voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png
