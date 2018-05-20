---
title: Maken van maximaal beschikbare Spark-Streaming-taken in YARN - Azure HDInsight | Microsoft Docs
description: Het instellen van het Spark-Streaming voor een scenario met hoge beschikbaarheid.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ramoha
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: ramoha
ms.openlocfilehash: bbb4da02cbe4b0685c715c4cd6bd7b15c6b5cce0
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="create-high-availability-spark-streaming-jobs-with-yarn"></a>Hoge beschikbaarheid Spark-Streaming taken met YARN maken

Spark-Streaming, kunt u voor het implementeren van schaalbare hoge gegevensdoorvoer en fouttolerantie toepassingen voor het verwerken van gegevensstromen. U kunt Spark-Streaming-toepassingen op een HDInsight Spark-cluster verbinden met een waaier aan gegevensbronnen, zoals Azure Event Hubs, Azure IoT Hub, Kafka, Flume, Twitter, ZeroMQ, onbewerkte TCP-sockets of door de bewaking van het bestandssysteem HDFS op wijzigingen. Spark-Streaming ondersteunt fouttolerantie met de garantie dat een bepaalde gebeurtenis slechts één keer zelfs met een knooppuntfout verwerkt.

Spark-Streaming maakt langlopende taken waarover u zich kunt transformaties toepassen op de gegevens en vervolgens de resultaten uit push bestandssystemen, databases, dashboards en de console. Spark-Streaming verwerkt micro-batches van gegevens, door het eerste verzamelen van een reeks gebeurtenissen gedurende een opgegeven tijdsinterval. Vervolgens wordt op die batch verzonden voor verwerking en uitvoer. Batch tijdsintervallen worden gewoonlijk gedefinieerd in delen van een seconde.

![Spark-Streaming](./media/apache-spark-streaming-high-availability/spark-streaming.png)

## <a name="dstreams"></a>DStreams

Een continue stroom van gegevens met Spark-Streaming vertegenwoordigt een *onderscheiden stroom* (DStream). Deze DStream kan worden gemaakt vanuit invoerbronnen zoals Event Hubs of Kafka of door het toepassen van transformaties op een andere DStream. Wanneer een gebeurtenis op uw toepassing Spark-Streaming arriveert, wordt de gebeurtenis wordt opgeslagen in een betrouwbare manier. Gegevens van de gebeurtenis is dat wil zeggen, zodat meerdere knooppunten een kopie van het hebben gerepliceerd. Dit zorgt ervoor dat het uitvallen van een enkel knooppunt niet tot het verlies van uw gebeurtenis leiden.

Maakt gebruik van de Spark core *flexibele gegevenssets gedistribueerd* (RDDs). Gegevens verdelen RDDs over meerdere knooppunten in het cluster, waarbij elk knooppunt in het algemeen de gegevens volledig in het geheugen voor optimale prestaties houdt. Elke RDD geeft gebeurtenissen verzameld tijdens een interval van de batch. Wanneer het batch-interval is verstreken, produceert Spark-Streaming een nieuwe RDD met alle gegevens in dat interval. Deze reeks aaneengesloten RDDs wordt verzameld in een DStream. Een Spark-Streaming-toepassing verwerkt de gegevens in elke batch RDD.

![Spark DStream](./media/apache-spark-streaming-high-availability/DStream.png)

## <a name="spark-structured-streaming-jobs"></a>Taken gestructureerde Spark-Streaming

Gestructureerde Spark-Streaming werd geïntroduceerd in Spark 2.0 als een analyse-engine voor gebruik op het streaming-gestructureerde gegevens. De engine-API's batchverwerking SparkSQL gestructureerde Spark-Streaming worden gebruikt. Met een Spark-Streaming gestructureerde Spark-Streaming de berekeningen uitgevoerd via micro continu binnenkomen batches van gegevens. Gestructureerde Spark-Streaming Hiermee geeft u een stream met gegevens als een invoertabel met onbeperkte rijen. Dat wil zeggen, blijft de invoertabel groeien zolang er nieuwe gegevens aankomen. Deze invoertabel continu wordt verwerkt door een langdurige query en de resultaten uit naar een uitvoertabel worden geschreven.

![Spark gestructureerd Streaming](./media/apache-spark-streaming-high-availability/structured-streaming.png)

