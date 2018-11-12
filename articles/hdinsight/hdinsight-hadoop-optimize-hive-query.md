---
title: In Azure HDInsight Hive-query's optimaliseren
description: In dit artikel wordt beschreven hoe u uw Apache Hive-query's optimaliseren voor Hadoop in HDInsight.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 6729a0e3ccbb96dc178925bbab4cfbf8189c4a14
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51278256"
---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a>In Azure HDInsight Hive-query's optimaliseren

In Azure HDInsight zijn er verschillende clustertypen en technologieën die Apache Hive-query's kunnen worden uitgevoerd. Wanneer u uw HDInsight-cluster maakt, kiest u het clustertype van de juiste om u te helpen de prestaties optimaliseren voor de behoeften van uw workload. 

Bijvoorbeeld, kiest u **Interactive Query** clustertype te optimaliseren voor ad-hoc, interactieve query's. Kies Apache **Hadoop** clustertype te optimaliseren voor Hive-query's gebruikt als een batchproces. **Spark** en **HBase** clustertypen kunnen ook Hive-query's uitvoeren. Zie voor meer informatie over het uitvoeren van Hive-query's op verschillende clustertypen van HDInsight [wat is Apache Hive en HiveQL op Azure HDInsight?](hadoop/hdinsight-use-hive.md).

HDInsight-clusters van Hadoop-clustertype zijn standaard niet geoptimaliseerd voor prestaties. In dit artikel worden enkele van de meest voorkomende Hive prestaties optimalisatie methoden die u op uw query's toepassen kunt beschreven.

## <a name="scale-out-worker-nodes"></a>De schaal vergroten worker-knooppunten

Het aantal worker-knooppunten in een HDInsight-cluster verhoogt, kunt het werk over meer mappers en reducers tegelijkertijd kunnen parallel worden uitgevoerd. Er zijn twee manieren waarop u scale-out in HDInsight kunt verhogen:

