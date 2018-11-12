---
title: "Zelfstudie: Werken met de Producer- en Consumer-API's van Apache Kafka - Azure HDInsight "
description: Leer hoe u de Producer- en Consumer-API's van Apache Kafka gebruikt met Kafka in HDInsight. In deze zelfstudie leert u hoe u deze API's vanuit een Java-toepassing gebruikt met Kafka in HDInsight.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 04/16/2018
ms.openlocfilehash: c824462a94a18b0633e53777cd0d73b4dae594b8
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51012357"
---
# <a name="tutorial-use-the-apache-kafka-producer-and-consumer-apis"></a>Zelfstudie: Werken met de Producer- en Consumer-API's van Apache Kafka

Leer hoe u de Producer- en Consumer-API's van Kafka gebruikt met Kafka in HDInsight.

Met de Producer-API van Kafka kunnen toepassingen gegevensstromen naar het Kafka-cluster verzenden. Met de Consumer-API van Kafka kunnen toepassingen gegevensstromen uit het cluster lezen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De ontwikkelomgeving instellen
> * De implementatieomgeving instellen
> * De code begrijpen
> * De toepassing compileren en implementeren
> * De toepassing uitvoeren in het cluster

Meer informatie over de [Producer-API](https://kafka.apache.org/documentation/#producerapi) en de [Consumer-API](https://kafka.apache.org/documentation/#consumerapi) kunt u lezen in de Apache-documentatie (Engelstalig).

## <a name="set-up-your-development-environment"></a>De ontwikkelomgeving instellen

De volgende onderdelen moeten zijn geïnstalleerd in de ontwikkelingsomgeving:

* [Java JDK 8](https://aka.ms/azure-jdks) of een equivalent, zoals OpenJDK.

* [Apache Maven](http://maven.apache.org/)

* Een SSH-client en de opdracht `scp`. Zie het document [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

* Een teksteditor of Java IDE.

U kunt de volgende omgevingsvariabelen instellen wanneer u Java en de JDK installeert op uw ontwikkelwerkstation. U moet dan echter wel controleren of ze bestaan en of ze de juiste waarden voor uw systeem bevatten.

* `JAVA_HOME` -moet verwijzen naar de map waarin de JDK is geïnstalleerd.
* `PATH` -moet de volgende paden bevatten:
  
    * `JAVA_HOME` (of het equivalente pad).
    * `JAVA_HOME\bin` (of het equivalente pad).
    * De map waarin Maven is geïnstalleerd.

## <a name="set-up-your-deployment-environment"></a>De implementatieomgeving instellen

Deze zelfstudie vereist Kafka in HDInsight 3.6. Zie [Aan de slag met Kafka in HDInsight](apache-kafka-get-started.md) voor informatie over het maken van een Kafka-cluster in HDInsight.

## <a name="understand-the-code"></a>De code begrijpen

De voorbeeldtoepassing bevindt zich op [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started), in de submap `Producer-Consumer`. De toepassing bestaat hoofdzakelijk uit vier bestanden:

* `pom.xml`: dit bestand definieert de projectafhankelijkheden, de Java-versie en de pakketmethoden.
* `Producer.java`: dit bestand verstuurt 1 miljoen (1.000.000) willekeurige zinnen naar Kafka met behulp van de Producer-API.
* `Consumer.java`: dit bestand gebruikt de Consumer-API om gegevens te lezen uit Kafka en deze te verzenden naar STDOUT.
* `Run.java`: de opdrachtregelinterface ie wordt gebruikt voor het uitvoeren van de Producer- en Consumer-code.

### <a name="pomxml"></a>Pom.xml

Belangrijke aandachtspunten voor het bestand `pom.xml`:

* Afhankelijkheden: dit project is afhankelijk van de Kafka-API's Producer en Consumer, die worden geleverd door het pakket `kafka-clients`. Deze afhankelijkheid wordt gedefinieerd met de volgende XML-code:

    ```xml
    <!-- Kafka client for producer/consumer operations -->
    <dependency>
      <groupId>org.apache.kafka</groupId>
      <artifactId>kafka-clients</artifactId>
      <version>${kafka.version}</version>
    </dependency>
    ```

    > [!NOTE]
    > De vermelding `${kafka.version}` wordt gedeclareerd in de sectie `<properties>..</properties>` van `pom.xml`, en wordt geconfigureerd voor de Kafka-versie van het HDInsight-cluster.

* Plugins: de Maven-plugins bieden diverse mogelijkheden. In dit project worden de volgende plugins of invoegtoepassingen gebruikt:

    * `maven-compiler-plugin`: wordt gebruikt om de Java-versie die wordt gebruikt door het project in te stellen op 8. Dit is de versie van Java die door HDInsight 3.6 wordt gebruikt.
    * `maven-shade-plugin`: wordt gebruikt voor het genereren van een uber jar die deze toepassing bevat, evenals eventuele afhankelijkheden. Dit bestand wordt ook gebruikt om het toegangspunt van de toepassing in te stellen, zodat u het Jar-bestand rechtstreeks kunt uitvoeren, dus zonder de hoofdklasse op te geven.

### <a name="producerjava"></a>Producer.java

De Producent-API communiceert met de Kafka-brokerhosts (werkknooppunten) voor het opslaan van gegevens in een Kafka-onderwerp. Het volgende codefragment is afkomstig uit het bestand `Producer.java`:

```java
package com.microsoft.example;

import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerRecord;
import java.util.Properties;
import java.util.Random;
import java.io.IOException;

public class Producer
{
    public static void produce(String brokers) throws IOException
    {

        // Set properties used to configure the producer
        Properties properties = new Properties();
        // Set the brokers (bootstrap servers)
        properties.setProperty("bootstrap.servers", brokers);
        // Set how to serialize key/value pairs
        properties.setProperty("key.serializer","org.apache.kafka.common.serialization.StringSerializer");
        properties.setProperty("value.serializer","org.apache.kafka.common.serialization.StringSerializer");
        KafkaProducer<String, String> producer = new KafkaProducer<>(properties);

        // So we can generate random sentences
        Random random = new Random();
        String[] sentences = new String[] {
                "the cow jumped over the moon",
                "an apple a day keeps the doctor away",
                "four score and seven years ago",
                "snow white and the seven dwarfs",
                "i am at two with nature"
        };

        String progressAnimation = "|/-\\";
        // Produce a bunch of records
        for(int i = 0; i < 1000000; i++) {
            // Pick a sentence at random
            String sentence = sentences[random.nextInt(sentences.length)];
            // Send the sentence to the test topic
            producer.send(new ProducerRecord<String, String>("test", sentence));
            String progressBar = "\r" + progressAnimation.charAt(i % progressAnimation.length()) + " " + i;
            System.out.write(progressBar.getBytes());
        }
    }
}
```

Deze code maakt verbinding met de Kafka-brokerhosts (werkknooppunten) en stuurt vervolgens 1.000.000 zinnen naar Kafka met behulp van de Producer-API.

### <a name="consumerjava"></a>Consumer.java

De Consumer-API communiceert met de Kafka-brokerhosts (werkknooppunten) en leest records in een lus. Het volgende codefragment is afkomstig uit het bestand `Consumer.java`:

```java
package com.microsoft.example;

import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import java.util.Properties;
import java.util.Arrays;

public class Consumer {
    public static void consume(String brokers, String groupId) {
        // Create a consumer
        KafkaConsumer<String, String> consumer;
        // Configure the consumer
        Properties properties = new Properties();
        // Point it to the brokers
        properties.setProperty("bootstrap.servers", brokers);
        // Set the consumer group (all consumers must belong to a group).
        properties.setProperty("group.id", groupId);
        // Set how to serialize key/value pairs
        properties.setProperty("key.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
        properties.setProperty("value.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
        // When a group is first created, it has no offset stored to start reading from. This tells it to start
        // with the earliest record in the stream.
        properties.setProperty("auto.offset.reset","earliest");
        consumer = new KafkaConsumer<>(properties);

        // Subscribe to the 'test' topic
        consumer.subscribe(Arrays.asList("test"));

        // Loop until ctrl + c
        int count = 0;
        while(true) {
            // Poll for records
            ConsumerRecords<String, String> records = consumer.poll(200);
            // Did we get any?
            if (records.count() == 0) {
                // timeout/nothing to read
            } else {
                // Yes, loop over records
                for(ConsumerRecord<String, String> record: records) {
                    // Display record and count
                    count += 1;
                    System.out.println( count + ": " + record.value());
                }
            }
        }
    }
}
```

In deze code is de Consumer-API geconfigureerd voor het lezen vanaf het begin van het onderwerp (`auto.offset.reset` is ingesteld op `earliest`.)

### <a name="runjava"></a>Run.java

Het bestand `Run.java` biedt een opdrachtregelinterface voor het uitvoeren van code van de Producer- of Consumer-API. U moet de gegevens van de Kafka-brokerhost opgeven als een parameter. U kunt eventueel een groeps-id opgeven, die wordt gebruikt door het Consumer-proces. Als u meerdere Consumer-exemplaren met dezelfde groeps-id maakt, worden de gelezen zinnen uit het onderwerp verdeeld over de exemplaren.

```java
package com.microsoft.example;

import java.io.IOException;
import java.util.UUID;

// Handle starting producer or consumer
public class Run {
    public static void main(String[] args) throws IOException {
        if(args.length < 2) {
            usage();
        }

        // Get the brokers
        String brokers = args[1];
        switch(args[0].toLowerCase()) {
            case "producer":
                Producer.produce(brokers);
                break;
            case "consumer":
                // Either a groupId was passed in, or we need a random one
                String groupId;
                if(args.length == 3) {
                    groupId = args[2];
                } else {
                    groupId = UUID.randomUUID().toString();
                }
                Consumer.consume(brokers, groupId);
                break;
            default:
                usage();
        }
        System.exit(0);
    }
    // Display usage
    public static void usage() {
        System.out.println("Usage:");
        System.out.println("kafka-example.jar <producer|consumer> brokerhosts [groupid]");
        System.exit(1);
    }
}
```

## <a name="build-and-deploy-the-example"></a>Het voorbeeld compileren en implementeren

1. Download de voorbeelden van [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Wijzig de mappen in de locatie van de map `Producer-Consumer` en gebruik de volgende opdracht:

    ```
    mvn clean package
    ```

    Met deze opdracht maakt u een directory met de naam `target`, die een bestand met de naam `kafka-producer-consumer-1.0-SNAPSHOT.jar` bevat.

3. Gebruik de volgende opdrachten om het bestand `kafka-producer-consumer-1.0-SNAPSHOT.jar` te kopiëren naar uw HDInsight-cluster:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    Vervang **SSHUSER** door de SSH-gebruiker van uw cluster en vervang **CLUSTERNAME** door de naam van het cluster. Voer het wachtwoord van de SSH-gebruiker in wanneer hierom wordt gevraagd.

## <a id="run"></a>Het voorbeeld uitvoeren

1. Gebruik de volgende opdracht om een SSH-verbinding met het cluster op te zetten:

    ```bash
    ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Vervang **SSHUSER** door de SSH-gebruiker van uw cluster en vervang **CLUSTERNAME** door de naam van het cluster. Voer het wachtwoord voor het SSH-gebruikersaccount in wanneer hierom wordt gevraagd. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie over het gebruik van `scp` met HDInsight.

2. Gebruik de volgende stappen om de Kafka onderwerpen te maken die worden gebruikt door dit voorbeeld:

    1. Gebruik de volgende opdrachten om de clusternaam op te slaan in een variabele en een hulpprogramma voor het parseren van JSON (`jq`) te installeren. Wanneer u daarom wordt gevraagd, geeft u de naam van het Kafka-cluster op:
    
        ```bash
        sudo apt -y install jq
        read -p 'Enter your Kafka cluster name:' CLUSTERNAME
        ```
    
    2. Gebruik de volgende opdrachten als u de Kafka-brokerhosts en de Zookeeper-hosts wilt opvragen. Voer desgevraagd het wachtwoord voor het account voor clusteraanmelding (admin).
    
        ```bash
        export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
        export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
        ```

    3. Gebruik de volgende opdracht om het onderwerp `test` te maken:

        ```bash
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
        ```
    4. U kunt ook het JAR-bestand gebruiken om een onderwerp te maken. Gebruik bijvoorbeeld de volgende opdracht om het onderwerp `test2` te maken:

        ```bash
        java -jar kafka-producer-consumer.jar create test2 $KAFKABROKERS
        ```

3. Gebruik de volgende opdracht om de Producer-API uit te voeren en gegevens te schrijven naar het onderwerp:

    ```bash
    java -jar kafka-producer-consumer.jar producer test $KAFKABROKERS
    ```

4. Als dit proces is voltooid, gebruikt u de volgende opdracht om gegevens uit het onderwerp te lezen:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS
    ```
   
    De gelezen records worden weergegeven, samen met een telling van de records.

5. Gebruik __Ctrl + C__ om de consument af te sluiten.

### <a name="multiple-consumers"></a>Meerdere consumenten

Kafka-consumenten gebruiken een consumentengroep bij het lezen van records. Door dezelfde groep voor meerdere consumenten te gebruiken, worden leestaken voor onderwerpen gelijk verdeeld. Elke consument in de groep ontvangt een deel van de records.

De Consumer-toepassing accepteert een parameter die wordt gebruikt als de groeps-id. Met de volgende opdracht start u bijvoorbeeld een Consumer met behulp van de groeps-id `mygroup`:
   
```bash
java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS mygroup
```

Om dit proces in actie te zien, gebruikt u de volgende opdracht:

```bash
tmux new-session 'java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS mygroup' \; split-w
indow -h 'java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS mygroup' \; attach
```

Deze opdracht gebruikt `tmux` om de terminal op te splitsen in twee kolommen. In elke kolom wordt een Consumer gestart, met dezelfde waarde voor de groeps-id. Als de Consumers klaar zijn met lezen, ziet u dat ieder Consumer slechts een deel van de records heeft gelezen. Druk tweemaal op __Ctrl + C __ tweemaal om de `tmux` af te sluiten.

Gebruik door clients binnen dezelfde groep wordt verwerkt door de partities voor het onderwerp. Het eerder gemaakte onderwerp `test` heeft acht partities. Als u acht Consumers start, leest elke Consumer records uit één partitie van het onderwerp.

> [!IMPORTANT]
> Een consumentengroep kan niet meer consumentexemplaren dan partities bevatten. In dit voorbeeld kan één consumentengroep maximaal acht consumenten bevatten, omdat het onderwerp dit aantal partities heeft. U kunt ook meerdere consumentengroepen hebben, waarvan elke groep niet meer dan acht consumenten bevat.

Records worden in Kafka opgeslagen in de volgorde waarin deze worden ontvangen binnen een partitie. Als u records *binnen een partitie* op volgorde wilt leveren, maakt u een consumentengroep waarvan het aantal consumentexemplaren gelijk is aan het aantal partities. Als u records *binnen het onderwerp* op volgorde wilt leveren, maakt u een consumentengroep met slechts één consumentexemplaar.

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u de Producer- en Consumer-API's van Kafka gebruikt met Kafka in HDInsight. Gebruik de volgende documenten voor meer informatie over het werken met Kafka:

* [Kafka-logboekbestanden analyseren](apache-kafka-log-analytics-operations-management.md)
* [Gegevens repliceren tussen Kafka-clusters](apache-kafka-mirroring.md)
* [Streams API van Kafka met HDInsight](apache-kafka-streams-api.md)
