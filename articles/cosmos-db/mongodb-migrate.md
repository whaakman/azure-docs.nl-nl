---
title: Uw MongoDB-gegevens migreren naar Azure Cosmos DB met behulp van mongoimport en mongorestore
description: U leert hoe u mongoimport en mongorestore gebruikt voor het importeren van gegevens in Cosmos DB.
keywords: mongoimport, mongorestore
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: rimman
ms.custom: mvc
Customer intent: As a developer, I want to migrate the data from my existing MongoDB to Cosmos DB.
ms.openlocfilehash: 4cd30c7981cd6807113729292db403a80cbddef0
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53793747"
---
# <a name="migrate-your-mongodb-data-to-azure-cosmos-db"></a>Uw MongoDB-gegevens naar Azure Cosmos DB migreren

 In deze zelfstudie vindt u instructies voor het migreren van gegevens die zijn opgeslagen in MongoDB naar Azure Cosmos DB dat is geconfigureerd om de API van Cosmos DB voor MongoDB te gebruiken. Als u gegevens uit MongoDB importeert en deze wilt gebruiken met de SQL-API van Azure Cosmos DB, moet u het [hulpprogramma voor gegevensmigratie](import-data.md) gebruiken om de gegevens te importeren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een migratieplan voorbereiden
> * Gegevens migreren met mongoimport
> * Gegevens migreren met mongorestore

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Zorg dat u aan de volgende vereisten voldoet voordat u de migratie start.

### <a name="plan-for-the-migration"></a>De migratie plannen

In dit gedeelte wordt beschreven hoe u de gegevensmigratie kunt plannen. U gaat RU-kosten schatten, de latentie van uw computer naar de cloudservice bepalen en de batchgrootte en het aantal invoegwerkrollen berekenen.


#### <a name="pre-create-and-scale-your-collections"></a>Uw verzamelingen vooraf maken en schalen

Maak voordat u gaat migreren met mongoimport of mongorestore eerst alle verzamelingen vanuit [Azure Portal](https://portal.azure.com) of uit MongoDB-stuurprogramma's en -hulpprogramma's. 

Verhoog de doorvoer van verzamelingen voor migratie vanuit [Azure Portal](https://portal.azure.com). Met een hogere doorvoer voorkomt u frequentielimieten en kost migreren minder tijd. U kunt de doorvoer onmiddellijk na de migratie verminderen om kosten te besparen.

Naast het inrichten van doorvoer op het verzamelingsniveau, kunt u ook doorvoer inrichten op het databaseniveau voor een set verzamelingen om de ingerichte doorvoer te delen. U moet de database en verzamelingen vooraf maken en een shardsleutel voor elke verzameling in de database met gedeelde doorvoer definiëren.

U kunt shardverzamelingen maken via uw favoriete hulpprogramma, stuurprogramma of SDK. In dit voorbeeld gebruiken we de Mongo-shell om een shardverzameling te maken:

```bash
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
De opdracht retourneert de volgende resultaten:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

#### <a name="calculate-the-approximate-ru-charge-for-a-single-document-write"></a>De RU-kosten voor het schrijven van één document bij benadering berekenen

Maak vanuit de MongoDB-shell verbinding met uw Cosmos-account dat is geconfigureerd voor het gebruik van de Cosmos DB-API voor MongoDB. U vindt instructies in [Connect a MongoDB application to Cosmos DB](connect-mongodb-account.md) (Een MongoDB-toepassing met Cosmos DB verbinden).

Voer vervolgens een voorbeeld van een 'insert'-opdracht uit door een van uw voorbeelddocumenten te gebruiken:
   
```bash
db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })
```
        
Voer de opdracht `db.runCommand({getLastRequestStatistics: 1})` uit.

U ontvangt een antwoord dat lijkt op de volgende uitvoer:
     
```bash
globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "insert",
    "RequestCharge": 10,
    "RequestDurationInMilliSeconds": NumberLong(50)
}
```
        
Houd rekening met de kosten voor de aanvraag.
    
#### <a name="determine-the-latency-from-your-machine-to-cosmos-db"></a>De latentie tussen uw computer en Cosmos DB bepalen
    
Schakel uitgebreide logboekregistratie in de MongoDB-shell in met de opdracht `setVerboseShell(true)`.
    
Voer een eenvoudige query uit op de database met de opdracht `db.coll.find().limit(1)`.

U ontvangt een antwoord dat lijkt op de volgende uitvoer:

```bash
Fetched 1 record(s) in 100(ms)
```
        
Verwijder voordat u de migratie uitvoert het ingevoegde document zodat er geen dubbele documenten voorkomen. U kunt documenten verwijderen met de opdracht `db.coll.remove({})`.

#### <a name="calculate-the-approximate-values-for-the-batchsize-and-numinsertionworkers-properties"></a>De waarden voor de eigenschappen batchSize en numInsertionWorkers bij benadering berekenen

Deel voor de eigenschap **batchSize** de totale ingerichte doorvoer (RU's/sec) door het aantal gebruikte RU's voor het schrijven van één document, zoals is gedaan in het gedeelte "De latentie tussen uw computer en Cosmos DB bepalen". Als de berekende waarde kleiner dan of gelijk aan 24 is, gebruikt u dat getal als de eigenschapswaarde. Als de berekende waarde groter dan 24 is, stelt u de eigenschapswaarde in op 24.
    
Voor de waarde van de eigenschap **numInsertionWorkers** gebruikt u deze vergelijking:

`numInsertionWorkers = (Provisioned RUs throughput * Latency in seconds) / (batchSize * Consumed RUs for a single write)`

We kunnen de volgende waarden gebruiken om een waarde voor de eigenschap **numInsertionWorkers** te berekenen:

| Eigenschap | Waarde |
|--------|-----|
| **batchSize** | 24 |
| Ingerichte RU's | 10.000 |
| Latentie | 0,100 s |
| Verbruikte RU's | Tien aanvraageenheden |
| **numInsertionWorkers** | (10.000 RU's x 0,100 s) / (24 x 10 RU's) = **4,1666** |

Voer de migratieopdracht **monogoimport** uit. De opdrachtparameters worden verderop in dit artikel beschreven.

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
```

