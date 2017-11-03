---
title: Azure Service Bus-bericht bladeren | Microsoft Docs
description: Bladeren en inspecteren van Service Bus-berichten
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
ms.date: 09/27/2017
ms.author: sethm
ms.openlocfilehash: b0bc1ef7570ccac07975e2560a1d0501d3cde2b3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="message-browsing"></a>Bericht bladeren

Bericht bladeren door ('weergeven') kan clients die zich in een wachtrij of een abonnement, meestal voor diagnose en foutopsporing alle berichten inventariseren.

De bewerkingen peek retourneert alle berichten die bestaan in het berichtenlogboek wachtrij of abonnement, niet alleen beschikbaar voor onmiddellijke aankoop met *Receive()* of de *OnMessage()* lus. De *status* eigenschap van elk bericht geeft aan of het bericht is geactiveerd (beschikbaar om te worden ontvangen), uitgestelde (Zie uitgestelde [koppeling nog te bepalen]) of geplande (Zie geplande berichten [koppeling nog te bepalen]).

Verbruikte en verlopen berichten worden opgeschoond door een asynchrone 'garbagecollection' uitvoeren en niet per se precies wanneer berichten verlopen en daarom Peek inderdaad berichten die al zijn verlopen en zal worden verwijderd of wanneer een receive onbestelbare lettered retourneren bewerking wordt vervolgens aangeroepen op de wachtrij of abonnement.

Dit is vooral belangrijk rekening moet houden bij het herstellen van uitgestelde berichten uit de wachtrij. Een bericht waarvoor de [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc#Microsoft_Azure_ServiceBus_Message_ExpiresAtUtc) instant is doorgegeven is niet meer in aanmerking komen voor reguliere ophalen op een andere manier, zelfs wanneer deze wordt geretourneerd door Peek. Deze berichten retourneren is opzettelijk, aangezien Peek is een hulpprogramma voor diagnostische gegevens als gevolg van de huidige status van het logboek.

Inspecteren ook retourneert berichten die zijn vergrendeld en door andere ontvangers momenteel wordt verwerkt, maar nog niet is voltooid. Echter, omdat Peek een momentopname van een niet-verbonden retourneert, de status van de vergrendeling van een bericht kan niet worden geobserveerd op gepeekt berichten, en de [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.lockeduntilutc#Microsoft_Azure_ServiceBus_Core_MessageReceiver_LockedUntilUtc) en [LockToken](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.locktoken#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_LockToken) eigenschappen throw een [ InvalidOperationException](/dotnet/api/system.invalidoperationexception) wanneer de toepassing probeert te lezen.

## <a name="peek-apis"></a>API's bekijken

De [Peek/PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_PeekAsync) en [PeekBatch/PeekBatchAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatchasync#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatchAsync_System_Int64_System_Int32_) methoden bestaan in de clientbibliotheken van alle .NET en Java en op alle objecten van de ontvanger: **MessageReceiver**, **MessageSession**, **QueueClient**, en **SubscriptionClient**. Inspecteren werkt op alle wachtrijen en abonnementen en hun respectieve wachtrijen voor onbestelbare berichten.

Wanneer meerdere keren aangeroepen, wordt in de methode Peek alle berichten die zijn opgenomen in de wachtrij of abonnement aanmelden, sequence number volgorde, van het laagste beschikbaar volgnummer op de hoogste waarde inventariseren. Dit is de volgorde waarin berichten in wachtrij gezet; zijn het is niet de volgorde waarin uiteindelijk berichten kunnen worden opgehaald.

[PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) meerdere berichten ophaalt en retourneert ze als een opsomming. Als er geen berichten beschikbaar zijn, wordt het opsommingsobject is leeg, niet-null.

U kunt ook een overbelasting van de methode met seed een [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) waarop u wilt starten en vervolgens de parameterloze methode-overload opsommen verder aan te roepen. **PeekBatch** oftewel functioneert, maar een aantal berichten in één keer worden opgehaald.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichtenservice, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)