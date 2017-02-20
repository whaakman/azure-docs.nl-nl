---
title: Inleiding tot Apache Kafka in HDInsight | Microsoft Docs
description: Meer informatie over Apache Kafka in HDInsight. Wat het is, wat het doet en waar u voorbeelden en gegevens kunt vinden om aan de slag te gaan.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: f284b6e3-5f3b-4a50-b455-917e77588069
ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/09/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ecc141c9afa46f23d31de4356068ef4f98a92aa
ms.openlocfilehash: a47a911967a50102f62d471d2e73cf86bdeda3b1

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

## <a name="use-cases"></a>Gebruiksvoorbeelden

* **Berichten**: omdat het publicatie-/abonnementspatroon voor berichten wordt ondersteund, wordt Kafka vaak gebruikt als berichtenbroker.

* **Activiteitsregistratie**: omdat Kafka records registreert in de volgorde waarin ze binnenkomen, kan dit worden gebruikt om activiteiten bij te houden en opnieuw te maken. Bijvoorbeeld gebruikersacties op een website of in een toepassing.

* **Aggregatie**: met streamverwerking kunt u de gegevens uit de verschillende streams combineren en samenvoegen in operationele gegevens.

* **Transformatie**: met streamverwerking kunt u de gegevens uit meerdere invoeronderwerpen combineren en vertalen naar één of meer uitvoeronderwerpen.

## <a name="where-do-i-start"></a>Waar moet ik beginnen?

Zie [Get started with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md) (Aan de slag met Kafka op HDInsight) voor meer informatie over het maken van een Kafka-cluster en het gebruiken van Kafka, met op Java gebaseerde gebruiksvoorbeelden van de producent-, consument- en streaming-API

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende koppelingen voor meer informatie over het gebruik van Apache Kafka in HDInsight:

* [Aan de slag met Kafka in HDInsight](hdinsight-apache-kafka-get-started.md)

* [MirrorMaker gebruiken voor het maken van een replica van Kafka in HDInsight](hdinsight-apache-kafka-mirroring.md)

* [Apache Storm gebruiken met Kafka in HDInsight](hdinsight-apache-storm-with-kafka.md)

* [Apache Spark gebruiken met Kafka in HDInsight](hdinsight-apache-spark-with-kafka.md)




<!--HONumber=Feb17_HO2-->


