---
title: Aanbevolen procedures voor het verbeteren van de prestaties met Azure Service Bus | Microsoft Docs
description: Beschrijft hoe Service Bus gebruiken om prestaties te optimaliseren wanneer brokered berichten uitwisselen.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e756c15d-31fc-45c0-8df4-0bca0da10bb2
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: sethm
ms.openlocfilehash: 1f57fbb8e2a86b744808ee844e5f853bdb587a5d
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2017
---
# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Aanbevolen procedures voor verbeterde prestaties met behulp van Service Bus-berichtenservice

In dit artikel wordt beschreven hoe u [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) om prestaties te optimaliseren wanneer brokered berichten uitwisselen. Het eerste deel van dit artikel beschrijft de verschillende methoden die worden aangeboden aan de prestaties verbeteren. Het tweede gedeelte biedt richtlijnen voor het gebruik van Service Bus op een manier die de beste prestaties in een bepaald scenario te kan bieden.

In dit onderwerp wordt de term 'client' verwijst naar een entiteit die toegang heeft tot de Service Bus. Een client kan duren voordat de rol van een afzender of een ontvanger. De term 'afzender' wordt gebruikt voor een Service Bus-wachtrij of onderwerp client dat berichten naar een Service Bus-wachtrij of onderwerp abonnement verzendt. De term "ontvanger" verwijst naar een Service Bus-wachtrij of abonnement client die berichten van een Service Bus-wachtrij of abonnement ontvangt.

Deze secties vindt enkele concepten die Service Bus gebruikt ter verbetering van prestaties.

## <a name="protocols"></a>Protocollen
Service Bus kunnen clients verzenden en ontvangen van berichten uit via een van drie protocollen:

1. Geavanceerde Message Queuing-Protocol (AMQP)
2. Servicebus-berichtenservice Protocol (SBMP)
3. HTTP

AMQP en SBMP zijn efficiënter, omdat ze de verbinding met Service Bus onderhouden, zolang de messagingfactory bestaat. Ook wordt batchverwerking en veelgevraagde geïmplementeerd. Tenzij expliciet wordt vermeld, is alle inhoud in dit onderwerp wordt aangenomen dat het gebruik van AMQP of SBMP.

## <a name="reusing-factories-and-clients"></a>Hergebruik van de fabrieken en clients
Service Bus-clienttoepassing objecten, zoals [QueueClient] [ QueueClient] of [MessageSender][MessageSender], worden gemaakt via een [ MessagingFactory] [ MessagingFactory] -object, dat ook interne beheer van verbindingen biedt. U moet messaging fabrieken of wachtrij, onderwerp en Abonnementclients niet sluiten nadat u een bericht verzenden en opnieuw maken wanneer u het volgende bericht verzenden. Sluiten van een messagingfactory Hiermee verwijdert u de verbinding met de Service Bus-service en een nieuwe verbinding tot stand is gebracht wanneer de factory opnieuw te maken. Maken van een verbinding is een dure bewerking die u voorkomen kunt door het opnieuw met de dezelfde fabriek en clientobjecten voor meerdere bewerkingen. U kunt veilig de [QueueClient] [ QueueClient] -object voor het verzenden van berichten van gelijktijdige asynchrone bewerkingen en meerdere threads. 

## <a name="concurrent-operations"></a>Gelijktijdige bewerkingen
Een bewerking (verzenden, ontvangen, verwijderen, enz.) neemt enige tijd in. Deze tijd bevat de verwerking van de bewerking door de Service Bus-service naast de latentie van de aanvraag en het antwoord. Voor een verhoging van het aantal bewerkingen per keer moeten worden operations gelijktijdig worden uitgevoerd. U kunt dit op verschillende manieren doen:

