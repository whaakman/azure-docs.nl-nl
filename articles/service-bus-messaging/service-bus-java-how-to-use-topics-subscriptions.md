---
title: Azure Service Bus-onderwerpen gebruiken met Java | Microsoft Docs
description: Gebruik Service Bus-onderwerpen en abonnementen in Azure.
services: service-bus-messaging
documentationcenter: java
author: spelluru
manager: timlt
editor: ''
ms.assetid: 63d6c8bd-8a22-4292-befc-545ffb52e8eb
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 09/17/2018
ms.author: spelluru
ms.openlocfilehash: 0be5f9842cd3aa90d82f3efe44451e624ed5d371
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395675"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-java"></a>Service Bus-onderwerpen en abonnementen gebruiken met Java

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Deze handleiding wordt beschreven hoe u Service Bus-onderwerpen en abonnementen. De voorbeelden zijn geschreven in Java en gebruik de [Azure SDK voor Java][Azure SDK for Java]. De behandelde scenario's zijn **het maken van onderwerpen en abonnementen**, **het maken van abonnementsfilters**, **verzenden van berichten naar een onderwerp**, **ontvangen berichten van een abonnement**, en **verwijderen van onderwerpen en abonnementen**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Wat zijn Service Bus-onderwerpen en -abonnementen?
Service Bus-onderwerpen en -abonnementen bieden ondersteuning voor een berichtencommunicatiemodel op basis van *publiceren/abonneren*. Wanneer u gebruikmaakt van onderwerpen en abonnementen, communiceren onderdelen van een gedistribueerde toepassing niet direct met elkaar. In plaats daarvan wisselen ze berichten uit via een onderwerp dat als intermediair fungeert.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Tegenstelling tot het Service Bus-wachtrijen, waarin elk bericht wordt verwerkt door een enkele gebruiker voorzien onderwerpen en -abonnementen in de vorm van een een-op-veel communicatie, met behulp van een patroon voor publiceren/abonneren. Het is mogelijk meerdere abonnementen voor een onderwerp te registreren. Wanneer een bericht naar een onderwerp wordt verzonden, wordt dit vervolgens beschikbaar gemaakt voor elk abonnement, waar het vervolgens zelfstandig wordt afgehandeld/verwerkt.

Een abonnement op een onderwerp lijkt op een virtuele wachtrij die kopieën van de berichten ontvangen die naar het onderwerp zijn verzonden. U kunt optioneel filterregels voor een onderwerp op basis van per abonnement, zodat u kunt filteren of beperken welke berichten naar een onderwerp worden ontvangen door welke berichtabonnementen registreren.

Service Bus-onderwerpen en abonnementen kunnen u schalen voor het verwerken van een groot aantal berichten over een groot aantal gebruikers en toepassingen.

## <a name="create-a-service-namespace"></a>Een servicenaamruimte maken
Als u wilt beginnen met behulp van Service Bus-onderwerpen en abonnementen in Azure, moet u eerst maken een *naamruimte*, waarmee u een scoping container voor het verwerken van Service Bus-resources in uw toepassing.

Ga als volgt te werk om een naamruimte te maken:

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor het gebruik van Service Bus
Zorg ervoor dat u hebt geïnstalleerd het [Azure SDK voor Java] [ Azure SDK for Java] voordat u dit voorbeeld maakt. Als u Eclipse gebruikt, kunt u de [Azure Toolkit voor Eclipse] [ Azure Toolkit for Eclipse] waarin de Azure SDK voor Java. Vervolgens kunt u toevoegen de **Microsoft Azure-bibliotheken voor Java** aan uw project:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Voeg de volgende `import` instructies toe aan het begin van de Java-bestand:

