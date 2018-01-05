---
title: MongoDB APIs gebruiken voor het bouwen van een Azure DB die Cosmos-app | Microsoft Docs
description: Een zelfstudie waarmee u maakt een online-database met behulp van de Azure Cosmos DB-API's voor MongoDB.
keywords: Voorbeelden van mongodb
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: fb38bc53-3561-487d-9e03-20f232319a87
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: anhoh
ms.openlocfilehash: aa545a9fbaac0686a0a29482c8dcd3379ee4db8f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="build-an-azure-cosmos-db-api-for-mongodb-app-using-nodejs"></a>Een Cosmos Azure DB bouwen:-API voor MongoDB-app met behulp van Node.js
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Node.js voor MongoDB](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [C++](sql-api-cpp-get-started.md)
>  
>

Dit voorbeeld ziet u hoe u een Cosmos Azure DB:-API voor MongoDB-console-app met behulp van Node.js.

Voor het gebruik van dit voorbeeld moet u het volgende doen:

* [Maak](create-mongodb-dotnet.md#create-account) een Cosmos Azure DB: API voor MongoDB-account.
* Ophalen van uw MongoDB [verbindingsreeks](connect-mongodb-account.md) informatie.

## <a name="create-the-app"></a>De app maken

1. Maak een *app.js* bestand en kopieer en plak de onderstaande code.

    ```nodejs
    var MongoClient = require('mongodb').MongoClient;
    var assert = require('assert');
    var ObjectId = require('mongodb').ObjectID;
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';

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

2. Wijzig de volgende variabelen in de *app.js* bestand per uw accountinstellingen (meer informatie over het zoeken naar uw [verbindingsreeks](connect-mongodb-account.md)):
   
    ```nodejs
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';
    ```
     
3. Open uw favoriete terminal, voert u **npm Installeer mongodb--opslaan**, Voer uw app met **knooppunt app.js**

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [MongoChef gebruiken](mongodb-mongochef.md) met uw Azure-Cosmos-DB: API voor MongoDB-account.
