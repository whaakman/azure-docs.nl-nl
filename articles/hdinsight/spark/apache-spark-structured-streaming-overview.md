---
title: Spark Structured Streaming in Azure HDInsight
description: Het gebruik van Spark Structured Streaming-toepassingen op HDInsight Spark-clusters.
services: hdinsight
author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/05/2018
ms.author: maxluk
ms.openlocfilehash: 7470783ba3ebac652c83c397ba2bbe683023c657
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43041582"
---
# <a name="overview-of-spark-structured-streaming"></a>Overzicht van Spark Structured Streaming

Spark Structured Streaming, kunt u voor het implementeren van schaalbare, hoge doorvoer, fouttolerante toepassingen voor het verwerken van gegevensstromen. Structured Streaming is gebaseerd op de Spark SQL-engine en de concepten van Spark SQL-gegevensframes aanzienlijk verbeterd en gegevenssets, zodat u kunt schrijven streaming-query's op dezelfde manier als u batch-query's schrijft.  

Structured Streaming-toepassingen uitvoeren in HDInsight Spark-clusters en verbinding maken met het streamen van gegevens van Kafka, een TCP-Sockets (ten behoeve van foutopsporing), Azure Storage of Azure Data Lake Store. De laatste twee opties, die afhankelijk van externe storage-services zijn, kunnen u bekijken voor nieuwe bestanden die zijn toegevoegd aan de opslag en verwerking van de inhoud ervan als wanneer ze zijn gestreamd. 

Structured Streaming maakt een langlopende query waarin u bewerkingen kunt toepassen op de invoergegevens, zoals selectie, projectie, aggregatie, windowing en lid worden van het gegevensframe streaming met verwijzing gegevensframes. U kunt vervolgens de resultaten met file storage (Azure Storage-Blobs of Data Lake Store) of naar een gegevensarchief uitvoeren met behulp van aangepaste code (zoals SQL-Database of Power BI). Structured Streaming biedt ook uitvoer naar de console voor foutopsporing op lokaal en in een tabel in het geheugen, zodat u de gegevens die worden gegenereerd voor foutopsporing in HDInsight kunt zien. 

![Stream verwerken met HDInsight en Spark Structured Streaming ](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming.png)

> [!NOTE]
> Spark Structured Streaming is vervangen door Spark Streaming (DStreams). Voortaan kunt ontvangt Structured Streaming uitbreidingen en onderhoud, terwijl DStreams in de onderhoudsmodus alleen. Structured Streaming is momenteel niet als functie aanvullen als DStreams voor de bronnen en sinks dat het gebruiksklaar ondersteunt, dus uw vereisten voor het kiezen van de juiste Spark stream verwerkingsoptie evalueren. 

## <a name="streams-as-tables"></a>Streams als tabellen

Spark Structured Streaming staat voor een stroom van gegevens als een tabel die niet-gebonden in de diepte, dat wil zeggen, de tabel blijft groeien zolang er nieuwe gegevens binnenkomen. Dit *invoertabel* continu wordt verwerkt door een langlopende query en de resultaten verzonden naar een *uitvoertabel*:

![Structured Streaming van Concept](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-concept.png)

In de Structured Streaming gegevens aankomt op het systeem en onmiddellijk in een invoertabel wordt opgenomen. Schrijven van query's (met behulp van het DataFrame en Dataset APIs) die bewerkingen op deze invoertabel uitvoeren. De query-uitvoer resulteert in een andere tabel, de *tabel met resultaten*. De tabel met resultaten bevat de resultaten van uw query, van waaruit u gegevens voor een externe gegevensopslag, een relationele database tekenen. De timing van wanneer gegevens worden verwerkt in de invoertabel wordt bepaald door de *trigger interval*. De trigger-interval is standaard nul, zodat de Structured Streaming wil de gegevens te verwerken wanneer ze worden ontvangen. In de praktijk, dit betekent dat als u de Structured Streaming is voltooid voor het verwerken van de uitvoering van de vorige query wordt een andere verwerking uitvoeren in een nieuw ontvangen gegevens. U kunt de trigger voor uitvoering op een interval configureren zodat de streaminggegevens in batches op basis van tijd is verwerkt. 

