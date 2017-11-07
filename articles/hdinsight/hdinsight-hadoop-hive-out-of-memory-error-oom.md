---
title: Een component buiten geheugenfout in Azure HDInsight lossen | Microsoft Docs
description: Herstel van een component buiten geheugenfout in HDInsight. De klant-scenario is een query voor veel grote tabellen.
keywords: buiten-geheugeninstellingen fout, OOM, Hive
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 7bce3dff-9825-4fa0-a568-c52a9f7d1dad
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/06/2017
ms.author: jgao
ms.openlocfilehash: 3149147a95ac6e12b8866fbd03dd159de97c43cb
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="fix-a-hive-out-of-memory-error-in-azure-hdinsight"></a>Herstel van een component buiten geheugenfout in Azure HDInsight

Informatie over het oplossen van een component buiten geheugenfout verwerken wanneer grote tabellen met het configureren van instellingen voor Hive-geheugen.

## <a name="run-hive-query-against-large-tables"></a>Hive-query uitvoeren op grote tabellen

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

Sommige nuances van deze query:

* T1 is een alias voor een grote tabel Tabel1, waarvoor veel kolommen van het type tekenreeks.
* Andere tabellen zijn niet die groot maar hoeven veel kolommen.
* Alle tabellen zijn lid te worden van elkaar worden verbonden, in sommige gevallen met meerdere kolommen in TABLE1 en anderen.

De Hive-query 26 minuten duurde om te eindigen op een 24-knooppunt A3 HDInsight-cluster. De klant al opgemerkt dat de volgende waarschuwing weergegeven:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Met behulp van de Tez-uitvoeringsengine. Dezelfde query uitgevoerd gedurende 15 minuten en vervolgens heeft de volgende fout geretourneerd:

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

De fout blijft wanneer u een grotere virtuele machine (bijvoorbeeld D12).


## <a name="debug-the-out-of-memory-error"></a>Fouten opsporen in de fout onvoldoende geheugen

Onze support en engineeringteams samen gevonden bij een van de problemen waardoor de fout onvoldoende geheugen is een [bekend probleem dat wordt beschreven in de Apache-JIRA](https://issues.apache.org/jira/browse/HIVE-8306):

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

De **hive.auto.convert.join.noconditionaltask** in de hive-site.xml bestand is ingesteld op **true**:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
              Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
              If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
              specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
      </property>

Is het waarschijnlijk kaart join is de oorzaak van de ruimte van de Heap Java onze van geheugenfout. Zoals wordt beschreven in de blogbericht [Hadoop Yarn-geheugeninstellingen in HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx)wanneer Tez-uitvoeringsengine is gebruikt de heap gebruikte ruimte daadwerkelijk behoort tot de container Tez. Zie de volgende afbeelding met een beschrijving van het geheugen van de container Tez.

![Tez container geheugen diagram: Hive-fout: onvoldoende geheugen](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

Als het blogbericht wordt voorgesteld, de volgende twee geheugeninstellingen definiëren het geheugen van de container voor de heap: **hive.tez.container.size** en **hive.tez.java.opts**. Onze ervaring van betekent de buiten-geheugen-uitzondering niet dat de container is te klein. Dit betekent dat de Java-heap-grootte (hive.tez.java.opts) is te klein. Dus als er onvoldoende geheugen, u proberen kunt te verhogen **hive.tez.java.opts**. Indien nodig moet u mogelijk verhogen **hive.tez.container.size**. De **java.opts** instelling moet ongeveer 80% van **container.size**.

> [!NOTE]
> De instelling **hive.tez.java.opts** moet altijd kleiner zijn dan **hive.tez.container.size**.
> 
> 

Omdat een machine D12 28GB geheugen heeft, besloten we de grootte van een container van 10GB (10240MB) gebruiken en 80% toewijzen aan java.opts:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Met de nieuwe instellingen worden de query is uitgevoerd onder 10 minuten.

## <a name="next-steps"></a>Volgende stappen

Ophalen van een OOM fout betekent niet dat de container is te klein. In plaats daarvan moet u de geheugeninstellingen configureren zodat de heapgrootte is toegenomen en ten minste 80% van de container geheugengrootte. Zie voor het optimaliseren van Hive-query's [optimaliseren Hive-query's voor Hadoop in HDInsight](hdinsight-hadoop-optimize-hive-query.md).