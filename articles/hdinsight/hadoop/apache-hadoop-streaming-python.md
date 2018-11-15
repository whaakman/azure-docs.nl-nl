---
title: Python MapReduce-taken met HDInsight - Azure streaming ontwikkelen
description: Informatie over het gebruik van Python in de streaming-MapReduce-taken. Apache Hadoop biedt een streaming-API voor MapReduce voor het schrijven van in een andere taal dan Java.
services: hdinsight
keyword: mapreduce python,python map reduce,python mapreduce
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: hrasheed
ms.openlocfilehash: d6a9de293d62ec6f25bd3a665d5ced5a1ac671ae
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634019"
---
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a>Python MapReduce-programma's voor HDInsight streaming ontwikkelen

Informatie over het gebruik van Python in de streaming-MapReduce-bewerkingen. Apache Hadoop biedt een streaming-API voor MapReduce waarmee u kaart schrijven en functies in een andere taal dan Java verlagen. De stappen in dit document voor het implementeren van de kaart en verminder onderdelen in Python.

## <a name="prerequisites"></a>Vereisten

* Een Linux-gebaseerde Apache Hadoop op HDInsight-cluster

  > [!IMPORTANT]
  > Voor de stappen in dit document hebt u een HDInsight-cluster nodig dat werkt met Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een teksteditor

  > [!IMPORTANT]
  > De teksteditor moet LF gebruiken als het einde van regel. Gebruik een einde van regel van CRLF zorgt ervoor dat fouten bij het uitvoeren van de MapReduce-taak op Linux gebaseerde HDInsight-clusters.

* De `ssh` en `scp` opdrachten of [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)

## <a name="word-count"></a>Aantal woorden

In dit voorbeeld wordt een aantal eenvoudige woorden die zijn geïmplementeerd in een python een-toewijzing en reducer. Het toewijzen van de zinnen opgesplitst in afzonderlijke woorden en de reducer combineert de woorden en telt als u wilt de uitvoer produceren.

Het volgende stroomdiagram ziet u wat er gebeurt tijdens de kaart en fasen verminderen.

