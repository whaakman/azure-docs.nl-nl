---
title: Wat is Azure Event Hubs Event Processor Host en waarom gebruiken | Microsoft Docs
description: Overzicht en inleiding tot Azure Event Hubs Gebeurtenisprocessorhost
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: shvija
ms.openlocfilehash: 4907004f4bb88cf0fe9fb799cab236ebf98bba7a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132892"
---
# <a name="azure-event-hubs-event-processor-host-overview"></a>Overzicht van Azure Event Hubs Event Processor Host

Azure Event Hubs is een krachtige telemetrie opname-service die kan worden gebruikt om de stroom miljoenen gebeurtenissen tegen lage kosten. Dit artikel wordt beschreven hoe verbruiken opgenomen gebeurtenissen met de *Event Processor Host* (EPH); een intelligente-agent die het beheer van het plaatsen van controlepunten vereenvoudigt, leasing en parallelle gebeurtenislezers.  

De sleutel te schalen voor Event Hubs is het idee van de gepartitioneerde consumenten. In tegenstelling tot de [concurrerende consumenten](http://msdn.microsoft.com/en-us/library/dn568101.aspx) patroon, de gepartitioneerd gebruikspatroon kan grote schaal door het aantal conflicten knelpunt verwijderen en vergemakkelijking complete parallelle uitvoering.

## <a name="home-security-scenario"></a>Beveiliging voor thuis-scenario

Een voorbeeldscenario, kunt u beter een beveiliging voor thuis-bedrijf dat wordt bewaakt 100.000 thuis. Elke minuut ontvangt deze gegevens uit verschillende sensoren zoals een beweging detectie, deur/venster open sensor, glas break detectie, enz., in elke huis geïnstalleerd. Het bedrijf biedt een website voor inwoners voor het bewaken van de activiteit van de startpagina in bijna realtime.

Elke sensor duwt gegevens naar een event hub. De event hub is geconfigureerd met 16 partities. U moet een mechanisme waarmee u kunt deze gebeurtenissen lezen, consolideren ze (cumulatieve, filter, enz.) en de statistische functie naar een blob storage, die vervolgens wordt toegewezen aan een gebruiksvriendelijke webpagina dump aan de kant in beslag neemt.

## <a name="write-the-consumer-application"></a>Schrijven van de consumer-toepassing

Bij het ontwerpen van de consument in een gedistribueerde omgeving, moet het scenario omgaan met de volgende vereisten:

1. **Schaal:** meerdere gebruikers maken met elke consumer eigenaar worden van het lezen van een paar Event Hubs-partities.
2. **Verdelen:** vergroten of verkleinen van de consument dynamisch. Bijvoorbeeld, wanneer een nieuw sensortype (bijvoorbeeld: een detectie koolmonoxide) is toegevoegd aan elke thuis, verhoogt het aantal gebeurtenissen. In dat geval verhoogt de operator (human) het aantal exemplaren van de consumer. Vervolgens, in de groep van gebruikers opnieuw kunt verdelen van het aantal partities ze eigenaar zijn, de belasting te delen met de zojuist toegevoegde consumenten.
3. **Naadloze hervatten op fouten:** als een consument (**consumer A**) mislukt (bijvoorbeeld de virtuele machine die als host fungeert de consumer plotseling vastloopt), en andere gebruikers moet kunnen worden opgepikt de partities die eigendom zijn van **consumer A** en door te gaan. Ook het vervolg punt aangeroepen een *controlepunt* of *offset*, moeten op het punt waarop **consumer A** mislukt, of iets daarvoor.
4. **Gebeurtenissen gebruiken:** tijdens de vorige drie punten hebben betrekking op het beheer van de consument, moet er code voor het gebruiken van de gebeurtenissen en doe iets nuttig bij met het; bijvoorbeeld samenvatten en uploaden naar blob-opslag.

In plaats van uw eigen oplossing bouwen voor deze, Event Hubs biedt deze functionaliteit via de [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) interface en de [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) klasse.

## <a name="ieventprocessor-interface"></a>IEventProcessor-interface

Toepassingen implementeren eerst verbruikt de [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) interface, waardoor vier methoden heeft: [OpenAsync, CloseAsync ProcessErrorAsync en ProcessEventsAsnyc](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor?view=azure-dotnet#methods). Deze interface bevat de werkelijke code voor het verbruik van de gebeurtenissen die Event Hubs verzendt. De volgende code toont een eenvoudige implementatie:

```csharp
public class SimpleEventProcessor : IEventProcessor
{
    public Task CloseAsync(PartitionContext context, CloseReason reason)
    {
       Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
       return Task.CompletedTask;
    }

    public Task OpenAsync(PartitionContext context)
    {
       Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
       return Task.CompletedTask;
     }

    public Task ProcessErrorAsync(PartitionContext context, Exception error)
    {
       Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
       return Task.CompletedTask;
    }

    public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
       foreach (var eventData in messages)
       {
          var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
             Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
       }
       return context.CheckpointAsync();
    }
}
```

Vervolgens exemplaar maken van een [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) exemplaar. Afhankelijk van de overbelasting bij het maken van de [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) exemplaar in de constructor, de volgende parameters worden gebruikt:

- **Hostnaam:** de naam van elke consumer-exemplaar. Elk exemplaar van **EventProcessorHost** moet een unieke waarde voor deze variabele in een consumergroep hebben dus is het beter niet harde code deze waarde.
- **eventHubPath:** de naam van de event hub.
- **consumerGroupName:** gebruikmaakt van Event Hubs **$Default** zoals de naam van de consument standaardgroep, maar het is raadzaam een consumergroep voor uw specifieke aspect van de verwerking.
- **eventHubConnectionString:** de verbindingsreeks naar de event hub, die kan worden opgehaald uit de Azure-portal. Deze verbindingsreeks moet **luisteren** machtigingen voor de event hub.
- **storageConnectionString:** het storage-account gebruikt voor het beheer van de interne resource.

Ten slotte consumenten registreren de [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) -exemplaar met de Event Hubs-service. Registreren van Hiermee geeft u de Event Hubs-service kunnen verwachten dat de app consumer gebeurtenissen van enkele van de partities verbruikt en aan te roepen de [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) implementatiecode wanneer deze gebeurtenissen gebruiken voor pushmeldingen.

### <a name="example"></a>Voorbeeld

Stel bijvoorbeeld dat er 5 virtuele machines (VM's) zijn toegewezen aan het gebruiken van gebeurtenissen en een eenvoudige consoletoepassing in elke virtuele machine, die het werkelijke verbruik werkt. Vervolgens elke consoletoepassing maakt een [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) -instantie en registreert deze bij de Event Hubs-service.

In dit voorbeeldscenario Stel dat 16 partities worden toegewezen aan de 5 **EventProcessorHost** exemplaren. Sommige **EventProcessorHost** exemplaren kunnen een paar meer partities dan andere eigenaar. Voor elke partitie een **EventProcessorHost** exemplaar eigenaar is, maakt het een exemplaar van de `SimpleEventProcessor` klasse. Er zijn daarom 16 exemplaren van `SimpleEventProcessor` algehele met een toegewezen aan elke partitie.

De volgende lijst bevat een overzicht van dit voorbeeld:

- 16 event Hubs-partities.
- 5 VM's in elke VM 1 consumer app (bijvoorbeeld Consumer.exe).
- 5 EPH exemplaren geregistreerd, 1 in elke virtuele machine door Consumer.exe.
- 16 `SimpleEventProcessor` objecten die door de 5 EPH exemplaren gemaakt.
- 1 Consumer.exe app bevat mogelijk 4 `SimpleEventProcessor` objecten, omdat het 1 EPH-exemplaar kan eigenaar zijn van 4 partities.

## <a name="partition-ownership-tracking"></a>Partitie eigendom bijhouden

Eigendom van een partitie met een exemplaar EPH (of een consumer) wordt bijgehouden via de Azure Storage-account dat is opgegeven voor bijhouden. U kunt als volgt de tracering als een eenvoudige tabel visualiseren. De werkelijke implementatie kunt u zien door de blobs onder het opgegeven opslagaccount in:

| **De naam van de consumer** | **Partitie-ID** | **Hostnaam (eigenaar)** | **Lease (of eigendom) verkregen tijd** | **Offset in partitie (controlepunt)** |
| --- | --- | --- | --- | --- |
| Standaar$d | 0 | Consumer\_VM3 | 2018-04-15T01:23:45 | 156 |
| Standaar$d | 1 | Consumer\_VM4 | 2018-04-15T01:22:13 | 734 |
| Standaar$d | 2 | Consumer\_VM0 | 2018-04-15T01:22:56 | 122 |
| : |   |   |   |   |
| : |   |   |   |   |
| Standaar$d | 15 | Consumer\_VM3 | 2018-04-15T01:22:56 | 976 |

Elke host verkrijgt hier, eigendom van een partitie voor een bepaalde duur (de leaseduur). Als een host mislukt (virtuele machine wordt afgesloten), verloopt de lease. Andere hosts proberen op te halen van de eigenaar van de partitie en een van de hosts is geslaagd. Dit proces stelt de lease op de partitie met een nieuwe eigenaar. Op deze manier slechts één lezer tegelijk kan lezen van een bepaalde partitie binnen een consumergroep.

## <a name="receive-messages"></a>Berichten ontvangen

Elke aanroep van [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) biedt een verzameling van gebeurtenissen. Het is uw verantwoordelijkheid om deze gebeurtenissen te verwerken. Het is raadzaam dat u relatief snel handelingen; dat wil zeggen, kunt u doen als weinig verschil in verwerking mogelijk. Gebruik in plaats daarvan consumer-groepen. Als u nodig hebt om te schrijven naar de opslag en sommige routering, is het doorgaans beter gebruik van twee groepen voor consumenten en twee [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) implementaties die afzonderlijk uitvoeren.

Op een bepaald moment tijdens de verwerking, is het raadzaam om wat u hebt gelezen en voltooid bij te houden. Het bijhouden is kritiek als u lezen, opnieuw opstarten moet zodat u aan het begin van de stroom niet retourneren. [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) vereenvoudigt dit bijhouden met behulp van *controlepunten*. Een controlepunt is een locatie of offset voor een bepaalde partitie, binnen een bepaalde consumergroep, waarna u zijn ervan overtuigd dat u de berichten zijn verwerkt. U markeert een controlepunt in **EventProcessorHost** wordt bereikt door het aanroepen van de [CheckpointAsync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) methode op de [PartitionContext](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext) object. Deze bewerking wordt doorgaans uitgevoerd binnen de [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) methode, maar kan ook worden uitgevoerd in [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync).

## <a name="checkpointing"></a>Controlepunten plaatsen

De [CheckpointAsync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) methode heeft twee overloads: de eerste, zonder parameters, controlepunten voor de hoogste gebeurtenis verschuiving binnen de verzameling die is geretourneerd door [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync). Deze offset is ingeschakeld 'high-water'; wordt ervan uitgegaan dat u alle recente gebeurtenissen bij het aanroepen van deze zijn verwerkt. Als u deze methode op deze manier gebruikt, Let erop dat u naar verwachting aanroepen nadat uw andere code voor de verwerking van de gebeurtenis heeft geretourneerd. De tweede overload-Hier geeft u een [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) exemplaar controlepunt. Deze methode kunt u een ander type watermerk controlepunt gebruiken. Met deze watermerk, kunt u een 'laag water' mark implementeren: de laagste u er zeker van gesequentieerde gebeurtenis is verwerkt. Deze overbelasting is om in te schakelen flexibiliteit bij het beheer van de offset opgegeven.

Wanneer het controlepunt wordt uitgevoerd, een JSON-bestand met partitie-specifieke informatie (met name de offset) wordt geschreven naar het storage-account opgegeven in de constructor voor het [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Dit bestand wordt voortdurend bijgewerkt. Het is essentieel rekening houden met het plaatsen van controlepunten in de context: het is niet verstandig controlepunt elk bericht. Het storage-account gebruikt voor het plaatsen van controlepunten waarschijnlijk zou deze laadbewerking niet verwerken, maar belangrijker plaatsen van controlepunten elke gebeurtenis is indicatief voor een wachtrij messaging-patroon waarvoor een Service Bus-wachtrij een betere optie dan een event hub mogelijk zijn. De gedachte achter Event Hubs is ophalen van 'ten minste eenmaal' levering op grote schaal. Doordat uw idempotent downstream systemen, het is gemakkelijk te herstellen van fouten of opnieuw wordt gestart die resulteren in dezelfde gebeurtenissen meerdere keren worden ontvangen.

## <a name="thread-safety-and-processor-instances"></a>Thread veiligheid en de processortijd exemplaren

Standaard [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) thread veilig is en functioneert in een synchrone manier ten opzichte van het exemplaar van [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor). Wanneer gebeurtenissen voor een partitie plaatsvinden [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) is aangeroepen voor de **IEventProcessor** exemplaar voor die partitie en verdere aanroepen naar blokkeert **ProcessEventsAsync**voor de partitie. Volgende berichten en aanroepen naar **ProcessEventsAsync** wachtrij achter de schermen wanneer het bericht pomp blijft op de achtergrond van andere threads uitgevoerd. Deze thread veiligheid hoeven thread-veilige verzamelingen en verhoogt de prestaties aanzienlijk.

## <a name="shut-down-gracefully"></a>Afsluiten

Ten slotte [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) kunt u een geldige afsluiting van alle partitie lezers en altijd moet worden aangeroepen wanneer een exemplaar van afgesloten [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Niet doet kan leiden tot vertragingen bij het starten van andere exemplaren van **EventProcessorHost** als gevolg van de lease is verlopen en -epoche conflicten. Epoche management wordt besproken in dit in detail [blogbericht](https://blogs.msdn.microsoft.com/gyan/2014/09/02/event-hubs-receiver-epoch/)

## <a name="lease-management"></a>Beheer van de lease

Zoals eerder vermeld, de tabel bijhouden vereenvoudigt de aard van het automatisch schalen van [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync). Als een instantie van **EventProcessorHost** wordt gestart, deze leases zo veel mogelijk verkrijgt en begint met het lezen van gebeurtenissen. Als de leases bijna is verlopen, **EventProcessorHost** probeert te vernieuwen door het plaatsen van een reservering. Als de lease beschikbaar voor vernieuwing is, de processor lezen blijft, maar als dit niet het geval is, de lezer is gesloten en [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync) wordt aangeroepen. **CloseAsync** is een goed moment om uit te voeren definitieve opschoontaken voor deze partitie.

**EventProcessorHost** bevat een [PartitionManagerOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.partitionmanageroptions) eigenschap. Deze eigenschap kunt controle over het beheer van de lease. Deze opties instellen voordat u registreert uw [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) implementatie.

## <a name="control-event-processor-host-options"></a>Opties van het Event Processor Host

Bovendien een overload van [RegisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) duurt een [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) -object als parameter. Met deze parameter kunt u bepalen de werking van [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) zelf. [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions) definieert vier eigenschappen en één gebeurtenis:

- [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize): de maximale grootte van de verzameling die u wilt ontvangen in een aanroep van [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync). Deze grootte is niet het minimum, alleen de maximale grootte. Als er minder berichten te ontvangen, **ProcessEventsAsync** wordt uitgevoerd met alle opties die beschikbaar zijn.
- [PrefetchCount](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.prefetchcount): een waarde die wordt gebruikt door het onderliggende AMQP-kanaal om te bepalen van de bovengrens van het aantal berichten voor de client ontvangt. Deze waarde moet groter zijn dan of gelijk zijn aan [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize).
- [InvokeProcessorAfterReceiveTimeout](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.invokeprocessorafterreceivetimeout): als deze parameter **true**, [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) wordt aangeroepen wanneer de onderliggende aanroep voor het ontvangen van gebeurtenissen op een andere partitie een optreedt time-out. Deze methode is handig voor het uitvoeren van acties op basis van tijd tijdens perioden van inactiviteit op de partitie.
- [InitialOffsetProvider](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider): Hiermee kunt u een functie aanwijzer of lambda-expressie moet worden ingesteld die wordt aangeroepen om de eerste offset als een lezer begint het lezen van een partitie. Zonder deze offset de lezer wordt gestart op de oudste gebeurtenis, tenzij een JSON-bestand met een offset al is opgeslagen in het opslagaccount dat is doorgegeven aan de [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) constructor. Deze methode is handig als u wilt wijzigen van het gedrag van het opstarten van de lezer. Wanneer deze methode wordt opgeroepen, bevat de objectparameter de partitie-ID waarvoor de lezer wordt gestart.
- [ExceptionReceivedEventArgs](/dotnet/api/microsoft.azure.eventhubs.processor.exceptionreceivedeventargs): Hiermee kunt u ontvangen van eventuele onderliggende uitzonderingen die optreden in [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Als dingen niet werkt zoals verwacht, is deze gebeurtenis een goede plaats om te bekijken.

## <a name="next-steps"></a>Volgende stappen

Nu u bekend bent met het Event Processor Host, gaat u naar de volgende artikelen voor meer informatie over Event Hubs:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Programmeerhandleiding voor Event Hubs](event-hubs-programming-guide.md)
* [Beschikbaarheid en consistentie in Event Hubs](event-hubs-availability-and-consistency.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Event Hubs-voorbeelden op GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)