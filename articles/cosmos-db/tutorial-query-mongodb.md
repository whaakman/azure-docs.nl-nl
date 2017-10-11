---
title: 'Azure Cosmos DB: Hoe kan ik query uitvoert met behulp van de API DocumentDB? | Microsoft Docs'
description: Meer informatie over query met de DocumentDB-API voor Azure Cosmos-DB
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: feffc553a9aa931d96cec71c101674fce08a466b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-how-to-query-with-api-for-mongodb"></a>Azure Cosmos DB: Hoe kan ik query met de API voor MongoDB?

De Azure DB die Cosmos [API voor MongoDB](mongodb-introduction.md) ondersteunt [MongoDB shell-query's](https://docs.mongodb.com/manual/tutorial/query-documents/). 

In dit artikel bevat informatie over de volgende taken: 

> [!div class="checklist"]
> * Een query met MongoDB

## <a name="sample-document"></a>Voorbeelddocument

De query's in dit artikel gebruikt het volgende voorbeelddocument.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a id="examplequery1"></a>Voorbeeldquery 1 

Het voorbeeld familie document bovenstaande gegeven, de volgende query retourneert de documenten waarbij het veld id overeenkomt met `WakefieldFamily`.

**Query**
    
    db.families.find({ id: “WakefieldFamily”})

**Resultaten**

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
    ],
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

## <a id="examplequery2"></a>Voorbeeldquery 2 

De volgende query retourneert alle onderliggende objecten in de familie. 

**Query**
    
    db.familes.find( { id: “WakefieldFamily” }, { children: true } )

**Resultaten**

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ]
    }


## <a id="examplequery3"></a>Voorbeeldquery 3 

De volgende query retourneert alle families die zijn geregistreerd. 

**Query**
    
    db.families.find( { "isRegistered" : true })
**Resultaten** geen document wordt geretourneerd. 

## <a id="examplequery4"></a>Voorbeeldquery 4

De volgende query retourneert alle families die niet zijn geregistreerd. 

**Query**
    
    db.families.find( { "isRegistered" : false })
**Resultaten**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery5"></a>Voorbeeldquery 5

De volgende query retourneert de families die niet zijn geregistreerd en status NY. 

**Query**
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

**Resultaten**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}


## <a id="examplequery6"></a>Voorbeeldquery 6

De volgende query retourneert alle families waar kinderen cijfers 8 zijn.

**Query**
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

**Resultaten**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery7"></a>Voorbeeldquery 7

De volgende query retourneert alle families waarbij de grootte van onderliggende matrix 3 is.

**Query**
  
      db.Family.find( {children: { $size:3} } )

**Resultaten**

Er zijn geen resultaten geretourneerd als er geen meer dan 2 onderliggende elementen. Alleen gebruikt als parameter 2 wordt deze query mislukt en wordt het volledige document retourneren.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Hebt geleerd hoe u een query uitvoert met behulp van MongoDB 

U kunt nu doorgaan met de volgende zelfstudie voor informatie over het distribueren van uw gegevens globaal.

> [!div class="nextstepaction"]
> [Uw gegevens globaal distribueren](tutorial-global-distribution-documentdb.md)

