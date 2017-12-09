---
title: YARN oplossen met behulp van Azure HDInsight | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over het werken met Apache Hadoop YARN en Azure HDInsight.
keywords: HDInsight, YARN Veelgestelde vragen over Azure, probleemoplossingsgids, veelgestelde vragen
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: F76786A9-99AB-4B85-9B15-CA03528FC4CD
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/2/2017
ms.author: arijitt
ms.openlocfilehash: fbcb4807aa7f6a3d6227cd630c77714c4d2834b3
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="troubleshoot-yarn-by-using-azure-hdinsight"></a>YARN oplossen met behulp van Azure HDInsight

Meer informatie over de meest voorkomende problemen en hun oplossingen bij het werken met Apache Hadoop YARN nettoladingen in Apache Ambari.

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>Hoe maak een nieuwe YARN-wachtrij op een cluster?


### <a name="resolution-steps"></a>Stappen voor het oplossen 

Gebruik de volgende stappen in de Ambari een nieuwe YARN-wachtrij maken en vervolgens de capaciteit toewijzing tussen alle wachtrijen worden verdeeld. 

In dit voorbeeld twee bestaande wachtrijen (**standaard** en **thriftsvr**) beide zijn gewijzigd van 50% capaciteit 25% capaciteit, waardoor de nieuwe wachtrij (spark) 50% capaciteit.
| Wachtrij | Capaciteit | Maximale capaciteit |
| --- | --- | --- | --- |
| standaardinstelling | 25% | 50% |
| thrftsvr | 25% | 50% |
| Spark | 50% | 50% |

1. Selecteer de **Ambari-weergaven** pictogram en selecteer vervolgens het rasterpatroon. Selecteer vervolgens **YARN Queue Manager**.

    ![Selecteer het pictogram Ambari-weergaven](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. Selecteer de **standaard** wachtrij.

    ![Selecteer de wachtrij](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. Voor de **standaard** wachtrij, wijzigt u de **capaciteit** van 50% 25%. Voor de **thriftsvr** wachtrij, wijzigt u de **capaciteit** 25%.

    ![De capaciteit 25% voor de standaard- en thriftsvr wachtrijen wijzigen](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. Maak een nieuwe wachtrij, selecteer **wachtrij toevoegen**.

    ![Selecteer een wachtrij toevoegen](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. De naam van de nieuwe wachtrij.

    ![De naam van de wachtrij Spark](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. Laat de **capaciteit** waarden op 50% en selecteer vervolgens de **acties** knop.

    ![Selecteer de knop acties](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. Selecteer **opslaan en vernieuw wachtrijen**.

    ![Selecteer opslaan en wachtrijen vernieuwen](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

Deze wijzigingen zijn onmiddellijk voor de gebruikersinterface van YARN Scheduler zichtbaar.

### <a name="additional-reading"></a>Aanvullende bronnen

- [YARN CapacityScheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>Hoe kan ik YARN-logboeken downloaden vanaf een cluster?


### <a name="resolution-steps"></a>Stappen voor het oplossen 

1. Verbinding maken met het HDInsight-cluster met behulp van een client Secure Shell (SSH). Zie voor meer informatie [lezen van aanvullende](#additional-reading-2).

2. Als u alle toepassing-id's van de YARN-toepassingen die momenteel worden uitgevoerd, moet u de volgende opdracht uitvoeren:

    ```apache
    yarn top
    ```
    De id's worden vermeld in de **APPLICATIONID** kolom. U kunt downloaden logboeken van de **APPLICATIONID** kolom.

    ```apache
    YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
    NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
    Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
    Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
    Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
    Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                      APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
     application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
     application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
    ```

3. Als u wilt downloaden YARN-logboeken voor container voor alle modellen van toepassing, moet u de volgende opdracht gebruiken:
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    Met deze opdracht maakt een logbestand met de naam amlogs.txt. 

4. Als u wilt downloaden YARN-logboeken voor container voor de meest recente toepassing master, gebruik de volgende opdracht:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    Met deze opdracht maakt een logbestand met de naam latestamlogs.txt. 

4. Als u wilt downloaden YARN-logboeken voor container voor de eerste twee toepassing modellen, moet u de volgende opdracht gebruiken:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    Met deze opdracht maakt een logbestand met de naam first2amlogs.txt. 

5. Gebruik de volgende opdracht voor het downloaden van alle container YARN-Logboeken:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    Met deze opdracht maakt een logbestand met de naam logs.txt. 

6. Gebruik de volgende opdracht voor het downloaden van het logboek van de container YARN voor een specifieke container:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    Met deze opdracht maakt een logbestand met de naam containerlogs.txt.

### <a name="additional-reading-2"></a>Aanvullende bronnen

- [Verbinding maken met HDInsight (Hadoop) via SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [Apache Hadoop YARN-concepten en toepassingen](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)


### <a name="see-also"></a>Zie ook
[Problemen oplossen met behulp van Azure HDInsight](hdinsight-troubleshoot-guide.md)