* **Asynchrone bewerkingen**: de client plant u bewerkingen door het uitvoeren van asynchrone bewerkingen. De volgende aanvraag is gestart voordat de vorige aanvraag is voltooid. Hier volgt een voorbeeld van een verzendbewerking asynchroon:
  
 ```csharp
  BrokeredMessage m1 = new BrokeredMessage(body);
  BrokeredMessage m2 = new BrokeredMessage(body);
  
  Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #1");
    });
  Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #2");
    });
  Task.WaitAll(send1, send2);
  Console.WriteLine("All messages sent");
  ```
  
  Dit is een voorbeeld van een asynchrone bewerking ontvangen:
  
  ```csharp
  Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
  Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
  
  Task.WaitAll(receive1, receive2);
  Console.WriteLine("All messages received");
  
  async void ProcessReceivedMessage(Task<BrokeredMessage> t)
  {
    BrokeredMessage m = t.Result;
    Console.WriteLine("{0} received", m.Label);
    await m.CompleteAsync();
    Console.WriteLine("{0} complete", m.Label);
  }
  ```
* **Meerdere fabrieken**: alle clients (afzenders naast ontvangers) die zijn gemaakt door dezelfde fabriek delen één TCP-verbinding. De maximale doorvoer wordt beperkt door het aantal bewerkingen die deze TCP-verbinding kunt doorlopen. De doorvoer die kan worden verkregen met een enkele factory varieert aanzienlijk met TCP-inclusief de tijd en de grootte van het bericht. U moet meerdere messaging fabrieken gebruiken voor hogere doorvoersnelheden.

## <a name="receive-mode"></a>Modus ontvangen
Wanneer u een wachtrij of abonnement client maakt, kunt u een receive-modus: *Peek vergrendeling* of *ontvangt en verwijdert*. De standaardwaarde modus ontvangen is [PeekLock][PeekLock]. Wanneer in deze modus werkt, verzendt de client een aanvraag voor een bericht ontvangen van Service Bus. Nadat de client kan het bericht heeft ontvangen, wordt een aanvraag voor het voltooien van het bericht verzonden.

Wanneer de receive-modus is ingesteld op [ReceiveAndDelete][ReceiveAndDelete], beide stappen worden gecombineerd in één aanvraag. Dit vermindert het totale aantal bewerkingen en kan verbeteren de algehele bericht doorvoer. Deze prestatieverbetering wordt geleverd indien het verlies van berichten.

Service Bus biedt geen ondersteuning voor transacties voor ontvangen en delete-bewerkingen. Bovendien peek vergrendeling semantiek zijn vereist voor elk scenario's waarin de client wil uit te stellen of [onbestelbare](service-bus-dead-letter-queues.md) een bericht.

## <a name="client-side-batching"></a>Batchverwerking aan de clientzijde
Clientzijde batchverwerking kan een wachtrij of onderwerp client vertraging in het verzenden van een bericht voor een bepaalde periode. Als de client aanvullende berichten tijdens deze periode verzendt, verzendt het de berichten in één batch. Client-side batchverwerking ook zorgt ervoor dat een wachtrij of abonnement client meerdere batch **Complete** aanvragen in één aanvraag. Batchverwerking is alleen beschikbaar voor asynchrone **verzenden** en **Complete** bewerkingen. Synchrone bewerkingen worden onmiddellijk naar de Service Bus-service verzonden. Geen batchverwerking optreden voor peek of ontvangstbewerkingen, noch gebeurt batchverwerking over clients.

Een client gebruikt standaard een batch-interval van 20ms. U kunt het interval batch wijzigen door in te stellen de [BatchFlushInterval] [ BatchFlushInterval] eigenschap voordat u de messagingfactory maakt. Deze instelling geldt voor alle clients die zijn gemaakt door deze factory.

Om te schakelen batchverwerking, stelt de [BatchFlushInterval] [ BatchFlushInterval] eigenschap **TimeSpan.Zero**. Bijvoorbeeld:

