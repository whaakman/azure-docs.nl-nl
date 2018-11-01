---
title: Akka Streams voor Apache Kafka met Azure Eventhubs | Microsoft Docs
description: Event Hub Akka Streams verbinden met een Kafka ingeschakeld
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: mvc
ms.date: 08/06/2018
ms.author: bahariri
ms.openlocfilehash: 9cfaec69d3c9cea7f6f3860e8f07df6dc1638a9a
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416066"
---
# <a name="using-akka-streams-with-event-hubs-for-apache-kafka"></a>Met behulp van Akka Streams voor Apache Kafka met Eventhubs
Deze zelfstudie leert u hoe verbinding Akka stromen maken met Kafka-functionaliteit eventhubs zonder te wijzigen van uw clients protocol of uitvoeren van uw eigen clusters. Biedt ondersteuning voor Azure Event Hubs voor de Kafka [Apache Kafka, versie 1.0.](https://kafka.apache.org/10/documentation.html)

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Een Event Hubs-naamruimte maken
> * De voorbeeldproject klonen
> * Akka Streams producent uitvoeren 
> * Akka Streams consument uitvoeren

> [!NOTE]
> Dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/akka/java)

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie, zorg ervoor dat u de volgende vereisten:

* Lees het artikel [Event Hubs voor Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md) door. 
* Een Azure-abonnement. Als u nog geen account hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.
* [Java Development Kit (JDK 1.8 +)](https://aka.ms/azure-jdks)
    * Voer op Ubuntu `apt-get install default-jdk` uit om de JDK te installeren.
    * Zorg dat de omgevingsvariabele JAVA_HOME verwijst naar de map waarin de JDK is geïnstalleerd.
* [Download](http://maven.apache.org/download.cgi) en [installeren](http://maven.apache.org/install.html) een binaire Maven-archief
    * Op Ubuntu kunt u `apt-get install maven` uitvoeren om Maven te installeren.
* [Git](https://www.git-scm.com/downloads)
    * Op Ubuntu kunt u `sudo apt-get install git` uitvoeren om Git te installeren.

## <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken

Een Event Hubs-naamruimte is vereist om te verzenden of ontvangen van alle Event Hubs-service. Zie [maken Kafka ingeschakeld Event Hubs](event-hubs-create-kafka-enabled.md) voor informatie over het ophalen van een Event Hubs Kafka-eindpunt. Zorg ervoor dat u het kopiëren van de Event Hubs-verbindingsreeks voor later gebruik.

## <a name="clone-the-example-project"></a>De voorbeeldproject klonen

Nu dat u een verbindingsreeks Event Hubs waarvoor Kafka is ingeschakeld hebt, de Azure Event Hubs voor Kafka-opslagplaats te klonen en navigeer naar de `akka` submap:

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/akka/java
```

## <a name="run-akka-streams-producer"></a>Akka Streams producent uitvoeren

In het opgegeven Akka Streams producent voorbeeld berichten verzenden naar de Event Hubs-service.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Geef een Event Hubs Kafka-eindpunt

#### <a name="producer-applicationconf"></a>Producent application.conf

Update de `bootstrap.servers` en `sasl.jaas.config` waarden in `producer/src/main/resources/application.conf` om te leiden van de producent naar de Event Hubs Kafka-eindpunt met de juiste verificatie.

```xml
akka.kafka.producer {
    #Akka Kafka producer properties can be defined here


    # Properties defined by org.apache.kafka.clients.producer.ProducerConfig
    # can be defined in this configuration section.
    kafka-clients {
        bootstrap.servers="{YOUR.EVENTHUBS.FQDN}:9093"
        sasl.mechanism=PLAIN
        security.protocol=SASL_SSL
        sasl.jaas.config="org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"{YOUR.EVENTHUBS.CONNECTION.STRING}\";"
    }
}
```

### <a name="run-producer-from-the-command-line"></a>Producent uitvoeren vanaf de opdrachtregel

Voor de producent uitvoeren vanaf de opdrachtregel, genereren de JAR en vervolgens uitvoeren in Maven (of de JAR met behulp van Maven, klikt u vervolgens uitvoeren in Java door toe te voegen van de benodigde Kafka JAR(s) aan het klassepad genereren):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestProducer"
```

De producent gestart voor het verzenden van gebeurtenissen naar de Kafka-functionaliteit event hub op onderwerp `test`, en worden de gebeurtenissen naar de stdout afgedrukt.

## <a name="run-akka-streams-consumer"></a>Akka Streams consument uitvoeren

In het voorbeeld opgegeven consumenten berichten ontvangen van de Kafka-functionaliteit eventhubs.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Geef een Event Hubs Kafka-eindpunt

#### <a name="consumer-applicationconf"></a>Consumenten application.conf

Update de `bootstrap.servers` en `sasl.jaas.config` waarden in `consumer/src/main/resources/application.conf` om te leiden van de gebruiker voor het Event Hubs Kafka-eindpunt met de juiste verificatie.

```xml
akka.kafka.consumer {
    #Akka Kafka consumer properties defined here
    wakeup-timeout=60s

    # Properties defined by org.apache.kafka.clients.consumer.ConsumerConfig
    # defined in this configuration section.
    kafka-clients {
       request.timeout.ms=60000
       group.id=akka-example-consumer

       bootstrap.servers="{YOUR.EVENTHUBS.FQDN}:9093"
       sasl.mechanism=PLAIN
       security.protocol=SASL_SSL
       sasl.jaas.config="org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"{YOUR.EVENTHUBS.CONNECTION.STRING}\";"
    }
}
```

### <a name="run-consumer-from-the-command-line"></a>Consumenten uitvoeren vanaf de opdrachtregel

Voor de consument uitvoeren vanaf de opdrachtregel, genereren de JAR en vervolgens uitvoeren in Maven (of de JAR met behulp van Maven, klikt u vervolgens uitvoeren in Java door toe te voegen van de benodigde Kafka JAR(s) aan het klassepad genereren):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestConsumer"
```

Als de Kafka-functionaliteit event hub gebeurtenissen (bijvoorbeeld, als uw producent wordt ook uitgevoerd), en vervolgens de consument begint het ontvangen van gebeurtenissen van onderwerp `test`. 

Bekijk de [Akka Streams Kafka handleiding](https://doc.akka.io/docs/akka-stream-kafka/current/home.html) voor meer informatie over Akka Streams gedetailleerde.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe Akka Streams zonder te wijzigen van uw clients protocol of uitvoeren van uw eigen clusters verbinden gebeurtenishubs waarvoor Kafka is ingeschakeld. Biedt ondersteuning voor Azure Event Hubs voor de Kafka [Apache Kafka, versie 1.0.](https://kafka.apache.org/10/documentation.html). U hebt de volgende acties als onderdeel van deze zelfstudie: 

> [!div class="checklist"]
> * Een Event Hubs-naamruimte maken
> * De voorbeeldproject klonen
> * Akka Streams producent uitvoeren 
> * Akka Streams consument uitvoeren

Zie voor meer informatie over Event Hubs en Event Hubs voor Kafka, het volgende onderwerp:  

* [Meer informatie over Event Hubs](event-hubs-what-is-event-hubs.md)
* [Meer informatie over Event Hubs for Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* [Meer voorbeelden van de Event Hubs for Kafka-GitHub verkennen](https://github.com/Azure/azure-event-hubs-for-kafka)
* Gebruik [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) om [gebeurtenissen van Kafka on-premises te streamen naar Event Hubs waarvoor Kafka is ingeschakeld in de cloud](event-hubs-kafka-mirror-maker-tutorial.md).
* Meer informatie over het streamen naar Kafka ingeschakelde Event Hubs met behulp [systeemeigen Kafka toepassingen](event-hubs-quickstart-kafka-enabled-event-hubs.md) of [Apache Flink](event-hubs-kafka-flink-tutorial.md)
