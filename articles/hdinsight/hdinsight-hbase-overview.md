---
title: Wat is HBase in Azure HDInsight? | Microsoft Docs
description: Een inleiding tot Apache HBase in HDInsight, een NoSQL-database gebaseerd op Hadoop. Meer informatie over de toepassingsmogelijkheden en HBase vergelijken met andere Hadoop-clusters.
keywords: bigtable, nosql, wat is hbase, apache hbase, hbase, hbase-overzicht,
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: d2a76d53-133a-4849-a30c-88d9c794391c
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 62b2700c368c4b0c66df140e8961408840beb626
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Wat is HBase in HDInsight: een NoSQL-database die op BigTable gelijkende mogelijkheden voor Hadoop levert
Apache HBase is een open-source NoSQL-database die is gebaseerd op Hadoop en die is gemodelleerd naar Google BigTable. HBase biedt willekeurige toegang en een sterke consistentie voor grote hoeveelheden ongestructureerde en semigestructureerde gegevens in een database zonder schema die is georganiseerd op basis van kolomfamilies.

De gegevens worden opgeslagen in de rijen van een tabel en de gegevens in een rij worden gegroepeerd op basis van de kolomfamilie. HBase is een database zonder schema in de zin dat zowel de kolommen als het type gegevens dat hierin wordt opgeslagen niet hoeven te worden gedefinieerd voordat u ze kunt gebruiken. De open-source code wordt lineair geschaald om petabytes aan gegevens op duizenden knooppunten te verwerken. Hiervoor kan gebruik worden gemaakt van gegevensredundantie, batchverwerking en andere functies die worden geboden door gedistribueerde toepassingen in het Hadoop-ecosysteem.

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>How wordt HBase geïmplementeerd in Azure HDInsight?
HDInsight HBase wordt aangeboden als een beheerd cluster dat is geïntegreerd in de Azure-omgeving. De clusters zijn geconfigureerd om gegevens rechtstreeks op te slaan in [Azure Storage](./hdinsight-hadoop-use-blob-storage.md) of [Azure Data Lake Store](./hdinsight-hadoop-use-data-lake-store.md), voor een lage latentie en verbeterde elasticiteit met betrekking tot de prestatie- en kostenopties. Hierdoor kunnen klanten interactieve websites bouwen die geschikt zijn voor grote gegevenssets. Daarnaast kunnen ze services bouwen voor het opslaan van sensor- en telemetriegegevens van miljoenen eindpunten en deze gegevens analyseren met Hadoop-taken. HBase en Hadoop vormen een goed startpunt voor big data-projecten in Azure. Met name omdat ze ervoor zorgen dat realtime toepassingen met grote gegevenssets kunnen werken.

De HDInsight-implementatie maakt gebruik van de opschaalbare architectuur van HBase om automatische sharding van tabellen, een sterke consistentie voor lees- en schrijfbewerkingen en automatische failover te bieden. De prestaties zijn verbeterd dankzij in-memory caching voor leesbewerkingen en streamen met een hoge gegevensdoorvoer voor schrijfbewerkingen. Een HBase-cluster kan worden gemaakt in het virtuele netwerk. Zie [HDInsight-clusters maken in Azure Virtual Network][hbase-provision-vnet] voor meer informatie.

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Hoe worden gegevens in HDInsight HBase beheerd?
Gegevens kunnen worden beheerd in HBase met de opdrachten `create`, `get`, `put` en `scan` in de HBase-shell. Gegevens worden met `put` naar de database geschreven en gelezen met `get`. De opdracht `scan` wordt gebruikt om gegevens uit meerdere rijen in een tabel op te halen. Gegevens kunnen ook worden beheerd met de HBase C# API, die naast de HBase REST API een clientbibliotheek biedt. Er kan ook een query op de HBase-database worden uitgevoerd met Hive. Zie [Aan de slag met HBase met Hadoop in HDInsight][hbase-get-started]voor een inleiding over deze programmeermodellen. Er zijn ook co-processoren beschikbaar. Hiermee kunnen de gegevens worden verwerkt in de knooppunten die de database hosten.

> [!NOTE]
> Thrift wordt niet ondersteund door HBase in HDInsight.
>

## <a name="scenarios-use-cases-for-hbase"></a>Scenario's: gebruiksvoorbeelden voor HBase
Het canonieke gebruiksvoorbeeld waarvoor BigTable (en door uitbreiding HBase) is gemaakt, was zoeken op het web. Zoekmachines bouwen indexen die termen koppelen aan de webpagina's die deze termen bevatten. Er zijn echter tal van andere gebruiksvoorbeeld waarvoor HBase geschikt is. Enkele daarvan worden gespecificeerd in deze sectie.

* Sleutel-waardearchief
  
    HBase kan worden gebruikt als een sleutel-waardearchief en is geschikt voor het beheren van berichtsystemen. Facebook gebruikt HBase voor het berichtensysteem. Daarnaast is HBase ideaal voor het opslaan en beheren van de communicatie via internet. WebTable gebruikt HBase om tabellen die worden geëxtraheerd uit webpagina's te zoeken en beheren.
* Sensorgegevens
  
    HBase is handig wanneer u gegevens wilt vastleggen die stapsgewijs uit diverse bronnen worden verzameld. Dit omvat sociale analyses, time series, het up-to-date houden van interactieve dashboards met trends en prestatiemeteritems en het beheren van auditlogboeksystemen. Voorbeelden zijn Bloomberg-traderterminal en de Open Time Series Database (OpenTSDB) voor het opslaan van en bieden van toegang tot metrische gegevens die worden verzameld over de status van serversystemen.
* Realtime query
  
    [Phoenix](http://phoenix.apache.org/) is een SQL-query-engine voor Apache HBase. Deze is toegankelijk als een JDBC-stuurprogramma en maakt het uitvoeren van query's en beheren van HBase-tabellen SQL mogelijk.
* HBase als een platform
  
    Toepassingen kunnen worden uitgevoerd in HBase door HBase te gebruiken als gegevensopslag. Voorbeelden hiervan zijn Phoenix, OpenTSDB, Kiji en Titan. Toepassingen kunnen ook worden geïntegreerd met HBase. Voorbeelden hiervan zijn Hive, Pig, Solr, Storm, Flume, Impala, Spark, Ganglia en Drill.

## <a name="next-steps"></a>Volgende stappen
* [Aan de slag met HBase en Hadoop in HDInsight][hbase-get-started]
* [HDInsight-clusters maken in Azure Virtual Network][hbase-provision-vnet]
* [HBase-replicatie in HDInsight configureren](hdinsight-hbase-replication.md)
* [Maven gebruiken om Java-apps te bouwen die HBase gebruiken met HDinsight (Hadoop)][hbase-build-java-maven]

## <a name="see-also"></a>Zie ook
* [Apache HBase](https://hbase.apache.org/)
* [Bigtable: een gedistribueerd opslagsysteem voor gestructureerde gegevens](http://research.google.com/archive/bigtable.html)

[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