```csharp
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Batchverwerking heeft geen invloed op het aantal factureerbare messaging bewerkingen en is alleen beschikbaar voor de Service Bus-client-protocol. Het HTTP-protocol biedt geen ondersteuning voor batchverwerking.

## <a name="batching-store-access"></a>Toegang tot store batchverwerking
Service Bus batches meerdere berichten voor een verhoging van de doorvoer van een wachtrij, onderwerp of abonnement wanneer deze naar de opslag van de interne schrijft. Als ingeschakeld op een wachtrij of onderwerp, berichten schrijven naar het archief wordt in batch worden opgenomen. Als ingeschakeld op een wachtrij of een abonnement, het verwijderen van berichten uit de store wordt in batch worden opgenomen. Als toegang tot batch store is ingeschakeld voor een entiteit, een Service Bus een schrijfbewerking van het archief met betrekking tot die entiteit door maximaal 20 MS vertraging. Aanvullende store bewerkingen die zich tijdens dit interval voordoen worden toegevoegd aan de batch. Batch verwerkt store toegang alleen van invloed op **verzenden** en **Complete** operations; ontvangen bewerkingen worden niet beïnvloed. Toegang tot batch store is een eigenschap van een entiteit. Batchverwerking vindt plaats via alle entiteiten waarmee toegang tot batch store.

Wanneer u een nieuwe wachtrij, onderwerp of abonnement maakt, wordt toegang tot batch store standaard ingeschakeld. Om te schakelen toegang tot batch store, stelt de [EnableBatchedOperations] [ EnableBatchedOperations] eigenschap **false** voordat u de entiteit. Bijvoorbeeld:

```csharp
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Toegang tot batch store heeft geen invloed op het aantal factureerbare messaging bewerkingen en is een eigenschap van een wachtrij, onderwerp of abonnement. Het is onafhankelijk van de receive-modus en het protocol dat wordt gebruikt tussen een client en de Service Bus-service.

## <a name="prefetching"></a>Veelgevraagde
[Veelgevraagde](service-bus-prefetch.md) kan de wachtrij of abonnement client aanvullende berichten laden van de service bij het uitvoeren van een ontvangstbewerking. De client slaat deze berichten in een lokale cache. De grootte van de cache wordt bepaald door de [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] of [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] eigenschappen. Elke client waarmee veelgevraagde onderhoudt een eigen cache. Een cache wordt niet gedeeld door clients. Als de client een ontvangstbewerking initieert en de cache leeg is, worden berichten batchgewijs verzonden door de service. De grootte van de batch is gelijk aan de grootte van de cache of 256 KB, afhankelijk van wat kleiner is. Als de client een ontvangstbewerking initieert en de cache een bericht bevat, wordt het bericht wordt overgenomen uit de cache.

Wanneer een bericht is tabelruimte, wordt het prefetched bericht vergrendeld in de service. Op deze manier kunnen de prefetched bericht kan niet ontvangen door een andere ontvanger. Als de ontvanger het bericht niet voltooien kan voordat de vergrendeling verloopt, wordt het bericht beschikbaar voor andere ontvangers. De prefetched kopie van het bericht blijft in de cache. De ontvanger die de verlopen cachekopie verbruikt ontvangt een uitzondering opgetreden bij een poging te voltooien dat bericht. Standaard wordt de bericht-vergrendeling verloopt na 60 seconden. Deze waarde kan worden uitgebreid tot 5 minuten. Om te voorkomen dat het verbruik van verlopen berichten, moet de cachegrootte altijd kleiner zijn dan het aantal berichten die door een client kan worden gebruikt binnen de time-outinterval vergrendelen.

Wanneer u de vervaldatum van de vergrendeling standaard van 60 seconden, een goede waarde voor [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] is de maximaal 20 keer verwerkingssnelheid weer van alle ontvangers van de gegevensfactory. Bijvoorbeeld, een factory maakt 3 ontvangers en elke ontvanger kan maximaal 10 berichten per seconde worden verwerkt. Het aantal prefetch mag niet meer dan 20 X 3 X 10 = 600. Standaard [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] is ingesteld op 0, wat betekent dat er geen berichten meer van de service worden opgehaald.

De totale doorvoer voor een wachtrij of een abonnement veelgevraagde berichten worden verhoogd omdat dit het totale aantal berichtbewerkingen of retouren verlaagt. Ophalen van het eerste bericht echter duurt langer (vanwege de verhoogde berichtgrootte). Ontvangen van berichten tabelruimte worden sneller omdat deze berichten door de client al zijn gedownload.

