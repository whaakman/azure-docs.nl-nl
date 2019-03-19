---
title: Met Hive oplossen met behulp van Azure HDInsight
description: Vind antwoorden op veelgestelde vragen over het werken met Apache Hive en Azure HDInsight.
keywords: HDInsight, Hive, veelgestelde vragen over Azure, problemen oplossen handleiding, veelgestelde vragen
services: hdinsight
ms.service: hdinsight
author: dharmeshkakadia
ms.author: dharmeshkakadia
ms.topic: conceptual
ms.date: 11/2/2017
ms.openlocfilehash: 6d75bf86dab8775e77efb21ecc3b0d60063a9823
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58088958"
---
# <a name="troubleshoot-apache-hive-by-using-azure-hdinsight"></a>Apache Hive oplossen met behulp van Azure HDInsight

Meer informatie over de meestgestelde vragen en hun oplossingen als u werkt met Apache Hive nettoladingen in Apache Ambari.


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a>Hoe ik een Hive-metastore exporteren en importeren op een ander cluster?


### <a name="resolution-steps"></a>Oplossingen

1. Verbinding maken met het HDInsight-cluster met behulp van een Secure Shell (SSH)-client. Zie voor meer informatie, [meer lezen](#additional-reading-end).

2. Voer de volgende opdracht op het HDInsight-cluster van waaruit u wilt de metastore exporteren:

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

   Met deze opdracht genereert u een bestand met de naam allatables.sql.

3. De alltables.sql bestand kopiëren naar de nieuwe HDInsight-cluster en voer de volgende opdracht:

   ```apache
   hive -f alltables.sql
   ```

De code in de Oplossingsstappen wordt ervan uitgegaan dat gegevenspaden op het nieuwe cluster hetzelfde als de gegevenspaden op het oude cluster zijn. Als de gegevenspaden verschillend zijn, kunt u handmatig het bestand gegenereerde alltables.sql aanleiding van wijzigingen bewerken.

### <a name="additional-reading"></a>Meer lezen

- [Verbinding maken met een HDInsight-cluster via SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a>Hoe vind ik Hive logboeken op een cluster?

### <a name="resolution-steps"></a>Oplossingen

1. Verbinding maken met het HDInsight-cluster met behulp van SSH. Zie voor meer informatie, **meer lezen**.

2. Hive-client als logboeken wilt raadplegen, gebruik de volgende opdracht:

   ```apache
   /tmp/<username>/hive.log 
   ```

3. Hive-metastore als logboeken wilt raadplegen, gebruikt u de volgende opdracht:

   ```apache
   /var/log/hive/hivemetastore.log 
   ```

4. Hiveserver als logboeken wilt raadplegen, gebruikt u de volgende opdracht:

   ```apache
   /var/log/hive/hiveserver2.log 
   ```

### <a name="additional-reading"></a>Meer lezen

- [Verbinding maken met een HDInsight-cluster via SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster"></a>Hoe ik de Hive-shell met specifieke configuraties op een cluster starten?

### <a name="resolution-steps"></a>Oplossingen

1. Geef een configuratie van sleutel / waarde-paar bij het starten van de Hive-shell. Zie voor meer informatie, [meer lezen](#additional-reading-end).

   ```apache
   hive -hiveconf a=b 
   ```

2. Als u alle effectieve configuraties op de Hive-shell, gebruik de volgende opdracht:

   ```apache
   hive> set;
   ```

   Bijvoorbeeld, gebruik de volgende opdracht om te starten van Hive-shell met logboekregistratie voor foutopsporing ingeschakeld op de console:

   ```apache
   hive -hiveconf hive.root.logger=ALL,console 
   ```

### <a name="additional-reading"></a>Meer lezen

- [Hive-configuratie-eigenschappen](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Hoe ik Apache Tez DAG gegevens op een cluster-kritieke pad analyseren?


### <a name="resolution-steps"></a>Oplossingen
 
1. Voor het analyseren van een Apache Tez directed acyclic graph (DAG) op een cluster-kritieke grafiek, verbinding met het HDInsight-cluster met behulp van SSH. Zie voor meer informatie, [meer lezen](#additional-reading-end).

2. Voer de volgende opdracht uit op een opdrachtprompt:
   
   ```apache
   hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
   ```

3. Als u andere analyzers die kunnen worden gebruikt voor het analyseren van de DAG Tez, gebruik de volgende opdracht:

   ```apache
   hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
   ```

   Als het eerste argument moet u een voorbeeldprogramma opgeven.

   Geldig programmanamen zijn onder andere:
    - **ContainerReuseAnalyzer**: Details van de container opnieuw worden gebruikt in een DAG afdrukken
    - **CriticalPath**: Zoek het kritieke pad van een DAG
    - **LocalityAnalyzer**: Details van de plaats waar zich bevinden in een DAG afdrukken
    - **ShuffleTimeAnalyzer**: De details van de tijd shuffle in een DAG analyseren
    - **SkewAnalyzer**: Het analyseren van de scheeftrekken details in een DAG
    - **SlowNodeAnalyzer**: Knooppuntdetails van het in een DAG afdrukken
    - **SlowTaskIdentifier**: Trage taakdetails in een DAG afdrukken
    - **SlowestVertexAnalyzer**: Details van de traagste hoekpunt in een DAG afdrukken
    - **SpillAnalyzer**: Gegevens worden gelekt in een DAG afdrukken
    - **TaskConcurrencyAnalyzer**: De details van de gelijktijdigheid van taken in een DAG afdrukken
    - **VertexLevelCriticalPathAnalyzer**: Het kritieke pad op het niveau van hoekpunt niet vinden in een DAG


### <a name="additional-reading"></a>Meer lezen

- [Verbinding maken met een HDInsight-cluster via SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a>Hoe kan ik Tez DAG gegevens downloaden uit een cluster?


#### <a name="resolution-steps"></a>Oplossingen

Er zijn twee manieren om de DAG van de Tez-gegevens te verzamelen:

- Vanaf de opdrachtregel:
 
    Verbinding maken met het HDInsight-cluster met behulp van SSH. Voer de volgende opdracht achter de opdrachtprompt:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- De Ambari Tez-weergave gebruiken:
   
  1. Ga naar Ambari. 
  2. Ga naar de weergave Tez (onder het pictogram van de tegels in de rechterbovenhoek). 
  3. Selecteer de DAG die u wilt weergeven.
  4. Selecteer **gegevens downloaden**.

### <a name="additional-reading-end"></a>Meer lezen

[Verbinding maken met een HDInsight-cluster via SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


### <a name="see-also"></a>Zie ook
[Problemen oplossen met behulp van Azure HDInsight](hdinsight-troubleshoot-guide.md)




