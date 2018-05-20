---
title: Apache Kafka gebruiken in HDInsight met Azure IoT Hub | Microsoft Docs
description: Informatie over het gebruik van Apache Kafka in HDInsight met Azure IoT Hub. Het project Kafka verbinding maken met Azure IoT Hub biedt een bron- en sink-connector voor Kafka. De bron-connector gegevens uit IoT Hub kan lezen, en de connector sink schrijft naar IoT Hub.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/15/2018
ms.author: larryfr
ms.openlocfilehash: 33fdb5b099efc40fec94a860b21cda75ced44fe9
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="use-apache-kafka-on-hdinsight-with-azure-iot-hub"></a>Apache Kafka op HDInsight met Azure IoT Hub gebruiken

Meer informatie over het gebruik van de [Kafka verbinding maken met Azure IoT Hub](https://github.com/Azure/toketi-kafka-connect-iothub) connector om gegevens te verplaatsen tussen Apache Kafka in HDInsight en Azure IoT Hub. In dit document leert u hoe de IoT Hub-connector uitvoeren vanaf een edge-knooppunt in het cluster.

De API Kafka verbinding kunt u voor het implementeren van connectors die continu ophalen van gegevens in Kafka of pushen van gegevens vanaf Kafka naar een ander systeem. De [Kafka verbinding maken met Azure IoT Hub](https://github.com/Azure/toketi-kafka-connect-iothub) is een connector die gegevens uit Azure IoT Hub in Kafka ophaalt. Het kan ook gegevens uit Kafka pushen naar de IoT-Hub. 

Wanneer binnenhalen van de IoT-Hub, gebruikt u een __bron__ connector. Wanneer pushen naar IoT Hub, gebruikt u een __sink__ connector. De connector IoT Hub biedt de bron- en sink-connectors.

Het volgende diagram toont de gegevensoverdracht tussen Azure IoT Hub en Kafka op HDInsight bij gebruik van de connector.

![Afbeelding van de gegevens die binnenkomen uit IoT Hub voor Kafka via de connector](./media/apache-kafka-connector-iot-hub/iot-hub-kafka-connector-hdinsight.png)

Zie voor meer informatie over de API verbinding [ https://kafka.apache.org/documentation/#connect ](https://kafka.apache.org/documentation/#connect).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

* Een Kafka op HDInsight-cluster. Zie de [snelstart voor Kafka in HDInsight](apache-kafka-get-started.md) voor meer informatie.

* Een edge-knooppunt in het cluster Kafka. Zie voor meer informatie de [edge-knooppunten gebruiken met HDInsight](../hdinsight-apps-use-edge-node.md) document.

* Een Azure-IoT-Hub. Voor deze zelfstudie ik het beste de [online simulator frambozen Pi verbinding maken met Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started) document.

* Een SSH-client. In de stappen in dit document wordt SSH gebruikt om verbinding te maken met het cluster. Zie het document [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

## <a name="install-the-connector"></a>De connector installeren

1. Download de nieuwste versie van de Kafka Connect voor Azure IoT Hub van [ https://github.com/Azure/toketi-kafka-connect-iothub/releases/ ](https://github.com/Azure/toketi-kafka-connect-iothub/releases).

2. Als u wilt de JAR-bestand uploaden naar het edge-knooppunt van uw Kafka op HDInsight-cluster, moet u de volgende opdracht gebruiken:

    > [!NOTE]
    > Vervang `sshuser` met het SSH-gebruikersaccount voor uw HDInsight-cluster. Vervang `new-edgenode` met de naam van het edge-knooppunt. Vervang `clustername` met de naam van het cluster. Zie voor meer informatie over het SSH-eindpunt voor het edge-knooppunt de [edge-knooppunten gebruikt met HDInsight](../hdinsight-apps-use-edge-node.md#access-an-edge-node) document.

    ```bash
    scp kafka-connect-iothub-assembly*.jar sshuser@new-edgenode.clustername-ssh.azurehdinsight.net:
    ```

3. Nadat het kopiëren van bestanden is voltooid, moet u een verbinding maken met het edge-knooppunt via SSH:

    ```bash
    ssh sshuser@new-edgenode.clustername-ssh.azurehdinsight.net
    ```

    Zie het document [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

4. Installatie van de connector in de Kafka `libs` directory, gebruik de volgende opdracht:

    ```bash
    sudo mv kafka-connect-iothub-assembly*.jar /usr/hdp/current/kafka-broker/libs/
    ```

> [!TIP]
> Als u problemen met de rest van de stappen in dit document uitvoert wanneer u een vooraf samengestelde JAR-bestand is, probeert u het opbouwen van het pakket van bron.
>
> De connector bouwen, hebt u een ontwikkelingsomgeving Scala aan de [Scala bouwen hulpprogramma](http://www.scala-sbt.org/).
>
> 1. De bron voor de connector uit downloaden [ https://github.com/Azure/toketi-kafka-connect-iothub/ ](https://github.com/Azure/toketi-kafka-connect-iothub/) op uw ontwikkelomgeving.
>
> 2. Vanaf een opdrachtprompt in de projectmap, moet u de volgende opdracht gebruiken om te bouwen en het project van het pakket:
>
>    ```bash
>    sbt assembly
>    ```
>
>    Deze opdracht maakt u een bestand met de naam `kafka-connect-iothub-assembly_2.11-0.6.jar` in de `target/scala-2.11` map voor het project.

## <a name="configure-kafka"></a>Kafka configureren

Gebruik de volgende stappen voor het configureren van Kafka voor het uitvoeren van de connector in zelfstandige modus van een SSH-verbinding met het edge-knooppunt:

1. Sla de clusternaam op een variabele. Met behulp van een variabele gemakkelijker kunt kopiëren en plakken van de andere opdrachten in deze sectie:

    ```bash
    read -p "Enter the Kafka on HDInsight cluster name: " CLUSTERNAME
    ```

2. Installeer de `jq` hulpprogramma. Dit hulpprogramma maakt bij het verwerken van JSON-documenten die zijn geretourneerd door Ambari query's:

    ```bash
    sudo apt -y install jq
    ```

3. Het adres van de Kafka beleggingsmakelaars ophalen. Mogelijk zijn er veel beleggingsmakelaars in uw cluster, maar u moet alleen verwijzen naar een of twee. Als u het adres van de twee broker hosts, gebruikt u de volgende opdracht:

    ```bash
    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $KAFKABROKERS
    ```

    Voer desgevraagd het wachtwoord voor het account voor clusteraanmelding (admin). De geretourneerde waarde ziet er ongeveer zo uit:

    `wn0-kafka.w5ijyohcxt5uvdhhuaz5ra4u5f.ex.internal.cloudapp.net:9092,wn1-kafka.w5ijyohcxt5uvdhhuaz5ra4u5f.ex.internal.cloudapp.net:9092`

4. Het adres van de Zookeeper-knooppunten ophalen. Er zijn verschillende Zookeeper-knooppunten in het cluster, maar hoeft u alleen te verwijzen naar een of twee. Als u het adres van de twee Zookeeper-knooppunten, gebruikt u de volgende opdracht:

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

5. Wanneer de connector wordt uitgevoerd in de zelfstandige modus, de `/usr/hdp/current/kafka-broker/config/connect-standalone.properties` bestand wordt gebruikt om te communiceren met de beleggingsmakelaars Kafka. Bewerken de `connect-standalone.properties` bestand, gebruikt u de volgende opdracht:

    ```bash
    sudo nano /usr/hdp/current/kafka-broker/config/connect-standalone.properties
    ```

    * Voor het configureren van de zelfstandige configuratie voor het vinden van de Kafka beleggingsmakelaars edge-knooppunt, zoek de regel die met begint `bootstrap.servers=`. Vervang de `localhost:9092` waarde met de broker-hosts uit de vorige stap.

    * Wijzig de `key.converter=` en `value.converter=` regels op de volgende waarden:

        ```text
        key.converter=org.apache.kafka.connect.storage.StringConverter
        value.converter=org.apache.kafka.connect.storage.StringConverter
        ```

        > [!IMPORTANT]
        > Deze wijziging kunt u testen met behulp van de console producent met Kafka opgenomen. U moet mogelijk verschillende converters voor andere producenten en consumenten. Zie voor meer informatie over het gebruik van andere waarden converter [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

    * Een regel toegevoegd aan het einde van het bestand met de volgende tekst:

        ```text
        consumer.max.poll.records=10
        ```

        > [!TIP]
        > Deze wijziging is om te voorkomen dat time-outs in de sink-connector door het beperken tot 10 records tegelijk. Zie voor meer informatie [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

6. Gebruiken om het bestand opslaat, __Ctrl + X__, __Y__, en vervolgens __Enter__.

7. Gebruik de volgende opdrachten voor het maken van de onderwerpen die wordt gebruikt door de connector:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic iotin --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic iotout --zookeeper $KAFKAZKHOSTS
    ```

    Om te controleren of de `iotin` en `iotout` onderwerpen bestaat, gebruik de volgende opdracht:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
    ```

    De `iotin` onderwerp wordt gebruikt voor het ontvangen van berichten uit IoT Hub. De `iotout` onderwerp wordt gebruikt om berichten te verzenden naar IoT Hub.

## <a name="get-iot-hub-connection-information"></a>Gegevens van IoT Hub-verbinding ophalen

Gebruik de volgende stappen voor het ophalen van IoT Hubgegevens die door de connector gebruikt:

1. Lees het Event Hub-compatibele eindpunt en de naam van de Event Hub-compatibele eindpunt voor uw iothub. Als u deze informatie, moet u een van de volgende methoden gebruiken:

    * __Van de [Azure-portal](https://portal.azure.com/)__, gebruik de volgende stappen:

        1. Navigeer naar uw IoT-Hub en selecteer __eindpunten__.
        2. Van __ingebouwde eindpunten__, selecteer __gebeurtenissen__.
        3. Van __eigenschappen__, Kopieer de waarde van de volgende velden:

            * __Event Hub-compatibele naam__
            * __Event Hub-compatibele eindpunt__
            * __Partities__

        > [!IMPORTANT]
        > De waarde van de endpoint van de portal bevat mogelijk extra tekst die niet in dit voorbeeld vereist. De tekst die overeenkomt met dit patroon ophalen `sb://<randomnamespace>.servicebus.windows.net/`.

    * __Van de [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)__, gebruik de volgende opdracht:

        ```azure-cli
        az iot hub show --name myhubname --query "{EventHubCompatibleName:properties.eventHubEndpoints.events.path,EventHubCompatibleEndpoint:properties.eventHubEndpoints.events.endpoint,Partitions:properties.eventHubEndpoints.events.partitionCount}"
        ```

        Vervang `myhubname` met de naam van uw IoT-hub. Het antwoord is vergelijkbaar met de volgende tekst:

        ```text
        "EventHubCompatibleEndpoint": "sb://ihsuprodbnres006dednamespace.servicebus.windows.net/",
        "EventHubCompatibleName": "iothub-ehub-myhub08-207673-d44b2a856e",
        "Partitions": 2
        ```

2. Ophalen van de __gedeeld toegangsbeleid__ en __sleutel__. In dit voorbeeld gebruiken de __service__ sleutel. Als u deze informatie, moet u een van de volgende methoden gebruiken:

    * __Van de [Azure-portal](https://portal.azure.com/)__, gebruik de volgende stappen:

        1. Selecteer __gedeeld toegangsbeleid__, en selecteer vervolgens __service__.
        2. Kopieer de __primaire sleutel__ waarde.
        3. Kopieer de __verbindingsreeks--primaire sleutel__ waarde.

    * __Van de [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)__, gebruik de volgende opdracht:

        1. Als u de waarde van de primaire sleutel, gebruikt u de volgende opdracht:

            ```azure-cli
            az iot hub policy show --hub-name myhubname --name service --query "primaryKey"
            ```

            Vervang `myhubname` met de naam van uw IoT-hub. Het antwoord is de primaire sleutel voor de `service` beleid voor deze hub.

        2. Ophalen van de verbindingsreeks voor de `service` -beleid, gebruikt u de volgende opdracht:

            ```azure-cli
            az iot hub show-connection-string --name myhubname --policy-name service --query "connectionString"
            ```

            Vervang `myhubname` met de naam van uw IoT-hub. Het antwoord is de verbindingsreeks voor de `service` beleid.

## <a name="configure-the-source-connection"></a>Verbinding met de gegevensbron configureren

Voor het configureren van de bron voor gebruik met uw IoT-Hub, moet u de volgende acties uitvoeren van een SSH-verbinding met het edge-knooppunt:

1. Maak een kopie van de `connect-iot-source.properties` bestand de `/usr/hdp/current/kafka-broker/config/` directory. Het bestand te downloaden uit het project toketi-kafka-connect-iothub, kunt u de volgende opdracht gebruiken:

    ```bash
    sudo wget -P /usr/hdp/current/kafka-broker/config/ https://raw.githubusercontent.com/Azure/toketi-kafka-connect-iothub/master/connect-iothub-source.properties
    ```

2. Bewerken de `connect-iot-source.properties` bestand en de gegevens van de IoT-hub toevoegen, gebruikt u de volgende opdracht:

    ```bash
    sudo nano /usr/hdp/current/kafka-broker/config/connect-iothub-source.properties
    ```

    Zoeken in de editor en wijzigen van de volgende items:

    * `Kafka.Topic=PLACEHOLDER`: Vervang `PLACEHOLDER` met `iotin`. Berichten ontvangen uit iothub worden geplaatst de `iotin` onderwerp.
    * `IotHub.EventHubCompatibleName=PLACEHOLDER`: Vervang `PLACEHOLDER` met de naam van de Event Hub-compatibele.
    * `IotHub.EventHubCompatibleEndpoint=PLACEHOLDER`: Vervang `PLACEHOLDER` met het Event Hub-compatibele eindpunt.
    * `IotHub.Partitions=PLACEHOLDER`: Vervang `PLACEHOLDER` met het aantal partities uit de vorige stap.
    * `IotHub.AccessKeyName=PLACEHOLDER`: Vervang `PLACEHOLDER` met `service`.
    * `IotHub.AccessKeyValue=PLACEHOLDER`: Vervang `PLACEHOLDER` met de primaire sleutel van de `service` beleid.
    * `IotHub.StartType=PLACEHOLDER`: Vervang `PLACEHOLDER` met een UTC-datum. Deze datum is wanneer de connector wordt gestart met het controleren of er berichten. De datumnotatie is `yyyy-mm-ddThh:mm:ssZ`.
    * `BatchSize=100`: Vervang `100` met `5`. Deze wijziging zorgt ervoor dat de connector om berichten te lezen in Kafka zodra er vijf nieuwe berichten in IoT-hub zijn.

    Zie voor een voorbeeldconfiguratie [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md).

3. Sla wijzigingen gebruiken __Ctrl + X__, __Y__, en vervolgens __Enter__.

Zie voor meer informatie over het configureren van de bron van de connector [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md).

## <a name="configure-the-sink-connection"></a>De sink-verbinding configureren

Voor het configureren van de sink-verbindingspunt werken met uw IoT-Hub, moet u de volgende acties uitvoeren van een SSH-verbinding met het edge-knooppunt:

1. Maak een kopie van de `connect-iothub-sink.properties` bestand de `/usr/hdp/current/kafka-broker/config/` directory. Het bestand te downloaden uit het project toketi-kafka-connect-iothub, kunt u de volgende opdracht gebruiken:

    ```bash
    sudo wget -P /usr/hdp/current/kafka-broker/config/ https://raw.githubusercontent.com/Azure/toketi-kafka-connect-iothub/master/connect-iothub-sink.properties
    ```

2. Bewerken de `connect-iothub-sink.properties` bestand en de gegevens van de IoT-hub toevoegen, gebruikt u de volgende opdracht:

    ```bash
    sudo nano /usr/hdp/current/kafka-broker/config/connect-iothub-sink.properties
    ```

    Zoeken in de editor en wijzigen van de volgende items:

    * `topics=PLACEHOLDER`: Vervang `PLACEHOLDER` met `iotout`. Berichten die naar geschreven `iotout` onderwerp worden doorgestuurd naar de IoT-hub.
    * `IotHub.ConnectionString=PLACEHOLDER`: Vervang `PLACEHOLDER` met de verbindingsreeks voor de `service` beleid.

    Zie voor een voorbeeldconfiguratie [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

3. Sla wijzigingen gebruiken __Ctrl + X__, __Y__, en vervolgens __Enter__.

Zie voor meer informatie over het configureren van de connector sink [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

## <a name="start-the-source-connector"></a>Starten van de bron-connector

Gebruik de volgende opdracht vanaf een SSH-verbinding met het edge-knooppunt voor het starten van de bron-connector:

```bash
/usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone.properties /usr/hdp/current/kafka-broker/config/connect-iothub-source.properties
```

Zodra de connector start, berichten verzenden met iothub van uw apparaten. Als de connector berichten uit iothub kan lezen en ze op in het onderwerp Kafka slaat, legt informatie vast voor de console:

```text
[2017-08-29 20:15:46,112] INFO Polling for data - Obtained 5 SourceRecords from IotHub (com.microsoft.azure.iot.kafka.co
nnect.IotHubSourceTask:39)
[2017-08-29 20:15:54,106] INFO Finished WorkerSourceTask{id=AzureIotHubConnector-0} commitOffsets successfully in 4 ms (
org.apache.kafka.connect.runtime.WorkerSourceTask:356)
```

> [!NOTE]
> Mogelijk ziet u enkele waarschuwingen als de connector wordt gestart. Deze waarschuwingen veroorzaken geen problemen met het ontvangen van berichten uit iothub.

Voor het beëindigen van de connector gebruikt __Ctrl + c drukken__.

## <a name="start-the-sink-connector"></a>Start de sink-connector

Gebruik de volgende opdracht om te starten van de sink-connector in de zelfstandige modus van een SSH-verbinding met het edge-knooppunt:

```bash
/usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone.properties /usr/hdp/current/kafka-broker/config/connect-iothub-sink.properties
```

Als de connector wordt uitgevoerd, wordt informatie de volgende strekking weergegeven:

```text
[2017-08-30 17:49:16,150] INFO Started tasks to send 1 messages to devices. (com.microsoft.azure.iot.kafka.connect.sink.
IotHubSinkTask:47)
[2017-08-30 17:49:16,150] INFO WorkerSinkTask{id=AzureIotHubSinkConnector-0} Committing offsets (org.apache.kafka.connec
t.runtime.WorkerSinkTask:262)
```

> [!NOTE]
> Merkt u wellicht enkele waarschuwingen als de connector wordt gestart. U kunt deze gewoon negeren.

Gebruik de volgende stappen voor het verzenden van berichten via de connector:

1. Open een andere SSH-sessie op het cluster Kafka:

    ```bash
    ssh sshuser@new-edgenode.clustername-ssh.azurehdinsight.net
    ```
2. Voor het verzenden van berichten naar de `iotout` onderwerp, gebruik de volgende opdracht:

    > [!WARNING]
    > Aangezien dit een nieuwe SSH-verbinding de `$KAFKABROKERS` variabele bevatten geen informatie. Als u wilt instellen, moet u een van de volgende methoden gebruiken:
    >
    > * Gebruik de eerste drie stappen in de [Kafka configureren](#configure-kafka) sectie.
    > * Gebruik `echo $KAFKABROKERS` uit de vorige SSH-verbinding met de waarden ophaalt en vervang `$KAFKABROKERS` in de volgende opdracht met de werkelijke waarden.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic iotout
    ```

    Met deze opdracht geeft geen resultaat op de normale Bash-vraag. In plaats daarvan het toetsenbordinvoer naar verzendt de `iotout` onderwerp.

3. Als u wilt een bericht verzendt naar uw apparaat, plakt u een JSON-document in de SSH-sessie voor de `kafka-console-producer`.

    > [!IMPORTANT]
    > U moet de waarde van de `"deviceId"` vermelding naar de ID van uw apparaat. In het volgende voorbeeld wordt het apparaat met de naam `fakepi`:

    ```text
    {"messageId":"msg1","message":"Turn On","deviceId":"fakepi"}
    ```

    Het schema voor deze JSON-document wordt beschreven in meer detail op [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

    Als u van de gesimuleerde frambozen Pi-apparaat gebruikmaakt en deze wordt uitgevoerd, wordt het volgende bericht door het apparaat geregistreerd:

    ```text
    Receive message: Turn On
    ```

    Het JSON-document verzenden, maar wijzig de waarde van de `"message"` vermelding. De nieuwe waarde wordt vastgelegd door het apparaat.

Zie voor meer informatie over het gebruik van de connector sink [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u met de API Kafka verbinding starten van de IoT-Kafka-Connector op HDInsight. Gebruik de volgende koppelingen voor het detecteren van andere manieren om te werken met Kafka:

* [Apache Spark gebruiken met Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Apache Storm gebruiken met Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)
