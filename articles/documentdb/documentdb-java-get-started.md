---
title: 'NoSQL-zelfstudie: Azure DocumentDB Java SDK | Microsoft Docs'
description: Een NoSQL-zelfstudie waarmee u een online database en een Java-consoletoepassing maakt met de DocumentDB Java SDK. Azure DocumentDB is een NoSQL-database voor JSON.
keywords: nosql zelfstudie, onlinedatabase, java-consoletoepassing
services: documentdb
documentationcenter: Java
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 75a9efa1-7edd-4fed-9882-c0177274cbb2
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 01/05/2017
ms.author: arramac
translationtype: Human Translation
ms.sourcegitcommit: ddd676df429c20d1c07cfe64abc9ab69ef11bd8c
ms.openlocfilehash: 845858c3df6456293a2552f55ffb35254024931b


---
# <a name="nosql-tutorial-build-a-documentdb-java-console-application"></a>NoSQL-zelfstudie: een DocumentDB Java-consoletoepassing maken
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Welkom bij de NoSQL-zelfstudie over de Azure DocumentDB Java SDK. Wanneer u deze zelfstudie hebt voltooid, beschikt u over een consoletoepassing waarmee u DocumentDB-resources kunt maken en er query's op kunt uitvoeren.

We behandelen de volgende onderwerpen:

