---
title: Node.js-zelfstudie voor de SQL-API voor Azure Cosmos DB | Microsoft Docs
description: Een Node.js-zelfstudie die laat zien hoe u verbinding kunt maken met en een query kunt uitvoeren op Azure Cosmos DB met behulp van de SQL-API
keywords: node.js zelfstudie, knooppuntdatabase
services: cosmos-db
author: deborahc
manager: andrl
editor: monicar
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 07/30/2018
ms.author: dech
ms.openlocfilehash: 97b1e13532817b63a76cb57c5b63feb87cb92716
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "41924742"
---
# <a name="nodejs-tutorial-create-a-nodejs-console-application-with-azure-cosmos-db-sql-api-and-javascript-sdk-20-preview"></a>Node.js-zelfstudie: een Node.js-consoletoepassing met Azure Cosmos DB SQL API en JavaScript SDK 2.0 (preview)

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [Node.js - v2.0 Preview](sql-api-nodejs-get-started-preview.md)
> * [Node.js voor MongoDB](mongodb-samples.md)
> * [Java](sql-api-java-get-started.md)


Welkom bij de Node.js-zelfstudie over de Azure Cosmos DB JavaScript-SDK. Wanneer u deze zelfstudie hebt voltooid, beschikt u over een consoletoepassing waarmee u Azure Cosmos DB-resources kunt maken en er query's op kunt uitvoeren.

De volgende onderwerpen komen aan bod:

* Een Azure Cosmos DB-account maken en er verbinding mee maken
* Uw toepassing instellen
* Een database maken
* Een container maken
* JSON-items aan de container toevoegen
* Een query uitvoeren op de container
* Een item vervangen
* Een item verwijderen
* De database verwijderen

