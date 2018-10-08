---
title: Voorbeeldgegevens laden in een Azure Cosmos DB Cassandra-API-tabel met behulp van een Java-toepassing | Microsoft Docs
description: Dit artikel laat zien hoe voorbeeldgegevens van een gebruiker worden geladen in een tabel van een Azure Cosmos DB Cassandra-API-account met behulp van een Java-toepassing.
services: cosmos-db
author: kanshiG
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 662d4b8812ca4b92c1130b9c2c38771e7ec30a06
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393983"
---
# <a name="load-sample-data-into-an-azure-cosmos-db-cassandra-api-table"></a>Voorbeeldgegevens laden in een Azure Cosmos DB Cassandra-API-tabel

Deze zelfstudie laat zien hoe voorbeeldgegevens van een gebruiker worden geladen in een tabel van een Azure Cosmos DB Cassandra-API-account met behulp van een Java-toepassing. De Java-toepassing maakt gebruik van het [Java-stuurprogramma](https://github.com/datastax/java-driver) en laadt gebruikersgegevens zoals gebruikers-id, gebruikersnaam, woonplaats van de gebruiker. 

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Gegevens laden in Cassandra-tabel
> * De app uitvoeren

## <a name="prerequisites"></a>Vereisten

* Dit artikel maakt deel uit van een meerdelige zelfstudie. Voordat u met dit document begint, moet u eerst [de Cassandra-API-account, de keyspace en de tabel maken](create-cassandra-api-account-java.md).   

## <a name="load-data-into-the-table"></a>Gegevens laden in de tabel

Open het bestand 'UserRepository.java' in de map 'src\main\java\com\azure\cosmosdb\cassandra' en voeg de code toe om de velden user_id, user_name en user_bcity in de tabel in te voegen:

```java
/**
* Insert a row into user table
*
* @param id   user_id
* @param name user_name
* @param city user_bcity
*/
public void insertUser(PreparedStatement statement, int id, String name, String city) {
        BoundStatement boundStatement = new BoundStatement(statement);
        session.execute(boundStatement.bind(id, name, city));
}

/**
* Create a PrepareStatement to insert a row to user table
*
* @return PreparedStatement
*/
public PreparedStatement prepareInsertStatement() {
    final String insertStatement = "INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (?,?,?)";
    return session.prepare(insertStatement);
}
```
 
Open het bestand 'UserProfile.java' in de map 'src\main\java\com\azure\cosmosdb\cassandra'. Deze klasse bevat de belangrijkste methode waarmee de methoden createKeyspace en createTable die u eerder hebt gedefinieerd, worden aangeroepen. Voeg nu de volgende code toe om wat voorbeeldgegevens in te voegen in de Cassandra-API-tabel.

```java
//Insert rows into user table
PreparedStatement preparedStatement = repository.prepareInsertStatement();
    repository.insertUser(preparedStatement, 1, "JohnH", "Seattle");
    repository.insertUser(preparedStatement, 2, "EricK", "Spokane");
    repository.insertUser(preparedStatement, 3, "MatthewP", "Tacoma");
    repository.insertUser(preparedStatement, 4, "DavidA", "Renton");
    repository.insertUser(preparedStatement, 5, "PeterS", "Everett");
```

## <a name="run-the-app"></a>De app uitvoeren

Open een opdrachtprompt of terminalvenster en wijzig het mappad in het pad waar u het project hebt gemaakt. Voer de 'mvn clean install'-opdracht uit om het bestand cosmosdb-cassandra-examples.jar in de doelmap te genereren en voer de toepassing uit. 

```bash
cd "cassandra-demo"

mvn clean install

java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
```

U kunt nu Data Explorer openen in de Azure-portal openen om te bevestigen dat de gegevens van de gebruiker aan de tabel worden toegevoegd.
    
## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u voorbeeldgegevens in de Azure Cosmos DB Cassandra-API-account kunt laden. U kunt nu verdergaan met het volgende artikel:

> [!div class="nextstepaction"]
> [Gegevens uit de Cassandra-API-account opvragen](cassandra-api-query-data.md)
