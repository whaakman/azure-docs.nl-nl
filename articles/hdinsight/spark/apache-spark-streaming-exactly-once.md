---
title: Taken met Spark Streaming precies maken-eenmaal gebeurtenis verwerken - Azure HDInsight
description: Over het instellen van Spark Streaming voor het verwerken van een gebeurtenis slechts eenmaal.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 194e6091180fa1dd0eaaf999e970c0248ea99db9
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53651772"
---
# <a name="create-apache-spark-streaming-jobs-with-exactly-once-event-processing"></a>Taken met Apache Spark Streaming precies maken-eenmaal gebeurtenis verwerken

Stream-toepassingen voor batchverwerking worden verschillende benaderingen hoe ze opnieuw verwerken om berichten te na een storing in het systeem verwerken:

* Ten minste één keer: Elk bericht wordt gegarandeerd moeten worden verwerkt, maar het kan meer dan één keer verwerkt.
* Maximaal eenmaal: Elk bericht kan of kan niet worden verwerkt. Als een bericht is verwerkt, wordt slechts één keer wordt verwerkt.
* Exact één keer: Elk bericht is gegarandeerd slechts eenmaal worden verwerkt.

In dit artikel leest u hoe het configureren van Spark Streaming te bereiken exact-één keer worden verwerkt.

## <a name="exactly-once-semantics-with-apache-spark-streaming"></a>Precies-once-semantiek met Apache Spark-Streaming

Eerst, houd rekening met hoe alle system Point of failure start opnieuw op nadat er een probleem en hoe kunt u voorkomen dat gegevens verloren gaan. Er is een toepassing voor Spark Streaming:

* Een invoerbron.
* Een of meer ontvanger processen die gegevens van de invoerbron ophalen.
* Taken die de gegevens te verwerken.
* Een uitvoer-sink.
* Een stuurprogramma-proces dat de taak langlopende beheert.

Precies-zodra semantiek vereisen dat er geen gegevens verloren gegaan op elk gewenst moment zijn en die verwerking van berichten kan opnieuw worden gestart, ongeacht waar de fout zich voordoet.

### <a name="replayable-sources"></a>Replayable bronnen

De bron die uw toepassing Spark Streaming van de gebeurtenissen van lezen moet zijn *replayable*. Dit betekent dat in gevallen waarin het bericht opgehaald, maar vervolgens het systeem is mislukt voordat het bericht kan worden bewaard of verwerkt, de bron moet hetzelfde bericht opnieuw opgeven.

