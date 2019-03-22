---
title: Wat is HBase in Azure HDInsight?
description: Een inleiding tot Apache HBase in HDInsight, een NoSQL-database gebaseerd op Hadoop. Meer informatie over de toepassingsmogelijkheden en HBase vergelijken met andere Hadoop-clusters.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: hrasheed
ms.openlocfilehash: d109a1a233aaaf1f3922561106caa15781e31e01
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58337343"
---
# <a name="what-is-apache-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-apache-hadoop"></a>Wat is Apache HBase in HDInsight: Een NoSQL-database die BigTable gelijkende mogelijkheden voor Apache Hadoop biedt
[Apache HBase](https://hbase.apache.org/) is een open-source NoSQL-database die is gebouwd op [Apache Hadoop](https://hadoop.apache.org/) en gemodelleerde na [Google BigTable](https://cloud.google.com/bigtable/). HBase biedt willekeurige toegang en een sterke consistentie voor grote hoeveelheden ongestructureerde en semigestructureerde gegevens in een database zonder schema die is georganiseerd op basis van kolomfamilies.

HBase is vanuit het perspectief van gebruiker, vergelijkbaar met een database. Gegevens worden opgeslagen in de rijen en kolommen in een tabel en gegevens in een rij worden gegroepeerd per kolomfamilie. HBase is een database zonder schema in de zin dat zowel de kolommen als het type gegevens dat hierin wordt opgeslagen niet hoeven te worden gedefinieerd voordat u ze kunt gebruiken. De open-source code wordt lineair geschaald om petabytes aan gegevens op duizenden knooppunten te verwerken. Hiervoor kan gebruik worden gemaakt van gegevensredundantie, batchverwerking en andere functies die worden geboden door gedistribueerde toepassingen in het Hadoop-ecosysteem.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

## <a name="how-is-apache-hbase-implemented-in-azure-hdinsight"></a>Hoe is Apache HBase geïmplementeerd in Azure HDInsight?

HDInsight HBase wordt aangeboden als een beheerd cluster dat is geïntegreerd in de Azure-omgeving. De clusters zijn geconfigureerd voor het opslaan van gegevens rechtstreeks in [Azure Storage](./../hdinsight-hadoop-use-blob-storage.md) waarmee u lage latentie en verbeterde elasticiteit met betrekking tot prestatie- en kostenopties. Hierdoor kunnen klanten interactieve websites bouwen die geschikt zijn voor grote gegevenssets. Daarnaast kunnen ze services bouwen voor het opslaan van sensor- en telemetriegegevens van miljoenen eindpunten en deze gegevens analyseren met Hadoop-taken. HBase en Hadoop vormen een goed startpunt voor big data-projecten in Azure. Met name omdat ze ervoor zorgen dat realtime toepassingen met grote gegevenssets kunnen werken.

De HDInsight-implementatie maakt gebruik van de opschaalbare architectuur van HBase om automatische sharding van tabellen, een sterke consistentie voor lees- en schrijfbewerkingen en automatische failover te bieden. De prestaties zijn verbeterd dankzij in-memory caching voor leesbewerkingen en streamen met een hoge gegevensdoorvoer voor schrijfbewerkingen. Een HBase-cluster kan worden gemaakt in het virtuele netwerk. Zie [HDInsight-clusters maken in Azure Virtual Network](./apache-hbase-provision-vnet.md) voor meer informatie.

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Hoe worden gegevens in HDInsight HBase beheerd?
Gegevens kunnen worden beheerd in HBase met de opdrachten `create`, `get`, `put` en `scan` in de HBase-shell. Gegevens worden met `put` naar de database geschreven en gelezen met `get`. De opdracht `scan` wordt gebruikt om gegevens uit meerdere rijen in een tabel op te halen. Gegevens kunnen ook worden beheerd met de HBase C# API, die naast de HBase REST API een clientbibliotheek biedt. Een HBase-database kan ook worden opgevraagd met behulp van [Apache Hive](https://hive.apache.org/). Zie voor een inleiding tot deze programmeermodellen, [aan de slag met Apache HBase met Hadoop in HDInsight Apache](./apache-hbase-tutorial-get-started-linux.md). Er zijn ook co-processoren beschikbaar. Hiermee kunnen de gegevens worden verwerkt in de knooppunten die de database hosten.

> [!NOTE]  
> Thrift wordt niet ondersteund door HBase in HDInsight.

## <a name="scenarios-use-cases-for-apache-hbase"></a>Scenario's: Use cases voor Apache HBase
Het canonieke gebruiksvoorbeeld waarvoor bigtable (en door uitbreiding HBase) is gemaakt op basis van zoeken op het web. Zoekmachines bouwen indexen die termen koppelen aan de webpagina's die deze termen bevatten. Er zijn echter tal van andere gebruiksvoorbeeld waarvoor HBase geschikt is. Enkele daarvan worden gespecificeerd in deze sectie.

* Sleutel-waardearchief
  
    HBase kan worden gebruikt als een sleutel-waardearchief en is geschikt voor het beheren van berichtsystemen. Facebook gebruikt HBase voor het berichtensysteem. Daarnaast is HBase ideaal voor het opslaan en beheren van de communicatie via internet. WebTable gebruikt HBase om tabellen die worden geëxtraheerd uit webpagina's te zoeken en beheren.
* Sensorgegevens
  
    HBase is handig wanneer u gegevens wilt vastleggen die stapsgewijs uit diverse bronnen worden verzameld. Dit omvat sociale analyses, time series, het up-to-date houden van interactieve dashboards met trends en prestatiemeteritems en het beheren van auditlogboeksystemen. Voorbeelden zijn Bloomberg-traderterminal en de Open Time Series Database (OpenTSDB) voor het opslaan van en bieden van toegang tot metrische gegevens die worden verzameld over de status van serversystemen.
* Realtime query
  
    [Apache Phoenix](https://phoenix.apache.org/) is van een SQL-query-engine voor Apache HBase. Deze is toegankelijk als een JDBC-stuurprogramma en maakt het uitvoeren van query's en beheren van HBase-tabellen SQL mogelijk.
* HBase als een platform
  
    Toepassingen kunnen worden uitgevoerd in HBase door HBase te gebruiken als gegevensopslag. Voorbeelden hiervan zijn Phoenix, [OpenTSDB](http://opentsdb.net/), Kiji en Titan. Toepassingen kunnen ook worden geïntegreerd met HBase. Voorbeelden zijn onder meer [Apache Hive](https://hive.apache.org/), [Apache Pig](https://pig.apache.org/), [Solr](https://lucene.apache.org/solr/), [Apache Storm](https://storm.apache.org/), [Apache Flume](https://flume.apache.org/), [ Apache Impala](https://impala.apache.org/), [Apache Spark](https://spark.apache.org/) , [Ganglia](http://ganglia.info/), en [Apache Drill](https://drill.apache.org/).

## <a name="next-steps"></a>Volgende stappen
* [Aan de slag met Apache HBase met Apache Hadoop in HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [HDInsight-clusters maken in Azure Virtual Network](./apache-hbase-provision-vnet.md)
* [Apache HBase-replicatie in HDInsight configureren](apache-hbase-replication.md)
* [Apache Maven gebruiken om Java-toepassingen die gebruikmaken van Apache HBase met HDInsight (Hadoop) te bouwen](./apache-hbase-build-java-maven-linux.md)

## <a name="see-also"></a>Zie ook
* [Apache HBase](https://hbase.apache.org/)
* [Apache HBase Reference Guide](https://hbase.apache.org/book.html)
* [Bigtable: Een gedistribueerd opslagsysteem voor gestructureerde gegevens](https://research.google.com/archive/bigtable.html)
