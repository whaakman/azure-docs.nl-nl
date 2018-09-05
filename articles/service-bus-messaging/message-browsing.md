---
title: Azure Service Bus-berichten doorzoeken | Microsoft Docs
description: Blader en Service Bus-berichten bekijken
services: service-bus-messaging
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2018
ms.author: spelluru
ms.openlocfilehash: bafc08eae4a32f803f485097401a586a662f64e9
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700404"
---
# <a name="message-browsing"></a>Berichten doorzoeken

Bericht te bladeren ("inspecteren') kunt een Service Bus-clienttoepassing voor het inventariseren van alle berichten die zich bevinden in een wachtrij of abonnement, doorgaans voor diagnose en foutopsporing.

De peek bewerkingen retourneren alle berichten die aanwezig zijn in de wachtrij of abonnement bericht-logboek, niet alleen beschikbaar voor onmiddellijke overname met `Receive()` of de `OnMessage()` lus. De `State` eigenschap van elk bericht geeft aan of het bericht actief is (beschikbaar om te worden ontvangen), [uitgestelde](message-deferral.md), of [geplande](message-sequencing.md).

Verbruikte en verlopen berichten worden opgeschoond door een asynchrone 'garbagecollection' die is uitgevoerd en niet per se precies wanneer berichten verlopen, en daarom `Peek` kan inderdaad berichten die al zijn verlopen en wordt verwijderd of wanneer dead lettered retourneren een ontvangstbewerking wordt vervolgens aangeroepen in de wachtrij of abonnement.

Dit is vooral belangrijk in waarmee u rekening moet houden bij het herstellen van uitgestelde berichten uit de wachtrij. Een bericht waarvoor de [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc#Microsoft_Azure_ServiceBus_Message_ExpiresAtUtc) instant is verstreken is niet langer in aanmerking voor het ophalen van de reguliere op een andere manier, zelfs wanneer deze wordt geretourneerd door Peek. Deze berichten retourneren is opzettelijk, aangezien Peek is een hulpprogramma voor diagnostische gegevens over de actuele status van het logboek.

Peek retourneert ook berichten die zijn vergrendeld en momenteel wordt verwerkt door andere ontvangers, maar nog niet is voltooid. Echter, omdat Peek een momentopname van een niet-verbonden retourneert, de status van de vergrendeling van een bericht kan niet worden waargenomen berichten worden bekeken, en de [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.lockeduntilutc#Microsoft_Azure_ServiceBus_Core_MessageReceiver_LockedUntilUtc) en [LockToken](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.locktoken#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_LockToken) eigenschappen throw een [ InvalidOperationException](/dotnet/api/system.invalidoperationexception) wanneer de toepassing probeert te lezen.

## <a name="peek-apis"></a>API's bekijken

De [Peek/PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_PeekAsync) en [PeekBatch/PeekBatchAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatchasync#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatchAsync_System_Int64_System_Int32_) methoden zijn beschikbaar in alle .NET en Java-clientbibliotheken en op alle objecten van de ontvanger: **MessageReceiver**, **MessageSession**, **QueueClient**, en **SubscriptionClient**. Werkt op alle wachtrijen en abonnementen en de desbetreffende wachtrijen voor onbestelbare berichten bekijken.

Als meerdere keren aangeroepen, wordt met de Peek-methode alle berichten die zijn opgenomen in de wachtrij of abonnement logboek, in volgorde met nummer, uit het laagste beschikbaar volgnummer op de hoogste waarde inventariseert. Dit is de volgorde waarin berichten in wachtrij gezet; zijn het is niet de volgorde waarin berichten mogelijk uiteindelijk worden opgehaald.

[PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) meerdere berichten ophaalt en retourneert ze als een opsomming. Als er geen berichten beschikbaar zijn, wordt het opsommingsobject is leeg, niet null zijn.

U kunt ook een overbelasting van de methode met seed een [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) op waarop u wilt starten en vervolgens de overload parameterloze methode om te inventariseren verder aan te roepen. **PeekBatch** oftewel functies, maar een set berichten in één keer worden opgehaald.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)