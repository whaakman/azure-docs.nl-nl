---
title: Met Apache Kafka beginnen - HDInsight | Microsoft Docs
description: Informatie over het maken van een Apache Kafka-cluster in Azure HDInsight. Informatie over het maken van onderwerpen, abonnees en consumenten.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/18/2018
ms.author: larryfr
ms.openlocfilehash: 6191d81d6b55f5ffe943f800be542d7ea4614eaf
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="start-with-apache-kafka-on-hdinsight"></a>Met Apache Kafka in HDInsight beginnen

Informatie over het maken en gebruiken van een [Apache Kafka](https://kafka.apache.org)-cluster in Azure HDInsight. Kafka is een open-source, gedistribueerd streamingplatform dat beschikbaar is met HDInsight. Het wordt vaak gebruikt als een berichtenbroker, omdat het een functionaliteit biedt die vergelijkbaar is met een publicatie-/abonnementswachtrij voor berichten. Kafka wordt vaak samen met Apache Spark en Apache Storm gebruikt.

> [!NOTE]
> Er zijn momenteel twee versies van Kafka beschikbaar met HDInsight: 0.9.0 (HDInsight 3.4) en 0.10.0 (HDInsight 3.5 en 3.6). Voor de stappen in dit document wordt ervan uitgegaan dat u Kafka in HDInsight 3.6 gebruikt.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a>Een Kafka-cluster maken

Gebruik de volgende stappen om een Kafka in HDInsight-cluster te maken:

1. In de [Azure Portal](https://portal.azure.com) selecteert u **+ Een resource maken**, **Gegevens en analyses** en vervolgens **HDInsight**.
   
    ![Een HDInsight-cluster maken](./media/apache-kafka-get-started/create-hdinsight.png)

2. Voer bij **Basisbeginselen** de volgende informatie in:

    * **Clusternaam**: de naam van het HDInsight-cluster.
    * **Abonnement**: selecteer het abonnement dat u wilt gebruiken.
    * **Aanmeldingsgebruikersnaam** en -**wachtwoord** van cluster: de aanmeldingsgegevens voor toegang tot het cluster via HTTPS. U gebruikt deze referenties voor toegang tot services als de Ambari-webinterface of de REST-API.
    * **SSH-gebruikersnaam (Secure Shell)**: de aanmeldingsgegevens voor toegang tot het cluster via SSH. Het wachtwoord is standaard hetzelfde als het aanmeldingswachtwoord van het cluster.
    * **Resourcegroep**: de resourcegroep waarin het cluster wordt gemaakt.
    * **Locatie**: de Azure-regio waarin het cluster wordt gemaakt.

        > [!IMPORTANT]
        > Voor hoge beschikbaarheid van gegevens wordt u geadviseerd om een locatie (regio) in te stellen die __drie foutdomeinen__ heeft. Zie de sectie [Hoge beschikbaarheid van gegevens](#data-high-availability) voor meer informatie.
   
 ![Abonnement selecteren](./media/apache-kafka-get-started/hdinsight-basic-configuration.png)

3. Selecteer **Clustertype** en stel bij **Clusterconfiguratie** de volgende waarden in:
   
    * **Clustertype**: Kafka
    * **Versie**: Kafka 0.10.0 (HDI 3.6)

    Gebruik ten slotte de knop **Selecteren** om de instellingen op te slaan.
     
 ![Clustertype selecteren](./media/apache-kafka-get-started/set-hdinsight-cluster-type.png)

4. Nadat u het clustertype hebt geselecteerd, gebruikt u de knop __Selecteren__ om het clustertype te selecteren. Gebruik vervolgens de knop __Volgende__ om de basisconfiguratie te voltooien.

5. Selecteer of maak bij **Opslag** een opslagaccount. Voor de stappen in dit document laat u de andere velden op de standaardwaarden ingesteld. Gebruik de knop __Volgende__ om de opslagconfiguratie op te slaan.

    ![De instellingen van het opslagaccount voor HDInsight configureren](./media/apache-kafka-get-started/set-hdinsight-storage-account.png)

6. Selecteer bij __Toepassingen (optioneel)__ de optie __Volgende__ om door te gaan. Er zijn geen toepassingen vereist voor dit voorbeeld.

7. Selecteer bij __Clustergrootte__ de optie __Volgende__ om door te gaan.

    > [!WARNING]
    > Om beschikbaarheid van Kafka op HDInsight te garanderen, moet uw cluster ten minste drie werkknooppunten bevatten. Zie de sectie [Hoge beschikbaarheid van gegevens](#data-high-availability) voor meer informatie.

    ![De Kafka-clustergrootte instellen](./media/apache-kafka-get-started/kafka-cluster-size.png)

    > [!IMPORTANT]
    > De waarde voor de **schijven per werkknooppunt** bepaalt de schaalbaarheid van Kafka op HDInsight. Kafka in HDInsight maakt gebruik van de lokale schijf van de virtuele machines in het cluster. Omdat Kafka veel gebruikmaakt van invoer/uitvoer, wordt [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md) gebruikt voor een hoge doorvoer en meer opslag per knooppunt. Het type beheerde schijf is __Standaard__ (HDD) of __Premium__ (SSD). Premium-schijven worden gebruikt met virtuele machines uit de DS- en GS-reeks. Alle andere VM-typen gebruiken standaardschijven.

8. Selecteer bij __Geavanceerde instellingen__ de optie __Volgende__ om door te gaan.

9. Controleer bij **Samenvatting** de configuratie van het cluster. Gebruik de koppeling __Bewerken__ om onjuiste instellingen te wijzigen. Gebruik tot slot de knop __Maken__ om het cluster te maken.
   
    ![Samenvatting clusterconfiguratie](./media/apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > Het kan tot 20 minuten duren om het cluster te maken.

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

> [!IMPORTANT]
> Wanneer u de volgende stappen uitvoert, moet u een SSH-client gebruiken. Zie het document [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

Gebruik SSH in uw client om verbinding te maken met het cluster:

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

Vervang **SSHUSER** door de SSH-gebruikersnaam die u hebt opgegeven tijdens het maken van het cluster. Vervang **CLUSTERNAME** door de naam van uw cluster.

Voer het wachtwoord van het SSH-account in wanneer hierom wordt gevraagd.

Zie [SSH-sleutels gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

## <a id="getkafkainfo"></a>Informatie over Zookeeper- en brokerhosts ophalen

Wanneer u met Kafka werkt, moet u twee hostwaarden weten: de *Zookeeper*-hosts en de *brokerhosts*. Deze hosts worden gebruikt met de Kafka-API en veel van de hulpprogramma's die bij Kafka worden meegeleverd.

Gebruik de volgende stappen om omgevingsvariabelen te maken die de hostinformatie bevatten. Deze omgevingsvariabelen worden in de stappen in dit document gebruikt.

1. Wanneer er een SSH-verbinding is gemaakt met het cluster, gebruikt u de volgende opdracht om het hulpprogramma `jq` te installeren. Dit hulpprogramma wordt gebruikt om JSON-documenten te parseren en is handig bij het ophalen van informatie van de brokerhost:
   
    ```bash
    sudo apt -y install jq
    ```

2. Gebruik de volgende opdrachten om de omgevingsvariabelen in te stellen met informatie die is verkregen van Ambari:

    ```bash
    CLUSTERNAME='your cluster name'
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    > [!IMPORTANT]
    > Stel `CLUSTERNAME=` in op de naam van het Kafka-cluster. Voer desgevraagd het wachtwoord voor het account voor clusteraanmelding (admin).

    De inhoud van `$KAFKAZKHOSTS` kan er als volgt uitzien:
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    De inhoud van `$KAFKABROKERS` kan er als volgt uitzien:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

    > [!NOTE]
    > De opdracht `cut` wordt gebruikt om de lijst met hosts in te korten tot twee hostvermeldingen. U hoeft niet de volledige lijst met hosts op te geven bij het maken van een Kafka-consument of -producent.
   
    > [!WARNING]
    > Ga er niet van uit dat de informatie die uit deze sessie naar voren komt altijd accuraat is. Als u het cluster schaalt, worden er nieuwe brokers toegevoegd of verwijderd. Als er een fout optreedt en een knooppunt is vervangen, kan de hostnaam van het knooppunt veranderen.
    >
    > Haal de informatie over de Zookeeper- en brokerhosts kort voordat u deze gebruikt pas op, om er zeker van te zijn dat de gegevens geldig zijn.

## <a name="create-a-topic"></a>Een onderwerp maken

Kafka slaat gegevensstromen op in categorieën, zogenaamde *onderwerpen*. Wanneer er een SSH-verbinding is gemaakt met het hoofdknooppunt van het cluster, gebruikt u een script dat bij Kafka is meegeleverd om een onderwerp te maken:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

Met deze opdracht brengt u een verbinding met Zookeeper tot stand met behulp van de hostinformatie die is opgeslagen in `$KAFKAZKHOSTS`. Maak vervolgens een Kafka-onderwerp met de naam **test**. U kunt controleren of het onderwerp is gemaakt door met het volgende script een lijst van de onderwerpen te genereren:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

Met deze opdracht krijgt u een lijst van Kafka-onderwerpen, waaronder het onderwerp **test**.

## <a name="produce-and-consume-records"></a>Records maken en gebruiken

Kafka slaat *records* op in onderwerpen. Records worden geproduceerd door *producenten* en worden gebruikt door *consumenten*. Producenten maken records voor Kafka-*brokers*. Elk werkrolknooppunt in uw HDInsight-cluster is een Kafka-broker.

Gebruik de volgende stappen om records op te slaan in het testonderwerp dat u eerder hebt gemaakt. Lees deze vervolgens met behulp van een consument:

1. Gebruik tijdens de SSH-sessie een script dat bij Kafka is meegeleverd om records te schrijven naar het onderwerp:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Na deze opdracht keert u niet terug naar de prompt. In plaats daarvan typt u een paar tekstberichten en gebruikt u vervolgens **Ctrl + C** om verzending naar het onderwerp te stoppen. Elke regel wordt als een afzonderlijke record verzonden.

2. Gebruik een script dat bij Kafka is meegeleverd om records in het onderwerp te lezen:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    Met deze opdracht haalt u de records van het onderwerp op en geeft u deze weer. Met `--from-beginning` wordt de consument gevraagd om bij het begin van de stream te beginnen, zodat alle records worden opgehaald.

    > [!NOTE]
    > Als u een oudere versie van Kafka gebruikt, moet u `--bootstrap-server $KAFKABROKERS` mogelijk vervangen door `--zookeeper $KAFKAZKHOSTS`.

3. Gebruik __Ctrl + C__ om de consument te stoppen.

U kunt ook programmatisch producenten en consumenten maken. Zie het document [Producer and Consumer API van Kafka met HDInsight](apache-kafka-producer-consumer-api.md) voor een voorbeeld van het gebruik van deze API.

## <a name="data-high-availability"></a>Hoge beschikbaarheid van gegevens

Elke Azure-regio (locatie) heeft _foutdomeinen_. Een foutdomein is een logische groepering van de onderliggende hardware in een Azure-datacenter. Elk foutdomein deelt een algemene voedingsbron en netwerkswitch. De virtuele machines en beheerde schijven die de knooppunten in een HDInsight-cluster implementeren zijn verdeeld over deze foutdomeinen. Deze architectuur beperkt de potentiële impact van problemen met de fysieke hardware.

Raadpleeg het document [Beschikbaarheid van virtuele Linux-machines](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) voor informatie over het aantal foutdomeinen in een regio.

> [!IMPORTANT]
> Wij raden het gebruik aan van een Azure-regio die drie foutdomeinen bevat en van een replicatiefactor van 3.

Als u een regio met slechts twee foutdomeinen moet gebruiken, gebruik dan een replicatiefactor van 4 om de replica's gelijkmatig te verdelen over de twee foutdomeinen.

### <a name="kafka-and-fault-domains"></a>Kafka en foutdomeinen

Kafka is niet bekend met foutdomeinen. Bij het maken van partitiereplica's voor onderwerpen worden replica's mogelijk niet goed gedistribueerd voor hoge beschikbaarheid. Gebruik het [partitieherverdelingsprogramma van Kafka](https://github.com/hdinsight/hdinsight-kafka-tools) voor gegarandeerde hoge beschikbaarheid. Dit hulpprogramma moet vanaf een SSH-sessie naar het hoofdknooppunt van het Kafka-cluster worden uitgevoerd.

Om de hoogst mogelijke beschikbaarheid van uw Kafka-gegevens te waarborgen, moet u de partitiereplica's voor uw onderwerp op de volgende tijden opnieuw indelen:

* wanneer een nieuw onderwerp of partitie wordt gemaakt

* wanneer u een cluster opschaalt

## <a name="delete-the-cluster"></a>Het cluster verwijderen

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](../hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u de basisbeginselen geleerd van hoe u met Apache Kafka in HDInsight werkt. Gebruik de volgende documenten voor meer informatie over het werken met Kafka:

* [Kafka-logboekbestanden analyseren](apache-kafka-log-analytics-operations-management.md)
* [Gegevens repliceren tussen Kafka-clusters](apache-kafka-mirroring.md)
* [Producer and Consumer API van Kafka met HDInsight](apache-kafka-producer-consumer-api.md)
* [Streams API van Kafka met HDInsight](apache-kafka-streams-api.md)
* [Apache Spark-streaming (DStream) gebruiken met Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Apache Spark Structured Streaming met Kafka in HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md)
* [Apache Spark Structured Streaming gebruiken om gegevens te verplaatsen van Kafka in HDInsight naar Cosmos DB](../apache-kafka-spark-structured-streaming-cosmosdb.md)
* [Apache Storm gebruiken met Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Verbinding maken met Kafka via een Azure Virtual Network](apache-kafka-connect-vpn-gateway.md)
