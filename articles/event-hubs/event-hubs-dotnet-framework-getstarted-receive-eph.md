---
title: Gebeurtenissen ontvangen van Azure Event Hubs met behulp van het .NET Framework | Microsoft Docs
description: Volg deze zelfstudie om gebeurtenissen te ontvangen van Azure Event Hubs middels het .NET Framework.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2018
ms.author: shvija
ms.openlocfilehash: cb1d26082fe4fbbd14b2b77f54d1bc7697b3538d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227957"
---
# <a name="receive-events-from-azure-event-hubs-using-the-net-framework"></a>Gebeurtenissen ontvangen van Azure Event Hubs met behulp van het .NET Framework

## <a name="introduction"></a>Inleiding

Event Hubs is een service die grote hoeveelheden gebeurtenisgegevens (telemetrie) van verbonden apparaten en toepassingen verwerkt. Nadat u gegevens in Event Hubs hebt verzameld, kunt u de gegevens opslaan met behulp van een opslagcluster of transformeren met een provider van realtime-analyses. Deze functie voor grootschalige gebeurtenisverzameling en -verwerking is een belangrijk onderdeel van de architectuur van moderne toepassingen, met inbegrip van het Internet der dingen (IoT). Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

Deze zelfstudie laat zien hoe u een .NET Framework-consoletoepassing die berichten van een event hub met behulp van ontvangt de [Event Processor Host](event-hubs-event-processor-host.md). De [EventProcessorHost](event-hubs-event-processor-host.md) is een .NET-klasse die het ontvangen van gebeurtenissen van Event Hubs vereenvoudigt door permanente controlepunten en parallelle ontvangst van deze Event Hubs te beheren. Met de EventProcessorHost kunt u gebeurtenissen splitsen over meerdere ontvangers, zelfs als deze worden gehost in verschillende knooppunten. In dit voorbeeld wordt het gebruik van de EventProcessorHost gedemonstreerd voor één ontvanger. De [Uitgeschaalde gebeurtenisverwerking] [ Scale out Event Processing with Event Hubs] voorbeeld ziet u hoe u de Event Processor Host met meerdere ontvangers.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* [Microsoft Visual Studio 2017 of hoger](https://visualstudio.com).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken
In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Volg de procedure in [dit artikel](event-hubs-create.md) om een naamruimte en een Event Hub te maken en ga daarna verder met de volgende stappen in deze zelfstudie.

[!INCLUDE [event-hubs-create-storage](../../includes/event-hubs-create-storage.md)]

## <a name="create-a-console-application"></a>Een consoletoepassing maken

Maak in Visual Studio een nieuw Visual C# bureaublad-app-project met behulp van de projectsjabloon**Consoletoepassing**. Noem het project **Ontvanger**.
   
![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)

## <a name="add-the-event-hubs-nuget-package"></a>Het Event Hubs NuGet-pakket toevoegen

1. Klik in Solution Explorer met de rechtermuisknop op het project **Ontvanger** en klik op **NuGet-pakketten beheren voor oplossing**.
2. Klik op het tabblad **Bladeren** en zoek vervolgens naar `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Klik op **Installeren** en accepteer de gebruiksvoorwaarden.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Door Visual Studio wordt een verwijzing naar het [ NuGet-pakket Azure Service Bus Event Hub - EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), inclusief alle bijbehorende afhankelijkheden, gedownload, geïnstalleerd en toegevoegd.

## <a name="implement-the-ieventprocessor-interface"></a>De IEventProcessor-interface implementeren

1. Klik met de rechtermuisknop op het project **Ontvanger**, klik op **Toevoegen** en op **Klasse**. Noem de nieuwe klasse **SimpleEventProcessor** en klik op **Toevoegen** om de klasse te maken.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
2. Voeg de volgende instructies toe aan het begin van het bestand SimpleEventProcessor.cs:
    
      ```csharp
      using Microsoft.ServiceBus.Messaging;
      using System.Diagnostics;
      ```
    
3. Vervang de volgende code voor de hoofdtekst van de klasse:
    
      ```csharp
      class SimpleEventProcessor : IEventProcessor
      {
        Stopwatch checkpointStopWatch;
        
        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }
        
        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }
        
        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
        
                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }
        
            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
      }
      ```
    
      Deze klasse wordt aangeroepen door de **EventProcessorHost** om gebeurtenissen te verwerken die worden ontvangen van de Event Hub. De klasse `SimpleEventProcessor` gebruikt een stopwatch om periodiek de controlepuntmethode voor de context **EventProcessorHost** aan te roepen. Op deze manier gaat er nooit meer werk verloren dan in vijf minuten kan worden verwerkt, als de ontvanger opnieuw wordt opgestart.

## <a name="update-the-main-method-to-use-simpleeventprocessor"></a>De Main-methode bijwerken voor gebruik van SimpleEventProcessor

1. Voeg in de klasse **Program** de volgende `using`-instructie toe aan het begin van het bestand:
    
      ```csharp
      using Microsoft.ServiceBus.Messaging;
      ```
    
2. Vervang de `Main` methode in de `Program` klasse met de volgende code wordt vervangen door de naam van de event hub en de verbindingsreeks op naamruimteniveau die u eerder hebt opgeslagen, en de storage-account en de sleutel die u in de vorige secties hebt gekopieerd. 
    
      ```csharp
      static void Main(string[] args)
      {
        string eventHubConnectionString = "{Event Hubs namespace connection string}";
        string eventHubName = "{Event Hub name}";
        string storageAccountName = "{storage account name}";
        string storageAccountKey = "{storage account key}";
        string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
        
        string eventProcessorHostName = Guid.NewGuid().ToString();
        EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
        Console.WriteLine("Registering EventProcessor...");
        var options = new EventProcessorOptions();
        options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
        eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
        
        Console.WriteLine("Receiving. Press enter key to stop worker.");
        Console.ReadLine();
        eventProcessorHost.UnregisterEventProcessorAsync().Wait();
      }
      ```
    
3. Voer het programma uit en controleer of er geen fouten zijn.
  
Gefeliciteerd! U hebt nu met behulp van de EventProcessorHost berichten ontvangen van een Event Hub.


> [!NOTE]
> In deze zelfstudie wordt één exemplaar van [EventProcessorHost](event-hubs-event-processor-host.md) gebruikt. Voor een betere doorvoer raden we u aan om meerdere exemplaren van [EventProcessorHost](event-hubs-event-processor-host.md) uit te voeren, zoals wordt geïllustreerd in het voorbeeld [Uitgeschaalde gebeurtenisverwerking](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3). In die gevallen werken de meerdere instanties automatisch samen om de ontvangen gebeurtenissen gelijkmatig te verdelen. 

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u .NET Framework-toepassing die berichten van een event hub ontvangen gemaakt. Zie voor meer informatie over het verzenden van gebeurtenissen naar een event hub met behulp van .NET Framework, [verzenden van gebeurtenissen van event hub - .NET Framework](event-hubs-dotnet-framework-getstarted-send.md).

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Event Hubs overview]: event-hubs-about.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]:../storage/common/storage-create-storage-account.md
[Event Processor Host]: event-hubs-event-processor-host.md
[Azure portal]: https://portal.azure.com
