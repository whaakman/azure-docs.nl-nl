---
title: Gepartitioneerde Azure Service Bus-wachtrijen en onderwerpen maken | Microsoft Docs
description: Beschrijft hoe Service Bus-wachtrijen en onderwerpen met behulp van meerdere bericht beleggingsmakelaars partitie.
services: service-bus-messaging
author: sethmanheim
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 06/06/2018
ms.author: sethm
ms.openlocfilehash: f9fb5f53496ea3f98a9d3341e77db283a3e3b570
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34824367"
---
# <a name="partitioned-queues-and-topics"></a>Gepartitioneerde wachtrijen en onderwerpen

Azure Service Bus maakt gebruik van meerdere bericht beleggingsmakelaars om berichten te verwerken en meerdere berichten-stores voor het opslaan van berichten. Een conventionele wachtrij of onderwerp is verwerkt door een enkel bericht broker en opgeslagen in één berichten-store. Service Bus *partities* wachtrijen en onderwerpen, inschakelen of *berichtentiteiten*, moet worden gepartitioneerd op meerdere bericht beleggingsmakelaars en berichten-stores. Partitioneren betekent dat de totale doorvoer van een gepartitioneerde entiteit wordt niet langer beperkt door de prestaties van een enkel bericht broker of berichten-store. Bovendien weer een tijdelijke onderbreking van berichten-store niet een gepartitioneerde wachtrij of onderwerp niet beschikbaar. Gepartitioneerde wachtrijen en onderwerpen kunnen bevatten alle uitgebreide Service Bus-functies, zoals ondersteuning voor transacties en sessies.

> [!NOTE]
> Partitioneren is beschikbaar bij het maken van de entiteit voor alle wachtrijen en onderwerpen in de basis of standaard SKU's. Het is niet beschikbaar voor de bericht-SKU Premium, maar eventuele bestaande gepartitioneerde entiteiten in Premium-naamruimten blijven werken zoals verwacht.
 
Het is niet mogelijk om te wijzigen van de partitionering optie op een bestaande wachtrij of onderwerp; u kunt de optie alleen instellen bij het maken van de entiteit.

## <a name="how-it-works"></a>Hoe werkt het?

Elke gepartitioneerde wachtrij of onderwerp bestaat uit meerdere fragmenten. Elke fragment is opgeslagen in een andere berichten-store en verwerkt door een ander bericht broker. Wanneer een bericht wordt verzonden naar een gepartitioneerde wachtrij of onderwerp, wijst Service Bus het bericht toe aan een van de fragmenten. De selectie wordt willekeurig gedaan door Service Bus- of met behulp van een partitiesleutel die de afzender kunt opgeven.

Wanneer een client wil ontvangen een bericht van een gepartitioneerde wachtrij of van een abonnement op een gepartitioneerde query's voor Service Bus-onderwerp alle fragmenten voor berichten, retourneert vervolgens het eerste bericht dat is verkregen van een van de berichten-stores bij de ontvanger. Service Bus-caches de andere berichten en retourneert ze als deze ontvangt aanvullende ontvangen aanvragen. Een ontvangende client is niet op de hoogte van de partitionering; het gedrag clientgerichte van een gepartitioneerde wachtrij of onderwerp (bijvoorbeeld, lezen, voltooid, uitstelt, wachtrij voor onbestelbare, veelgevraagde) is vrijwel identiek aan het gedrag van een reguliere entiteit.

Er is geen extra kosten bij het verzenden van een bericht of het bericht in een gepartitioneerde wachtrij of onderwerp.

## <a name="enable-partitioning"></a>Partitionering inschakelen

De Azure SDK versie 2.2 of hoger gebruiken voor het gebruik van gepartitioneerde wachtrijen en onderwerpen met Azure Service Bus, of geef `api-version=2013-10` of hoger in de HTTP-aanvragen.

### <a name="standard"></a>Standard