De time-to-live (TTL) eigenschap van een bericht wordt gecontroleerd door de server op het moment dat de server het bericht naar de client verzendt. De client controleert de berichteigenschap TTL niet wanneer het bericht wordt ontvangen. Het bericht kan in plaats daarvan worden ontvangen, zelfs als de TTL van het bericht is verlopen terwijl het bericht is in het cachegeheugen van de client.

Veelgevraagde heeft geen invloed op het aantal factureerbare messaging bewerkingen en is alleen beschikbaar voor de Service Bus-client-protocol. Het HTTP-protocol biedt geen ondersteuning voor veelgevraagde. Veelgevraagde is beschikbaar voor synchrone en asynchrone bewerkingen ontvangen.

## <a name="express-queues-and-topics"></a>Wachtrijen en onderwerpen Express

Express-entiteiten hoge doorvoer en lagere latentie scenario's inschakelen en worden alleen ondersteund in de Standard-laag. Entiteiten die zijn gemaakt [Premium-naamruimten](service-bus-premium-messaging.md) bieden geen ondersteuning voor de snelle optie. Met de express-entiteiten, als een bericht wordt verzonden naar een wachtrij of onderwerp, is het bericht niet onmiddellijk opgeslagen in de berichten-store. In plaats daarvan wordt het tijdelijk opgeslagen in het geheugen. Als een bericht in de wachtrij van meer dan een paar seconden blijft, is het automatisch naar een stabiele opslag, dus te beveiligen tegen verlies als gevolg van een storing geschreven. Schrijven van het bericht in een geheugencache verhoogt de doorvoer en vermindert de latentie omdat er geen toegang tot opslag stabiele op het moment dat het bericht wordt verzonden. Berichten die worden gebruikt binnen een paar seconden worden niet naar de berichten-store geschreven. Het volgende voorbeeld wordt een snelle onderwerp.

```csharp
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Als een bericht weergegeven met essentiële informatie die mag geen verloren wordt verzonden naar een snelle entiteit, Service Bus het bericht naar een stabiele opslaglocatie door in te stellen om onmiddellijk kunt afdwingen door de afzender de [ForcePersistence] [ ForcePersistence] eigenschap **true**.

> [!NOTE]
> Express-entiteiten bieden geen ondersteuning voor transacties.

## <a name="use-of-partitioned-queues-or-topics"></a>Gebruik van gepartitioneerde wachtrijen en onderwerpen
Intern, Service Bus maakt gebruik van hetzelfde knooppunt en messaging opslaan om te verwerken en opslaan van alle berichten voor een Berichtentiteit (wachtrij of onderwerp). Een [gepartitioneerde wachtrij of onderwerp](service-bus-partitioning.md), aan de andere kant is verdeeld over meerdere knooppunten en de messaging-stores. Gepartitioneerde wachtrijen en onderwerpen niet alleen resulteert in een hogere doorvoer dan reguliere wachtrijen en onderwerpen, ze hogere beschikbaarheid vertonen. Als u een gepartitioneerde entiteit, stelt de [EnablePartitioning] [ EnablePartitioning] eigenschap **true**, zoals wordt weergegeven in het volgende voorbeeld. Zie voor meer informatie over gepartitioneerde entiteiten [gepartitioneerde berichtentiteiten][Partitioned messaging entities].

```csharp
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Gebruik van meerdere wachtrijen

Als het is niet mogelijk om een gepartitioneerde wachtrij of onderwerp te gebruiken of de verwachte belasting kan niet worden verwerkt door een enkele gepartitioneerde wachtrij of onderwerp, moet u meerdere messaging-entiteiten. Wanneer u meerdere entiteiten, moet u een specifieke client voor elke entiteit in plaats van dezelfde client voor alle entiteiten maken.

## <a name="development-and-testing-features"></a>Ontwikkeling en tests functies

Service Bus is een functie die wordt gebruikt voor het ontwikkeling die **mag nooit worden gebruikt in productie configuraties**: [TopicDescription.EnableFilteringMessagesBeforePublishing][].

