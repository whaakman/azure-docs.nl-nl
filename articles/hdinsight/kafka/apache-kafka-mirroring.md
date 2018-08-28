---
title: Onderwerpen over Apache Kafka - Azure HDInsight spiegelen
description: Informatie over het gebruik van Apache Kafka mirroring-functie voor het onderhouden van een replica van een Kafka in HDInsight-cluster door het spiegelen van de onderwerpen in een secundaire cluster.
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.openlocfilehash: 9f8204eb29566aaae9170c21b8dde33aa3178384
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43042700"
---
# <a name="use-mirrormaker-to-replicate-apache-kafka-topics-with-kafka-on-hdinsight"></a>MirrorMaker gebruiken voor het Apache Kafka-onderwerpen repliceren met Kafka in HDInsight

Informatie over het gebruik van Apache Kafka mirroring functie onderwerpen repliceren naar een secundaire cluster. Spiegelen kan worden uitgevoerd als een continu proces of soms gebruikt als een methode van de migratie van gegevens van het ene cluster naar een andere.

In dit voorbeeld wordt spiegelen gebruikt voor het repliceren van onderwerpen tussen twee clusters met HDInsight. Beide clusters zich in een Azure-netwerk in dezelfde regio.

> [!WARNING]
> Spiegeling moet niet worden beschouwd als een manier om fouttolerantie. De verschuiving naar items in een onderwerp verschillen tussen de bron- en -clusters, zodat clients kunnen niet beide worden door elkaar gebruikt.
>
> Als u zich zorgen maken over-fouttolerantie gebruikt, moet u replicatie voor de onderwerpen in uw cluster instellen. Zie voor meer informatie, [aan de slag met Kafka in HDInsight](apache-kafka-get-started.md).

## <a name="how-kafka-mirroring-works"></a>De werking van Kafka spiegelen

Werkt met het hulpprogramma MirrorMaker (onderdeel van Apache Kafka) te spiegelen records van de onderwerpen in de broncluster gebruiken en vervolgens maakt u een lokale kopie op het doelcluster. MirrorMaker maakt gebruik van een (of meer) *consumenten* die van het broncluster gelezen en een *producent* die worden geschreven naar het lokale (doel)-cluster.

Het volgende diagram illustreert het proces spiegelen:

![Diagram van de mirroring-proces](./media/apache-kafka-mirroring/kafka-mirroring.png)

Apache Kafka in HDInsight biedt geen toegang tot de Kafka-service via het openbare internet. Kafka producenten en consumenten moeten zich in dezelfde Azure virtual network als de knooppunten in het Kafka-cluster. In dit voorbeeld worden zowel de Kafka-bron- en de clusters bevinden zich in een Azure-netwerk. Het volgende diagram toont hoe de communicatie tussen de clusters met stromen:

![Diagram van de bron- en Kafka-clusters in een Azure-netwerk](./media/apache-kafka-mirroring/spark-kafka-vnet.png)

De bron- en -clusters kunnen afwijken van het aantal knooppunten en partities en verschuivingen in de onderwerpen zijn ook andere. De waarde van de sleutel die wordt gebruikt voor het partitioneren, spiegelen worden bijgehouden, zodat de volgorde van de records wordt behouden op basis van de per-sleutel.

### <a name="mirroring-across-network-boundaries"></a>Spiegelen van de grenzen van netwerken

Als u spiegelen tussen Kafka-clusters in verschillende netwerken wilt, zijn er de volgende aanvullende overwegingen:

* **Gateways**: de netwerken moeten kunnen communiceren op het TCP/IP-niveau.

