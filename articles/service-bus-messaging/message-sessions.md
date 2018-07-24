---
title: Azure Service Bus-berichtsessies | Microsoft Docs
description: Reeksen met Azure Service Bus-berichten met sessies worden verwerkt.
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
ms.date: 01/02/2018
ms.author: sethm
ms.openlocfilehash: cc7ef24a3e1781969284a3d1e683fe73ce48b551
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216390"
---
# <a name="message-sessions-first-in-first-out-fifo"></a>Sessies bericht: eerste, eerst out (FIFO) 

Microsoft Azure Service Bus-sessies kunt gezamenlijke en geordende verwerking van niet-gebonden reeksen verwante berichten. Als u wilt profiteren van een garantie FIFO in Service Bus-sessies te gebruiken. Service Bus is niet duidelijk over de aard van de relatie tussen de berichten, en definieert ook niet een bepaald model om te bepalen wanneer een bericht-reeks begint of eindigt.

Een afzender kunt maken een sessie wanneer de verzenden wordt berichten in een onderwerp of een wachtrij door in te stellen de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId) eigenschap aan enkele toepassingsspecifieke-id die uniek is voor de sessie. Op het niveau van AMQP 1.0-protocol, wordt deze waarde toegewezen aan de *groep-id* eigenschap.

Ontstaan op sessie-bewuste wachtrijen of -abonnementen, sessies, wanneer er ten minste één bericht van de sessie [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId). Wanneer een sessie bestaat, is er geen gedefinieerde tijd of API voor wanneer de sessie is verlopen of verdwijnt. In theorie, kunt u een bericht ontvangen voor vandaag de dag een sessie het volgende bericht in de tijd van een jaar, en als de **SessionId** overeenkomt met de sessie is hetzelfde vanuit het perspectief van de Service Bus.

Normaal gesproken heeft een toepassing echter een duidelijke begrip van een set gerelateerde berichten begint en eindigt. Service Bus wordt niet ingesteld voor een specifieke regels.

Een voorbeeld van hoe u een reeks afbakenen voor een bestand is om in te stellen de **Label** eigenschap voor het eerste bericht **start**, tussenliggende berichten **inhoud**, en voor de laatste bericht **end**. De relatieve positie van de inhoud berichten kan worden berekend als het huidige bericht *SequenceNumber* verschillen van de **start** bericht *SequenceNumber*.

De sessie-functie in een specifieke bewerking, krijgen in de vorm van Service Bus-Hiermee [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) in de C# en Java-API's. U de functie inschakelen door in te stellen de [requiresSession](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) eigenschap in de wachtrij of abonnement via Azure Resource Manager of door de vlag instellen in de portal. Dit is vereist voordat u probeert te gebruiken van de bijbehorende API-bewerkingen.

Stel de markering op in het volgende selectievakje in de portal:

![][2]

De API's voor sessies bestaan op de wachtrij en de abonnement-clients. Er is een imperatieve model die bepaalt wanneer de sessies en berichten worden ontvangen, en een handler op basis van model, vergelijkbaar met *OnMessage*, dat wordt ingeschakeld, wordt de complexiteit van het beheer van de ontvangen in een lus.

## <a name="session-features"></a>Sessie-functies

Sessies bieden gelijktijdige ongedaan maken multiplexing van interleaved berichtstromen te behouden en aflevering te garanderen.

![][1]

