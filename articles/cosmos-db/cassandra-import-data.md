---
title: Gegevens migreren naar een Azure Cosmos DB Cassandra-API-account
description: Lees hoe u met de CQL-opdracht Copy en Spark gegevens van Apache kopieert naar de Cassandra-API van Azure Cosmos DB.
services: cosmos-db
author: kanshiG
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.author: govindk
ms.topic: tutorial
ms.date: 09/24/2018
ms.reviewer: sngun
ms.openlocfilehash: 56fc07c6d775ee8015ce244acb7782607bda802a
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739784"
---
# <a name="tutorial-migrate-your-data-to-azure-cosmos-db-cassandra-api-account"></a>Zelfstudie: Gegevens migreren naar een Azure Cosmos DB Cassandra-API-account

In deze zelfstudie vindt u instructies voor het migreren van Apache Cassandra-gegevens naar de Cassandra-API van Azure Cosmos DB. 

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Plan voor migratie
> * Vereisten voor migratie
> * Gegevens migreren met de cqlsh-opdracht COPY
> * Gegevens migreren met Spark 

## <a name="plan-for-migration"></a>Plan voor migratie

Voordat u gegevens migreert naar de Cassandra-API van Azure Cosmos DB, moet u een schatting maken van de doorvoervereisten voor uw workload. In het algemeen verdient het aanbeveling om te beginnen met de gemiddelde doorvoer die is vereist voor de CRUD-bewerkingen en vervolgens de aanvullende doorvoer toe te voegen die vereist is voor ETL-bewerkingen (Extract Transform Load) of piekbewerkingen. U hebt de volgende gegevens nodig om de migratie te plannen: 

* **Bestaande gegevensgrootte of een schatting van de gegevensgrootte:** de minimale databasegrootte en de vereiste voor de doorvoer. Als u een schatting maakt van de gegevensgrootte voor een nieuwe toepassing, kunt u ervan uitgaan dat de gegevens gelijkmatig worden verdeeld over de rijen en een schatting van de waarde maken door deze te vermenigvuldigen met de gegevensgrootte. 

* **Vereiste doorvoer:** een schatting van de doorvoersnelheid voor lezen (query/get) en schrijven (update/delete/insert). Deze waarde is vereist voor het berekenen van de vereiste aanvraageenheden en de gegevensgrootte bij een stabiele toestand.  

* **Ophalen van het schema:** maak verbinding met uw bestaande Cassandra-cluster via cqlsh en exporteer het schema uit Cassandra: 

  ```bash
  cqlsh [IP] "-e DESC SCHEMA" > orig_schema.cql
  ```

Nadat u de vereisten van uw bestaande workload hebt geïdentificeerd, maakt u een Azure Cosmos DB-account, database en containers op basis van de verzamelde vereisten voor de doorvoer.  

* **De RU-kosten voor een bewerking bepalen:** u kunt de RU's bepalen met behulp van de Azure Cosmos DB Cassandra API-SDK van uw keuze. In dit voorbeeld ziet u de .NET-versie van het ophalen van RU-kosten.

  ```csharp
  var tableInsertStatement = table.Insert(sampleEntity);
  var insertResult = await tableInsertStatement.ExecuteAsync();

  foreach (string key in insertResult.Info.IncomingPayload)
    {
       byte[] valueInBytes = customPayload[key];
       string value = Encoding.UTF8.GetString(valueInBytes);
       Console.WriteLine($"CustomPayload:  {key}: {value}");
    }
  ```

