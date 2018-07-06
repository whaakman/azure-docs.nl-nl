---
title: MongoDB API’s gebruiken om een Azure Cosmos DB-app te bouwen | Microsoft Docs
description: Een zelfstudie waarmee u een online database maakt met de Azure Cosmos DB-API’s voor MongoDB.
keywords: mongodb-voorbeelden
services: cosmos-db
author: SnehaGunda
manager: kfile
editor: ''
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: sample
ms.date: 03/23/2018
ms.author: sngun
ms.openlocfilehash: 188b192cf9b86a2d28a578bbcec0d6b19a8cc5d0
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37084899"
---
# <a name="build-an-azure-cosmos-db-api-for-mongodb-app-using-nodejs"></a>Een Cosmos Azure DB bouwen: API bouwen voor MongoDB-app met behulp van Node.js
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Node.js voor MongoDB](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
>

In dit voorbeeld ziet u hoe u een Azure Cosmos DB: API bouwt voor MongoDB-console-app met Node.js.

Als u dit voorbeeld wilt gebruiken, moet u het volgende doen:

* [Maak](create-mongodb-dotnet.md#create-account) een Azure Cosmos DB: API voor MongoDB-account.
* Haal uw MongoDB-informatie over de [verbindingsreeks](connect-mongodb-account.md) op.

## <a name="create-the-app"></a>De app maken

1. Maak een *app.js*-bestand en kopieer en plak de onderstaande code.

    ```nodejs
    var MongoClient = require('mongodb').MongoClient;
    var assert = require('assert');
    var ObjectId = require('mongodb').ObjectID;
    var url = 'mongodb://<username>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';

    var insertDocument = function(db, callback) {
    db.collection('families').insertOne( {
            "id": "AndersenFamily",
            "lastName": "Andersen",
            "parents": [
                { "firstName": "Thomas" },
                { "firstName": "Mary Kay" }
            ],
            "children": [
                { "firstName": "John", "gender": "male", "grade": 7 }
            ],
            "pets": [
                { "givenName": "Fluffy" }
            ],
            "address": { "country": "USA", "state": "WA", "city": "Seattle" }
        }, function(err, result) {
        assert.equal(err, null);
        console.log("Inserted a document into the families collection.");
        callback();
    });
    };
    
    var findFamilies = function(db, callback) {
    var cursor =db.collection('families').find( );
    cursor.each(function(err, doc) {
        assert.equal(err, null);
        if (doc != null) {
            console.dir(doc);
        } else {
            callback();
        }
    });
    };
    
    var updateFamilies = function(db, callback) {
    db.collection('families').updateOne(
        { "lastName" : "Andersen" },
        {
            $set: { "pets": [
                { "givenName": "Fluffy" },
                { "givenName": "Rocky"}
            ] },
            $currentDate: { "lastModified": true }
        }, function(err, results) {
        console.log(results);
        callback();
    });
    };
    
    var removeFamilies = function(db, callback) {
    db.collection('families').deleteMany(
        { "lastName": "Andersen" },
        function(err, results) {
            console.log(results);
            callback();
        }
    );
    };
    
    MongoClient.connect(url, function(err, client) {
    assert.equal(null, err);
    var db = client.db('familiesdb');
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                client.close();
            });
        });
        });
    });
    });
    ```
    
    **Optioneel**: als u het **MongoDB Node.js 2.2-stuurprogramma** gebruikt, vervangt u het volgende codefragment:

    Oorspronkelijk:

    ```nodejs
    MongoClient.connect(url, function(err, client) {
    assert.equal(null, err);
    var db = client.db('familiesdb');
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                client.close();
            });
        });
        });
    });
    });
    ```
    
    Moet worden vervangen door:

    ```nodejs
    MongoClient.connect(url, function(err, db) {
    assert.equal(null, err);
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                db.close();
            });
        });
        });
    });
    });
    ```
    
2. Wijzig de volgende variabelen in het bestand *app.js* volgens uw accountinstellingen (meer informatie over het zoeken naar uw [verbindingsreeks](connect-mongodb-account.md)):

    > [!IMPORTANT]
    > Voor het **MongoDB Node.js 3.0-stuurprogramma** is codering vereist van speciale tekens in het Cosmos DB-wachtwoord. Zorg ervoor dat u de tekens '=' codeert als %3D
    >
    > Voorbeeld: het wachtwoord *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv==* wordt gecodeerd naar *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv%3D%3D*
    >
    > Voor het **MongoDB Node.js 2.2-stuurprogramma** is geen codering vereist van speciale tekens in het Cosmos DB-wachtwoord.
    >
    >
   
    ```nodejs
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';
    ```
     
3. Open uw favoriete terminal, voer **npm install mongodb --save** uit en voer uw app uit met **node app.js**

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe u [MongoChef gebruikt](mongodb-mongochef.md) met uw Azure-Cosmos-DB: API voor MongoDB-account.
