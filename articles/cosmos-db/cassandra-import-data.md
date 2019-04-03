---
title: 'Zelfstudie: Gegevens migreren naar een Cassandra-API-account in Azure Cosmos DB'
description: In deze zelfstudie leert u hoe u met de CQL-opdracht Copy en Spark gegevens van Apache Cassandra kopieert naar een Cassandra-API-account in Azure Cosmos DB.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 12/03/2018
ms.custom: seodec18
Customer intent: As a developer, I want to migrate my existing Cassandra workloads to Azure Cosmos DB so that the overhead to manage resources, clusters, and garbage collection is automatically handled by Azure Cosmos DB.
ms.openlocfilehash: cc312a707f5ab74967b9d3bc050fec7bfcad9dbc
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851070"
---
# <a name="tutorial-migrate-your-data-to-cassandra-api-account-in-azure-cosmos-db"></a>Zelfstudie: Gegevens migreren naar Cassandra-API-account in Azure Cosmos DB

Als ontwikkelaar hebt u mogelijk bestaande Cassandra-workloads die on-premises worden uitgevoerd of in de cloud, die u mogelijk wilt migreren naar Azure. U kunt dergelijke workloads migreren naar een Cassandra-API-account in Azure Cosmos DB. In deze zelfstudie vindt u instructies over verschillende opties voor het migreren van Apache Cassandra-gegevens naar het Cassandra-API-account in Azure Cosmos DB.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Plan voor migratie
> * Vereisten voor migratie
> * Gegevens migreren met de cqlsh-opdracht COPY
> * Gegevens migreren met Spark

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites-for-migration"></a>Vereisten voor migratie

* **Maak een schatting van de doorvoervereisten:** Voordat u gegevens migreert naar het Cassandra-API-account in Azure Cosmos DB, moet u een schatting maken van de doorvoervereisten voor uw workload. In het algemeen verdient het aanbeveling om te beginnen met de gemiddelde doorvoer die is vereist voor de CRUD-bewerkingen en vervolgens de aanvullende doorvoer toe te voegen die vereist is voor ETL-bewerkingen (Extract Transform Load) of piekbewerkingen. U hebt de volgende gegevens nodig om de migratie te plannen: 

  * **Bestaande of geschatte gegevensgrootte:** Hiermee definieert u de minimale databasegrootte en doorvoervereiste. Als u een schatting maakt van de gegevensgrootte voor een nieuwe toepassing, kunt u ervan uitgaan dat de gegevens gelijkmatig worden verdeeld over de rijen en een schatting van de waarde maken door deze te vermenigvuldigen met de gegevensgrootte. 

  * **Vereiste doorvoer:** Een schatting van de doorvoersnelheid voor lezen (query/get) en schrijven (update/delete/insert). Deze waarde is vereist voor het berekenen van de vereiste aanvraageenheden en de gegevensgrootte bij een stabiele toestand.  

  * **Het schema:** Maak verbinding met uw bestaande Cassandra-cluster via cqlsh en exporteer het schema uit Cassandra: 

    ```bash
    cqlsh [IP] "-e DESC SCHEMA" > orig_schema.cql
    ```

    Nadat u de vereisten van uw bestaande workload hebt geïdentificeerd, maakt u een Azure Cosmos-account, database en containers op basis van de verzamelde vereisten voor de doorvoer.  

  * **De RU-kosten voor een bewerking bepalen:** U kunt de RU's bepalen met behulp van een van de SDK's die worden ondersteund door de Cassandra-API. In dit voorbeeld ziet u de .NET-versie van het ophalen van RU-kosten.

    ```csharp
    var tableInsertStatement = table.Insert(sampleEntity);
    var insertResult = await tableInsertStatement.ExecuteAsync();

    foreach (string key in insertResult.Info.IncomingPayload)
      {
         byte[] valueInBytes = customPayload[key];
         double value = Encoding.UTF8.GetString(valueInBytes);
         Console.WriteLine($"CustomPayload:  {key}: {value}");
      }
    ```

