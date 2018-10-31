---
title: Gebeurtenissen ontvangen van Azure Event Hubs met behulp van Java | Microsoft Docs
description: Aan de slag ontvangen van Event Hubs met behulp van Java
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 08/26/2018
ms.author: shvija
ms.openlocfilehash: dce7c4067ba6d96bf14f4e3300d951b594afe930
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240629"
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a>Gebeurtenissen ontvangen van Azure Event Hubs met behulp van Java

Eventhubs is een zeer schaalbare opname-systeem dat kan miljoenen gebeurtenissen per seconde opnemen, zodat een toepassing te verwerken en analyseren van de enorme hoeveelheden gegevens die worden geproduceerd door uw verbonden apparaten en toepassingen. Als ze eenmaal in Event Hubs zijn verzameld, kunt u de gegevens omzetten en opslaan met een realtime analytics-provider of opslagcluster.

Zie voor meer informatie de [overzicht van Event Hubs][Event Hubs overview].

Deze zelfstudie laat zien hoe u voor het ontvangen van gebeurtenissen in een event hub met een consoletoepassing die is geschreven in Java.

## <a name="prerequisites"></a>Vereisten

Als u wilt in deze zelfstudie hebt voltooid, moet u de volgende vereisten:

* Een Java-ontwikkelomgeving. Voor deze zelfstudie gaan we ervan uit [Eclipse](https://www.eclipse.org/).
* Een actief Azure-account. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account][] aan voordat u begint.

De code in deze zelfstudie is gebaseerd op de [EventProcessorSample code op GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/EventProcessorSample), die u kunt controleren om te zien van de volledige werkende toepassing.

## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Berichten ontvangen met EventProcessorHost in Java

**EventProcessorHost** is een Javaklasse die het ontvangen van gebeurtenissen van Event Hubs vereenvoudigt door permanente controlepunten beheren en parallelle ontvangst van deze Event Hubs. Met EventProcessorHost kunt splitsen u gebeurtenissen over meerdere ontvangers, zelfs als deze worden gehost in verschillende knooppunten. In dit voorbeeld wordt het gebruik van EventProcessorHost gedemonstreerd voor één ontvanger.

### <a name="create-a-storage-account"></a>Create a storage account

Voor het gebruik van EventProcessorHost, hebt u een [Azure Storage-account][Azure Storage account]:

1. Meld u aan bij de [Azure-portal][Azure portal], en klikt u op **+ een resource maken** aan de linkerkant van het scherm.
2. Klik op **Opslag** en klik vervolgens op **Opslagaccount**. In de **storage-account maken** venster, typ een naam voor het opslagaccount. Voltooi de rest van de velden, selecteer de gewenste regio en klik vervolgens op **maken**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Klik op het zojuist gemaakte opslagaccount en klik vervolgens op **toegangssleutels**:
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Kopieer de key1-waarde naar een tijdelijke locatie. U gaat deze verderop in de zelfstudie gebruiken.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>Maak een Java-project met behulp van EventProcessorHost

