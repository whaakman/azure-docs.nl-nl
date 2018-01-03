---
title: Azure Service Bus gekoppeld naamruimten | Microsoft Docs
description: Implementatiegegevens gekoppelde naamruimte en de kosten
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 2440c8d3-ed2e-47e0-93cf-ab7fbb855d2e
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/21/2017
ms.author: sethm
ms.openlocfilehash: f16c65286b0aa079889c9d53e98bf54e3d57c95f
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Implementatiegegevens naamruimte gekoppeld en de gevolgen kosten

De [PairNamespaceAsync] [ PairNamespaceAsync] methode, met behulp van een [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] exemplaar, zichtbaar taken worden uitgevoerd op uw naam. Omdat er worden kosten overwegingen wanneer u de functie gebruikt, is het handig om te begrijpen die taken zodat u het gedrag verwachten wanneer dit gebeurt. De API stelt de volgende automatische gedrag namens jou:

* Het maken van achterstand wachtrijen.
* Het maken van een [MessageSender] [ MessageSender] -object dat wordt gesproken naar wachtrijen en onderwerpen.
* Wanneer een Berichtentiteit niet beschikbaar is, ping verzendt berichten naar de entiteit in een poging om te detecteren die entiteit weer beschikbaar is.
* Optioneel wordt gemaakt van een reeks 'bericht pompen' verplaatsen van berichten uit de wachtrij achterstand in de primaire wachtrijen.
* Coördineert/afgesloten sluiten van de primaire en secundaire [MessagingFactory] [ MessagingFactory] exemplaren.

Op een hoog niveau de functie werkt als volgt: wanneer de primaire entiteit in orde is, er geen gedragswijzigingen doorgevoerd. Wanneer de [FailoverInterval] [ FailoverInterval] duur is verstreken, en de primaire entiteit ziet niet geslaagde verzendt na een tijdelijke [MessagingException] [ MessagingException] of een [TimeoutException][TimeoutException], gebeurt het volgende:

1. Verzenden naar de primaire entiteit zijn uitgeschakeld en het systeem pingt de primaire entiteit totdat pings kunnen worden bezorgd.
2. Een wachtrij willekeurige achterstand is geselecteerd.
3. [BrokeredMessage] [ BrokeredMessage] objecten worden doorgestuurd naar de wachtrij gekozen achterstand.
4. Als een verzendbewerking naar de wachtrij gekozen achterstand mislukt, die wachtrij opgehaald uit de draaihoek en een nieuwe wachtrij is geselecteerd. Alle afzenders op de [MessagingFactory] [ MessagingFactory] exemplaar meer van de fout.

De volgende afbeeldingen weer de reeks. De afzender verzendt eerst berichten.

![Gekoppelde naamruimten][0]

Bij een fout te verzenden naar de primaire wachtrij van de afzender begint het verzenden van berichten naar een willekeurig gekozen achterstand wachtrij. Tegelijk, wordt er een ping-taak gestart.

![Gekoppelde naamruimten][1]

De berichten die op dit moment worden nog steeds in de secundaire wachtrij en niet naar de primaire wachtrij zijn geleverd. Zodra de primaire wachtrij weer in orde is, moet ten minste één proces de syphon worden uitgevoerd. De syphon biedt de berichten van de verschillende achterstand wachtrijen op de juiste bestemming entiteiten (wachtrijen en onderwerpen).

![Gekoppelde naamruimten][2]

De rest van dit onderwerp wordt de specifieke details over de werking van deze onderdelen beschreven.

## <a name="creation-of-backlog-queues"></a>Maken van wachtrijen achterstand
De [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] -object doorgegeven aan de [PairNamespaceAsync] [ PairNamespaceAsync] methode geeft het aantal achterstand wachtrijen die u wilt gebruiken. Elke wachtrij achterstand wordt vervolgens gemaakt met de volgende eigenschappen expliciet ingesteld (alle andere waarden zijn ingesteld op de [QueueDescription] [ QueueDescription] standaardwaarden):

