---
title: 'Quickstart: Cassandra-API met Java - Azure Cosmos DB | Microsoft Docs'
description: In deze quickstart ziet u hoe u de Cassandra-API in Azure Cosmos DB gebruikt om een profieltoepassing te maken met Azure Portal en Java
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: 
ms.assetid: ef611081-0195-4ad8-9b54-b313588e5754
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: 4d12fe3890b1d3190af1bc94eba7b93059a03f10
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="quickstart-build-a-cassandra-app-with-java-and-azure-cosmos-db"></a>Quickstart: Een Cassandra-app bouwen met Java en Azure Cosmos DB

Deze quickstart laat zien hoe u Java en de [Cassandra-API](cassandra-introduction.md) van Azure Cosmos DB gebruikt om een profiel-app te maken door een voorbeeld uit GitHub te klonen. Deze quickstart begeleidt u ook bij het maken van een Azure Cosmos DB-account via Azure Portal op het web.

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query's op uitvoeren. Deze databases genieten allemaal het voordeel van de wereldwijde distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] [Probeer Azure Cosmos DB gratis uit](https://azure.microsoft.com/try/cosmosdb/) zonder Azure-abonnement, zonder kosten en zonder verplichtingen.

Toegang tot het preview-programma van de Cassandra-API van Azure Cosmos DB. Als u nog geen toegang hebt aangevraagd, [meldt u zich nu aan](cassandra-introduction.md#sign-up-now).

Daarnaast doet u het volgende: 

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Voer op Ubuntu `apt-get install default-jdk` uit om de JDK te installeren.
    * Zorg dat de omgevingsvariabele JAVA_HOME verwijst naar de map waarin de JDK is geïnstalleerd.
* [Download](http://maven.apache.org/download.cgi) en [installeer](http://maven.apache.org/install.html) een binair [Maven](http://maven.apache.org/)-archief
    * Op Ubuntu kunt u `apt-get install maven` uitvoeren om Maven te installeren.
* [Git](https://www.git-scm.com/)
    * Op Ubuntu kunt u `sudo apt-get install git` uitvoeren om Git te installeren.



## <a name="create-a-database-account"></a>Een databaseaccount maken

Voordat u een documentdatabase kunt maken, moet u een Cassandra-account maken met Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Nu gaan we werken met code. We gaan een Cassandra-app klonen vanuit GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een git-terminalvenster, bijvoorbeeld git bash, en gebruik de opdracht `cd` om naar een map te gaan voor het installeren van de voorbeeld-app. 

    ```bash
    cd "C:\git-samples"
    ```

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt een kopie van de voorbeeld-app op uw computer.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started.git
    ```

## <a name="review-the-code"></a>De code bekijken

Deze stap is optioneel. Als u wilt weten hoe de databaseresources in de code worden gemaakt, kunt u de volgende codefragmenten bekijken. Als u deze stap wilt overslaan, kunt u verdergaan naar [Uw verbindingsreeks bijwerken](#update-your-connection-string). Deze fragmenten zijn allemaal afkomstig uit src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java.  

* De Cassandra-opties voor de host, poort, gebruikersnaam, het wachtwoord en SSL zijn ingesteld. De vereiste verbindingsreeksinformatie is afkomstig van de pagina Verbindingsreeks in Azure Portal.

   ```java
   cluster = Cluster.builder().addContactPoint(cassandraHost).withPort(cassandraPort).withCredentials(cassandraUsername, cassandraPassword).withSSL(sslOptions).build();
   ```

* Het `cluster` maakt verbinding met de Cassandra-API van Azure Cosmos DB en retourneert een sessie voor het verkrijgen van toegang.

    ```java
    return cluster.connect();
    ```

De volgende codefragmenten zijn afkomstig uit het bestand src/main/java/com/azure/cosmosdb/cassandra/repository/UserRepository.java file.

* Maak een nieuwe keyspace.

    ```java
    public void createKeyspace() {
        final String query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '3' } ";
        session.execute(query);
        LOGGER.info("Created keyspace 'uprofile'");
    }
    ```

* Maak een nieuwe tabel.

   ```java
   public void createTable() {
        final String query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)";
        session.execute(query);
        LOGGER.info("Created table 'user'");
   }
   ```

* Voeg gebruikersentiteiten in met een voorbereid instructieobject.

    ```java
    public PreparedStatement prepareInsertStatement() {
        final String insertStatement = "INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (?,?,?)";
        return session.prepare(insertStatement);
    }

    public void insertUser(PreparedStatement statement, int id, String name, String city) {
        BoundStatement boundStatement = new BoundStatement(statement);
        session.execute(boundStatement.bind(id, name, city));
    }
    ```

* Voer een query uit voor het ophalen van alle gebruikersgegevens.

    ```java
   public void selectAllUsers() {
        final String query = "SELECT * FROM uprofile.user";
        List<Row> rows = session.execute(query).all();

        for (Row row : rows) {
            LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
        }
    }
    ```

* Voer een query uit voor het ophalen van de gebruikersgegevens van één gebruiker.

    ```java
    public void selectUser(int id) {
        final String query = "SELECT * FROM uprofile.user where user_id = 3";
        Row row = session.execute(query).one();

        LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
    }
    ```

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app. Hierdoor kan de app communiceren met de gehoste database.

1. Klik in [Azure Portal](http://portal.azure.com/) op **Verbindingsreeks**. 

    ![Een gebruikersnaam bekijken en kopiëren via de pagina Verbindingsreeks in Azure Portal](./media/create-cassandra-java/keys.png)

2. Gebruik de ![knop Kopiëren](./media/create-cassandra-java/copy.png) aan de rechterkant van het scherm om de CONTACT POINT-waarde te kopiëren.

3. Open het bestand `config.properties` vanuit de map C:\git-samples\azure-cosmosdb-cassandra-java-getting-started\java-examples\src\main\resources. 

3. Plak de CONTACT POINT-waarde uit de portal over `<Cassandra endpoint host>` op regel 2 heen.

    Regel 2 van config.properties moet er nu als volgt uitzien 

    `cassandra_host=cosmos-db-quickstarts.documents.azure.com`

3. Ga terug naar de portal en kopieer de USERNAME-waarde. Plak de USERNAME-waarde uit de portal over `<cassandra endpoint username>` op regel 4 heen.

    Regel 4 van config.properties moet er nu als volgt uitzien 

    `cassandra_username=cosmos-db-quickstart`

4. Ga terug naar de portal en kopieer de PASSWORD-waarde. Plak de PASSWORD-waarde uit de portal over `<cassandra endpoint password>` op regel 5 heen.

    Regel 5 van config.properties moet er nu als volgt uitzien 

    `cassandra_password=2Ggkr662ifxz2Mg...==`

5. Als u wilt dat er een specifiek SSL-certificaat wordt gebruikt, vervangt u op regel 6 `<SSL key store file location>` door de locatie van het SSL-certificaat. Als er geen waarde wordt opgegeven, wordt het JDK certificaat gebruikt dat in <JAVA_HOME>/jre/lib/security/cacerts is geïnstalleerd. 

6. Als u regel 6 hebt gewijzigd omdat u wilt dat er een specifiek SSL-certificaat wordt gebruikt, werkt u regel 7 bij, zodat het wachtwoord voor dat certificaat wordt gebruikt. 

7. Sla het bestand config.properties op.

## <a name="run-the-app"></a>De app uitvoeren

1. `cd` in het git-terminalvenster naar de map azure-cosmosdb-cassandra-java-getting-started\java-examples.

    ```git
    cd "C:\git-samples\azure-cosmosdb-cassandra-java-getting-started\java-examples"
    ```

2. Gebruik de volgende opdracht in het git-terminalvenster om het bestand cosmosdb-cassandra-examples.jar te genereren.

    ```git
    mvn clean install
    ```

3. Voer in het git-terminalvenster de volgende opdracht uit om de Java-toepassing te starten.

    ```git
    java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
    ```

    In het terminalvenster worden meldingen weergegeven dat de keyspace en de tabel zijn gemaakt. Vervolgens worden gebruikers in de tabel geselecteerd en geretourneerd en wordt de uitvoer weergegeven, waarna een rij per id wordt geselecteerd en de waarde wordt weergegeven.  

    Druk op Ctrl+C om de uitvoering van het programma te stoppen en het consolevenster te sluiten. 
    
    U kunt nu Data Explorer openen in Azure Portal om deze nieuwe gegevens te bekijken, te wijzigen, een query erop uit te voeren of er iets anders mee te doen. 

    ![De gegevens bekijken in Data Explorer](./media/create-cassandra-java/data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u geleerd hoe u een Azure Cosmos DB-account, Cassandra-database en verzameling kunt maken met behulp van Data Explorer, en hoe u een app kunt uitvoeren die hetzelfde doet via een programma. Nu kunt u aanvullende gegevens in uw Microsoft Azure Cosmos DB-verzameling importeren. 

> [!div class="nextstepaction"]
> [Cassandra-gegevens importeren in Azure Cosmos DB](cassandra-import-data.md)
