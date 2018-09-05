---
title: Azure Service Bus-berichtvolgorde en -timestamps | Microsoft Docs
description: Volgorde van Service Bus-bericht en volgorde met tijdstempels behouden
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
ms.openlocfilehash: 7f31adcd7ebdd51cf930fcaf0cd2f214c7566565
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43699366"
---
# <a name="message-sequencing-and-timestamps"></a>Berichtvolgorde en -timestamps

Sequentiëren en tijdstempel zijn twee functies die altijd zijn ingeschakeld op alle Service Bus-entiteiten en surface via de [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber) en [EnqueuedTimeUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc) eigenschappen van ontvangen of bekeken berichten.

Voor gevallen waarin absolute volgorde van berichten belangrijk is en/of waarin moet een gebruiker een betrouwbare unieke id voor berichten, de broker stempels berichten met een gat gratis, verhogen volgnummer ten opzichte van de wachtrij of onderwerp. Voor gepartitioneerde entiteiten, is het volgnummer uitgegeven ten opzichte van de partitie.

De **SequenceNumber** waarde is een unieke 64-bits geheel getal zijn toegewezen aan een bericht wanneer ze worden geaccepteerd en door de broker en functies als de interne id opgeslagen. Voor gepartitioneerde entiteiten weerspiegelen de bovenste 16 bits de partitie-id. Volgnummers meenemen naar nul wanneer de 48/64-bits-bereik is verbruikt.

Het volgnummer kan worden vertrouwd als een unieke id omdat deze is toegewezen door een CA-centraal en neutrale en niet door clients. Deze ook vertegenwoordigt de waarde true volgorde van aankomst van en nauwkeuriger is dan een tijdstempel als een criterium volgorde omdat tijdstempels geen een hoog genoeg resolutie extreme bericht tarieven en zijn mogelijk onderhevig aan (maar minimale) tijdsverschil in situaties waar de broker eigendom van omgevingen tussen knooppunten.

De absolute aankomst volgorde belangrijk is, bijvoorbeeld in zakelijke scenario's waarin een beperkt aantal goederen aangeboden worden verwerkt op basis van first-komen-eerst maalt zolang de voorraad strekt laatst; concert kaartverkoop vormen een voorbeeld.

De mogelijkheid tijdstempel fungeert als een neutrale en betrouwbaar (CA) die nauwkeurig de UTC-tijd van aankomst van een bericht weergegeven legt in de **EnqueuedTimeUtc** eigenschap. De waarde is handig als een bedrijfsscenario is afhankelijk van deadlines, zoals of een werkitem is ingediend op een bepaalde datum vóór middernacht, maar de verwerking is uiterst achter de achterstand in de replicatiewachtrij.

## <a name="scheduled-messages"></a>Geplande berichten

U kunt berichten naar een wachtrij of onderwerp voor vertraagde verwerking; indienen Als u bijvoorbeeld voor het plannen van een taak weer beschikbaar voor verwerking door een systeem op een bepaalde periode. Deze mogelijkheid realiseert een betrouwbare gedistribueerde planner die op basis van tijd.

Geplande berichten doen niet realiseren in de wachtrij tot het tijdstip gedefinieerd in de wachtrij plaatsen. Vóór die tijd kunnen geplande berichten worden geannuleerd. Annulering wordt het bericht verwijderd.

U kunt plannen dat berichten door in te stellen de [ScheduledEnqueueTimeUtc](/dotnet/api/microsoft.azure.servicebus.message.scheduledenqueuetimeutc) eigenschap bij het verzenden van een bericht dat het pad naar de normale verzenden of expliciet met de [ScheduleMessageAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync#Microsoft_Azure_ServiceBus_QueueClient_ScheduleMessageAsync_Microsoft_Azure_ServiceBus_Message_System_DateTimeOffset_) API. De laatste retourneert onmiddellijk het geplande bericht **SequenceNumber**, die u later kunt gebruiken om te annuleren de gepland bericht, indien nodig. Geplande berichten en hun volgnummers kunnen ook worden gedetecteerd met behulp van [door berichten bladert](message-browsing.md).

De **SequenceNumber** voor een gepland bericht is alleen geldig bij het bericht is in deze status. Als de bericht-overgangen naar de actieve status, het bericht wordt toegevoegd aan de wachtrij als waren in de wachtrij op de huidige chatberichten, waaronder het toewijzen van een nieuwe **SequenceNumber**.

Omdat de functie is verankerd op afzonderlijke berichten en berichten kunnen in de wachtrij alleen één keer worden, ondersteunt Service Bus geen terugkerende schema's voor berichten.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)