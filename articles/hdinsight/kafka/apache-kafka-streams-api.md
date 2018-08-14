---
title: 'Zelfstudie: Werken met de Streams-API van Apache Kafka - Azure HDInsight '
description: Leer hoe u de Streams-API van Apache Kafka gebruikt met Kafka in HDInsight. Met deze API kunt u gegevensstromen tussen onderwerpen in Kafka verwerken.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 04/17/2018
ms.openlocfilehash: d285575802dd830247533420154f6f5e868272a2
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621042"
---
# <a name="tutorial-apache-kafka-streams-api"></a>Zelfstudie: Streams-API van Apache Kafka

Leer hoe u een toepassing maakt die gebruikmaakt van de Streams-API van Kafka en hoe u deze uitvoert met Kafka in HDInsight. 

De voorbeeldtoepassing die wordt gebruikt in deze zelfstudie, is een app voor het tellen van woorden die via een stream worden aangeboden. Eerst worden er tekstgegevens gelezen uit een onderwerp van Kafka, vervolgens worden afzonderlijke woorden uitgepakt en ten slotte worden de woorden en het aantal woorden opgeslagen in een ander Kafka-onderwerp.

> [!NOTE]
> De verwerking van Kafka-gegevensstromen gebeurt vaak met Apache Spark of Storm. De Streams-API is geïntroduceerd in Kafka versie 0.10.0 (in HDInsight 3.5 en 3.6). Met deze API kunt u gegevensstromen transformeren tussen invoer- en uitvoeronderwerpen. In sommige gevallen kan dit een alternatief zijn voor het maken van een streamingoplossing op basis van Spark of Storm. 
>
> Meer informatie over de Streams-API van Kafka vindt u in het Engelstalige artikel [Intro to Streams](https://kafka.apache.org/10/documentation/streams/) op Apache.org.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De ontwikkelomgeving instellen
> * De code begrijpen
> * De toepassing compileren en implementeren
> * Kafka onderwerpen configureren
> * De code uitvoeren

## <a name="prerequisites"></a>Vereisten

* Een Kafka in HDInsight 3.6-cluster. Zie [Aan de slag met Kafka in HDInsight](apache-kafka-get-started.md) voor informatie over het maken van een Kafka-cluster in HDInsight.

* Voer de stappen in het document [Consumer- en Producer-API's van Kafka](apache-kafka-producer-consumer-api.md) uit. In de stappen in dit document worden de voorbeeldtoepassing en onderwerpen gebruikt die in deze zelfstudie zijn gemaakt.

## <a name="set-up-your-development-environment"></a>De ontwikkelomgeving instellen

De volgende onderdelen moeten zijn geïnstalleerd in de ontwikkelingsomgeving:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) of een equivalent, zoals OpenJDK.

* [Apache Maven](http://maven.apache.org/)

* Een SSH-client en de opdracht `scp`. Zie het document [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

## <a name="understand-the-code"></a>De code begrijpen

De voorbeeldtoepassing bevindt zich op [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started), in de submap `Streaming`. De toepassing bestaat uit twee bestanden:

* `pom.xml`: dit bestand definieert de projectafhankelijkheden, de Java-versie en de pakketmethoden.
* `Stream.java`: dit bestand implementeert de streaming-logica.

### <a name="pomxml"></a>Pom.xml

Belangrijke aandachtspunten voor het bestand `pom.xml`:

* Afhankelijkheden: dit project is afhankelijk van de Kafka-API Streams, die wordt geleverd door het pakket `kafka-clients`. Deze afhankelijkheid wordt gedefinieerd met de volgende XML-code:

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

    * `maven-compiler-plugin`: wordt gebruikt om de Java-versie die wordt gebruikt door het project in te stellen op 8. Java 8 is vereist voor HDInsight 3.6.
    * `maven-shade-plugin`: wordt gebruikt voor het genereren van een uber jar die deze toepassing bevat, evenals eventuele afhankelijkheden. Dit bestand wordt ook gebruikt om het toegangspunt van de toepassing in te stellen, zodat u het Jar-bestand rechtstreeks kunt uitvoeren, dus zonder de hoofdklasse op te geven.

### <a name="streamjava"></a>Stream.java

Het bestand `Stream.java` gebruikt de Streams-API voor het implementeren van een toepassing voor het tellen van woorden. De toepassing leest gegevens uit een Kafka-onderwerp met de naam `test` en schrijft het gelezen aantal woorden naar een onderwerp met de naam `wordcounts`.

Met de volgende code wordt de toepassing gedefinieerd:

```java
package com.microsoft.example;

import org.apache.kafka.common.serialization.Serde;
import org.apache.kafka.common.serialization.Serdes;
import org.apache.kafka.streams.KafkaStreams;
import org.apache.kafka.streams.KeyValue;
import org.apache.kafka.streams.StreamsConfig;
import org.apache.kafka.streams.kstream.KStream;
import org.apache.kafka.streams.kstream.KStreamBuilder;

import java.util.Arrays;
import java.util.Properties;

public class Stream
{
    public static void main( String[] args ) {
        Properties streamsConfig = new Properties();
        // The name must be unique on the Kafka cluster
        streamsConfig.put(StreamsConfig.APPLICATION_ID_CONFIG, "wordcount-example");
        // Brokers
        streamsConfig.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, args[0]);
        // SerDes for key and values
        streamsConfig.put(StreamsConfig.KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());
        streamsConfig.put(StreamsConfig.VALUE_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());

        // Serdes for the word and count
        Serde<String> stringSerde = Serdes.String();
        Serde<Long> longSerde = Serdes.Long();

        KStreamBuilder builder = new KStreamBuilder();
        KStream<String, String> sentences = builder.stream(stringSerde, stringSerde, "test");
        KStream<String, Long> wordCounts = sentences
                .flatMapValues(value -> Arrays.asList(value.toLowerCase().split("\\W+")))
                .map((key, word) -> new KeyValue<>(word, word))
                .countByKey("Counts")
                .toStream();
        wordCounts.to(stringSerde, longSerde, "wordcounts");

        KafkaStreams streams = new KafkaStreams(builder, streamsConfig);
        streams.start();

        Runtime.getRuntime().addShutdownHook(new Thread(streams::close));
    }
}
```


## <a name="build-and-deploy-the-example"></a>Het voorbeeld compileren en implementeren

Als u het project wilt implementeren in het Kafka-cluster in HDInsight, voert u de volgende stappen uit:

1. Download de voorbeelden van [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Wijzig de mappen in de locatie van de map `Streaming` en gebruik de volgende opdracht om een JAR-pakket te maken:

    ```bash
    mvn clean package
    ```

    Met deze opdracht maakt u het pakket op `target/kafka-streaming-1.0-SNAPSHOT.jar`.

3. Gebruik de volgende opdracht om het bestand `kafka-streaming-1.0-SNAPSHOT.jar` naar uw HDInsight-cluster te kopiëren:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar sshuser@clustername-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Vervang `sshuser` door de SSH-gebruiker voor uw cluster en `clustername` door de naam van het cluster. Voer het wachtwoord voor het SSH-gebruikersaccount in wanneer hierom wordt gevraagd. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie over het gebruik van `scp` met HDInsight.

## <a name="create-kafka-topics"></a>Kafka-onderwerpen maken

1. Gebruik de volgende opdracht om een SSH-verbinding met het cluster op te zetten:

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Vervang `sshuser` door de SSH-gebruiker voor uw cluster en `clustername` door de naam van het cluster. Voer het wachtwoord voor het SSH-gebruikersaccount in wanneer hierom wordt gevraagd. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie over het gebruik van `scp` met HDInsight.

2. Gebruik de volgende opdrachten om de clusternaam op te slaan in een variabele en een hulpprogramma voor het parseren van JSON (`jq`) te installeren. Wanneer u daarom wordt gevraagd, geeft u de naam van het Kafka-cluster op:

    ```bash
    sudo apt -y install jq
    read -p 'Enter your Kafka cluster name:' CLUSTERNAME
    ```

3. Gebruik de volgende opdrachten als u de Kafka-brokerhosts en de Zookeeper-hosts wilt opvragen. Voer desgevraagd het wachtwoord voor het account voor clusteraanmelding (admin). U wordt tweemaal om uw wachtwoord gevraagd.

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
    ```

4. Gebruik de volgende opdrachten om de onderwerpen te maken die worden gebruikt door de streaming-bewerking:

    > [!NOTE]
    > Er kan een foutbericht worden weergegeven dat het onderwerp `test` al bestaat. Dit is geen probleem omdat het onderwerp mogelijk in de zelfstudie over de Producer- en Consumer-API's van Apache Kafka is gemaakt.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic RekeyedIntermediateTopic --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcount-example-Counts-changelog --zookeeper $KAFKAZKHOSTS
    ```

    De onderwerpen worden gebruikt voor de volgende doeleinden:

    * `test`: dit is het onderwerp waarin records worden ontvangen. De streaming-toepassing leest uit dit onderwerp.
    * `wordcounts`: dit is het onderwerp waarin de uitvoer van de streaming-toepassing wordt opgeslagen.
    * `RekeyedIntermediateTopic`: dit onderwerp wordt gebruikt voor het opnieuw partitioneren van gegevens wanneer het aantal woorden wordt bijgewerkt door de operator `countByKey`.
    * `wordcount-example-Counts-changelog`: in dit onderwerp worden statuswaarden opgeslagen die worden gebruikt door de bewerking `countByKey`.

    > [!IMPORTANT]
    > Kafka in HDInsight kan ook worden geconfigureerd voor het automatisch maken van onderwerpen. Zie [How to configure Apache Kafka on HDInsight to automatically create topics](apache-kafka-auto-create-topics.md) (Apache Kafka in HDInsight configureren voor het automatisch maken van onderwerpen) voor meer informatie.

## <a name="run-the-code"></a>De code uitvoeren

1. Gebruik de volgende opdracht om de streaming-toepassing te starten als een achtergrondproces:

    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS &
    ```

    > [!NOTE]
    > Er kan een waarschuwing over log4j worden weergegeven. Deze kunt u negeren.

2. Als u records wilt verzenden naar het onderwerp `test`, gebruikt u de volgende opdracht gebruiken om de Producer-toepassing te starten:

    ```bash
    java -jar kafka-producer-consumer.jar producer test $KAFKABROKERS
    ```

3. Zodra de toepassing is voltooid, gebruikt u de volgende opdracht om de informatie weer te geven die is opgeslagen in het onderwerp `wordcounts`:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --from-beginning
    ```

    > [!NOTE]
    > De parameters `--property` geven de Consumer-API opdracht om de key (het woord) samen met de count (waarde) weer te geven. Deze parameter configureert ook welke deserializer moet worden gebruikt bij het lezen van deze waarden uit Kafka.

    De uitvoer lijkt op het volgende:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
   
    > [!NOTE]
    > De parameter `--from-beginning` configureert de Consumer om te beginnen bij het begin van de records die zijn opgeslagen in het onderwerp. Het aantal wordt met elk ontvangen woord opgehoogd, zodat het onderwerp meerdere vermeldingen voor elk woord bevat, met een oplopend aantal.

7. Gebruik __Ctrl+C__ om de Producer af te sluiten. Blijf op __Ctrl+C__ drukken om de toepassing en de Consumer af te sluiten.

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u de Streams-API van Kafka gebruikt met Kafka in HDInsight. Gebruik de volgende documenten voor meer informatie over het werken met Kafka:

* [Kafka-logboekbestanden analyseren](apache-kafka-log-analytics-operations-management.md)
* [Gegevens repliceren tussen Kafka-clusters](apache-kafka-mirroring.md)