```java
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

De Azure-beheerbibliotheken voor Java toevoegen aan uw build-pad en deze opnemen in uw project implementatie-assembly.

## <a name="create-a-topic"></a>Een onderwerp maken
Bewerkingen voor Service Bus-onderwerpen kunnen worden uitgevoerd via de **ServiceBusContract** klasse. Een **ServiceBusContract** object is gemaakt met een geschikte configuratie die de SAS-token met machtigingen kapselt voor beheer, en de **ServiceBusContract** klasse is het enige storingspunt communicatie met Azure.

De **ServiceBusService** klasse biedt methoden voor het maken, opsommen en verwijderen van onderwerpen. Het volgende voorbeeld wordt getoond hoe een **ServiceBusService** object kan worden gebruikt voor het maken van een onderwerp met de naam `TestTopic`, met een naamruimte met de naam `HowToSample`:

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
      "HowToSample",
      "RootManageSharedAccessKey",
      "SAS_key_value",
      ".servicebus.windows.net"
      );

ServiceBusContract service = ServiceBusService.create(config);
TopicInfo topicInfo = new TopicInfo("TestTopic");
try  
{
    CreateTopicResult result = service.createTopic(topicInfo);
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Er zijn methoden op **TopicInfo** waarmee eigenschappen van het onderwerp moet worden ingesteld (bijvoorbeeld: om in te stellen van de standaard time-to-live (TTL) waarde moet worden toegepast op berichten die worden verzonden naar het onderwerp). Het volgende voorbeeld ziet u hoe u een onderwerp met de naam `TestTopic` met een maximale grootte van 5 GB:

```java
long maxSizeInMegabytes = 5120;  
TopicInfo topicInfo = new TopicInfo("TestTopic");  
topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateTopicResult result = service.createTopic(topicInfo);
```

U kunt de **listTopics** methode voor **ServiceBusContract** objecten om te controleren of een onderwerp met een opgegeven naam al in een Servicenaamruimte bestaat.

## <a name="create-subscriptions"></a>Maken van abonnementen
Abonnementen op onderwerpen ook worden gemaakt met de **ServiceBusService** klasse. Abonnementen hebben een naam en kunnen een optioneel filter hebben waarmee de verzameling berichten wordt beperkt die aan de virtuele wachtrij van het abonnement wordt doorgegeven.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)
Het **MatchAll**-filter is het standaardfilter dat wordt gebruikt als er bij het maken van een nieuw abonnement geen filter is opgegeven. Wanneer de **MatchAll** filter wordt gebruikt, worden alle berichten die zijn gepubliceerd naar het onderwerp in de virtuele wachtrij van het abonnement geplaatst. Het volgende voorbeeld wordt een abonnement genaamd `AllMessages` en gebruikmaakt van de `MatchAll` filter.

```java
SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
CreateSubscriptionResult result =
    service.createSubscription("TestTopic", subInfo);
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken
U kunt ook filters waarmee u aan het bereik dat berichten die worden verzonden naar een onderwerp binnen een bepaald onderwerpabonnement weergegeven maken.

