---
title: Spark-Streaming in Azure HDInsight
description: Het gebruik van Spark Streaming-toepassingen op HDInsight Spark-clusters.
services: hdinsight
ms.service: hdinsight
author: maxluk
ms.author: maxluk
editor: jasonwhowell
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/05/2018
ms.openlocfilehash: b3420737147f9ee67d5d2d021c28a98d34e209df
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39617612"
---
# <a name="overview-of-spark-streaming"></a>Overzicht van de Spark-Streaming

Spark Streaming zorgt voor verwerking van gegevensstromen in HDInsight Spark-clusters, met een garantie dat een gebeurtenis invoer exact één keer wordt verwerkt, zelfs als er een storing op een knooppunt optreedt. Een Spark-Stream is een langlopende taak die invoergegevens ontvangt van een groot aantal bronnen, met inbegrip van Azure Event Hubs, een Azure IoT Hub, Kafka, Flume, Twitter, ZeroMQ, onbewerkte TCP-sockets, of van het HDFS-bestandssystemen bewaken. In tegenstelling tot een proces uitsluitend op gebeurtenissen gebaseerde, een Spark-Stream invoergegevens batches in tijdvensters, zoals een segment 2 seconden en klikt u vervolgens elke batch met behulp van de kaart worden getransformeerd, verminderen, join en uitpakken van activiteiten. De getransformeerde gegevens schrijft uit de Spark-Stream vervolgens bestandssystemen, databases, dashboards en de-console.

![Stream verwerken met HDInsight en Spark-Streaming](./media/apache-spark-streaming-overview/hdinsight-spark-streaming.png)

Spark-Streaming-toepassingen moeten wachten tot een fractie van een seconde voor het verzamelen van elk *microbatches* van gebeurtenissen voor het verzenden van die partij op voor de verwerking van. Daarentegen verwerkt een gebeurtenisgestuurde toepassing elke gebeurtenis onmiddellijk. Spark Streaming latentie is normaal gesproken onder een paar seconden. De voordelen van de aanpak van microbatches zijn efficiënter gegevensverwerking en eenvoudigere statistische berekeningen.

## <a name="introducing-the-dstream"></a>Maak kennis met de DStream

Een continue stroom van binnenkomende gegevens met Spark Streaming vertegenwoordigt een *onderscheiden stream* een DStream genoemd. Een DStream kan worden gemaakt op basis van invoerbronnen, zoals Event Hubs of Kafka, of door het toepassen van transformaties op een andere DStream.

Een DStream biedt een laag van abstractie boven op de onbewerkte gebeurtenisgegevens. 

Beginnen met één gebeurtenis, een temperatuur lezen van een verbonden thermostaat zeggen. Als deze gebeurtenis wordt ontvangen op uw toepassing Spark Streaming, wordt de gebeurtenis opgeslagen in een betrouwbare manier, waar deze worden gerepliceerd op meerdere knooppunten. Deze fouttolerantie zorgt ervoor dat het uitvallen van een enkel knooppunt niet tot verlies van de gebeurtenis leiden wordt. De Spark core maakt gebruik van een gegevensstructuur die gegevens worden verdeeld over meerdere knooppunten in het cluster waar elk knooppunt in het algemeen onderhoudt een eigen gegevens in het geheugen voor de beste prestaties. Deze gegevensstructuur heet een *resilient distributed gegevensset* (RDD).

Elke RDD gebeurtenissen die worden verzameld via een gebruiker gedefinieerde periode met de naam vertegenwoordigt de *batch interval*. Omdat elk batch-interval is verstreken, wordt met alle gegevens uit dat het interval voor een nieuwe RDD geproduceerd. De continue reeks rdd's worden verzameld in een DStream. Bijvoorbeeld, als het batch-interval 1 seconde lang is, verzendt uw DStream een batch elke tweede met één RDD met alle gegevens die zijn opgenomen in die seconde. Bij het verwerken van de DStream, wordt de gebeurtenis temperatuur weergegeven in een van deze batches. Een toepassing voor Spark Streaming verwerkt de batches die de gebeurtenissen bevatten en uiteindelijk omgaat met de gegevens die zijn opgeslagen in elke RDD.

