---
title: Kafka MirrorMaker voor Kafka ecosysteem met Azure Event Hubs | Microsoft Docs
description: Kafka MirrorMaker gebruiken voor het spiegelen van een cluster Kafka in Event Hubs.
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: mirror-maker
ms.custom: mvc
ms.date: 05/07/2018
ms.author: bahariri
ms.openlocfilehash: 0693fc2fff5735fb2b3c0a9b8f1d3d256746f40d
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298318"
---
# <a name="using-kafka-mirrormaker-with-event-hubs-for-kafka-ecosystems"></a>Met behulp van Kafka MirrorMaker met Event Hubs voor Kafka ecosystemen

> [!NOTE]
> Dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs)

Een belangrijke overweging voor moderne cloud-apps voor scale is de mogelijkheid om te werken, te verbeteren en infrastructuur wijzigen zonder dat service wordt onderbroken. Deze zelfstudie laat zien hoe een Kafka ingeschakeld event hub en Kafka MirrorMaker kunt een bestaande Kafka-pipeline integreren in Azure door 'spiegelen' van de invoerstroom Kafka in de Event Hubs-service. 

Een Azure Event Hubs Kafka-eindpunt kunt u verbinding maken met Azure Event Hubs met behulp van de Kafka-protocol (dat wil zeggen Kafka clients). U kunt minimale wijzigingen aanbrengt in een toepassing Kafka, verbinding maken met Azure Event Hubs en profiteren van de voordelen van het Azure-ecosysteem. Kafka ingeschakeld Event Hubs ondersteunt momenteel Kafka versies 1.0 en hoger.

In dit voorbeeld laat zien hoe een broker Kafka in een Kafka ingeschakelde event hub met Kafka MirrorMaker spiegelen.

   ![Kafka MirrorMaker met Event Hubs](./media/event-hubs-kafka-mirror-maker-tutorial/evnent-hubs-mirror-maker1.png)

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u hebt voor het voltooien van deze zelfstudie:

* Een Azure-abonnement. Als u nog geen account hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * Voer op Ubuntu `apt-get install default-jdk` uit om de JDK te installeren.
    * Zorg dat de omgevingsvariabele JAVA_HOME verwijst naar de map waarin de JDK is geïnstalleerd.
