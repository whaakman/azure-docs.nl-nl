---
title: Aan de slag met Apache Kafka in HDInsight | Microsoft Docs
description: Leer de basisbeginselen van het maken en werken met Kafka op HDInsight.
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
ms.date: 02/14/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: 695c6bd0a08e88be2d8e28eb15d903f3ae1eccaf
ms.contentlocale: nl-nl
ms.lasthandoff: 05/03/2017

---
# <a name="get-started-with-apache-kafka-preview-on-hdinsight"></a>Aan de slag met Apache Kafka (preview) in HDInsight

[Apache Kafka](https://kafka.apache.org) is een open-source gedistribueerd streamingplatform dat beschikbaar is met HDInsight. Het wordt vaak gebruikt als een berichtenbroker, omdat het een functionaliteit biedt die vergelijkbaar is met een publicatie-/abonnementswachtrij voor berichten. In dit document vindt u informatie over het maken van een Kafka in HDInsight-cluster en het verzenden en ontvangen van gegevens in een Java-toepassing.

> [!NOTE]
> Momenteel zijn er twee versies van Kafka beschikbaar met HDInsight: 0.9.0 (HDInsight 3.4) en 0.10.0 (HDInsight 3.5). Voor de stappen in dit document wordt ervan uitgegaan dat u Kafka in HDInsight 3.5 gebruikt.

## <a name="prerequisite"></a>Vereiste

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Om deze Apache Kafka-zelfstudie te voltooien, moet u beschikken over het volgende:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Kennis van SSH en SCP**. Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) of een equivalent, zoals OpenJDK.

