---
title: Gegevens opvragen uit een Azure Cosmos DB Cassandra API-account
description: Dit artikel laat zien hoe u met behulp van een Java-toepassing gebruikersgegevens opvraagt uit een Cassandra API-account van Azure Cosmos DB.
services: cosmos-db
ms.service: cosmos-db
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: c1fb4c27f897e3c0952ed6419e167613ac8204f7
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223488"
---
# <a name="query-data-from-an-azure-cosmos-db-cassandra-api-account"></a>Gegevens opvragen uit een Azure Cosmos DB Cassandra API-account

Deze zelfstudie laat zien hoe u met behulp van een Java-toepassing gebruikersgegevens opvraagt uit een Cassandra API-account van Azure Cosmos DB. De Java-toepassing maakt gebruik van het [Java-stuurprogramma](https://github.com/datastax/java-driver) en vraagt gebruikersgegevens op zoals gebruikers-id, gebruikersnaam, woonplaats van de gebruiker. 

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Gegevens opvragen uit Cassandra-tabel
> * De app uitvoeren

## <a name="prerequisites"></a>Vereisten

* Dit artikel maakt deel uit van een meerdelige zelfstudie. Begin pas met deze zelfstudie nadat u de stappen hebt voltooid voor [het maken van het Cassandra API-account, de keyspace en de tabel](create-cassandra-api-account-java.md) en nadat [de voorbeeldgegevens in de tabel zijn geladen](cassandra-api-load-data.md). 

## <a name="query-data"></a>Querygegevens

Open het bestand `UserRepository.java` in de map `src\main\java\com\azure\cosmosdb\cassandra`. Voeg het volgende codeblok toe aan het einde van het bestand. Deze code biedt drie functies: query uitvoeren op alle gebruikers in de database, query uitvoeren op een specifieke gebruiker gefilterd op gebruikers-id en een tabel verwijderen. 

```java
/**
* Select all rows from user table
*/
public void selectAllUsers() {

    final String query = "SELECT * FROM uprofile.user";
    List<Row> rows = session.execute(query).all();

    for (Row row : rows) {
       LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
    }
}

/**
* Select a row from user table
*
* @param id user_id
*/
public void selectUser(int id) {
    final String query = "SELECT * FROM uprofile.user where user_id = 3";
    Row row = session.execute(query).one();

    LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
}

/**
* Delete user table.
*/
public void deleteTable() {
   final String query = "DROP TABLE IF EXISTS uprofile.user";
   session.execute(query);
}
```

Open het bestand `UserProfile.java` in de map `src\main\java\com\azure\cosmosdb\cassandra`. Deze klasse bevat de main-methode die de methoden createKeyspace en createTable aanroept, de methoden voor het invoegen van gegevens die u eerder hebt gedefinieerd. Voeg nu de volgende code toe om een query uit te voeren op alle gebruikers of een specifieke gebruiker:

```java
LOGGER.info("Select all users");
repository.selectAllUsers();

LOGGER.info("Select a user by id (3)");
repository.selectUser(3);

LOGGER.info("Delete the users profile table");
repository.deleteTable();
```

## <a name="run-the-java-app"></a>De Java-app uitvoeren
1. Open een opdrachtprompt of terminalvenster. Plak het volgende blok met code. 

   Met deze code gaat u naar de map (cd) waarin u het project hebt gemaakt. Vervolgens wordt de opdracht `mvn clean install` uitgevoerd voor het genereren van het bestand `cosmosdb-cassandra-examples.jar` in de doelmap. Ten slotte wordt de Java-toepassing uitgevoerd.

   ```bash
   cd "cassandra-demo"
   
   mvn clean install
   
   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
   ```

2. Open nu **Data Explorer** in de Azure-portal en controleer of de gebruikerstabel is verwijderd.

## <a name="next-steps"></a>Volgende stappen

* In deze zelfstudie hebt u geleerd hoe u een query kunt uitvoeren op gegevens in een Azure Cosmos DB Cassandra API-account. U kunt nu verdergaan met het volgende artikel:

> [!div class="nextstepaction"]
> [Gegevens migreren naar Cassandra API-account](cassandra-import-data.md)