* **Toewijzen van de vereiste doorvoer:** Azure Cosmos DB kan opslag en doorvoer automatisch schalen als uw vereisten veranderen. U kunt uw doorvoer schatten met behulp van de [calculator voor aanvraageenheden en gegevensopslag](https://www.documentdb.com/capacityplanner). 

## <a name="prerequisites-for-migration"></a>Vereisten voor migratie

* **Tabellen maken in Cassandra API-account van Azure Cosmos DB:** voordat u gegevens gaat migreren, maakt u vooraf alle tabellen vanuit de Azure Portal of met cqlsh. Als u migreert naar een Azure Cosmos DB-account dat doorvoer op databaseniveau heeft, moet u een partitiesleutel opgeven bij het maken van de Azure Cosmos DB-containers.

* **Doorvoer verhogen**: de duur van de gegevensmigratie is afhankelijk van de hoeveelheid doorvoer die u voor de tabellen hebt ingericht in Azure Cosmos DB. Verhoog de doorvoer voor de duur van de migratie. Met een hogere doorvoer voorkomt u frequentielimieten en kost migreren minder tijd. Nadat u de migratie hebt voltooid, verlaagt u de doorvoer om kosten te besparen. Zie [Doorvoer instellen](set-throughput.md) voor Azure Cosmos DB-containers voor meer informatie over het verhogen van de doorvoer. Het is ook raadzaam om het Azure Cosmos DB-account in dezelfde regio te plaatsen als uw brondatabase. 

* **Schakel SSL in:** voor Azure Cosmos DB gelden strenge beveiligingsvereisten en -normen. Schakel SSL in wanneer u uw account gebruikt. Wanneer u CQL gebruikt met SSH, hebt u een optie om SSL-informatie op te geven.

## <a name="options-to-migrate-data"></a>Mogelijkheden voor migreren van gegevens

U kunt gegevens op de volgende manieren verplaatsen van bestaande Cassandra-workloads naar Azure Cosmos DB:

* [Met de cqlsh-opdracht COPY](#using-cqlsh-copy-command)  
* [Met Spark](#using-spark) 

## <a name="migrate-data-using-cqlsh-copy-command"></a>Gegevens migreren met de cqlsh-opdracht COPY

De [opdracht CQL COPY](http://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) wordt gebruikt om lokale gegevens te kopiëren naar een Cassandra API-account van Azure Cosmos DB. Voer de volgende stappen uit om gegevens te kopiëren:

1. Vraag de verbindingsreeks van uw Cassandra API-account op:

   * Meld u aan bij de [Azure Portal](https://portal.azure.com) en ga naar uw Azure Cosmos DB-account.

   * Open het deelvenster **Verbindingsreeks**, met daarin alle gegevens die u nodig hebt om met behulp van cqlsh verbinding te maken met uw Cassandra API-account.

2. Meld u aan bij cqhsh met de verbindingsgegevens uit de portal.

3. Gebruik de CQL-opdracht COPY om lokale gegevens te kopiëren naar het Cassandra API-account.

   ```bash
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="migrate-data-using-spark"></a>Gegevens migreren met Spark 

Gebruik de volgende stappen om gegevens met Spark naar het Azure Cosmos DB Cassandra API-account te migreren:

- Richt een [Azure Databricks](cassandra-spark-databricks.md)- of een [HDInsight-cluster](cassandra-spark-hdinsight.md) in 

- Verplaats gegevens naar een Cassandra API-eindpunt met behulp van een [tabel-kopieerbewerking](cassandra-spark-table-copy-ops.md) 

Het migreren van gegevens met behulp van Spark-taken wordt aanbevolen als er gegevens aanwezig zijn in een bestaand cluster op virtuele Azure-machines of in een andere cloud. Voor deze optie moet Spark voor eenmalige of regelmatige gegevensopname worden ingesteld als intermediair. U kunt deze migratie versnellen met behulp van ExpressRoute-connectiviteit tussen on-premises en Azure. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u gegevens migreert naar een Cassandra API-account van Azure Cosmos DB. U kunt nu doorgaan naar de sectie Concepten voor meer informatie over Azure Cosmos DB. 

> [!div class="nextstepaction"]
> [Instelbare gegevensconsistentieniveaus in Azure Cosmos DB](../cosmos-db/consistency-levels.md)


