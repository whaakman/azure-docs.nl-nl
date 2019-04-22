---
title: 'Zelfstudie: werken met de Streams-API van Apache Kafka - Azure HDInsight '
description: Leer hoe u de Streams-API van Apache Kafka gebruikt met Kafka in HDInsight. Met deze API kunt u gegevensstromen tussen onderwerpen in Kafka verwerken.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 04/02/2019
ms.openlocfilehash: 1e02eaeae4757a9a41ec59be81c3d9510d035232
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59273818"
---
# <a name="tutorial-apache-kafka-streams-api"></a>Zelfstudie: Streams-API van Apache Kafka

Leer hoe u een toepassing maakt die gebruikmaakt van de Streams-API van Apache Kafka en hoe u deze uitvoert met Kafka in HDInsight. 

De voorbeeldtoepassing die wordt gebruikt in deze zelfstudie, is een app voor het tellen van woorden die via een stream worden aangeboden. Eerst worden er tekstgegevens gelezen uit een onderwerp van Kafka, vervolgens worden afzonderlijke woorden uitgepakt en ten slotte worden de woorden en het aantal woorden opgeslagen in een ander Kafka-onderwerp.

> [!NOTE]  
> De verwerking van Kafka-gegevensstromen gebeurt vaak met Apache Spark of Apache Storm. Kafka versie 1.1.0 (in HDInsight 3.5 en 3.6) werd geïntroduceerd de Kafka Streams-API. Met deze API kunt u gegevensstromen transformeren tussen invoer- en uitvoeronderwerpen. In sommige gevallen kan dit een alternatief zijn voor het maken van een streamingoplossing op basis van Spark of Storm. 
>
> Meer informatie over de Streams-API van Kafka vindt u in het Engelstalige artikel [Intro to Streams](https://kafka.apache.org/10/documentation/streams/) op Apache.org.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De code begrijpen
> * De toepassing compileren en implementeren
> * Kafka onderwerpen configureren
> * De code uitvoeren

## <a name="prerequisites"></a>Vereisten

* Een Kafka in HDInsight 3.6-cluster. Zie [Aan de slag met Apache Kafka in HDInsight](apache-kafka-get-started.md) voor informatie over het maken van een Kafka-cluster in HDInsight.

* Voer de stappen in het document [Consumer- en Producer-API's van Apache Kafka](apache-kafka-producer-consumer-api.md) uit. In de stappen in dit document worden de voorbeeldtoepassing en onderwerpen gebruikt die in deze zelfstudie zijn gemaakt.

* [Java Developer Kit (JDK) versie 8](https://aka.ms/azure-jdks) of een equivalent, zoals OpenJDK.

* [Apache Maven](https://maven.apache.org/download.cgi) goed [geïnstalleerd](https://maven.apache.org/install.html) op basis van Apache.  Maven is een project maken voor Java-projecten.

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="understand-the-code"></a>De code begrijpen

De voorbeeldtoepassing bevindt zich op [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started), in de submap `Streaming`. De toepassing bestaat uit twee bestanden:

* `pom.xml`: met dit bestand worden de projectafhankelijkheden, de Java-versie en de pakketmethoden gedefinieerd.
* `Stream.java`: dit bestand implementeert de streaming-logica.

### <a name="pomxml"></a>Pom.xml

Belangrijke aandachtspunten voor het bestand `pom.xml`:

* Afhankelijkheden: dit project is afhankelijk van de Kafka Streams-API, die wordt geleverd door het pakket `kafka-clients`. Deze afhankelijkheid wordt gedefinieerd met de volgende XML-code:

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

* Invoegtoepassingen: de Maven-invoegtoepassingen bieden diverse mogelijkheden. In dit project worden de volgende plugins of invoegtoepassingen gebruikt:

    * `maven-compiler-plugin`: wordt gebruikt om de Java-versie die wordt gebruikt door het project in te stellen op 8. Java 8 is vereist voor HDInsight 3.6.
    * `maven-shade-plugin`: wordt gebruikt voor het genereren van een uber jar die deze toepassing bevat, evenals eventuele afhankelijkheden. Dit bestand wordt ook gebruikt om het toegangspunt van de toepassing in te stellen, zodat u het Jar-bestand rechtstreeks kunt uitvoeren, dus zonder de hoofdklasse op te geven.

### <a name="streamjava"></a>Stream.java

Het bestand [Stream.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Streaming/src/main/java/com/microsoft/example/Stream.java) gebruikt de Streams-API voor het implementeren van een toepassing voor het tellen van woorden. De toepassing leest gegevens uit een Kafka-onderwerp met de naam `test` en schrijft het gelezen aantal woorden naar een onderwerp met de naam `wordcounts`.

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

1. Instellen van uw huidige map naar de locatie van de `hdinsight-kafka-java-get-started-master\Streaming` directory, en gebruik vervolgens de volgende opdracht uit om een jar-pakket te maken:

    ```cmd
    mvn clean package
    ```

    Met deze opdracht maakt u het pakket op `target/kafka-streaming-1.0-SNAPSHOT.jar`.

2. Vervang `sshuser` door de SSH-gebruiker voor uw cluster en `clustername` door de naam van het cluster. Gebruik de volgende opdracht uit om te kopiëren de `kafka-streaming-1.0-SNAPSHOT.jar` bestand met uw HDInsight-cluster. Voer het wachtwoord voor het SSH-gebruikersaccount in wanneer hierom wordt gevraagd.

    ```cmd
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar sshuser@clustername-ssh.azurehdinsight.net:kafka-streaming.jar
    ```

## <a name="create-apache-kafka-topics"></a>Apache Kafka-onderwerpen maken

1. Vervang `sshuser` door de SSH-gebruiker voor uw cluster en `CLUSTERNAME` door de naam van het cluster. Open een SSH-verbinding met het cluster, met de volgende opdracht. Voer het wachtwoord voor het SSH-gebruikersaccount in wanneer hierom wordt gevraagd.

    ```bash
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Installeer [jq](https://stedolan.github.io/jq/), een opdrachtregelprogramma JSON-processor. Van de SSH-verbinding openen, voert u de volgende opdracht uit om te installeren `jq`:

    ```bash
    sudo apt -y install jq
    ```

3. Omgevingsvariabelen instellen. Vervang `PASSWORD` en `CLUSTERNAME` naam respectievelijk met het wachtwoord voor clusteraanmelding en cluster, voert u de opdracht:

    ```bash
    export password='PASSWORD'
    export clusterNameA='CLUSTERNAME'
    ```

4. De naam van cluster extract correct-indeling. De werkelijke behuizing van de naam van het cluster is mogelijk anders dan u verwacht, afhankelijk van hoe het cluster is gemaakt. Met deze opdracht wordt het werkelijke hoofdlettergebruik verkrijgen, opslaan in een variabele en vervolgens de naam van de juiste omhuld en de naam die u eerder hebt opgegeven, worden weergegeven. Voer de volgende opdracht in:

    ```bash
    export clusterName=$(curl -u admin:$password -sS -G "https://$clusterNameA.azurehdinsight.net/api/v1/clusters" \
  	| jq -r '.items[].Clusters.cluster_name')
    echo $clusterName, $clusterNameA
    ```

5. Gebruik de volgende opdrachten als u de Kafka-brokerhosts en de Apache Zookeeper-hosts wilt opvragen. Voer desgevraagd het wachtwoord voor het account voor clusteraanmelding (admin). U wordt tweemaal om uw wachtwoord gevraagd.

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin:$password -G \
    https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER \
  	| jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`;
    export KAFKABROKERS=`curl -sS -u admin:$password -G \
    https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER \
  	| jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`;
    ```

6. Gebruik de volgende opdrachten om de onderwerpen te maken die worden gebruikt door de streaming-bewerking:

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
   * `wordcount-example-Counts-changelog`: in dit onderwerp worden statuswaarden opgeslagen die worden gebruikt door de bewerking `countByKey`

     > [!IMPORTANT]  
     > Kafka in HDInsight kan ook worden geconfigureerd voor het automatisch maken van onderwerpen. Zie [How to configure Apache Kafka on HDInsight to automatically create topics](apache-kafka-auto-create-topics.md) (Apache Kafka in HDInsight configureren voor het automatisch maken van onderwerpen) voor meer informatie.

## <a name="run-the-code"></a>De code uitvoeren

1. Gebruik de volgende opdracht om de streaming-toepassing te starten als een achtergrondproces:

    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS &
    ```

    > [!NOTE]  
    > Er kan een waarschuwing over Apache log4j worden weergegeven. Deze kunt u negeren.

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

4. Gebruik __Ctrl+C__ om de Producer af te sluiten. Blijf op __Ctrl+C__ drukken om de toepassing en de Consumer af te sluiten.

5. Als u wilt verwijderen van de onderwerpen die door de streaming-bewerking wordt gebruikt, gebruik de volgende opdrachten:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic test --zookeeper $KAFKAZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic wordcounts --zookeeper $KAFKAZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic RekeyedIntermediateTopic --zookeeper $KAFKAZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic wordcount-example-Counts-changelog --zookeeper $KAFKAZKHOSTS
    ```

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u de Streams-API van Apache Kafka gebruikt met Kafka in HDInsight. Gebruik de volgende documenten voor meer informatie over het werken met Kafka:

* [Apache Kafka-logboeken analyseren](apache-kafka-log-analytics-operations-management.md)
* [Gegevens repliceren tussen Apache Kafka-clusters](apache-kafka-mirroring.md)
