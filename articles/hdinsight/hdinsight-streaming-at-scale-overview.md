---
title: Streaming op grote schaal in Azure HDInsight | Microsoft Docs
description: Het gebruik van gegevens streamen met schaalbare HDInsight-clusters.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: raghavmohan
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: ramoha
ms.openlocfilehash: 46b5723805ab5d8bc1cf5b5183d9501cd3e4e3a2
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="streaming-at-scale-in-hdinsight"></a>Streaming op grote schaal in HDInsight

Realtime big data-oplossingen werken op gegevens in beweging. Deze gegevens is doorgaans het nuttigst op het tijdstip van aankomst. Als de gegevensstroom inkomende groter is wordt dan op dat moment kan worden verwerkt, moet u wellicht beperken resources. U kunt ook schaalbaar een HDInsight-cluster en om te voldoen aan uw oplossing voor streaming door knooppunten toe te voegen op aanvraag.

Een of meer gegevensbronnen zijn gebeurtenissen (soms ook in de miljoenen per seconde) die snel worden ingenomen zonder om nuttige informatie wilt genereren in een streaming-toepassing. De binnenkomende gebeurtenissen worden verwerkt met *stream buffer*, ook wel *gebeurtenis queuing*, door een service zoals [Kafka](kafka/apache-kafka-introduction.md) of [Event Hubs](https://azure.microsoft.com/services/event-hubs/). Nadat u de gebeurtenissen verzameld, kunt u vervolgens de gegevens met behulp van een systeem realtime analyses binnen analyseren de *stroom verwerking* layer, zoals [Storm](storm/apache-storm-overview.md) of [Spark-Streaming](spark/apache-spark-streaming-overview.md). Verwerkte gegevens kunnen worden opgeslagen in langdurige opslagsystemen, zoals [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/), en weergegeven in realtime op een dashboard voor bedrijfsinformatie, zoals [Power BI](https://powerbi.microsoft.com), Tableau of een aangepaste webpagina .

![HDInsight Streaming patronen](./media/hdinsight-streaming-at-scale-overview/HDInsight-streaming-patterns.png)

## <a name="apache-kafka"></a>Apache Kafka

Apache Kafka biedt een hoge doorvoer, lage latentie message Queuing-service en maakt nu deel uit van de Apache-suite van Open Source Software (besturingssystemen). Kafka gebruikt publiceren en abonneren messaging model en winkels gegevensstromen gepartitioneerde veilig in een gedistribueerde, gerepliceerde-cluster. Kafka wordt lineair geschaald als verhoogt de doorvoer.

Zie voor meer informatie [Inleiding tot Apache Kafka op HDInsight](kafka/apache-kafka-introduction.md).

## <a name="apache-storm"></a>Apache Storm

Apache Storm is een gedistribueerd, fouttolerant, open source berekeningssysteem die is geoptimaliseerd voor het verwerken van gegevensstromen in realtime met Hadoop. De eenheid core van gegevens voor een inkomende gebeurtenis is een Tuple een niet-wijzigbaar reeks sleutel/waarde-paren is. Een unbounded volgorde van deze Tuples vormen een stroom die afkomstig is van een Spout. De Spout loopt van een streaming-gegevensbron (zoals Kafka) en verzendt Tuples. Een storm-topologie is een opeenvolging van transformaties voor deze stromen.

Zie voor meer informatie [wat is Apache Storm op Azure HDInsight?](storm/apache-storm-overview.md).

## <a name="spark-streaming"></a>Spark-Streaming

Spark-Streaming is een uitbreiding van Spark, zodat u kunt het hergebruik van de dezelfde code die u voor batchverwerking gebruikt. U kunt combineren batch- en interactieve query's in dezelfde toepassing. In tegenstelling tot Storm, Spark-Streaming biedt stateful exact-zodra de semantiek voor verwerking. Wanneer gebruikt in combinatie met de [Kafka directe API](http://spark.apache.org/docs/latest/streaming-kafka-integration.html), waardoor wordt gegarandeerd dat alle Kafka gegevens worden ontvangen door het Spark-Streaming exact één keer, is het mogelijk end-to-end exact bereiken-eenmaal wordt gegarandeerd. Een van de sterkte van Spark Streaming fouttolerante mogelijkheden ervan herstellen is snel knooppunten mislukt wanneer meerdere knooppunten binnen het cluster worden gebruikt.

Zie voor meer informatie [wat is er Spark-Streaming?](hdinsight-spark-streaming-overview.md).

## <a name="scaling-a-cluster"></a>Schalen van een cluster

Maar u het aantal knooppunten in het cluster tijdens het maken opgeven kunt, kunnen u vergroten of verkleinen van het cluster zodat deze overeenkomen met de werkbelasting. Alle HDInsight-clusters kunnen u [wijzigen van het aantal knooppunten in het cluster](hdinsight-administer-use-management-portal.md#scale-clusters). Spark-clusters kunnen worden verwijderd zonder verlies van gegevens, zoals alle gegevens worden opgeslagen in Azure Storage of de Data Lake Store.

Er zijn voordelen ontkoppeling technologieën. Kafka is bijvoorbeeld een gebeurtenis buffer-technologie, zodat het is zeer intensieve IO en niet veel hoeft verwerkingskracht. Ter vergelijking zijn stream-processors zoals Spark-Streaming rekenintensieve krachtiger VM's vereisen. Doordat deze technologieën losgekoppeld in verschillende clusters, u kunt ze afzonderlijk van elkaar schalen tijdens het beste gebruik van de virtuele machines.

### <a name="scale-the-stream-buffering-layer"></a>De stroom buffer laag schalen

De stroom buffer technologieën Event Hubs en Kafka gebruiken beide partities en consumenten van deze partities lezen. Schalen van de invoer doorvoer vereist het aantal partities schaalt en toenemende parallelle uitvoering toe te voegen partities bevat. In Event Hubs, kan niet het aantal partities worden gewijzigd na implementatie dus is het belangrijk om te beginnen met de schaal doel in gedachten. Met Kafka, is het mogelijk te [partities toevoegen](https://kafka.apache.org/documentation.html#basic_ops_cluster_expansion), zelfs terwijl Kafka van gegevens verwerken. Kafka voorziet in een hulpprogramma voor opnieuw toewijzen van partities `kafka-reassign-partitions.sh`. HDInsight biedt een [partitie replica herverdeling hulpprogramma](https://github.com/hdinsight/hdinsight-kafka-tools), `rebalance_rackaware.py`. Dit hulpprogramma rebalancing roept de `kafka-reassign-partitions.sh` hulpprogramma zodanig dat elke replica in een afzonderlijke foutdomein en het updatedomein is, waardoor Kafka rack bewust is en de toenemende fouttolerantie.

### <a name="scale-the-stream-processing-layer"></a>De stroom verwerken laag schalen

Apache Storm- en Spark-Streaming ondersteuning voor worker-knooppunten toe te voegen met hun clusters, zelfs bij het verwerken van gegevens.

Om te profiteren van nieuwe knooppunten die zijn toegevoegd door te schalen Storm, moet u eventuele gestart voordat de clustergrootte werd uitgebreid Storm-topologieën opnieuw verdelen. Deze herverdeling kan worden gedaan met behulp van de Storm web UI of de CLI. Zie voor meer informatie de [Apache Storm documentatie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

Apache Spark drie belangrijke parameters gebruikt voor het configureren van de omgeving, afhankelijk van de vereisten voor application: `spark.executor.instances`, `spark.executor.cores`, en `spark.executor.memory`. Een *executor* is een proces dat voor een Spark-toepassing wordt gestart. Een executor wordt uitgevoerd op het werkrolknooppunt en is verantwoordelijk voor de toepassing taken uit te voeren. Het standaardaantal Executor en de grootte van de executor voor elk cluster worden berekend op basis van het aantal worker-knooppunten en de grootte van het worker-knooppunt. Deze getallen worden opgeslagen in de `spark-defaults.conf`-bestand op elk clusterknooppunt head.

Deze drie parameters kunnen worden geconfigureerd op het clusterniveau van de voor alle toepassingen die worden uitgevoerd op het cluster en kan ook worden opgegeven voor elke afzonderlijke toepassing. Zie voor meer informatie [bronnen beheren voor Apache Spark-clusters](spark/apache-spark-resource-manager.md).

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Apache Storm op HDInsight](storm/apache-storm-tutorial-get-started-linux.md)
* [Example Storm toplogies and components for Apache Storm on HDInsight](storm/apache-storm-example-topology.md) (Voorbeelden van Storm-topologieën en -onderdelen in HDInsight)
* [Inleiding tot Spark in HDInsight](spark/apache-spark-overview.md)
* [Beginnen met Apache Kafka in HDInsight](kafka/apache-kafka-get-started.md)