In de Standard messaging laag, kunt u Service Bus-wachtrijen en onderwerpen in 1, 2, 3, 4 of 5 GB-grootten (de standaardwaarde is 1 GB). Service Bus maakt met het partitionering ingeschakeld, 16 kopieën (16 partities) van de entiteit voor elke GB die u opgeeft. Als zodanig als u een wachtrij die is 5 GB groot maken, met 16 partities de maximale wachtrijgrootte wordt (5 \* 16) = 80 GB. U kunt de maximale grootte van uw gepartitioneerde wachtrij of onderwerp zien door te kijken op de vermelding ervan op de [Azure-portal][Azure portal], in de **overzicht** blade voor die entiteit.

### <a name="premium"></a>Premium

In een naamruimte van de laag Premium wordt partitioneren entiteiten niet ondersteund. U kunt Service Bus-wachtrijen en onderwerpen nog steeds maken in 1, 2, 3, 4, 5, 10, 20, 40 of 80 GB-grootten (de standaardwaarde is 1 GB). U kunt de grootte van uw wachtrij of onderwerp zien door te kijken op de vermelding ervan op de [Azure-portal][Azure portal], in de **overzicht** blade voor die entiteit.

### <a name="create-a-partitioned-entity"></a>Maak een gepartitioneerde entiteit

Er zijn verschillende manieren voor het maken van een gepartitioneerde wachtrij of onderwerp. Wanneer u de wachtrij of onderwerp van uw toepassing maakt, kunt u inschakelen voor de wachtrij of onderwerp partitioneren door respectievelijk de [QueueDescription.EnablePartitioning] [ QueueDescription.EnablePartitioning] of [ TopicDescription.EnablePartitioning] [ TopicDescription.EnablePartitioning] eigenschap **true**. Deze eigenschappen moeten worden ingesteld op het moment dat de wachtrij of onderwerp is gemaakt en zijn alleen beschikbaar in de oudere [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) bibliotheek. Zoals eerder gezegd, is het niet mogelijk om te wijzigen van deze eigenschappen op een bestaande wachtrij of onderwerp. Bijvoorbeeld:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

U kunt ook maken een gepartitioneerde wachtrij of onderwerp in de [Azure-portal][Azure portal]. Wanneer u een wachtrij of onderwerp in de portal maakt de **inschakelen partitioneren** optie in de wachtrij of onderwerp **maken** in het dialoogvenster is standaard ingeschakeld. U kunt deze optie in een entiteit standaardcategorie; alleen uitschakelen in de laag Premium partitioneren wordt niet ondersteund en het selectievakje heeft geen effect. 

## <a name="use-of-partition-keys"></a>Gebruik van partitiesleutels

Wanneer een bericht in de wachtrij in een gepartitioneerde wachtrij of onderwerp is, controleert de aanwezigheid van een partitiesleutel op Service Bus. Als er een is gevonden, selecteert u het fragment op basis van die sleutel. Als een partitiesleutel niet wordt gevonden, wordt het fragment op basis van een interne algoritme geselecteerd.

### <a name="using-a-partition-key"></a>Met behulp van een partitiesleutel

Sommige scenario's, zoals sessies of transacties, vereisen berichten worden opgeslagen in een specifieke fragment. Deze scenario's vereist het gebruik van een partitiesleutel. Alle berichten die gebruikmaken van dezelfde partitiesleutel zijn toegewezen aan de dezelfde fragment. Als het fragment tijdelijk niet beschikbaar is, Service Bus een fout geretourneerd.

Afhankelijk van het scenario worden verschillende berichteigenschappen als een partitiesleutel gebruikt:

**Sessie-id**: als een bericht heeft de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) eigenschap instelt, wordt de Service Bus maakt gebruik van **SessionID** als de partitiesleutel. Op deze manier worden alle berichten die deel uitmaken van dezelfde sessie verwerkt door de dezelfde bericht broker. Sessies inschakelen Service Bus garanderen bericht ordening en consistentie van de sessiestatus.

