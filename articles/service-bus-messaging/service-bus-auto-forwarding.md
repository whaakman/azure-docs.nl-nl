---
title: Azure Service Bus messaging-entiteiten met automatisch doorsturen | Microsoft Docs
description: Klik hier voor meer informatie over het koppelen van een Service Bus-wachtrij of abonnement naar een andere wachtrij of onderwerp.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2018
ms.author: spelluru
ms.openlocfilehash: 608510f76d54cc5f3e10587a6f9d1306612672ad
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47391095"
---
# <a name="chaining-service-bus-entities-with-autoforwarding"></a>Service Bus-entiteiten met autoforwarding-koppeling

De Service Bus *autoforwarding* functie kunt u een wachtrij of abonnement naar een andere wachtrij of onderwerp die deel uitmaakt van dezelfde naamruimte keten. Wanneer autoforwarding is ingeschakeld, wordt Service Bus automatisch worden berichten die worden geplaatst in de eerste wachtrij of abonnement (bron) verwijderd en worden de tegels in de tweede wachtrij of onderwerp (doel). Het is nog steeds mogelijk rechtstreeks een bericht verzenden naar de doelentiteit. Het is ook niet mogelijk om te koppelen van een subwachtrij opgehaald, zoals een wachtrij voor onbestelbare transactieberichten, naar een andere wachtrij of onderwerp.

## <a name="using-autoforwarding"></a>Met behulp van autoforwarding

U kunt autoforwarding inschakelen door in te stellen de [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] of [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] Eigenschappen van de [QueueDescription] [ QueueDescription] of [SubscriptionDescription] [ SubscriptionDescription] objecten voor de bron, zoals in de bijvoorbeeld:

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

De doelentiteit moet bestaan op het moment dat de bron-entiteit wordt gemaakt. Als de doelentiteit niet bestaat, retourneert de Service Bus een uitzondering wanneer u wordt gevraagd om te maken van de bronentiteit.

U kunt autoforwarding gebruiken om uit een onderwerp te schalen. Limieten voor Service Bus de [aantal abonnementen op een bepaald onderwerp](service-bus-quotas.md) tot 2.000. Door het maken van onderwerpen op het tweede niveau, kunt u gebruikmaken van aanvullende abonnementen. Zelfs als u niet door de Service Bus-beperking op het aantal abonnementen zijn gebonden, kunt toevoegen van een tweede niveau van de onderwerpen over de algemene doorvoer van uw onderwerp verbeteren.

![Automatisch doorsturen scenario][0]

U kunt ook autoforwarding afzenders van berichten van ontvangers van los te koppelen. Neem bijvoorbeeld een ERP-systeem dat uit drie modules bestaat: order verwerking, voorraadbeheer en beheer van klantrelaties. Elk van deze modules worden berichten die in de wachtrij in een corresponderende onderwerp zijn gegenereerd. Alice en Bob zijn vertegenwoordigers die zijn geïnteresseerd in alle berichten die gerelateerd aan hun klanten zijn. Voor het ontvangen van deze berichten, maken Els en Bob van een persoonlijke wachtrij en een abonnement op elk van de ERP-onderwerpen die automatisch alle berichten naar de wachtrij doorsturen.

![Automatisch doorsturen scenario][1]

Als Els op vakantie, haar persoonlijke wachtrij, in plaats van het onderwerp ERP gaat, gevuld. In dit scenario, omdat een verkoopmedewerker niet van berichten ontvangen is, bereiken geen van de onderwerpen ERP ooit quotum.

## <a name="autoforwarding-considerations"></a>Overwegingen voor Autoforwarding

Als de doelentiteit worden te veel berichten bij elkaar opgeteld en het quotum overschrijdt of als de doelentiteit is uitgeschakeld, de bron-entiteit wordt toegevoegd de berichten naar de [dead-letter-wachtrij](service-bus-dead-letter-queues.md) totdat er ruimte in de bestemming (of de entiteit is opnieuw ingeschakeld). Deze berichten blijven bevinden zich in de wachtrij voor onbestelbare berichten, zodat u moet expliciet ontvangt en verwerkt door ze uit de wachtrij voor onbestelbare berichten.

Het verdient de voorkeur dat u een gemiddeld aantal abonnementen op het eerste niveau onderwerp en het aantal abonnementen op de onderwerpen op het tweede niveau hebt voor het koppelen van afzonderlijke onderwerpen voor het verkrijgen van een samengestelde onderwerp met veel abonnementen. Bijvoorbeeld kunt een onderwerp van het eerste niveau met 20-abonnementen, elk van deze gekoppeld aan een onderwerp op het tweede niveau met 200 abonnementen, u hogere doorvoer dan een onderwerp van het eerste niveau met 200 abonnementen, elk gekoppeld aan een onderwerp op het tweede niveau met 20 abonnementen.

Service Bus kosten in rekening gebracht één bewerking voor elk bericht dat wordt doorgestuurd. Bijvoorbeeld, een bericht verzenden naar een onderwerp met 20 abonnementen, elk van deze geconfigureerd autoforward berichten naar een andere wachtrij of onderwerp, wordt in rekening gebracht als 21 bewerkingen als alle abonnementen van het eerste niveau een kopie van het bericht ontvangt.

Voor het maken van een abonnement dat is gekoppeld aan een andere wachtrij of onderwerp, de maker van het abonnement moet hebben **beheren** machtigingen voor zowel de bron- als de doelentiteit. Alleen berichten verzenden naar het onderwerp van de bron nodig **verzenden** machtigingen op de bron-onderwerp.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen met naslaginformatie voor gedetailleerde informatie over autoforwarding:

* [ForwardTo][QueueDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

Zie voor meer informatie over Service Bus voor verbeterde prestaties 

* [Aanbevolen procedures voor prestatieverbeteringen met Service Bus-berichten](service-bus-performance-improvements.md)
* [Gepartitioneerde berichtentiteiten][Partitioned messaging entities].

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
