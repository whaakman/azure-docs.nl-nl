---
title: Wat is Azure Event Hubs en waarom is het nuttig | Microsoft Docs
description: Overzicht van en inleiding tot Azure Event Hubs - Telemetrie van websites, apps en apparaten op cloudniveau opnemen
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: sethm; babanisa
ms.openlocfilehash: 236d49b7d8104b410e2a4c768820c3c9ebcdce96
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-event-hubs"></a>Wat is Event Hubs?

Azure Event Hubs is een uiterst schaalbaar platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Event Hubs biedt mogelijkheden voor klein- en grootschalige scenario’s voor [publiceren/abonneren](/biztalk/core/publish-and-subscribe-architecture). Hierbij fungeert de service als toegangspunt voor big data.

## <a name="why-use-event-hubs"></a>Het nut van Event Hubs

De verwerkingsmogelijkheden voor gebeurtenissen en telemetrie van Event Hubs zijn ideaal voor:

* Toepassingsinstrumentatie
* Verwerking van gebruikerservaringen of werkstromen
* Internet of Things (IoT)-scenario’s

Event Hubs maakt bijvoorbeeld het bijhouden van het gebruik van mobiele apps mogelijk, evenals het verzamelen van informatie over gegevensverkeer van web-farms, het vastleggen van spelgebeurtenissen in consolegames en het verzamelen van telemetriegegevens van industriële machines, verbonden voertuigen of andere apparaten.

## <a name="azure-event-hubs-overview"></a>Overzicht van Azure Event Hubs

De algemene rol die Event Hubs in oplossingsarchitecturen speelt, is die van 'voordeur' van een gebeurtenispijplijn. We spreken ook wel van een *event ingestor*. Een event ingestor is een onderdeel dat of een service die zich tussen gebeurtenisuitgever en gebeurtenisconsumer bevindt en de productie van de gebeurtenisstroom loskoppelt van het gebruik van de betreffende gebeurtenissen. U ziet deze architectuur in de volgende afbeelding:

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

Event Hubs biedt de mogelijkheid voor het verwerken van een berichtenstroom, maar sommige van de kenmerken wijken af van wat u gewend bent bij traditionele zakelijke tools voor berichtenverzending. De mogelijkheden van Event Hubs zijn gebaseerd op maximale doorvoer en scenario's voor de verwerking van gebeurtenissen. Event Hubs wijkt af van [Azure Service Bus](https://azure.microsoft.com/services/service-bus/)-messaging en implementeert niet alle berichtenmogelijkheden die beschikbaar zijn voor [Service Bus messaging](/azure/service-bus-messaging/)-entiteiten, zoals onderwerpen.

## <a name="event-hubs-features"></a>Functies van Event Hubs

Event Hubs bevat de volgende belangrijke elementen:

- [**Gebeurtenisproducent/-uitgever**](event-hubs-features.md#event-publishers): dit is een entiteit die gegevens naar een gebeurtenishub verzendt. Een gebeurtenis wordt gepubliceerd via AMQP 1.0 of HTTPS.
- [**Vastleggen**](event-hubs-features.md#capture): hiermee kunt u streaminggegevens van gebeurtenishubs vastleggen en opslaan in een Azure Blob-opslagaccount.
- [**Partities**](event-hubs-features.md#partitions): hiermee kan elke consument alleen een specifieke subset, of partitie, van de gebeurtenisstroom lezen.
- [**SAS-tokens**](event-hubs-features.md#sas-tokens): voor het identificeren en verifiëren van de gebeurtenisuitgever.
- [**Gebeurtenisconsumenten**](event-hubs-features.md#event-consumers): een entiteit die gebeurtenisgegevens van een gebeurtenishub leest. Gebeurtenisconsumenten maken verbinding via AMQP 1.0. 
- [**Consumentengroepen**](event-hubs-features.md#consumer-groups): biedt elke veelvoudige verbruikstoepassing een afzonderlijke weergave van de gebeurtenisstroom, waardoor deze consumenten onafhankelijk kunnen reageren.
- [**Doorvoereenheden**](event-hubs-features.md#capacity): vooraf aangeschafte capaciteitseenheden. Per partitie is maximaal één doorvoereenheid mogelijk.

Zie voor technische informatie over deze en andere functies van Event Hubs het [Overzicht van functies voor Event Hubs](event-hubs-features.md). 

## <a name="next-steps"></a>Volgende stappen

Zie [Prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) voor gedetailleerde informatie over prijzen van Event Hubs.

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
 
 

