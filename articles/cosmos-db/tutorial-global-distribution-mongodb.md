---
title: Zelfstudie voor Azure DB Cosmos globale distributie voor MongoDB-API | Microsoft Docs
description: Informatie over het instellen van Azure DB die Cosmos globale distributie op basis van de MongoDB-API.
services: cosmos-db
keywords: globale distributie, MongoDB
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: a934643be02abeadecf9e8384e29bd3e0e1a36f5
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-mongodb-api"></a>Het instellen van Azure DB die Cosmos globale distributie op basis van de MongoDB-API

In dit artikel, laten we zien hoe de Azure portal instellen van Azure DB die Cosmos globale distributie en vervolgens verbinding met de MongoDB-API gebruiken.

In dit artikel bevat informatie over de volgende taken: 

> [!div class="checklist"]
> * Globale distributie op basis van de Azure-portal configureren
> * Configureren globale distributie met behulp van de [MongoDB-API](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-the-mongodb-api"></a>Verifiëren van de regionale instellingen met de MongoDB-API
De eenvoudigste manier van dubbele uw globale configuratie binnen API controleren voor MongoDB om uit te voeren, is de *isMaster()* opdracht van de Mongo-Shell.

Vanuit de Mongo-Shell:

   ```
      db.isMaster()
   ```
   
Voorbeeld van resultaten:

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

## <a name="connecting-to-a-preferred-region-using-the-mongodb-api"></a>Verbinding maken met een voorkeursregio met de MongoDB-API

De MongoDB-API kunt u uw verzameling lezen voorkeur voor een globaal gedistribueerde database opgeven. Voor beide lage latentie leest en globale hoge beschikbaarheid, wordt aangeraden uw verzameling lezen voorkeur instelt op *dichtstbijzijnde*. Een voorkeur voor het lezen *dichtstbijzijnde* is geconfigureerd voor het lezen van de dichtstbijzijnde regio.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

Voor toepassingen met een primaire lezen/schrijven regio en een secundaire regio voor noodherstel (DR) scenario's, wordt aangeraden dat uw verzameling lezen voorkeur instelt op *secundaire voorkeur*. Een voorkeur voor het lezen *secundaire voorkeur* is geconfigureerd voor het lezen van de secundaire regio als de primaire regio niet beschikbaar is.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Als u handmatig wilt ten slotte uw lezen gebieden opgeven. U kunt de regio Tag binnen uw voorkeur lezen instellen.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

Dat is, die in deze zelfstudie is voltooid. U kunt informatie over het beheren van de consistentie van uw account globaal gerepliceerde door te lezen [consistentieniveaus in Azure Cosmos DB](consistency-levels.md). En Zie voor meer informatie over hoe globale databasereplicatie in Azure Cosmos DB werkt, [gegevens globaal met Azure Cosmos DB distribueren](distribute-data-globally.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Globale distributie op basis van de Azure-portal configureren
> * Globale distributie op basis van de DocumentDB APIs configureren

U kunt nu doorgaan met de volgende zelfstudie voor meer informatie over het ontwikkelen van lokaal via de lokale Azure DB die Cosmos-emulator.

> [!div class="nextstepaction"]
> [Lokaal ontwikkelen met de emulator](local-emulator.md)
