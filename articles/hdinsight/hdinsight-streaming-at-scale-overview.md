---
title: Schaalbaar streamen in Azure HDInsight
description: Het gebruik van gegevens streamen met schaalbare HDInsight-clusters.
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/19/2018
ms.openlocfilehash: 35304a51ff7fda8bbf7ef6ebb0366ebe740abfa6
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39591252"
---
# <a name="streaming-at-scale-in-hdinsight"></a>Schaalbaar streamen in HDInsight

Big data-oplossingen voor realtime reageren op gegevens in beweging. Deze gegevens is doorgaans het nuttigst op de tijd van aankomst. Als de gegevensstroom inkomende groter is wordt dan op dat moment kan worden verwerkt, moet u mogelijk resources beperken. Een HDInsight-cluster kunt u kunt ook omhoog schalen om te voldoen aan uw streaming-oplossing door toe te voegen knooppunten op aanvraag.

In een streaming-toepassing, zijn een of meer gegevensbronnen genereren van gebeurtenissen (soms in miljoenen per seconde) die snel worden opgenomen moeten dat alle nuttige informatie. De binnenkomende gebeurtenissen worden verwerkt met *stroombuffering*, ook wel genoemd *gebeurteniswachtrij*, door een service zoals [Kafka](kafka/apache-kafka-introduction.md) of [Event Hubs](https://azure.microsoft.com/services/event-hubs/). Nadat u de gebeurtenissen verzameld, kunt u vervolgens de gegevens met behulp van een realtime analytics-systeem binnen analyseren de *streamverwerking* laag, zoals [Storm](storm/apache-storm-overview.md) of [Spark Streaming](spark/apache-spark-streaming-overview.md). De verwerkte gegevens kunnen worden opgeslagen in langdurige opslagsystemen, zoals [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/), en wordt weergegeven in realtime op een dashboard voor bedrijfsinformatie, zoals [Power BI](https://powerbi.microsoft.com), Tableau of een aangepaste web-pagina .

![HDInsight Streaming-patronen](./media/hdinsight-streaming-at-scale-overview/HDInsight-streaming-patterns.png)

## <a name="apache-kafka"></a>Apache Kafka

Apache Kafka biedt een hoge doorvoer, lage latentie message Queuing-service en is nu onderdeel van de Apache-suite van Open Source Software (OSS). Kafka maakt gebruik van een publiceren en abonneren messaging-model en winkels streams van gepartitioneerde gegevens veilig in een gedistribueerde, gerepliceerde-cluster. Kafka schaalt lineair als doorvoer toeneemt.

Zie voor meer informatie, [Inleiding tot Apache Kafka in HDInsight](kafka/apache-kafka-introduction.md).

## <a name="apache-storm"></a>Apache Storm

Apache Storm is een gedistribueerde, fouttolerante, open source berekeningssysteem die is geoptimaliseerd voor het verwerken van gegevensstromen in realtime met Hadoop. De core-eenheid van de gegevens voor een inkomende gebeurtenis is een Tuple een onveranderbare set van sleutel/waarde-paren is. Een niet-gebonden volgorde van de volgende Tuples vormen een Stream, die afkomstig is van een Spout. De Spout loopt van een streaming-gegevensbron (zoals Kafka), en verzendt Tuples. Een storm-topologie is een opeenvolging van transformaties op deze stromen.

Zie voor meer informatie, [wat is Apache Storm op Azure HDInsight?](storm/apache-storm-overview.md).

## <a name="spark-streaming"></a>Spark-Streaming

Spark Streaming is een uitbreiding van Spark, zodat u kunt de dezelfde code die u voor batchverwerking gebruikt hergebruikt. U kunt batch- en interactieve query's in dezelfde toepassing combineren. In tegenstelling tot Storm, Spark Streaming biedt stateful precies-semantiek verwerking. Wanneer gebruikt in combinatie met de [directe API van Kafka](http://spark.apache.org/docs/latest/streaming-kafka-integration.html), die zorgt ervoor dat alle Kafka-gegevens exact één keer wordt ontvangen door Spark Streaming, is het mogelijk om end-to-end precies-eenmaal wordt gegarandeerd. Een van de sterke punten van Spark Streaming is fouttolerant de mogelijkheden ervan herstellen knooppunten snel mislukt wanneer meerdere knooppunten binnen het cluster worden gebruikt.

Zie voor meer informatie, [wat is Spark Streaming?](hdinsight-spark-streaming-overview.md).

## <a name="scaling-a-cluster"></a>Een cluster schalen

U kunt het aantal knooppunten in uw cluster opgeven tijdens het maken, kunt u de vergroten of verkleinen van het cluster zodat deze overeenkomt met de werkbelasting. Alle HDInsight-clusters kunnen u [wijzigen van het aantal knooppunten in het cluster](hdinsight-administer-use-management-portal.md#scale-clusters). Spark-clusters kunnen worden verwijderd zonder verlies van gegevens, zoals alle gegevens worden opgeslagen in Azure Storage of Data Lake Store.

Er zijn voordelen ontkoppeling technologieën. Bijvoorbeeld: Kafka is een buffer-technologie, zodat het is zeer intensieve I/O en niet veel hoeft verwerkingskracht. Ter vergelijking: zijn streamprocessors zoals Spark Streaming rekenintensieve, vereisen krachtigere virtuele machines. Door deze technologieën losgekoppeld in verschillende clusters, kunt u schalen ze onafhankelijk van elkaar tijdens het beste gebruik van de virtuele machines.

### <a name="scale-the-stream-buffering-layer"></a>De buffer van de laag stroom schalen

De stroom buffer technologieën Event Hubs en Kafka gebruiken beide partities en consumenten lezen uit deze partities. Het schalen van de invoer doorvoer vereist omhoog schalen van het aantal partities en vindt u steeds meer parallelle uitvoering partities toe te voegen. In Event Hubs, wordt het aantal partities kan niet gewijzigd na de implementatie dus is het belangrijk om te beginnen met de doel-schaal in gedachten. Met Kafka, is het mogelijk om te [partities toevoegen](https://kafka.apache.org/documentation.html#basic_ops_cluster_expansion), zelfs terwijl Kafka van gegevens verwerken. Kafka biedt een hulpprogramma voor het toewijzen van partities, `kafka-reassign-partitions.sh`. HDInsight biedt een [partitie replica herverdeling hulpprogramma](https://github.com/hdinsight/hdinsight-kafka-tools), `rebalance_rackaware.py`. Dit verdelen hulpprogramma roept de `kafka-reassign-partitions.sh` hulpprogramma zodanig dat elke replica in een afzonderlijke foutdomein en het updatedomein is, waardoor Kafka bewust is en de toenemende fouttolerantie voor rekken.

### <a name="scale-the-stream-processing-layer"></a>De laag stroom verwerking schalen

Apache Storm en Spark Streaming ondersteuning voor worker-knooppunten toe te voegen met hun-clusters, zelfs wanneer gegevens worden verwerkt.

Als u wilt profiteren van nieuwe knooppunten die zijn toegevoegd door te schalen, Storm, moet u opnieuw verdelen van een Storm-topologieën gestart voordat de clustergrootte werd uitgebreid. Deze herverdelen kan worden gedaan met behulp van de Storm web UI of de CLI. Zie voor meer informatie de [Apache Storm documentatie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

Apache Spark maakt gebruik van drie belangrijke parameters voor het configureren van de omgeving, afhankelijk van de toepassingsvereisten: `spark.executor.instances`, `spark.executor.cores`, en `spark.executor.memory`. Een *executor* is een proces dat voor een Spark-toepassing wordt gestart. Een executor wordt uitgevoerd op de worker-knooppunt en is verantwoordelijk voor het uitvoeren van taken van de toepassing. Het standaardaantal Executor en de grootte van de executor voor elk cluster worden berekend op basis van het aantal worker-knooppunten en de grootte van het worker-knooppunt. Deze getallen zijn opgeslagen in de `spark-defaults.conf`-bestand op het hoofdknooppunt van elke cluster.

Deze drie parameters kunnen worden geconfigureerd op het niveau van het cluster voor alle toepassingen die op het cluster worden uitgevoerd, en kan ook worden opgegeven voor elke afzonderlijke toepassing. Zie voor meer informatie, [-resources beheren voor Apache Spark-clusters](spark/apache-spark-resource-manager.md).

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Apache Storm op HDInsight](storm/apache-storm-tutorial-get-started-linux.md)
* [Example Storm toplogies and components for Apache Storm on HDInsight](storm/apache-storm-example-topology.md) (Voorbeelden van Storm-topologieën en -onderdelen in HDInsight)
* [Inleiding tot Spark in HDInsight](spark/apache-spark-overview.md)
* [Beginnen met Apache Kafka in HDInsight](kafka/apache-kafka-get-started.md)
