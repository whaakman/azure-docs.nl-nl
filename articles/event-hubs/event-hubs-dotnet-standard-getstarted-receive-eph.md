---
title: Ontvangen van gebeurtenissen van Azure Event Hubs met behulp van standaard .NET | Microsoft Docs
description: Ontvangen van berichten met de EventProcessorHost in .NET standaard aan de slag
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: cc62792dad0284f9514664795fdfb32e94a85943
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-receiving-messages-with-the-event-processor-host-in-net-standard"></a>Ontvangen van berichten met de Gebeurtenisprocessorhost in .NET standaard aan de slag

> [!NOTE]
> Dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).

Deze zelfstudie laat zien hoe u schrijft een consoletoepassing .NET Core die berichten van een event hub met behulp van ontvangt **EventProcessorHost**. U kunt uitvoeren de [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) oplossing als-is, de tekenreeksen vervangen door uw event hub en storage account waarden. Of u kunt de stappen in deze zelfstudie voor het maken van uw eigen.

## <a name="prerequisites"></a>Vereisten

* [Microsoft Visual Studio 2015 of 2017](http://www.visualstudio.com). De voorbeelden in deze zelfstudie Gebruik Visual Studio 2017, maar de Visual Studio 2015 wordt ook ondersteund.
* [.NET core Visual Studio 2015 of hulpprogramma's voor 2017](http://www.microsoft.com/net/core).
* Een Azure-abonnement.
* Een Azure Event Hubs-naamruimte.
* Een Azure Storage-account.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken  

De eerste stap is het gebruik van de [Azure-portal](https://portal.azure.com) voor het maken van een naamruimte voor het type Event Hubs en beheerreferenties die uw toepassing moet communiceren met de event hub. Volg de procedure in voor het maken van een naamruimte en event hub [in dit artikel](event-hubs-create.md), en vervolgens doorgaan met de volgende stappen uit.  

## <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken  

1. Meld u aan bij [Azure Portal](https://portal.azure.com).  
2. Klik in het linkernavigatievenster van de portal op **nieuw**, klikt u op **opslag**, en klik vervolgens op **Opslagaccount**.  
3. Vul de velden in de blade opslagaccount en klik vervolgens op **maken**.

    ![Storage-account maken][1]

4. Nadat u de **implementaties is geslaagd** bericht wordt weergegeven, klikt u op de naam van het nieuwe opslagaccount. In de **Essentials** blade, klikt u op **Blobs**. Wanneer de **Blob-service** blade wordt geopend, klikt u op **+ Container** aan de bovenkant. Geef een naam op voor de container en sluit vervolgens de **Blob-service** blade.  
5. Klik op **toegangssleutels** in het linkerdeelvenster blade en kopieer de naam van de storage-container, het opslagaccount en de waarde van **key1**. Deze waarden naar Kladblok of een andere tijdelijke locatie opslaan.  

## <a name="create-a-console-application"></a>Een consoletoepassing maken

Start Visual Studio. Klik in het menu **File** op **New** en klik vervolgens op **Project**. Maak een consoletoepassing .NET Core.

![Nieuw project][2]

## <a name="add-the-event-hubs-nuget-package"></a>Toevoegen van het Event Hubs NuGet-pakket

Voeg de [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) en [ `Microsoft.Azure.EventHubs.Processor` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET standaardbibliotheek NuGet-pakketten aan uw project met de volgende stappen: 

1. Klik met de rechtermuisknop op het nieuwe project en selecteer **NuGet-pakketten beheren**.
2. Klik op de **Bladeren** tabblad, zoekt u naar 'Microsoft.Azure.EventHubs' en selecteer de **Microsoft.Azure.EventHubs** pakket. Klik op **Installeren** om de installatie te voltooien en sluit vervolgens dit dialoogvenster.
3. Herhaal stap 1 en 2 en installeer de **Microsoft.Azure.EventHubs.Processor** pakket.

## <a name="implement-the-ieventprocessor-interface"></a>De IEventProcessor-interface implementeren

1. Klik in Solution Explorer met de rechtermuisknop op het project, klikt u op **toevoegen**, en klik vervolgens op **klasse**. Naam van de nieuwe klasse **SimpleEventProcessor**.

2. Open het bestand simpleeventprocessor.cs en voeg de volgende `using` instructies boven aan het bestand.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. Implementeer de `IEventProcessor` interface. Vervang de volledige inhoud van de `SimpleEventProcessor` klasse met de volgende code:

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

## <a name="write-a-main-console-method-that-uses-the-simpleeventprocessor-class-to-receive-messages"></a>Schrijven van een methode hoofdconsole die gebruikmaakt van de klasse SimpleEventProcessor om berichten te ontvangen

1. Voeg aan het begin van het bestand Program.cs de volgende `using`-instructies toe.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. Constanten toevoegen de `Program` klasse voor de verbindingsreeks voor event hub, naam event hub container opslagaccountnaam, opslagaccountnaam en opslagaccountsleutel. Voeg de volgende code, vervang de tijdelijke aanduidingen door de bijbehorende waarden.

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. Voeg een nieuwe methode met de naam `MainAsync` naar de `Program` klasse als volgt:

    ```csharp
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
    ```

3. Voeg de volgende regel code aan de `Main` methode:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    Zo zou het bestand Program.cs er moeten uitzien:

    ```csharp
    namespace SampleEphReceiver
    {

        public class Program
        {
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";
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

Gefeliciteerd. U hebt nu berichten ontvangen van een event hub met behulp van de Event Processor Host.

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
