---
title: Interactieve Query gebruiken met Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van interactieve Query (Hive LLAP) met HDInsight.
keywords: 
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: jgao
ms.openlocfilehash: 80e96e6bb727e6d5c1331580fad328d570b21494
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="use-interactive-query-with-hdinsight"></a>Interactieve Query gebruiken met HDInsight
Interactieve Query (ook wel Hive LLAP of [lange Live en proces](https://cwiki.apache.org/confluence/display/Hive/LLAP)) is een Azure HDInsight [type cluster](../hdinsight-hadoop-provision-linux-clusters.md#cluster-types). Interactieve Query ondersteunt in-memory caching, waardoor Hive-query's sneller en nog veel meer interactieve.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)] 

Een cluster interactieve Query wijkt af van een Hadoop-cluster. Het bevat alleen de Hive-service. 

> [!NOTE]
> U kunt toegang tot de Hive-service in het cluster interactieve Query alleen via de Ambari Hive-weergave, Beeline en het stuurprogramma Microsoft Hive ODBC (Hive ODBC). U kunt deze niet openen via de console Hive, Templeton, de Azure-opdrachtregelprogramma (Azure CLI) of Azure PowerShell. 
> 
> 

## <a name="create-an-interactive-query-cluster"></a>Een interactieve Query-cluster maken
Zie voor meer informatie over het maken van een HDInsight-cluster [maken Hadoop-clusters in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Kies het type van de cluster interactieve Query.

## <a name="execute-hive-queries-from-interactive-query"></a>Hive-query's van interactieve Query uitvoeren
Voor het uitvoeren van Hive-query's, hebt u de volgende opties:

* Power BI gebruiken

    Zie [gegevens interactieve Hive-Query visualiseren met Power BI in Azure HDInsight](./apache-hadoop-connect-hive-power-bi-directquery.md) Zie [big-gegevens visualiseren met Power BI in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
 
* Zeppelin gebruiken

    Zie [Zeppelin gebruik Hive-query's uitvoeren in Azure HDInsight ](../hdinsight-connect-hive-zeppelin.md).

* Visual Studio gebruiken

    Zie [verbinding maken met Azure HDInsight en voer Hive-query's met Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).

* Visual Studio Code gebruiken:

    Zie [Visual Studio Code gebruiken voor Hive, LLAP of pySpark](../hdinsight-for-vscode.md).
* Voer Hive via Ambari Hive-weergave.
  
    Zie [Hive-weergave gebruiken met Hadoop in Azure HDInsight](../hadoop/apache-hadoop-use-hive-ambari-view.md).
* Voer Hive via Beeline.
  
    Zie [Hive gebruiken met Hadoop in HDInsight met Beeline](../hadoop/apache-hadoop-use-hive-beeline.md).
  
    U kunt Beeline van het hoofdknooppunt of van een leeg edge-knooppunt. U wordt aangeraden met Beeline uit een lege edge-knooppunt. Zie voor meer informatie over het maken van een HDInsight-cluster met behulp van een leeg edge-knooppunt [leeg edge-knooppunten gebruiken in HDInsight](../hdinsight-apps-use-edge-node.md).
* Voer Hive via Hive ODBC.
  
    Zie [verbinding maken met Excel en Hadoop met het Microsoft Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

Zoek de verbindingsreeks Java Database Connectivity (JDBC):

1. Aanmelden bij de Ambari met behulp van de volgende URL: https://\<clusternaam\>. AzureHDInsight.net.
2. Selecteer in het menu links **Hive**.
3. Als de URL kopiëren, selecteert u het pictogram van het Klembord:
   
   ![HDInsight Hadoop interactieve Query LLAP JDBC](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over hoe [interactieve Query-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).
* Meer informatie over hoe [big-gegevens visualiseren met Power BI in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* Meer informatie over hoe [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight ](../hdinsight-connect-hive-zeppelin.md).
* Meer informatie over hoe [uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).
* Meer informatie over hoe [gebruik van HDInsight Tools voor Visual Studio Code](../hdinsight-for-vscode.md).
* Meer informatie over hoe [weergave Hive gebruiken met Hadoop in HDInsight](../hadoop/apache-hadoop-use-hive-ambari-view.md)
* Meer informatie over hoe [Beeline gebruikt voor het indienen van Hive-query's in HDInsight](../hadoop/apache-hadoop-use-hive-beeline.md).
* Meer informatie over hoe [Excel en Hadoop koppelen met het Microsoft Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