In de gestructureerde Streaming gegevens aankomt bij het systeem en onmiddellijk wordt ingenomen in de invoer-tabel. Schrijven van query's die bewerkingen op basis van deze tabel invoer uitvoeren. De query-uitvoer resulteert in een andere tabel, met de naam van de tabel met resultaten. De tabel met resultaten bevat de resultaten van de query, van waaruit u gegevens worden verzonden naar een externe gegevensarchief tekent een relationele database. De *trigger interval* stelt de timing voor wanneer de gegevens uit de tabel invoer worden verwerkt. Standaard verwerkt Streaming-gestructureerde gegevens zodra deze binnenkomen. U kunt echter ook de trigger wordt uitgevoerd op een langer interval, dus de streaminggegevens in batches op basis van tijd is verwerkt configureren. De gegevens in de tabel met resultaten kan worden volledig vernieuwd telkens wanneer er nieuwe gegevens is zodat alle uitvoergegevens sinds het begin van de streaming-query (*volledige modus*), of alleen de gegevens die nieuw zijn sinds de laatste kan alleen bevatten Wanneer de query is verwerkt (*modus append*).

## <a name="create-fault-tolerant-spark-streaming-jobs"></a>Fouttolerantie Spark-Streaming-taken maken

Start uw afzonderlijke taken voor herstel in geval van storing programmeren voor het maken van een omgeving met hoge beschikbaarheid voor uw Spark-Streaming-taken. Dergelijke zelf herstellende taken zijn fouttolerantie.

RDDs hebben verschillende eigenschappen die maximaal beschikbaar en fouttolerante Spark-Streaming taken helpen:

* Batches van invoergegevens in RDDs opgeslagen als een DStream worden automatisch gerepliceerd in het geheugen voor fouttolerantie.
* Gegevens verloren gegaan vanwege fout worker kan worden herberekend gerepliceerde invoergegevens van verschillende werknemers, zolang deze worker-knooppunten beschikbaar zijn.
* Snel herstel van de fout kan optreden binnen één seconde, zoals het herstellen van fouten/stragglers gebeurt via berekening in het geheugen.

### <a name="exactly-once-semantics-with-spark-streaming"></a>Exact-eenmaal semantiek met Spark-Streaming

Voor het maken van een toepassing die elke gebeurtenis wordt verwerkt eenmaal (en slechts één keer) Overweeg hoe alle system foutpunten opnieuw opstarten nadat ik een probleem en hoe kunt u voorkomen dat gegevens verloren gaan. Exact-zodra semantiek vereisen dat gegevens niet verloren op elk gewenst moment en die berichtverwerking kan opnieuw worden gestart, ongeacht waar de fout zich voordoet. Zie [Spark-Streaming maken voor taken met exact-eenmaal gebeurtenis verwerken](apache-spark-streaming-exactly-once.md).

## <a name="spark-streaming-and-yarn"></a>Spark-Streaming en YARN

In HDInsight, cluster werk wordt gecoördineerd door *nog een andere Resource onderhandelaar* (YARN). Ontwerpen van hoge beschikbaarheid voor Spark-Streaming ook technieken voor Spark-Streaming en voor YARN-onderdelen.  Een voorbeeldconfiguratie met behulp van YARN worden hieronder weergegeven. 

![YARN-architectuur](./media/apache-spark-streaming-high-availability/yarn-arch.png)

De volgende secties beschrijven ontwerpoverwegingen voor deze configuratie.

### <a name="plan-for-failures"></a>Plan voor fouten

Voor het maken van een YARN-configuratie voor hoge beschikbaarheid, moet u plannen voor een mogelijke executor of het stuurprogramma-fout. Sommige taken Spark-Streaming ook gegevens garantie vereisten waaraan moet aanvullende configuratie en setup opnieuw uit. Een streaming-toepassing kan bijvoorbeeld een zakelijke vereiste om een nul--verlies van gegevens gegarandeerd ondanks een fout die in de streaming hostsysteem of de HDInsight-cluster optreedt.

Als een **executor** mislukt, de taken en de ontvangers worden automatisch opnieuw opgestart door Spark, zodat er geen wijziging in de configuratie nodig.

