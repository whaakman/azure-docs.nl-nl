---
title: Streamen naar Event Hubs waarvoor Kafka is ingeschakeld - Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat informatie over het streamen naar Azure Event Hubs met behulp van het Kafka-protocol en API's.
services: event-hubs
author: basilhariri
ms.author: bahariri
ms.service: event-hubs
ms.topic: quickstart
ms.custom: seodec18
ms.date: 12/06/2018
ms.openlocfilehash: d6e293b80df1ab227a3100040a54b189f48d8027
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53102806"
---
# <a name="stream-into-event-hubs-for-the-apache-kafka"></a>Streamen naar Event Hubs voor Apache Kafka
Deze snelstart laat zien hoe u kunt streamen naar Event Hubs waarvoor Kafka is ingeschakeld zonder uw protocolclients te wijzigen of uw eigen clusters uit te voeren. U leert hoe u uw producenten kunt gebruiken en consumenten met Event Hubs waarvoor Kafka is ingeschakeld, kunnen praten met slechts een configuratiewijziging in uw toepassingen. Azure Event Hubs ondersteunt [Apache Kafka versie 1.0.](https://kafka.apache.org/10/documentation.html)

> [!NOTE]
> Dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/java)

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u aan de volgende vereisten voldoet om deze snelstart uit te voeren:

* Lees het artikel [Event Hubs voor Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md) door.
* Een Azure-abonnement. Als u nog geen account hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.
* [Java Development Kit (JDK) 1.7+](https://aka.ms/azure-jdks).
* [Download](http://maven.apache.org/download.cgi) en [installeer](http://maven.apache.org/install.html) een binair Maven-archief.
* [Git](https://www.git-scm.com/)
* [Een Event Hubs-naamruimte waarvoor Kafka is ingeschakeld](event-hubs-create.md)

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken waarvoor Kafka is ingeschakeld

1. Meld u aan bij de [Azure-portal] [Azure-portal] en klik linksboven in het scherm op **Een resource maken**.

2. Zoek naar Event Hubs en selecteer de hier getoonde opties:
    
    ![Zoeken naar Event Hubs in de portal](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Geef een unieke naam op en schakel Kafka in op de naamruimte. Klik op **Create**.
    
    ![Een naamruimte maken](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.jpg)
 
4. Als de naamruimte is gemaakt, klikt u op het tabblad **Instellingen** op **Beleid voor gedeelde toegang** om de verbindingsreeks op te halen.

    ![Klikken op Beleid voor gedeelde toegang](./media/event-hubs-create/create-event-hub7.png)

5. U kunt de standaardwaarde **RootManageSharedAccessKey** kiezen of een nieuwe beleid toevoegen. Klik op de beleidsnaam en kopieer de verbindingsreeks. 
    
    ![Beleid selecteren](./media/event-hubs-create/create-event-hub8.png)
 
6. Voeg deze verbindingsreeks toe aan de Kafka-toepassingsconfiguratie.

U kunt nu gebeurtenissen vanaf uw toepassing, waarbij gebruikgemaakt wordt van het Kafka-protocol, naar Event Hubs streamen.

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Berichten verzenden en ontvangen met Kafka in Event Hubs

1. Kloon de [Azure Event Hubs for Kafka-opslagplaats](https://github.com/Azure/azure-event-hubs-for-kafka).

2. Navigeer naar `azure-event-hubs-for-kafka/quickstart/java/producer`.

3. Werk de configuratiegegevens voor de producent in `src/main/resources/producer.config` als volgt bij:

    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```
    
4. Voer de producentcode uit en stream deze naar Event Hubs waarvoor Kafka is ingeschakeld.
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
    
5. Navigeer naar `azure-event-hubs-for-kafka/quickstart/java/consumer`.

6. Werk de configuratiegegevens voor de consument in `src/main/resources/consumer.config` als volgt bij:
   
    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```

7. Voer de consumentcode en het proces uit vanuit Event Hubs waarvoor Kafka is ingeschakeld met behulp van uw Kafka-clients:

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Als uw Event Hubs Kafka-cluster gebeurtenissen heeft, moet u deze nu ontvangen van de consument.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd u hoe u kunt streamen naar Event Hubs waarvoor Kafka is ingeschakeld zonder uw protocolclients te wijzigen of uw eigen clusters uit te voeren. Ga verder met de volgende zelfstudie voor meer informatie:

* [Meer informatie over Event Hubs](event-hubs-what-is-event-hubs.md)
* [Meer informatie over Event Hubs for Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* [Meer voorbeelden van de Event Hubs for Kafka-GitHub verkennen](https://github.com/Azure/azure-event-hubs-for-kafka)
* Gebruik [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) om [gebeurtenissen van Kafka on-premises te streamen naar Event Hubs waarvoor Kafka is ingeschakeld in de cloud](event-hubs-kafka-mirror-maker-tutorial.md).
* Meer informatie over streamen naar Kafka waarvoor Event Hubs is ingeschakeld, met behulp van [Apache Flink](event-hubs-kafka-flink-tutorial.md) of [Akka Streams](event-hubs-kafka-akka-streams-tutorial.md)