De gegevens in de resultaten van tabellen bevatten mag alleen de gegevens die nieuw zijn sinds de laatste keer dat de query is verwerkt (*toevoeg-modus*), of de tabel worden volledig vernieuwd telkens wanneer er nieuwe gegevens is, zodat de tabel alle van de uitvoergegevens omvat sinds het begin van de streaming-query (*volledige modus*).

### <a name="append-mode"></a>Toevoegmodus

Toevoeg-modus, alleen de rijen toegevoegd aan de tabel met resultaten sinds de laatste uitvoering van query's zijn aanwezig in de tabel met resultaten en geschreven naar de externe opslag in. Bijvoorbeeld, de meest eenvoudige query alleen alle worden gegevens gekopieerd van de invoertabel aan de tabel met resultaten ongewijzigd. Telkens wanneer die een trigger-interval is verstreken, de nieuwe gegevens zijn verwerkt en de rijen die aangeeft dat nieuwe gegevens worden weergegeven in de tabel met resultaten. 

U hebt een scenario waarin u telemetrie van Temperatuursensoren, zoals een thermostaat worden verwerkt. Wordt ervan uitgegaan dat de eerste trigger is die een gebeurtenis op tijd 00:01 voor 1 apparaat met een temperatuur van 95 graden verwerkt. In de eerste trigger is van de query, wordt alleen de rij met tijd 00:01 weergegeven in de tabel met resultaten. Het alleen nieuwe rij is de rij met tijd 00:02 om tijd 00:02 wanneer er een andere gebeurtenis binnenkomt, en dus alleen die één rij zou bevatten in de tabel met resultaten.

![Structured Streaming modus toevoegen](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-append-mode.png)

Wanneer met behulp van toevoeg uw query-modus zou worden toepassen projecties (het is belangrijk voor kolommen selecteren), filteren (alleen rijen die voldoen aan bepaalde voorwaarden ophalen) of samenvoegen (uit te breiden met de gegevens met gegevens uit een statische opzoektabel). Toevoeg-modus is het eenvoudig om alleen de relevante nieuwe gegevens te pushen naar een externe opslag wijst.

### <a name="complete-mode"></a>Volledige modus

Houd rekening met de dezelfde scenario, met behulp van de volledige modus. In de modus is voltooid, wordt de hele uitvoertabel vernieuwd op elke trigger, zodat de tabel gegevens niet alleen door de meest recente trigger uitvoeren, maar van alle uitvoeringen bevat. U kunt volledige modus gebruiken om de gegevens ongewijzigd terug uit een invoertabel naar de tabel met resultaten te kopiëren. Voor elke geactiveerde uitvoert, worden de nieuwe Resultatenrijen weergegeven samen met alle rijen in de vorige. De uitvoertabel resultaten uiteindelijk opslaan van alle gegevens die worden verzameld omdat de query is begonnen en u uiteindelijk onvoldoende geheugen uitvoeren zou. Volledige modus is bedoeld voor gebruik met statistische query's waarin de binnenkomende gegevens op een bepaalde manier worden samengevat en enzovoort elke trigger van de tabel met resultaten bijgewerkt met een nieuwe samenvatting. 

Er zijn vijf seconden een hoeveelheid gegevens die al verwerkt en is het tijd om de gegevens voor de zesde seconde verwerken, wordt ervan uitgegaan tot nu toe. De invoertabel heeft gebeurtenissen voor tijd 00:01 en tijd 00:03. Het doel van dit voorbeeld van een query is te geven de gemiddelde temperatuur van het apparaat om de vijf seconden. De uitvoering van deze query is van toepassing een statistische functie waarin alle waarden die kunnen worden onderverdeeld in het venster van elke 5 seconden, een gemiddelde temperatuur en produceert een rij voor de gemiddelde temperatuur tijdens dat interval neemt. Aan het einde van het eerste venster van 5 seconden, er zijn twee tuples: (00:01, 1, 95) en (00:03, 1, 98). Dit het geval is voor het venster 00:00-00:05 de aggregatie produceert een tuple met de gemiddelde temperatuur van 96.5 graden. In het volgende venster 5 seconden is er slechts één gegevenspunt op tijd 00:06, zodat de resulterende gemiddelde temperatuur hoger 98 graden is. Op tijd 00:10, volledige-modus met de tabel met resultaten heeft de rijen voor zowel windows-00:00-00:05 en 00:05:00: 10 omdat de samengevoegde rijen, de uitvoer van de query niet alleen de nieuwe licenties. Daarom blijft de tabel met resultaten groeien zolang er nieuwe windows worden toegevoegd.    

