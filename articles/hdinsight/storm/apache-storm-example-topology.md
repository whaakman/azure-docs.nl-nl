---
title: "Voorbeeld van Apache Storm-topologieën op HDInsight | Microsoft Docs"
description: "Een lijst met Storm-voorbeeldtopologieën gemaakt en getest met Apache Storm op HDInsight, met inbegrip van eenvoudige C# en Java-topologieën en werken met Event Hubs."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f9b1bdff-5928-4705-a76d-52fd200917cb
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/01/2017
ms.author: larryfr
ms.openlocfilehash: c6c1a1483191e654c245eb3f05ee9e8406510b08
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="example-storm-topologies-and-components-for-apache-storm-on-hdinsight"></a>Voorbeeld van de Storm-topologieën en onderdelen voor Apache Storm op HDInsight

Hier volgt een lijst met voorbeelden gemaakt en beheerd door Microsoft voor gebruik met Apache Storm op HDInsight. Deze voorbeelden hebben betrekking op diverse onderwerpen uit het maken van eenvoudige C# en Java-topologieën te werken met Azure-services zoals Event Hubs, Cosmos DB, SQL-Database, HBase op HDInsight en Azure Storage. Sommige voorbeelden ook het werken met niet-Azure of zelfs niet-Microsoft-technologieën, zoals SignalR en Socket.IO.

| Beschrijving | Demonstreert | Taal/Framework |
|:--- |:--- |:--- |
| [Schrijven naar Azure Data Lake Store van Apache Storm](apache-storm-write-data-lake-store.md) |Schrijven naar het Azure Data Lake Store |Java |
| [Event Hub Spout en Bolt bron](https://github.com/apache/storm/tree/master/external/storm-eventhubs) |Bron voor de Event Hub Spout en Bolt |Java |
| [Java gebaseerde topologieën ontwikkelen voor Apache Storm op HDInsight][5797064f] |Maven |Java |
| [C#-topologieën ontwikkelen voor Apache Storm op HDInsight met behulp van Visual Studio][16fce2d1] |HDInsight-hulpprogramma's voor Visual Studio |C#, Java |
| [Procesgebeurtenissen van Azure Event Hubs met Storm op HDInsight (C#)][844d1d81] |Event Hubs |C# en Java |
| [Process events from Azure Event Hubs with Storm on HDInsight (Java)](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/) (Gebeurtenissen uit Azure Event Hubs verwerken met Storm op HDInsight (Java)) |Event Hubs |Java |
| [Verwerken van sensorgegevens vehicle van Event Hubs met behulp van Storm op HDInsight][246ee964] |Event Hubs, Cosmos-DB, Azure Storage-Blob (WASB) |C#, Java |
| [Uitpakken, transformeren en Load (ETL) uit Azure Event Hubs HBase, Storm op HDInsight gebruiken][b4b68194] |Event Hubs, HBase |C# |
| [C# Storm-topologie sjabloonproject voor het werken met Azure-services van Storm op HDInsight][ce0c02a2] |Event Hubs, Cosmos-DB, SQL Database, HBase, SignalR |C#, Java |
| [Schaalbaarheid benchmarks voor het lezen van Azure Event Hubs met behulp van Storm op HDInsight][d6c540e3] |Bericht doorvoer, Event Hubs, SQL-Database |C#, Java |
| [Python met Storm op HDInsight gebruiken](apache-storm-develop-python-topology.md) |Python-onderdelen met een lichtstroom-topologie |Python |
| [Gebruik Kafka met Storm op HDInsight](../hdinsight-apache-storm-with-kafka.md) | Apache Storm lezen en schrijven van Apache Kafka | Java |

### <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Apache Storm in HDInsight][2b8c3488]
* [Meer informatie over het implementeren en beheren van Storm-topologieën met Storm op HDInsight][6eb0d3b8]

[2b8c3488]:apache-storm-tutorial-get-started-linux.md "Informatie over het maken van een Storm op HDInsight-cluster en het Storm-Dashboard gebruiken voor het implementeren van voorbeeldtopologieën."
[6eb0d3b8]:apache-storm-deploy-monitor-topology.md "Informatie over het implementeren en beheren van topologieën met het web gebaseerde Storm-Dashboard en Storm-gebruikersinterface of de HDInsight Tools voor Visual Studio."
[16fce2d1]:apache-storm-develop-csharp-visual-studio-topology.md "Informatie over het maken van C# Storm-topologieën met behulp van de HDInsight Tools voor Visual Studio."
[5797064f]:apache-storm-develop-java-topology.md "Informatie over het maken van Storm-topologieën in Java, met behulp van Maven, door het maken van een eenvoudige wordcount-topologie."
[844d1d81]:apache-storm-develop-csharp-event-hub-topology.md "Informatie over het lezen en schrijven van gegevens uit Azure Event Hubs met Storm op HDInsight."
[246ee964]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md "Informatie over het gebruik van een Storm-topologie om berichten te lezen uit Azure Event Hubs gelezen documenten Azure Cosmos DB voor gegevens die verwijzen naar en opslaan van gegevens naar Azure Storage."
[d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Verschillende topologieën voor het demonstreren van doorvoer wanneer lezen uit Azure Event Hubs en opslaan met SQL Database met Apache Storm op HDInsight."
[b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Informatie over het lezen van gegevens uit Azure Event Hubs, cumulatieve & de gegevens transformeren en sla deze op in HBase in HDInsight."
[ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Dit project bevat sjablonen voor spouts, bolts en topologieën kunnen communiceren met verschillende Azure-services zoals Event Hubs, Cosmos-database en SQL-Database."