De Java-clientbibliotheek voor Event Hubs is beschikbaar voor gebruik in Maven-projecten uit de [Maven Central Repository][Maven Package], en kan worden verwezen met behulp van de volgende afhankelijkheidsverklaring binnen uw Maven Project-bestand. De huidige versie van het artefact azure-eventhubs-eph 2.0.1 is en de huidige versie voor de azure-eventhubs artefact 1.0.2 is:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.2</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>2.0.1</version>
</dependency>
```

Voor verschillende soorten build-omgevingen, kunt u de meest recente vrijgegeven JAR-bestanden van expliciet verkrijgen de [Maven Central Repository][Maven Package].  

1. Maak voor het volgende voorbeeld eerst een nieuw Maven-project voor een console/shell-toepassing in uw favoriete Java-ontwikkelomgeving. De klasse wordt aangeroepen `ErrorNotificationHandler`.     
   
    ```java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   
    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```
2. Gebruik de volgende code om een nieuwe klasse te maken met de naam `EventProcessorSample`. Vervang de tijdelijke aanduidingen door de waarden die worden gebruikt bij het maken van de event hub- en storage-account:
   
   ```java
   package com.microsoft.azure.eventhubs.samples.eventprocessorsample;

   import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
   import com.microsoft.azure.eventhubs.EventData;
   import com.microsoft.azure.eventprocessorhost.CloseReason;
   import com.microsoft.azure.eventprocessorhost.EventProcessorHost;
   import com.microsoft.azure.eventprocessorhost.EventProcessorOptions;
   import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   import com.microsoft.azure.eventprocessorhost.IEventProcessor;
   import com.microsoft.azure.eventprocessorhost.PartitionContext;

   import java.util.concurrent.ExecutionException;
   import java.util.function.Consumer;

   public class EventProcessorSample
   {
       public static void main(String args[]) throws InterruptedException, ExecutionException
       {
           String consumerGroupName = "$Default";
           String namespaceName = "----NamespaceName----";
           String eventHubName = "----EventHubName----";
           String sasKeyName = "----SharedAccessSignatureKeyName----";
           String sasKey = "----SharedAccessSignatureKey----";
           String storageConnectionString = "----AzureStorageConnectionString----";
           String storageContainerName = "----StorageContainerName----";
           String hostNamePrefix = "----HostNamePrefix----";
        
           ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder()
                .setNamespaceName(namespaceName)
                .setEventHubName(eventHubName)
                .setSasKeyName(sasKeyName)
                .setSasKey(sasKey);
        
           EventProcessorHost host = new EventProcessorHost(
                EventProcessorHost.createHostName(hostNamePrefix),
                eventHubName,
                consumerGroupName,
                eventHubConnectionString.toString(),
                storageConnectionString,
                storageContainerName);
        
           System.out.println("Registering host named " + host.getHostName());
           EventProcessorOptions options = new EventProcessorOptions();
           options.setExceptionNotification(new ErrorNotificationHandler());

           host.registerEventProcessor(EventProcessor.class, options)
           .whenComplete((unused, e) ->
           {
               if (e != null)
               {
                   System.out.println("Failure while registering: " + e.toString());
                   if (e.getCause() != null)
                   {
                       System.out.println("Inner exception: " + e.getCause().toString());
                   }
               }
           })
           .thenAccept((unused) ->
           {
               System.out.println("Press enter to stop.");
               try 
               {
                   System.in.read();
               }
               catch (Exception e)
               {
                   System.out.println("Keyboard read failed: " + e.toString());
               }
           })
           .thenCompose((unused) ->
           {
               return host.unregisterEventProcessor();
           })
           .exceptionally((e) ->
           {
               System.out.println("Failure while unregistering: " + e.toString());
               if (e.getCause() != null)
               {
                   System.out.println("Inner exception: " + e.getCause().toString());
               }
               return null;
           })
           .get(); // Wait for everything to finish before exiting main!
        
           System.out.println("End of sample");
       }
    ```
3. Maak een meer klasse met de naam `EventProcessor`, met de volgende code:
   
    ```java
    public static class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        // OnOpen is called when a new event processor instance is created by the host. 
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        // OnClose is called when an event processor instance is being shut down. 
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        // onError is called when an error occurs in EventProcessorHost code that is tied to this partition, such as a receiver failure.
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        // onEvents is called when events are received on this partition of the Event Hub. 
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> events) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got event batch");
            int eventCount = 0;
            for (EventData data : events)
            {
                try
                {
                    System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                            data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBytes(), "UTF8"));
                    eventCount++;
                    
                    // Checkpointing persists the current position in the event stream for this partition and means that the next
                    // time any host opens an event processor on this event hub+consumer group+partition combination, it will start
                    // receiving at the event after this one. 
                    this.checkpointBatchingCount++;
                    if ((checkpointBatchingCount % 5) == 0)
                    {
                        System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                            data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                        // Checkpoints are created asynchronously. It is important to wait for the result of checkpointing
                        // before exiting onEvents or before creating the next checkpoint, to detect errors and to ensure proper ordering.
                        context.checkpoint(data).get();
                    }
                }
                catch (Exception e)
                {
                    System.out.println("Processing failed for an event: " + e.toString());
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + eventCount + " for host " + context.getOwner());
        }
    }
    ```

In deze zelfstudie wordt één exemplaar van EventProcessorHost gebruikt. U wordt aangeraden dat u meerdere exemplaren van EventProcessorHost, bij voorkeur uitgevoerd op afzonderlijke computers voor een betere doorvoer.  Het biedt ook redundantie. In die gevallen werken de verschillende exemplaren automatisch samen om de ontvangen gebeurtenissen gelijkmatig te verdelen. Als u wilt dat meerdere ontvangers *alle* gebeurtenissen verwerken, gebruik dan het concept **ConsumerGroup**. Wanneer er gebeurtenissen van verschillende computers worden ontvangen, kan het nuttig zijn om namen voor EventProcessorHost-exemplaren op te geven op basis van de computers waarop (of rollen waarin) ze zijn geïmplementeerd.

## <a name="publishing-messages-to-eventhub"></a>Publiceren berichten naar EventHub

Voordat u berichten worden opgehaald door de consumenten, die ze moeten worden gepubliceerd naar de partities eerst door de uitgevers. Dit is het vermelden waard dat wanneer berichten worden gepubliceerd naar event hub synchroon met de methode sendSync() op het object com.microsoft.azure.eventhubs.EventHubClient, het bericht kan worden verzonden naar een specifieke partitie of gedistribueerd naar alle beschikbare partities op een wijze round robin, afhankelijk van of u is de partitiesleutel of niet opgegeven.

Als een tekenreeks voor de partitiesleutel is opgegeven, wordt de sleutel wordt gehasht om te bepalen welke partitie voor het verzenden van de gebeurtenis.

Als de partitiesleutel is niet ingesteld, klikt u vervolgens zijn berichten round-robined op alle beschikbare partities

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

```

