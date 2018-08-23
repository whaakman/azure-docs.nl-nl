---
title: Apache Hive gebruiken als een hulpprogramma voor het ETL - Azure HDInsight
description: Apache Hive gebruiken om te extraheren, transformeren en laden (ETL) gegevens in Azure HDInsight.
services: hdinsight
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
editor: jasonwhowell
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2017
ms.openlocfilehash: 2bdfe35c7ce705966904487c3de6691e05c09098
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060620"
---
# <a name="use-apache-hive-as-an-extract-transform-and-load-etl-tool"></a>Apache Hive gebruiken als een hulpprogramma voor uitpakken, transformeren en laden (ETL)

U moet meestal opschonen en transformeren van binnenkomende gegevens voordat deze naar een bestemming die geschikt is voor de analyse wordt geladen. Extraheren, transformeren en laden (ETL)-bewerkingen worden gebruikt voor het voorbereiden van gegevens en deze te laden in een bestemming.  Hive in HDInsight kunt lezen in niet-gestructureerde gegevens, indien nodig de gegevens worden verwerkt en vervolgens de gegevens laden in een relationele datawarehouse voor besluit ondersteuning voor systemen. In deze benadering, worden gegevens opgehaald uit de bron en opgeslagen in schaalbare opslag, zoals Azure Storage-blobs of Azure Data Lake Store. De gegevens vervolgens worden getransformeerd met behulp van een reeks van Hive-query's en ten slotte wordt klaargezet binnen Hive in voorbereiding voor bulksgewijs laden in het beoogde gegevensarchief.

## <a name="use-case-and-model-overview"></a>Overzicht van de aanvraag en modellen gebruiken

De volgende afbeelding toont een overzicht van de use-case en het model voor het automatiseren van ETL. Invoergegevens getransformeerd is voor het genereren van de gewenste uitvoer.  Tijdens deze transformatie kunt de gegevens vormgeven, gegevenstype en zelfs taal wijzigen.  ETL-processen kunnen Engels stelsel converteren naar metrische gegevens, tijdzones wijzigen en verbeteren van de precisie van goed zijn afgestemd op bestaande gegevens in het doel.  ETL-processen kunnen ook nieuwe gegevens met bestaande gegevens up-to-date houden reporting of bieden meer inzicht in uw bestaande gegevens combineren.  Toepassingen, zoals rapportage-hulpprogramma's en services kunnen vervolgens deze gegevens in de gewenste indeling gebruiken.

![Apache Hive als ETL](./media/apache-hadoop-using-apache-hive-as-an-etl-tool/hdinsight-etl-architecture.png)

