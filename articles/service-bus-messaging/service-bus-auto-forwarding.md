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
ms.date: 02/22/2018
ms.author: spelluru
ms.openlocfilehash: 563fa6f38bb5baffb9a4ae86f944b7597d325d30
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43698992"
---
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Koppeling van Service Bus-entiteiten met automatisch doorsturen

De Service Bus *automatisch doorsturen* functie kunt u een wachtrij of abonnement naar een andere wachtrij of onderwerp die deel uitmaakt van dezelfde naamruimte keten. Wanneer automatisch doorsturen is ingeschakeld, worden berichten die in de eerste wachtrij of het eerste abonnement (bron) worden geplaatst, automatisch verwijdert en in de tweede wachtrij of het tweede onderwerp (doel) geplaatst. Houd er rekening mee dat het is nog steeds mogelijk rechtstreeks een bericht verzenden naar de doelentiteit. Het is ook niet mogelijk om te koppelen van een subwachtrij opgehaald, zoals een wachtrij voor onbestelbare transactieberichten, naar een andere wachtrij of onderwerp.

## <a name="using-auto-forwarding"></a>Met behulp van automatisch doorsturen

U kunt automatisch doorsturen inschakelen door in te stellen de [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] of [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] Eigenschappen van de [QueueDescription] [ QueueDescription] of [SubscriptionDescription] [ SubscriptionDescription] objecten voor de bron, zoals in de bijvoorbeeld:

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

De doelentiteit moet bestaan op het moment dat de bron-entiteit wordt gemaakt. Als de doelentiteit niet bestaat, retourneert de Service Bus een uitzondering wanneer u wordt gevraagd om te maken van de bronentiteit.

U kunt automatisch doorsturen gebruiken om uit een onderwerp te schalen. Limieten voor Service Bus de [aantal abonnementen op een bepaald onderwerp](service-bus-quotas.md) tot 2.000. Door het maken van onderwerpen op het tweede niveau, kunt u gebruikmaken van aanvullende abonnementen. Zelfs als u niet door de Service Bus-beperking op het aantal abonnementen zijn gebonden, kunt toevoegen van een tweede niveau van de onderwerpen over de algemene doorvoer van uw onderwerp verbeteren.

![Automatisch doorsturen scenario][0]

U kunt ook automatisch doorsturen afzenders van berichten van ontvangers van los te koppelen. Neem bijvoorbeeld een ERP-systeem dat uit drie modules bestaat: order verwerking, voorraadbeheer en beheer van klantrelaties. Elk van deze modules worden berichten die in de wachtrij in een corresponderende onderwerp zijn gegenereerd. Alice en Bob zijn vertegenwoordigers die zijn geïnteresseerd in alle berichten die gerelateerd aan hun klanten zijn. Voor het ontvangen van deze berichten, maken Els en Bob van een persoonlijke wachtrij en een abonnement op elk van de ERP-onderwerpen die automatisch alle berichten naar de wachtrij doorsturen.

![Automatisch doorsturen scenario][1]

Als Els op vakantie, haar persoonlijke wachtrij, in plaats van het onderwerp ERP gaat, gevuld. In dit scenario, omdat een verkoopmedewerker niet van berichten ontvangen is, bereiken geen van de onderwerpen ERP ooit quotum.

## <a name="auto-forwarding-considerations"></a>Overwegingen met betrekking tot automatisch doorsturen

Als de doelentiteit worden te veel berichten bij elkaar opgeteld en het quotum overschrijdt of als de doelentiteit is uitgeschakeld, de bron-entiteit wordt toegevoegd de berichten naar de [dead-letter-wachtrij](service-bus-dead-letter-queues.md) totdat er ruimte in de bestemming (of de entiteit is opnieuw ingeschakeld). Deze berichten blijven bevinden zich in de wachtrij voor onbestelbare berichten, zodat u moet expliciet ontvangt en verwerkt door ze uit de wachtrij voor onbestelbare berichten.

Het verdient de voorkeur dat u een gemiddeld aantal abonnementen op het eerste niveau onderwerp en het aantal abonnementen op de onderwerpen op het tweede niveau hebt voor het koppelen van afzonderlijke onderwerpen voor het verkrijgen van een samengestelde onderwerp met veel abonnementen. Bijvoorbeeld kunt een onderwerp van het eerste niveau met 20-abonnementen, elk van deze gekoppeld aan een onderwerp op het tweede niveau met 200 abonnementen, u hogere doorvoer dan een onderwerp van het eerste niveau met 200 abonnementen, elk gekoppeld aan een onderwerp op het tweede niveau met 20 abonnementen.

Service Bus kosten in rekening gebracht één bewerking voor elk bericht dat wordt doorgestuurd. Bijvoorbeeld, een bericht verzenden naar een onderwerp met 20 abonnementen, elk van deze geconfigureerd voor het automatisch doorsturen van berichten naar een andere wachtrij of onderwerp, wordt in rekening gebracht als 21 bewerkingen als alle abonnementen van het eerste niveau een kopie van het bericht ontvangt.

Voor het maken van een abonnement dat is gekoppeld aan een andere wachtrij of onderwerp, de maker van het abonnement moet hebben **beheren** machtigingen voor zowel de bron- als de doelentiteit. Alleen berichten verzenden naar het onderwerp van de bron nodig **verzenden** machtigingen op de bron-onderwerp.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen met naslaginformatie voor gedetailleerde informatie over automatisch doorsturen:

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
