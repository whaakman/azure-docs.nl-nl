---
title: MapReduce en extern bureaublad met Apache Hadoop in HDInsight - Azure
description: Informatie over het gebruik van extern bureaublad verbinding maken met Apache Hadoop op HDInsight en MapReduce-taken worden uitgevoerd.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 43daae3f3cb8a2b95413983464e41a69e69fcf36
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54120560"
---
# <a name="use-mapreduce-in-apache-hadoop-on-hdinsight-with-remote-desktop"></a>MapReduce gebruiken in een Apache Hadoop op HDInsight met extern bureaublad
[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

In dit artikel leert u hoe u verbinding maakt met een Apache Hadoop op HDInsight-cluster met behulp van extern bureaublad en vervolgens MapReduce-taken uitvoeren met behulp van de Hadoop-opdracht.

> [!IMPORTANT]  
> Extern bureaublad is alleen beschikbaar op Windows gebaseerde HDInsight-clusters. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.
>
> Voor HDInsight 3.4 of hoger, Zie [MapReduce gebruiken met SSH](apache-hadoop-use-mapreduce-ssh.md) voor meer informatie over verbinding maken met de HDInsight-cluster en MapReduce-taken uitvoeren.

## <a id="prereq"></a>Vereisten
Als u wilt de stappen in dit artikel hebt voltooid, moet u het volgende:

* Een cluster op basis van Windows HDInsight (Hadoop op HDInsight)
* Een clientcomputer met Windows 10, Windows 8 of Windows 7

## <a id="connect"></a>Verbinding maken met extern bureaublad
Extern bureaublad inschakelen voor het HDInsight-cluster en vervolgens verbinding mee maken door de instructies op [verbinding maken met HDInsight-clusters met behulp van RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hadoop"></a>Gebruik de opdracht Hadoop
Wanneer u met het bureaublad van de HDInsight-cluster verbonden bent, gebruikt u de volgende stappen uit in een MapReduce-taak uitvoeren met behulp van de Hadoop-opdracht:

1. Start vanaf het bureaublad HDInsight de **Hadoop-opdrachtregels**. Hiermee opent u een nieuwe opdrachtprompt in de **c:\apps\dist\hadoop-&lt;versienummer >** directory.

   > [!NOTE]  
   > Het versienummer verandert als Hadoop is bijgewerkt. De **HADOOP_HOME** omgevingsvariabele kan worden gebruikt om het pad niet vinden. Bijvoorbeeld, `cd %HADOOP_HOME%` mappen naar de map Hadoop, zonder dat u te weten het versienummer wordt gewijzigd.
   >
   >
2. Gebruik de **Hadoop** opdracht voor het uitvoeren van een voorbeeld van de MapReduce-taak, gebruikt u de volgende opdracht:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Hiermee start u de **wordcount** klasse, die is opgenomen in de **hadoop-mapreduce-examples.jar** bestand in de huidige map. Als invoer, wordt de **wasb://example/data/gutenberg/davinci.txt** document en de uitvoer wordt opgeslagen op: **wasb: / / / voorbeeld/data/WordCountOutput**.

   > [!NOTE]
   > Zie voor meer informatie over deze MapReduce-taak en de voorbeeldgegevens <a href="hdinsight-use-mapreduce.md">MapReduce gebruiken in HDInsight Hadoop</a>.
   >
   >
3. De taak verzendt gegevens zoals deze wordt verwerkt, en gegevens worden geretourneerd met de volgende strekking wanneer de taak voltooid is:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. Wanneer de taak voltooid is, gebruikt u de volgende opdracht uit om de Data-at-uitvoerbestanden weer te geven **wasb://example/data/WordCountOutput**:

        hadoop fs -ls wasb:///example/data/WordCountOutput

    Twee bestanden weergegeven **_SUCCESS** en **onderdeel-r-00000**. De **onderdeel-r-00000** bestand bevat de uitvoer voor deze taak.

   > [!NOTE]
   > Bepaalde MapReduce-taken kunnen de resultaten splitsen over meerdere **onderdeel-r-###** bestanden. Als dit het geval is, gebruikt u de ### achtervoegsel om aan te geven van de volgorde van de bestanden.
   >
   >
5. Als u de uitvoer, gebruik de volgende opdracht:

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    Dit geeft een lijst van de woorden die zijn opgenomen in de **wasb://example/data/gutenberg/davinci.txt** bestand, samen met het aantal malen dat elk woord heeft plaatsgevonden. Hier volgt een voorbeeld van de gegevens die worden opgenomen in het bestand:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Samenvatting
Zoals u ziet, biedt de Hadoop-opdracht een eenvoudige manier MapReduce-taken uitvoeren op een HDInsight-cluster en bekijk vervolgens de taakuitvoer.

## <a id="nextsteps"></a>Volgende stappen
Voor algemene informatie over MapReduce-taken in HDInsight:

* [MapReduce gebruiken met HDInsight Hadoop](hdinsight-use-mapreduce.md)

Voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)
* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)
