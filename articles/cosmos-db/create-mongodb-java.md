---
title: 'Azure Cosmos DB: een console-app ontwikkelen met Java en de MongoDB-API | Microsoft Docs'
description: Biedt een voorbeeld van Java-code dat u kunt gebruiken om verbinding te maken met de MongoDB-API van Azure Cosmos DB en er query’s op uit te voeren
services: cosmos-db
author: slyons
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.custom: quick start connect, mvc
ms.devlang: java
ms.topic: quickstart
ms.date: 05/10/2017
ms.author: sclyon
ms.openlocfilehash: ac5c0427ee178cee3abd71f4fbdfd5f8697f11a7
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43698669"
---
# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-java-and-the-azure-portal"></a>Azure Cosmos DB: een MongoDB-API-console-app ontwikkelen met Java en Azure Portal

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze snelstartgids laat zien hoe u een [MongoDB-API](mongodb-introduction.md)-account van Azure Cosmos DB, een documentdatabase en een verzameling kunt maken met behulp van Azure Portal. U gaat vervolgens een console-app ontwikkelen en implementeren op het [MongoDB Java-stuurprogramma](https://docs.mongodb.com/ecosystem/drivers/java/). 

## <a name="prerequisites"></a>Vereisten

Voordat u met dit voorbeeld aan de slag gaat, moet u aan de volgende vereisten voldoen:
* JDK 1.7+ (voer `apt-get install default-jdk` uit als u niet over JDK beschikt)
* Maven (voer `apt-get install maven` uit als u niet over Maven beschikt)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [mongodb-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="add-a-collection"></a>Een verzameling toevoegen

Geef uw nieuwe database de naam **db**, en uw nieuwe verzameling **verz**.

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)] 

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een MongoDB API-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een opdrachtprompt, maak een nieuwe map met de naam git-samples en sluit vervolgens de opdrachtprompt.

    ```bash
    md "C:\git-samples"
    ```

2. Open een git-terminalvenster, bijvoorbeeld git bash, en gebruik de `cd`-opdracht om naar de nieuwe map te gaan voor het installeren van de voorbeeld-app.

    ```bash
    cd "C:\git-samples"
    ```

3. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt een kopie van de voorbeeld-app op uw computer.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started.git
    ```

3. Open daarna de code in uw favoriete editor. 

## <a name="review-the-code"></a>De code bekijken

Deze stap is optioneel. Als u wilt weten hoe de databaseresources in de code worden gemaakt, kunt u de volgende codefragmenten bekijken. Als u deze stap wilt overslaan, kunt u verdergaan naar [Uw verbindingsreeks bijwerken](#update-your-connection-string). 

De volgende codefragmenten zijn allemaal afkomstig uit het bestand Program.java.

* De DocumentClient wordt geïnitialiseerd.

    ```java
    MongoClientURI uri = new MongoClientURI("FILLME");`

    MongoClient mongoClient = new MongoClient(uri);            
    ```

* Er wordt een nieuwe database en verzameling gemaakt.

    ```java
    MongoDatabase database = mongoClient.getDatabase("db");

    MongoCollection<Document> collection = database.getCollection("coll");
    ```

* Een aantal documenten wordt ingevoegd met behulp van `MongoCollection.insertOne`

    ```java
    Document document = new Document("fruit", "apple")
    collection.insertOne(document);
    ```

* Een aantal query's wordt uitgevoerd met behulp van `MongoCollection.find`

    ```java
    Document queryResult = collection.find(Filters.eq("fruit", "apple")).first();
    System.out.println(queryResult.toJson());       
    ```

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app.

1. Selecteer in het account **Quick Start**, selecteer Java en kopieer vervolgens de verbindingsreeks naar het klembord

2. Open het bestand `Program.java`, vervang het argument voor de MongoClientURI-constructor door de verbindingsreeks. U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 
    
## <a name="run-the-console-app"></a>De console-app uitvoeren

1. Voer `mvn package` uit op een terminal zodat de vereiste npm-modules worden geïnstalleerd.

2. Voer `mvn exec:java -D exec.mainClass=GetStarted.Program` uit op een terminal om uw Java-toepassing te starten.

U kunt nu [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) gebruiken om query’s op deze nieuwe gegevens uit te voeren, ze te wijzigen en er op een ander manier mee te werken.

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hoe u een verzameling kunt maken met Data Explorer en hebt u een console-app uitgevoerd. Nu kunt u aanvullende gegevens in uw Cosmos DB-account importeren. 

> [!div class="nextstepaction"]
> [MongoDB-gegevens importeren in Azure Cosmos DB](mongodb-migrate.md)


