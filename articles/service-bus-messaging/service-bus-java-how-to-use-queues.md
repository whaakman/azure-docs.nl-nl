---
title: Over het gebruik van Azure Service Bus-wachtrijen met Java | Microsoft Docs
description: Informatie over het gebruiken van Service Bus-wachtrijen in Azure Voorbeelden van code geschreven in Java.
services: service-bus-messaging
documentationcenter: java
author: spelluru
manager: timlt
ms.assetid: f701439c-553e-402c-94a7-64400f997d59
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: e4099c8228e9434276242a3c49ebcb4fc2e995b2
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696462"
---
# <a name="how-to-use-service-bus-queues-with-java"></a>Over het gebruik van Service Bus-wachtrijen met Java
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In dit artikel wordt het gebruik van de Service Bus-wachtrijen beschreven. De voorbeelden zijn geschreven in Java en gebruik de [Azure SDK voor Java][Azure SDK for Java]. De behandelde scenario's zijn **maken van wachtrijen**, **verzenden en ontvangen van berichten**, en **verwijderen van wachtrijen**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor het gebruik van Service Bus
Zorg ervoor dat u hebt geïnstalleerd het [Azure SDK voor Java] [ Azure SDK for Java] voordat u dit voorbeeld maakt. Als u Eclipse gebruikt, kunt u de [Azure Toolkit voor Eclipse] [ Azure Toolkit for Eclipse] waarin de Azure SDK voor Java. Vervolgens kunt u toevoegen de **Microsoft Azure-bibliotheken voor Java** aan uw project:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Voeg de volgende `import` instructies toe aan het begin van de Java-bestand:

```java
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Een wachtrij maken
Bewerkingen voor Service Bus-wachtrijen kunnen worden uitgevoerd via de **ServiceBusContract** klasse. Een **ServiceBusContract** object is gemaakt met een geschikte configuratie die de SAS-token met machtigingen kapselt voor beheer, en de **ServiceBusContract** klasse is het enige storingspunt communicatie met Azure.

De **ServiceBusService** klasse biedt methoden voor het maken, opsommen en verwijderen van wachtrijen. Het voorbeeld hieronder toont hoe een **ServiceBusService** object kan worden gebruikt voor het maken van een wachtrij met de naam `TestQueue`, met een naamruimte met de naam `HowToSample`:

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Er zijn methoden op `QueueInfo` waarmee eigenschappen van de wachtrij om te worden afgestemd op (bijvoorbeeld: om in te stellen van de standaard time-to-live (TTL) waarde moet worden toegepast op berichten die worden verzonden naar de wachtrij). Het volgende voorbeeld laat zien hoe een wachtrij met de naam maken `TestQueue` met een maximale grootte van 5 GB:

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Let op: u kunt de `listQueues` methode voor **ServiceBusContract** objecten om te controleren of een wachtrij met een opgegeven naam al in een Servicenaamruimte bestaat.

## <a name="send-messages-to-a-queue"></a>Berichten verzenden naar een wachtrij
Als u wilt een bericht verzenden naar een Service Bus-wachtrij, uw toepassing verkrijgt een **ServiceBusContract** object. De volgende code laat zien hoe u een bericht verzenden voor de `TestQueue` wachtrij eerder hebt gemaakt de `HowToSample` naamruimte:

```java
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Berichten verzonden naar en ontvangen van Service Bus-wachtrijen zijn exemplaren van de [BrokeredMessage] [ BrokeredMessage] klasse. [BrokeredMessage] [ BrokeredMessage] objecten hebben een aantal standaardeigenschappen (zoals [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) en [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), een woordenlijst die wordt gebruikt voor het opslaan van aangepaste toepassingsspecifieke eigenschappen, en een hoofdtekst met willekeurige toepassingsgegevens. Een toepassing kan de hoofdtekst van het bericht instellen door elk serialiseerbaar object doorgeven aan de constructor van het [BrokeredMessage][BrokeredMessage], en de juiste serializer wordt vervolgens gebruikt om het object te serialiseren. U kunt ook opgeven een **java. I/O. InputStream** object.

Het volgende voorbeeld ziet u hoe u voor het verzenden van vijf testberichten naar het `TestQueue` **MessageSender** we hebt verkregen in het vorige codefragment:

```java
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Service Bus-wachtrijen ondersteunen een maximale berichtgrootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een wachtrij, maar er is een limiet voor de totale grootte van de berichten in een wachtrij. De grootte van de wachtrij wordt gedefinieerd tijdens het aanmaken, met een bovengrens van 5 GB.

## <a name="receive-messages-from-a-queue"></a>Berichten ontvangen van een wachtrij
De belangrijkste manier om berichten te ontvangen van een wachtrij is met een **ServiceBusContract** object. Ontvangen berichten kunnen werken in twee verschillende modi: **ReceiveAndDelete** en **PeekLock**.

Wanneer u de **ReceiveAndDelete** modus, ontvangen een bewerking is voor één - dat wil zeggen, Service Bus een leesaanvraag voor een bericht in een wachtrij ontvangt, wordt het bericht als verbruikt gemarkeerd en geeft dit terug aan de toepassing. **ReceiveAndDelete** modus (dit is de standaardmodus) is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt.
Omdat Service Bus het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint, ontbreekt het bericht dat voor het vastlopen is verbruikt.

In **PeekLock** modus, ontvangen een bewerking met twee fasen, waardoor er toepassingen kunnen worden ondersteund die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met het verwerken van bericht (of deze op betrouwbare wijze voor toekomstige verwerking slaat), is de tweede fase van het ontvangstproces voltooid door het aanroepen van **verwijderen** voor het ontvangen bericht. Wanneer Service Bus ziet de **verwijderen** aanroep, wordt het bericht als verbruikt markeren en verwijderen uit de wachtrij.

Het volgende voorbeeld laat zien hoe berichten kunnen worden ontvangen en verwerkt met behulp **PeekLock** -modus (niet de standaardinstelling). In het volgende voorbeeld wordt een oneindige lus en berichten worden verwerkt wanneer ze binnenkomen in onze `TestQueue`:

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                 service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan het bericht te verwerken voor een of andere reden niet, dan kan worden aangeroepen de **unlockMessage** methode voor het ontvangen bericht (in plaats van de **deleteMessage** methode). Dit zorgt ervoor dat Service Bus het bericht in de wachtrij ontgrendelt en het beschikbaar maakt om opnieuw te worden ontvangen, ofwel door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht dat in de wachtrij is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld, als de toepassing vastloopt), en vervolgens de Service Bus het bericht automatisch ontgrendelt en wordt het beschikbaar voor het opnieuw worden ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de **deleteMessage** aanvraag is uitgegeven, wordt het bericht is opnieuw naar de toepassing bezorgd wanneer deze opnieuw wordt opgestart. Dit wordt vaak genoemd *tenminste eenmaal verwerken*; dat wil zeggen, elk bericht ten minste één keer wordt verwerkt, maar in bepaalde situaties hetzelfde bericht opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. Dit wordt vaak bereikt met behulp van de **getMessageId** -methode van het bericht dat gelijk bij meerdere bezorgingspogingen blijft.

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van Service Bus-wachtrijen hebt geleerd, Zie [wachtrijen, onderwerpen en abonnementen] [ Queues, topics, and subscriptions] voor meer informatie.

Raadpleeg het [Java Developer Center](https://azure.microsoft.com/develop/java/) voor meer informatie.

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
