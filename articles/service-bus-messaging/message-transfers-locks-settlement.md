---
title: Azure Service Bus-bericht overdrachten, vergrendelingen en verwerken | Microsoft Docs
description: Overzicht van Service Bus-bericht worden overgedragen en verwerken bewerkingen
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
ms.openlocfilehash: de3f23f58ef34bdd5f9769f820d64ed7e00ca7d8
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44715071"
---
# <a name="message-transfers-locks-and-settlement"></a>Berichten overdragen, vergrendelen en verwerken

De centrale mogelijkheid van een berichtenbroker, zoals Service Bus is het accepteren van berichten in een wachtrij of onderwerp en deze beschikbaar zijn voor het ophalen van later worden vastgehouden. *Verzenden* is de term die vaak wordt gebruikt voor de overdracht van een bericht in de berichtenbroker. *Ontvangen* is de term die vaak worden gebruikt voor de overdracht van een bericht naar een bij het ophalen van client.

Wanneer een client een bericht verzendt, wil dit meestal weten of het bericht is goed overgebracht naar en geaccepteerd door de broker of of er een fout opgetreden. Deze bevestiging positieve of negatieve betaalt de client en de broker inzicht krijgen in over de status van de overdracht van het bericht en wordt dus aangeduid als *verwerken*.

Evenzo, wanneer de broker een bericht naar een client draagt, de broker en de client tot stand wilt brengen een goed begrip van of het bericht is verwerkt en kan daarom worden verwijderd, of of de bezorging van berichten of de verwerking is mislukt, en dus de bericht mogelijk opnieuw worden geleverd.

## <a name="settling-send-operations"></a>Vereffenen bewerkingen verzenden

Bewerkingen in Service Bus altijd expliciet zijn betaald, wat betekent dat de API-bewerking voor een resultaat van de acceptatie van Service Bus aankomt, wacht u een van de ondersteunde API-Service Bus-clients, verzenden en voltooit daarna de bewerking voor het verzenden.

Als het bericht is geweigerd door de Service Bus, bevat de weigering een foutindicator en tekst met een 'tracerings-id' daarbinnen. De afwijzing bevat ook informatie over of de bewerking kan opnieuw worden geprobeerd met een verwachting van succes. Deze informatie is in de client, omgezet in een uitzondering en verheven tot de aanroeper van de bewerking voor het verzenden. Als het bericht is geaccepteerd, wordt de bewerking op de achtergrond is voltooid.

Bij het gebruik van het AMQP-protocol, dat de exclusieve protocol voor de .NET Standard-client en de Java-client is en [is een optie voor de .NET Framework client](service-bus-amqp-dotnet.md), bericht worden overgedragen en vereffeningen worden gebruikt in een pijplijn en volledig asynchrone, en het wordt aanbevolen dat u varianten van de asynchrone programming model-API.

Een afzender kunt plaatsen verschillende berichten op de kabel snel achter elkaar zonder te wachten op elk bericht dat wordt bevestigd, zoals het geval is met het protocol SBMP of met HTTP 1.1. Deze bewerkingen asynchroon verzenden voltooid omdat de respectieve berichten worden geaccepteerd en die zijn opgeslagen op gepartitioneerde entiteiten of wanneer opnieuw te verzenden naar verschillende entiteiten elkaar overlappen. De voltooiingen kunnen ook gebeuren buiten de volgorde van de oorspronkelijke verzenden.

De strategie voor het verwerken van het resultaat van de bewerkingen verzenden kan hebben invloed op de onmiddellijke en aanzienlijke prestaties voor uw toepassing. De voorbeelden in deze sectie zijn geschreven in C# en oftewel voor Java uitstel van toepassing.

Als de toepassing pieken van berichten produceert, hier met een gewone lus weergegeven en zijn om te wachten op de voltooiing van elk verzenden bewerking voordat u het volgende bericht synchrone of asynchrone API vormen taxibedrijven, 10 berichten verzenden alleen is voltooid na 10 sequentiële volledige retouren voor verwerken.

Bij een veronderstelde 70 milliseconde TCP retour latentie afstand van een on-premises site tot Service Bus en geeft slechts 10 ms voor Service Bus om te accepteren en opslaan van elk bericht inneemt de volgende lus ten minste 8 seconden, niet tellen nettolading overdrachtstijd of potentiële gevolgen van de congestie route:

```csharp
for (int i = 0; i < 100; i++)
{
  // creating the message omitted for brevity
  await client.SendAsync(…);
}
```

