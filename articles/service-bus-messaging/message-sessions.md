---
title: Azure Service Bus-berichten uitwisselen | Microsoft Docs
description: Reeksen met Azure Service Bus-berichten met sessies worden verwerkt.
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
ms.date: 01/02/2018
ms.author: sethm
ms.openlocfilehash: 16f641c7b6fdd1d6730d2ae229c93ce4a33b9492
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="message-sessions-first-in-first-out-fifo"></a>Sessies bericht: first in eerst uit FIFO-principe () 

Microsoft Azure Service Bus-sessies inschakelen gemeenschappelijke en geordende verwerking van unbounded reeksen verwante berichten. Als u een garantie FIFO-principe in Service Bus, sessies te gebruiken. Service Bus is geen richtlijnen over de aard van de relatie tussen de berichten en een bepaald model om te bepalen waar een message-reeks begint of eindigt ook geen wordt gedefinieerd.

Een afzender een sessie wanneer verzenden door in te stellen met naar een onderwerp of een wachtrij berichten kunt maken de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId) eigenschap in op sommige toepassingen gedefinieerde id geleverd die uniek is voor de sessie. Op het niveau AMQP 1.0-protocol wordt deze waarde toegewezen aan de *groep-id* eigenschap.

Ontstaan op sessie-bewuste wachtrijen of abonnementen en sessies, als er ten minste één bericht met de sessie [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId). Wanneer een sessie bestaat, is er geen gedefinieerde tijd of API voor wanneer de sessie is verlopen of verdwijnt. Theoretisch, kunt u een bericht ontvangen voor vandaag de dag een sessie het volgende bericht in een jaar tijd, en als de **SessionId** overeenkomt, de sessie is hetzelfde vanuit het perspectief van de Service Bus.

Normaal gesproken heeft een toepassing echter een duidelijke begrip waar een set gerelateerde berichten begint en eindigt. Service Bus biedt een specifieke regels niet ingesteld.

Een voorbeeld van hoe u een reeks afbakenen voor het overbrengen van een bestand is het instellen van de **Label** eigenschap voor de eerste bericht **start**, tussenliggende berichten **inhoud**, en voor het laatste bericht aan **end**. De relatieve positie van de inhoud berichten worden berekend als het huidige bericht *SequenceNumber* verschillen van de **start** bericht *SequenceNumber*.

De sessie-functie in een specifieke bewerking ontvangen in de vorm van Service Bus-kan [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) in de C# en Java-API's. U de functie inschakelen door in te stellen de [requiresSession](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) eigenschap in de wachtrij of abonnement via Azure Resource Manager, of door de vlag in de portal. Dit is vereist voordat u probeert te gebruiken van de bijbehorende API-bewerkingen.

Stel de vlag met het volgende selectievakje in de portal:

![][2]

De API's voor sessies aanwezig op de wachtrij en de abonnement-clients. Er is een imperatieve model die bepaalt wanneer de sessies en berichten worden ontvangen en een handler op basis van een model, vergelijkbaar met *OnMessage*, dat verborgen de complexiteit van het beheer van de receive-lus.

## <a name="session-features"></a>Sessie-functies

Sessies bieden gelijktijdige ongedaan multiplex van interleaved berichtstromen tijdens behouden en levering garanderen.

![][1]

