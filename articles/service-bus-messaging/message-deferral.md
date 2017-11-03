---
title: Azure Service Bus bericht uitgestelde | Microsoft Docs
description: De bezorging van Service Bus-berichten
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: sethm
ms.openlocfilehash: f84b870de4b79399d5edc90284c9c56222156b5d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="message-deferral"></a>Uitgesteld bericht

Een wachtrij of abonnement client ontvangt wanneer een bericht dat kunnen worden verwerkt, maar voor waar de bewerking momenteel niet mogelijk vanwege speciale omstandigheden binnen de toepassing is, de optie heeft 'uitstellende' voor het ophalen van het bericht naar een later tijdstip. Blijft het bericht in de wachtrij of een abonnement, maar deze wordt ingesteld.

Uitgestelde is een functie die speciaal voor scenario's voor verwerking van de werkstroom. Werkstroom frameworks mogelijk bepaalde bewerkingen moeten worden verwerkt in een bepaalde volgorde, en mogelijk moet u de verwerking van een aantal ontvangen berichten uitstellen tot voorgeschreven voorafgaande werk die door andere berichten is op de hoogte is voltooid.

Een voorbeeld van een eenvoudige voorbeelden voor dit is een reeks waarin een melding betaling van een externe betalingsprovider wordt weergegeven in een systeem voordat de overeenkomende inkooporder is doorgegeven vanaf het begin van de store op het systeem vervulling verwerken order. In dat geval kan het systeem vervulling uitstellen de melding betaling worden verwerkt totdat een bepaalde volgorde te koppelen. De volgorde realtime worden uitgevoerd inderdaad mogelijk juist in rendezvous-scenario's, waar berichten van verschillende bronnen station een werkstroom doorsturen, maar de berichten die als gevolg van de resultaten mogelijk een verkeerde volgorde binnenkomen.

Uiteindelijk uitstel helpt opnieuw rangschikken van berichten van de volgorde van de aankomst in de volgorde waarin ze kunnen worden verwerkt, terwijl deze berichten veilig in het archief voor welke verwerking moet worden uitgesteld.

## <a name="message-deferral-apis"></a>Bericht uitgestelde API 's

De API is [BrokeredMessage.Defer](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.defer?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_Defer) of [BrokeredMessage.DeferAsync](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deferasync?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeferAsync) in de .NET Framework-client [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) in de standaard .NET-client en **mesageReceiver.defer** of **messageReceiver.deferSync** in de Java-client. 

Uitgestelde berichten blijven in de hoofdwachtrij samen met andere actieve berichten (in tegenstelling tot onbestelbare berichten die bevinden zich in een submap wachtrij), maar ze kunnen niet meer worden ontvangen met de reguliere ontvangen/ReceiveAsync-functies. Uitgestelde berichten kunnen worden gedetecteerd [bericht Bladeren](message-browsing.md) als een toepassing verliest voor het bijhouden van beide.

Voor het ophalen van een uitgestelde bericht, de 'eigenaar' is verantwoordelijk voor het Onthoud de [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) omdat deze het past omleiding. Een ontvanger die kent de **SequenceNumber** van een uitgestelde bericht kan later ontvangen het bericht expliciet met Receive(sequenceNumber).

Als u een bericht kan niet worden verwerkt omdat een bepaalde bron voor het verwerken van dat bericht tijdelijk niet beschikbaar is, maar berichtverwerking moet niet summarily worden onderbroken, een elegante manier te plaatsen dat bericht aan de kant voor een paar minuten is te onthouden van de **SequenceNumber** in een [geplande bericht](message-sequencing.md) worden geplaatst in een paar minuten en opnieuw ophalen van het uitgesteld bericht wanneer de geplande bericht binnenkomt. Als een berichtenhandler afhankelijk van een database voor alle bewerkingen is, en dat de database tijdelijk niet beschikbaar is, het mag geen uitgestelde gebruiken, maar in plaats van te onderbreken ontvangen van berichten helemaal totdat de database weer beschikbaar is.

Berichten uitstellende heeft geen gevolgen voor de verloopdatum voor het bericht, wat betekent dat uitgestelde berichten nog steeds op de eerste geplande tijd verlopen en in de wachtrij met onbestelbare berichten worden verplaatst als zodanig worden geconfigureerd.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichtenservice, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)