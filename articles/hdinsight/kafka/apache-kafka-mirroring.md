---
title: Onderwerpen over Apache Kafka - Azure HDInsight spiegelen
description: Informatie over het gebruik van Apache Kafka mirroring-functie voor het onderhouden van een replica van een Kafka in HDInsight-cluster door het spiegelen van de onderwerpen in een secundaire cluster.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/24/2019
ms.openlocfilehash: bdc393d041bd40fd27493ccc8f3c4f39adfa35b2
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657167"
---
# <a name="use-mirrormaker-to-replicate-apache-kafka-topics-with-kafka-on-hdinsight"></a>MirrorMaker gebruiken voor het Apache Kafka-onderwerpen repliceren met Kafka in HDInsight

Informatie over het gebruik van Apache Kafka mirroring functie onderwerpen repliceren naar een secundaire cluster. Spiegelen kan worden uitgevoerd als een continu proces of soms gebruikt als een methode van de migratie van gegevens van het ene cluster naar een andere.

In dit voorbeeld wordt spiegelen gebruikt voor het repliceren van onderwerpen tussen twee clusters met HDInsight. Beide clusters zich in verschillende virtuele netwerken in verschillende datacenters.

> [!WARNING]  
> Spiegeling moet niet worden beschouwd als een manier om fouttolerantie. De verschuiving naar items in een onderwerp verschillen tussen de primaire en secundaire clusters, zodat clients kunnen niet beide worden door elkaar gebruikt.
>
> Als u zich zorgen maken over-fouttolerantie gebruikt, moet u replicatie voor de onderwerpen in uw cluster instellen. Zie voor meer informatie, [aan de slag met Apache Kafka in HDInsight](apache-kafka-get-started.md).

## <a name="how-apache-kafka-mirroring-works"></a>Hoe Apache Kafka spiegelen werkt

Spiegelen werkt met behulp van de [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) hulpprogramma (onderdeel van Apache Kafka) naar de records van de onderwerpen op het primaire cluster gebruiken en maak vervolgens een lokale kopie op de secundaire-cluster. MirrorMaker maakt gebruik van een (of meer) *consumenten* die gelezen uit de primaire cluster en een *producent* die worden geschreven naar het lokale cluster (secundair).

De instellingen van de handigste mirroring voor herstel na noodgevallen maakt gebruik van Kafka-clusters in verschillende Azure-regio's. Om dit te bereiken, worden de virtuele netwerken waarbij de clusters bevinden zich bij elkaar gekoppeld.

Het volgende diagram illustreert het proces voor mirroring en hoe de communicatie stroomt tussen clusters:

![Diagram van de mirroring-proces](./media/apache-kafka-mirroring/kafka-mirroring-vnets2.png)

De primaire en secundaire clusters kunnen afwijken van het aantal knooppunten en partities en verschuivingen in de onderwerpen zijn ook andere. De waarde van de sleutel die wordt gebruikt voor het partitioneren, spiegelen worden bijgehouden, zodat de volgorde van de records wordt behouden op basis van de per-sleutel.

### <a name="mirroring-across-network-boundaries"></a>Spiegelen van de grenzen van netwerken

Als u spiegelen tussen Kafka-clusters in verschillende netwerken wilt, zijn er de volgende aanvullende overwegingen:

* **Gateways**: De netwerken moeten kunnen communiceren op het TCP/IP-niveau.

