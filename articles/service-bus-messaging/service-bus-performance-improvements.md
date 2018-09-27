---
title: Aanbevolen procedures voor het verbeteren van de prestaties met behulp van Azure Service Bus | Microsoft Docs
description: Beschrijft hoe u Service Bus gebruiken voor het uitwisselen van verwerkte berichten om prestaties te optimaliseren.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/14/2018
ms.author: spelluru
ms.openlocfilehash: cfce11546249310ce00e5f19ba81520cc9dd78cf
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392632"
---
# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Aanbevolen procedures voor prestatieverbeteringen met Service Bus-berichten

In dit artikel wordt beschreven hoe u Azure Service Bus gebruiken voor het uitwisselen van verwerkte berichten om prestaties te optimaliseren. Het eerste deel van dit artikel beschrijft de verschillende methoden die worden aangeboden aan de prestaties verbeteren. Het tweede gedeelte bevat richtlijnen over het gebruik van Service Bus op een begrijpelijke manier de beste prestaties in een bepaald scenario bieden kunt.

In dit artikel wordt de term 'client' verwijst naar een entiteit die toegang heeft tot de Service Bus. Een client kan duren voordat de rol van een zender of ontvanger. De term 'zender' wordt gebruikt voor een Service Bus-wachtrij of onderwerp client waarmee berichten worden verzonden naar een Service Bus-wachtrij of onderwerp-abonnement. De term "ontvanger" verwijst naar een Service Bus-wachtrij of abonnement clienttoepassing die berichten van een Service Bus-wachtrij of abonnement ontvangt.

Deze secties worden enkele concepten die Service Bus gebruikt om u te helpen de prestaties verbeteren.

## <a name="protocols"></a>Protocollen

Service Bus kunnen clients verzenden en ontvangen van berichten uit via een van drie protocollen:

1. Geavanceerde voor Message Queuing Protocol (AMQP)
2. Servicebus Messaging-Protocol (SBMP)
3. HTTP

AMQP en SBMP zijn efficiënter, omdat ze de verbinding met Service Bus onderhouden, zolang de berichtenfactory bestaat. Ook worden batchverwerking en veelgevraagde geïmplementeerd. Tenzij uitdrukkelijk vermeld, wordt het gebruik van AMQP of SBMP aangenomen dat alle inhoud in dit artikel.

## <a name="reusing-factories-and-clients"></a>Factory's en-clients

Service Bus-clienttoepassing objecten, zoals [QueueClient] [ QueueClient] of [MessageSender][MessageSender], zijn gemaakt via een [ MessagingFactory] [ MessagingFactory] object, dat ook interne beheren van verbindingen biedt. Het is raadzaam dat u niet sluiten messaging fabrieken of wachtrij, onderwerp en abonnement clients nadat u een bericht verzenden en opnieuw maken wanneer u het volgende bericht verzonden. Het sluiten van een berichtenfactory Hiermee verwijdert u de verbinding met de Service Bus-service en een nieuwe verbinding tot stand is gebracht wanneer de factory opnieuw te maken. Een verbinding is een dure bewerking die u voorkomen kunt door de clientobjecten voor meerdere bewerkingen en dezelfde factory opnieuw te gebruiken. U kunt veilig de [QueueClient] [ QueueClient] -object voor het verzenden van berichten van gelijktijdige asynchrone bewerkingen en meerdere threads. 

## <a name="concurrent-operations"></a>Gelijktijdige bewerkingen

Door een bewerking uitgevoerd (verzenden, ontvangen, verwijderen, enz.) het duurt enige tijd. Deze keer bevat de verwerking van de bewerking door de Service Bus-service naast de latentie van de aanvraag en het antwoord. Als u wilt het aantal bewerkingen per keer verhogen, moeten gelijktijdig bewerkingen uitvoeren. 

