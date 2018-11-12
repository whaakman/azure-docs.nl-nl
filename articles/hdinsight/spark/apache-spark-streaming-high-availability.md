---
title: Maximaal beschikbare Spark Streaming-taken maken in de YARN - Azure HDInsight
description: Over het instellen van Spark Streaming voor een scenario voor hoge beschikbaarheid.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/26/2018
ms.openlocfilehash: 8f680b60a8f457e1a8619ac044798ff02df15694
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51013646"
---
# <a name="create-high-availability-spark-streaming-jobs-with-yarn"></a>Hoge beschikbaarheid Spark Streaming taken met YARN maken

Spark Streaming, kunt u voor het implementeren van schaalbare, hoge doorvoer, fouttolerante toepassingen voor verwerking van gegevensstromen. U kunt Spark Streaming-toepassingen op een HDInsight Spark-cluster verbinding maken met een verscheidenheid aan gegevensbronnen, zoals Azure Event Hubs, Azure IoT Hub, Kafka, Flume, Twitter, ZeroMQ, onbewerkte TCP-sockets, of door de bewaking van het HDFS-bestandssysteem voor wijzigingen. Biedt ondersteuning voor Spark Streaming-fouttolerantie gebruikt met de garantie dat een bepaalde gebeurtenis exact één keer worden verwerkt, zelfs met een storing op een knooppunt.

Hiermee maakt u Spark Streaming langdurige taken waarbij u zich kunt transformaties toepassen op de gegevens en wordt vervolgens de resultaten voor bestandssystemen, databases, dashboards en de-console. Spark Streaming micro-batches van de gegevens worden verwerkt door het eerste verzamelen van een batch van gebeurtenissen gedurende een opgegeven tijdsinterval. Vervolgens wordt die batch verzonden op voor de verwerking en uitvoer. Batch-tijdsintervallen worden gewoonlijk gedefinieerd in fracties van een seconde.

![Spark-Streaming](./media/apache-spark-streaming-high-availability/spark-streaming.png)

## <a name="dstreams"></a>DStreams

Een continue stroom van gegevens met Spark Streaming vertegenwoordigt een *onderscheiden stream* (DStream). Deze DStream kan worden gemaakt op basis van invoerbronnen, zoals Event Hubs of Kafka of door het toepassen van transformaties op een andere DStream. Wanneer een gebeurtenis in uw toepassing Spark Streaming aankomt, wordt de gebeurtenis opgeslagen in een betrouwbare manier. Gegevens van de gebeurtenis is dat wil zeggen, zodat meerdere knooppunten een kopie van het hebben gerepliceerd. Dit zorgt ervoor dat het uitvallen van een enkel knooppunt niet tot verlies van de gebeurtenis leiden wordt.