U kunt ook de opdracht **monogorestore** gebruiken. Zorg ervoor dat de doorvoer voor alle verzamelingen is ingesteld op het aantal RU's dat is gebruikt in de voorgaande berekeningen of een hoger getal.
   
```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
```

### <a name="complete-the-prerequisites"></a>Aan de vereisten voldoen

Nadat u de migratie hebt gepland, voltooit u de volgende stappen: 

* **Voorbeeldgegevens ophalen**: zorg ervoor dat u voorbeeldgegevens hebt voordat u de migratie start. 

* **Doorvoer vergroten**: de duur van de gegevensmigratie is afhankelijk van de doorvoerhoeveelheid die u voor een afzonderlijke verzameling of database inricht. Verhoog de doorvoer voor grotere gegevensmigraties. Nadat u de migratie hebt voltooid, verlaagt u de doorvoer om kosten te besparen. 

* **SSL inschakelen**:  voor Cosmos DB gelden strenge beveiligingsvereisten en -normen. Schakel SSL in wanneer u uw Cosmos-account gebruikt. De procedures in dit artikel bevatten informatie over het inschakelen van SSL voor de opdrachten mongoimport en mongorestore.

* **Cosmos DB-resources maken**: voordat u gegevens gaat migreren, maakt u vooraf alle verzamelingen vanuit Azure Portal. Als u wilt migreren naar een Cosmos-account dat doorvoer ingericht op databaseniveau heeft, geeft u een partitiesleutel op wanneer u de verzamelingen maakt.

* **Verbindingsreeks ophalen**: selecteer in [Azure Portal](https://portal.azure.com) aan de linkerkant de vermelding **Azure Cosmos DB**. Selecteer onder **Abonnementen** uw accountnaam. Selecteer onder **Verbindingsreeks** de optie **Verbindingsreeks**. Aan de rechterkant van de portal staat de informatie die u nodig hebt om verbinding met uw account te maken:

    ![Verbindingsreeksgegevens](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="use-mongoimport"></a>mongoimport gebruiken

Als u gegevens wilt importeren in uw Cosmos-account, gebruikt u de volgende sjabloon.

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

Vervang de parameters \<your_hostname>, \<your_username> en \<your_password> door de specifieke waarden voor uw account. In het volgende voorbeeld gebruiken we **sampleDB** als de waarde voor \<your_database> en **sampleColl** als de waarde voor \<your_collection>:

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="use-mongorestore"></a>mongorestore gebruiken

Als u gegevens wilt herstellen voor uw Cosmos-account dat is geconfigureerd met de Cosmos DB-API voor MongoDB, gebruikt u de volgende sjabloon om de import uit te voeren.

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

Vervang de parameters \<your_hostname>, \<your_username> en \<your_password> door de specifieke waarden voor uw account. In het volgende voorbeeld gebruiken we **./dumps/dump-2016-12-07** als de waarde voor \<path_to_backup>:

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --db mydatabase --collection mycollection --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de resources niet meer nodig hebt, kunt u de resourcegroep, het Cosmos-account en alle bijbehorende resources verwijderen. Gebruik de volgende stappen om de resourcegroep te verwijderen:

1. Ga naar de resourcegroep waarin u het Cosmos-account hebt gemaakt.
1. Selecteer **Resourcegroep verwijderen**.
1. Bevestig de naam van de resourcegroep die u wilt verwijderen en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Ga verder met de volgende zelfstudie om te leren hoe u query's uitvoert op gegevens met de Azure Cosmos DB-API voor MongoDB. 

> [!div class="nextstepaction"]
> [Query's uitvoeren op MongoDB-gegevens](../cosmos-db/tutorial-query-mongodb.md)