![afbeelding van de mapreduce-proces](./media/apache-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a>Streaming MapReduce

Hadoop kunt u om een bestand opgeeft dat de kaart bevat en logica die wordt gebruikt door een taak te verminderen. De specifieke vereisten voor de kaart en beperken van logica zijn:

* **Invoer**: de kaart en verminder onderdelen invoergegevens uit STDIN moeten lezen.
* **Uitvoer**: de kaart en verminder onderdelen uitvoergegevens naar STDOUT moeten schrijven.
* **Gegevensindeling**: de gegevens verwerkt en geproduceerd moet een sleutel/waarde-paar, gescheiden door een tab.

Python eenvoudig deze vereisten kan verwerken met behulp van de `sys` module lezen uit STDIN en het gebruik van `print` af te drukken op STDOUT. De resterende taak is gewoon de gegevens met een tabblad Opmaak (`\t`) tekens tussen de sleutel en waarde.

## <a name="create-the-mapper-and-reducer"></a>Maak de toewijzing en reducer

1. Maak een bestand met de naam `mapper.py` en gebruik de volgende code als de inhoud:

   ```python
   #!/usr/bin/env python

   # Use the sys module
   import sys

   # 'file' in this case is STDIN
   def read_input(file):
       # Split each line into words
       for line in file:
           yield line.split()

   def main(separator='\t'):
       # Read the data using read_input
       data = read_input(sys.stdin)
       # Process each word returned from read_input
       for words in data:
           # Process each word
           for word in words:
               # Write to STDOUT
               print '%s%s%d' % (word, separator, 1)

   if __name__ == "__main__":
       main()
   ```

2. Maak een bestand met de naam **reducer.py** en gebruik de volgende code als de inhoud:

   ```python
   #!/usr/bin/env python

   # import modules
   from itertools import groupby
   from operator import itemgetter
   import sys

   # 'file' in this case is STDIN
   def read_mapper_output(file, separator='\t'):
       # Go through each line
       for line in file:
           # Strip out the separator character
           yield line.rstrip().split(separator, 1)

   def main(separator='\t'):
       # Read the data using read_mapper_output
       data = read_mapper_output(sys.stdin, separator=separator)
       # Group words and counts into 'group'
       #   Since MapReduce is a distributed process, each word
       #   may have multiple counts. 'group' will have all counts
       #   which can be retrieved using the word as the key.
       for current_word, group in groupby(data, itemgetter(0)):
           try:
               # For each word, pull the count(s) for the word
               #   from 'group' and create a total count
               total_count = sum(int(count) for current_word, count in group)
               # Write to stdout
               print "%s%s%d" % (current_word, separator, total_count)
           except ValueError:
               # Count was not a number, so do nothing
               pass

   if __name__ == "__main__":
       main()
   ```

## <a name="run-using-powershell"></a>Uitvoeren met PowerShell

Om ervoor te zorgen dat uw bestanden rechts regeleinden, gebruikt u de volgende PowerShell-script:

[!code-powershell[main](../../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]

Gebruik de volgende PowerShell-script op de bestanden uploaden, uitvoeren van de taak en de uitvoer weergeven:

[!code-powershell[main](../../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]

## <a name="run-from-an-ssh-session"></a>Uitvoeren vanaf een SSH-sessie

1. Vanuit uw ontwikkelomgeving in dezelfde map als `mapper.py` en `reducer.py` bestanden, gebruikt u de volgende opdracht:

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    Vervang `username` met de SSH-gebruikersnaam voor uw cluster en `clustername` met de naam van uw cluster.

    Met deze opdracht kopieert de bestanden uit het lokale systeem naar het hoofdknooppunt.

    > [!NOTE]
    > Als u een wachtwoord gebruikt om uw SSH-account te beveiligen, wordt u gevraagd het wachtwoord op te geven. Als u een SSH-sleutel gebruikt, moet u mogelijk gebruiken de `-i` parameter en het pad naar de persoonlijke sleutel. Bijvoorbeeld `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

2. Verbinding maken met het cluster via SSH:

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    Zie voor meer informatie over [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

3. Om ervoor te zorgen dat de mapper.py en reducer.py hebt de juiste regeleinden, gebruik de volgende opdrachten:

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. Gebruik de volgende opdracht om de MapReduce-taak te starten.

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    Met deze opdracht heeft de volgende onderdelen:

   * **hadoop-streaming.jar**: gebruikt bij het uitvoeren van streaming MapReduce-bewerkingen. Het Hadoop-interfaces met de externe MapReduce-code die u opgeeft.

   * **-bestanden**: de opgegeven bestanden toevoegt aan de MapReduce-taak.

   * **-mapper**: Hadoop aangeeft welk bestand moet worden gebruikt als het toewijzen van de.

   * **-reducer**: Hadoop aangeeft welk bestand moet worden gebruikt als de reducer.

   * **-invoer**: het invoerbestand die we moeten tellen van woorden.

   * **-uitvoer**: de map waarin de uitvoer wordt geschreven naar.

    Als de MapReduce-taak werkt, wordt het proces weergegeven als percentage.

        02/15/05 19:01:04 INFO mapreduce. Taak: kaart 0% verminderen 0% 02/15/05 19:01:16 INFO mapreduce. Taak: kaart 100% verminderen 0% 02/15/05 19:01:27 INFO mapreduce. Taak: kaart 100% verminderen 100%


5. Als u de uitvoer, gebruik de volgende opdracht:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Met deze opdracht geeft een lijst met woorden en het aantal keren dat het woord is opgetreden.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe streamingtaken MapRedcue gebruiken met HDInsight, gebruikt u de volgende koppelingen op andere manieren om te werken met Azure HDInsight.

* [Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce-taken gebruiken met HDInsight](hdinsight-use-mapreduce.md)
