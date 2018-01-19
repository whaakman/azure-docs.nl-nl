---
title: Een Azure Cosmos DB Node.js-toepassing ontwikkelen met behulp van de Graph API | Microsoft Docs
description: Is een Node.js-codevoorbeeld dat u kunt gebruiken om verbinding te maken met en gegevens op te vragen uit Azure Cosmos DB
services: cosmos-db
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 01/08/2018
ms.author: lbosq
ms.openlocfilehash: 95c0ac43e468d3655cfddc7ae0de6cefb649131d
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="azure-cosmos-db-build-a-nodejs-application-by-using-graph-api"></a>Azure Cosmos DB: een Node.js-toepassing ontwikkelen met de Graph API

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

In dit artikel ziet u hoe u met Azure Portal een Azure Cosmos DB-account voor Graph API, een database en een graaf kunt maken. U gaat vervolgens een console-app ontwikkelen en uitvoeren met behulp van het opensourcestuurprogramma [Gremlin Node.js](https://www.npmjs.com/package/gremlin).

## <a name="prerequisites"></a>Vereisten

Voordat u met dit voorbeeld aan de slag gaat, moet u aan de volgende vereisten voldoen:
* [Node.js](https://nodejs.org/en/) versie v0.10.29 of hoger
* [Git](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Een graaf toevoegen

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een Graph API-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een venster in een Git-terminal, zoals Git Bash, en ga (met de opdracht `cd`) naar een werkmap.

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen: 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-nodejs-getting-started.git
    ```

3. Open het oplossingenbestand in Visual Studio. 

## <a name="review-the-code"></a>De code bekijken

Laten we eens kijken wat er precies gebeurt in de app. Open het bestand `app.js`. Hier ziet u de volgende regels code. 

* De Gremlin-client wordt gemaakt.

    ```nodejs
    const client = Gremlin.createClient(
        443, 
        config.endpoint, 
        { 
            "session": false, 
            "ssl": true, 
            "user": `/dbs/${config.database}/colls/${config.collection}`,
            "password": config.primaryKey
        });
    ```

  De configuraties bevinden zich allemaal in `config.js`, wat in de [volgende sectie](#update-your-connection-string) wordt bewerkt.

* Een reeks functies wordt gedefinieerd voor het uitvoeren van verschillende Gremlin-bewerkingen. Dit is er een van:

    ```nodejs
    function addVertex1(callback)
    {
        console.log('Running Add Vertex1'); 
        client.execute("g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44).property('userid', 1)", { }, (err, results) => {
          if (err) callback(console.error(err));
          console.log("Result: %s\n", JSON.stringify(results));
          callback(null)
        });
    }
    ```

* Elke functie voert een `client.execute`-methode uit met behulp van een Gremlin-querytekenreeksparameter. Hier volgt een voorbeeld van hoe `g.V().count()` wordt uitgevoerd:

    ```nodejs
    console.log('Running Count'); 
    client.execute("g.V().count()", { }, (err, results) => {
        if (err) return console.error(err);
        console.log(JSON.stringify(results));
        console.log();
    });
    ```

* Aan het einde van het bestand worden alle methoden aangeroepen met behulp van de `async.waterfall()`-methode. Hiermee worden ze achter elkaar uitgevoerd:

    ```nodejs
    try{
        async.waterfall([
            dropGraph,
            addVertex1,
            addVertex2,
            addEdge,
            countVertices
            ], finish);
    } catch(err) {
        console.log(err)
    }
    ```


## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

1. Open het bestand config.js. 

2. Vul in config.js voor de sleutel `config.endpoint` de **Gremlin URI**-waarde in van de pagina **Overzicht** in Azure Portal. 

    `config.endpoint = "GRAPHENDPOINT";`

    ![Een toegangssleutel bekijken en kopiëren in Azure Portal, blade Sleutels](./media/create-graph-nodejs/gremlin-uri.png)

   Als er geen **Gremlin URI**-waarde wordt opgegeven, kunt u de waarde genereren met behulp van de waarde op de pagina **Sleutels** in de portal. Gebruik de **URI**-waarde, verwijder https:// en wijzig documenten in gremlin.cosmosdb. Als uw grafiekaccount is gemaakt vóór 20 december 2017, wijzigt u documenten in grafieken. 

   Het Gremlin-eindpunt moet alleen de hostnaam zijn zonder het protocol-/poortnummer, zoals `mygraphdb.gremlin.cosmosdb.azure.com` (niet `https://mygraphdb.gremlin.cosmosdb.azure.com` of `mygraphdb.gremlin.cosmosdb.azure.com:433`).

3. Vul in config.js voor de waarde config.primaryKey de waarde van de **Primaire sleutel** in van de pagina **Sleutels** in Azure Portal. 

    `config.primaryKey = "PRIMARYKEY";`

   ![De blade Sleutels in Azure Portal](./media/create-graph-nodejs/keys.png)

4. Voer de databasenaam en de naam van de graaf (container) in voor de waarde van config.database en config.collection. 

Hier volgt een voorbeeld van hoe het voltooide bestand config.js eruit moet zien:

```nodejs
var config = {}

// Note that this must not have HTTPS or the port number
config.endpoint = "testgraphacct.gremlin.cosmosdb.azure.com";
config.primaryKey = "Pams6e7LEUS7LJ2Qk0fjZf3eGo65JdMWHmyn65i52w8ozPX2oxY3iP0yu05t9v1WymAHNcMwPIqNAEv3XDFsEg==";
config.database = "graphdb"
config.collection = "Persons"

module.exports = config;
```

## <a name="run-the-console-app"></a>De console-app uitvoeren

1. Open een terminalvenster en ga (via de opdracht `cd`) naar de installatiemap voor het bestand package.json dat is opgenomen in het project.

2. Voer `npm install` uit om de vereiste npm-modules, waaronder `gremlin`, te installeren.

3. Voer `node app.js` uit op een terminal om uw knooppunttoepassing te starten.

## <a name="browse-with-data-explorer"></a>Bladeren met Data Explorer

U kunt nu teruggaan naar Data Explorer in Azure Portal en uw nieuwe graafgegevens bekijken, aanpassen en bewerken en er query's op uitvoeren.

De nieuwe database wordt in Data Explorer weergegeven in het deelvenster **Grafieken**. Vouw de database uit, gevolgd door de verzameling en selecteer vervolgens **Grafiek**.

De gegevens die worden gegenereerd door de voorbeeld-app worden weergegeven in het volgende deelvenster binnen het tabblad **Grafiek** wanneer u het tabblad **Filter toepassen** selecteert.

Probeer `g.V()` voltooien met `.has('firstName', 'Thomas')` om het filter te testen. Houd er rekening mee dat de waarde hoofdlettergevoelig is.

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-your-resources"></a>Uw resources opschonen

Als u niet van plan bent om door te gaan met het gebruik van deze app, verwijdert u alle resources die u in dit artikel hebt gemaakt als volgt: 

1. Selecteer in Azure Portal in het navigatiemenu links **Resourcegroepen**. Selecteer vervolgens de naam van de resource die u hebt gemaakt. 

2. Selecteer **Verwijderen** op de pagina van de resourcegroep. Typ de naam van de resource die moet worden verwijderd en selecteer vervolgens **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hebt u een graaf gemaakt met Data Explorer en hebt u een app uitgevoerd. U kunt nu complexere query's maken en met Gremlin krachtige logica implementeren om door een graaf te gaan. 

> [!div class="nextstepaction"]
> [Query’s uitvoeren met Gremlin](tutorial-query-graph.md)
