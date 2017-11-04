---
title: Spark oplossen met behulp van Azure HDInsight | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over het werken met Apache Spark en Azure HDInsight.
keywords: Azure HDInsight, Spark, veelgestelde vragen, het oplossen van de handleiding, bekende problemen, de configuratie van de toepassing, Ambari
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: 25D89586-DE5B-4268-B5D5-CC2CE12207ED
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/2/2017
ms.author: arijitt
ms.openlocfilehash: b75aca7d9f78cb425099cc33034b44a80d7b6b81
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-spark-by-using-azure-hdinsight"></a>Spark oplossen met behulp van Azure HDInsight

Meer informatie over de meest voorkomende problemen en hun oplossingen bij het werken met Apache Spark nettoladingen in Apache Ambari.

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a>Hoe kan ik een Spark-toepassing configureren met behulp van Ambari op clusters?

### <a name="resolution-steps"></a>Stappen voor het oplossen

De configuratiewaarden voor deze procedure zijn eerder in HDInsight ingesteld. Om te bepalen welke Spark configuraties moeten worden ingesteld en op welke waarden Zie [wat een Spark-toepassing OutofMemoryError uitzondering veroorzaakt](#what-causes-a-spark-application-outofmemoryerror-exception). 

1. Selecteer in de lijst van clusters **Spark2**.

    ![Selecteer de cluster in lijst](./media/apache-troubleshoot-spark/update-config-1.png)

2. Selecteer de **Configs** tabblad.

    ![Selecteer het tabblad Configs](./media/apache-troubleshoot-spark/update-config-2.png)

3. Selecteer in de lijst van configuraties **standaardinstellingen van aangepaste spark2**.

    ![Selecteer de aangepaste-spark-standaardinstellingen](./media/apache-troubleshoot-spark/update-config-3.png)

4. Zoek naar de waarde-instelling die u aanpassen wilt, zoals **spark.executor.memory**. In dit geval wordt de waarde van **4608m** is te hoog.

    ![Selecteer het veld spark.executor.memory](./media/apache-troubleshoot-spark/update-config-4.png)

5. Stel de waarde voor de aanbevolen instelling. De waarde **2048m** voor deze instelling wordt aanbevolen.

    ![Waarde wijzigen in 2048m](./media/apache-troubleshoot-spark/update-config-5.png)

6. Sla de waarde en sla de configuratie. Selecteer op de werkbalk **opslaan**.

    ![Sla de configuratie en de instelling](./media/apache-troubleshoot-spark/update-config-6a.png)

    U wordt gewaarschuwd als alle configuraties aandacht vereisen. Houd rekening met het en selecteer vervolgens **toch doorgaan**. 

    ![Selecteer wilt u toch doorgaan](./media/apache-troubleshoot-spark/update-config-6b.png)

    Voeg een opmerking over wijzigingen in de configuratie en selecteer vervolgens **opslaan**.

    ![Voer een opmerking over de aangebrachte wijzigingen](./media/apache-troubleshoot-spark/update-config-6c.png)

7. Telkens wanneer een configuratie wordt opgeslagen, wordt u gevraagd de service te starten. Selecteer **opnieuw**.

    ![Opnieuw opstarten selecteren](./media/apache-troubleshoot-spark/update-config-7a.png)

    Bevestig het opnieuw opstarten.

    ![Selecteer bevestigen alle opnieuw starten](./media/apache-troubleshoot-spark/update-config-7b.png)

    De processen die worden uitgevoerd, kunt u bekijken.

    ![Bekijk actieve processen](./media/apache-troubleshoot-spark/update-config-7c.png)

8. U kunt configuraties kunt toevoegen. Selecteer in de lijst van configuraties **standaardinstellingen van aangepaste spark2**, en selecteer vervolgens **eigenschap toevoegen**.

    ![Selecteer een eigenschap toevoegen](./media/apache-troubleshoot-spark/update-config-8.png)

9. Een nieuwe eigenschap definiëren. U kunt één eigenschap definiëren met behulp van een dialoogvenster voor specifieke instellingen, zoals het gegevenstype. Of u kunt meerdere eigenschappen definiëren met behulp van één definitie per regel. 

    In dit voorbeeld wordt de **spark.driver.memory** eigenschap is gedefinieerd met een waarde van **4g**.

    ![Nieuwe eigenschap definiëren](./media/apache-troubleshoot-spark/update-config-9.png)

10. De configuratie op te slaan en start de service opnieuw zoals beschreven in stap 6 en 7.

Deze wijzigingen zijn hele cluster, maar kunnen worden genegeerd wanneer u de taak Spark indient.

### <a name="additional-reading"></a>Aanvullende bronnen

[Verzending van de taak Spark in HDInsight-clusters](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Hoe kan ik een Spark-toepassing configureren met behulp van een Jupyter-notebook in clusters?

### <a name="resolution-steps"></a>Stappen voor het oplossen

1. Om te bepalen welke Spark configuraties moeten worden ingesteld en op welke waarden Zie [wat een Spark-toepassing OutofMemoryError uitzondering veroorzaakt](#what-causes-a-spark-application-outofmemoryerror-exception).

2. In de eerste cel van de Jupyter-notebook nadat de **%% configureren** richtlijn, geeft u de Spark-configuraties in geldige JSON-indeling. De werkelijke waarden zo nodig wijzigen:

    ![Een configuratie toevoegen](./media/apache-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a>Aanvullende bronnen

[Verzending van de taak Spark in HDInsight-clusters](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a>Hoe kan ik een Spark-toepassing configureren met behulp van Livy op clusters?

### <a name="resolution-steps"></a>Stappen voor het oplossen

1. Om te bepalen welke Spark configuraties moeten worden ingesteld en op welke waarden Zie [wat een Spark-toepassing OutofMemoryError uitzondering veroorzaakt](#what-causes-a-spark-application-outofmemoryerror-exception). 

2. De Spark aanvraag indienen bij Livy met behulp van een REST-client, zoals cURL. Een vergelijkbaar met de volgende opdracht gebruiken. De werkelijke waarden zo nodig wijzigen:

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a>Aanvullende bronnen

[Verzending van de taak Spark in HDInsight-clusters](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a>Hoe configureer een toepassing met behulp van spark indienen Spark op clusters?

### <a name="resolution-steps"></a>Stappen voor het oplossen

1. Om te bepalen welke Spark configuraties moeten worden ingesteld en op welke waarden Zie [wat een Spark-toepassing OutofMemoryError uitzondering veroorzaakt](#what-causes-a-spark-application-outofmemoryerror-exception).

2. Spark-shell start met behulp van een vergelijkbaar met de volgende opdracht. De werkelijke waarde van de configuraties zo nodig wijzigen: 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a>Aanvullende bronnen

[Verzending van de taak Spark in HDInsight-clusters](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a>Wat is de oorzaak een Spark OutofMemoryError Toepassingsuitzondering?

### <a name="detailed-description"></a>Gedetailleerde beschrijving

De Spark-toepassing mislukt, met de volgende typen van niet-onderschepte uitzonderingen:

```apache
ERROR Executor: Exception in task 7.0 in stage 6.0 (TID 439) 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

```apache
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main] 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

### <a name="probable-cause"></a>Mogelijke oorzaak

De meest waarschijnlijke oorzaak van deze uitzondering is dat er niet voldoende geheugen wordt toegewezen aan de virtuele machines voor Java (JVMs). Deze JVMs worden gestart als Executor of stuurprogramma's als onderdeel van de Spark-toepassing. 

### <a name="resolution-steps"></a>Stappen voor het oplossen

1. De maximale grootte van de gegevens de Spark bepalen toepassing verwerkt. U kunt een schatting op basis van de maximale grootte van de invoergegevens, de tussenliggende gegevens die wordt geproduceerd door de invoergegevens transformeren en de uitvoergegevens die wordt gemaakt wanneer de toepassing verder van de tussenliggende gegevens omzetten is maken. Dit proces is een iteratief als u een initiële formele schatting niet maken. 

2. Zorg ervoor dat het HDInsight-cluster dat u gaat gebruiken heeft onvoldoende bronnen in termen van geheugen en kernen om de toepassing Spark onder te brengen. U kunt dit bepalen door het bekijken van de sectie cluster metrische gegevens van de gebruikersinterface van YARN voor de waarden van **geheugen gebruikt** vs. **Totaal geheugen**, en **VCores gebruikt** vs. **Totaal aantal VCores**.

3. De volgende Spark-configuraties naar de juiste waarden, dit mag niet meer dan 90% van het beschikbare geheugen en kernen ingesteld. De waarden moeten binnen de geheugenvereisten van de Spark-toepassing: 

    ```apache
    spark.executor.instances (Example: 8 for 8 executor count) 
    spark.executor.memory (Example: 4g for 4 GB) 
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
    spark.executor.cores (Example: 2 for 2 cores per executor) 
    spark.driver.memory (Example: 8g for 8GB) 
    spark.driver.cores (Example: 4 for 4 cores)   
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
    ```

    Aan de totale hoeveelheid geheugen die wordt gebruikt door alle Executor Low: 
    
    ```apache
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```
   Aan de totale hoeveelheid geheugen die wordt gebruikt door het stuurprogramma Low:
    
    ```apache
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

### <a name="additional-reading"></a>Aanvullende bronnen

- [Overzicht van Spark geheugen](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [Fouten opsporen in een Spark-toepassing op een HDInsight-cluster](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)


### <a name="see-also"></a>Zie ook
[Problemen oplossen met behulp van Azure HDInsight](../../hdinsight/hdinsight-troubleshoot-guide.md)

