---
title: Overzicht van de standaard .NET API's van Azure Event Hubs | Microsoft Docs
description: Standard API .NET-overzicht
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a173f8e4-556c-42b8-b856-838189f7e636
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 855f6e7f401621d7f923d68215ca880c05d38629
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="event-hubs-net-standard-api-overview"></a>Overzicht van Event Hubs .NET Standard API

In dit artikel ziet u een aantal van de sleutel Event Hubs .NET standaard client-API's. Er zijn twee standaard .NET clientbibliotheken:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs): alle bewerkingen voor basic-runtime biedt.
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor): bevat aanvullende functionaliteit die wordt bijgehouden hoeveel verwerkte gebeurtenissen en is de eenvoudigste manier om te lezen van een event hub.

## <a name="event-hubs-client"></a>Event Hubs-client

[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) is het primaire object dat u wilt verzenden van gebeurtenissen, maakt u ontvangers en runtime-gegevens worden opgehaald. Deze client is gekoppeld aan een specifieke event hub en maakt een nieuwe verbinding met het Event Hubs-eindpunt.

### <a name="create-an-event-hubs-client"></a>Een Event Hubs-client maken

Een [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) object is gemaakt vanuit een verbindingsreeks. De eenvoudigste manier om het instantiëren van een nieuwe client wordt weergegeven in het volgende voorbeeld:

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("{Event Hubs connection string}");
```

De verbindingsreeks programmatisch bewerken, kunt u de [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) klasse en de verbindingsreeks doorgeven als een parameter voor [EventHubClient.CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("{Event Hubs connection string}")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a>Gebeurtenissen verzenden

Voor het verzenden van gebeurtenissen naar een event hub, gebruiken de [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) klasse. De inhoud moet een `byte` matrix, of een `byte` segment van de matrix.

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a>Gebeurtenissen ontvangen

De aanbevolen manier om het ontvangen van gebeurtenissen van Event Hubs is met behulp van de [Event Processor Host](#event-processor-host-apis), waarmee u functionaliteit voor het automatisch bijhouden van de event hub offset en partitie gegevens. Er zijn echter bepaalde situaties waarin u gebruiken de flexibiliteit van de kernbibliotheek van Event Hubs wilt mogelijk voor het ontvangen van gebeurtenissen.

#### <a name="create-a-receiver"></a>Een ontvanger maken

Ontvangers zijn gekoppeld aan specifieke partities, dus kunnen alle gebeurtenissen in een event hub worden ontvangen, moet u meerdere exemplaren. Het is raadzaam om op te halen van de partitiegegevens via programmacode, in plaats van hard-coding van de partitie-id. Als u wilt doen, kunt u de [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) methode.

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

Omdat gebeurtenissen worden nooit verwijderd uit een event hub (en alleen verlopen), moet u het juiste beginpunt. Het volgende voorbeeld ziet u mogelijke combinaties:

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

Deze API's bieden tolerantie voor werkprocessen die mogelijk niet beschikbaar is, door partities over beschikbare werknemers verdeeld.

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

Hieronder volgt een Voorbeeldimplementatie van de [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).

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