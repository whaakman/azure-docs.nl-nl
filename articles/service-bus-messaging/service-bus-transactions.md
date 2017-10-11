---
title: Overzicht van de transactie worden verwerkt in de Azure Service Bus | Microsoft Docs
description: Overzicht van Azure Service Bus-atomic-transacties en verzenden via
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 64449247-1026-44ba-b15a-9610f9385ed8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: clemensv;sethm
ms.openlocfilehash: a88f2d81ab43e38c9363a67aaefc178b47bfb259
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-service-bus-transaction-processing"></a>Overzicht van Service Bus-transactieverwerking
Dit artikel wordt de transactiemogelijkheden van Azure Service Bus. Veel van de bespreking van de wordt geïllustreerd door het [atomische transacties met Service Bus-voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions). In dit artikel is beperkt tot een overzicht van transactieverwerking en de *verzenden* functie in Service Bus, terwijl het voorbeeld atomische transacties grotere en complexere binnen het bereik is.

## <a name="transactions-in-service-bus"></a>Transacties in de Servicebus
Een [transactie](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) twee of meer bewerkingen gegroepeerd in een *uitvoering bereik*. Een dergelijke transactie moet ervoor zorgen dat alle bewerkingen die horen bij een bepaalde groep bewerkingen slagen of gezamenlijk mislukken door aard. In dit opzicht transacties fungeren als één eenheid ondergebracht, die vaak aangeduid als *atomisch*. 

Service Bus is een broker transactionele berichten en garandeert een transactionele integriteit voor alle interne bewerkingen op basis van de bericht-stores. Alle overdrachten van berichten op de Service Bus, zoals het verplaatsen van berichten naar een [wachtrij voor onbestelbare berichten](service-bus-dead-letter-queues.md) of [automatisch doorsturen](service-bus-auto-forwarding.md) van berichten tussen entiteiten transactionele zijn. Als zodanig als Service Bus een bericht accepteert, is al opgeslagen en gelabeld met een volgnummer. Daarna de overdracht van een bericht binnen de Service Bus zijn gecoördineerde bewerkingen voor alle entiteiten en wordt geen van beide leiden tot verlies (bron is geslaagd en mislukt doel) of om te worden gedupliceerd (mislukt van de bron en doel is geslaagd) van het bericht.

Service Bus biedt ondersteuning voor groepering bewerkingen op een enkele Berichtentiteit (wachtrij, onderwerp, abonnement) binnen het bereik van een transactie. Bijvoorbeeld, u kunt meerdere berichten verzenden naar een wachtrij uit vanuit een transactiebereik en de berichten worden pas doorgevoerd in de wachtrij logboek wanneer de transactie is voltooid.

## <a name="operations-within-a-transaction-scope"></a>Bewerkingen binnen een transactiebereik
De bewerkingen die kunnen worden uitgevoerd vanuit een transactiebereik zijn als volgt:

* **[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: verzenden, SendAsync, SendBatch, SendBatchAsync 
* **[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: voltooid, CompleteAsync, afbreken, AbandonAsync, wachtrij voor onbestelbare, DeadletterAsync, uitstelt, DeferAsync, RenewLock, RenewLockAsync 

Ontvangen bewerkingen zijn niet toegevoegd, omdat ervan wordt uitgegaan dat de toepassing krijgt berichten met behulp van de [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) modus binnen enkele lus voor ontvangen of met een [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) callback en vervolgens pas Hiermee opent u een transactiebereik voor het verwerken van het bericht.

De bestemming van het bericht (voltooid, afbreken, onbestelbare, uitstellen) uitgevoerd binnen het bereik van, en afhankelijk van de algehele uitkomst van de transactie.

## <a name="transfers-and-send-via"></a>Overschrijvingen en 'verzenden '
Om in te schakelen transactionele overdracht van gegevens uit een wachtrij op een processor en vervolgens naar een andere wachtrij, Service Bus ondersteunt *overdrachten*. Een afzender verzendt een bericht eerst naar een wachtrij' transfer' en het bericht in de wachtrij onmiddellijk worden verplaatst naar de bestemmingswachtrij met de dezelfde robuuste overdracht implementatie die afhankelijk is van het automatisch doorsturen van berichten in een overdrachtsbewerking voor. Het bericht is nooit doorgevoerd in het logboek van de wachtrij zodanig dat deze zichtbaar is voor de overdracht van de wachtrij consumenten.

De kracht van deze mogelijkheid transactionele wordt zichtbaar wanneer de wachtrij zelf de bron van de invoer berichten van de afzender is. Met andere woorden, Service Bus kunt overdragen het bericht naar de doelwachtrij 'via' de wachtrij, tijdens het uitvoeren van een complete (of uitstelt, of onbestelbare)-bewerking op het invoerbericht, allemaal in een atomic-bewerking. 

### <a name="see-it-in-code"></a>Deze weergegeven in de code
Als u deze overschrijvingen instelt, moet u een afzender die gericht is op de doelwachtrij via de wachtrij maken. U hebt ook een ontvanger waarmee berichten van die dezelfde wachtrij opgehaald. Bijvoorbeeld:

```csharp
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Vervolgens een eenvoudige transactie maakt gebruik van deze elementen, zoals in het volgende voorbeeld:

```csharp
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over Service Bus-wachtrijen:

* [Service Bus-entiteiten met automatisch doorsturen Chaining](service-bus-auto-forwarding.md)
* [Automatisch doorsturen voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [Atomische transacties met Service Bus-voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [Azure-wachtrijen en Service Bus-wachtrijen vergeleken](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
* [Service Bus-wachtrijen gebruiken](service-bus-dotnet-get-started-with-queues.md)

