---
title: Gebruik Apache Flink met Azure Eventhubs voor Apache Kafka | Microsoft Docs
description: Maken van verbinding Apache Flink naar een Kafka event hub ingeschakeld
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: mvc
ms.date: 08/06/2018
ms.author: bahariri
ms.openlocfilehash: 28ebcd0fe10492b730d73e19f9206545084b4030
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277338"
---
# <a name="use-apache-flink-with-azure-event-hubs-for-apache-kafka"></a>Gebruik Apache Flink met Azure Eventhubs voor Apache Kafka
Deze zelfstudie leert u hoe verbinding maken met Apache Flink gebeurtenishubs Kafka-functionaliteit zonder te wijzigen van uw clients protocol of uitvoeren van uw eigen clusters. Biedt ondersteuning voor Azure Event Hubs [Apache Kafka, versie 1.0.](https://kafka.apache.org/10/documentation.html).

Een van de belangrijkste voordelen van het gebruik van Apache Kafka is het ecosysteem van frameworks voor die deze verbinding met maken kan. Kafka ingeschakeld Event Hubs combineert de flexibiliteit van Kafka met de schaalbaarheid, consistentie en ondersteuning van het Azure-ecosysteem.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Een Event Hubs-naamruimte maken
> * De voorbeeldproject klonen
> * Flink producent uitvoeren 
> * Flink consument uitvoeren

> [!NOTE]
> Dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/flink)

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie, zorg ervoor dat u de volgende vereisten:

* Lees het artikel [Event Hubs voor Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md) door. 
* Een Azure-abonnement. Als u nog geen account hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.
* [Java Development Kit (JDK) 1.7+](https://aka.ms/azure-jdks)
    * Voer op Ubuntu `apt-get install default-jdk` uit om de JDK te installeren.
    * Zorg dat de omgevingsvariabele JAVA_HOME verwijst naar de map waarin de JDK is geïnstalleerd.
* [Download](http://maven.apache.org/download.cgi) en [installeren](http://maven.apache.org/install.html) een binaire Maven-archief
    * Op Ubuntu kunt u `apt-get install maven` uitvoeren om Maven te installeren.
* [Git](https://www.git-scm.com/downloads)
    * Op Ubuntu kunt u `sudo apt-get install git` uitvoeren om Git te installeren.

## <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken

Een Event Hubs-naamruimte is vereist om te verzenden of ontvangen van alle Event Hubs-service. Zie [maken Kafka ingeschakeld Event Hubs](event-hubs-create-kafka-enabled.md) voor informatie over het ophalen van een Event Hubs Kafka-eindpunt. Zorg ervoor dat u het kopiëren van de Event Hubs-verbindingsreeks voor later gebruik.

## <a name="clone-the-example-project"></a>De voorbeeldproject klonen

Nu dat u een verbindingsreeks Event Hubs waarvoor Kafka is ingeschakeld hebt, de Azure Event Hubs voor Kafka-opslagplaats te klonen en navigeer naar de `flink` submap:

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/flink
```

## <a name="run-flink-producer"></a>Flink producent uitvoeren

In het opgegeven Flink producent voorbeeld berichten verzenden naar de Event Hubs-service.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Geef een Event Hubs Kafka-eindpunt

#### <a name="producerconfig"></a>Producer.config

Update de `bootstrap.servers` en `sasl.jaas.config` waarden in `producer/src/main/resources/producer.config` om te leiden van de producent naar de Event Hubs Kafka-eindpunt met de juiste verificatie.

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

Voor de producent uitvoeren vanaf de opdrachtregel, genereren de JAR en vervolgens uitvoeren in Maven (of de JAR met behulp van Maven, klikt u vervolgens uitvoeren in Java door toe te voegen van de benodigde Kafka JAR(s) aan het klassepad genereren):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestProducer"
```

De producent gaat nu verder met Event Hub verzenden van gebeurtenissen naar het Kafka worden ingeschakeld op onderwerp `test` en de gebeurtenissen naar de stdout afdrukken.

## <a name="run-flink-consumer"></a>Flink consument uitvoeren

Met behulp van de opgegeven consumentvoorbeelden, ontvangt berichten van Event Hubs waarvoor de Kafka is ingeschakeld.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Geef een Event Hubs Kafka-eindpunt

#### <a name="consumerconfig"></a>consumer.config

Update de `bootstrap.servers` en `sasl.jaas.config` waarden in `consumer/src/main/resources/consumer.config` om te leiden van de gebruiker voor het Event Hubs Kafka-eindpunt met de juiste verificatie.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
group.id=FlinkExampleConsumer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-consumer-from-the-command-line"></a>Consumenten uitvoeren vanaf de opdrachtregel

Voor de consument uitvoeren vanaf de opdrachtregel, genereren de JAR en vervolgens uitvoeren in Maven (of de JAR met behulp van Maven, klikt u vervolgens uitvoeren in Java door toe te voegen van de benodigde Kafka JAR(s) aan het klassepad genereren):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestConsumer"
```

Als de Kafka-functionaliteit event hub heeft gebeurtenissen (bijvoorbeeld als de producent wordt ook uitgevoerd), klikt u vervolgens de consument nu begint het ontvangen van gebeurtenissen in het onderwerp `test`.

Bekijk [Flink van Kafka-Connector-handleiding](https://ci.apache.org/projects/flink/flink-docs-stable/dev/connectors/kafka.html) voor meer informatie gedetailleerde over het verbinden van Flink met Kafka.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie, het geleerde verbinden Flink Apache Kafka-functionaliteit eventhubs zonder te wijzigen van uw clients protocol of uitvoeren van uw eigen clusters. U de volgende stappen uit als onderdeel van deze zelfstudie hebt uitgevoerd: 

> [!div class="checklist"]
> * Een Event Hubs-naamruimte maken
> * De voorbeeldproject klonen
> * Flink producent uitvoeren 
> * Flink consument uitvoeren

Zie voor meer informatie over Event Hubs en Event Hubs voor Kafka, het volgende onderwerp:  

- [Meer informatie over Event Hubs](event-hubs-what-is-event-hubs.md)
- [Eventhubs voor Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md)
- [Over het maken van Kafka ingeschakeld Event Hubs](event-hubs-create-kafka-enabled.md)
- [Stream naar Event Hubs van uw Kafka-toepassingen](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Een Kafka-broker in een Kafka-functionaliteit event hub spiegelen](event-hubs-kafka-mirror-maker-tutorial.md)
- [Verbinding maken met Apache Spark naar een Kafka-functionaliteit event hub](event-hubs-kafka-spark-tutorial.md)
- [Integratie van Kafka verbinding maken met een Kafka-functionaliteit event hub](event-hubs-kafka-connect-tutorial.md)
- [Akka Streams verbinden met een Kafka-functionaliteit event hub](event-hubs-kafka-akka-streams-tutorial.md)
- [Voorbeelden verkennen op onze GitHub](https://github.com/Azure/azure-event-hubs-for-kafka)