Wanneer u filters of nieuwe regels worden toegevoegd aan het onderwerp, kunt u [TopicDescription.EnableFilteringMessagesBeforePublishing][] om te controleren of de nieuwe filterexpressie werkt zoals verwacht.

## <a name="scenarios"></a>Scenario's
De volgende secties beschrijven typische berichtverzending en overzicht van de gewenste instellingen op Service Bus. Doorvoersnelheid (minder dan 1 bericht/seconde) zo klein zijn geclassificeerd, Gemiddeld (bericht per seconde van 1 of groter maar minder dan 100 berichten/seconde) en een hoge (100 berichten/seconde of hoger). Het aantal clients zijn geclassificeerd als kleine (5 of minder), Gemiddeld (meer dan 5, maar kleiner dan of gelijk aan 20), en grote (meer dan 20).

### <a name="high-throughput-queue"></a>Hoge gegevensdoorvoer wachtrij
Doel: Maximaliseert de doorvoer van een enkele wachtrij. Het aantal afzenders en ontvangers is klein.

* Een gepartitioneerde wachtrij gebruiken voor betere prestaties en beschikbaarheid.
* Om te verbeteren de algehele verzendsnelheid in de wachtrij, gebruikt u meerdere messagingfactory afzenders maken. Gebruik voor elke afzender asynchrone bewerkingen of meerdere threads.
* Gebruik meerdere messagingfactory ontvangers maken voor een verhoging van de totale frequentie van ontvangen uit de wachtrij.
* Asynchrone bewerkingen om te profiteren van de client-side batchverwerking gebruiken.
* Het interval voor batchen instelt op 50ms om te beperken het aantal gegevensoverdrachten van Service Bus-client-protocol. Als u meerdere afzenders gebruikt, vergroot u het interval dat batchen moet 100ms.
* Toegang tot batch store ingeschakeld laten. Dit verhoogt de snelheid waarmee berichten kunnen worden geschreven in de wachtrij.
* Het aantal prefetch ingesteld op 20 keer de maximale verwerkingssnelheden van alle ontvangers van een fabriek. Dit vermindert het aantal gegevensoverdrachten van Service Bus-client-protocol.

### <a name="multiple-high-throughput-queues"></a>Meerdere hoge gegevensdoorvoer wachtrijen
Doel: Het maximaliseren totale doorvoer van meerdere wachtrijen. De doorvoer van een afzonderlijke wachtrij is Gemiddeld of hoog.

Gebruik de instellingen die worden beschreven om de doorvoer van een enkele wachtrij maximaliseren om maximale doorvoer via meerdere wachtrijen. Bovendien kunt u verschillende fabrieken maken clients die verzenden of ontvangen van andere wachtrijen.

### <a name="low-latency-queue"></a>Lage latentie wachtrij
Doel: Latentie van de end-to-end van een wachtrij of onderwerp minimaliseren. Het aantal afzenders en ontvangers is klein. De doorvoer van de wachtrij is klein of Gemiddeld.

* Een gepartitioneerde wachtrij gebruiken voor verbeterde beschikbaarheid.
* Batchverwerking aan de clientzijde niet uitschakelen. De client onmiddellijk een bericht.
* Batch store toegang uitschakelen. Het bericht schrijft de service onmiddellijk naar de store.
* Als een enkele client wordt gebruikt, moet u het aantal prefetch ingesteld op 20 keer de verwerkingssnelheid van de ontvanger. Als er meerdere berichten plaatsvinden in de wachtrij op hetzelfde moment, verzendt de Service Bus-clientprotocol ze allemaal op hetzelfde moment. Wanneer de client het volgende bericht ontvangt, is dat bericht al in de lokale cache. De cache moet klein zijn.
* Als meerdere clients wordt gebruikt, moet u het aantal prefetch ingesteld op 0. Op deze manier kan de tweede client tijdens de eerste client het eerste bericht steeds verwerkt nog het tweede bericht ontvangen.

