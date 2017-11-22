---
title: Een Azure Cosmos DB-documentdatabase maken met Java | Microsoft Docs | Microsoft Docs'
description: "Biedt een voorbeeld van Java-code dat u kunt gebruiken om verbinding te maken met de DocumentDB API van Azure Cosmos DB en er query’s op uit te voeren"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect, mvc, devcenter
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 11/20/2017
ms.author: mimig
ms.openlocfilehash: b36de6bce597569b4e1eaa615860acdf28dfa798
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2017
---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-the-azure-portal"></a>Azure Cosmos DB: een document maken met behulp van Java en Azure Portal

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. Met behulp van Azure Cosmos DB, kunt u snel maken en query beheerd document, de tabel en de databases van de grafiek.

In deze snelstartgids wordt een documentdatabase gemaakt met behulp van de hulpprogramma's voor Azure Cosmos DB in Azure Portal. In deze snelstartgids leest u ook hoe u snel een Java-console-app kunt maken via de [DocumentDB Java-API](documentdb-sdk-java.md). De instructies in deze snelstartgids kunnen worden uitgevoerd in elk besturingssysteem waarmee Java kan worden uitgevoerd. Door het voltooien van deze snelstartgids zult u bekend bent met het maken en wijzigen van document databaseresources in de gebruikersinterface of via een programma, afhankelijk van wat is uw voorkeur.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Daarnaast doet u het volgende: 

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Voer op Ubuntu `apt-get install default-jdk` uit om de JDK te installeren.
    * Zorg dat de omgevingsvariabele JAVA_HOME verwijst naar de map waarin de JDK is geïnstalleerd.
