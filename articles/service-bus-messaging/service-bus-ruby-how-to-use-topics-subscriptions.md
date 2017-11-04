---
title: Het gebruik van Service Bus-onderwerpen (Ruby) | Microsoft Docs
description: Informatie over het gebruik van Service Bus-onderwerpen en abonnementen in Azure. Codevoorbeelden zijn geschreven voor Ruby-toepassingen.
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3ef2295e-7c5f-4c54-a13b-a69c8045d4b6
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 4a4c9949843b16ae6be2f516de4fd1e3f7415959
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-ruby"></a>Service Bus-onderwerpen en abonnementen gebruiken met Ruby
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dit artikel wordt beschreven hoe u Service Bus-onderwerpen en abonnementen van Ruby toepassingen. De scenario's worden behandeld: **maken van onderwerpen en abonnementen, het maken van abonnementsfilters, het verzenden van berichten** naar een onderwerp **ontvangen van berichten van een abonnement**, en **verwijderen van onderwerpen en abonnementen**. Zie voor meer informatie over onderwerpen en abonnementen, de [Vervolgstappen](#next-steps) sectie.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a>Een onderwerp maken
De **Azure::ServiceBusService** object kunt u werken met onderwerpen. De volgende code maakt een **Azure::ServiceBusService** object. Gebruik voor het maken van een onderwerp de `create_topic()` methode. Het volgende voorbeeld maakt een onderwerp of de fouten wordt afgedrukt als er een.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

U kunt ook doorgeven een **Azure::ServiceBus::Topic** object met extra opties waarmee u kunt de standaardinstellingen voor onderwerp zoals de live of Maximale wachtrijgrootte bericht negeren. Het volgende voorbeeld ziet u de maximale wachtrijgrootte 5 GB en tijd voor live op 1 minuut instellen:

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Abonnementen maken
Ook onderwerpabonnementen worden gemaakt met de **Azure::ServiceBusService** object. Abonnementen kunnen worden genoemd en een optioneel filter waarmee de verzameling berichten in de virtuele wachtrij van het abonnement wordt beperkt.

Abonnementen worden permanent en blijft bestaan totdat beide ze of het onderwerp zijn gekoppeld, worden verwijderd. Als uw toepassing bevat de logica voor het maken van een abonnement, moet eerst controleren of het abonnement bestaat al met de methode getSubscription.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)
Het **MatchAll**-filter is het standaardfilter dat wordt gebruikt als er bij het maken van een nieuw abonnement geen filter is opgegeven. Wanneer de **MatchAll** filter wordt gebruikt, worden alle berichten die zijn gepubliceerd naar het onderwerp in de virtuele wachtrij van het abonnement geplaatst. Het volgende voorbeeld wordt een abonnement genaamd 'all '-berichten en gebruikt de standaardinstallatielocatie **MatchAll** filter.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken
Ook kunt u filters waarmee u kunt opgeven welke berichten die worden verzonden naar een onderwerp weergegeven binnen een bepaald abonnement.

Het meest flexibele type filter dat wordt ondersteund door abonnementen is de **Azure::ServiceBus::SqlFilter**, waarmee een subset van SQL92 wordt geïmplementeerd implementeert. SQL-filters worden uitgevoerd voor de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Raadpleeg voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter, de [SqlFilter](service-bus-messaging-sql-filter.md) syntaxis.

U kunt filters toevoegen aan een abonnement met behulp van de `create_rule()` methode van de **Azure::ServiceBusService** object. Deze methode kunt u nieuwe filters toevoegen aan een bestaand abonnement.

Aangezien het standaardfilter automatisch op alle nieuwe abonnementen toegepast wordt, moet u eerst het standaardfilter verwijderen of de **MatchAll** overschrijft alle andere filters die u kunt opgeven. U kunt de standaardregel verwijderen met behulp van de `delete_rule()` methode op de **Azure::ServiceBusService** object.

