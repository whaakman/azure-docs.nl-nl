---
title: Gebeurtenissen verzenden met behulp van Java - Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat een walkthough voor het maken van een Java-toepassing die gebeurtenissen naar Azure Event Hubs verzendt.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 4da89e0f99832c429091e0a028fafd8943df811a
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55300112"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Gebeurtenissen verzenden naar Azure Event Hubs met behulp van Java

Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

Deze zelfstudie laat zien hoe u gebeurtenissen naar een event hub verzendt via een consoletoepassing die is geschreven in Java. 

> [!NOTE]
> U kunt deze snelstart als voorbeeld downloaden van de [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend), de tekenreeksen `EventHubConnectionString` en `EventHubName` vervangen door uw event hub-waarden en deze uitvoeren. U kunt ook de stappen in deze zelfstudie volgen om uw eigen oplossing te maken.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* Een Java-ontwikkelomgeving. Deze zelfstudie wordt gebruikgemaakt van [Eclipse](https://www.eclipse.org/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken
In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Volg de procedure in [dit artikel](event-hubs-create.md) om een naamruimte en een Event Hub te maken.

Haal de waarde van de toegangssleutel voor de event hub met de instructies in het artikel: [Verbindingstekenreeks ophalen](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). U gebruikt de toegangssleutel in de code die u verderop in deze zelfstudie schrijft. De naam van de standaard-sleutel is: **RootManageSharedAccessKey**.

Nu gaat u verder met de volgende stappen in deze zelfstudie.

## <a name="add-reference-to-azure-event-hubs-library"></a>Verwijzing naar de Azure Event Hubs-bibliotheek toevoegen

De Java-clientbibliotheek voor Event Hubs is beschikbaar voor gebruik in Maven-projecten uit de [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). U kunt verwijzen naar deze bibliotheek met behulp van de volgende afhankelijkheidsverklaring binnen het Maven-projectbestand:

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>2.2.0</version>
</dependency>
```

Voor verschillende soorten build-omgevingen, kunt u de meest recente vrijgegeven JAR-bestanden van expliciet verkrijgen de [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Voor een eenvoudige gebeurtenisuitgever, importeert u de *com.microsoft.azure.eventhubs* -pakket voor de Event Hubs-clientklassen en de *com.microsoft.azure.servicebus* pakket voor hulpprogramma voor klassen, zoals Algemene uitzonderingen die worden gedeeld met de Azure Service Bus messaging-client. 

## <a name="write-code-to-send-messages-to-the-event-hub"></a>Code schrijven om berichten te verzenden naar de event hub

Maak voor het volgende voorbeeld eerst een nieuw Maven-project voor een console/shell-toepassing in uw favoriete Java-ontwikkelomgeving. Voeg een klasse met de naam `SimpleSend`, en voeg de volgende code toe aan de klasse:

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;

public class SimpleSend {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
            
            
    }
 }
```

### <a name="construct-connection-string"></a>Verbindingsreeks

De klasse ConnectionStringBuilder gebruiken om samen te stellen van een string-waarde van de verbinding moet worden doorgegeven aan het exemplaar van de client Event Hubs. Vervang de tijdelijke aanduidingen door de waarden die u hebt verkregen tijdens het maken van de naamruimte en event hub:

```java
        final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
                .setNamespaceName("speventhubns") 
                .setEventHubName("speventhub")
                .setSasKeyName("RootManageSharedAccessKey")
                .setSasKey("2+WMsyyy1XmUtEnRsfOmTTyGasfJgsVjGAOIN20J1Y8=");
```

### <a name="send-events"></a>Gebeurtenissen verzenden

Een enkelvoudige gebeurtenis maken door het omzetten van een tekenreeks in de codering UTF-8 bytes. Vervolgens maakt u een nieuw exemplaar van de Event Hubs-client uit de verbindingsreeks en verzenden van het bericht:   

```java 
        final Gson gson = new GsonBuilder().create();

        // The Executor handles all asynchronous tasks and this is passed to the EventHubClient instance.
        // This enables the user to segregate their thread pool based on the work load.
        // This pool can then be shared across multiple EventHubClient instances.
        // The following sample uses a single thread executor, as there is only one EventHubClient instance,
        // handling different flavors of ingestion to Event Hubs here.
        final ScheduledExecutorService executorService = Executors.newScheduledThreadPool(4);

        // Each EventHubClient instance spins up a new TCP/SSL connection, which is expensive.
        // It is always a best practice to reuse these instances. The following sample shows this.
        final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);


        try {
            for (int i = 0; i < 10; i++) {

                String payload = "Message " + Integer.toString(i);
                byte[] payloadBytes = gson.toJson(payload).getBytes(Charset.defaultCharset());
                EventData sendEvent = EventData.create(payloadBytes);

                // Send - not tied to any partition
                // Event Hubs service will round-robin the events across all Event Hubs partitions.
                // This is the recommended & most reliable way to send to Event Hubs.
                ehClient.sendSync(sendEvent);
            }

            System.out.println(Instant.now() + ": Send Complete...");
            System.out.println("Press Enter to stop.");
            System.in.read();
        } finally {
            ehClient.closeSync();
            executorService.shutdown();
        }

``` 

Bouwen en voer het programma uit en zorg ervoor dat er geen fouten zijn.

Gefeliciteerd! U hebt nu berichten verzonden naar een Event Hub.

### <a name="appendix-how-messages-are-routed-to-eventhub-partitions"></a>Bijlage: Hoe berichten worden gerouteerd naar Event hub-partities

Voordat u berichten worden opgehaald door de consumenten, die ze moeten worden gepubliceerd naar de partities eerst door de uitgevers. Wanneer berichten worden gepubliceerd naar event hub synchroon met de methode sendSync() op het object com.microsoft.azure.eventhubs.EventHubClient, kan het bericht worden verzonden naar een specifieke partitie of gedistribueerd naar alle beschikbare partities op een wijze round robin afhankelijk van of de partitiesleutel is opgegeven of niet.

Als een tekenreeks voor de partitiesleutel is opgegeven, wordt de sleutel wordt gehasht om te bepalen welke partitie voor het verzenden van de gebeurtenis.

Als de partitiesleutel is niet ingesteld, klikt u vervolgens wordt berichten round-robined op alle beschikbare partities

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

// close the client at the end of your program
eventHubClient.closeSync();

```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u berichten verzonden naar een event hub met behulp van Java. Zie voor meer informatie over gebeurtenissen ontvangen van een event hub met behulp van Java, [ontvangen van gebeurtenissen van event hub - Java](event-hubs-java-get-started-receive-eph.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

