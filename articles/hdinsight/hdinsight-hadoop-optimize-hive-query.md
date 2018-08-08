---
title: In Azure HDInsight Hive-query's optimaliseren
description: Informatie over het optimaliseren van uw Hive-query's voor Hadoop in HDInsight.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: 5142f2d2c3828a5311a67ac4a7e5abd3cc434801
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39594672"
---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a>In Azure HDInsight Hive-query's optimaliseren

Hadoop-clusters zijn standaard niet geoptimaliseerd voor prestaties. Dit artikel vindt u enkele veelgebruikte Hive prestaties optimalisatie methoden die u op uw query's toepassen kunt.

## <a name="scale-out-worker-nodes"></a>De schaal vergroten worker-knooppunten

Het aantal worker-knooppunten in een cluster verhoogt kan gebruikmaken van meer mappers en reducers tegelijkertijd kunnen parallel worden uitgevoerd. Er zijn twee manieren waarop u scale-out in HDInsight kunt verhogen:

* Bij het inrichten, kunt u het aantal worker-knooppunten met behulp van de Azure portal, Azure PowerShell of platformonafhankelijke opdrachtregelinterface.  Zie [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie. De volgende schermafbeelding ziet de werknemer de knooppuntconfiguratie in Azure portal:
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* Tijdens de runtime, kunt u ook uitbreiden een cluster zonder opnieuw te maken op een:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Zie voor meer informatie over de verschillende virtuele machines die door HDInsight worden ondersteund, [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="enable-tez"></a>Schakel Tez in

[Apache Tez](http://hortonworks.com/hadoop/tez/) is een engine voor het alternatieve uitvoeren met de MapReduce-engine:

![tez_1][image-hdi-optimize-hive-tez_1]

Tez is sneller, omdat:

* **Omgeleid Acyclic Graph (DAG) worden uitgevoerd als één taak in de MapReduce-engine**. De DAG is vereist voor elke set mappers moet worden gevolgd door een reeks reducers tegelijkertijd. Dit zorgt ervoor dat meerdere MapReduce-taken worden voor elke Hive-query uit hebben ingeschakeld. Tez geen dergelijke beperking en complexe DAG kan worden verwerkt als een taak, dus de taak starten overhead worden geminimaliseerd.
* **Vermijdt u onnodige schrijfbewerkingen**. Vanwege meerdere taken hebben voor de dezelfde Hive-query in de MapReduce-engine wordt ingeschakeld, wordt de uitvoer van elke taak geschreven naar HDFS voor de tussentijdse gegevens. Aangezien Tez het aantal taken voor elke Hive-query wordt uitgevoerd minimaliseert om te voorkomen dat onnodige schrijven.
* **Startfase vertragingen minimaliseert**. Tez is beter kunnen opstarten vertraging minimaliseren door het aantal mappers nodig is om te beginnen te verminderen en het verbeteren van ook optimalisatie in het hele.
* **Containers hergebruikt**. Wanneer mogelijk Tez is niet opnieuw gebruiken van containers om ervoor te zorgen dat de latentie vanwege het opstarten van containers wordt verminderd.
* **Continue optimalisatietechnieken**. Optimalisatie is gewoonlijk gedaan tijdens de compilatie-fase. Maar meer informatie over de invoer beschikbaar is waarmee voor betere optimalisatie tijdens runtime. Continue optimalisatietechnieken die toe te staan om het optimaliseren van het plan verder in de fase van de runtime maakt gebruik van Tez.

Zie voor meer informatie over deze concepten [Apache TEZ](http://hortonworks.com/hadoop/tez/).

U kunt elke query Hive Tez ingeschakeld door het voorvoegsel van de query met de instelling hieronder:

    set hive.execution.engine=tez;

HDInsight-clusters op basis van Linux hebben Tez standaard ingeschakeld.


## <a name="hive-partitioning"></a>Hive partitioneren

I/o-bewerking is de belangrijkste prestatieknelpunt voor het uitvoeren van Hive-query's. De prestaties kan worden verbeterd als de hoeveelheid gegevens die moet worden gelezen kan worden teruggebracht. Hive-query's scannen standaard volledige Hive-tabellen. Dit is ideaal voor query's zoals tabelscans. Maar voor query's die alleen moeten worden gescand van een kleine hoeveelheid gegevens (die bijvoorbeeld query's met filteren), dit gedrag maakt onnodige overhead. Hive partitioneren zorgt ervoor dat Hive-query's voor toegang tot de benodigde hoeveelheid gegevens in Hive-tabellen.

Hive partitioneren wordt geïmplementeerd door het opnieuw indelen van de onbewerkte gegevens in de nieuwe mappen met elke partitie met een eigen directory - waarbij de partitie wordt gedefinieerd door de gebruiker. Het volgende diagram illustreert de partitionering van een Hive-tabel met de kolom *jaar*. Een nieuwe map wordt gemaakt voor elk jaar.

![Partitioneren][image-hdi-optimize-hive-partitioning_1]

Sommige partitioneren overwegingen:

* **Kan niet onder de partitie** -partitioneren op kolommen met slechts enkele waarden kan leiden tot enkele partities. Partitioneren op geslacht alleen maakt u bijvoorbeeld twee partities worden gemaakt (mannelijke en vrouwelijke), dus alleen Verminder de latentie door een maximum van de helft.
* **Kan niet via partitie** - op andere theorie, het maken van een partitie in een kolom met een unieke waarde (bijvoorbeeld gebruikers-id) zorgt ervoor dat meerdere partities. Via partitie zorgt ervoor dat veel belasting op het cluster namenode omdat er voor het afhandelen van het grote aantal mappen.
* **Vermijd gegevensverschil** -kiest u de te nemen partitionerende sleutel goed te overwegen zodat alle partities zelfs grootte zijn. Een voorbeeld is een partitionerende op *status* kan ertoe leiden dat het aantal records onder Californië bijna 30 x van Vermont vanwege het verschil in populatie.

Gebruik voor het maken van een partitietabel de *gepartitioneerd door* component:

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE            STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Zodra de gepartitioneerde tabel is gemaakt, kunt u het partitioneren van statische of dynamische partitioneren maken.

* **Statische partitioneren** betekent dat u al shard-gegevens in de juiste mappen hebt en kunt u handmatig op de locatie op basis van Hive-partities vragen. Het volgende codefragment wordt een voorbeeld.
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* **Dynamische partitioneren** betekent dat u wilt dat Hive partities automatisch voor u te maken. Omdat we de partitionering tabel al hebt gemaakt in de faseringstabel, is er hoeft alleen gegevens aan de gepartitioneerde tabel invoegen:
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Zie voor meer informatie, [gepartitioneerde tabellen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

## <a name="use-the-orcfile-format"></a>Gebruik de ORCFile-indeling
Hive ondersteunt verschillende bestandsindelingen. Bijvoorbeeld:

* **Tekst**: dit is de standaardbestandsindeling en werkt met de meeste scenario's
* **Avro**: geschikt is voor scenario's voor interoperabiliteit
* **ORC/Parquet**: bij uitstek geschikt voor prestaties

(Geoptimaliseerd rij kolomvorm) ORC-indeling is een zeer efficiënte manier voor het opslaan van gegevens met Hive. Vergeleken met andere indelingen, heeft ORC de volgende voordelen:

* ondersteuning voor complexe typen, inclusief datum/tijd- en complexe en semi-gestructureerde typen
* tot 70% compressie
* elke 10.000 rijen, waarmee de rijen overgeslagen indexen
* een aanzienlijke afname in runtime-uitvoering

Om in te schakelen ORC-indeling, u eerst een tabel maken met de component *opgeslagen als ORC*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Vervolgens plaatst u gegevens naar het ORC-tabel uit de tijdelijke tabel. Bijvoorbeeld:

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

U kunt meer lezen over het ORC-indeling [hier](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

## <a name="vectorization"></a>Vectorisatie in

Vectorisatie in kunt Hive voor het verwerken van een batch van 1024 rijen samen in plaats van één rij tegelijk verwerken. Dit betekent dat eenvoudige bewerkingen sneller worden uitgevoerd omdat er minder interne code moet worden uitgevoerd.

Om in te schakelen voorvoegsel vectorisatie in uw Hive-query met de volgende instellingen:

    set hive.vectorized.execution.enabled = true;

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
