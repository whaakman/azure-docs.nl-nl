---
title: Streaming-MapReduce-taken met HDInsight - Azure Python ontwikkelen | Microsoft Docs
description: Informatie over het gebruik van Python in streaming MapReduce-taken. Hadoop biedt een streaming-API voor MapReduce voor het schrijven in andere talen dan Java.
services: hdinsight
keyword: mapreduce python,python map reduce,python mapreduce
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7631d8d9-98ae-42ec-b9ec-ee3cf7e57fb3
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: b86605c49291a99f49c4b2841d46324cfd0db56d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a>Python streaming MapReduce-programma's voor HDInsight ontwikkelen

Informatie over het gebruik van Python in streaming MapReduce-bewerkingen. Hadoop biedt een streaming-API voor MapReduce waarmee u kunt schrijven kaart en functies in andere talen dan Java te verminderen. De stappen in dit document implementeren van de kaart en onderdelen in Python verminderen.

## <a name="prerequisites"></a>Vereisten

* Een op Linux gebaseerde Hadoop op HDInsight-cluster

  > [!IMPORTANT]
  > De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een teksteditor

  > [!IMPORTANT]
  > De teksteditor moet LF gebruiken als het beëindigen van de regel. Met behulp van een regel beëindigen van CRLF zorgt ervoor dat fouten bij het uitvoeren van de MapReduce-taak op Linux gebaseerde HDInsight-clusters.

* De `ssh` en `scp` opdrachten, of [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)

## <a name="word-count"></a>Aantal woorden

In dit voorbeeld is een aantal eenvoudige woorden die zijn geïmplementeerd in een python een toewijzen en reducer. De mapper zinnen opgesplitst in afzonderlijke woorden en de reducer de woorden samenvoegt en telt de uitvoer te produceren.

Het volgende stroomdiagram ziet u wat er gebeurt tijdens de kaart en fasen te verminderen.

![afbeelding van het mapreduce-proces](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a>Streaming MapReduce

Hadoop kunt u een bestand opgeeft dat de kaart bevat en logica die wordt gebruikt door een taak te verminderen. De specifieke vereisten voor de kaart en logica te verminderen zijn:

* **Invoer**: de kaart en verminderen onderdelen invoergegevens van STDIN moeten lezen.
* **Uitvoer**: de kaart en de onderdelen moeten uitvoergegevens naar STDOUT schrijven.
* **Gegevensindeling**: de gegevens die worden gebruikt en die wordt geproduceerd moet een sleutel-waardepaar, gescheiden door een tab-teken.

Python eenvoudig deze vereisten kan verwerken met behulp van de `sys` module die u wilt lezen uit STDIN en het gebruik van `print` af te drukken op STDOUT. De resterende taak is gewoon de gegevens op een tabblad formatteren (`\t`) tussen de sleutel en waarde teken.

## <a name="create-the-mapper-and-reducer"></a>De toewijzen en reducer maken

1. Maak een bestand met de naam `mapper.py` en de volgende code gebruiken als de inhoud:

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

2. Maak een bestand met de naam **reducer.py** en de volgende code gebruiken als de inhoud:

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

[!code-powershell[belangrijkste](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]

Gebruik de volgende PowerShell-script de bestanden uploaden, voer de taak en de uitvoer weergeven:

[!code-powershell[belangrijkste](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]

## <a name="run-from-an-ssh-session"></a>Uitvoeren van een SSH-sessie

1. Van uw ontwikkelomgeving in dezelfde map als `mapper.py` en `reducer.py` bestanden, gebruik de volgende opdracht:

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    Vervang `username` met de SSH-gebruikersnaam voor uw cluster en `clustername` met de naam van het cluster.

    Met deze opdracht kopieert de bestanden van het lokale systeem met het hoofdknooppunt.

    > [!NOTE]
    > Als u een wachtwoord gebruikt om uw SSH-account te beveiligen, wordt u gevraagd het wachtwoord op te geven. Als u een SSH-sleutel gebruikt, moet u mogelijk gebruiken de `-i` parameter en het pad naar de persoonlijke sleutel. Bijvoorbeeld `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

2. Verbinding maken met het cluster via SSH:

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    Zie voor meer informatie over [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

3. De mapper.py en reducer.py hebt de juiste regeleinden, gebruik de volgende opdrachten:

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. Gebruik de volgende opdracht de MapReduce-taak starten.

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    Deze opdracht heeft de volgende onderdelen:

   * **hadoop-streaming.jar**: gebruikt bij het uitvoeren van streaming MapReduce-bewerkingen. Het Hadoop-interfaces met de externe MapReduce-code die u opgeeft.

   * **-bestanden**: de opgegeven bestanden toevoegt aan de MapReduce-taak.

   * **-toewijzer**: vertelt Hadoop welk bestand moet worden gebruikt als de toewijzen.

   * **-reducer**: vertelt Hadoop welk bestand moet worden gebruikt als de reducer.

   * **-invoer**: het bestand voor invoer die moeten worden geteld woorden uit.

   * **-uitvoer**: de map die de uitvoer naar worden geschreven.

    Als de MapReduce-taak werkt, wordt het proces weergegeven als percentage.

        15-02-05 19:01:04 INFO mapreduce. Taak: kaart 0% verminderen 0% 15-02-05 19:01:16 INFO mapreduce. Taak: kaart 100% verminderen 0% 15-02-05 19:01:27 INFO mapreduce. Taak: kaart 100% verminderen 100%


5. Gebruik de volgende opdracht om de weergave van de uitvoer:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Met deze opdracht geeft een lijst met woorden en hoe vaak het woord is opgetreden.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u streaming MapRedcue taken gebruiken met HDInsight, gebruik de volgende koppelingen om te verkennen andere manieren om te werken met Azure HDInsight.

* [Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce-taken gebruiken met HDInsight](hdinsight-use-mapreduce.md)
