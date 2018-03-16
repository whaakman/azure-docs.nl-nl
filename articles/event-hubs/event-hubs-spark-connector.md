---
title: Apache Spark integreren met Azure Event Hubs | Microsoft Docs
description: Integreren met Apache Spark om gestructureerde Streaming met Event Hubs te schakelen
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.service: event-hubs
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: shvija;sethm
ms.openlocfilehash: 3b44c7e7387eceb2d9ea0b2c214b13a82869110f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="integrating-apache-spark-with-azure-event-hubs"></a>Apache Spark integreren met Azure Event Hubs

Azure Event Hubs integreert naadloos met [Apache Spark](https://spark.apache.org/) gedistribueerd streamingtoepassingen gemakkelijk maken gebouw end-to-end. Deze integratie ondersteunt [Spark Core](http://spark.apache.org/docs/latest/rdd-programming-guide.html), [Spark-Streaming](http://spark.apache.org/docs/latest/streaming-programming-guide.html), [gestructureerde Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html). De Event Hubs-connector voor Apache Spark is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs-spark). Deze bibliotheek is ook beschikbaar voor gebruik in Maven-projecten uit de [Maven centrale opslagplaats](http://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-eventhubs-spark_2.11%7C2.1.6%7C).

Dit artikel laat zien hoe u toepassingen in een continue [Azure Databrick](https://azure.microsoft.com/services/databricks/). Hoewel dit artikel wordt [Azure Databricks](https://azure.microsoft.com/services/databricks/), Spark-Clusters zijn ook leverbaar met [HDInsight](../hdinsight/spark/apache-spark-overview.md).

Het volgende voorbeeld worden twee Scala notitieblokken: één voor het streaming-gebeurtenissen van een event hub en een voor het verzenden van gebeurtenissen naar het.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u niet hebt, [een gratis account maken](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* Een exemplaar van Event Hubs. Als u niet hebt, [maken](event-hubs-create.md)
* Een [Azure Databricks](https://azure.microsoft.com/services/databricks/) exemplaar. Als u niet hebt, [maken](../azure-databricks/quickstart-create-databricks-workspace-portal.md)
* [Maken van een tapewisselaar met maven coördinaat](https://docs.databricks.com/user-guide/libraries.html#upload-a-maven-package-or-spark-package): `com.microsoft.azure:azure‐eventhubs‐spark_2.11:2.3.0`

Gebruik de volgende code in uw laptop stroom gebeurtenissen van de event hub.

```scala
import org.apache.spark.eventhubs._

// To connect to an event hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build 
val ehConf = EventHubsConf(connectionString)
  .setStartingPosition(EventPosition.fromEndOfStream)

// Create a stream that reads data from the specified Event Hub.
val reader = spark.readStream
  .format("eventhubs")
  .options(ehConf.toMap)
  .load()

// Select the body column and cast it to a string.
val eventhubs = reader.select($"body" cast "string")

eventhubs.writeStream
  .format("console")
  .outputMode("append")
  .start()
  .awaitTermination()
```
De volgende voorbeeldcode verzendt gebeurtenissen naar uw event hub, met de Spark-batch-API's. U kunt ook een streaming-query voor het verzenden van gebeurtenissen naar de event hub schrijven.

```scala
import org.apache.spark.eventhubs._
import org.apache.spark.sql.functions._

// To connect to an event hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build

val eventHubsConf = EventHubsConf(connectionString)

// Create a column representing the partitionKey.
val partitionKeyColumn = (col("id") % 5).cast("string").as("partitionKey")
// Create random strings as the body of the message.
val bodyColumn = concat(lit("random nunmber: "), rand()).as("body")

// Write 200 rows to the specified event hub.
val df = spark.range(200).select(partitionKeyColumn, bodyColumn)
df.write
  .format("eventhubs")
  .options(eventHubsConf.toMap)
  .save() 

```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe de Event Hubs-connector werkt voor realtime-fout fouttolerante streaming oplossingen bouwen. Meer informatie over het streamen van gestructureerde en Event Hubs geïntegreerde connector door deze koppelingen volgen:

* [Gestructureerde Streaming + Integratiehandleiding voor Azure Event Hubs](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/structured-streaming-eventhubs-integration.md)
* [Spark-Streaming + Integratiehandleiding voor Event Hubs](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/spark-streaming-eventhubs-integration.md)