| Pad | [primaire namespace] / x-servicebus-overdracht / [index] waarbij [index] is een waarde in [0, BacklogQueueCount) |
| --- | --- |
| MaxSizeInMegabytes |5120 |
| maxDeliveryCount |int zijn. MaxValue |
| DefaultMessageTimeToLive |TimeSpan.MaxValue op |
| AutoDeleteOnIdle |TimeSpan.MaxValue op |
| LockDuration |1 minuut |
| EnableDeadLetteringOnMessageExpiration |waar |
| EnableBatchedOperations |waar |

Bijvoorbeeld de eerste achterstand wachtrij gemaakt voor naamruimte **contoso** heet `contoso/x-servicebus-transfer/0`.

Bij het maken van de wachtrijen, de code eerst gecontroleerd of deze wachtrij bestaat. De wachtrij wordt gemaakt als de wachtrij niet bestaat. De code niet opschonen 'extra' achterstand wachtrijen. In het bijzonder als de toepassing met de primaire naamruimte **contoso** aanvragen vijf achterstand wachtrijen, maar een achterstand wachtrij met het pad `contoso/x-servicebus-transfer/7` bestaat, die extra achterstand wachtrij nog steeds aanwezig is, maar wordt niet gebruikt. Het systeem kan expliciet extra achterstand wachtrijen bestaan die niet worden gebruikt. Als de eigenaar van de naamruimte bent u verantwoordelijk voor het opschonen van ongebruikte/ongewenste achterstand wachtrijen. De reden voor deze beslissing is dat Service Bus kan niet weet welke doelen voor alle wachtrijen in uw naamruimte bestaat. Bovendien, als een wachtrij met de gegeven naam bestaat, maar niet aan de veronderstelde voldoet [QueueDescription][QueueDescription], en vervolgens uw redenen zijn uw eigen voor het standaardgedrag wijzigen. Er zijn geen garanties worden gemaakt om de wijzigingen in de wachtrijen achterstand door uw code. Zorg ervoor dat uw wijzigingen grondig te testen.

## <a name="custom-messagesender"></a>Aangepaste MessageSender
Wanneer u verzendt, gaat u alle berichten via een interne [MessageSender] [ MessageSender] -object dat normaal gedraagt zich als alles werkt en wordt omgeleid naar de achterstand in de wachtrij geplaatst wanneer dingen 'verbreekt." Bij ontvangst van een tijdelijke fout, wordt een timer gestart. Na een [TimeSpan] [ TimeSpan] periode die bestaan uit de [FailoverInterval] [ FailoverInterval] eigenschapswaarde waarover geen geslaagde berichten worden verzonden, de failover is ingeschakeld. Op dit punt wordt plaats het volgende voor elke entiteit:

* Een ping-taak wordt uitgevoerd elke [PingPrimaryInterval] [ PingPrimaryInterval] om te controleren of de entiteit beschikbaar is. Wanneer deze taak is voltooid, wordt alle clientcode die gebruikmaakt van de entiteit onmiddellijk gestart nieuwe berichten verzenden naar de primaire naamruimte.
* Toekomstige aanvragen verzenden die dezelfde entiteit van een andere afzenders leidt ertoe dat de [BrokeredMessage] [ BrokeredMessage] worden gewijzigd om ze te zitten in de wachtrij achterstand worden verzonden. De wijziging wordt verwijderd van sommige eigenschappen van de [BrokeredMessage] [ BrokeredMessage] object en slaat ze op elders. De volgende eigenschappen zijn uitgeschakeld en wordt toegevoegd onder een nieuwe alias, zodat de Service Bus en de SDK om berichten te verwerken op uniforme wijze:

| Oude naam van eigenschap | Nieuwe naam van eigenschap |
| --- | --- |
| sessie-id |x-ms-sessie-id |
| TimeToLive |x-ms-timetolive |
| ScheduledEnqueueTimeUtc |x-ms-pad |

Het oorspronkelijke doelpad wordt ook opgeslagen in het bericht als een eigenschap met de naam x-ms-path. Dit ontwerp kunt berichten voor veel entiteiten worden gecombineerd in een wachtrij één achterstand. De eigenschappen worden weer met de syphon omgezet.

De aangepaste [MessageSender] [ MessageSender] object problemen kan optreden wanneer berichten benadering de limiet van 256 KB en failover is ingeschakeld. De aangepaste [MessageSender] [ MessageSender] object berichten voor alle wachtrijen en onderwerpen samen in de wachtrijen achterstand worden opgeslagen. Dit object combineert berichten van veel primaire samen in de wachtrijen achterstand. Voor het afhandelen van taakverdeling tussen veel clients die elkaar niet kent de SDK willekeurig kiest een achterstand wachtrij voor elk [QueueClient] [ QueueClient] of [TopicClient] [ TopicClient] u in code maken.

## <a name="pings"></a>Pings
Een ping-bericht is een lege [BrokeredMessage] [ BrokeredMessage] met de [ContentType] [ ContentType] eigenschap is ingesteld op application/vnd.ms-servicebus-ping en een [TimeToLive] [ TimeToLive] waarde van 1 seconde. Deze ping is een speciaal kenmerk in de Service Bus: de server biedt een ping nooit wanneer elk aanroepend aanvraagt een [BrokeredMessage][BrokeredMessage]. U moet dus nooit de informatie over het ontvangen en deze berichten genegeerd. Elke entiteit (unieke wachtrij of onderwerp) per [MessagingFactory] [ MessagingFactory] exemplaar per client zal worden gepingd wanneer deze worden beschouwd als niet beschikbaar is. Standaard is dit gebeurt eenmaal per minuut. Ping-berichten worden beschouwd als gewone Service Bus-berichten en kunnen leiden tot de kosten van bandbreedte en berichten. Als de clients detecteren dat het systeem beschikbaar is, stopt de berichten.

## <a name="the-syphon"></a>De syphon
Ten minste één uitvoerbaar programma in de toepassing moet actief zijn de syphon. De syphon voert een long poll ontvangen die 15 minuten duurt. Wanneer alle entiteiten beschikbaar zijn en hebt u 10 achterstand wachtrijen, roept de toepassing die als host fungeert voor de syphon de ontvangstbewerking 40 keer per uur, 960 keer per dag en 28800 keer in 30 dagen. Wanneer de syphon actief berichten van de achterstand naar de primaire wachtrij verplaatst is, krijgt elk bericht met de volgende kosten (standaard kosten voor de grootte van het bericht en de bandbreedte van toepassing in alle fasen):

1. Verzenden naar de achterstand.
2. Ontvangen van de achterstand.
3. Verzenden naar de primaire.
4. Ontvangen van de primaire.

## <a name="closefault-behavior"></a>Sluit/fout gedrag
In een toepassing die als host fungeert voor de syphon, nadat de primaire of secundaire [MessagingFactory] [ MessagingFactory] bedrijfsstoringen of wordt afgesloten zonder diens partner is ook mislukt of gesloten en de syphon deze toestand is, detecteert de syphon fungeert. Als de andere [MessagingFactory] [ MessagingFactory] is niet afgesloten binnen 5 seconden, de syphon nog steeds openen bedrijfsstoringen [MessagingFactory][MessagingFactory].

## <a name="next-steps"></a>Volgende stappen
Zie [asynchrone messaging-patronen en hoge beschikbaarheid] [ Asynchronous messaging patterns and high availability] voor een gedetailleerde bespreking van de asynchrone Service Bus-berichtenservice. 

[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[MessageSender]: /dotnet/api/microsoft.servicebus.messaging.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[FailoverInterval]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions#Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_FailoverInterval
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
[PingPrimaryInterval]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_PingPrimaryInterval
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[ContentType]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType
[TimeToLive]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md
[0]: ./media/service-bus-paired-namespaces/IC673405.png
[1]: ./media/service-bus-paired-namespaces/IC673406.png
[2]: ./media/service-bus-paired-namespaces/IC673407.png
