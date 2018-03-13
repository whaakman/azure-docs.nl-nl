---
title: Spark gestructureerd Streaming in Azure HDInsight | Microsoft Docs
description: Het gebruik van gestructureerde Spark-Streaming-toepassingen op HDInsight Spark-clusters.
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
ms.openlocfilehash: aa56c1e2f1f506be51f449a1cf10b4f0bc57a152
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="overview-of-spark-structured-streaming"></a>Overzicht van Spark gestructureerd Streaming

Gestructureerde Spark-Streaming, kunt u voor het implementeren van schaalbare hoge gegevensdoorvoer en fouttolerantie toepassingen voor het verwerken van gegevensstromen. Gestructureerde Streaming is gebaseerd op de Spark SQL-engine en de constructs uit Spark SQL gegevensframes aanzienlijk verbeterd en gegevenssets, dus u kunt schrijven streaming query's op dezelfde manier als u zou de batch-query's schrijven.  

Gestructureerde Streaming-toepassingen worden uitgevoerd op een HDInsight Spark-clusters en verbinding maken met het streamen van gegevens van Kafka, een TCP-socket (ten behoeve van foutopsporing), Azure Storage of Azure Data Lake Store. De laatste twee opties, die afhankelijk van services voor externe opslag zijn, zodat u kunt bekijken voor nieuwe bestanden die zijn toegevoegd aan de opslag en verwerken van de inhoud ervan alsof ze zijn gestreamd. 

Gestructureerde Streaming maakt een langlopende query waarin u bewerkingen kunt toepassen op de invoergegevens, zoals selectie, projectie aggregatie, windowing en de streaming DataFrame met verwijzing DataFrames samenvoegen. Vervolgens voert u de resultaten tot bestandsopslag (Azure Storage-Blobs of Data Lake Store) of met een gegevensarchief met behulp van aangepaste code (zoals SQL-Database of Power BI). Gestructureerde Streaming biedt ook uitvoer naar de console voor het opsporen van lokaal en aan een tabel in het geheugen zodat u de gegevens die zijn gegenereerd voor foutopsporing in HDInsight kunt zien. 

![De stroom verwerken met HDInsight en Spark gestructureerd Streaming ](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming.png)

> [!NOTE]
> Gestructureerde Spark-Streaming vervangt Spark-Streaming (DStreams). Voortaan kunt ontvangt gestructureerde Streaming verbeteringen en onderhoud, terwijl de DStreams gaan in de onderhoudsmodus alleen. Gestructureerde Streaming is momenteel niet als functie aan te vullen als DStreams voor de bronnen en sinks ondersteunt gebruiksklaar, dus evalueren uw vereisten voor de juiste Spark stroom verwerkingsoptie kiest. 

## <a name="streams-as-tables"></a>Streams tabellen

Een stream met gegevens als een tabel die in de diepte unbounded gestructureerde Spark-Streaming vertegenwoordigt, dat wil zeggen, de tabel blijft groeien zolang er nieuwe gegevens aankomen. Dit *invoertabel* continu wordt verwerkt door een langdurige query's en de resultaten die zijn verzonden naar een *uitvoertabel*:

![Structuur van Streaming Concept](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-concept.png)

In de gestructureerde Streaming gegevens aankomt bij het systeem en direct in een invoertabel wordt ingenomen. Schrijven van query's (met de DataFrame en Dataset APIs) die bewerkingen op basis van deze invoertabel uitvoeren. De queryuitvoer een andere tabel levert de *resultatentabel*. De tabel met resultaten bevat de resultaten van de query, van waaruit u gegevens voor een externe gegevensarchief, een relationele database wordt getekend. De timing van wanneer gegevens worden verwerkt in de invoertabel wordt bepaald door de *trigger interval*. De trigger-interval is nul, standaard, zodat gestructureerde Streaming probeert de gegevens verwerken zodra deze binnenkomen. In de praktijk dit betekent dat zoals gestructureerde Streaming wordt uitgevoerd voor het verwerken van de uitvoering van de vorige query start een andere verwerking uitvoeren op een nieuw ontvangen gegevens. U kunt de trigger uit te voeren met een tussenpoos configureren zodat de streaminggegevens in batches op basis van tijd is verwerkt. 

