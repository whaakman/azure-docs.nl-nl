---
title: Gepartitioneerde Azure Service Bus-wachtrijen en onderwerpen maken | Microsoft Docs
description: Beschrijft hoe u voor het partitioneren van Service Bus-wachtrijen en onderwerpen met behulp van meerdere berichtenbrokers.
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/06/2018
ms.author: spelluru
ms.openlocfilehash: ccdd17bf5a6b3ff00fd3fdada8f4ae02a071274c
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392436"
---
# <a name="partitioned-queues-and-topics"></a>Gepartitioneerde wachtrijen en onderwerpen

Azure Service Bus maakt gebruik van meerdere berichtenbrokers voor het verwerken van berichten en meerdere berichten-stores voor het opslaan van berichten. Een conventionele wachtrij of onderwerp is afgehandeld door een enkel bericht-broker en die zijn opgeslagen in één berichten-store. Service Bus *partities* wachtrijen en onderwerpen, inschakelen of *berichtentiteiten*, moet worden gepartitioneerd voor meerdere berichtenbrokers en berichten-stores. Partitioneren, betekent dat de algehele doorvoer van een gepartitioneerde eenheid niet meer op basis van de prestaties van één bericht-broker of berichtenarchief wordt beperkt. Bovendien weer een tijdelijke onderbreking van een berichten-store niet een gepartitioneerde wachtrij of onderwerp niet beschikbaar. Gepartitioneerde wachtrijen en onderwerpen kunnen bevatten alle geavanceerde functies van de Service Bus, zoals ondersteuning voor transacties en sessies.

> [!NOTE]
> Partitioneren is beschikbaar bij het maken van entiteit voor alle wachtrijen en onderwerpen in Basic of Standard-SKU's. Het is niet beschikbaar voor de Premium SKU messaging, maar eventuele bestaande gepartitioneerde entiteiten in Premium-naamruimten blijven eveneens werken zoals verwacht.
 
Het is niet mogelijk om te wijzigen van de partitionering optie op een bestaande wachtrij of onderwerp; u kunt de optie alleen instellen bij het maken van de entiteit.

## <a name="how-it-works"></a>Hoe werkt het?

Elke gepartitioneerde wachtrij of onderwerp bestaat uit meerdere fragmenten. Elk fragment wordt opgeslagen in een verschillende berichten-store en verwerkt door een ander bericht-broker. Wanneer een bericht wordt verzonden naar een gepartitioneerde wachtrij of onderwerp, wijst Service Bus het bericht toe aan een van de fragmenten. De selectie wordt willekeurig gedaan door Service Bus of met behulp van een partitiesleutel waarmee de afzender kunt opgeven.

Wanneer een client wil ontvangen een bericht van een gepartitioneerde wachtrij of van een abonnement op een gepartitioneerde query's voor Service Bus-onderwerp alle fragmenten voor berichten, retourneert vervolgens het eerste bericht dat wordt opgehaald uit een van de berichten-stores voor de ontvanger. Service Bus-caches de andere berichten en retourneert ze als er aanvullende ontvangen aanvragen. Een ontvangende client is niet op de hoogte van de partities; het gedrag clientgerichte van een gepartitioneerde wachtrij of onderwerp (bijvoorbeeld lezen, is voltooid, uitstellen, onbestelbare berichten, veelgevraagde) moet identiek zijn aan het gedrag van een reguliere-entiteit.

Er is geen extra kosten bij het verzenden van een bericht naar of ontvangen van een bericht van een gepartitioneerde wachtrij of onderwerp.

## <a name="enable-partitioning"></a>Partitionering inschakelen

Voor het gebruik van gepartitioneerde wachtrijen en onderwerpen met Azure Service Bus, gebruikt u de Azure SDK versie 2.2 of hoger, of geef `api-version=2013-10` of hoger in de HTTP-aanvragen.

### <a name="standard"></a>Standard

