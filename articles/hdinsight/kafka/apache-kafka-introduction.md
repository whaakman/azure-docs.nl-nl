---
title: Inleiding tot Apache Kafka in HDInsight - Azure | Microsoft Docs
description: 'Meer informatie over Apache Kafka in HDInsight: wat het is, wat het doet en waar u voorbeelden en gegevens kunt vinden om aan de slag te gaan.'
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: f284b6e3-5f3b-4a50-b455-917e77588069
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: larryfr
ms.openlocfilehash: 945b16553d56d5138b17e7768e43a298b310551d
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="introducing-apache-kafka-on-hdinsight"></a>Inleiding tot Apache Kafka in HDInsight

[Apache Kafka](https://kafka.apache.org) is een open-source gedistribueerd streamingplatform dat kan worden gebruikt voor het bouwen van pijplijnen en toepassingen voor realtime streaming van gegevens. Kafka biedt ook berichtenbrokerfunctionaliteit vergelijkbaar met een berichtenwachtrij, waarmee u benoemde gegevensstromen kunt publiceren en zich erop kunt abonneren. Kafka in HDInsight biedt u een beheerde, zeer schaalbare en maximaal beschikbare service in Microsoft Azure Cloud.

## <a name="why-use-kafka-on-hdinsight"></a>Waarom Kafka op HDInsight gebruiken?

Kafka op HDInsight biedt de volgende functies:

* __99% Service Level Agreement (SLA) op Kafka uptime__: zie voor meer informatie het document [SLA-gegevens voor HDInsight](https://azure.microsoft.com/support/legal/sla/hdinsight/v1_0/).

* __Fouttolerantie en herkenning van racks__: Kafka is ontworpen met een eendimensionale weergave van een rack wat voor bepaalde omgevingen goed werkt. In omgevingen zoals Azure wordt een rack echter onderverdeeld in twee dimensies: updatedomeinen (UD's) en foutdomeinen (FD's). Microsoft biedt hulpprogramma's die zorgen voor een herverdeling van Kafka-partities en -replica's op UD’s en FD’s. 

    Zie [Hoge beschikbaarheid met Kafka op HDInsight](apache-kafka-high-availability.md) voor meer informatie.

* **Integratie met Azure Managed Disks**: beheerde schijven bieden een hogere schaal en doorvoer voor de schijven die door Kafka worden gebruikt op HDInsight, tot 16 TB per knooppunt in het cluster.

    Zie [Increase scalability of Kafka on HDInsight](apache-kafka-scalability.md) (Schaalbaarheid verhogen van Kafka in HDInsight) voor informatie over het configureren van beheerde schijven met Kafka in HDInsight.

    Zie [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md) voor meer informatie over beheerde schijven.

* **Waarschuwingen, controle en predictief onderhoud**: Azure Log Analytics kan worden gebruikt om Kafka op HDInsight te controleren. Log Analytics haalt informatie op VM-niveau naar boven, zoals schijf- en NIC-gegevens en JMX-gegevens van Kafka.

    Zie [Logboeken voor Kafka op HDInsight analyseren](apache-kafka-log-analytics-operations-management.md) voor meer informatie.

* **Replicatie van Kafka-gegevens**: Kafka biedt het hulpprogramma MirrorMaker, waarmee gegevens worden gerepliceerd tussen Kafka-clusters.

    Zie [Kafka-onderwerpen repliceren met Kafka op HDInsight](apache-kafka-mirroring.md) voor informatie over het gebruik van MirrorMaker.

* **Clusterschaling**: met HDInsight kunt u het aantal worker-knooppunten (waarop de Kafka-broker wordt gehost) wijzigen na het maken van het cluster. Een cluster omhoog schalen als de werkbelasting toeneemt of omlaag schalen om de kosten te verlagen. Schalen kan worden uitgevoerd vanuit Azure Portal, Azure PowerShell en andere interfaces voor Azure-beheer. Voor Kafka moet u partitiereplica's na het schalen herverdelen. Door herverdeling van partities kan Kafka gebruikmaken van het nieuwe aantal worker-knooppunten.

    Zie [Hoge beschikbaarheid met Kafka op HDInsight](apache-kafka-high-availability.md) voor meer informatie.

* **Publicatie-/abonnementsberichten**: Kafka biedt een producent-API voor het publiceren van records naar een Kafka-onderwerp. De consument-API wordt gebruikt bij het abonneren op een onderwerp.

    Zie [Starten met Kafka op HDInsight](apache-kafka-get-started.md) voor meer informatie.

* **Streamverwerking**: Kafka wordt vaak gebruikt met Apache Storm of Spark voor streamverwerking in realtime. In Kafka 0.10.0.0 (HDInsight-versie 3.5 en 3.6) werd een streaming-API geïntroduceerd waarmee u streamingoplossingen kunt maken zonder Storm of Spark.

    Zie [Starten met Kafka op HDInsight](apache-kafka-get-started.md) voor meer informatie.

* **Horizontale schaal**: Kafka partitioneert streams op de knooppunten in het HDInsight-cluster. Consumentenprocessen kunnen worden gekoppeld aan afzonderlijke partities voor een evenwichtige taakverdeling bij het gebruiken van records.

    Zie [Starten met Kafka op HDInsight](apache-kafka-get-started.md) voor meer informatie.

* **Levering op volgorde**: binnen elke partitie worden records in de stream opgeslagen in de volgorde waarin ze zijn ontvangen. Door één consumentenproces aan een partitie te koppelen, kunt u garanderen dat de records in de juiste volgorde worden verwerkt.

    Zie [Starten met Kafka op HDInsight](apache-kafka-get-started.md) voor meer informatie.

## <a name="use-cases"></a>Gebruiksvoorbeelden

* **Berichten**: omdat het publicatie-/abonnementspatroon voor berichten wordt ondersteund, wordt Kafka vaak gebruikt als berichtenbroker.

* **Activiteitsregistratie**: omdat Kafka records registreert in de volgorde waarin ze binnenkomen, kan dit worden gebruikt om activiteiten bij te houden en opnieuw te maken. Bijvoorbeeld gebruikersacties op een website of in een toepassing.

* **Aggregatie**: met streamverwerking kunt u de gegevens uit de verschillende streams combineren en samenvoegen in operationele gegevens.

* **Transformatie**: met streamverwerking kunt u de gegevens uit meerdere invoeronderwerpen combineren en vertalen naar één of meer uitvoeronderwerpen.

## <a name="architecture"></a>Architectuur

![Kafka-clusterconfiguratie](./media/apache-kafka-introduction/kafka-cluster.png)

Dit diagram toont een typische Kafka-configuratie die gebruikmaakt van de consumentgroepen, partitionering en replicatie om het parallel lezen van gebeurtenissen met fouttolerantie mogelijk te maken. Apache ZooKeeper is gebouwd voor gelijktijdige, robuuste transacties met een lage latentie voor het beheren van de status van het Kafka-cluster. Kafka slaat records op in *onderwerpen*. Records worden geproduceerd door *producenten* en worden gebruikt door *consumenten*. Producenten halen records op uit Kafka-*brokers*. Elk werkrolknooppunt in uw HDInsight-cluster is een Kafka-broker. Voor elke consument wordt één partitie gemaakt, zodat de streaminggegevens parallel kunnen worden verwerkt. Replicatie wordt gebruikt om de partities over knooppunten te verdelen, zodat er beveiliging is tegen storingen van knooppunten (broker). Een partitie die is gemarkeerd met een *(L)* is de leidende partitie voor de opgegeven partitie. Producentverkeer wordt doorgestuurd naar de leider van elk knooppunt, met behulp van de status die wordt beheerd door ZooKeeper.

Elke Kafka-broker maakt gebruik van Azure Managed Disks. Het aantal schijven is per gebruiker gedefinieerd en kan maximaal 16 TB aan opslag per broker bieden.

> [!IMPORTANT]
> Kafka is niet op de hoogte van de onderliggende hardware (rack) in het Azure-datacentrum. Om ervoor te zorgen dat partities correct worden verdeeld over de onderliggende hardware raadpleegt u het document [Hoge beschikbaarheid configureren (Kafka)](apache-kafka-high-availability.md).

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende koppelingen voor meer informatie over het gebruik van Apache Kafka in HDInsight:

* [Aan de slag met Kafka in HDInsight](apache-kafka-get-started.md)

* [MirrorMaker gebruiken voor het maken van een replica van Kafka in HDInsight](apache-kafka-mirroring.md)

* [Apache Storm gebruiken met Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)

* [Apache Spark gebruiken met Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)

* [Verbinding maken met Kafka via een Azure Virtual Network](apache-kafka-connect-vpn-gateway.md)