Echter, als een **stuurprogramma** mislukt, en vervolgens alle van de bijbehorende Executor, mislukt dit en alle ontvangen blokken en berekeningen resultaten gaan verloren. Gebruik een stuurprogramma om fout te herstellen, *DStream plaatsen van controlepunten* zoals beschreven in [Spark-Streaming maken voor taken met exact-eenmaal gebeurtenis verwerken](apache-spark-streaming-exactly-once.md#use-checkpoints-for-drivers). Het plaatsen van controlepunten DStream regelmatig opgeslagen de *omgeleid acyclische grafiek* (DAG) van DStreams voor fouttolerantie opslag zoals Azure Storage.  Het plaatsen van controlepunten kan gestructureerde Streaming van Spark opnieuw opstarten van het mislukte stuurprogramma van het controlepunt-informatie.  Deze stuurprogramma opnieuw start nieuwe Executor en ontvangers ook opnieuw is opgestart.

Stuurprogramma's voor het plaatsen van controlepunten DStream herstellen:

* Opnieuw opstarten automatisch stuurprogramma op YARN configureren met de configuratie-instelling `yarn.resourcemanager.am.max-attempts`.
* Een directory controlepunt instellen in het bestandssysteem van een HDFS-compatibele met `streamingContext.checkpoint(hdfsDirectory)`.
* Herstructureren broncode voor het gebruik van controlepunten voor herstel, bijvoorbeeld:

    ```scala
        def creatingFunc() : StreamingContext = {
            val context = new StreamingContext(...)
            val lines = KafkaUtils.createStream(...)
            val words = lines.flatMap(...)
            ...
            context.checkpoint(hdfsDir)
        }

        val context = StreamingContext.getOrCreate(hdfsDir, creatingFunc)
        context.start()
    ```

* Configureren van verloren gegevens te herstellen, doordat u de vooraf geschreven logboek (WAL) met `sparkConf.set("spark.streaming.receiver.writeAheadLog.enable","true")`, en uitschakelen van de replicatie in het geheugen voor invoer DStreams met `StorageLevel.MEMORY_AND_DISK_SER`.

Samengevat: plaatsen van controlepunten, WAL + betrouwbare ontvangers gebruikt, kunt u zich 'ten minste eenmaal' gegevensherstel leveren:

* Exact zodra, zolang de ontvangen gegevens niet verloren en wordt de uitvoer zijn beide idempotent of transactionele.
* Slechts één keer met de nieuwe Kafka Direct benaderen waarvoor Kafka worden gebruikt als een gerepliceerde logboek in plaats van met ontvangers of WALs.

### <a name="typical-concerns-for-high-availability"></a>Typische zorgen voor hoge beschikbaarheid

* Er is het moeilijker voor het bewaken van streaming taken dan batchtaken. Spark-Streaming-taken zijn doorgaans langlopende en YARN Logboeken niet samenvoegen tot een taak is voltooid.  Spark controlepunten gaan verloren tijdens de toepassing of upgrades voor Spark en moet u de map controlepunt wissen tijdens een upgrade.

* De modus van de cluster YARN om uit te voeren van stuurprogramma's, zelfs als een client niet configureren. Automatisch opnieuw opstarten voor stuurprogramma's instellen:

    ```
    spark.yarn.maxAppAttempts = 2
    spark.yarn.am.attemptFailuresValidityInterval=1h
    ```

* Spark en de gebruikersinterface voor het streamen van Spark beschikken over een configureerbare metrische gegevens. U kunt ook aanvullende bibliotheken, zoals grafiek/Grafana dashboard metrische gegevens te downloaden, zoals 'num records verwerkt', ' Geheugen/GC gebruik op stuurprogramma & Executor', 'Totaal aantal vertraging', '-gebruik van het cluster, enzovoort. In het gestructureerde Streaming versie 2.1 of hoger, kunt u `StreamingQueryListener` voor het verzamelen van aanvullende gegevens.

* U moet langlopende taken segmenteren.  Als een toepassing Spark-Streaming wordt verzonden naar het cluster, moet de YARN-wachtrij waarin de taak wordt uitgevoerd worden gedefinieerd. U kunt een [YARN capaciteit Scheduler](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html) om langlopende taken naar een afzonderlijke wachtrijen te verzenden.

* Afsluiten uw streaming-toepassing. Als uw offsets worden aangeduid, en alle statussen van de toepassing is die extern zijn opgeslagen, kunt u uw streaming toepassing op de juiste plaats programmatisch stoppen. Een techniek is het gebruik van 'thread hooks' in Spark, door te controleren voor een externe vlag elke *n* seconden. U kunt ook een *markeringsbestand dat* die op HDFS gemaakt wanneer de toepassing wordt gestart, wordt verwijderd wanneer u wilt stoppen. Gebruik voor een benadering markering-bestand een afzonderlijke thread in uw Spark-toepassing die code ongeveer als volgt aanroept:

    ```scala
    streamingContext.stop(stopSparkContext = true, stopGracefully = true)
    // to be able to recover on restart, store all offsets in an external database
    ```

## <a name="next-steps"></a>Volgende stappen

* [Spark-Streaming-overzicht](apache-spark-streaming-overview.md)
* [Taken met Spark-Streaming exact maken-eenmaal gebeurtenis verwerken](apache-spark-streaming-exactly-once.md)
* [Langlopende Spark-Streaming-taken op YARN](http://mkuthan.github.io/blog/2016/09/30/spark-streaming-on-yarn/) 
* [Gestructureerde Streaming: Fout met betrekking tot fouttolerante semantiek](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html#fault-tolerance-semantics)
* [Onderscheiden Streams: Een fouttolerantie Model voor de verwerking van schaalbare Stream](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2012/EECS-2012-259.pdf)
