---
title: Apache Flink gebruiken met Azure Event Hubs voor het ecosysteem Kafka | Microsoft Docs
description: Maken van verbinding Apache Flink naar een Kafka event hub ingeschakeld
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: mvc
ms.date: 06/06/2018
ms.author: bahariri
ms.openlocfilehash: cb7ef0e9b6a612e3f4116cb626903770e4035368
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303719"
---
# <a name="apache-flink-with-event-hubs-for-the-kafka-ecosystem"></a>Apache Flink met Event Hubs voor het ecosysteem Kafka

Een van de belangrijkste voordelen van het gebruik van Apache Kafka is het ecosysteem van deze verbinding met maken kan frameworks. Kafka ingeschakeld Event Hubs combineert de flexibiliteit van Kafka met de schaalbaarheid, consistentie en ondersteuning van het Azure-ecosysteem.

Deze zelfstudie laat zien hoe u verbinding maken met Apache Flink Kafka ingeschakeld event hubs zonder uw clients protocol wijzigen of uw eigen clusters die zijn uitgevoerd. Azure Event Hubs voor Kafka ecosysteem ondersteunt [Apache Kafka versie 1.0.](https://kafka.apache.org/10/documentation.html)

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u hebt de volgende vereisten voor het voltooien van deze zelfstudie:

* Een Azure-abonnement. Als u nog geen account hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * Voer op Ubuntu `apt-get install default-jdk` uit om de JDK te installeren.
    * Zorg dat de omgevingsvariabele JAVA_HOME verwijst naar de map waarin de JDK is geïnstalleerd.
* [Download](http://maven.apache.org/download.cgi) en [installeren](http://maven.apache.org/install.html) een binaire Maven-archief
    * Op Ubuntu kunt u `apt-get install maven` uitvoeren om Maven te installeren.
* [Git](https://www.git-scm.com/downloads)
    * Op Ubuntu kunt u `sudo apt-get install git` uitvoeren om Git te installeren.

## <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken

Een Event Hubs-naamruimte is vereist voor het verzenden of ontvangen van alle Event Hubs-service. Zie [maken Kafka ingeschakeld Event Hubs](event-hubs-create-kafka-enabled.md) voor informatie over het ophalen van een Event Hubs Kafka-eindpunt. Zorg ervoor dat voor het kopiëren van de Event Hubs-verbindingsreeks voor later gebruik.

## <a name="clone-the-example-project"></a>Klonen van de voorbeeldproject

Nu dat u een verbindingsreeks Kafka ingeschakeld Event Hubs hebt, kloon de opslagplaats Azure Event Hubs en navigeer naar de `flink` submap:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/flink
```

## <a name="flink-producer"></a>Flink producent

Het opgegeven Flink producent voorbeeld berichten verzenden naar Event Hubs-service.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Geef een Event Hubs Kafka-eindpunt

#### <a name="producerconfig"></a>Producer.config

Update de `bootstrap.servers` en `sasl.jaas.config` in waarden `producer/src/main/resources/producer.config` de producent met het Event Hubs Kafka-eindpunt met de juiste verificatie wordt omgeleid.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=FlinkExampleProducer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-producer-from-the-command-line"></a>Producent uitvoeren vanaf de opdrachtregel

Voor de producent uitvoeren vanaf de opdrachtregel, genereert de JAR en vervolgens uitgevoerd vanuit Maven (of de JAR met behulp van Maven, uit te voeren in Java door toe te voegen nodig Kafka JAR(s) aan het klassepad):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestProducer"
```

De producent gaat nu verder met het verzenden van gebeurtenissen naar de Kafka Event Hub ingeschakeld op onderwerp `test` en afdrukken van de gebeurtenissen naar stdout.

## <a name="flink-consumer"></a>Flink consumer

Ontvangen van berichten in het voorbeeld opgegeven consument de Kafka ingeschakeld Event Hubs.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Geef een Event Hubs Kafka-eindpunt

#### <a name="consumerconfig"></a>consumer.config

Update de `bootstrap.servers` en `sasl.jaas.config` in waarden `consumer/src/main/resources/consumer.config` om te leiden van de consument de Event Hubs Kafka-eindpunt met de juiste verificatie.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
group.id=FlinkExampleConsumer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-consumer-from-the-command-line"></a>Consumer uitvoeren vanaf de opdrachtregel

Als u wilt de consument uitvoeren vanaf de opdrachtregel, genereert de JAR en vervolgens uitgevoerd vanuit Maven (of de JAR met behulp van Maven, uit te voeren in Java door toe te voegen nodig Kafka JAR(s) aan het klassepad):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestConsumer"
```

Als de Kafka ingeschakeld event hub heeft gebeurtenissen (bijvoorbeeld als de producent ook wordt uitgevoerd), klikt u vervolgens de consument nu begint gebeurtenissen ontvangen van het onderwerp `test`.

Bekijk [van Flink Kafka Connector handleiding](https://ci.apache.org/projects/flink/flink-docs-stable/dev/connectors/kafka.html) voor meer informatie gedetailleerde over het aansluiten van Flink op Kafka.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Event Hubs](event-hubs-what-is-event-hubs.md)
* [Meer informatie over Event Hubs voor het Kafka-ecosysteem](event-hubs-for-kafka-ecosystem-overview.md)
* Gebruik [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) naar [stroom gebeurtenissen van Kafka on-premises Kafka ingeschakeld Event Hubs in de cloud.](event-hubs-kafka-mirror-maker-tutorial.md)
* Meer informatie over het stream in Kafka met behulp van ingeschakelde Event Hubs [systeemeigen Kafka toepassingen](event-hubs-quickstart-kafka-enabled-event-hubs.md) of [Akka Streams](event-hubs-kafka-akka-streams-tutorial.md).