### <a name="queue-with-a-large-number-of-senders"></a>Wachtrij met een groot aantal afzenders
Doel: Het maximaliseren van doorvoer van een wachtrij of onderwerp met een groot aantal afzenders. Elke afzender verzendt berichten met een gemiddeld tarief. Het aantal ontvangers is klein.

Service Bus kunnen maximaal 1000 gelijktijdige verbindingen met een Berichtentiteit (of 5000 met AMQP). Deze limiet wordt afgedwongen op het niveau van de naamruimte en onderwerpen-wachtrijen-abonnementen worden beperkt door de limiet van gelijktijdige verbindingen per naamruimte. Voor wachtrijen, wordt dit nummer gedeeld tussen afzenders en ontvangers. Als alle 1000 verbindingen vereist voor afzenders zijn, moet u de wachtrij vervangen door een onderwerp met één abonnement. Een onderwerp maximaal 1000 gelijktijdige verbindingen aanvaardt van afzenders, terwijl het abonnement een extra 1000 gelijktijdige verbindingen van ontvangers aanvaardt. Als meer dan 1000 gelijktijdige afzenders vereist zijn, moeten de afzenders van berichten verzenden naar de Service Bus-protocol via HTTP.

Als u wilt maximaliseren van doorvoer, het volgende doen:

* Een gepartitioneerde wachtrij gebruiken voor betere prestaties en beschikbaarheid.
* Als u elke afzender bevindt zich in een ander proces, gebruikt u dezelfde fabriek per proces.
* Asynchrone bewerkingen om te profiteren van de client-side batchverwerking gebruiken.
* Gebruik de standaardwaarde voor het interval van 20ms batchverwerking om Verminder het aantal gegevensoverdrachten van Service Bus-client-protocol.
* Toegang tot batch store ingeschakeld laten. Dit verhoogt de snelheid waarmee berichten kunnen worden geschreven in de wachtrij of onderwerp.
* Het aantal prefetch ingesteld op 20 keer de maximale verwerkingssnelheden van alle ontvangers van een fabriek. Dit vermindert het aantal gegevensoverdrachten van Service Bus-client-protocol.

### <a name="queue-with-a-large-number-of-receivers"></a>Wachtrij met een groot aantal ontvangers
Doel: Het maximaliseren van de snelheid van het ontvangen van een wachtrij of een abonnement met een groot aantal ontvangers. Elke ontvanger ontvangt berichten met een gemiddeld snelheid. Het aantal afzenders is klein.

Service Bus kunnen maximaal 1000 gelijktijdige verbindingen met een entiteit. Als een wachtrij meer dan 1000 ontvangers vereist, moet u de wachtrij vervangen door een onderwerp met meerdere abonnementen. Elk abonnement kan maximaal 1000 gelijktijdige verbindingen ondersteunen. U kunt ook ontvangers hebben toegang tot de wachtrij via het HTTP-protocol.

Als u wilt maximaliseren van doorvoer, het volgende doen:

* Een gepartitioneerde wachtrij gebruiken voor betere prestaties en beschikbaarheid.
* Als u elke ontvanger zich bevindt in een ander proces, gebruikt u dezelfde fabriek per proces.
* Ontvangers kunnen synchrone of asynchrone bewerkingen gebruiken. Gezien de frequentie gemiddeld ontvangen van een afzonderlijke ontvanger, client-side batchverwerking van een volledige aanvraag heeft geen invloed op ontvanger doorvoer.
* Toegang tot batch store ingeschakeld laten. Dit vermindert de algehele belasting van de entiteit. Dit verlaagt ook de snelheid waarmee berichten kunnen worden geschreven in de wachtrij of onderwerp.
* Het aantal prefetch ingesteld op een kleine waarde (bijvoorbeeld PrefetchCount = 10). Dit voorkomt dat ontvangers inactiviteit terwijl andere ontvangers grote aantallen berichten in de cache opgeslagen hebben.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Onderwerp met een klein aantal abonnementen
Doel: Maximaliseert de doorvoer van een onderwerp met een klein aantal abonnementen. Een bericht wordt ontvangen door veel abonnementen, wat betekent dat de snelheid van de gecombineerde ontvangen via alle abonnementen is groter dan de frequentie verzenden. Het aantal afzenders is klein. Het aantal ontvangers per abonnement is klein.

