---
title: Oplossen van Spark in Azure HDInsight
description: Vind antwoorden op veelgestelde vragen over het werken met Apache Spark en Azure HDInsight.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.date: 11/2/2017
ms.openlocfilehash: 835a56413a4274a2be2511278b4dedd7f83c54c3
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016672"
---
# <a name="troubleshoot-spark-by-using-azure-hdinsight"></a>Met Spark oplossen met behulp van Azure HDInsight

Meer informatie over de meest voorkomende problemen en hun oplossingen als u werkt met Apache Spark-nettoladingen in Apache Ambari.

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a>Hoe configureer ik een Spark-toepassing via Ambari op clusters?

### <a name="resolution-steps"></a>Oplossingen

De configuratiewaarden voor deze procedure zijn eerder ingesteld in HDInsight. Om te bepalen welke Spark configuraties moeten worden ingesteld en naar welke waarden, Zie [wat een Spark-toepassing OutofMemoryError uitzondering veroorzaakt](#what-causes-a-spark-application-outofmemoryerror-exception). 

1. Selecteer in de lijst met clusters, **Spark2**.

    ![Cluster selecteren in lijst](./media/apache-troubleshoot-spark/update-config-1.png)

2. Selecteer de **Peeringconfiguraties** tabblad.

    ![Selecteer het tabblad configuraties](./media/apache-troubleshoot-spark/update-config-2.png)

3. Selecteer in de lijst met configuraties **aangepaste-spark2-standaardinstellingen**.

    ![Selecteer de aangepaste-spark-standaardinstellingen](./media/apache-troubleshoot-spark/update-config-3.png)

4. Zoek de instelling van de waarde die u nodig hebt om aan te passen, zoals **spark.executor.memory**. In dit geval wordt de waarde van **4608m** is te hoog.

    ![Selecteer het veld spark.executor.memory](./media/apache-troubleshoot-spark/update-config-4.png)

5. Stel de waarde in op de aanbevolen instelling. De waarde **2048m** voor deze instelling wordt aanbevolen.

    ![Waarde wijzigen naar 2048 min.](./media/apache-troubleshoot-spark/update-config-5.png)

6. Sla de waarde en sla vervolgens de configuratie. Selecteer op de werkbalk **opslaan**.

    ![De instelling en de configuratie opslaan](./media/apache-troubleshoot-spark/update-config-6a.png)

    U wordt gewaarschuwd als de configuraties vereisen aandacht. Houd er rekening mee de items en selecteer vervolgens **toch doorgaan**. 

    ![Selecteer Doorgaan toch](./media/apache-troubleshoot-spark/update-config-6b.png)

    Voeg een opmerking over wijzigingen in de configuratie en selecteer vervolgens **opslaan**.

    ![Voer een opmerking over de wijzigingen die u hebt gemaakt](./media/apache-troubleshoot-spark/update-config-6c.png)

7. Wanneer een configuratie is opgeslagen, wordt u gevraagd de service opnieuw te starten. Selecteer **opnieuw**.

    ![Opnieuw opstarten selecteren](./media/apache-troubleshoot-spark/update-config-7a.png)

    Controleer of het opnieuw opstarten.

    ![Bevestigen selecteren Alles opnieuw starten](./media/apache-troubleshoot-spark/update-config-7b.png)

    De processen die worden uitgevoerd, kunt u bekijken.

    ![Actieve processen controleren](./media/apache-troubleshoot-spark/update-config-7c.png)

8. U kunt configuraties toevoegen. Selecteer in de lijst met configuraties **aangepaste-spark2-standaardinstellingen**, en selecteer vervolgens **eigenschap toevoegen**.

    ![Selecteer toevoegen de eigenschap](./media/apache-troubleshoot-spark/update-config-8.png)

9. Een nieuwe eigenschap definiëren. U kunt één eigenschap definiëren met behulp van een dialoogvenster voor specifieke instellingen, zoals het gegevenstype. Of u kunt meerdere eigenschappen definiëren met behulp van één definitie per regel. 

    In dit voorbeeld wordt de **spark.driver.memory** eigenschap is gedefinieerd met een waarde van **4g**.

    ![Nieuwe eigenschap definiëren](./media/apache-troubleshoot-spark/update-config-9.png)

10. De configuratie op te slaan en start de service opnieuw zoals beschreven in stap 6 en 7.

Deze wijzigingen zijn brede, door het cluster, maar kunnen worden overschreven wanneer u de Spark-taak verzendt.

### <a name="additional-reading"></a>Meer lezen

[Verzenden van taken voor Spark op HDInsight-clusters](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Hoe configureer ik een Spark-toepassing met behulp van een Jupyter-notebook op clusters?

### <a name="resolution-steps"></a>Oplossingen

1. Om te bepalen welke Spark configuraties moeten worden ingesteld en naar welke waarden, Zie [wat een Spark-toepassing OutofMemoryError uitzondering veroorzaakt](#what-causes-a-spark-application-outofmemoryerror-exception).

2. In de eerste cel van de Jupyter-notebook nadat de **%% configureren** richtlijn, geeft u de Spark-configuraties in geldige JSON-indeling. De werkelijke waarden zo nodig wijzigen:

    ![Een configuratie toevoegen](./media/apache-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a>Meer lezen

[Verzenden van taken voor Spark op HDInsight-clusters](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a>Hoe configureer ik een Spark-toepassing met behulp van Livy op clusters?

### <a name="resolution-steps"></a>Oplossingen

1. Om te bepalen welke Spark configuraties moeten worden ingesteld en naar welke waarden, Zie [wat een Spark-toepassing OutofMemoryError uitzondering veroorzaakt](#what-causes-a-spark-application-outofmemoryerror-exception). 

2. De Spark-toepassing naar Livy indienen met behulp van een REST-client, zoals cURL. Een vergelijkbaar met de volgende opdracht gebruiken. De werkelijke waarden zo nodig wijzigen:

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a>Meer lezen

[Verzenden van taken voor Spark op HDInsight-clusters](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a>Hoe configureer ik een toepassing met behulp van spark-submit Spark in clusters?

### <a name="resolution-steps"></a>Oplossingen

1. Om te bepalen welke Spark configuraties moeten worden ingesteld en naar welke waarden, Zie [wat een Spark-toepassing OutofMemoryError uitzondering veroorzaakt](#what-causes-a-spark-application-outofmemoryerror-exception).

2. Spark-shell met behulp van een vergelijkbaar met de volgende opdracht start. De werkelijke waarde van de configuraties zo nodig wijzigen: 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a>Meer lezen

[Verzenden van taken voor Spark op HDInsight-clusters](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a>Wat een Spark-toepassing OutofMemoryError uitzondering veroorzaakt?

### <a name="detailed-description"></a>Gedetailleerde beschrijving

De Spark-toepassing mislukt, met de volgende typen niet-onderschepte uitzonderingen:

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

De meest waarschijnlijke oorzaak van deze uitzondering is dat niet voldoende heapgeheugen is toegewezen aan de Java virtual machines (JVMs). Deze JVMs worden gestart als uitvoerder of stuurprogramma's als onderdeel van de Spark-toepassing. 

### <a name="resolution-steps"></a>Oplossingen

1. Bepaal de maximale grootte van de gegevens van de Spark toepassing verwerkt. U kunt een schatting op basis van de maximale grootte van de ingevoerde gegevens, de tussentijdse gegevens die wordt geproduceerd door de ingevoerde gegevens transformeren en de uitvoergegevens die wordt gemaakt wanneer de toepassing meer de tussentijdse gegevens transformeert. Dit proces is een iteratief als u een initiële formele schatting niet maken. 

2. Zorg ervoor dat het HDInsight-cluster dat u gaat gebruiken onvoldoende bronnen in termen van geheugen en kernen heeft voor de Spark-toepassing. U kunt dit vaststellen door te bekijken van de sectie cluster metrische gegevens van de gebruikersinterface van YARN voor de waarden van **geheugen gebruikt** vs. **Totaal geheugen**, en **VCores gebruikt** vs. **Totaal aantal VCores**.

3. De volgende Spark-configuraties instellen op de juiste waarden, dit mag niet meer dan 90% van het beschikbare geheugen en kernen. De waarden moeten binnen de geheugenvereisten van de Spark-toepassing: 

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

### <a name="additional-reading"></a>Meer lezen

- [Overzicht van Spark geheugen](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [Fouten opsporen in een Spark-toepassing op een HDInsight-cluster](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)


### <a name="see-also"></a>Zie ook
[Problemen oplossen met behulp van Azure HDInsight](../../hdinsight/hdinsight-troubleshoot-guide.md)

