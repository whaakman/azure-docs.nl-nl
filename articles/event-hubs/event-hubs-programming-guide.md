---
title: Programmeerhandleiding voor Azure Event Hubs | Microsoft Docs
description: Code schrijven voor Azure Event Hubs met behulp van de Azure .NET SDK.
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 64cbfd3d-4a0e-4455-a90a-7f3d4f080323
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 11/16/2017
ms.author: sethm
ms.openlocfilehash: 7d5f14d5a65253cf0aad1811ace419bf2f39f7db
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="event-hubs-programming-guide"></a>Programmeerhandleiding voor Event Hubs

Dit artikel worden enkele algemene scenario's bij het schrijven van code met Azure Event Hubs en de Azure .NET SDK. Er wordt uitgegaan van een basisbegrip van Event Hubs. Zie het [Overzicht van Event Hubs](event-hubs-what-is-event-hubs.md) voor een conceptueel overzicht van Event Hubs.

## <a name="event-publishers"></a>Gebeurtenisuitgevers

U kunt gebeurtenissen verzenden naar een event hub met behulp van HTTP POST of via een AMQP 1.0-verbinding. De keuze van die wordt gebruikt en wanneer is afhankelijk van het specifieke scenario worden toegepast. AMQP 1.0-verbindingen zijn brokered verbindingen in Service Bus. Ze zijn met name geschikt voor scenario‘s met vaak voorkomende hogere berichtvolumes en lagere latentievereisten, omdat ze een permanent berichtenkanaal bieden.

U kunt maken en beheren van Event Hubs met behulp van de [NamespaceManager][] klasse. Wanneer de beheerde .NET-API‘s worden gebruikt, zijn de klassen [EventHubClient](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) en [EventData][] de primaire constructs voor het publiceren van gegevens naar Event Hubs. [EventHubClient][] biedt het AMQP-communicatiekanaal gedurende welke gebeurtenissen worden verzonden naar de event hub. De [EventData][] klasse vertegenwoordigt een gebeurtenis en wordt gebruikt om berichten te publiceren naar een event hub. Deze klasse bevat de hoofdtekst, bepaalde metagegevens en headerinformatie over de gebeurtenis. Andere eigenschappen worden toegevoegd aan de [EventData][] object als het een event hub passeert.

## <a name="get-started"></a>Aan de slag