Het volgende voorbeeld maakt u een abonnement genaamd 'hoog '-berichten met een **Azure::ServiceBus::SqlFilter** die alleen berichten selecteert die een aangepaste `message_number` eigenschap groter is dan 3:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Op deze manier in het volgende voorbeeld wordt een abonnement genaamd `low-messages` met een **Azure::ServiceBus::SqlFilter** die alleen berichten selecteert die een `message_number` eigenschap kleiner dan of gelijk aan 3:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Wanneer is nu een bericht verzonden naar `test-topic`, wordt altijd worden bezorgd bij ontvangers die zijn geabonneerd op de `all-messages` onderwerpabonnement en selectief bezorgd bij ontvangers die zijn geabonneerd op de `high-messages` en `low-messages` onderwerpabonnementen (afhankelijk van de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp
Als u wilt een bericht verzendt naar een Service Bus-onderwerp, de toepassing moet gebruiken de `send_topic_message()` methode op de **Azure::ServiceBusService** object. Berichten die worden verzonden naar Service Bus-onderwerpen, zijn exemplaren van de **Azure::ServiceBus::BrokeredMessage** objecten. **Azure::ServiceBus::BrokeredMessage** objecten hebben een aantal standaardeigenschappen (zoals `label` en `time_to_live`), een woordenlijst die wordt gebruikt om aangepaste toepassingsspecifieke eigenschappen te bewaren en een hoofdtekst met tekenreeksen. Een toepassing kunt instellen in de hoofdtekst van het bericht voor het doorgeven van een tekenreekswaarde naar het `send_topic_message()` methode en alle vereiste standaardeigenschappen standaardwaarden worden ingevuld.

Het volgende voorbeeld toont hoe vijf testberichten naar verzendt `test-topic`. Houd er rekening mee dat de `message_number` waarde van de aangepaste eigenschap van elk bericht varieert op de herhaling van de lus (deze bepaalt welke abonnement ontvangt):

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Service Bus-onderwerpen ondersteunen een maximale grootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een onderwerp, maar er is een limiet voor de totale grootte van de berichten in een onderwerp. De grootte van het onderwerp wordt gedefinieerd tijdens het maken, met een bovengrens van 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangen van een abonnement
Berichten worden ontvangen van een abonnement met de `receive_subscription_message()` methode op de **Azure::ServiceBusService** object. Standaard berichten zijn read(peak) en zonder het te verwijderen uit het abonnement is vergrendeld. U kunt lezen en verwijderen van bericht van het abonnement door in te stellen de `peek_lock` optie naar **false**.

Het standaardgedrag kunt lezen en verwijderen van een bewerking met twee fasen, waardoor dit ook ondersteuning bieden voor toepassingen die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met verwerking van het bericht (of veilig heeft opgeslagen voor toekomstige verwerking), is de tweede fase van het ontvangstproces voltooid door het aanroepen van `delete_subscription_message()` methode en het geven van het bericht moet worden verwijderd als een parameter. De `delete_subscription_message()` methode wordt het bericht als verbruikt markeren en deze te verwijderen uit het abonnement.

Als de `:peek_lock` parameter is ingesteld op **false**, lezen en verwijderen van het bericht wordt het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt. Omdat Service Bus het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint, ontbreekt het bericht dat voor het vastlopen is verbruikt.

Het volgende voorbeeld laat zien hoe berichten kunnen worden ontvangen en verwerkt met behulp `receive_subscription_message()`. In het voorbeeld eerst ontvangt en verwijdert u een bericht van de `low-messages` abonnement via `:peek_lock` ingesteld op **false**, en vervolgens het ontvangt van een ander bericht van de `high-messages` en verwijdert vervolgens het bericht met `delete_subscription_message()`:

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan niet worden verwerkt het bericht om een bepaalde reden, dan kan worden aangeroepen de `unlock_subscription_message()` methode op de **Azure::ServiceBusService** object. Dit zorgt ervoor dat Service Bus het bericht in het abonnement ontgrendelt en het beschikbaar maakt om opnieuw te worden ontvangen, ofwel door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht in het abonnement is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld als de toepassing vastloopt), en vervolgens de Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar zijn om opnieuw te ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de `delete_subscription_message()` methode wordt aangeroepen en vervolgens het bericht is opnieuw bij de toepassing bezorgd wanneer opnieuw wordt gestart. Dit wordt vaak genoemd *tenminste eenmaal verwerken*; dat wil zeggen, elk bericht ten minste één keer wordt verwerkt maar in sommige situaties hetzelfde bericht opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. Deze logica wordt vaak bereikt met de `message_id` eigenschap van het bericht dat gelijk bij meerdere bezorgingspogingen blijft.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen
Onderwerpen en abonnementen worden permanent en expliciet moet worden verwijderd via de [Azure-portal] [ Azure portal] of via een programma. Het volgende voorbeeld laat zien hoe u het onderwerp met de naam `test-topic`.

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

Als een onderwerp wordt verwijderd, worden ook alle abonnementen verwijderd die zijn geregistreerd bij het onderwerp. Abonnementen kunnen ook afzonderlijk worden verwijderd. De volgende code laat zien hoe u het abonnement met de naam `high-messages` van de `test-topic` onderwerp:

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Service Bus-onderwerpen hebt geleerd, volgt u deze koppelingen voor meer informatie.

* Zie [wachtrijen, onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md).
* API-naslaginformatie voor [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).
* Ga naar de [Azure SDK voor Ruby](https://github.com/Azure/azure-sdk-for-ruby) opslagplaats op GitHub.

[Azure portal]: https://portal.azure.com
