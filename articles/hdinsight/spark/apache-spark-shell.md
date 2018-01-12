---
title: Gebruik van een Shell interactieve Spark in Azure HDInsight | Microsoft Docs
description: "Een interactieve Spark-Shell biedt een lees-uitvoeren-print-proces voor Spark opdrachten één uitgevoerd op een moment en de resultaten."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: maxluk
ms.openlocfilehash: 55ff40165a1b25949cb202d9a5174e1f52fe4630
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="run-spark-from-the-spark-shell"></a>Spark uitvoeren vanuit de Shell Spark

Een interactieve Spark-Shell biedt een REPL-omgeving (Lees-uitvoeren-print loop) voor Spark opdrachten één uitgevoerd op een moment en de resultaten. Dit proces is nuttig voor ontwikkeling en foutopsporing. Spark biedt een shell voor elk van de ondersteunde talen: Scala, Python en R.

## <a name="get-to-a-spark-shell-with-ssh"></a>Ophalen van een Spark-Shell met SSH

Toegang tot een Shell Spark in HDInsight door verbinding te maken met het primaire hoofdknooppunt van het cluster via SSH:

     ssh <sshusername>@<clustername>-ssh.azurehdinsight.net

U kunt de volledige SSH-opdracht voor uw cluster krijgen van de Azure-portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar het deelvenster voor HDInsight Spark-cluster.
3. Selecteer Secure Shell (SSH).

    ![HDInsight-deelvenster in Azure-portal](./media/apache-spark-shell/hdinsight-spark-blade.png)

4. De weergegeven SSH-opdracht kopiëren en voer deze in uw terminal.

    ![HDInsight SSH deelvenster in Azure-portal](./media/apache-spark-shell/hdinsight-spark-ssh-blade.png)

Zie voor meer informatie over het gebruik van SSH verbinding maken met HDInsight [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run-a-spark-shell"></a>Een Spark-Shell uitvoeren

Spark biedt houders voor Scala (spark-shell), Python (pyspark) en R (sparkR). Voer een van de volgende opdrachten in uw SSH-sessie op het hoofdknooppunt van uw HDInsight-cluster:

    ./bin/spark-shell
    ./bin/pyspark
    ./bin/sparkR

U kunt nu Spark opdrachten invoeren in de juiste taal.

## <a name="sparksession-and-sparkcontext-instances"></a>SparkSession en SparkContext instanties

Standaard tijdens het uitvoeren van de Shell Spark worden exemplaren van SparkSession en SparkContext automatisch gemaakt voor u.

Voer voor toegang tot het exemplaar SparkSession moet `spark`. Voer voor toegang tot het exemplaar SparkContext moet `sc`.

## <a name="important-shell-parameters"></a>Belangrijke shell parameters

De opdracht Spark-Shell (`spark-shell`, `pyspark`, of `sparkR`) ondersteunt vele opdrachtregelparameters. Als u wilt zien van een volledige lijst met parameters, start u de Spark-Shell met de schakeloptie `--help`. Houd er rekening mee dat sommige van deze parameters kunnen alleen van toepassing op `spark-submit`, die geschikt is voor de Shell Spark.

| Switch | description | Voorbeeld |
| --- | --- | --- |
| --de hoofdsleutel van de MASTER_URL | Hiermee geeft u de URL van de master. In HDInsight, is deze waarde altijd `yarn`. | `--master yarn`|
| --jars JAR_LIST | Door komma's gescheiden lijst met lokale potten moeten worden opgenomen in de stuurprogramma's en executor klassepaden. Deze lijst is in HDInsight bestaat uit de paden naar het bestandssysteem standaard in Azure Storage of de Data Lake Store. | `--jars /path/to/examples.jar` |
| --MAVEN_COORDS pakketten | Door komma's gescheiden lijst met maven-coördinaten van potten moeten worden opgenomen in de stuurprogramma's en executor klassepaden. De lokale maven-opslagplaats en vervolgens centraal maven, zoekt en eventuele aanvullende externe opslagplaatsen met opgegeven `--repositories`. De indeling voor de coördinaten *groupId*:*artefact-id*:*versie*. | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| LIJST met--py bestanden | Voor Python alleen een door komma's gescheiden lijst met ZIP- of .egg .py bestanden in de PYTHONPATH te plaatsen. | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>Volgende stappen

- Zie [Inleiding tot Spark op Azure HDInsight](apache-spark-overview.md) voor een overzicht.
- Zie [een Apache Spark-cluster maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) werken met Spark-clusters en SparkSQL.
- Zie [wat is er gestructureerde Spark-Streaming?](apache-spark-streaming-overview.md) schrijven van toepassingen die streaminggegevens met Spark verwerken.