![Voorbeeld DStream met temperatuur-gebeurtenissen ](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-example.png)

## <a name="structure-of-a-spark-streaming-application"></a>Structuur van een toepassing met Spark Streaming

Een toepassing Spark Streaming is een langlopende-toepassing die gegevens uit bronnen van de opname ontvangt, transformaties voor het verwerken van de gegevens is van toepassing en vervolgens implementeert de gegevens naar een of meer doelen. De structuur van een toepassing met Spark Streaming heeft een deel van de statische en dynamische onderdeel. Het statische deel bepaalt wanneer de gegevens zijn afkomstig uit welke verwerking op de gegevens te doen, en de resultaten te gaan. Het dynamische deel, wordt de toepassing voor onbepaalde tijd wachten op een stopsignaal uitgevoerd.

Bijvoorbeeld de volgende eenvoudige toepassing ontvangt van een tekstregel via een TCP-socket en telt het aantal keren dat elk woord wordt weergegeven.

### <a name="define-the-application"></a>De toepassing definiëren

De definitie van de toepassing logische bestaat uit vier stappen:

1. Maak een StreamingContext.
2. Maak een DStream van de StreamingContext.
3. Transformaties toepassen op de DStream.
4. De resultaten.

Deze definitie statisch is en er zijn geen gegevens worden verwerkt totdat u de toepassing uitvoert.

#### <a name="create-a-streamingcontext"></a>Een StreamingContext maken

Maak een StreamingContext van de SparkContext die naar het cluster verwijst. Bij het maken van een StreamingContext, u de grootte van de batch in seconden, bijvoorbeeld:

    val ssc = new StreamingContext(spark, Seconds(1))

#### <a name="create-a-dstream"></a>Een DStream maken

Met het exemplaar StreamingContext maken een invoer DStream voor uw invoerbron. In dit geval wordt de toepassing bekijken voor de weergave van nieuwe bestanden in de standaardopslag die is gekoppeld aan het HDInsight-cluster.

    val lines = ssc.textFileStream("/uploads/2017/01/")

#### <a name="apply-transformations"></a>Transformaties toepassen

U implementeren de verwerking door het toepassen van transformaties op de DStream. Deze toepassing een regel tekst op een tijdstip ontvangt van het bestand, splitst elke regel in woorden en vervolgens een map-reduce-patroon gebruikt voor het tellen van het aantal keer dat die elk woord wordt weergegeven.

    val words = lines.flatMap(_.split(" "))
    val pairs = words.map(word => (word, 1))
    val wordCounts = pairs.reduceByKey(_ + _)

#### <a name="output-results"></a>Resultaten van de uitvoer

Resultaten van de transformatie van push naar de doelsystemen door toe te passen van uitvoer-bewerkingen. In dit geval wordt het resultaat van elke uitvoering door de berekening in de console-uitvoer.

    wordCounts.print()

### <a name="run-the-application"></a>De toepassing uitvoeren

De streaming-toepassing te starten en uitvoeren totdat er een afsluitingssignaal is ontvangen.

    ssc.start()            
    ssc.awaitTermination()

Zie voor meer informatie over de Spark Stream-API, samen met de bronnen van gebeurtenissen, transformaties en uitvoer-bewerkingen ondersteund [Spark Streaming Programming Guide](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html).

Het volgende voorbeeld van toepassing is ingesloten, zodat u kunt uitvoeren in een [Jupyter-Notebook](apache-spark-jupyter-notebook-kernels.md). In dit voorbeeld maakt een mock-gegevensbron in de klasse DummySource die de waarde van een item en de huidige tijd in milliseconden om de vijf seconden uitvoert. Een nieuwe StreamingContext-object heeft een batch-interval van 30 seconden. Telkens wanneer een batch is gemaakt, de streaming toepassing onderzoekt de RDD geproduceerd de RDD converteert naar een Spark-gegevensframe en een tijdelijke tabel wordt gemaakt via het DataFrame.

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

De resulterende uitvoer ziet er als volgt uit:

| waarde | tijd |
| --- | --- |
|10 | 1497314465256 |
|11 | 1497314470272 |
|12 | 1497314475289 |
|13 | 1497314480310 |
|14 | 1497314485327 |
|15 | 1497314490346 |

