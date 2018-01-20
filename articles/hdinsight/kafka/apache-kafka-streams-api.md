---
title: De Apache Kafka Streams API - Azure HDInsight gebruiken | Microsoft Docs
description: Informatie over het gebruik van de API van Apache Kafka stromen met Kafka op HDInsight. Deze API kunt u stroomverwerkingslogica tussen de onderwerpen in Kafka uitvoeren.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: larryfr
ms.openlocfilehash: 1ea20eceb28fead003c7279632b1e75ae1fd3553
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="apache-kafka-streams-api"></a>Apache Kafka streams API

Informatie over het maken van een toepassing die gebruikmaakt van de stromen Kafka API en deze worden uitgevoerd met Kafka op HDInsight.

Als u werkt met Apache Kafka, stroom verwerken vaak is voltooid met Apache Spark of Storm. Kafka versie 0.10.0 (in HDInsight 3.5 en 3.6) is de API van de stromen Kafka geïntroduceerd. Deze API kunt u voor het transformeren van gegevensstromen tussen invoer- en onderwerpen, met behulp van een toepassing die wordt uitgevoerd op Kafka. In sommige gevallen kan dit een alternatief voor het maken van een Spark of Storm streamingoplossing. Zie voor meer informatie over Kafka Streams de [Inleiding tot Streams](https://kafka.apache.org/10/documentation/streams/) documentatie op Apache.org.

## <a name="set-up-your-development-environment"></a>De ontwikkelomgeving instellen

De volgende onderdelen moeten zijn geïnstalleerd in de ontwikkelingsomgeving:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) of een equivalent, zoals OpenJDK.

* [Apache Maven](http://maven.apache.org/)

* Een SSH-client en de opdracht `scp`. Zie het document [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

## <a name="set-up-your-deployment-environment"></a>Uw implementatieomgeving instellen

Dit voorbeeld vereist Kafka op HDInsight 3.6. Zie voor meer informatie over het maken van een Kafka op HDInsight-cluster, de [beginnen met Kafka op HDInsight](apache-kafka-get-started.md) document.

## <a name="build-and-deploy-the-example"></a>Bouw en implementeer het voorbeeld

Gebruik de volgende stappen uit om te bouwen en het project implementeren in uw Kafka op HDInsight-cluster.

1. Download de voorbeelden van [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Wijzig de mappen op de `Streaming` directory, en gebruik de volgende opdracht om een jar-pakket te maken:

    ```bash
    mvn clean package
    ```

    Met deze opdracht maakt u een directory met de naam `target`, die een bestand met de naam `kafka-streaming-1.0-SNAPSHOT.jar` bevat.

3. Gebruik de volgende opdracht om te kopiëren de `kafka-streaming-1.0-SNAPSHOT.jar` bestand met uw HDInsight-cluster:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Vervang **SSHUSER** door de SSH-gebruiker van uw cluster en vervang **CLUSTERNAME** door de naam van het cluster. Als u wordt gevraagd, voer het wachtwoord voor de SSH-gebruikersaccount. Voor meer informatie over het gebruik van `scp` met HDInsight, raadpleegt u [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run-the-example"></a>Het voorbeeld uitvoert

1. U kunt een SSH-verbinding met het cluster openen met de volgende opdracht:

    ```bash
    ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Vervang **SSHUSER** door de SSH-gebruiker van uw cluster en vervang **CLUSTERNAME** door de naam van het cluster. Als u wordt gevraagd, voer het wachtwoord voor de SSH-gebruikersaccount. Voor meer informatie over het gebruik van `scp` met HDInsight, raadpleegt u [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

4. Voor het maken van de Kafka onderwerpen die door dit voorbeeld worden gebruikt, gebruik de volgende opdrachten:

    ```bash
    sudo apt -y install jq

    CLUSTERNAME='your cluster name'

    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

    Vervang __uw clusternaam__ met de naam van uw HDInsight-cluster. Wanneer u wordt gevraagd, voert u het wachtwoord voor de aanmeldingsaccount van het HDInsight-cluster.

    > [!NOTE]
    > Als uw cluster-aanmelding anders dan de standaardwaarde van is `admin`, vervang de `admin` waarde in de vorige opdrachten met de naam van uw cluster-aanmelding.

5. U voert dit voorbeeld, moet u drie dingen doen:

    * Starten van de oplossing Streams in `kafka-streaming.jar`.
    * Start een producent die naar schrijft de `test` onderwerp.
    * Een consumer te starten, zodat u de uitvoer geschreven ziet naar de `wordcounts` onderwerp

    U kunt deze bewerkingen kan uitvoeren door het openen van drie SSH-sessies. Maar u moet vervolgens ingesteld `$KAFKABROKERS` en `$KAFKAZKHOSTS` stap voor elk door het uitvoeren van 4 in deze sectie bij elke SSH-sessie. Een eenvoudiger oplossing is met de `tmux` hulpprogramma kan de huidige weergave SSH in meerdere secties gesplitst. Starten van de stroom, producent en consumenten met `tmux`, gebruik de volgende opdracht:

    ```bash
    tmux new-session '/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer' \; split-window -h 'java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS' \; split-window -v '/usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test' \; attach
    ```

    Met deze opdracht splitst de SSH-weergave in drie secties:

    * Het linkergedeelte wordt uitgevoerd een console consumer, welke leesbewerkingen van berichten van de `wordcounts` onderwerp:`/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer`

        > [!NOTE]
        > De `--property` parameters laat de consument console afdrukken van de sleutel (word) samen met het aantal (waarde). Deze parameter configureert ook de deserializer moet worden gebruikt bij het lezen van deze waarden van Kafka.

    * Het bovenste gedeelte van de juiste voert de Streams API-oplossing:`java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS`

    * Het onderste gedeelte van de juiste de producent console wordt uitgevoerd en wordt gewacht op berichten te verzenden geeft u de `test` onderwerp:`/usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test`
 
6. Na de `tmux` opdracht splitst de weergave, wordt de cursor in het onderste gedeelte van de juiste. Beginnen met het invoeren van zinnen. Het linkerdeelvenster wordt bijgewerkt na elke zin om weer te geven van een aantal unieke woorden. De uitvoer lijkt op het volgende:
   
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
    > De telling wordt verhoogd wanneer er een woord wordt gedetecteerd.

7. Gebruik de __Ctrl + c drukken__ om af te sluiten van de producent. Blijven gebruiken __Ctrl + c drukken__ om af te sluiten van de toepassing en de consumer.

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u de API Kafka-stromen met Kafka op HDInsight. Gebruik de volgende documenten voor meer informatie over het werken met Kafka:

* [Kafka-logboekbestanden analyseren](apache-kafka-log-analytics-operations-management.md)
* [Gegevens repliceren tussen Kafka-clusters](apache-kafka-mirroring.md)
* [Kafka producenten en consumenten API met HDInsight](apache-kafka-producer-consumer-api.md)
* [Apache Spark-streaming (DStream) gebruiken met Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Apache Spark Structured Streaming met Kafka in HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md)
* [Apache Spark gestructureerde Streaming gebruiken om gegevens te verplaatsen van Kafka op HDInsight aan de Cosmos-database](../apache-kafka-spark-structured-streaming-cosmosdb.md)
* [Apache Storm gebruiken met Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Verbinding maken met Kafka via een Azure Virtual Network](apache-kafka-connect-vpn-gateway.md)