---
title: Azure Service Bus gekoppelde naamruimten | Microsoft Docs
description: Gekoppelde naamruimte implementatiegegevens en kosten
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 2440c8d3-ed2e-47e0-93cf-ab7fbb855d2e
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: aschhab
ms.openlocfilehash: 54376f3067e6aca52af8d283b42f6363f8016bff
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55728721"
---
# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Gekoppelde naamruimte implementatiegegevens en de implicaties kosten

De [PairNamespaceAsync] [ PairNamespaceAsync] methode, met behulp van een [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] exemplaar, zichtbaar taken worden uitgevoerd op uw naam. Omdat er zijn kosten overwegingen bij het gebruik van de functie, is het handig om te begrijpen die taken die u het gedrag verwacht wanneer dit gebeurt. De API voert de volgende automatisch gedrag namens:

* Het maken van achterstand wachtrijen.
* Het maken van een [MessageSender] [ MessageSender] object gebruikt die met wachtrijen of onderwerpen communiceert.
* Wanneer een Berichtentiteit beschikbaar is, pingen verzendt berichten naar de entiteit in een poging om te detecteren wanneer die entiteit weer beschikbaar wordt.
* (Optioneel) wordt gemaakt van een set "bericht pompen" die berichten uit de wachtrij achterstand verplaatsen naar de primaire wachtrijen.
* Coördineert de afsluitende/fout van de primaire en secundaire [MessagingFactory] [ MessagingFactory] exemplaren.

Op hoog niveau, de functie werkt als volgt: wanneer de primaire entiteit in orde is, geen gedragswijzigingen optreden. Wanneer de [FailoverInterval] [ FailoverInterval] duur is verstreken en de primaire entiteit ziet niet geslaagd verzendt na een niet-tijdelijke [MessagingException] [ MessagingException] of een [TimeoutException][TimeoutException], gebeurt het volgende:

1. Bewerkingen op de primaire entiteit zijn uitgeschakeld en het systeem pingt de primaire entiteit totdat pings kunnen worden bezorgd verzenden.
2. Een wachtrij willekeurige achterstand is geselecteerd.
3. [BrokeredMessage] [ BrokeredMessage] objecten worden doorgestuurd naar de wachtrij gekozen achterstand.
4. Als een bewerking voor het verzenden naar de wachtrij gekozen achterstand is mislukt, die wachtrij wordt opgehaald uit de draaihoek en een nieuwe wachtrij is geselecteerd. Alle afzenders op de [MessagingFactory] [ MessagingFactory] exemplaar meer van de fout.

De volgende afbeelding weer de reeks. De afzender verzendt eerst berichten.

![Gekoppelde naamruimten][0]

Bij fouten te verzenden naar de primaire wachtrij van de afzender begint het verzenden van berichten naar een willekeurig gekozen achterstand-wachtrij. Tegelijkertijd wordt er een ping-taak gestart.

![Gekoppelde naamruimten][1]

De berichten die op dit moment nog steeds in de secundaire wachtrij staan en niet naar de primaire wachtrij zijn geleverd. Zodra de primaire wachtrij weer in orde is, moet ten minste één proces de syphon worden uitgevoerd. De syphon bezorgt de berichten uit de verschillende achterstand wachtrijen op de juiste bestemming-entiteiten (wachtrijen en onderwerpen).

![Gekoppelde naamruimten][2]

De rest van dit onderwerp wordt de specifieke details over de werking van deze onderdelen beschreven.

## <a name="creation-of-backlog-queues"></a>Het maken van wachtrijen met achterstand
De [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] object doorgegeven aan de [PairNamespaceAsync] [ PairNamespaceAsync] methode geeft het aantal achterstand wachtrijen die u wilt gebruiken. Elke wachtrij achterstand wordt vervolgens gemaakt met de volgende eigenschappen expliciet ingesteld (alle andere waarden zijn ingesteld op de [QueueDescription] [ QueueDescription] standaardwaarden):

