---
title: Voorbeeld van Apache Storm-topologieën in Azure HDInsight
description: Een lijst van voorbeeld van de Storm-topologieën die zijn gemaakt en getest met Apache Storm op HDInsight, waaronder basic C# en Java-topologieën en het werken met Event Hubs.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: 42780eae0a85ec8a2ca7e9d6ca1149f6530c25e1
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448982"
---
# <a name="example-apache-storm-topologies-and-components-for-apache-storm-on-hdinsight"></a>Voorbeeld van Apache Storm-topologieën en -onderdelen voor Apache Storm op HDInsight

Hierna volgt een lijst van voorbeelden die zijn gemaakt en beheerd door Microsoft voor gebruik met [Apache Storm](https://storm.apache.org/) op HDInsight. Deze voorbeelden omvatten een verscheidenheid aan onderwerpen, van het maken van eenvoudige C# en Java-topologieën te werken met Azure-services zoals Event Hubs, Cosmos DB, SQL-Database, [Apache HBase](https://hbase.apache.org/) op HDInsight en Azure Storage. Enkele voorbeelden laten ook zien hoe u werkt met niet-Azure, of zelfs niet-Microsoft-technologieën, zoals SignalR en Socket.IO.

| Description | Ziet u | Language/Framework |
|:--- |:--- |:--- |
| [Schrijven naar Azure Data Lake-opslag van Apache Storm](apache-storm-write-data-lake-store.md) |Schrijven naar Azure Data Lake-opslag |Java |
| [Event Hub Spout en Bolt bron](https://github.com/apache/storm/tree/master/external/storm-eventhubs) |Bron voor de Event Hub Spout en Bolt |Java |
| [Op Java gebaseerde topologieën ontwikkelen voor Apache Storm op HDInsight][5797064f] |Maven |Java |
| [C#-topologieën ontwikkelen voor Apache Storm op HDInsight met behulp van Visual Studio][16fce2d1] |HDInsight-hulpprogramma's voor Visual Studio |C#, Java |
| [Verwerken van gebeurtenissen uit Azure Event Hubs met Apache Storm op HDInsight (C#)][844d1d81] |Event Hubs |C# en Java |
| [Process events from Azure Event Hubs with Storm on HDInsight (Java)](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/) (Gebeurtenissen uit Azure Event Hubs verwerken met Storm op HDInsight (Java)) |Event Hubs |Java |
| [Voertuigsensorgegevens van Event Hubs met Apache Storm op HDInsight verwerken][246ee964] |Event Hubs, Cosmos DB, Azure Storage Blob (WASB) |C#, Java |
| [Extraheren, transformeren en laden (ETL) van Azure Event Hubs voor Apache HBase, Apache Storm op HDInsight gebruiken][b4b68194] |Eventhubs, HBase |C# |
| [Sjabloon C# Storm-topologie-project voor het werken met Azure-services van Apache Storm op HDInsight][ce0c02a2] |Event Hubs, Cosmosdb, SQL Database, HBase, SignalR |C#, Java |
| [Schaalbaarheid benchmarks voor het lezen van Azure Event Hubs met Apache Storm op HDInsight][d6c540e3] |Doorvoer van berichten, Event Hubs, SQL-Database |C#, Java |
| [Python gebruiken met Apache Storm op HDInsight](apache-storm-develop-python-topology.md) |Python-onderdelen met een topologie lichtstroom |Python |
| [Apache Kafka gebruiken met Apache Storm op HDInsight](../hdinsight-apache-storm-with-kafka.md) | Apache Storm lezen en schrijven naar Apache Kafka | Java |

> [!WARNING]  
> De C#-voorbeelden in deze lijst zijn oorspronkelijk gemaakt en getest met HDInsight op basis van Windows en mei niet werken goed met HDInsight op basis van Linux-clusters. Op basis van Linux-clusters Mono gebruiken voor het uitvoeren van .NET-code, en mogelijk problemen met de compatibiliteit met de frameworks en -pakketten die in het voorbeeld.
>
> Linux is het enige besturingssysteem gebruikt op HDInsight versie 3.4 of hoger.

### <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Apache Storm in HDInsight][2b8c3488]
* [Meer informatie over het implementeren en beheren van Apache Storm-topologieën met Apache Storm op HDInsight][6eb0d3b8]

[2b8c3488]:apache-storm-tutorial-get-started-linux.md "Informatie over het maken van een Apache Storm op HDInsight-cluster en het Storm-Dashboard gebruiken voor voorbeeldtopologieën implementeren."
[6eb0d3b8]:apache-storm-deploy-monitor-topology-linux.md "Leer hoe u met behulp van het web gebaseerde Apache Storm-Dashboard en Storm-gebruikersinterface of de HDInsight-hulpprogramma's voor Visual Studio topologieën implementeren en beheren."
[16fce2d1]:apache-storm-develop-csharp-visual-studio-topology.md "Informatie over het maken van C# Storm-topologieën met behulp van de HDInsight-hulpprogramma's voor Visual Studio."
[5797064f]:apache-storm-develop-java-topology.md "Leer hoe u Storm-topologieën maken in Java, met behulp van Maven, met het maken van een eenvoudige wordcount-topologie."
[844d1d81]:apache-storm-develop-csharp-event-hub-topology.md "Informatie over het lezen en schrijven van gegevens uit Azure Event Hubs met Storm op HDInsight."
[246ee964]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md "Informatie over het gebruik van een Storm-topologie om berichten te lezen uit Azure Event Hubs, documenten lezen uit Azure Cosmos DB voor gegevens die verwijzen naar en gegevens opslaan in Azure Storage."
[d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Verschillende topologieën ter illustratie van de doorvoer bij het lezen van Azure Event Hubs en opslaan met SQL-Database met Apache Storm op HDInsight."
[b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Informatie over het lezen van gegevens uit Azure Event Hubs, statistische en de gegevens te transformeren en sla deze op in HBase op HDInsight."
[ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Dit project bevat sjablonen voor spouts en bolts de topologieën om te communiceren met verschillende Azure-services zoals Event Hubs, Cosmos DB en SQL-Database."

