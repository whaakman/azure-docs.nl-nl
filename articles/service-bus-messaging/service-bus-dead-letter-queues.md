---
title: Service Bus-wachtrijen voor onbestelbare berichten | Microsoft Docs
description: Overzicht van Azure Service Bus-wachtrijen voor onbestelbare berichten
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68b2aa38-dba7-491a-9c26-0289bc15d397
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: clemensv;sethm
ms.openlocfilehash: c16bcf30ab96f79e59404a41852e4cd227e28b08
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-service-bus-dead-letter-queues"></a>Overzicht van Service Bus-wachtrijen voor onbestelbare berichten

Service Bus-wachtrijen en onderwerpabonnementen bieden u een secundaire onderliggende wachtrij aangeroepen een *wachtrij voor onbestelbare berichten* (DLQ). De wachtrij voor onbestelbare berichten hoeft niet expliciet worden gemaakt en kan niet worden verwijderd of anders beheerd onafhankelijk van de belangrijkste entiteit.

Dit artikel wordt beschreven wachtrijen voor onbestelbare berichten in de Azure Service Bus. Veel van de bespreking van de wordt geïllustreerd door het [wachtrijen voor onbestelbare berichten voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/DeadletterQueue) op GitHub.
 
## <a name="the-dead-letter-queue"></a>De wachtrij voor onbestelbare berichten

Het doel van de wachtrij voor onbestelbare berichten is voor het opslaan van berichten die naar een ontvanger kunnen niet worden bezorgd of berichten die niet kunnen worden verwerkt. Berichten kunnen vervolgens worden verwijderd uit de DLQ en gecontroleerd. Een toepassing kan met behulp van een operator wordt oplossen van problemen en verzend het bericht opnieuw, meld het feit dat er een fout is en corrigerende maatregelen nemen. 

Vanuit een perspectief API en het protocol is voornamelijk de DLQ vergelijkbaar met de wachtrij, behalve dat berichten kunnen alleen worden verzonden via de onbestelbare gebaar van de bovenliggende entiteit. Bovendien time to live is niet-naleving en u kunt een bericht van een DLQ onbestelbare niet. Volledige ondersteuning voor de wachtrij voor onbestelbare berichten peek vergrendeling levering en transactionele bewerkingen.

Houd er rekening mee dat er geen automatische opschoning van de DLQ is. Berichten blijven in de DLQ totdat u ze expliciet uit de DLQ en aanroep ophaalt [Complete()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_CompleteAsync) voor het bericht met onbestelbare berichten.

## <a name="moving-messages-to-the-dlq"></a>Verplaatsen van berichten naar de DLQ

Er zijn verschillende activiteiten in de Service Bus die ertoe leiden dat berichten naar de DLQ uit binnen de messaging-engine zelf ophalen gepusht. Een toepassing kunt u ook expliciet berichten verplaatsen naar de DLQ. 

Als het bericht wordt verplaatst met de broker, twee eigenschappen worden toegevoegd aan het bericht wanneer de broker de interne versie van roept de [wachtrij voor onbestelbare](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeadLetter_System_String_System_String_) methode voor het bericht: `DeadLetterReason` en `DeadLetterErrorDescription`.

Toepassingen kunnen hun eigen codes definiëren voor de `DeadLetterReason` eigenschap, maar het systeem stelt u de volgende waarden.

| Voorwaarde | DeadLetterReason | DeadLetterErrorDescription |
| --- | --- | --- |
| Altijd |HeaderSizeExceeded |De grootte voor deze stroom is overschreden. |
| ! TopicDescription.<br />EnableFilteringMessagesBeforePublishing en SubscriptionDescription.<br />EnableDeadLetteringOnFilterEvaluationExceptions |uitzondering. GetType(). Naam |uitzondering. Bericht |
| EnableDeadLetteringOnMessageExpiration |TTLExpiredException |Het bericht is verlopen en dode is lettered. |
| SubscriptionDescription.RequiresSession |Sessie-id is null. |Sessie ingeschakeld entiteit is niet mogelijk een bericht waarvan sessie-id null is. |
| ! wachtrij voor onbestelbare berichten |MaxTransferHopCountExceeded |Null |
| Toepassing expliciete dode lettering |Opgegeven door toepassing |Opgegeven door toepassing |

