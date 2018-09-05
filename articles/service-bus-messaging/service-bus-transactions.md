---
title: Overzicht van transactieverwerking in Azure Service Bus | Microsoft Docs
description: Overzicht van Azure Service Bus atomic-transacties en verzenden via
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 64449247-1026-44ba-b15a-9610f9385ed8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: spelluru
ms.openlocfilehash: bc0e0bfb8d4dc8637c191785a98f5d15e086cbf5
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696428"
---
# <a name="overview-of-service-bus-transaction-processing"></a>Overzicht van Service Bus-transactieverwerking

Dit artikel worden de transactiemogelijkheden van Microsoft Azure Service Bus. Veel van de discussie wordt aangegeven door de [atomische transacties met Service Bus-voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions). In dit artikel is beperkt tot een overzicht van de verwerking van transacties en het *verzenden* functie in Service Bus, terwijl het voorbeeld Atomic-transacties grotere en complexere binnen het bereik is.

## <a name="transactions-in-service-bus"></a>Transacties in Servicebus

Een [ *transactie* ](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) gegroepeerd in twee of meer bewerkingen een *uitvoering bereik*. Een dergelijke transactie moet ervoor zorgen dat alle bewerkingen die behoren tot een bepaalde groep bewerkingen slagen of gezamenlijk mislukken door aard. In dit opzicht transacties fungeren als één eenheid, waarmee wordt vaak aangeduid als *atomisch*. 

Service Bus is een transactioneel bericht-broker en zorgt ervoor transactionele integriteit voor alle interne bewerkingen op de berichten-stores. Alle overdrachten van berichten in Service Bus, zoals het verplaatsen van berichten naar een [dead-letter-wachtrij](service-bus-dead-letter-queues.md) of [automatisch doorsturen](service-bus-auto-forwarding.md) van berichten tussen entiteiten zijn transactionele. Als zodanig als Service Bus een bericht accepteert, is al opgeslagen en met het label met een volgnummer. Daarna eventuele overdrachten bericht in Service Bus zijn gecoördineerde bewerkingen via entiteiten en wordt geen van beide leiden tot verlies (bron is geslaagd en mislukt doel) of om te kopiëren (mislukt van de bron en doel is geslaagd) van het bericht.

Service Bus biedt ondersteuning voor het groeperen van bewerkingen voor één berichtentiteit (wachtrij, onderwerp of abonnement) binnen het bereik van een transactie. Bijvoorbeeld, u kunt verschillende berichten verzenden naar een wachtrij van binnen het transactiebereik van een en de berichten worden alleen vastgelegd in Logboeken van de wachtrij als de transactie is voltooid.

## <a name="operations-within-a-transaction-scope"></a>Bewerkingen in een transactiebereik

De bewerkingen die kunnen worden uitgevoerd in een transactiebereik zijn als volgt:

* **[QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient), [MessageSender](/dotnet/api/microsoft.azure.servicebus.core.messagesender), [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient)**: verzenden, SendAsync, SendBatch, SendBatchAsync 
* **[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: voltooid CompleteAsync, dit, AbandonAsync, onbestelbare berichten, DeadletterAsync, uitstellen, DeferAsync, RenewLock, RenewLockAsync 

Ontvangen bewerkingen zijn niet opgenomen, omdat ervan wordt uitgegaan dat de toepassing krijgt van berichten met behulp van de [ReceiveMode.PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modus, in een paar lus ontvangen of met een [OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) terugbellen, en opent alleen vervolgens een transactiebereik voor het verwerken van het bericht.

De toestand van het bericht (voltooid, dit, dead-letter uitvoeren, uitstellen) uitgevoerd binnen het bereik van, en zijn afhankelijk van de algehele resultaat van de transactie.

## <a name="transfers-and-send-via"></a>Bestanden worden overgedragen en 'verzenden '

Om in te schakelen transactionele oproepdienstoverdracht van gegevens van een wachtrij op een processor en klik vervolgens op een andere wachtrij, Service Bus ondersteunt *overdrachten*. In een overdrachtbewerking een afzender eerst verzendt een bericht naar een *wachtrij voor overdracht*, en de wachtrij voor overdracht wordt onmiddellijk het bericht verplaatst naar de bestemmingswachtrij met de dezelfde robuuste overdracht-implementatie die automatisch doorsturen afhankelijk van capaciteit. Het bericht is niet doorgevoerd in de wachtrij voor de overdracht logboek op een manier dat zichtbaar voor gebruikers van de wachtrij voor overdracht wordt.

De kracht van deze mogelijkheid transactionele wordt zichtbaar wanneer de wachtrij voor overdracht zelf de bron van de invoer van de afzender-berichten is. Met andere woorden, Service Bus kunt overbrengen het bericht naar de doelwachtrij "via' de wachtrij voor overdracht, tijdens het uitvoeren van een complete (of uitstellen, of dead-letter)-bewerking op de invoerbericht, allemaal op een atomische bewerking. 

### <a name="see-it-in-code"></a>In de code weergeven

Als u deze overschrijvingen instelt, maakt u een afzender die gericht is op de doelwachtrij via de wachtrij voor overdracht. U hebt ook een ontvanger die berichten op uit die dezelfde wachtrij haalt. Bijvoorbeeld:

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

* [Service Bus-wachtrijen gebruiken](service-bus-dotnet-get-started-with-queues.md)
* [Koppeling van Service Bus-entiteiten met automatisch doorsturen](service-bus-auto-forwarding.md)
* [Voorbeeld van automatisch doorsturen](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [Atomische transacties met Service Bus-voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [Azure-wachtrijen en Service Bus-wachtrijen in vergelijking met](service-bus-azure-and-service-bus-queues-compared-contrasted.md)


