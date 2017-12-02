---
title: Voorbeelden van Hadoop MapReduce uitvoeren op HDInsight - Azure | Microsoft Docs
description: Aan de slag met MapReduce-voorbeelden in jar-bestanden die zijn opgenomen in HDInsight. SSH gebruiken voor verbinding met het cluster en vervolgens met de opdracht Hadoop voorbeeld taken uitvoeren.
keywords: hadoop voorbeeld jar, hadoop voorbeelden jar, voorbeelden van hadoop-mapreduce, mapreduce-voorbeelden
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e1d2a0b9-1659-4fab-921e-4a8990cbb30a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: larryfr
ms.openlocfilehash: 700f73d86686761908d241f989aab25e0fb43c0e
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="run-the-mapreduce-examples-included-in-hdinsight"></a>Voer de MapReduce-voorbeelden opgenomen in HDInsight

[!INCLUDE [samples-selector](../../../includes/hdinsight-run-samples-selector.md)]

Informatie over het uitvoeren van de MapReduce-voorbeelden opgenomen met Hadoop op HDInsight.

## <a name="prerequisites"></a>Vereisten

* **Een HDInsight-cluster**: Zie [aan de slag met Hadoop met Hive in HDInsight op Linux](apache-hadoop-linux-tutorial-get-started.md)

    > [!IMPORTANT]
    > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* **Een SSH-client**: Zie voor meer informatie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="the-mapreduce-examples"></a>De MapReduce-voorbeelden

**Locatie**: voorbeelden bevinden zich op het HDInsight-cluster op `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.

**Inhoud**: de volgende voorbeelden zijn opgenomen in dit archief:

* `aggregatewordcount`: Er is een statistische functie gebaseerd mapreduce-programma dat de woorden in de invoerbestanden geteld.
* `aggregatewordhist`: Mapreduce-programma dat het histogram van de woorden in de invoerbestanden wordt berekend op basis van er is een statistische functie.
* `bbp`: Een mapreduce-programma dat gebruikmaakt van Bailey-Borwein-Plouffe exacte cijfers van Pi berekenen.
* `dbcount`: Er is een voorbeeld van de taak die de logboeken van de pageview opgeslagen in een database telt.
* `distbbp`: Een mapreduce-programma dat gebruikmaakt van een formule BBP type exacte bits van Pi berekenen.
* `grep`: Een mapreduce-programma waarmee het overeenkomen met een reguliere expressie in de invoer wordt geteld.
* `join`: Een taak die een join uitvoert via gesorteerd, evenveel gepartitioneerd gegevenssets.
* `multifilewc`: Een taak die woorden uit verschillende bestanden telt.
* `pentomino`: Een mapreduce tegel legdatum programma om oplossingen voor problemen pentomino te vinden.
* `pi`: Een mapreduce-programma dat u maakt een schatting van Pi met behulp van een quasi Monte Carlo-methode.
* `randomtextwriter`: Een mapreduce-programma dat 10 GB met willekeurige tekstgegevens per knooppunt schrijft.
* `randomwriter`: Een mapreduce-programma dat 10 GB willekeurige gegevens per knooppunt schrijft.
* `secondarysort`: Er is een voorbeeld van een tweede sortering definiëren met de fase verminderen.
* `sort`: Een mapreduce-programma dat deze gegevens geschreven door willekeurige writer sorteert.
* `sudoku`: Een sudoku Solver '.
* `teragen`: De gegevens voor de terasort genereren.
* `terasort`: De terasort worden uitgevoerd.
* `teravalidate`: De resultaten van terasort controleren.
* `wordcount`: Een mapreduce-programma dat de woorden in de invoerbestanden geteld.
* `wordmean`: Een mapreduce-programma dat de gemiddelde lengte van de woorden in de invoerbestanden telt.
* `wordmedian`: Een mapreduce-programma dat de mediaan lengte van de woorden in de invoerbestanden telt.
* `wordstandarddeviation`: Een mapreduce-programma dat de standaarddeviatie van de lengte van de woorden in de invoerbestanden telt.

**Broncode**: broncode voor deze voorbeelden is opgenomen op het HDInsight-cluster op `/usr/hdp/current/hadoop-client/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.

## <a name="run-the-wordcount-example"></a>Het wordcount-voorbeeld uitvoeren

1. Verbinding maken met HDInsight met behulp van SSH. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Van de `username@#######:~$` gevraagd, gebruikt u de volgende opdracht voor een lijst met voorbeelden:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    Met deze opdracht genereert de lijst van voorbeeld uit de vorige sectie van dit document.

3. Gebruik de volgende opdracht om hulp te krijgen van een specifieke steekproef. In dit geval de **wordcount** voorbeeld:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    Het volgende foutbericht wordt weergegeven:

        Usage: wordcount <in> [<in>...] <out>

    Dit bericht geeft aan dat u verschillende invoerpaden voor de brondocumenten bieden kunt. Het laatste pad is waar de uitvoer (aantal woorden in de brondocumenten) wordt opgeslagen.

4. Gebruik de volgende voor het tellen van alle woorden in de laptops van Leonardo Da Vinci, die beschikbaar worden gesteld als voorbeeldgegevens met het cluster:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    Invoer voor deze taak wordt gelezen uit `/example/data/gutenberg/davinci.txt`. Uitvoer voor dit voorbeeld wordt opgeslagen in `/example/data/davinciwordcount`. Beide paden bevinden zich op standaard opslagruimte voor het cluster, niet het lokale bestandssysteem.

   > [!NOTE]
   > Zoals beschreven in de help voor het wordcount-voorbeeld, kunt u ook meerdere invoerbestanden opgeven. Bijvoorbeeld: `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` woorden in zowel davinci.txt en ulysses.txt geteld.

5. Zodra de taak is voltooid, gebruikt u de volgende opdracht om de uitvoer weer te geven:

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    Met deze opdracht worden alle de uitvoerbestanden die wordt geproduceerd door de taak. Hiermee wordt de uitvoer naar de console weergegeven. De uitvoer lijkt op het volgende:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Elke regel vertegenwoordigt een woord en hoe vaak het is opgetreden in de invoergegevens.

## <a name="the-sudoku-example"></a>In het voorbeeld Sudoku

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) is een logische puzzel bestaat uit negen 3 x 3 rasters. Sommige cellen in het raster hebben aantallen, terwijl andere leeg zijn en het doel is om op te lossen voor de lege cellen. De vorige koppeling meer informatie over de puzzel heeft, maar het doel van dit voorbeeld is voor het oplossen van voor de lege cellen. De invoer moet dus een bestand dat zich in de volgende indeling:

* Negen rijen van de negen kolommen
* Elke kolom mag een getal of `?` (geeft een lege cel)
* Cellen zijn gescheiden door een spatie

Er is een bepaalde manier om samen te stellen Sudoku puzzels; een getal in een kolom of de rij kan niet worden herhaald. Er is een voorbeeld op het HDInsight-cluster correct is samengesteld. Deze bevindt zich op `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` en bevat de volgende tekst:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

Om dit probleem voorbeeld uitvoert via het voorbeeld Sudoku, gebruik de volgende opdracht:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

De resultaten worden de volgende strekking weergegeven:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a>Voorbeeld van pi (π)

Het voorbeeld pi maakt gebruik van een statistische (quasi Monte Carlo) methode voor het schatten van de waarde van pi. Punten worden in willekeurige volgorde in een vierkant eenheid geplaatst. Het kwadraat bevat ook een cirkel. De kans dat de punten valt binnen de cirkel gelijk zijn aan het gebied van de cirkel pi/4. De waarde van pi kan worden geschat van de waarde van 4R. R is de verhouding tussen het aantal punten die in het totale aantal punten die binnen het kwadraat van de cirkel. Hoe groter het voorbeeld van punten gebruikt, hoe nauwkeuriger de schatting is.

