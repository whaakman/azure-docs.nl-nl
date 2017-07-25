---
title: Een Azure Cosmos DB Node.js-toepassing ontwikkelen met behulp van de Graph API | Microsoft Docs
description: Is een Node.js-codevoorbeeld dat u kunt gebruiken om verbinding te maken met en gegevens op te vragen uit Azure Cosmos DB
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: denlee
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: 153b4cc668fdebd28cec5f3d95093a595064202a
ms.contentlocale: nl-nl
ms.lasthandoff: 07/17/2017

---
# <a name="azure-cosmos-db-build-a-nodejs-application-by-using-graph-api"></a>Azure Cosmos DB: een Node.js-toepassing ontwikkelen met de Graph API

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

In dit artikel ziet u hoe u met Azure Portal een Azure Cosmos DB-account voor Graph API (preview), een database en een graaf kunt maken. U gaat vervolgens een console-app ontwikkelen en uitvoeren met behulp van het opensourcestuurprogramma [Gremlin Node.js](https://www.npmjs.com/package/gremlin-secure).  

> [!NOTE]
> De npm-module `gremlin-secure` is een gewijzigde versie van de `gremlin`-module met ondersteuning voor SSL en SASL, vereist om verbinding te maken met Azure Cosmos DB. Broncode is beschikbaar op [GitHub](https://github.com/CosmosDB/gremlin-javascript).
>

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

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. 

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

  De configuraties bevinden zich allemaal in `config.js`, wat in het volgende gedeelte wordt bewerkt.

* Een reeks Gremlin-stappen wordt uitgevoerd met behulp van de methode `client.execute`.

    ```nodejs
    console.log('Running Count'); 
    client.execute("g.V().count()", { }, (err, results) => {
        if (err) return console.error(err);
        console.log(JSON.stringify(results));
        console.log();
    });
    ```

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

1. Open het bestand config.js. 

2. Vul in config.js voor de sleutel config.endpoint de **Gremlin URI**-waarde in van de pagina **Overzicht** van de Azure Portal. 

    `config.endpoint = "GRAPHENDPOINT";`

    ![Een toegangssleutel bekijken en kopiëren in Azure Portal, blade Sleutels](./media/create-graph-nodejs/gremlin-uri.png)

   Als de **Gremlin URI**-waarde leeg is, kunt u de waarde van de pagina **Sleutels** in de portal genereren met behulp van de **URI**-waarde, waarbij https:// wordt verwijderd en documenten in grafieken worden gewijzigd.

   Het Gremlin-eindpunt moet alleen de hostnaam zijn zonder het protocol-/poortnummer, zoals `mygraphdb.graphs.azure.com` (niet `https://mygraphdb.graphs.azure.com` of `mygraphdb.graphs.azure.com:433`).

3. Vul in config.js voor de waarde config.primaryKey de waarde van de **Primaire sleutel** in van de pagina **Sleutels** van de Azure Portal. 

    `config.primaryKey = "PRIMARYKEY";`

   ![De blade Sleutels in Azure Portal](./media/create-graph-nodejs/keys.png)

4. Voer de databasenaam en de naam van de graaf (container) in voor de waarde van config.database en config.collection. 

Hier volgt een voorbeeld van hoe het voltooide bestand config.js eruit moet zien:

```nodejs
var config = {}

// Note that this must not have HTTPS or the port number
config.endpoint = "testgraphacct.graphs.azure.com";
config.primaryKey = "Pams6e7LEUS7LJ2Qk0fjZf3eGo65JdMWHmyn65i52w8ozPX2oxY3iP0yu05t9v1WymAHNcMwPIqNAEv3XDFsEg==";
config.database = "graphdb"
config.collection = "Persons"

module.exports = config;
```

## <a name="run-the-console-app"></a>De app console uitvoeren

1. Open een terminalvenster en ga (via de opdracht `cd`) naar de installatiemap voor het bestand package.json dat is opgenomen in het project.  

2. Voer `npm install` uit om de vereiste npm-modules, waaronder `gremlin-secure`, te installeren.

3. Voer `node app.js` uit op een terminal om uw knooppunttoepassing te starten.

## <a name="browse-with-data-explorer"></a>Bladeren met Data Explorer

U kunt nu teruggaan naar Data Explorer in Azure Portal en uw nieuwe graafgegevens bekijken, aanpassen en bewerken en er query's op uitvoeren.

De nieuwe database wordt in Data Explorer weergegeven in het deelvenster **Verzamelingen**. Vouw **graphdb**, **graphcoll** uit en klik vervolgens op **Graaf**.

De gegevens die worden gegenereerd door de voorbeeldapp worden weergegeven in het deelvenster **Grafen**.

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-your-resources"></a>Uw resources opschonen

Als u niet van plan bent om door te gaan met het gebruik van deze app, verwijdert u alle resources die u in dit artikel hebt gemaakt als volgt: 

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hebt u een graaf gemaakt met Data Explorer en hebt u een app uitgevoerd. U kunt nu complexere query's maken en met Gremlin krachtige logica implementeren om door een graaf te gaan. 

> [!div class="nextstepaction"]
> [Query’s uitvoeren met Gremlin](tutorial-query-graph.md)

