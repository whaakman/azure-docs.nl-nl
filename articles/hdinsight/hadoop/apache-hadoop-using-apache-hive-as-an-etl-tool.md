---
title: Apache Hive gebruiken als een hulpprogramma voor het ETL - Azure HDInsight | Microsoft Docs
description: Gebruik de Apache Hive uitpakken, transformeren en laden (ETL) gegevens in Azure HDInsight.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: 1ccbfe23e9c887a98a0dbfa8031078a15c6e41b6
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="use-apache-hive-as-an-extract-transform-and-load-etl-tool"></a>Apache Hive gebruiken als een hulpprogramma voor uitpakken, transformeren en Load (ETL)

U moet doorgaans te reinigen en transformeren van binnenkomende gegevens voordat het laden in een geschikt is voor analytics bestemming. Bewerkingen voor uitpakken, transformeren en laden (ETL) worden gebruikt voor het voorbereiden van gegevens en in een gegevensbestemming laden.  Hive in HDInsight kunt lezen in ongestructureerde gegevens, verwerken van de gegevens zo nodig en vervolgens de gegevens in een relationele datawarehouse voor besluit ondersteuning voor systemen te laden. In deze benadering gegevens opgehaald uit de bron en opgeslagen in schaalbare opslag, zoals Azure Storage blobs of Azure Data Lake Store. De gegevens vervolgens wordt omgezet met behulp van een reeks van Hive-query's en tot slot gefaseerde binnen Hive in voorbereiding voor bulksgewijs laden in het gegevensarchief van de bestemming.

## <a name="use-case-and-model-overview"></a>Overzicht van de aanvraag en -model gebruiken

De volgende afbeelding toont een overzicht van de gebruiksvoorbeeld en het model voor ETL-automatisering. Invoergegevens wordt voor het genereren van de juiste uitvoer omgezet.  Tijdens deze transformatie worden de gegevens kunt wijzigen vorm, gegevenstype en zelfs taal.  ETL-processen kunnen Engels stelsel converteren naar metriek, tijdzones wijzigen en precisie goed zijn uitgelijnd met de bestaande gegevens in de doel-verbeteren.  ETL-processen kunnen ook nieuwe gegevens met bestaande gegevens voor het up-to-date houden reporting of om meer inzicht in de bestaande gegevens combineren.  Toepassingen zoals reporting hulpprogramma's en services kunnen vervolgens deze gegevens in de gewenste indeling gebruiken.

![Apache Hive als ETL](./media/apache-hadoop-using-apache-hive-as-an-etl-tool/hdinsight-etl-architecture.png)