* [Download](http://maven.apache.org/download.cgi) en [installeren](http://maven.apache.org/install.html) een binaire Maven-archief
    * Op Ubuntu kunt u `apt-get install maven` uitvoeren om Maven te installeren.
* [Git](https://www.git-scm.com/downloads)
    * Op Ubuntu kunt u `sudo apt-get install git` uitvoeren om Git te installeren.

## <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken

Een Event Hubs-naamruimte is vereist voor het verzenden en ontvangen van alle Event Hubs-service. Zie [Event Hub maken van een Kafka ingeschakeld](event-hubs-create.md) voor instructies over het ophalen van een Event Hubs Kafka-eindpunt. Zorg ervoor dat voor het kopiëren van de Event Hubs-verbindingsreeks voor later gebruik.

## <a name="clone-the-example-project"></a>Klonen van de voorbeeldproject

Nu dat u hebt een Kafka Event Hubs-verbindingsreeks ingeschakeld, kloon de opslagplaats Azure Event Hubs en navigeer naar de `mirror-maker` submap:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/mirror-maker
```

## <a name="set-up-a-kafka-cluster"></a>Een Kafka-cluster instellen

Gebruik de [Kafka Quick Start guide](https://kafka.apache.org/quickstart) naar een cluster met de gewenste instellingen instellen (of gebruik een bestaand Kafka-cluster).

## <a name="kafka-mirrormaker"></a>Kafka MirrorMaker

Kafka MirrorMaker kunnen de 'mirroring' van een stroom. Gegeven bron- en doelserver Kafka-clusters, MirrorMaker zorgt ervoor dat berichten die worden verzonden naar de broncluster worden ontvangen door de bron- en doelserver clusters. In dit voorbeeld laat zien hoe voor het spiegelen van een bron Kafka-cluster met een bestemming Kafka ingeschakeld event hub. Dit scenario kan worden gebruikt om gegevens te verzenden vanuit een bestaande Kafka-pipeline naar Event Hubs zonder dat de stroom van gegevens wordt onderbroken. 

Zie voor meer informatie over Kafka MirrorMaker, de [Kafka spiegelen/MirrorMaker handleiding](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330).

### <a name="configuration"></a>Configuratie

Voor het configureren van Kafka MirrorMaker hieraan een cluster met Kafka als de consument/bron- en een Kafka ingeschakeld event hub als de producent/het doel.

#### <a name="consumer-configuration"></a>Configuratie van de consumer

Het configuratiebestand van de consument bijwerken `source-kafka.config`, die de eigenschappen van de bron Kafka cluster MirrorMaker vertelt.

##### <a name="source-kafkaconfig"></a>bron kafka.config

```xml
bootstrap.servers={SOURCE.KAFKA.IP.ADDRESS1}:{SOURCE.KAFKA.PORT1},{SOURCE.KAFKA.IP.ADDRESS2}:{SOURCE.KAFKA.PORT2},etc
group.id=example-mirrormaker-group
exclude.internal.topics=true
client.id=mirror_maker_consumer
```

#### <a name="producer-configuration"></a>Configuratie van de producent

Nu bijwerken die in het configuratiebestand voor de producent `mirror-eventhub.config`, deze code wordt MirrorMaker de gedupliceerde (of 'gespiegelde') gegevens verzenden naar Event Hubs-service. In het bijzonder wijzigen `bootstrap.servers` en `sasl.jaas.config` om te verwijzen naar uw Event Hubs Kafka-eindpunt. De Event Hubs-service vereist een beveiligde communicatie voor (SASL), die wordt bereikt door het instellen van de laatste drie eigenschappen in de volgende configuratie: 

##### <a name="mirror-eventhubconfig"></a>mirror-eventhub.config

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=mirror_maker_producer

#Required for Event Hubs
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-mirrormaker"></a>MirrorMaker uitvoeren

Voer het script Kafka MirrorMaker vanuit de hoofdmap Kafka directory met behulp van de bijgewerkte configuratiebestanden. Zorg ervoor dat de configuratiebestanden kopiëren naar de hoofdmap Kafka directory, of de paden in de volgende opdracht kunt bijwerken.

```shell
bin/kafka-mirror-maker.sh --consumer.config source-kafka.config --num.streams 1 --producer.config mirror-eventhub.config --whitelist=".*"
```

Om te controleren dat de Kafka ingeschakeld event hub worden bereikt door gebeurtenissen, Zie de statistieken van de inkomende gegevens in de [Azure-portal](https://azure.microsoft.com/features/azure-portal/), of een consumer uitgevoerd door de event hub.

Alle gebeurtenissen die worden verzonden naar de bron Kafka cluster worden ontvangen door zowel de Kafka-cluster met MirrorMaker uitgevoerd, en de gespiegelde Kafka event hub-service is ingeschakeld. Met behulp van MirrorMaker en een eindpunt Event Hubs Kafka, kunt u een bestaande Kafka-pijplijn met de beheerde Azure Event Hubs-service migreren zonder het bestaande cluster wijzigen of een doorlopende gegevensstroom onderbreken.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Event Hubs](event-hubs-what-is-event-hubs.md)
* [Meer informatie over Event Hubs voor Kafka ecosysteem](event-hubs-for-kafka-ecosystem-overview.md)
* Meer informatie over [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) stroom gebeurtenissen op Kafka on-premises naar Kafka ingeschakeld event hubs in de cloud.
* Meer informatie over het stream in Kafka ingeschakelde Event Hubs met behulp [systeemeigen Kafka toepassingen](event-hubs-quickstart-kafka-enabled-event-hubs.md), [Apache Flink](event-hubs-kafka-flink-tutorial.md), of [Akka Streams](event-hubs-kafka-akka-streams-tutorial.md).