Als u wilt maximaliseren van doorvoer, het volgende doen:

* Gebruik een gepartitioneerde onderwerp voor betere prestaties en beschikbaarheid.
* Om te verbeteren de algehele verzendsnelheid in het onderwerp, gebruikt u meerdere messagingfactory afzenders maken. Gebruik voor elke afzender asynchrone bewerkingen of meerdere threads.
* Gebruik meerdere messagingfactory ontvangers maken voor een verhoging van de totale frequentie van ontvangen van een abonnement. Gebruik voor elke ontvanger asynchrone bewerkingen of meerdere threads.
* Asynchrone bewerkingen om te profiteren van de client-side batchverwerking gebruiken.
* Gebruik de standaardwaarde voor het interval van 20ms batchverwerking om Verminder het aantal gegevensoverdrachten van Service Bus-client-protocol.
* Toegang tot batch store ingeschakeld laten. Dit verhoogt de snelheid waarmee berichten kunnen worden geschreven in het onderwerp.
* Het aantal prefetch ingesteld op 20 keer de maximale verwerkingssnelheden van alle ontvangers van een fabriek. Dit vermindert het aantal gegevensoverdrachten van Service Bus-client-protocol.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Onderwerp met een groot aantal abonnementen
Doel: Maximaliseert de doorvoer van een onderwerp met een groot aantal abonnementen. Een bericht wordt ontvangen door veel abonnementen, wat betekent dat de frequentie van de gecombineerde ontvangen via alle abonnementen is veel groter is dan de frequentie verzenden. Het aantal afzenders is klein. Het aantal ontvangers per abonnement is klein.

Onderwerpen met een groot aantal abonnementen wordt doorgaans een lage totale doorvoer blootstellen als er geen alle berichten worden doorgestuurd naar alle abonnementen. Dit wordt veroorzaakt door het feit dat elk bericht wordt vaak ontvangen en alle berichten die zijn opgenomen in een onderwerp en alle bijbehorende abonnementen op dezelfde locatie worden opgeslagen. Ervan wordt uitgegaan dat het aantal afzenders en ontvangers per abonnement aantal klein is. Service Bus ondersteunt maximaal 2.000 abonnementen per onderwerp.

Als u wilt maximaliseren van doorvoer, het volgende doen:

* Gebruik een gepartitioneerde onderwerp voor betere prestaties en beschikbaarheid.
* Asynchrone bewerkingen om te profiteren van de client-side batchverwerking gebruiken.
* Gebruik de standaardwaarde voor het interval van 20ms batchverwerking om Verminder het aantal gegevensoverdrachten van Service Bus-client-protocol.
* Toegang tot batch store ingeschakeld laten. Dit verhoogt de snelheid waarmee berichten kunnen worden geschreven in het onderwerp.
* Het aantal prefetch ingesteld op 20 keer de verwachte ontvangen frequentie in seconden. Dit vermindert het aantal gegevensoverdrachten van Service Bus-client-protocol.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het optimaliseren van de prestaties van Service Bus, [gepartitioneerde berichtentiteiten][Partitioned messaging entities].

[QueueClient]: /dotnet/api/microsoft.azure.servicebus.queueclient
[MessageSender]: /dotnet/api/microsoft.azure.servicebus.core.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[PeekLock]: /dotnet/api/microsoft.azure.servicebus.receivemode
[ReceiveAndDelete]: /dotnet/api/microsoft.azure.servicebus.receivemode
[BatchFlushInterval]: /dotnet/api/microsoft.servicebus.messaging.messagesender.batchflushinterval
[EnableBatchedOperations]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations
[QueueClient.PrefetchCount]: /dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount
[SubscriptionClient.PrefetchCount]: /dotnet/api/microsoft.azure.servicebus.subscriptionclient.prefetchcount
[ForcePersistence]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.forcepersistence
[EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[Partitioned messaging entities]: service-bus-partitioning.md
[TopicDescription.EnableFilteringMessagesBeforePublishing]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing
