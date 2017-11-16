---
title: Gepartitioneerde Azure Service Bus-wachtrijen en onderwerpen maken | Microsoft Docs
description: Beschrijft hoe Service Bus-wachtrijen en onderwerpen met behulp van meerdere bericht beleggingsmakelaars partitie.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a0c7d5a2-4876-42cb-8344-a1fc988746e7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2017
ms.author: sethm
ms.openlocfilehash: beebfb496604b422e091cd3b4425933f3cea1283
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="partitioned-queues-and-topics"></a>Gepartitioneerde wachtrijen en onderwerpen
Azure Service Bus maakt gebruik van meerdere bericht beleggingsmakelaars om berichten te verwerken en meerdere berichten-stores voor het opslaan van berichten. Een conventionele wachtrij of onderwerp is verwerkt door een enkel bericht broker en opgeslagen in één berichten-store. Service Bus *partities* wachtrijen en onderwerpen, inschakelen of *berichtentiteiten*, moet worden gepartitioneerd op meerdere bericht beleggingsmakelaars en berichten-stores. Dit betekent dat de totale doorvoer van een gepartitioneerde entiteit wordt niet langer beperkt door de prestaties van een enkel bericht broker of berichten-store. Bovendien weer een tijdelijke onderbreking van berichten-store niet een gepartitioneerde wachtrij of onderwerp niet beschikbaar. Gepartitioneerde wachtrijen en onderwerpen kunnen bevatten alle uitgebreide Service Bus-functies, zoals ondersteuning voor transacties en sessies.

Zie voor meer informatie over de interne werking van Service Bus de [Service Bus-architectuur] [ Service Bus architecture] artikel.

Partitioneren is standaard ingeschakeld bij het maken van de entiteit in alle wachtrijen en onderwerpen in de standaard- en Premium messaging. U kunt standaard laag berichtentiteiten zonder partitioneren maken, maar wachtrijen en onderwerpen in een Premium-naamruimte altijd worden gepartitioneerd; Deze optie kan niet worden uitgeschakeld. 

Het is niet mogelijk om te wijzigen van de partitionering optie op een bestaande wachtrij of onderwerp in de lagen Standard of Premium, kunt u alleen de optie instellen bij het maken van de entiteit.

## <a name="how-it-works"></a>Hoe werkt het?

Elke gepartitioneerde wachtrij of onderwerp bestaat uit meerdere fragmenten. Elke fragment is opgeslagen in een andere berichten-store en verwerkt door een ander bericht broker. Wanneer een bericht wordt verzonden naar een gepartitioneerde wachtrij of onderwerp, wijst Service Bus het bericht toe aan een van de fragmenten. De selectie wordt willekeurig gedaan door Service Bus- of met behulp van een partitiesleutel die de afzender kunt opgeven.

Wanneer een client wil ontvangen een bericht van een gepartitioneerde wachtrij of van een abonnement op een gepartitioneerde query's voor Service Bus-onderwerp alle fragmenten voor berichten, retourneert vervolgens het eerste bericht dat is verkregen van een van de berichten-stores bij de ontvanger. Service Bus-caches de andere berichten en retourneert ze als deze ontvangt aanvullende ontvangen aanvragen. Een ontvangende client is niet op de hoogte van de partitionering; het gedrag clientgerichte van een gepartitioneerde wachtrij of onderwerp (bijvoorbeeld, lezen, voltooid, uitstelt, wachtrij voor onbestelbare, veelgevraagde) is vrijwel identiek aan het gedrag van een reguliere entiteit.

Er is geen extra kosten bij het verzenden van een bericht of het bericht in een gepartitioneerde wachtrij of onderwerp.

## <a name="enable-partitioning"></a>Partitioneren inschakelen

De Azure SDK versie 2.2 of hoger gebruiken voor het gebruik van gepartitioneerde wachtrijen en onderwerpen met Azure Service Bus, of geef `api-version=2013-10` of hoger in de HTTP-aanvragen.

### <a name="standard"></a>Standard

