---
title: Log Analytics voor Apache Kafka - Azure HDInsight
description: Informatie over het gebruik van Log Analytics voor het analyseren van Logboeken van Apache Kafka-cluster in Azure HDInsight.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/15/2018
ms.openlocfilehash: 69eaa0028f1115cafbd1ed28b66940d7faaed062
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53608542"
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>Logboeken voor Apache Kafka in HDInsight analyseren

Informatie over het gebruik van Log Analytics voor het analyseren van logboeken die worden gegenereerd door Apache Kafka in HDInsight.

## <a name="enable-log-analytics-for-apache-kafka"></a>Log Analytics voor Apache Kafka inschakelen

De stappen voor het inschakelen van Log Analytics voor HDInsight zijn hetzelfde voor alle HDInsight-clusters. Gebruik de volgende koppelingen voor meer informatie over het maken en configureren van de vereiste services:

1. Een Log Analytics-werkruimte maken. Zie voor meer informatie de [aan de slag met Log Analytics-werkruimte](https://docs.microsoft.com/azure/log-analytics) document.

2. Maken van een Kafka op HDInsight-cluster. Zie voor meer informatie de [beginnen met Apache Kafka in HDInsight](apache-kafka-get-started.md) document.

3. Configureer de Kafka-cluster voor het gebruik van Log Analytics. Zie voor meer informatie de [met Log Analytics voor het bewaken van HDInsight](../hdinsight-hadoop-oms-log-analytics-tutorial.md) document.

    > [!NOTE]  
    > U kunt ook configureren met het cluster voor het gebruik van Log Analytics met behulp van de `Enable-AzureRmHDInsightOperationsManagementSuite` cmdlet. Deze cmdlet vereist de volgende gegevens:
    >
    > * De naam van het HDInsight-cluster.
    > * De ID van de werkruimte voor Log Analytics. U kunt de werkruimte-ID vinden in uw Log Analytics-werkruimte.
    > * De primaire sleutel voor de verbinding met Log Analytics. Als u zoekt de primaire sleutel, open de werkruimte in Azure portal, selecteert u __geavanceerde instellingen__ in het menu links. Selecteer in de geavanceerde instellingen __verbonden bronnen__>__Linux-Servers__.


> [!IMPORTANT]  
> Het kan ongeveer 20 minuten duren voordat gegevens beschikbaar voor Log Analytics zijn.

## <a name="query-logs"></a>Logboeken voor query 's

1. Uit de [Azure-portal](https://portal.azure.com), selecteer uw Log Analytics-werkruimte.

2. Selecteer __zoeken in logboeken__. Hier vindt u de gegevens die worden verzameld van Kafka. Hier volgen enkele voorbeelden van zoekopdrachten:

    * Gebruik van de schijf: `Perf | where ObjectName == "Logical Disk" and CounterName == "Free Megabytes" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * CPU-gebruik: `Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * Binnenkomende berichten per seconde: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-MessagesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s, bin(TimeGenerated, 1h)`

    * Binnenkomende bytes per seconde: `metrics_kafka_CL | where HostName_s == "wn0-kafka" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    * Uitgaande bytes per seconde: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesOutPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    > [!IMPORTANT]  
    > Vervang de querywaarden met de specifieke gegevens van uw cluster. Bijvoorbeeld, `ClusterName_s` moet worden ingesteld op de naam van uw cluster. `HostName_s` moet worden ingesteld op de domeinnaam van een worker-knooppunt in het cluster.

    U kunt ook opgeven `*` om te zoeken naar alle typen in het logboek geregistreerd. De volgende logboeken zijn momenteel beschikbaar voor query's:

    | Logboektype | Description |
    | ---- | ---- |
    | log\_kafkaserver\_CL | Kafka-broker server.log |
    | log\_kafkacontroller\_CL | Kafka-broker controller.log |
    | metrische gegevens\_kafka\_CL | Kafka JMX-meetwaarden |

    ![Afbeelding van het CPU-gebruik zoeken](./media/apache-kafka-log-analytics-operations-management/kafka-cpu-usage.png)
 
 ## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Log Analytics, de [aan de slag met Log Analytics-werkruimte](../../log-analytics/log-analytics-get-started.md) document.

Zie de volgende documenten voor meer informatie over het werken met Apache Kafka:

 * [Mirror Apache Kafka tussen clusters met HDInsight](apache-kafka-mirroring.md)
 * [Verhoog de schaalbaarheid van Apache Kafka in HDInsight](apache-kafka-scalability.md)
 * [Apache Spark streaming (DStreams) met Apache Kafka gebruiken](../hdinsight-apache-spark-with-kafka.md)
 * [Apache Spark structured streaming met Apache Kafka gebruiken](../hdinsight-apache-kafka-spark-structured-streaming.md)
