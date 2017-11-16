---
title: Apache Kafka gebruiken met Storm op HDInsight - Azure | Microsoft Docs
description: "Apache Kafka wordt geïnstalleerd met Apache Storm op HDInsight. Informatie over het schrijven naar Kafka en gelezen, met behulp van de KafkaBolt en KafkaSpout-onderdelen die zijn opgegeven met Storm. Ook informatie over het gebruik van het framework lichtstroom te definiëren en het verzenden van Storm-topologieën."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e4941329-1580-4cd8-b82e-a2258802c1a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/07/2017
ms.author: larryfr
ms.openlocfilehash: 50a22877241c77ccb1a7df24ab7df006094a439f
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="use-apache-kafka-with-storm-on-hdinsight"></a>Apache Kafka met Storm op HDInsight gebruiken

Informatie over het gebruik van Apache Storm om te lezen van en schrijven naar Apache Kafka. Dit voorbeeld wordt ook het opslaan van gegevens van een Storm-topologie naar het bestandssysteem van HDFS-compatibele die door HDInsight worden gebruikt.

> [!NOTE]
> De stappen in dit document wordt een Azure-resourcegroep met zowel een Storm op HDInsight en een Kafka op HDInsight-cluster maken Deze clusters zijn beide zich binnen een virtueel netwerk van Azure, waardoor de Storm-cluster rechtstreeks communiceren met de Kafka-cluster.
> 
> Wanneer u klaar bent met de stappen in dit document, vergeet niet de clusters om te voorkomen dat de overtollige kosten te verwijderen.

## <a name="get-the-code"></a>Code ophalen

De code voor het voorbeeld in dit document gebruikt is beschikbaar op [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).

Als u wilt compileren dit project, moet u de volgende configuratie voor uw ontwikkelomgeving:

* [Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) of hoger. HDInsight 3.5 of hoger vereisen Java 8.