Een [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) ontvanger is gemaakt door de client een sessie worden geaccepteerd. Het clientaanroepen van de [QueueClient.AcceptMessageSession](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesession#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSession) of [QueueClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesessionasync#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSessionAsync) in C#. In het model reactieve callback registreert het een handler sessie zoals verderop besproken.

Wanneer de [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) object is geaccepteerd en terwijl deze wordt gehouden door een client, dat de client een exclusieve vergrendeling bevat voor alle berichten die met die sessie [SessionId](/en-us/dotnet/api/microsoft.servicebus.messaging.messagesession.sessionid#Microsoft_ServiceBus_Messaging_MessageSession_SessionId) die zijn opgenomen in de wachtrij of een abonnement, en ook voor alle berichten die met die **SessionId** die nog steeds binnenkomen, terwijl de sessie wordt gehouden.

De vergrendeling wordt vrijgegeven wanneer **sluiten** of **CloseAsync** worden genoemd, of wanneer de vergrendeling in gevallen waarin de toepassing kan niet worden uitgevoerd van de afsluitbewerking is verlopen. De sessievergrendeling moet worden behandeld als een exclusieve vergrendeling op een bestand, wat betekent dat de sessie omdat deze niet langer nodig heeft en/of alle verdere berichten niet verwacht door de toepassing moet worden gesloten.

Wanneer meerdere gelijktijdige ontvangers uit de wachtrij ophalen, worden de berichten die horen bij een bepaalde sessie verzonden naar de specifieke ontvanger die momenteel de vergrendeling voor die sessie bevat. Met deze bewerking een interleaved berichtenstroom die zich in een wachtrij of abonnement ongedaan foutloos wordt gebruikt voor andere ontvangers en deze ontvangers kunnen ook live op andere client-machines, omdat het Vergrendelingsbeheer aan servicezijde, gebeurt in Servicebus.

Een wachtrij is echter nog steeds een wachtrij: Er is geen willekeurige toegang. Als meerdere gelijktijdige ontvangers wachten met het specifieke sessies accepteren of wachten op berichten van specifieke sessies en er is een bericht boven aan een wachtrij die horen bij een sessie die is nog geen ontvanger geclaimd, bevatten leveringen totdat een sessie-ontvanger die claims de sessie.

De vorige afbeelding ziet u drie gelijktijdige sessies ontvangers, die alle berichten uit de wachtrij voor elke ontvanger voortgang actief moet nemen. De vorige sessie met `SessionId` = 4 heeft geen actieve, eigenaar-client, wat betekent dat er geen berichten worden afgeleverd voor iedereen totdat dat bericht is genomen door een nieuw gemaakte die eigenaar is van de ontvanger sessie.

Terwijl die kunnen worden weergegeven om te worden met beperkingen, kan een proces één ontvanger verwerken veel gelijktijdige sessies eenvoudig, vooral wanneer ze zijn geschreven met strikt asynchrone code; gelijktijdige sessies op verschillende dozijn Jongleren is effectief automatisch met het model retouraanroep.

De strategie voor het verwerken van veel gelijktijdige sessies in, waarbij elke sessie slechts sporadisch berichten ontvangt, wordt de handler voor het verwijderen van de sessie na enige tijd inactief en hervatten verwerking wanneer de sessie wordt gezien als de volgende sessie binnenkomt.

De sessievergrendeling vastgehouden door de ontvanger van de sessie is een overkoepelende voor het bericht wordt vergrendeld die wordt gebruikt door de *peek vergrendeling* vereffening modus. Een ontvanger kan niet gelijktijdig twee berichten hebben 'tijdens de vlucht', maar de berichten op volgorde moeten worden verwerkt. Een nieuw bericht kan alleen worden opgehaald wanneer het voorafgaande bericht is voltooid of onbestelbare lettered. Een bericht wordt hetzelfde bericht aan worden geleverd opgegeven opnieuw met de volgende ontvangstbewerking.

## <a name="message-session-state"></a>Bericht-sessiestatus

Wanneer de werkstromen worden verwerkt in hoge schaalbaarheid, hoge beschikbaarheid cloudsystemen, de werkstroom-handler gekoppeld aan een bepaalde sessie moet kunnen herstellen van onverwachte fouten en ook geschikt voor gedeeltelijk voltooide werkzaamheden op een andere hervatten proces of computer waar de werken werden gestart.

De sessie status faciliteit kan een aantekening toepassingsspecifieke van een sessie bericht binnen de broker, zodat de vastgelegde verwerkingsstatus ten opzichte van die sessie onmiddellijk beschikbaar wordt wanneer de sessie wordt verkregen door een nieuwe processor.

Vanuit het perspectief van Service Bus is de sessiestatus bericht een ondoorzichtige binaire object waarin gegevens van de grootte van een bericht dat 256 KB voor Service Bus Standard en 1 MB voor Service Bus Premium is. De verwerkingsstatus ten opzichte van een sessie kan plaatsvinden binnen de sessiestatus of de sessiestatus kan verwijzen naar een enkele locatie voor de opslag of databaserecord dat dergelijke informatie bevat.

De API's voor het beheren van de sessiestatus, [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) en [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState), kunt u vinden op de [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) object in de C# en Java-API's. Een sessie die eerder geen session state was ingesteld retourneert een **null** referentiemateriaal **GetState**. Wissen van de eerder ingestelde sessiestatus is klaar met [SetState(null)](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_).

Alle bestaande sessies in een wachtrij of -abonnement kunnen worden geïnventariseerd met de **SessionBrowser** methode in de Java-API en met [GetMessageSessions](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessions) op de [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) en [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) in de .NET-client.

De sessiestatus is ondergebracht in een wachtrij of in dat een abonnement telt naar de opslaglimiet van de entiteit. Wanneer de toepassing is voltooid met een sessie, is het daarom raadzaam voor de toepassing om de terugkerende status op te schonen om te voorkomen dat externe beheerkosten.

## <a name="next-steps"></a>Volgende stappen

- [Een voorbeeld van een volledige](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) verzenden en ontvangen van berichten op basis van Service Bus-wachtrijen met de standaard .NET-bibliotheek.
- [Een voorbeeld van een](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/Sessions) die gebruikmaakt van de .NET Framework-client voor het afhandelen van sessie-bewuste berichten. 

Zie voor meer informatie over Service Bus-berichtenservice, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/message-sessions/sessions.png
[2]: ./media/message-sessions/queue-sessions.png