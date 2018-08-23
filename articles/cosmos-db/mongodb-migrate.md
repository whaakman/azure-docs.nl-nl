---
title: De hulpprogramma's mongoimport en mongorestore gebruiken met de Azure Cosmos DB-API voor MongoDB | Microsoft Docs
description: Meer informatie over het gebruik van mongoimport en mongorestore voor het importeren van gegevens in een API voor een MongoDB-account
keywords: mongoimport, mongorestore
services: cosmos-db
author: SnehaGunda
manager: slyons
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: sclyon
ms.custom: mvc
ms.openlocfilehash: ffb15c3a608cb7b7be275913cf9dec84e655334a
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "41919109"
---
# <a name="azure-cosmos-db-import-mongodb-data"></a>Azure Cosmos DB: MongoDB-gegevens importeren 

Als u gegevens wilt migreren van MongoDB naar een Azure Cosmos DB-account voor gebruik met de API voor MongoDB, moet u:

* In het [MongoDB-downloadcentrum](https://www.mongodb.com/download-center) de communityserver downloaden en deze installeren.
* De bestanden mongoimport.exe of mongorestore.exe gebruiken die zijn geïnstalleerd in de map 'installatiemap/bin'. 
* De [API voor de MongoDB-verbindingsreeks](connect-mongodb-account.md) ophalen.

Als u gegevens uit MongoDB importeert en deze wilt gebruiken met de SQL-API voor Azure Cosmos DB, moet u het [hulpprogramma voor gegevensmigratie](import-data.md) gebruiken om gegevens te importeren.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * De verbindingsreeks ophalen
> * MongoDB-gegevens importeren met mongoimport
> * MongoDB-gegevens importeren met mongorestore

## <a name="prerequisites"></a>Vereisten

* Verhoog de doorvoer: de duur van de gegevensmigratie is afhankelijk van de hoeveelheid doorvoer die u voor een afzonderlijke verzameling of een reeks verzamelingen instelt. Verhoog de doorvoer voor grotere gegevensmigraties. Nadat u de migratie hebt voltooid, verlaagt u de doorvoer om kosten te besparen. Zie [Prestatieniveaus en prijscategorieën in Azure Cosmos DB](performance-levels.md) voor meer informatie over het verhogen van de doorvoer in [Azure Portal](https://portal.azure.com).

* Schakel SSL in: voor Azure Cosmos DB gelden strenge beveiligingsvereisten en -normen. Schakel SSL in wanneer u met uw account communiceert. De procedures in de rest van het artikel bevatten informatie over het inschakelen van SSL voor mongoimport en mongorestore.

## <a name="find-your-connection-string-information-host-port-username-and-password"></a>Uw verbindingsreeksgegevens (host, poort, gebruikersnaam en wachtwoord) zoeken

1. Klik in [Azure Portal](https://portal.azure.com) in het linkerdeelvenster op het item **Azure Cosmos DB**.
1. Selecteer in het deelvenster **Abonnementen** uw accountnaam.
1. Klik in de blade **Verbindingsreeks** op **Verbindingsreeks**.

   Het rechterdeelvenster bevat alle informatie die u nodig hebt om verbinding te maken met uw account.

   ![De blade Verbindingsreeks](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-to-the-api-for-mongodb-by-using-mongoimport"></a>Gegevens importeren in de API voor MongoDB met mongoimport

Als u gegevens wilt importeren in uw Azure Cosmos DB-account, moet u de volgende sjabloon gebruiken. Vul voor *host*, *gebruikersnaam* en *wachtwoord* de waarden in die gelden voor uw account.  

Sjabloon:

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

Voorbeeld:  

    mongoimport.exe --host comsosdb-mongodb-account.documents.azure.com:10255 -u comsosdb-mongodb-account -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\admin\Desktop\*.json

## <a name="import-data-to-the-api-for-mongodb-by-using-mongorestore"></a>Gegevens importeren in de API voor MongoDB met mongorestore

Als u gegevens wilt herstellen voor de API voor een MongoDB-account, moet u de volgende sjabloon gebruiken om de import uit te voeren. Vul voor *host*, *gebruikersnaam* en *wachtwoord* de waarden in die gelden voor uw account.

Sjabloon:

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

Voorbeeld:

    mongorestore.exe --host comsosdb-mongodb-account.documents.azure.com:10255 -u comsosdb-mongodb-account -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
    
## <a name="guide-for-a-successful-migration"></a>Handleiding voor een geslaagde migratie

1. Maak van tevoren de verzamelingen en schaal deze:
        
    * Azure Cosmos DB levert standaard een nieuwe MongoDB-verzameling met 1000 aanvraageenheden per seconde. Maak voordat u de migratie start met mongoimport, mongorestore of mongomirror van tevoren alle verzamelingen in [Azure Portal](https://portal.azure.com) of uit de MongoDB-stuurprogramma's en -hulpprogramma's. Als uw verzameling groter is dan 10 GB is, moet u een [shardverzameling/gepartitioneerde verzameling](partition-data.md) met de betreffende shardsleutel maken.

    * Verhoog vlak voor de migratie in [Azure Portal](https://portal.azure.com) de doorvoer voor uw verzamelingen van 1000 aanvraageenheden per seconde voor één gepartitioneerde verzameling en 2500 aanvraageenheden per seconde voor een gedeelde verzameling. Met een hogere doorvoer voorkomt u frequentielimieten en kost migreren minder tijd. Met facturering per uur in Azure Cosmos DB kunt u de doorvoer onmiddellijk na de migratie verminderen om kosten te besparen.

    * Naast het leveren van aanvraageenheden per seconde op het verzamelingsniveau kunt u ook aanvraageenheden per seconde leveren voor een reeks verzamelingen op het bovenliggende databaseniveau. In dat geval moet u van tevoren de database en verzamelingen maken. Daarnaast moet u voor elke verzameling een shardsleutel definiëren.

    * U kunt shardverzamelingen maken via uw favoriete hulpprogramma, stuurprogramma of SDK. In dit voorbeeld gebruiken we de Mongo-shell om een shardverzameling te maken:

        ```
        db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
        ```
    
        Resultaten:

        ```JSON
        {
            "_t" : "ShardCollectionResponse",
            "ok" : 1,
            "collectionsharded" : "admin.people"
        }
        ```

1. Bereken bij benadering de aanvraageenheidskosten voor het schrijven van één document:

    a. Maak in de MongoDB-shell verbinding met uw Azure Cosmos DB MongoDB-database. U vindt instructies in [Een MongoDB-toepassing verbinden met Azure Cosmos DB](connect-mongodb-account.md).
    
    b. Voer een voorbeeld van een 'insert'-opdracht uit van een van de voorbeelddocumenten in de MongoDB-shell:
    
        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```
        
    c. Voer ```db.runCommand({getLastRequestStatistics: 1})``` uit en u ontvangt een antwoord dat lijkt op het volgende:
     
        ```
        globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
        {
            "_t": "GetRequestStatisticsResponse",
            "ok": 1,
            "CommandName": "insert",
            "RequestCharge": 10,
            "RequestDurationInMilliSeconds": NumberLong(50)
        }
        ```
        
    d. Houd rekening met de kosten voor de aanvraag.
    
1. Bepaal hoe lang het duurt voordat aanvragen vanaf uw computer worden beantwoord door de Azure Cosmos DB-cloudservice:
    
    a. Schakel uitgebreide logboekregistratie in de MongoDB-shell in met de volgende opdracht: ```setVerboseShell(true)```
    
    b. Voer een eenvoudige query uit voor de database: ```db.coll.find().limit(1)```. U ontvangt een antwoord dat lijkt op het volgende:

        ```
        Fetched 1 record(s) in 100(ms)
        ```
        
1. Verwijder het ingevoegde document vóór de migratie zodat er geen dubbele documenten voorkomen. U kunt documenten met de volgende opdracht verwijderen: ```db.coll.remove({})```

1. Bereken bij benadering de waarden voor *batchSize* en *numInsertionWorkers*:

    * Voor *batchSize* deelt u het totale aantal geleverde aanvraageenheden door de aanvraageenheden die zijn verbruikt bij het schrijven van één document in stap 3.
    
    * Als de berekende *batchSize* < = 24, gebruikt u dat getal als de waarde voor uw *batchSize*.
    
    * Als de berekende *batchSize* > 24, stelt u de waarde voor *batchSize* in op 24.
    
    * Voor *numInsertionWorkers* gebruikt u de volgende vergelijking: *numInsertionWorkers = (geleverde doorvoer * wachttijd in seconden) / (batchgrootte * aanvraageenheden verbruikt voor een enkele schrijfbewerking)*.
        
    |Eigenschap|Waarde|
    |--------|-----|
    |batchSize| 24 |
    |Geleverde aanvraageenheden | 10.000 |
    |Latentie | 0,100 s |
    |In rekening gebrachte aanvraageenheden voor het schrijven van één document | Tien aanvraageenheden |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (10.000 RU’s x 0,1 s) / (24 x 10 RU’s) = 4,1666*

1. Voer de laatste migratieopdracht uit:

   ```
   mongoimport.exe --host comsosdb-mongodb-account.documents.azure.com:10255 -u comsosdb-mongodb-account -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```
   Of met mongorestore (zorg ervoor dat de doorvoer voor alle verzamelingen is ingesteld op of boven het aantal aanvraageenheden dat is gebruikt in de voorgaande berekeningen):
   
   ```
   mongorestore.exe --host comsosdb-mongodb-account.documents.azure.com:10255 -u comsosdb-mongodb-account -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
   ```

## <a name="next-steps"></a>Volgende stappen

U kunt doorgaan met de volgende zelfstudie om te leren hoe u query's uitvoert op MongoDB-gegevens in Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Hoe moet ik query's uitvoeren op MongoDB-gegevens?](../cosmos-db/tutorial-query-mongodb.md)
