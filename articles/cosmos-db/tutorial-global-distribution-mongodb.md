---
title: Zelfstudie voor Azure DB Cosmos globale distributie voor MongoDB-API | Microsoft Docs
description: Lees hoe u wereldwijde distributie met Azure Cosmos DB kunt instellen met behulp van de MongoDB-API.
services: cosmos-db
keywords: globale distributie, MongoDB
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 1885c979fe2532d26b2e7b59111675bebee8ec05
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38668085"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-mongodb-api"></a>Wereldwijde distributie met Azure Cosmos DB instellen met behulp van de MongoDB-API

In dit artikel laten we zien hoe u de Azure-portal kunt gebruiken om de Azure Cosmos DB globale distributie op te zetten en vervolgens verbinding te maken met behulp van de MongoDB-API.

Dit artikel behandelt de volgende taken: 

> [!div class="checklist"]
> * Wereldwijde distributie configureren met behulp van Azure Portal
> * Globale distributie configureren met behulp van de [MongoDB-API](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-the-mongodb-api"></a>Regionale instellingen verifiëren met behulp van de MongoDB-API
De eenvoudigste manier om uw globale configuratie binnen de API voor MongoDB te controleren is het uitvoeren van de opdracht *isMaster()* vanuit de Mongo-shell.

Vanuit de Mongo-shell:

   ```
      db.isMaster()
   ```
   
Voorbeeldresultaten:

   ```JSON
      {
         "_t": "IsMasterResponse",
         "ok": 1,
         "ismaster": true,
         "maxMessageSizeBytes": 4194304,
         "maxWriteBatchSize": 1000,
         "minWireVersion": 0,
         "maxWireVersion": 2,
         "tags": {
            "region": "South India"
         },
         "hosts": [
            "vishi-api-for-mongodb-southcentralus.documents.azure.com:10255",
            "vishi-api-for-mongodb-westeurope.documents.azure.com:10255",
            "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
         ],
         "setName": "globaldb",
         "setVersion": 1,
         "primary": "vishi-api-for-mongodb-southindia.documents.azure.com:10255",
         "me": "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
      }
   ```

## <a name="connecting-to-a-preferred-region-using-the-mongodb-api"></a>Verbinding maken met een voorkeursregio met behulp van de MongoDB-API

Met de MongoDB-API kunt u de leesvoorkeur van uw verzameling opgeven voor een globaal gedistribueerde database. Voor zowel leesbewerkingen met lage latentie als globale hoge beschikbaarheid raden wij u aan de leesvoorkeur van uw verzameling in te stellen op *dichtstbijzijnde*. De leesvoorkeur *dichtstbijzijnde* is geconfigureerd voor het lezen van de dichtstbijzijnde regio.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

Voor toepassingen met een primaire lees-/schrijfregio en een secundaire regio voor noodherstel (DR) scenario's, wordt aangeraden de leesvoorkeur van uw verzameling in te stellen op *secundaire voorkeur*. De leesvoorkeur *secundaire voorkeur* is geconfigureerd om uit de secundaire regio te lezen wanneer de primaire regio niet beschikbaar is.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Tot slot wilt u mogelijk uw leesregio’s handmatig opgeven. U kunt de regiotag instellen in uw leesvoorkeur.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

En daarmee is deze zelfstudie voltooid. Informatie over het beheren van de consistentie van uw wereldwijd gerepliceerde account kunt u vinden in [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md). En voor meer informatie over hoe wereldwijde databasereplicatie werkt in Azure Cosmos DB, gaat u naar [Gegevens wereldwijd distribueren met Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Wereldwijde distributie configureren met behulp van Azure Portal
> * Wereldwijde distributie configureren met behulp van de SQL-API's

U kunt nu doorgaan met de volgende zelfstudie voor informatie over lokaal ontwikkelen met behulp van de lokale Azure Cosmos DB-emulator.

> [!div class="nextstepaction"]
> [Lokaal ontwikkelen met de emulator](local-emulator.md)
