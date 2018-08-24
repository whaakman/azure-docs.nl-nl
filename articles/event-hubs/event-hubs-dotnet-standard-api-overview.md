---
title: Overzicht van de Azure Eventhubs standaard .NET-API's | Microsoft Docs
description: Standaard .NET-API-overzicht
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 08/13/2018
ms.author: shvija
ms.openlocfilehash: 9b952bd96828c4f2c140cb2d75cecb9379895a63
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746640"
---
# <a name="event-hubs-net-standard-api-overview"></a>Overzicht van Event Hubs .NET Standard-API

In dit artikel vindt u een samenvatting van de sleutel Azure Event Hubs [.NET Standard client-API's](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/). Er zijn momenteel twee .NET Standard-clientbibliotheken voor Event Hubs:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs): biedt alle basic runtime-bewerkingen.
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor): extra functionaliteit waarmee het bijhouden van de verwerkte gebeurtenissen, en is de eenvoudigste manier om te lezen van een event hub wordt toegevoegd.

## <a name="event-hubs-client"></a>Event Hubs-client

[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) is het primaire object dat u wilt verzenden van gebeurtenissen, maakt u ontvangers en runtime-gegevens ophalen. Deze client is gekoppeld aan een specifieke event hub, en maakt een nieuwe verbinding met het eindpunt van de Event Hubs.

### <a name="create-an-event-hubs-client"></a>Een Event Hubs-client maken

Een [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) object is gemaakt op basis van een verbindingsreeks. De eenvoudigste manier om het instantiëren van een nieuwe client wordt weergegeven in het volgende voorbeeld:

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("Event Hubs connection string");
```

Als u wilt de verbindingsreeks programmatisch bewerken, kunt u de [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) klasse en de verbindingsreeks doorgeven als een parameter voor [EventHubClient.CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("Event Hubs connection string")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a>Gebeurtenissen verzenden

Gebruiken voor het verzenden van gebeurtenissen naar een event hub, de [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) klasse. De hoofdtekst moet een `byte` matrix of een `byte` segment van de matrix.

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a>Gebeurtenissen ontvangen

De aanbevolen manier om u te ontvangen van gebeurtenissen van Event Hubs gebruikt het [Event Processor Host](#event-processor-host-apis), waarmee u functionaliteit voor het automatisch bijhouden van de event hub offset en partitie-informatie. Er zijn echter bepaalde situaties waarin kunt u de flexibiliteit van de kernbibliotheek van Event Hubs gebruiken om u te ontvangen van gebeurtenissen.

#### <a name="create-a-receiver"></a>Een ontvanger maken

Ontvangers zijn gekoppeld aan specifieke partities, dus als u wilt ontvangen van alle gebeurtenissen in een event hub, moet u meerdere exemplaren maken. Het is raadzaam voor de partitie-informatie via de programmacode, in plaats van hard-coding de partitie-id. Als u wilt doen, kunt u de [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) methode.

```csharp
// Create a list to keep track of the receivers
var receivers = new List<PartitionReceiver>();
// Use the eventHubClient created above to get the runtime information
var runTimeInformation = await eventHubClient.GetRuntimeInformationAsync();
// Loop over the resulting partition IDs
foreach (var partitionId in runTimeInformation.PartitionIds)
{
    // Create the receiver
    var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);
    // Add the receiver to the list
    receivers.Add(receiver);
}
```

Omdat de gebeurtenissen worden nooit verwijderd van een event hub (en alleen verlopen), moet u het juiste beginpunt. Het volgende voorbeeld ziet u mogelijke combinaties:

```csharp
// partitionId is assumed to come from GetRuntimeInformationAsync()

// Using the constant PartitionReceiver.EndOfStream only receives all messages from this point forward.
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);

// All messages available
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, "-1");

// From one day ago
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, DateTime.Now.AddDays(-1));
```

#### <a name="consume-an-event"></a>Een gebeurtenis gebruiken

```csharp
// Receive a maximum of 100 messages in this call to ReceiveAsync
var ehEvents = await receiver.ReceiveAsync(100);
// ReceiveAsync can return null if there are no messages
if (ehEvents != null)
{
    // Since ReceiveAsync can return more than a single event you will need a loop to process
    foreach (var ehEvent in ehEvents)
    {
        // Decode the byte array segment
        var message = UnicodeEncoding.UTF8.GetString(ehEvent.Body.Array);
        // Load the custom property that we set in the send example
        var customType = ehEvent.Properties["Type"];
        // Implement processing logic here
    }
}       
```

## <a name="event-processor-host-apis"></a>Event Processor Host API 's

Deze API's bieden tolerantie tegen werkprocessen die mogelijk niet beschikbaar is, door de partities over beschikbare werkrollen verdeeld:

```csharp
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.

// Read these connection strings from a secure location
var ehConnectionString = "{Event Hubs connection string}";
var ehEntityPath = "{event hub path/name}";
var storageConnectionString = "{Storage connection string}";
var storageContainerName = "{Storage account container name}";

var eventProcessorHost = new EventProcessorHost(
    ehEntityPath,
    PartitionReceiver.DefaultConsumerGroupName,
    ehConnectionString,
    storageConnectionString,
    storageContainerName);

// Start/register an EventProcessorHost
await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

// Disposes the Event Processor Host
await eventProcessorHost.UnregisterEventProcessorAsync();
```

Hieronder volgt een Voorbeeldimplementatie van de [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) interface:

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

## <a name="next-steps"></a>Volgende stappen

Volg deze koppelingen voor meer informatie over Event Hubs-scenario‘s:

* [Wat is Azure Event Hubs?](event-hubs-what-is-event-hubs.md)
* [Beschikbare Event Hubs-API 's](event-hubs-api-overview.md)

De .NET API-verwijzingen zijn hier:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs)
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor)