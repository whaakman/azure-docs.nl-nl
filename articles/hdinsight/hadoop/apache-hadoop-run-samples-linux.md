---
title: Voorbeelden van Hadoop MapReduce uitvoeren op HDInsight - Azure
description: Aan de slag met voorbeelden van MapReduce in jar-bestanden die zijn opgenomen in HDInsight. SSH gebruiken om te verbinden met het cluster, en gebruik vervolgens de opdracht Hadoop voorbeeldtaken uit te voeren.
keywords: Voorbeeld van de jar van hadoop, hadoop voorbeelden jar, hadoop mapreduce-voorbeelden, mapreduce-voorbeelden
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 37ba412d9463ccf4cdd18c842910c3c6f5349ef2
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007091"
---
# <a name="run-the-mapreduce-examples-included-in-hdinsight"></a>De opgenomen in HDInsight MapReduce-voorbeelden uitvoeren

[!INCLUDE [samples-selector](../../../includes/hdinsight-run-samples-selector.md)]

Informatie over het uitvoeren van de MapReduce-voorbeelden opgenomen met Hadoop op HDInsight.

## <a name="prerequisites"></a>Vereisten

* **Een HDInsight-cluster**: Zie [aan de slag met Hadoop met Hive in HDInsight op Linux](apache-hadoop-linux-tutorial-get-started.md)

    > [!IMPORTANT]
    > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* **Een SSH-client**: Zie voor meer informatie, [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="the-mapreduce-examples"></a>De MapReduce-voorbeelden

**Locatie**: de voorbeelden bevinden zich op het HDInsight-cluster op `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.

**Inhoud**: de volgende voorbeelden zijn opgenomen in dit archief:

* `aggregatewordcount`: Een statistische functie op basis van de mapreduce-programma dat de woorden in de invoerbestanden geteld.
* `aggregatewordhist`: Mapreduce-programma dat het histogram van de woorden in de invoerbestanden worden berekend op basis van een statistische functie.
* `bbp`: Een mapreduce-programma dat gebruikmaakt van Bailey-Borwein-Plouffe voor het berekenen van de exacte cijfers van Pi.
* `dbcount`: Een voorbeeld van de taak waarmee de paginaweergave-logboeken die zijn opgeslagen in een database wordt geteld.
* `distbbp`: Een mapreduce-programma dat gebruikmaakt van een formule BBP-type voor het berekenen van de exacte aantal bits van Pi.
* `grep`: Een mapreduce-programma waarmee de resultaten van een reguliere expressie in de invoer wordt geteld.
* `join`: Een taak die een lid worden van uitvoert gesorteerd, even gepartitioneerd gegevenssets.
* `multifilewc`: Een taak die de woorden uit verschillende bestanden geteld.
* `pentomino`: Een mapreduce tegel rangschikken programma om oplossingen voor problemen die pentomino te vinden.
* `pi`: Een mapreduce-programma dat u maakt een schatting van Pi met behulp van een quasi Monte Carlo-methode.
* `randomtextwriter`: Een mapreduce-programma dat 10 GB aan willekeurige tekstuele gegevens per knooppunt schrijft.
* `randomwriter`: Een mapreduce-programma dat 10 GB aan willekeurige gegevens per knooppunt schrijft.
* `secondarysort`: Een voorbeeld van een secundaire sorteren definiëren met de fase verminderen.
* `sort`: Een mapreduce-programma dat de gegevens die zijn geschreven door de willekeurige writer worden.
* `sudoku`: Een solver sudoku.
* `teragen`: De gegevens voor de terasort genereren.
* `terasort`: De terasort worden uitgevoerd.
* `teravalidate`: De resultaten van terasort controleren.
* `wordcount`: Een mapreduce-programma dat de woorden in de invoerbestanden geteld.
* `wordmean`: Een mapreduce-programma dat de gemiddelde lengte van de woorden in de invoerbestanden telt.
* `wordmedian`: Een mapreduce-programma dat de gemiddelde duur van de woorden in de invoerbestanden telt.
* `wordstandarddeviation`: Een mapreduce-programma waarmee de standaardafwijking van de lengte van de woorden in de invoerbestanden wordt geteld.

**Broncode**: broncode voor deze voorbeelden is opgenomen in het HDInsight-cluster op `/usr/hdp/current/hadoop-client/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.

## <a name="run-the-wordcount-example"></a>Het wordcount-voorbeeld uitvoeren

1. Verbinding maken met HDInsight met behulp van SSH. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Uit de `username@#######:~$` vragen, gebruikt u de volgende opdracht uit om de voorbeelden weer te geven:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    Met deze opdracht wordt de lijst met voorbeeld gegenereerd uit de vorige sectie van dit document.

3. Gebruik de volgende opdracht om hulp te krijgen op een specifiek voorbeeld. In dit geval de **wordcount** voorbeeld:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    Het volgende foutbericht wordt weergegeven:

        Usage: wordcount <in> [<in>...] <out>

    Dit bericht geeft aan dat u verschillende invoerpaden voor de brondocumenten bieden kan. Het uiteindelijke pad is waar de uitvoer (aantal woorden in de brondocumenten) worden opgeslagen.

4. Gebruik de volgende voor het tellen van alle woorden in de laptops van Leonardo Da Vinci, die alleen als van voorbeeldgegevens met uw cluster dienen:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    Invoer voor deze taak worden gelezen uit de `/example/data/gutenberg/davinci.txt`. Uitvoer voor dit voorbeeld wordt opgeslagen in `/example/data/davinciwordcount`. Beide paden bevinden zich in de standaardopslag voor het cluster, niet het lokale bestandssysteem.

   > [!NOTE]
   > Zoals aangegeven in de help voor het wordcount-voorbeeld, kunt u ook meerdere invoerbestanden opgeven. Bijvoorbeeld, `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` woorden in zowel davinci.txt en ulysses.txt zou tellen.

5. Zodra de taak is voltooid, gebruikt u de volgende opdracht uit om de uitvoer weer te geven:

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    Met deze opdracht worden samengevoegd in alle de uitvoerbestanden die worden geproduceerd door de taak. De uitvoer wordt weergegeven in de console. De uitvoer lijkt op het volgende:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Elke regel vertegenwoordigt een woord en het aantal keren dat het probleem is opgetreden in de ingevoerde gegevens.

## <a name="the-sudoku-example"></a>Het voorbeeld Sudoku

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) is een logische puzzel opgebouwd uit negen 3 x 3 rasters. Aantal cellen in het raster zijn getallen, terwijl anderen leeg zijn en het doel is om op te lossen voor de lege cellen. De vorige koppeling meer informatie over de puzzel heeft, maar het doel van dit voorbeeld is om op te lossen voor de lege cellen. De invoer moet dus een bestand dat zich in de volgende indeling:

* Negen rijen van de negen kolommen
* Elke kolom kan bevatten een getal of `?` (wat aangeeft dat het een lege cel)
* Cellen worden gescheiden door een spatie

Er is een bepaalde manier om samen te stellen Sudoku puzzels; een getal in een kolom of rij kan niet worden herhaald. Er is een voorbeeld van het HDInsight-cluster die correct is samengesteld. Deze bevindt zich op `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` en bevat de volgende tekst:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

Als u wilt dit probleem voorbeeld doorloopt u het voorbeeld Sudoku, gebruik de volgende opdracht:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

De resultaten worden weergegeven die vergelijkbaar is met de volgende tekst:

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

In het pi-voorbeeld wordt een statistische (quasi Monte Carlo) methode voor het schatten van de waarde van pi. Punten worden in willekeurige volgorde geplaatst in een vierkant eenheid. Het vierkant bevat ook een cirkel. De kans dat de punten vallen binnen de cirkel gelijk zijn aan het gebied van de cirkel, pi/4. De waarde van pi kan worden geschat van de waarde van 4R. R is de verhouding van het aantal punten die in de cirkel in op het totale aantal punten die zich binnen het vierkant. Hoe groter het voorbeeld van de punten die wordt gebruikt, hoe beter de schatting wordt weergegeven.