Maakt gebruik van de Spark core *resilient distributed datasets* (rdd's). Gegevens verdelen rdd's over meerdere knooppunten in het cluster, waarbij elk knooppunt in het algemeen de gegevens volledig in het geheugen voor de beste prestaties houdt. Elke RDD vertegenwoordigt gebeurtenissen die worden verzameld tijdens een interval van batch. Als het batch-interval is verstreken, levert Spark Streaming een nieuwe RDD met alle gegevens in dat het interval. Deze continue reeks rdd's zijn verzameld in een DStream. Een toepassing voor Spark Streaming verwerkt de gegevens die zijn opgeslagen in de RDD van elke batch.

![Spark DStream](./media/apache-spark-streaming-high-availability/DStream.png)

## <a name="spark-structured-streaming-jobs"></a>Spark Structured Streaming-taken

Spark Structured Streaming werd geïntroduceerd in Spark 2.0 als een analytische engine voor gebruik op het streaming-gestructureerde gegevens. Spark Structured Streaming maakt gebruik van de SparkSQL batchverwerking engine API's. Spark Structured Streaming met een Spark Streaming, wordt de berekeningen via continu binnenkomen micro-batches van gegevens uitgevoerd. Spark Structured Streaming staat voor een stroom van gegevens als een invoertabel met onbeperkte rijen. Dat wil zeggen, blijft de invoertabel groeien zolang er nieuwe gegevens binnenkomen. Deze invoertabel continu wordt verwerkt door een langlopende query en de resultaten worden geschreven naar een uitvoer-tabel.

![Spark Structured Streaming](./media/apache-spark-streaming-high-availability/structured-streaming.png)

In de Structured Streaming gegevens aankomt op het systeem en direct in de tabel invoer wordt opgenomen. Schrijven van query's die bewerkingen op basis van deze tabel invoer uitvoeren. De query-uitvoer resulteert in een andere tabel, met de naam van de tabel met resultaten. Resultaten van uw query, van waaruit u gegevens worden verzonden naar een externe gegevensarchief tekenen van de tabel met resultaten bevat een relationele database. De *trigger interval* Hiermee stelt u het tijdstip voor wanneer de gegevens uit de invoer-tabel is verwerkt. Standaard verwerkt Structured Streaming de gegevens als ze worden ontvangen. U kunt echter ook de trigger om uit te voeren op een langere interval, zodat de streaminggegevens wordt verwerkt in batches op basis van tijd configureren. De gegevens in de tabel met resultaten kunnen worden volledig vernieuwd telkens wanneer er nieuwe gegevens is, zodat deze alle van de uitvoergegevens bevat sinds het begin van de streaming-query (*volledige modus*), of alleen de gegevens die nieuw zijn sinds de laatste is alleen kan bevatten Wanneer de query is verwerkt (*toevoeg-modus*).

## <a name="create-fault-tolerant-spark-streaming-jobs"></a>Fouttolerante Spark Streaming taken maken

Start uw afzonderlijke taken voor herstel in het geval van storing coderen voor het maken van een maximaal beschikbare omgeving voor uw Spark Streaming-taken. Dergelijke zelf herstellen taken zijn fouttolerantie.

Rdd's hebben verschillende eigenschappen die hoge mate beschikbare en fouttolerante Spark Streaming taken helpen:

* Batches van invoergegevens die in de rdd's worden opgeslagen als een DStream worden automatisch gerepliceerd in het geheugen voor fouttolerantie.
* Gegevens verloren zijn gegaan vanwege fout bij de werkrol kan worden herberekend gerepliceerde invoergegevens van verschillende werknemers, zolang deze worker-knooppunten beschikbaar zijn.
* Snel herstel van de fout kan zich voordoen binnen één seconde, zoals herstel na fouten/stragglers via berekening in het geheugen gebeurt.

### <a name="exactly-once-semantics-with-spark-streaming"></a>Precies-once-semantiek met Spark Streaming

Voor het maken van een toepassing waarmee elke gebeurtenis wordt verwerkt zodra (en slechts één keer), houd rekening met hoe alle system Point of failure start opnieuw op nadat er een probleem en hoe kunt u voorkomen dat gegevens verloren gaan. Precies-zodra semantiek vereisen dat er geen gegevens verloren gegaan op elk gewenst moment zijn en die verwerking van berichten kan opnieuw worden gestart, ongeacht waar de fout zich voordoet. Zie [maakt Spark Streaming taken met exact-eenmaal gebeurtenis verwerken](apache-spark-streaming-exactly-once.md).

## <a name="spark-streaming-and-yarn"></a>Spark-Streaming en YARN

In HDInsight, cluster werken, wordt gecoördineerd door *nog een andere Resource Negotiator* (YARN). Hoge beschikbaarheid ontwerpen voor Spark Streaming bestaat uit technieken voor het streamen van Spark en ook voor YARN-onderdelen.  Hieronder ziet u een van de voorbeeldconfiguratie met behulp van YARN. 

![YARN-architectuur](./media/apache-spark-streaming-high-availability/yarn-arch.png)

De volgende secties worden de overwegingen bij het ontwerp voor deze configuratie.

### <a name="plan-for-failures"></a>Plan voor fouten

Als u wilt een YARN-configuratie voor hoge beschikbaarheid maken, moet u van plan bent een mogelijke uitvoerder of stuurprogramma is mislukt. Sommige taken Spark Streaming ook garantie gegevensvereisten waarvoor aanvullende configuratie en installatie. Een streaming-toepassing kan bijvoorbeeld een zakelijke vereiste is om een nul--verlies van gegevens gegarandeerd ondanks elke fout die in de streaming hostsysteem of het HDInsight-cluster optreedt.

Als een **executor** mislukt, de taken en de ontvangers worden automatisch opnieuw opgestart door Spark, dus er geen wijziging in de configuratie die nodig zijn is.

Echter, als een **stuurprogramma** mislukt, en vervolgens alle van de bijbehorende Executor mislukken en alle ontvangen blokken en berekenen van de resultaten gaan verloren. Een stuurprogramma om fout te herstellen, gebruikt u *DStream plaatsen van controlepunten* zoals beschreven in [maakt Spark Streaming taken met exact-eenmaal gebeurtenis verwerken](apache-spark-streaming-exactly-once.md#use-checkpoints-for-drivers). Het plaatsen van controlepunten DStream slaat periodiek de *omgeleid acyclische grafiek* (DAG) van DStreams naar fouttolerante opslag zoals Azure Storage.  Het plaatsen van controlepunten kunt Spark Structured Streaming om opnieuw te starten van de mislukte stuurprogramma van de controlepunt-informatie.  Dit stuurprogramma opnieuw wordt gestart nieuwe Executor en ontvangers ook opnieuw is opgestart.

Stuurprogramma's met DStream plaatsen van controlepunten herstellen:

* Opnieuw opstarten automatisch stuurprogramma op YARN configureren met de configuratie-instelling `yarn.resourcemanager.am.max-attempts`.
* Een controlepunt instellen in een HDFS-compatibele bestandssysteem met `streamingContext.checkpoint(hdfsDirectory)`.
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

* Herstel voor verlies van gegevens configureren doordat de write-ahead van logboek (WAL) met `sparkConf.set("spark.streaming.receiver.writeAheadLog.enable","true")`, en schakel de replicatie in het geheugen voor invoer DStreams met `StorageLevel.MEMORY_AND_DISK_SER`.

Om samen te vatten, met behulp van het plaatsen van controlepunten, WAL + betrouwbare ontvangers, kunt u zich te leveren 'ten minste eenmaal' gegevensherstel:

* Eenmalige, zolang de ontvangen gegevens niet verloren en wordt de uitvoer zijn beide idempotent of transactionele.
* Exact één keer met de nieuwe Kafka directe methode die gebruikmaakt van Kafka als een gerepliceerde logboek, in plaats van met ontvangers of WALs.

### <a name="typical-concerns-for-high-availability"></a>Typische zorgen voor hoge beschikbaarheid

* Het is het moeilijker voor het bewaken van streamingtaken dan batch-taken. Spark Streaming taken zijn doorgaans langlopende en YARN Logboeken niet samenvoegen tot een taak is voltooid.  Spark controlepunten zijn verloren gaan tijdens de toepassing of Spark upgrades worden uitgevoerd en moet u de map controlepunt wissen tijdens een upgrade.

* Configureer uw clustermodus YARN om uit te voeren van stuurprogramma's, zelfs als een client is mislukt. Automatisch opnieuw opstarten voor stuurprogramma's instellen:

    ```
    spark.yarn.maxAppAttempts = 2
    spark.yarn.am.attemptFailuresValidityInterval=1h
    ```

* Spark- en de gebruikersinterface voor het streamen van Spark beschikken over een configureerbare metrische gegevens. U kunt ook extra bibliotheken, zoals grafiek/Grafana voor het downloaden van dashboard metrische gegevens, zoals 'num records verwerkt', ' Geheugen/GC gebruik op stuurprogramma & Executor', 'totale vertraging', '-gebruik van het cluster' enzovoort. In de Structured Streaming versie 2.1 of hoger, kunt u `StreamingQueryListener` voor het verzamelen van aanvullende metrische gegevens.

* U te langlopende taken segmenteren.  Als een toepassing voor Spark Streaming wordt verzonden naar het cluster, moet de YARN-wachtrij waarop de taak wordt uitgevoerd worden gedefinieerd. U kunt een [YARN capaciteit Scheduler](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html) langlopende taken naar een afzonderlijke wachtrijen indienen.

* Uw toepassing met streaming zonder problemen worden afgesloten. Als uw verschuivingen bekend zijn, en alle toepassingsstatus extern is opgeslagen, kunt u uw streaming-toepassing op de juiste plaats via een programma stoppen. Een techniek is om te gebruiken "thread haken" in Spark, door te controleren voor een externe vlag elke *n* seconden. U kunt ook een *markeringsbestand* die is gemaakt op HDFS wanneer de toepassing wordt gestart, wordt verwijderd wanneer u wilt stoppen. Gebruik een afzonderlijke thread in uw Spark-toepassing die code ongeveer als volgt worden aangeroepen voor een benadering van het bestand markering:

    ```scala
    streamingContext.stop(stopSparkContext = true, stopGracefully = true)
    // to be able to recover on restart, store all offsets in an external database
    ```

## <a name="next-steps"></a>Volgende stappen

* [Spark-Streaming-overzicht](apache-spark-streaming-overview.md)
* [Taken met Spark Streaming precies maken-eenmaal gebeurtenis verwerken](apache-spark-streaming-exactly-once.md)
* [Langlopende Spark-Streamingtaken in YARN](http://mkuthan.github.io/blog/2016/09/30/spark-streaming-on-yarn/) 
* [Structured Streaming: Fout met betrekking tot fouttolerante semantiek](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html#fault-tolerance-semantics)
* [Onderscheiden Streams: Een fouttolerantie Model voor de verwerking van schaalbare Stream](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2012/EECS-2012-259.pdf)