In de Standard messaging laag, kunt u Service Bus-wachtrijen en onderwerpen in 1, 2, 3, 4 of 5 GB-grootten (de standaardwaarde is 1 GB). Service Bus maakt met het partitionering ingeschakeld, 16 kopieën (16 partities) van de entiteit voor elke GB die u opgeeft. Als zodanig als u een wachtrij die is 5 GB groot maken, met 16 partities de maximale wachtrijgrootte wordt (5 \* 16) = 80 GB. U kunt de maximale grootte van uw gepartitioneerde wachtrij of onderwerp zien door te kijken op de vermelding ervan op de [Azure-portal][Azure portal], in de **overzicht** blade voor die entiteit.

### <a name="premium"></a>Premium

In een naamruimte van de laag Premium kunt u Service Bus-wachtrijen en onderwerpen in 1, 2, 3, 4, 5, 10, 20, 40 of 80 GB grootten (de standaardwaarde is 1 GB). Service Bus maakt twee partities per entiteit met het partitioneren van de standaard ingeschakeld. U kunt de maximale grootte van uw gepartitioneerde wachtrij of onderwerp zien door te kijken op de vermelding ervan op de [Azure-portal][Azure portal], in de **overzicht** blade voor die entiteit.

Zie voor meer informatie over partitioneren in de laag Premium messaging uit [Service Bus Premium en Standard Messaging](service-bus-premium-messaging.md). 

### <a name="create-a-partitioned-entity"></a>Maak een gepartitioneerde entiteit

Er zijn verschillende manieren voor het maken van een gepartitioneerde wachtrij of onderwerp. Wanneer u de wachtrij of onderwerp van uw toepassing maakt, kunt u inschakelen voor de wachtrij of onderwerp partitioneren door respectievelijk de [QueueDescription.EnablePartitioning] [ QueueDescription.EnablePartitioning] of [ TopicDescription.EnablePartitioning] [ TopicDescription.EnablePartitioning] eigenschap **true**. Deze eigenschappen moeten worden ingesteld op het moment dat de wachtrij of onderwerp is gemaakt. Zoals eerder gezegd, is het niet mogelijk om te wijzigen van deze eigenschappen op een bestaande wachtrij of onderwerp. Bijvoorbeeld:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

U kunt ook maken een gepartitioneerde wachtrij of onderwerp in de [Azure-portal] [ Azure portal] of in Visual Studio. Wanneer u een wachtrij of onderwerp in de portal maakt de **inschakelen partitioneren** optie in de wachtrij of onderwerp **maken** in het dialoogvenster is standaard ingeschakeld. U kunt deze optie in een entiteit standaardcategorie; alleen uitschakelen in de laag Premium is partitioneren altijd ingeschakeld. Klik in Visual Studio de **inschakelen partitioneren** selectievakje in de **nieuwe wachtrij** of **nieuw onderwerp** in het dialoogvenster.

## <a name="use-of-partition-keys"></a>Gebruik van partitiesleutels
Wanneer een bericht in de wachtrij in een gepartitioneerde wachtrij of onderwerp is, controleert de aanwezigheid van een partitiesleutel op Service Bus. Als er een is gevonden, selecteert u het fragment op basis van die sleutel. Als een partitiesleutel niet wordt gevonden, wordt het fragment op basis van een interne algoritme geselecteerd.

### <a name="using-a-partition-key"></a>Met behulp van een partitiesleutel
Sommige scenario's, zoals sessies of transacties, vereisen berichten worden opgeslagen in een specifieke fragment. Deze scenario's vereist het gebruik van een partitiesleutel. Alle berichten die gebruikmaken van dezelfde partitiesleutel zijn toegewezen aan de dezelfde fragment. Als het fragment tijdelijk niet beschikbaar is, Service Bus een fout geretourneerd.

Afhankelijk van het scenario worden verschillende berichteigenschappen als een partitiesleutel gebruikt:

**Sessie-id**: als een bericht heeft de [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] eigenschap instelt, wordt de Service Bus deze eigenschap wordt gebruikt als de partitiesleutel. Op deze manier worden alle berichten die deel uitmaken van dezelfde sessie verwerkt door de dezelfde bericht broker. Hierdoor kunnen Service Bus bericht ordening en consistentie van de sessiestatus te garanderen.

