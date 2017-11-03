---
title: Azure Service Bus bericht overdrachten vergrendelingen en vereffening | Microsoft Docs
description: Overzicht van Service Bus bericht overdrachten en vereffening bewerkingen
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
ms.date: 09/27/2017
ms.author: sethm
ms.openlocfilehash: edb6e207852fa59d5828906c891693f367739c9c
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="message-transfers-locks-and-settlement"></a>Bericht overdrachten, vergrendelingen en vereffening

De meest centrale mogelijkheid van een bericht broker zoals Service Bus is berichten in een wachtrij of onderwerp accepteren en houdt deze beschikbaar voor later gebruik. *Verzenden* is de term die meestal voor de overdracht van een bericht in de broker bericht gebruikt wordt. *Ontvangen* is de term meestal gebruikt voor de overdracht van een bericht naar een client ophalen.

Wanneer een client een bericht verzendt, wil software meestal weten of het bericht is goed overgebracht naar en geaccepteerd door de broker of of de fout is opgetreden. Deze bevestiging positief of negatief betaalt van de client en de broker kennis over de status van de overdracht van het bericht en dus wordt aangeduid als *vereffening*.

Evenzo, wanneer de broker een bericht naar een client overdraagt, de broker en de client tot stand wilt brengen een goed begrip van of het bericht is verwerkt en daarom kan worden verwijderd, of of de levering van berichten of verwerking is mislukt, en dus de bericht wellicht opnieuw worden geleverd.

## <a name="settling-send-operations"></a>Verzenden operations vereffenen

Bewerkingen in Service Bus altijd expliciet worden afgewikkeld, wat betekent dat de API-bewerking is een resultaat acceptatie van Service Bus aankomt, wacht u een van de ondersteunde Service Bus-API-clients, verzenden en voltooit daarna de verzendbewerking.

Als het bericht is geweigerd door de Service Bus, bevat de afwijzing een foutindicator en tekst met een 'tracerings-id' daarbinnen. De afwijzing bevat ook informatie over of de bewerking kan opnieuw worden geprobeerd met een verwachting van slagen. Deze informatie is omgezet in een uitzondering en verheven tot de aanroeper van de verzendbewerking in de client. Als het bericht is geaccepteerd, wordt de bewerking achtergrond is voltooid.

Wanneer u het AMQP-protocol, dat het exclusieve protocol voor de standaard .NET client en de Java-client en [dit is een optie voor de .NET Framework client](service-bus-amqp-dotnet.md), bericht overdrachten en vereffeningen worden gebruikt in een pijplijn en volledig asynchrone, en het wordt aanbevolen dat u de asynchrone programming model API varianten.

Een afzender kunt plaatsen meerdere berichten op de kabel snel achter elkaar zonder te wachten op elk bericht bevestigd, zoals het geval is het protocol SBMP of met HTTP 1.1 anders zouden zijn. Deze bewerkingen asynchrone verzending niet voltooien omdat de respectieve berichten worden geaccepteerd en op een gepartitioneerde entiteiten opgeslagen of wanneer opnieuw verzenden naar andere entiteiten elkaar overlappen. De voltooiingen kunnen ook gebeuren buiten de volgorde van de oorspronkelijke verzenden.

De strategie voor het verwerken van de resultaten van de bewerkingen verzenden kan hebben invloed op de onmiddellijke en aanzienlijke prestaties voor uw toepassing. De voorbeelden in deze sectie zijn geschreven in C# en oftewel voor Java Futures toepassen.

Als de toepassing bursts van berichten produceert, hier geïllustreerd met een gewone lus en zou wacht tot de voltooiing van elke verzendbewerking voor het verzenden van het volgende bericht synchrone of asynchrone API shapes voor zowel verzenden van berichten 10 alleen is voltooid na 10 sequentiële volledige retouren voor vereffening.

Met een veronderstelde 70 milliseconde TCP RTT latentie afstand van een on-premises site tot Service Bus en net 10 ms voor Service Bus geven om te accepteren en opslaan van elk bericht de volgende lus in beslag neemt ten minste 8 seconden, tellen niet mee nettolading overdrachtstijd of mogelijke gevolgen van de congestie route:

```csharp
for (int i = 0; i < 100; i++)
{
  // creating the message omitted for brevity
  await client.SendAsync(…);
}
```

