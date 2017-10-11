---
title: MapReduce en extern bureaublad met Hadoop in HDInsight - Azure | Microsoft Docs
description: Informatie over het gebruik van extern bureaublad verbinding maken met Hadoop op HDInsight en MapReduce-taken uitvoeren.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9d3a7b34-7def-4c2e-bb6c-52682d30dee8
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: b56674857b013f9bb3d4dd4b6e97b34e0a97b1b2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>MapReduce in Hadoop in HDInsight met extern bureaublad gebruiken
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In dit artikel leert u hoe u verbinding met een Hadoop op HDInsight-cluster met behulp van extern bureaublad en voer vervolgens MapReduce-taken via de Hadoop-opdracht.

> [!IMPORTANT]
> Extern bureaublad is alleen beschikbaar op Windows gebaseerde HDInsight-clusters. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.
>
> Voor HDInsight 3.4 of hoger, Zie [MapReduce gebruikt met SSH](hdinsight-hadoop-use-mapreduce-ssh.md) voor meer informatie over verbinding maken met het HDInsight-cluster en MapReduce-taken uitvoeren.

## <a id="prereq"></a>Vereisten
Voor het voltooien van de stappen in dit artikel, moet u het volgende:

* Een cluster op basis van Windows HDInsight (Hadoop in HDInsight)
* Een clientcomputer met Windows 10, Windows 8 of Windows 7

## <a id="connect"></a>Verbinding maken met extern bureaublad
Extern bureaublad inschakelen voor het HDInsight-cluster en vervolgens verbinding maken met het door de instructies op [verbinding maken met HDInsight-clusters met RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hadoop"></a>Gebruik de opdracht Hadoop
Wanneer u met het bureaublad van het HDInsight-cluster verbonden bent, gebruikt u de volgende stappen uit een MapReduce-taak uitvoeren met behulp van de Hadoop-opdracht:

1. Start vanaf het bureaublad HDInsight de **Hadoop-opdrachtregel**. Hiermee opent u een nieuwe opdrachtprompt in de **c:\apps\dist\hadoop-&lt;versienummer >** directory.

   > [!NOTE]
   > Het versienummer verandert als Hadoop wordt bijgewerkt. De **HADOOP_HOME** omgevingsvariabele kan worden gebruikt om het pad niet vinden. Bijvoorbeeld: `cd %HADOOP_HOME%` mappen naar de map Hadoop, zonder u te weten het versienummer wordt gewijzigd.
   >
   >
2. Gebruik de **Hadoop** opdracht voor het uitvoeren van een voorbeeld van de MapReduce-taak, gebruik de volgende opdracht:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Hiermee start u de **wordcount** klasse, die is opgenomen in de **hadoop-mapreduce-examples.jar** bestand in de huidige map. Als invoer, gebruikt de **wasb://example/data/gutenberg/davinci.txt** document en uitvoer is opgeslagen in: **wasb: / / / voorbeeld/data/WordCountOutput**.

   > [!NOTE]
   > Zie voor meer informatie over deze MapReduce-taak en de bijvoorbeeld gegevens <a href="hdinsight-use-mapreduce.md">gebruik MapReduce in HDInsight Hadoop</a>.
   >
   >
3. De taak verzendt details, zoals deze wordt verwerkt en gegevens worden geretourneerd met de volgende strekking wanneer de taak voltooid is:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. Wanneer de taak voltooid is, gebruik de volgende opdracht voor het weergeven van de uitvoerbestanden die is opgeslagen op **wasb://example/data/WordCountOutput**:

        hadoop fs -ls wasb:///example/data/WordCountOutput

    Dit moet worden weergegeven in twee bestanden: **_SUCCESS** en **onderdeel-r-00000**. De **onderdeel-r-00000** bestand bevat de uitvoer voor deze taak.

   > [!NOTE]
   > Bepaalde MapReduce-taken kunnen de resultaten worden gesplitst over meerdere **onderdeel-r-###** bestanden. Als dit het geval is, gebruiken de ### achtervoegsel om aan te geven van de volgorde van de bestanden.
   >
   >
5. Gebruik de volgende opdracht om de weergave van de uitvoer:

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    Dit geeft een lijst met de woorden die zijn opgenomen in de **wasb://example/data/gutenberg/davinci.txt** bestand samen met het aantal keren dat elk woord is opgetreden. Hier volgt een voorbeeld van de gegevens die worden opgenomen in het bestand:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Samenvatting
Zoals u ziet Hiermee de opdracht Hadoop kunt u eenvoudig MapReduce-taken uitvoeren op een HDInsight-cluster en bekijk vervolgens de taakuitvoer van de.

## <a id="nextsteps"></a>Volgende stappen
Voor algemene informatie over MapReduce-taken in HDInsight:

* [Gebruik MapReduce op HDInsight Hadoop](hdinsight-use-mapreduce.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)
