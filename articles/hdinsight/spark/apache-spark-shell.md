---
title: Een interactieve Spark-Shell gebruiken in Azure HDInsight
description: Een interactieve Spark-Shell biedt een alleen-uitvoeren-print-proces voor het uitvoeren van de Spark opdrachten één bewerking tegelijk en de resultaten.
services: hdinsight
ms.service: hdinsight
author: maxluk
ms.author: maxluk
editor: jasonwhowell
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/09/2018
ms.openlocfilehash: 454f05f6ec17a42d0f0d3795d490352e5e74783a
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39617503"
---
# <a name="run-spark-from-the-spark-shell"></a>Spark kan worden uitgevoerd vanuit de Spark-Shell

Een interactieve Spark-Shell biedt een REPL-omgeving (read-execute-print-loop) voor het uitvoeren van de Spark opdrachten één bewerking tegelijk en de resultaten. Dit proces is handig voor ontwikkeling en foutopsporing. Spark biedt een shell voor elk van de ondersteunde talen: Scala, Python en R.

## <a name="get-to-a-spark-shell-with-ssh"></a>Kennismaking met een Spark-Shell met SSH

Toegang tot een Spark-Shell op HDInsight door verbinding te maken met het primaire hoofdknooppunt van het cluster via SSH:

     ssh <sshusername>@<clustername>-ssh.azurehdinsight.net

U kunt de volledige SSH-opdracht krijgen voor uw cluster vanuit Azure portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar het deelvenster voor uw HDInsight Spark-cluster.
3. Selecteer Secure Shell (SSH).

    ![HDInsight-deelvenster in Azure portal](./media/apache-spark-shell/hdinsight-spark-blade.png)

4. Kopieer de weergegeven SSH-opdracht en deze in de terminal uitvoeren.

    ![HDInsight SSH deelvenster in Azure portal](./media/apache-spark-shell/hdinsight-spark-ssh-blade.png)

Zie voor meer informatie over het gebruik van SSH verbinding maken met HDInsight [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run-a-spark-shell"></a>Een Spark-Shell uitvoeren

Spark biedt houders (spark-shell) Scala, Python (pyspark) en R (sparkR). Voer in de SSH-sessie op het hoofdknooppunt van het HDInsight-cluster in een van de volgende opdrachten:

    ./bin/spark-shell
    ./bin/pyspark
    ./bin/sparkR

U kunt nu Spark opdrachten invoeren in de juiste taal.

## <a name="sparksession-and-sparkcontext-instances"></a>SparkSession en SparkContext-instanties

Standaard tijdens het uitvoeren van de Spark-Shell worden exemplaren van SparkSession en SparkContext automatisch gemaakt voor u.

Voor toegang tot het exemplaar SparkSession, voer `spark`. Voor toegang tot de SparkContext-exemplaar, voer `sc`.

## <a name="important-shell-parameters"></a>Belangrijke shell-parameters

De Spark-Shell-opdracht (`spark-shell`, `pyspark`, of `sparkR`) biedt ondersteuning voor veel opdrachtregelparameters. Een volledige lijst van parameters wilt bekijken, start u de Spark-Shell met de schakeloptie `--help`. Houd er rekening mee dat sommige van deze parameters kunnen alleen van toepassing op `spark-submit`, die geschikt is voor de Spark-Shell.

| Switch | description | Voorbeeld |
| --- | --- | --- |
| --model MASTER_URL | Hiermee geeft u de URL van de master. In HDInsight, is deze waarde altijd `yarn`. | `--master yarn`|
| --jars JAR_LIST | Door komma's gescheiden lijst met lokale JAR-bestanden moeten worden opgenomen in de stuurprogramma- en executor klassepaden. In HDInsight, wordt deze lijst bestaat uit de paden naar het bestandssysteem standaard in Azure Storage of Data Lake Store. | `--jars /path/to/examples.jar` |
| --MAVEN_COORDS-pakketten | Door komma's gescheiden lijst met maven-coördinaten van de JAR-bestanden moeten worden opgenomen in de stuurprogramma- en executor klassepaden. Zoekt naar de lokale maven-opslagplaats en klik vervolgens op maven central, en vervolgens extra externe opslagplaatsen die is opgegeven met `--repositories`. De notatie voor de coördinaten is *groeps-id*:*artifactId*:*versie*. | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| LIJST met--py-bestanden | Voor Python, een door komma's gescheiden lijst met ZIP- of .egg .py bestanden op de PYTHONPATH te plaatsen. | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>Volgende stappen

- Zie [Inleiding tot Spark in Azure HDInsight](apache-spark-overview.md) voor een overzicht.
- Zie [een Apache Spark-cluster maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) om te werken met Spark-clusters en SparkSQL.
- Zie [wat is Spark Structured Streaming?](apache-spark-streaming-overview.md) toepassingen schrijft die met Spark-gegevensstromen verwerken.