Het verwerken van de gegevens in de resultaten tabellen mogen alleen de gegevens die nieuw zijn sinds de laatste keer dat de query bevatten (*modus append*), of de tabel niet volledig vernieuwd telkens wanneer er nieuwe gegevens is, zodat de tabel alle van de uitvoergegevens bevat sinds het begin van de streaming-query (*volledige modus*).

### <a name="append-mode"></a>De modus Append

In de modus, alleen de rijen toevoegen toegevoegd aan de tabel met resultaten sinds de laatste uitvoering van de query zijn aanwezig in de tabel met resultaten en geschreven naar een externe opslag. Bijvoorbeeld kopieert de meest eenvoudige query net alle gegevens van de invoertabel naar de tabel met resultaten ongewijzigd. Telkens wanneer die een trigger-interval is verstreken, de nieuwe gegevens worden verwerkt en de rijen die aangeeft dat de nieuwe gegevens worden weergegeven in de tabel met resultaten. 

Overweeg een scenario waarin u telemetrie van temperatuursensors, zoals een thermostaat verwerkt. Stel dat de eerste trigger is een gebeurtenis op tijd 00:01 om 1 apparaat met een temperatuur bij het lezen van 95 graden verwerkt. In de eerste trigger is van de query, wordt alleen de rij met de tijd 00:01 weergegeven in de tabel met resultaten. De alleen nieuwe rij is de rij met de tijd 00:02 op tijd 00:02 wanneer een andere gebeurtenis binnenkomt, en dus de tabel met resultaten alleen die een rij zou bevatten.

![Modus gestructureerde Streaming Append](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-append-mode.png)

Wanneer met behulp van toevoegen uw query-modus zou worden toepassen projecties (selecteren de kolommen die deze hecht), filteren (ophalen alleen rijen die voldoen aan bepaalde voorwaarden) of samenvoegen (de gegevens met gegevens uit een statische opzoektabel uitbreiden). Modus zorgt ervoor dat deze gemakkelijk voor de push-alleen de relevante nieuwe gegevens uit naar een externe opslag verwijst toevoegen.

### <a name="complete-mode"></a>Volledige modus

Houd rekening met hetzelfde scenario met behulp van de volledige modus. In de modus is voltooid, wordt de hele uitvoertabel op elke trigger vernieuwd, zodat de tabel de gegevens niet alleen uit de meest recente trigger uitvoeren, maar op alle wordt uitgevoerd bevat. U kunt de volledige modus om de gegevens uit een invoertabel ongewijzigd naar de tabel met resultaten te kopiëren. Op elke triggered wordt uitgevoerd weergegeven de nieuwe Resultatenrijen samen met de voorgaande rijen. De resultaten uitvoertabel uiteindelijk opslaan van alle gegevens verzameld, omdat de query is begonnen en u uiteindelijk onvoldoende geheugen uitvoeren zou. Volledige-modus is bedoeld voor gebruik met cumulatieve query's die de binnenkomende gegevens op een bepaalde manier samenvatten en enzovoort elke trigger de tabel met resultaten bijgewerkt met een nieuw overzicht. 

Er zijn vijf seconden aan gegevens is al verwerkt en is het tijd om de gegevens voor het tweede zesde verwerken, wordt ervan uitgegaan tot nu toe. De invoertabel heeft gebeurtenissen voor tijd 00:01 en tijd 00:03. Het doel van deze voorbeeldquery is de gemiddelde temperatuur van het apparaat geven om de vijf seconden. De uitvoering van deze query is van toepassing een statistische functie waarmee alle waarden die elk venster 5 seconden vallen, het gemiddelde van de temperatuur en resulteert in een rij voor de gemiddelde temperatuur via dat interval. Aan het einde van het eerste venster van 5 seconden, er zijn twee tuples: (00:01, 1, 95) en (00:03, 1, 98). Voor het venster 00:00-00:05 de aggregatie produceert een tuple met de gemiddelde temperatuur van 96.5 graden. In het volgende venster 5 seconden is er slechts één gegevenspunt op tijd 00:06, zodat de resulterende gemiddelde temperatuur 98 graden. Op tijd 00:10, volledige-modus met de tabel met resultaten heeft de rijen voor zowel windows-00:00-00:05 en 00:05-00:10 omdat de query alle geaggregeerde rijen worden niet alleen de nieuwe maakt. Daarom blijft de tabel met resultaten groeien zolang er nieuwe vensters worden toegevoegd.    

