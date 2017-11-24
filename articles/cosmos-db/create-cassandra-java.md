---
title: 'Snelstartgids: Cassandra API met behulp van Java - Azure Cosmos DB | Microsoft Docs'
description: Deze snelstartgids wordt gedemonstreerd hoe met de Azure-API voor Cassandra Cosmos DB een profiel-toepassing maken met de Azure-portal en Java
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
ms.topic: article
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: 2097aa1c158f88a06ab93123f4e374b4245430d6
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="quickstart-build-a-cassandra-app-with-java-and-azure-cosmos-db"></a>Snelstartgids: Een app Cassandra met Java en Azure Cosmos DB bouwen

Deze snelstartgids laat zien hoe u Java en de Azure DB die Cosmos [Cassandra API](cassandra-introduction.md) voor het bouwen van een app profiel door het klonen van een voorbeeld van GitHub. Deze snelstartgids leidt u door het maken van een Azure DB die Cosmos-account via het web gebaseerde Azure-portal.

Azure Cosmos-database is de service van Microsoft wereldwijd gedistribueerde database voor meerdere model. U kunt snel maken en query document, tabel, sleutel / waarde- en grafiek databases, die allemaal van de algemene distributie en de mogelijkheden van de horizontale schaal de kern van Azure Cosmos DB profiteren. 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]U kunt ook [gratis Azure Cosmos DB probeer](https://azure.microsoft.com/try/cosmosdb/) gratis verbintenissen en zonder een Azure-abonnement.

Toegang tot de API van Azure Cosmos DB Cassandra preview-programma. Als u nog niet voor toegang tot toegepast [nu aanmelden](cassandra-introduction.md#sign-up-now).

Daarnaast doet u het volgende: 

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Voer op Ubuntu `apt-get install default-jdk` uit om de JDK te installeren.
    * Zorg dat de omgevingsvariabele JAVA_HOME verwijst naar de map waarin de JDK is geïnstalleerd.
* [Download](http://maven.apache.org/download.cgi) en [installeer](http://maven.apache.org/install.html) een binair [Maven](http://maven.apache.org/)-archief
    * Op Ubuntu kunt u `apt-get install maven` uitvoeren om Maven te installeren.
* [Git](https://www.git-scm.com/)
    * Op Ubuntu kunt u `sudo apt-get install git` uitvoeren om Git te installeren.



## <a name="create-a-database-account"></a>Een databaseaccount maken

Voordat u een documentdatabase maken kunt, moet u een Cassandra-account maken met Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Nu gaan we werken met code. We gaan een app Cassandra vanuit GitHub, de verbindingsreeks instellen en voer dit klonen. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een git-terminalvenster zoals git bash, en gebruik de `cd` opdracht om te wijzigen naar een map voor het installeren van de voorbeeld-app. 

    ```bash
    cd "C:\git-samples"
    ```

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt u een kopie van de voorbeeld-app op uw computer.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started.git
    ```

## <a name="review-the-code"></a>De code bekijken

Deze stap is optioneel. Als u wilt weten hoe de databaseresources worden gemaakt in de code bent, kunt u de volgende codefragmenten bekijken. Anders kunt u verder gaan naar [bijwerken van de verbindingsreeks](#update-your-connection-string). Deze fragmenten worden genomen van de src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java.  

* Cassandra host, poort, gebruikersnaam, wachtwoord en SSL-opties worden ingesteld. De verbindingsinformatie komt uit de pagina van de tekenreeks verbinding in de Azure portal.

   ```java
   cluster = Cluster.builder().addContactPoint(cassandraHost).withPort(cassandraPort).withCredentials(cassandraUsername, cassandraPassword).withSSL(sslOptions).build();
   ```

* De `cluster` maakt verbinding met de Azure-API voor Cassandra Cosmos DB en retourneert een sessie voor toegang tot.

    ```java
    return cluster.connect();
    ```

De volgende codefragmenten zijn uit het bestand src/main/java/com/azure/cosmosdb/cassandra/repository/UserRepository.java.

* Maak een nieuwe keyspace.

    ```java
    public void createKeyspace() {
        final String query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '3' } ";
        session.execute(query);
        LOGGER.info("Created keyspace 'uprofile'");
    }
    ```

* Een nieuwe tabel maken.

   ```java
   public void createTable() {
        final String query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)";
        session.execute(query);
        LOGGER.info("Created table 'user'");
   }
   ```

* Gebruiker entiteiten met een voorbereide instructie-object worden ingevoegd.

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

* De query voor het ophalen van alle gebruikersgegevens.

    ```java
   public void selectAllUsers() {
        final String query = "SELECT * FROM uprofile.user";
        List<Row> rows = session.execute(query).all();

        for (Row row : rows) {
            LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
        }
    }
    ```

* Query uitvoeren voor een enkele gebruiker informatie.

    ```java
    public void selectUser(int id) {
        final String query = "SELECT * FROM uprofile.user where user_id = 3";
        Row row = session.execute(query).one();

        LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
    }
    ```

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app. Hierdoor kan uw app kan communiceren met uw gehoste-database.

1. In de [Azure-portal](http://portal.azure.com/), klikt u op **verbindingsreeks**. 

    ![Bekijken en een gebruikersnaam van de Azure portal, verbindingsreeks pagina kopiëren](./media/create-cassandra-java/keys.png)

2. Gebruik de ![Knop Kopiëren](./media/create-cassandra-java/copy.png) knop aan de rechterkant van het scherm om de waarde van de punt van CONTACT te kopiëren.

3. Open de `config.properties` bestand vanuit de map C:\git-samples\azure-cosmosdb-cassandra-java-getting-started\java-examples\src\main\resources. 

3. De waarde van het punt van CONTACT vanuit de portal via plakken `<Cassandra endpoint host>` op regel 2.

    Regel 2 van config.properties nu zijn vergelijkbaar met 

    `cassandra_host=cosmos-db-quickstarts.documents.azure.com`

3. Ga terug naar de portal en kopieer de waarde van de gebruikersnaam. Na de waarde van de gebruikersnaam van de portal via `<cassandra endpoint username>` op regel 4.

    Regel 4 van config.properties nu zijn vergelijkbaar met 

    `cassandra_username=cosmos-db-quickstart`

4. Ga terug naar de portal en kopieer de waarde van het wachtwoord. De waarde van het wachtwoord van de portal via plakken `<cassandra endpoint password>` op regel 5.

    Regel 5 van config.properties nu zijn vergelijkbaar met 

    `cassandra_password=2Ggkr662ifxz2Mg...==`

5. Op de regel 6, als u een specifieke SSL-certificaat gebruiken wilt, vervangt `<SSL key store file location>` met de locatie van het SSL-certificaat. Als een waarde niet is opgegeven, wordt het JDK certificaat geïnstalleerd op < JAVA_HOME >/jre/lib/beveiliging/cacerts gebruikt. 

6. Als u gewijzigd regel 6 een specifieke SSL-certificaat te gebruiken, bijwerken regel 7 het wachtwoord voor dat certificaat gebruiken. 

7. Sla het bestand config.properties.

## <a name="run-the-app"></a>De app uitvoeren

1. In het terminalvenster git `cd` naar de map azure-cosmosdb-cassandra-java-getting-started\java-examples.

    ```git
    cd "C:\git-samples\azure-cosmosdb-cassandra-java-getting-started\java-examples"
    ```

2. Gebruik de volgende opdracht voor het genereren van het bestand cosmosdb-cassandra-examples.jar in het venster van de git-terminal.

    ```git
    mvn clean install
    ```

3. Voer de volgende opdracht om de Java-toepassing te starten in het venster van de git-terminal.

    ```git
    java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
    ```

    Het terminalvenster weergegeven meldingen de keyspace en de tabel worden gemaakt. Vervolgens selecteert en retourneert alle gebruikers in de tabel en de uitvoer wordt weergegeven, en vervolgens selecteert een rij door id en wordt de waarde weergegeven.  

    Druk op CTRL + c drukken om te stoppen exection van het programma en sluit het consolevenster. 
    
    U kunt nu Data Explorer openen in de Azure portal om te zien van de query, wijzigen en werken met deze nieuwe gegevens. 

    ![De gegevens in Data Explorer weergeven](./media/create-cassandra-java/data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe een Azure DB die Cosmos-account, Cassandra-database en verzameling met de gegevensverkenner maken en uitvoeren van een app als u wilt dit ook doen via een programma. U kunt nu aanvullende gegevens importeren in uw Azure DB die Cosmos-verzameling. 

> [!div class="nextstepaction"]
> [Cassandra gegevens importeren in Azure Cosmos-DB](cassandra-import-data.md)
