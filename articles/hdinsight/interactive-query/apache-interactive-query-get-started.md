---
title: Interactive Query gebruiken met Azure HDInsight
description: Informatie over het Interactive Query (LLAP Hive) gebruiken met HDInsight.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/22/2018
ms.openlocfilehash: 73f7e523ed0abc7d0453096cf783761dd6a884ba
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628661"
---
# <a name="use-interactive-query-with-hdinsight"></a>Interactive Query gebruiken met HDInsight
Interactive Query (ook wel genoemd Hive LLAP of [met lage latentie analytische verwerking](https://cwiki.apache.org/confluence/display/Hive/LLAP)) is een Azure HDInsight [clustertype](../hdinsight-hadoop-provision-linux-clusters.md#cluster-types). Interactieve query's ondersteunt in-memory caching, waardoor Hive-query's sneller en nog veel meer interactieve.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)] 

Een interactieve Query-cluster wijkt af van een Hadoop-cluster. Het bevat alleen de Hive-service. 

> [!NOTE]
> U kunt toegang tot de Hive-service in het interactieve Query-cluster alleen via Ambari Hive-weergave, Beeline en het stuurprogramma Microsoft Hive Open Database Connectivity (ODBC Hive). U geen toegang krijgt via de Hive-console, Templeton, de Azure-opdrachtregelprogramma (Azure CLI) of Azure PowerShell. 
> 
> 

## <a name="create-an-interactive-query-cluster"></a>Een interactieve Query-cluster maken
Zie voor meer informatie over het maken van een HDInsight-cluster [Hadoop-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Kies het type Interactive Query-cluster.

## <a name="execute-hive-queries-from-interactive-query"></a>Hive-query's uitvoeren in Interactive Query
Voor het uitvoeren van Hive-query's, hebt u de volgende opties:

* Power BI gebruiken

    Zie [gegevens met Power BI in Azure HDInsight Interactive Query Hive visualiseren](./apache-hadoop-connect-hive-power-bi-directquery.md) Zie [big-gegevens visualiseren met Power BI in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
 
* Zeppelin gebruiken

    Zie [Zeppelin gebruik Hive-query's uitvoeren in Azure HDInsight ](../hdinsight-connect-hive-zeppelin.md).

* Visual Studio gebruiken

    Zie [verbinding maken met Azure HDInsight en het uitvoeren Hive-query's met Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).

* Gebruik Visual Studio Code

    Zie [Visual Studio-Code gebruiken voor Hive, LLAP of pySpark](../hdinsight-for-vscode.md).
* Hive uitvoeren met behulp van de Ambari Hive-weergave.
  
    Zie [Hive-weergave gebruiken met Hadoop in Azure HDInsight](../hadoop/apache-hadoop-use-hive-ambari-view.md).
* Hive met behulp van Beeline uitvoeren.
  
    Zie [Hive gebruiken met Hadoop in HDInsight met Beeline](../hadoop/apache-hadoop-use-hive-beeline.md).
  
    U kunt Beeline gebruiken vanaf het hoofdknooppunt of vanaf een lege edge-knooppunt. We adviseren Beeline gebruiken vanuit een lege edge-knooppunt. Zie voor meer informatie over het maken van een HDInsight-cluster met behulp van een lege edge-knooppunt [lege edge-knooppunten gebruiken in HDInsight](../hdinsight-apps-use-edge-node.md).
* Hive uitvoeren met behulp van Hive ODBC.
  
    Zie [verbinding maken met Excel aan Hadoop met het Microsoft Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

Zoek de verbindingsreeks Java Database Connectivity (JDBC):

1. Aanmelden bij de Ambari met behulp van de volgende URL: https://\<clusternaam\>. AzureHDInsight.net.
2. Selecteer in het menu links **Hive**.
3. Als de URL kopiëren, selecteert u het pictogram van het Klembord:
   
   ![Hadoop in HDInsight Interactive Query LLAP JDBC](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [Interactive Query-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).
* Meer informatie over het [big-gegevens visualiseren met Power BI in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* Meer informatie over het [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight ](../hdinsight-connect-hive-zeppelin.md).
* Meer informatie over het [uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).
* Meer informatie over het [HDInsight-hulpprogramma's voor Visual Studio Code gebruiken](../hdinsight-for-vscode.md).
* Meer informatie over het [Hive-weergave gebruiken met Hadoop in HDInsight](../hadoop/apache-hadoop-use-hive-ambari-view.md)
* Meer informatie over het [Beeline gebruiken voor het indienen van Hive-query's in HDInsight](../hadoop/apache-hadoop-use-hive-beeline.md).
* Meer informatie over het [koppel Excel aan Hadoop met het Microsoft Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