De client plant gelijktijdige bewerkingen door asynchrone bewerkingen worden uitgevoerd. De volgende aanvraag is gestart voordat de vorige aanvraag is voltooid. Het volgende codefragment wordt een voorbeeld van een bewerking voor het asynchroon verzenden:
  
 ```csharp
  Message m1 = new BrokeredMessage(body);
  Message m2 = new BrokeredMessage(body);
  
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
  
  De volgende code is een voorbeeld van een asynchrone bewerking ontvangen. Zie het volledige programma [hier](https://github.com/Azure/azure-service-bus/blob/master/samples/DotNet/Microsoft.Azure.ServiceBus/SendersReceiversWithQueues):
  
  ```csharp
  var receiver = new MessageReceiver(connectionString, queueName, ReceiveMode.PeekLock);
  var doneReceiving = new TaskCompletionSource<bool>();

  receiver.RegisterMessageHandler(...);
  ```

## <a name="receive-mode"></a>Modus ontvangen

Bij het maken van een wachtrij of abonnement client, kunt u een modus ontvangen: *Peek-lock* of *ontvangt en verwijdert*. De standaardwaarde modus ontvangen is [PeekLock][PeekLock]. Als het wordt uitgevoerd in deze modus, verzendt de client een aanvraag voor het ontvangen van een bericht van Service Bus. Nadat de client heeft het bericht hebt ontvangen, stuurt een aanvraag voor het voltooien van het bericht.

Bij het instellen van de modus ontvangen naar [ReceiveAndDelete][ReceiveAndDelete], beide stappen worden gecombineerd in één aanvraag. Deze stappen beperken het totale aantal bewerkingen en kunnen de verbeteren de algehele doorvoer van berichten. Deze prestatieverbetering wordt geleverd indien het verlies van berichten.

Service Bus biedt geen ondersteuning voor transacties voor ontvangen en delete-bewerkingen. Bovendien peek-lock semantiek zijn vereist voor alle scenario's waarin de client wil om uit te stellen of [dead-letter uitvoeren voor](service-bus-dead-letter-queues.md) een bericht.

## <a name="client-side-batching"></a>Client-side batchverwerking

Client-side batchverwerking, kunt een wachtrij of onderwerp client van het verzenden van een bericht voor een bepaalde periode vertragen. Als de client aanvullende berichten verstuurt tijdens deze periode, worden de berichten in één batch verstuurd. Client-side batchverwerking ook zorgt ervoor dat een wachtrij of abonnement client meerdere batch **voltooid** aanvragen in één aanvraag. Batchverwerking is alleen beschikbaar voor asynchrone **verzenden** en **voltooid** bewerkingen. Synchrone bewerkingen worden onmiddellijk verzonden naar de Service Bus-service. Batchverwerking niet wordt omgeleid voor peek of ontvangen van bewerkingen, noch doet batchverwerking zich in clients.

Een client maakt standaard gebruik van een batch-interval van 20 ms. U kunt de batch-interval wijzigen door in te stellen de [BatchFlushInterval] [ BatchFlushInterval] eigenschap voordat u de berichtenfactory maakt. Deze instelling is van invloed op alle clients die zijn gemaakt door deze factory.

Als wilt uitschakelen batchverwerking, stel de [BatchFlushInterval] [ BatchFlushInterval] eigenschap **TimeSpan.Zero**. Bijvoorbeeld:

```csharp
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Batchverwerking heeft geen invloed op het aantal factureerbare berichten bewerkingen en is alleen beschikbaar voor de Service Bus client protocol met behulp van de [Microsoft.ServiceBus.Messaging](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) bibliotheek. Het HTTP-protocol biedt geen ondersteuning voor batchverwerking.

## <a name="batching-store-access"></a>Toegang tot de store batchverwerking

Service Bus batches meerdere berichten voor een betere de doorvoer van een wachtrij, onderwerp of abonnement wanneer er wordt geschreven naar de interne opslag. Als ingeschakeld op een wachtrij of onderwerp, berichten schrijven naar het archief wordt in batch worden opgenomen. Als ingeschakeld op een wachtrij of abonnement, berichten verwijderen uit de store worden in batch worden opgenomen. Als batch store-toegang is ingeschakeld voor een entiteit, vertraagd Service Bus een schrijfbewerking van store met betrekking tot die entiteit door maximaal 20 ms. 