Een [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) ontvanger is gemaakt door de client accepteert van een sessie. Het clientaanroepen van de [QueueClient.AcceptMessageSession](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesession#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSession) of [QueueClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesessionasync#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSessionAsync) in C#. In het model reactieve callback, wordt een sessie-handler geregistreerd.

Wanneer de [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) object is geaccepteerd en terwijl deze wordt gehouden door een client, dat de client een exclusieve vergrendeling op alle berichten met een van deze sessie bevat [SessionId](/en-us/dotnet/api/microsoft.servicebus.messaging.messagesession.sessionid#Microsoft_ServiceBus_Messaging_MessageSession_SessionId) die zijn opgenomen in de wachtrij of abonnement, en ook voor alle berichten met die **SessionId** die nog steeds binnenkomen, terwijl de sessie wordt gehouden.

De vergrendeling wordt vrijgegeven wanneer **sluiten** of **CloseAsync** worden genoemd, of wanneer de vergrendeling is verlopen in gevallen waarin de toepassing kan niet worden uitgevoerd van de bewerking sluiten. De sessievergrendeling moet worden behandeld, zoals een exclusieve vergrendeling voor een bestand, wat betekent dat de toepassing u de sessie sluit moet als u deze niet meer nodig heeft en/of alle verdere berichten niet verwacht.

Wanneer meerdere gelijktijdige ontvangers uit de wachtrij ophalen, worden de berichten die behoren tot een bepaalde sessie naar de specifieke ontvanger die op dit moment de vergrendeling voor die sessie bevat worden verzonden. Met deze bewerking een interleaved berichtenstroom die zich bevinden in een wachtrij of abonnement ongedaan maken foutloos wordt gebruikt voor andere ontvangers en de ontvangers kunnen ook live op andere clientcomputers, omdat het lock-beheer aan de serverkant, gebeurt in Servicebus.

De vorige afbeelding ziet u drie gelijktijdige sessies ontvangers. Een sessie met `SessionId` = 4 heeft geen client actief, die eigenaar is, wat betekent dat er geen berichten van deze specifieke sessie worden geleverd. Een sessie fungeert op tal van manieren, zoals een sub-wachtrij.

De sessievergrendeling gehouden door de ontvanger van de sessie is een overkoepelende voor het bericht wordt vergrendeld die worden gebruikt door de *peek-lock* modus verwerken. Een ontvanger kan niet gelijktijdig worden twee berichten hebben 'tijdens de vlucht', maar de berichten op volgorde moeten worden verwerkt. Een nieuw bericht kan alleen worden verkregen wanneer het voorafgaande bericht is voltooid of dead lettered. Een bericht wordt hetzelfde bericht te laten leveren wordt afgebroken opnieuw met de volgende ontvangen bewerking.

## <a name="message-session-state"></a>Bericht-sessiestatus

Als werkstromen worden verwerkt in hoge schaalbaarheid, hoge beschikbaarheid cloudsystemen, de werkstroom-handler die is gekoppeld aan een bepaalde sessie moet kunnen herstellen van onverwachte fouten en ook in staat het hervatten van de gedeeltelijk voltooid werk op een andere zijn proces of machine vanaf waar het werk is begonnen.

De sessie-status-faciliteit kan een aantekening toepassingsspecifieke van de sessie van een bericht in de broker zodat de verwerkingsstatus van het geregistreerde ten opzichte van die sessie onmiddellijk beschikbaar wordt wanneer de sessie wordt verkregen door een nieuwe processor.

Vanuit het perspectief van de Service Bus is de bericht-sessiestatus een ondoorzichtige binaire object die gegevens van de grootte van één bericht, dat 256 KB voor Service Bus Standard en 1 MB voor Service Bus Premium is kan bevatten. De verwerkingsstatus ten opzichte van een sessie kan worden ondergebracht in de sessiestatus of de sessiestatus kunt verwijzen naar een opslaglocatie of databaserecord dat dergelijke informatie bevat.

De API's voor het beheren van de sessiestatus, [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) en [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState), kunt u vinden op de [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) -object in de C# en Java-API's. Een sessie die eerder geen sessiestatus was instellen retourneert een **null** referentiemateriaal **GetState**. Wissen van de eerder ingestelde sessiestatus is klaar met [SetState(null)](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_).

Houd er rekening mee dat de sessiestatus, mits deze is niet gewist van blijft (retourneren **null**), zelfs als alle berichten in een sessie worden gebruikt.

Alle bestaande sessies in een wachtrij of abonnement kunnen worden geïnventariseerd met de **SessionBrowser** methode in de Java-API en met [GetMessageSessions](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessions) op de [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) en [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) in de .NET-client.

De sessiestatus ondergebracht in een wachtrij of in dat een abonnement wordt geteld tegen het opslagquotum van die entiteit. Wanneer de toepassing met een sessie is voltooid, is het daarom raadzaam voor de toepassing voor het opschonen van de status behouden om te voorkomen dat externe beheerkosten.

## <a name="next-steps"></a>Volgende stappen

- [Een volledig voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) verzenden en ontvangen van berichten op basis van een sessie van Service Bus-wachtrijen met behulp van de standaard .NET-bibliotheek.
- [Een voorbeeld van een](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/Sessions) die gebruikmaakt van de .NET Framework-client om sessie gerichte berichten te verwerken. 

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/message-sessions/sessions.png
[2]: ./media/message-sessions/queue-sessions.png