In de standaardlaag voor berichten, kunt u Service Bus-wachtrijen en onderwerpen in 1, 2, 3, 4 of 5 GB-grootten (de standaardwaarde is 1 GB) kunt maken. Partitionering ingeschakeld, maakt Service Bus 16 exemplaren (16 partities) van de entiteit voor elke GB die u opgeeft. Als zodanig, als u een wachtrij die is 5 GB groot maakt, met 16 partities de maximale wachtrijgrootte wordt (5 \* 16) = 80 GB. U kunt de maximale grootte van de gepartitioneerde wachtrij of onderwerp zien door te kijken op de vermelding ervan op de [Azure-portal][Azure portal], in de **overzicht** blade voor die entiteit.

### <a name="premium"></a>Premium

In een naamruimte van de laag Premium wordt entiteiten partitioneren niet ondersteund. U kunt Service Bus-wachtrijen en onderwerpen nog steeds maken in 1, 2, 3, 4, 5, 10, 20, 40 of 80 GB-grootten (de standaardwaarde is 1 GB). U kunt de grootte van uw wachtrij of onderwerp zien door te kijken op de vermelding ervan op de [Azure-portal][Azure portal], in de **overzicht** blade voor die entiteit.

### <a name="create-a-partitioned-entity"></a>Een gepartitioneerde eenheid maken

Er zijn verschillende manieren om te maken van een gepartitioneerde wachtrij of onderwerp. Wanneer u de wachtrij of onderwerp van uw toepassing maakt, kunt u inschakelen voor de wachtrij of onderwerp partitioneren door respectievelijk de [QueueDescription.EnablePartitioning] [ QueueDescription.EnablePartitioning] of [ TopicDescription.EnablePartitioning] [ TopicDescription.EnablePartitioning] eigenschap **waar**. Deze eigenschappen moeten worden ingesteld op het moment dat de wachtrij of onderwerp is gemaakt, en zijn alleen beschikbaar in de oudere [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) bibliotheek. Zoals eerder gezegd, is het niet mogelijk om te wijzigen van deze eigenschappen op een bestaande wachtrij of onderwerp. Bijvoorbeeld:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

U kunt ook maken een gepartitioneerde wachtrij of onderwerp in de [Azure-portal][Azure portal]. Wanneer u een wachtrij of onderwerp in de portal, maakt de **partitionering inschakelen** optie in de wachtrij of onderwerp **maken** in het dialoogvenster is standaard ingeschakeld. U kunt deze optie in een Standard-laag entity; alleen uitschakelen in de Premium-laag partitioneren wordt niet ondersteund, en het selectievakje heeft geen effect. 

## <a name="use-of-partition-keys"></a>Gebruik van partitiesleutels

Wanneer een bericht in de wachtrij in een gepartitioneerde wachtrij of onderwerp is, controleert de aanwezigheid van een partitiesleutel op Service Bus. Als er een wordt gevonden, selecteert u het fragment op basis van die sleutel. Als een partitiesleutel niet wordt gevonden, selecteert u het fragment op basis van een interne algoritme.

### <a name="using-a-partition-key"></a>Met behulp van een partitiesleutel

Sommige scenario's, zoals sessies of transacties, nodig berichten worden opgeslagen in een specifieke fragment hebt. Alle deze scenario's vereisen het gebruik van een partitiesleutel. Alle berichten die gebruikmaken van dezelfde partitiesleutel worden toegewezen aan de hetzelfde fragment. Als het fragment tijdelijk niet beschikbaar is, betekent dit dat Service Bus een fout geretourneerd.

Eigenschappen van verschillende berichten zijn afhankelijk van het scenario, als een partitiesleutel gebruikt:

**Sessie-id**: als een bericht heeft de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) eigenschap ingesteld, wordt de Service Bus maakt gebruik van **SessionID** als de partitiesleutel. Op deze manier worden alle berichten die deel uitmaken van dezelfde sessie afgehandeld door de dezelfde berichtenbroker. Sessies kunt Service Bus garanderen berichtrangschikking en de consistentie van de sessiestatus.