* **Naamomzetting**: de Kafka-clusters in elk netwerk moeten kunnen met elkaar verbinden met behulp van hostnamen. Dit is mogelijk een Domain Name System (DNS)-server in elk netwerk dat is geconfigureerd om aanvragen doorsturen naar de andere netwerken.

    Bij het maken van een Azure-netwerk, in plaats van de automatische DNS-server opgegeven met het netwerk, moet u een aangepaste DNS-server en het IP-adres voor de server opgeven. Nadat het Virtueelnetwerk is gemaakt, moet u vervolgens een Azure-Machine die gebruikmaakt van dit IP-adres, maakt en vervolgens installeren en configureren van DNS-software op het.

    > [!WARNING]
    > Maak en configureer de aangepaste DNS-server voor de installatie van HDInsight in het Virtueelnetwerk. Er is geen aanvullende configuratie vereist voor HDInsight de DNS-server geconfigureerd voor het Virtueelnetwerk te gebruiken.

Zie voor meer informatie over het verbinden van twee virtuele netwerken van Azure, [een VNet-naar-VNet-verbinding configureren](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

## <a name="create-kafka-clusters"></a>Kafka-clusters maken

Hoewel u een Azure virtual network maken kunt en Kafka-clusters handmatig, is het eenvoudiger te gebruiken van een Azure Resource Manager-sjabloon. Gebruik de volgende stappen voor het implementeren van een Azure-netwerk en twee Kafka-clusters naar uw Azure-abonnement.

1. Gebruik de volgende knop om u aan te melden bij Azure en de sjabloon in de Azure Portal te openen.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    De Azure Resource Manager-sjabloon bevindt zich op **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.

    > [!WARNING]
    > Om beschikbaarheid van Kafka op HDInsight te garanderen, moet uw cluster ten minste drie werkknooppunten bevatten. Met deze sjabloon maakt u een Kafka-cluster dat drie werkknooppunten bevat.

2. Gebruik de volgende informatie voor het vullen van de vermeldingen in de **aangepaste implementatie** blade:
    
    ![Aangepaste HDInsight-implementatie](./media/apache-kafka-mirroring/parameters.png)
    
    * **Resourcegroep**: Maak een groep of Selecteer een bestaande resourcegroep. Deze groep bevat het HDInsight-cluster.

    * **Locatie**: Selecteer een locatie geografisch dicht bij u.
     
    * **Clusternaam baseren**: deze waarde wordt gebruikt als basisnaam voor de Kafka-clusters. Bijvoorbeeld, voeren **hdi** maakt clusters met de naam **bron hdi** en **dest hdi**.

    * **Gebruikersnaam voor aanmelding cluster**: de beheerdersnaam voor de bron- en Kafka-clusters.

    * **Cluster-wachtwoord voor Clusteraanmelding**: het beheerderswachtwoord voor de bron- en Kafka-clusters.

    * **SSH-gebruikersnaam**: de SSH-gebruiker maken voor de bron- en Kafka-clusters.

    * **SSH-wachtwoord**: het wachtwoord voor de SSH-gebruiker voor de bron- en Kafka-clusters.

3. Lees de **voorwaarden** en schakel vervolgens het selectievakje **Ik ga akkoord met de bovenstaande voorwaarden** in.

4. Schakel tot slot **Vastmaken aan dashboard** in en selecteer **Kopen**. Het duurt ongeveer 20 minuten om de clusters te maken.

> [!IMPORTANT]
> De naam van de HDInsight-clusters zijn **bron BASENAME** en **dest BASENAME**, waarbij BASENAME is de naam die u hebt opgegeven voor de sjabloon. U gebruikt deze namen in latere stappen bij het verbinden met de clusters.

## <a name="create-topics"></a>Help-onderwerpen maken

1. Verbinding maken met de **bron** cluster via SSH:

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    Vervang **sshuser** met de naam van de SSH-gebruiker die wordt gebruikt bij het maken van het cluster. Vervang **BASENAME** met de naam die wordt gebruikt bij het maken van het cluster.

    Zie [SSH-sleutels gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Gebruik de volgende opdrachten de Zookeeper-hosts vinden voor de broncluster:

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get the zookeeper hosts for the source cluster
    export SOURCE_ZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

    Vervang `$CLUSTERNAME` met de naam van de broncluster. Voer desgevraagd het wachtwoord voor het account voor clusteraanmelding (admin).

3. Een onderwerp met de naam `testtopic`, gebruikt u de volgende opdracht:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. Gebruik de volgende opdracht uit om te controleren of het onderwerp is gemaakt:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    Het antwoord bevat `testtopic`.

4. Gebruik de volgende om de hostinformatie van Zookeeper voor deze weer te geven (de **bron**) cluster:

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    Hiermee wordt informatie weergegeven die vergelijkbaar is met de volgende tekst:

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    Deze gegevens worden opgeslagen. Het wordt gebruikt in de volgende sectie.

## <a name="configure-mirroring"></a>Configureer het spiegelen

1. Verbinding maken met de **bestemming** cluster met behulp van een andere SSH-sessie:

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    Vervang **sshuser** met de naam van de SSH-gebruiker die wordt gebruikt bij het maken van het cluster. Vervang **BASENAME** met de naam die wordt gebruikt bij het maken van het cluster.

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

    Vervang **SOURCE_ZKHOSTS** met de gegevens van de Zookeeper-hosts uit de **bron** cluster.

    Dit bestand beschrijft de consument informatie over het gebruik bij het lezen van de bron van Kafka-cluster. Zie voor meer informatie consumer-configuratie, [consument configuraties](https://kafka.apache.org/documentation#consumerconfigs) op kafka.apache.org.

    Gebruiken om het bestand hebt opgeslagen, **Ctrl + X**, **Y**, en vervolgens **Enter**.

3. Voordat u configureert de producent die communiceert met het doelcluster opstart, vindt u de broker-hosts voor de **bestemming** cluster. Gebruik de volgende opdrachten om deze gegevens te halen:

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    Vervang `$CLUSTERNAME` met de naam van het doelcluster. Voer desgevraagd het wachtwoord voor het account voor clusteraanmelding (admin).

    De `echo` opdracht retourneert de gegevens die vergelijkbaar is met de volgende tekst:

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. Een `producer.properties` wordt gebruikt om te communiceren de __bestemming__ cluster. Gebruik de volgende opdracht voor het maken van het bestand:

    ```bash
    nano producer.properties
    ```

    Gebruik de volgende tekst als de inhoud van de `producer.properties` bestand:

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    Vervang **DEST_BROKERS** met de broker-gegevens uit de vorige stap.

    Zie voor meer informatie producent-configuratie, [producent configuraties](https://kafka.apache.org/documentation#producerconfigs) op kafka.apache.org.

5. Gebruik de volgende opdrachten de Zookeeper-hosts vinden voor het doelcluster:

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get the zookeeper hosts for the source cluster
    export DEST_ZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

    Vervang `$CLUSTERNAME` met de naam van het doelcluster. Voer desgevraagd het wachtwoord voor het account voor clusteraanmelding (admin).

7. De standaardconfiguratie voor Kafka in HDInsight is niet toegestaan voor het automatisch maken van onderwerpen. Voordat u begint met het proces spiegelen moet u een van de volgende opties gebruiken:

    * **Maken van de onderwerpen op de doelcluster**: deze optie kunt u het aantal partities en de replicatie van meerdere factoren instellen.

        U kunt onderwerpen tevoren maken met behulp van de volgende opdracht uit:

        ```bash
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $DEST_ZKHOSTS
        ```

        Vervang `testtopic` met de naam van het onderwerp te maken.

    * **Configureren van het cluster voor het maken van automatische onderwerp**: met deze optie kunt MirrorMaker automatisch maken van onderwerpen, maar deze met een verschillend aantal partities of replicatiefactor dan de bron-onderwerp maken kan.

        Voor het configureren van het doelcluster voor het automatisch maken van onderwerpen, moet u deze stappen uitvoeren:

        1. Uit de [Azure-portal](https://portal.azure.com), selecteert u de bestemming Kafka-cluster.
        2. Selecteer in het overzicht van de cluster __clusterdashboard__. Selecteer vervolgens __HDInsight-clusterdashboard__. Wanneer u hierom wordt gevraagd, moet u verifiëren met behulp van de referenties voor clusteraanmelding (admin) voor het cluster.
        3. Selecteer de __Kafka__ service in de lijst aan de linkerkant van de pagina.
        4. Selecteer __Peeringconfiguraties__ in het midden van de pagina.
        5. In de __Filter__ veld, voert u een waarde van `auto.create`. Hiermee filtert u de lijst met eigenschappen en geeft de `auto.create.topics.enable` instelling.
        6. Wijzig de waarde van `auto.create.topics.enable` op true en selecteer vervolgens __opslaan__. Een opmerking toevoegen en selecteer vervolgens __opslaan__ opnieuw.
        7. Selecteer de __Kafka__ service en selecteer __opnieuw__, en selecteer vervolgens __opnieuw alle betrokken__. Wanneer u hierom wordt gevraagd, selecteert u __bevestigen opnieuw alle__.

## <a name="start-mirrormaker"></a>MirrorMaker starten

1. Van de SSH-verbinding met de **bestemming** cluster, gebruikt u de volgende opdracht uit om de MirrorMaker-proces te starten:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    De parameters die in dit voorbeeld zijn:

    * **--consumer.config**: Hiermee geeft u het bestand dat consumenten-eigenschappen bevat. Deze eigenschappen worden gebruikt voor het maken van een gebruiker die leest uit de *bron* Kafka-cluster.

    * **--producer.config**: Hiermee geeft u het bestand dat de producent-eigenschappen bevat. Deze eigenschappen worden gebruikt voor het maken van een producent die naar schrijft de *bestemming* Kafka-cluster.

    * **--lijst met toegestane adressen**: een lijst met onderwerpen die MirrorMaker van het broncluster zijn gerepliceerd naar de bestemming.

    * **--num.streams**: het aantal threads van de consument te maken.

 Bij het opstarten wordt MirrorMaker informatie weergegeven die vergelijkbaar is met de volgende tekst:

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. Van de SSH-verbinding met de **bron** cluster, gebruikt u de volgende opdracht uit om te starten van een producent en berichten verzenden naar het onderwerp:

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    Vervang `$CLUSTERNAME` met de naam van de broncluster. Voer desgevraagd het wachtwoord voor het account voor clusteraanmelding (admin).

     Wanneer u bij een lege regel met een cursor aankomt, typt u in een paar tekstberichten. De berichten worden verzonden naar het onderwerp op de **bron** cluster. Wanneer u klaar bent, gebruikt u **Ctrl + C** de producent-proces te beëindigen.

3. Van de SSH-verbinding met de **bestemming** cluster, gebruikt u **Ctrl + C** de MirrorMaker-proces te beëindigen. Het duurt enkele seconden om het proces te beëindigen. Om te controleren dat de berichten zijn gerepliceerd naar de bestemming, gebruikt u de volgende opdracht uit:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    Vervang `$CLUSTERNAME` met de naam van het doelcluster. Voer desgevraagd het wachtwoord voor het account voor clusteraanmelding (admin).

    De lijst met onderwerpen bevat nu `testtopic`, die wordt gemaakt wanneer MirrorMaster komt overeen met het onderwerp van het broncluster naar de bestemming. De berichten die worden opgehaald van het onderwerp zijn hetzelfde als op het broncluster ingevoerd.

## <a name="delete-the-cluster"></a>Het cluster verwijderen

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Omdat de stappen in dit document worden beide clusters in de dezelfde Azure-resourcegroep maakt, kunt u de resourcegroep in Azure portal verwijderen. De resourcegroep verwijdert, worden alle resources hebt gemaakt op basis van dit document, Azure Virtual Network en storage-account door de clusters worden gebruikt.

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u MirrorMaker gebruiken voor het maken van een replica van een Kafka-cluster. Gebruik de volgende koppelingen voor het detecteren van andere manieren om te werken met Kafka:

* [Documentatie voor Apache Kafka MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) op cwiki.apache.org.
* [Aan de slag met Apache Kafka in HDInsight](apache-kafka-get-started.md)
* [Apache Spark gebruiken met Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Apache Storm gebruiken met Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Verbinding maken met Kafka via een Azure Virtual Network](apache-kafka-connect-vpn-gateway.md)