Als de toepassing wordt de 10 asynchrone verzending bewerkingen onmiddellijk achter elkaar gestart en wacht op de voltooiing van hun respectieve afzonderlijk, overlapt round trip time 10 bewerkingen verzenden. De 10 berichten worden overgedragen onmiddellijk achter elkaar mogelijk zelfs TCP frames, delen en de totale duur van de overdracht grotendeels afhankelijk van het netwerk gerelateerde tijdsduur voor het ophalen van de berichten die zijn overgebracht naar de broker.

Dezelfde veronderstellingen als voor de voorafgaande lus, de totale overlapte uitvoeringstijd voor de volgende lus mogelijk ook onder één seconde blijven:

```csharp
var tasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
  tasks.Add(client.SendAsync(…));
}
await Task.WhenAll(tasks.ToArray());
```

Het is belangrijk te weten dat alle asynchrone programmeermodellen gebruiken een vorm van geheugen, verborgen wachtrij van de bewerkingen die in behandeling. Wanneer [SendAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.sendasync#Microsoft_Azure_ServiceBus_QueueClient_SendAsync_Microsoft_Azure_ServiceBus_Message_) (C#) of **verzenden** return (Java), de taak verzenden is in de wachtrij geplaatst in die wachtrij, maar de gebaar protocol alleen begint zodra dit is ingeschakeld voor de taak om uit te voeren. Voor de code die doorgaans push bursts van berichten en waar de betrouwbaarheid van belang is, moet nauwkeurig niet te veel berichten in één keer 'tijdens de vlucht' worden gebracht omdat alle verzonden berichten geheugen in beslag totdat ze naar de kabel feitelijk zijn geplaatst.

Semaforen, zijn zoals wordt weergegeven in het volgende codefragment in C#, synchronisatieobjecten waarmee u dergelijke op toepassingsniveau beperking wanneer deze nodig is. Dit gebruik van een semafoor kunt u maximaal 10 berichten in één keer worden tijdens de vlucht. Een van de 10 beschikbaar semafoor vergrendelingen wordt uitgevoerd vóór het verzenden en ze zijn vrijgegeven, zoals het verzenden is voltooid. De 11 pass through-de lus wacht totdat ten minste één van de voorafgaande verzendt is voltooid en vervolgens de vergrendeling beschikbaar:

```csharp
var semaphore = new SemaphoreSlim(10);

var tasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
  await semaphore.WaitAsync();

  tasks.Add(client.SendAsync(…).ContinueWith((t)=>semaphore.Release()));
}
await Task.WhenAll(tasks.ToArray());
```

Toepassingen moeten **nooit** initiëren van een asynchrone verzending-bewerking op een manier 'activeert en vergeet' zonder het ophalen van de uitkomst van de bewerking. In dat geval kunt laden van de interne en onzichtbaar takenwachtrij tot uitputting geheugen en voorkomen dat de toepassing verzenden fouten worden opgespoord:

```csharp
for (int i = 0; i < 100; i++)
{

  client.SendAsync(message); // DON’T DO THIS
}
```

Service Bus accepteert met een laag niveau AMQP-client ook 'vooraf vereffende' overdrachten. De overdracht van een vooraf vereffende is een bewerking fire en vergeet waarvoor de uitkomst, in beide gevallen is niet gemeld terug naar de client en het bericht wordt beschouwd als afgewikkeld wanneer ze worden verzonden. Het ontbreken van feedback naar de client betekent ook dat er geen bruikbare gegevens beschikbaar voor diagnostische gegevens, wat betekent zijn dat deze modus niet in aanmerking voor hulp via de ondersteuning van Azure.

## <a name="settling-receive-operations"></a>Ontvangstbewerkingen vereffenen

Voor ontvangstbewerkingen, de Service Bus-API-clients inschakelen twee verschillende expliciete modi: *ontvangen en Delete* en *Peek vergrendeling*.

De [ontvangen en Delete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modus vertelt de broker rekening houden met alle berichten die aan de ontvangende client worden als vereffende wanneer verzonden verzonden. Dat betekent dat het bericht wordt beschouwd als verbruikt zodra de broker is geplaatst op de kabel. Als de overdracht van het bericht is mislukt, wordt het bericht is verloren gegaan.

Opwaartse van deze modus is dat de ontvanger heeft geen verdere actie te ondernemen voor het bericht nodig en ook niet gehouden is door het wachten op het resultaat van de vereffening. Als de gegevens in de afzonderlijke berichten lage waarde hebben en/of alleen zinvol voor zeer korte tijd zijn, is deze modus een redelijke keuze.

De [Peek vergrendeling](/dotnet/api/microsoft.servicebus.messaging.receivemode) modus vertelt de broker dat de ontvangende client wil ontvangen berichten expliciet vereffenen. Het bericht wordt beschikbaar gesteld voor de ontvanger worden verwerkt, terwijl gehouden onder een exclusieve vergrendeling in de service, zodat deze kunnen niet worden weergegeven door andere, concurrerende ontvangers. De duur van de vergrendeling in eerste instantie is gedefinieerd op het niveau van de wachtrij of -abonnement en kan worden uitgebreid door de client die eigenaar is van de vergrendeling de [RenewLock](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_RenewLockAsync_System_String_) bewerking.

Wanneer een bericht is vergrendeld, kunnen andere clients ontvangen uit dezelfde wachtrij of abonnement ondernemen vergrendelingen en de volgende beschikbare berichten niet onder actieve vergrendeling ophalen. Wanneer de vergrendeling voor een bericht expliciet wordt vrijgegeven of wanneer de vergrendeling verloopt, verschijnt het bericht een back-up op of in de buurt van het begin van de volgorde ophalen voor redelivery.

Wanneer het bericht wordt herhaaldelijk vrijgegeven door ontvangers of ze de vergrendeling verstrijken voor een opgegeven aantal keren laten ([maxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount)), het bericht automatisch wordt verwijderd uit de wachtrij of een abonnement en in het bijbehorende geplaatst wachtrij voor onbestelbare berichten.

De ontvangende client regeling van een ontvangen bericht met een positieve bevestiging initieert wanneer roept [Complete](/dotnet/api/microsoft.servicebus.messaging.queueclient.complete#Microsoft_ServiceBus_Messaging_QueueClient_Complete_System_Guid_) op het niveau van de API. Hiermee geeft u aan de broker dat het bericht is verwerkt en het bericht is verwijderd uit de wachtrij of abonnement. De broker antwoorden op de ontvanger vereffening bedoeling met antwoord geeft aan of de vereffening kan worden uitgevoerd.

Wanneer de ontvangende client een bericht niet kan verwerken, maar het bericht opnieuw worden bezorgd wil, expliciet kunt vragen voor het bericht worden vrijgegeven en onmiddellijk worden ontgrendeld door het aanroepen van [afbreken](/dotnet/api/microsoft.servicebus.messaging.queueclient.abandon) of het niets te doen en laat de vergrendeling is verlopen.

Als een ontvangende client mislukt een bericht te verwerken en weet dat het bericht redelivering en het opnieuw te proberen niet helpt, kan hij het bericht dat wordt verplaatst in de wachtrij voor onbestelbare berichten door aan te roepen afwijzen [wachtrij voor onbestelbare](/dotnet/api/microsoft.servicebus.messaging.queueclient.deadletter), die ook kunt u de instelling van een aangepaste eigenschap met inbegrip van een redencode dat kan worden opgehaald met het bericht uit de wachtrij voor onbestelbare berichten.

Een speciaal soort vereffening is uitgesteld, dit wordt beschreven in een apart artikel.

De **Complete** of **wachtrij voor onbestelbare** operations, evenals de **RenewLock** bewerkingen kunnen mislukken vanwege netwerkproblemen, als de vastgehouden vergrendeling is verlopen of er andere zijn voorwaarden aan servicezijde waardoor vereffening. In een van de laatste gevallen verzendt de service een negatieve bevestiging dat verwerkingsinformatie als een uitzondering in de API-clients. Als de reden een verbroken netwerkverbinding is, wordt de vergrendeling is verwijderd sinds de Service Bus biedt geen ondersteuning voor herstel van bestaande AMQP koppelingen op een andere verbinding.

Als **Complete** mislukt, dat plaatsvindt doorgaans aan het einde van de verwerking van het bericht en in sommige gevallen na de minuten, verwerking van de ontvangende toepassing kunt bepalen of het behouden de status van het werk blijft en dezelfde worden genegeerd bericht wanneer deze een tweede keer is geleverd of of Hiermee verwijdert het resultaat van het werk en probeert opnieuw als het bericht opnieuw wordt bezorgd.

De typische mechanisme voor het identificeren van dubbele berichten leveringen is door het controleren van de bericht-id, die kan en moet worden ingesteld door de afzender naar een unieke waarde, mogelijk zijn uitgelijnd met een id van het oorspronkelijke proces. Een jobplanner waarschijnlijk de bericht-id wordt ingesteld op de id van de taak die deze probeert om toe te wijzen aan een werknemer met het opgegeven werkproces en de werknemer zou het tweede exemplaar van de taaktoewijzing van de negeren als die taak is al voltooid.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichtenservice, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)