![Structured Streaming volledige modus](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-complete-mode.png)

Niet alle query's voor het gebruik van de volledige modus zorgt ervoor dat de tabel om te groeien zonder grenzen.  Houd rekening met in het vorige voorbeeld, dat in plaats van het gemiddelde berekenen van de temperatuur per tijdvenster, het gemiddelde in plaats daarvan op apparaat-ID. De resulterende tabel bevat een vast aantal rijen (één per apparaat) met de gemiddelde temperatuur van het apparaat voor alle gegevenspunten ontvangen vanaf dat apparaat. Nieuwe temperaturen worden ontvangen, wordt de tabel met resultaten bijgewerkt, zodat de gemiddelden in de tabel nog altijd actueel zijn. 

## <a name="components-of-a-spark-structured-streaming-application"></a>Onderdelen van een toepassing voor Spark Structured Streaming

Een eenvoudig voorbeeld van een query kunt u de temperatuur, serverlogs, door uur windows samenvatten. In dit geval worden de gegevens worden opgeslagen in JSON-bestanden in Azure Storage (gekoppeld als de standaardopslag voor het HDInsight-cluster):

    {"time":1469501107,"temp":"95"}
    {"time":1469501147,"temp":"95"}
    {"time":1469501202,"temp":"95"}
    {"time":1469501219,"temp":"95"}
    {"time":1469501225,"temp":"95"}

Deze JSON-bestanden worden opgeslagen in de `temps` submap onder de container van het HDInsight-cluster. 

### <a name="define-the-input-source"></a>De invoerbron definiëren

Een DataFrame die worden beschreven van de bron van de gegevens en alle instellingen die vereist zijn door die bron voor het eerst configureert. In dit voorbeeld haalt gegevens uit de JSON-bestanden in Azure Storage en een schema is van toepassing op deze tijdens het lezen.

    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._

    //Cluster-local path to the folder containing the JSON files
    val inputPath = "/temps/" 

    //Define the schema of the JSON files as having the "time" of type TimeStamp and the "temp" field of type String
    val jsonSchema = new StructType().add("time", TimestampType).add("temp", StringType)

    //Create a Streaming DataFrame by calling readStream and configuring it with the schema and path
    val streamingInputDF = spark.readStream.schema(jsonSchema).json(inputPath) 

#### <a name="apply-the-query"></a>De query toepassen

Een query met de gewenste bewerkingen op basis van het gegevensframe Streaming vervolgens toepassen. In dit geval een aggregatie van alle rijen worden gegroepeerd in windows van 1 uur en vervolgens worden de minimale, maximale en gemiddelde temperatuur in dit venster 1 uur berekend.

    val streamingAggDF = streamingInputDF.groupBy(window($"time", "1 hour")).agg(min($"temp"), avg($"temp"), max($"temp"))

### <a name="define-the-output-sink"></a>De uitvoerlocatie definiëren

Vervolgens definieert u de bestemming voor de rijen die zijn toegevoegd aan de tabel met resultaten binnen elke trigger-interval. In dit voorbeeld voert alleen alle rijen in een tabel in het geheugen `temps` dat u kunt later een query met SparkSQL. Volledige uitvoermodus zorgt ervoor dat alle rijen voor alle windows uitgevoerd elke keer worden.

    val streamingOutDF = streamingAggDF.writeStream.format("memory").queryName("temps").outputMode("complete") 