Hebt u geen tijd? Geen probleem. De volledige oplossing is beschikbaar via [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started ). Zie [De volledige oplossing gebruiken](#GetSolution) voor beknopte instructies.

Wanneer u de Node.js-zelfstudie hebt voltooid, gebruikt u de stemknoppen boven en onder aan deze pagina om feedback te verzenden. Als u graag rechtstreeks contact wilt opnemen, voegt u uw e-mailadres ook toe aan uw reactie.

Tijd om aan de slag te gaan.

## <a name="prerequisites-for-the-nodejs-tutorial"></a>Vereisten voor de Node.js-zelfstudie

Zorg ervoor dat u over de volgende zaken beschikt:

* Een actief Azure-account. Als u nog geen abonnement hebt, kunt u zich registreren voor een [gratis Azure-proefversie](https://azure.microsoft.com/pricing/free-trial/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js](https://nodejs.org/) versie v6.0.0 of hoger.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Stap 1: een Azure Cosmos DB-account maken

Begin met het maken van een Azure Cosmos DB-account. Als u al een account hebt dat u wilt gebruiken, kunt u verder naar de stap [Uw Node.js-toepassing instellen](#SetupNode). Als u de Azure Cosmos DB-emulator gebruikt, volgt u de stappen in [Azure Cosmos DB-emulator](local-emulator.md) om de emulator in te stellen en meteen naar [Uw Node.js-toepassing instellen](#SetupNode) te gaan.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>Stap 2: Uw Node.js-toepassing instellen

1. Open uw favoriete terminal.
2. Ga naar de map of directory waarin u de Node.js-toepassing wilt opslaan.
3. Maak twee lege JavaScript-bestanden met de volgende opdrachten:
   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```
4. Installeer de DocumentDB-module via NPM. Gebruik de volgende opdracht:
   * ```npm install @azure/cosmos --save```

Goed gedaan. U bent klaar met het instellen. U gaat nu aan de slag met het schrijven van code.

## <a id="Config"></a>Stap 3: de configuratie van de app instellen

Open ```config.js``` in uw favoriete teksteditor.

Kopieer en plak vervolgens het onderstaande codefragment. Stel de eigenschappen ```config.endpoint``` en ```config.primaryKey``` in op de eindpunt-URI en de primaire sleutel van Azure Cosmos DB. Deze beide configuraties vindt u in [Azure Portal](https://portal.azure.com).

![Node.js-zelfstudie: schermopname van Azure Portal waarin een Azure Cosmos DB-account wordt weergegeven met de hub ACTIEF gemarkeerd. Verder is de knop SLEUTELS gemarkeerd op de Azure Cosmos DB-accountblade en zijn de waarden URI, PRIMAIRE SLEUTEL en SECUNDAIRE SLEUTEL gemarkeerd op de blade Sleutels - Knooppuntdatabase][keys]

```nodejs
// ADD THIS PART TO YOUR CODE
var config = {}

config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
config.primaryKey = "~your primary key here~";
``` 

Kopieer de ```database```-, ```container```- en ```items```-gegevens en plak deze in uw ```config```-object hieronder, waar u de eigenschappen ```config.endpoint``` en ```config.primaryKey``` instelt. Als u al gegevens hebt die u in de database wilt opslaan, kunt u het [hulpprogramma voor gegevensmigratie](import-data.md) van Azure Cosmos DB gebruiken in plaats van de gegevens hier te definiëren.

```nodejs
var config = {}

config.endpoint = "~your Azure Cosmos DB account endpoint uri here~";
config.primaryKey = "~your primary key here~";

config.database = {
    "id": "FamilyDatabase"
};

config.container = {
    "id": "FamilyContainer"
};

config.items = {
    "Andersen": {
        "id": "Anderson.1",
        "lastName": "Andersen",
        "parents": [{
            "firstName": "Thomas"
        }, {
                "firstName": "Mary Kay"
            }],
        "children": [{
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [{
                "givenName": "Fluffy"
            }]
        }],
        "address": {
            "state": "WA",
            "county": "King",
            "city": "Seattle"
        }
    },
    "Wakefield": {
        "id": "Wakefield.7",
        "parents": [{
            "familyName": "Wakefield",
            "firstName": "Robin"
        }, {
                "familyName": "Miller",
                "firstName": "Ben"
            }],
        "children": [{
            "familyName": "Merriam",
            "firstName": "Jesse",
            "gender": "female",
            "grade": 8,
            "pets": [{
                "givenName": "Goofy"
            }, {
                    "givenName": "Shadow"
                }]
        }, {
                "familyName": "Miller",
                "firstName": "Lisa",
                "gender": "female",
                "grade": 1
            }],
        "address": {
            "state": "NY",
            "county": "Manhattan",
            "city": "NY"
        },
        "isRegistered": false
    }
};
```
Als u bekend bent met de vorige versie van de JavaScript SDK, komen de termen 'verzameling' en 'document' u vertrouwd voor. Azure Cosmos DB biedt ondersteuning voor [meerdere API-modellen](https://docs.microsoft.com/azure/cosmos-db/introduction#key-capabilities). Daarom wordt in versie 2.0+ van de JavaScript-SDK gebruikgemaakt van de generieke termen 'container' en 'item'. Een container kan een verzameling, een graaf of een tabel zijn. Een item kan een document, rand/hoekpunt of rij zijn en is de inhoud binnen een container. 

Als laatste exporteert u uw ```config```-object zodat u ernaar kunt verwijzen binnen het ```app.js```-bestand.
```nodejs
        },
        "isRegistered": false
    }
};

// ADD THIS PART TO YOUR CODE
module.exports = config;
```
## <a id="Connect"></a>Stap 4: verbinding maken met een Azure Cosmos DB-account

Open uw lege ```app.js```-bestand in de teksteditor. Kopieer en plak de onderstaande code om de ```@azure/cosmos```-module en de zojuist gemaakte ```config```-module te importeren.

```nodejs
// ADD THIS PART TO YOUR CODE
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');
```

Kopieer en plak de code om de eerder opgeslagen ```config.endpoint``` en ```config.primaryKey``` te gebruiken voor het maken van een nieuwe CosmosClient.

```nodejs
const url = require('url');

// ADD THIS PART TO YOUR CODE
const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });
```

Nu u de code hebt om de Azure Cosmos DB-client te initialiseren, kunt u zich verder verdiepen in het werken met Azure Cosmos DB-resources.

## <a name="step-5-create-a-database"></a>Stap 5: een database maken

Kopieer en plak de onderstaande code om de HTTP-status voor Niet gevonden, de database-id en de container-id in te stellen. Op basis van deze id's zoekt de Azure Cosmos DB-client de juiste database en container.

```nodejs
const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

