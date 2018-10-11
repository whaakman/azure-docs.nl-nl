---
title: Streamen naar Azure Event Hubs voor Apache Kafka | Microsoft Docs
description: Streamen naar Event Hubs met behulp van API's en het Kafka-protocol.
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2018
ms.author: bahariri
ms.openlocfilehash: 2fbca7179d8bc64a92f79f6eaced8d4f1666c530
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49069213"
---
# <a name="stream-into-event-hubs-for-the-apache-kafka"></a>Streamen naar Event Hubs voor Apache Kafka
Deze snelstart laat zien hoe u kunt streamen naar Event Hubs waarvoor Kafka is ingeschakeld zonder uw protocolclients te wijzigen of uw eigen clusters uit te voeren. U leert hoe u uw producenten kunt gebruiken en consumenten met Event Hubs waarvoor Kafka is ingeschakeld, kunnen praten met slechts een configuratiewijziging in uw toepassingen. Azure Event Hubs ondersteunt [Apache Kafka versie 1.0.](https://kafka.apache.org/10/documentation.html)

> [!NOTE]
> Dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs)

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u aan de volgende vereisten voldoet om deze snelstart uit te voeren:

* Lees het artikel [Event Hubs voor Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md) door.
* Een Azure-abonnement. Als u nog geen account hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Download](http://maven.apache.org/download.cgi) en [installeer](http://maven.apache.org/install.html) een binair Maven-archief.
* [Git](https://www.git-scm.com/)
* [Een Event Hubs-naamruimte waarvoor Kafka is ingeschakeld](event-hubs-create.md)

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken waarvoor Kafka is ingeschakeld

1. Meld u aan bij de [Azure-portal] [Azure-portal] en klik linksboven in het scherm op **Een resource maken**.

2. Zoek naar Event Hubs en selecteer de hier getoonde opties:
    
    ![Zoeken naar Event Hubs in de portal](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Geef een unieke naam op en schakel Kafka in op de naamruimte. Klik op **Create**.
    
    ![Een naamruimte maken](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Als de naamruimte is gemaakt, klikt u op het tabblad **Instellingen** op **Beleid voor gedeelde toegang** om de verbindingsreeks op te halen.

    ![Klikken op Beleid voor gedeelde toegang](./media/event-hubs-create/create-event-hub7.png)

5. U kunt de standaardwaarde **RootManageSharedAccessKey** kiezen of een nieuwe beleid toevoegen. Klik op de beleidsnaam en kopieer de verbindingsreeks. 
    
    ![Beleid selecteren](./media/event-hubs-create/create-event-hub8.png)
 
6. Voeg deze verbindingsreeks toe aan de Kafka-toepassingsconfiguratie.

U kunt nu gebeurtenissen vanaf uw toepassing, waarbij gebruikgemaakt wordt van het Kafka-protocol, naar Event Hubs streamen.

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Berichten verzenden en ontvangen met Kafka in Event Hubs

1. Kloon de [Azure Event Hubs-opslagplaats](https://github.com/Azure/azure-event-hubs).

2. Navigeer naar `azure-event-hubs/samples/kafka/quickstart/producer`.

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
    
5. Navigeer naar `azure-event-hubs/samples/kafka/quickstart/consumer`.

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

> [!div class="nextstepaction"]
> [Apache Kafka MirrorMaker met Event Hubs gebruiken](event-hubs-kafka-mirror-maker-tutorial.md)