**PartitionKey**: als een bericht heeft de [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) eigenschap, maar niet de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) eigenschap ingesteld, wordt de Service Bus maakt gebruik van de [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) eigenschap de waarde als de partitiesleutel. Als het bericht zowel heeft de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) en de [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) set eigenschappen, beide eigenschappen moeten identiek zijn. Als de [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) eigenschap is ingesteld op een andere waarde dan de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) eigenschap, Service Bus een ongeldige bewerking-uitzondering geretourneerd. De [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) eigenschap moet worden gebruikt als een afzender verzendt een niet-sessie op de hoogte transactionele berichten. De partitiesleutel zorgt ervoor dat alle berichten die binnen een transactie worden verzonden door de dezelfde berichtbroker worden verwerkt.

**MessageId**: als de wachtrij of onderwerp heeft de [RequiresDuplicateDetection](/dotnet/api/microsoft.azure.management.servicebus.models.sbqueue.requiresduplicatedetection) eigenschap ingesteld op **waar** en de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) of [ PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) eigenschappen zijn niet ingesteld, dan zal de [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) eigenschapswaarde fungeert als de partitiesleutel. (De bibliotheken .NET van Microsoft en AMQP automatisch toewijzen een bericht-ID als de verzendende toepassing niet bestaat.) In dit geval worden alle exemplaren van hetzelfde bericht worden afgehandeld door de dezelfde berichtenbroker. Deze ID kunt Service Bus om te detecteren en verwijderen van dubbele berichten. Als de [RequiresDuplicateDetection](/dotnet/api/microsoft.azure.management.servicebus.models.sbqueue.requiresduplicatedetection) eigenschap niet is ingesteld op **waar**, Service Bus houdt geen rekening met de [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) eigenschap als een partitiesleutel.

### <a name="not-using-a-partition-key"></a>Niet met behulp van een partitiesleutel

In de afwezigheid van een partitiesleutel distribueert Service Bus berichten in een round robin besturingsaanvraag aan alle fragmenten van de gepartitioneerde wachtrij of onderwerp. Als de gekozen fragment niet beschikbaar is, wijst Service Bus het bericht toe aan een andere fragment. Op deze manier wordt de bewerking voor het verzenden slaagt ondanks tijdelijk ontbreken van een berichten-store. U wordt echter niet bereiken het gegarandeerde ordenen waarmee een partitiesleutel.

Zie voor een uitgebreidere bespreking van de verhouding tussen de beschikbaarheid (geen partitiesleutel) en consistentie (met behulp van een partitiesleutel) [in dit artikel](../event-hubs/event-hubs-availability-and-consistency.md). Deze informatie is evenveel van toepassing op gepartitioneerde Service Bus-entiteiten.

Service Bus geven genoeg tijd heeft in de wachtrij plaatsen het bericht in een andere fragment, de [OperationTimeout](/dotnet/api/microsoft.azure.servicebus.queueclient.operationtimeout) waarde die is opgegeven door de client waarmee het bericht moet groter zijn dan 15 seconden worden verzonden. Het verdient aanbeveling in te stellen de [OperationTimeout](/dotnet/api/microsoft.azure.servicebus.queueclient.operationtimeout) eigenschap op de standaardwaarde van 60 seconden.

Een partitiesleutel kledingwinkelketen' "een bericht naar een specifieke fragment. Als het berichtenarchief waarin dit fragment niet beschikbaar is, betekent dit dat Service Bus een fout geretourneerd. In de afwezigheid van een partitiesleutel, Service Bus een verschillende fragment kunt kiezen en de bewerking is geslaagd. Het verdient daarom een partitiesleutel niet te leveren, tenzij dit vereist is.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Geavanceerde onderwerpen: transacties gebruiken met gepartitioneerde entiteiten