* **Server-adressering**: U kunt kiezen om uw clusterknooppunten met behulp van hun IP-adressen of de volledig gekwalificeerde domeinnamen.

    * **IP-adressen**: Als u uw Kafka-clusters voor het gebruik van IP-adres reclame configureert, kunt u doorgaan met de mirroring installatie met behulp van de IP-adressen van de broker-knooppunten en de zookeeper-knooppunten.
    
    * **Domeinnamen**: Als u uw Kafka-clusters voor reclame voor IP-adres niet configureert, moeten de clusters kunnen met elkaar verbinden met behulp van de volledig gekwalificeerde domeinnamen (FQDN's) zijn. Dit is een Domain Name System (DNS)-server in elk netwerk dat is geconfigureerd om aanvragen doorsturen naar de andere netwerken vereist. Bij het maken van een Azure-netwerk, in plaats van de automatische DNS-server opgegeven met het netwerk, moet u een aangepaste DNS-server en het IP-adres voor de server opgeven. Nadat het Virtueelnetwerk is gemaakt, moet u vervolgens een Azure-Machine die gebruikmaakt van dit IP-adres, maakt en vervolgens installeren en configureren van DNS-software op het.

    > [!WARNING]  
    > Maak en configureer de aangepaste DNS-server voor de installatie van HDInsight in het Virtueelnetwerk. Er is geen aanvullende configuratie vereist voor HDInsight de DNS-server geconfigureerd voor het Virtueelnetwerk te gebruiken.

Zie voor meer informatie over het verbinden van twee virtuele netwerken van Azure, [een VNet-naar-VNet-verbinding configureren](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

## <a name="mirroring-architecture"></a>Architectuur spiegelen

Deze architectuur biedt twee clusters in verschillende resourcegroepen en virtuele netwerken: een **primaire** en **secundaire**.

### <a name="creation-steps"></a>Stappen voor het maken

1. Maak twee nieuwe resourcegroepen:

    |Resourcegroep | Location |
    |---|---|
    | kafka-primary-rg | US - centraal |
    | kafka-secondary-rg | US - noord-centraal |

1. Een nieuw virtueel netwerk maken **kafka-primaire-vnet** in **kafka-primaire-rg**. Behoud de standaardinstellingen.
1. Een nieuw virtueel netwerk maken **kafka-secundaire-vnet** in **kafka-secundaire-rg**, ook met de standaardinstellingen.

1. Twee nieuwe Kafka-clusters maken:

    | Clusternaam | Resourcegroep | Virtueel netwerk | Opslagaccount |
    |---|---|---|---|
    | kafka-primary-cluster | kafka-primary-rg | kafka-primary-vnet | kafkaprimarystorage |
    | kafka-secondary-cluster | kafka-secondary-rg | kafka-secondary-vnet | kafkasecondarystorage |

1. Peerings van virtuele netwerken maken. Deze stap maakt u twee peerings: één van **kafka-primaire-vnet** naar **kafka-secundaire-vnet** en een back-ups maken van **kafka-secundaire-vnet** naar  **kafka-primaire-vnet**.
    1. Selecteer de **kafka-primaire-vnet** virtueel netwerk.
    1. Klik op **Peerings** onder **instellingen**.
    1. Klik op **Toevoegen**.
    1. Op de **peering toevoegen** scherm, voert u de details zoals wordt weergegeven in de onderstaande schermafbeelding.

        ![vnet-peering toevoegen](./media/apache-kafka-mirroring/add-vnet-peering.png)

1. Configureer IP-advertenties:
    1. Ga naar de Ambari-dashboard voor de primaire cluster: `https://PRIMARYCLUSTERNAME.azurehdinsight.net`.
    1. Klik op **Services** > **Kafka**. Klik op de **Peeringconfiguraties** tabblad.
    1. De volgende configuratie regels toevoegen aan de onderkant **kafka-env sjabloon** sectie. Klik op **Opslaan**.
    
        ```
        # Configure Kafka to advertise IP addresses instead of FQDN
        IP_ADDRESS=$(hostname -i)
        echo advertised.listeners=$IP_ADDRESS
        sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
        echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
        ```

    1. Voer een opmerking op de **configuratie opslaan** scherm en klikt u op **opslaan**.
    1. Als u met de volgende Configuratiewaarschuwing wordt gevraagd, klikt u op **toch doorgaan**.
    1. Klik op **Ok** op de **configuratiewijzigingen opslaan**.
    1. Klik op **opnieuw** > **start opnieuw op alle betrokken** in de **opnieuw opstarten is vereist** melding. Klik op **opnieuw starten bevestigen alle**.

        ![opnieuw opstarten van kafka-knooppunten](./media/apache-kafka-mirroring/ambari-restart-notification.png)

1. Kafka om te luisteren op alle netwerkinterfaces configureren.
    1. Blijf op de **Peeringconfiguraties** tabblad onder **Services** > **Kafka**. In de **Kafka-Broker** sectie set de **listeners** eigenschap `PLAINTEXT://0.0.0.0:9092`.
    1. Klik op **Opslaan**.
    1. Klik op **opnieuw**, en **opnieuw starten bevestigen alle**.

1. Noteer de Broker-IP-adressen en Zookeeper-adressen voor de primaire-cluster.
    1. Klik op **Hosts** op de Ambari-dashboard.
    1. Noteer de IP-adressen voor de Brokers en Zookeepers. De broker-knooppunten hebben **omlaag** als de eerste twee letters van de naam van de host en de zookeeper-knooppunten hebben **zk** als de eerste twee letters van de naam van de host.

        ![ip-adressen weergeven](./media/apache-kafka-mirroring/view-node-ip-addresses2.png)

1. Herhaal de vorige drie stappen voor het tweede cluster **kafka-secundaire-cluster**: configureren van IP-reclame, listeners instellen en noteer de Broker en Zookeeper-IP-adressen.

## <a name="create-topics"></a>Help-onderwerpen maken

1. Verbinding maken met de **primaire** cluster via SSH:

    ```bash
    ssh sshuser@PRIMARYCLUSTER-ssh.azurehdinsight.net
    ```

    Vervang **sshuser** met de naam van de SSH-gebruiker die wordt gebruikt bij het maken van het cluster. Vervang **BASENAME** met de naam die wordt gebruikt bij het maken van het cluster.

    Zie [SSH-sleutels gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Gebruik de volgende opdracht om te maken van een variabele met de Apache Zookeeper-hosts voor de primaire cluster. De tekenreeksen, zoals `ZOOKEEPER_IP_ADDRESS1` moeten worden vervangen door de werkelijke IP-adressen, worden eerder hebt genoteerd, zoals `10.23.0.11` en `10.23.0.7`. Als u van de resolutie van de FQDN-naam met een aangepaste DNS-server gebruikmaakt, voert u de [stappen](apache-kafka-get-started.md#getkafkainfo) om op te halen broker en zookeeper namen.:

    ```bash
    # get the zookeeper hosts for the primary cluster
    export PRIMARY_ZKHOSTS='ZOOKEEPER_IP_ADDRESS1:2181, ZOOKEEPER_IP_ADDRESS2:2181, ZOOKEEPER_IP_ADDRESS3:2181'
    ```

3. Een onderwerp met de naam `testtopic`, gebruikt u de volgende opdracht:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $PRIMARY_ZKHOSTS
    ```

3. Gebruik de volgende opdracht uit om te controleren of het onderwerp is gemaakt:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $PRIMARY_ZKHOSTS
    ```

    Het antwoord bevat `testtopic`.

4. Gebruik de volgende om de hostinformatie van Zookeeper voor deze weer te geven (de **primaire**) cluster:

    ```bash
    echo $PRIMARY_ZKHOSTS
    ```

    Hiermee wordt informatie weergegeven die vergelijkbaar is met de volgende tekst:

    `10.23.0.11:2181,10.23.0.7:2181,10.23.0.9:2181`

    Deze gegevens worden opgeslagen. Het wordt gebruikt in de volgende sectie.

## <a name="configure-mirroring"></a>Configureer het spiegelen

1. Verbinding maken met de **secundaire** cluster met behulp van een andere SSH-sessie:

    ```bash
    ssh sshuser@SECONDARYCLUSTER-ssh.azurehdinsight.net
    ```

    Vervang **sshuser** met de naam van de SSH-gebruiker die wordt gebruikt bij het maken van het cluster. Vervang **SECONDARYCLUSTER** met de naam die wordt gebruikt bij het maken van het cluster.

    Zie [SSH-sleutels gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Een `consumer.properties` bestand wordt gebruikt voor het configureren van de communicatie met de **primaire** cluster. Gebruik de volgende opdracht voor het maken van het bestand:

    ```bash
    nano consumer.properties
    ```

    Gebruik de volgende tekst als de inhoud van de `consumer.properties` bestand:

    ```yaml
    zookeeper.connect=PRIMARY_ZKHOSTS
    group.id=mirrorgroup
    ```

    Vervang **PRIMARY_ZKHOSTS** met de Zookeeper-IP-adressen van de **primaire** cluster.

    Dit bestand beschrijft de consument informatie over het gebruik bij het lezen van het primaire Kafka-cluster. Zie voor meer informatie consumer-configuratie, [consument configuraties](https://kafka.apache.org/documentation#consumerconfigs) op kafka.apache.org.

    Gebruiken om het bestand hebt opgeslagen, **Ctrl + X**, **Y**, en vervolgens **Enter**.

3. Voordat u configureert de producent die communiceert met het tweede cluster, instellen van een variabele voor de broker-IP-adressen van de **secundaire** cluster. Gebruik de volgende opdrachten om te maken van deze variabele:

    ```bash
    export SECONDARY_BROKERHOSTS='BROKER_IP_ADDRESS1:9092,BROKER_IP_ADDRESS2:9092,BROKER_IP_ADDRESS2:9092'
    ```

    De opdracht `echo $SECONDARY_BROKERHOSTS` geretourneerde informatie moet er die vergelijkbaar is met de volgende tekst:

    `10.23.0.14:9092,10.23.0.4:9092,10.23.0.12:9092`

4. Een `producer.properties` wordt gebruikt om te communiceren de **secundaire** cluster. Gebruik de volgende opdracht voor het maken van het bestand:

    ```bash
    nano producer.properties
    ```

    Gebruik de volgende tekst als de inhoud van de `producer.properties` bestand:

    ```yaml
    bootstrap.servers=SECONDARY_BROKERHOSTS
    compression.type=none
    ```

    Vervang **SECONDARY_BROKERHOSTS** met de broker-IP-adressen die worden gebruikt in de vorige stap.

    Zie voor meer informatie producent-configuratie, [producent configuraties](https://kafka.apache.org/documentation#producerconfigs) op kafka.apache.org.

5. Gebruik de volgende opdrachten om te maken van een omgevingsvariabele met de IP-adressen van de Zookeeper-hosts voor de secundaire cluster:

    ```bash
    # get the zookeeper hosts for the secondary cluster
    export SECONDARY_ZKHOSTS='ZOOKEEPER_IP_ADDRESS1:2181,ZOOKEEPER_IP_ADDRESS2:2181,ZOOKEEPER_IP_ADDRESS3:2181'
    ```

7. De standaardconfiguratie voor Kafka in HDInsight is niet toegestaan voor het automatisch maken van onderwerpen. Voordat u begint met het proces spiegelen moet u een van de volgende opties gebruiken:

    * **Maken van de onderwerpen op het secundaire cluster**: Deze optie kunt u het aantal partities en de replicatie van meerdere factoren instellen.

        U kunt onderwerpen tevoren maken met behulp van de volgende opdracht uit:

        ```bash
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SECONDARY_ZKHOSTS
        ```

        Vervang `testtopic` met de naam van het onderwerp te maken.

    * **Configureren van het cluster voor het maken van automatische onderwerp**: Deze optie kunt MirrorMaker automatisch maken van onderwerpen, maar deze met een verschillend aantal partities of replicatiefactor dan de primaire onderwerp maken kan.

        Als u wilt configureren voor het automatisch maken van onderwerpen de secundaire cluster, moet u deze stappen uitvoeren:

        1. Ga naar de Ambari-dashboard voor het tweede cluster: `https://SECONDARYCLUSTERNAME.azurehdinsight.net`.
        1. Klik op **Services** > **Kafka**. Klik op de **Peeringconfiguraties** tabblad.
        5. In de __Filter__ veld, voert u een waarde van `auto.create`. Hiermee filtert u de lijst met eigenschappen en geeft de `auto.create.topics.enable` instelling.
        6. Wijzig de waarde van `auto.create.topics.enable` op true en selecteer vervolgens __opslaan__. Een opmerking toevoegen en selecteer vervolgens __opslaan__ opnieuw.
        7. Selecteer de __Kafka__ service en selecteer __opnieuw__, en selecteer vervolgens __opnieuw alle betrokken__. Wanneer u hierom wordt gevraagd, selecteert u __bevestigen opnieuw alle__.

        ![configureren van onderwerp automatisch maken](./media/apache-kafka-mirroring/kafka-enable-auto-create-topics.png)

## <a name="start-mirrormaker"></a>MirrorMaker starten

1. Van de SSH-verbinding met de **secundaire** cluster, gebruikt u de volgende opdracht uit om de MirrorMaker-proces te starten:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    De parameters die in dit voorbeeld zijn:

    * **--consumer.config**: Hiermee geeft u het bestand dat consumenten-eigenschappen bevat. Deze eigenschappen worden gebruikt voor het maken van een gebruiker die leest uit de *primaire* Kafka-cluster.

    * **--producer.config**: Hiermee geeft u het bestand dat de producent-eigenschappen bevat. Deze eigenschappen worden gebruikt voor het maken van een producent die naar schrijft de *secundaire* Kafka-cluster.

    * **--lijst met toegestane adressen**: Een lijst met onderwerpen die MirrorMaker worden gerepliceerd uit de primaire cluster naar de secundaire server.

    * **--num.streams**: Het aantal threads van de consument te maken.

    De gebruiker op het secundaire knooppunt is nu wachten om berichten te ontvangen.

2. Van de SSH-verbinding met de **primaire** cluster, gebruikt u de volgende opdracht uit om te starten van een producent en berichten verzenden naar het onderwerp:

    ```bash
    export PRIMARY_BROKERHOSTS=BROKER_IP_ADDRESS1:9092,BROKER_IP_ADDRESS2:9092,BROKER_IP_ADDRESS2:9092
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

     Wanneer u bij een lege regel met een cursor aankomt, typt u in een paar tekstberichten. De berichten worden verzonden naar het onderwerp op de **primaire** cluster. Wanneer u klaar bent, gebruikt u **Ctrl + C** de producent-proces te beëindigen.

3. Van de SSH-verbinding met de **secundaire** cluster, gebruikt u **Ctrl + C** de MirrorMaker-proces te beëindigen. Het duurt enkele seconden om het proces te beëindigen. Om te controleren dat de berichten zijn gerepliceerd naar de secundaire, gebruik de volgende opdracht:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $SECONDARY_ZKHOSTS --topic testtopic --from-beginning
    ```

    De lijst met onderwerpen bevat nu `testtopic`, die wordt gemaakt wanneer MirrorMaster komt overeen met het onderwerp van het primaire cluster naar de secundaire server. De berichten die worden opgehaald van het onderwerp zijn hetzelfde als de computer die u hebt ingevoerd op de primaire-cluster.

## <a name="delete-the-cluster"></a>Het cluster verwijderen

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

De stappen in dit document clusters in verschillende Azure-resourcegroepen hebt gemaakt. Als u wilt verwijderen van alle resources die zijn gemaakt, kunt u de twee resourcegroepen die zijn gemaakt verwijderen: **kafka-primaire-rg** en **kafka-secondary_rg**. De resourcegroepen verwijdert alle resources hebt gemaakt op basis van dit document, inclusief clusters, virtuele netwerken en opslagaccounts.

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) te maken van een replica van een [Apache Kafka](https://kafka.apache.org/) cluster. Gebruik de volgende koppelingen voor het detecteren van andere manieren om te werken met Kafka:

* [Documentatie voor Apache Kafka MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) op cwiki.apache.org.
* [Aan de slag met Apache Kafka in HDInsight](apache-kafka-get-started.md)
* [Apache Spark gebruiken met Apache Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Apache Storm gebruiken met Apache Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Verbinding maken met Apache Kafka via een Azure-netwerk](apache-kafka-connect-vpn-gateway.md)
