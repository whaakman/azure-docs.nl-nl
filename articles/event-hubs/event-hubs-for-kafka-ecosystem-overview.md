---
title: Gebruik event hub uit de app in Apache Kafka - Azure Event Hubs | Microsoft Docs
description: In dit artikel bevat informatie over de ondersteuning van Apache Kafka met Azure Event Hubs.
services: event-hubs
documentationcenter: .net
author: shvija
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: bahariri
ms.openlocfilehash: 8bf381e7c66e06bbaa140ed865f0f7c9b4f001af
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452340"
---
# <a name="use-azure-event-hubs-from-apache-kafka-applications"></a>Azure Event Hubs gebruiken vanuit toepassingen van Apache Kafka
Eventhubs biedt een Kafka-eindpunt dat kan worden gebruikt door uw bestaande Kafka op basis van toepassingen als alternatief voor het uitvoeren van uw eigen Kafka-cluster. Biedt ondersteuning voor Eventhubs [Apache Kafka-protocol 1.0 en hoger](https://kafka.apache.org/documentation/), en werkt met uw bestaande Kafka-toepassingen, met inbegrip van MirrorMaker.  

## <a name="what-does-event-hubs-for-kafka-provide"></a>Wat biedt Event Hubs voor Kafka

De Event Hubs voor Kafka-functie biedt een protocol hoofd boven op Azure Event Hubs die binaire compatibel is met Kafka-versies 1.0 en hoger voor zowel lezen van en schrijven naar het Kafka-onderwerpen. U kunt beginnen met behulp van het Kafka-eindpunt van uw toepassingen met een minimale configuratie wijzigen, maar er zijn geen codewijzigingen. U kunt de verbindingsreeks in configuraties om te verwijzen naar het Kafka-eindpunt beschikbaar gesteld door uw event hub in plaats van dat verwijst naar het Kafka-cluster bijwerken. Vervolgens kun u streaming-gebeurtenissen uit uw toepassingen die gebruikmaken van het Kafka-protocol in Event Hubs. Deze integratie biedt ook ondersteuning voor frameworks, zoals [Kafka verbinding](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/connect), deze bevindt zich momenteel in preview. 

Conceptueel gezien Kafka en Event Hubs zijn bijna identiek zijn: ze zijn beide gepartitioneerde logboeken die is gebouwd voor het streamen van gegevens. De volgende tabel worden de concepten tussen Kafka- en Event Hubs toegewezen.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Kafka en Event Hub conceptuele toewijzing

| Kafka-Concept | Event Hubs Concept|
| --- | --- |
| Cluster | Naamruimte |
| Onderwerp | Event Hub |
| Partitie | Partitie|
| Consumentengroep | Consumentengroep |
| Offset | Offset|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Belangrijke verschillen tussen Kafka- en Event Hubs

Terwijl [Apache Kafka](https://kafka.apache.org/) -software, die u uitvoeren kunt wanneer u kiest, is Event Hubs is een cloudservice die vergelijkbaar is met Azure Blob Storage. Er zijn geen servers of netwerken te beheren en geen brokers te configureren. U maakt een naamruimte een FQDN-naam in die uw onderwerpen is, en maak vervolgens Event Hubs en onderwerpen binnen die naamruimte. Zie voor meer informatie over Event Hubs en naamruimten [functies van Event Hubs](event-hubs-features.md#namespace). Als een cloudservice, Event Hubs maakt gebruik van één stabiel virtuele IP-adres als het eindpunt in, zodat clients niet hoeft te weten over de brokers of machines binnen een cluster. 

Schaal in Event Hubs wordt bepaald door u hoeveel doorvoereenheden u hebt gekocht, met elke throughput unit grond u 1 MB per seconde of 1000 gebeurtenissen per seconde van inkomend verkeer. Standaard Event Hubs omhoog wordt geschaald uitgaande doorvoereenheden wanneer u uw limiet voor het privégeheugen bereikt de [automatisch vergroten](event-hubs-auto-inflate.md) functie; deze functie ook werkt met de Event Hubs voor Kafka-functie. 

### <a name="security-and-authentication"></a>Verificatie en beveiliging

Azure Event Hubs SSL of TLS is vereist voor alle communicatie en Shared Access Signatures (SAS) gebruikt voor verificatie. Deze vereiste geldt ook voor een Kafka-eindpunt in Event Hubs. Voor compatibiliteit met Kafka, wordt met Event Hubs SASL zonder opmaak voor verificatie en SASL SSL gebruikt voor transportbeveiliging. Zie voor meer informatie over beveiliging in Event Hubs [Event Hubs-verificatie en beveiliging](event-hubs-authentication-and-security-model-overview.md).

## <a name="other-event-hubs-features-available-for-kafka"></a>Andere functies van Event Hubs voor Kafka

De Event Hubs voor Kafka-functie kunt u met één protocol schrijven en lezen met een andere, zodat uw huidige Kafka producenten publiceren via Kafka kunt, en u lezers met Event Hubs, zoals Azure Stream Analytics of Azure Functions kunt toevoegen. Bovendien Event Hubs functies zoals [vastleggen](event-hubs-capture-overview.md) en [Geo-noodherstel](event-hubs-geo-dr.md) ook gebruiken in combinatie met de Event Hubs voor Kafka-functie.

## <a name="features-that-are-not-yet-supported"></a>Functies die nog niet ondersteund 

Hier volgt de lijst van Kafka-functies die nog niet ondersteund:

*   Idempotent producent
*   Transactie
*   Compressie
*   Op basis van grootte
*   Meld u compressie
*   Partities toe te voegen aan een bestaand onderwerp
*   Kafka met HTTP-API-ondersteuning
*   Kafka Streams

## <a name="next-steps"></a>Volgende stappen

In dit artikel opgegeven een inleiding tot Event Hubs voor Kafka. Zie de volgende koppelingen voor meer informatie:

- [Event Hubs maken waarvoor Kafka is ingeschakeld](event-hubs-create-kafka-enabled.md)
- [Streamen naar Event Hubs vanaf uw Kafka-toepassingen](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Een Kafka-broker spiegelen in een event hub waarvoor Kafka is ingeschakeld](event-hubs-kafka-mirror-maker-tutorial.md)
- [Connect Apache Spark to a Kafka-enabled event hub](event-hubs-kafka-spark-tutorial.md) (Apache Spark verbinden met een Event Hub waarvoor Kafka is ingeschakeld)
- [Apache Flink verbinden met een Event Hub waarvoor Kafka is ingeschakeld](event-hubs-kafka-flink-tutorial.md)
- [Integrate Kafka Connect with a Kafka-enabled event hub](event-hubs-kafka-connect-tutorial.md) (Kafka Connect integreren met een Event Hub waarvoor Kafka is ingeschakeld)
- [Akka Streams verbinden met een Event Hub waarvoor Kafka is ingeschakeld](event-hubs-kafka-akka-streams-tutorial.md)
- [Explore samples on our GitHub](https://github.com/Azure/azure-event-hubs-for-kafka) (Voorbeelden bekijken op GitHub)


