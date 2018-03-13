---
title: Spark-Streaming in Azure HDInsight | Microsoft Docs
description: Het gebruik van Spark-Streaming-toepassingen op HDInsight Spark-clusters.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: maxluk
ms.openlocfilehash: a4cc2768f0d4217b2bd14938889e9b71c26009c9
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="overview-of-spark-streaming"></a>Overzicht van Spark-Streaming

Spark-Streaming biedt gegevensverwerking stream op HDInsight Spark-clusters, met een garantie dat een gebeurtenis invoer van slechts één keer wordt verwerkt, zelfs als een knooppuntfout optreedt. Een Spark-stroom is een langlopende taak die invoergegevens ontvangt van een groot aantal bronnen, met inbegrip van Azure Event Hubs, een Azure IoT Hub, Kafka, Flume, Twitter, ZeroMQ, onbewerkte TCP-sockets, of van HDFS-bestandssystemen bewaken. In tegenstelling tot een proces uitsluitend gebeurtenisafhankelijke, een Spark-stroom batches invoergegevens in tijdvensters, zoals een segment 2 seconden en klik vervolgens transformeert elke batch van gegevens met behulp van de kaart, verminderen, deelnemen aan en uitpakken van bewerkingen. De stroom Spark schrijft vervolgens de getransformeerde gegevens uit naar bestandssystemen, databases, dashboards en de console.

![De stroom verwerken met HDInsight en Spark-Streaming](./media/apache-spark-streaming-overview/hdinsight-spark-streaming.png)

Spark-Streaming-toepassingen moeten wachten totdat een fractie van seconden voor het verzamelen van elk *micro batch* van gebeurtenissen voor het verzenden van deze batch op voor de verwerking. Daarentegen is verwerkt een gebeurtenisafhankelijke toepassing elke gebeurtenis onmiddellijk. Spark-Streaming latentie is doorgaans in enkele seconden. De voordelen van de aanpak micro batch zijn efficiënter gegevensverwerking en eenvoudigere cumulatieve berekeningen.

## <a name="introducing-the-dstream"></a>Inleiding tot de DStream

Een continue stroom van binnenkomende gegevens met Spark-Streaming vertegenwoordigt een *onderscheiden stroom* een DStream aangeroepen. Een DStream kan worden gemaakt vanuit invoerbronnen zoals Event Hubs of Kafka of door het toepassen van transformaties op een andere DStream.

Een DStream biedt een laag van abstractie boven op de onbewerkte gebeurtenisgegevens worden opgeslagen. 

Beginnen met een enkelvoudige gebeurtenis, een temperatuur lezen van een verbonden thermostaat spreken. Wanneer deze gebeurtenis bij uw toepassing Spark-Streaming aankomen, wordt de gebeurtenis wordt opgeslagen in een betrouwbare manier, waar deze op meerdere knooppunten worden gerepliceerd. Deze fouttolerantie zorgt ervoor dat het uitvallen van een enkel knooppunt niet tot het verlies van uw gebeurtenis leiden. De Spark core maakt gebruik van een gegevensstructuur die verdeelt gegevens over meerdere knooppunten in het cluster waar elk knooppunt in het algemeen onderhoudt een eigen gegevens in het geheugen voor optimale prestaties. Deze gegevensstructuur heet een *robuuste gedistribueerde gegevensset* (RDD).

Elke RDD gebeurtenissen verzameld via een gebruiker gedefinieerde periode aangeroepen vertegenwoordigt de *batch interval*. Omdat elk batch-interval is verstreken, wordt een nieuwe RDD geproduceerd met de gegevens van dat interval. De continue reeks RDDs worden verzameld in een DStream. Bijvoorbeeld, als het batch-interval één seconde lang is, verzendt uw DStream een batch elke tweede met één RDD die de gegevens die zijn geconsumeerd tijdens deze tweede bevat. Bij het verwerken van de DStream, wordt de gebeurtenis temperatuur weergegeven in een van deze batches. Een Spark-Streaming-toepassing verwerkt de batches met de gebeurtenissen en uiteindelijk omgaat met de gegevens in elke RDD.

![Voorbeeld DStream met temperatuur-gebeurtenissen ](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-example.png)

## <a name="structure-of-a-spark-streaming-application"></a>Structuur van een toepassing Spark-Streaming

Een Spark-Streaming-toepassing is een langdurige-toepassing die gegevens ontvangt uit bronnen opnemen en vervolgens implementeert de gegevens naar een of meer doelen transformaties voor het verwerken van de gegevens van toepassing is. De structuur van een Spark-Streaming-toepassing heeft een statische gedeelte en een dynamische deel. Het statische deel definieert waar de gegevens afkomstig van welke verwerking op de gegevens, en waar de resultaten moeten gaan. Het dynamische deel, wordt de toepassing voor onbepaalde tijd wachten op een stopsignaal ontvangen uitgevoerd.

Bijvoorbeeld de volgende eenvoudige toepassing ontvangt van een tekstregel via een TCP-socket en telt het aantal keren dat elk woord wordt weergegeven.

### <a name="define-the-application"></a>De toepassing definiëren