| Pad | [primaire naamruimte] / x-service bus-overdracht / [index] [index] is waar een waarde in [0, BacklogQueueCount) |
| --- | --- |
| MaxSizeInMegabytes |5120 |
| MaxDeliveryCount |int.MaxValue |
| DefaultMessageTimeToLive |TimeSpan.MaxValue |
| AutoDeleteOnIdle |TimeSpan.MaxValue |
| LockDuration |1 minuut |
| EnableDeadLetteringOnMessageExpiration |true |
| EnableBatchedOperations |true |

Bijvoorbeeld, de eerste achterstand wachtrij gemaakt voor de naamruimte **contoso** heet `contoso/x-servicebus-transfer/0`.

Bij het maken van de wachtrijen, de code eerst gecontroleerd of deze wachtrij bestaat. Als de wachtrij niet bestaat, wordt de wachtrij gemaakt. De code niet opschonen "extra" achterstand wachtrijen. Met name als de toepassing met de primaire naamruimte **contoso** aanvragen vijf achterstand wachtrijen, maar een achterstand wachtrij met het pad `contoso/x-servicebus-transfer/7` bestaat, die wachtrij extra achterstand nog steeds aanwezig is, maar wordt niet gebruikt. Extra achterstand wachtrijen aanwezig zijn die niet worden gebruikt door het systeem expliciet toegestaan. Als de eigenaar van de naamruimte bent u verantwoordelijk voor het opschonen van niet-gebruikte/ongewenste achterstand wachtrijen. De reden voor deze beslissing is dat Service Bus niet weten welke doelen bestaan voor alle wachtrijen in uw naamruimte. Bovendien, als een wachtrij met de opgegeven naam bestaat, maar voldoet niet aan de veronderstelde [QueueDescription][QueueDescription], en vervolgens de redenen waarom, zijn uw eigen voor het wijzigen van het standaardgedrag. Er zijn geen garanties worden gemaakt voor wijzigingen in de wachtrijen achterstand door uw code. Zorg ervoor dat u uw wijzigingen grondig te testen.

## <a name="custom-messagesender"></a>Aangepaste MessageSender
Wanneer u verzendt, gaat u alle berichten via een interne [MessageSender] [ MessageSender] object dat normaal gesproken zich gedraagt wanneer alles werkt en wordt omgeleid naar de achterstand in de wachtrij geplaatst wanneer dingen "verbreken." Na ontvangst van een niet-tijdelijke fout, wordt een timer gestart. Na een [TimeSpan] [ TimeSpan] periode die bestaat uit de [FailoverInterval] [ FailoverInterval] eigenschapswaarde waarin er geen geslaagde berichten worden verzonden, de failover is ingeschakeld. De volgende dingen gebeuren op dit moment voor elke entiteit:

* Een ping-taak wordt uitgevoerd elke [PingPrimaryInterval] [ PingPrimaryInterval] om te controleren of de entiteit beschikbaar is. Zodra deze taak is voltooid, wordt alle clientcode die gebruikmaakt van de entiteit onmiddellijk gestart nieuwe berichten verzenden naar de primaire naamruimte.
* Toekomstige aanvragen verzenden naar die dezelfde entiteit van alle andere afzenders leidt de [BrokeredMessage] [ BrokeredMessage] worden gewijzigd om ze te zitten in de wachtrij achterstand worden verzonden. De wijziging verwijdert u enkele eigenschappen van de [BrokeredMessage] [ BrokeredMessage] object en slaat ze op elders. De volgende eigenschappen zijn uitgeschakeld en onder een nieuwe alias, kunnen de Service Bus en de SDK voor het verwerken van berichten op uniforme wijze toegevoegd:

| Oude naam van eigenschap | Nieuwe naam van eigenschap |
| --- | --- |
| sessie-id |x-ms-sessionid |
| TimeToLive |x-ms-timetolive |
| ScheduledEnqueueTimeUtc |x-ms-path |

