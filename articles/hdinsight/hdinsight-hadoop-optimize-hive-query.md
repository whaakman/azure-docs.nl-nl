---
title: In Azure HDInsight Hive-query's optimaliseren | Microsoft Docs
description: Informatie over het optimaliseren van uw Hive-query's voor Hadoop in HDInsight.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d6174c08-06aa-42ac-8e9b-8b8718d9978e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/03/2017
ms.author: jgao
ms.openlocfilehash: 4bd7f5b584030f9c1554b56895493837d2eac357
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a>In Azure HDInsight Hive-query's optimaliseren

Hadoop-clusters zijn standaard niet geoptimaliseerd voor prestaties. In dit artikel bevat informatie over een aantal veelgebruikte Hive prestaties optimalisatie methoden die u op uw query's toepassen kunt.

## <a name="scale-out-worker-nodes"></a>Worker-knooppunten uitbreiden

Het aantal worker-knooppunten in een cluster kunt gebruikmaken van meer mappers en verkleiningstoestellen parallel worden uitgevoerd. Er zijn twee manieren kunt u scale-out in HDInsight verhogen:

* Bij het inrichten, kunt u het aantal worker-knooppunten met behulp van de Azure-portal, Azure PowerShell of platformoverschrijdende opdrachtregelinterface.  Zie [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie. De volgende schermafbeelding ziet de werknemer knooppuntconfiguratie op de Azure-portal:
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* Tijdens de runtime, kunt u ook uitschalen een cluster zonder opnieuw te maken op een:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Zie voor meer informatie over de verschillende virtuele machines die door HDInsight worden ondersteund, [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="enable-tez"></a>Inschakelen van Tez

[Apache Tez](http://hortonworks.com/hadoop/tez/) is een engine voor het alternatieve uitvoeren met de MapReduce-engine:

![tez_1][image-hdi-optimize-hive-tez_1]

Tez is sneller omdat:

* **Omgeleid acyclische grafiek (DAG) worden uitgevoerd als een enkele taak in de MapReduce-engine**. De DAG is vereist voor elke set van mappers worden gevolgd door een reeks verkleiningstoestellen. Dit zorgt ervoor dat meerdere MapReduce-taken worden ingezet uit voor elke Hive-query. Tez heeft geen dergelijke beperking en complexe DAG kan worden verwerkt als een taak dus taak starten van de overhead te minimaliseren.
* **Onnodige schrijfbewerkingen voorkomt**. Vanwege meerdere taken wordt ingezet voor dezelfde Hive-query in de MapReduce-engine, wordt de uitvoer van elke taak naar HDFS geschreven voor tussenliggende gegevens. Aangezien Tez aantal taken voor elke Hive-query minimaliseert is het vermijden van onnodige schrijven.
* **Vertragingen bij het opstarten minimaliseert**. Tez is beter kunnen opstarten vertraging minimaliseren door minder mappers moet worden gestart en de optimalisatie in de gehele ook worden verbeterd.
* **Containers hergebruikt**. Telkens wanneer mogelijke Tez is aan te hergebruiken containers om ervoor te zorgen dat de latentie als gevolg van containers opstarten wordt geminimaliseerd.
* **Continue optimalisatietechnieken**. Optimalisatie is normaal uitgevoerd tijdens de compilatiefase. Maar vindt u meer informatie over de invoer waardoor voor betere optimalisatie tijdens runtime. Tez maakt gebruik van doorlopende optimalisatietechnieken waarmee het plan verder in de runtime-fase te optimaliseren.

Zie voor meer informatie over deze begrippen [Apache TEZ](http://hortonworks.com/hadoop/tez/).

Kunt u een Hive-query Tez ingeschakeld door het voorvoegsel van de query met de onderstaande instelling:

    set hive.execution.engine=tez;

HDInsight-clusters op basis van Linux hebben Tez is standaard ingeschakeld.


## <a name="hive-partitioning"></a>Hive partitioneren

I/o-bewerking is de belangrijkste prestatieknelpunt voor het uitvoeren van Hive-query's. De prestaties kan worden verbeterd als de hoeveelheid gegevens die moeten worden gelezen kan worden verkleind. Hive-query's scan standaard volledige Hive-tabellen. Dit is ideaal voor query's zoals tabelscans. Maar voor query's die alleen moeten worden gescand van een kleine hoeveelheid gegevens (bijvoorbeeld, query's filteren), dit gedrag maakt onnodige overhead. Hive partitionering kunt Hive-query's voor toegang tot de benodigde hoeveelheid gegevens in de Hive-tabellen.

Hive partitioneren wordt geïmplementeerd door het opnieuw indelen van de onbewerkte gegevens naar de nieuwe mappen met elke partitie hebben een eigen map - waarvoor de partitie is gedefinieerd door de gebruiker. Het volgende diagram illustreert het partitioneren van een Hive-tabel de kolom *jaar*. Een nieuwe map wordt gemaakt voor elk jaar.

![Partitioneren][image-hdi-optimize-hive-partitioning_1]

Enkele overwegingen partitionering:

* **Kan niet onder de partitie** -partitionering voor kolommen met slechts enkele waarden kan leiden tot enkele partities. Partitioneren op geslacht alleen maakt u bijvoorbeeld twee partities worden gemaakt (man en vrouwelijke), waardoor alleen de latentie te verminderen door een maximum van de helft.
* **Kan niet via partitie** - op het andere uiterste maken van een partitie in een kolom met een unieke waarde (bijvoorbeeld userid) zorgt ervoor dat meerdere partities. Via partitie zorgt ervoor dat veel belasting op het cluster namenode omdat deze voor het afhandelen van het grote aantal mappen.
* **Voorkomen dat gegevens scheeftrekken** -Kies uw te nemen partitionerende sleutel goed zodat alle partities zelfs grootte zijn. Een voorbeeld is een partitionerende op *status* kan ertoe leiden dat het aantal records onder Californië moet bijna 30 x die Vermont vanwege de verschillen in de populatie.

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

Zodra de gepartitioneerde tabel is gemaakt, kunt u het partitioneren van statische of dynamische partitionering maken.

* **Statische partitioneren** betekent dat u al gedeelde gegevens in de juiste mappen hebt en u handmatig op basis van de maplocatie Hive-partities vraagt. Het volgende codefragment is een voorbeeld.
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* **Dynamische partitionering** betekent dat u wilt dat Hive partities automatisch voor u te maken. Aangezien we de partitionering tabel van de faseringstabel al hebt gemaakt, is hoeft er gegevens naar de gepartitioneerde tabel invoegen:
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Zie voor meer informatie [gepartitioneerde tabellen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

## <a name="use-the-orcfile-format"></a>Gebruik de indeling ORCFile
Hive ondersteunt verschillende bestandsindelingen. Bijvoorbeeld:

* **Tekst**: dit is de standaardbestandsindeling en werkt samen met de meeste scenario's
* **Avro**: geschikt is voor interoperabiliteit scenario's
* **ORC/parketvloeren**: meest geschikt voor prestaties

Indeling van ORC (geoptimaliseerd rij kolommen) is een zeer efficiënte manier voor het opslaan van gegevens met Hive. Vergeleken met andere indelingen, heeft ORC de volgende voordelen:

* ondersteuning voor complexe gegevenstypen zoals DateTime en complexe en semi-gestructureerde typen
* maximaal 70% compressie
* elke 10.000 rijen, waardoor de rijen overgeslagen indexen
* een aanzienlijke daling van de runtime-uitvoering

Om in te schakelen ORC-indeling, eerst maakt u een tabel met de component *opgeslagen als ORC*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Vervolgens plaatst u gegevens naar de tabel ORC van de faseringstabel. Bijvoorbeeld:

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

U kunt meer lezen over de indeling ORC [hier](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

## <a name="vectorization"></a>Vectorization

Vectorization kunt component voor het verwerken van een batch rijen 1024 samen in plaats van één rij tegelijk verwerken. Dit betekent dat eenvoudige bewerkingen sneller worden uitgevoerd omdat minder interne code moet worden uitgevoerd.

Om in te schakelen voorvoegsel vectorization uw Hive-query met de volgende instellingen:

    set hive.vectorized.execution.enabled = true;

Zie voor meer informatie [Vectorized queryuitvoering](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).

## <a name="other-optimization-methods"></a>Andere methoden optimalisatie
Er zijn meer optimalisatie methoden die u, bijvoorbeeld overwegen kunt:

* **Hive bucketing:** een techniek waarmee cluster of segmenteren grote reeksen gegevens om queryprestaties te optimaliseren.
* **Deelnemen aan optimalisatie:** optimalisatie van de uitvoering van Hive-query plannen om de efficiëntie van joins verbeteren en de noodzaak van gebruiker hints te verminderen. Zie voor meer informatie [Join optimalisatie](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
* **Verhogen verkleiningstoestellen**.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u verschillende algemene Hive query optimalisatie methoden geleerd. Zie voor meer informatie de volgende artikelen:

* [Apache Hive in HDInsight gebruiken](hadoop/hdinsight-use-hive.md)
* [Vertraging vluchtgegevens analyseren met behulp van Hive in HDInsight](hdinsight-analyze-flight-delay-data.md)
* [Twitter-gegevens met Hive in HDInsight analyseren](hdinsight-analyze-twitter-data.md)
* [Met behulp van de Console Hive-Query met Hadoop in HDInsight-sensorgegevens analyseren](hadoop/apache-hive-analyze-sensor-data.md)
* [Hive gebruiken met HDInsight logboeken van websites te analyseren](hadoop/apache-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
