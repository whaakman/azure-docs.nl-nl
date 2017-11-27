---
title: Functies voor gegevens in een Hadoop-cluster met behulp van Hive-query's maken | Microsoft Docs
description: Voorbeelden van Hive-query's die functies in de gegevens die zijn opgeslagen in een Azure HDInsight Hadoop-cluster genereren.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: e8a94c71-979b-4707-b8fd-85b47d309a30
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 91ea23b732f520b02af7e9a9dd77ee62190a520c
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="create-features-for-data-in-a-hadoop-cluster-using-hive-queries"></a>Functies voor gegevens in een Hadoop-cluster met behulp van Hive-query's maken
Dit document wordt beschreven hoe de functies voor gegevens die zijn opgeslagen in een Azure HDInsight Hadoop-cluster met behulp van Hive-query's maken. Deze Hive-query's gebruiken ingesloten Hive User-Defined-functies (UDF's), de scripts die worden geleverd.

De bewerkingen die nodig zijn voor het maken van functies kunnen geheugenintensief zijn. De prestaties van Hive-query's wordt meer kritieke in dergelijke gevallen en kan worden verbeterd door het afstemmen van bepaalde parameters. Het afstemmen van deze parameters wordt besproken in het laatste gedeelte.

Voorbeelden van de query's die worden aangeboden, zijn specifiek voor de [NYC Taxi reis gegevens](http://chriswhong.com/open-data/foil_nyc_taxi/) scenario's zijn ook beschikbaar [GitHub-opslagplaats](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Deze query's al hebt opgegeven gegevensschema en klaar bent om te worden verzonden om uit te voeren. Parameters die gebruikers afstemmen kunnen, zodat de prestaties van Hive-query's kan worden verbeterd worden ook besproken in het laatste gedeelte.

[!INCLUDE [cap-create-features-data-selector](../../../includes/cap-create-features-selector.md)]

Dit **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u functies voor gegevens in verschillende omgevingen maken. Deze taak is een stap in de [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een Azure storage-account gemaakt. Als u instructies nodig hebt, raadpleegt u [een Azure Storage-account maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Een aangepaste Hadoop-cluster met de HDInsight-service wordt ingericht.  Als u instructies nodig hebt, raadpleegt u [aanpassen Azure HDInsight Hadoop-Clusters voor geavanceerde analyses](customize-hadoop-cluster.md).
* De gegevens is geüpload naar de Hive-tabellen in Azure HDInsight Hadoop-clusters. Als dit niet het geval is, voert u de [maken en gegevens laden met Hive-tabellen](move-hive-tables.md) eerst gegevens uploaden naar Hive-tabellen.
* Externe toegang tot het cluster is ingeschakeld. Als u instructies nodig hebt, raadpleegt u [toegang tot de hoofd-knooppunt van Hadoop-Cluster](customize-hadoop-cluster.md).

## <a name="hive-featureengineering"></a>Functie generatie
In deze sectie vindt u enkele voorbeelden van de manieren waarop functies kunnen genereren met behulp van Hive-query's. Nadat u extra functies hebt gegenereerd, kunt u ze als kolommen toevoegen aan de bestaande tabel of een nieuwe tabel maken met de aanvullende functies en de primaire sleutel, die vervolgens kan worden samengevoegd met de oorspronkelijke tabel. Hier volgen de voorbeelden:

1. [Generatie van de functie op basis van de frequentie](#hive-frequencyfeature)
2. [Risico's van Categorische variabelen in binaire classificatie](#hive-riskfeature)
3. [Functies van Datetime Field uitpakken](#hive-datefeatures)
4. [Functies van tekstveld uitpakken](#hive-textfeatures)
5. [Afstand tussen GPS-coördinaten berekenen](#hive-gpsdistance)

### <a name="hive-frequencyfeature"></a>Generatie van de functie op basis van de frequentie
Vaak is het nuttig om de frequenties van de niveaus van een categorische variabele of de frequenties van bepaalde combinaties van niveaus uit meerdere categorische variabelen te berekenen. Gebruikers kunnen het volgende script gebruiken voor het berekenen van deze frequenties:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <a name="hive-riskfeature"></a>Risico's van categorische variabelen in binaire classificatie
In binaire indeling, moeten niet-numerieke categorische variabelen worden geconverteerd naar numerieke onderdelen wanneer de modellen die alleen wordt gebruikt, neemt de numerieke onderdelen. Deze conversie wordt uitgevoerd door verschillende niveaus van niet-numerieke vervangen door een numerieke risico. Deze sectie vindt enkele algemene Hive-query's die de waarden van de risico's (log kans) van een variabele categorische berekenen.

        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

In dit voorbeeld variabelen `smooth_param1` en `smooth_param2` vloeiend de risico-waarden berekend op basis van de gegevens zijn ingesteld. Risico's hebben een bereik tussen -Inf en inf-bestand. Een risico > 0 geeft aan dat de kans dat het doel gelijk aan 1 is groter dan 0,5 is.

Na het risico is tabel berekend dat gebruikers kunnen risico waarden toewijzen aan een tabel door deze samen te voegen met de tabel risico. De gekoppelde Hive-query is opgegeven in de vorige sectie.

### <a name="hive-datefeatures"></a>Functies van datetime-velden uitpakken
Hive wordt geleverd met een reeks UDF's voor het verwerken van datetime-velden. In component, de standaardnotatie voor datum/tijd is ' jjjj-MM-dd 00:00:00 ' ('01-01-1970 12:21:32 ' bijvoorbeeld). In deze sectie bevat voorbeelden die de dag van een maand, de maand van een datetime-veld uitpakken en andere voorbeelden die een datum/tijd-tekenreeks in een indeling geconverteerd andere dan de standaardindeling naar een datum/tijd-tekenreeks in standaard opmaken.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Deze Hive-query wordt ervan uitgegaan dat de *&#60; datetime-veld >* bevindt zich in de standaardnotatie voor datum/tijd.

Als een datetime-veld niet in de standaardindeling is, moet u eerst het datetime-veld converteren naar Unix tijdstempel, en vervolgens het tijdstempel Unix converteren naar een datum/tijd-tekenreeks die in de standaardindeling. Wanneer de datum/tijd in-indeling wordt, kunnen gebruikers de ingesloten datetime UDF's uitpakken functies toepassen.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

In deze query als de *&#60; datetime-veld >* heeft het patroon zoals *03/26/2015 12:04:39*, wordt de *' &#60; patroon van de datetime-veld >'* moet `'MM/dd/yyyy HH:mm:ss'`. Als u wilt testen, kunnen gebruikers uitvoeren

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

De *hivesampletable* in deze query vooraf is geïnstalleerd op alle Azure HDInsight Hadoop-clusters standaard wanneer de clusters zijn ingericht.

### <a name="hive-textfeatures"></a>Functies van tekstvelden uitpakken
Wanneer de Hive-tabel een tekstveld die een tekenreeks met woorden die worden gescheiden door spaties bevat heeft, wordt de volgende query de lengte van de tekenreeks en het aantal woorden in de tekenreeks.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <a name="hive-gpsdistance"></a>Afstand tussen sets GPS-coördinaten berekenen
De query is opgegeven in deze sectie kan rechtstreeks worden toegepast op de NYC Taxi reis gegevens. Het doel van deze query is om het toepassen van een ingesloten wiskundige functie in de component voor het genereren van de functies weer te geven.

De velden die worden gebruikt in deze query zijn de GPS-coördinaten van ophalen en dropoff locaties, met de naam *ophalen\_lengtegraad*, *ophalen\_breedtegraad*, *dropoff\_lengtegraad*, en *dropoff\_breedtegraad*. De query's die de directe afstand tussen de coördinaten ophalen en dropoff berekenen zijn:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

De wiskundige vergelijkingen die de afstand tussen twee GPS-coördinaten berekenen vindt u op de <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">MovableType Scripts</a> geschreven door Peter Lapisu-site. In deze Javascript, de functie `toRad()` is zojuist *lat_or_lon*pi/180 *, die graden converteert naar radialen. Hier *lat_or_lon* is de breedtegraad of lengtegraad. Aangezien Hive niet in de functie voorziet `atan2`, maar de functie biedt `atan`, wordt de `atan2` functie wordt geïmplementeerd door `atan` functie in de bovenstaande Hive-query met de opgegeven in de definitie <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia (Engelstalig)</a>.

![Werkruimte maken](./media/create-features-hive/atan2new.png)

Een volledige lijst met Hive ingesloten UDF's vindt u in de **ingebouwde functies** sectie op de <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).  

## <a name="tuning"></a>Onderwerpen over geavanceerde: stemmen Hive-parameters voor het verbeteren van de query
De standaardinstellingen van de parameter van Hive-cluster is mogelijk niet geschikt is voor de Hive-query's en de gegevens die de verwerking van de query's. Deze sectie wordt beschreven sommige parameters die gebruikers afstemmen kunnen om de prestaties van Hive-query's te verbeteren. Gebruikers moeten de parameter afstemmen van query's voordat de query's van het verwerken van gegevens toevoegen.

1. **Java-heap ruimte**: voor query's met betrekking tot lid te worden grote gegevenssets of verwerken van lange records **bijna vol heap** is een van de algemene fouten. Deze fout kan worden voorkomen door het instellen van de parameters *mapreduce.map.java.opts* en *mapreduce.task.io.sort.mb* naar de gewenste waarden. Hier volgt een voorbeeld:
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    Deze parameter 4GB geheugen met Java heap adresruimte toewijst en maakt ook sorteren efficiënter door meer geheugen toewijzen voor deze. Er is een goed idee om af te spelen met deze toewijzingen als er een taak mislukt fouten met betrekking tot heap-ruimte.

1. **DFS-blokgrootte**: deze parameter stelt de kleinste gegevenseenheid die het bestandssysteem worden opgeslagen. Als u bijvoorbeeld dat als de blokgrootte van DFS-128 MB, klikt u vervolgens de gegevens van de grootte van minder dan en maximaal is 128 MB wordt opgeslagen in één blok. Gegevens die groter is dan 128 MB extra blokken wordt toegewezen. 
2. Het kiezen van een kleine blokgrootte zorgt ervoor dat grote overhead in Hadoop omdat de naam van knooppunt heeft veel meer aanvragen zoeken van de relevante blok met betrekking tot het bestand te verwerken. Een aanbevolen instelling als betreft gigabyte (of groter) gegevens:

        set dfs.block.size=128m;

2. **Join-bewerking in Hive optimaliseren**: terwijl joinbewerkingen in het kader van de kaart/verminderen doorgaans in de verminderen fase soms plaatsvinden enorm veel toeneemt kunnen worden bereikt door planning joins in de fase van de kaart (ook wel 'mapjoins' genoemd). Rechtstreeks Hive om dit te doen indien mogelijk, instellen:
   
       set hive.auto.convert.join=true;

3. **Waarmee het aantal mappers aan component**: terwijl Hadoop kan de gebruiker het aantal verkleiningstoestellen instellen, het aantal mappers is doorgaans niet worden ingesteld door de gebruiker. Een slag waarmee bepaalde mate van controle op dit aantal is het kiezen van de Hadoop-variabelen *mapred.min.split.size* en *mapred.max.split.size* als de grootte van elke kaart taak wordt bepaald door:
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    Normaal gesproken de standaardwaarde van:
    
    - *mapred.min.split.Size* is ingesteld op 0 van
    - *mapred.Max.split.Size* is **Long.MAX** en die van 
    - *DFS.Block.Size* is 64 MB.

    Zoals we zien, krijgt de gegevensgrootte afstemming van deze parameters door 'instelling' ze kunt u ons af te stemmen, het aantal mappers gebruikt.

4. Hier volgen enkele andere meer **geavanceerde opties voor** voor Hive-prestaties te optimaliseren. Deze kunnen u instellen van het geheugen toegewezen als u wilt toewijzen en taken te verminderen en nuttig kunnen zijn bij het afstemmen van de prestaties. Houd rekening met de *mapreduce.reduce.memory.mb* mag niet groter zijn dan de grootte van fysiek geheugen van elk knooppunt van de werknemer in het Hadoop-cluster.
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

