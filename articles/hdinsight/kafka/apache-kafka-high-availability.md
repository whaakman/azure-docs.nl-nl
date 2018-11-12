---
title: Hoge beschikbaarheid met Apache Kafka - Azure HDInsight
description: Informatie over hoge beschikbaarheid garanderen met Apache Kafka in Azure HDInsight. Informatie over het opnieuw verdelen van partitiereplica's in Kafka zodat deze zich bevinden op verschillende foutdomeinen binnen de Azure-regio met HDInsight.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.openlocfilehash: e2cd03f28e4a3cd8176f6126817c61508697af94
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007787"
---
# <a name="high-availability-of-your-data-with-apache-kafka-on-hdinsight"></a>Hoge beschikbaarheid van uw gegevens met Apache Kafka in HDInsight

Informatie over het configureren van partitiereplica's voor Kafka-onderwerpen om te profiteren van de onderliggende rek-configuratie van de hardware. Deze configuratie waarborgt de beschikbaarheid van gegevens die zijn opgeslagen in Apache Kafka op HDInsight.

## <a name="fault-and-update-domains-with-kafka"></a>Probleem- en updatedomeinen met Kafka

Een foutdomein is een logische groepering van de onderliggende hardware in een Azure-datacenter. Elk foutdomein deelt een algemene voedingsbron en netwerkswitch. De virtuele machines en beheerde schijven die de knooppunten in een HDInsight-cluster implementeren zijn verdeeld over deze foutdomeinen. Deze architectuur beperkt de potentiële impact van problemen met de fysieke hardware.

Elke Azure-regio heeft een bepaald aantal foutdomeinen. Zie de [Beschikbaarheidssets](../../virtual-machines/windows/regions-and-availability.md#availability-sets)-documentatie voor een lijst met domeinen en het aantal foutdomeinen die ze bevatten.

> [!IMPORTANT]
> Kafka is niet bekend met foutdomeinen. Wanneer u een onderwerp in Kafka maakt, is het daarom mogelijk dat alle partitiereplica's in hetzelfde foutdomein worden opgeslagen. Als oplossing voor dit probleem bevat HDInsight het [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools) (hulpprogramma voor het opnieuw indelen van Kafka-partities).

## <a name="when-to-rebalance-partition-replicas"></a>Wanneer partitiereplica 's opnieuw moeten worden ingedeeld

Om de hoogst mogelijke beschikbaarheid van uw Kafka-gegevens te waarborgen, moet u de partitiereplica's voor uw onderwerp op de volgende tijden opnieuw indelen:

* wanneer een nieuw onderwerp of partitie wordt gemaakt

* wanneer u een cluster opschaalt

## <a name="replication-factor"></a>Replicatiefactor

> [!IMPORTANT]
> Wij raden het gebruik aan van een Azure-regio die drie foutdomeinen bevat en van een replicatiefactor van 3.

Als u een regio met slechts twee foutdomeinen moet gebruiken, gebruik dan een replicatiefactor van 4 om de replica's gelijkmatig te verdelen over de twee foutdomeinen.

Zie het document [Aan de slag met Apache Kafka in HDInsight](apache-kafka-get-started.md) voor een voorbeeld van het maken van onderwerpen en instellen van de replicatiefactor.

## <a name="how-to-rebalance-partition-replicas"></a>Hoe partitiereplica 's opnieuw moeten worden ingedeeld

Gebruik het [Kafka partition rebalance tool](https://github.com/hdinsight/hdinsight-kafka-tools) (hulpprogramma voor het opnieuw indelen van Kafka-partities) om de geselecteerde onderwerpen opnieuw in te delen. Dit hulpprogramma moet vanaf een SSH-sessie naar het hoofdknooppunt van het Kafka-cluster worden uitgevoerd.

Zie het document [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie over verbinding maken met HDInsight met behulp van SSH.

## <a name="next-steps"></a>Volgende stappen

* [Schaalbaarheid van Kafka in HDInsight](apache-kafka-scalability.md)
* [Spiegeling met Kafka in HDInsight](apache-kafka-mirroring.md)
