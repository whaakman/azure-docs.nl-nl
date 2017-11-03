---
title: Vervaldatum van Azure Service Bus-berichten | Microsoft Docs
description: Verlopen en de TTL van Azure Service Bus-berichten
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
ms.openlocfilehash: 504010a39a4012b9a9edb60bb9a5b33ac20499c1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="message-expiration-time-to-live"></a>Verloopdatum voor het bericht (Time to Live)

De nettolading in een bericht of een opdracht of informatie die door een bericht met een ontvanger weergegeven is bijna altijd onderworpen aan een vorm van de deadline op toepassingsniveau verlopen. De inhoud niet langer wordt geleverd of de aangevraagde bewerking wordt niet langer uitgevoerd na een deadline.

Voor ontwikkeling en testomgevingen waarin de wachtrijen en onderwerpen vaak worden gebruikt in de context van gedeeltelijk wordt uitgevoerd van toepassingen of onderdelen van de toepassing, is het ook wenselijk gestrande testberichten automatisch worden kunt garbage collector zijn verzameld, zodat de volgende test uitvoeren schone start.

De vervaldatum voor een afzonderlijk bericht kan worden beheerd door het instellen van de [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) systeemeigenschap, waarmee een relatieve duur. De vervaldatum wordt een absolute instant wanneer het bericht in de wachtrij in de entiteit. Op dat moment wordt de [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc) voor deze eigenschap is van de waarde [ **EnqueuedTimeUtc**](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc#Microsoft_ServiceBus_Messaging_BrokeredMessage_EnqueuedTimeUtc) + [**TimeToLive**](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive).

Afgelopen de **ExpiresAtUtc** chatberichten, worden niet in aanmerking voor het ophalen van berichten. De vervaldatum heeft geen invloed op berichten die momenteel zijn vergrendeld voor de levering van; Deze berichten worden nog steeds normaal verwerkt. Als de vergrendeling is verlopen of het bericht wordt afgesloten, wordt de vervaldatum van onmiddellijke kracht.

Terwijl het bericht onder vergrendelen is, wordt de toepassing mogelijk in bezit is van een bericht dat nominaal is verlopen. Hiermee wordt aangegeven of de toepassing wordt wil opwekken tegen verwerking of kiest voor het afbreken van het bericht is tot de implementeerder.

## <a name="entity-level-expiration"></a>Entiteitsniveau verlopen

Alle berichten die worden verzonden naar een wachtrij of onderwerp zijn onderworpen aan de vervaldatum van een standaard die is ingesteld op de entiteit die met de [defaultMessageTimeToLive](/azure/templates/microsoft.servicebus/namespaces/queues) eigenschap en die kunnen ook worden ingesteld in de portal tijdens het maken en later wordt aangepast. De standaardwaarde is verlopen, wordt gebruikt voor alle berichten die naar de entiteit waar [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) niet expliciet is ingesteld. De standaardwaarde is verlopen functioneert ook als een maximum voor de **TimeToLive** waarde. Berichten met een langere **TimeToLive** verlopen dan de standaardwaarde achtergrond worden aangepast aan de **defaultMessageTimeToLive** waarde voordat wordt in de wachtrij.

Verlopen berichten kunnen eventueel worden verplaatst naar een [wachtrij voor onbestelbare berichten](service-bus-dead-letter-queues.md) door in te stellen de [EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enabledeadletteringonmessageexpiration#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) eigenschap of het bijbehorende selectievakje in de portal. Als de optie is uitgeschakeld, worden verlopen berichten verwijderd. Verlopen berichten verplaatst naar de wachtrij voor onbestelbare berichten kunnen worden onderscheiden van andere lettered van onbestelbare berichten door te evalueren de [DeadletterReason](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq) eigenschap die de broker in het eigenschappengedeelte van gebruiker opslaat; de waarde is [TTLExpiredException](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq) in dit geval.

Het bericht wordt verplaatst naar de wachtrij voor onbestelbare berichten in de hiervoor genoemde geval waarin het bericht is beveiligd tegen verlopen tijdens onder vergrendelen en als de vlag is ingesteld op de entiteit als de vergrendeling wordt verlaten of is verlopen. Het is echter niet verplaatst als het bericht is met succes afgewikkeld, die vervolgens wordt ervan uitgegaan dat de toepassing met succes, ondanks de nominaal is verlopen verwerkt is.

De combinatie van [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) en automatische (en transactionele) verwerking van onbestelbare berichten op verstrijken zijn een waardevol hulpmiddel voor het vaststellen van vertrouwen in of een taak die is toegekend aan een handler of een groep handlers onder een deadline opgehaald voor kunnen worden verwerkt als de deadline is bereikt.

Neem bijvoorbeeld een website die moet op betrouwbare wijze taken uitvoeren op een schaal beperkte back-end en welke soms ervaringen verkeer bereikt of wil worden geïsoleerd tegen periodes van de beschikbaarheid van die back-end. In het geval reguliere de serverzijde-handler voor de verzonden gegevens wordt de informatie in een wachtrij en vervolgens ontvangt een bevestiging van geslaagde verwerking van de transactie in een antwoordenwachtrij antwoord. Als er een piek verkeer en de back-end-handler achterstand items in de tijd kan niet worden verwerkt, worden de verlopen taken in de wachtrij voor onbestelbare berichten geretourneerd. De interactieve gebruiker kan worden gewaarschuwd dat de aangevraagde bewerking iets langer dan normaal duurt en de aanvraag kan vervolgens worden geplaatst op een andere wachtrij voor een verwerken van het pad waar het resultaat van de uiteindelijke verwerking naar de gebruiker via e-mail is verzonden. 

## <a name="temporary-entities"></a>Tijdelijke entiteiten

Service Bus-wachtrijen, onderwerpen en abonnementen kunnen worden gemaakt als tijdelijke entiteiten worden automatisch verwijderd wanneer deze gedurende een opgegeven periode niet is gebruikt.
 
Automatisch opschonen is handig in ontwikkeling en tests scenario's waarin entiteiten worden dynamisch gemaakt en worden niet opgeschoond na gebruik, als gevolg van de test of foutopsporing uitvoeren die is onderbroken. Het is ook handig wanneer een toepassing dynamische entiteiten, zoals een antwoordenwachtrij maakt voor het ontvangen van antwoorden terug in een web server-proces of in een ander relatief eenvoudige object waar het is moeilijk om op te schonen betrouwbaar deze entiteiten wanneer het object exemplaar verdwijnt.

De functie is ingeschakeld met behulp van de [autoDeleteOnIdle](/azure/templates/microsoft.servicebus/namespaces/queues) eigenschap die is ingesteld op de duur waarvoor een entiteit moet zijn inactief (ongebruikte) voordat deze automatisch wordt verwijderd. De minimale duur is 5 minuten.
 
De eigenschap moet worden ingesteld via een Azure Resource Manager-bewerking of via de .NET Framework client [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API's. Deze waarde kan niet worden ingesteld via de portal.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichtenservice, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)