## <a name="exceeding-maxdeliverycount"></a>Meer dan MaxDeliveryCount
Wachtrijen en abonnementen hebben een [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) en [SubscriptionDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_MaxDeliveryCount) eigenschap respectievelijk; de standaardwaarde is 10. Wanneer een bericht is bezorgd onder een vergrendeling ([ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)), maar is een expliciet verlaten of de vergrendeling is verlopen, wordt het bericht [BrokeredMessage.DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) is verhoogd. Wanneer [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) overschrijdt [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount), het bericht is verplaatst naar de DLQ geven de `MaxDeliveryCountExceeded` redencode.

Dit gedrag kan niet worden uitgeschakeld, maar u kunt instellen [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) met een zeer groot aantal.

## <a name="exceeding-timetolive"></a>TimeToLive overschrijdt
Wanneer de [QueueDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) of [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnMessageExpiration) eigenschap is ingesteld op **waar** (de standaardwaarde is **false**), alle verlopende berichten worden verplaatst naar de DLQ geven de `TTLExpiredException` redencode.

Houd er rekening mee dat verlopen berichten worden alleen opgeschoond en daarom naar de DLQ verplaatst wanneer er ten minste één actieve ontvanger binnenhalen op de hoofdwachtrij of abonnement. Dit gedrag is inherent aan het ontwerp.

## <a name="errors-while-processing-subscription-rules"></a>Fouten tijdens het verwerken van abonnementsregels
Wanneer de [SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnFilterEvaluationExceptions) eigenschap voor een abonnement is ingeschakeld, eventuele fouten die zich voordoen tijdens een abonnement SQL filterregel wordt uitgevoerd in de DLQ langs worden vastgelegd met de strijdige bericht.

## <a name="application-level-dead-lettering"></a>Op toepassingsniveau verwerking van onbestelbare berichten
Naast de verwerking van onbestelbare berichten systeem geleverde functies toepassingen de DLQ gebruiken voor het expliciet weigeren onaanvaardbaar berichten. Dit omvat mogelijk berichten die correct kunnen niet worden verwerkt vanwege een of andere vorm van een systeemprobleem, berichten waarin een verkeerd ingedeelde nettoladingen of berichten die niet door de verificatie als sommige berichtniveau beveiligingsschema wordt gebruikt.

## <a name="dead-lettering-in-forwardto-or-sendvia-scenarios"></a>Verwerking van onbestelbare berichten in scenario's ForwardTo of SendVia

Berichten worden verzonden naar de wachtrij onbestelbare berichten in de volgende omstandigheden:

- Een bericht wordt doorgegeven via meer dan 3 wachtrijen of onderwerpen die [aaneengeschakeld](service-bus-auto-forwarding.md).
- De doelwachtrij of onderwerp is uitgeschakeld of verwijderd.
- De doelwachtrij of onderwerp wordt de maximale entiteit overschrijdt.

Voor het ophalen van deze lettered van onbestelbare berichten, kunt u een ontvanger met behulp van de [FormatTransferDeadletterPath](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_FormatTransferDeadLetterPath_System_String_) hulpprogramma methode.

## <a name="example"></a>Voorbeeld
Het volgende codefragment maakt een bericht ontvanger. In de receive-lus voor de hoofdwachtrij, de code haalt het bericht met [Receive(TimeSpan.Zero)](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_Receive_System_TimeSpan_), waarin wordt gevraagd de broker onmiddellijk retourneren elk bericht direct beschikbaar, of om te retourneren met geen resultaat. Als de code wordt een bericht ontvangt, het onmiddellijk verlaat deze welke stappen de `DeliveryCount`. Zodra het bericht wordt verplaatst naar de DLQ het systeem, de hoofdwachtrij leeg is en dat de lus wordt afgesloten, als [ReceiveAsync](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_ReceiveAsync_System_TimeSpan_) retourneert **null**.

```csharp
var receiver = await receiverFactory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);
while(true)
{
    var msg = await receiver.ReceiveAsync(TimeSpan.Zero);
    if (msg != null)
    {
        Console.WriteLine("Picked up message; DeliveryCount {0}", msg.DeliveryCount);
        await msg.AbandonAsync();
    }
    else
    {
        break;
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over Service Bus-wachtrijen:

* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Azure-wachtrijen en Service Bus-wachtrijen vergeleken](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