Hadoop wordt doorgaans gebruikt in de ETL-processen op dat importeren van een enorm aantal tekstbestanden (zoals CSV's) of een kleinere maar vaak veranderen aantal tekstbestanden, of beide.  Hive is een uitstekend hulpprogramma voor het gebruik voor het voorbereiden van de gegevens voordat deze in het doel van de gegevens te laden.  Hive kunt u een schema maken via de CSV en gebruiken van een SQL-achtige taal voor het genereren van MapReduce-programma's die met de gegevens communiceren. 

De gebruikelijke stappen voor het gebruik van Hive om uit te voeren ETL zijn als volgt:

1. Gegevens laden in Azure Data Lake Store of Azure Blob Storage.
2. Maak een metagegevens Store-database (met behulp van Azure SQL Database) voor gebruik door Hive bij het opslaan van uw schema.
3. Maak een HDInsight-cluster en verbinding maken met het gegevensarchief.
4. Het schema toepassen op het moment van lezen van gegevens in het gegevensarchief definiëren:

    ```
    DROP TABLE IF EXISTS hvac;

    --create the hvac table on comma-separated sensor data stored in Azure Storage blobs
    
    CREATE EXTERNAL TABLE hvac(`date` STRING, time STRING, targettemp BIGINT,
        actualtemp BIGINT, 
        system BIGINT, 
        systemage BIGINT, 
        buildingid BIGINT)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    STORED AS TEXTFILE LOCATION 'wasb://{container}@{storageaccount}.blob.core.windows.net/HdiSamples/SensorSampleData/hvac/';
    ```

5. De gegevens transformeren en laden in de bestemming.  Er zijn verschillende manieren voor gebruik van Hive tijdens het transformatie en laden:

    * Query en voorbereiden van gegevens met Hive en sla deze op als een CSV in Azure Data Lake Store of Azure blob-opslag.  Vervolgens gebruiken een hulpprogramma zoals SQL Server Integration Services (SSIS) voor de CSV's verkrijgen en de gegevens in een relationele database bestemming zoals SQL Server laden.
    * Query uitvoeren op de gegevens rechtstreeks vanuit Excel of C# met het Hive ODBC-stuurprogramma.
    * Gebruik [Apache Sqoop](apache-hadoop-use-sqoop-mac-linux.md) voorbereide platte CSV-bestanden lezen en in de doeldatabase voor relationele geladen.

## <a name="data-sources"></a>Gegevensbronnen

Gegevensbronnen worden doorgaans externe gegevens die kunnen worden afgestemd op bestaande gegevens in uw data store, bijvoorbeeld:

* Gegevens van sociale media, logboekbestanden, sensoren en toepassingen die gegevensbestanden genereren.
* Gegevenssets is verkregen van gegevensproviders, zoals de statistieken weer of de leverancier van verkoopcijfers.
* Gegevensstromen vastgelegd, gefilterd en verwerkt via een geschikt hulpprogramma of framework.

<!-- TODO: (see Collecting and loading data into HDInsight). -->

## <a name="output-targets"></a>Doelen voor uitvoer

U kunt Hive uitvoergegevens naar allerlei doelen, met inbegrip van:

* Een relationele database, zoals SQL Server of Azure SQL Database.
* Een datawarehouse, zoals Azure SQL Data Warehouse.
* Excel.
* Azure tabel en de blob-opslag.
* Toepassingen of services die gegevens worden verwerkt tot specifieke indelingen vereisen, of omdat de bestanden die bevatten specifieke typen informatiestructuur.
* Een archief JSON-Document, zoals <a href="https://azure.microsoft.com/services/cosmos-db/">CosmosDB</a>.

## <a name="considerations"></a>Overwegingen

Het ETL-model wordt doorgaans gebruikt wanneer u wilt:

* Stroomgegevens of grote volumes met semi-gestructureerde of ongestructureerde gegevens uit externe bronnen laden in een bestaande database of een systeem voor informatie.
* Opschonen, transformeren, en de gegevens worden gevalideerd voordat deze worden geladen, mogelijk met behulp van meer dan één transformatie doorgeven het cluster.
* Genereren van rapporten en visualisaties die regelmatig worden bijgewerkt.  Als het rapport te lang om te genereren tijdens de dag duurt, kunt u bijvoorbeeld het rapport uitvoeren nachts plannen.  U kunt Azure Scheduler en PowerShell automatisch een Hive-query wordt uitgevoerd.

Als het doel van de gegevens niet een database, genereert u een bestand in de juiste notatie binnen de query, bijvoorbeeld een CSV. Dit bestand kan vervolgens worden geïmporteerd in Excel of Power BI.

Als u verschillende bewerkingen op de gegevens die worden uitgevoerd als onderdeel van het ETL-proces moet, kunt u overwegen hoe u ze beheren. Als de bewerkingen worden beheerd door een extern programma, moet en niet als een werkstroom binnen de oplossing u om te bepalen of bepaalde bewerkingen parallel kunnen worden uitgevoerd en om te detecteren wanneer elke taak is voltooid. Met een werkstroom mechanisme zoals Oozie in Hadoop is mogelijk beter dan bij het indelen van een reeks bewerkingen met externe scripts of op aangepaste programma's. Zie voor meer informatie over Oozie [werkstroom en taak orchestration](https://msdn.microsoft.com/library/dn749829.aspx).

<!-- ## Next steps -->
<!-- * [ETL at scale](../hdinsight-etl-at-scale.md): Learn more about performing ETL at scale. -->
<!-- * [Operationalize Data Pipelines with Oozie](hdinsight-operationalize-data-pipeline.md): Learn how to build a data pipeline that uses Hive to summarize CSV flight delay data, stage the prepared data in Azure Storage blobs, and then use Sqoop to load the summarized data into Azure SQL Database. -->
<!-- * [ETL Deep Dive](../hdinsight-etl-deep-dive.md): Walk through an end-to-end ETL pipeline.  -->