* Op het moment wanneer u een cluster maakt, kunt u het aantal worker-knooppunten met behulp van de Azure portal, Azure PowerShell of opdrachtregelinterface.  Zie [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie. De volgende schermafbeelding ziet de werknemer de knooppuntconfiguratie in Azure portal:
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
    
* U kunt ook het aantal worker-knooppunten om uit te schalen meer een cluster zonder opnieuw te maken op een na het maken bewerken:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Zie voor meer informatie over het schalen van HDInsight [schaal HDInsight-clusters](hdinsight-scaling-best-practices.md)

## <a name="use-tez-instead-of-map-reduce"></a>Tez gebruiken in plaats van Mapreduce

[Apache Tez](http://hortonworks.com/hadoop/tez/) is een engine voor het alternatieve uitvoeren met de MapReduce-engine. HDInsight-clusters op basis van Linux hebben Tez standaard ingeschakeld.

![tez_1][image-hdi-optimize-hive-tez_1]

Tez is sneller, omdat:

* **Omgeleid Acyclic Graph (DAG) worden uitgevoerd als één taak in de MapReduce-engine**. De DAG is vereist voor elke set mappers moet worden gevolgd door een reeks reducers tegelijkertijd. Dit zorgt ervoor dat meerdere MapReduce-taken worden voor elke Hive-query uit hebben ingeschakeld. Tez geen dergelijke beperking en complexe DAG kan worden verwerkt als een taak, dus de taak starten overhead worden geminimaliseerd.
* **Vermijdt u onnodige schrijfbewerkingen**. Meerdere taken worden gebruikt voor het verwerken van de dezelfde Hive-query in de MapReduce-engine. De uitvoer van elke MapReduce-taak is geschreven naar HDFS voor de tussentijdse gegevens. Aangezien Tez het aantal taken voor elke Hive-query minimaliseert, is het vermijden van onnodige schrijfbewerkingen.
* **Startfase vertragingen minimaliseert**. Tez is beter kunnen opstarten vertraging minimaliseren door het aantal mappers nodig is om te beginnen te verminderen en het verbeteren van ook optimalisatie in het hele.
* **Containers hergebruikt**. Wanneer mogelijk Tez is niet opnieuw gebruiken van containers om ervoor te zorgen dat de latentie vanwege het opstarten van containers wordt verminderd.
* **Continue optimalisatietechnieken**. Optimalisatie is gewoonlijk gedaan tijdens de compilatie-fase. Maar meer informatie over de invoer beschikbaar is waarmee voor betere optimalisatie tijdens runtime. Continue optimalisatietechnieken die toe te staan om het optimaliseren van het plan verder in de fase van de runtime maakt gebruik van Tez.

Zie voor meer informatie over deze concepten [Apache TEZ](http://hortonworks.com/hadoop/tez/).

U kunt elke query Hive Tez ingeschakeld door het voorvoegsel van de query met de volgende set-opdracht:

   ```hive
   set hive.execution.engine=tez;
   ```

## <a name="hive-partitioning"></a>Hive partitioneren

I/o-bewerkingen zijn de belangrijkste prestatieknelpunt voor het uitvoeren van Hive-query's. De prestaties kan worden verbeterd als de hoeveelheid gegevens die moet worden gelezen kan worden teruggebracht. Hive-query's scannen standaard volledige Hive-tabellen. Maar voor query's die alleen moeten worden gescand van een kleine hoeveelheid gegevens (die bijvoorbeeld query's met filteren), dit gedrag maakt onnodige overhead. Hive partitioneren zorgt ervoor dat Hive-query's voor toegang tot de benodigde hoeveelheid gegevens in Hive-tabellen.

Hive partitioneren wordt geïmplementeerd door het opnieuw indelen van de onbewerkte gegevens in de nieuwe directory's. Elke partitie heeft een eigen map. De partitionering wordt gedefinieerd door de gebruiker. Het volgende diagram illustreert de partitionering van een Hive-tabel met de kolom *jaar*. Een nieuwe map wordt gemaakt voor elk jaar.

![Hive partitioneren][image-hdi-optimize-hive-partitioning_1]

Sommige partitioneren overwegingen:

* **Kan niet onder de partitie** -partitioneren op kolommen met slechts enkele waarden kan leiden tot enkele partities. Partitioneren op geslacht alleen maakt u bijvoorbeeld twee partities worden gemaakt (mannelijke en vrouwelijke), dus alleen Verminder de latentie door een maximum van de helft.
* **Kan niet via partitie** - op andere theorie, het maken van een partitie in een kolom met een unieke waarde (bijvoorbeeld gebruikers-id) zorgt ervoor dat meerdere partities. Via partitie zorgt ervoor dat veel belasting op het cluster namenode omdat er voor het afhandelen van het grote aantal mappen.
* **Vermijd gegevensverschil** -kiest u de te nemen partitionerende sleutel goed te overwegen zodat alle partities zelfs grootte zijn. Bijvoorbeeld, partitioneren op *status* kolom mogelijk scheeftrekken de verdeling van gegevens. Aangezien de staat Californië een populatie bijna heeft kunnen 30 x dat van Vermont, de grootte van de partitie is mogelijk vervormd en de prestaties variëren kostte.

Gebruik voor het maken van een partitietabel de *gepartitioneerd door* component:

   ```hive
   CREATE TABLE lineitem_part
       (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
        L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
        L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
        L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING, 
        L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT STRING)
   PARTITIONED BY(L_SHIPDATE STRING)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
   STORED AS TEXTFILE;
   ```
   
Zodra de gepartitioneerde tabel is gemaakt, kunt u het partitioneren van statische of dynamische partitioneren maken.

* **Statische partitioneren** betekent dat u al shard-gegevens in de juiste mappen hebt. Met statische partities, moet u handmatig op de locatie op basis van Hive-partities toevoegen. Het volgende codefragment wordt een voorbeeld.
  
   ```hive
   INSERT OVERWRITE TABLE lineitem_part
   PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
   SELECT * FROM lineitem 
   WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
   
   ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
   LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
   ```
   
* **Dynamische partitioneren** betekent dat u wilt dat Hive partities automatisch voor u te maken. Nadat u de tabel partitioneren van de faseringstabel al hebt gemaakt, is hoeft u alleen gegevens voor de gepartitioneerde tabel invoegen:
  
   ```hive
   SET hive.exec.dynamic.partition = true;
   SET hive.exec.dynamic.partition.mode = nonstrict;
   INSERT INTO TABLE lineitem_part
   PARTITION (L_SHIPDATE)
   SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
       L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
       L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
       L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as L_RETURNFLAG,
       L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as L_SHIPDATE_PS,
       L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as L_RECEIPTDATE,
       L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as L_SHIPMODE, 
       L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;
   ```
   
Zie voor meer informatie, [gepartitioneerde tabellen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

## <a name="use-the-orcfile-format"></a>Gebruik de ORCFile-indeling
Hive ondersteunt verschillende bestandsindelingen. Bijvoorbeeld:

* **Tekst**: de standaard-bestandsindeling en werkt met de meeste scenario's
* **Avro**: geschikt is voor scenario's voor interoperabiliteit
* **ORC/Parquet**: bij uitstek geschikt voor prestaties

(Geoptimaliseerd rij kolomvorm) ORC-indeling is een zeer efficiënte manier voor het opslaan van gegevens met Hive. Vergeleken met andere indelingen, heeft ORC de volgende voordelen:

* ondersteuning voor complexe typen, inclusief datum/tijd- en complexe en semi-gestructureerde typen
* tot 70% compressie
* elke 10.000 rijen, waarmee de rijen overgeslagen indexen
* een aanzienlijke afname in runtime-uitvoering

Om in te schakelen ORC-indeling, u eerst een tabel maken met de component *opgeslagen als ORC*:

   ```hive
   CREATE TABLE lineitem_orc_part
       (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
        L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
        L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
        L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
        L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
   PARTITIONED BY(L_SHIPDATE STRING)
   STORED AS ORC;
   ```
   
Vervolgens plaatst u gegevens naar het ORC-tabel uit de tijdelijke tabel. Bijvoorbeeld:

   ```hive
   INSERT INTO TABLE lineitem_orc
   SELECT L_ORDERKEY as L_ORDERKEY, 
          L_PARTKEY as L_PARTKEY , 
          L_SUPPKEY as L_SUPPKEY,
          L_LINENUMBER as L_LINENUMBER,
          L_QUANTITY as L_QUANTITY, 
          L_EXTENDEDPRICE as L_EXTENDEDPRICE,
          L_DISCOUNT as L_DISCOUNT,
          L_TAX as L_TAX,
          L_RETURNFLAG as L_RETURNFLAG,
          L_LINESTATUS as L_LINESTATUS,
          L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;
   ```
   
U kunt meer lezen op de ORC-indeling in de [Hive taal handmatig](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

## <a name="vectorization"></a>Vectorisatie in

Vectorisatie in kunt Hive voor het verwerken van een batch van 1024 rijen samen in plaats van één rij tegelijk verwerken. Dit betekent dat eenvoudige bewerkingen sneller worden uitgevoerd omdat er minder interne code moet worden uitgevoerd.

Om in te schakelen voorvoegsel vectorisatie in uw Hive-query met de volgende instellingen:

   ```hive
    set hive.vectorized.execution.enabled = true;
   ```

Zie voor meer informatie, [Vectorized queryuitvoering](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).

## <a name="other-optimization-methods"></a>Andere methoden voor optimalisatie
Er zijn meer optimalisatie-methoden die u, bijvoorbeeld overwegen kunt:

* **Hive bucketing:** een techniek die kunt cluster of segmenteren grote sets met gegevens om queryprestaties te optimaliseren.
* **Deelnemen aan optimalisatie:** optimalisatie van het uitvoeren van Hive van query's te verbeteren van de efficiëntie van samenvoegingen verminderen de noodzaak voor gebruiker hints plannen. Zie voor meer informatie, [Join optimalisatie](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
* **Verhogen reducers tegelijkertijd**.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u verschillende algemene Hive query optimalisatie methoden geleerd. Zie de volgende artikelen voor meer informatie:

* [Apache Hive in HDInsight gebruiken](hadoop/hdinsight-use-hive.md)
* [Gegevens over vertraagde vluchten analyseren met behulp van Hive in HDInsight](hdinsight-analyze-flight-delay-data.md)
* [Twitter-gegevens met behulp van Hive in HDInsight analyseren](hdinsight-analyze-twitter-data.md)
* [Analyseren van sensorgegevens met behulp van de Hive-Query-Console van Hadoop in HDInsight](hadoop/apache-hive-analyze-sensor-data.md)
* [Hive gebruiken met HDInsight voor het analyseren van Logboeken van websites](hadoop/apache-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
