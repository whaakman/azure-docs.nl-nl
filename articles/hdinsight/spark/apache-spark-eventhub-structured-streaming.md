---
title: Gebruik Apache Spark gestructureerd streaming met Event Hubs in Azure HDInsight | Microsoft Docs
description: Een Apache Spark-streaming-codevoorbeeld voor het verzenden van een gegevensstroom naar Azure Event Hub en vervolgens deze gebeurtenissen ontvangen in HDInsight Spark-cluster met een scala-toepassing bouwen.
keywords: Apache spark-streaming, spark-streaming, spark-voorbeeld, apache spark-streaming bijvoorbeeld event hub azure voorbeeld, spark-voorbeeld
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jgao
ms.openlocfilehash: e0486d2c5f78da1d1e4a12703f120eccef43c305
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="apache-spark-structured-streaming-on-hdinsight-to-process-events-from-event-hubs"></a>Apache Spark gestructureerde Streaming op HDInsight om gebeurtenissen te verwerken van Event Hubs

In dit artikel leert u realtime telemetrie met gestructureerde Spark-Streaming verwerken. Hiervoor moet u de volgende geavanceerde stappen uitvoeren:

1. Compileren en uitvoeren op uw lokale werkstation gebeurtenis producent met een voorbeeldtoepassing die gebeurtenissen genereert verzenden naar Event Hubs.
2. Gebruik de [Spark Shell](apache-spark-shell.md) om te definiëren en een eenvoudige gestructureerde Spark-Streaming-toepassing uitvoeren.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Een Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

