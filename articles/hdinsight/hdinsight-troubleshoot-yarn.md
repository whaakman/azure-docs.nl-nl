---
title: Problemen met YARN in Azure HDInsight oplossen
description: Vind antwoorden op veelgestelde vragen over het werken met Apache Hadoop YARN en Azure HDInsight.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.date: 11/2/2017
ms.openlocfilehash: c3904286fc998769b9a72522bd31d3a9797c55c2
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51008042"
---
# <a name="troubleshoot-yarn-by-using-azure-hdinsight"></a>Met YARN oplossen met behulp van Azure HDInsight

Meer informatie over de meest voorkomende problemen en hun oplossingen als u werkt met Apache Hadoop YARN-nettoladingen in Apache Ambari.

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>Hoe maak ik een nieuwe YARN-wachtrij op een cluster?


### <a name="resolution-steps"></a>Oplossingen 

De volgende stappen uit in Ambari gebruiken om te maken van een nieuwe YARN-wachtrij en vervolgens de capaciteitstoewijzing tussen alle wachtrijen in balans brengen. 

In dit voorbeeld twee bestaande wachtrijen (**standaard** en **thriftsvr**) beide worden gewijzigd van 50% capaciteit aan 25% capaciteit, waardoor de nieuwe capaciteit van de wachtrij (spark)-50%.
| Wachtrij | Capaciteit | Maximale capaciteit |
| --- | --- | --- | --- |
| standaardinstelling | 25% | 50% |
| thrftsvr | 25% | 50% |
| Spark | 50% | 50% |

1. Selecteer de **Ambari-weergaven** pictogram en selecteer vervolgens het rasterpatroon. Selecteer vervolgens **YARN wachtrijbeheerder**.

    ![Selecteer het pictogram van de Ambari-weergaven](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. Selecteer de **standaard** wachtrij.

    ![Selecteer de standaardwachtrij](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. Voor de **standaard** in de wachtrij, wijzigt u de **capaciteit** van 50% tot 25%. Voor de **thriftsvr** in de wachtrij, wijzigt u de **capaciteit** 25%.

    ![De capaciteit aan 25% van de standaard- en thriftsvr wachtrijen wijzigen](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. Voor het maken van een nieuwe wachtrij selecteert **wachtrij toevoegen**.

    ![Selecteer de wachtrij toevoegen](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. De naam van de nieuwe wachtrij.

    ![De naam van de wachtrij Spark](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. Laat de **capaciteit** waarden op 50% en selecteer vervolgens de **acties** knop.

    ![Selecteer de knop acties](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. Selecteer **slaan en te vernieuwen wachtrijen**.

    ![Selecteer opslaan en vernieuwen van wachtrijen](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

Deze wijzigingen zijn zichtbaar onmiddellijk op de gebruikersinterface van YARN Scheduler.

### <a name="additional-reading"></a>Meer lezen

- [YARN CapacityScheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>Hoe kan ik YARN-logboeken downloaden uit een cluster?


### <a name="resolution-steps"></a>Oplossingen 

1. Verbinding maken met het HDInsight-cluster met behulp van een Secure Shell (SSH)-client. Zie voor meer informatie, [meer lezen](#additional-reading-2).

2. Als u de toepassing-id's voor de YARN-toepassingen die momenteel worden uitgevoerd, moet u de volgende opdracht uitvoeren:

    ```apache
    yarn top
    ```
    De id's worden vermeld in de **APPLICATIONID** kolom. U kunt Logboeken downloaden de **APPLICATIONID** kolom.

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

3. YARN-container om Logboeken te downloaden voor alle toepassingen masters, gebruik de volgende opdracht:
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    Deze opdracht maakt een logboekbestand met de naam amlogs.txt. 

4. Als u wilt downloaden YARN-logboeken voor containers voor de meest recente toepassing master, gebruik de volgende opdracht:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    Deze opdracht maakt een logboekbestand met de naam latestamlogs.txt. 

4. Als u wilt downloaden YARN-logboeken voor containers voor de eerste twee toepassing masters, gebruik de volgende opdracht:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    Deze opdracht maakt een logboekbestand met de naam first2amlogs.txt. 

5. Gebruik de volgende opdracht voor het downloaden van alle YARN-logboeken voor containers:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    Deze opdracht maakt een logboekbestand met de naam logs.txt. 

6. Gebruik de volgende opdracht voor het downloaden van de container YARN-logboek voor een specifieke container:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    Deze opdracht maakt een logboekbestand met de naam containerlogs.txt.

### <a name="additional-reading-2"></a>Meer lezen

- [Verbinding maken met HDInsight (Hadoop) via SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [Apache Hadoop YARN-concepten en toepassingen](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)


### <a name="see-also"></a>Zie ook
[Problemen oplossen met behulp van Azure HDInsight](hdinsight-troubleshoot-guide.md)







