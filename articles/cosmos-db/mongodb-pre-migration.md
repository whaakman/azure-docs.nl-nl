---
title: Stappen vóór de migratie voor migraties van MongoDB naar Azure Cosmos DB-API voor MongoDB
description: Dit document bevat een overzicht van de vereisten voor gegevensmigratie van van MongoDB naar Cosmos DB.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: roaror
ms.openlocfilehash: 476a143555323bbb5058541000a5b1a26d23b71a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60013578"
---
# <a name="pre-migration-steps-for-data-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>Stappen vóór de migratie voor migraties van MongoDB naar Azure Cosmos DB-API voor MongoDB

Voordat u uw gegevens van MongoDB migreren (on-premises of in de cloud (IaaS)) voor Azure Cosmos DB-API voor MongoDB, moet u:

1. [Azure Cosmos DB-account maken](#create-account)
2. [Maak een schatting van de doorvoer die nodig zijn voor uw workloads](#estimate-throughput)
3. [Kies een optimale partitiesleutel voor uw gegevens](#partitioning)
4. [Het indexeringsbeleid dat u voor uw gegevens instellen kunt begrijpen](#indexing)

Als u de bovenstaande vereisten voor de migratie al hebt voltooid, raadpleegt u de [MongoDB migreren van gegevens naar Azure Cosmos DB-API voor MongoDB](../dms/tutorial-mongodb-cosmos-db.md) voor de gegevens daadwerkelijk migratie-instructies. Als dat niet het geval is, moet u dit document vindt u instructies voor het afhandelen van deze vereisten. 

## <a id="create-account"></a> Een Azure Cosmos DB-account maken 

Voordat u de migratie start, moet u eerst [maken van een Azure Cosmos-account met behulp van Azure Cosmos DB-API voor MongoDB](create-mongodb-dotnet.md). 

Op het account te maken, kunt u instellingen kiezen [wereldwijd distribueren](distribute-data-globally.md) uw gegevens. U hebt ook de optie voor schrijfbewerkingen in meerdere regio's inschakelen (of meerdere masters configuratie), waarmee elk van uw regio voor beide een schrijven en lezen regio's.

![Account-Creation](./media/mongodb-pre-migration/account-creation.png)

## <a id="estimate-throughput"></a> Maak een schatting van de doorvoer nodig voor uw workloads

Voordat u begint met de migratie met behulp van de [Database Migration Service (DMS)](../dms/dms-overview.md), dient u de hoeveelheid doorvoer om in te richten voor uw Azure Cosmos-databases en verzamelingen te schatten.

Doorvoer kan worden ingericht op een:

- Verzameling

- Database

> [!NOTE]
> U kunt ook een combinatie van de bovenstaande hebben, waarbij sommige verzamelingen in een database kunnen hebben speciale doorvoer ingericht en anderen kunnen delen de doorvoer. Zie voor meer informatie de [doorvoer instellen op een database en een container](set-throughput.md) pagina.
>

Eerst moet u bepalen of u wilt inrichten van de database of verzameling niveau doorvoer of een combinatie van beide. Het verdient in het algemeen een speciaal toegewezen doorvoer configureren op het niveau verzameling. Inrichting doorvoer op databaseniveau kan verzamelingen in uw database voor het delen van de ingerichte doorvoer. Met gedeelde doorvoer, maar er is geen garantie voor een specifieke doorvoer voor elke afzonderlijke verzameling en krijgt u geen voorspelbare prestaties op een specifieke verzameling.

Als u niet zeker weet over hoeveel doorvoer moet worden toegewezen aan elke afzonderlijke verzameling, kunt u de doorvoer op databaseniveau. U kunt zien van de ingerichte doorvoer geconfigureerd in uw Azure Cosmos-database als een logische equivalent met die van de computercapaciteit van een MongoDB-VM of een fysieke server, maar rendabeler de mogelijkheid om elastisch te schalen. Zie voor meer informatie, [doorvoer inrichten op Azure Cosmos-containers en databases](set-throughput.md).

Als u de doorvoer op het databaseniveau van de inrichten, moeten alle verzamelingen die zijn gemaakt in die database worden gemaakt met een partitie/shard-sleutel. Zie voor meer informatie over het partitioneren van [partitioneren en horizontaal schalen in Azure Cosmos DB](partition-data.md). Als u een partitie/shard-sleutel tijdens de migratie niet opgeeft, de Azure Database Migration Service wordt automatisch gevuld het veld voor de shard-sleutel met een *i_d* kenmerk dat automatisch is gegenereerd voor elk document.

### <a name="optimal-number-of-request-units-rus-to-provision"></a>Optimale aantal Aanvraageenheden (ru's) om in te richten

In Azure Cosmos DB is de doorvoer vooraf is ingericht, en wordt gemeten in Aanvraageenheden (RU's) per seconde. Als u workloads met MongoDB op een virtuele machine of on-premises hebt, zien RU's als een eenvoudige abstractie voor fysieke resources, zoals voor de grootte van een virtuele machine of op-een lokale server en de resources die ze, zoals hebben geheugen, CPU, IOP's. 

In tegenstelling tot virtuele machines of on-premises servers zijn de RU's eenvoudig omhoog en omlaag schalen op elk gewenst moment. U kunt het aantal ingerichte ru's binnen enkele seconden wijzigen en u alleen voor het maximum aantal ru's die u voor een bepaalde periode van één uur inricht worden gefactureerd. Zie voor meer informatie, [Aanvraageenheden in Azure Cosmos DB](request-units.md).

Hier volgen de belangrijkste factoren die invloed hebben op het aantal vereiste ru's:
- **De grootte van artikel (dat wil zeggen, document)**: Als de grootte van een item/document toeneemt, wordt het aantal ru's gebruikt om te lezen of schrijven van het item/document ook toeneemt.
- **De eigenschap count-item**: Ervan uitgaande dat de [standaard indexeren](index-overview.md) voor alle eigenschappen, het aantal ru's die worden gebruikt voor het schrijven van een item wordt verhoogd wanneer het item eigenschap aantal toeneemt. U kunt kan worden verkleind de verzoek-eenheid voor schrijfbewerkingen door [beperkt het aantal geïndexeerde eigenschappen](index-policy.md).
- **Gelijktijdige bewerkingen**: Vraag eenheden verbruikt ook afhankelijk van de frequentie met welke andere CRUD-bewerkingen (zoals schrijfbewerkingen, lezen, updates, verwijderingen) en meer complexe query's worden uitgevoerd. U kunt [mongostat](https://docs.mongodb.com/manual/reference/program/mongostat/) om uit te voeren van de behoeften van de gelijktijdigheid van uw huidige MongoDB-gegevens.
- **Query uitvoeren op patronen**: De complexiteit van een query is van invloed op het aantal aanvraageenheden worden verbruikt door de query.

Als u met behulp van JSON-bestanden exporteren [mongoexport](https://docs.mongodb.com/manual/reference/program/mongoexport/) en begrijpen hoeveel geschreven, gelezen, bijgewerkt en verwijderd die plaatsvinden per seconde, kunt u de [Azure Cosmos DB-Capaciteitsplanner](https://www.documentdb.com/capacityplanner) om in te schatten het oorspronkelijke aantal ru's om in te richten. De Capaciteitsplanner heeft geen factor in de kosten van complexe query's. Dus als u complexe query's op uw gegevens hebt, worden extra ru's gebruikt. De prijzencalculator zijn ook van uitgegaan dat alle velden worden geïndexeerd en sessieconsistentie wordt gebruikt. De beste manier om informatie over de kosten van query's voor het migreren van uw gegevens (of voorbeeldgegevens) met Azure Cosmos DB is [verbinding maken met de Cosmos-DB-eindpunt](connect-mongodb-account.md) en een voorbeeldquery uitvoeren vanuit de MongoDB-Shell met behulp van de `getLastRequestStastistics` -opdracht voor de aanvraag kosten in rekening gebracht, die het aantal verbruikte ru's wordt uitgevoerd:

`db.runCommand({getLastRequestStatistics: 1})`

Met deze opdracht wordt een JSON-document die vergelijkbaar is met de volgende uitvoer:

```{  "_t": "GetRequestStatisticsResponse",  "ok": 1,  "CommandName": "find",  "RequestCharge": 10.1,  "RequestDurationInMilliSeconds": 7.2}```

Nadat u het aantal ru's die worden gebruikt door een query te begrijpen en de gelijktijdigheid van taken die nodig zijn voor deze query, kunt u het aantal ingerichte ru's aanpassen. RU's optimaliseren is niet een eenmalige gebeurtenis - u moet voortdurend optimaliseren of schalen van de RU's ingericht, afhankelijk van of u niet verwacht een zwaar verkeer, in plaats van zwaar worden belast of importeren van gegevens.

## <a id="partitioning"></a>De partitiesleutel kiezen
Partitioneren is een belangrijk punt van overweging voordat u migreert naar een globaal gedistribueerde Database, zoals Azure Cosmos DB. Azure Cosmos DB maakt gebruik van partitionering van afzonderlijke containers schalen in een database om te voldoen aan de behoeften van de schaalbaarheid en prestaties van uw toepassing. In het partitioneren, worden de items in een container onderverdeeld in afzonderlijke subsets die logische partities worden genoemd. Raadpleeg voor meer informatie en aanbevelingen over het kiezen van de juiste partitiesleutel voor uw gegevens, de [kiezen van een partitiesleutel sectie](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey). 

## <a id="indexing"></a>Uw gegevens indexeren
Azure Cosmos DB indexeert standaard de gegevensvelden bij opname. U kunt de [indexeringsbeleid](index-policy.md) in Azure Cosmos DB op elk gewenst moment. Het verdient aanbeveling in feite vaak uitschakelen bij het migreren van gegevens indexeren, en vervolgens weer inschakelen wanneer de gegevens zich al in Cosmos DB. Voor meer informatie over het indexeren, kunt u meer informatie over het in de [indexeren in Azure Cosmos DB](index-overview.md) sectie. 

[Azure Database Migration Service](../dms/tutorial-mongodb-cosmos-db.md) worden automatisch gemigreerd MongoDB verzamelingen met unieke indexen. De unieke indexen moeten echter worden gemaakt vóór de migratie. Azure Cosmos DB biedt geen ondersteuning voor het maken van de unieke indexen, als er al gegevens in uw verzamelingen. Zie voor meer informatie, [unieke sleutels in Azure Cosmos DB](unique-keys.md).

## <a name="next-steps"></a>Volgende stappen
* [Migreer uw MongoDB-gegevens met Cosmos DB met behulp van de Database Migration Service.](../dms/tutorial-mongodb-cosmos-db.md) 
* [Doorvoer inrichten op Azure Cosmos-containers en databases](set-throughput.md)
* [Partitionering in Azure Cosmos DB](partition-data.md)
* [Wereldwijde distributie in Azure Cosmos DB](distribute-data-globally.md)
* [Indexering in Azure Cosmos DB](index-overview.md)
* [Aanvraageenheden in Azure Cosmos DB](request-units.md)
