---
title: Asynchrone Service Bus-berichtenservice | Microsoft Docs
description: Beschrijving van Azure Service Bus asynchrone berichten.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f1435549-e1f2-40cb-a280-64ea07b39fc7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 50778ae742c1ec66857a6c2fa6250dc3d67e5601
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301567"
---
# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynchrone berichtpatronen en hoge beschikbaarheid

Asynchrone berichten kan worden geïmplementeerd in een aantal verschillende manieren. Azure Service Bus ondersteunt met wachtrijen, onderwerpen en abonnementen, asynchronism via een winkel en een mechanisme voor doorsturen. Verzenden van berichten met wachtrijen en onderwerpen in de normale werking (synchrone) en ontvangen van berichten van wachtrijen en abonnementen. Toepassingen die u schrijft, is afhankelijk van deze entiteiten die altijd beschikbaar worden gesteld. Wanneer de entiteitsstatus wordt gewijzigd, vanwege allerlei omstandigheden, moet u een manier om aan te bieden van een entiteit met verminderde functionaliteit dat is voor de meeste behoeften geschikt.

Asynchrone berichtpatronen toepassingen meestal gebruiken om in te schakelen van een aantal scenario's voor communicatie. U kunt toepassingen waarin clients berichten naar services, verzenden kunnen zelfs wanneer de service wordt niet uitgevoerd op te bouwen. Voor toepassingen die ervaring pieken in communicatie, een wachtrij kunt niveau van de belasting door te geven van een interne buffer-communicatie. Ten slotte, krijgt u een eenvoudige maar effectieve load balancer om berichten te verdelen over meerdere machines.

U kunt een aantal verschillende manieren waarop deze entiteiten kunnen worden weergegeven is niet beschikbaar voor een duurzaam berichtensysteem gehandhaafd beschikbaarheid van elk van deze entiteiten. In het algemeen zien we de entiteit niet meer beschikbaar zijn voor toepassingen die we in de volgende manieren schrijven:

* Kan het verzenden van berichten.
* Kan geen berichten ontvangen.
* Kan geen entiteiten beheren (maken, ophalen, bijwerken of verwijderen van entiteiten).
* Kan geen contact op met de service.

Voor elk van deze fouten bestaan verschillende foutmodi waarmee een toepassing om door te gaan om uit te voeren werk op basis van bepaalde mate van verminderde functionaliteit. Bijvoorbeeld, een systeem dat u kunt berichten verzenden, maar deze niet meer ontvangen kunt nog steeds orders van klanten ontvangen maar deze orders kan niet worden verwerkt. In dit onderwerp worden de mogelijke problemen die kunnen optreden, en hoe die problemen zijn verholpen. Service Bus heeft een aantal oplossingen die u moet kiezen voor geïntroduceerd en in dit onderwerp worden ook de regels voor het gebruik van deze oplossingen aanmelden.

## <a name="reliability-in-service-bus"></a>Betrouwbaarheid in Servicebus
Er zijn verschillende manieren voor het afhandelen van problemen met het bericht en de entiteit en er zijn richtlijnen voor het juiste gebruik van deze oplossingen. Voor meer informatie over de richtlijnen, moet u eerst te begrijpen wat in Service Bus kan mislukken. Vanwege het ontwerp van de Azure-systemen vaak al deze problemen worden tijdelijke. Op hoog niveau uitzien de andere oorzaken van niet beschikbaar zijn als volgt:

* Beperking van een extern systeem waarop Service Bus hangt ervan af. Beperking optreedt van de interacties met de opslag en rekenresources.
* Probleem voor een systeem waarop Service Bus hangt ervan af. Een onderdeel van de opslag kan bijvoorbeeld problemen optreden.
* Fout van Service Bus op één subsysteem. In dit geval een rekenknooppunt kunt krijgen in een inconsistente toestand en moet opnieuw worden opgestart, waardoor alle entiteiten die het resultaat wordt gebruikt om taken te verdelen naar andere knooppunten. Dit kan een korte periode van trage berichtverwerking op zijn beurt veroorzaken.
* Fout van Service Bus in een Azure-datacenter. Dit is een 'Onherstelbare fout"gedurende welke het systeem niet bereikbaar voor het aantal minuten of een paar uur is.

> [!NOTE]
> De term **opslag** kan betekenen zowel Azure Storage en SQL Azure.
> 
> 