Het meest flexibele type filter dat wordt ondersteund door abonnementen is de [SqlFilter][SqlFilter], die wordt geïmplementeerd met een subset van SQL92 wordt geïmplementeerd. SQL-filters worden uitgevoerd voor de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Raadpleeg voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter de [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxis.

Het volgende voorbeeld wordt een abonnement genaamd `HighMessages` met een [SqlFilter] [ SqlFilter] -object dat alleen berichten selecteert die een aangepaste **MessageNumber** de eigenschap is groter dan 3:

```java
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Op deze manier in het volgende voorbeeld wordt een abonnement genaamd `LowMessages` met een [SqlFilter] [ SqlFilter] -object dat alleen berichten selecteert die een **MessageNumber** eigenschap minder dan of gelijk aan 3:

```java
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Wanneer nu een bericht is verzonden naar `TestTopic`, wordt het altijd bezorgd bij ontvangers die zijn geabonneerd op de `AllMessages` abonnement, en selectief bezorgd bij ontvangers die zijn geabonneerd op de `HighMessages` en `LowMessages` abonnementen (al naar gelang de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp
Voor het verzenden van een bericht naar een Service Bus-onderwerp, uw toepassing verkrijgt een **ServiceBusContract** object. De volgende code laat zien hoe u een bericht verzenden voor de `TestTopic` onderwerp dat eerder is gemaakt binnen de `HowToSample` naamruimte:

```java
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Berichten verzonden naar Service Bus-onderwerpen zijn exemplaren van de [BrokeredMessage] [ BrokeredMessage] klasse. [BrokeredMessage][BrokeredMessage]* objecten hebben een aantal standaard-methoden (zoals **setLabel** en **TimeToLive**), een woordenlijst die wordt gebruikt voor het opslaan van aangepaste toepassingsspecifieke eigenschappen, en een hoofdtekst met willekeurige toepassingsgegevens. Een toepassing kan de hoofdtekst van het bericht instellen door elk serialiseerbaar object doorgeven aan de constructor van het [BrokeredMessage][BrokeredMessage], waarna de juiste **DataContractSerializer** wordt vervolgens gebruikt om het object te serialiseren. U kunt ook een **java.io.InputStream** kan worden opgegeven.

Het volgende voorbeeld ziet u hoe u voor het verzenden van vijf testberichten naar het `TestTopic` **MessageSender** we hebt verkregen in het vorige codefragment.
Houd er rekening mee hoe de **MessageNumber** eigenschapswaarde van elk bericht varieert op de herhaling van de lus (deze waarde bepaalt welke abonnementen ontvangen):

```java
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Service Bus-onderwerpen ondersteunen een maximale grootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een onderwerp, maar er is een limiet voor de totale grootte van de berichten op een onderwerp. De grootte van het onderwerp wordt gedefinieerd tijdens het maken, met een bovengrens van 5 GB.

## <a name="how-to-receive-messages-from-a-subscription"></a>Berichten van een abonnement ontvangen
Voor het ontvangen van berichten van een abonnement, gebruikt u een **ServiceBusContract** object. Ontvangen berichten kunnen werken in twee verschillende modi: **ReceiveAndDelete** en **PeekLock** (de standaardinstelling).

Wanneer u de **ReceiveAndDelete** modus, ontvangen een bewerking is voor één - dat wil zeggen, Service Bus een leesaanvraag voor een bericht ontvangt, wordt het bericht als verbruikt gemarkeerd en geeft dit terug aan de toepassing. **ReceiveAndDelete** modus is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing niet verwerken van een bericht tolereren kan als er een fout optreedt. Neem bijvoorbeeld een scenario waarin de consument problemen met de aanvraag ontvangen en vervolgens vastloopt voordat deze wordt verwerkt. Omdat Service Bus kan het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens ontbreekt wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint het het bericht dat voor het vastlopen is verbruikt.

In **PeekLock** modus, ontvangen een bewerking met twee fasen, waardoor er toepassingen kunnen worden ondersteund die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met het verwerken van bericht (of deze op betrouwbare wijze voor toekomstige verwerking slaat), is de tweede fase van het ontvangstproces voltooid door het aanroepen van **verwijderen** voor het ontvangen bericht. Wanneer Service Bus ziet de **verwijderen** -aanroep wordt gemarkeerd het bericht als verbruikt en wordt deze verwijderd uit het onderwerp.

Het volgende voorbeeld laat zien hoe berichten kunnen worden ontvangen en verwerkt met behulp **PeekLock** (de standaardmodus). Het voorbeeld voert een For-lus en verwerkt berichten in de `HighMessages` abonnement en vervolgens wordt afgesloten als er geen berichten meer (u kunt ook kan worden ingesteld om te wachten op nieuwe berichten).

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
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
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
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
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan het bericht te verwerken voor een of andere reden niet, dan kan worden aangeroepen de **unlockMessage** methode voor het ontvangen bericht (in plaats van de **deleteMessage** methode). Aanroepen van deze methode zorgt ervoor dat Service Bus het bericht in het onderwerp ontgrendelt en het beschikbaar worden om opnieuw te ontvangen, ofwel door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht dat in het onderwerp is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld, als de toepassing vastloopt), en vervolgens de Service Bus het bericht automatisch ontgrendelt en wordt het beschikbaar voor het opnieuw worden ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de **deleteMessage** aanvraag is uitgegeven, wordt het bericht is opnieuw naar de toepassing bezorgd wanneer deze opnieuw wordt opgestart. Dit proces heet vaak **ten minste één keer worden verwerkt**, dat wil zeggen, elk bericht ten minste één keer wordt verwerkt, maar in bepaalde situaties hetzelfde bericht opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. U kunt dit doen met behulp van de **getMessageId** -methode van het bericht dat gelijk bij meerdere bezorgingspogingen blijft.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen
De belangrijkste manier om te verwijderen van onderwerpen en abonnementen, is met een **ServiceBusContract** object. Als een onderwerp wordt verwijderd, worden ook alle abonnementen verwijderd die zijn geregistreerd bij het onderwerp. Abonnementen kunnen ook afzonderlijk worden verwijderd.

```java
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie, [Service Bus-wachtrijen, onderwerpen en abonnementen] [ Service Bus queues, topics, and subscriptions] voor meer informatie.

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.azure.servicebus.sqlfilter
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.azure.servicebus.sqlfilter.sqlexpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

[0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
[2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
[3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