**PartitionKey**: als een bericht heeft de [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] eigenschap maar niet de [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] eigenschap instelt, wordt de Service Bus maakt gebruik van de [PartitionKey] [ PartitionKey] eigenschap als de partitiesleutel. Als het bericht zowel bevat de [SessionId] [ SessionId] en de [PartitionKey] [ PartitionKey] set eigenschappen, beide eigenschappen moeten identiek zijn. Als de [PartitionKey] [ PartitionKey] eigenschap is ingesteld op een andere waarde dan de [SessionId] [ SessionId] retourneert een ongeldige eigenschap, Service Bus uitzondering van de bewerking. De [PartitionKey] [ PartitionKey] eigenschap moet worden gebruikt als een afzender verzendt een niet-sessie op de hoogte transactionele berichten. De partitiesleutel zorgt ervoor dat alle berichten die zijn verzonden binnen een transactie worden verwerkt door de dezelfde messaging broker.

**MessageId**: als de wachtrij of onderwerp heeft de [QueueDescription.RequiresDuplicateDetection] [ QueueDescription.RequiresDuplicateDetection] eigenschap ingesteld op **true** en de [ BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] of [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] eigenschappen zijn niet ingesteld, wordt de [ BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] eigenschap fungeert als de partitiesleutel. (Houd er rekening mee dat de Microsoft .NET- en AMQP-bibliotheken een bericht-ID automatisch toewijzen als de betreffende toepassing niet bestaat.) In dit geval worden alle kopieën van hetzelfde bericht verwerkt door de dezelfde bericht broker. Hierdoor kunnen Service Bus om te detecteren en verwijderen van dubbele berichten. Als de [QueueDescription.RequiresDuplicateDetection] [ QueueDescription.RequiresDuplicateDetection] eigenschap niet is ingesteld op **true**, Service Bus houdt geen rekening met de [MessageId] [ MessageId] eigenschap als partitiesleutel.

### <a name="not-using-a-partition-key"></a>Niet met behulp van een partitiesleutel
In het ontbreken van een partitiesleutel distribueert Service Bus berichten round-robin toewijst op alle fragmenten van gepartitioneerde wachtrij of onderwerp. Als het gekozen fragment niet beschikbaar is, wijst Service Bus het bericht toe aan een andere fragment. Op deze manier de verzendbewerking slaagt ondanks berichten-store, tijdelijk niet beschikbaar. U wordt echter niet bereiken de gegarandeerde ordening waarmee een partitiesleutel.

Zie voor een diepgaandere bespreking van de verhouding tussen de beschikbaarheid (geen partitiesleutel) en consistentie (met behulp van een partitiesleutel) [in dit artikel](../event-hubs/event-hubs-availability-and-consistency.md). Deze informatie is evenveel van toepassing op gepartitioneerde Service Bus-entiteiten.

Service Bus geven genoeg tijd heeft in de wachtrij plaatsen het bericht in een andere fragment de [MessagingFactorySettings.OperationTimeout] [ MessagingFactorySettings.OperationTimeout] waarde die is opgegeven door de client die u verzendt het bericht moet groter zijn dan 15 seconden. Het verdient aanbeveling in te stellen de [OperationTimeout] [ OperationTimeout] eigenschap op de standaardwaarde van 60 seconden.