Gebruik de volgende opdracht om uit te voeren in dit voorbeeld. Met deze opdracht gebruikt 16-kaarten met 10.000.000 samples als indicatie van de waarde van pi:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

De waarde die is geretourneerd door deze opdracht is vergelijkbaar met **3.14159155000000000000**. Voor verwijzingen naar zijn de eerste 10 decimalen van pi 3.1415926535.

## <a name="10-gb-greysort-example"></a>Voorbeeld van 10 GB-Greysort

GraySort is een benchmark-sortering. De meetwaarde is de sorteren snelheid (TB per minuut) die tijdens het sorteren van grote hoeveelheden gegevens, meestal een 100 TB minimale wordt bereikt.

Dit voorbeeld wordt een matige 10 GB aan gegevens gebruikt, zodat deze kan relatief snel kan worden uitgevoerd. De MapReduce-toepassingen die zijn ontwikkeld door Owen O'Malley en Arun Murthy wordt gebruikt. Deze toepassingen gewonnen de jaarlijkse Algemeen ('daytona') terabyte sorteren benchmark in 2009, met een snelheid van 0.578 TB/min (100 TB in 173 minuten). Zie voor meer informatie over deze en andere sorteren benchmarks de [Sortbenchmark](http://sortbenchmark.org/) site.

Dit voorbeeld worden drie sets van MapReduce-programma's gebruikt:

* **TeraGen**: A MapReduce programma dat wordt gegenereerd rijen met gegevens te sorteren

* **TeraSort**: voorbeelden van de invoergegevens en MapReduce gebruikt voor het sorteren van de gegevens in een totale volgorde

    TeraSort is een standaard sortering MapReduce, met uitzondering van een aangepaste partitioner. De partitioner maakt gebruik van een gesorteerde lijst met actieve N-1 sleutels die het belangrijkste bereik voor elke verminderen definiëren. In het bijzonder alle sleutels dergelijke waarvan de steekproef [i-1] < sleutel = < voorbeeld [i] worden verzonden naar de i verminderen. Deze partitioner garanties dat de uitvoer van ik beperken zijn alle kleiner is dan de uitvoer van verminderen i + 1.

* **TeraValidate**: een MapReduce-programma te valideren dat de uitvoer globaal is gesorteerd

    Een kaart per bestand wordt gemaakt in de uitvoermap en elke toewijzing zorgt ervoor dat elke sleutel kleiner dan of gelijk zijn aan de vorige sectie is. De functie kaart genereert records van de eerste en laatste sleutels van elk bestand. De functie verminderen zorgt ervoor dat de eerste sleutel van het bestand i groter dan de laatste sleutel van het bestand i-1 is. Eventuele problemen worden gemeld als uitvoer van de fase verminderen met de sleutels die onjuist zijn.

Gebruik de volgende stappen voor het genereren van gegevens, sorteren en controleer vervolgens de uitvoer:

1. Genereren van 10 GB aan gegevens, die is opgeslagen op het HDInsight-cluster standaard opslag op `/example/data/10GB-sort-input`:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    De `-Dmapred.map.tasks` vertelt Hadoop hoeveel kaart taken moet worden gebruikt voor deze taak. De taak voor het maken van 10 GB aan gegevens en bewaar deze op de opdracht geven de laatste twee parameters `/example/data/10GB-sort-input`.

2. Gebruik de volgende opdracht om de gegevens te sorteren:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    De `-Dmapred.reduce.tasks` Hadoop geeft aan hoeveel verminderen taken moet worden gebruikt voor de taak. De laatste twee parameters zijn alleen de invoer- en locaties voor de gegevens.

3. Gebruik de volgende om de gegevens die worden gegenereerd door de sortering te valideren:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u de voorbeelden van opgenomen in de Linux gebaseerde HDInsight-clusters uitvoeren. Zie de volgende onderwerpen voor zelfstudies over het gebruik van Pig, Hive en MapReduce met HDInsight:

* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)
* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-introduction]:apache-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

