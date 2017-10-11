---
title: Azure HDInsight troubleshooting Guide | Microsoft Docs
description: Los problemen met Hadoop-werkbelastingen met behulp van Azure HDInsight. Stapsgewijze documentatie wordt beschreven hoe u HDInsight gebruiken voor het oplossen van veelvoorkomende problemen met Hive, Spark, YARN, HBase, HDFS en Storm.
services: hdinsight
author: arijitt
manager: arijitt
layout: LandingPage
ms.assetid: 
ms.service: hdinsight
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: landing - page
ms.date: 7/06/2017
ms.author: arijitt
ms.openlocfilehash: f246c6d1355220314a2f56aa91634625ec352c69
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-by-using-azure-hdinsight"></a>Problemen oplossen met behulp van Azure HDInsight

| Apache werkbelasting | De vragen |
|---|---|
|![HBase](./media/hdinsight-troubleshoot-guide/HBASE.png)<br>[HBase oplossen](hdinsight-troubleshoot-hbase.md)|<br>[Hoe voer hbck opdracht rapporten met meerdere niet-toegewezen gebieden](hdinsight-troubleshoot-hbase.md#how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions)<br><br>[Hoe los ik problemen time-out bij gebruik van hbck opdrachten voor toewijzingen regio](hdinsight-troubleshoot-hbase.md#how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments)<br><br>[Hoe ik geforceerde-/ uitschakelen HDFS veilige modus op een cluster](hdinsight-troubleshoot-hbase.md#how-do-i-force-disable-hdfs-safe-mode-in-a-cluster)<br><br>[Hoe los JDBC of SQLLine verbinding problemen met Apache Phoenix](hdinsight-troubleshoot-hbase.md#how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix)<br><br>[Wat zorgt ervoor dat een master server niet worden gestart](hdinsight-troubleshoot-hbase.md#what-causes-a-master-server-to-fail-to-start)<br><br>[Wat veroorzaakt een fout opnieuw opstarten op een server regio](hdinsight-troubleshoot-hbase.md#what-causes-a-restart-failure-on-a-region-server)|
|![HDFS](./media/hdinsight-troubleshoot-guide/HDFS.png)<br>[HDFS oplossen](hdinsight-troubleshoot-hdfs.md)|<br>[Hoe krijg ik toegang tot een lokale HDFS uit in een cluster](hdinsight-troubleshoot-hdfs.md#how-do-i-access-local-hdfs-from-inside-a-cluster)<br><br>[Hoe ik geforceerde-/ uitschakelen HDFS veilige modus op een cluster](hdinsight-troubleshoot-hdfs.md#how-do-i-force-disable-hdfs-safe-mode-in-a-cluster)|
|![Hive](./media/hdinsight-troubleshoot-guide/HIVE.png)<br>[HBase oplossen](hdinsight-troubleshoot-hive.md)|<br>[Hoe ik een Hive-metastore exporteren en importeren op een ander cluster](hdinsight-troubleshoot-hive.md#how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster)<br><br>[Hoe ga ik Hive logboeken naar op een cluster](hdinsight-troubleshoot-hive.md#how-do-i-locate-hive-logs-on-a-cluster)<br><br>[Hoe ik de Hive-shell met specifieke configuraties die op een cluster starten](hdinsight-troubleshoot-hive.md#how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster)<br><br>[Hoe analyseer ik Tez DAG gegevens op een cluster kritiek pad](hdinsight-troubleshoot-hive.md#how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path)<br><br>[Hoe kan ik Tez DAG gegevens downloaden vanaf een cluster](hdinsight-troubleshoot-hive.md#how-do-i-download-tez-dag-data-from-a-cluster)|
|![Spark](./media/hdinsight-troubleshoot-guide/SPARK.png)<br>[Spark oplossen](hdinsight-troubleshoot-SPARK.md)|<br>[Hoe kan ik een Spark-toepassing configureren met behulp van Ambari op clusters](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-by-using-ambari-on-clusters)<br><br>[Hoe kan ik een Spark-toepassing configureren met behulp van een Jupyter-notebook in clusters](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters)<br><br>[Hoe kan ik een Spark-toepassing configureren met behulp van Livy op clusters](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-by-using-livy-on-clusters)<br><br>[Hoe configureer een toepassing met behulp van spark indienen Spark op clusters](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters)<br><br>[Wat een Spark-toepassing OutOfMemoryError uitzondering veroorzaakt](hdinsight-troubleshoot-spark.md#what-causes-a-spark-application-outofmemoryerror-exception)|
|![Storm](./media/hdinsight-troubleshoot-guide/STORM.png)<br>[Storm oplossen](hdinsight-troubleshoot-STORM.md)|<br>[Hoe krijg ik toegang tot de Storm-gebruikersinterface op een cluster](hdinsight-troubleshoot-storm.md#how-do-i-access-the-storm-ui-on-a-cluster)<br><br>[Hoe worden ik Storm event hub spout controlepunt informatie uit een topologie overgedragen naar een andere](hdinsight-troubleshoot-storm.md#how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another)<br><br>[Hoe ik Storm binaire bestanden vinden op een cluster](hdinsight-troubleshoot-storm.md#how-do-i-locate-storm-binaries-on-a-cluster)<br><br>[Hoe bepaal ik de implementatietopologie van een Storm-cluster](hdinsight-troubleshoot-storm.md#how-do-i-determine-the-deployment-topology-of-a-storm-cluster)<br><br>[Hoe ik Storm event hub spout binaire bestanden voor de ontwikkeling van vinden](hdinsight-troubleshoot-storm.md#how-do-i-locate-storm-event-hub-spout-binaries-for-development)<br><br>[Hoe ik Storm Log4J configuratiebestanden op clusters vinden](hdinsight-troubleshoot-storm.md#how-do-i-locate-storm-log4j-configuration-files-on-clusters)|
|![YARN](./media/hdinsight-troubleshoot-guide/YARN.png)<br>[YARN oplossen](hdinsight-troubleshoot-YARN.md)|<br>[Hoe maak een nieuwe YARN-wachtrij op een cluster](hdinsight-troubleshoot-yarn.md#how-do-i-create-a-new-yarn-queue-on-a-cluster)<br><br>[Hoe kan ik YARN-logboeken downloaden vanaf een cluster](hdinsight-troubleshoot-yarn.md#how-do-i-download-yarn-logs-from-a-cluster)|

## <a name="hdinsight-troubleshooting-resources"></a>Bronnen voor probleemoplossing voor HDInsight

| Voor informatie over | Zie de volgende artikelen |
| --- | --- |
| HDInsight op Linux- en optimalisatie | - [Informatie over het gebruik van HDInsight op Linux](hdinsight-hadoop-linux-information.md)<br>- [Hadoop-geheugen en het oplossen van prestaties](hdinsight-hadoop-stack-trace-error-messages.md)<br>- [Prestaties van de hive-query 's](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/) |
| Logboeken en dumpbestanden | - [Toegang tot toepassingslogboeken Hadoop YARN op Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md)<br>- [Heap dumpbestanden voor Hadoop-services op Linux inschakelen](hdinsight-hadoop-collect-debug-heap-dump-linux.md)<br>- [Logboekbestanden van HDInsight analyseren](hdinsight-debug-jobs.md)|
| Fouten | - [Begrijpen en WebHCat fouten oplossen](hdinsight-hadoop-templeton-webhcat-debug-errors.md)<br>- [Hive-instellingen om op te lossen OutofMemory-fout](hdinsight-hadoop-hive-out-of-memory-error-oom.md) |
| Hulpprogramma's | - [Ambari-weergaven gebruiken om op te sporen Tez-taken](hdinsight-debug-ambari-tez-view.md)<br>- [Hive-query's optimaliseren](hdinsight-hadoop-optimize-hive-query.md) |