// ADD THIS PART TO YOUR CODE
const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;
```

Er kan een [database](sql-api-resources.md#databases) worden gemaakt met behulp van de functie [createIfNotExists](/javascript/api/%40azure/cosmos/databases) of [create](/javascript/api/%40azure/cosmos/databases) van de klasse **Databases**. Een database is de logische container voor items die zijn gepartitioneerd in containers. 

Kopieer en plak de functies **createDatabase** en **readDatabase** in het app.js-bestand onder de definitie van ```databaseId``` en ```containerId```. Als de database nog niet bestaat wordt met de functie **createDatabase** een nieuwe database gemaakt met id ```FamilyDatabase```, opgegeven vanuit het ```config```-object. De functie **readDatabase** leest de definitie van de database om ervoor te zorgen dat de database bestaat.

```nodejs
/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}
```

Kopieer de code en plak deze onder de plaats waar u de functies **createDatabase** en **readDatabase** instelt om de helperfunctie **exit** toe te voegen waarmee het afsluitende bericht wordt afgedrukt. 

```nodejs
// ADD THIS PART TO YOUR CODE
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
};
```
Kopieer de code en plak deze onder de plaats waar u de functie **exit** instelt om de functies **createDatabase** en **readDatabase** aan te roepen.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error \${JSON.stringify(error)}`) });
```

Op dit punt moet de code in ```app.js``` er als volgt uitzien:

```nodejs
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');

const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;


/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}

/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}

createDatabase()
    .then(() => readDatabase())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Zoek in de terminal het ```app.js```-bestand en voer de volgende opdracht uit: 
```bash 
node app.js
```

Gefeliciteerd! U hebt een Azure Cosmos DB-database gemaakt.

## <a id="CreateContainer"></a>Stap 6: een container maken

> [!WARNING]
> Door het aanroepen van de functie **createContainer** wordt er een nieuwe container gemaakt. Hiervoor worden kosten in rekening gebracht. Zie onze [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie.

Er kan een container worden gemaakt met behulp van de functie [createIfNotExists](/javascript/api/%40azure/cosmos/containers) of [create](/javascript/api/%40azure/cosmos/containers) van de klasse **Containers**. 

Een container bestaat uit items (die in het geval van de SQL-API JSON-documenten zijn) en bijbehorende JavaScript-toepassingslogica.

Kopieer en plak de functies **createContainer** en **readContainer** onder de functie **readDatabase** in het app.js-bestand. Als de database nog niet bestaat wordt met de functie **createContainer** een nieuwe container gemaakt met id ```containerId```, opgegeven vanuit het ```config```-object. De functie **readContainer** leest de containerdefinitie om te verifiëren of de container bestaat.

```nodejs
/**
 * Create the container if it does not exist
 */
async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
}

/**
 * Read the container definition
 */
async function readContainer() {
    const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
}
```

Kopieer en plak de code onder de aanroep van **readDatabase** om de functies **createContainer** en **readContainer** uit te voeren.
```nodejs
createDatabase()
    .then(() => readDatabase())

    // ADD THIS PART TO YOUR CODE
    .then(() => createContainer())
    .then(() => readContainer())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Op dit punt moet de code in ```app.js``` er als volgt uitzien:

```nodejs
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');

const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;

/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}

/**
 * Create the container if it does not exist
 */
async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
}

/**
 * Read the container definition
 */
async function readContainer() {
    const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
}

/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}

createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Zoek in de terminal het ```app.js```-bestand en voer de volgende opdracht uit: 

```bash 
node app.js
```

Gefeliciteerd! U hebt nu een Azure Cosmos DB-container gemaakt.

## <a id="CreateItem"></a>Stap 7: een item maken

Er kan een item worden gemaakt met de functie [create](/javascript/api/%40azure/cosmos/items) van de klasse **Items**. Als u de SQL-API gebruikt, worden items als documenten geprojecteerd. Deze zijn door de gebruiker gedefinieerde (willekeurige) JSON-inhoud. U kunt nu een item invoegen in Azure Cosmos DB.

Kopieer de functie **createFamilyItem** en plak deze onder de functie **readContainer**. De functie **createFamilyItem** maakt de items met de JSON-gegevens die in het ```config```-object zijn opgeslagen. Voordat het item wordt gemaakt, wordt gecontroleerd of er niet al een item bestaat met dezelfde id.

```nodejs
/**
 * Create family item if it does not exist
 */
async function createFamilyItem(itemBody) {
    try {
        // read the item to see if it exists
        const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).read();
        console.log(`Item with family id ${itemBody.id} already exists\n`);
    }
    catch (error) {
        // create the family item if it does not exist
        if (error.code === HttpStatusCodes.NOTFOUND) {
            const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
            console.log(`Created family item with id:\n${itemBody.id}\n`);
        } else {
            throw error;
        }
    }
};
```

Kopieer en plak de code onder de aanroep van **readContainer** om de functie **createFamilyItem** uit te voeren.
```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())

    // ADD THIS PART TO YOUR CODE
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Zoek in de terminal het ```app.js```-bestand en voer de volgende opdracht uit: 

```bash 
node app.js
```

Gefeliciteerd! U hebt nu een Azure Cosmos DB-item gemaakt.


## <a id="Query"></a>Stap 8: query's uitvoeren op Azure Cosmos DB-resources
Azure Cosmos DB biedt ondersteuning voor [uitgebreide query's](sql-api-sql-query.md) voor de JSON-documenten die in elke container zijn opgeslagen. In de volgende voorbeeldcode ziet u een query die u kunt uitvoeren op de documenten in uw container.

Kopieer en plak de functie **queryContainer** onder de functie **createFamilyItem** in het app.js-bestand. Azure Cosmos DB biedt ondersteuning voor SQL-achtige query's, zoals hieronder wordt weergegeven. Zie de [Query Playground](https://www.documentdb.com/sql/demo) (Queryspeelplaats) en de [querydocumentatie](sql-api-sql-query.md) voor meer informatie over het bouwen van complexe query's.

```nodejs
/**
 * Query the container using SQL
 */
async function queryContainer() {
    console.log(`Querying container:\n${config.container.id}`);

    // query to return all children in a family
    const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
};
```

Kopieer en plak de code onder de aanroepen van **createFamilyItem** om de functie **queryContainer** uit te voeren.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryContainer())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Zoek in de terminal het ```app.js```-bestand en voer de volgende opdracht uit:

```bash 
node app.js
```

Gefeliciteerd! U hebt een query uitgevoerd op Azure Cosmos DB-items.

## <a id="ReplaceItem"></a>Stap 9: een item vervangen
Azure Cosmos DB biedt ondersteuning voor het vervangen van inhoud van items.

Kopieer en plak de functie **replaceFamilyItem** onder de functie **queryContainer** in het app.js-bestand. Merk op dat de waarde van de eigenschap 'grade' van een onderliggend item van 5 is gewijzigd in 6.

```nodejs
// ADD THIS PART TO YOUR CODE
/**
 * Replace the item by ID.
 */
async function replaceFamilyItem(itemBody) {
    console.log(`Replacing item:\n${itemBody.id}\n`);
    // Change property 'grade'
    itemBody.children[0].grade = 6;
    const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).replace(itemBody);
};
```
Kopieer en plak de code onder de aanroep van **queryContainer** om de functie **replaceFamilyItem** uit te voeren. Voeg ook de code toe om **queryContainer** opnieuw aan te roepen om te verifiëren dat het item is gewijzigd.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```
Zoek in de terminal het ```app.js```-bestand en voer de volgende opdracht uit:

```bash 
node app.js
```

Gefeliciteerd! U hebt een Azure Cosmos DB-item vervangen.

## <a id="DeleteItem"></a>Stap 10: een item verwijderen

Azure Cosmos DB biedt ondersteuning voor het verwijderen van JSON-items.

Kopieer en plak de functie **deleteFamilyItem** onder de functie **replaceFamilyItem**.

```nodejs
/**
 * Delete the item by ID.
 */
async function deleteFamilyItem(itemBody) {
    await client.database(databaseId).container(containerId).item(itemBody.id).delete(itemBody);
    console.log(`Deleted item:\n${itemBody.id}\n`);
};
```

