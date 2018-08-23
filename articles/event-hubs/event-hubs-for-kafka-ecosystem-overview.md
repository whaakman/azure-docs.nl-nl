---
title: Azure Eventhubs voor Apache Kafka | Microsoft Docs
description: Overzicht van en inleiding tot Kafka ingeschakeld Azure Event Hubs
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 08/16/2018
ms.author: bahariri
ms.openlocfilehash: 16c101068be48ba1435ef230b29c679fcef17d08
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42054112"
---
# <a name="azure-event-hubs-for-apache-kafka-preview"></a>Azure Event Hubs voor Apache Kafka (preview)

Eventhubs biedt een Kafka-eindpunt dat kan worden gebruikt door uw bestaande Kafka op basis van toepassingen als alternatief voor het uitvoeren van uw eigen Kafka-cluster. Biedt ondersteuning voor Eventhubs [Apache Kafka 1.0](https://kafka.apache.org/10/documentation.html) en nieuwere clientversies en werkt met uw bestaande Kafka-toepassingen, met inbegrip van MirrorMaker. 

## <a name="what-does-event-hubs-for-kafka-provide"></a>Wat biedt Event Hubs voor Kafka

De Event Hubs voor Kafka-functie biedt een protocol hoofd boven op Azure Event Hubs die binaire compatibel is met Kafka-versies 1.0 en hoger voor zowel lezen van en schrijven naar het Kafka-onderwerpen. U kunt beginnen met behulp van het Kafka-eindpunt van uw toepassingen met een minimale configuratie wijzigen, maar er zijn geen codewijzigingen. U kunt de verbindingsreeks in configuraties om te verwijzen naar het Kafka-eindpunt beschikbaar gesteld door uw event hub in plaats van dat verwijst naar het Kafka-cluster bijwerken. Vervolgens kun u streaming-gebeurtenissen uit uw toepassingen die gebruikmaken van het Kafka-protocol in Event Hubs. 

Conceptueel gezien Kafka en Event Hubs zijn bijna identiek zijn: ze zijn beide gepartitioneerde logboeken die is gebouwd voor het streamen van gegevens. De volgende tabel worden de concepten tussen Kafka- en Event Hubs toegewezen.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Kafka en Event Hub conceptuele toewijzing

| Kafka-Concept | Event Hubs Concept|
| --- | --- |
| Cluster | Naamruimte |
| Onderwerp | Event Hubs |
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

## <a name="features-that-are-not-supported-in-the-preview"></a>Functies die worden niet ondersteund in de Preview-versie

De volgende Kafka-functies worden niet ondersteund voor de openbare preview van de Event Hubs voor Kafka-integratie:

*   Idempotent producent
*   Transactie
*   Compressie
*   Op basis van grootte
*   Meld u compressie
*   Partities toe te voegen aan een bestaand onderwerp
*   Kafka met HTTP-API-ondersteuning
*   Kafka verbinding maken
*   Kafka Streams

## <a name="next-steps"></a>Volgende stappen

In dit artikel opgegeven een inleiding tot Event Hubs voor Kafka. Zie de volgende koppelingen voor meer informatie:

* [Over het maken van Kafka ingeschakeld Event Hubs](event-hubs-create-kafka-enabled.md)
* [Stream naar Event Hubs van uw Kafka-toepassingen](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

 
 