> [!NOTE]
> Er is geen risico van het verlies van berichten met batches, zelfs als er een Service Bus-fout aan het einde van een interval van 20 MS-batchverwerkingsindeling. 

Aanvullende store-bewerkingen die zich tijdens dit interval voordoen worden toegevoegd aan de batch. Batch verwerkt store toegang alleen van invloed op **verzenden** en **voltooid** operations; ontvangen van bewerkingen worden niet beïnvloed. Toegang tot batch store is een eigenschap van een entiteit. Batchverwerking gebeurt via alle entiteiten die in batch store-toegang inschakelen.

Bij het maken van een nieuwe wachtrij, onderwerp of abonnement, wordt toegang tot batch store is standaard ingeschakeld. Als wilt uitschakelen in batch store-toegang, stel de [EnableBatchedOperations] [ EnableBatchedOperations] eigenschap **false** voordat u de entiteit maakt. Bijvoorbeeld:

```csharp
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Toegang tot batch store heeft geen invloed op het aantal factureerbare berichten bewerkingen en is een eigenschap van een wachtrij, onderwerp of abonnement. Het is onafhankelijk van de modus ontvangen en het protocol dat wordt gebruikt tussen een client en de Service Bus-service.

## <a name="prefetching"></a>Veelgevraagde

[Veelgevraagde](service-bus-prefetch.md) kan de wachtrij of abonnement client extra berichten laden van de service wanneer deze een receive-bewerking wordt uitgevoerd. De client slaat deze berichten in een lokale cache. De grootte van de cache wordt bepaald door de [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] of [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] eigenschappen. Elke client waarmee veelgevraagde onderhoudt een eigen cache. Een cache is niet gedeeld tussen de clients. Als de client een ontvangstbewerking initieert en de cache leeg is, verzendt de service een batch van berichten. De grootte van de batch is gelijk aan de grootte van de cache of 256 KB, afhankelijk van wat kleiner is. Als de client een ontvangstbewerking initieert en de cache een bericht bevat, wordt het bericht afkomstig uit de cache.

Wanneer een bericht is tabelruimte, wordt het prefetched bericht vergrendeld in de service. Met het lock kan niet het prefetched bericht worden ontvangen door een andere ontvanger. Als de ontvanger het bericht niet voltooien kan voordat de vergrendeling is verlopen, wordt het bericht beschikbaar voor andere ontvangers. De prefetched kopie van het bericht blijft in de cache. De ontvanger die de kopie van de verlopen cache wordt een uitzondering ontvangen als er wordt geprobeerd om uit te voeren dat bericht. Standaard wordt de bericht-vergrendeling verloopt na 60 seconden. Deze waarde kan worden uitgebreid tot 5 minuten. Om te voorkomen dat het verbruik van verlopen berichten, moet grootte van de cache altijd kleiner zijn dan het aantal berichten die kunnen worden gebruikt door een client binnen de time-outinterval vergrendelen.

Wanneer u de vervaldatum van de vergrendeling standaardwaarde van 60 seconden, een goede waarde voor de [PrefetchCount] [ SubscriptionClient.PrefetchCount] is 20 keer het maximum-verwerkingssnelheden van alle ontvangers van de factory. Bijvoorbeeld, een factory maakt drie ontvangers en elke ontvanger kan maximaal 10 berichten per seconde verwerken. Het aantal prefetch mag niet meer dan 20 X 3 X 10 = 600. Standaard [PrefetchCount] [ QueueClient.PrefetchCount] is ingesteld op 0, wat betekent dat er geen berichten meer van de service worden opgehaald.

De totale doorvoersnelheid voor een wachtrij of abonnement veelgevraagde berichten worden verhoogd omdat het totale aantal berichtbewerkingen of retouren beperkt. Ophalen van het eerste bericht, maar duurt het langer (als gevolg van de grootte van het toegenomen bericht). Ontvangen van berichten tabelruimte worden sneller omdat deze berichten door de client al zijn gedownload.

De time-to-live (TTL) eigenschap van een bericht wordt gecontroleerd door de server op het moment dat de server wordt het bericht naar de client verzonden. De client controleert niet de berichteigenschap TTL-waarde wanneer het bericht wordt ontvangen. In plaats daarvan kan het bericht worden ontvangen, zelfs als de TTL van het bericht is verlopen terwijl het bericht in de cache door de client opgeslagen is.

Veelgevraagde heeft geen invloed op het aantal factureerbare berichten bewerkingen en is alleen beschikbaar voor de Service Bus-client-protocol. Het HTTP-protocol biedt geen ondersteuning voor veelgevraagde. Veelgevraagde is beschikbaar voor zowel asynchrone als synchrone bewerkingen ontvangen.

## <a name="express-queues-and-topics"></a>Express-wachtrijen en onderwerpen

Express-entiteiten hoge doorvoer en lagere latentie scenario's inschakelt en worden alleen ondersteund in de standaardlaag voor berichten. Entiteiten die zijn gemaakt in [Premium-naamruimten](service-bus-premium-messaging.md) bieden geen ondersteuning voor de snelle optie. Met express-entiteiten, als er een bericht wordt verzonden naar een wachtrij of onderwerp, is het bericht niet onmiddellijk opgeslagen in de berichten-store. In plaats daarvan wordt het tijdelijk opgeslagen in het geheugen. Als een bericht in de wachtrij voor meer dan een paar seconden blijft, wordt deze automatisch geschreven naar een stabiele, opslag, dus beschermd tegen verlies vanwege een storing. Schrijven van het bericht in een geheugencache verhoogt de doorvoer en vermindert de latentie omdat er geen toegang tot opslag stabiele op het moment dat het bericht is verzonden. Berichten die worden verbruikt binnen een paar seconden worden niet naar het berichtenarchief geschreven. Het volgende voorbeeld wordt een snelle onderwerp gemaakt.

```csharp
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Als een bericht weergegeven met essentiële informatie die niet verbroken worden moet wordt verzonden naar een snelle entiteit, Service Bus het bericht naar een stabiele opslag door in te stellen direct persistent kunt afdwingen dat de afzender het [ForcePersistence] [ ForcePersistence] eigenschap **waar**.