Houd er rekening mee dat een partitiesleutel 'pincodes"een bericht naar een specifieke fragment. Als de berichten-store die dit fragment bevat niet beschikbaar is, betekent dit dat Service Bus een fout geretourneerd. In het ontbreken van een partitiesleutel, Service Bus geen andere fragment kunt kiezen en de bewerking is geslaagd. Daarom wordt aanbevolen dat u niet een partitiesleutel opgeeft tenzij deze vereist is.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Onderwerpen over geavanceerde: het gebruik van transacties met gepartitioneerde entiteiten
Berichten die worden verzonden als onderdeel van een transactie moeten een partitiesleutel opgeven. Dit is een van de volgende eigenschappen: [BrokeredMessage.SessionId][BrokeredMessage.SessionId], [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey], of [ BrokeredMessage.MessageId][BrokeredMessage.MessageId]. Alle berichten die worden verzonden als onderdeel van dezelfde transactie moeten dezelfde partitiesleutel opgeven. Als u probeert een bericht zonder een partitiesleutel binnen een transactie te verzenden, wordt met Service Bus een ongeldige bewerking-uitzondering geretourneerd. Als u probeert te verzenden binnen dezelfde transactie meerdere berichten met verschillende partitiesleutels, betekent dit dat Service Bus een ongeldige bewerking-uitzondering geretourneerd. Bijvoorbeeld:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Als een van de eigenschappen die als een partitiesleutel fungeren is ingesteld, pincodes Service Bus het bericht naar een specifieke fragment. Dit probleem treedt al dan niet een transactie wordt gebruikt. Het verdient aanbeveling dat u geen een partitiesleutel opgeeft als het is niet nodig.

## <a name="using-sessions-with-partitioned-entities"></a>Met behulp van sessies met gepartitioneerde entiteiten
Transactionele berichten worden verzonden naar een sessie-bewuste onderwerp of een wachtrij, het bericht moet hebben de [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] eigenschap is ingesteld. Als de [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] eigenschap ook is opgegeven, moet gelijk aan de [SessionId] [ SessionId] eigenschap. Als deze verschillen, wordt met Service Bus een ongeldige bewerking-uitzondering geretourneerd.

In tegenstelling tot gewone (niet-gepartitioneerde) wachtrijen en onderwerpen is het niet mogelijk meerdere berichten verzenden naar verschillende sessies met een enkele transactie. Als er is geprobeerd, wordt met Service Bus een ongeldige bewerking-uitzondering geretourneerd. Bijvoorbeeld:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Automatische bericht doorsturen met gepartitioneerde entiteiten
Service Bus ondersteunt automatische bericht doorsturen van aan of tussen gepartitioneerde entiteiten. Instellen om automatische bericht doorsturen, de [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] eigenschap in de wachtrij of abonnement. Als een partitiesleutel is opgegeven voor het bericht ([SessionId][SessionId], [PartitionKey][PartitionKey], of [MessageId] [ MessageId]), die de partitiesleutel wordt gebruikt voor de doelentiteit.

## <a name="considerations-and-guidelines"></a>Overwegingen en richtlijnen
* **Hoge consistentie functies**: als een entiteit functies zoals sessies, detectie van duplicaten of expliciete controle van de partitiesleutel gebruikt, wordt de messaging bewerkingen altijd worden omgeleid naar specifieke fragmenten. Als een van de fragmenten intensief verkeer ervaren of het onderliggende archief niet in orde is, wordt deze bewerkingen mislukken en beschikbaarheid wordt beperkt. Over het algemeen is de consistentiecontrole nog steeds veel hoger zijn dan niet-gepartitioneerde entiteiten; alleen een subset van verkeer ondervindt problemen, in plaats van alle verkeer. Zie voor meer informatie dit [bespreking van de beschikbaarheid en consistentie](../event-hubs/event-hubs-availability-and-consistency.md).
* **Beheer**: bewerkingen zoals het maken, bijwerken en verwijderen moeten worden uitgevoerd op de fragmenten van de entiteit. Als een fragment niet in orde is, kan dit leiden tot fouten voor deze bewerkingen. Voor de Get-bewerking moet informatie zoals bericht telt worden samengevoegd uit alle fragmenten. Als een fragment niet in orde is, wordt de beschikbaarheidsstatus van de entiteit gerapporteerd als beperkt.
* **Laag volume bericht scenario's**: voor dergelijke scenario's, vooral wanneer het HTTP-protocol, moet u mogelijk meerdere uitvoeren ontvangstbewerkingen ter verkrijging van alle berichten. Voor ontvangstaanvragen, het front-end voert een receive op alle fragmenten en plaatst alle antwoorden die worden ontvangen. Een aanvraag van de volgende ontvangen op de verbinding wilt profiteren van deze opslaan in cache en ontvangen latenties wordt niet lager zijn. Als u meerdere verbindingen of HTTP gebruiken, stelt die een nieuwe verbinding voor elke aanvraag. Hierdoor is er geen garantie dat deze zou land op hetzelfde knooppunt. Als alle bestaande berichten die worden vergrendeld en in de cache opgeslagen in een andere front-end, de ontvangstbewerking retourneert **null**. Berichten uiteindelijk verlopen en u ze opnieuw kunt ontvangen. HTTP-keepalive wordt aanbevolen.
* **Bladeren/Peek berichten**: [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) altijd retourneert geen het aantal berichten dat is opgegeven in de [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.messagecount) eigenschap. Er zijn twee veelvoorkomende redenen voor dit. Een reden hiervoor is dat de geaggregeerde grootte van de verzameling van berichten is groter dan de maximale grootte van 256 KB. Een andere reden is dat als de wachtrij of onderwerp heeft de [EnablePartitioning eigenschap](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning) ingesteld op **true**, een partitie geen voldoende berichten naar het aangevraagde aantal berichten te voltooien. In het algemeen als een toepassing wil ontvangen van een bepaald aantal berichten, deze moet aanroepen [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) herhaaldelijk totdat het aantal berichten ophalen of er zijn geen berichten meer om te kijken. Zie voor meer informatie, codevoorbeelden, waaronder de [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) of [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient.peekbatch) API-documentatie.