Service Bus bevat een aantal oplossingen voor deze problemen. De volgende secties worden besproken elk probleem en hun respectieve oplossingen.

### <a name="throttling"></a>Beperking
Met Service Bus kunt beperking gezamenlijke bericht tarief management. Elke afzonderlijke Service Bus-knooppunt ook veel entiteiten nieuwste. Elk van deze entiteiten kunt eisen op het systeem in termen van CPU, geheugen, opslag en andere facetten. Wanneer een van deze facetten gebruik detecteert die groter is dan gedefinieerde drempelwaarden, Service Bus een bepaalde aanvraag kunt weigeren. De oproepende functie ontvangt een [ServerBusyException] [ ServerBusyException] en nieuwe pogingen na tien seconden.

Als een beperking, moet de code de fout bij het lezen en stoppen van een nieuwe pogingen van het bericht ten minste 10 seconden. Omdat de fout op verschillende onderdelen van de Klanttoepassing plaatsvinden kan, is het waarschijnlijk dat elk onafhankelijk wordt uitgevoerd de logica voor opnieuw proberen. De code kunt verminderen de kans wordt beperkt door in te schakelen op een wachtrij of onderwerp partitioneren.

### <a name="issue-for-an-azure-dependency"></a>Probleem met een Azure-afhankelijkheid
Andere onderdelen in Azure kunnen van tijd tot tijd hebben problemen met de service. Bijvoorbeeld, wanneer een systeem dat gebruikmaakt van Service Bus wordt bijgewerkt, kan dat systeem tijdelijk ervaren beperkte mogelijkheden. Als tijdelijke oplossing voor dit soort problemen, Service Bus regelmatig onderzoekt het probleem en oplossingen implementeert. Neveneffecten van deze oplossingen worden ook weergegeven. Voor het afhandelen van tijdelijke problemen met opslag, bijvoorbeeld: implementeert Service Bus een systeem dat Hiermee kunt u bewerkingen van bericht verzenden naar het consistent worden gewerkt. Vanwege de aard van de oplossing, kan een verzonden bericht tot 15 minuten in de betreffende wachtrij of abonnement wordt weergegeven en gereed voor een ontvangstbewerking duren. De meeste entiteiten wordt dit probleem in het algemeen niet optreden. Echter, gezien het aantal entiteiten in Service Bus in Azure, deze beperking is soms nodig voor een kleine subset van Service Bus-klanten.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Service Bus-fout op een enkele subsysteem
Met elke toepassing omstandigheden kunnen leiden tot een interne onderdeel van Service Bus inconsistent worden. Wanneer Service Bus gedetecteerd, worden gegevens verzameld van de toepassing om te helpen bij het vaststellen van wat is er gebeurd. Zodra de gegevens zijn verzameld, kan de toepassing wordt opnieuw gestart in een poging om een consistente status weer. Dit proces vrij snel wordt uitgevoerd, en resultaten in een entiteit zijn niet beschikbaar voor maximaal een paar minuten, maar typische uitvaltijden lijkt veel korter.

In dergelijke gevallen de clienttoepassing genereert een [System.TimeoutException] [ System.TimeoutException] of [MessagingException] [ MessagingException] uitzondering. Service Bus bevat een beperking voor dit probleem in de vorm van logica voor opnieuw proberen van geautomatiseerde client. Zodra de tijd tussen elke poging is verbruikt en het bericht is niet afgeleverd, u kunt verkennen met behulp van andere vermeld in het artikel op [uitval en noodgevallen afhandelen][handling outages and disasters].

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt de basisbeginselen van asynchrone berichten in Service Bus, Raadpleeg voor meer informatie over [uitval en noodgevallen afhandelen][handling outages and disasters].

[ServerBusyException]: /dotnet/api/microsoft.servicebus.messaging.serverbusyexception
[System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Microsoft.ServiceBus.Messaging.MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[MessageReceiver]: /dotnet/api/microsoft.servicebus.messaging.messagereceiver
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[EnableSyphon]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[System.TimeSpan.Zero]: https://msdn.microsoft.com/library/system.timespan.zero.aspx
[IsTransient]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[UnauthorizedAccessException]: https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx
[BacklogQueueCount]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions?redirectedfrom=MSDN
[handling outages and disasters]: service-bus-outages-disasters.md
