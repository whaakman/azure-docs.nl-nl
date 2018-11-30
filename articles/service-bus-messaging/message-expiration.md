---
title: Verlopen van Azure Service Bus-berichten | Microsoft Docs
description: Verlopen en de TTL van Azure Service Bus-berichten
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
ms.date: 11/29/2018
ms.author: spelluru
ms.openlocfilehash: c5df5f43c4f01013cc44a2497203947f303f3e81
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52634826"
---
# <a name="message-expiration-time-to-live"></a>Verlopen van berichten (Time to Live)

De nettolading in een bericht of een opdracht of een query die door een bericht met een ontvanger weergegeven is bijna altijd onderhevig aan een vorm van de deadline op toepassingsniveau verlopen. De inhoud niet meer wordt geleverd of de aangevraagde bewerking is niet meer uitgevoerd na een deadline.

Voor ontwikkeling en testomgevingen waarin de wachtrijen en onderwerpen vaak worden gebruikt in de context van gedeeltelijke uitvoeringen van toepassingen of onderdelen van de toepassing, is het ook wenselijk gestrande testberichten automatisch worden verwijderd zodat de volgende test uitvoeren kunt schone start.

De vervaldatum voor een afzonderlijk bericht kan worden beheerd door in te stellen de [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) systeemeigenschap die Hiermee geeft u een relatieve duur. De vervaldatum wordt een absolute instant wanneer het bericht in de wachtrij in de entiteit is. Op dat moment de [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc) voor deze eigenschap is van de waarde [(**EnqueuedTimeUtc**](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc#Microsoft_ServiceBus_Messaging_BrokeredMessage_EnqueuedTimeUtc) + [**TimeToLive**)](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive). De time-to-live (TTL) instelling voor een brokered bericht wordt niet afgedwongen wanneer er geen client is niet actief luistert.

Afgelopen de **ExpiresAtUtc** direct, worden niet in aanmerking voor het ophalen van berichten. De vervaldatum heeft geen invloed op berichten die momenteel zijn vergrendeld voor de levering van; Deze berichten worden nog steeds normaal gesproken verwerkt. Als de vergrendeling is verlopen of het bericht wordt verbroken, duurt het verlopen onmiddellijk van kracht wordt.

Terwijl het bericht onder vergrendelen is, wordt de toepassing mogelijk in het bezit is van een bericht dat is verlopen. Of de toepassing wil gaan met verwerking in of kiest voor het bericht afbreken is aan de implementeerder.

## <a name="entity-level-expiration"></a>Op entiteitsniveau verlopen

Alle berichten die worden verzonden naar een wachtrij of onderwerp zijn afhankelijk van de vervaldatum van een standaard die is ingesteld op de entiteit met de [defaultMessageTimeToLive](/azure/templates/microsoft.servicebus/namespaces/queues) eigenschap en die kan ook worden ingesteld in de portal tijdens het maken en later wordt aangepast. De vervaldatum van het standaard wordt gebruikt voor alle berichten die naar de entiteit waarin [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) niet expliciet is ingesteld. De vervaldatum van het standaard fungeert ook als een maximum voor de **TimeToLive** waarde. Berichten met een langere **TimeToLive** verlopen dan de standaardwaarde op de achtergrond worden aangepast aan de **defaultMessageTimeToLive** waarde voordat wordt in de wachtrij.

Verlopen berichten kunnen desgewenst worden verplaatst naar een [dead-letter-wachtrij](service-bus-dead-letter-queues.md) door in te stellen de [EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enabledeadletteringonmessageexpiration#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) eigenschap of het bijbehorende selectievakje in de portal. Als de optie is uitgeschakeld, worden verlopen berichten verwijderd. Verlopen berichten verplaatst naar de dead-letter-wachtrij kunnen worden onderscheiden van andere berichten dead lettered door het evalueren van de [DeadletterReason](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq) eigenschap die de broker wordt opgeslagen in het eigenschappengedeelte van gebruiker; de waarde is [TTLExpiredException](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq) in dit geval.

In de hiervoor genoemde geval waarin het bericht is beveiligd tegen verlopen tijdens het vergrendelen en als de vlag is ingesteld op de entiteit, wordt het bericht verplaatst naar de dead-letter-wachtrij als de vergrendeling wordt verlaten of is verlopen. Het is echter niet verplaatst als het bericht is met succes verrekend, die vervolgens wordt ervan uitgegaan dat de toepassing met succes, er een nominaal bedrag is verlopen verwerkt is.

De combinatie van [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) en automatische (en transactionele) onbestelbare na het verstrijken zijn een waardevol hulpmiddel voor het tot stand brengen van vertrouwen in of een taak die wordt verleend aan een handler of een groep van de handlers onder een deadline voor wordt opgehaald kunnen worden verwerkt als de deadline is bereikt.

Neem bijvoorbeeld een website die moet worden betrouwbaar taken uitvoeren op een schaal beperkte back-end en die van tijd tot tijd ervaringen verkeer pieken of wil worden geïsoleerd tegen afleveringen van de beschikbaarheid van deze back-end. In het geval regelmatig de server-side-handler voor de verzonden gegevens pushes van de informatie in een wachtrij en daarna een bevestiging van geslaagde verwerking van de transactie in een antwoordenwachtrij antwoord ontvangt. Als er een verkeerspiek en de back-end-handler achterstand items niet in de tijd verwerken, worden de verlopen taken op de dead-letter-wachtrij geretourneerd. De interactieve gebruiker kan worden gewaarschuwd dat de aangevraagde bewerking iets langer dan normaal duurt en de aanvraag kan vervolgens worden geplaatst op een andere wachtrij voor een verwerken van het pad waar het resultaat van de uiteindelijke verwerking naar de gebruiker via e-mail is verzonden. 


## <a name="temporary-entities"></a>Tijdelijke entiteiten

Service Bus-wachtrijen, onderwerpen en abonnementen kunnen worden gemaakt als tijdelijke entiteiten, deze worden automatisch verwijderd wanneer deze gedurende een opgegeven periode niet is gebruikt.
 
Automatisch op te schonen is handig in ontwikkelings- en scenario's waarin entiteiten worden dynamisch gemaakt en worden niet opgeschoond na gebruik vanwege een onderbreking van de test- of foutopsporing uitvoeren. Het is ook handig wanneer een toepassing dynamische entiteiten, zoals een antwoordenwachtrij maakt, voor het ontvangen van antwoorden weer in een web server-proces of in een ander relatief eenvoudige object waar het is moeilijk om op te schonen betrouwbaar deze entiteiten als het object exemplaar verdwijnt.

De functie is ingeschakeld met behulp van de [autoDeleteOnIdle](/azure/templates/microsoft.servicebus/namespaces/queues) eigenschap. Deze eigenschap is ingesteld op de duur waarvoor een entiteit moet worden actieve (niet-gebruikte) voordat deze automatisch wordt verwijderd. De minimale waarde voor deze eigenschap is 5.
 
De **autoDeleteOnIdle** eigenschap moet worden ingesteld via een Azure Resource Manager-bewerking of via de .NET Framework client [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API's. U kunt deze niet instellen in de portal.

## <a name="idleness"></a>Inactiviteit

Hier volgt wat beschouwd als inactiviteit van entiteiten (wachtrijen, onderwerpen en abonnementen):

- Wachtrijen
    - Er is geen verzendt  
    - Niet ontvangen  
    - Er zijn geen updates naar de wachtrij  
    - Er is geen geplande berichten  
    - Er is geen bladeren/peek 
- Onderwerpen  
    - Er is geen verzendt  
    - Er zijn geen updates naar het onderwerp  
    - Er is geen geplande berichten 
- Abonnementen
    - Niet ontvangen  
    - Er zijn geen updates voor het abonnement  
    - Er zijn geen nieuwe regels toegevoegd aan het abonnement  
    - Er is geen bladeren/peek  
 


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)