In Azure, zowel Azure Event Hubs en [Apache Kafka](https://kafka.apache.org/) op HDInsight bieden replayable bronnen. Een ander voorbeeld van een replayable bron is een fouttolerante bestandssysteem, zoals [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html), Azure Storage-blobs of Azure Data Lake-opslag, waarbij alle gegevens definitief worden bewaard en op elk gewenst moment kunt u de gegevens in zijn geheel opnieuw lezen.

### <a name="reliable-receivers"></a>Betrouwbare ontvangers

Gegevensbronnen in de Spark Streaming, zoals Event Hubs en Kafka *betrouwbare ontvangers*, waarbij elke ontvanger van wordt bijgehouden de voortgang lezen van de bron. Een betrouwbare ontvanger zich blijft voordoen de status in fouttolerante opslag, hetzij in [Apache ZooKeeper](https://zookeeper.apache.org/) of in Spark Streaming controlepunten die worden geschreven naar HDFS. Als deze een ontvanger mislukt en later wordt opnieuw is opgestart, het verder kan gaan waar het afgebroken.

### <a name="use-the-write-ahead-log"></a>Gebruik de Write-Ahead van logboek

Spark Streaming ondersteunt het gebruik van een Write-Ahead van logboek, waarbij elke ontvangen gebeurtenis eerst naar van Spark controlepunt directory in de fouttolerante opslag geschreven en vervolgens opgeslagen in een flexibele gedistribueerd gegevensset (RDD). In Azure is de fouttolerantie opslag ondersteund door Azure Storage of Azure Data Lake Storage HDFS. In uw toepassing Spark Streaming de Write-Ahead van logboek is ingeschakeld voor alle ontvangers door in te stellen de `spark.streaming.receiver.writeAheadLog.enable` configuratie-instelling op `true`. De Write-Ahead van logboek biedt fouttolerantie voor fouten van zowel de Executor als het stuurprogramma.

Voor werknemers met het uitvoeren van taken in de gebeurtenisgegevens, is elke RDD per definitie beide gerepliceerd en verdeeld over meerdere werknemers. Als een taak is mislukt omdat de werknemer die wordt uitgevoerd, is vastgelopen, de taak opnieuw wordt gestart op een andere werknemer die een replica van gegevens van de gebeurtenis heeft, zodat de gebeurtenis is niet verloren gaan.

### <a name="use-checkpoints-for-drivers"></a>Gebruik van controlepunten voor stuurprogramma 's

De taak stuurprogramma's moeten opnieuw worden gestart. Als het uitvoeren van uw toepassing Spark Streaming stuurprogramma vastloopt, duurt het naar beneden ermee alle actieve ontvangers, taken en eventuele rdd's bij het opslaan van gebeurtenisgegevens. In dit geval moet u mogelijk om op te slaan van de voortgang van de taak, zodat u deze later kunt hervatten. Dit wordt bereikt door het plaatsen van controlepunten de omgeleid Acyclic Graph (DAG) van de DStream regelmatig te fouttolerante opslag. De metagegevens van de DAG bevat de configuratie die is gebruikt voor het maken van de streaming-toepassing, de bewerkingen die de toepassing definiëren en batches die in de wachtrij geplaatst, maar nog niet voltooid. Deze metagegevens kan opnieuw worden gestart vanuit de controlepunt-gegevens van een stuurprogramma is mislukt. Wanneer het stuurprogramma opnieuw wordt opgestart, wordt het nieuwe ontvangers dat zelf de gebeurtenisgegevens naar rdd's van de Write-Ahead van logboek herstellen gestart.

Controlepunten zijn ingeschakeld in Spark Streaming in twee stappen. 

1. In het object StreamingContext het opslagpad voor de controlepunten te configureren:

    ```Scala
    val ssc = new StreamingContext(spark, Seconds(1))
    ssc.checkpoint("/path/to/checkpoints")
    ```

    In HDInsight, moeten deze controlepunten worden opgeslagen op de standaardopslag die is gekoppeld aan het cluster, Azure Storage of Azure Data Lake-opslag.

2. Geef vervolgens een controlepunt-interval (in seconden) op de DStream. Tijdens een interval, de gegevens van de gebruikersstatus is afgeleid van de invoer gebeurtenis persistent gemaakt met storage. Gegevens van de permanente status kunnen de berekening die nodig zijn tijdens het opnieuw opbouwen van de status van de bron-gebeurtenis verminderen.

    ```Scala
    val lines = ssc.socketTextStream("hostname", 9999)
    lines.checkpoint(30)
    ssc.start()
    ssc.awaitTermination()
    ```

### <a name="use-idempotent-sinks"></a>Gebruik idempotent sinks

De doel-sink waarnaar uw taak resultaten schrijft moet mogelijk zijn voor het afhandelen van de situatie waarin deze meer dan één keer hetzelfde resultaat wordt gegeven. De sink moet kunnen dergelijke dubbele resultaten detecteren en te negeren. Een *idempotent* sink kan meerdere keren worden aangeroepen met dezelfde gegevens met geen wijziging van de status.

U kunt idempotent sinks maken door het implementeren van logica die eerst controleert op de aanwezigheid van het binnenkomende resultaat in het gegevensarchief. Als het resultaat al bestaat, wordt het schrijven moet worden weergegeven te voltooien vanuit het perspectief van de Spark-taak, maar in werkelijkheid uw data store de dubbele gegevens genegeerd. Als het resultaat niet bestaat, kunnen de sink moet dit nieuwe resultaat invoegen in de opslag. 

U kunt bijvoorbeeld een opgeslagen procedure gebruiken met Azure SQL Database die gebeurtenissen in een tabel invoegen. Deze opgeslagen procedure zoekt eerst naar de gebeurtenis die door de velden voor sleutels, en alleen als er geen overeenkomende gebeurtenis gevonden is de record ingevoegd in de tabel.

Een ander voorbeeld is het gebruik van een gepartitioneerde bestandssysteem, zoals Azure Storage-blobs of Azure Data Lake-opslag. Uw logica sink hoeft in dit geval niet te controleren of er sprake van een bestand. Als het bestand voor de gebeurtenis bestaat, wordt het eenvoudig met dezelfde gegevens overschreven. Anders wordt wordt een nieuw bestand gemaakt in de berekende pad.

## <a name="next-steps"></a>Volgende stappen

* [Apache Spark-Streaming-overzicht](apache-spark-streaming-overview.md)
* [Het maken van maximaal beschikbare Apache Spark Streaming taken in Apache Hadoop YARN](apache-spark-streaming-high-availability.md)