## <a name="latest-added-features"></a>Meest recente extra functies
* Toevoegen of verwijderen regel wordt nu ondersteund met gepartitioneerde entiteiten. Verschillend zijn van niet-gepartitioneerde entiteiten, deze bewerkingen worden niet ondersteund onder transacties. 
* AMQP wordt nu ondersteund voor het verzenden en ontvangen van berichten naar en van een gepartitioneerde entiteit.
* AMQP wordt nu ondersteund voor de volgende bewerkingen: [Batch verzenden](/dotnet/api/microsoft.servicebus.messaging.queueclient.sendbatch), [Batch ontvangen](/dotnet/api/microsoft.servicebus.messaging.queueclient.receivebatch), [ontvangen door volgnummer](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive), [inspecteren](/dotnet/api/microsoft.servicebus.messaging.queueclient.peek), [ Vernieuwen van de vergrendeling](/dotnet/api/microsoft.servicebus.messaging.queueclient.renewmessagelock), [bericht plannen](/dotnet/api/microsoft.servicebus.messaging.queueclient.schedulemessageasync), [geplande bericht annuleren](/dotnet/api/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync), [regel toevoegen](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [regel verwijderen](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [Sessie vernieuwen vergrendeling](/dotnet/api/microsoft.servicebus.messaging.messagesession.renewlock), [Set sessiestatus](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate), [Get-sessiestatus](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate), en [opsommen sessies](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions).

## <a name="partitioned-entities-limitations"></a>Gepartitioneerde entiteiten beperkingen
Service Bus legt momenteel de volgende beperkingen met betrekking tot gepartitioneerde wachtrijen en onderwerpen:

* Gepartitioneerde wachtrijen en onderwerpen bieden geen ondersteuning voor het verzenden van berichten die deel uitmaken van verschillende sessies in één transactie.
* Service Bus kunnen momenteel maximaal 100 gepartitioneerde wachtrijen en onderwerpen per naamruimte. Elke gepartitioneerde wachtrij of onderwerp telt naar het quotum van 10.000 entiteiten per naamruimte (geldt niet voor Premium-laag).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de belangrijkste concepten van de AMQP 1.0-messaging-specificatie in de [AMQP 1.0-protocol handleiding](service-bus-amqp-protocol-guide.md).

[Service Bus architecture]: service-bus-architecture.md
[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablepartitioning
[BrokeredMessage.SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid
[BrokeredMessage.PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.partitionkey
[SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.partitionkey
[QueueDescription.RequiresDuplicateDetection]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid
[MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid
[MessagingFactorySettings.OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md