**PartitionKey**: als een bericht heeft de [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) eigenschap maar niet de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) eigenschap instelt, wordt de Service Bus maakt gebruik van de [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) eigenschap de waarde als de partitiesleutel. Als het bericht zowel bevat de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) en de [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) set eigenschappen, beide eigenschappen moeten identiek zijn. Als de [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) eigenschap is ingesteld op een andere waarde dan de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) eigenschap, Service Bus een ongeldige bewerking uitzondering geretourneerd. De [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) eigenschap moet worden gebruikt als een afzender verzendt een niet-sessie op de hoogte transactionele berichten. De partitiesleutel zorgt ervoor dat alle berichten die zijn verzonden binnen een transactie worden verwerkt door de dezelfde messaging broker.

**MessageId**: als de wachtrij of onderwerp heeft de [RequiresDuplicateDetection](/dotnet/api/microsoft.azure.management.servicebus.models.sbqueue.requiresduplicatedetection) eigenschap ingesteld op **true** en de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) of [ PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) eigenschappen zijn niet ingesteld, wordt de [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) eigenschapswaarde fungeert als de partitiesleutel. (De Microsoft .NET- en AMQP-bibliotheken wijst automatisch een bericht-ID als de betreffende toepassing niet bestaat.) In dit geval worden alle kopieën van hetzelfde bericht verwerkt door de dezelfde bericht broker. Deze ID kunt Service Bus om te detecteren en verwijderen van dubbele berichten. Als de [RequiresDuplicateDetection](/dotnet/api/microsoft.azure.management.servicebus.models.sbqueue.requiresduplicatedetection) eigenschap niet is ingesteld op **true**, Service Bus houdt geen rekening met de [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) eigenschap als partitiesleutel.

### <a name="not-using-a-partition-key"></a>Niet met behulp van een partitiesleutel

In het ontbreken van een partitiesleutel distribueert Service Bus berichten round-robin toewijst op alle fragmenten van gepartitioneerde wachtrij of onderwerp. Als het gekozen fragment niet beschikbaar is, wijst Service Bus het bericht toe aan een andere fragment. Op deze manier de verzendbewerking slaagt ondanks berichten-store, tijdelijk niet beschikbaar. U wordt echter niet bereiken de gegarandeerde ordening waarmee een partitiesleutel.

Zie voor een diepgaandere bespreking van de verhouding tussen de beschikbaarheid (geen partitiesleutel) en consistentie (met behulp van een partitiesleutel) [in dit artikel](../event-hubs/event-hubs-availability-and-consistency.md). Deze informatie is evenveel van toepassing op gepartitioneerde Service Bus-entiteiten.

Service Bus geven genoeg tijd heeft in de wachtrij plaatsen het bericht in een andere fragment de [OperationTimeout](/dotnet/api/microsoft.azure.servicebus.queueclient.operationtimeout) waarde die is opgegeven door de client die u verzendt het bericht moet groter zijn dan 15 seconden. Het verdient aanbeveling in te stellen de [OperationTimeout](/dotnet/api/microsoft.azure.servicebus.queueclient.operationtimeout) eigenschap op de standaardwaarde van 60 seconden.

Een partitiesleutel pincodes' "een bericht naar een specifieke fragment. Als de berichten-store die dit fragment bevat niet beschikbaar is, betekent dit dat Service Bus een fout geretourneerd. In het ontbreken van een partitiesleutel, Service Bus geen andere fragment kunt kiezen en de bewerking is geslaagd. Daarom wordt aanbevolen dat u niet een partitiesleutel opgeeft tenzij deze vereist is.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Onderwerpen over geavanceerde: het gebruik van transacties met gepartitioneerde entiteiten

