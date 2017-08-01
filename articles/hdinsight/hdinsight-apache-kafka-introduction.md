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
ms.date: 06/15/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: f6a49be148ded1a90a642b83d3d70a83e572f4e4
ms.contentlocale: nl-nl
ms.lasthandoff: 06/17/2017

---
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a>Inleiding tot Apache Kafka in HDInsight (preview)

[Apache Kafka](https://kafka.apache.org) is een open-source gedistribueerd streamingplatform dat kan worden gebruikt voor het bouwen van pijplijnen en toepassingen voor realtime streaming van gegevens. Kafka biedt ook berichtenbrokerfunctionaliteit vergelijkbaar met een berichtenwachtrij, waarmee u benoemde gegevensstromen kunt publiceren en zich erop kunt abonneren. Kafka in HDInsight biedt u een beheerde, zeer schaalbare en maximaal beschikbare service in Microsoft Azure Cloud.

## <a name="why-use-kafka-on-hdinsight"></a>Waarom Kafka op HDInsight gebruiken?

Kafka biedt de volgende functies:

* Publicatie-/abonnementsberichten: Kafka biedt een producent-API voor het publiceren van records naar een Kafka-onderwerp. De consument-API wordt gebruikt bij het abonneren op een onderwerp.

* Streamverwerking: Kafka wordt vaak gebruikt met Apache Storm of Spark voor streamverwerking in realtime. In Kafka 0.10.0.0 (HDInsight-versie 3.5) werd een streaming-API geïntroduceerd waarmee u streamingoplossingen kunt maken zonder Storm of Spark.

* Horizontale schaal: Kafka partitioneert streams op de knooppunten in het HDInsight-cluster. Consumentenprocessen kunnen worden gekoppeld aan afzonderlijke partities voor een evenwichtige taakverdeling bij het gebruiken van records.

* Levering op volgorde: binnen elke partitie worden records in de stream opgeslagen in de volgorde waarin ze zijn ontvangen. Door één consumentenproces aan een partitie te koppelen, kunt u garanderen dat de records in de juiste volgorde worden verwerkt.

* Fouttolerantie: partities kunnen worden gerepliceerd tussen knooppunten voor fouttolerantie.

* Integratie met Azure Managed Disks: beheerde schijven bieden een hogere schaal en doorvoer voor de schijven die worden gebruikt met de virtuele machines in het HDInsight-cluster.

    Beheerde schijven zijn standaard ingeschakeld voor Kafka in HDInsight. Het aantal schijven dat per knooppunt wordt gebruikt, kan worden geconfigureerd tijdens het maken in HDInsight. Zie [Azure Managed Disks](../storage/storage-managed-disks-overview.md) voor meer informatie over beheerde schijven.

    Zie [Increase scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md) (Schaalbaarheid verhogen van Kafka in HDInsight) voor informatie over het configureren van beheerde schijven met Kafka in HDInsight.

## <a name="use-cases"></a>Gebruiksvoorbeelden

* **Berichten**: omdat het publicatie-/abonnementspatroon voor berichten wordt ondersteund, wordt Kafka vaak gebruikt als berichtenbroker.

* **Activiteitsregistratie**: omdat Kafka records registreert in de volgorde waarin ze binnenkomen, kan dit worden gebruikt om activiteiten bij te houden en opnieuw te maken. Bijvoorbeeld gebruikersacties op een website of in een toepassing.

* **Aggregatie**: met streamverwerking kunt u de gegevens uit de verschillende streams combineren en samenvoegen in operationele gegevens.

* **Transformatie**: met streamverwerking kunt u de gegevens uit meerdere invoeronderwerpen combineren en vertalen naar één of meer uitvoeronderwerpen.

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende koppelingen voor meer informatie over het gebruik van Apache Kafka in HDInsight:

* [Aan de slag met Kafka in HDInsight](hdinsight-apache-kafka-get-started.md)

* [MirrorMaker gebruiken voor het maken van een replica van Kafka in HDInsight](hdinsight-apache-kafka-mirroring.md)

* [Apache Storm gebruiken met Kafka in HDInsight](hdinsight-apache-storm-with-kafka.md)

* [Apache Spark gebruiken met Kafka in HDInsight](hdinsight-apache-spark-with-kafka.md)

* [Verbinding maken met Kafka via een Azure Virtual Network](hdinsight-apache-kafka-connect-vpn-gateway.md)