> [!NOTE]
> Express-entiteiten bieden geen ondersteuning voor transacties.

## <a name="partitioned-queues-or-topics"></a>Gepartitioneerde wachtrijen en onderwerpen

Intern, Service Bus maakt gebruik van hetzelfde knooppunt en berichten opslaan om te verwerken en opslaan van alle berichten voor een Berichtentiteit (wachtrij of onderwerp). Een [gepartitioneerde wachtrij of onderwerp](service-bus-partitioning.md), aan de andere kant is verdeeld over meerdere knooppunten en berichten-stores. Gepartitioneerde wachtrijen en onderwerpen niet alleen een hogere doorvoer dan reguliere wachtrijen en onderwerpen opleveren, ze vertonen superieure beschikbaarheid. Voor het maken van een gepartitioneerde eenheid, stel de [EnablePartitioning] [ EnablePartitioning] eigenschap **waar**, zoals wordt weergegeven in het volgende voorbeeld. Zie voor meer informatie over gepartitioneerde entiteiten [gepartitioneerde berichtentiteiten][Partitioned messaging entities].

> [!NOTE]
> Gepartitioneerde entiteiten worden niet ondersteund in de [Premium-SKU](service-bus-premium-messaging.md). 

```csharp
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="multiple-queues"></a>Meerdere wachtrijen

Als het is niet mogelijk om te gebruiken een gepartitioneerde wachtrij of onderwerp of de verwachte belasting kan niet worden verwerkt door een enkele gepartitioneerde wachtrij of onderwerp, moet u meerdere messaging-entiteiten. Wanneer u meerdere entiteiten, maakt u een specifieke client voor elke entiteit, in plaats van dezelfde client voor alle entiteiten.

## <a name="development-and-testing-features"></a>Ontwikkeling en testen functies

Service Bus is een functie, die wordt gebruikt voor ontwikkeling, die **moeten nooit worden gebruikt in productie configuraties**: [TopicDescription.EnableFilteringMessagesBeforePublishing][].

Wanneer nieuwe regels of filters worden toegevoegd aan het onderwerp, kunt u [TopicDescription.EnableFilteringMessagesBeforePublishing][] om te controleren of de nieuwe filterexpressie werkt zoals verwacht.

## <a name="scenarios"></a>Scenario's

De volgende secties beschrijven typische berichtverzending en beschrijven de gewenste instellingen voor Service Bus. Doorvoersnelheden (minder dan 1 bericht/seconde) zo klein zijn geclassificeerd, Gemiddeld (1 bericht per seconde of hoger, maar minder dan 100 berichten/seconde) en een hoge (100 berichten/seconde of hoger). Het aantal clients zijn geclassificeerd als kleine (5 of minder), Gemiddeld (meer dan 5, maar kleiner dan of gelijk aan 20), en groot (meer dan 20).

### <a name="high-throughput-queue"></a>Hoge doorvoer wachtrij

Doel: Maximaliseert de doorvoer van één wachtrij. Het aantal afzenders en ontvangers is klein.

* Als u wilt verhogen de verzendsnelheid in de wachtrij wordt geplaatst, gebruikt u meerdere messagingfactory afzenders maken. Voor iedere verzender asynchrone bewerkingen of meerdere threads te gebruiken.
* Om te vergroten de totale snelheid van de ontvangen uit de wachtrij, gebruikt u meerdere messagingfactory ontvangers te maken.
* Asynchrone bewerkingen om te profiteren van de client-side batchverwerking te gebruiken.
* De batchverwerkingsindeling interval ingesteld op 50 ms te verminderen van het aantal verzendingen van Service Bus-client-protocol. Als meerdere afzenders worden gebruikt, vergroot u de batchverwerkingsindeling interval tot 100 ms.
* Laat in batch store-toegang is ingeschakeld. Dit verhoogt de algemene frequentie waarmee berichten kunnen worden geschreven in de wachtrij wordt geplaatst.
* Het aantal prefetch ingesteld op 20 keer de maximale verwerkingssnelheden van alle ontvangers van een fabriek. Dit aantal vermindert het aantal verzendingen van Service Bus-client-protocol.
* Een gepartitioneerde wachtrij gebruiken voor verbeterde prestaties en beschikbaarheid.

### <a name="multiple-high-throughput-queues"></a>Meerdere wachtrijen met hoge doorvoer

Doel: Algehele doorvoer van meerdere wachtrijen te maximaliseren. De doorvoer van een afzonderlijke wachtrij is Gemiddeld of hoog.

Gebruik de instellingen die worden beschreven de doorvoer van één wachtrij kunt maximaliseren om maximale doorvoer via meerdere wachtrijen. Bovendien kunt u verschillende factory's maken clients die verzenden of ontvangen van verschillende wachtrijen.

### <a name="low-latency-queue"></a>Wachtrij met lage latentie

Doel: Minimaliseren end-to-end latentie van een wachtrij of onderwerp. Het aantal afzenders en ontvangers is klein. De doorvoer van de wachtrij is klein of gemiddelde.

* Client-side batchverwerking uitschakelen De client onmiddellijk een bericht.
* Toegang tot batch store uitschakelen. Het bericht schrijft de service onmiddellijk naar de store.
* Als één client, stelt u het aantal prefetch op 20 keer de verwerkingssnelheid van de ontvanger. Als meerdere berichten in de wachtrij op hetzelfde moment binnenkomen, verzendt de Service Bus-clientprotocol ze allemaal op hetzelfde moment. Wanneer de client het volgende bericht ontvangt, wordt dat bericht al in de lokale cache. De cache moet zo klein zijn.
* Als u meerdere clients, moet u het aantal prefetch ingesteld op 0. De tweede client kan het tweede bericht ontvangen tijdens de eerste client die het eerste bericht nog steeds worden verwerkt door het instellen van het aantal.
* Een gepartitioneerde wachtrij gebruiken voor verbeterde prestaties en beschikbaarheid.

### <a name="queue-with-a-large-number-of-senders"></a>In de wachtrij met een groot aantal afzenders

Doel: Maximale doorvoer van een wachtrij of onderwerp met een groot aantal afzenders te maken. Elke afzender verzendt berichten met een gemiddeld tarief. Het aantal ontvangers is klein.

Service Bus kunnen maximaal 1000 gelijktijdige verbindingen met een Berichtentiteit (of 5000 met behulp van AMQP). Deze limiet wordt afgedwongen op het niveau van de naamruimte en wachtrijen/onderwerpen/abonnementen worden beperkt door de limiet van gelijktijdige verbindingen per naamruimte. Voor wachtrijen, wordt dit getal gedeeld tussen afzenders en ontvangers. Als alle 1000 verbindingen vereist voor afzenders zijn, vervangt u de wachtrij met een onderwerp en één abonnement. Een onderwerp accepteert maximaal 1000 gelijktijdige verbindingen van afzenders, terwijl het abonnement een extra 1000 gelijktijdige verbindingen van ontvangers aanvaardt. Als meer dan 1000 gelijktijdige afzenders vereist zijn, moeten de afzenders van berichten verzenden naar het Service Bus-protocol via HTTP.

Voor maximale doorvoer, moet u de volgende stappen uitvoeren:

* Als de afzender bevindt zich in een ander proces, gebruikt u alleen een enkel factory per proces.
* Asynchrone bewerkingen om te profiteren van de client-side batchverwerking te gebruiken.
* Gebruik de standaardwaarde batchverwerking interval van 20 ms te verminderen van het aantal verzendingen van Service Bus-client-protocol.
* Laat in batch store-toegang is ingeschakeld. Dit verhoogt de algemene frequentie waarmee berichten kunnen worden geschreven in de wachtrij of onderwerp.
* Het aantal prefetch ingesteld op 20 keer de maximale verwerkingssnelheden van alle ontvangers van een fabriek. Dit aantal vermindert het aantal verzendingen van Service Bus-client-protocol.
* Een gepartitioneerde wachtrij gebruiken voor verbeterde prestaties en beschikbaarheid.

### <a name="queue-with-a-large-number-of-receivers"></a>In de wachtrij met een groot aantal ontvangers

Doel: Maximaliseert de snelheid van het ontvangen van een wachtrij of abonnement met een groot aantal ontvangers. Elke ontvanger ontvangt berichten met een gemiddelde snelheid. Het aantal afzenders is klein.

Service Bus kunnen maximaal 1000 gelijktijdige verbindingen met een entiteit. Als een wachtrij meer dan 1000 ontvangers vereist, vervangt u de wachtrij met een onderwerp en meerdere abonnementen. Elk abonnement kan maximaal 1000 gelijktijdige verbindingen ondersteunen. Ontvangers hebben ook toegang tot de wachtrij via het HTTP-protocol.

Voor maximale doorvoer, het volgende doen:

* Als elke ontvanger zich in een ander proces bevindt, gebruikt u alleen een enkel factory per proces.
* Ontvangers kunnen synchrone of asynchrone bewerkingen gebruiken. Gezien de frequentie matige ontvangen van een afzonderlijke ontvanger, client-side batchverwerking van een volledige aanvraag heeft geen invloed op ontvanger doorvoer.
* Laat in batch store-toegang is ingeschakeld. Dit vermindert de algehele belasting van de entiteit. Het vermindert ook de algemene frequentie waarmee berichten kunnen worden geschreven in de wachtrij of onderwerp.
* Het aantal prefetch ingesteld op een kleine waarde (bijvoorbeeld PrefetchCount = 10). Dit aantal wordt voorkomen dat ontvangers inactiviteit terwijl andere ontvangers grote aantallen berichten in de cache opgeslagen hebben.
* Een gepartitioneerde wachtrij gebruiken voor verbeterde prestaties en beschikbaarheid.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Onderwerp met een klein aantal abonnementen

Doel: Maximaliseert de doorvoer van een onderwerp met een klein aantal abonnementen. Een bericht wordt ontvangen door veel abonnementen, wat betekent dat de frequentie gecombineerde ontvangen voor alle abonnementen is groter dan de frequentie verzenden. Het aantal afzenders is klein. Het aantal ontvangers per abonnement is klein.

Voor maximale doorvoer, het volgende doen:

* Als u wilt verhogen de verzendsnelheid in het onderwerp, gebruikt u meerdere messagingfactory afzenders maken. Voor iedere verzender asynchrone bewerkingen of meerdere threads te gebruiken.
* Om te verhogen de snelheid van de ontvangen van een abonnement, gebruikt u meerdere messagingfactory ontvangers te maken. Voor elke ontvanger asynchrone bewerkingen of meerdere threads te gebruiken.
* Asynchrone bewerkingen om te profiteren van de client-side batchverwerking te gebruiken.
* Gebruik de standaardwaarde batchverwerking interval van 20 ms te verminderen van het aantal verzendingen van Service Bus-client-protocol.
* Laat in batch store-toegang is ingeschakeld. Dit verhoogt de algemene frequentie waarmee berichten kunnen worden geschreven in het onderwerp.
* Het aantal prefetch ingesteld op 20 keer de maximale verwerkingssnelheden van alle ontvangers van een fabriek. Dit aantal vermindert het aantal verzendingen van Service Bus-client-protocol.
* Een gepartitioneerde onderwerp gebruiken voor verbeterde prestaties en beschikbaarheid.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Onderwerp met een groot aantal abonnementen

Doel: Maximaliseert de doorvoer van een onderwerp met een groot aantal abonnementen. Een bericht wordt ontvangen door veel abonnementen, wat betekent dat de frequentie gecombineerde ontvangen voor alle abonnementen is veel groter is dan de frequentie verzenden. Het aantal afzenders is klein. Het aantal ontvangers per abonnement is klein.

Onderwerpen met een groot aantal abonnementen stellen een lage totale doorvoer doorgaans beschikbaar als alle berichten voor alle abonnementen worden gerouteerd. Deze lage doorvoer wordt veroorzaakt door het feit dat elk bericht wordt vaak ontvangen en alle berichten die zijn opgenomen in een onderwerp en de abonnementen worden opgeslagen in dezelfde locatie. Ervan wordt uitgegaan dat het aantal afzenders en ontvangers per abonnement aantal klein is. Service Bus ondersteunt maximaal 2.000 abonnementen per onderwerp.

Voor maximale doorvoer, probeert u de volgende stappen uit:

* Asynchrone bewerkingen om te profiteren van de client-side batchverwerking te gebruiken.
* Gebruik de standaardwaarde batchverwerking interval van 20 ms te verminderen van het aantal verzendingen van Service Bus-client-protocol.
* Laat in batch store-toegang is ingeschakeld. Dit verhoogt de algemene frequentie waarmee berichten kunnen worden geschreven in het onderwerp.
* Het aantal prefetch ingesteld op 20 keer de verwachte ontvangen frequentie in seconden. Dit aantal vermindert het aantal verzendingen van Service Bus-client-protocol.
* Een gepartitioneerde onderwerp gebruiken voor verbeterde prestaties en beschikbaarheid.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het optimaliseren van prestaties van Service Bus, [gepartitioneerde berichtentiteiten][Partitioned messaging entities].

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
