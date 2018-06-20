---
title: Node.js-zelfstudie voor de SQL-API voor Azure Cosmos DB | Microsoft Docs
description: Dit is een Node.js-zelfstudie waarin een Cosmos DB wordt gemaakt met de SQL-API.
keywords: node.js zelfstudie, knooppuntdatabase
services: cosmos-db
author: SnehaGunda
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 08/14/2017
ms.author: sngun
ms.openlocfilehash: 70bedfc26c900521dba8c6b211a4d4e4eda24e9c
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823687"
---
# <a name="nodejs-tutorial-use-the-sql-api-in-azure-cosmos-db-to-create-a-nodejs-console-application"></a>Node.js-zelfstudie: De SQL-API gebruiken in Azure Cosmos DB om een Node.js-consoletoepassing te maken

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Node.js voor MongoDB](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [C++](sql-api-cpp-get-started.md)

Welkom bij de Node.js-zelfstudie over de Azure Cosmos DB Node.js-SDK. Wanneer u deze zelfstudie hebt voltooid, beschikt u over een consoletoepassing waarmee u Azure Cosmos DB-resources kunt maken en er query's op kunt uitvoeren.

De volgende onderwerpen komen aan bod:

* Een Azure Cosmos DB-account maken en er verbinding mee maken
* Uw toepassing instellen
* Een knooppuntdatabase maken
* Een verzameling maken
* JSON-documenten maken
* Query's uitvoeren op de verzameling
* Een document vervangen
* Een document verwijderen
* De knooppuntdatabase verwijderen

Hebt u geen tijd? Geen probleem. De volledige oplossing is beschikbaar via [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started). Zie [De volledige oplossing gebruiken](#GetSolution) voor beknopte instructies.

Wanneer u de Node.js-zelfstudie hebt voltooid, gebruikt u de stemknoppen boven en onder aan deze pagina om feedback te verzenden. Als u graag rechtstreeks contact wilt opnemen, voegt u uw e-mailadres ook toe aan uw reactie.

Tijd om aan de slag te gaan.

## <a name="prerequisites-for-the-nodejs-tutorial"></a>Vereisten voor de Node.js-zelfstudie

Zorg ervoor dat u over de volgende zaken beschikt:

* Een actief Azure-account. Als u nog geen abonnement hebt, kunt u zich registreren voor een [gratis Azure-proefversie](https://azure.microsoft.com/pricing/free-trial/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js](https://nodejs.org/) versie v0.10.29 of hoger.

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
   * ```npm install documentdb --save```

Goed gedaan. U bent klaar met het instellen. U gaat nu aan de slag met het schrijven van code.

## <a id="Config"></a>Stap 3: de configuratie van de app instellen

Open ```config.js``` in uw favoriete teksteditor.

Kopieer en plak vervolgens het onderstaande codefragment. Stel de eigenschappen ```config.endpoint``` en ```config.primaryKey``` in op uw Azure Cosmos DB-eindpunt-URI en primaire sleutel. Deze beide configuraties vindt u in [Azure Portal](https://portal.azure.com).

![Node.js-zelfstudie: schermopname van Azure Portal waarin een Azure Cosmos DB-account wordt weergegeven met de hub ACTIEF gemarkeerd. Verder is de knop SLEUTELS gemarkeerd op de Azure Cosmos DB-accountblade en zijn de waarden URI, PRIMAIRE SLEUTEL en SECUNDAIRE SLEUTEL gemarkeerd op de blade Sleutels - Knooppuntdatabase][keys]

    // ADD THIS PART TO YOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

Kopieer de ```database id```, ```collection id``` en ```JSON documents```, en plak deze in uw ```config```-object hieronder, waar u de eigenschappen ```config.endpoint``` en ```config.primaryKey``` instelt. Als u al gegevens hebt die u in de database wilt opslaan, kunt u het [hulpprogramma voor gegevensmigratie](import-data.md) van Azure Cosmos DB gebruiken in plaats van de documentdefinities toe te voegen.

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART TO YOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
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

De database, de verzameling en de documentdefinities fungeren als ```database id``` en ```collection id``` voor uw Azure Cosmos DB en de documentgegevens.

Als laatste exporteert u uw ```config```-object zodat u ernaar kunt verwijzen binnen het ```app.js```-bestand.

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART TO YOUR CODE
    module.exports = config;

## <a id="Connect"></a>Stap 4: verbinding maken met een Azure Cosmos DB-account

Open uw lege ```app.js```-bestand in de teksteditor. Kopieer en plak de onderstaande code om de ```documentdb```-module en de zojuist gemaakte ```config```-module te importeren.

    // ADD THIS PART TO YOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    const uriFactory = require('documentdb').UriFactory;
    var config = require("./config");

Kopieer en plak de code om de eerder opgeslagen ```config.endpoint``` en ```config.primaryKey``` te gebruiken voor het maken van een nieuwe DocumentClient.

    var config = require("./config");

    // ADD THIS PART TO YOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

Nu u de code hebt om de Azure Cosmos DB-client te initialiseren, kunt u zich verder verdiepen in het werken met Azure Cosmos DB-resources.

## <a name="step-5-create-a-node-database"></a>Stap 5: een knooppuntdatabase maken

Kopieer en plak de onderstaande code om de HTTP-status voor Niet gevonden, de database-id en de verzameling-id in te stellen. Op basis van deze id's zoekt de Azure Cosmos DB-client de juiste database en verzameling.

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART TO YOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseId = config.database.id;
    var collectionId = config.collection.id;

U kunt een [database](sql-api-resources.md#databases) maken met de functie [createDatabase](/javascript/api/documentdb/documentclient) van de klasse **DocumentClient**. Een database is een logische container voor documentopslag, gepartitioneerd in verzamelingen.

Kopieer de functie **getDatabase** voor het maken van de nieuwe database en plak deze in het bestand app.js, waarbij ```databaseId``` is opgegeven van het ```config```-object. Met de functie wordt gecontroleerd of er al een database bestaat met dezelfde ```FamilyRegistry```-id. Als er al een database met dezelfde id bestaat, wordt die database geretourneerd en wordt er geen nieuwe database gemaakt.

    // ADD THIS PART TO YOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${databaseId}\n`);
        let databaseUrl = uriFactory.createDatabaseUri(databaseId);
        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase({ id: databaseId }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Kopieer de code en plak deze onder de plaats waar u de functie **getDatabase** instelt om de hulpfunctie **exit** toe te voegen waarmee het afsluitende bericht en de aanroep van de functie **getDatabase** worden afgedrukt.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key to exit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    };

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Zoek het ```app.js```-bestand in de terminal en voer de volgende opdracht uit: ```node app.js```

Gefeliciteerd. U hebt een Azure Cosmos DB-database gemaakt.

## <a id="CreateColl"></a>Stap 6: een verzameling maken

> [!WARNING]
> Met **createCollection** maakt u een nieuwe verzameling, wat gevolgen heeft voor de kosten. Zie onze [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie.

U kunt een [verzameling](sql-api-resources.md#collections) maken met de functie [createCollection](/javascript/api/documentdb/documentclient) van de klasse **DocumentClient**. Een verzameling is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica.

Kopieer de functie **getCollection** onder de functie **getDatabase** in het app.js-bestand voor het maken van uw nieuwe verzameling, waarbij ```collectionId``` is opgegeven van het ```config```-object. Er wordt opnieuw gecontroleerd of er niet al een verzameling bestaat met dezelfde ```FamilyCollection```-id. Als er al een verzameling met dezelfde id bestaat, wordt die verzameling geretourneerd en wordt er geen nieuwe verzameling gemaakt.

                } else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${collectionId}\n`);
        let collectionUrl = uriFactory.createDocumentCollectionUri(databaseId, collectionId);
        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        let databaseUrl = uriFactory.createDatabaseUri(databaseId);
                        client.createCollection(databaseUrl, { id: collectionId }, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Kopieer en plak de code onder de aanroep van **getDatabase** om de functie **getCollection** uit te voeren.

    getDatabase()

    // ADD THIS PART TO YOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Zoek het ```app.js```-bestand in de terminal en voer de volgende opdracht uit: ```node app.js```

Gefeliciteerd. U hebt nu een Azure Cosmos DB-verzameling gemaakt.

## <a id="CreateDoc"></a>Stap 7: een document maken

U kunt een [document](sql-api-resources.md#documents) maken met de functie [createDocument](/javascript/api/documentdb/documentclient) van de klasse **DocumentClient**. Documenten bestaan uit door gebruikers gedefinieerde (willekeurige) JSON-inhoud. U kunt nu een document invoegen in Azure Cosmos DB.

Kopieer de functie **getFamilyDocument** en plak deze onder de functie **getCollection** om documenten te maken die de JSON-gegevens bevatten die zijn opgeslagen in het ```config```-object. Er wordt opnieuw gecontroleerd of er niet al een document bestaat met dezelfde id.

                } else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function getFamilyDocument(document) {
        console.log(`Getting document:\n${document.id}\n`);
        let documentUrl = uriFactory.createDocumentUri(databaseId, collectionId, document.id);
        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        let collectionUrl = uriFactory.createDocumentCollectionUri(databaseId, collectionId);
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Kopieer en plak de code onder de aanroep van **getCollection** om de functie **getFamilyDocument** uit te voeren.

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Zoek het ```app.js```-bestand in de terminal en voer de volgende opdracht uit: ```node app.js```

Gefeliciteerd. U hebt nu een Azure Cosmos DB-document gemaakt.

![Node.js-zelfstudie: diagram waarin u de hiërarchische relatie ziet tussen het account, de database, de verzameling en de documenten - Knooppuntdatabase](./media/sql-api-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <a id="Query"></a>Stap 8: query's uitvoeren op Azure Cosmos DB-resources
Azure Cosmos DB biedt ondersteuning voor [uitgebreide query's](sql-api-sql-query.md) op de JSON-documenten die zijn opgeslagen in elke verzameling. In de volgende voorbeeldcode ziet u een query die u kunt uitvoeren op de documenten in uw verzameling.

Kopieer en plak de functie **queryCollection** onder de functie **getFamilyDocument** in het app.js-bestand. Azure Cosmos DB biedt ondersteuning voor SQL-achtige query's, zoals hieronder wordt weergegeven. Zie de [Query Playground](https://www.documentdb.com/sql/demo) (Queryspeelplaats) en de [querydocumentatie](sql-api-sql-query.md) voor meer informatie over het bouwen van complexe query's.

                } else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${collectionId}`);
        let collectionUrl = uriFactory.createDocumentCollectionUri(databaseId, collectionId);
        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };

In het volgende diagram ziet u hoe de Azure Cosmos DB SQL-querysyntaxis wordt aangeroepen voor de verzameling u hebt gemaakt.

![Node.js-zelfstudie: diagram waarin het bereik en de betekenis van de query worden beschreven - Knooppuntdatabase](./media/sql-api-nodejs-get-started/node-js-tutorial-collection-documents.png)

Het trefwoord [FROM](sql-api-sql-query.md#FromClause) is optioneel in de query omdat Azure Cosmos DB-query's al zijn afgestemd op één verzameling. Daarom kan FROM Families f worden ingewisseld door FROM root r, of een andere gewenste variabelenaam. Azure Cosmos DB leidt af dat 'Families', 'root' of de variabelenaam die u hebt gekozen, standaard verwijst naar de huidige verzameling.

Kopieer en plak de code onder de aanroep van **getFamilyDocument** om de functie **queryCollection** uit te voeren.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Zoek het ```app.js```-bestand in de terminal en voer de volgende opdracht uit: ```node app.js```

Gefeliciteerd. U hebt een query uitgevoerd op Azure Cosmos DB-documenten.

## <a id="ReplaceDocument"></a>Stap 9: een document vervangen
Azure Cosmos DB biedt ondersteuning voor het vervangen van JSON-documenten.

Kopieer en plak de functie **replaceFamilyDocument** onder de functie **queryCollection** in het app.js-bestand.

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function replaceFamilyDocument(document) {
        console.log(`Replacing document:\n${document.id}\n`);
        let documentUrl = uriFactory.createDocumentUri(databaseId, collectionId, document.id);
        document.children[0].grade = 6;
        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Kopieer en plak de code onder de aanroep van **queryCollection** om de functie **replaceDocument** uit te voeren. Voeg ook de code toe om **queryCollection** opnieuw aan te roepen om te verifiëren dat het document is gewijzigd.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Zoek het ```app.js```-bestand in de terminal en voer de volgende opdracht uit: ```node app.js```

Gefeliciteerd. U hebt een Azure Cosmos DB-document vervangen.

## <a id="DeleteDocument"></a>Stap 10: een document verwijderen

Azure Cosmos DB biedt ondersteuning voor het verwijderen van JSON-documenten.

Kopieer en plak de functie **deleteFamilyDocument** onder de functie **replaceFamilyDocument**.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function deleteFamilyDocument(document) {
        console.log(`Deleting document:\n${document.id}\n`);
        let documentUrl = uriFactory.createDocumentUri(databaseId, collectionId, document.id);
        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Kopieer en plak de code onder de aanroep van de tweede **queryCollection** om de functie **replaceDocument** uit te voeren.

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Zoek het ```app.js```-bestand in de terminal en voer de volgende opdracht uit: ```node app.js```

Gefeliciteerd. U hebt een Azure Cosmos DB-document verwijderd.

## <a id="DeleteDatabase"></a>Stap 11: de knooppuntdatabase verwijderen

Als u de gemaakte database verwijdert, worden de database en alle onderliggende resources (verzamelingen, documenten, enz.) verwijderd.

Kopieer en plak de functie **cleanup** onder de functie **deleteFamilyDocument** om de database en alle onderliggende resources te verwijderen.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${databaseId}`);
        let databaseUrl = uriFactory.createDatabaseUri(databaseId);
        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    };

Kopieer en plak de code onder de aanroep van **deleteFamilyDocument** om de functie **cleanup** uit te voeren.

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <a id="Run"></a>Stap 12: uw Note.js-toepassing uitvoeren

De volgorde voor het aanroepen van functies moet er als volgt uitzien:

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Zoek het ```app.js```-bestand in de terminal en voer de volgende opdracht uit: ```node app.js```

U ziet de uitvoer van uw GetStarted-app. De uitvoer moet overeenkomen met de onderstaande voorbeeldtekst.

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key to exit

Gefeliciteerd. U hebt de Node.js-zelfstudie voltooid en beschikt nu over uw eerste Azure Cosmos DB-consoletoepassing.

## <a id="GetSolution"></a>De volledige Node.js-zelfstudieoplossing gebruiken

Als u geen tijd hebt gehad om de stappen in deze zelfstudie uit te voeren of als u alleen de code wilt downloaden, kunt u deze ophalen van [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).

Als u de oplossing GetStarted wilt uitvoeren met alle voorbeelden uit dit artikel hebt u het volgende nodig:

* [Azure Cosmos DB-account][create-account].
* De [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started)-oplossing die beschikbaar is via GitHub.

Installeer de **DocumentDB**-module via NPM. Gebruik de volgende opdracht:

* ```npm install documentdb --save```

Daarna gebruikt u in het bestand ```config.js``` de waarden config.endpoint en config.primaryKey, zoals wordt beschreven in [Stap 3: de configuratie van de app instellen](#Config). 

Zoek dan het ```app.js```-bestand in de terminal en voer de volgende opdracht uit: ```node app.js```.

Dat is alles, bouw nu de oplossing. Succes! 

## <a name="next-steps"></a>Volgende stappen
* Wilt u een complexer Node.js-voorbeeld? Zie [Een Node.js-webtoepassing bouwen met Azure Cosmos DB](sql-api-nodejs-application.md).
* Leer hoe het [bewaken van een Azure Cosmos DB-account](monitor-accounts.md) werkt.
* Voer query's uit op onze voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png