* [Maven 3.x](https://maven.apache.org/download.cgi)

* Een SSH-client (u moet de `ssh` en `scp` opdrachten) - informatie, Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* Een teksteditor of IDE.

De volgende omgevingsvariabelen kunnen worden ingesteld wanneer u Java en de JDK op uw ontwikkelwerkstation installeert. Echter, moet u controleren of ze bestaan en dat ze de juiste waarden voor uw systeem bevatten.

* `JAVA_HOME`-moet verwijzen naar de map waar de JDK die is geïnstalleerd.
* `PATH`-moet bevatten de volgende paden:
  
    * `JAVA_HOME`(of equivalente paden).
    * `JAVA_HOME\bin`(of equivalente paden).
    * De map waar Maven is geïnstalleerd.

## <a name="create-the-clusters"></a>Clusters maken

Apache Kafka op HDInsight biedt geen toegang voor de beleggingsmakelaars Kafka via het openbare internet. Alles wat wordt gesproken naar Kafka moet zich in hetzelfde virtuele netwerk van Azure als de knooppunten in het cluster Kafka. In dit voorbeeld worden de Kafka en de Storm-clusters bevinden zich in een Azure-netwerk. Het volgende diagram toont hoe de communicatie tussen clusters stroomt:

![Diagram van Storm en Kafka clusters in een Azure-netwerk](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> Andere services op het cluster zoals SSH en Ambari toegankelijk zijn via internet. Zie voor meer informatie over de openbare poorten beschikbaar met HDInsight [poorten en URI's die worden gebruikt door HDInsight](hdinsight-hadoop-port-settings-for-services.md).

U kunt een Azure-netwerk Kafka, maken en handmatig Storm-clusters, is het ook eenvoudiger te gebruiken van een Azure Resource Manager-sjabloon. Gebruik de volgende stappen voor het implementeren van een Azure-netwerk Kafka, en Storm-clusters met uw Azure-abonnement.

1. Gebruik de volgende knop om te melden bij Azure en opent u de sjabloon in de Azure portal.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-storm-cluster-in-vnet-v2.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    De Azure Resource Manager-sjabloon bevindt zich op **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v2.json**. Hiermee maakt u de volgende bronnen:
    
    * Azure-resourcegroep
    * Azure Virtual Network
    * Azure-opslagaccount
    * Kafka op HDInsight versie 3.6 (drie worker-knooppunten)
    * Storm op HDInsight versie 3.6 (drie worker-knooppunten)

  > [!WARNING]
  > Om beschikbaarheid van Kafka op HDInsight te garanderen, moet uw cluster ten minste drie werkknooppunten bevatten. Deze sjabloon maakt een Kafka-cluster dat drie worker-knooppunten bevat.

2. Gebruik de volgende richtlijnen voor het vullen van de vermeldingen op de **aangepaste implementatie** sectie:
   
    ![Aangepaste HDInsight-implementatie](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * **Resourcegroep**: Maak een groep of een bestaande set selecteren. Deze groep bevat het HDInsight-cluster.
   
    * **Locatie**: Selecteer een locatie geografisch dicht bij u.

    * **Clusternaam baseren**: deze waarde wordt gebruikt als de basisnaam aan voor de Storm en Kafka-clusters. Bijvoorbeeld, voeren **hdi** maakt een Storm-cluster met de naam **storm hdi** en een Kafka-cluster met de naam **kafka hdi**.
   
    * **Aanmeldingsnaam van gebruiker cluster**: de gebruikersnaam van de beheerder voor de Storm en Kafka-clusters.
   
    * **Aanmeldingswachtwoord cluster**: het beheerderswachtwoord voor de Storm en Kafka-clusters.
    
    * **SSH-gebruikersnaam**: de SSH-gebruiker maken voor de Storm en Kafka-clusters.
    
    * **SSH-wachtwoord**: het wachtwoord voor de SSH-gebruiker voor de Storm en Kafka-clusters.

3. Lees de **voorwaarden en bepalingen**, en selecteer vervolgens **ik ga akkoord met de voorwaarden en bepalingen bovengenoemde**.

4. Controleer ten slotte **vastmaken aan dashboard** en selecteer vervolgens **aankoop**. Het duurt ongeveer 20 minuten om de clusters te maken.

Zodra de resources zijn gemaakt, wordt de sectie voor de resourcegroep weergegeven.

![Sectie van de resource-groep voor het vnet en clusters](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> Merk op dat de namen van de HDInsight-clusters **storm BASENAME** en **kafka BASENAME**, waarbij BASENAME is de naam die u hebt opgegeven in de sjabloon. U kunt deze namen in latere stappen gebruiken bij het verbinden met de clusters.

## <a name="understanding-the-code"></a>Wat is de code?

Dit project bevat twee topologieën:

* **KafkaWriter**: gedefinieerd door de **writer.yaml** -bestand, deze topologie schrijft willekeurig zinnen naar Kafka met behulp van de opgegeven met Apache Storm KafkaBolt.

    Deze topologie maakt gebruik van een aangepaste **SentenceSpout** onderdeel voor het genereren van willekeurige zinnen.

* **KafkaReader**: gedefinieerd door de **reader.yaml** bestand, deze topologie leest de gegevens uit met behulp van de opgegeven met Apache Storm KafkaSpout Kafka en vervolgens de gegevens geregistreerd in stdout.

    Deze topologie wordt de Storm-HdfsBolt gebruikt om gegevens te schrijven naar de opslag van de standaard voor het Storm-cluster.
### <a name="flux"></a>Lichtstroom

De topologieën worden gedefinieerd met [lichtstroom](https://storm.apache.org/releases/1.1.0/flux.html). Lichtstroom werd geïntroduceerd in 0.10.x Storm en kunt u de topologieconfiguratie van de code te scheiden. De topologie is voor de topologieën die gebruikmaken van het framework lichtstroom, gedefinieerd in een YAML-bestand. Het YAML-bestand kan worden opgenomen als onderdeel van de topologie. Het kan ook een zelfstandig bestand gebruikt wanneer u de topologie indienen zijn. Lichtstroom ondersteunt ook het vervangen van de variabelen tijdens runtime, die wordt gebruikt in dit voorbeeld.

De volgende parameters zijn ingesteld tijdens de uitvoering voor deze topologieën:

* `${kafka.topic}`: De naam van het onderwerp Kafka die de topologieën lezen/schrijven naar.

* `${kafka.broker.hosts}`: De hosts die de Kafka makelaars uitvoeren op. De broker-informatie wordt gebruikt door de KafkaBolt bij het schrijven naar Kafka.

* `${kafka.zookeeper.hosts}`: De hosts die Zookeeper op in het cluster Kafka wordt uitgevoerd.

Zie voor meer informatie over topologieën lichtstroom [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).

## <a name="download-and-compile-the-project"></a>Download en compileren van het project

1. Download het project uit op uw ontwikkelomgeving [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), open een opdrachtregel en wijzig de locatie die u het project hebt gedownload.

2. Van de **hdinsight, storm, java, kafka** directory, de volgende opdracht gebruiken om te compileren van het project en maken van een pakket voor implementatie:

  ```bash
  mvn clean package
  ```

    Het proces van het pakket maakt een bestand met de naam `KafkaTopology-1.0-SNAPSHOT.jar` in de `target` directory.

3. Gebruik de volgende opdrachten in het pakket kopiëren naar uw Storm op HDInsight-cluster. Vervang **gebruikersnaam** met de SSH-gebruikersnaam voor het cluster. Vervang **BASENAME** met de basisnaam die u hebt gebruikt bij het maken van het cluster.

  ```bash
  scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
  ```

    Wanneer u wordt gevraagd, typt u het wachtwoord die u hebt gebruikt bij het maken van clusters.

## <a name="configure-the-topology"></a>De topologie configureren

1. Gebruik een van de volgende methoden voor het detecteren van de broker Kafka hosts:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($brokerHosts -join ":9092,") + ":9092"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > Het Bash-voorbeeld wordt ervan uitgegaan dat `$CLUSTERNAME` bevat de naam van het HDInsight-cluster. Ook wordt ervan uitgegaan dat [jq](https://stedolan.github.io/jq/) is geïnstalleerd. Voer desgevraagd het wachtwoord voor de aanmeldingsaccount van het cluster.

    De waarde die het resultaat is vergelijkbaar met de volgende tekst:

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]
    > Hoewel er mogelijk meer dan twee broker hosts voor uw cluster, hoeft u niet te bevatten een volledige lijst van alle hosts naar clients. Een of twee is voldoende.

2. Gebruik een van de volgende methoden voor het detecteren van de hosts Kafka Zookeeper:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $zookeeperHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($zookeeperHosts -join ":2181,") + ":2181"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > Het Bash-voorbeeld wordt ervan uitgegaan dat `$CLUSTERNAME` bevat de naam van het HDInsight-cluster. Ook wordt ervan uitgegaan dat [jq](https://stedolan.github.io/jq/) is geïnstalleerd. Voer desgevraagd het wachtwoord voor de aanmeldingsaccount van het cluster.

    De waarde die het resultaat is vergelijkbaar met de volgende tekst:

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]
    > Hoewel er meer dan twee Zookeeper-knooppunten, hoeft u niet te bevatten een volledige lijst van alle hosts naar clients. Een of twee is voldoende.

    Deze waarde niet opslaan omdat het wordt later gebruikt.

3. Bewerk de `dev.properties` bestand in de hoofdmap van het project. De informatie van de hosts Broker en Zookeeper toevoegen aan de overeenkomende regels in dit bestand. Het volgende voorbeeld wordt geconfigureerd met behulp van de voorbeeldwaarden uit de vorige stappen:

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

4. Sla de `dev.properties` -bestand en gebruikt u de volgende opdracht te uploaden naar de Storm-cluster:

     ```bash
    scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
    ```

    Vervang **gebruikersnaam** met de SSH-gebruikersnaam voor het cluster. Vervang **BASENAME** met de basisnaam die u hebt gebruikt bij het maken van het cluster.

## <a name="start-the-writer"></a>De writer starten

1. Gebruik de volgende verbinding maken met de Storm-cluster via SSH. Vervang **gebruikersnaam** met de SSH-gebruikersnaam gebruikt bij het maken van het cluster. Vervang **BASENAME** met de naam die wordt gebruikt bij het maken van het cluster.

  ```bash
  ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
  ```

    Wanneer u wordt gevraagd, typt u het wachtwoord die u hebt gebruikt bij het maken van clusters.
   
    Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Gebruik de volgende opdracht voor het maken van het onderwerp Kafka is gebruikt door de topologie van de SSH-verbinding:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic stormtopic --zookeeper $KAFKAZKHOSTS
    ```

    Vervang `$KAFKAZKHOSTS` host met de Zookeeper informatie die u hebt opgehaald in de vorige sectie.

2. Gebruik de volgende opdracht om te starten van de topologie van de schrijver van de SSH-verbinding met het Storm-cluster:

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    De parameters gebruikt met deze opdracht zijn:

    * `org.apache.storm.flux.Flux`: Gebruik lichtstroom configureren en uitvoeren van deze topologie.

    * `--remote`: De topologie Nimbus verzenden. De topologie wordt verdeeld over de worker-knooppunten in het cluster.

    * `-R /writer.yaml`: Gebruik de `writer.yaml` bestand voor het configureren van de topologie. `-R`Hiermee wordt aangegeven dat deze bron wordt opgenomen in het jar-bestand. Het is in de hoofdmap van de jar dus `/writer.yaml` is het pad naar het.

    * `--filter`: Vullen vermeldingen in de `writer.yaml` topologie met behulp van de waarden in de `dev.properties` bestand. Bijvoorbeeld: de waarde van de `kafka.topic` vermelding in het bestand wordt gebruikt ter vervanging van de `${kafka.topic}` vermelding in de definitie van de topologie.

5. Zodra de topologie is gestart, gebruikt u de volgende opdracht om te verifiëren dat het wegschrijven van gegevens naar het onderwerp Kafka:

  ```bash
  /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtopic
  ```

    Vervang `$KAFKAZKHOSTS` host met de Zookeeper informatie die u hebt opgehaald in de vorige sectie.

    Met deze opdracht maakt gebruik van een script met Kafka verzonden voor het bewaken van het onderwerp. Na korte tijd, moet deze eerst retourneren willekeurig zinnen dat is geschreven naar het onderwerp. De uitvoer lijkt op die in het volgende voorbeeld:

        i am at two with nature             
        an apple a day keeps the doctor away
        snow white and the seven dwarfs     
        the cow jumped over the moon        
        an apple a day keeps the doctor away
        an apple a day keeps the doctor away
        the cow jumped over the moon        
        an apple a day keeps the doctor away
        an apple a day keeps the doctor away
        four score and seven years ago      
        snow white and the seven dwarfs     
        snow white and the seven dwarfs     
        i am at two with nature             
        an apple a day keeps the doctor away

    Gebruik Ctrl + c drukken script wordt beëindigd.

## <a name="start-the-reader"></a>Start de lezer

1. Gebruik de volgende opdracht om te starten van de topologie van de lezer van de SSH-sessie met het Storm-cluster:

  ```bash
  storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
  ```

2. Zodra de topologie wordt gestart, opent u de Storm-gebruikersinterface. Dit web UI bevindt zich op `https://storm-BASENAME.azurehdinsight.net/stormui`. Vervang __BASENAME__ met de naam die wordt gebruikt als het cluster is gemaakt. 

    Wanneer u wordt gevraagd, gebruikt u de aanmeldingsnaam voor de beheerder (standaard `admin`) en het wachtwoord als het cluster is gemaakt. Er is een webpagina die vergelijkbaar is met de volgende afbeelding:

    ![Storm-gebruikersinterface](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. Selecteer in de gebruikersinterface van Storm de __kafka-lezer__ koppelen de __Topology Summary__ sectie informatie weergeven over de __kafka-lezer__ topologie.

    ![Samenvatting van de Storm-webgebruikersinterface topologie](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. Om weer te geven informatie over de exemplaren van het onderdeel berichtenlogboek bolt, selecteer de __berichtenlogboek bolt__ koppelen de __Bolts (alle tijd)__ sectie.

    ![Berichtenlogboek bolt koppeling in de sectie bolts](./media/hdinsight-apache-storm-with-kafka/bolts.png)

5. In de __Executor__ sectie, selecteert u een koppeling in de __poort__ kolom logboekregistratie informatie over dit exemplaar van het onderdeel weergeven.

    ![Executor koppelen](./media/hdinsight-apache-storm-with-kafka/executors.png)

    Het logboek bevat een logboek van de gegevens die in het onderwerp Kafka gelezen. De informatie in het logboek is vergelijkbaar met de volgende tekst:

        2016-11-04 17:47:14.907 c.m.e.LoggerBolt [INFO] Received data: four score and seven years ago
        2016-11-04 17:47:14.907 STDIO [INFO] the cow jumped over the moon
        2016-11-04 17:47:14.908 c.m.e.LoggerBolt [INFO] Received data: the cow jumped over the moon
        2016-11-04 17:47:14.911 STDIO [INFO] snow white and the seven dwarfs
        2016-11-04 17:47:14.911 c.m.e.LoggerBolt [INFO] Received data: snow white and the seven dwarfs
        2016-11-04 17:47:14.932 STDIO [INFO] snow white and the seven dwarfs
        2016-11-04 17:47:14.932 c.m.e.LoggerBolt [INFO] Received data: snow white and the seven dwarfs
        2016-11-04 17:47:14.969 STDIO [INFO] an apple a day keeps the doctor away
        2016-11-04 17:47:14.970 c.m.e.LoggerBolt [INFO] Received data: an apple a day keeps the doctor away

## <a name="stop-the-topologies"></a>De topologieën stoppen

Gebruik de volgende opdrachten om te stoppen van de Storm-topologieën van een SSH-sessie met het Storm-cluster:

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="delete-the-cluster"></a>Het cluster verwijderen

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Aangezien de stappen in dit document beide clusters in de groep met dezelfde Azure-resource maakt, kunt u de resourcegroep in de Azure portal kunt verwijderen. De resourcegroep verwijdert, worden alle resources die zijn gemaakt op basis van dit document.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer voorbeeldtopologieën die kunnen worden gebruikt met Storm op HDInsight [voorbeeld Storm-topologieën en -onderdelen](storm/apache-storm-example-topology.md).

Zie voor meer informatie over de implementatie en controle-topologieën op Linux gebaseerde HDInsight [implementeren en beheren van Apache Storm-topologieën op HDInsight op basis van Linux](storm/apache-storm-deploy-monitor-topology-linux.md)