De .NET-klassen die ondersteuning bieden voor Event Hubs, worden aangeboden in de Microsoft.ServiceBus.dll-assembly. Downloaden van het [Service Bus NuGet-pakket](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is de eenvoudigste manier om te verwijzen naar de Service Bus-API en om de toepassing te configureren met alle Service Bus-afhankelijkheden. U kunt echter ook de [Pakketbeheerconsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) in Visual Studio gebruiken. Hiervoor voert u de volgende opdracht uit in het venster voor de [Pakketbeheerconsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

```
Install-Package WindowsAzure.ServiceBus
```

## <a name="create-an-event-hub"></a>Een Event Hub maken
U kunt de klasse [NamespaceManager][] gebruiken om Event Hubs te maken. Bijvoorbeeld:

```csharp
var manager = new Microsoft.ServiceBus.NamespaceManager("mynamespace.servicebus.windows.net");
var description = manager.CreateEventHub("MyEventHub");
```

In de meeste gevallen wordt aanbevolen om de methoden [CreateEventHubIfNotExists][] te gebruiken. Zo voorkomt u dat er uitzonderingen worden gegenereerd bij het opnieuw starten van de service. Bijvoorbeeld:

```csharp
var description = manager.CreateEventHubIfNotExists("MyEventHub");
```

Voor alle bewerkingen met betrekking tot het maken van Event Hubs, inclusief [CreateEventHubIfNotExists][], zijn in de betreffende naamruimte machtigingen voor **Beheren** vereist. Als u de machtigingen van de publicatie- of consumertoepassingen wilt beperken, kunt u het aanroepen van deze bewerkingen in productiecode vermijden door referenties met beperkte machtigingen te gebruiken.

De [EventHubDescription](/dotnet/api/microsoft.servicebus.messaging.eventhubdescription) klasse bevat details over een event hub, met inbegrip van de autorisatieregels, het bewaarinterval bericht, partitie-id's, status en pad. U kunt deze klasse de metagegevens voor een event hub bij te werken.

## <a name="create-an-event-hubs-client"></a>Een Event Hubs-client maken
De primaire klasse voor interactie met Event Hubs is [Microsoft.ServiceBus.Messaging.EventHubClient][EventHubClient]. Deze klasse biedt mogelijkheden voor zowel verzenden als ontvangen. U kunt ook het gebruik van deze klasse instantiëren de [maken](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.create) methode, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
var client = EventHubClient.Create(description.Path);
```

Deze methode maakt gebruik van de Service Bus-verbindingsinformatie in het App-configuratiebestand in de sectie `appSettings`. Zie de documentatie voor de methode [Microsoft.ServiceBus.Messaging.EventHubClient.Create(System.String)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Create_System_String_) voor een voorbeeld van de `appSettings`-XML die wordt gebruikt voor het opslaan van de Service Bus-verbindingsinformatie.

Een andere optie is om de client te maken uit een verbindingsreeks. Deze optie werkt goed bij het gebruik van Azure-werkrollen, omdat u de verbindingsreeks kunt opslaan in de configuratie-eigenschappen voor de werkrol. Bijvoorbeeld:

```csharp
EventHubClient.CreateFromConnectionString("your_connection_string");
```

De verbindingsreeks heeft dezelfde indeling als in het App-configuratiebestand voor de vorige methoden:

```
Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[key]
```

Ten slotte het is ook mogelijk maken een [EventHubClient][] object uit een [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) exemplaar, zoals weergegeven in het volgende voorbeeld:

```csharp
var factory = MessagingFactory.CreateFromConnectionString("your_connection_string");
var client = factory.CreateEventHubClient("MyEventHub");
```

Het is belangrijk mee dat extra [EventHubClient][] objecten die zijn gemaakt op basis van een messagingfactory-exemplaar opnieuw de dezelfde onderliggende TCP-verbinding gebruiken. Voor deze objecten geldt daarom een doorvoerlimiet aan clientzijde. De methode [Create](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Create_System_String_) maakt gebruik van één MessagingFactory. Als u van één afzender een zeer hoge doorvoersnelheid nodig hebt, kunt u meerdere MessagingFactory-exemplaren maken en vervolgens van elk MessagingFactory-exemplaar één [EventHubClient][]-object.

## <a name="send-events-to-an-event-hub"></a>Gebeurtenissen verzenden naar een event hub
Verzenden van gebeurtenissen naar een event hub door het maken van een [EventData][] exemplaar en dit te verzenden via de [verzenden](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) methode. Deze methode heeft één [EventData][] instantieparameter en verzendt deze synchroon naar een event hub.

## <a name="event-serialization"></a>Gebeurtenisserialisatie
De klasse [EventData][] heeft [vier overbelaste constructors](/dotnet/api/microsoft.servicebus.messaging.eventdata#constructors_) die verschillende parameters hebben, zoals een object en serializer, een bytematrix of een stroom. Het is ook mogelijk om de klasse [EventData][] te instantiëren en de hoofdstroom later in te stellen. Als u JSON gebruikt met [EventData][], kunt u **Encoding.UTF8.GetBytes()** gebruiken om de bytematrix op te halen voor een met JSON gecodeerde tekenreeks.

## <a name="partition-key"></a>Partitiesleutel
De klasse [EventData][] heeft een eigenschap [PartitionKey][] waarmee de afzender een waarde kan opgeven die wordt gehasht om een partitietoewijzing te produceren. Met behulp van een partitiesleutel zorgt ervoor dat alle gebeurtenissen met dezelfde sleutel worden verzonden naar dezelfde partitie in de event hub. Vaak gebruikte partitiesleutels zijn gebruikerssessie-id's en unieke afzender-id‘s. De eigenschap [PartitionKey][] is optioneel en kan worden opgegeven bij het gebruik van de methode [Microsoft.ServiceBus.Messaging.EventHubClient.Send(Microsoft.ServiceBus.Messaging.EventData)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) of de methode [Microsoft.ServiceBus.Messaging.EventHubClient.SendAsync(Microsoft.ServiceBus.Messaging.EventData)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendAsync_Microsoft_ServiceBus_Messaging_EventData_). Als u geen waarde opgeeft voor [PartitionKey][], worden verzonden gebeurtenissen gedistribueerd naar partities die gebruikmaken van een round robin-model.

### <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen

Met behulp van een partitiesleutel is optioneel en moet u zorgvuldig al dan niet een te gebruiken. In veel gevallen is met behulp van een partitiesleutel een goede keuze als ordening van gebeurtenissen belangrijk is. Wanneer u een partitiesleutel, deze partities vereisen beschikbaarheid op één knooppunt en storingen kunnen optreden na verloop van tijd; Wanneer compute bijvoorbeeld patch en knooppunten opnieuw opstarten. Als zodanig als u een partitie-ID instellen en deze partitie niet meer beschikbaar is voor een bepaalde reden, wordt een poging tot toegang tot de gegevens in de betreffende partitie mislukken. Als hoge beschikbaarheid zeer belangrijk is, Geef een partitiesleutel; in dat geval worden gebeurtenissen verzonden naar partities die gebruikmaken van de eerder beschreven round robin-model. In dit scenario maakt u een expliciete keuze tussen beschikbaarheid (geen partitie-ID) en consistentie (vastmaken gebeurtenissen naar een partitie-ID).

Een andere overweging verwerkt vertragingen bij de verwerking van gebeurtenissen. In sommige gevallen is mogelijk beter te verwijderen van gegevens en probeer het opnieuw dan bij de verwerking, waardoor mogelijk meer downstreamverwerking vertragingen te houden. Met een aandelenkoersen is het bijvoorbeeld beter moet worden gewacht voor een volledige, actuele gegevens, maar in een live chatten of VOIP-scenario in plaats daarvan hebt u de gegevens snel, zelfs als het is niet voltooid.

Deze overwegingen beschikbaarheid gegeven in deze scenario's kunt u een van de volgende fout verwerking strategieën:

- Stoppen (stop lezen uit Event Hubs totdat dingen zijn opgelost)
- Drop (berichten zijn niet belangrijk, neerzetten ze)
- Probeer (nieuwe poging die de berichten, zoals u ziet past)
- [Wachtrij voor onbestelbare](../service-bus-messaging/service-bus-dead-letter-queues.md) (een wachtrij gebruiken of een andere event hub naar onbestelbare letter alleen de berichten kunnen niet worden verwerkt)

Zie voor meer informatie en een discussie over de verschillen tussen de beschikbaarheid en consistentie [beschikbaarheid en consistentie in Event Hubs](event-hubs-availability-and-consistency.md). 

## <a name="batch-event-send-operations"></a>Batchbewerkingen voor het verzenden van gebeurtenissen
Verzenden van gebeurtenissen in batches kunt verhogen doorvoer. De [SendBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__) methode duurt een **IEnumerable** parameter van het type [EventData][] en de hele batch als een atomic-bewerking naar de event hub verzendt.

```csharp
public void SendBatch(IEnumerable<EventData> eventDataList);
```

Let op: één batch mag niet groter zijn dan de limiet van 256 kB voor een gebeurtenis. Daarnaast maakt elk bericht in de batch gebruik van dezelfde uitgever-id. Het is de verantwoordelijkheid van de afzender om ervoor te zorgen dat de batch de maximale gebeurtenisgrootte niet overschrijdt. Als dit wel gebeurt, wordt aan clientzijde een **Verzendfout** gegenereerd. U kunt de Help-methode [EventHubClient.CreateBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.createbatch) om ervoor te zorgen dat de batch niet meer dan 256 KB. Ophalen van een lege [EventDataBatch](/dotnet/api/microsoft.servicebus.messaging.eventdatabatch) van de [CreateBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.createbatch) API en gebruik vervolgens [TryAdd](/dotnet/api/microsoft.servicebus.messaging.eventdatabatch.tryadd#Microsoft_ServiceBus_Messaging_EventDataBatch_TryAdd_Microsoft_ServiceBus_Messaging_EventData_) om toe te voegen gebeurtenissen kan de batch. Gebruik tot slot [EventDataBatch.ToEnumerable](/dotnet/api/microsoft.servicebus.messaging.eventdatabatch.toenumerable) ophalen van de onderliggende gebeurtenissen moeten worden doorgegeven aan de [EventHubClient.Send](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.send) API.

## <a name="send-asynchronously-and-send-at-scale"></a>Asynchroon verzenden en op schaal verzenden
U kunt ook gebeurtenissen naar een event hub asynchroon verzenden. Asynchroon verzenden kan de snelheid verhogen waarmee een client gebeurtenissen verzendt. Zowel de methode [Send](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.send) als de methode [SendBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendbatch) is beschikbaar in asynchrone versies die een [Taak](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)-object retourneren. Hoewel dit de doorvoer kan verhogen, kan het er ook voor zorgen dat de client gebeurtenissen blijft verzenden terwijl deze wordt beperkt door de Event Hubs-service. Bij onjuiste implementatie kan dit resulteren in fouten of verloren berichten. Daarnaast kunt u voor de client de eigenschap [RetryPolicy](/dotnet/api/microsoft.servicebus.messaging.cliententity.retrypolicy) gebruiken om de opties van de client voor direct opnieuw proberen te beheren.

## <a name="create-a-partition-sender"></a>Partitieafzender maken
Hoewel het meest voorkomende gebeurtenissen verzenden naar een event hub zonder een partitiesleutel, in sommige gevallen kunt u gebeurtenissen rechtstreeks naar een bepaalde partitie verzenden. Bijvoorbeeld:

```csharp
var partitionedSender = client.CreatePartitionedSender(description.PartitionIds[0]);
```

[CreatePartitionedSender](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_CreatePartitionedSender_System_String_) retourneert een [EventHubSender](/dotnet/api/microsoft.servicebus.messaging.eventhubsender) object dat u gebruiken kunt om gebeurtenissen te publiceren naar een specifieke event hub-partitie.

## <a name="event-consumers"></a>Gebeurtenisconsumers
Event Hubs heeft twee primaire modellen voor gebeurtenisgebruik: directe ontvangers en abstracties van een hoger niveau, zoals [EventProcessorHost][]. Directe ontvangers zijn verantwoordelijk voor hun eigen coördinatie van de toegang tot partities binnen een *consumergroep*. Een consumergroep is een weergave (status, positie of offset) in een gepartitioneerde event hub.

### <a name="direct-consumer"></a>Directe consumer
De meest directe manier om te lezen van een partitie is met de [EventHubReceiver](/dotnet/apie/microsoft.servicebus.messaging.eventhubreceiver) klasse. Als u een instantie van deze klasse wilt maken, moet u een exemplaar van de klasse [EventHubConsumerGroup](/dotnet/api/microsoft.servicebus.messaging.eventhubconsumergroup) gebruiken. In het volgende voorbeeld wordt worden de partitie-ID opgegeven bij het maken van de ontvanger voor de consumergroep:

```csharp
EventHubConsumerGroup group = client.GetDefaultConsumerGroup();
var receiver = group.CreateReceiver(client.GetRuntimeInformation().PartitionIds[0]);
```

De methode [CreateReceiver](/dotnet/api/microsoft.servicebus.messaging.eventhubconsumergroup#methods_summary) heeft verschillende overloads die het beheer vergemakkelijken van de lezer die wordt gemaakt. Deze methoden omvatten het opgeven van een offset als een tekenreeks of tijdstempel, en de mogelijkheid om op te geven of deze opgegeven offset moet worden opgenomen in de geretourneerde stroom of later moet worden gestart. Nadat u de ontvanger hebt gemaakt, kunt u gebeurtenissen ontvangen op het geretourneerde object. De methode[Receive](/dotnet/api/microsoft.servicebus.messaging.eventhubreceiver#methods_summary) heeft vier overloads die de ontvangstparameters beheren, zoals batchgrootte en wachttijd. U kunt de asynchrone versies van deze methoden gebruiken om de doorvoer van een consumer te verhogen. Bijvoorbeeld:

```csharp
bool receive = true;
string myOffset;
while(receive)
{
    var message = receiver.Receive();
    myOffset = message.Offset;
    string body = Encoding.UTF8.GetString(message.GetBytes());
    Console.WriteLine(String.Format("Received message offset: {0} \nbody: {1}", myOffset, body));
}
```

De berichten worden met betrekking tot een specifieke partitie ontvangen in de volgorde waarin ze naar de event hub zijn verzonden. De offset is een tekenreekstoken dat wordt gebruikt voor het identificeren van een bericht in een partitie.

Houd er rekening mee dat één partitie kan niet meer dan 5 lezers op elk gewenst moment. Wanneer lezers verbinding maken of de verbinding verbreken, kunnen hun sessies gedurende enkele minuten actief blijven voordat wordt herkend dat de verbinding is verbroken. Gedurende deze tijd kan het opnieuw verbinding maken met een partitie mislukken. Zie voor een compleet voorbeeld van het schrijven van een directe ontvanger voor Event Hubs, de [Event Hubs directe ontvangers](https://code.msdn.microsoft.com/Event-Hub-Direct-Receivers-13fa95c6) voorbeeld.

### <a name="event-processor-host"></a>Gebeurtenisprocessorhost
Met de klasse [EventProcessorHost][] worden gegevens uit Event Hubs verwerkt. Gebruik deze implementatie bij het bouwen van gebeurtenislezers op het .NET-platform. [EventProcessorHost][] biedt een thread-veilige, beveiligde runtimeomgeving met meerdere processen voor implementaties van gebeurtenisprocessors die ook beheer biedt van controlepunten en partitielease.

Als u de klasse [EventProcessorHost][] wilt gebruiken, kunt u [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor) implementeren. Deze interface bevat drie methoden:

* [OpenAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_OpenAsync_Microsoft_ServiceBus_Messaging_PartitionContext_)
* [CloseAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_CloseAsync_Microsoft_ServiceBus_Messaging_PartitionContext_Microsoft_ServiceBus_Messaging_CloseReason_)
* [ProcessEventsAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_ProcessEventsAsync_Microsoft_ServiceBus_Messaging_PartitionContext_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__)

Voor het starten van de verwerking van gebeurtenissen instantiëren [EventProcessorHost][], bieden de juiste parameters voor uw event hub. Roep vervolgens [RegisterEventProcessorAsync](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost#Microsoft_ServiceBus_Messaging_EventProcessorHost_RegisterEventProcessorAsync__1) aan om de [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor)-implementatie te registreren met de runtime. Op dit moment probeert de host een lease op elke partitie in de event hub met behulp van een greedy-algoritme te verkrijgen. Deze leases gedurende een bepaald tijdsbestek geldig en moeten daarna worden vernieuwd. Zodra nieuwe knooppunten, in dit geval werkrolexemplaren, online komen, worden er reserveringen voor leases geplaatst. Later verschuift de werklast tussen de knooppunten wanneer elk knooppunt probeert om meer leases te verkrijgen.

![Gebeurtenisprocessorhost](./media/event-hubs-programming-guide/IC759863.png)

In de loop van de tijd komt er een evenwicht tot stand. Dankzij deze dynamische mogelijkheid kan er op consumers automatisch schalen op basis van CPU worden toegepast, zowel voor omhoog als voor omlaag schalen. Omdat Event Hubs niet over een direct concept voor berichtentelling beschikt, is gemiddelde CPU-gebruik vaak de beste manier om te meten back-end- of de consument schaal. Als uitgevers meer gebeurtenissen publiceren dan consumers kunnen verwerken, kan de toename van het CPU-gebruik voor consumers worden gebruikt om het aantal werkrolexemplaren automatisch te schalen.

Met de klasse [EventProcessorHost][] wordt ook een op Azure Storage gebaseerd mechanisme van controlepunten geïmplementeerd. Hiermee wordt de offset opgeslagen op basis van partitie, zodat elke consumer kan bepalen wat het laatste controlepunt van de vorige consumer was. Terwijl partities via leases schakelen tussen knooppunten, maakt dit synchronisatiemechanisme de verschuiving van werklasten mogelijk.

## <a name="publisher-revocation"></a>Uitgever intrekken
Naast de geavanceerde functies van de runtime van [EventProcessorHost][], Event Hubs kunt uitgever intrekken om te zorgen dat specifieke uitgevers geen gebeurtenis kunnen verzenden naar een event hub. Deze functies zijn handig als token van een uitgever is aangetast, of een software-update wordt veroorzaakt door niet naar behoren functioneert. In deze gevallen kan de uitgever-id, die onderdeel is van het bijbehorende SAS-token, worden geblokkeerd voor het uitgeven van gebeurtenissen.

Zie het voorbeeld [Op grote schaal veilig publiceren met Event Hubs](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab) voor meer informatie over het intrekken van uitgevers en over het als uitgever verzenden naar Event Hubs.

## <a name="next-steps"></a>Volgende stappen
Volg deze koppelingen voor meer informatie over Event Hubs-scenario‘s:

* [Event Hubs-API-overzicht](event-hubs-api-overview.md)
* [Wat is Event Hubs](event-hubs-what-is-event-hubs.md)
* [Beschikbaarheid en consistentie in Event Hubs](event-hubs-availability-and-consistency.md)
* [API-verwijzing voor de gebeurtenisprocessorhost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)

[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[EventHubClient]: /dotnet/api/microsoft.servicebus.messaging.eventhubclient
[EventData]: /dotnet/api/microsoft.servicebus.messaging.eventdata
[CreateEventHubIfNotExists]: /dotnet/api/microsoft.servicebus.namespacemanager.createeventhubifnotexists
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.eventdata#Microsoft_ServiceBus_Messaging_EventData_PartitionKey
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
