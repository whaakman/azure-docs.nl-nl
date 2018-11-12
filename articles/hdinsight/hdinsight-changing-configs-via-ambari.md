---
title: Configuraties van clusters met Apache Ambari - Azure HDInsight optimaliseren
description: Gebruik Apache Ambari-Webinterface voor het configureren en optimaliseren van HDInsight-clusters.
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: ashish
ms.openlocfilehash: 82995f2cc8facac9bef6f8c84c9667775ac81463
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51038515"
---
# <a name="use-ambari-to-optimize-hdinsight-cluster-configurations"></a>Ambari gebruiken om de configuraties van HDInsight-clusters optimaliseren

HDInsight bevat Apache Hadoop-clusters voor grootschalige gegevensverwerking toepassingen. Beheer, controle en optimaliseren van deze complexe clusters met meerdere knooppunten kunnen lastig zijn. [Apache Ambari](http://ambari.apache.org/) is een webinterface voor het beheren en controleren van HDInsight Linux-clusters.  Voor Windows-clusters, gebruikt u de Ambari [REST-API](hdinsight-hadoop-manage-ambari-rest-api.md).

Zie voor een inleiding tot het gebruik van de Ambari-Webgebruikersinterface, [beheren HDInsight-clusters met behulp van de Ambari-Webinterface](hdinsight-hadoop-manage-ambari.md)

Meld u aan bij de Ambari op `https://CLUSTERNAME.azurehdidnsight.net` met de clusterreferenties van uw. Het eerste scherm bevat een dashboard met een.

![Ambari-dashboard](./media/hdinsight-changing-configs-via-ambari/ambari-dashboard.png)

De Ambari-Webgebruikersinterface kan worden gebruikt voor het beheren van hosts, services, waarschuwingen, configuraties en weergaven. Ambari kan niet worden gebruikt om een HDInsight-cluster maken, bijwerken van services, stacks en versies beheren, buiten gebruik stellen of recommission hosts of services toevoegen aan het cluster.

## <a name="manage-your-clusters-configuration"></a>Configuratie van uw cluster beheren

Configuratie-instellingen kunnen een bepaalde service af te stemmen. Als u wilt wijzigen van een service-configuratie-instellingen, selecteert u de service van de **Services** zijbalk (aan de linkerkant), en navigeer vervolgens naar de **configuraties** tabblad in de detailpagina van de service.

![Zijbalk voor services](./media/hdinsight-changing-configs-via-ambari/services-sidebar.png)

### <a name="modify-namenode-java-heap-size"></a>NameNode Java heap-grootte wijzigen

De NameNode Java heap-grootte is afhankelijk van veel factoren zoals de belasting van het cluster, het aantal bestanden en de nummers van blokken. De standaardgrootte van 1 GB werkt goed samen met de meeste clusters, hoewel sommige werkbelastingen kunnen meer of minder geheugen vereisen. 

De NameNode Java heap-grootte wijzigen:

1. Selecteer **HDFS** in de zijbalk Services en navigeer naar de **Peeringconfiguraties** tabblad.

    ![HDFS-configuratie](./media/hdinsight-changing-configs-via-ambari/hdfs-config.png)

1. Ga naar de instelling **NameNode Java heap-grootte**. U kunt ook de **filter** in het tekstvak te typen en een bepaalde instelling te vinden. Selecteer de **pen** pictogram naast de naam van de instelling.

    ![NameNode Java heap-grootte](./media/hdinsight-changing-configs-via-ambari/java-heap-size.png)

1. Typ de nieuwe waarde in het tekstvak in en druk vervolgens op **Enter** de wijziging op te slaan.

    ![NameNode Java heap-grootte bewerken](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edit.png)

1. De NameNode Java heap-grootte wordt gewijzigd naar 2 GB van 1 GB.

    ![Bewerkt NameNode Java heap-grootte](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edited.png)

1. Sla de wijzigingen door te klikken op de groene **opslaan** knop boven aan het Configuratiescherm.

    ![Wijzigingen opslaan](./media/hdinsight-changing-configs-via-ambari/save-changes.png)

## <a name="hive-optimization"></a>Hive-optimalisatie

De volgende secties beschrijven de configuratieopties voor het optimaliseren van de algehele prestaties van Hive.

1. Voor het wijzigen van Hive-configuratieparameters, selecteer **Hive** in de zijbalk Services.
1. Navigeer naar de **Peeringconfiguraties** tabblad.

### <a name="set-the-hive-execution-engine"></a>De engine voor het uitvoeren van Hive instellen

Hive biedt twee uitvoering motoren: MapReduce en Tez. Tez is veel sneller dan MapReduce. HDInsight Linux-clusters hebben Tez als de standaard-uitvoeringsengine. Wijzigen van de engine voor het uitvoeren:

1. In de component **Peeringconfiguraties** tabblad, typt u **-uitvoeringsengine** in het filtervak.

    ![Engine voor het uitvoeren van Search](./media/hdinsight-changing-configs-via-ambari/search-execution.png)

1. De **optimalisatie** is de standaardwaarde van de eigenschap **Tez**.

    ![Optimalisatie - Tez](./media/hdinsight-changing-configs-via-ambari/optimization-tez.png)

### <a name="tune-mappers"></a>Mappers afstemmen

Hadoop probeert te splitsen (*kaart*) één bestand in meerdere bestanden en de resulterende proces parallel-bestanden. Het aantal mappers is afhankelijk van het aantal splitsingen. De volgende twee configuratieparameters station het aantal splitsingen voor de Tez-uitvoeringsengine:

* `tez.grouping.min-size`: Lagere limiet voor de grootte van een gegroepeerde splitsing, met een standaardwaarde van 16 MB (16,777,216 bytes).
* `tez.grouping.max-size`: De bovenste limiet voor de grootte van een gegroepeerde splitsing, met een standaardwaarde van 1 GB (1,073,741,824 bytes).

Als een vuistregel prestaties afnemen beide van deze parameters latentie verbeteren, te verhogen voor hogere doorvoer.

Bijvoorbeeld, om in te stellen vier mapper-taken voor een gegevensgrootte van 128 MB, stelt u beide parameters op 32 MB elk (33,554,432 bytes).

1. Als u wilt de parameters van de limiet wijzigen, gaat u naar de **Peeringconfiguraties** tabblad van de Tez-service. Vouw de **algemene** deelvenster en zoek de `tez.grouping.max-size` en `tez.grouping.min-size` parameters.

1. Beide parameters ingesteld op **33,554,432** bytes (32 MB).

    ![Tez groepering grootten](./media/hdinsight-changing-configs-via-ambari/tez-grouping-size.png)
 
Deze wijzigingen hebben invloed op alle Tez-taken op de server. Als u een optimale resultaten, kies juiste parameterwaarden.

### <a name="tune-reducers"></a>Reducers tegelijkertijd afstemmen

ORC en Snappy bieden hoge prestaties. Hive hebben echter te weinig reducers tegelijkertijd standaard, waardoor knelpunten.

Stel dat u hebt een grootte van de invoergegevens van 50 GB. Gegevens in ORC-indeling met Snappy compressie is 1 GB. Hive maakt een schatting van het aantal benodigde reducers tegelijkertijd: (aantal bytes invoer voor mappers / `hive.exec.reducers.bytes.per.reducer`).

In dit voorbeeld wordt met de standaardinstellingen 4 reducers tegelijkertijd.

De `hive.exec.reducers.bytes.per.reducer` parameter geeft u het aantal bytes dat per reducer wordt verwerkt. De standaardwaarde is 64 MB. Deze waarde afstemmen omlaag verhoogt de parallelle uitvoering en de prestaties mogelijk verbeterd. Afstemmen te laag kan ook resulteren in te veel reducers tegelijkertijd, mogelijk negatieve die betrekking hebben op prestaties. Deze parameter is gebaseerd op uw specifieke vereisten, compressie-instellingen en andere omgevingsfactoren.

1. Als u wilt wijzigen met de parameter, gaat u naar de component **Peeringconfiguraties** tabblad en zoek de **gegevens per Reducer** parameter op de pagina instellingen.

    ![Gegevens per Reducer](./media/hdinsight-changing-configs-via-ambari/data-per-reducer.png)
 
1. Selecteer **bewerken** wijzigen van de waarde van 128 MB (134,217,728 bytes) en druk vervolgens op **Enter** om op te slaan.

    ![Gegevens per Reducer - bewerkt](./media/hdinsight-changing-configs-via-ambari/data-per-reducer-edited.png)
  
    Gegeven een invoergrootte van 1024 MB, met 128 MB aan gegevens per reducer, er 8 reducers tegelijkertijd zijn (1024/128).

1. Een onjuiste waarde voor de **gegevens per Reducer** parameter kan leiden tot een groot aantal reducers tegelijkertijd, nadelige invloed heeft op de prestaties van query's. Instellen om te beperken het maximum aantal reducers tegelijkertijd, `hive.exec.reducers.max` naar een geschikte waarde. De standaardwaarde is 1009.

### <a name="enable-parallel-execution"></a>Inschakelen van parallelle uitvoering

Een Hive-query wordt uitgevoerd in een of meer fasen. Als de onafhankelijke fasen kunnen parallel worden uitgevoerd, wordt die prestaties van query's vergroten.

1.  Als u wilt uitvoeren van parallelle query's inschakelen, gaat u naar de component **Config** tabblad en zoek de `hive.exec.parallel` eigenschap. De standaardwaarde is false. Wijzig de waarde true en druk vervolgens op **Enter** om op te slaan de waarde.
 
1.  Als u wilt beperken het aantal taken parallel worden uitgevoerd, wijzigt de `hive.exec.parallel.thread.number` eigenschap. De standaardwaarde is 8.

    ![Exec parallelle hive](./media/hdinsight-changing-configs-via-ambari/hive-exec-parallel.png)


### <a name="enable-vectorization"></a>Schakel vectorisatie in

Hive verwerkt gegevens per rij. Vectorisatie in stuurt Hive om gegevens te verwerken in blokken van 1024 rijen in plaats van één rij tegelijk. Vectorisatie in is alleen van toepassing op het ORC-bestandsindeling.

1. Als u wilt een vectorized queryuitvoering inschakelen, gaat u naar de component **Peeringconfiguraties** tabblad en zoek de `hive.vectorized.execution.enabled` parameter. De standaardwaarde is true voor Hive 0.13.0 of hoger.
 
1. Om in te schakelen vectorized voor het uitvoeren van de kant van het verminderen van de query, stel de `hive.vectorized.execution.reduce.enabled` parameter op waar. De standaardwaarde is false.

    ![Het uitvoeren van hive vectorized](./media/hdinsight-changing-configs-via-ambari/hive-vectorized-execution.png)

### <a name="enable-cost-based-optimization-cbo"></a>Kosten op basis van optimalisatie (CBO) inschakelen

Hive volgt standaard een set regels om te vinden van een optimale queryplan voor uitvoering. Kosten op basis van optimalisatie (CBO) meerdere abonnementen voor het uitvoeren van een query wordt geëvalueerd en kosten toegewezen aan elk abonnement en vervolgens bepaalt de goedkoopste plan een query uit te voeren.

Om in te schakelen CBO, gaat u naar de component **Peeringconfiguraties** tabblad en zoek naar de `parameter hive.cbo.enable`, schakelt u de wisselknop voor **op**.

![CBO config](./media/hdinsight-changing-configs-via-ambari/cbo.png)

De volgende aanvullende configuratieparameters verhogen Hive-queryprestaties als CBO is ingeschakeld:

* `hive.compute.query.using.stats`

    Wanneer is ingesteld op true, Hive statistieken die zijn opgeslagen in de metastore gebruikt om te beantwoorden van eenvoudige query's zoals `count(*)`.

    ![CBO statistieken](./media/hdinsight-changing-configs-via-ambari/hive-compute-query-using-stats.png)

* `hive.stats.fetch.column.stats`

    Kolomstatistieken worden gemaakt wanneer CBO is ingeschakeld. Kolomstatistieken, die zijn opgeslagen in metastore, het optimaliseren van query's maakt gebruik van hive. Ophalen van de kolomstatistieken voor elke kolom duurt langer als het aantal kolommen hoog is. Deze instelling is ingesteld op false, schakelt ophalen kolomstatistieken met de metastore.

    ![Hive-statistieken set kolomstatistieken](./media/hdinsight-changing-configs-via-ambari/hive-stats-fetch-column-stats.png)

* `hive.stats.fetch.partition.stats`

    Standaard-partitie-statistieken, zoals het aantal rijen, de gegevensgrootte en de grootte worden opgeslagen in metastore. Wanneer ingesteld op true, de partitie statistieken van metastore worden opgehaald. Als deze eigenschap ONWAAR is, de grootte van het bestandssysteem wordt opgehaald en het aantal rijen wordt opgehaald uit het schema van de rij.

    ![Hive-statistieken set partitie statistieken](./media/hdinsight-changing-configs-via-ambari/hive-stats-fetch-partition-stats.png)

### <a name="enable-intermediate-compression"></a>Tussenliggende compressie inschakelen

Kaart taken maken tussenliggende bestanden die worden gebruikt door de taken reducer. Tussenliggende compressie Hiermee verkleint u de grootte van het tussenliggende.

Hadoop-taken zijn meestal i/o knelpunt. Het comprimeren van gegevens sneller kan i/o en algemene netwerkoverdracht.

De beschikbare compressietypen zijn:

| Indeling | Hulpprogramma | Algoritme | Bestandsextensie | Splitsbare? |
| -- | -- | -- | -- | -- |
| Gzip | Gzip | VERKLEINEN | .gz | Nee |
| Bzip2 | Bzip2 | Bzip2 |.bz2 | Ja |
| LZO | Lzop | LZO | .lzo | Ja, als geïndexeerd |
| Snappy | N/A | Snappy | Snappy | Nee |

Als een regel met de compressiemethode splitsbare is belangrijk, anders heel weinig mappers wordt gemaakt. Als de invoergegevens tekst, `bzip2` is de beste optie. Snappy is de snelste compressieoptie voor ORC-indeling.

1. Om in te schakelen tussenliggende compressie, gaat u naar de component **Peeringconfiguraties** tabblad en stel vervolgens de `hive.exec.compress.intermediate` parameter op waar. De standaardwaarde is false.

    ![Hive exec comprimeren tussenliggende](./media/hdinsight-changing-configs-via-ambari/hive-exec-compress-intermediate.png)

    > [!NOTE]
    > Voor het comprimeren van tussenliggende bestanden, kiest u een compressiecodec met lagere CPU kosten, zelfs als de codec beschikt niet over een hoge mate van compressie-uitvoer.

1. De aangepaste eigenschap toevoegen om in te stellen de tussenliggende compressiecodec, `mapred.map.output.compression.codec` naar de `hive-site.xml` of `mapred-site.xml` bestand.

1. Een aangepaste instelling toevoegen:

    a. Navigeer naar de component **Peeringconfiguraties** tabblad en selecteer de **Geavanceerd** tabblad.

    b. Onder de **Geavanceerd** tabblad, zoeken en vouw de **aangepaste hive-site** deelvenster.

    c. Klik op de koppeling **eigenschap toevoegen** onderaan in het deelvenster met aangepaste hive-site.

    d. Voer in het venster eigenschap toevoegen `mapred.map.output.compression.codec` als de sleutel en `org.apache.hadoop.io.compress.SnappyCodec` als de waarde.

    e. Klik op **Add**.

    ![De aangepaste eigenschap hive](./media/hdinsight-changing-configs-via-ambari/hive-custom-property.png)

    Hiermee wordt het tijdelijke bestand met behulp van Snappy compressie comprimeren. Zodra de eigenschap is toegevoegd, wordt deze weergegeven in het deelvenster met aangepaste hive-site.

    > [!NOTE]
    > Hiermee wijzigt u deze procedure de `$HADOOP_HOME/conf/hive-site.xml` bestand.

### <a name="compress-final-output"></a>Comprimeren van de uiteindelijke uitvoer

De uiteindelijke uitvoer van Hive kan ook worden gecomprimeerd.

1. Voor het comprimeren van de uiteindelijke uitvoer van Hive, gaat u naar de component **Peeringconfiguraties** tabblad en stel vervolgens de `hive.exec.compress.output` parameter op waar. De standaardwaarde is false.

1. Als u de compressiecodec uitvoer, voeg de `mapred.output.compression.codec` aangepaste eigenschap aan het aangepaste hive-site deelvenster, zoals beschreven in de vorige sectie stap 3.

    ![De aangepaste eigenschap hive](./media/hdinsight-changing-configs-via-ambari/hive-custom-property2.png)

### <a name="enable-speculative-execution"></a>Speculatieve uitvoering inschakelen

Speculatieve uitvoering wordt gestart voor een bepaald aantal dubbele taken om te detecteren en het vastleggen van de taak trage zwarte bij het verbeteren van de algehele taak kan worden uitgevoerd door een afzonderlijke Taakresultaten te optimaliseren.

Speculatieve uitvoering mag niet worden ingeschakeld voor langlopende MapReduce-taken met grote hoeveelheden invoer.

* Om in te schakelen speculatieve uitvoering, gaat u naar de component **Peeringconfiguraties** tabblad en stel vervolgens de `hive.mapred.reduce.tasks.speculative.execution` parameter op waar. De standaardwaarde is false.

    ![Hive mapred taken speculatieve uitvoering beperken](./media/hdinsight-changing-configs-via-ambari/hive-mapred-reduce-tasks-speculative-execution.png)

### <a name="tune-dynamic-partitions"></a>Dynamische partities afstemmen

Hive kunt u dynamische partities maken bij het invoegen van records in een tabel, zonder vooraf elke partitie te definiëren. Dit is een krachtige functie, maar dit tot het maken van een groot aantal partities en een groot aantal bestanden voor elke partitie leiden kan.

1. Voor Hive dynamische partities, doet de `hive.exec.dynamic.partition` parameterwaarde moet ' True ' (de standaardinstelling).

1. De partitiemodus dynamische te wijzigen *strikte*. In de strikte modus moet ten minste één partitie niet statisch zijn. Dit voorkomt dat query's zonder dat het filter partitie in de WHERE-component, dat wil zeggen, *strikte* voorkomt dat query's die scannen van alle partities. Navigeer naar de component **configuraties** tabblad en stel vervolgens `hive.exec.dynamic.partition.mode` naar **strikte**. De standaardwaarde is **nonstrict**.
 
1. Als u wilt beperken het aantal dynamische partities wilt maken, wijzigen de `hive.exec.max.dynamic.partitions` parameter. De standaardwaarde is 5000.
 
1. Als u wilt beperken het totale aantal dynamische partities per knooppunt, wijzigen `hive.exec.max.dynamic.partitions.pernode`. De standaardwaarde is 2000.

### <a name="enable-local-mode"></a>De lokale modus inschakelen

Lokale-modus kunnen Hive voor alle taken van een taak op één computer, of soms in een enkel proces. Dit verbetert de prestaties van query's als de ingevoerde gegevens klein is en de overhead van het starten van taken voor query's maakt gebruik van een aanzienlijk deel van de algemene queryuitvoering.

Lokale modus wilt activeren, voeg de `hive.exec.mode.local.auto` parameter aan de aangepaste hive-site Configuratiescherm, zoals wordt beschreven in stap 3 van de [tussenliggende compressie inschakelen](#enable-intermediate-compression) sectie.

![Hive exec modus lokale automatisch](./media/hdinsight-changing-configs-via-ambari/hive-exec-mode-local-auto.png)

### <a name="set-single-mapreduce-multigroup-by"></a>Set één MapReduce MultiGROUP door

Als deze eigenschap is ingesteld op true, een query MultiGROUP door met de algemene groep door sleutels genereert een enkel MapReduce-taak.  

Als u wilt inschakelen dit gedrag, toevoegen de `hive.multigroupby.singlereducer` parameter voor de aangepaste hive-site in het deelvenster, zoals wordt beschreven in stap 3 van de [tussenliggende compressie inschakelen](#enable-intermediate-compression) sectie.

![Hive ingesteld één MapReduce MultiGROUP door](./media/hdinsight-changing-configs-via-ambari/hive-multigroupby-singlereducer.png)

### <a name="additional-hive-optimizations"></a>Aanvullende optimaliseringen voor Hive

De volgende secties beschrijven aanvullende Hive-gerelateerde optimalisaties die u kunt instellen.

#### <a name="join-optimizations"></a>Optimalisaties toevoegen

Het jointype in component is een *shuffle join*. In Hive, speciale mappers Lees de invoer- en verzenden van een join-sleutel/waarde-paar naar een tussenliggende-bestand. Hadoop worden gesorteerd en worden deze paren samengevoegd in de fase van een willekeurige volgorde. Deze fase shuffle is kostbaar. De juiste join op basis van uw gegevens te selecteren, kan de prestaties aanzienlijk verbeteren.

| Jointype | Wanneer | Hoe | Hive-instellingen | Opmerkingen |
| -- | -- | -- | -- | -- |
| Shuffle Join | <ul><li>Standaardoptie</li><li>Altijd werkt</li></ul> | <ul><li>Leest uit deel uitmaakt van een van de tabellen</li><li>Buckets en gesorteerd op Join-sleutel</li><li>Verzendt een bucket naar elke verminderen</li><li>Join is uitgevoerd op de zijde verminderen</li></ul> | Er is geen aanzienlijke Hive benodigde instellen | Elke keer werkt |
| Lid worden van kaart | <ul><li>Een tabel past in het geheugen</li></ul> | <ul><li>Kleine tabel leest in geheugen hash-tabel</li><li>Stromen via deel van de grote bestanden</li><li>Lid wordt van elke record van hash-tabel</li><li>Moeten worden samengevoegd door het toewijzen van de alleen</li></ul> | `hive.auto.confvert.join=true` | Zeer snelle maar beperkte |
| Bucket voor het samenvoegen van sorteren | Als beide tabellen zijn: <ul><li>Dezelfde gesorteerd</li><li>Dezelfde bucketed</li><li>Op de gesorteerd/gerangschikte kolom toevoegen</li></ul> | Elk proces: <ul><li>Leest een bucket uit elke tabel</li><li>De rij met de laagste waarde verwerkt</li></ul> | `hive.auto.convert.sortmerge.join=true` | Zeer efficiënt |

#### <a name="execution-engine-optimizations"></a>Optimalisaties voor uitvoering van engine

Extra aanbevelingen voor het optimaliseren van de engine voor het uitvoeren van Hive:

| Instelling | Aanbevolen | HDInsight-standaard |
| -- | -- | -- |
| `hive.mapjoin.hybridgrace.hashtable` | = True veiliger, langzamer; False = sneller | false |
| `tez.am.resource.memory.mb` | Bovengrens van 4 GB voor de meeste | Automatisch zijn afgestemd |
| `tez.session.am.dag.submit.timeout.secs` | 300+ | 300 |
| `tez.am.container.idle.release-timeout-min.millis` | 20000+ | 10.000 |
| `tez.am.container.idle.release-timeout-max.millis` | 40000+ | 20000 |

## <a name="pig-optimization"></a>Pig-optimalisatie

Pig-eigenschappen kunnen worden gewijzigd via de Ambari-webgebruikersinterface om af te stemmen Pig-query's. Pig-eigenschappen van Ambari rechtstreeks wijzigen wijzigt u de Pig-eigenschappen in de `/etc/pig/2.4.2.0-258.0/pig.properties` bestand.

1. Pig om eigenschappen te wijzigen, gaat u naar de Pig **Peeringconfiguraties** tabblad uit en vouw vervolgens de **geavanceerde pig-eigenschappen** deelvenster.

1. Zoeken, verwijder de opmerkingen en wijzig de waarde van de eigenschap die u wilt wijzigen.

1. Selecteer **opslaan** op de rechtsboven in het venster om op te slaan van de nieuwe waarde. Sommige eigenschappen is mogelijk een service opnieuw is gestart.

    ![Geavanceerde pig-eigenschappen](./media/hdinsight-changing-configs-via-ambari/advanced-pig-properties.png)
 
> [!NOTE]
> Sessie-niveau instellingen overschrijven eigenschapswaarden in het `pig.properties` bestand.

### <a name="tune-execution-engine"></a>Engine voor het uitvoeren van stemmen

Twee motoren voor uitvoering beschikbaar zijn voor Pig-scripts uit te voeren: MapReduce en Tez. Tez is een engine voor geoptimaliseerde en is veel sneller dan MapReduce.

1. Wijzigen van de engine voor het uitvoeren, in de **geavanceerde pig-eigenschappen** in het deelvenster Zoeken van de eigenschap `exectype`.

1. De standaardwaarde is **MapReduce**. Wijzig dit in **Tez**.


### <a name="enable-local-mode"></a>De lokale modus inschakelen

Net als bij Hive, lokale modus wordt gebruikt om de snelheid van taken met relatief kleinere hoeveelheden gegevens.

1. De lokale modus wilt activeren, stel `pig.auto.local.enabled` naar **waar**. De standaardwaarde is false.

1. Taken met een grootte van invoergegevens kleiner is dan de `pig.auto.local.input.maxbytes` eigenschapswaarde worden beschouwd als kleine taken. De standaardwaarde is 1 GB.


### <a name="copy-user-jar-cache"></a>Cache van de gebruiker jar kopiëren

Pig kopieert de JAR-bestanden vereist voor UDF's aan een gedistribueerde cache zodat ze beschikbaar zijn voor de knooppunten van de taak. Deze JAR-bestanden wijzigen niet vaak. Indien ingeschakeld, de `pig.user.cache.enabled` beleidsinstelling JAR-bestanden in een cache te hergebruiken voor taken die worden uitgevoerd door dezelfde gebruiker worden geplaatst. Dit resulteert in een kleine toename in de prestaties van de taak.

1. Instellen om in te schakelen, `pig.user.cache.enabled` op ' True '. De standaardwaarde is false.

1. Om in te stellen het basispad van de JAR-bestanden in de cache `pig.user.cache.location` aan het basispad. De standaardwaarde is `/tmp`.


### <a name="optimize-performance-with-memory-settings"></a>Optimaliseer de prestaties met instellingen voor het geheugen

De volgende instellingen voor het geheugen kunnen helpen Pig-script-prestaties te optimaliseren.

* `pig.cachedbag.memusage`: De hoeveelheid geheugen toegewezen aan een eigenschappenverzameling. Een eigenschappenverzameling is een verzameling van tuples. Een tuple is een geordende reeks velden en een veld is een hoeveelheid gegevens. Als de gegevens in een eigenschappenverzameling buiten het toegewezen geheugen valt, is het een zich verspreid naar schijf. De standaardwaarde is 0.2, waarvan 20 procent van het beschikbare geheugen vertegenwoordigt. Dit geheugen wordt gedeeld tussen alle zakken in een toepassing.

* `pig.spill.size.threshold`: Zakken groter is dan deze drempelwaarde worden gelekt (in bytes) zijn zich verspreid naar schijf. De standaardwaarde is 5 MB.


### <a name="compress-temporary-files"></a>Tijdelijke bestanden comprimeren

Pig genereert tijdelijke bestanden tijdens het uitvoeren van taak. De tijdelijke bestanden te comprimeren resulteert in een toename van de prestaties bij het lezen of schrijven van bestanden naar de schijf. De volgende instellingen kunnen worden gebruikt voor het comprimeren van tijdelijke bestanden.

* `pig.tmpfilecompression`: Als deze eigenschap waar is, kunt tijdelijk bestandscompressie. De standaardwaarde is false.

* `pig.tmpfilecompression.codec`: De compressiecodec moet worden gebruikt voor de tijdelijke bestanden te comprimeren. De aanbevolen compressiecodecs zijn LZO en Snappy voor lagere CPU-gebruik.

### <a name="enable-split-combining"></a>Inschakelen van split combineren

Wanneer dit is ingeschakeld, worden kleine bestanden worden gecombineerd voor minder taken van de kaart. Dit verbetert de efficiëntie van taken met veel kleine bestanden. Instellen om in te schakelen, `pig.noSplitCombination` op ' True '. De standaardwaarde is false.


### <a name="tune-mappers"></a>Mappers afstemmen

Het aantal mappers wordt bepaald door het wijzigen van de eigenschap `pig.maxCombinedSplitSize`. Hiermee geeft u de grootte van de gegevens moeten worden verwerkt door een taak één kaart. De standaardwaarde is de Standaardblokgrootte van het bestandssysteem. Deze waarde resulteert in een afname van het aantal taken toewijzen te vergroten.


### <a name="tune-reducers"></a>Reducers tegelijkertijd afstemmen

Het aantal reducers tegelijkertijd wordt berekend op basis van de parameter `pig.exec.reducers.bytes.per.reducer`. De parameter geeft u het aantal bytes dat per reducer wordt verwerkt, standaard 1 GB. Om te beperken het maximum aantal reducers tegelijkertijd, stel de `pig.exec.reducers.max` eigenschap standaard 999.


## <a name="hbase-optimization-with-the-ambari-web-ui"></a>HBase-optimalisatie met de Ambari-Webinterface

HBase-configuratie is gewijzigd vanuit de **HBase Configs** tabblad. De volgende secties beschrijven enkele van de belangrijke configuratie-instellingen die invloed hebben op prestaties van HBase.

### <a name="set-hbaseheapsize"></a>Set HBASE_HEAPSIZE

De HBase-heap-grootte geeft de maximale hoeveelheid opslagruimte moet worden gebruikt in megabytes aangegeven door *regio* en *master* servers. De standaardwaarde is 1000 MB. Dit moet zijn afgestemd op voor de clusterwerkbelasting van het.

1. Als u wilt wijzigen, gaat u naar de **geavanceerde HBase-env** deelvenster in de HBase **Peeringconfiguraties** tabblad en gaat u naar de `HBASE_HEAPSIZE` instelling.

1. Wijzig de standaardwaarde in 5000 MB.

    ![HBASE_HEAPSIZE](./media/hdinsight-changing-configs-via-ambari/hbase-heapsize.png)


### <a name="optimize-read-heavy-workloads"></a>Leesintensief workloads optimaliseren

De volgende configuraties zijn belangrijk om de prestaties van leesintensief werkbelastingen te verbeteren.

#### <a name="block-cache-size"></a>Grootte van blok-cache

De blokcache is de leescache. De grootte wordt bepaald door de `hfile.block.cache.size` parameter. De standaardwaarde is 0.4, 40 procent van de totale regio server-geheugen. Hoe groter de cache blokgrootte, hoe sneller de willekeurige leesbewerkingen zijn.

1. Als u wilt deze parameter wijzigen, gaat u naar de **instellingen** tabblad in de HBase **Peeringconfiguraties** tabblad en zoek vervolgens **% van de RegionServer toegewezen aan de lezen Buffers**.

    ![Blokgrootte van HBase voor cache](./media/hdinsight-changing-configs-via-ambari/hbase-block-cache-size.png)
 
1. De waarde wilt wijzigen, selecteert u de **bewerken** pictogram.


#### <a name="memstore-size"></a>Grootte van de geheugenopslag

Alle wijzigingen worden opgeslagen in het geheugen voor buffer, met de naam een *setSize()*. Dit verhoogt de totale hoeveelheid gegevens die kunnen worden geschreven naar de schijf in één bewerking, en het daaropvolgende toegang versnelt naar de recente wijzigingen. De grootte van de geheugenopslag is gedefinieerd door de volgende twee parameters:

* `hbase.regionserver.global.memstore.UpperLimit`: Hiermee definieert u het maximale percentage van de regioserver die setSize() gecombineerd kunt gebruiken.

* `hbase.regionserver.global.memstore.LowerLimit`: Hiermee definieert u het minimumpercentage van de regioserver die setSize() gecombineerd kunt gebruiken.

Om te optimaliseren voor willekeurige lees, kunt u de geheugenopslag hogere en lagere limieten te verminderen.


#### <a name="number-of-rows-fetched-when-scanning-from-disk"></a>Aantal rijen als resultaat bij het scannen van schijf

De `hbase.client.scanner.caching` instelling bepaalt het aantal rijen lezen van schijf wanneer de `next` methode wordt aangeroepen op een scanner.  De standaardwaarde is 100. Hoe hoger het getal, minder externe oproepen gemaakt van de client naar de regioserver, wat resulteert in een snellere scans. Dit wordt echter ook geheugendruk op de client verhogen.

![Het aantal rijen als resultaat HBase](./media/hdinsight-changing-configs-via-ambari/hbase-num-rows-fetched.png)

> [!IMPORTANT]
> Stel de waarde niet, zodat de tijd tussen de aanroep van de volgende methode op een scanner groter dan de time-out van de scanner is. De duur van de scanner time-out wordt gedefinieerd door de `hbase.regionserver.lease.period` eigenschap.


### <a name="optimize-write-heavy-workloads"></a>Schrijfintensief workloads optimaliseren

De volgende configuraties zijn belangrijk om de prestaties van schrijfintensief workloads te verbeteren.


#### <a name="maximum-region-file-size"></a>De regio van de maximale bestandsgrootte

HBase-gegevens opslaat in een interne bestandsindeling, met de naam *HFile*. De eigenschap `hbase.hregion.max.filesize` bepaalt de grootte van een enkele HFile voor een regio.  Een regio is gesplitst in twee regio's als de som van alle HFiles in een regio groter is dan deze instelling.
 
![HBase HRegion maximale bestandsgrootte](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-max-filesize.png)

Hoe groter de regio bestandsgrootte, hoe kleiner het aantal splitsingen. U kunt de bestandsgrootte om te bepalen van een waarde resulteert in het maximum schrijfvaardigheden verhogen.


#### <a name="avoid-update-blocking"></a>Vermijd update blokkeren

* De eigenschap `hbase.hregion.memstore.flush.size` bepaalt de grootte waarmee setSize() worden leeggemaakt op schijf. De standaardgrootte is 128 MB.

* De Hbase-regio blok vermenigvuldiger wordt gedefinieerd door `hbase.hregion.memstore.block.multiplier`. De standaardwaarde is 4. De maximaal toegestane aantal is 8.

* HBase updates als de geheugenopslag is geblokkeerd (`hbase.hregion.memstore.flush.size` * `hbase.hregion.memstore.block.multiplier`) bytes.

    Met de standaardwaarden van leegmaken grootte en hetzelfde blok vermenigvuldiger, worden de updates worden geblokkeerd wanneer de geheugenopslag is 128 * 4 = 512 MB in grootte. Verhoog de waarde van de update aantal blokkeren, verminderen `hbase.hregion.memstore.block.multiplier`.

![Vermenigvuldiger voor het blokkeren van HBase-regio](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-memstore-block-multiplier.png)


### <a name="define-memstore-size"></a>Grootte van de geheugenopslag definiëren

Grootte van de geheugenopslag is gedefinieerd door de `hbase.regionserver.global.memstore.UpperLimit` en `hbase.regionserver.global.memstore.LowerLimit` parameters. Als u deze waarden gelijk zijn die aan elke andere wordt onderbroken tijdens vermindert schrijfbewerkingen (ook waardoor vaker leegmaken) en resulteert in betere schrijven prestaties.


### <a name="set-memstore-local-allocation-buffer"></a>SetSize() lokale toewijzing buffer ingesteld

Gebruik van setSize() lokale toewijzing buffer wordt bepaald door de eigenschap `hbase.hregion.memstore.mslab.enabled`. Wanneer dit is ingeschakeld (waar) of zo wordt voorkomen dat heapfragmentatie van de tijdens zware schrijfbewerking. De standaardwaarde is true.
 
![hbase.hregion.memstore.mslab.Enabled](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-memstore-mslab-enabled.png)


## <a name="next-steps"></a>Volgende stappen

* [HDInsight-clusters met de Ambari-Webinterface beheren](hdinsight-hadoop-manage-ambari.md)
* [Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)
