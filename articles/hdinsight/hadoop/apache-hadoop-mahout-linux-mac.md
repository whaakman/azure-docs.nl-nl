---
title: Genereren van aanbevelingen met behulp van Apache Mahout en HDInsight (SSH) - Azure
description: Informatie over het gebruik van de Apache Mahout-machine learning-bibliotheek Genereer filmaanbevelingen met HDInsight (Hadoop).
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.openlocfilehash: 1025b004eb49f82532d3815be3536dbc1f336273
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634614"
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-apache-hadoop-in-hdinsight-ssh"></a>Filmaanbevelingen genereren met behulp van Apache Mahout met Apache Hadoop op basis van Linux in HDInsight (SSH)

[!INCLUDE [mahout-selector](../../../includes/hdinsight-selector-mahout.md)]

Meer informatie over het gebruik van de [Apache Mahout](http://mahout.apache.org) machine learning-bibliotheek met Azure HDInsight filmaanbevelingen genereren.

Mahout is een [machine learning] [ ml] -bibliotheek voor Apache Hadoop. Mahout bevat algoritmes voor het verwerken van gegevens, zoals filteren, classificeren en clusteren. In dit artikel gebruikt u een engine voor aanbevelingen voor het genereren van filmaanbevelingen die zijn gebaseerd op films die je vrienden hebt gezien.

## <a name="prerequisites"></a>Vereisten

* Een Linux gebaseerde HDInsight-cluster. Zie voor meer informatie over het maken van een [aan de slag met Hadoop op basis van Linux in HDInsight][getstarted].

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een SSH-client. Zie het document [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

## <a name="mahout-versioning"></a>Mahout-versiebeheer

Zie voor meer informatie over de versie van Mahout in HDInsight [HDInsight-versies en Hadoop-onderdelen](../hdinsight-component-versioning.md).

## <a name="recommendations"></a>Informatie over aanbevelingen

Een van de functies die wordt geleverd door Mahout is een engine voor aanbevelingen. Deze engine accepteert gegevens in de indeling van `userID`, `itemId`, en `prefValue` (de voorkeur voor het item). Mahout vervolgens uitgevoerd voor analyse van CO-exemplaar om te bepalen: *gebruikers die een voorkeur voor een item hebben ook een voorkeur voor deze andere items hebt*. Mahout bepaalt vervolgens gebruikers met een dergelijke-item voorkeuren, die kunnen worden gebruikt om aanbevelingen te doen.

De volgende werkstroom is een vereenvoudigd voorbeeld dat gebruikmaakt van filmgegevens:

* **CO exemplaar**: Jaap Els en Bob alle beviel *Star Wars*, *terug ramp in de Empire*, en *rendement van de Jedi*. Mahout bepaalt dat gebruikers die ook een van deze films, zoals de andere twee.

* **CO exemplaar**: Bob en Els ook beviel *de Phantom Menace*, *een aanval van het klonen*, en *Revenge van de Sith*. Mahout bepaalt dat gebruikers die de vorige drie films ook leuk vinden, zoals deze drie films.

* **Gelijkenis aanbeveling**: omdat Jaap leuk vinden van de eerste drie films, Mahout kijkt naar films die anderen met vergelijkbare voorkeuren leuk vinden, maar Jaap niet heeft bekeken (leuk vinden/geclassificeerd). In dit geval Mahout raadt *de Phantom Menace*, *een aanval van het klonen*, en *Revenge van de Sith*.

### <a name="understanding-the-data"></a>Wat zijn de gegevens?

Eenvoudig [GroupLens onderzoek] [ movielens] classificatie gegevens biedt voor films in een indeling die compatibel is met Mahout. Deze gegevens zijn beschikbaar op de standaardopslag van uw cluster op `/HdiSamples/HdiSamples/MahoutMovieData`.

Er zijn twee bestanden `moviedb.txt` en `user-ratings.txt`. De `user-ratings.txt` -bestand wordt gebruikt tijdens de analyse. De `moviedb.txt` wordt gebruikt voor beschrijvende tekst wanneer de resultaten weer te geven.

De gegevens in gebruiker ratings.txt heeft een structuur van `userID`, `movieID`, `userRating`, en `timestamp`, waarmee wordt aangegeven hoe maximaal elke gebruiker een film beoordeeld. Hier volgt een voorbeeld van de gegevens:

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

## <a name="run-the-analysis"></a>De analyse uitvoeren

Gebruik de volgende opdracht om uit te voeren van de taak aanbeveling van een SSH-verbinding met het cluster:

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]
> De taak duurt enkele minuten, en meerdere MapReduce-taken kunnen uitvoeren.

## <a name="view-the-output"></a>De uitvoer weergeven

1. Zodra de taak is voltooid, gebruikt u de volgende opdracht uit om de gegenereerde uitvoer weer te geven:

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    De uitvoer ziet er als volgt uit:

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    De eerste kolom is de `userID`. De waarden die zijn opgenomen in ' [' en ']' zijn `movieId`:`recommendationScore`.

2. U kunt de uitvoer, samen met de moviedb.txt bevatten meer informatie over de aanbevelingen. Kopieer eerst de bestanden lokaal via de volgende opdrachten:

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    Met deze opdracht wordt de uitvoergegevens die worden gekopieerd naar een bestand met de naam **recommendations.txt** in de huidige map, samen met de gegevensbestanden van de film.

3. Gebruik de volgende opdracht om te maken van een Python-script waarmee film-namen voor de gegevens in de uitvoer van de aanbevelingen worden opgezocht:

    ```bash
    nano show_recommendations.py
    ```

    Wanneer de editor wordt geopend, gebruikt u de volgende tekst als de inhoud van het bestand:

   ```python
   #!/usr/bin/env python

   import sys

   if len(sys.argv) != 5:
        print "Arguments: userId userDataFilename movieFilename recommendationFilename"
        sys.exit(1)

   userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

   print "Reading Movies Descriptions"
   movieFile = open(movieFilename)
   movieById = {}
   for line in movieFile:
       tokens = line.split("|")
       movieById[tokens[0]] = tokens[1:]
   movieFile.close()

   print "Reading Rated Movies"
   userDataFile = open(userDataFilename)
   ratedMovieIds = []
   for line in userDataFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           ratedMovieIds.append((tokens[1],tokens[2]))
   userDataFile.close()

   print "Reading Recommendations"
   recommendationFile = open(recommendationFilename)
   recommendations = []
   for line in recommendationFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           movieIdAndScores = tokens[1].strip("[]\n").split(",")
           recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
           break
   recommendationFile.close()

   print "Rated Movies"
   print "------------------------"
   for movieId, rating in ratedMovieIds:
       print "%s, rating=%s" % (movieById[movieId][0], rating)
   print "------------------------"

   print "Recommended Movies"
   print "------------------------"
   for movieId, score in recommendations:
       print "%s, score=%s" % (movieById[movieId][0], score)
   print "------------------------"
   ```

    Druk op **Ctrl X**, **Y**, en tot slot **Enter** de gegevens op te slaan.

4. Het Python-script uitvoeren. De volgende opdracht wordt ervan uitgegaan dat u zich in de map waar alle bestanden zijn gedownload:

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    Met deze opdracht wordt de aanbevelingen die worden gegenereerd voor de gebruiker-ID 4 gekeken.

    * De **gebruiker ratings.txt** bestand wordt gebruikt om op te halen van films die zijn geclassificeerd.

    * De **moviedb.txt** bestand wordt gebruikt om op te halen van de namen van de films.

    * De **recommendations.txt** wordt gebruikt voor het ophalen van de filmaanbevelingen voor deze gebruiker.

     De uitvoer van deze opdracht is vergelijkbaar met de volgende tekst:

        Zeven jaar in Tibet (1997), score = 5.0 Indiana Jones en de laatste Crusade (1989), score = 5.0 Jaws (1975), score = 5.0 idee en mee (1995), score = 5.0 onafhankelijkheid dag (ID4) (1996), score = 5.0 mijn beste vriend Wedding (1997), score = 5.0 Jerry Maguire (1996), score = 5.0 Scream 2 (1997), score = 5.0 tijd Kill, een (1996), score = 5.0

## <a name="delete-temporary-data"></a>Tijdelijke gegevens verwijderen

Mahout taken verwijderen tijdelijke gegevens die zijn gemaakt tijdens het verwerken van de taak niet. De `--tempDir` parameter is opgegeven in het voorbeeld van de taak voor het isoleren van de tijdelijke bestanden naar een specifiek pad voor het eenvoudig verwijderen. Als u wilt de tijdelijke bestanden verwijderen, gebruik de volgende opdracht:

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]
> Als u wilt dat de opdracht opnieuw uitvoeren, moet u ook de uitvoermap verwijderen. Gebruik de volgende om deze map te verwijderen:
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een Mahout, Ontdek andere manieren van het werken met gegevens in HDInsight:

* [Hive met HDInsight](hdinsight-use-hive.md)
* [Pig met HDInsight](hdinsight-use-pig.md)
* [MapReduce met HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]:apache-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