![Structuur van de volledige modus met stromen](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-complete-mode.png)

Niet alle query's op basis van de volledige modus zorgt ervoor dat de tabel om u te vergroten zonder grenzen.  U kunt in het vorige voorbeeld die in plaats van gemiddelde temperatuur per tijdvenster deze in plaats daarvan een gemiddelde waarde van apparaat-ID. De resultaattabel bevat een vast aantal rijen (één per apparaat) met de gemiddelde temperatuur voor het apparaat voor alle gegevenspunten ontvangen vanaf dat apparaat. Als u nieuwe temperaturen worden ontvangen, wordt de tabel met resultaten bijgewerkt zodat de gemiddelden in de tabel altijd actueel zijn. 

## <a name="components-of-a-spark-structured-streaming-application"></a>Onderdelen van een toepassing gestructureerde Spark-Streaming

Een eenvoudig voorbeeldquery kunt u de metingen temperatuur door uur windows samenvatten. In dit geval worden de gegevens worden opgeslagen in de JSON-bestanden in de Azure-opslag (als de standaard-opslag voor het HDInsight-cluster gekoppeld):

    {"time":1469501107,"temp":"95"}
    {"time":1469501147,"temp":"95"}
    {"time":1469501202,"temp":"95"}
    {"time":1469501219,"temp":"95"}
    {"time":1469501225,"temp":"95"}

Deze JSON-bestanden worden opgeslagen in de `temps` submap onder de container van het HDInsight-cluster. 

### <a name="define-the-input-source"></a>De invoerbron definiëren

Configureer eerst een DataFrame die de bron van de gegevens en instellingen vereist voor deze bron beschrijft. In dit voorbeeld wordt getekend van de JSON-bestanden in Azure Storage en geldt een schema voor deze tijdens het lezen.

    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._

    //Cluster-local path to the folder containing the JSON files
    val inputPath = "/temps/" 

    //Define the schema of the JSON files as having the "time" of type TimeStamp and the "temp" field of type String
    val jsonSchema = new StructType().add("time", TimestampType).add("temp", StringType)

    //Create a Streaming DataFrame by calling readStream and configuring it with the schema and path
    val streamingInputDF = spark.readStream.schema(jsonSchema).json(inputPath) 

#### <a name="apply-the-query"></a>De query toepassen

Vervolgens past u een query met de gewenste bewerkingen op het Streaming-DataFrame toe. In dit geval een aggregatie groepen alle rijen in de windows 1 uur en berekent vervolgens de minimale, gemiddelde en maximale temperaturen in dit venster 1 uur.

    val streamingAggDF = streamingInputDF.groupBy(window($"time", "1 hour")).agg(min($"temp"), avg($"temp"), max($"temp"))

### <a name="define-the-output-sink"></a>De uitvoerlocatie definiëren

Definieer vervolgens de bestemming voor de rijen die zijn toegevoegd aan de tabel met resultaten binnen elke trigger-interval. In dit voorbeeld voert alleen alle rijen aan een tabel in het geheugen `temps` dat u kunt later een query met SparkSQL. Volledige uitvoermodus zorgt ervoor dat alle rijen voor alle windows uitgevoerd elke keer worden.

    val streamingOutDF = streamingAggDF.writeStream.format("memory").queryName("temps").outputMode("complete") 

### <a name="start-the-query"></a>Definieer de query

