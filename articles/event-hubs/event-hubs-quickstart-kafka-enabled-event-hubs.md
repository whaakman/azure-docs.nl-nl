---
title: Streamen naar Azure Event Hubs voor het Kafka-ecosysteem | Microsoft Docs
description: Streamen naar Event Hubs met behulp van API's en het Kafka-protocol.
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2018
ms.author: bahariri
ms.openlocfilehash: cabbb7ed6157a6c68530ab6b5f405aa67b31a1b2
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="stream-into-event-hubs-for-kafka-ecosystem"></a>Streamen naar Azure Event Hubs voor het Kafka-ecosysteem

> [!NOTE]
> Dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs)

Deze snelstart laat zien hoe u kunt streamen naar Event Hubs waarvoor Kafka is ingeschakeld zonder uw protocolclients te wijzigen of uw eigen clusters uit te voeren. U leert hoe u uw producenten kunt gebruiken en consumenten kunnen met Event Hubs waarvoor Kafka is ingeschakeld praten met slechts een configuratiewijziging in uw toepassingen. Azure Event Hubs voor het Kafka-ecosysteem biedt ondersteuning voor [Apache Kafka versie 1.0.](https://kafka.apache.org/10/documentation.html)

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

* Een Azure-abonnement. Als u nog geen account hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Download](http://maven.apache.org/download.cgi) en [installeer](http://maven.apache.org/install.html) een binair Maven-archief.
* [Git](https://www.git-scm.com/)
* [Een Event Hubs-naamruimte waarvoor Kafka is ingeschakeld](event-hubs-create.md)

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Berichten verzenden en ontvangen met Kafka in Event Hubs

1. Kloon de [Azure Event Hubs-opslagplaats](https://github.com/Azure/azure-event-hubs).

2. Navigeer naar `azure-event-hubs/samples/kafka/quickstart/producer`.

3. Werk de configuratiegegevens voor de producent bij in src/main/resources/producer.config zoals hier wordt weergegeven.

    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```
4. Voer de producentcode uit en stream deze naar Event Hubs waarvoor Kafka is ingeschakeld.
   
    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
5. Navigeer naar azure-event-hubs/samples/kafka/Quick Start/consumer.

6. Werk de configuratiegegevens voor de consument bij in src/main/resources/consumer.config zoals hier wordt weergegeven.
   
    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```

7. Voer de consumentcode en het proces uit vanuit Event Hubs waarvoor Kafka is ingeschakeld met behulp van uw Kafka-clients.

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Als uw Event Hubs Kafka-cluster gebeurtenissen in de wachtrij van de producent heeft, moet u deze nu ontvangen van de consument.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Event Hubs voor het Kafka-ecosysteem](event-hubs-for-kafka-ecosystem-overview.md)
* [Meer informatie over Event Hubs](event-hubs-what-is-event-hubs.md)
* Gebruik [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) om gebeurtenissen van Kafka on-premises te streamen naar Event Hubs waarvoor Kafka is ingeschakeld in de cloud.] (event-hubs-kafka-mirror-maker-tutorial.md)
