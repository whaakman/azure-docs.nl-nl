---
title: Gebeurtenissen ontvangen met Eventprocessorhost - Azure Event Hubs | Microsoft Docs
description: Dit artikel beschrijft de Event Processor Host in Azure Event Hubs, dat het beheer van het plaatsen van controlepunten vereenvoudigt, overdracht en bewaartermijn parallel lezen van gebeurtenissen.
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
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 2b4fcb42c913149f8caf05a72fb089586ee21e2a
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106117"
---
# <a name="receive-events-from-azure-event-hubs-using-event-processor-host"></a>Gebeurtenissen ontvangen van Azure Event Hubs met behulp van Event Processor Host

Azure Event Hubs is een krachtige service voor telemetrieopname die kan worden gebruikt voor streaming van miljoenen gebeurtenissen tegen lage kosten. In dit artikel wordt beschreven hoe u verbruiken opgenomen gebeurtenissen met behulp van de *Event Processor Host* (EPH); een intelligente consumeragent die het beheer van het plaatsen van controlepunten vereenvoudigt, overdracht en parallelle gebeurtenislezers.  

De sleutel worden geschaald voor Event Hubs is het idee van de gepartitioneerde consumenten. In tegenstelling tot de [concurrerende consumenten](https://msdn.microsoft.com/library/dn568101.aspx) patroon, de basis van gepartitioneerd gebruik patroon kan grote schaal door te verwijderen van het knelpunt conflicten en end-to-end parallelle uitvoering te vergemakkelijken.

## <a name="home-security-scenario"></a>Beveiliging voor thuis-scenario

Een voorbeeldscenario, kunt u beter een beveiliging voor thuis-bedrijf dat 100.000 huisadressen bewaakt. Elke minuut, deze gegevens worden opgehaald uit diverse sensoren, zoals een bewegingsherkenning, of venster van de klep open sensor, glas break detector, enz., geïnstalleerd bij elke thuis. Het bedrijf biedt een website voor inwoners van de voor het bewaken van de activiteit van de startpagina in bijna realtime.

Elke sensor pushes gegevens naar een event hub. De event hub is geconfigureerd met 16 partities. Aan het einde in beslag nemen moet u een mechanisme dat u kunt deze gebeurtenissen worden gelezen, kunt u deze (filteren, samen te voegen enzovoort) en de statistische functie naar een blob storage, die vervolgens wordt toegewezen aan een gebruiksvriendelijke webpagina dump.

## <a name="write-the-consumer-application"></a>De consument toepassing schrijven

Bij het ontwerpen van de gebruiker in een gedistribueerde omgeving, moet het scenario ingang in de volgende vereisten:

1. **Schaal:** Meerdere consumenten worden gedistribueerd, met elke consument eigenaar van het lezen van een paar Event Hubs-partities te maken.
2. **Verdeel de belasting van:** Vergroten of verkleinen van de consumenten dynamisch. Bijvoorbeeld, wanneer een nieuw sensortype (bijvoorbeeld een koolmonoxide detector) wordt toegevoegd aan elke start, verhoogt het aantal gebeurtenissen. In dat geval wordt het aantal consumentexemplaren wordt verhoogd in de operator (een mens). Vervolgens, in de groep consumenten herverdelen van het aantal partities ze eigenaar zijn, voor het delen van de belasting met de zojuist toegevoegde consumenten.
3. **Naadloze hervatten op fouten:** Als een consument (**consument A**) mislukt (bijvoorbeeld de virtuele machine die als host fungeert de consument plotseling vastloopt), en vervolgens andere gebruikers moeten in staat om op te halen de partities die eigendom zijn **consument A** en door te gaan. Ook het vervolg-punt met de naam een *controlepunt* of *offset*, moet op het exacte punt waarop **consument A** is mislukt, of iets vóór die.
4. **Gebeurtenissen gebruiken:** Tijdens de vorige drie punten hebben betrekking op het beheer van de consumenten, moet er code te gebruiken van de gebeurtenissen en iets nuttig met bijvoorbeeld, aggregeren en te uploaden naar blob-opslag.

In plaats van het bouwen van uw eigen oplossing voor dit, Event Hubs biedt deze functionaliteit via de [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) interface en de [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) klasse.

## <a name="ieventprocessor-interface"></a>IEventProcessor-interface

Eerst, verbruikt toepassingen implementeren de [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) interface, met vier methoden: [OpenAsync, CloseAsync, ProcessErrorAsync en ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor?view=azure-dotnet#methods). Deze interface bevat de werkelijke code voor het gebruiken van de gebeurtenissen die Event Hubs wordt verzonden. De volgende code toont een eenvoudige implementatie:

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

Vervolgens exemplaar maken van een [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) exemplaar. Afhankelijk van de overbelasting bij het maken van de [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) exemplaar in de constructor de volgende parameters worden gebruikt:

- **Hostnaam:** de naam van elke consumentexemplaar. Elk exemplaar van **EventProcessorHost** moet een unieke waarde voor deze variabele binnen een consumentengroep beperkt, dus is het raadzaam om niet te deze waarde hard-code.
- **eventHubPath:** De naam van de event hub.
- **consumerGroupName:** Eventhubs gebruikt **$Default** zoals de naam van de standaardgroep voor consumenten, maar het is raadzaam om te maken van een consumentengroep voor uw specifieke aspect van de verwerking.
- **eventHubConnectionString:** De verbindingsreeks naar de event hub, die kan worden opgehaald uit de Azure-portal. Deze verbindingsreeks moet **luisteren** machtigingen voor de event hub.
- **StorageConnectionString:** Het opslagaccount dat wordt gebruikt voor het resourcebeheer van de interne.

Ten slotte consumenten registreren de [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) -exemplaar met de Event Hubs-service. Registreren van een klasse event processor met een exemplaar van EventProcessorHost start gebeurtenisverwerking. Registreren van geeft u de Event Hubs-service kunnen verwachten dat de app voor consumenten van gebeurtenissen van enkele van de partities verbruikt en om aan te roepen de [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) implementatiecode wanneer deze gebeurtenissen te verbruiken pushes. 


### <a name="example"></a>Voorbeeld

Een voorbeeld: Stel dat er 5 virtuele machines (VM's) zijn toegewezen aan het verbruiken van gebeurtenissen en een eenvoudige consoletoepassing in elke virtuele machine, die het werkelijke verbruik werkt. Elke consoletoepassing maakt dan een [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) exemplaar en geregistreerd bij de Event Hubs-service.

In dit voorbeeldscenario, laten we zeggen dat 16 partities worden toegewezen aan de 5 **EventProcessorHost** exemplaren. Sommige **EventProcessorHost** exemplaren mogelijk enkele meer partities dan andere eigenaar. Voor elke partitie een **EventProcessorHost** exemplaar eigenaar is, maakt het een exemplaar van de `SimpleEventProcessor` klasse. Er zijn daarom 16 exemplaren van `SimpleEventProcessor` algehele, met een toegewezen aan elke partitie.

De volgende lijst bevat een overzicht van dit voorbeeld:

- 16 event Hubs-partities.
- 5-VM's, 1 consumer-app (bijvoorbeeld Consumer.exe) in elke virtuele machine.
- 5 EPH-exemplaren geregistreerd, 1 in elke virtuele machine door Consumer.exe.
- 16 `SimpleEventProcessor` objecten die zijn gemaakt door de 5 EPH-exemplaren.
- 1 Consumer.exe app bevat mogelijk 4 `SimpleEventProcessor` objecten, omdat het 1 EPH-exemplaar is bijvoorbeeld eigenaar van 4 partities.

## <a name="partition-ownership-tracking"></a>Partitie eigendom bijhouden

Eigendom van een partitie op een EPH-exemplaar (of een gebruiker) wordt bijgehouden via de Azure Storage-account dat is opgegeven voor tracering. U kunt als volgt de tracering als een eenvoudige tabel visualiseren. U kunt de daadwerkelijke implementatie zien door te controleren van de blobs onder het opgegeven opslagaccount:

| **Naam van de groep consumenten** | **Partitie-ID** | **Hostnaam (eigenaar)** | **Lease (of eigenaar) verkregen tijd** | **Offset in partitie (controlepunt)** |
| --- | --- | --- | --- | --- |
| Standaar$d | 0 | Consumenten\_VM3 | 2018-04-15T01:23:45 | 156 |
| Standaar$d | 1 | Consumenten\_VM4 | 2018-04-15T01:22:13 | 734 |
| Standaar$d | 2 | Consumenten\_VM0 | 2018-04-15T01:22:56 | 122 |
| : |   |   |   |   |
| : |   |   |   |   |
| Standaar$d | 15 | Consumenten\_VM3 | 2018-04-15T01:22:56 | 976 |

Hier, krijgt elke host eigendom van een partitie gedurende een bepaalde periode (de duur van de lease). Als een host mislukt (virtuele machine wordt afgesloten), is de lease is verlopen. Andere hosts proberen op te halen van de eigenaar van de partitie en een van de hosts is geslaagd. Dit proces stelt de lease op de partitie met een nieuwe eigenaar. Op deze manier slechts één lezer tegelijk kunt lezen uit een bepaalde partitie binnen een consumergroep.

## <a name="receive-messages"></a>Berichten ontvangen

Elke aanroep van [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) voorziet in een verzameling van gebeurtenissen. Het is uw verantwoordelijkheid om deze gebeurtenissen te verwerken. Als u controleren of dat de processor host elk bericht ten minste één keer verwerkt wilt, moet u uw eigen behouden opnieuw te proberen code schrijven. Maar wees voorzichtig met berichten bevatten.

Het is raadzaam dat u relatief snel handelingen; dat wil zeggen, kunt u doen als weinig verwerking mogelijk. Gebruik in plaats daarvan consumentengroepen. Als u nodig hebt te schrijven naar de opslag en sommige routering, is het in het algemeen beter gebruik van twee consumentengroepen en twee [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) implementaties die afzonderlijk worden uitgevoerd.

Op een bepaald moment tijdens de verwerking, is het raadzaam om wat u hebt gelezen en voltooid bij te houden. Het bijhouden is van essentieel belang als u het lezen, opnieuw opstarten moet, zodat u niet teruggaan naar het begin van de stroom. [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) vereenvoudigt deze bijhouden met behulp van *controlepunten*. Een controlepunt is een locatie of een offset voor een bepaalde partitie, binnen een bepaalde consumergroep, op het moment waarop u bent ervan overtuigd dat u de berichten zijn verwerkt. Een controlepunt in markering **EventProcessorHost** wordt bereikt door het aanroepen van de [CheckpointAsync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) methode voor het [PartitionContext](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext) object. Deze bewerking wordt uitgevoerd binnen de [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) methode, maar kan ook worden uitgevoerd in [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync).

## <a name="checkpointing"></a>Controlepunten plaatsen

De [CheckpointAsync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) methode heeft twee overloads: de eerste, zonder parameters, controlepunten aan de hoogste verschuiving van de gebeurtenis binnen de verzameling die wordt geretourneerd door [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync). Deze offset is een teken 'high-water'; wordt ervan uitgegaan dat u alle recente gebeurtenissen wanneer u deze aanroepen zijn verwerkt. Als u deze methode op deze manier gebruikt, Let erop dat u wordt naar deze aanroepen nadat de verwerking van de gebeurtenis-code heeft geretourneerd. De tweede overbelasting kunt u opgeven een [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) exemplaar naar controlepunt. Deze methode kunt u met een ander type watermerk controlepunt. Met deze watermerk, kunt u een ' laag ' watermerk implementeren: de laagste gesequentieerde gebeurtenis u er zeker van zijn verwerkt. Deze overbelasting is opgegeven voor het inschakelen van flexibiliteit bij het beheer van offset.

Wanneer het controlepunt wordt uitgevoerd, een JSON-bestand met partitie-specifieke informatie (met name de offset), is geschreven naar het opslagaccount dat is opgegeven in de constructor voor het [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Dit bestand wordt voortdurend bijgewerkt. Het is essentieel om te overwegen het plaatsen van controlepunten in de context: het is niet verstandig controlepunt elk bericht. Het opslagaccount dat wordt gebruikt voor het plaatsen van controlepunten waarschijnlijk verwerkt niet deze belasting, maar nog belangrijker plaatsen van controlepunten voor elke gebeurtenis is indicatief voor een in de wachtrij abonnementsberichten waarvoor mogelijk een Service Bus-wachtrij een betere optie dan een event hub. Het idee achter Event Hubs, is dat u 'ten minste eenmaal' levering op grote schaal. Door uw downstream-systemen idempotent zijn, het is eenvoudig om fouten te herstellen of opnieuw wordt gestart die resulteren in dezelfde gebeurtenissen meerdere keren worden ontvangen.

## <a name="thread-safety-and-processor-instances"></a>Thread-veiligheid en processor-instanties

Standaard [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) thread-veilig is en functioneert in een synchrone wijze met betrekking tot het exemplaar van [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor). Bij het ontvangen van gebeurtenissen voor een partitie [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) wordt aangeroepen voor de **IEventProcessor** exemplaar voor die partitie en aanroepen van verdere blokkeert **ProcessEventsAsync**voor de partitie. Volgende berichten en aanroepen naar **ProcessEventsAsync** naarmate de bericht-pomp voor andere threads worden uitgevoerd op de achtergrond van de wachtrij achter de schermen. Deze thread veiligheid hoeven zich niet voor thread-veilige verzamelingen en worden de prestaties aanzienlijk verbeterd.

## <a name="shut-down-gracefully"></a>Zonder problemen worden afgesloten

Ten slotte [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) foutloos sluiten van alle partitie lezers kunnen en moet altijd worden aangeroepen wanneer een exemplaar van afgesloten [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Dit niet doet, kan dit vertraging veroorzaken bij het starten van andere exemplaren van **EventProcessorHost** vanwege de verlooptijd van de lease en epoche conflicten. Epoche management wordt beschreven in dit [blogbericht](https://blogs.msdn.microsoft.com/gyan/2014/09/02/event-hubs-receiver-epoch/)

## <a name="lease-management"></a>Lease-management
Registreren van een klasse event processor met een exemplaar van EventProcessorHost start gebeurtenisverwerking. De instantie van de host haalt leases op aantal partities van de Event Hub, mogelijk gegevens uit formulieren sommige vanuit andere exemplaren hosten op een manier die convergeert wel op een gelijkmatige verdeling van partities voor alle instanties van de host ligt. Voor elke partitie geleased de instantie van de host maakt een exemplaar van de klasse opgegeven event processor, klikt u vervolgens ontvangt gebeurtenissen van deze partitie en geeft deze door aan de event processor-instantie. Als u meer instanties zijn toegevoegd en meer leases afkomstig zijn, verdeelt EventProcessorHost uiteindelijk de belasting tussen alle gebruikers.

Zoals eerder uitgelegd, de tabel bijhouden vereenvoudigt de aard van de functie voor automatisch schalen van [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync). Als een exemplaar van **EventProcessorHost** wordt gestart, deze leases zo veel mogelijk verkrijgt en begint met het lezen van gebeurtenissen. Als de leases bijna is verlopen, **EventProcessorHost** probeert te vernieuwen door een reservering. Als de lease beschikbaar voor vernieuwen is, blijft lezen van de processor, maar als dat niet het geval is, de lezer is gesloten en [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync) wordt genoemd. **CloseAsync** is een goed moment om uit te voeren van laatste opschoontaken voor deze partitie.

**EventProcessorHost** bevat een [PartitionManagerOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.partitionmanageroptions) eigenschap. Deze eigenschap kunt u bepalen leasebeheer. Deze opties instellen voordat u registreert uw [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) implementatie.

## <a name="control-event-processor-host-options"></a>Opties voor broncodebeheer Event Processor Host

Bovendien een overbelasting van de [RegisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) duurt een [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) object als parameter. Gebruik deze parameter om te bepalen het gedrag van [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) zelf. [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions) definieert vier eigenschappen en één gebeurtenis:

- [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize): De maximale grootte van de verzameling die u wilt ontvangen in een aanroep van [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync). Deze grootte is niet het minimum, alleen de maximale grootte. Als er minder berichten wilt ontvangen, **ProcessEventsAsync** voert met zoveel als beschikbaar waren.
- [PrefetchCount](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.prefetchcount): Een waarde die door het onderliggende AMQP-kanaal wordt gebruikt om te bepalen van de bovengrens van het aantal berichten dat de client moet ontvangen. Deze waarde moet groter zijn dan of gelijk zijn aan [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize).
- [InvokeProcessorAfterReceiveTimeout](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.invokeprocessorafterreceivetimeout): Als deze parameter **waar**, [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) wordt aangeroepen wanneer de onderliggende aanroep voor het ontvangen van gebeurtenissen op een partitie een optreedt time-out. Deze methode is handig voor het uitvoeren van acties op basis van tijd tijdens perioden van inactiviteit van de partitie.
- [InitialOffsetProvider](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider): Hiermee kunt een functie aanwijzer of lambda-expressie moet worden ingesteld met de naam voor de eerste offset wanneer een lezer wordt begonnen met het lezen van een partitie. Zonder op te geven deze offset, de lezer wordt gestart op de oudste gebeurtenis, tenzij een JSON-bestand met een offset al is opgeslagen in de storage-account dat is doorgegeven aan de [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) constructor. Deze methode is handig als u wilt wijzigen van het gedrag van het opstarten van de lezer. Wanneer deze methode wordt aangeroepen, bevat de parameter van het object in de partitie-ID waarvoor de lezer wordt gestart.
- [ExceptionReceivedEventArgs](/dotnet/api/microsoft.azure.eventhubs.processor.exceptionreceivedeventargs): Hiermee kunt u ontvangen van eventuele onderliggende uitzonderingen die optreden in [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Als dingen niet werkt zoals verwacht, is deze gebeurtenis een goede plaats te bekijken.

## <a name="next-steps"></a>Volgende stappen

Nu u bekend bent met de Event Processor Host, Zie de volgende artikelen voor meer informatie over Event Hubs:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Programmeerhandleiding voor Event Hubs](event-hubs-programming-guide.md)
* [Beschikbaarheid en consistentie in Event Hubs](event-hubs-availability-and-consistency.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Event Hubs-voorbeelden op GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)
