---
title: Azure Service Bus prefetch berichten | Microsoft Docs
description: De prestaties verbeteren door veelgevraagde Azure Service Bus-berichten.
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
ms.date: 10/04/2017
ms.author: sethm
ms.openlocfilehash: 4a4a06f90c2c48d35d836f0be89fec9cc47f32c0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="prefetch-azure-service-bus-messages"></a>Prefetch Azure Service Bus-berichten

Wanneer *Prefetch* is ingeschakeld in een van de officiële Service Bus-clients, de ontvanger stil verkrijgt meer berichten tot de [PrefetchCount](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) limiet, afgezien van wat de toepassing in eerste instantie gevraagd.

Een enkele initiële [ontvangen](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive) of [ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) aanroep verkrijgt daarom een bericht voor onmiddellijke consumptie die wordt geretourneerd zo snel als beschikbaar. De client vervolgens ontvangt verdere berichten op de achtergrond voor het vervullen van de buffer prefetch.

## <a name="enable-prefetch"></a>Prefetch inschakelen

In .NET, schakelt u de functie Prefetch door in te stellen de [PrefetchCount](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) eigenschap van een **MessageReceiver**, **QueueClient**, of **SubscriptionClient**  naar een getal groter dan nul. Als de waarde nul schakelt prefetch.

U kunt deze instelling eenvoudig toevoegen aan de receive-zijde van de [QueuesGettingStarted](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/QueuesGettingStarted) of [ReceiveLoop](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/ReceiveLoop) voorbeelden de instellingen voor een overzicht van het effect in deze context.

Berichten zijn beschikbaar in de buffer prefetch alle daaropvolgende **ontvangen**/**ReceiveAsync** aanroepen onmiddellijk uit de buffer is voldaan, en de buffer wordt aangevuld de Zodra er ruimte beschikbaar op de achtergrond. Als er geen berichten beschikbaar voor de levering van zijn, de ontvangstbewerking wordt leeggemaakt de buffer en wacht of blokken bij, zoals verwacht.

Prefetch werkt ook op dezelfde manier als met de [OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) en [OnMessageAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessageasync) API's.

## <a name="if-it-is-faster-why-is-prefetch-not-the-default-option"></a>Als het is sneller en waarom Prefetch niet de standaardoptie?

Prefetch sneller de berichtenstroom wanneer er een bericht direct beschikbaar is voor het ophalen van lokale wanneer en voordat de toepassing wordt gevraagd om een. Het voordeel van deze doorvoer is het resultaat van een afweging beslissing die de auteur van de toepassing moet expliciet aanbrengen:

Met de [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode.receiveanddelete) modus ontvangen, worden alle berichten die zijn verkregen in de buffer prefetch zijn niet meer beschikbaar in de wachtrij en alleen bevinden zich in de buffer in het geheugen prefetch totdat ze worden ontvangen in de toepassing via de **ontvangen**/**ReceiveAsync** of **OnMessage**/**OnMessageAsync** API's. Als de toepassing wordt beëindigd voordat de berichten worden ontvangen in de toepassing, zijn deze berichten permanent verloren.

In de [PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode.peeklock) modus ontvangen, berichten in de buffer Prefetch opgehaalde zijn verkregen in de buffer in een vergrendelde status en de klok time-out voor de vergrendeling tikken hebben. Als de prefetch-buffer groot is en verwerking te lang waardoor bericht vergrendelingen duurt tijdens die zich in de buffer prefetch of zelfs terwijl de toepassing het bericht wordt verwerkt, is er mogelijk enkele verwarrend gebeurtenissen voor de toepassing om te verwerken.

De toepassing mogelijk een bericht met een verlopen of imminently verlopende vergrendeling verkrijgen. Zo ja, de toepassing kan verwerken van het bericht, maar gaat u naar dat niet kan deze worden voltooid vanwege het verlopen van een vergrendeling. De toepassing kunt controleren de [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.lockeduntilutc#Microsoft_Azure_ServiceBus_Core_MessageReceiver_LockedUntilUtc) eigenschap (dit is onderworpen aan het tijdsverschil tussen de broker en klok van de lokale computer). Als de vergrendeling van het bericht is verlopen, de toepassing moet worden gebruikt voor het negeren van het bericht. Er is geen API-aanroep of met het bericht moet worden aangebracht. Als het bericht niet is verlopen, maar verlopen handen is, de vergrendeling kan worden vernieuwd en uitgebreid door een andere standaardperiode voor vergrendeling door aan te roepen [bericht. RenewLock()](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_RenewLockAsync_System_String_)

Als de vergrendeling achtergrond in de buffer prefetch verloopt, wordt het bericht wordt behandeld als afgeschaft en wordt opnieuw beschikbaar gesteld voor ophalen uit de wachtrij. Dat kan ertoe leiden dat deze worden opgehaald in de buffer prefetch; aan het einde geplaatst. Als de prefetch-buffer kan niet tijdens de verloopdatum voor het bericht meestal via worden gewerkt, hierdoor berichten herhaaldelijk prefetched maar nooit effectief geleverde zich in een bruikbaar (geldige vergrendelde) staat en uiteindelijk verplaatst naar de wachtrij voor onbestelbare berichten eenmaal de levering van het maximum aantal is overschreden.

Als u een hoge mate van betrouwbaarheid nodig voor de verwerking van berichten en verwerking aanzienlijke werkbelasting en de tijd duurt, wordt het aanbevolen dat u de functie prefetch conservatieve of helemaal niet.

Als u hoog in de gehele moet en berichtverwerking vaak goedkope is, levert prefetch doorvoer aanzienlijke voordelen.

Het aantal maximale prefetch en de duur van de vergrendeling is geconfigureerd voor de wachtrij of een abonnement moeten worden verdeeld, zodat de time-out van de vergrendeling ten minste overschrijdt de cumulatieve verwacht bericht verwerkingstijd voor de maximale grootte van de buffer prefetch, plus één bericht. Op hetzelfde moment de time-out van de vergrendeling zou niet moeten zo lang dat berichten groter zijn dan de maximale [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) wanneer ze per ongeluk worden verwijderd, waardoor de vergrendeling verloopt voordat opnieuw wordt bezorgd vereisen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichtenservice, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)
