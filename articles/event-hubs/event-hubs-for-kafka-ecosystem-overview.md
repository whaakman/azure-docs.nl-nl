---
title: Azure Event Hubs voor ecosystemen Kafka | Microsoft Docs
description: Overzicht en inleiding tot Kafka ingeschakeld Azure Event Hubs
services: event-hubs
documentationcenter: .net
author: djrosanova
manager: timlt
ms.service: event-hubs
ms.workload: na
ms.date: 05/07/2018
ms.author: darosa
ms.openlocfilehash: 40805c27e7e2eaef1932c967af2d291f67b4192d
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="event-hubs-for-kafka-ecosystems"></a>Event Hubs voor Kafka ecosystemen

Event Hubs voor ecosystemen Kafka biedt een Kafka-eindpunt dat kan worden gebruikt door uw bestaande Kafka op basis van toepassingen als alternatief voor het uitvoeren van uw eigen Kafka-cluster. Event Hubs voor Kafka ecosysteem ondersteunt [Apache Kafka 1.0](https://kafka.apache.org/10/documentation.html) en nieuwere clientversies, en werkt met uw bestaande Kafka-toepassingen, inclusief MirrorMaker. Wijzig de verbindingsreeks en start streaming-gebeurtenissen uit uw toepassingen die gebruikmaken van het protocol Kafka in Event Hubs.

## <a name="what-does-event-hubs-for-kafka-ecosystems-provide"></a>Wat biedt Event Hubs voor Kafka ecosystemen

Event Hubs voor Kafka ecosysteem biedt een protocol head boven op Azure Event Hubs die binaire compatibel is met Kafka versies 1.0 en hoger voor zowel lezen van en schrijven naar Kafka onderwerpen. Conceptueel Kafka en Event Hubs zijn bijna identiek: ze zijn beide gepartitioneerde logboeken gebouwd voor het streamen van gegevens. De volgende tabel worden de concepten tussen Kafka en Event Hubs toegewezen.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Kafka en Event Hub conceptuele toewijzing

| Kafka Concept | Event Hubs Concept|
| --- | --- |
| Cluster | Naamruimte |
| Onderwerp | Event Hubs |
| Partitie | Partitie|
| Consumentengroep | Consumentengroep |
| Offset | Offset|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Belangrijke verschillen tussen Kafka en Event Hubs

Terwijl [Apache Kafka](https://kafka.apache.org/) is software die u uitvoeren kunt wanneer u kiest, Event Hubs is een cloudservice die vergelijkbaar is met Azure Blob Storage. Er zijn geen servers of netwerken voor het beheren en geen beleggingsmakelaars configureren. U maakt een naamruimte een FQDN-naam waarin uw onderwerpen live is, en vervolgens maken Event Hubs of onderwerpen binnen deze naamruimte. Zie voor meer informatie over Event Hubs en naamruimten [wat Event Hubs is](event-hubs-what-is-event-hubs.md). Als een cloudservice gebruikt Event Hubs één stabiele virtuele IP-adres als het eindpunt, zodat clients niet hoeft te weten over de beleggingsmakelaars of machines binnen een cluster. 

Schaal in Event Hubs wordt bepaald door hoeveel doorvoereenheden u koopt, met elke doorvoereenheid grond u 1 MB per seconde of 1000 gebeurtenissen per seconde van toegangsroutes. Standaard Event Hubs schaalt doorvoereenheden wanneer u uw limiet met bereikt de [automatisch vergroten](event-hubs-auto-inflate.md) functie; deze functie ook werkt met Event Hubs voor Kafka ecosystemen. 

### <a name="security-and-authentication"></a>Verificatie en beveiliging

Azure Event Hubs SSL of TLS vereist voor alle communicatie en Shared Access Signatures (SAS) gebruikt voor verificatie. Deze vereiste geldt ook voor een eindpunt Kafka binnen Event Hubs. Voor compatibiliteit met Kafka gebruikt Event Hubs SASL zonder opmaak voor verificatie en SASL SSL voor transportbeveiliging. Zie voor meer informatie over beveiliging in Event Hubs [Event Hubs-verificatie en beveiliging](event-hubs-authentication-and-security-model-overview.md).

## <a name="other-event-hubs-features-available-for-kafka"></a>Andere Event Hubs-functies die beschikbaar zijn voor Kafka

Event Hubs voor Kafka ecosysteem kunt u met één protocol schrijven en te lezen met een andere, zodat uw huidige Kafka producenten publiceren via Kafka blijven kunnen en u lezers met Event Hubs, zoals Azure Stream Analytics of Azure-functies kunt toevoegen. Bovendien Event Hubs functies zoals [vastleggen](event-hubs-capture-overview.md) en [Geo-herstel na noodgevallen](event-hubs-geo-dr.md) werken ook met Event Hubs voor Kafka ecosystemen.

## <a name="features-that-are-not-supported-in-the-preview"></a>Functies die worden niet ondersteund in de Preview-versie

De volgende Kafka-functies worden niet ondersteund voor de openbare preview van de Event-Hubs voor Kafka ecosystemen integratie:

*   De Idempotent producent
*   Transactie
*   Compressie
*   Bewaren op basis van grootte
*   Meld u compressie
*   Partities toe te voegen aan een bestaand onderwerp
*   HTTP-Kafka API-ondersteuning
*   Kafka verbinding maken
*   Kafka stromen

## <a name="next-steps"></a>Volgende stappen

In dit artikel voorziet in een inleiding tot Event Hubs Kafka ecosystemen. Zie de volgende koppelingen voor meer informatie:

* [Het maken van Kafka ingeschakeld Event Hubs](event-hubs-create-kafka-enabled.md)
* [Stream in Event Hubs uit uw Kafka-toepassingen](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

 
 

