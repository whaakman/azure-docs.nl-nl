---
title: MapReduce en SSH-verbinding met Apache Hadoop in HDInsight - Azure
description: Informatie over het gebruik van SSH met behulp van Apache Hadoop op HDInsight MapReduce-taken uit te voeren.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: hrasheed
ms.openlocfilehash: bce79aed49b94071b4e83524de2d599fca182256
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53438562"
---
# <a name="use-mapreduce-with-apache-hadoop-on-hdinsight-with-ssh"></a>MapReduce gebruiken met Apache Hadoop op HDInsight met SSH

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Leer hoe u MapReduce-taken verzenden via een Secure Shell (SSH)-verbinding naar HDInsight.

> [!NOTE]
> Als u al bekend bent met behulp van Apache Hadoop op basis van Linux-servers, maar u niet bekend bent met HDInsight, raadpleegt u [HDInsight op basis van Linux tips](../hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Vereisten

* Een cluster op basis van Linux HDInsight (Hadoop op HDInsight)

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een SSH-client. Zie voor meer informatie, [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md)

## <a id="ssh"></a>Verbinding maken met SSH

Verbinding maken met het cluster via SSH. Bijvoorbeeld, de volgende opdracht maakt verbinding met een cluster met de naam **myhdinsight** als de **sshuser** account:

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

**Als u een certificaatsleutel voor SSH-verificatie**, moet u mogelijk de locatie van de persoonlijke sleutel bijvoorbeeld op het clientsysteem opgeven:

```bash
ssh -i ~/mykey.key sshuser@myhdinsight-ssh.azurehdinsight.net
```

**Als u een wachtwoord voor SSH-verificatie**, moet u het wachtwoord wanneer hierom wordt gevraagd.

Zie voor meer informatie over het gebruik van SSH met HDInsight [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="hadoop"></a>Hadoop-opdrachten gebruiken

1. Nadat u met het HDInsight-cluster verbonden bent, gebruikt u de volgende opdracht uit om een MapReduce-taak te starten:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    Met deze opdracht start de `wordcount` klasse, die is opgenomen in de `hadoop-mapreduce-examples.jar` bestand. Hierbij de `/example/data/gutenberg/davinci.txt` document als invoer en uitvoer wordt opgeslagen op de `/example/data/WordCountOutput`.

    > [!NOTE]
    > Zie voor meer informatie over deze MapReduce-taak en de voorbeeldgegevens [MapReduce gebruiken in een Apache Hadoop op HDInsight](hdinsight-use-mapreduce.md).

2. De taak verzendt de gegevens worden verwerkt, en gegevens worden geretourneerd die vergelijkbaar is met de volgende tekst wanneer de taak is voltooid:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Wanneer de taak is voltooid, gebruikt u de volgende opdracht uit om de uitvoerbestanden weer te geven:

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    Met deze opdracht u twee bestanden, `_SUCCESS` en `part-r-00000`. De `part-r-00000` -bestand bevat de uitvoer voor deze taak.

    > [!NOTE]  
    > Bepaalde MapReduce-taken kunnen de resultaten splitsen over meerdere **onderdeel-r-###** bestanden. Als dit het geval is, gebruikt u de ### achtervoegsel om aan te geven van de volgorde van de bestanden.

4. Als u de uitvoer, gebruik de volgende opdracht:

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    Met deze opdracht geeft een lijst van de woorden die zijn opgenomen in de **wasb://example/data/gutenberg/davinci.txt** bestands- en het aantal malen dat elk woord heeft plaatsgevonden. De volgende tekst is een voorbeeld van de gegevens die zijn opgenomen in het bestand:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Samenvatting

U kunt zien, bieden Hadoop-opdrachten een eenvoudige manier MapReduce-taken uitvoeren in een HDInsight-cluster en bekijk vervolgens de taakuitvoer.

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce-taken in HDInsight:

* [MapReduce gebruiken met HDInsight Hadoop](hdinsight-use-mapreduce.md)

Voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)
* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)