Kopieer en plak de code onder de aanroep van de tweede **queryContainer** om de functie **deleteFamilyItem** uit te voeren.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer
    ())
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyItem(config.items.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Zoek in de terminal het ```app.js```-bestand en voer de volgende opdracht uit: 

```bash 
node app.js
```

Gefeliciteerd! U hebt een Azure Cosmos DB-item verwijderd.

## <a id="DeleteDatabase"></a>Stap 11: de database verwijderen

Als u de gemaakte database verwijdert, worden de database en alle onderliggende resources (containers, items, enzovoort) verwijderd.

Kopieer en plak de functie **cleanup** onder de functie **deleteFamilyItem** om de database en alle onderliggende resources te verwijderen.

```nodejs
/**
 * Cleanup the database and container on completion
 */
async function cleanup() {
    await client.database(databaseId).delete();
}
```

Kopieer en plak de code onder de aanroep van **deleteFamilyItem** om de functie **cleanup** uit te voeren.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer())
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())
    .then(() => deleteFamilyItem(config.items.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

## <a id="Run"></a>Stap 12: uw Note.js-toepassing uitvoeren

De code moet er nu als volgt uit zien:
```nodejs
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');

const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}

/**
 * Create the container if it does not exist
 */
async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
}

/**
 * Read the container definition
 */
async function readContainer() {
    const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
}

/**
 * Create family item if it does not exist
 */
async function createFamilyItem(itemBody) {
    try {
        // read the item to see if it exists
        const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).read();
        console.log(`Item with family id ${itemBody.id} already exists\n`);
    }
    catch (error) {
        // create the family item if it does not exist
        if (error.code === HttpStatusCodes.NOTFOUND) {
            const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
            console.log(`Created family item with id:\n${itemBody.id}\n`);
        } else {
            throw error;
        }
    }
};

/**
 * Query the container using SQL
 */
async function queryContainer() {
    console.log(`Querying container:\n${config.container.id}`);

    // query to return all children in a family
    const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
};

/**
 * Replace the item by ID.
 */
async function replaceFamilyItem(itemBody) {
    console.log(`Replacing item:\n${itemBody.id}\n`);
    // Change property 'grade'
    itemBody.children[0].grade = 6;
    const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).replace(itemBody);
};

/**
 * Delete the item by ID.
 */
async function deleteFamilyItem(itemBody) {
    await client.database(databaseId).container(containerId).item(itemBody.id).delete(itemBody);
    console.log(`Deleted item:\n${itemBody.id}\n`);
};

/**
 * Cleanup the database and container on completion
 */
async function cleanup() {
    await client.database(databaseId).delete();
}

/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}

createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer())
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())
    .then(() => deleteFamilyItem(config.items.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Zoek in de terminal het ```app.js```-bestand en voer de volgende opdracht uit: 

```bash 
node app.js
```

U ziet de uitvoer van uw GetStarted-app. De uitvoer moet overeenkomen met de onderstaande voorbeeldtekst.

    Created database:
    FamilyDatabase

    Reading database:
    FamilyDatabase

    Created container:
    FamilyContainer

    Reading container:
    FamilyContainer

    Created family item with id:
    Anderson.1

    Created family item with id:
    Wakefield.7

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing item:
    Anderson.1

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleted item:
    Anderson.1

    Completed successfully
    Press any key to exit

Gefeliciteerd! U hebt de Node.js-zelfstudie voltooid en beschikt nu over uw eerste Azure Cosmos DB-consoletoepassing.

## <a id="GetSolution"></a>De volledige Node.js-zelfstudieoplossing gebruiken

Als u geen tijd hebt gehad om de stappen in deze zelfstudie uit te voeren of als u alleen de code wilt downloaden, kunt u deze ophalen van [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started ).

Als u de Aan de slag-oplossing wilt uitvoeren met alle code uit dit artikel, hebt u het volgende nodig:

* Een [Azure Cosmos DB-account][create-account].
* De [Aan de slag](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started)-oplossing die beschikbaar is via GitHub.

Installeer de **@azure/cosmos**-module via npm. Gebruik de volgende opdracht:

* ```npm install @azure/cosmos --save```

Daarna gebruikt u in het bestand ```config.js``` de waarden config.endpoint en config.primaryKey, zoals wordt beschreven in [Stap 3: de configuratie van de app instellen](#Config). 

Zoek vervolgens het ```app.js```-bestand in de terminal en voer de volgende opdracht uit: 

```bash 
node app.js
```

Dat is alles, u kunt nu verder. 

## <a name="next-steps"></a>Volgende stappen
* Wilt u een complexer Node.js-voorbeeld? Zie [Een Node.js-webtoepassing bouwen met Azure Cosmos DB](sql-api-nodejs-application-preview.md).
* Leer hoe het [bewaken van een Azure Cosmos DB-account](monitor-accounts.md) werkt.
* Voer query's uit op onze voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png