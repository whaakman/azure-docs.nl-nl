---
title: Optimaliseren voor prestaties - Azure HDInsight Spark-taken
description: Bevat algemene strategieën samen voor optimale prestaties van Spark-clusters.
services: hdinsight
ms.service: hdinsight
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/11/2018
ms.openlocfilehash: a6ab4d751be74b66d9e75a37f88bc8d441f9b003
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53653727"
---
# <a name="optimize-apache-spark-jobs"></a>Apache Spark-taken optimaliseren

Meer informatie over het optimaliseren van [Apache Spark](https://spark.apache.org/) clusterconfiguratie voor uw specifieke workload.  De meest voorkomende uitdaging is geheugengbelasting, vanwege een onjuiste configuraties (met name verkeerde formaat executor), langlopende bewerkingen en taken die in Cartesische bewerkingen resulteren. U kunt taken met de juiste opslaan in cache en door toe te staan voor versnellen [gegevensverschil](#optimize-joins-and-shuffles). Voor de beste prestaties bewaken en controleren van langlopende en resource-intensieve Spark taakuitvoeringen.

De volgende secties beschrijven algemene optimalisaties voor Spark-taak en aanbevelingen.

## <a name="choose-the-data-abstraction"></a>Kies de abstractie van de gegevens

Spark-1.x gebruikt rdd's tot abstracte gegevens en vervolgens Spark 2.x geïntroduceerd DataFrames en gegevenssets. Houd rekening met de volgende relatieve voordelen:

* **Gegevensframes**
    * Beste keuze in de meeste situaties.
    * Queryoptimalisatie via katalysator biedt.
    * Genereren van code geheel-fase.
    * Directe geheugentoegang op.
    * Lage overhead garbagecollection (GC).
    * Niet als ontwikkelaarsvriendelijke als gegevenssets, als er geen controles compilatietijdswitch of domein object programmeren zijn.
* **Gegevenssets**
    * Goede in complexe ' ETL-pijplijnen waarbij de invloed op de prestaties acceptabel is.
    * Geen geschikte in aggregaties waar de invloed op de prestaties aanzienlijk kan worden.
    * Queryoptimalisatie via katalysator biedt.
    * Ontwikkelaarsvriendelijke door te geven van de domein-object programmeren en compileren controles.
    * Serialisatie/deserialisatie overhead toegevoegd.
    * Hoge GC overhead.
    * Genereren van code geheel fasen, verbroken.
* **RDDs**
    * In Spark 2.x gebruikt, u hoeft te gebruiken van de rdd's, tenzij u moet een nieuwe aangepaste RDD bouwen.
    * Er is geen queryoptimalisatie via katalysator.
    * Er is geen geheel fase-code genereren.
    * Hoge GC overhead.
    * Gebruik Spark 1.x legacy API's.

## <a name="use-optimal-data-format"></a>Optimale gegevensindeling gebruiken

Spark ondersteunt een groot aantal indelingen, zoals csv, json, xml, parquet, orc en avro. Spark kan worden uitgebreid naar veel meer indelingen met externe gegevensbronnen - ondersteuning voor meer informatie, Zie [Apache Spark-pakketten](https://spark-packages.org).

De beste indeling voor prestaties is parquet met *snappy compressie*, dit is de standaardinstelling in Spark 2.x. Parquet gegevens worden opgeslagen in de indeling in kolomvorm en maximaal in Spark wordt geoptimaliseerd.

## <a name="select-default-storage"></a>Standaardopslag selecteren

Wanneer u een nieuw Spark-cluster maakt, hebt u de optie voor het selecteren van Azure Blob Storage of Azure Data Lake Storage als standaardopslag van uw cluster. Beide opties bieden u het voordeel van langdurige opslag voor tijdelijke clusters, zodat uw gegevens worden niet automatisch verwijderd wanneer u uw cluster verwijdert. U kunt een tijdelijke cluster opnieuw en nog steeds toegang tot uw gegevens.

| Store-Type | Bestandssysteem | Snelheid | Tijdelijke | Gebruiksvoorbeelden |
| --- | --- | --- | --- | --- |
| Azure Blob Storage | **wasb:**//url/ | **Standard** | Ja | Tijdelijke cluster |
| Azure Data Lake Storage | **adl:**//url/ | **Faster** | Ja | Tijdelijke cluster |
| Lokale HDFS | **hdfs:**//url/ | **Fastest** | Nee | Interactieve 24/7-cluster |

## <a name="use-the-cache"></a>De cache gebruiken

Spark biedt een eigen systeemeigen opslaan in cache-mechanismen die kunnen worden gebruikt via verschillende methoden, zoals `.persist()`, `.cache()`, en `CACHE TABLE`. Deze systeemeigen caching werkt met kleine gegevenssets ook zoals in ' ETL-pijplijnen waarbij u tussenliggende resultaten in de cache moet. Echter werkt Spark systeemeigen caching op dit moment niet goed met partitionering, omdat een tabel in de cache de partitionering gegevens niet behouden. Een meer algemene en betrouwbare techniek voor het opslaan in cache is *opslag laag caching*.

* Systeemeigen Spark opslaan in cache (niet aanbevolen)
    * Goed voor kleine gegevenssets.
    * Werkt niet met partities, die kan worden gewijzigd in toekomstige releases van Spark.

* Opslag op opslaan in cache (aanbevolen)
    * Kan worden geïmplementeerd met behulp van [Alluxio](https://www.alluxio.org/).
    * In het geheugen en SSD-opslag gebruikt.

* Lokale HDFS (aanbevolen)
    * `hdfs://mycluster` het pad.
    * Maakt gebruik van SSD opslaan in cache.
    * In de cache opgeslagen gegevens gaan verloren wanneer u het cluster verwijdert dat cache opnieuw opbouwen.

## <a name="use-memory-efficiently"></a>Efficiënt gebruik van geheugen

Spark werkt door het plaatsen van gegevens in het geheugen, zodat het beheren van geheugenbronnen is een belangrijk aspect van het optimaliseren van de uitvoering van Spark-taken.  Er zijn verschillende methoden die u kunt toepassen om efficiënt gebruik van geheugen van uw cluster.

* De voorkeur geeft aan kleinere gegevenspartities en account voor de gegevensgrootte, typen en distributie in uw strategie voor partitioneren.
* Houd rekening met de nieuwere efficiënter [Kryo gegevensserialisatie](https://github.com/EsotericSoftware/kryo), in plaats van de serialisatie van standaard Java.
* Liever met YARN, zoals het scheidt `spark-submit` door de batch.
* Controleren en afstemmen van de Spark-configuratie-instellingen.

De structuur van de Spark-geheugen en enkele belangrijke executor geheugen parameters ter referentie, worden weergegeven in de volgende afbeelding.

### <a name="spark-memory-considerations"></a>Overwegingen met betrekking tot Spark-geheugen

Als u [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), en vervolgens YARN de maximale som van geheugen gebruikt door alle containers op elk knooppunt Spark bepaalt.  Het volgende diagram toont de belangrijkste objecten en hun relaties.

![YARN Spark geheugenbeheer](./media/apache-spark-perf/yarn-spark-memory.png)

Om berichten 'onvoldoende geheugen' op te lossen, kunt u in het volgende proberen:

* DAG Management Shuffles bekijken. Verminderen door de kaart aan clientzijde reducting, vooraf partitie (of worden gecategoriseerd) brongegevens, één shuffles te maximaliseren en verminder de hoeveelheid gegevens die worden verzonden.
* Liever `ReduceByKey` met de vaste geheugenlimiet voor `GroupByKey`, waarmee u aggregaties, windowing en andere functies, maar heeft de niet-gebonden geheugenlimiet Anne.
* Liever `TreeReduce`, die meer werk op de Executor of partities, om te worden `Reduce`, die alle activiteiten op het stuurprogramma biedt.
* Maak gebruik van DataFrames in plaats van de objecten van de RDD op lagere niveaus.
* Maak ComplexTypes die acties, zoals 'Top N', diverse aggregaties of windowing bewerkingen.

## <a name="optimize-data-serialization"></a>Gegevensserialisatie optimaliseren

Spark-taken zijn verdeeld, zodat de juiste gegevensserialisatie is het belangrijk is voor de beste prestaties.  Er zijn twee opties voor serialisatie voor Spark:

* Java-serialisatie is de standaardinstelling.
* Kryo serialisatie is een nieuwere indeling en kan leiden tot snellere en meer serialisatie comprimeren dan Java.  Kryo vereist dat u de klassen in uw programma registreren en het nog niet alle Serialiseerbare typen ondersteunt.

## <a name="use-bucketing"></a>Gebruik bucketing

Bucketing is vergelijkbaar met het partitioneren van gegevens, maar elke bucket kan bevatten een set kolomwaarden in plaats van slechts één. Bucket geschikt voor het partitioneren op (in miljoenen of meer) groot aantal waarden, zoals product-id. Een bucket wordt bepaald door de bucket-sleutel van de rij-hashing. Gerangschikte tabellen bieden unieke optimalisaties omdat ze slaan metagegevens over de manier waarop ze zijn gerangschikte en gesorteerd.

Sommige geavanceerde bucket functies zijn:

* Queryoptimalisatie op basis van bucket meta-informatie.
* Geoptimaliseerde aggregaties.
* Geoptimaliseerde joins.

U kunt partitioneren en op hetzelfde moment bucketing gebruiken.

## <a name="optimize-joins-and-shuffles"></a>Samenvoegingen en shuffles optimaliseren

Als er trage taken op een Join of een willekeurige volgorde, de oorzaak is waarschijnlijk *gegevensverschil*, welke asymmetrie in de taakgegevens van uw is. Bijvoorbeeld, een kaart-taak kan 20 seconden duren, maar het uitvoeren van een taak waar de gegevens is een domein of zij uur duurt.   Om op te lossen gegevensverschil, moet u de volledige code salt of gebruik een *geïsoleerd salt* voor alleen een subset van de sleutels.  Als u een geïsoleerde salt gebruikt, moet u verder filteren om te isoleren van uw subset van gezouten sleutels in kaart joins. Een andere optie is om te leiden van een kolom bucket en vooraf aggregeren in buckets eerst.

Een andere factor die worden veroorzaakt door trage joins, kan het jointype zijn. Standaard Spark maakt gebruik van de `SortMerge` join-type. Dit type join is het meest geschikt voor grote gegevenssets, maar is anders qua rekenkracht kostbaar omdat deze eerst de links en rechts van de gegevens sorteren moet voordat ze samen te voegen.

Een `Broadcast` join is geschikt voor kleinere gegevenssets of wanneer een-zijde van de join is veel kleiner is dan de andere kant. Dit type join zendt een-zijde naar alle Executor en dus meer geheugen in het algemeen voor uitzendingen vereist.

U kunt het jointype in uw configuratie wijzigen door in te stellen `spark.sql.autoBroadcastJoinThreshold`, of kunt u een join-hint met behulp van de APIs DataFrame instellen (`dataframe.join(broadcast(df2))`).

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

Als u van gerangschikte tabellen gebruikmaakt, hebt u een derde join-type, de `Merge` join. Een gegevensset correct vooraf gepartitioneerde en vooraf gesorteerde slaat de duur sorteren fase van een `SortMerge` join.

De volgorde van een join is belangrijk, met name in complexere query's. Beginnen met de meest selectieve joins. Ook, verplaatsen, samenvoegingen die het aantal rijen na aggregaties indien mogelijk verhogen.

U kunt geneste structuren toevoegen voor het beheren van parallelle uitvoering, specifiek in het geval van Cartesische joins, windowing, en mogelijk een of meer stappen in uw Spark-taak overslaan.

## <a name="customize-cluster-configuration"></a>Configuratie van het cluster aanpassen

Afhankelijk van de workload van Spark-cluster kunt u besluiten dat de configuratie van een niet-standaard Spark tot meer leiden zou uitvoeren van de Spark-taak geoptimaliseerd.  Benchmark testen met voorbeeldworkloads te valideren elke niet-standaard-clusterconfiguraties uitvoeren.

Hier volgen enkele algemene parameters die u kunt aanpassen:

* `--num-executors` Hiermee stelt u het juiste aantal Executor.
* `--executor-cores` Hiermee stelt het aantal kernen voor elke executor. U moet hebben doorgaans middle-sized Executor als andere processen enkele van de hoeveelheid beschikbaar geheugen in beslag nemen.
* `--executor-memory` Hiermee stelt de grootte van het geheugen voor elke executor, waarmee wordt bepaald van de heapgrootte van de op YARN. Laat u sommige geheugen voor uitvoering overhead.

### <a name="select-the-correct-executor-size"></a>Selecteer de juiste executor-grootte

Bij het bepalen van de configuratie van de executor, houd rekening met de overhead van Java garbage collection (GC).

* Factoren om executor grootte te beperken:
    1. Heap kleiner dan 32 GB GC overhead < 10% houden.
    2. Verminder het aantal kernen dat GC overhead < 10%.

* Factoren executor vergroten:
    1. De communicatie tussen de Executor overhead te beperken.
    2. Verminder het aantal open verbindingen tussen Executor (N2) op grotere clusters (> 100 executor).
    3. Verhoog de heap-grootte voor de voor geheugen-intensieve taken.
    4. Optioneel: Per executor geheugenoverhead verminderen.
    5. Optioneel: Gebruik en gelijktijdigheid van hogere toewijzing van CPU verhogen.

Als een algemene vuistregel bij het selecteren van de executor-grootte:
    
1. Beginnen met 30 GB per uitvoerder en distribueren van beschikbare machine kernen.
2. Het aantal kernen voor grotere clusters (> 100 executor) executor verhogen.
3. Verhogen of verlagen op basis van zowel evaluatieversie wordt uitgevoerd en op de voorgaande factoren zoals GC overhead grootten.

Bij het uitvoeren van gelijktijdige query's, het volgende overwegen:

1. Beginnen met 30 GB per uitvoerder en alle machine kernen.
2. Spark-meerdere parallelle toepassingen maken met een hogere toewijzing van CPU (ongeveer 30% latentie verbetering).
3. Query's over parallelle toepassingen verdelen.
4. Verhogen of verlagen op basis van zowel evaluatieversie wordt uitgevoerd en op de voorgaande factoren zoals GC overhead grootten.

De prestaties van uw query's voor uitbijters of andere prestatieproblemen controleren door te kijken naar de tijdlijnweergave, SQL-grafiek, taakstatistieken, enzovoort. Soms een of een paar van de Executor zijn langzamer dan de andere, en taken neemt veel langer om uit te voeren. Dit gebeurt vaak op grotere clusters (> 30 knooppunten). In dit geval wordt het werk in een groter aantal taken verdelen, zodat de scheduler voor trage taken compenseren kan. Bijvoorbeeld, hebt u ten minste twee keer zoveel taken als het aantal kernen executor in de toepassing. U kunt ook inschakelen speculatieve uitvoering van taken met `conf: spark.speculation = true`.

## <a name="optimize-job-execution"></a>Taakuitvoering optimaliseren

* Indien nodig, bijvoorbeeld als u twee keer gebruikmaken van de gegevens vervolgens opslaan in cache deze de cache.
* Verzend variabelen naar alle Executor. De variabelen worden alleen geserialiseerd als leidt tot sneller zoekacties.
* De thread-groep gebruiken voor het stuurprogramma, wat tot snellere bewerking voor veel taken leidt.

Bewaken van uw actieve taken regelmatig om de prestaties. Als u meer inzicht in bepaalde problemen nodig hebt, kunt u overwegen een van de volgende prestaties profilering van hulpprogramma's:

* [Intel PAL hulpprogramma](https://github.com/intel-hadoop/PAT) bewaakt CPU, opslag en netwerkgebruik voor bandbreedte.
* [Oracle Java 8 grip](https://www.oracle.com/technetwork/java/javaseproducts/mission-control/java-mission-control-1998576.html) profielen Spark en executor-code.

Sleutel voor de prestaties van Spark 2.x query's is de engine wolfraam, die afhankelijk van het genereren van code geheel-fase is. In sommige gevallen kan genereren van code geheel fasen zijn uitgeschakeld. Bijvoorbeeld, als u een niet-veranderlijke type gebruiken (`string`) in de expressie aggregatie `SortAggregate` wordt weergegeven in plaats van `HashAggregate`. Bijvoorbeeld, voor betere prestaties, probeert u het volgende en genereren van code vervolgens weer inschakelen:

```sql
MAX(AMOUNT) -> MAX(cast(AMOUNT as DOUBLE))
```

## <a name="next-steps"></a>Volgende stappen

* [Apache Spark-taken die worden uitgevoerd op Azure HDInsight](apache-spark-job-debugging.md)
* [Resources beheren voor een Apache Spark-cluster in HDInsight](apache-spark-resource-manager.md)
* [Apache Spark REST-API gebruiken voor het indienen van externe taken met een Apache Spark-cluster](apache-spark-livy-rest-interface.md)
* [Apache Spark afstemmen](https://spark.apache.org/docs/latest/tuning.html)
* [Hoe om af te stemmen daadwerkelijk de Apache Spark-taken zodat werken ze](https://www.slideshare.net/ilganeli/how-to-actually-tune-your-spark-jobs-so-they-work)
* [Kryo serialisatie](https://github.com/EsotericSoftware/kryo)
