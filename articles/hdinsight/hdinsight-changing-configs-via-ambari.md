---
title: Configuraties van clusters met Ambari - Azure HDInsight optimaliseren | Microsoft Docs
description: Gebruik de Ambari-webgebruikersinterface om te configureren en optimaliseren van HDInsight-clusters.
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: ashish
ms.openlocfilehash: 74c1b3298cd7b6ffd5b4a60e2fa78ed733232f92
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="use-ambari-to-optimize-hdinsight-cluster-configurations"></a>Ambari gebruiken om de configuraties van HDInsight-clusters optimaliseren

HDInsight biedt Apache Hadoop-clusters voor grootschalige gegevensverwerking toepassingen. Beheer, bewaking en optimaliseren van deze complexe clusters met meerdere knooppunten kunnen lastig zijn. [Apache Ambari](http://ambari.apache.org/) is een webinterface beheren en bewaken van Linux in HDInsight-clusters.  Voor Windows-clusters, gebruikt u de Ambari [REST-API](hdinsight-hadoop-manage-ambari-rest-api.md).

Zie voor een inleiding tot met de Ambari-Webgebruikersinterface, [HDInsight-clusters beheren met behulp van de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md)

Aanmelden bij de Ambari op `https://CLUSTERNAME.azurehdidnsight.net` met de referenties van uw cluster. Het eerste scherm bevat een Overzichtsdashboard.

![Ambari-dashboard](./media/hdinsight-changing-configs-via-ambari/ambari-dashboard.png)

De Ambari-webgebruikersinterface kan worden gebruikt voor het beheren van hosts, services, waarschuwingen, configuraties en weergaven. Ambari kan niet worden gebruikt om een HDInsight-cluster maken, bijwerken van services, stacks en versies beheren, uit bedrijf nemen of recommission hosts of nieuwe services toegevoegd aan het cluster.

## <a name="manage-your-clusters-configuration"></a>Configuratie van uw cluster beheren

Configuratie-instellingen helpen bij het afstemmen van een bepaalde service. Voor het wijzigen van configuratie-instellingen van een service, selecteer de service uit de **Services** zijbalk (aan de linkerkant) en navigeer vervolgens naar de **Configs** tabblad in de detailpagina service.

![Services zijbalk](./media/hdinsight-changing-configs-via-ambari/services-sidebar.png)

### <a name="modify-namenode-java-heap-size"></a>NameNode Java, heap-grootte wijzigen

De NameNode Java, heap-grootte is afhankelijk van veel factoren zoals de belasting van het cluster, het aantal bestanden en het aantal blokken. De standaardgrootte van 1 GB werkt goed samen met de meeste clusters, hoewel sommige werkbelastingen kunnen meer of minder geheugen vereist. 

De NameNode Java, heap-grootte wijzigen:

1. Selecteer **HDFS** van de zijbalk Services en navigeer naar de **Configs** tabblad.

    ![HDFS-configuratie](./media/hdinsight-changing-configs-via-ambari/hdfs-config.png)

2. De instelling **NameNode Java, heap-grootte**. U kunt ook de **filter** tekstvak te typen en een bepaalde instelling te vinden. Selecteer de **pen** het pictogram naast de naam van de instelling.

    ![NameNode Java, heap-grootte](./media/hdinsight-changing-configs-via-ambari/java-heap-size.png)

3. Typ de nieuwe waarde in het tekstvak in en druk vervolgens op **Enter** de wijziging op te slaan.

    ![NameNode Java, heap-grootte bewerken](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edit.png)

4. De NameNode Java, heap-grootte wordt gewijzigd naar 2 GB van 1 GB.

    ![Bewerkt NameNode Java, heap-grootte](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edited.png)

5. Sla de wijzigingen door te klikken op de groene **opslaan** knop boven aan het Configuratiescherm.

    ![Wijzigingen opslaan](./media/hdinsight-changing-configs-via-ambari/save-changes.png)

## <a name="hive-optimization"></a>Hive-optimalisatie

De volgende secties beschrijven de configuratieopties voor het optimaliseren van de algehele prestaties van Hive.

1. Selecteer voor het wijzigen van Hive-configuratieparameters **Hive** van de zijbalk Services.
2. Navigeer naar de **Configs** tabblad.

### <a name="set-the-hive-execution-engine"></a>De engine voor het uitvoeren van Hive instellen

Hive biedt twee uitvoering engines: MapReduce en Tez. Tez is sneller dan MapReduce. Tez hebben HDInsight Linux-clusters als de engine voor het uitvoeren van standaard. De engine voor het uitvoeren wijzigen:

1. In de component **Configs** tabblad, typt u **uitvoeringsengine** in het filtervak.

    ![Engine voor het uitvoeren van zoeken](./media/hdinsight-changing-configs-via-ambari/search-execution.png)

2. De **optimalisatie** is de standaardwaarde van de eigenschap **Tez**.

    ![Optimalisatie - Tez](./media/hdinsight-changing-configs-via-ambari/optimization-tez.png)

### <a name="tune-mappers"></a>Mappers afstemmen

Hadoop probeert te splitsen (*kaart*) één bestand in meerdere bestanden en de resulterende proces parallel bestanden. Het aantal mappers is afhankelijk van het aantal splitsingen. De volgende configuratie met twee parameters stations het aantal splitsingen voor de Tez-uitvoeringsengine:

* `tez.grouping.min-size`: Ondergrens van de grootte van een gegroepeerde splitsing met een standaardwaarde van 16 MB (16,777,216 bytes).
* `tez.grouping.max-size`: Bovengrens van de grootte van een gegroepeerde splitsing met een standaardwaarde van 1 GB (1,073,741,824 bytes).

Als een vuistregel prestaties afnemen u beide parameters te verhogen voor een meer doorvoer, latentie verbeterd.

Bijvoorbeeld, om in te stellen vier mapper-taken voor een gegevensgrootte van 128 MB, stelt u beide parameters op 32 MB elke (33,554,432 bytes).

1. Voor het wijzigen van de parameters limiet, gaat u naar de **Configs** tabblad van de Tez-service. Vouw de **algemene** deelvenster en zoek de `tez.grouping.max-size` en `tez.grouping.min-size` parameters.

2. Beide parameters ingesteld op **33,554,432** bytes (32 MB).

    ![Tez groepering grootten](./media/hdinsight-changing-configs-via-ambari/tez-grouping-size.png)
 
Deze wijzigingen van invloed op alle Tez-taken via de server. Als u een optimaal resultaat, kies juiste parameterwaarden.

### <a name="tune-reducers"></a>Verkleiningstoestellen afstemmen

ORC en Snappy bieden hoge prestaties. Echter Hive mogelijk te weinig verkleiningstoestellen standaard knelpunten veroorzaakt.

Stel dat u hebt een grootte van de invoergegevens van 50 GB. Dat gegevens in ORC met treffende compressie opmaken is 1 GB. Hive maakt een schatting van het aantal verkleiningstoestellen nodig als: (aantal bytes invoer mappers / `hive.exec.reducers.bytes.per.reducer`).

In dit voorbeeld wordt met de standaardinstellingen 4 verkleiningstoestellen.

De `hive.exec.reducers.bytes.per.reducer` parameter geeft u het aantal bytes dat per reducer wordt verwerkt. De standaardwaarde is 64 MB. Deze waarde afstemmen omlaag verhoogt de parallelle uitvoering en de prestaties mogelijk verbeterd. Afstemmen te laag kan ook produceren voor te veel verkleiningstoestellen, heeft mogelijk een nadelige invloed op prestaties. Deze parameter is gebaseerd op uw specifieke vereisten, compressie-instellingen en andere omgevingsfactoren.

1. Navigeer naar de component voor het wijzigen van de parameter **Configs** tabblad en zoek de **gegevens per Reducer** parameter op de pagina instellingen.

    ![Gegevens per Reducer](./media/hdinsight-changing-configs-via-ambari/data-per-reducer.png)
 
2. Selecteer **bewerken** voor het wijzigen van de waarde moet 128 MB (134,217,728 bytes) en druk vervolgens op **Enter** om op te slaan.

    ![Gegevens per Reducer - bewerkt](./media/hdinsight-changing-configs-via-ambari/data-per-reducer-edited.png)
  
    Gegeven van een invoer grootte van maximaal 1024 MB, met 128 MB aan gegevens per reducer, er 8 verkleiningstoestellen zijn (1024/128).

3. Een onjuiste waarde voor de **gegevens per Reducer** parameter kan leiden tot een groot aantal verkleiningstoestellen, heeft een nadelige invloed op prestaties van query's. Instellen om te beperken het maximum aantal verkleiningstoestellen, `hive.exec.reducers.max` naar een geschikte waarde. De standaardwaarde is 1009.

### <a name="enable-parallel-execution"></a>Parallelle uitvoering inschakelen

Een Hive-query wordt uitgevoerd in een of meer fasen. Als de onafhankelijke fasen kunnen parallel worden uitgevoerd, wordt die queryprestaties verhogen.

1.  Om parallelle queryuitvoering, gaat u naar de component **Config** tabblad en zoek naar de `hive.exec.parallel` eigenschap. De standaardwaarde is ingesteld op false. Wijzig de waarde true en druk vervolgens op **Enter** om op te slaan van de waarde.
 
2.  Beperk het aantal taken parallel worden uitgevoerd, wijzigt u de `hive.exec.parallel.thread.number` eigenschap. De standaardwaarde is 8.

    ![Exec parallel hive](./media/hdinsight-changing-configs-via-ambari/hive-exec-parallel.png)


### <a name="enable-vectorization"></a>Vectorization inschakelen

Hive verwerkt gegevens per rij. Vectorization stuurt Hive om gegevens te verwerken in blokken van 1024 rijen in plaats van één rij tegelijk. Vectorization is alleen van toepassing op de ORC-bestandsindeling.

1. Om een vectorized queryuitvoering, gaat u naar de component **Configs** tabblad en zoek naar de `hive.vectorized.execution.enabled` parameter. De standaardwaarde is true voor Hive 0.13.0 of hoger.
 
2. Om in te schakelen vectorized uitvoeren voor de verminderen-zijde van de query, stel de `hive.vectorized.execution.reduce.enabled` parameter in op true. De standaardwaarde is ingesteld op false.

    ![Uitvoeren van hive vectorized](./media/hdinsight-changing-configs-via-ambari/hive-vectorized-execution.png)

### <a name="enable-cost-based-optimization-cbo"></a>Kosten gebaseerde optimalisatie (CBO) inschakelen

Standaard volgt Hive een reeks regels voor een optimale query-uitvoeringsplan vinden. Kosten gebaseerde optimalisatie (CBO) meerdere plannen om uit te voeren een query wordt geëvalueerd en kosten toegewezen aan elk plan vervolgens bepaalt de goedkoopste plan een query uit te voeren.

Om in te schakelen CBO, gaat u naar de component **Configs** tabblad en zoek naar `parameter hive.cbo.enable`, schakelt u de wisselknop wilt **op**.

![CBO config](./media/hdinsight-changing-configs-via-ambari/cbo.png)

De volgende aanvullende configuratieparameters verhogen Hive-query-prestaties als CBO is ingeschakeld:

* `hive.compute.query.using.stats`

    Wanneer ingesteld op true, Hive statistieken die zijn opgeslagen in de metastore gebruikt voor het beantwoorden van eenvoudige query's zoals `count(*)`.

    ![CBO statistieken](./media/hdinsight-changing-configs-via-ambari/hive-compute-query-using-stats.png)

* `hive.stats.fetch.column.stats`

    Kolom statistieken worden gemaakt wanneer CBO is ingeschakeld. Hive gebruikt kolomstatistieken die zijn opgeslagen in metastore, query's optimaliseren. Ophalen van de kolomstatistieken voor elke kolom duurt langer als het aantal kolommen hoog is. Deze instelling wanneer ingesteld op false, wordt ophalen kolomstatistieken van de metastore uitgeschakeld.

    ![Hive-statistieken set kolom statistieken](./media/hdinsight-changing-configs-via-ambari/hive-stats-fetch-column-stats.png)

* `hive.stats.fetch.partition.stats`

    Basic partitiestatistieken zoals het aantal rijen, de gegevensgrootte en de bestandsgrootte worden opgeslagen in metastore. Wanneer ingesteld op true, wordt de partitie van metastore statistieken worden opgehaald. Als deze eigenschap ONWAAR is, de grootte van het bestandssysteem wordt opgehaald en het aantal rijen wordt opgehaald uit het schema van de rij.

    ![Hive-statistieken set partitie statistieken](./media/hdinsight-changing-configs-via-ambari/hive-stats-fetch-partition-stats.png)

### <a name="enable-intermediate-compression"></a>Tussenliggende compressie inschakelen

Taken map maken tussenliggende bestanden die worden gebruikt door de taken reducer. Tussenliggende compressie wordt de grootte van het tussenliggende kleiner.

Hadoop-taken zijn meestal i/o knelpunt worden verwerkt. Comprimeren van gegevens kunt versnellen i/o- en algemene netwerkoverdracht.

De beschikbare compressietypen zijn:

| Indeling | Hulpprogramma | Algoritme | Bestandsextensie | SplitTable? |
| -- | -- | -- | -- | -- |
| Gzip | Gzip | VERKLEINEN | .gz | Nee |
| Bzip2 | Bzip2 | Bzip2 |.bz2 | Ja |
| LZO | Lzop | LZO | .lzo | Ja, als geïndexeerd |
| treffende | N/A | treffende | treffende | Nee |

Als een regel met de splittable compressiemethode is belangrijk, anders heel weinig mappers wordt gemaakt. Als de invoergegevens tekst, `bzip2` is de beste optie. Snappy is de snelste compressieoptie voor ORC-indeling.

1. Voor het tussenliggende compressie inschakelen, gaat u naar de component **Configs** tabblad en stel vervolgens de `hive.exec.compress.intermediate` parameter in op true. De standaardwaarde is ingesteld op false.

    ![Exec comprimeren tussenliggende hive](./media/hdinsight-changing-configs-via-ambari/hive-exec-compress-intermediate.png)

    > [!NOTE]
    > Tussenliggende om bestanden te comprimeren, kiest u een compressiecodec bij lagere kosten, zelfs als de codec beschikt niet over een hoge mate van compressie-uitvoer.

2. De aangepaste eigenschap toevoegen om in te stellen de tussenliggende compressiecodec, `mapred.map.output.compression.codec` naar de `hive-site.xml` of `mapred-site.xml` bestand.

3. Een aangepaste instelling toevoegen:

    a. Navigeer naar de component **Configs** tabblad en selecteer de **Geavanceerd** tabblad.

    b. Onder de **Geavanceerd** tabblad, zoeken en vouw de **aangepaste hive-site** deelvenster.

    c. Klik op de koppeling **eigenschap toevoegen** onderaan in het deelvenster met aangepaste hive-site.

    d. Voer in het venster Eigenschappen toevoegen `mapred.map.output.compression.codec` als de sleutel en `org.apache.hadoop.io.compress.SnappyCodec` als de waarde.

    e. Klik op **Add**.

    ![De aangepaste eigenschap hive](./media/hdinsight-changing-configs-via-ambari/hive-custom-property.png)

    Hiermee wordt het tijdelijke bestand met treffende compressie comprimeren. Wanneer de eigenschap is toegevoegd, wordt deze weergegeven in het deelvenster met aangepaste hive-site.

    > [!NOTE]
    > Deze procedure wijzigt de `$HADOOP_HOME/conf/hive-site.xml` bestand.

### <a name="compress-final-output"></a>Uiteindelijke uitvoer comprimeren

De uiteindelijke Hive-uitvoer kan ook worden gecomprimeerd.

1. Voor het comprimeren van de uiteindelijke Hive-uitvoer, gaat u naar de component **Configs** tabblad en stel vervolgens de `hive.exec.compress.output` parameter in op true. De standaardwaarde is ingesteld op false.

2. Als u de compressiecodec uitvoer, voeg de `mapred.output.compression.codec` aangepaste eigenschap aan het aangepaste hive-site deelvenster, zoals beschreven in de vorige sectie stap 3.

    ![De aangepaste eigenschap hive](./media/hdinsight-changing-configs-via-ambari/hive-custom-property2.png)

### <a name="enable-speculative-execution"></a>Speculatieve uitvoering inschakelen

Speculatieve uitvoering wordt gestart van een bepaald aantal dubbele taken om te detecteren en afgekeurde beheer taak trage uitgevoerd bij het verbeteren van de algehele taak wordt uitgevoerd door een afzonderlijke taak resultaten te optimaliseren.

Speculatieve uitvoering mag niet worden ingeschakeld voor langlopende MapReduce-taken met een grote hoeveelheid invoer.

* Om in te schakelen speculatieve uitvoering, gaat u naar de component **Configs** tabblad en stel vervolgens de `hive.mapred.reduce.tasks.speculative.execution` parameter in op true. De standaardwaarde is ingesteld op false.

    ![Hive mapred verminderen speculatieve uitvoering van taken](./media/hdinsight-changing-configs-via-ambari/hive-mapred-reduce-tasks-speculative-execution.png)

### <a name="tune-dynamic-partitions"></a>Dynamische partities afstemmen

Hive kunt u dynamische partities maken bij het invoegen van records in een tabel, zonder elke partitie vooraf te definiëren. Dit is een krachtige functie, hoewel dit tot het maken van een groot aantal partities en een groot aantal bestanden voor elke partitie leiden kan.

1. Voor Hive dynamische partities doen de `hive.exec.dynamic.partition` parameterwaarde moet true (standaard).

2. Wijzig de modus dynamische partitie *strikte*. In de strikte modus moet ten minste één partitie niet statisch zijn. Hiermee voorkomt u dat query's zonder dat het filter partitie in de WHERE-component, dat wil zeggen, *strikte* voorkomt dat query's waarmee alle partities worden gescand. Navigeer naar de component **Configs** tabblad en stel vervolgens `hive.exec.dynamic.partition.mode` naar **strikte**. De standaardwaarde is **nonstrict**.
 
3. Beperk het aantal dynamische partities worden gemaakt, wijzigt u de '' hive.exec.max.dynamic.partitions-parameter. De standaardwaarde is 5.000.
 
4. Alleen het totaal aantal dynamische partities per knooppunt wijzigen `hive.exec.max.dynamic.partitions.pernode`. De standaardwaarde is 2000.

### <a name="enable-local-mode"></a>Lokale modus inschakelen

Lokale modus kunnen Hive alle taken van een taak uitvoeren op een enkele computer of soms in een enkel proces. Dit verbetert de prestaties van query's als de invoergegevens klein is en de overhead van de lancering van taken voor query's van een aanzienlijk deel van de algehele queryuitvoering verbruikt.

Om in te schakelen in lokale modus, voeg de `hive.exec.mode.local.auto` -parameter voor aangepaste hive-site Configuratiescherm, zoals wordt beschreven in stap 3 van de [tussenliggende compressie inschakelen](#enable-intermediate-compression) sectie.

![Hive exec modus lokale automatisch](./media/hdinsight-changing-configs-via-ambari/hive-exec-mode-local-auto.png)

### <a name="set-single-mapreduce-multigroup-by"></a>Stel één MapReduce MultiGROUP door

Als deze eigenschap is ingesteld op true, wordt een query MultiGROUP door met de algemene groep door sleutels genereert een enkele MapReduce-taak.  

Om dit gedrag, voeg de `hive.multigroupby.singlereducer` -parameter voor aangepaste hive-site deelvenster, zoals wordt beschreven in stap 3 van de [tussenliggende compressie inschakelen](#enable-intermediate-compression) sectie.

![Hive ingesteld één MapReduce MultiGROUP door](./media/hdinsight-changing-configs-via-ambari/hive-multigroupby-singlereducer.png)

### <a name="additional-hive-optimizations"></a>Aanvullende optimaliseringen van Hive

De volgende secties beschrijven aanvullende Hive-gerelateerde op die u kunt instellen.

#### <a name="join-optimizations"></a>Optimalisaties koppelen

Het standaardjointype in de component is een *willekeurige volgorde join*. In component, speciale mappers Lees de invoer- en verzenden van een join sleutel-waardepaar naar een tussenliggende-bestand. Hadoop worden gesorteerd en worden deze paren samengevoegd in een willekeurige volgorde-fase. Deze fase willekeurige volgorde is kostbaar. De juiste join op basis van uw gegevens te selecteren, kan de prestaties aanzienlijk verbeteren.

| Jointype | Wanneer | Hoe | Hive-instellingen | Opmerkingen |
| -- | -- | -- | -- | -- |
| Willekeurige volgorde Join | <ul><li>Standaardkeuze</li><li>Altijd werkt</li></ul> | <ul><li>Leest uit deel uit van een van de tabellen</li><li>Buckets en gesorteerd op Join-sleutel</li><li>Hiermee verzendt u één bucket naar elke verminderen</li><li>Join wordt gedaan aan de kant verminderen</li></ul> | Er is geen aanzienlijke Hive benodigde instellen | Elke keer werkt |
| Lid worden toegewezen | <ul><li>Een tabel past in het geheugen</li></ul> | <ul><li>Kleine tabel leest in geheugen hash-tabel</li><li>Streams via deel van de grote bestanden</li><li>Lid wordt van elke record uit de hash-tabel</li><li>Joins zijn door de alleen-toewijzing</li></ul> | `hive.auto.confvert.join=true` | Zeer snel, maar beperkt |
| Sorteren samenvoegen Bucket | Als beide tabellen zijn: <ul><li>Dezelfde gesorteerd</li><li>Dezelfde bucketed</li><li>Lid worden van de kolom gesorteerd/gerangschikte</li></ul> | Elk proces: <ul><li>Een bucket leest uit elke tabel</li><li>De rij met de laagste waarde verwerkt</li></ul> | `hive.auto.convert.sortmerge.join=true` | Zeer efficiënte |

#### <a name="execution-engine-optimizations"></a>Engine-optimalisaties uitvoeren

Extra aanbevelingen voor het optimaliseren van de engine voor het uitvoeren van Hive:

| Instelling | Aanbevolen | HDInsight-standaard |
| -- | -- | -- |
| `hive.mapjoin.hybridgrace.hashtable` | = True veiliger, tragere; False = sneller | onwaar |
| `tez.am.resource.memory.mb` | Bovengrens van 4 GB voor de meeste | Automatisch zijn afgestemd |
| `tez.session.am.dag.submit.timeout.secs` | 300+ | 300 |
| `tez.am.container.idle.release-timeout-min.millis` | 20000+ | 10.000 |
| `tez.am.container.idle.release-timeout-max.millis` | 40000+ | 20000 |

## <a name="pig-optimization"></a>Pig-optimalisatie

Pig-eigenschappen kunnen worden gewijzigd vanuit de Ambari-webgebruikersinterface om af te stemmen Pig-query's. Pig-eigenschappen van Ambari rechtstreeks wijzigen, wijzigt de Pig-eigenschappen in de `/etc/pig/2.4.2.0-258.0/pig.properties` bestand.

1. Pig om eigenschappen te wijzigen, gaat u naar de Pig **Configs** tabblad uit en vouw vervolgens de **geavanceerde eigenschappen van pig** deelvenster.

2. Zoeken, verwijder de opmerkingen en wijzig de waarde van de eigenschap die u wilt wijzigen.

3. Selecteer **opslaan** op de rechtsboven in het venster voor het opslaan van de nieuwe waarde. Bepaalde eigenschappen moet mogelijk een herstart van de service.

    ![Geavanceerde pig-eigenschappen](./media/hdinsight-changing-configs-via-ambari/advanced-pig-properties.png)
 
> [!NOTE]
> Sessie-niveau instellingen overschrijven eigenschapswaarden in de `pig.properties` bestand.

### <a name="tune-execution-engine"></a>Engine voor het uitvoeren van afstemmen

Twee motoren voor uitvoering beschikbaar zijn voor Pig-scripts uitvoeren: MapReduce en Tez. Tez is een geoptimaliseerde-engine en is veel sneller dan MapReduce.

1. De engine voor het uitvoeren, wijzigen de **geavanceerde eigenschappen van pig** deelvenster vinden van de eigenschap `exectype`.

2. De standaardwaarde is **MapReduce**. Wijzig dit in **Tez**.


### <a name="enable-local-mode"></a>Lokale modus inschakelen

Vergelijkbaar met Hive, lokale modus wordt gebruikt om taken met relatief kleinere hoeveelheden gegevens sneller te laten.

1. Instellen zodat de lokale modus `pig.auto.local.enabled` naar **true**. De standaardwaarde is ingesteld op false.

2. Taken met een grootte van de invoergegevens kleiner zijn dan de `pig.auto.local.input.maxbytes` eigenschapswaarde worden beschouwd als kleine taken. De standaardwaarde is 1 GB.


### <a name="copy-user-jar-cache"></a>Jar gebruikerscache kopiëren

Pig kopieert de JAR-bestanden die vereist zijn door UDF's naar een gedistribueerde cache zodat ze beschikbaar is voor knooppunten van de taak. Deze potten wijzigen niet vaak. Indien ingeschakeld, de `pig.user.cache.enabled` instelling kan potten worden geplaatst in een cachegeheugen om te blijven gebruiken voor taken worden uitgevoerd door dezelfde gebruiker. Dit resulteert in een kleine toename van de prestaties van de taak.

1. Om in te schakelen, stelt `pig.user.cache.enabled` op true. De standaardwaarde is ONWAAR.

2. Om in te stellen het basispad van de cache potten `pig.user.cache.location` aan het basispad. De standaardwaarde is `/tmp`.


### <a name="optimize-performance-with-memory-settings"></a>Optimaliseren met instellingen voor het geheugen

De volgende geheugeninstellingen kunt Pig-script optimaliseren.

* `pig.cachedbag.memusage`: De hoeveelheid geheugen toegewezen aan een eigenschappenverzameling. Een eigenschappenverzameling is een verzameling van tuples. Een tuple is een geordende reeks velden en een veld is een stukje informatie. Als de gegevens in een eigenschappenverzameling buiten het toegewezen geheugen valt, is het een zich verspreid naar schijf. De standaardwaarde is 0,2 waarmee 20 procent van het beschikbare geheugen wordt aangeduid. Dit geheugen wordt gedeeld door alle zakken in een toepassing.

* `pig.spill.size.threshold`: Zakken groter is dan deze drempelwaarde grootte worden gelekt (in bytes) zich worden verspreid naar schijf. De standaardwaarde is 5 MB.


### <a name="compress-temporary-files"></a>Comprimeren van tijdelijke bestanden

Pig genereert tijdelijke bestanden tijdens het uitvoeren van de taak. De tijdelijke bestanden te comprimeren resulteert in een prestatieverhoging van de bij het lezen of schrijven van bestanden naar de schijf. De volgende instellingen kunnen worden gebruikt voor het comprimeren van tijdelijke bestanden.

* `pig.tmpfilecompression`: Indien true, kunt tijdelijk bestandscompressie. De standaardwaarde is ingesteld op false.

* `pig.tmpfilecompression.codec`: De compressiecodec moet worden gebruikt voor de tijdelijke bestanden te comprimeren. De aanbevolen compressiecodecs zijn LZO en Snappy lager CPU-gebruik.

### <a name="enable-split-combining"></a>Inschakelen van gesplitste combineren

Wanneer dit is ingeschakeld, worden kleine bestanden gecombineerd voor minder kaart taken. Dit verbetert de efficiëntie van taken met een aantal kleine bestanden. Om in te schakelen, stelt `pig.noSplitCombination` op true. De standaardwaarde is ingesteld op false.


### <a name="tune-mappers"></a>Mappers afstemmen

Het aantal mappers wordt beheerd door het wijzigen van de eigenschap `pig.maxCombinedSplitSize`. Hiermee geeft u de grootte van de gegevens worden verwerkt door een taak één kaart. De standaardwaarde is Standaardblokgrootte van het bestandssysteem. Deze waarde resulteert in een vermindering van het aantal taken toewijzen te vergroten.


### <a name="tune-reducers"></a>Verkleiningstoestellen afstemmen

Het aantal verkleiningstoestellen wordt berekend op basis van de parameter `pig.exec.reducers.bytes.per.reducer`. De parameter geeft u het aantal bytes dat per reducer wordt verwerkt, standaard 1 GB. Om te beperken het maximum aantal verkleiningstoestellen, stel de `pig.exec.reducers.max` eigenschap standaard 999.


## <a name="hbase-optimization-with-the-ambari-web-ui"></a>Optimalisatie van HBase met de Ambari-webgebruikersinterface

HBase-configuratie is gewijzigd van de **HBase Configs** tabblad. De volgende secties worden enkele belangrijke configuratie-instellingen die invloed hebben op prestaties HBase beschreven.

### <a name="set-hbaseheapsize"></a>Set HBASE_HEAPSIZE

De HBase-heap-grootte geeft de maximale heap moet worden gebruikt in megabytes door *regio* en *master* servers. De standaardwaarde is 1000 MB. Dit moet worden geconfigureerd voor de clusterwerkbelasting worden opgeslagen.

1. Als u wilt wijzigen, gaat u naar de **geavanceerde HBase-env** deelvenster in de HBase **Configs** tabblad en gaat u naar de `HBASE_HEAPSIZE` instelling.

2. De standaardwaarde op 5000 MB wijzigen.

    ![HBASE_HEAPSIZE](./media/hdinsight-changing-configs-via-ambari/hbase-heapsize.png)


### <a name="optimize-read-heavy-workloads"></a>Lees zware workloads optimaliseren

De volgende configuraties zijn belangrijk voor het verbeteren van de prestaties van lezen zware werkbelastingen.

#### <a name="block-cache-size"></a>Cache-blokgrootte

De blok-cache is de leescache. De grootte wordt bepaald door de `hfile.block.cache.size` parameter. De standaardwaarde is 0,4 40 procent van het geheugen van de server totale regio. Hoe groter de cache blokgrootte, hoe sneller willekeurig lezen is.

1. Deze als parameter wilt wijzigen, gaat u naar de **instellingen** tabblad in de HBase **Configs** tabblad en ga vervolgens naar **% van de RegionServer toegewezen aan lezen Buffers**.

    ![HBase cache blokgrootte](./media/hdinsight-changing-configs-via-ambari/hbase-block-cache-size.png)
 
2. De waarde wilt wijzigen, selecteert u de **bewerken** pictogram.


#### <a name="memstore-size"></a>De grootte van de geheugenopslag

Alle wijzigingen worden opgeslagen in de geheugenbuffer aangeroepen een *geheugenopslag*. Dit verhoogt de totale hoeveelheid gegevens die kunnen worden geschreven naar de schijf in één bewerking, en deze versnelt daaropvolgende toegang voor recente wijzigingen. De grootte van de geheugenopslag is gedefinieerd door de volgende twee parameters:

* `hbase.regionserver.global.memstore.UpperLimit`: Hiermee definieert u het maximale percentage van de regio-server die geheugenopslag gecombineerd kunt gebruiken.

* `hbase.regionserver.global.memstore.LowerLimit`: Hiermee definieert u het minimum percentage van de regio-server die geheugenopslag gecombineerd kunt gebruiken.

Om te optimaliseren voor willekeurige leesbewerkingen, kunt u de boven en ondergrenzen van de geheugenopslag verminderen.


#### <a name="number-of-rows-fetched-when-scanning-from-disk"></a>Aantal rijen bij het scannen van de schijf opgehaald

De `hbase.client.scanner.caching` instelling definieert het aantal rijen dat is gelezen uit schijf wanneer de `next` methode wordt aangeroepen op een scanner.  De standaardwaarde is 100. Hoe hoger het getal, minder externe oproepen gemaakt van de client naar de server regio, wat leidt tot sneller scans. Dit wordt echter ook geheugendruk op de client verhogen.

![Het aantal rijen als resultaat HBase](./media/hdinsight-changing-configs-via-ambari/hbase-num-rows-fetched.png)

> [!IMPORTANT]
> Stel de waarde niet zo dat de tijd tussen het aanroepen van de volgende methode op een scanner groter dan de time-out van de scanner is. De scanner time-outduur wordt gedefinieerd door de `hbase.regionserver.lease.period` eigenschap.


### <a name="optimize-write-heavy-workloads"></a>Schrijven zware workloads optimaliseren

De volgende configuraties zijn belangrijk voor het verbeteren van de prestaties van schrijven zware werkbelastingen.


#### <a name="maximum-region-file-size"></a>Regio maximale bestandsgrootte

HBase-gegevens opslaat in een interne bestandsindeling, aangeroepen *HFile*. De eigenschap `hbase.hregion.max.filesize` definieert de grootte van een enkele HFile voor een regio.  Een gebied wordt gesplitst in twee gebieden als de som van alle HFiles in een regio groter dan deze instelling is.
 
![Maximale bestandsgrootte van HBase HRegion](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-max-filesize.png)

Hoe groter de regio bestand, hoe kleiner het aantal splitsingen. U kunt de bestandsgrootte om te bepalen van een waarde resulteert in het maximum schrijfprestaties verhogen.


#### <a name="avoid-update-blocking"></a>Vermijd update blokkeren

* De eigenschap `hbase.hregion.memstore.flush.size` definieert de grootte waarmee geheugenopslag worden leeggemaakt op schijf. De standaardgrootte is 128 MB.

* De vermenigvuldigingsfactor Hbase regio blok wordt gedefinieerd door `hbase.hregion.memstore.block.multiplier`. De standaardwaarde is 4. Het toegestane maximum is 8.

* HBase updates als de geheugenopslag is geblokkeerd (`hbase.hregion.memstore.flush.size` * `hbase.hregion.memstore.block.multiplier`) bytes.

    Met de standaardwaarden van de grootte van het leegmaken en blok vermenigvuldiger worden de updates worden geblokkeerd wanneer de geheugenopslag is 128 * 4 = 512 MB zijn. Als u de update aantal blokkeren, verhoogt u de waarde van `hbase.hregion.memstore.block.multiplier`.

![HBase regio blok vermenigvuldiger](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-memstore-block-multiplier.png)


### <a name="define-memstore-size"></a>Grootte van de geheugenopslag definiëren

De grootte van de geheugenopslag is gedefinieerd door de `hbase.regionserver.global.memstore.UpperLimit` en `hbase.regionserver.global.memstore.LowerLimit` parameters. Instellen van deze waarden gelijk die op elke andere onderbroken tijdens vermindert schrijft (ook waardoor vaker domeincontrollerlocaties leeg) en leidt tot verbeterde schrijfprestaties.


### <a name="set-memstore-local-allocation-buffer"></a>Instellen van de geheugenopslag lokale toewijzing buffer

Gebruik van de buffer geheugenopslag lokale toewijzing wordt bepaald door de eigenschap `hbase.hregion.memstore.mslab.enabled`. Wanneer dit is ingeschakeld (true) zo voorkomt u dat heapfragmentatie tijdens zware schrijfbewerking. De standaardwaarde is true.
 
![hbase.hregion.memstore.mslab.enabled](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-memstore-mslab-enabled.png)


## <a name="next-steps"></a>Volgende stappen

* [HDInsight-clusters met de Ambari-webgebruikersinterface beheren](hdinsight-hadoop-manage-ambari.md)
* [Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)