Er zijn zes waarden, omdat de DummySource een waarde om de vijf seconden maakt en de toepassing een batch elke 30 seconden verzendt.

## <a name="sliding-windows"></a>Windows schuiven

Als u wilt uitvoeren van statistische berekeningen op uw DStream gedurende een bepaalde tijd, bijvoorbeeld om een gemiddelde temperatuur gedurende de afgelopen twee seconden, kunt u de *venster Verschuivend* bewerkingen die zijn opgenomen met Spark Streaming. Een sliding window heeft een duur (lengte van het venster) en het interval gedurende welke de inhoud van het venster zijn geëvalueerd (de dia interval).

Windows Verschuivend kan overlappen, bijvoorbeeld, kunt u een venster met een lengte van twee seconden, die elk één seconde dia's definiëren. Dit betekent dat telkens wanneer u de berekening van een aggregatie uitvoert, in het venster opgenomen gegevens uit de laatste één seconde van de vorige venster en geen nieuwe gegevens in de volgende één seconde.

![Voorbeeld van de eerste venster met temperatuur-gebeurtenissen](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-01.png)

![Voorbeeld-venster met temperatuur gebeurtenissen na schuiven](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-02.png)

Het volgende voorbeeld werkt de code die gebruikmaakt van de DummySource voor het verzamelen van de partijen in een venster met een duur van één minuut en een dia één minuut.

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

Na de eerste minuut zijn er 12 vermeldingen - zes vermeldingen van elk van de twee batches die worden verzameld in het venster.

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

Het sliding vensterfuncties die beschikbaar zijn in de Spark Streaming-API opnemen venster, countByWindow reduceByWindow en countByValueAndWindow. Zie voor meer informatie over deze functies, [transformaties op DStreams](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html#transformations-on-dstreams).

## <a name="checkpointing"></a>Controlepunten plaatsen

Voor het leveren van tolerantie en fouttolerantie, Spark Streaming is afhankelijk van het plaatsen van controlepunten om ervoor te zorgen dat verwerking niet wordt onderbroken, zelfs bij storingen knooppunt. In HDInsight maakt Spark controlepunten duurzame opslag (Azure Storage of Data Lake Store). Deze controlepunten opslaan van de metagegevens over de streaming toepassing zoals de configuratie, de bewerkingen die zijn gedefinieerd door de toepassing en batches die in de wachtrij geplaatst, zijn, maar nog niet is verwerkt. In sommige gevallen worden de controlepunten ook bevatten de gegevens opslaan in de rdd's sneller opnieuw opbouwen van de status van de gegevens van wat aanwezig in de rdd's worden beheerd door Spark is.

## <a name="deploying-spark-streaming-applications"></a>Spark-Streaming-toepassingen implementeren

U doorgaans een Spark Streaming-toepassing lokaal in een JAR-bestand maken en deze vervolgens tot Spark in HDInsight implementeren door in het JAR-bestand kopiëren naar de standaardopslag die is gekoppeld aan uw HDInsight-cluster. U kunt uw toepassing starten met de LIVY REST-API's beschikbaar in uw cluster met behulp van een POST-bewerking. De hoofdtekst van het bericht bevat een JSON-document dat Hiermee wordt het pad naar het JAR, de naam van de klasse waarvan de belangrijkste methode definieert en voert de streaming-toepassing en (optioneel) de resourcevereisten van de taak (zoals het aantal Executor, geheugen en kernen) , en de configuratie-instellingen code van uw toepassing vereist.

![Implementatie van een toepassing voor Spark Streaming](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

De status van alle toepassingen kan ook worden gecontroleerd met een GET-aanvraag voor een LIVY-eindpunt. Ten slotte kunt u een toepassing die wordt uitgevoerd met behulp van een DELETE-aanvraag voor het eindpunt LIVY beëindigen. Zie voor meer informatie over de API LIVY [externe taken met LIVY](apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Volgende stappen

* [Maken van een Apache Spark-cluster in HDInsight](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Spark Streaming Programming Guide](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html)
* [Spark-taken met LIVY op afstand starten](apache-spark-livy-rest-interface.md)