* Een Azure Event Hubs-naamruimte. Zie [maken van een Azure Event Hubs-naamruimte](apache-spark-eventhub-streaming.md#create-an-azure-event-hub) voor meer informatie.

* Oracle Java Development kit. Kunt u het installeren van [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

* Apache Maven. U kunt downloaden van [hier](https://maven.apache.org/download.cgi). Vindt u instructies voor het installeren van Maven [hier](https://maven.apache.org/install.html).

## <a name="build-configure-and-run-the-event-producer"></a>Maken, configureren en uitvoeren van de producent gebeurtenis
In deze taak kunt u een voorbeeldtoepassing die u maakt willekeurige gebeurtenissen en stuurt deze naar een geconfigureerde Event Hub klonen. Deze voorbeeldtoepassing is beschikbaar in GitHub op [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).

1. Zorg ervoor dat u hebt de git-hulpprogramma's geïnstalleerd. U kunt downloaden van [GIT downloadt](http://www.git-scm.com/downloads). 
2. Open een opdrachtprompt of terminal shell en voer de volgende opdracht uit de directory van uw keuze voor het klonen van het project.
        
        git clone https://github.com/hdinsight/eventhubs-sample-event-producer.git eventhubs-client-sample

3. Hiermee maakt u een nieuwe map met de naam eventhubs-client-sample. Navigeer naar deze map binnen de shell of opdrachtprompt.
4. Maven om samen te stellen van de toepassing met behulp van de volgende opdracht uitvoeren:

          mvn package

5. Binnen de shell of opdrachtprompt, gaat u naar de doelmap dat is gemaakt en het bestand bevat ``com-microsoft-azure-eventhubs-client-example-0.2.0.jar``.
6. Vervolgens moet u voor het bouwen van de opdrachtregel voor het uitvoeren van de producent gebeurtenis op uw Event Hub. Dit doen door Vervang de waarden in de opdracht als volgt:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.azure.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "<namespace>" --eventhubs-name "hub1" --policy-name "RootManageSharedAccessKey" --policy-key "<policyKey>" --message-length 32 --thread-count 1 --message-count -1

7. Bijvoorbeeld, eruit een volledige opdracht ziet er als volgt:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.azure.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "sparkstreaming" --eventhubs-name "hub1" --policy-name "RootManageSharedAccessKey" --policy-key "2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU=" --message-length 32 --thread-count 1 --message-count -1

8. De opdracht wordt gestart en als de configuratie juist over enkele ogenblikken ziet u uitvoer die betrekking hebben op de gebeurtenissen die is het verzenden naar uw Event Hub, vergelijkbaar met het volgende:

        Map('policyKey -> 2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU, 'eventhubsName -> hub1, 'policyName -> RootManageSharedAccessKey, 'eventhubsNamespace -> sparkstreaming, 'messageCount -> -1, 'messageLength -> 32, 'threadCount -> 1)
        Events per thread: -1 (-1 for unlimited)
        10 > Sun Jun 18 11:32:58 PDT 2017 > 1024 > 1024 > Sending event: ZZ93958saG5BUKbvUI9wHVmpuA2TrebS
        10 > Sun Jun 18 11:33:46 PDT 2017 > 2048 > 2048 > Sending event: RQorGRbTPp6U2wYzRSnZUlWEltRvTZ7R
        10 > Sun Jun 18 11:34:33 PDT 2017 > 3072 > 3072 > Sending event: 36Eoy2r8ptqibdlfCYSGgXe6ct4AyOX3
        10 > Sun Jun 18 11:35:19 PDT 2017 > 4096 > 4096 > Sending event: bPZma9V0CqOn6Hj9bhrrJT0bX2rbPSn3
        10 > Sun Jun 18 11:36:06 PDT 2017 > 5120 > 5120 > Sending event: H2TVD77HNTVyGsVcj76g0daVnYxN4Sqs

9. Laat de maker van de gebeurtenis die wordt uitgevoerd terwijl u door te gaan naar de stappen.

## <a name="run-spark-shell-on-your-hdinsight-cluster"></a>Spark Shell uitvoeren op uw HDInsight-cluster
In deze taak wordt u SSH in het hoofdknooppunt van uw HDInsight-cluster, start de Spark-Shell en voert u een Spark-Streaming-toepassing die worden opgehaald en worden de gebeurtenissen van Event Hubs verwerkt. 

Door dit punt moet uw HDInsight-cluster klaar. Als dat niet het geval is, moet u wachten totdat het klaar is met het inrichten. Zodra deze gereed is, kunt u doorgaan met de volgende stappen uit:

1. Verbinding maken met het HDInsight-cluster via SSH. Zie voor instructies [verbinding maken met HDInsight met behulp van SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

5. De toepassing die u maakt moet het pakket Spark Streaming Event Hubs. De Spark-Shell uitgevoerd zodat deze automatisch deze afhankelijkheid van opgehaald [Maven centrale](https://search.maven.org), zorg ervoor dat de levering de pakketten switch met de coördinaten Maven als volgt:

        spark-shell --packages "com.microsoft.azure:spark-streaming-eventhubs_2.11:2.1.5"

6. Zodra de Spark-Shell wordt geladen, ziet u:

        Welcome to
            ____              __
            / __/__  ___ _____/ /__
            _\ \/ _ \/ _ `/ __/  '_/
        /___/ .__/\_,_/_/ /_/\_\   version 2.1.1.2.6.2.3-1
            /_/
                
        Using Scala version 2.11.8 (OpenJDK 64-Bit Server VM, Java 1.8.0_151)
        Type in expressions to have them evaluated.
        Type :help for more information.

        scala> 

7. Kopieer het volgende codefragment in een teksteditor en wijzig het zodat hieraan de beleid-sleutel en Namespace Stel voor uw Event Hub.

        val eventhubParameters = Map[String, String] (
            "eventhubs.policyname" -> "RootManageSharedAccessKey",
            "eventhubs.policykey" -> "<policyKey>",
            "eventhubs.namespace" -> "<namespace>",
            "eventhubs.name" -> "hub1",
            "eventhubs.partition.count" -> "2",
            "eventhubs.consumergroup" -> "$Default",
            "eventhubs.progressTrackingDir" -> "/eventhubs/progress",
            "eventhubs.sql.containsProperties" -> "true"
            )
            
8. Als u uw EventHub-compatibele eindpunt in de volgende vorm bekijkt, worden het onderdeel leest `iothub-xxxxxxxxxx` is de naam van uw Namespace EventHub-compatibel en kan worden gebruikt voor `eventhubs.namespace`. Het veld `SharedAccessKeyName` kan worden gebruikt voor `eventhubs.policyname`, en `SharedAccessKey` voor `eventhubs.policykey`: 

        Endpoint=sb://iothub-xxxxxxxxxx.servicebus.windows.net/;SharedAccessKeyName=xxxxx;SharedAccessKey=xxxxxxxxxx 

9. Plak het gewijzigde fragment in het scala wachten >-prompt en druk op return. Dit is het geval als de uitvoer er ongeveer zo uitziet:

        scala> val eventhubParameters = Map[String, String] (
            |       "eventhubs.policyname" -> "RootManageSharedAccessKey",
            |       "eventhubs.policykey" -> "2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU",
            |       "eventhubs.namespace" -> "sparkstreaming",
            |       "eventhubs.name" -> "hub1",
            |       "eventhubs.partition.count" -> "2",
            |       "eventhubs.consumergroup" -> "$Default",
            |       "eventhubs.progressTrackingDir" -> "/eventhubs/progress",
            |       "eventhubs.sql.containsProperties" -> "true"
            |     )
        eventhubParameters: scala.collection.immutable.Map[String,String] = Map(eventhubs.sql.containsProperties -> true, eventhubs.name -> hub1, eventhubs.consumergroup -> $Default, eventhubs.partition.count -> 2, eventhubs.progressTrackingDir -> /eventhubs/progress, eventhubs.policykey -> 2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU, eventhubs.namespace -> hdiz-docs-eventhubs, eventhubs.policyname -> RootManageSharedAccessKey)

10. Vervolgens maakt u begint met het ontwerpen van een Spark gestructureerde Streaming query worden de bron opgeven. Plak het volgende in Spark-Shell en druk op return.

        val inputStream = spark.readStream.
        format("eventhubs").
        options(eventhubParameters).
        load()

11. Dit is het geval als de uitvoer er ongeveer zo uitziet:

        inputStream: org.apache.spark.sql.DataFrame = [body: binary, offset: bigint ... 5 more fields]

12. Vervolgens de machtigingen van de query zodat deze de uitvoer ervan weergegeven in de Console geschreven. Dit doen door de volgende plakken in Spark-Shell en drukt u op return.

        val streamingQuery1 = inputStream.writeStream.
        outputMode("append").
        format("console").start().awaitTermination()

13. U ziet enkele batches starten met de uitvoer ziet er als volgt

        -------------------------------------------
        Batch: 0
        -------------------------------------------
        [Stage 0:>                                                          (0 + 2) / 2]

14. Dit wordt gevolgd door de resultaten van de uitvoer van de verwerking van elke microbatch van gebeurtenissen. 

        -------------------------------------------
        Batch: 0
        -------------------------------------------
        17/06/18 18:57:39 WARN TaskSetManager: Stage 1 contains a task of very large size (419 KB). The maximum recommended task size is 100 KB.
        +--------------------+------+---------+------------+---------+------------+----------+
        |                body|offset|seqNumber|enqueuedTime|publisher|partitionKey|properties|
        +--------------------+------+---------+------------+---------+------------+----------+
        |[7B 22 74 65 6D 7...|     0|        0|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   112|        1|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   224|        2|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   336|        3|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   448|        4|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   560|        5|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   672|        6|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   784|        7|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   896|        8|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1008|        9|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1120|       10|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1232|       11|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1344|       12|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1456|       13|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1568|       14|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1680|       15|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1792|       16|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1904|       17|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  2016|       18|  1497734889|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  2128|       19|  1497734889|     null|        null|     Map()|
        +--------------------+------+---------+------------+---------+------------+----------+
        only showing top 20 rows

15. Als u nieuwe gebeurtenissen van de gebeurtenis producent binnenkomen, moeten ze worden verwerkt door deze query gestructureerde Streaming.
16. Zorg ervoor dat uw HDInsight-cluster verwijderen als u hebt dit voorbeeld wordt uitgevoerd.



## <a name="see-also"></a>Zie ook

* [Overzicht van Spark-streaming](apache-spark-streaming-overview.md)













