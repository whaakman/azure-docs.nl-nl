---
title: Taken voor prestaties - Azure HDInsight Spark optimaliseren | Microsoft Docs
description: "Bevat algemene strategieën voor de beste prestaties van Spark-clusters."
services: hdinsight
documentationcenter: 
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: maxluk
ms.openlocfilehash: 64ddb70f071a9fadc6fef64dcd3506c6d6255481
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="optimize-spark-jobs"></a>Spark taken optimaliseren

Informatie over het optimaliseren van de configuratie van het Spark-cluster voor uw specifieke werkbelasting.  De meest voorkomende uitdaging is geheugendruk vanwege onjuiste configuraties (met name verkeerde formaat executor), langlopende bewerkingen en taken die in Cartesisch bewerkingen resulteren. U kunt taken met de juiste opslaan in cache en doordat voor versnellen [gegevens tijdverschil](#optimize-joins-and-shuffles). Voor de beste prestaties bewaken en controleren van langlopende en gebruiken van resource Spark taak uitvoeringen.

De volgende secties beschrijven optimalisaties van algemene Spark-taak en aanbevelingen.

## <a name="choose-the-data-abstraction"></a>De abstrahering gegevens kiezen

Spark 1.x gebruikt RDDs abstracte gegevens en Spark 2.x geïntroduceerd DataFrames en gegevenssets. Houd rekening met de volgende relatieve voordelen:

* **DataFrames**
    * Beste keuze in de meeste situaties
    * Biedt queryoptimalisatie via exemplaar
    * Genereren van code geheel fase
    * Directe geheugentoegang
    * Lage overheadkosten garbagecollection (GC)
    * Niet als developer-vriendelijk als gegevenssets, als er geen controles compilatie of domein object programmering zijn
* **DataSets**
    * Goede in complexe ETL pijplijnen waar de prestatie-invloed geaccepteerd wordt
    * Geen geschikte in aggregaties waar de prestatie-invloed aanzienlijk zijn kan
    * Biedt queryoptimalisatie via exemplaar
    * Developer-vriendelijk dankzij de domein-object programmeren en compilatie controles
    * Serialisatie/deserialisatie overhead toegevoegd
    * Hoge GC-overhead
    * Geheel fase codegeneratie wordt verbroken
* **RDDs**
    * In Spark 2.x, u niet hoeft te gebruiken RDDs, tenzij u nodig hebt om een nieuwe aangepaste RDD ontwikkelen
    * Er is geen queryoptimalisatie via exemplaar
    * Er is geen geheel fase codegeneratie
    * Hoge GC-overhead
    * Spark 1.x legacy moet gebruiken API's

## <a name="use-optimal-data-format"></a>Optimale gegevensindeling gebruiken

Spark ondersteunt veel indelingen, zoals csv, json, xml, parketvloeren, orc en avro. Spark kan worden uitgebreid naar veel meer indelingen met externe gegevensbronnen - ondersteuning voor meer informatie, Zie [Spark pakketten](https://spark-packages.org).

De indeling van de beste prestaties is parketvloeren met *treffende compressie*, dit is de standaardwaarde in Spark 2.x. Parketvloeren-gegevens opslaat in kolomindeling en in Spark is geoptimaliseerd.

## <a name="select-default-storage"></a>Standaard-opslag selecteren

Wanneer u een nieuw Spark-cluster maakt, hebt u de optie Azure Blob Storage of Azure Data Lake Store als uw cluster standaard opslag selecteren. Beide opties bieden u het voordeel van opslag op lange termijn voor tijdelijke clusters, zodat uw gegevens komt niet automatisch verwijderd wanneer u het cluster verwijdert. U kunt een tijdelijke cluster opnieuw maken en nog steeds toegang tot uw gegevens.

| Opslagtype | Bestandssysteem | Snelheid | Tijdelijke | Gebruiksvoorbeelden |
| --- | --- | --- | --- | --- |
| Azure Blob Storage | **wasb:**//url/ | **Standard** | Ja | Tijdelijke cluster |
| Azure Data Lake Store | **adl:**//url/ | **Faster** | Ja | Tijdelijke cluster |
| Lokale HDFS | **hdfs:**//url/ | **Fastest** | Nee | Interactieve 24/7-cluster |

## <a name="use-the-cache"></a>Gebruik de cache

Spark biedt een eigen systeemeigen cachebewerkingen mechanismen die kunnen worden gebruikt via andere methoden, zoals `.persist()`, `.cache()`, en `CACHE TABLE`. Deze systeemeigen opslaan in cache werkt met kleine gegevenssets ook zoals ETL pijplijnen waarin u tussenliggende resultaten in de cache wilt in. Echter, Spark systeemeigen opslaan in cache momenteel werkt niet goed met partitioneren, omdat een tabel in de cache de partitionering gegevens niet behouden. Is een techniek voor meer algemene en betrouwbare cachebewerkingen *storage-laag caching*.

* Systeemeigen Spark opslaan in cache (niet aanbevolen)
    * Geschikt voor kleine gegevenssets.
    * Werkt niet met partitioneren, die kunnen worden gewijzigd in toekomstige releases van Spark.

* Opslag niveau opslaan in cache (aanbevolen)
    * Kan worden geïmplementeerd met behulp van [Alluxio](http://www.alluxio.org/).
    * In het geheugen en SSD-caching gebruikt.

* Lokale HDFS (aanbevolen)
    * `hdfs://mycluster`het pad.
    * Maakt gebruik van SSD opslaan in cache.
    * In de cache opgeslagen gegevens niet verloren wanneer u het cluster verwijdert vereisen cache opnieuw opbouwen.

## <a name="use-memory-efficiently"></a>Efficiënt gebruik van geheugen

Spark werkt door het plaatsen van gegevens in het geheugen, zodat het beheren van geheugenbronnen een belangrijk aspect is van het optimaliseren van de uitvoering van taken Spark.  Er zijn verschillende technieken die u kunt toepassen om efficiënt gebruik van geheugen van uw cluster.

* Liever kleinere gegevenspartities en het account voor de gegevensgrootte, typen en distributiepunten in uw strategie voor partitionering.
* Houd rekening met de nieuwere efficiënter [Kryo gegevens serialisatie](https://github.com/EsotericSoftware/kryo), in plaats van de standaard Java-serialisatie.
* Met behulp van YARN, zoals van elkaar gescheiden liever `spark-submit` door de batch.
* Controleren en afstemmen van Spark configuratie-instellingen.

De structuur van de Spark-geheugen en enkele belangrijke executor geheugen parameters voor eigen referentie worden weergegeven in de volgende afbeelding.

### <a name="spark-memory-considerations"></a>Overwegingen met betrekking tot Spark-geheugen

Als u van YARN gebruikmaakt, bepaalt YARN de maximale som van geheugen gebruikt door alle containers op elk knooppunt Spark.  Het volgende diagram toont de belangrijkste objecten en hun relaties.

![YARN Spark geheugenbeheer](./media/apache-spark-perf/yarn-spark-memory.png)

Om 'onvoldoende geheugen' berichten op te lossen, probeert u het:

* Bekijk DAG Management Shuffles. Door de kaart aan clientzijde reducting verminderen, vooraf partitie (of bucketize) brongegevens, één shuffles maximaliseren en verklein de hoeveelheid gegevens die worden verzonden.
* Liever `ReduceByKey` met de vaste geheugenlimiet voor `GroupByKey`, waarmee u aggregaties windowing en andere functies, maar heeft Anne unbounded geheugenlimiet.
* Liever `TreeReduce`, doet meer werk op de Executor of partities te `Reduce`, doet al het werk op het stuurprogramma.
* Maak gebruik van DataFrames in plaats van de objecten RDD op lagere niveaus.
* Maak ComplexTypes dat acties, zoals "Top N", verschillende aggregaties of windowing bewerkingen inkapselen.

## <a name="optimize-data-serialization"></a>Serialisatie van gegevens optimaliseren

Spark taken worden gedistribueerd, zodat de juiste gegevens serialisatie is belangrijk voor de beste prestaties.  Er zijn twee opties voor serialisatie voor Spark:

* Java-serialisatie is de standaardinstelling.
* Kryo serialisatie is een nieuwere indeling en kan leiden tot snellere en meer serialisatie comprimeren dan Java.  Kryo vereist dat u de klassen in uw programma registreren en deze nog niet alle Serialiseerbare typen ondersteunt.

## <a name="use-bucketing"></a>Gebruik bucketing

Bucketing is vergelijkbaar met het partitioneren van gegevens, maar elke bucket kan bevatten een set met kolomwaarden in plaats van slechts één. Bucketing geschikt voor het partitioneren op (in miljoenen of meer) een groot aantal waarden, zoals de product-id's. Een bucket wordt bepaald door de bucket-sleutel van de rij-hashing. Gerangschikte tabellen bieden unieke optimalisaties omdat ze slaan metagegevens over hoe ze waren gerangschikte en gesorteerd.

Sommige geavanceerde functies bucketing zijn:

* Queryoptimalisatie op basis van bucketing metagegevens
* Geoptimaliseerde aggregaties
* Geoptimaliseerde joins

U kunt partitioneren en op hetzelfde moment bucketing gebruiken.

## <a name="optimize-joins-and-shuffles"></a>Samenvoegingen en shuffles optimaliseren

Als u trage taken op een Join of een willekeurige volgorde, de oorzaak is waarschijnlijk *gegevens tijdverschil*, namelijk asymmetrie in uw taakgegevens. Bijvoorbeeld: een taak kaart 20 seconden kan duren, maar uitvoering van een project waarbij de gegevens die lid zijn van of verplaatste uur duurt.   Om op te lossen scheeftrekken gegevens, moet u de gehele sleutel salt of gebruik een *geïsoleerd salt* voor alleen een subset van sleutels.  Als u een geïsoleerde salt gebruikt, moet u verder filteren om te isoleren van uw subset van gezouten sleutels in kaart joins. Een andere optie is een kolom bucket introduceren en vooraf samenvoegen in buckets eerst.

Een andere factor die worden veroorzaakt door trage joins, het jointype kan worden. Spark gebruikt standaard de `SortMerge` type join. Dit type join is het meest geschikt voor grote gegevenssets, maar is anders gegevensindeling omdat deze eerst de linker- en rechterkant van de gegevens sorteren moet voordat ze samen te voegen.

Een `Broadcast` join is het meest geschikt voor kleinere gegevenssets of wanneer een-zijde van de join veel kleiner is dan de andere zijde. Dit type join zendt een-zijde naar alle Executor en dus meer geheugen in het algemeen voor uitzendingen vereist.

U kunt het jointype in uw configuratie wijzigen door in te stellen `spark.sql.autoBroadcastJoinThreshold`, of u kunt een joinhint met behulp van de DataFrame APIs instellen (`dataframe.join(broadcast(df2))`).

```scala
// Option 1
spark.conf.set("spark.sql.autoBroadcastJoinThreshold", 1*1024*1024*1024)

// Option 2
val df1 = spark.table("FactTableA")
val df2 = spark.table("dimMP")
df1.join(broadcast(df2), Seq("PK")).
    createOrReplaceTempView("V_JOIN")
sql("SELECT col1, col2 FROM V_JOIN")
```

Als u gerangschikte tabellen gebruikt, hebt u een externe join-type, de `Merge` join. Een gegevensset correct vooraf gepartitioneerde en vooraf gesorteerde slaat de duur sorteren-fase van een `SortMerge` join.

De volgorde van joins belangrijk is, met name in complexere query's. Beginnen met de meest selectieve joins. Bovendien verplaatsen samenvoegingen die het aantal rijen na aggregaties zo veel mogelijk verhogen.

U kunt geneste structuren toevoegen voor het beheren van parallelle uitvoering specifiek in het geval van Cartesisch joins windowing, en mogelijk een of meer stappen in uw Spark-taak overslaan.

## <a name="customize-cluster-configuration"></a>Clusterconfiguratie aanpassen

Afhankelijk van de werkbelasting van uw Spark-cluster, u kan bepalen dat een niet-standaard Spark configuratie tot meer leiden zou Spark taakuitvoering geoptimaliseerd.  Benchmark testen met voorbeeld werkbelastingen naar een niet-standaard clusterconfiguraties valideren uitvoeren.

Hier volgen enkele algemene parameters die kunt u aanpassen:

* `--num-executors`Hiermee stelt u het juiste aantal Executor.
* `--executor-cores`Hiermee stelt het aantal kernen voor elke executor. U moet hebben doorgaans middle-sized Executor als andere processen verbruiken een deel van het beschikbare geheugen.
* `--executor-memory`Hiermee stelt de grootte van het geheugen voor elke executor die de heapgrootte van de op YARN bepaalt. Laat u sommige geheugen voor uitvoering overhead.

### <a name="select-the-correct-executor-size"></a>Selecteer de juiste executor-grootte

De configuratie van uw executor Houd rekening met de overhead van Java garbagecollection (GC)-collectie.

* Factoren om executor grootte te beperken:
    1. Heap kleiner dan 32 GB te houden GC overhead < 10%.
    2. Verminder het aantal kernen dat GC overhead < 10%.

* Factoren executor vergroot:
    1. Communicatie tussen Executor overhead verminderen.
    2. Verminder het aantal geopende verbindingen tussen Executor (N2) op grotere clusters (> 100 executor).
    3. Om voor geheugenintensieve taken onder te brengen, heap-grootte verhogen.
    4. Optioneel: Verminderen geheugenoverhead per executor.
    5. Optioneel: Vergroten gebruik en een gelijktijdigheid van taken door hogere toewijzing van CPU.

Als een algemene vuistregel bij het selecteren van de grootte van de executor:
    
1. Beginnen met 30 GB per executor en beschikbare machine kernen te verdelen.
2. Verhoog het aantal kernen executor voor grotere clusters (executor > 100).
3. Vergroten of verkleinen op basis van zowel op proefversie wordt uitgevoerd als op de voorgaande factoren zoals GC overhead grootten.

Als u gelijktijdige query's, het volgende overwegen:

1. Beginnen met 30 GB per executor en alle machine kernen.
2. Meerdere parallelle Spark-toepassingen maken door hogere toewijzing van CPU (ongeveer 30% latentie verbetering).
3. Query's verdelen over parallelle toepassingen.
4. Vergroten of verkleinen op basis van zowel op proefversie wordt uitgevoerd als op de voorgaande factoren zoals GC overhead grootten.

Uw queryprestaties voor uitschieters of andere prestatieproblemen bewaken door te kijken naar de tijdlijnweergave, SQL-grafiek, taak statistieken, enzovoort. Soms een of enkele van de Executor zijn langzamer dan de andere en taken neemt veel langer uit te voeren. Dit gebeurt vaak op grotere clusters (> 30 knooppunten). In dit geval wordt het werk onderverdelen in een groter aantal taken, zodat de planner voor langzame taken compenseren kunt. Bijvoorbeeld, hebt u ten minste twee keer zoveel taken als het aantal kernen executor in de toepassing. U kunt ook inschakelen speculatieve uitvoering van taken met `conf: spark.speculation = true`.

## <a name="optimize-job-execution"></a>Taakuitvoering optimaliseren

* In de cache waar nodig, bijvoorbeeld als u de gegevens tweemaal gebruiken, cache plaatsen.
* Variabelen naar alle Executor verzonden. De variabelen geserialiseerde slechts eenmaal in, wat leidt tot sneller zoekacties.
* De thread-groep gebruiken voor het stuurprogramma, wat in een snellere bewerking voor veel taken resulteert.

Uw actieve taken regelmatig om de prestaties bewaken. Als u meer inzicht in bepaalde problemen moet, kunt u een van de volgende prestaties profileren van hulpprogramma's:

* [Intel PAL hulpprogramma](https://github.com/intel-hadoop/PAT) bewaakt CPU-, opslag- en netwerkgebruik bandbreedte.
* [Oracle Java 8 missie besturingselement](http://www.oracle.com/technetwork/java/javaseproducts/mission-control/java-mission-control-1998576.html) profielen Spark en executor-code.

Sleutel voor Spark 2.x queryprestaties is de engine wolfraam, die afhankelijk van het genereren van code geheel-fase is. In sommige gevallen geheel fase codegeneratie mogelijk uitgeschakeld. Bijvoorbeeld, als u een niet-veranderlijke type gebruiken (`string`) in de expressie aggregatie `SortAggregate` wordt weergegeven in plaats van `HashAggregate`. Bijvoorbeeld, voor betere prestaties, probeert u het volgende en genereren van code opnieuw inschakelen:

```sql
MAX(AMOUNT) -> MAX(cast(AMOUNT as DOUBLE))
```

## <a name="next-steps"></a>Volgende stappen

* [Fouten opsporen in uitgevoerd op Azure HDInsight Spark-taken](apache-spark-job-debugging.md)
* [Resources beheren voor een Spark-cluster in HDInsight](apache-spark-resource-manager.md)
* [De Spark REST-API gebruiken om externe taken aan een Spark-cluster te verzenden](apache-spark-livy-rest-interface.md)
* [Spark afstemmen](https://spark.apache.org/docs/latest/tuning.html)
* [Hoe af te stemmen daadwerkelijk uw Spark zodat taken werken ze](https://www.slideshare.net/ilganeli/how-to-actually-tune-your-spark-jobs-so-they-work)
* [Kryo serialisatie](https://github.com/EsotericSoftware/kryo)