De definitie van de toepassing logica bestaat uit vier stappen:

1. Maak een StreamingContext.
2. Maak een DStream van de StreamingContext.
3. Transformaties toepassen op de DStream.
4. De resultaten.

Deze definitie is statisch en er zijn geen gegevens worden verwerkt totdat u de toepassing uitvoert.

#### <a name="create-a-streamingcontext"></a>Een StreamingContext maken

Maak een StreamingContext van de SparkContext die naar het cluster verwijst. Geef bij het maken een StreamingContext u de grootte van de batch in seconden, bijvoorbeeld:

    val ssc = new StreamingContext(spark, Seconds(1))

#### <a name="create-a-dstream"></a>Een DStream maken

Met het exemplaar StreamingContext maken een invoer DStream voor uw invoerbron. In dit geval wordt de toepassing bekijken voor de weergave van nieuwe bestanden in de standaard-opslag gekoppeld aan het HDInsight-cluster.

    val lines = ssc.textFileStream("/uploads/2017/01/")

#### <a name="apply-transformations"></a>Transformaties toepassen

U implementeert dat de verwerking transformaties toepassen op de DStream. Deze toepassing ontvangt van een tekstregel tegelijk uit het bestand, elke regel splitst in woorden en vervolgens een map-reduce-patroon gebruikt voor het tellen van het aantal keren dat elk woord wordt weergegeven.

    val words = lines.flatMap(_.split(" "))
    val pairs = words.map(word => (word, 1))
    val wordCounts = pairs.reduceByKey(_ + _)

#### <a name="output-results"></a>Resultaten voor uitvoer

Resultaten van de transformatie uit naar de doelsystemen forceren door toe te passen o-bewerkingen. In dit geval wordt het resultaat van elke uitvoeren via de berekening afgedrukt in de console-uitvoer.

    wordCounts.print()

### <a name="run-the-application"></a>De toepassing uitvoeren

Start de streaming-toepassing en worden uitgevoerd totdat een beëindiging signaal ontvangen.

    ssc.start()            
    ssc.awaitTermination()

Zie voor meer informatie over de API van een Spark-stroom, samen met de bronnen van gebeurtenissen, transformaties en uitvoerbewerkingen die worden ondersteund, [Spark Streaming Programming Guide](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html).

Het volgende voorbeeld van een toepassing staat op zichzelf, zodat u kunt uitvoeren in een [Jupyter-Notebook](apache-spark-jupyter-notebook-kernels.md). Dit voorbeeld wordt een mock-gegevensbron in de klasse DummySource waarmee de waarde van een item en de huidige tijd in milliseconden elke vijf seconden. Een nieuwe StreamingContext-object heeft een batch-interval van dertig seconden. Telkens wanneer een batch is gemaakt, de streaming-toepassing de RDD geproduceerd onderzoekt de RDD converteert naar een Spark-DataFrame en een tijdelijke tabel wordt gemaakt via de DataFrame.

    class DummySource extends org.apache.spark.streaming.receiver.Receiver[(Int, Long)](org.apache.spark.storage.StorageLevel.MEMORY_AND_DISK_2) {

        /** Start the thread that simulates receiving data */
        def onStart() {
            new Thread("Dummy Source") { override def run() { receive() } }.start()
        }

        def onStop() {  }

        /** Periodically generate a random number from 0 to 9, and the timestamp */
        private def receive() {
            var counter = 0  
            while(!isStopped()) {
                store(Iterator((counter, System.currentTimeMillis)))
                counter += 1
                Thread.sleep(5000)
            }
        }
    }

    // A batch is created every 30 seconds
    val ssc = new org.apache.spark.streaming.StreamingContext(spark.sparkContext, org.apache.spark.streaming.Seconds(30))

    // Set the active SQLContext so that we can access it statically within the foreachRDD
    org.apache.spark.sql.SQLContext.setActive(spark.sqlContext)

    // Create the stream
    val stream = ssc.receiverStream(new DummySource())

    // Process RDDs in the batch
    stream.foreachRDD { rdd =>

        // Access the SQLContext and create a table called demo_numbers we can query
        val _sqlContext = org.apache.spark.sql.SQLContext.getOrCreate(rdd.sparkContext)
        _sqlContext.createDataFrame(rdd).toDF("value", "time")
            .registerTempTable("demo_numbers")
    } 

    // Start the stream processing
    ssc.start()

U kunt vervolgens een query de DataFrame regelmatig om te zien van de huidige set waarden aanwezig zijn in de batch, bijvoorbeeld met behulp van deze SQL-query:

    %%sql
    SELECT * FROM demo_numbers

De resulterende uitvoer ziet er als volgt:

| waarde | tijd |
| --- | --- |
|10 | 1497314465256 |
|11 | 1497314470272 |
|12 | 1497314475289 |
|13 | 1497314480310 |
|14 | 1497314485327 |
|15 | 1497314490346 |

Er zijn zes waarden, omdat de DummySource een waarde om de vijf seconden maakt en de toepassing een batch elke 30 seconden verzendt.

## <a name="sliding-windows"></a>Windows Verschuivend