* Een DocumentDB-account maken en er verbinding mee maken
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

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/). U kunt voor deze zelfstudie ook de [Azure DocumentDB-emulator](documentdb-nosql-local-emulator.md) gebruiken.
* [Git](https://git-scm.com/downloads)
* [Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-a-documentdb-account"></a>Stap 1: een DocumentDB-account maken
U maakt om te beginnen een DocumentDB-account. Als u al een account hebt dat u wilt gebruiken, kunt u verder naar de stap [Het GitHub-project klonen](#GitClone). Als u de DocumentDB-emulator gebruikt, volgt u de stappen in [Azure DocumentDB-emulator](documentdb-nosql-local-emulator.md) om de emulator in te stellen en meteen naar [Het GitHub-project klonen](#GitClone) te gaan.

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a name="a-idgitcloneastep-2-clone-the-github-project"></a><a id="GitClone"></a>Stap 2: het GitHub-project klonen
Begin met de GitHub-opslagplaats te klonen voor [Get Started with DocumentDB and Java](https://github.com/Azure-Samples/documentdb-java-getting-started) (Aan de slag met DocumentDB en Java). Voer bijvoorbeeld vanuit een lokale map het volgende uit als u het voorbeeldproject lokaal wilt ophalen.

    git clone git@github.com:Azure-Samples/documentdb-java-getting-started.git

    cd documentdb-java-getting-started

De map bevat een `pom.xml` voor het project en een `src`-map met Java-broncode, waaronder `Program.java`. Deze toont hoe u eenvoudige bewerkingen kunt uitvoeren met Azure DocumentDB zoals documenten maken en query's toepassen op gegevens binnen een verzameling. De `pom.xml` bevat een afhankelijkheid van de [DocumentDB Java SDK op Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a name="a-idconnectastep-3-connect-to-a-documentdb-account"></a><a id="Connect"></a>Stap 3: verbinding maken met een DocumentDB-account
Ga vervolgens terug naar [Azure Portal](https://portal.azure.com) om uw eindpunt en primaire hoofdsleutel op te halen. Uw toepassing heeft het DocumentDB-eindpunt en de primaire sleutel nodig om te weten waarmee verbinding moet worden gemaakt en om ervoor te zorgen dat DocumentDB de verbinding van uw toepassing vertrouwt.

Navigeer in de Azure Portal naar uw DocumentDB-account en klik daarna op **Keys**. Kopieer in de portal de URI en plak deze in `<your endpoint URI>` in het bestand Program.java. Kopieer vervolgens de PRIMAIRE SLEUTEL van de portal en plak deze in `<your key>`.

    this.client = new DocumentClient(
        "<your endpoint URI>",
        "<your key>"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Schermopname van Azure Portal die voor de NoSQL-zelfstudie wordt gebruikt om een Java-consoletoepassing te maken. Schermopname van Azure Portal waarin een DocumentDB-account wordt weergegeven met de hub ACTIEF gemarkeerd. Verder is de knop SLEUTELS gemarkeerd op de DocumentDB-accountblade en zijn de waarden URI, PRIMAIRE SLEUTEL en SECUNDAIRE SLEUTEL gemarkeerd op de blade Sleutels.][keys]

## <a name="step-4-create-a-database"></a>Stap 4: een database maken
Uw DocumentDB-[database](documentdb-resources.md#databases) kan worden gemaakt met de methode [createDatabase](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createDatabase-com.microsoft.azure.documentdb.Database-com.microsoft.azure.documentdb.RequestOptions-) van de klasse **DocumentClient**. Een database is een logische container voor een JSON-documentopslag, gepartitioneerd in verzamelingen.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a name="a-idcreatecollastep-5-create-a-collection"></a><a id="CreateColl"></a>Stap 5: een verzameling maken
> [!WARNING]
> Met **createCollection** maakt u een nieuwe verzameling met gereserveerde doorvoer, wat gevolgen heeft voor de kosten. Zie onze [pagina met prijzen](https://azure.microsoft.com/pricing/details/documentdb/) voor meer informatie.
> 
> 

U kunt een [verzameling](documentdb-resources.md#collections) maken met de methode [createCollection](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createCollection-java.lang.String-com.microsoft.azure.documentdb.DocumentCollection-com.microsoft.azure.documentdb.RequestOptions-) van de klasse **DocumentClient**. Een verzameling is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // DocumentDB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a name="a-idcreatedocastep-6-create-json-documents"></a><a id="CreateDoc"></a>Stap 6: JSON-documenten maken
U kunt een [document](documentdb-resources.md#documents) maken met de methode [createDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createDocument-java.lang.String-java.lang.Object-com.microsoft.azure.documentdb.RequestOptions-boolean-) van de klasse **DocumentClient**. Documenten bestaan uit door gebruikers gedefinieerde (willekeurige) JSON-inhoud. U kunt nu een of meer documenten invoegen. Als u al gegevens hebt die u in de database wilt opslaan, kunt u het [hulpprogramma voor gegevensmigratie](documentdb-import-data.md) van DocumentDB gebruiken voor het importeren van gegevens in een database.

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

![Diagram waarin u de hiërarchische relatie ziet tussen het account, de online database, de verzameling en de documenten die in de NoSQL-zelfstudie worden gebruikt om een Java-consoletoepassing te maken](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a name="a-idqueryastep-7-query-documentdb-resources"></a><a id="Query"></a>Stap 7: een query uitvoeren op DocumentDB-resources
DocumentDB biedt ondersteuning voor uitgebreide [query's](documentdb-sql-query.md) op de JSON-documenten die zijn opgeslagen in elke verzameling.  De volgende voorbeeldcode laat zien hoe u query's kunt toepassen op documenten in DocumentDB met behulp van de SQL-syntaxis met de methode [queryDocuments](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#queryDocuments-java.lang.String-com.microsoft.azure.documentdb.SqlQuerySpec-com.microsoft.azure.documentdb.FeedOptions-).

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a name="a-idreplacedocumentastep-8-replace-json-document"></a><a id="ReplaceDocument"></a>Stap 8: JSON-document vervangen
DocumentDB ondersteunt het bijwerken van JSON-documenten met behulp van de methode [replaceDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#replaceDocument-com.microsoft.azure.documentdb.Document-com.microsoft.azure.documentdb.RequestOptions-).

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a name="a-iddeletedocumentastep-9-delete-json-document"></a><a id="DeleteDocument"></a>Stap 9: JSON-document verwijderen
DocumentDB ondersteunt ook het verwijderen van JSON-documenten met behulp van de methode [deleteDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#deleteDocument-java.lang.String-com.microsoft.azure.documentdb.RequestOptions-).  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a name="a-iddeletedatabaseastep-10-delete-the-database"></a><a id="DeleteDatabase"></a>Stap 10: de database verwijderen
Als u de gemaakte database verwijdert, worden de database en alle onderliggende resources (verzamelingen, documenten enz.) verwijderd.

    this.client.deleteDatabase("/dbs/familydb", null);

## <a name="a-idrunastep-11-run-your-java-console-application-all-together"></a><a id="Run"></a>Stap 11: uw Java-consoletoepassing volledig uitvoeren
Voordat u de toepassing vanaf de console kunt uitvoeren, dient u deze eerst te compileren met behulp van Maven:
    
    mvn package

Als u `mvn package` uitvoert, wordt de nieuwste DocumentDB-bibliotheek vanuit Maven gedownload en wordt `GetStarted-0.0.1-SNAPSHOT.jar` geproduceerd. Voer vervolgens de app uit door het volgende uit te voeren:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Gefeliciteerd. U hebt de NoSQL-zelfstudie voltooid en beschikt nu over een werkende Java-consoletoepassing.

## <a name="next-steps"></a>Volgende stappen
* Wilt u een zelfstudie voor Java-web-apps volgen? Zie [Een Java-web-app maken met DocumentDB](documentdb-java-application.md).
* Informatie over [het bewaken van een DocumentDB-account](documentdb-monitor-accounts.md).
* Voer query's uit op onze voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).
* Meer informatie over het programmeermodel vindt u in de sectie Ontwikkelen van de pagina [DocumentDB-documentatie](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md
[keys]: media/documentdb-get-started/nosql-tutorial-keys.png



<!--HONumber=Jan17_HO1-->


