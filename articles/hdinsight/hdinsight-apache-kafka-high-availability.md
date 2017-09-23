---
title: Hoge beschikbaarheid met Apache Kafka - Azure HDInsight | Microsoft Docs
description: Informatie over hoge beschikbaarheid garanderen met Apache Kafka in Azure HDInsight. Informatie over het opnieuw verdelen van partitiereplica's in Kafka zodat deze zich bevinden op verschillende foutdomeinen binnen de Azure-regio met HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/20/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: f8164d1c3483b28e5f2abcc8035da78880daec1e
ms.contentlocale: nl-nl
ms.lasthandoff: 06/28/2017

---
# <a name="high-availability-of-your-data-with-apache-kafka-preview-on-hdinsight"></a>Hoge beschikbaarheid van uw gegevens met Apache Kafka (preview) in HDInsight

Informatie over het configureren van partitiereplica's voor Kafka-onderwerpen om te profiteren van de onderliggende rek-configuratie van de hardware. Deze configuratie waarborgt de beschikbaarheid van gegevens die zijn opgeslagen in Apache Kafka op HDInsight.

## <a name="fault-and-update-domains-with-kafka"></a>Probleem- en updatedomeinen met Kafka

Een foutdomein is een logische groepering van de onderliggende hardware in een Azure-datacenter. Elk foutdomein deelt een algemene voedingsbron en netwerkswitch. De virtuele machines en beheerde schijven die de knooppunten in een HDInsight-cluster implementeren zijn verdeeld over deze foutdomeinen. Deze architectuur beperkt de potentiële impact van problemen met de fysieke hardware.

Elke Azure-regio heeft een bepaald aantal foutdomeinen. Zie de [Beschikbaarheidssets](../virtual-machines/linux/regions-and-availability.md#availability-sets)-documentatie voor een lijst met domeinen en het aantal foutdomeinen die ze bevatten.

> [!IMPORTANT]
> Kafka is niet bekend met foutdomeinen. Wanneer u een onderwerp in Kafka maakt, is het daarom mogelijk dat alle partitiereplica's in hetzelfde foutdomein worden opgeslagen. Als oplossing voor dit probleem bieden we het [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools) (hulpprogramma voor het opnieuw indelen van Kafka-partities).

## <a name="when-to-rebalance-partition-replicas"></a>Wanneer partitiereplica 's opnieuw moeten worden ingedeeld

Om de hoogst mogelijke beschikbaarheid van uw Kafka-gegevens te waarborgen, moet u de partitiereplica's voor uw onderwerp op de volgende tijden opnieuw indelen:

* wanneer een nieuw onderwerp of partitie wordt gemaakt

* wanneer u een cluster opschaalt

## <a name="replication-factor"></a>Replicatiefactor

> [!IMPORTANT]
> Wij raden het gebruik aan van een Azure-regio die drie foutdomeinen bevat en van een replicatiefactor van 3.

Als u een regio met slechts twee foutdomeinen moet gebruiken, gebruik dan een replicatiefactor van 4 om de replica's gelijkmatig te verdelen over de twee foutdomeinen.

Zie het document [Aan de slag met Apache Kafka in HDInsight](hdinsight-apache-kafka-get-started.md) voor een voorbeeld van het maken van onderwerpen en instellen van de replicatiefactor.

## <a name="how-to-rebalance-partition-replicas"></a>Hoe partitiereplica 's opnieuw moeten worden ingedeeld

Gebruik het [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools) (hulpprogramma voor het opnieuw indelen van Kafka-partities) om de geselecteerde onderwerpen opnieuw in te delen. Dit hulpprogramma moet vanaf een SSH-sessie naar het hoofdknooppunt van het Kafka-cluster worden uitgevoerd.

Zie het document [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie over verbinding maken met HDInsight met behulp van SSH.

## <a name="next-steps"></a>Volgende stappen

* [Schaalbaarheid van Kafka in HDInsight](hdinsight-apache-kafka-scalability.md)
* [Spiegeling met Kafka in HDInsight](hdinsight-apache-kafka-mirroring.md)