## <a name="implementing-a-custom-checkpointmanager-for-eventprocessorhost-eph"></a>Implementeren van een aangepaste CheckpointManager voor EventProcessorHost (EPH)

De API biedt een mechanisme voor het implementeren van uw aangepaste controlepuntbeheer voor scenario's waarbij de standaardimplementatie niet compatibel met uw situatie is.

De standaard controlepunt manager maakt gebruik van blob-opslag, maar als u de controlepunt-manager gebruikt door EPH met uw eigen implementatie overschrijft, kunt u geen store die u wilt maken van uw manager-implementatie van het controlepunt.

Maken van een klasse die de interface com.microsoft.azure.eventprocessorhost.ICheckpointManager

Gebruik van uw aangepaste implementatie van de manager van het controlepunt (com.microsoft.azure.eventprocessorhost.ICheckpointManager)

In uw implementatie, kunt u het standaardmechanisme voor het plaatsen van controlepunten overschrijven en implementeren van onze eigen controlepunten op basis van uw eigen gegevensarchief (SQL Server, cosmos DB, Redis Cache enzovoort). U wordt aangeraden dat de store gebruikt voor het back-ups maken uw manager-implementatie van het controlepunt is toegankelijk voor alle EPH-exemplaren die zijn verwerking van gebeurtenissen voor de consumergroep.

U kunt een gegevensopslag die beschikbaar is in uw omgeving.

De klasse com.microsoft.azure.eventprocessorhost.EventProcessorHost beschikt u over twee constructors waarmee u kunt voor de onderdrukking van de manager van het controlepunt voor de EventProcessorHost.

## <a name="next-steps"></a>Volgende stappen
In deze quickstart maakt u een Java-toepassing die berichten van een event hub ontvangen gemaakt. Zie voor meer informatie over het verzenden van gebeurtenissen naar een event hub met behulp van Java, [verzenden van gebeurtenissen van event hub - Java](event-hubs-java-get-started-send.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
[gratis account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
