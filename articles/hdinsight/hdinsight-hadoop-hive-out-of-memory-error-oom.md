---
title: Herstellen van een component fout door onvoldoende geheugen in Azure HDInsight
description: Herstellen van een component fout door onvoldoende geheugen in HDInsight. De klant-scenario is een query voor veel grote tabellen.
keywords: uit geheugen fout, OOM, Hive-instellingen
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: hrasheed
ms.openlocfilehash: 9aff828dcb9dfea6d5f35ad92bb09ba7cd802fea
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53711853"
---
# <a name="fix-an-apache-hive-out-of-memory-error-in-azure-hdinsight"></a>Herstellen van een Apache Hive fout door onvoldoende geheugen in Azure HDInsight

Meer informatie over het oplossen van een Apache Hive fout door onvoldoende geheugen (OOM) bij het verwerken van grote tabellen door het configureren van instellingen voor Hive-geheugen.

## <a name="run-apache-hive-query-against-large-tables"></a>Apache Hive-query uitvoeren op grote tabellen

Een klant een Hive-query is uitgevoerd:

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

Sommige aspecten van deze query:

* T1 is een alias voor een grote tabel, Tabel1, die voorzien van tal van kolommen van het type tekenreeks is.
* Andere tabellen worden niet dat grote maar hoeven veel kolommen.
* Alle tabellen samenvoegt elkaar worden verbonden, in sommige gevallen met meerdere kolommen in Tabel1 en anderen.

De Hive-query duurde 26 minuten in een 24-knooppunt A3 HDInsight-cluster. De klant al opgemerkt dat de volgende waarschuwingen:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Met behulp van de Apache Tez-uitvoeringsengine. Dezelfde query uitgevoerd gedurende 15 minuten en heeft vervolgens de volgende fout geretourneerd:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

De fout blijft bij het gebruik van een grotere virtuele machine (bijvoorbeeld, D12).


## <a name="debug-the-out-of-memory-error"></a>Fouten opsporen in de buiten-geheugen

De ondersteuning en de engineering-teams samen te vinden was een van de problemen waardoor de out-of geheugenfout een [bekend probleem dat wordt beschreven in het Apache-JIRA](https://issues.apache.org/jira/browse/HIVE-8306):

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

De **hive.auto.convert.join.noconditionaltask** in de hive-site.xml bestand is ingesteld op **waar**:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
              Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
              If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
              specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
      </property>

Is het waarschijnlijk kaart join is de oorzaak van de Java-Heap-ruimte onze van geheugen. Zoals uitgelegd in het blogbericht [Hadoop Yarn-geheugeninstellingen in HDInsight](https://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx)wanneer Tez-uitvoeringsengine wordt gebruikt de heap gebruikte ruimte daadwerkelijk behoort tot de Tez-container. Zie de volgende afbeelding met een beschrijving van het geheugen van de container Tez.

![Tez container geheugen diagram: Hive-fout door onvoldoende geheugen](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

Als het blogbericht: al aangeeft, de volgende twee geheugeninstellingen het geheugen van de container voor de heap definiëren: **hive.tez.container.size** en **hive.tez.java.opts**. Uit onze ervaring van betekent het buiten-geheugen-uitzondering niet dat de container is te klein. Dit betekent dat de Java-heap-grootte (hive.tez.java.opts) is te klein. Dus wanneer er onvoldoende geheugen, u proberen kunt te verhogen **hive.tez.java.opts**. Indien nodig u mogelijk verhogen **hive.tez.container.size**. De **java.opts** instelling moet ongeveer 80% van de **container.size**.

> [!NOTE]  
> De instelling **hive.tez.java.opts** moet altijd kleiner zijn dan **hive.tez.container.size**.
> 
> 

Omdat een machine D12 28GB geheugen heeft, besloten we de containergrootte van een van 10GB (10240MB) gebruiken en 80% toewijzen aan java.opts:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Met de nieuwe instellingen, worden de query is uitgevoerd in minder dan 10 minuten.

## <a name="next-steps"></a>Volgende stappen

Een foutmelding krijgt OOM betekent niet dat de container is te klein. In plaats daarvan moet u de instellingen van het geheugen configureren zodat de heapgrootte wordt verhoogd en ten minste 80% van de grootte van de container-geheugen wordt. Zie voor het optimaliseren van Hive-query's, [optimaliseren Apache Hive-query's voor Apache Hadoop in HDInsight](hdinsight-hadoop-optimize-hive-query.md).