* **De vereiste doorvoer toewijzen:** Azure Cosmos DB kan opslag en doorvoer automatisch schalen als uw vereisten veranderen. U kunt uw doorvoer schatten met behulp van de [calculator voor aanvraageenheden en gegevensopslag](https://www.documentdb.com/capacityplanner). 

* **Tabellen maken in het Cassandra API-account:** Voordat u gegevens gaat migreren, maakt u vooraf alle tabellen vanuit Azure Portal of met cqlsh. Als u migreert naar een Azure Cosmos-account dat doorvoer op databaseniveau heeft, moet u een partitiesleutel opgeven bij het maken van de Azure Cosmos-containers.

* **Doorvoer vergroten:** De duur van de gegevensmigratie is afhankelijk van de hoeveelheid doorvoer die u voor de tabellen hebt ingericht in Azure Cosmos DB. Verhoog de doorvoer voor de duur van de migratie. Met een hogere doorvoer voorkomt u frequentielimieten en kost migreren minder tijd. Nadat u de migratie hebt voltooid, verlaagt u de doorvoer om kosten te besparen. Het is ook raadzaam om het Azure Cosmos-account in dezelfde regio te plaatsen als uw brondatabase. 

* **SSL inschakelen:** Voor Azure Cosmos DB gelden strenge beveiligingsvereisten en -normen. Schakel SSL in wanneer u uw account gebruikt. Wanneer u CQL gebruikt met SSH, hebt u een optie om SSL-informatie op te geven.

## <a name="options-to-migrate-data"></a>Mogelijkheden voor migreren van gegevens

U kunt gegevens op de volgende manieren verplaatsen van bestaande Cassandra-workloads naar Azure Cosmos DB:

* [Met de cqlsh-opdracht COPY](#migrate-data-using-cqlsh-copy-command)  
* [Met Spark](#migrate-data-using-spark) 

## <a name="migrate-data-using-cqlsh-copy-command"></a>Gegevens migreren met de cqlsh-opdracht COPY

De [opdracht CQL COPY](https://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) wordt gebruikt om lokale gegevens te kopiëren naar het Cassandra API-account in Azure Cosmos DB. Voer de volgende stappen uit om gegevens te kopiëren:

1. Vraag de verbindingsreeks van uw Cassandra API-account op:

   * Meld u aan bij de [Azure-portal](https://portal.azure.com) en ga naar uw Azure Cosmos-account.

   * Open het deelvenster **Verbindingsreeks**, met daarin alle gegevens die u nodig hebt om met behulp van cqlsh verbinding te maken met uw Cassandra API-account.

2. Meld u aan bij cqhsh met de verbindingsgegevens uit de portal.

3. Gebruik de CQL-opdracht COPY om lokale gegevens te kopiëren naar het Cassandra API-account.

   ```bash
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="migrate-data-using-spark"></a>Gegevens migreren met Spark 

Gebruik de volgende stappen om gegevens met Spark naar het Cassandra API-account te migreren:

- Richt een [Azure Databricks-cluster](cassandra-spark-databricks.md) of een [HDInsight-cluster](cassandra-spark-hdinsight.md) in 

- Verplaats gegevens naar het Cassandra API-eindpunt met behulp van een [tabel-kopieerbewerking](cassandra-spark-table-copy-ops.md) 

Het migreren van gegevens met behulp van Spark-taken wordt aanbevolen als er gegevens aanwezig zijn in een bestaand cluster op virtuele Azure-machines of in een andere cloud. Voor deze optie moet Spark als intermediair voor eenmalige of regelmatige gegevensopname worden ingesteld. U kunt deze migratie versnellen met behulp van Azure ExpressRoute-connectiviteit tussen on-premises en Azure. 

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de resourcegroep, het Azure Cosmos-account en alle gerelateerde resources verwijderen wanneer u ze niet meer nodig hebt. Daarvoor selecteert u de resourcegroep voor de virtuele machine en selecteert u **Verwijderen**. Vervolgens bevestigt u de naam van de resourcegroep die u wilt verwijderen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u gegevens migreert naar een Cassandra API-account in Azure Cosmos DB. U kunt nu doorgaan naar het volgende artikel voor meer informatie over andere concepten voor Azure Cosmos DB:

> [!div class="nextstepaction"]
> [Instelbare gegevensconsistentieniveaus in Azure Cosmos DB](../cosmos-db/consistency-levels.md)