Gebruik de volgende opdracht om uit te voeren in dit voorbeeld. Met deze opdracht maakt gebruik van 16 kaarten met 10.000.000 voorbeelden om te schatten van de waarde van pi:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

De waarde die wordt geretourneerd door deze opdracht is vergelijkbaar met **3.14159155000000000000**. Voor verwijzingen naar zijn de eerste 10 decimalen van pi 3.1415926535.

## <a name="10-gb-greysort-example"></a>Voorbeeld van 10 GB Greysort

GraySort is een benchmark-sorteren. De metrische gegevens is de snelheid met sorteren (TB per minuut) die tijdens het sorteren van grote hoeveelheden gegevens, meestal een 100 TB minimale is bereikt.

In dit voorbeeld maakt gebruik van een gemiddelde hoeveelheden 10 GB aan gegevens zodat deze kan relatief snel kan worden uitgevoerd. De MapReduce-toepassingen die zijn ontwikkeld door Owen O'Malley en Arun Murthy wordt gebruikt. Deze toepassingen gewonnen de jaarlijkse voor algemeen gebruik ("daytona") terabyte sorteren benchmark in 2009, met een snelheid van 0.578 TB/min (100 TB in 173 minuten). Zie voor meer informatie over deze en andere sorteren benchmarks de [Sortbenchmark](http://sortbenchmark.org/) site.

In dit voorbeeld maakt gebruik van drie sets MapReduce-programma's:

* **TeraGen**: een MapReduce-programma dat wordt gegenereerd rijen met gegevens om te sorteren

* **TeraSort**: voorbeelden van de ingevoerde gegevens en MapReduce gebruikt voor het sorteren van de gegevens in een volgorde van de totale

    TeraSort is een standaard MapReduce sorteren, met uitzondering van een aangepaste partitioner. De partitioner maakt gebruik van een gesorteerde lijst met sleutels voor N-1 steekproef die het belangrijkste bereik voor elke verminderen definiëren. In het bijzonder, alle sleutels dergelijke die voorbeeld [i-1] < sleutel = < voorbeeld [i] om te beperken i worden verzonden. Deze partitioner garanties dat de uitvoer van i beperken zijn alle kleiner is dan de uitvoer van verminderen i + 1.

* **TeraValidate**: een MapReduce-programma die valideert dat de uitvoer wereldwijd is gesorteerd

    Een kaart per bestand in de uitvoermap wordt gemaakt en elke kaart zorgt ervoor dat elke sleutel kleiner dan of gelijk zijn aan het vorige voorbeeld is. De functie kaart genereert records van de eerste en laatste sleutels van elk bestand. De functie verminderen zorgt ervoor dat de eerste sleutel van i-bestand groter dan de laatste sleutel van het bestand i-1 is. Geen problemen worden gerapporteerd als uitvoer van de fase verminderen met de sleutels die niet de juiste volgorde zijn.

Gebruik de volgende stappen voor het genereren van gegevens, sorteren en controleer vervolgens of de uitvoer:

1. Genereren van 10 GB aan gegevens, die is opgeslagen in standaardopslag voor het HDInsight-cluster op `/example/data/10GB-sort-input`:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    De `-Dmapred.map.tasks` Hadoop aangeeft hoeveel kaart taken moet worden gebruikt voor deze taak. De laatste twee parameters geven de taak te maken van 10 GB aan gegevens en bewaar deze op `/example/data/10GB-sort-input`.

2. Gebruik de volgende opdracht om de gegevens te sorteren:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    De `-Dmapred.reduce.tasks` Hadoop geeft aan hoeveel aantal taken te verkleinen om te gebruiken voor de taak. De laatste twee parameters zijn alleen de invoer- en -locaties voor gegevens.

3. Gebruik de volgende voor het valideren van de gegevens die worden gegenereerd door de sortering:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u kunt de voorbeelden met de HDInsight op basis van Linux-clusters uitvoeren. Zie de volgende onderwerpen voor zelfstudies over Pig, Hive en MapReduce gebruiken met HDInsight:

* [Pig gebruiken met Hadoop op HDInsight](hdinsight-use-pig.md)
* [Hive gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-introduction]:apache-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

