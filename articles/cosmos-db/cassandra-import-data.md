---
title: Cassandra-gegevens importeren in Azure Cosmos DB | Microsoft Docs
description: Lees hoe u met de CQL-opdracht COPY Cassandra-gegevens kunt kopiëren naar Azure Cosmos DB.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: govindk
ms.custom: mvc
ms.openlocfilehash: b53328875f2242faba369dea0df655bc78117009
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "41919111"
---
# <a name="azure-cosmos-db-import-cassandra-data"></a>Azure Cosmos DB: Cassandra-gegevens importeren

In deze zelfstudie vindt u instructies voor het importeren van Cassandra-gegevens in Azure Cosmos DB met de CQL-opdracht (Cassandra Query Language) COPY. 

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * De verbindingsreeks ophalen
> * Gegevens importeren met behulp van de cqlsh-opdracht COPY
> * Importeren met de Spark-connector 

# <a name="prerequisites"></a>Vereisten

* Installeer [Apache Cassandra](http://cassandra.apache.org/download/) en zorg er met name voor dat *cqlsh* aanwezig is.
* Verhoog de doorvoer: de duur van de gegevensmigratie is afhankelijk van de hoeveelheid doorvoer die u voor uw tabellen hebt ingericht. Verhoog de doorvoer voor grotere gegevensmigraties. Nadat u de migratie hebt voltooid, verlaagt u de doorvoer om kosten te besparen. Zie [Doorvoer instellen voor Azure Cosmos DB](set-throughput.md) voor meer informatie over het verhogen van de doorvoer in [Azure Portal](https://portal.azure.com).
* Schakel SSL in: voor Azure Cosmos DB gelden strenge beveiligingsvereisten en -normen. Schakel SSL in wanneer u uw account gebruikt. Wanneer u CQL gebruikt met SSH, hebt u een optie om SSL-informatie op te geven. 

## <a name="find-your-connection-string"></a>Uw verbindingsreeks zoeken

1. Klik in [Azure Portal](https://portal.azure.com) uiterst links op **Azure Cosmos DB**.

2. Selecteer in het deelvenster **Abonnementen** uw accountnaam.

3. Klik op **Verbindingsreeks**. Het rechterdeelvenster bevat alle informatie die u nodig hebt om verbinding te maken met uw account.

    ![Pagina Verbindingsreeks](./media/cassandra-import-data/keys.png)

## <a name="use-cqlsh-copy"></a>Cqlsh-opdracht COPY gebruiken

Als u Cassandra-gegevens wilt importeren in Azure Cosmos DB voor gebruik met de Cassandra-API, gebruikt u de volgende richtlijnen:

1. Meld u aan bij cqhsh met de verbindingsgegevens van de portal.
2. Gebruik de [CQL-opdracht COPY](http://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) om lokale gegevens te kopiëren naar het eindpunt van de Apache Cassandra-API. Controleer of de bron en het doel zich in hetzelfde datacenter bevinden om latentieproblemen te minimaliseren.

### <a name="guide-for-moving-data-with-cqlsh"></a>Handleiding voor het verplaatsen van gegevens met cqlsh

1. Maak van tevoren de tabel en schaal deze:
    * Standaard wordt met Azure Cosmos DB een nieuwe Cassandra-API-tabel ingericht met 1000 aanvraageenheden per seconde (RU/s) (bij gebruik van CQL wordt de tabel ingericht met 400 RU/s). Voordat u de migratie start met cqlsh, maakt u vooraf alle tabellen vanuit [Azure Portal](https://portal.azure.com) of cqlsh. 

    * Verhoog de doorvoer van de tabellen vanuit [Azure Portal](https://portal.azure.com) van de standaarddoorvoer (400 of 1000 RU/s) naar 10.000 RU/s voor de duur van de migratie. Met een hogere doorvoer voorkomt u frequentielimieten en kost migreren minder tijd. Met facturering per uur in Azure Cosmos DB kunt u de doorvoer onmiddellijk na de migratie verminderen om kosten te besparen.

2. Bepaal de RU-kosten voor een bewerking. U kunt dit doen met de Azure Cosmos DB Cassandra-API SDK van uw keuze. In dit voorbeeld ziet u de .NET-versie van het ophalen van RU-kosten. 

    ```csharp
    var tableInsertStatement = table.Insert(sampleEntity);
    var insertResult = await tableInsertStatement.ExecuteAsync();

    foreach (string key in insertResult.Info.IncomingPayload)
            {
                byte[] valueInBytes = customPayload[key];
                string value = Encoding.UTF8.GetString(valueInBytes);
                Console.WriteLine($“CustomPayload:  {key}: {value}”);
            }
 
    ``` 

3. Bepaal hoe lang het duurt voordat aanvragen vanaf uw computer worden beantwoord door de Azure Cosmos DB-service. Als u zich in een Azure-datacenter bevindt, moet de latentie een aantal milliseconden van één cijfer zijn. Als u zich buiten het Azure-datacenter bevindt, kunt u psping of azurespeed.com gebruiken om de geschatte latentie van uw locatie op te halen.   

4. Bereken de juiste waarden voor parameters (NUMPROCESS, INGESTRATE, MAXBATCHSIZE of MINBATCHSIZE) die goede prestaties leveren. 

5. Voer de laatste migratieopdracht uit. Als u deze opdracht uitvoert, wordt ervan uitgegaan dat u cqlsh hebt gestart met de verbindingsreeksgegevens.

   ```
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="use-spark-to-import-data"></a>Spark gebruiken om gegevens te importeren

Voor gegevens die zich in een bestaand cluster op virtuele Azure-machines bevinden, kunt u ook gegevens importeren met Spark. Hiervoor moet Spark voor eenmalige of regelmatige gegevensopname worden ingesteld als intermediair. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de volgende taken kunt uitvoeren:

> [!div class="checklist"]
> * De verbindingsreeks ophalen
> * Gegevens importeren met de CQL-opdracht COPY
> * Importeren met de Spark-connector 

U kunt nu doorgaan naar de sectie Concepten voor meer informatie over Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Instelbare gegevensconsistentieniveaus in Azure Cosmos DB](../cosmos-db/consistency-levels.md)
