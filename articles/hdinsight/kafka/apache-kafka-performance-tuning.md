---
title: Optimalisatie van prestaties voor Apache Kafka HDInsight-clusters
description: Biedt een overzicht van technieken voor het optimaliseren van Apache Kafka-workloads op Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/21/2019
ms.openlocfilehash: 903cd8921801ffb47dd73f48e507f30aa0b6dccc
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58373148"
---
# <a name="performance-optimization-for-apache-kafka-hdinsight-clusters"></a>Optimalisatie van prestaties voor Apache Kafka HDInsight-clusters

In dit artikel biedt enkele suggesties voor het optimaliseren van de prestaties van uw workloads Apache Kafka in HDInsight. De focus is over het aanpassen van de producent en broker-configuratie. Er zijn verschillende manieren voor het meten van prestaties en de optimalisaties die u van toepassing is afhankelijk van de behoeften van uw bedrijf.

## <a name="architecture-overview"></a>Overzicht van de architectuur

Kafka-onderwerpen worden gebruikt voor het ordenen van records. Records worden geproduceerd door producenten en die worden gebruikt door consumenten. Producenten verzenden records voor Kafka-brokers, die vervolgens de gegevens worden opgeslagen. Elk werkrolknooppunt in uw HDInsight-cluster is een Kafka-broker.

Onderwerpen verdelen records over brokers. Wanneer records worden verbruikt, kunt u maximaal één consumer per partitie gebruiken voor parallelle verwerking van de gegevens.

Replicatie wordt gebruikt voor het dupliceren van partities over knooppunten. Dit biedt bescherming tegen storingen van knooppunten (broker). Een enkele partitie binnen de groep van replica's is ingesteld als de leider partitie. Producentverkeer wordt doorgestuurd naar de leider van elk knooppunt, met behulp van de status die wordt beheerd door ZooKeeper.

## <a name="identify-your-scenario"></a>Uw scenario identificeren

Apache Kafka prestaties heeft twee belangrijke aspecten – doorvoer en latentie. Doorvoer is de maximale snelheid waarmee gegevens kunnen worden verwerkt. Hogere doorvoer is het meestal beter. Latentie is de tijd die nodig zijn voor gegevens worden opgeslagen of opgehaald. Lagere latentie is het meestal beter. Zoeken naar de juiste balans tussen doorvoer, latentie en de kosten van de infrastructuur van de toepassing kan lastig zijn. Uw prestatievereisten wordt waarschijnlijk overeenkomen met een van de volgende drie veelvoorkomende situaties, afhankelijk van of u nodig hoge doorvoer, lage latentie, of beide hebt:

* Hoge doorvoer en lage latentie. Dit scenario vereist zowel een hoge doorvoer en lage latentie (~ 100 milliseconden). Er is een voorbeeld van dit type toepassing service beschikbaarheidsbewaking van webtoepassingen.
* Hoge doorvoer, hoge latentie. In dit scenario hoge doorvoer (~1.5 GBps) is vereist, maar kan tolereren hogere latentie (< 250 ms). Een voorbeeld van dit type toepassing is telemetrie gegevensopname voor bijna realtime processen, zoals beveiliging en inbraakdetectie detectie-toepassingen.
* Lage doorvoer, lage latentie. In dit scenario is vereist (< 10 ms) voor lage latentie voor verwerking in realtime, maar lagere doorvoer kan tolereren. Een voorbeeld van dit type toepassing is online spelling en grammatica-controles.

## <a name="producer-configurations"></a>Producent-configuraties

