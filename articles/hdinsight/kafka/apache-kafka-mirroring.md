---
title: Het spiegelen van Apache Kafka onderwerpen - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruiken van Apache Kafka mirroring-functie voor een replica van een Kafka op HDInsight-cluster door mirroring van onderwerpen aan een secundaire cluster.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 015d276e-f678-4f2b-9572-75553c56625b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/07/2017
ms.author: larryfr
ms.openlocfilehash: a7063375ac4a2f9f172b5c380c2d5472a12e1bfb
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="use-mirrormaker-to-replicate-apache-kafka-topics-with-kafka-on-hdinsight"></a>MirrorMaker gebruiken om te repliceren van Apache Kafka onderwerpen met Kafka in HDInsight

Informatie over het gebruik van Apache Kafka mirroring functie te onderwerpen worden gerepliceerd naar een secundaire cluster. Het spiegelen kan worden uitgevoerd als een continu proces of af en toe worden gebruikt als een methode voor het migreren van gegevens van de ene cluster naar een andere.

In dit voorbeeld wordt spiegelen gebruikt voor het repliceren van onderwerpen tussen twee clusters met HDInsight. Beide clusters zijn in een Azure-netwerk in dezelfde regio.

> [!WARNING]
> Spiegeling moet niet worden beschouwd als een manier om fouttolerantie. De verschuiving van de items in een onderwerp zijn de verschillen tussen de bron- en doelserver clusters, zodat clients geen gebruikmaken van de twee door elkaar.
>
> Als u zich zorgen over fouttolerantie, stelt u replicatie voor de onderwerpen in het cluster. Zie voor meer informatie [aan de slag met Kafka op HDInsight](apache-kafka-get-started.md).

## <a name="how-kafka-mirroring-works"></a>De werking van Kafka spiegelen

Spiegelen werkt met het hulpprogramma MirrorMaker (onderdeel van Apache Kafka) te verbruiken records van de onderwerpen in de broncluster en maakt vervolgens een lokale kopie op het doelcluster. MirrorMaker maakt gebruik van een (of meer) *consumenten* die gelezen uit het broncluster en een *producent* die schrijft naar het lokale (doel)-cluster.

Het volgende diagram illustreert het proces voor het spiegelen:

![Diagram van de spiegeling proces](./media/apache-kafka-mirroring/kafka-mirroring.png)

Apache Kafka op HDInsight biedt geen toegang tot de service Kafka via het openbare internet. Kafka producenten en consumenten moeten zich in hetzelfde virtuele netwerk van Azure als de knooppunten in het cluster Kafka. Voor dit voorbeeld worden zowel de Kafka-bron- en doelserver de clusters bevinden zich in een Azure-netwerk. Het volgende diagram toont hoe de communicatie tussen clusters stroomt:

![Diagram van de bron en bestemming Kafka-clusters in een Azure-netwerk](./media/apache-kafka-mirroring/spark-kafka-vnet.png)

De bron- en doelserver clusters kunnen afwijken van het aantal knooppunten en partities en verschuivingen in de onderwerpen zijn ook verschillend. De waarde van de sleutel die wordt gebruikt voor het partitioneren, mirroring worden bijgehouden, zodat de volgorde van de records op basis van de per-sleutel wordt behouden.

### <a name="mirroring-across-network-boundaries"></a>Spiegelen buiten de netwerkgrenzen van

Als u nodig hebt voor het spiegelen tussen clusters met Kafka in verschillende netwerken, zijn er de volgende aanvullende overwegingen:

* **Gateways**: de netwerken moet kunnen communiceren op het TCP/IP-niveau.

* **Naamomzetting**: de Kafka-clusters in elk netwerk moet kunnen met elkaar verbinding maken met behulp van hostnamen. Hiervoor moet mogelijk een Domain Name System (DNS) server in elk netwerk dat is geconfigureerd voor het doorsturen van aanvragen naar de andere netwerken.

    Bij het maken van een virtueel netwerk van Azure, in plaats van met automatische DNS is opgegeven met het netwerk, moet u een aangepaste DNS-server en het IP-adres voor de server opgeven. Nadat het virtuele netwerk is gemaakt, moet u vervolgens maken een Azure-virtuele Machine die wordt gebruikt dat IP-adres, en vervolgens installeren en configureren van DNS-software op deze.

    > [!WARNING]
    > Maak en configureer de aangepaste DNS-server voordat u HDInsight installeert in het virtuele netwerk. Er is geen aanvullende configuratie vereist voor HDInsight de DNS-server geconfigureerd voor het virtuele netwerk te gebruiken.