Open de streaming-query en voer totdat een signaal beëindiging is ontvangen. 

    val query = streamingOutDF.start()  

### <a name="view-the-results"></a>De resultaten bekijken

Terwijl de query wordt uitgevoerd in de dezelfde SparkSession, kunt u een query SparkSQL tegen uitvoeren de `temps` tabel waar de queryresultaten worden opgeslagen. 

    select * from temps

Deze query geeft de resultaten die vergelijkbaar is met het volgende:


| venster |  min(Temp) | Avg(Temp) | Max(Temp) |
| --- | --- | --- | --- |
|{u'start': u'2016-07-26T02:00:00.000Z', u'end'... |    95 |    95.231579 | 99 |
|{u'start': u'2016-07-26T03:00:00.000Z', u'end'...  |95 |   96.023048 | 99 |
|{u'start': u'2016-07-26T04:00:00.000Z', u'end'...  |95 |   96.797133 | 99 |
|{u'start': u'2016-07-26T05:00:00.000Z', u'end'...  |95 |   96.984639 | 99 |
|{u'start': u'2016-07-26T06:00:00.000Z', u'end'...  |95 |   97.014749 | 99 |
|{u'start': u'2016-07-26T07:00:00.000Z', u'end'...  |95 |   96.980971 | 99 |
|{u'start': u'2016-07-26T08:00:00.000Z', u'end'...  |95 |   96.965997 | 99 |  

Voor meer informatie over de API van Spark Structured Stream samen met de invoergegevens bronnen, bewerkingen en uitvoer sinks dit ondersteunt, Zie [Spark gestructureerde Streaming Programming Guide](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html).

## <a name="checkpointing-and-write-ahead-logs"></a>Het plaatsen van controlepunten en automatisch aangevulde schrijven Logboeken

Voor het leveren van de tolerantie en fouttolerantie gestructureerde Streaming is afhankelijk van *plaatsen van controlepunten* om ervoor te zorgen dat stroom verwerking wordt onderbroken, zelfs met knooppuntfouten. In HDInsight maakt Spark controlepunten voor duurzame opslag, Azure Storage of Data Lake Store. Deze controlepunten opslaan van de van de Voortgangsinformatie over de streaming-query. Bovendien gestructureerde Streaming gebruikt een *vooraf geschreven logboek* (WAL). De WAL vastgelegd opgenomen gegevens die zijn ontvangen, maar nog niet is verwerkt door een query. Als een fout optreedt en verwerking van de WAL is gestart, zijn alle gebeurtenissen ontvangen van de bron niet verloren gegaan.

## <a name="deploying-spark-streaming-applications"></a>Spark-Streaming-toepassingen implementeren

U doorgaans een Spark-Streaming-toepassing lokaal in een JAR-bestand maakt en vervolgens implementeren op Spark in HDInsight door het JAR-bestand kopiëren naar de standaard-opslag dat is gekoppeld aan uw HDInsight-cluster. U kunt uw toepassing met de LIVY REST-API's beschikbaar starten van het cluster met een POST-bewerking. De hoofdtekst van het bericht bevat een JSON-document dat het pad aan uw Directory met JAR, de naam van de klasse waarvan hoofdmethode worden gedefinieerd en wordt uitgevoerd de streaming-toepassing, en desgewenst de resourcevereisten van de taak (zoals het aantal Executor, geheugen en kernen) , en alle configuratie-instellingen van uw toepassingscode vereist.

![Een Spark-Streaming-toepassing implementeren](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

De status van alle toepassingen kan ook worden gecontroleerd met een GET-aanvraag voor een eindpunt LIVY. Ten slotte kunt u een actieve toepassing door uitgifte van een DELETE-aanvraag voor het eindpunt LIVY beëindigen. Zie voor meer informatie over de API LIVY [externe taken met LIVY](apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Volgende stappen

* [Maken van een Apache Spark-cluster in HDInsight](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Spark gestructureerd Programmeerhandleiding voor Streaming](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html)
* [Spark-taken op afstand met LIVY starten](apache-spark-livy-rest-interface.md)