Berichten die worden verzonden als onderdeel van een transactie moeten een partitiesleutel opgeven. De sleutel kan bestaan uit een van de volgende eigenschappen: [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid), [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey), of [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid). Alle berichten die worden verzonden als onderdeel van dezelfde transactie moeten dezelfde partitiesleutel opgeven. Als u probeert om een bericht zonder een partitiesleutel binnen een transactie te verzenden, retourneert de Service Bus een uitzondering is een ongeldige bewerking. Als u probeert te verzenden binnen dezelfde transactie meerdere berichten met verschillende partitiesleutels, retourneert de Service Bus een uitzondering is een ongeldige bewerking. Bijvoorbeeld:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    Message msg = new Message("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.SendAsync(msg); 
    ts.CompleteAsync();
}
committableTransaction.Commit();
```

Als een van de eigenschappen die als een partitiesleutel fungeren zijn ingesteld, kledingwinkelketen Service Bus het bericht naar een specifieke fragment. Dit gedrag is echter al dan niet een transactie wordt gebruikt. Het is raadzaam dat u geen een partitiesleutel opgeeft als het is niet nodig.

## <a name="using-sessions-with-partitioned-entities"></a>Sessies gebruiken met gepartitioneerde entiteiten

Transactionele berichten worden verzonden naar een sessie-bewuste onderwerp of een wachtrij, het bericht moet hebben de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) eigenschap is ingesteld. Als de [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) eigenschap ook is opgegeven, moet gelijk zijn aan de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) eigenschap. Als deze verschillen, wordt in Service Bus een ongeldige bewerking-uitzondering geretourneerd.

In tegenstelling tot normale (niet-gepartitioneerde) wachtrijen of onderwerpen is het niet mogelijk met gebruik van een enkele transactie meerdere berichten te verzenden naar verschillende sessies. Als er is geprobeerd, retourneert de Service Bus een uitzondering is een ongeldige bewerking. Bijvoorbeeld:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    Message msg = new Message("This is a message");
    msg.SessionId = "mySession";
    messageSender.SendAsync(msg); 
    ts.CompleteAsync();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Automatische bericht doorsturen met gepartitioneerde entiteiten

Service Bus ondersteunt automatische bericht doorsturen van, op, of tussen gepartitioneerde entiteiten. Instellen om in te schakelen automatisch bericht doorsturen, de [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] eigenschap op de bronwachtrij of abonnement. Als het bericht een partitiesleutel bevat ([SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid), [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey), of [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid)), die partitiesleutel wordt gebruikt voor de doelentiteit.

## <a name="considerations-and-guidelines"></a>Overwegingen en richtlijnen
* **Hoge consistentie functies**: als een entiteit maakt gebruik van functies zoals sessies, detectie van duplicaten of expliciete controle van de partitiesleutel, wordt de berichtbewerkingen altijd doorgestuurd naar specifieke fragmenten. Als een van de fragmenten intensief verkeer of de onderliggende opslag niet in orde is, mislukt deze bewerkingen en beschikbaarheid wordt beperkt. Over het algemeen is de consistentie nog steeds veel hoger dan voor niet-gepartitioneerde entiteiten; alleen een subset van verkeer ondervindt problemen, in plaats van al het verkeer. Zie voor meer informatie dit [bespreking van de beschikbaarheid en consistentie](../event-hubs/event-hubs-availability-and-consistency.md).
* **Beheer**: bewerkingen zoals het maken, bijwerken en verwijderen moeten worden uitgevoerd op alle fragmenten van de entiteit. Als een fragment niet in orde is, kan dit resulteren in fouten voor deze bewerkingen. Voor de Get-bewerking moet gegevens zoals bericht telt worden samengevoegd uit alle fragmenten. Als een fragment niet in orde is, wordt de status van de entiteit-beschikbaarheid gerapporteerd als beperkt.
* **Laag volume bericht scenario's**: voor dergelijke scenario's met name wanneer u het HTTP-protocol, moet u mogelijk uitvoeren van meerdere bewerkingen voor het verkrijgen van alle berichten ontvangen. Ontvangen aanvragen, de front-end wordt uitgevoerd een ontvangen voor alle fragmenten en alle antwoorden ontvangen in de cache opslaat. Een aanvraag van de volgende ontvangen op dezelfde verbinding wilt profiteren van deze cache en ontvang latenties zijn lager. Als u meerdere verbindingen of HTTP gebruiken, maakt die een nieuwe verbinding voor elke aanvraag. Hierdoor is er geen garantie dat deze op hetzelfde knooppunt zou land. Alle bestaande berichten worden vergrendeld als in de cache opgeslagen in een andere front-end, de receive-bewerking retourneert **null**. Berichten uiteindelijk verlopen en kunt u ze opnieuw ontvangen. HTTP-keepalive wordt aanbevolen.
* **Bladeren/Peek berichten**: alleen beschikbaar in de oudere [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) bibliotheek. [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) niet altijd retourneert het aantal berichten dat is opgegeven de [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.messagecount) eigenschap. Er zijn twee gangbare redenen voor dit gedrag. Eén reden is dat de cumulatieve grootte van de verzameling van berichten is groter dan de maximale grootte van 256 KB. Een andere reden is dat als de wachtrij of onderwerp heeft de [EnablePartitioning eigenschap](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning) ingesteld op **waar**, een partitie mogelijk niet voldoende berichten naar het aangevraagde aantal berichten te voltooien. In het algemeen als een toepassing wil ontvangen van een bepaald aantal berichten, deze moet aanroepen [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) herhaaldelijk totdat het aantal berichten opgehaald, of er zijn geen berichten meer om te bekijken. Zie voor meer informatie, codevoorbeelden, waaronder de [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) of [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient.peekbatch) API-documentatie.

## <a name="latest-added-features"></a>Meest recente extra functies

* Toevoegen of verwijderen van regel wordt nu ondersteund met gepartitioneerde entiteiten. Verschillende van niet-gepartitioneerde entiteiten, deze bewerkingen worden niet ondersteund onder transacties. 
* AMQP wordt nu ondersteund voor het verzenden en ontvangen van berichten naar en van een gepartitioneerde eenheid.
* AMQP wordt nu ondersteund voor de volgende bewerkingen: [Batch verzenden](/dotnet/api/microsoft.servicebus.messaging.queueclient.sendbatch), [Batch ontvangen](/dotnet/api/microsoft.servicebus.messaging.queueclient.receivebatch), [ontvangen door volgnummer](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive), [Peek](/dotnet/api/microsoft.servicebus.messaging.queueclient.peek), [ Vergrendeling vernieuwen](/dotnet/api/microsoft.servicebus.messaging.queueclient.renewmessagelock), [bericht plannen](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync), [gepland bericht annuleren](/dotnet/api/microsoft.azure.servicebus.queueclient.cancelscheduledmessageasync), [regel toevoegen](/dotnet/api/microsoft.azure.servicebus.ruledescription), [regel verwijderen](/dotnet/api/microsoft.azure.servicebus.ruledescription), [Sessie vergrendeling vernieuwen](/dotnet/api/microsoft.servicebus.messaging.messagesession.renewlock), [Set sessiestatus](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate), [Get-sessiestatus](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate), en [opsommen sessies](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions).

## <a name="partitioned-entities-limitations"></a>Gepartitioneerde entiteiten beperkingen

Service Bus opgelegd momenteel de volgende beperkingen met betrekking tot gepartitioneerde wachtrijen en onderwerpen:

* Gepartitioneerde wachtrijen en onderwerpen worden niet ondersteund in de laag Premium-messaging.
* Gepartitioneerde wachtrijen en onderwerpen bieden geen ondersteuning voor het verzenden van berichten die deel uitmaken van verschillende sessies in één transactie.
* Service Bus staat momenteel maximaal 100 gepartitioneerde wachtrijen en onderwerpen per naamruimte toe. Elke gepartitioneerde wachtrij of onderwerp telt voor het quotum van 10.000 entiteiten per naamruimte (geldt niet voor Premium-laag).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de basisconcepten van de AMQP 1.0-messaging-specificatie van het in de [AMQP 1.0-protocolhandleiding](service-bus-amqp-protocol-guide.md).

[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablepartitioning
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md
