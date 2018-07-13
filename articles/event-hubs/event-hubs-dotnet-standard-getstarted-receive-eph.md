---
title: Gebeurtenissen ontvangen van Azure Event Hubs met behulp van .NET Framework-Standard-bibliotheek | Microsoft Docs
description: Aan de slag met het ontvangen van berichten met de EventProcessorHost in .NET Standard
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2017
ms.author: sethm
ms.openlocfilehash: fd74405c8ca95ca1a5880ab26bf87705bde217de
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127530"
---
# <a name="get-started-receiving-messages-with-the-event-processor-host-in-net-standard"></a>Aan de slag met het ontvangen van berichten met de Event Processor Host in .NET Standard

> [!NOTE]
> Dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).

In deze zelfstudie ziet u hoe u een .NET Core-consoletoepassing schrijft die met de **EventProcessorHost** berichten ontvangt van een Event Hub. U kunt de [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver)-oplossing ongewijzigd uitvoeren en de tekenreeksen vervangen door uw event hub- en opslagaccountwaarden. Of u kunt de stappen in deze zelfstudie volgen om uw eigen oplossing te maken.

## <a name="prerequisites"></a>Vereisten

* [Microsoft Visual Studio 2015 of 2017](http://www.visualstudio.com). In de voorbeelden in deze zelfstudie wordt gebruikgemaakt van Visual Studio 2017, maar Visual Studio 2015 wordt ook ondersteund.
* [.NET core Visual Studio 2015- of 2017-hulpprogramma's](http://www.microsoft.com/net/core).
* Een Azure-abonnement.
* Een Azure Event Hubs-naamruimte en een Event Hub.
* Een Azure Storage-account.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken  

In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Volg de procedure in [dit artikel](event-hubs-create.md) en ga daarna verder met deze zelfstudie om een naamruimte en Event Hub te maken en ga daarna verder met de volgende stappen.  

## <a name="create-an-azure-storage-account"></a>Een Azure Storage-account maken  

1. Meld u aan bij [Azure Portal](https://portal.azure.com).  
2. Klik in het linkernavigatievenster van de portal achtereenvolgens op **Een nieuwe resource maken**, **Opslag** en **Opslagaccount**.  
3. Vul de velden in het opslagaccountvenster en klik vervolgens op **Maken**.

    ![Een opslagaccount maken][1]

4. Als u het bericht **Implementaties voltooid** ziet, klikt u op de naam van de nieuwe opslagaccount. Klik in het venster **Essentials** op **Blobs**. Wanneer het dialoogvenster **Blob service** wordt geopend, klikt u op **+ Container** bovenaan. Geef een naam op voor de container en sluit **Blob service**.  
5. Klik op **Toegangssleutels** in het venster links en kopieer de naam van de opslagcontainer, de opslagaccount en de waarde van **key1**. Plak deze waarde in Kladblok of een andere tijdelijke locatie.  

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

## <a name="write-a-main-console-method-that-uses-the-simpleeventprocessor-class-to-receive-messages"></a>Een hoofdconsolemethode schrijven die gebruikmaakt van de klasse SimpleEventProcessor om berichten te ontvangen

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
                    EhEntityPath,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EhConnectionString,
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

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcorercv.png
