---
title: Mongoimport en mongorestore gebruiken met de Azure Cosmos DB-API voor MongoDB | Microsoft Docs
description: Informatie over het gebruik van mongoimport en mongorestore voor het importeren van gegevens naar een API voor MongoDB-account
keywords: mongoimport, mongorestore
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 1555f13c3ea88b61be0ea240b51218b83f6f9724
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cosmos-db-import-mongodb-data"></a>Azure Cosmos DB: Import MongoDB-gegevens 

Als u wilt migreren van gegevens van MongoDB naar een Cosmos-DB Azure-account voor gebruik met de API voor MongoDB, moet u het volgende doen:

* Download een *mongoimport.exe* of *mongorestore.exe* van de [MongoDB-Downloadcentrum](https://www.mongodb.com/download-center).
* Ophalen van uw [-API voor MongoDB-verbindingsreeks](connect-mongodb-account.md).

Als u gegevens uit MongoDB importeert en plannen voor gebruik met de Azure-Cosmos-database, moet u de [hulpprogramma voor gegevensmigratie](import-data.md) om gegevens te importeren.

Deze zelfstudie bevat de volgende taken:

> [!div class="checklist"]
> * Bij het ophalen van de verbindingsreeks
> * MongoDB-gegevens importeren met behulp van mongoimport
> * MongoDB-gegevens importeren met behulp van mongorestore

## <a name="prerequisites"></a>Vereisten

* Verhoogt de doorvoer: de duur van de migratie is afhankelijk van de hoeveelheid doorvoer die u voor uw verzamelingen instellen. Zorg ervoor dat de doorvoer voor grotere gegevens migraties verhogen. Nadat u de migratie hebt voltooid, verlaagt u de doorvoer voor het opslaan van kosten. Voor meer informatie over de toenemende doorvoer in de [Azure-portal](https://portal.azure.com), Zie [prestatieniveaus en prijscategorieën in Azure Cosmos DB](performance-levels.md).

* Schakel SSL: Azure Cosmos DB heeft strenge beveiligingsvereisten en standaarden. Zorg ervoor dat SSL in te schakelen wanneer u met uw account communiceert. De procedures in de rest van het artikel bevatten informatie over het inschakelen van SSL voor mongoimport en mongorestore.

## <a name="find-your-connection-string-information-host-port-username-and-password"></a>Verbindingsreeksgegevens (host, poort, gebruikersnaam en wachtwoord) vinden

1. In de [Azure-portal](https://portal.azure.com), in het linkerdeelvenster klikt u op de **Azure Cosmos DB** vermelding.
2. In de **abonnementen** deelvenster, selecteer de accountnaam van uw.
3. In de **verbindingsreeks** blade, klikt u op **verbindingsreeks**.  
Het rechter deelvenster bevat alle informatie die u nodig hebt om verbinding te maken aan uw account.

    ![Connection String-blade](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-to-the-api-for-mongodb-by-using-mongoimport"></a>Gegevens importeren in de API voor MongoDB met behulp van mongoimport

Om gegevens te importeren naar uw Azure DB die Cosmos-account, moet u de volgende sjabloon gebruikt. Vul *host*, *gebruikersnaam*, en *wachtwoord* met de waarden die specifiek voor uw account zijn.  

Sjabloon:

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

Voorbeeld:  

    mongoimport.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## <a name="import-data-to-the-api-for-mongodb-by-using-mongorestore"></a>Gegevens importeren in de API voor MongoDB met behulp van mongorestore

Voor het herstellen van gegevens naar uw API voor MongoDB-account, moet u de volgende sjabloon gebruiken uit te voeren van het importeren. Vul *host*, *gebruikersnaam*, en *wachtwoord* met de waarden die specifiek voor uw account zijn.

Sjabloon:

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

Voorbeeld:

    mongorestore.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
    
## <a name="guide-for-a-successful-migration"></a>Handleiding voor een succesvolle migratie

1. Vooraf maken en schalen van uw verzamelingen:
        
    * Standaard levert Azure Cosmos DB een nieuwe verzameling MongoDB met 1000 aanvraageenheden (RUs). Voordat u de migratie met behulp van mongoimport, mongorestore of mongomirror, vooraf maken van alle verzamelingen uit de [Azure-portal](https://portal.azure.com) of van MongoDB-stuurprogramma's en hulpprogramma's. Als uw verzameling groter dan 10 GB is, zorg ervoor dat u een [shard/gepartitioneerd verzameling](partition-data.md) met een juiste shard-sleutel.

    * Van de [Azure-portal](https://portal.azure.com), waardoor de doorvoercapaciteit uw verzamelingen van 1000 RUs voor een verzameling van één partitie en 2500 RUs voor een verzameling shard slechts voor de migratie. U kunt met de hogere doorvoer voorkomen bandbreedtebeperking en migreren in minder tijd. Met elk uur facturering in Azure Cosmos DB, kunt u de doorvoer reduceren onmiddellijk na de migratie om kosten te besparen.

2. De geschatte kosten RU voor een schrijfbewerking voor één document berekenen:

    a. Verbinding maken met uw Azure Cosmos DB MongoDB-database vanuit de MongoDB-Shell. U vindt instructies in [verbinding maken met een toepassing MongoDB bij Azure Cosmos DB](connect-mongodb-account.md).
    
    b. Een voorbeeld van een opdracht insert uitvoeren via een van uw voorbeelddocumenten van de MongoDB-Shell:
    
        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```
        
    c. Voer ```db.runCommand({getLastRequestStatistics: 1})``` en ontvangt u een antwoord zoals deze:
     
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
        
    d. Let op de kosten van de aanvraag.
    
3. Bepaal de latentie van uw computer naar de cloudservice van Azure DB die Cosmos:
    
    a. Uitgebreide logboekregistratie in de MongoDB-Shell inschakelen met behulp van deze opdracht:```setVerboseShell(true)```
    
    b. Een eenvoudige query uitgevoerd voor de database: ```db.coll.find().limit(1)```. U ontvangt een antwoord zoals deze:

        ```
        Fetched 1 record(s) in 100(ms)
        ```
        
4. De ingevoegde document vóór de migratie om ervoor te zorgen dat er geen dubbele documenten zijn verwijderen. Met deze opdracht kunt u documenten:```db.coll.remove({})```

5. De geschatte berekenen *batchSize* en *numInsertionWorkers* waarden:

    * Voor *batchSize*, het totale aantal RUs door de RUs verbruikt van uw schrijven voor één document in stap 3 ingericht delen.
    
    * Als de berekende *batchSize* < = 24, gebruikt u dat nummer als uw *batchSize* waarde.
    
    * Als de berekende *batchSize* > 24, stel de *batchSize* waarde 24.
    
    * Voor *numInsertionWorkers*, gebruiken deze vergelijking: *numInsertionWorkers = (ingerichte doorvoer * latentie in seconden) / (batchgrootte * RUs verbruikt voor een enkele schrijven)*.
        
    |Eigenschap|Waarde|
    |--------|-----|
    |batchSize| 24 |
    |RUs ingericht | 10.000 |
    |Latentie | 0.100 s |
    |RU in rekening gebracht voor 1 doc schrijven | 10 RUs |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (10000 RUs x 0,1 s) / (24 x 10 RUs) 4.1666 =*

6. Voer de laatste migratie-opdracht:

   ```
   mongoimport.exe --host anhoh-mongodb.documents.azure.com:10255 -u anhoh-mongodb -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```

## <a name="next-steps"></a>Volgende stappen

U kunt doorgaan met de volgende zelfstudie en informatie over het opvragen van MongoDB-gegevens met behulp van Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Hoe MongoDB querygegevens?](../cosmos-db/tutorial-query-mongodb.md)