* [Apache Maven](http://maven.apache.org/) 

## <a name="create-a-kafka-cluster"></a>Een Kafka-cluster maken

Gebruik de volgende stappen om een Kafka in HDInsight-cluster te maken:

1. In [Azure Portal](https://portal.azure.com) selecteert u **+ NIEUW**, **Intelligence en analyse** en vervolgens **HDInsight**.
   
    ![Een HDInsight-cluster maken](./media/hdinsight-apache-kafka-get-started/create-hdinsight.png)

2. Geef op de blade **Basisbeginselen** de volgende gegevens op:

    * **Clusternaam**: de naam van het HDInsight-cluster.
    * **Abonnement**: selecteer het abonnement dat u wilt gebruiken.
    * **Aanmeldingsgebruikersnaam** en -**wachtwoord** van cluster: de aanmeldingsgegevens voor toegang tot het cluster via HTTPS. U gebruikt deze referenties voor toegang tot services als de Ambari-webinterface of de REST-API.
    * **SSH-gebruikersnaam (Secure Shell)**: de aanmeldingsgegevens voor toegang tot het cluster via SSH. Het wachtwoord is standaard hetzelfde als het aanmeldingswachtwoord van het cluster.
    * **Resourcegroep**: de resourcegroep waarin het cluster wordt gemaakt.
    * **Locatie**: de Azure-regio waarin het cluster wordt gemaakt.
   
    ![Abonnement selecteren](./media/hdinsight-apache-kafka-get-started/hdinsight-basic-configuration.png)

3. Selecteer **Clustertype** en stel de volgende waarden in op de blade **Clusterconfiguratie**:
   
    * **Clustertype**: Kafka

    * **Versie**: Kafka 0.10.0 (HDI 3.5)

    * **Clusterlaag**: standaard
     
    Gebruik ten slotte de knop **Selecteren** om de instellingen op te slaan.
     
    ![Clustertype selecteren](./media/hdinsight-apache-kafka-get-started/set-hdinsight-cluster-type.png)

    > [!NOTE]
    > Als uw Azure-abonnement geen toegang geeft tot de Kafka-preview, worden er instructies voor preview-toegang weergegeven. De weergegeven instructies zijn vergelijkbaar met de volgende afbeelding:
    >
    > ![Preview-bericht: Als u een beheerde Apache Kafka-cluster in HDInsight wilt implementeren, stuur ons dan een e-mail om toegang tot de preview te vragen](./media/hdinsight-apache-kafka-get-started/no-kafka-preview.png)

4. Nadat u het clustertype hebt geselecteerd, gebruikt u de knop __Selecteren__ om het clustertype te selecteren. Gebruik vervolgens de knop __Volgende__ om de basisconfiguratie te voltooien.

5. Op de blade **Opslag** selecteert of maakt u een opslagaccount. Voor de stappen in dit document laat u de andere velden in deze blade op de standaardwaarden. Gebruik de knop __Volgende__ om de opslagconfiguratie op te slaan.

    ![De instellingen van het opslagaccount voor HDInsight configureren](./media/hdinsight-apache-kafka-get-started/set-hdinsight-storage-account.png)

6. Controleer op de blade **Samenvatting** de configuratie van het cluster. Gebruik de koppeling __Bewerken__ om onjuiste instellingen te wijzigen. Gebruik tot slot de knop __Maken__ om het cluster te maken.
   
    ![Samenvatting clusterconfiguratie](./media/hdinsight-apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > Het kan tot 20 minuten duren om het cluster te maken.

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Gebruik SSH in uw client om verbinding te maken met het cluster. Als u gebruikmaakt van Linux, Unix, Mac OS of Bash in Windows 10, gebruikt u de volgende opdracht:

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

Vervang **SSHUSER** door de SSH-gebruikersnaam die u hebt opgegeven tijdens het maken van het cluster. Vervang **CLUSTERNAME** door de naam van uw cluster.

Voer het wachtwoord van het SSH-account in wanneer hierom wordt gevraagd.

Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

##<a id="getkafkainfo"></a>Informatie over Zookeeper- en brokerhosts ophalen

Wanneer u met Kafka werkt, moet u twee hostwaarden weten: de *Zookeeper*-hosts en de *brokerhosts*. Deze hosts worden gebruikt met de Kafka-API en veel van de hulpprogramma's die bij Kafka worden meegeleverd.

Gebruik de volgende stappen om omgevingsvariabelen te maken die de hostinformatie bevatten. Deze omgevingsvariabelen worden in de stappen in dit document gebruikt.

1. Wanneer er een SSH-verbinding is gemaakt met het cluster, gebruikt u de volgende opdracht om het hulpprogramma `jq` te installeren. Dit hulpprogramma wordt gebruikt om JSON-documenten te parseren en is handig bij het ophalen van informatie van de brokerhost:
   
    ```bash
    sudo apt -y install jq
    ```

2. Gebruik de volgende opdrachten om de omgevingsvariabelen in te stellen met informatie die is verkregen van Ambari. Vervang __KAFKANAME__ door de naam van het Kafka-cluster. Vervang __wachtwoord__ door het aanmeldingswachtwoord (beheerder) die u hebt gebruikt bij het maken van het cluster.

    ```bash
    export KAFKAZKHOSTS=`curl --silent -u admin:'PASSWORD' -G http://headnodehost:8080/api/v1/clusters/KAFKANAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")'`

    export KAFKABROKERS=`curl --silent -u admin:'PASSWORD' -G http://headnodehost:8080/api/v1/clusters/KAFKANAME/services/HDFS/components/DATANODE | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")'`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    De inhoud van `$KAFKAZKHOSTS` kan er als volgt uitzien:
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk3-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    De inhoud van `$KAFKABROKERS` kan er als volgt uitzien:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`
   
    > [!WARNING]
    > Ga er niet van uit dat de informatie die uit deze sessie naar voren komt altijd accuraat is. Als u het cluster schaalt, worden er nieuwe brokers toegevoegd of verwijderd. Als er een fout optreedt en een knooppunt is vervangen, kan de hostnaam van het knooppunt veranderen. 
    > 
    > Haal de informatie over de Zookeeper- en brokerhosts kort voordat u deze gebruikt pas op, om er zeker van te zijn dat de gegevens geldig zijn.

## <a name="create-a-topic"></a>Een onderwerp maken

Kafka slaat gegevensstromen op in categorieën, zogenaamde *onderwerpen*. Wanneer er een SSH-verbinding is gemaakt met het hoofdknooppunt van het cluster, gebruikt u een script dat bij Kafka is meegeleverd om een onderwerp te maken:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

Met deze opdracht brengt u een verbinding met Zookeeper tot stand met behulp van de hostinformatie die is opgeslagen in `$KAFKAZKHOSTS`. Maak vervolgens een Kafka-onderwerp met de naam **test**. U kunt controleren of het onderwerp is gemaakt door met het volgende script een lijst van de onderwerpen te genereren:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

Met deze opdracht krijgt u een lijst van Kafka-onderwerpen, waaronder het onderwerp **test**.

## <a name="produce-and-consume-records"></a>Records maken en gebruiken

Kafka slaat *records* op in onderwerpen. Records worden geproduceerd door *producenten* en worden gebruikt door *consumenten*. Producenten halen records op uit Kafka-*brokers*. Elk werkrolknooppunt in uw HDInsight-cluster is een Kafka-broker.

Gebruik de volgende stappen om records op te slaan in het testonderwerp dat u eerder hebt gemaakt. Lees deze vervolgens met behulp van een consument:

1. Gebruik tijdens de SSH-sessie een script dat bij Kafka is meegeleverd om records te schrijven naar het onderwerp:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Na deze opdracht keert u niet terug naar de prompt. In plaats daarvan typt u een paar tekstberichten en gebruikt u vervolgens **Ctrl + C** om verzending naar het onderwerp te stoppen. Elke regel wordt als een afzonderlijke record verzonden.

2. Gebruik een script dat bij Kafka is meegeleverd om records in het onderwerp te lezen:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic test --from-beginning
    ```
   
    Hiermee haalt u de records van het onderwerp op en geeft u deze weer. Met `--from-beginning` wordt de consument gevraagd om bij het begin van de stream te beginnen, zodat alle records worden opgehaald.

3. Gebruik __Ctrl + C__ om de consument te stoppen.

## <a name="producer-and-consumer-api"></a>Producent- en consument-API

U kunt records ook programmatisch maken en gebruiken met behulp van de [Kafka-API's](http://kafka.apache.org/documentation#api). Gebruik de volgende stappen om een op Java gebaseerde producent en consument te downloaden en bouwen:

1. Download de voorbeelden van [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started). Voor de producent/consumentvoorbeelden gebruikt u het project in de directory `Producer-Consumer`. Dit voorbeeld bevat de volgende klassen:
   
    * **Uitvoering**: hiermee wordt de consument of de producent gestart.

    * **Producent**: hiermee kunnen 1.000.000 records in het onderwerp worden opgeslagen.

    * **Consument**: hiermee kunnen records in een onderwerp worden gelezen.

2. In de opdrachtregel in uw ontwikkelingsomgeving wijzigt u de directory's in de locatie van de directory `Producer-Consumer` van het voorbeeld. Vervolgens gebruikt u de volgende opdracht om een JAR-pakket te maken:
   
    ```
    mvn clean package
    ```
   
    Met deze opdracht maakt u een directory met de naam `target`, die een bestand met de naam `kafka-producer-consumer-1.0-SNAPSHOT.jar` bevat.

3. Gebruik de volgende opdrachten om het bestand `kafka-producer-consumer-1.0-SNAPSHOT.jar` te kopiëren naar uw HDInsight-cluster:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    Vervang **SSHUSER** door de SSH-gebruiker van uw cluster en vervang **CLUSTERNAME** door de naam van het cluster. Voer het wachtwoord van de SSH-gebruiker in wanneer hierom wordt gevraagd.

4. Nadat de opdracht `scp` klaar is met het kopiëren van het bestand, maakt u een SSH-verbinding met het cluster en gebruikt u het volgende om records te schrijven naar het testonderwerp dat u eerder hebt gemaakt.
   
    ```bash
    ./kafka-producer-consumer.jar producer $KAFKABROKERS
    ```
   
    Met deze opdracht wordt de producent gestart en worden er records geschreven. Een teller geeft aan hoeveel records er zijn geschreven.

    > [!NOTE]
    > Als u de fout Toegang geweigerd ontvangt, gebruikt u de volgende opdracht om het bestand uitvoerbaar te maken: ```chmod +x kafka-producer-consumer.jar```

5. Nadat het proces is voltooid, gebruikt u de volgende opdracht om het onderwerp te lezen:
   
    ```bash
    ./kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    De gelezen records worden weergegeven, samen met een telling van de records. Mogelijk ziet u tellingen van meer dan 1.000.000, omdat u in een eerdere stap meerdere records naar het onderwerp hebt verzonden met behulp van een script.

6. Gebruik __Ctrl + C__ om de consument af te sluiten.

### <a name="multiple-consumers"></a>Meerdere consumenten

Een belangrijk concept bij Kafka is dat consumenten een consumentengroep (gedefinieerd door een groeps-id) gebruiken bij het lezen van records. Door dezelfde groep voor meerdere consumenten te gebruiken, worden leestaken voor onderwerpen gelijk verdeeld. Elke consument in de groep ontvangt een deel van de records. Voer de volgende stappen uit om dit proces in actie te zien:

1. Open een nieuwe SSH-sessie met het cluster, zodat u twee sessies hebt. Gebruik het volgende in elke sessie om een consument te starten met dezelfde consumentengroeps-id:
   
    ```bash
    ./kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```

    > [!NOTE]
    > Aangezien dit een nieuwe SSH-sessie is, moet u de opdrachten in het gedeelte [Informatie over Zookeeper- en brokerhosts ophalen](#getkafkainfo) gebruiken om `$KAFKABROKERS` in te stellen.

2. Voor elke sessie worden nu de records geteld die worden ontvangen van het onderwerp. Het totaal van beide sessies moet hetzelfde zijn als wat u eerder ontving van één consument.

Gebruik door clients binnen dezelfde groep wordt verwerkt door de partities voor het onderwerp. Het eerder gemaakte onderwerp `test` heeft acht partities. Als u acht SSH-sessies opent en in alle sessies een consument start, leest de consument de records van één partitie voor het onderwerp.

> [!IMPORTANT]
> Een consumentengroep kan niet meer consumentexemplaren dan partities bevatten. In dit voorbeeld kan één klantengroep maximaal acht consumenten bevatten, omdat het onderwerp dit aantal partities heeft. U kunt ook meerdere consumentengroepen hebben, die elk niet meer dan acht consumenten bevat.

Records worden in Kafka opgeslagen in de volgorde waarin deze worden ontvangen binnen een partitie. Als u records *binnen een partitie* op volgorde wilt leveren, maakt u een consumentengroep waarvan het aantal consumentexemplaren gelijk is aan het aantal partities. Als u records *binnen het onderwerp* op volgorde wilt leveren, maakt u een consumentengroep met slechts één consumentexemplaar.

## <a name="streaming-api"></a>Streaming-API

De streaming-API is in versie 0.10.0 aan Kafka toegevoegd. Eerdere versies zijn afhankelijk van Apache Spark of Storm voor streamverwerking.

1. Download de voorbeelden van [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) naar uw ontwikkelingsomgeving als u dit nog niet hebt gedaan. Voor het streamingvoorbeeld gebruikt u het project in de directory `streaming`.
   
    Dit project bevat slechts één klasse: `Stream`. Deze leest records van het eerder gemaakte onderwerp `test`. De gelezen woorden worden geteld, en elk woord en de telling worden verzonden een onderwerp met de naam `wordcounts`. Het onderwerp `wordcounts` wordt in een latere stap in dit gedeelte gemaakt.

2. In de opdrachtregel in uw ontwikkelingsomgeving wijzigt u de directory's in de locatie van de directory `Streaming`. Vervolgens gebruikt u de volgende opdracht om een JAR-pakket te maken:
   
    ```
    mvn clean package
    ```
   
    Met deze opdracht maakt u een directory met de naam `target`, die een bestand met de naam `kafka-streaming-1.0-SNAPSHOT.jar` bevat.

3. Gebruik de volgende opdrachten om het bestand `kafka-streaming-1.0-SNAPSHOT.jar` te kopiëren naar uw HDInsight-cluster:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Vervang **SSHUSER** door de SSH-gebruiker van uw cluster en vervang **CLUSTERNAME** door de naam van het cluster. Voer het wachtwoord van de SSH-gebruiker in wanneer hierom wordt gevraagd.

4. Nadat de opdracht `scp` klaar is met het kopiëren van het bestand, maakt u een SSH-verbinding met het cluster en gebruikt u de volgende opdracht om het onderwerp `wordcounts` te maken:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

5. Start vervolgens het streamingproces met behulp van de volgende opdracht:
   
    ```bash
    ./kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS 2>/dev/null &
    ```
   
    Met deze opdracht wordt het streamingproces op de achtergrond gestart.

6. Gebruik de volgende opdracht om berichten naar het onderwerp `test` te verzenden. Deze berichten worden verwerkt door het streamingvoorbeeld:
   
    ```bash
    ./kafka-producer-consumer.jar producer $KAFKABROKERS &>/dev/null &
    ```

7. Gebruik de volgende opdracht om de uitvoer weer te geven die tijdens het streamingproces wordt geschreven naar het onderwerp `wordcounts`:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic wordcounts --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
    ```
   
    > [!NOTE]
    > Om de gegevens te bekijken, geeft u de consument de opdracht om de sleutel af te drukken, evenals de deserialisatiefunctie die wordt gebruikt voor de sleutel en waarde. De sleutelnaam is het woord en de sleutelwaarde bevat de telling.
   
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
        a       13805
        snow    13637
   
    > [!NOTE]
    > De telling wordt verhoogd wanneer er een woord wordt gedetecteerd.

7. Gebruik __Ctrl + C__ om de consument af te sluiten. Gebruik vervolgens de opdracht `fg` om de streamingtaak weer op de voorgrond uit te voeren. Gebruik __Ctrl + C__ om af te sluiten.

## <a name="delete-the-cluster"></a>Het cluster verwijderen

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u de basisbeginselen geleerd van hoe u met Apache Kafka in HDInsight werkt. Gebruik de volgende documenten voor meer informatie over het werken met Kafka:

* [Documentatie voor Apache Kafka](http://kafka.apache.org/documentation.html) op kafka.apache.org.
* [MirrorMaker gebruiken voor het maken van een replica van Kafka in HDInsight](hdinsight-apache-kafka-mirroring.md)
* [Apache Storm gebruiken met Kafka in HDInsight](hdinsight-apache-storm-with-kafka.md)
* [Apache Spark gebruiken met Kafka in HDInsight](hdinsight-apache-spark-with-kafka.md)
* [Verbinding maken met Kafka via een Azure Virtual Network](hdinsight-apache-kafka-connect-vpn-gateway.md)
