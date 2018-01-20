---
title: De Apache Kafka producenten en consumenten API's gebruiken - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van de Apache Kafka producenten en consumenten API's met Kafka op HDInsight. Deze API's kunnen u toepassingen ontwikkelen die schrijven naar en van Apache Kafka lezen.
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
ms.date: 01/18/2018
ms.author: larryfr
ms.openlocfilehash: b57745d6bd993a993e923c964327d9071e745413
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="apache-kafka-producer-and-consumer-apis"></a>Apache Kafka producenten en consumenten API 's

Informatie over het maken van een toepassing die gebruikmaakt van de Consumer-API's en Kafka producent met Kafka op HDInsight.

Zie voor documentatie over de API's, [producent API](https://kafka.apache.org/documentation/#producerapi) en [Consumer API](https://kafka.apache.org/documentation/#consumerapi).

## <a name="set-up-your-development-environment"></a>De ontwikkelomgeving instellen

De volgende onderdelen moeten zijn geïnstalleerd in de ontwikkelingsomgeving:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) of een equivalent, zoals OpenJDK.

* [Apache Maven](http://maven.apache.org/)

* Een SSH-client en de opdracht `scp`. Zie het document [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

## <a name="set-up-your-deployment-environment"></a>Uw implementatieomgeving instellen

Dit voorbeeld vereist Kafka op HDInsight 3.6. Zie voor meer informatie over het maken van een Kafka op HDInsight-cluster, de [beginnen met Kafka op HDInsight](apache-kafka-get-started.md) document.

## <a name="build-and-deploy-the-example"></a>Bouw en implementeer het voorbeeld

1. Download de voorbeelden van [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Wijzig de mappen naar de locatie van de `Producer-Consumer` directory en gebruik de volgende opdracht:

    ```
    mvn clean package
    ```

    Met deze opdracht maakt u een directory met de naam `target`, die een bestand met de naam `kafka-producer-consumer-1.0-SNAPSHOT.jar` bevat.

3. Gebruik de volgende opdrachten om het bestand `kafka-producer-consumer-1.0-SNAPSHOT.jar` te kopiëren naar uw HDInsight-cluster:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    Vervang **SSHUSER** door de SSH-gebruiker van uw cluster en vervang **CLUSTERNAME** door de naam van het cluster. Voer het wachtwoord van de SSH-gebruiker in wanneer hierom wordt gevraagd.

## <a id="run"></a>Het voorbeeld uitvoert

1. U kunt een SSH-verbinding met het cluster openen met de volgende opdracht:

    ```bash
    ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Vervang **SSHUSER** door de SSH-gebruiker van uw cluster en vervang **CLUSTERNAME** door de naam van het cluster. Als u wordt gevraagd, voer het wachtwoord voor de SSH-gebruikersaccount. Voor meer informatie over het gebruik van `scp` met HDInsight, raadpleegt u [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Voor het maken van de Kafka onderwerpen die door dit voorbeeld worden gebruikt, gebruik de volgende opdrachten:

    ```bash
    sudo apt -y install jq

    CLUSTERNAME='your cluster name'

    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
    ```

    Vervang __uw clusternaam__ met de naam van uw HDInsight-cluster. Wanneer u wordt gevraagd, voert u het wachtwoord voor de aanmeldingsaccount van het HDInsight-cluster.

    > [!NOTE]
    > Als uw cluster-aanmelding anders dan de standaardwaarde van is `admin`, vervang de `admin` waarde in de vorige opdrachten met de naam van uw cluster-aanmelding.

3. De producent uitvoeren en gegevens schrijven naar het onderwerp, kunt u de volgende opdracht gebruiken:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

4. Zodra de producent heeft, gebruikt u de volgende opdracht om te lezen van het onderwerp:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    De gelezen records worden weergegeven, samen met een telling van de records.

5. Gebruik __Ctrl + C__ om de consument af te sluiten.

### <a name="multiple-consumers"></a>Meerdere consumenten

Kafka-consumenten gebruiken een consumentengroep bij het lezen van records. Door dezelfde groep voor meerdere consumenten te gebruiken, worden leestaken voor onderwerpen gelijk verdeeld. Elke consument in de groep ontvangt een deel van de records.

De consumer-toepassing accepteert een parameter die wordt gebruikt als de groep-ID. Voor bijvoorbeeld de volgende opdracht vanaf een consumer met behulp van een groep-ID van `mygroup`:
   
```bash
java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
```

Als dit proces in actie weergeven, gebruikt u de volgende stappen

1. Herhaal stap 1 en 2 in de [het voorbeeld uitvoert](#run) sectie een nieuwe SSH-sessie te openen.

2. In beide SSH-sessies, voer de volgende opdracht:

    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```
    
    > [!IMPORTANT]
    > Voer beide opdrachten tegelijk, zodat ze worden uitgevoerd op hetzelfde moment.

    U ziet dat het aantal berichten is niet hetzelfde zijn als de vorige sectie, u slechts één consumer moest lezen. In plaats daarvan wordt het lezen van berichten gesplitst tussen de exemplaren.

Gebruik door clients binnen dezelfde groep wordt verwerkt door de partities voor het onderwerp. Het eerder gemaakte onderwerp `test` heeft acht partities. Als u acht SSH-sessies opent en in alle sessies een consument start, leest de consument de records van één partitie voor het onderwerp.

> [!IMPORTANT]
> Een consumentengroep kan niet meer consumentexemplaren dan partities bevatten. In dit voorbeeld kan één consumentengroep maximaal acht consumenten bevatten, omdat het onderwerp dit aantal partities heeft. U kunt ook meerdere consumentengroepen hebben, waarvan elke groep niet meer dan acht consumenten bevat.

Records worden in Kafka opgeslagen in de volgorde waarin deze worden ontvangen binnen een partitie. Als u records *binnen een partitie* op volgorde wilt leveren, maakt u een consumentengroep waarvan het aantal consumentexemplaren gelijk is aan het aantal partities. Als u records *binnen het onderwerp* op volgorde wilt leveren, maakt u een consumentengroep met slechts één consumentexemplaar.

## <a name="next-steps"></a>Volgende stappen

In dit document, u hebt geleerd hoe u de Kafka producent en consumenten API met Kafka op HDInsight. Gebruik de volgende documenten voor meer informatie over het werken met Kafka:

* [Kafka-logboekbestanden analyseren](apache-kafka-log-analytics-operations-management.md)
* [Gegevens repliceren tussen Kafka-clusters](apache-kafka-mirroring.md)
* [Kafka Streams API met HDInsight](apache-kafka-streams-api.md)
* [Apache Spark-streaming (DStream) gebruiken met Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Apache Spark Structured Streaming met Kafka in HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md)
* [Apache Spark gestructureerde Streaming gebruiken om gegevens te verplaatsen van Kafka op HDInsight aan de Cosmos-database](../apache-kafka-spark-structured-streaming-cosmosdb.md)
* [Apache Storm gebruiken met Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Verbinding maken met Kafka via een Azure Virtual Network](apache-kafka-connect-vpn-gateway.md)