---
title: Cassandra gegevens importeren in Azure Cosmos DB | Microsoft Docs
description: "Informatie over het gebruik van de opdracht Copy CQL Cassandra om gegevens te kopiëren naar Azure Cosmos DB."
services: cosmos-db
author: govindk
manager: jhubbard
documentationcenter: 
ms.assetid: eced5f6a-3f56-417a-b544-18cf000af33a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: govindk
ms.custom: mvc
ms.openlocfilehash: 21168d0862cfdaaaced60fa80a2dc04859f49550
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="azure-cosmos-db-import-cassandra-data"></a>Azure Cosmos DB: Import Cassandra gegevens

Deze zelfstudie vindt u instructies voor Cassandra gegevens importeren in Azure Cosmos DB met de opdracht Cassandra Query Language (CQL) kopiëren. 

Deze zelfstudie bevat de volgende taken:

> [!div class="checklist"]
> * Bij het ophalen van de verbindingsreeks
> * Gegevens importeren met behulp van de opdracht cqlsh kopiëren
> * Importeren met behulp van de Spark-connector 

# <a name="prerequisites"></a>Vereisten

* Installeer [Apache Cassandra](http://cassandra.apache.org/download/) en in het bijzonder zorgen ervoor *cqlsh* aanwezig is.
* Verhoogt de doorvoer: de duur van de migratie is afhankelijk van de hoeveelheid doorvoer die u hebt ingericht voor uw tabellen. Zorg ervoor dat de doorvoer voor grotere gegevens migraties verhogen. Nadat u de migratie hebt voltooid, verlaagt u de doorvoer voor het opslaan van kosten. Voor meer informatie over de toenemende doorvoer in de [Azure-portal](https://portal.azure.com), Zie [Set doorvoer voor Azure Cosmos DB containers](set-throughput.md).
* Schakel SSL: Azure Cosmos DB heeft strenge beveiligingsvereisten en standaarden. Zorg ervoor dat SSL in te schakelen wanneer u met uw account communiceert. Wanneer u CQL met SSH gebruiken, hebt u een optie om SSL-informatie te geven. 

## <a name="find-your-connection-string"></a>De verbindingsreeks vinden

1. In de [Azure-portal](https://portal.azure.com), op het uiterst linkse, klikt u op **Azure Cosmos DB**.

2. In de **abonnementen** deelvenster, selecteer de accountnaam van uw.

3. Klik op **verbindingsreeks**. Het rechter deelvenster bevat alle informatie die u nodig hebt om verbinding te maken aan uw account.

    ![Pagina van de tekenreeks verbinding](./media/cassandra-import-data/keys.png)

## <a name="use-cqlsh-copy"></a>Gebruik cqlsh kopiëren

Cassandra om gegevens te importeren in Azure Cosmos DB voor gebruik met de Cassandra-API, gebruik de volgende richtlijnen:

1. Aanmelden bij cqhsh met de verbindingsgegevens van de portal.
2. Gebruik de [opdracht CQL kopiëren](http://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) lokale gegevens kopiëren naar het eindpunt van de Apache Cassandra API. Controleer of de bron en doel in hetzelfde datacenter te minimaliseren latentieproblemen.

### <a name="guide-for-moving-data-with-cqlsh"></a>Handleiding voor het verplaatsen van gegevens met cqlsh

1. Vooraf maken en schalen van uw tabel:
    * Standaard Azure Cosmos DB voorziet in een nieuwe Cassandra API tabel met 1000 aanvraageenheden per seconde (RU/s) (maken op basis van CQL is ingericht met 400 RU/s). Voordat u de migratie met behulp van cqlsh, vooraf maken van alle tabellen van de [Azure-portal](https://portal.azure.com) of van cqlsh. 

    * Van de [Azure-portal](https://portal.azure.com), de doorvoer van uw tabellen van de standaard-doorvoer (400 of 1000 RU/s) verhogen tot 10.000 RU/s voor de duur van de migratie. U kunt met de hogere doorvoer voorkomen bandbreedtebeperking en migreren in minder tijd. Met elk uur facturering in Azure Cosmos DB, kunt u de doorvoer reduceren onmiddellijk na de migratie om kosten te besparen.

2. Bepaal de RU kosten voor een bewerking. U kunt dit doen met behulp van Azure Cosmos DB Cassandra API SDK van uw keuze. In dit voorbeeld ziet de .NET-versie van het ophalen van RU kosten. 

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

3. Bepaal de latentie van uw computer naar de service Azure Cosmos DB. Als u binnen een Azure-Datacenter, moet de latentie een lage één milliseconde cijfers. Als u zich buiten het Datacenter Azure, kunt u gebruiken psping om of azurespeed.com ophalen van de geschatte latentie van uw locatie.   

4. Berekenen van de juiste waarden voor parameters (INGESTRATE NUMPROCESS MAXBATCHSIZE of MINBATCHSIZE) die goede prestaties bieden. 

5. Voer de laatste migratie-opdracht. Met deze opdracht wordt ervan uitgegaan cqlsh met behulp van de verbindingsinformatie hebt gestart.

   ```
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="use-spark-to-import-data"></a>Spark gebruiken om gegevens te importeren

Voor gegevens die zich in een bestaand cluster in Azure virtuele machines, is het importeren van gegevens met Spark ook haalbare optie. Hiervoor moet Spark voor één keer of reguliere opname als intermediair worden ingesteld. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de volgende taken uitvoeren:

> [!div class="checklist"]
> * De verbindingsreeks ophalen
> * Gegevens importeren met behulp van de opdracht copy cql
> * Importeren met behulp van de Spark-connector 

U kunt nu doorgaan naar de sectie concepten voor meer informatie over Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Gegevens instelbare consistentieniveaus in Azure Cosmos-DB](../cosmos-db/consistency-levels.md)