### <a name="start-the-query"></a>De query starten

Start het streaming-query en uitvoeren totdat er een afsluitingssignaal is ontvangen. 

    val query = streamingOutDF.start()  

### <a name="view-the-results"></a>De resultaten bekijken

Terwijl de query wordt uitgevoerd, in de dezelfde SparkSession, kunt u een query SparkSQL tegen uitvoeren de `temps` tabel waar de queryresultaten worden opgeslagen. 

    select * from temps

Deze query levert het resultaat is vergelijkbaar met het volgende:


| venster |  min(Temp) | Avg(Temp) | Max(Temp) |
| --- | --- | --- | --- |
|{u'start': u'2016-07-26T02:00:00.000Z', u'end'... |    95 |    95.231579 | 99 |
|{u'start': u'2016-07-26T03:00:00.000Z', u'end'...  |95 |   96.023048 | 99 |
|{u'start': u'2016-07-26T04:00:00.000Z', u'end'...  |95 |   96.797133 | 99 |
|{u'start': u'2016-07-26T05:00:00.000Z', u'end'...  |95 |   96.984639 | 99 |
|{u'start': u'2016-07-26T06:00:00.000Z', u'end'...  |95 |   97.014749 | 99 |
|{u'start': u'2016-07-26T07:00:00.000Z', u'end'...  |95 |   96.980971 | 99 |
|{u'start': u'2016-07-26T08:00:00.000Z', u'end'...  |95 |   96.965997 | 99 |  

Voor meer informatie over de Spark gestructureerde Stream-API, samen met de invoergegevens bronnen, bewerkingen en uitvoer sinks deze ondersteunt, Zie [Spark Structured Streaming Programming Guide](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html).

## <a name="checkpointing-and-write-ahead-logs"></a>Het plaatsen van controlepunten en write-ahead Logboeken

Voor het leveren van tolerantie en fouttolerantie, Structured Streaming is afhankelijk van *plaatsen van controlepunten* om ervoor te zorgen dat stream verwerking niet wordt onderbroken, zelfs met knooppuntfouten. In HDInsight maakt Spark controlepunten naar duurzame opslag, Azure Storage of Data Lake Store. Deze controlepunten opslaan van de van Voortgangsinformatie over de streaming-query. Bovendien Structured Streaming gebruikt een *write-ahead van logboek* (WAL). De WAL vastgelegd opgenomen gegevens die is ontvangen, maar nog niet is verwerkt door een query. Als een fout optreedt en verwerking van de WAL is gestart, zijn geen gebeurtenissen ontvangen van de bron niet verloren gaan.

## <a name="deploying-spark-streaming-applications"></a>Spark-Streaming-toepassingen implementeren

U doorgaans een Spark Streaming-toepassing lokaal in een JAR-bestand maken en deze vervolgens tot Spark in HDInsight implementeren door in het JAR-bestand kopiëren naar de standaardopslag die is gekoppeld aan uw HDInsight-cluster. U kunt uw toepassing starten met de LIVY REST-API's beschikbaar in uw cluster met behulp van een POST-bewerking. De hoofdtekst van het bericht bevat een JSON-document dat Hiermee wordt het pad naar het JAR, de naam van de klasse waarvan de belangrijkste methode definieert en voert de streaming-toepassing en (optioneel) de resourcevereisten van de taak (zoals het aantal Executor, geheugen en kernen) , en de configuratie-instellingen code van uw toepassing vereist.

![Implementatie van een toepassing voor Spark Streaming](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

De status van alle toepassingen kan ook worden gecontroleerd met een GET-aanvraag voor een LIVY-eindpunt. Ten slotte kunt u een toepassing die wordt uitgevoerd met behulp van een DELETE-aanvraag voor het eindpunt LIVY beëindigen. Zie voor meer informatie over de API LIVY [externe taken met LIVY](apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Volgende stappen

* [Maken van een Apache Spark-cluster in HDInsight](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Spark Structured Streaming Programming Guide](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html)
* [Spark-taken met LIVY op afstand starten](apache-spark-livy-rest-interface.md)
