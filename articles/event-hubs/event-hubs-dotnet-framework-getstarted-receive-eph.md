---
title: Gebeurtenissen ontvangen van Azure Event Hubs met behulp van het .NET Framework | Microsoft Docs
description: Volg deze zelfstudie om gebeurtenissen te ontvangen van Azure Event Hubs middels het .NET Framework.
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/10/2017
ms.author: sethm
ms.openlocfilehash: 5d2f6f53af182a8ac0430de0ca3701a9a30e0bf4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="receive-events-from-azure-event-hubs-using-the-net-framework"></a>Gebeurtenissen ontvangen van Azure Event Hubs met behulp van het .NET Framework

## <a name="introduction"></a>Inleiding

Event Hubs is een service die grote hoeveelheden gebeurtenisgegevens (telemetrie) van verbonden apparaten en toepassingen verwerkt. Nadat u gegevens in Event Hubs hebt verzameld, kunt u de gegevens opslaan met behulp van een opslagcluster of transformeren met een provider van realtime-analyses. Deze functie voor grootschalige gebeurtenisverzameling en -verwerking is een belangrijk onderdeel van de architectuur van moderne toepassingen, met inbegrip van het Internet der dingen (IoT).

In deze zelfstudie ziet u hoe u een .NET Framework-consoletoepassing schrijft die met de **[EventProcessorHost][EventProcessorHost]** berichten ontvangt van een Event Hub. Als u gebeurtenissen wilt verzenden met het .NET Framework, raadpleeg dan het artikel [Gebeurtenissen verzenden naar Azure Event Hubs met behulp van .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) of klik in de inhoudsopgave aan de linkerkant op de juiste taal voor verzending.

De [EventProcessorHost][EventProcessorHost] is een .NET-klasse die het ontvangen van gebeurtenissen van Event Hubs vereenvoudigt door permanente controlepunten en parallelle ontvangst van deze Event Hubs te beheren. Met de [EventProcessorHost][Event Processor Host] kunt u gebeurtenissen splitsen over meerdere ontvangers, zelfs als deze worden gehost in verschillende knooppunten. In dit voorbeeld wordt het gebruik van de [EventProcessorHost][EventProcessorHost] gedemonstreerd voor één ontvanger. In het voorbeeld [Uitgeschaalde gebeurtenisverwerking][Scale out Event Processing with Event Hubs] ziet u hoe u de [EventProcessorHost][EventProcessorHost] gebruikt met meerdere ontvangers.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* [Microsoft Visual Studio 2015 of hoger](http://visualstudio.com). In de schermafbeeldingen in deze zelfstudie wordt Visual Studio 2017 gebruikt.
* Een actief Azure-account. Als u geen Azure-account hebt, kunt u binnen een paar minuten een gratis account maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/free/) voor meer informatie.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken

In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Volg de procedure in [dit artikel](event-hubs-create.md) om een naamruimte en Event Hub te maken en ga daarna verder met de volgende stappen in deze zelfstudie.

## <a name="create-an-azure-storage-account"></a>Een Azure Storage-account maken

Als u de [EventProcessorHost][EventProcessorHost] wilt gebruiken, moet u een [Azure Storage-account][Azure Storage account] hebben:

1. Meld u aan bij [Azure Portal][Azure portal] en klik op**Nieuw** linksboven in het scherm.
2. Klik op **Opslag** en klik vervolgens op **Opslagaccount**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. Typ op de blade **Opslagaccount maken** een naam voor het opslagaccount. Kies een Azure-abonnement, resourcegroep en locatie voor het maken van de resource. Klik vervolgens op **Maken**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. Klik in de lijst met opslagaccounts op het zojuist gemaakte opslagaccount.
5. Klik op de blade Opslagaccount op **Toegangssleutels**. Kopieer de waarde van **key1** voor gebruik verderop in deze zelfstudie.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

## <a name="create-a-receiver-console-application"></a>Een consoletoepassing voor ontvangers maken

1. Maak in Visual Studio een nieuw Visual C# bureaublad-app-project met behulp van de projectsjabloon**Consoletoepassing**. Noem het project **Ontvanger**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
2. Klik in Solution Explorer met de rechtermuisknop op het project **Ontvanger** en klik op **NuGet-pakketten beheren voor oplossing**.
3. Klik op het tabblad **Bladeren** en zoek vervolgens naar `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Klik op **Installeren** en accepteer de gebruiksvoorwaarden.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Door Visual Studio wordt een verwijzing naar het [ NuGet-pakket Azure Service Bus Event Hub - EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), inclusief alle bijbehorende afhankelijkheden, gedownload, geïnstalleerd en toegevoegd.
4. Klik met de rechtermuisknop op het project **Ontvanger**, klik op **Toevoegen** en op **Klasse**. Noem de nieuwe klasse **SimpleEventProcessor** en klik op **Toevoegen** om de klasse te maken.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
5. Voeg de volgende instructies toe aan het begin van het bestand SimpleEventProcessor.cs:
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  using System.Diagnostics;
  ```
    
  Vervang daarna de hoofdtekst van de klasse door de volgende code:
    
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
6. Voeg in de klasse **Program** de volgende `using`-instructie toe aan het begin van het bestand:
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  ```
    
  Vervang daarna de methode `Main` in de klasse `Program` door de volgende code, waarbij u de naam van de Event Hub en de verbindingsreeks op naamruimteniveau die u eerder hebt opgeslagen, en het opslagaccount en de sleutel die u in de vorige secties hebt gekopieerd, vervangt. 
    
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

7. Voer het programma uit en controleer of er geen fouten zijn.
  
Gefeliciteerd. U hebt nu met behulp van de EventProcessorHost berichten ontvangen van een Event Hub.


> [!NOTE]
> In deze zelfstudie wordt één exemplaar van [EventProcessorHost][EventProcessorHost] gebruikt. Voor een betere doorvoer wordt geadviseerd om meerdere exemplaren van [EventProcessorHost][EventProcessorHost] uit te voeren, zoals wordt geïllustreerd in het voorbeeld [Uitgeschaalde gebeurtenisverwerking][Uitgeschaalde gebeurtenisverwerking]. In die gevallen werken de verschillende instanties automatisch samen om de ontvangen gebeurtenissen gelijkmatig te verdelen. Als u wilt dat meerdere ontvangers *alle* gebeurtenissen verwerken, gebruik dan het concept **ConsumerGroup**. Wanneer er gebeurtenissen van verschillende computers worden ontvangen, kan het nuttig zijn om namen voor [EventProcessorHost-exemplaren][EventProcessorHost] op te geven op basis van de computers waarop (of rollen waarin) ze zijn geïmplementeerd. Zie [Overzicht van Event Hubs][Event Hubs overview] en [Event Hubs-programmeergids][Event Hubs Programming Guide] voor meer informatie over deze onderwerpen.
> 
> 

## <a name="next-steps"></a>Volgende stappen

Nu u een werkende toepassing hebt gebouwd die een Event Hub maakt en gegevens verzendt en ontvangt, kunt u via de volgende koppelingen meer informatie opdoen:

* [EventProcessorHost][Event Processor Host]
* [Event Hubs-overzicht][Event Hubs overview]
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[EventProcessorHost]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]:../storage/common/storage-create-storage-account.md
[Event Processor Host]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Azure portal]: https://portal.azure.com
