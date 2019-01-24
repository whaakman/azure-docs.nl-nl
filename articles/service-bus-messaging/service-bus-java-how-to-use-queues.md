---
title: Over het gebruik van Azure Service Bus-wachtrijen met Java | Microsoft Docs
description: Informatie over het gebruiken van Service Bus-wachtrijen in Azure Voorbeelden van code geschreven in Java.
services: service-bus-messaging
documentationcenter: java
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f701439c-553e-402c-94a7-64400f997d59
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 09/13/2018
ms.author: aschhab
ms.openlocfilehash: 9a26a08e7b1fb313e25a7dc8d9f5e58e843fca76
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54849615"
---
# <a name="how-to-use-service-bus-queues-with-java"></a>Over het gebruik van Service Bus-wachtrijen met Java
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In dit artikel wordt het gebruik van de Service Bus-wachtrijen beschreven. De voorbeelden zijn geschreven in Java en gebruik de [Azure SDK voor Java][Azure SDK for Java]. De behandelde scenario's zijn **maken van wachtrijen**, **verzenden en ontvangen van berichten**, en **verwijderen van wachtrijen**.

> [!NOTE]
> U vindt Java-voorbeelden op GitHub in de [azure-service-bus repository](https://github.com/Azure/azure-service-bus/tree/master/samples/Java).

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-service-bus-namespace"></a>Een Service Bus-naamruimte maken
[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-service-bus-queue"></a>Een Service Bus-wachtrij maken
[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor het gebruik van Service Bus
Zorg ervoor dat u hebt geïnstalleerd het [Azure SDK voor Java] [ Azure SDK for Java] voordat u dit voorbeeld maakt. Als u Eclipse gebruikt, kunt u de [Azure Toolkit voor Eclipse] [ Azure Toolkit for Eclipse] waarin de Azure SDK voor Java. Vervolgens kunt u toevoegen de **Microsoft Azure-bibliotheken voor Java** aan uw project:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Voeg de volgende `import` instructies toe aan het begin van de Java-bestand:

```java
// Include the following imports to use Service Bus APIs
import com.google.gson.reflect.TypeToken;
import com.microsoft.azure.servicebus.*;
import com.microsoft.azure.servicebus.primitives.ConnectionStringBuilder;
import com.google.gson.Gson;

import static java.nio.charset.StandardCharsets.*;

import java.time.Duration;
import java.util.*;
import java.util.concurrent.*;

import org.apache.commons.cli.*;

```

## <a name="send-messages-to-a-queue"></a>Berichten verzenden naar een wachtrij
Om berichten te verzenden naar een Service Bus-wachtrij, uw toepassing maakt een **QueueClient** object en berichten asynchroon worden verzonden. De volgende code toont hoe u een bericht verzendt voor een wachtrij die is gemaakt via de portal.

```java
public void run() throws Exception {
    // Create a QueueClient instance and then asynchronously send messages.
    // Close the sender once the send operation is complete.
    QueueClient sendClient = new QueueClient(new ConnectionStringBuilder(ConnectionString, QueueName), ReceiveMode.PEEKLOCK);
    this.sendMessageAsync(sendClient).thenRunAsync(() -> sendClient.closeAsync());

    sendClient.close();
}

    CompletableFuture<Void> sendMessagesAsync(QueueClient sendClient) {
        List<HashMap<String, String>> data =
                GSON.fromJson(
                        "[" +
                                "{'name' = 'Einstein', 'firstName' = 'Albert'}," +
                                "{'name' = 'Heisenberg', 'firstName' = 'Werner'}," +
                                "{'name' = 'Curie', 'firstName' = 'Marie'}," +
                                "{'name' = 'Hawking', 'firstName' = 'Steven'}," +
                                "{'name' = 'Newton', 'firstName' = 'Isaac'}," +
                                "{'name' = 'Bohr', 'firstName' = 'Niels'}," +
                                "{'name' = 'Faraday', 'firstName' = 'Michael'}," +
                                "{'name' = 'Galilei', 'firstName' = 'Galileo'}," +
                                "{'name' = 'Kepler', 'firstName' = 'Johannes'}," +
                                "{'name' = 'Kopernikus', 'firstName' = 'Nikolaus'}" +
                                "]",
                        new TypeToken<List<HashMap<String, String>>>() {}.getType());

        List<CompletableFuture> tasks = new ArrayList<>();
        for (int i = 0; i < data.size(); i++) {
            final String messageId = Integer.toString(i);
            Message message = new Message(GSON.toJson(data.get(i), Map.class).getBytes(UTF_8));
            message.setContentType("application/json");
            message.setLabel("Scientist");
            message.setMessageId(messageId);
            message.setTimeToLive(Duration.ofMinutes(2));
            System.out.printf("\nMessage sending: Id = %s", message.getMessageId());
            tasks.add(
                    sendClient.sendAsync(message).thenRunAsync(() -> {
                        System.out.printf("\n\tMessage acknowledged: Id = %s", message.getMessageId());
                    }));
        }
        return CompletableFuture.allOf(tasks.toArray(new CompletableFuture<?>[tasks.size()]));
    }

```

Berichten verzonden naar en ontvangen van Service Bus-wachtrijen zijn exemplaren van de [bericht](/java/api/com.microsoft.azure.servicebus.message?view=azure-java-stable) klasse. Berichtobjecten hebben een aantal standaardeigenschappen (zoals Label en TimeToLive), een woordenlijst die wordt gebruikt om aangepaste toepassingsspecifieke eigenschappen te bewaren, en een hoofdtekst met willekeurige toepassingsgegevens. Een toepassing kan de hoofdtekst van het bericht instellen door elk serialiseerbaar object doorgeven aan de constructor van het bericht en de juiste serializer wordt vervolgens gebruikt om het object te serialiseren. U kunt ook opgeven een **java. I/O. InputStream** object.


Service Bus-wachtrijen ondersteunen een maximale berichtgrootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een wachtrij, maar er is een limiet voor de totale grootte van de berichten in een wachtrij. De grootte van de wachtrij wordt gedefinieerd tijdens het aanmaken, met een bovengrens van 5 GB.

## <a name="receive-messages-from-a-queue"></a>Berichten ontvangen van een wachtrij
De belangrijkste manier om berichten te ontvangen van een wachtrij is met een **ServiceBusContract** object. Ontvangen berichten kunnen werken in twee verschillende modi: **ReceiveAndDelete** en **PeekLock**.

Wanneer u de **ReceiveAndDelete** modus, ontvangen een bewerking is voor één - dat wil zeggen, Service Bus een leesaanvraag voor een bericht in een wachtrij ontvangt, wordt het bericht als verbruikt gemarkeerd en geeft dit terug aan de toepassing. **ReceiveAndDelete** modus (dit is de standaardmodus) is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt.
Omdat Service Bus kan het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens ontbreekt wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint het het bericht dat voor het vastlopen is verbruikt.

In **PeekLock** modus, ontvangen een bewerking met twee fasen, waardoor er toepassingen kunnen worden ondersteund die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met het verwerken van bericht (of deze op betrouwbare wijze voor toekomstige verwerking slaat), is de tweede fase van het ontvangstproces voltooid door het aanroepen van **verwijderen** voor het ontvangen bericht. Wanneer Service Bus ziet de **verwijderen** -aanroep wordt gemarkeerd het bericht als verbruikt en verwijderen uit de wachtrij.

Het volgende voorbeeld laat zien hoe berichten kunnen worden ontvangen en verwerkt met behulp **PeekLock** -modus (niet de standaardinstelling). In het volgende voorbeeld wordt een oneindige lus en berichten worden verwerkt wanneer ze binnenkomen in onze `TestQueue`:

```java
    public void run() throws Exception {
        // Create a QueueClient instance for receiving using the connection string builder
        // We set the receive mode to "PeekLock", meaning the message is delivered
        // under a lock and must be acknowledged ("completed") to be removed from the queue
        QueueClient receiveClient = new QueueClient(new ConnectionStringBuilder(ConnectionString, QueueName), ReceiveMode.PEEKLOCK);
        this.registerReceiver(receiveClient);

        // shut down receiver to close the receive loop
        receiveClient.close();
    }
    void registerReceiver(QueueClient queueClient) throws Exception {
        // register the RegisterMessageHandler callback
        queueClient.registerMessageHandler(new IMessageHandler() {
        // callback invoked when the message handler loop has obtained a message
            public CompletableFuture<Void> onMessageAsync(IMessage message) {
            // receives message is passed to callback
                if (message.getLabel() != null &&
                    message.getContentType() != null &&
                    message.getLabel().contentEquals("Scientist") &&
                    message.getContentType().contentEquals("application/json")) {

                        byte[] body = message.getBody();
                        Map scientist = GSON.fromJson(new String(body, UTF_8), Map.class);

                        System.out.printf(
                            "\n\t\t\t\tMessage received: \n\t\t\t\t\t\tMessageId = %s, \n\t\t\t\t\t\tSequenceNumber = %s, \n\t\t\t\t\t\tEnqueuedTimeUtc = %s," +
                            "\n\t\t\t\t\t\tExpiresAtUtc = %s, \n\t\t\t\t\t\tContentType = \"%s\",  \n\t\t\t\t\t\tContent: [ firstName = %s, name = %s ]\n",
                            message.getMessageId(),
                            message.getSequenceNumber(),
                            message.getEnqueuedTimeUtc(),
                            message.getExpiresAtUtc(),
                            message.getContentType(),
                            scientist != null ? scientist.get("firstName") : "",
                            scientist != null ? scientist.get("name") : "");
                    }
                    return CompletableFuture.completedFuture(null);
                }

                // callback invoked when the message handler has an exception to report
                public void notifyException(Throwable throwable, ExceptionPhase exceptionPhase) {
                    System.out.printf(exceptionPhase + "-" + throwable.getMessage());
                }
        },
        // 1 concurrent call, messages are auto-completed, auto-renew duration
        new MessageHandlerOptions(1, true, Duration.ofMinutes(1)));
    }

```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan het bericht te verwerken voor een of andere reden niet, dan kan worden aangeroepen de **unlockMessage** methode voor het ontvangen bericht (in plaats van de **deleteMessage** methode). Dit zorgt ervoor dat Service Bus het bericht in de wachtrij ontgrendelt en het beschikbaar maakt om opnieuw te worden ontvangen, ofwel door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht dat in de wachtrij is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld, als de toepassing vastloopt), en vervolgens de Service Bus het bericht automatisch ontgrendelt en wordt het beschikbaar voor het opnieuw worden ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de **deleteMessage** aanvraag is uitgegeven, wordt het bericht is opnieuw naar de toepassing bezorgd wanneer deze opnieuw wordt opgestart. Dit wordt vaak *Ten minste eenmaal verwerken* genoemd; dat wil zeggen dat elk bericht ten minste één keer wordt verwerkt, maar dat hetzelfde bericht in sommige situaties opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. Dit wordt vaak bereikt met behulp van de **getMessageId** -methode van het bericht dat gelijk bij meerdere bezorgingspogingen blijft.

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van Service Bus-wachtrijen hebt geleerd, Zie [wachtrijen, onderwerpen en abonnementen] [ Queues, topics, and subscriptions] voor meer informatie.

Raadpleeg het [Java Developer Center](https://azure.microsoft.com/develop/java/) voor meer informatie.

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