Als de toepassing wordt gestart van de 10 verzenden van asynchrone bewerkingen direct achter elkaar en wacht op de voltooiing van hun respectieve afzonderlijk, overlapt round trip time voor deze bewerkingen met 10 verzenden. De 10 berichten worden overgedragen direct achter elkaar, mogelijk zelfs TCP frames, delen en de totale duur van de overdracht grotendeels afhankelijk van de netwerk-gerelateerde tijd die nodig is voor het ophalen van de berichten die zijn overgebracht naar de broker.

De dezelfde veronderstellingen als voor de vorige lus, kan de totale overlappende uitvoeringstijd voor de volgende lus ook onder één seconde blijven:

```csharp
var tasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
  tasks.Add(client.SendAsync(…));
}
await Task.WhenAll(tasks);
```

Het is belangrijk te weten dat alle asynchrone programmeermodellen gebruiken een vorm van geheugen, verborgen wachtrij waarin de bewerkingen in behandeling. Wanneer [SendAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.sendasync#Microsoft_Azure_ServiceBus_QueueClient_SendAsync_Microsoft_Azure_ServiceBus_Message_) (C#) of **verzenden** (Java) retourneren, de taak verzenden is in de wachtrij in deze wachtrij, maar het protocol-gebaar alleen begint zodra deze is van de taak inschakelen om uit te voeren. Voor de code die doorgaans push pieken van berichten en waar de betrouwbaarheid van belang is, moet men niet te veel berichten in één keer 'tijdens de vlucht"zijn geplaatst omdat alle verzonden berichten geheugen duren totdat ze feitelijk zijn opgeslagen op de kabel.

Semaforen, zijn zoals wordt weergegeven in het volgende codefragment in C#, synchronisatieobjecten waarmee deze op toepassingsniveau beperking wanneer dat nodig is. Dit gebruik van een semafoor kunt u maximaal 10 berichten in één keer worden tijdens de overdracht. Een van de 10 beschikbaar semafoor vergrendelingen wordt uitgevoerd vóór het verzenden en dit wordt uitgebracht, zoals het verzenden is voltooid. De 11e pass through-de lus wordt er gewacht totdat ten minste één van de voorafgaande verzendt is voltooid en vervolgens maakt de vergrendeling beschikbaar:

```csharp
var semaphore = new SemaphoreSlim(10);

var tasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
  await semaphore.WaitAsync();

  tasks.Add(client.SendAsync(…).ContinueWith((t)=>semaphore.Release()));
}
await Task.WhenAll(tasks);
```

Apps moeten **nooit** initiëren van een bewerking voor het verzenden van asynchrone op een manier 'geactiveerd en vergeten' zonder op te halen van het resultaat van de bewerking. In dat geval kunt u de interne en onzichtbaar takenwachtrij tot uitputting geheugen laden, en voorkomen dat de toepassing verzenden fouten worden opgespoord:

```csharp
for (int i = 0; i < 100; i++)
{

  client.SendAsync(message); // DON’T DO THIS
}
```

Met een laag niveau AMQP-client ook accepteert "vooraf vereffende" overdrachten Service Bus. Een vooraf vereffende overdracht is een fire-and-forget-bewerking waarvoor het resultaat, in beide gevallen wordt niet gerapporteerd terug naar de client en het bericht wordt beschouwd als verrekend wanneer verzonden. Het ontbreken van feedback aan de client betekent ook dat er geen bruikbare gegevens beschikbaar voor diagnostische gegevens, wat betekent is dat deze modus niet in aanmerking voor ondersteuning via de ondersteuning van Azure.

## <a name="settling-receive-operations"></a>Vereffenen ontvangstbewerkingen

Ontvangen voor bewerkingen, de Service Bus-API-clients inschakelen twee verschillende modi worden expliciete: *ontvangen en Delete* en *Peek-Lock*.

De [ontvangen en Delete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modus geeft aan de broker rekening houden met alle berichten die het verzendt naar de ontvangende client als vereffende wanneer verzonden. Dit betekent dat dat het bericht wordt beschouwd als verbruikt zodra de broker is geplaatst op de kabel. Als de overdracht van het bericht is mislukt, wordt het bericht is verloren gegaan.

Het voordeel van deze modus is dat de ontvanger geen hoeft verdere actie te ondernemen op het bericht en ook niet wordt vertraagd door te wachten op het resultaat van de verwerken. Als de gegevens in de afzonderlijke berichten lage waarde hebben en/of alleen relevant zijn voor een zeer korte tijd zijn, is deze modus een redelijke keuze.

De [Peek-Lock](/dotnet/api/microsoft.servicebus.messaging.receivemode) modus wordt de broker aangegeven dat de ontvangende client wil ontvangen berichten expliciet bedragen. Het bericht wordt beschikbaar gesteld voor de ontvanger om te verwerken, terwijl gehouden onder een exclusieve vergrendeling in de service, zodat andere, concurrerende ontvangers niet kunnen zien. De duur van de vergrendeling in eerste instantie is gedefinieerd op het niveau van de wachtrij of abonnement en kan worden uitgebreid door de client die eigenaar is van de vergrendeling de [RenewLock](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_RenewLockAsync_System_String_) bewerking.

Wanneer een bericht is vergrendeld, kunnen andere clients ontvangen van dezelfde wachtrij of abonnement vergrendelingen ondernemen en het volgende beschikbare berichten niet in actieve vergrendeling ophalen. Als de vergrendeling op een bericht expliciet wordt vrijgegeven of als de vergrendeling is verlopen, verschijnt het bericht een back-up op of in de buurt van het begin van de volgorde voor het ophalen van redelivery.

Wanneer het bericht wordt herhaaldelijk vrijgegeven door ontvangers of ze de vergrendeling verstrijken voor een opgegeven aantal keren laten ([maxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount)), het bericht automatisch wordt verwijderd uit de wachtrij of abonnement en in de bijbehorende geplaatst dead-letter-wachtrij.

De ontvangende client initieert verwerken van een bericht met een positieve bevestiging wanneer wordt de [voltooid](/dotnet/api/microsoft.servicebus.messaging.queueclient.complete#Microsoft_ServiceBus_Messaging_QueueClient_Complete_System_Guid_) op het niveau van de API. Hiermee geeft u aan de broker dat het bericht is verwerkt en het bericht is verwijderd uit de wachtrij of abonnement. De broker-antwoorden op van de ontvanger verwerken doel met een reactie die aangeeft of de verwerken kan worden uitgevoerd.

Wanneer de client ontvangt een bericht niet kan verwerken, maar wil dat het bericht dat moet opnieuw worden bezorgd, expliciet kunt stellen voor het bericht dat moet worden vrijgegeven en ontgrendeld direct door het aanroepen van [afbreken](/dotnet/api/microsoft.servicebus.messaging.queueclient.abandon) of het niets te doen en laat de vergrendeling is verlopen.

Als een client ontvangen een bericht niet kan verwerken en weet dat het bericht redelivering en opnieuw wordt geprobeerd de bewerking niet helpt, wordt het bericht dat deze is verplaatst naar de dead-letter-wachtrij door het aanroepen kunt weigeren [onbestelbare berichten](/dotnet/api/microsoft.servicebus.messaging.queueclient.deadletter), die ook kan het instellen van een aangepaste eigenschap met inbegrip van een redencode dat kan worden opgehaald met het bericht uit de wachtrij voor onbestelbare berichten.

Een speciaal geval van schikking is om dit in een apart artikel wordt beschreven.

De **voltooid** of **onbestelbare berichten** bewerkingen, evenals de **RenewLock** bewerkingen mislukken vanwege netwerkproblemen, als de vastgehouden vergrendeling is verlopen, of er andere zijn aan de serverkant voorwaarden die voorkomen verwerken dat. In een van de laatste gevallen verzendt de service een negatieve bevestiging die als een uitzondering in de API-clients. Als de reden hiervoor een netwerkverbinding verbroken is, wordt de vergrendeling is verwijderd omdat Service Bus biedt geen ondersteuning voor herstel van bestaande AMQP koppelingen op een andere verbinding.

Als **voltooid** mislukt, dit gebeurt doorgaans aan het einde van de afhandeling van berichten en in sommige gevallen na minuten, verwerking van de ontvangende toepassing kunt bepalen of deze de status van het werk blijven behouden en hetzelfde negeert bericht wanneer deze een tweede keer wordt geleverd of of Hiermee verwijdert het resultaat van het werk en nieuwe als het bericht pogingen opnieuw wordt bezorgd.

De typische mechanisme voor het identificeren van dubbele bericht leveringen is door het controleren van de bericht-id, dat kan en moet worden ingesteld door de afzender naar een unieke waarde, mogelijk zijn uitgelijnd met een id van het oorspronkelijke proces. Een jobplanner waarschijnlijk de bericht-id wordt ingesteld op de id van de taak die deze probeert om toe te wijzen aan een werknemer met de opgegeven werknemer en de werkrol de tweede instantie van de taaktoewijzing zou negeren als deze taak is al voltooid.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)
