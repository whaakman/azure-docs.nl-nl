---
title: MapReduce en SSH-verbinding met Hadoop in HDInsight - Azure | Microsoft Docs
description: Informatie over het SSH gebruiken met Hadoop op HDInsight MapReduce-taken uitvoeren.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 844678ba-1e1f-4fda-b9ef-34df4035d547
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2018
ms.author: larryfr
ms.openlocfilehash: 08c67168cde60c1b7551806da8542c98dcb82f55
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>MapReduce gebruiken met Hadoop op HDInsight met SSH

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Informatie over het verzenden van MapReduce-taken van een Secure Shell (SSH) verbinding naar HDInsight.

> [!NOTE]
> Als u al bekend bent met Hadoop op basis van Linux-servers gebruiken, maar u niet bekend met HDInsight bent, raadpleegt u [Linux gebaseerde HDInsight tips](../hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Vereisten

* Een cluster op basis van Linux HDInsight (Hadoop in HDInsight)

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een SSH-client. Zie voor meer informatie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md)

## <a id="ssh"></a>Verbinding maken met SSH

Verbinding maken met het cluster via SSH. Bijvoorbeeld de volgende opdracht maakt verbinding met een cluster met de naam **myhdinsight**:

```bash
ssh admin@myhdinsight-ssh.azurehdinsight.net
```

**Als u een certificaatsleutel voor SSH-verificatie**, mogelijk moet u de locatie opgeven van de persoonlijke sleutel op uw clientsysteem, bijvoorbeeld:

```bash
ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net
```

**Als u een wachtwoord voor de SSH-verificatie**, moet u het wachtwoord als u wordt gevraagd.

Zie voor meer informatie over het gebruik van SSH met HDInsight [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="hadoop"></a>Hadoop-opdrachten gebruiken

1. Nadat u met het HDInsight-cluster verbonden bent, gebruikt u de volgende opdracht een MapReduce-taak starten:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    Deze opdracht start de `wordcount` klasse, die is opgenomen in de `hadoop-mapreduce-examples.jar` bestand. Dit maakt gebruik van de `/example/data/gutenberg/davinci.txt` document als invoer en uitvoer is opgeslagen op `/example/data/WordCountOutput`.

    > [!NOTE]
    > Zie voor meer informatie over deze MapReduce-taak en de bijvoorbeeld gegevens [gebruik MapReduce in Hadoop op HDInsight](hdinsight-use-mapreduce.md).

2. De taak verzendt gegevens worden verwerkt en gegevens worden geretourneerd vergelijkbaar met de volgende tekst wanneer de taak is voltooid:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Wanneer de taak is voltooid, moet u de volgende opdracht gebruiken voor het weergeven van de uitvoerbestanden:

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    Deze opdracht weer twee bestanden: `_SUCCESS` en `part-r-00000`. De `part-r-00000` -bestand bevat de uitvoer voor deze taak.

    > [!NOTE]
    > Bepaalde MapReduce-taken kunnen de resultaten worden gesplitst over meerdere **onderdeel-r-###** bestanden. Als dit het geval is, gebruiken de ### achtervoegsel om aan te geven van de volgorde van de bestanden.

4. Gebruik de volgende opdracht om de weergave van de uitvoer:

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    Deze opdracht geeft een lijst van de woorden die zijn opgenomen in de **wasb://example/data/gutenberg/davinci.txt** bestands- en het aantal keren dat elk woord is opgetreden. De volgende tekst is een voorbeeld van de gegevens die zijn opgenomen in het bestand:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Samenvatting

Zoals u ziet, zijn opdrachten Hadoop een gemakkelijke manier MapReduce-taken uitvoeren in een HDInsight-cluster en bekijk vervolgens de taakuitvoer van de.

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce-taken in HDInsight:

* [Gebruik MapReduce op HDInsight Hadoop](hdinsight-use-mapreduce.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)
