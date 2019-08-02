---
title: OutOfMemoryError-uitzonde ring voor Apache Spark in azure HDInsight
description: OutOfMemoryError-uitzonde ring voor Apache Spark in azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/26/2019
ms.openlocfilehash: ac360312fdb3c4a238e6280872bc8c8b548e8544
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68620985"
---
# <a name="scenario-outofmemoryerror-exception-for-apache-spark-in-azure-hdinsight"></a>Scenario: OutOfMemoryError-uitzonde ring voor Apache Spark in azure HDInsight

In dit artikel worden probleemoplossings stappen en mogelijke oplossingen voor problemen beschreven bij het gebruik van Apache Spark-onderdelen in azure HDInsight-clusters.

## <a name="issue"></a>Probleem

Uw Apache Spark-toepassing is mislukt met een onverwerkte OutOfMemoryError-uitzonde ring. Er wordt mogelijk een fout bericht met de volgende strekking weer gegeven:

```error
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

```error
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main]

java.lang.OutOfMemoryError
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source)
    ...
```

## <a name="cause"></a>Oorzaak

De meest waarschijnlijke oorzaak van deze uitzonde ring is onvoldoende geheugen op de opslag ruimte. Uw Spark-toepassing vereist voldoende JVM-heap-geheugen (Java Virtual Machines) wanneer u als uitvoerender of stuur programma wordt uitgevoerd.

## <a name="resolution"></a>Oplossing

1. Bepaal de maximale grootte van de gegevens die de Spark-toepassing moet verwerken. Maak een schatting van de grootte op basis van het maximum van de invoer gegevens, de tussenliggende gegevens die worden geproduceerd door het transformeren van de invoer gegevens en de uitvoer gegevens die de tussenliggende gegevens hebben getransformeerd. Als de oorspronkelijke schatting niet voldoende is, verg root u de grootte en herhaalt u dit tot de geheugen fouten.

1. Zorg ervoor dat het HDInsight-cluster dat u wilt gebruiken, over voldoende resources (geheugen en kernen) beschikt voor uitvoering van de Spark-toepassing. Raadpleeg om dit te bepalen het gedeelte Cluster Metrics in de YARN-gebruikersinterface van het cluster. Hier ziet u de waarden voor het gebruikte geheugen (Memory Used) versus het totale geheugen (Memory Total) en de waarden voor het aantal gebruikte kernen (VCores Used) versus het totale aantal kernen (VCores Total).

    ![kern geheugen weergave van garen](./media/apache-spark-ts-outofmemory/yarn-core-memory-view.png)

1. Stel de volgende Spark-configuraties in op de juiste waarden. De toepassings vereisten te verdelen over de beschik bare resources in het cluster. Deze waarden mogen niet groter zijn dan 90% van het beschik bare geheugen en de kernen die door GARENs worden weer gegeven en moet ook voldoen aan de minimale geheugen vereisten van de Spark-toepassing:

    ```
    spark.executor.instances (Example: 8 for 8 executor count)
    spark.executor.memory (Example: 4g for 4 GB)
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB)
    spark.executor.cores (Example: 2 for 2 cores per executor)
    spark.driver.memory (Example: 8g for 8GB)
    spark.driver.cores (Example: 4 for 4 cores)
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB)
    ```

    Totaal geheugen dat wordt gebruikt door alle uitvoerders =

    ```
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```

    Totaal geheugen gebruikt door stuur programma =

    ```
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* [Overzicht van Spark-geheugen beheer](https://spark.apache.org/docs/latest/tuning.html#memory-management-overview).

* [Fout opsporing van Spark-toepassing op HDInsight-clusters](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/).

* Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

* Maak verbinding [@AzureSupport](https://twitter.com/azuresupport) met-het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring door de Azure-community te verbinden met de juiste resources: antwoorden, ondersteuning en experts.

* Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees voor meer gedetailleerde informatie [hoe u een ondersteunings aanvraag voor Azure maakt](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). De toegang tot abonnementen voor abonnements beheer en facturering is inbegrepen bij uw Microsoft Azure-abonnement en technische ondersteuning wordt geleverd via een van de ondersteunings [abonnementen voor Azure](https://azure.microsoft.com/support/plans/).