De volgende secties worden enkele van de belangrijkste eigenschappen van de configuratie van het optimaliseren van uw Kafka producenten markeren. Zie voor een gedetailleerde beschrijving van alle configuratie-eigenschappen, [Apache Kafka-documentatie over producent configuraties](https://kafka.apache.org/documentation/#producerconfigs).

### <a name="batch-size"></a>Batchgrootte

Apache Kafka producenten samenstellen groepen van berichten (batches genoemd) die worden verzonden als een eenheid worden opgeslagen in een enkel opslagaccount-partitie. Batchgrootte betekent dat het aantal bytes dat aanwezig zijn moet voordat u deze groep wordt verzonden. Vergroot de `batch.size` parameter kunt betere doorvoer, omdat het vermindert de verwerking protocoloverhead van netwerk- en i/o-aanvragen. Onder lichte belasting verhogen verbeterde batchgrootte Kafka verzenden latentie als de producent moet voor een batch wachten gereed is. Onder zware belasting, is het raadzaam om de batchgrootte om doorvoer en latentie te verbeteren.

### <a name="producer-required-acknowledgements"></a>Producent vereist bevestigingen

De producent vereist `acks` bepaalt het aantal bevestigingen vereist door de leider partitie voordat een aanvraag wordt beschouwd als de configuratie is voltooid. Deze instelling is van invloed op de betrouwbaarheid van gegevens en het duurt voordat de waarden van `0`, `1`, of `-1`. De waarde van `-1` betekent dat een bevestiging van alle replica's moet worden ontvangen voordat de schrijfbewerking wordt voltooid. Instellen van `acks = -1` biedt betere garanties tegen verlies van gegevens, maar ook resultaten in hogere latentie en lagere doorvoer. Als de vereisten van uw toepassing hogere doorvoer, probeer instelling `acks = 0` of `acks = 1`. Houd er rekening mee dat alle replica's niet zijn bevestigd betrouwbaarheid van gegevens kunt beperken.

### <a name="compression"></a>Compressie

Een Kafka producer kan worden geconfigureerd voor het comprimeren van berichten voordat ze worden verzonden naar brokers. De `compression.type` instelling geeft u aan de compressiecodec moet worden gebruikt. Ondersteunde compressiecodecs zijn 'gzip,""snappy,"en"lz4." Compressie is nuttig zijn en moet worden beschouwd als er een beperking op schijfcapaciteit geldt.

Tussen de twee meest gebruikte compressiecodecs `gzip` en `snappy`, `gzip` heeft een hogere snelheid compressie, wat tot lagere schijfgebruik leiden van hogere CPU-belasting leidt. De `snappy` codec biedt minder compressie met minder CPU-overhead. U kunt bepalen welke codec om te gebruiken op basis van de broker schijf of de producent CPU-beperkingen. `gzip` kan comprimeren van gegevens tegen een tarief vijf keer hoger is dan `snappy`.

Met behulp van compressie van gegevens, wordt het aantal records dat kan worden opgeslagen op een schijf verhogen. Het kan worden verhoogd CPU overhead in gevallen waarbij er een discrepantie tussen de compressie-indelingen die worden gebruikt door de producent en de broker. Als de gegevens moeten worden gecomprimeerd voordat ze worden verzonden en vervolgens voorafgaand aan de verwerking is gedecomprimeerd.

## <a name="broker-settings"></a>Broker-instellingen

De volgende secties worden enkele van de belangrijkste instellingen voor het optimaliseren van uw Kafka-brokers markeren. Voor een gedetailleerde beschrijving van alle instellingen broker, Zie [Apache Kafka-documentatie over producent configuraties](https://kafka.apache.org/documentation/#producerconfigs).


### <a name="number-of-disks"></a>Aantal schijven

Storage-schijven hebben beperkte IOPS (invoer/uitvoer-bewerkingen Per seconde) en bytes per seconde voor lezen/schrijven. Bij het maken van nieuwe partities, wordt elke nieuwe partitie in Kafka opgeslagen op de schijf met zo weinig mogelijk bestaande partities op de beschikbare schijven verdeeld. Ondanks de opslagstrategie voor, bij het verwerken van honderden partitiereplica's op elke schijf, verzadigen Kafka kan eenvoudig de doorvoer van de beschikbare schijfruimte. Er is hier een afweging tussen doorvoer en de kosten. Als uw toepassing betere doorvoer vereist, maakt u een cluster met meer beheerde schijven per broker. HDInsight ondersteunt momenteel geen beheerde schijven toe te voegen aan een actieve cluster. Zie voor meer informatie over het configureren van het aantal beheerde schijven [opslag en schaalbaarheid configureren voor Apache Kafka in HDInsight](apache-kafka-scalability.md). Begrijp de gevolgen van de kosten van steeds meer opslagruimte voor de knooppunten in uw cluster.

### <a name="number-of-topics-and-partitions"></a>Aantal onderwerpen en partities

Kafka producenten schrijven naar onderwerpen. Kafka-consumenten van onderwerpen lezen. Een onderwerp is gekoppeld aan een logboek, een gegevensstructuur op schijf is. Kafka voegt records uit een producent(en) aan het einde van het logboek van een onderwerp. Een onderwerp logboek bestaat uit veel partities die worden verdeeld over meerdere bestanden. Deze bestanden worden, op zijn beurt verdeeld over meerdere clusterknooppunten voor Kafka. Consumenten lezen van Kafka-onderwerpen op hun uitgebracht en en hun positie (offset) in het onderwerp logboek kan kiezen.

Elke partitie Kafka is een logboekbestand op het systeem en producent threads gelijktijdig naar meerdere logboeken kunnen schrijven. Omdat elke thread consumenten berichten van één partitie leest, wordt verbruikt uit meerdere partities op deze manier uitgevoerd ook parallel.

Vergroten van de partitie dichtheid (het aantal partities per broker), wordt een overhead die betrekking hebben op bewerkingen van metagegevens en per partitie aanvraag/antwoord tussen de leider partitie en de Volgers toegevoegd. Zelfs in de afwezigheid van gegevens die binnenkomen via ophalen partitiereplica's nog steeds gegevens van de leiders die resulteert in extra verwerking voor verzenden en ontvangen van aanvragen via het netwerk.

Voor Apache Kafka-clusters 1.1 en boven HDInsight, raden we u maximaal 1000 partities per broker, met inbegrip van replica's hebben. Het aantal partities per broker vergroten, verkleint u doorvoer en kan ook leiden tot onderwerp niet beschikbaar zijn. Zie voor meer informatie over de ondersteuning van Kafka-partitie [de officiële Apache Kafka-blogbericht op de toename van het aantal ondersteunde partities in versie 1.1.0](https://blogs.apache.org/kafka/entry/apache-kafka-supports-more-partitions). Zie voor meer informatie over het aanpassen van onderwerpen [Apache Kafka: wijzigen van onderwerpen](https://kafka.apache.org/documentation/#basic_ops_modify_topic).

### <a name="number-of-replicas"></a>Aantal replica's

Hogere replicatiefactor resulteert in extra aanvragen tussen de leider partitie en Volgers. Als gevolg daarvan kan een hogere replicatiefactor verbruikt meer schijf- en CPU-aanvullende aanvragen verwerken verhogen schrijven latentie en doorvoer te verlagen.

U wordt aangeraden dat u ten minste 3 x replicatie voor Kafka in Azure HDInsight gebruikt. De meeste Azure-regio's hebben drie foutdomeinen, maar in regio's met slechts twee foutdomeinen gebruikers 4 x replicatie moeten gebruiken.

Zie voor meer informatie over replicatie [Apache Kafka: replicatie](https://kafka.apache.org/documentation/#replication) en [Apache Kafka: verhogen replicatiefactor](https://kafka.apache.org/documentation/#basic_ops_increase_replication_factor).

## <a name="next-steps"></a>Volgende stappen

* [Biljoenen gebeurtenissen per dag verwerken met Apache Kafka in Azure](https://azure.microsoft.com/blog/processing-trillions-of-events-per-day-with-apache-kafka-on-azure/)
* [Wat is Apache Kafka in HDInsight?](apache-kafka-introduction.md)