Zie voor meer informatie over het verbinden van twee virtuele netwerken van Azure [een VNet-naar-VNet-verbinding configureren](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

## <a name="create-kafka-clusters"></a>Kafka-clusters maken

Terwijl u een Azure-netwerk maken kunt en Kafka handmatig clusters, is het gemakkelijker te gebruiken van een Azure Resource Manager-sjabloon. Gebruik de volgende stappen uit een Azure-netwerk en twee Kafka-clusters implementeren in uw Azure-abonnement.

1. Gebruik de volgende knop om te melden bij Azure en opent u de sjabloon in de Azure portal.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    De Azure Resource Manager-sjabloon bevindt zich op **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.

    > [!WARNING]
    > Om beschikbaarheid van Kafka op HDInsight te garanderen, moet uw cluster ten minste drie werkknooppunten bevatten. Deze sjabloon maakt een Kafka-cluster dat drie worker-knooppunten bevat.

2. Gebruik de volgende informatie voor het vullen van de vermeldingen op de **aangepaste implementatie** blade:
    
    ![Aangepaste HDInsight-implementatie](./media/apache-kafka-mirroring/parameters.png)
    
    * **Resourcegroep**: Maak een groep of een bestaande set selecteren. Deze groep bevat het HDInsight-cluster.

    * **Locatie**: Selecteer een locatie geografisch dicht bij u.
     
    * **Clusternaam baseren**: deze waarde wordt gebruikt als de basisnaam aan voor de Kafka-clusters. Bijvoorbeeld, voeren **hdi** wordt gemaakt met de naam clusters **bron hdi** en **dest hdi**.

    * **Aanmeldingsnaam van gebruiker cluster**: de gebruikersnaam van de beheerder voor de bron- en doelserver Kafka-clusters.

    * **Aanmeldingswachtwoord cluster**: het beheerderswachtwoord voor de bron en bestemming Kafka-clusters.

    * **SSH-gebruikersnaam**: de SSH-gebruiker maken voor de bron en bestemming Kafka clusters.

    * **SSH-wachtwoord**: het wachtwoord voor de SSH-gebruiker voor de bron- en doelserver Kafka-clusters.

3. Lees de **voorwaarden en bepalingen**, en selecteer vervolgens **ik ga akkoord met de voorwaarden en bepalingen bovengenoemde**.

4. Controleer ten slotte **vastmaken aan dashboard** en selecteer vervolgens **aankoop**. Het duurt ongeveer 20 minuten om de clusters te maken.

> [!IMPORTANT]
> De naam van de HDInsight-clusters zijn **bron BASENAME** en **dest BASENAME**, waarbij BASENAME is de naam die u hebt opgegeven in de sjabloon. U kunt deze namen in latere stappen gebruiken bij het verbinden met de clusters.

## <a name="create-topics"></a>Help-onderwerpen maken

1. Verbinding maken met de **bron** cluster via SSH:

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    Vervang **sshuser** met de SSH-gebruikersnaam gebruikt bij het maken van het cluster. Vervang **BASENAME** met de naam die wordt gebruikt bij het maken van het cluster.

    Zie [SSH-sleutels gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Gebruik de volgende opdrachten de Zookeeper hosts vinden voor de broncluster:

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get the zookeeper hosts for the source cluster
    export SOURCE_ZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

    Vervang `$CLUSTERNAME` met de naam van de broncluster. Voer desgevraagd het wachtwoord voor de cluster-serviceaccount voor aanmelding (admin).

3. Maken van een onderwerp met de naam `testtopic`, gebruik de volgende opdracht:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. Gebruik de volgende opdracht om te controleren of het onderwerp is gemaakt:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    Het antwoord bevat `testtopic`.

4. Gebruik de volgende om de informatie van de host Zookeeper voor deze te bekijken (de **bron**) cluster:

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    Hierdoor wordt informatie geretourneerd vergelijkbaar met de volgende tekst:

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    Deze gegevens worden opgeslagen. Het wordt gebruikt in de volgende sectie.

## <a name="configure-mirroring"></a>Configureer het spiegelen

1. Verbinding maken met de **bestemming** cluster met behulp van een andere SSH-sessie:

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    Vervang **sshuser** met de SSH-gebruikersnaam gebruikt bij het maken van het cluster. Vervang **BASENAME** met de naam die wordt gebruikt bij het maken van het cluster.

    Zie [SSH-sleutels gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Een `consumer.properties` bestand wordt gebruikt voor het configureren van de communicatie met de **bron** cluster. Gebruik de volgende opdracht voor het maken van het bestand:

    ```bash
    nano consumer.properties
    ```

    Gebruik de volgende tekst als de inhoud van de `consumer.properties` bestand:

    ```yaml
    zookeeper.connect=SOURCE_ZKHOSTS
    group.id=mirrorgroup
    ```

    Vervang **SOURCE_ZKHOSTS** met de gegevens van de hosts Zookeeper uit de **bron** cluster.

    Dit bestand beschrijft de consumer-informatie gebruiken bij het lezen van de bron Kafka-cluster. Zie voor meer informatie consumer-configuratie [Consumer Configs](https://kafka.apache.org/documentation#consumerconfigs) op kafka.apache.org.

    Gebruiken om het bestand opslaat, **Ctrl + X**, **Y**, en vervolgens **Enter**.

3. Voordat u configureert de producent die communiceert met het doelcluster, vindt u de broker-hosts voor de **bestemming** cluster. Gebruik de volgende opdrachten voor het ophalen van deze informatie:

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    Vervang `$CLUSTERNAME` met de naam van het doelcluster. Voer desgevraagd het wachtwoord voor de cluster-serviceaccount voor aanmelding (admin).

    De `echo` opdracht retourneert informatie vergelijkbaar met de volgende tekst:

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. Een `producer.properties` bestand wordt gebruikt om te communiceren de __bestemming__ cluster. Gebruik de volgende opdracht voor het maken van het bestand:

    ```bash
    nano producer.properties
    ```

    Gebruik de volgende tekst als de inhoud van de `producer.properties` bestand:

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    Vervang **DEST_BROKERS** met de broker-informatie van de vorige stap.

    Zie voor meer informatie producent configuratie, [producent Configs](https://kafka.apache.org/documentation#producerconfigs) op kafka.apache.org.

## <a name="start-mirrormaker"></a>MirrorMaker starten

1. Van de SSH-verbinding met de **bestemming** cluster, gebruik de volgende opdracht om het proces MirrorMaker te starten:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    De parameters die in dit voorbeeld zijn:

    * **--consumer.config**: Hiermee geeft u het bestand dat de consument eigenschappen bevat. Deze eigenschappen worden gebruikt voor het maken van een consumer die leest uit de *bron* Kafka-cluster.

    * **--producer.config**: Hiermee geeft u het bestand dat de producent eigenschappen bevat. Deze eigenschappen worden gebruikt voor het maken van een producent die naar schrijft de *bestemming* Kafka-cluster.

    * **--geaccepteerde**: een lijst met onderwerpen die MirrorMaker van het broncluster zijn gerepliceerd naar de bestemming.

    * **--num.streams**: het aantal threads consumenten te maken.

 Bij het opstarten retourneert MirrorMaker informatie vergelijkbaar met de volgende tekst:

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. Van de SSH-verbinding met de **bron** cluster, de volgende opdracht gebruiken om te beginnen een producent en berichten verzenden naar het onderwerp:

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    Vervang `$CLUSTERNAME` met de naam van de broncluster. Voer desgevraagd het wachtwoord voor de cluster-serviceaccount voor aanmelding (admin).

     Wanneer u bij een lege regel met een cursor aankomt, typt u in een paar tekstberichten. De berichten worden verzonden naar het onderwerp op de **bron** cluster. Wanneer u klaar bent, gebruikt u **Ctrl + c drukken** om de producent proces te beëindigen.

3. Van de SSH-verbinding met de **bestemming** cluster, gebruikt u **Ctrl + c drukken** om de MirrorMaker proces te beëindigen. Om te controleren of het onderwerp en de berichten zijn gerepliceerd naar de bestemming, gebruik de volgende opdrachten:

    ```bash
    DEST_ZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    Vervang `$CLUSTERNAME` met de naam van het doelcluster. Voer desgevraagd het wachtwoord voor de cluster-serviceaccount voor aanmelding (admin).

    De lijst met onderwerpen bevat nu `testtopic`, die wordt gemaakt wanneer MirrorMaster komt overeen met het onderwerp van het broncluster naar de bestemming. De berichten die zijn opgehaald van het onderwerp zijn hetzelfde als in de broncluster ingevoerd.

## <a name="delete-the-cluster"></a>Het cluster verwijderen

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Aangezien de stappen in dit document beide clusters in de groep met dezelfde Azure-resource maakt, kunt u de resourcegroep in de Azure portal kunt verwijderen. De resourcegroep verwijdert, worden alle resources die zijn gemaakt op basis van dit document, Azure Virtual Network en storage-account door de clusters worden gebruikt.

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe MirrorMaker gebruiken voor het maken van een replica van een cluster Kafka. Gebruik de volgende koppelingen voor het detecteren van andere manieren om te werken met Kafka:

* [Apache Kafka MirrorMaker documentatie](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) op cwiki.apache.org.
* [Aan de slag met Apache Kafka in HDInsight](apache-kafka-get-started.md)
* [Apache Spark gebruiken met Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Apache Storm gebruiken met Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Verbinding maken met Kafka via een Azure Virtual Network](apache-kafka-connect-vpn-gateway.md)
