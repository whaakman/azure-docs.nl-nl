---
title: Azure Service Bus-berichten voor prefetch | Microsoft Docs
description: De prestaties verbeteren door veelgevraagde Azure Service Bus-berichten.
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
ms.date: 01/30/2018
ms.author: spelluru
ms.openlocfilehash: e6dd30fc8da919995849ba818f608604a57a0b37
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44346823"
---
# <a name="prefetch-azure-service-bus-messages"></a>Azure Service Bus-berichten vooraf ophalen

Wanneer *Prefetch* is ingeschakeld in een van de officiële Service Bus-clients, de ontvanger stille verkrijgt meer berichten, tot de [PrefetchCount](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) limiet, dan wat de toepassing in eerste instantie gevraagd.

Een enkele initiële [ontvangen](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive) of [ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) aanroep krijgt dus een bericht voor direct gebruik dat wordt geretourneerd zo snel als beschikbaar. De client vervolgens ontvangt verdere berichten op de achtergrond te vullen van de buffer prefetch.

## <a name="enable-prefetch"></a>Prefetch inschakelen

Met .NET, u de functie voor Prefetch inschakelen door in te stellen de [PrefetchCount](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) eigenschap van een **MessageReceiver**, **QueueClient**, of **SubscriptionClient**  naar een getal groter dan nul. Als de waarde nul schakelt prefetch.

U kunt deze instelling eenvoudig toevoegen aan de ontvangstzijde van de [QueuesGettingStarted](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/QueuesGettingStarted) of [ReceiveLoop](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/ReceiveLoop) instellingen voor de voorbeelden om te zien van het effect in deze context.

Terwijl er berichten zijn beschikbaar in de buffer prefetch, kan voor elke volgende **ontvangen**/**ReceiveAsync** aanroepen onmiddellijk uit de buffer is voldaan, en de buffer wordt aangevuld de zoals ruimte beschikbaar op de achtergrond. Als er geen berichten beschikbaar voor de levering, de receive-bewerking wordt leeggemaakt de buffer en vervolgens wacht of blokken, zoals verwacht.

Prefetch werkt ook op dezelfde manier als met de [OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) en [OnMessageAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessageasync) API's.

## <a name="if-it-is-faster-why-is-prefetch-not-the-default-option"></a>Als het is sneller en waarom is Prefetch niet standaard ingeschakeld?

Prefetch versnelt de stroom door een bericht direct beschikbaar zijn voor het ophalen van lokale wanneer en voordat de toepassing wordt gevraagd om een. Dit voordeel doorvoer is het resultaat van een afweging die de auteur van de toepassing moet expliciet aanbrengen:

Met de [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modus ontvangen, worden alle berichten die zijn aangeschaft in de buffer prefetch zijn niet meer beschikbaar in de wachtrij, en alleen bevinden zich in de buffer in-memory prefetch totdat ze worden ontvangen in de toepassing via de **ontvangen**/**ReceiveAsync** of **OnMessage**/**OnMessageAsync** API's. Als de toepassing wordt beëindigd voordat de berichten worden ontvangen in de toepassing, zijn deze berichten permanent verloren gaan.

In de [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock) modus ontvangen, opgehaald in de Prefetch buffer-berichten zijn aangeschaft in de buffer in een vergrendelde status en heeft de timeout klok voor de vergrendeling blijft draaien. Als de prefetch-buffer groot is en de verwerking zo lang dat bericht vergrendelingen verlopen duurt terwijl die zich bevinden in de buffer prefetch of zelfs terwijl de toepassing het bericht wordt verwerkt, is het mogelijk dat er enkele verwarrend gebeurtenissen voor de toepassing om af te handelen.

De toepassing mogelijk een bericht met een verlopen of imminently verlopen vergrendeling verkrijgen. Als dit het geval is, kan de toepassing verwerken van het bericht, maar gaat u naar dat niet kan deze worden voltooid vanwege een verlopen vergrendeling. De toepassing kunt controleren de [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.lockeduntilutc) eigenschap (dit is onderhevig aan het tijdsverschil tussen de broker en klok van de lokale computer). Als de bericht-vergrendeling is verlopen, moet de toepassing het bericht; negeren Er zijn geen API-aanroep op of met het bericht moet worden gemaakt. Als het bericht niet is verlopen, maar vervaltijd onmiddellijk wordt, de vergrendeling kan worden vernieuwd en uitgebreid door een andere standaardperiode van vergrendeling door het aanroepen van [bericht. RenewLock()](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_RenewLockAsync_System_String_)

Als de vergrendeling op de achtergrond in de buffer prefetch verloopt, wordt het bericht wordt behandeld als afgeschaft en wordt opnieuw beschikbaar gesteld voor ophalen uit de wachtrij. Dat kan leiden tot het moet worden opgehaald in de buffer prefetch; geplaatst aan het einde. Als de prefetch-buffer kan niet tijdens de vervaldatum van het bericht meestal via worden gewerkt, dit zorgt ervoor dat berichten herhaaldelijk prefetched maar nooit effectief geleverde zich in een bruikbaar (geldige vergrendelde) staat en worden uiteindelijk verplaatst naar de dead-letter-wachtrij één keer de maximum aantal bezorgingen is overschreden.

Als u een hoge mate van betrouwbaarheid voor de verwerking van berichten moet en verwerking veel werk en het tijdstip neemt, is het raadzaam dat u de functie prefetch dan, of helemaal niet gebruiken.

Als u hoge in de gehele moet en berichtverwerking vaak goedkope is, levert prefetch doorvoer aanzienlijke voordelen.

De maximale prefetch aantal en de vergrendelingsduur van de is geconfigureerd voor de wachtrij of abonnement moeten worden verdeeld, zodat de time-out van de vergrendeling ten minste groter is dan de cumulatieve verwacht bericht verwerkingstijd voor de maximale grootte van de buffer prefetch, plus één bericht. Op hetzelfde moment, de time-out van de vergrendeling niet had zo lang dat berichten groter zijn dan de maximale [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) wanneer ze per ongeluk worden verwijderd, dus de vergrendeling verloopt voordat opnieuw wordt bezorgd vereisen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)
