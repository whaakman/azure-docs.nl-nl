---
title: Gebeurtenissen ontvangen met .NET Core - Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat een overzicht voor het maken van een .NET Core-toepassing die berichten ontvangt met de Event Processor Host.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: a3c4bc741cef60576bec17cd3257914132b72666
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56452622"
---
# <a name="get-started-receiving-messages-with-the-event-processor-host-in-net-core"></a>Aan de slag met het ontvangen van berichten met de Event Processor Host in .NET Core
Event Hubs is een service die grote hoeveelheden gebeurtenisgegevens (telemetrie) van verbonden apparaten en toepassingen verwerkt. Nadat u gegevens in Event Hubs hebt verzameld, kunt u de gegevens opslaan met behulp van een opslagcluster of transformeren met een provider van realtime-analyses. Deze functie voor grootschalige gebeurtenisverzameling en -verwerking is een belangrijk onderdeel van de architectuur van moderne toepassingen, met inbegrip van het Internet der dingen (IoT). Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

In deze zelfstudie ziet u hoe u een .NET Core-consoletoepassing schrijft die met de [EventProcessorHost](event-hubs-event-processor-host.md) berichten ontvangt van een Event Hub. De [EventProcessorHost](event-hubs-event-processor-host.md) is een .NET-klasse die het ontvangen van gebeurtenissen van Event Hubs vereenvoudigt door permanente controlepunten en parallelle ontvangst van deze Event Hubs te beheren. Met de EventProcessorHost kunt u gebeurtenissen splitsen over meerdere ontvangers, zelfs als deze worden gehost in verschillende knooppunten. In dit voorbeeld wordt het gebruik van de EventProcessorHost gedemonstreerd voor één ontvanger. In het voorbeeld [Uitgeschaalde gebeurtenisverwerking](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) ziet u hoe u de EventProcessorHost gebruikt met meerdere ontvangers.

> [!NOTE]
> U kunt deze snelstart downloaden als een voorbeeld van de [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver), de tekenreeksen `EventHubConnectionString` en `EventHubName`, `StorageAccountName`, `StorageAccountKey` en `StorageContainerName` vervangen door uw Event Hub-waarden en deze uitvoeren. U kunt ook de stappen in deze zelfstudie volgen om uw eigen oplossing te maken.

