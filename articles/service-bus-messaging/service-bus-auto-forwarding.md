---
title: Automatisch doorsturen van Azure Service Bus-berichtentiteiten | Microsoft Docs
description: Klik hier voor meer informatie over het koppelen van een Service Bus-wachtrij of een abonnement op een andere wachtrij of onderwerp.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2017
ms.author: sethm
ms.openlocfilehash: 6c92acee9d7609f4fedcddd40563b1a55fa08fac
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Service Bus-entiteiten met automatisch doorsturen Chaining

De Service Bus *automatisch doorsturen* functie kunt u een wachtrij of een abonnement op een andere wachtrij of onderwerp maakt deel uit van dezelfde naamruimte zijn gekoppeld. Wanneer automatisch doorsturen is ingeschakeld, wordt Service Bus automatisch verwijdert van berichten die worden geplaatst in de eerste wachtrij of abonnement (bron) en plaatst deze in de tweede wachtrij of onderwerp (doel). Houd er rekening mee dat het nog steeds mogelijk rechtstreeks een bericht verzenden naar de doelentiteit is. Het is ook niet mogelijk om te koppelen van een subwachtrij, zoals een wachtrij voor onbestelbare berichten, naar een andere wachtrij of onderwerp.

## <a name="using-auto-forwarding"></a>Automatisch doorsturen gebruiken
U kunt automatisch doorsturen inschakelen door het instellen van de [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] of [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] eigenschappen op de [QueueDescription] [ QueueDescription] of [SubscriptionDescription] [ SubscriptionDescription] objecten voor de bron, zoals in het volgende voorbeeld.

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

De doelentiteit moet bestaan op het moment dat de bronentiteit is gemaakt. Als de doelentiteit niet bestaat, retourneert Service Bus een uitzondering wanneer u wordt gevraagd om te maken van de bronentiteit.

Automatisch doorsturen kunt u een onderwerp uitbreiden. Service Bus-limieten de [aantal abonnementen op een bepaald onderwerp](service-bus-quotas.md) tot 2.000. Door het tweede niveau onderwerpen maken, kunt u gebruikmaken van aanvullende abonnementen. Houd er rekening mee dat zelfs als u niet aan de Service Bus-beperking van het aantal abonnementen gebonden, het toevoegen van een tweede verificatieniveau onderwerpen de algehele doorvoer van uw onderwerp verbeteren kunt.

![Automatisch doorsturen scenario][0]

U kunt ook automatisch doorsturen loskoppelen van de afzenders van berichten van ontvangers. Neem bijvoorbeeld een ERP-systeem dat uit drie modules bestaat: volgorde verwerking, voorraadbeheer en customer relationship management. Elk van deze modules worden berichten die in de wachtrij in een bijbehorende onderwerp zijn gegenereerd. Els en Bob zijn vertegenwoordigers die geïnteresseerd zijn in alle berichten die gerelateerd zijn aan hun klanten. Voor het ontvangen van deze berichten maken Alice en Bob van een persoonlijke wachtrij en een abonnement op elk van de ERP-onderwerpen die automatisch alle berichten naar de wachtrij doorsturen.

![Automatisch doorsturen scenario][1]

Als Els op vakantie, haar persoonlijke wachtrij, in plaats van het onderwerp ERP gaat, vol raakt. In dit scenario, omdat een verkoopmedewerker niet alle berichten ontvangen bereiken geen van de onderwerpen ERP ooit quotum.

## <a name="auto-forwarding-considerations"></a>Automatisch doorsturen overwegingen

Als de doelentiteit stelt samen te veel berichten en het quotum wordt overschreden, of de doelentiteit is uitgeschakeld, de bronentiteit voegt de berichten naar de [wachtrij voor onbestelbare berichten](service-bus-dead-letter-queues.md) totdat er ruimte in de doel-(of de entiteit opnieuw ingeschakeld is). De berichten blijven bevinden zich in de wachtrij voor onbestelbare berichten, zodat u moet expliciet ontvangen en van de wachtrij voor onbestelbare berichten verwerken.

Het verdient de voorkeur dat u een beperkt aantal abonnementen op het eerste niveau onderwerp en veel abonnementen op de tweede niveau onderwerpen hebt voor het koppelen van afzonderlijke onderwerpen voor het verkrijgen van een samengestelde onderwerp met veel abonnementen. Een onderwerp van het eerste niveau met 20 abonnementen, elk van deze gekoppeld aan een onderwerp van het tweede niveau met 200 abonnementen kan bijvoorbeeld voor een hogere doorvoer dan een onderwerp van het eerste niveau met 200 abonnementen, elk gekoppeld aan een onderwerp van het tweede niveau met 20 abonnementen.

Service Bus stuklijsten één bewerking voor elk bericht. Bijvoorbeeld, een bericht verzenden naar een onderwerp met 20 abonnementen, elk van deze geconfigureerd voor het automatisch doorsturen van berichten naar een andere wachtrij of onderwerp, wordt in rekening gebracht als 21 bewerkingen als alle abonnementen van het eerste niveau een kopie van het bericht ontvangt.

Voor het maken van een abonnement dat is gekoppeld aan een andere wachtrij of onderwerp, de maker van het abonnement moet hebben **beheren** machtigingen voor zowel de bron en de doelentiteit. Vereist het verzenden van berichten naar de bron-onderwerp alleen **verzenden** machtigingen op de bron-onderwerp.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen met naslaginformatie voor gedetailleerde informatie over het automatisch doorsturen:

* [ForwardTo][QueueDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

Zie voor meer informatie over Service Bus-prestatieverbeteringen. 

* [Aanbevolen procedures voor verbeterde prestaties met behulp van Service Bus-berichtenservice](service-bus-performance-improvements.md)
* [Gepartitioneerde berichtentiteiten][Partitioned messaging entities].

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