Berichten die worden verzonden als onderdeel van een transactie moeten een partitiesleutel opgeven. De sleutel kan bestaan uit een van de volgende eigenschappen: [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid), [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey), of [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid). Alle berichten die worden verzonden als onderdeel van dezelfde transactie moeten dezelfde partitiesleutel opgeven. Als u probeert een bericht zonder een partitiesleutel binnen een transactie te verzenden, wordt met Service Bus een ongeldige bewerking-uitzondering geretourneerd. Als u probeert te verzenden binnen dezelfde transactie meerdere berichten met verschillende partitiesleutels, betekent dit dat Service Bus een ongeldige bewerking-uitzondering geretourneerd. Bijvoorbeeld:

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

Als een van de eigenschappen die als een partitiesleutel fungeren is ingesteld, pincodes Service Bus het bericht naar een specifieke fragment. Dit probleem treedt al dan niet een transactie wordt gebruikt. Het verdient aanbeveling dat u geen een partitiesleutel opgeeft als het is niet nodig.

## <a name="using-sessions-with-partitioned-entities"></a>Met behulp van sessies met gepartitioneerde entiteiten

Transactionele berichten worden verzonden naar een sessie-bewuste onderwerp of een wachtrij, het bericht moet hebben de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) eigenschap is ingesteld. Als de [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) eigenschap ook is opgegeven, moet gelijk aan de [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) eigenschap. Als deze verschillen, wordt met Service Bus een ongeldige bewerking-uitzondering geretourneerd.

In tegenstelling tot gewone (niet-gepartitioneerde) wachtrijen en onderwerpen is het niet mogelijk meerdere berichten verzenden naar verschillende sessies met een enkele transactie. Als er is geprobeerd, wordt met Service Bus een ongeldige bewerking-uitzondering geretourneerd. Bijvoorbeeld:

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

Service Bus ondersteunt automatische bericht doorsturen van aan of tussen gepartitioneerde entiteiten. Instellen om automatische bericht doorsturen, de [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] eigenschap in de wachtrij of abonnement. Als een partitiesleutel is opgegeven voor het bericht ([SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid), [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey), of [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid)), die de partitiesleutel wordt gebruikt voor de doelentiteit.

## <a name="considerations-and-guidelines"></a>Overwegingen en richtlijnen
* **Hoge consistentie functies**: als een entiteit functies zoals sessies, detectie van duplicaten of expliciete controle van de partitiesleutel gebruikt, wordt de messaging bewerkingen altijd worden omgeleid naar specifieke fragmenten. Als een van de fragmenten intensief verkeer ervaren of het onderliggende archief niet in orde is, wordt deze bewerkingen mislukken en beschikbaarheid wordt beperkt. Over het algemeen is de consistentiecontrole nog steeds veel hoger zijn dan niet-gepartitioneerde entiteiten; alleen een subset van verkeer ondervindt problemen, in plaats van alle verkeer. Zie voor meer informatie dit [bespreking van de beschikbaarheid en consistentie](../event-hubs/event-hubs-availability-and-consistency.md).
* **Beheer**: bewerkingen zoals het maken, bijwerken en verwijderen moeten worden uitgevoerd op de fragmenten van de entiteit. Als een fragment niet in orde is, kan dit leiden tot fouten voor deze bewerkingen. Voor de Get-bewerking moet informatie zoals bericht telt worden samengevoegd uit alle fragmenten. Als een fragment niet in orde is, wordt de beschikbaarheidsstatus van de entiteit gerapporteerd als beperkt.
* **Laag volume bericht scenario's**: voor dergelijke scenario's, vooral wanneer het HTTP-protocol, moet u mogelijk meerdere uitvoeren ontvangstbewerkingen ter verkrijging van alle berichten. Voor ontvangstaanvragen, het front-end voert een receive op alle fragmenten en plaatst alle antwoorden die worden ontvangen. Een aanvraag van de volgende ontvangen op de verbinding wilt profiteren van deze opslaan in cache en ontvangen latenties wordt niet lager zijn. Als u meerdere verbindingen of HTTP gebruiken, stelt die een nieuwe verbinding voor elke aanvraag. Hierdoor is er geen garantie dat deze zou land op hetzelfde knooppunt. Als alle bestaande berichten die worden vergrendeld en in de cache opgeslagen in een andere front-end, de ontvangstbewerking retourneert **null**. Berichten uiteindelijk verlopen en u ze opnieuw kunt ontvangen. HTTP-keepalive wordt aanbevolen.
* **Bladeren/Peek berichten**: alleen beschikbaar in de oudere [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) bibliotheek. [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) altijd retourneert geen het aantal berichten dat is opgegeven in de [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.messagecount) eigenschap. Er zijn twee veelvoorkomende redenen voor dit probleem. Een reden hiervoor is dat de geaggregeerde grootte van de verzameling van berichten is groter dan de maximale grootte van 256 KB. Een andere reden is dat als de wachtrij of onderwerp heeft de [EnablePartitioning eigenschap](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning) ingesteld op **true**, een partitie geen voldoende berichten naar het aangevraagde aantal berichten te voltooien. In het algemeen als een toepassing wil ontvangen van een bepaald aantal berichten, deze moet aanroepen [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) herhaaldelijk totdat het aantal berichten ophalen of er zijn geen berichten meer om te kijken. Zie voor meer informatie, codevoorbeelden, waaronder de [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) of [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient.peekbatch) API-documentatie.

