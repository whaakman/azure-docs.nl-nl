---
title: Een interactieve Spark-shell gebruiken in azure HDInsight
description: Een interactieve Spark-shell biedt een proces voor lezen-uitvoeren-afdrukken voor het uitvoeren van Spark-opdrachten een voor een tegelijk en de resultaten te zien.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/09/2018
ms.openlocfilehash: 7aac2812787a7c14d99377754a4f85e699ef3f09
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68441897"
---
# <a name="run-apache-spark-from-the-spark-shell"></a>Apache Spark uitvoeren vanuit de Spark-shell

Een interactieve [Apache Spark](https://spark.apache.org/) shell biedt een repl-omgeving (read-execute-print-lus) voor het uitvoeren van Spark-opdrachten een voor een tegelijk en de resultaten te bekijken. Dit proces is handig voor het ontwikkelen en opsporen van fouten. Spark biedt één shell voor elk van de ondersteunde talen: Scala, python en R.

## <a name="get-to-an-apache-spark-shell-with-ssh"></a>Een Apache Spark shell openen met SSH

Toegang tot een Apache Spark shell op HDInsight door verbinding te maken met het primaire hoofd knooppunt van het cluster met behulp van SSH:

     ssh <sshusername>@<clustername>-ssh.azurehdinsight.net

U kunt de volledige SSH-opdracht voor uw cluster ophalen uit de Azure Portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar het deel venster voor uw HDInsight Spark-cluster.
3. Selecteer Secure Shell (SSH).

    ![Deel venster HDInsight in Azure Portal](./media/apache-spark-shell/hdinsight-spark-blade.png)

4. Kopieer de weer gegeven SSH-opdracht en voer deze uit in uw Terminal.

    ![Het deel venster HDInsight SSH in Azure Portal](./media/apache-spark-shell/hdinsight-spark-ssh-blade.png)

Zie [SSH gebruiken met hdinsight](../hdinsight-hadoop-linux-use-ssh-unix.md)voor meer informatie over het gebruik van SSH om verbinding te maken met hdinsight.

## <a name="run-an-apache-spark-shell"></a>Een Apache Spark shell uitvoeren

Spark biedt shells voor scala (Spark-shell), python (pyspark) en R (sparkr). Voer in uw SSH-sessie op het hoofd knooppunt van uw HDInsight-cluster een van de volgende opdrachten in:

    ./bin/spark-shell
    ./bin/pyspark
    ./bin/sparkR

U kunt nu Spark-opdrachten invoeren in de juiste taal.

## <a name="sparksession-and-sparkcontext-instances"></a>SparkSession-en SparkContext-instanties

Standaard worden instanties van SparkSession en SparkContext automatisch voor u geïnstantieerd wanneer u de Spark-shell uitvoert.

Voer `spark`in om toegang te krijgen tot het SparkSession-exemplaar. Voer `sc`in om toegang te krijgen tot het SparkContext-exemplaar.

## <a name="important-shell-parameters"></a>Belang rijke shell parameters

De Spark shell-opdracht`spark-shell`( `pyspark`, of `sparkR`) ondersteunt veel opdracht regel parameters. Als u een volledige lijst met para meters wilt weer geven, start u `--help`de Spark-shell met de switch. Houd er rekening mee dat sommige van deze para meters alleen van toepassing zijn op `spark-submit`, die in de Spark-shell worden geplaatst.

| /tijdnotatie | description | Voorbeeld |
| --- | --- | --- |
| --master MASTER_URL | Hiermee geeft u de hoofd-URL op. In HDInsight is deze waarde altijd `yarn`. | `--master yarn`|
| --potten JAR_LIST | Een door komma's gescheiden lijst met lokale potten die moeten worden toegevoegd aan het stuur programma en de namen van de uitvoerder. In HDInsight bestaat deze lijst uit paden naar het standaard bestands systeem in Azure Storage of Data Lake Storage. | `--jars /path/to/examples.jar` |
| --pakketten MAVEN_COORDS | Een door komma's gescheiden lijst met maven-coördinaten van potten die moeten worden vermeld op het stuur programma en de uitvoerder classpath. Doorzoekt de lokale maven opslag plaats, vervolgens maven Central, en eventuele extra externe opslag plaatsen `--repositories`die zijn opgegeven bij. De notatie voor de coördinaten is *groupid*:*artifactId*:*Version*. | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| --py-bestanden lijst | Alleen voor python: een door komma's gescheiden lijst met zip-, eier-of py-bestanden die op de PYTHONPATH moet worden geplaatst. | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>Volgende stappen

- Zie [Inleiding tot Apache Spark in azure HDInsight](apache-spark-overview.md) voor een overzicht.
- Zie [een Apache Spark cluster in azure HDInsight maken](apache-spark-jupyter-spark-sql.md) voor gebruik met Spark-clusters en SparkSQL.
- Zie [Wat is Apache Spark Structured streaming?](apache-spark-streaming-overview.md) om toepassingen te schrijven die streaminggegevens verwerken met Spark.
