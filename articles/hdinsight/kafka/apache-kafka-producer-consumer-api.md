---
title: "Zelfstudie: Werken met de Producer- en Consumer-API's van Apache Kafka - Azure HDInsight "
description: Leer hoe u de Producer- en Consumer-API's van Apache Kafka gebruikt met Kafka in HDInsight. In deze zelfstudie leert u hoe u deze API's vanuit een Java-toepassing gebruikt met Kafka in HDInsight.
author: dhgoelmsft
ms.author: dhgoel
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 06/24/2019
ms.openlocfilehash: 7a23d30e940417a6191cf14ad5d60159bd11c3da
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446402"
---
# <a name="tutorial-use-the-apache-kafka-producer-and-consumer-apis"></a>Zelfstudie: Werken met de Producer- en Consumer-API's van Apache Kafka

Leer hoe u de Producer- en Consumer-API's van Apache Kafka gebruikt met Kafka in HDInsight.

Met de Producer-API van Kafka kunnen toepassingen gegevensstromen naar het Kafka-cluster verzenden. Met de Consumer-API van Kafka kunnen toepassingen gegevensstromen uit het cluster lezen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Vereisten
> * De code begrijpen
> * De toepassing compileren en implementeren
> * De toepassing uitvoeren in het cluster

Meer informatie over de [Producer-API](https://kafka.apache.org/documentation/#producerapi) en de [Consumer-API](https://kafka.apache.org/documentation/#consumerapi) kunt u lezen in de Apache-documentatie (Engelstalig).

## <a name="prerequisites"></a>Vereisten

* Apache Kafka in HDInsight 3.6. Zie voor meer informatie over het maken van een Kafka op HDInsight-cluster, [beginnen met Apache Kafka in HDInsight](apache-kafka-get-started.md).

* [Java Developer Kit (JDK) versie 8](https://aka.ms/azure-jdks) of een equivalent, zoals OpenJDK.

* [Apache Maven](https://maven.apache.org/download.cgi) goed [geïnstalleerd](https://maven.apache.org/install.html) op basis van Apache.  Maven is een project maken voor Java-projecten.

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="understand-the-code"></a>De code begrijpen

De voorbeeldtoepassing bevindt zich op [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started), in de submap `Producer-Consumer`. De toepassing bestaat hoofdzakelijk uit vier bestanden:

* `pom.xml`: met dit bestand worden de projectafhankelijkheden, de Java-versie en de pakketmethoden gedefinieerd.
* `Producer.java`: met dit bestand worden willekeurige zinnen naar Kafka verzonden met behulp van de Producer-API.
* `Consumer.java`: dit bestand gebruikt de Consumer-API om gegevens te lezen uit Kafka en deze te verzenden naar STDOUT.
* `Run.java`: de opdrachtregelinterface die wordt gebruikt voor het uitvoeren van de Producer- en Consumer-code.

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

    De vermelding `${kafka.version}` wordt gedeclareerd in de sectie `<properties>..</properties>` van `pom.xml`, en wordt geconfigureerd voor de Kafka-versie van het HDInsight-cluster.

* Invoegtoepassingen: de Maven-invoegtoepassingen bieden diverse mogelijkheden. In dit project worden de volgende plugins of invoegtoepassingen gebruikt:

    * `maven-compiler-plugin`: wordt gebruikt om de Java-versie die wordt gebruikt door het project in te stellen op 8. Dit is de versie van Java die door HDInsight 3.6 wordt gebruikt.
    * `maven-shade-plugin`: wordt gebruikt voor het genereren van een uber jar die deze toepassing bevat, evenals eventuele afhankelijkheden. Dit bestand wordt ook gebruikt om het toegangspunt van de toepassing in te stellen, zodat u het Jar-bestand rechtstreeks kunt uitvoeren, dus zonder de hoofdklasse op te geven.

### <a name="producerjava"></a>Producer.java

De producer communiceert met de Kafka-brokerhosts (werkknooppunten) en verzendt gegevens naar een Kafka-onderwerp. Het volgende codefragment is afkomstig uit de [Producer.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Producer.java) -bestand uit de [GitHub-opslagplaats](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) en laat zien hoe de producent-eigenschappen in te stellen:

```java
Properties properties = new Properties();
// Set the brokers (bootstrap servers)
properties.setProperty("bootstrap.servers", brokers);
// Set how to serialize key/value pairs
properties.setProperty("key.serializer","org.apache.kafka.common.serialization.StringSerializer");
properties.setProperty("value.serializer","org.apache.kafka.common.serialization.StringSerializer");
KafkaProducer<String, String> producer = new KafkaProducer<>(properties);
```

### <a name="consumerjava"></a>Consumer.java

De Consumer-API communiceert met de Kafka-brokerhosts (werkknooppunten) en leest records in een lus. Met het volgende codefragment van het bestand [Consumer.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Consumer.java) worden de Consumer-eigenschappen ingesteld:

```java
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
```

In deze code is de Consumer-API geconfigureerd voor het lezen vanaf het begin van het onderwerp (`auto.offset.reset` is ingesteld op `earliest`.)

### <a name="runjava"></a>Run.java

De [Run.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Run.java) bestand biedt een opdrachtregelinterface die wordt uitgevoerd op de producent of de consument code. U moet de gegevens van de Kafka-brokerhost opgeven als een parameter. U kunt eventueel een groep-ID-waarde, die wordt gebruikt door het consumerproces opnemen. Als u meerdere consumentexemplaren met behulp van dezelfde groep-ID maakt, worden ze lezen van het onderwerp Verdeel de belasting.

## <a name="build-and-deploy-the-example"></a>Het voorbeeld compileren en implementeren

1. Downloaden en uitpakken van de voorbeelden van [ https://github.com/Azure-Samples/hdinsight-kafka-java-get-started ](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Instellen van uw huidige map naar de locatie van de `hdinsight-kafka-java-get-started\Producer-Consumer` directory en gebruik de volgende opdracht uit:

    ```cmd
    mvn clean package
    ```

    Met deze opdracht maakt u een directory met de naam `target`, die een bestand met de naam `kafka-producer-consumer-1.0-SNAPSHOT.jar` bevat.

3. Vervang `sshuser` door de SSH-gebruiker voor uw cluster en `CLUSTERNAME` door de naam van het cluster. Voer de volgende opdracht om te kopiëren de `kafka-producer-consumer-1.0-SNAPSHOT.jar` bestand met uw HDInsight-cluster. Voer het wachtwoord van de SSH-gebruiker in wanneer hierom wordt gevraagd.

    ```cmd
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```

## <a id="run"></a>Het voorbeeld uitvoeren

1. Vervang `sshuser` door de SSH-gebruiker voor uw cluster en `CLUSTERNAME` door de naam van het cluster. Open een SSH-verbinding met het cluster, met de volgende opdracht. Voer het wachtwoord voor het SSH-gebruikersaccount in wanneer hierom wordt gevraagd.

    ```cmd
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

5. Als u de Kafka-broker-hosts en de Apache Zookeeper-hosts, gebruikt u de volgende opdracht uit:

    ```bash
    export KAFKABROKERS=`curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER \
  	| jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    ```

6. Kafka-onderwerp maken `myTest`, met de volgende opdracht:

    ```bash
    java -jar kafka-producer-consumer.jar create myTest $KAFKABROKERS
    ```

7. Gebruik de volgende opdracht om de Producer-API uit te voeren en gegevens te schrijven naar het onderwerp:

    ```bash
    java -jar kafka-producer-consumer.jar producer myTest $KAFKABROKERS
    ```

8. Als dit proces is voltooid, gebruikt u de volgende opdracht om gegevens uit het onderwerp te lezen:

    ```bash
    java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS
    ```

    De gelezen records worden weergegeven, samen met een telling van de records.

9. Gebruik __Ctrl + C__ om de consument af te sluiten.

### <a name="multiple-consumers"></a>Meerdere consumenten

Kafka-consumenten gebruiken een consumentengroep bij het lezen van records. Door dezelfde groep voor meerdere consumenten te gebruiken, worden leestaken voor onderwerpen gelijk verdeeld. Elke consument in de groep ontvangt een deel van de records.

De Consumer-toepassing accepteert een parameter die wordt gebruikt als de groeps-id. Met de volgende opdracht start u bijvoorbeeld een Consumer met behulp van de groeps-id `myGroup`:

```bash
java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS myGroup
```

Gebruik __Ctrl + C__ om de consument af te sluiten.

Om dit proces in actie te zien, gebruikt u de volgende opdracht:

```bash
tmux new-session 'java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS myGroup' \
\; split-window -h 'java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS myGroup' \
\; attach
```

Deze opdracht gebruikt `tmux` om de terminal op te splitsen in twee kolommen. In elke kolom wordt een Consumer gestart, met dezelfde waarde voor de groeps-id. Als de Consumers klaar zijn met lezen, ziet u dat ieder Consumer slechts een deel van de records heeft gelezen. Gebruik __Ctrl + C__ twee keer om af te sluiten `tmux`.

Gebruik door clients binnen dezelfde groep wordt verwerkt door de partities voor het onderwerp. Het eerder gemaakte onderwerp `test` uit dit codevoorbeeld heeft acht partities. Als u acht Consumers start, leest elke Consumer records uit één partitie van het onderwerp.

> [!IMPORTANT]  
> Een consumentengroep kan niet meer consumentexemplaren dan partities bevatten. In dit voorbeeld kan één consumentengroep maximaal acht consumenten bevatten, omdat het onderwerp dit aantal partities heeft. U kunt ook meerdere consumentengroepen hebben, waarvan elke groep niet meer dan acht consumenten bevat.

Records worden in Kafka opgeslagen in de volgorde waarin deze worden ontvangen binnen een partitie. Als u records *binnen een partitie* op volgorde wilt leveren, maakt u een consumentengroep waarvan het aantal consumentexemplaren gelijk is aan het aantal partities. Als u records *binnen het onderwerp* op volgorde wilt leveren, maakt u een consumentengroep met slechts één consumentexemplaar.

## <a name="clean-up-resources"></a>Resources opschonen

Als u de in deze zelfstudie gemaakte resources wilt opschonen, kunt u de resourcegroep verwijderen. Als u de resourcegroep verwijdert, worden ook het bijbehorende HDInsight-cluster en eventuele andere resources die aan de resourcegroep zijn gekoppeld, verwijderd.

Ga als volgt te werk om de resourcegroep te verwijderen in Azure Portal:

1. Vouw het menu aan de linkerkant in Azure Portal uit om het menu met services te openen en kies __Resourcegroepen__ om de lijst met resourcegroepen weer te geven.
2. Zoek de resourcegroep die u wilt verwijderen en klik met de rechtermuisknop op de knop __Meer__ (... ) aan de rechterkant van de vermelding.
3. Selecteer __Resourcegroep verwijderen__ en bevestig dit.

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u de Producer- en Consumer-API's van Apache Kafka gebruikt met Kafka in HDInsight. Gebruik de volgende documenten voor meer informatie over het werken met Kafka:

> [!div class="nextstepaction"]
> [Apache Kafka-logboeken analyseren](apache-kafka-log-analytics-operations-management.md)