---
title: Hive oplossen met behulp van Azure HDInsight | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over het werken met Apache Hive en Azure HDInsight.
keywords: HDInsight, Hive, veelgestelde vragen over Azure, probleemoplossingsgids, veelgestelde vragen
services: Azure HDInsight
documentationcenter: na
author: dharmeshkakadia
manager: 
editor: 
ms.assetid: 15B8D0F3-F2D3-4746-BDCB-C72944AA9252
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/2/2017
ms.author: dharmeshkakadia
ms.openlocfilehash: 398cc7cd0fa815f12ba5b503a1328688e1159eee
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-hive-by-using-azure-hdinsight"></a>Hive oplossen met behulp van Azure HDInsight

Meer informatie over de bovenste vragen en hun oplossingen bij het werken met Apache Hive nettoladingen in Apache Ambari.


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a>Hoe ik een Hive-metastore exporteren en importeren op een ander cluster?


### <a name="resolution-steps"></a>Stappen voor het oplossen

1. Verbinding maken met het HDInsight-cluster met behulp van een client Secure Shell (SSH). Zie voor meer informatie [lezen van aanvullende](#additional-reading-end).

2. Voer de volgende opdracht uit op het HDInsight-cluster van waaruit u wilt exporteren van de metastore:

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

  Met deze opdracht genereert u een bestand met de naam allatables.sql.

3. De alltables.sql bestand kopiëren naar het nieuwe HDInsight-cluster en voer vervolgens de volgende opdracht:

  ```apache
  hive -f alltables.sql
  ```

De code in de stappen voor het oplossen wordt ervan uitgegaan dat gegevenspaden op het nieuwe cluster hetzelfde als de gegevenspaden op het oude cluster zijn. Als de gegevenspaden verschillend zijn, kunt u handmatig het bestand gegenereerde alltables.sql om wijzigingen weer te geven.

### <a name="additional-reading"></a>Aanvullende bronnen

- [Verbinding maken met een HDInsight-cluster via SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a>Hoe ga ik Hive logboeken naar op een cluster?

### <a name="resolution-steps"></a>Stappen voor het oplossen

1. Verbinding maken met het HDInsight-cluster via SSH. Zie voor meer informatie **lezen van aanvullende**.

2. Hive-client om Logboeken te raadplegen, kunt u de volgende opdracht gebruiken:

  ```apache
  /tmp/<username>/hive.log 
  ```

3. Hive metastore om Logboeken te raadplegen, kunt u de volgende opdracht gebruiken:

  ```apache
  /var/log/hive/hivemetastore.log 
  ```

4. Hiveserver om Logboeken te raadplegen, kunt u de volgende opdracht gebruiken:

  ```apache
  /var/log/hive/hiveserver2.log 
  ```

### <a name="additional-reading"></a>Aanvullende bronnen

- [Verbinding maken met een HDInsight-cluster via SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster"></a>Hoe ik de Hive-shell met specifieke configuraties die op een cluster starten?

### <a name="resolution-steps"></a>Stappen voor het oplossen

1. Geef een sleutel-waardepaar configuratie wanneer u de Hive-shell start. Zie voor meer informatie [lezen van aanvullende](#additional-reading-end).

  ```apache
  hive -hiveconf a=b 
  ```

2. Als alle effectieve configuraties op Hive-shell wilt weergeven, gebruikt u de volgende opdracht:

  ```apache
  hive> set;
  ```

  Gebruik bijvoorbeeld de volgende opdracht Hive-shell starten met logboekregistratie voor foutopsporing is ingeschakeld op de console:

  ```apache
  hive -hiveconf hive.root.logger=ALL,console 
  ```

### <a name="additional-reading"></a>Aanvullende bronnen

- [Hive-configuratie-eigenschappen](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Hoe analyseer ik Tez DAG gegevens op een cluster kritiek pad


### <a name="resolution-steps"></a>Stappen voor het oplossen
 
1. Voor het analyseren van een Apache Tez gerichte acyclische grafiek (DAG) in een cluster kritieke grafiek, verbinding maken met de HDInsight-cluster via SSH. Zie voor meer informatie [lezen van aanvullende](#additional-reading-end).

2. Voer de volgende opdracht achter een opdrachtprompt:
   
  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
  ```

3. Als andere analyzers die kunnen worden gebruikt voor het analyseren van Tez DAG weergeven, gebruikt u de volgende opdracht:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
  ```

  U moet een voorbeeldprogramma opgeven als het eerste argument.

  Namen van geldig programma zijn onder andere:
    - **ContainerReuseAnalyzer**: details van de container opnieuw kunnen worden gebruikt in een DAG afdrukken
    - **CriticalPath**: het kritieke pad van een DAG niet vinden
    - **LocalityAnalyzer**: details in een DAG plaats afdrukken
    - **ShuffleTimeAnalyzer**: de details van de tijd willekeurige volgorde in een DAG analyseren
    - **SkewAnalyzer**: de scheeftrekken details in een DAG analyseren
    - **SlowNodeAnalyzer**: details knooppunt in een DAG afdrukken
    - **SlowTaskIdentifier**: afdrukken trage taakdetails in een DAG
    - **SlowestVertexAnalyzer**: traagste hoekpunt details in een DAG afdrukken
    - **SpillAnalyzer**: gelekt details afdrukken in een DAG
    - **TaskConcurrencyAnalyzer**: de taakdetails gelijktijdigheid van taken in een DAG afdrukken
    - **VertexLevelCriticalPathAnalyzer**: het kritieke pad op niveau van het hoekpunt niet vinden in een DAG


### <a name="additional-reading"></a>Aanvullende bronnen

- [Verbinding maken met een HDInsight-cluster via SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a>Hoe kan ik Tez DAG gegevens downloaden vanaf een cluster?


#### <a name="resolution-steps"></a>Stappen voor het oplossen

Er zijn twee manieren om de DAG van de Tez-gegevens te verzamelen:

- Vanaf de opdrachtregel:
 
    Verbinding maken met het HDInsight-cluster via SSH. Voer de volgende opdracht achter de opdrachtprompt:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- De Ambari Tez-weergave gebruiken:
   
  1. Ga naar Ambari. 
  2. Ga naar de weergave Tez (onder het pictogram tegels in de rechterbovenhoek). 
  3. Selecteer de DAG die u wilt weergeven.
  4. Selecteer **downloaden van gegevens**.

### <a name="additional-reading-end"></a>Aanvullende bronnen

[Verbinding maken met een HDInsight-cluster via SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


### <a name="see-also"></a>Zie ook
[Problemen oplossen met behulp van Azure HDInsight](hdinsight-troubleshoot-guide.md)




