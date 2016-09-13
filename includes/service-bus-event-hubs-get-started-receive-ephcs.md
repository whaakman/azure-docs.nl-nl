## Berichten ontvangen met EventProcessorHost

[EventProcessorHost][] is een .NET-klasse die de het ontvangen van gebeurtenissen van Event Hubs vereenvoudigt door permanente controlepunten en parallelle ontvangst van deze Event Hubs te beheren. Met [EventProcessorHost][] kunt u gebeurtenissen splitsen over meerdere ontvangers, zelfs als deze worden gehost in verschillende knooppunten. In dit voorbeeld wordt het gebruik van [EventProcessorHost][] gedemonstreerd voor één ontvanger. In het voorbeeld [Uitgeschaalde gebeurtenisverwerking][] ziet u hoe u [EventProcessorHost][] gebruikt met meerdere ontvangers.

U moet over een [Azure Storage-account][] beschikken om [EventProcessorHost][] te gebruiken:

1. Meld u aan bij [Azure Portal][] en klik op **Nieuw** linksboven in het scherm.

2. Klik op **Gegevens en opslag** en klik vervolgens op **Opslagaccount**.

    ![][1]

3. Typ op de blade **Opslagaccount maken** een naam voor het opslagaccount. Kies een Azure-abonnement, resourcegroep en locatie voor het maken van de resource. Klik vervolgens op **Maken**.

    ![][2]

4. Klik in de lijst met opslagaccounts op het zojuist gemaakte opslagaccount.

5. Klik op de blade Opslagaccount op **Toegangssleutels**. Kopieer de waarde van **key1** voor gebruik verderop in deze zelfstudie.

    ![][3]

4. Maak in Visual Studio een nieuw Visual C# bureaublad-app-project met behulp van de projectsjabloon**Consoletoepassing**. Noem het project **Ontvanger**.

    ![][14]

5. Klik in Solution Explorer met de rechtermuisknop op de oplossing en kies **NuGet-pakketten beheren voor oplossing**.

6. Klik op het tabblad **Bladeren** en zoek naar `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Controleer of de projectnaam (**Ontvanger**) is opgegeven in het vak **Versie(s)**. Klik op **Installeren** en accepteer de gebruiksvoorwaarden.

    ![][13]

    Door Visual Studio wordt een verwijzing naar het [ NuGet-pakket Azure Service Bus Event Hub - EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), inclusief alle bijbehorende afhankelijkheden, gedownload, geïnstalleerd en toegevoegd.

7. Klik met de rechtermuisknop op het project **Ontvanger**, klik op **Toevoegen** en op **Klasse**. Noem de nieuwe klasse **SimpleEventProcessor** en klik op **Toevoegen** om de klasse te maken.

    ![][15]

8. Voeg de volgende instructies toe aan het begin van het bestand SimpleEventProcessor.cs:

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    ```

    Vervang daarna de hoofdtekst van de klasse door de volgende code:

    ```
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

    Deze klasse wordt aangeroepen door de **EventProcessorHost** om gebeurtenissen te verwerken die van de Event Hub worden ontvangen. U ziet dat de klasse `SimpleEventProcessor` een stopwatch gebruikt om regelmatig de controlepuntmethode voor de context **EventProcessorHost** aan te roepen. Op die manier gaat er nooit meer werk verloren dan in vijf minuten kan worden verwerkt, als de ontvanger opnieuw wordt opgestart.

9. Voeg in de klasse **Program** de volgende `using`-instructie toe aan het begin van het bestand:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

    Vervang daarna de methode `Main` in de klasse `Program` door de volgende code, waarbij u de naam van de Event Hub en de verbindingsreeks op naamruimteniveau die u eerder hebt opgeslagen, en het opslagaccount en de sleutel die u in de vorige secties hebt gekopieerd, vervangt. 

    ```
    static void Main(string[] args)
    {
      string eventHubConnectionString = "{Event Hub connection string}";
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

> [AZURE.NOTE] In deze zelfstudie wordt één exemplaar van [EventProcessorHost][] gebruikt. Voor een betere doorvoer wordt geadviseerd om meerdere exemplaren van [EventProcessorHost][] uit te voeren, zoals wordt geïllustreerd in het voorbeeld [Uitgeschaalde gebeurtenisverwerking][]. In die gevallen werken de verschillende instanties automatisch samen om de ontvangen gebeurtenissen gelijkmatig te verdelen. Als u wilt dat meerdere ontvangers *alle* gebeurtenissen verwerken, gebruik dan het concept **ConsumerGroup**. Wanneer gebeurtenissen van andere computers worden ontvangen, kan het nuttig zijn om namen voor [EventProcessorHost][]-exemplaren op te geven op basis van de computers waarop (of rollen waarin) ze zijn geïmplementeerd. Voor meer informatie over deze onderwerpen raadpleegt u het [Overzicht van Event Hubs][] en de [Event Hubs-programmeergids][].

<!-- Links -->
[Overzicht van Event Hubs]: event-hubs-overview.md
[Event Hubs-programmeergids]: event-hubs-programming-guide.md
[Uitgeschaalde gebeurtenisverwerking]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Azure Storage-account]: ../storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure Portal]: https://portal.azure.com

<!-- Images -->

[1]: ./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png
[2]: ./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png
[3]: ./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png
[13]: ./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png
[14]: ./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png
[15]: ./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png




<!--HONumber=sep16_HO1-->