## <a name="prerequisites"></a>Vereisten
* [Microsoft Visual Studio 2015 of 2017](https://www.visualstudio.com). In de voorbeelden in deze zelfstudie wordt gebruikgemaakt van Visual Studio 2017, maar Visual Studio 2015 wordt ook ondersteund.
* [.NET core Visual Studio 2015- of 2017-hulpprogramma's](https://www.microsoft.com/net/core).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken
In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Volg de procedure in [dit artikel](event-hubs-create.md) om een naamruimte en een Event Hub te maken en ga daarna verder met de volgende stappen in deze zelfstudie.

[!INCLUDE [event-hubs-create-storage](../../includes/event-hubs-create-storage.md)]

## <a name="create-a-console-application"></a>Een consoletoepassing maken

Start Visual Studio. Klik in het menu **File** op **New** en klik vervolgens op **Project**. Maak een .NET Core-consoletoepassing.

![Nieuw project][2]

## <a name="add-the-event-hubs-nuget-package"></a>Het Event Hubs NuGet-pakket toevoegen

Voeg de NuGet-pakketten voor [ **Microsoft.Azure.EventHubs** ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) en [ **Microsoft.Azure.EventHubs.Processor** ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard-bibliotheek met de volgende stappen aan uw project toe: 

1. Klik met de rechtermuisknop op het nieuwe project en selecteer **NuGet-pakketten beheren**.
2. Klik op het tabblad **Bladeren**, zoek naar **Microsoft.Azure.EventHubs** en selecteer het pakket **Microsoft.Azure.EventHubs**. Klik op **Installeren** om de installatie te voltooien en sluit vervolgens dit dialoogvenster.
3. Herhaal stap 1 en 2 en installeer het **Microsoft.Azure.EventHubs.Processor**-pakket.

## <a name="implement-the-ieventprocessor-interface"></a>De IEventProcessor-interface implementeren

1. Klik in Solution Explorer met de rechtermuisknop op het project, klik op **Toevoegen** en klik vervolgens op **Klasse**. Noem de nieuwe klasse **SimpleEventProcessor**.

2. Open het bestand SimpleEventProcessor.cs en voeg de volgende `using`-instructies toe aan het begin van het bestand.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. Implementeer de `IEventProcessor`-interface. Vervang de volledige inhoud van de `SimpleEventProcessor`-klasse door de volgende code:

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

## <a name="update-the-main-method-to-use-simpleeventprocessor"></a>De Main-methode bijwerken voor gebruik van SimpleEventProcessor

1. Voeg aan het begin van het bestand Program.cs de volgende `using`-instructies toe.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. Voeg constanten toe aan de `Program`-klasse voor de verbindingsreeks van de event hub, naam van de event hub, naam van opslagaccountcontainer, opslagaccountnaam en opslagaccountsleutel. Voeg de volgende code toe, waarbij u de tijdelijke aanduidingen vervangt door de bijbehorende waarden:

    ```csharp
    private const string EventHubConnectionString = "{Event Hubs connection string}";
    private const string EventHubName = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. Voeg als volgt een nieuwe methode met de naam `MainAsync` toe aan de klasse `Program`:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        Console.WriteLine("Registering EventProcessor...");

        var eventProcessorHost = new EventProcessorHost(
            EventHubName,
            PartitionReceiver.DefaultConsumerGroupName,
            EventHubConnectionString,
            StorageConnectionString,
            StorageContainerName);

        // Registers the Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER to stop worker.");
        Console.ReadLine();

        // Disposes of the Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. Voeg de volgende coderegel toe aan de methode `Main`:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    Zo zou het bestand Program.cs er moeten uitzien:

    ```csharp
    namespace SampleEphReceiver
    {

        public class Program
        {
            private const string EventHubConnectionString = "{Event Hubs connection string}";
            private const string EventHubName = "{Event Hub path/name}";
            private const string StorageContainerName = "{Storage account container name}";
            private const string StorageAccountName = "{Storage account name}";
            private const string StorageAccountKey = "{Storage account key}";

            private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                Console.WriteLine("Registering EventProcessor...");

                var eventProcessorHost = new EventProcessorHost(
                    EventHubName,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EventHubConnectionString,
                    StorageConnectionString,
                    StorageContainerName);

                // Registers the Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER to stop worker.");
                Console.ReadLine();

                // Disposes of the Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. Voer het programma uit en controleer of er geen fouten zijn.

Gefeliciteerd. U hebt nu met behulp van de EventProcessorHost berichten ontvangen van een Event Hub.

> [!NOTE]
> In deze zelfstudie wordt één exemplaar van [EventProcessorHost](event-hubs-event-processor-host.md) gebruikt. Voor een betere doorvoer raden we u aan om meerdere exemplaren van [EventProcessorHost](event-hubs-event-processor-host.md) uit te voeren, zoals wordt geïllustreerd in het voorbeeld [Uitgeschaalde gebeurtenisverwerking](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3). In die gevallen werken de meerdere instanties automatisch samen om de ontvangen gebeurtenissen gelijkmatig te verdelen. 

## <a name="next-steps"></a>Volgende stappen
In deze quickstart hebt u een .NET Core-toepassing gemaakt die berichten heeft ontvangen van een Event Hub. Zie [Gebeurtenissen verzenden vanuit Event Hub - .NET Core](event-hubs-dotnet-standard-getstarted-send.md) voor meer informatie over het verzenden van gebeurtenissen naar een Event Hub met behulp van .NET Core.


[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcorercv.png