Het oorspronkelijke doelpad worden ook opgeslagen in het bericht als een eigenschap met de naam x-ms-pad. Dit ontwerp kunt berichten van vele entiteiten naar het naast elkaar worden gebruikt in een wachtrij één achterstand. De eigenschappen worden vertaald door de syphon worden teruggestuurd.

De aangepaste [MessageSender] [ MessageSender] object kan er problemen optreden wanneer berichten benadering de limiet van 256 KB en failover is betrokken. De aangepaste [MessageSender] [ MessageSender] object berichten voor alle wachtrijen en onderwerpen in de wachtrijen achterstand samen worden opgeslagen. Dit object combineert berichten van veel voorverkiezingen uit binnen de wachtrijen achterstand samen te brengen. Voor het afhandelen van taakverdeling tussen veel clients die elkaar niet kent, de SDK willekeurig kiest een achterstand wachtrij voor elk [QueueClient] [ QueueClient] of [TopicClient] [ TopicClient] u in code maken.

## <a name="pings"></a>Pings
Een pingbericht is een lege [BrokeredMessage] [ BrokeredMessage] met de [ContentType] [ ContentType] eigenschap is ingesteld op application/vnd.ms-servicebus-ping en een [TimeToLive] [ TimeToLive] waarde van 1 seconde. Deze ping heeft een speciale kenmerken in Service Bus: de server levert nooit een ping wanneer een aanroeper vraagt een [BrokeredMessage][BrokeredMessage]. U moet dus nooit de informatie over het ontvangen en deze berichten negeren. Elke entiteit (de unieke wachtrij of onderwerp) per [MessagingFactory] [ MessagingFactory] exemplaar per client zal worden gepingd wanneer deze worden beschouwd als niet beschikbaar is. Dit gebeurt standaard één keer per minuut. Ping-berichten worden beschouwd als normale Service Bus-berichten en kunnen leiden tot kosten in rekening gebracht voor bandbreedte en berichten. Als de clients detecteren dat het systeem beschikbaar is, stopt de berichten.

## <a name="the-syphon"></a>De syphon
Ten minste één uitvoerbaar programma in de toepassing moet actief zijn de syphon. De syphon voert een lange poll ontvangen die duurt 15 minuten. Wanneer alle entiteiten beschikbaar zijn en u 10 achterstand wachtrijen hebt, roept de toepassing die als host fungeert voor de syphon de ontvangstbewerking 40 keer per uur, 960 keer per dag en 28800 tijden in 30 dagen. Wanneer de syphon actief berichten van de achterstand naar de primaire wachtrij verplaatst is, krijgt elk bericht met de volgende kosten (standaardkosten voor de grootte van het bericht en de bandbreedte van toepassing in alle fasen):

1. Verzenden naar de achterstand.
2. Ontvangen van de achterstand.
3. Verzenden naar de primaire.
4. Ontvangen van de primaire.

## <a name="closefault-behavior"></a>Sluit/fout gedrag
Binnen een toepassing die als host fungeert voor de syphon, zodra de primaire of secundaire [MessagingFactory] [ MessagingFactory] bedrijfsstoringen of wordt afgesloten zonder dat bijbehorende partner ook mislukt of gesloten en de syphon detecteert deze status kunnen de syphon fungeert. Als de andere [MessagingFactory] [ MessagingFactory] is niet afgesloten binnen 5 seconden, de syphon het nog steeds openen bedrijfsstoringen [MessagingFactory][MessagingFactory].

## <a name="next-steps"></a>Volgende stappen
Zie [asynchrone messaging-patronen en hoge beschikbaarheid] [ Asynchronous messaging patterns and high availability] voor een gedetailleerde beschrijving van de Service Bus asynchrone berichten. 

[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[MessageSender]: /dotnet/api/microsoft.servicebus.messaging.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[FailoverInterval]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
[PingPrimaryInterval]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[ContentType]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[TimeToLive]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md
[0]: ./media/service-bus-paired-namespaces/IC673405.png
[1]: ./media/service-bus-paired-namespaces/IC673406.png
[2]: ./media/service-bus-paired-namespaces/IC673407.png