* [Download](http://maven.apache.org/download.cgi) en [installeer](http://maven.apache.org/install.html) een binair [Maven](http://maven.apache.org/)-archief
    * Op Ubuntu kunt u `apt-get install maven` uitvoeren om Maven te installeren.
* [Git](https://www.git-scm.com/)
    * Op Ubuntu kunt u `sudo apt-get install git` uitvoeren om Git te installeren.

## <a name="create-a-database-account"></a>Een databaseaccount maken

Voordat u een documentdatabase kunt maken, moet u een SQL-databaseaccount (DocumentDB) maken met Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Een verzameling toevoegen

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Voorbeeldgegevens toevoegen

U kunt nu gegevens aan uw nieuwe verzameling toevoegen met behulp van Data Explorer.

1. Vouw de **Items** verzameling, klikt u op **documenten** > **Nieuw Document**.

   ![Nieuwe documenten maken in Data Explorer in de Azure Portal](./media/create-documentdb-java/azure-cosmosdb-data-explorer-new-document.png)
  
2. Nu een document toevoegen aan de verzameling met de volgende structuur en klik op **opslaan**.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

    ![JSON-gegevens kopiëren en op Opslaan klikken in Data Explorer in Azure Portal](./media/create-documentdb-java/azure-cosmosdb-data-explorer-save-document.png)

3.  Maak een meer document waarin u wijzigen en sla `id` op 2 en de overige eigenschappen zoals u ziet past wijzigen. De nieuwe documenten kunnen elke gewenste structuur hebben, omdat in Azure Cosmos DB uw gegevens geen schema krijgen opgelegd.

## <a name="query-your-data"></a>Uw gegevens opvragen

U kunt nu query's in Data Explorer gebruiken om te halen en filter uw gegevens.

1. Zie dat standaard de query is ingesteld op `SELECT * FROM c`. Deze standaardquery opgehaald en alle documenten in de verzameling weergegeven. 

    ![Standaard-query in Data Explorer is ' Selecteer * van c'](./media/create-documentdb-java/azure-cosmosdb-data-explorer-query.png)

2. De query wijzigen door te klikken op de **-Filter bewerken** knop, toe te voegen `ORDER BY c._ts DESC` aan de query-predicaat in en vervolgens klikken op **Filter toepassen**.

    ![De standaardquery wijzigen door ORDER BY c._ts DESC toe te voegen en te klikken op Filter toepassen](./media/create-documentdb-java/azure-cosmosdb-data-explorer-edit-query.png)

Dit gewijzigd query lijsten documenten in aflopende volgorde op basis van hun tijdstempel, nu uw tweede document wordt eerst weergegeven. Als u bekend met SQL-syntaxis bent, kunt u een van de ondersteunde [SQL-query's](documentdb-sql-query.md) in dit vak. 

Onze werk in Data Explorer is voltooid. Voordat we verdergaan met het werken met code, houd er rekening mee dat u ook Data Explorer gebruiken kunt voor het maken van opgeslagen procedures, UDF's en triggers voor bedrijfslogica op de server uitvoeren, evenals een doorvoer te schalen. In Data Explorer wordt alle ingebouwde programmatische gegevenstoegang zichtbaar die beschikbaar is in de API's, maar biedt eenvoudige toegang tot uw gegevens in Azure Portal.

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Nu gaan we werken met code. We gaan nu een DocumentDB API-app klonen vanuit GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een opdrachtprompt, maak een nieuwe map met de naam git-samples en sluit de opdrachtprompt.

    ```bash
    md "C:\git-samples"
    ```

2. Open een git-terminalvenster zoals git bash, en gebruik de `cd` opdracht om te wijzigen naar de nieuwe map voor het installeren van de voorbeeld-app. 

    ```bash
    cd "C:\git-samples"
    ```

3. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt u een kopie van de voorbeeld-app op uw computer.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-the-code"></a>De code bekijken

Deze stap is optioneel. Als u wilt weten hoe de databaseresources worden gemaakt in de code bent, kunt u de volgende codefragmenten bekijken. De codefragmenten zijn afkomstig uit de `Program.java` bestand in de map C:\git-samples\azure-cosmos-db-documentdb-java-getting-started\src\GetStarted geïnstalleerd. Anders kunt u verder gaan naar [bijwerken van de verbindingsreeks](#update-your-connection-string). 

* `DocumentClient`de initialisatie. De [DocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client) logische representatie van de clientzijde voor de database-service van Azure DB die Cosmos biedt. Deze client wordt gebruikt om te configureren en uitvoeren van aanvragen op basis van de service.

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* [Database](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._database) maken.

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* [Documentcollection maakt](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_collection) maken.

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* Maken van het document met behulp van de [createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createdocument) methode.

    ```java
    // Any Java object within your code can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* SQL-query's via JSON worden uitgevoerd met behulp van de [queryDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) methode.

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    FeedResponse<Document> queryResults = this.client.queryDocuments(
        collectionLink,
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }
    ```    

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app. Hierdoor kan uw app kan communiceren met uw gehoste-database.

1. In de [Azure-portal](http://portal.azure.com/), klikt u op **sleutels**. 

    Gebruik de knoppen Kopieer aan de rechterkant van het scherm voor het kopiëren van de bovenste waarde, de URI.

    ![Weergeven en een toegangssleutel in de Azure portal, sleutels pagina kopiëren](./media/create-documentdb-java/keys.png)

2. Open de `Program.java` bestand vanuit de map C:\git-samples\azure-cosmos-db-documentdb-java-getting-started\src\GetStarted. 

3. Plak de URI-waarde van de portal via `https://FILLME.documents.azure.com` op regel 45.

4. Ga terug naar de portal en kopieer de primaire sleutel-waarde, zoals weergegeven in de schermafbeelding. De waarde van het primaire sleutel van de portal via plakken `FILLME` op regel 46.

    De methode getStartedDemo moet nu er ongeveer als volgt uitzien: 
    
    ```java
    private void getStartedDemo() throws DocumentClientException, IOException {
        this.client = new DocumentClient("https://youraccountname.documents.azure.com:443/",
                "your-primary-key...RJhQrqQ5QQ==", 
                new ConnectionPolicy(),
                ConsistencyLevel.Session);
    ```

5. Sla het bestand Program.java.

## <a name="run-the-app"></a>De app uitvoeren

1. `cd` in het git-terminalvenster naar de map azure-cosmos-db-documentdb-java-getting-started.

    ```git
    cd "C:\git-samples\azure-cosmos-db-documentdb-java-getting-started"
    ```

2. Gebruik de volgende opdracht om de vereiste Java-pakketten te installeren in het venster van de git-terminal.

    ```
    mvn package
    ```

3. Gebruik de volgende opdracht om de Java-toepassing te starten in het venster van de git-terminal.

    ```
    mvn exec:java -D exec.mainClass=GetStarted.Program
    ```

    Het terminalvenster geeft een melding dat de FamilyDB-database is gemaakt. 
    
4. Druk op een toets om de verzameling te maken. 

5. Ga terug naar de Data Explorer en u ziet dat deze nu een FamilyDB-database bevat.
    
6. Blijven toetsen in het consolevenster hebben de code-documenten maken en een query uit te voeren.
    
    Alle resources uit deze app worden verwijderd uit je account aan het einde van het programma, zodat u niet alle kosten. 

    ![Console-uitvoer](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u een Azure Cosmos DB-account, documentdatabase en verzameling kunt maken met behulp van Data Explorer, en hoe u een app kunt uitvoeren die hetzelfde doet via een programma. U kunt nu aanvullende gegevens importeren in uw Azure DB die Cosmos-verzameling. 

> [!div class="nextstepaction"]
> [Gegevens importeren in Azure Cosmos DB](import-data.md)


