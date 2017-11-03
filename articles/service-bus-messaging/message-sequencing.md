---
title: Azure Service Bus-berichtvolgorde en tijdstempels | Microsoft Docs
description: Service Bus-bericht sequence en volgorde met tijdstempels behouden
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
ms.date: 09/28/2017
ms.author: sethm
ms.openlocfilehash: e97bdd645ef2a3efba83e3f87114c998f9a9e344
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="message-sequencing-and-timestamps"></a>Berichtvolgorde en tijdstempels

Sequentiëren en timestamping zijn twee functies die altijd is ingeschakeld op alle Service Bus-entiteiten en surface via de [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber) en [EnqueuedTimeUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc) eigenschappen van ontvangen of bekeken berichten.

Voor gevallen waarin absolute volgorde van berichten is belangrijk en/of waarin een consumer moet een betrouwbaar unieke id voor berichten, de broker stempels berichten met een gap vrijmaken, verhogen volgnummer ten opzichte van de wachtrij of onderwerp. Het volgnummer is uitgegeven voor gepartitioneerde entiteiten ten opzichte van de partitie.

De **SequenceNumber** waarde is een uniek 64-bits geheel getal toegewezen aan een bericht wanneer ze worden geaccepteerd en door de broker en functies als de interne id opgeslagen. Gepartitioneerde entiteiten weerspiegelen de bovenste 16-bits de partitie-id. Volgnummers overschakelen naar nul wanneer de 48/64-bits-bereik is verbruikt.

Het volgnummer kan worden vertrouwd als een unieke id nadat deze is toegewezen door een instantie van de centrale en neutrale en niet door clients. Deze ook de waar volgorde van de aankomst vertegenwoordigt en nauwkeuriger is dan een tijdstempel een criterium volgorde omdat tijdstempels geen een hoog genoeg resolutie volgens de tarieven voor extreme bericht en mogelijk onderhevig aan (echter minimale) tijdsverschil in situaties waar de broker eigendom transities tussen knooppunten.

De absolute aankomst acties, bijvoorbeeld in zakelijke scenario's waarin een beperkt aantal goederen aangeboden worden behandeld op een eerste-afkomstig zijn-eerst maalt tijdens leveringen laatste; overleg ticket verkoop zijn een voorbeeld.

De mogelijkheid tijdstempel fungeert als een neutrale en betrouwbare autoriteit waarmee nauwkeurig vastgelegd de UTC-tijd van de aankomst van een bericht weergegeven in de **EnqueuedTimeUtc** eigenschap. De waarde is handig als een bedrijfsscenario is afhankelijk van deadlines, zoals of een werkitem is ingediend op een bepaalde datum vóór middernacht, maar de verwerking is ver achter de wachtrijachterstand.

## <a name="scheduled-messages"></a>Geplande berichten

U kunt verzenden berichten naar een wachtrij of onderwerp voor de vertraagde verwerking; Als u bijvoorbeeld een taak weer beschikbaar voor verwerking door een systeem op een bepaald moment plannen. Deze mogelijkheid realiseert een betrouwbare gedistribueerde op basis van tijd scheduler.

Geplande berichten geven komen niet aan in de wachtrij totdat de gedefinieerd in de wachtrij plaatsen. Voordat u deze periode kan kunnen geplande berichten worden geannuleerd. Annulering verwijdert het bericht.

U kunt plannen dat berichten door in te stellen de [ScheduledEnqueueTimeUtc](/dotnet/api/microsoft.azure.servicebus.message.scheduledenqueuetimeutc) eigenschap bij het verzenden van een bericht dat het pad naar de normale verzenden of expliciet met de [ScheduleMessageAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync#Microsoft_Azure_ServiceBus_QueueClient_ScheduleMessageAsync_Microsoft_Azure_ServiceBus_Message_System_DateTimeOffset_) API. De laatste retourneert onmiddellijk het geplande bericht **SequenceNumber**, waarin u kunt later de geplande bericht annuleren indien nodig. Geplande berichten en hun volgnummers kunnen ook worden gedetecteerd met [bericht Bladeren](message-browsing.md).

De **SequenceNumber** voor een geplande bericht is alleen geldig bij het bericht is in deze status. Als de bericht-overgangen naar de actieve status, het bericht wordt toegevoegd aan de wachtrij als waren in de wachtrij op de huidige instant, waaronder het toewijzen van een nieuwe **SequenceNumber**.

Omdat de functie wordt verankerd op afzonderlijke berichten en berichten kunnen in de wachtrij alleen één keer worden, ondersteunt Service Bus geen terugkerende schema's voor berichten.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichtenservice, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)