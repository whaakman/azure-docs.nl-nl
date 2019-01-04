---
title: Interactive Query gebruiken met Azure HDInsight
description: Informatie over het Interactive Query (LLAP Hive) gebruiken met HDInsight.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/22/2018
ms.openlocfilehash: b34d67e640f09ac6949e992d3af9388581851622
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53629523"
---
# <a name="use-interactive-query-with-hdinsight"></a>Interactive Query gebruiken met HDInsight
Interactive Query (ook wel genoemd Apache Hive LLAP of [met lage latentie analytische verwerking](https://cwiki.apache.org/confluence/display/Hive/LLAP)) is een Azure HDInsight [clustertype](../hdinsight-hadoop-provision-linux-clusters.md#cluster-types). Interactieve query's ondersteunt in-memory caching, waardoor Apache Hive-query's sneller en nog veel meer interactieve.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)] 

Een interactieve Query-cluster wijkt af van een Apache Hadoop-cluster. Het bevat alleen de Hive-service. 

> [!NOTE]  
> U kunt toegang tot de Hive-service in het interactieve Query-cluster alleen via Apache Ambari Hive-weergave, Beeline en het stuurprogramma Microsoft Hive Open Database Connectivity (ODBC Hive). U geen toegang krijgt via de Hive-console, Templeton, de klassieke Azure-CLI of Azure PowerShell. 

## <a name="create-an-interactive-query-cluster"></a>Een interactieve Query-cluster maken
Zie voor meer informatie over het maken van een HDInsight-cluster [Apache Hadoop-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Kies het type Interactive Query-cluster.

## <a name="execute-apache-hive-queries-from-interactive-query"></a>Apache Hive-query's uitvoeren in Interactive Query
Voor het uitvoeren van Hive-query's, hebt u de volgende opties:

* Microsoft Power BI gebruiken

    Zie [visualiseren interactieve Query Apache Hive-gegevens met Power BI in Azure HDInsight](./apache-hadoop-connect-hive-power-bi-directquery.md) Zie [big-gegevens visualiseren met Power BI in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
 
* Apache Zeppelin gebruiken

    Zie [gebruik Apache Zeppelin Apache Hive-query's uitvoeren in Azure HDInsight ](../hdinsight-connect-hive-zeppelin.md).

* Visual Studio gebruiken

    Zie [verbinding maken met Azure HDInsight en voer Apache Hive-query's met Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-apache-hive-queries).

* Gebruik Visual Studio Code

    Zie [Visual Studio-Code gebruiken voor Apache Hive, LLAP of pySpark](../hdinsight-for-vscode.md).
* Voer de Apache Hive met behulp van Apache Ambari Hive-weergave.
  
    Zie [Apache Hive-weergave gebruiken met Apache Hadoop in Azure HDInsight](../hadoop/apache-hadoop-use-hive-ambari-view.md).

* Voer de Apache Hive met behulp van Beeline.
  
    Zie [Apache Hive gebruiken met Apache Hadoop in HDInsight met Beeline](../hadoop/apache-hadoop-use-hive-beeline.md).
  
    U kunt Beeline gebruiken vanaf het hoofdknooppunt of vanaf een lege edge-knooppunt. We adviseren Beeline gebruiken vanuit een lege edge-knooppunt. Zie voor meer informatie over het maken van een HDInsight-cluster met behulp van een lege edge-knooppunt [lege edge-knooppunten gebruiken in HDInsight](../hdinsight-apps-use-edge-node.md).
* Voer de Apache Hive met behulp van Hive ODBC.
  
    Zie [Excel verbinding maken met Apache Hadoop met het Microsoft Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

Zoek de verbindingsreeks Java Database Connectivity (JDBC):

1. Aanmelden bij de Apache Ambari met behulp van de volgende URL: https://\<clusternaam\>. AzureHDInsight.net.
2. Selecteer in het menu links **Hive**.
3. Als de URL kopiëren, selecteert u het pictogram van het Klembord:
   
   ![Hadoop in HDInsight Interactive Query LLAP JDBC](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [Interactive Query-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).
* Meer informatie over het [big-gegevens visualiseren met Power BI in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* Meer informatie over het [Apache Zeppelin gebruiken voor het Apache Hive-query's uitvoeren in Azure HDInsight ](../hdinsight-connect-hive-zeppelin.md).
* Meer informatie over het [Apache Hive-query's uitvoeren met behulp van Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-apache-hive-queries).
* Meer informatie over het [HDInsight-hulpprogramma's voor Visual Studio Code gebruiken](../hdinsight-for-vscode.md).
* Meer informatie over het [Apache Hive-weergave gebruiken met Apache Hadoop in HDInsight](../hadoop/apache-hadoop-use-hive-ambari-view.md)
* Meer informatie over het [Beeline gebruiken voor het indienen van Apache Hive-query's in HDInsight](../hadoop/apache-hadoop-use-hive-beeline.md).
* Meer informatie over het [Excel verbinden met Apache Hadoop met het Microsoft Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