Cumulatieve als berekeningen wilt uitvoeren op uw DStream gedurende enkele periode, bijvoorbeeld een gemiddelde temperatuur ophalen via de laatste twee seconden, kunt u de *venster Verschuivend* bewerkingen die zijn opgenomen met Spark-Streaming. Een sliding window heeft een duration (de lengte van het venster) en het interval gedurende welke de inhoud van het venster zijn geëvalueerd (de interval dia).

Windows Verschuivend kan elkaar overlappen, bijvoorbeeld, kunt u een venster met een lengte van twee seconden, die elk één seconde dia definiëren. Dit betekent dat elke keer dat u een statistische berekening uitvoeren, in het venster opgenomen gegevens van de laatste één seconde van het vorige venster evenals geen nieuwe gegevens in de volgende één seconde.

![Venster initiële voor voorbeeld met temperatuur-gebeurtenissen](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-01.png)

![Voorbeeld-venster met temperatuur gebeurtenissen na Verschuivend](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-02.png)

Het volgende voorbeeld de code die de DummySource gebruikt voor het verzamelen van de batches in een venster met een duur van één minuut en een dia één minuut bijgewerkt.

    // A batch is created every 30 seconds
    val ssc = new org.apache.spark.streaming.StreamingContext(spark.sparkContext, org.apache.spark.streaming.Seconds(30))

    // Set the active SQLContext so that we can access it statically within the foreachRDD
    org.apache.spark.sql.SQLContext.setActive(spark.sqlContext)

    // Create the stream
    val stream = ssc.receiverStream(new DummySource())

    // Process batches in 1 minute windows
    stream.window(org.apache.spark.streaming.Minutes(1)).foreachRDD { rdd =>

        // Access the SQLContext and create a table called demo_numbers we can query
        val _sqlContext = org.apache.spark.sql.SQLContext.getOrCreate(rdd.sparkContext)
        _sqlContext.createDataFrame(rdd).toDF("value", "time")
        .registerTempTable("demo_numbers")
    } 

    // Start the stream processing
    ssc.start()

Na de eerste minuut zijn er 12 vermeldingen - zes vermeldingen van elk van de twee batches verzameld in het venster.

| waarde | tijd |
| --- | --- |
| 1 | 1497316294139 |
| 2 | 1497316299158
| 3 | 1497316304178
| 4 | 1497316309204
| 5 | 1497316314224
| 6 | 1497316319243
| 7 | 1497316324260
| 8 | 1497316329278
| 9 | 1497316334293
| 10 | 1497316339314
| 11 | 1497316344339
| 12 | 1497316349361

Het sliding vensterfuncties die beschikbaar zijn in de Spark-Streaming-API bevatten venster, countByWindow reduceByWindow en countByValueAndWindow. Zie voor meer informatie over deze functies [transformaties voor DStreams](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html#transformations-on-dstreams).

## <a name="checkpointing"></a>Controlepunten plaatsen

Voor het leveren van tolerantie en fouttolerantie, Spark-Streaming is afhankelijk van het plaatsen van controlepunten om ervoor te zorgen dat de verwerking van de stroom onderbroken, zelfs met betrekking tot knooppuntfouten wordt. In HDInsight maakt Spark controlepunten voor duurzame opslag (Azure Storage of Data Lake Store). Deze controlepunten opslaan van de metagegevens over de streaming toepassing zoals de configuratie, de bewerkingen die zijn gedefinieerd door de toepassing en batches die in de wachtrij zijn, maar nog niet is verwerkt. In sommige gevallen worden de controlepunten ook bevatten de gegevens opslaan in de RDDs sneller opnieuw opbouwen van de status van de gegevens van wat aanwezig in de RDDs beheerd door Spark is.

## <a name="deploying-spark-streaming-applications"></a>Spark-Streaming-toepassingen implementeren

U doorgaans een Spark-Streaming-toepassing lokaal in een JAR-bestand maakt en vervolgens implementeren op Spark in HDInsight door het JAR-bestand kopiëren naar de standaard-opslag dat is gekoppeld aan uw HDInsight-cluster. U kunt uw toepassing met de LIVY REST-API's beschikbaar starten van het cluster met een POST-bewerking. De hoofdtekst van het bericht bevat een JSON-document dat het pad aan uw Directory met JAR, de naam van de klasse waarvan hoofdmethode worden gedefinieerd en wordt uitgevoerd de streaming-toepassing, en desgewenst de resourcevereisten van de taak (zoals het aantal Executor, geheugen en kernen) , en alle configuratie-instellingen van uw toepassingscode vereist.

![Een Spark-Streaming-toepassing implementeren](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

De status van alle toepassingen kan ook worden gecontroleerd met een GET-aanvraag voor een eindpunt LIVY. Ten slotte kunt u een actieve toepassing door uitgifte van een DELETE-aanvraag voor het eindpunt LIVY beëindigen. Zie voor meer informatie over de API LIVY [externe taken met LIVY](apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Volgende stappen

* [Maken van een Apache Spark-cluster in HDInsight](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Programmeerhandleiding voor Spark-Streaming](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html)
* [Spark-taken op afstand met LIVY starten](apache-spark-livy-rest-interface.md)