## <a name="latest-added-features"></a>Meest recente extra functies

* Toevoegen of verwijderen regel wordt nu ondersteund met gepartitioneerde entiteiten. Verschillend zijn van niet-gepartitioneerde entiteiten, deze bewerkingen worden niet ondersteund onder transacties. 
* AMQP wordt nu ondersteund voor het verzenden en ontvangen van berichten naar en van een gepartitioneerde entiteit.
* AMQP wordt nu ondersteund voor de volgende bewerkingen: [Batch verzenden](/dotnet/api/microsoft.servicebus.messaging.queueclient.sendbatch), [Batch ontvangen](/dotnet/api/microsoft.servicebus.messaging.queueclient.receivebatch), [ontvangen door volgnummer](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive), [inspecteren](/dotnet/api/microsoft.servicebus.messaging.queueclient.peek), [ Vernieuwen van de vergrendeling](/dotnet/api/microsoft.servicebus.messaging.queueclient.renewmessagelock), [bericht plannen](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync), [geplande bericht annuleren](/dotnet/api/microsoft.azure.servicebus.queueclient.cancelscheduledmessageasync), [regel toevoegen](/dotnet/api/microsoft.azure.servicebus.ruledescription), [regel verwijderen](/dotnet/api/microsoft.azure.servicebus.ruledescription), [Sessie vernieuwen vergrendeling](/dotnet/api/microsoft.servicebus.messaging.messagesession.renewlock), [Set sessiestatus](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate), [Get-sessiestatus](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate), en [opsommen sessies](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions).

## <a name="partitioned-entities-limitations"></a>Gepartitioneerde entiteiten beperkingen

Service Bus legt momenteel de volgende beperkingen met betrekking tot gepartitioneerde wachtrijen en onderwerpen:

* Gepartitioneerde wachtrijen en onderwerpen worden niet ondersteund in de laag Premium messaging uit.
* Gepartitioneerde wachtrijen en onderwerpen bieden geen ondersteuning voor het verzenden van berichten die deel uitmaken van verschillende sessies in één transactie.
* Service Bus staat momenteel maximaal 100 gepartitioneerde wachtrijen en onderwerpen per naamruimte toe. Elke gepartitioneerde wachtrij of onderwerp telt naar het quotum van 10.000 entiteiten per naamruimte (geldt niet voor Premium-laag).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de belangrijkste concepten van de AMQP 1.0-messaging-specificatie in de [AMQP 1.0-protocol handleiding](service-bus-amqp-protocol-guide.md).

[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablepartitioning
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md