Hadoop wordt doorgaans gebruikt in de ETL-processen die ofwel een groot aantal tekstbestanden (zoals CSV's) of een kleiner, maar vaak wijzigen, aantal tekstbestanden, of beide importeren.  Hive is een uitstekend hulpprogramma voor het gebruik van het voorbereiden van de gegevens voordat deze naar de gegevensbestemming worden geladen.  Hive kunt u een schema maken via de CSV en gebruiken van een SQL-achtige taal voor het genereren van MapReduce-programma's die met de gegevens communiceren. 

De gebruikelijke stappen voor het gebruik van Hive om uit te voeren ETL zijn als volgt:

1. Gegevens laden in Azure Data Lake Store of Azure Blob Storage.
2. Maak een metagegevens Store-database (met behulp van Azure SQL Database) voor gebruik door Hive bij het opslaan van uw schema's.
3. Een HDInsight-cluster maakt en verbinding maken met het gegevensarchief.
4. Definiëren welk schema moet worden toegepast op het moment van lezen van gegevens in het gegevensarchief:

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

5. De gegevens te transformeren en laden naar de bestemming.  Er zijn verschillende manieren om Hive gebruiken tijdens de transformatie en laden:

    * Query's uitvoeren en voorbereiden van gegevens met Hive en sla deze op als een CSV in het Azure Data Lake Store of Azure blob-opslag.  Gebruik vervolgens een hulpprogramma zoals SQL Server Integration Services (SSIS) te verwerven die CSV's en de gegevens in een relationele database van de bestemming, zoals SQL Server laden.
    * De gegevens rechtstreeks vanuit Excel of C# met behulp van het Hive ODBC-stuurprogramma op te vragen.
    * Gebruik [Apache Sqoop](apache-hadoop-use-sqoop-mac-linux.md) naar de voorbereide platte CSV-bestanden lezen en te laden in de relationele database.

## <a name="data-sources"></a>Gegevensbronnen

Gegevensbronnen worden doorgaans externe gegevens die kunnen worden afgestemd op bestaande gegevens in uw gegevensopslag, bijvoorbeeld:

* Gegevens van sociale media, logboekbestanden, sensoren en toepassingen die gegevens genereren.
* Gegevenssets ophalen uit-gegevensproviders, zoals de statistieken weer of de leverancier van verkoopcijfers.
* Streaming-gegevens vastgelegd, gefilterd en verwerkt via een geschikt hulpprogramma of framework.

<!-- TODO: (see Collecting and loading data into HDInsight). -->

## <a name="output-targets"></a>Uitvoer-doelen

U kunt Hive uitvoergegevens naar diverse doelen, met inbegrip van:

* Een relationele database, zoals SQL Server of Azure SQL Database.
* Een datawarehouse, zoals Azure SQL Data Warehouse.
* Excel.
* Azure table en blob storage.
* Toepassingen of services waarvoor gegevens moeten worden verwerkt in specifieke indelingen vereist of als bestanden die bevatten specifieke soorten informatiestructuur.
* Een JSON-Document-Store, zoals <a href="https://azure.microsoft.com/services/cosmos-db/">CosmosDB</a>.

## <a name="considerations"></a>Overwegingen

De ETL-model wordt doorgaans gebruikt wanneer u wilt:

* Streaminggegevens of grote volumes van semi-gestructureerde of ongestructureerde gegevens uit externe bronnen in een bestaande database of een systeem voor klantinformatie laden.
* Opschonen, transformeren, en de gegevens te valideren voordat deze worden geladen, mogelijk met behulp van meer dan één transformatie doorgeven via het cluster.
* Genereren van rapporten en visualisaties die regelmatig worden bijgewerkt.  Bijvoorbeeld, als het rapport te genereren tijdens de dag lang duurt, kunt u het rapport om uit te voeren 's nachts plannen.  U kunt Azure Scheduler en PowerShell automatisch een Hive-query wordt uitgevoerd.

Als het doel van de gegevens niet gelijk is aan een database, kunt u een bestand in de juiste indeling in de query, bijvoorbeeld een CSV genereren. Dit bestand kan vervolgens worden geïmporteerd in Excel of Power BI.

Als u nodig hebt voor het uitvoeren van verschillende bewerkingen van de gegevens als onderdeel van het ETL-proces, kunt u overwegen hoe u ze beheren. Als de bewerkingen worden beheerd door een extern programma, moet in plaats van als een werkstroom in de oplossing, u om te bepalen of bepaalde bewerkingen tegelijk kunnen worden uitgevoerd, en om te detecteren wanneer elke taak is voltooid. Met behulp van een werkstroom-mechanisme, zoals Oozie in Hadoop is mogelijk eenvoudiger dan bij voor het indelen van een reeks bewerkingen met behulp van externe scripts of aangepaste programma's. Zie voor meer informatie over Oozie [indelen van werkstroom en taak](https://msdn.microsoft.com/library/dn749829.aspx).

## <a name="next-steps"></a>Volgende stappen

* [ETL op schaal](apache-hadoop-etl-at-scale.md)
* [Een gegevenspijplijn operationeel maken](../hdinsight-operationalize-data-pipeline.md)

<!-- * [ETL Deep Dive](../hdinsight-etl-deep-dive.md) -->
