---
title: Het gebruik van Service Bus-onderwerpen (Ruby) | Microsoft Docs
description: Meer informatie over het gebruik van Service Bus-onderwerpen en abonnementen in Azure. Codevoorbeelden zijn geschreven voor Ruby-toepassingen.
services: service-bus-messaging
documentationcenter: ruby
author: spelluru
manager: timlt
editor: ''
ms.assetid: 3ef2295e-7c5f-4c54-a13b-a69c8045d4b6
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2018
ms.author: spelluru
ms.openlocfilehash: 0310b532fae69aa2509a427b73c40ba732a740de
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54102310"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-ruby"></a>Service Bus-onderwerpen en abonnementen gebruiken met Ruby
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dit artikel wordt beschreven hoe u Service Bus-onderwerpen en abonnementen van Ruby-toepassingen. De behandelde scenario's zijn **het maken van onderwerpen en abonnementen, het maken van abonnementsfilters, het verzenden van berichten** naar een onderwerp **ontvangen van berichten van een abonnement**, en **verwijderen onderwerpen en abonnementen**. Zie voor meer informatie over onderwerpen en abonnementen, de [Vervolgstappen](#next-steps) sectie.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a>Een onderwerp maken
De **Azure::ServiceBusService** object kunt u werken met onderwerpen. De volgende code maakt een **Azure::ServiceBusService** object. Gebruik voor het maken van een onderwerp de `create_topic()` methode. Het volgende voorbeeld wordt een onderwerp gemaakt of eventuele fouten afgedrukt.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_topic("test-topic")
rescue
  puts $!
end
```

U kunt ook doorgeven een **Azure::ServiceBus::Topic** object met extra opties waarmee u kunt de standaardinstellingen van onderwerp zoals time to live of Maximale wachtrijgrootte bericht negeren. Het volgende voorbeeld ziet u de maximale wachtrijgrootte naar 5 GB en tijd op live op 1 minuut instellen:

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Maken van abonnementen
Ook onderwerpabonnementen worden gemaakt met de **Azure::ServiceBusService** object. Abonnementen zijn met de naam en een optioneel filter dat u Hiermee beperkt u de verzameling berichten die worden geleverd aan de virtuele wachtrij van het abonnement kunnen hebben.

Abonnementen zijn permanent. Ze blijven totdat een van beide ze bestaan of het onderwerp zijn gekoppeld, worden verwijderd. Als uw toepassing bevat de logica voor het maken van een abonnement, moet er eerst gecontroleerd of het abonnement dat al bestaat met behulp van de methode getSubscription.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)
Als geen filter is opgegeven wanneer een nieuw abonnement wordt gemaakt, de **MatchAll** filter (standaard) wordt gebruikt. Wanneer de **MatchAll** filter wordt gebruikt, worden alle berichten die zijn gepubliceerd naar het onderwerp in de virtuele wachtrij van het abonnement geplaatst. Het volgende voorbeeld maakt u een abonnement met de naam 'all '-berichten en gebruikmaakt van de **MatchAll** filter.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken
Ook kunt u filters waarmee u kunt opgeven welke berichten die worden verzonden naar een onderwerp binnen een specifiek abonnement weergegeven.

Het meest flexibele type filter dat wordt ondersteund door abonnementen is de **Azure::ServiceBus::SqlFilter**, die wordt geïmplementeerd met een subset van SQL92 wordt geïmplementeerd. SQL-filters worden uitgevoerd voor de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Raadpleeg voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter de [SqlFilter](service-bus-messaging-sql-filter.md) syntaxis.

U kunt filters toevoegen aan een abonnement met behulp van de `create_rule()` -methode van de **Azure::ServiceBusService** object. Deze methode kunt u nieuwe filters toevoegen aan een bestaand abonnement.

Omdat het standaardfilter automatisch op alle nieuwe abonnementen toegepast wordt, moet u eerst het standaardfilter verwijderen of de **MatchAll** overschrijft alle andere filters die u kunt opgeven. U kunt de standaardregel verwijderen met behulp van de `delete_rule()` methode voor het **Azure::ServiceBusService** object.

Het volgende voorbeeld wordt een abonnement met de naam 'hoog '-berichten met een **Azure::ServiceBus::SqlFilter** dat alleen berichten selecteert die een aangepaste `message_number` eigenschap groter dan 3:

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

Op deze manier in het volgende voorbeeld wordt een abonnement genaamd `low-messages` met een **Azure::ServiceBus::SqlFilter** dat alleen berichten selecteert die een `message_number` eigenschap minder dan of gelijk aan 3:

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

Wanneer nu een bericht is verzonden naar `test-topic`, wordt altijd worden bezorgd bij ontvangers die zijn geabonneerd op de `all-messages` onderwerpabonnement en selectief bezorgd bij ontvangers die zijn geabonneerd op de `high-messages` en `low-messages` onderwerpabonnementen (afhankelijk van bij de berichtinhoud).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp
Voor het verzenden van een bericht naar een Service Bus-onderwerp, uw toepassing moet gebruikmaken van de `send_topic_message()` methode voor het **Azure::ServiceBusService** object. Berichten verzonden naar Service Bus-onderwerpen zijn exemplaren van de **Azure::ServiceBus::BrokeredMessage** objecten. **Azure::ServiceBus::BrokeredMessage** objecten hebben een aantal standaardeigenschappen (zoals `label` en `time_to_live`), een woordenlijst die wordt gebruikt om aangepaste toepassingsspecifieke eigenschappen te bewaren en een hoofdtekst met tekenreeksen. Een toepassing de hoofdtekst van het bericht worden ingesteld door door te geven van een string-waarde voor de `send_topic_message()` methode en alle vereiste standaardeigenschappen zijn gevuld met standaardwaarden.

Het volgende voorbeeld ziet u hoe u voor het verzenden van vijf testberichten naar `test-topic`. De `message_number` aangepaste eigenschapswaarde van elk bericht varieert op de herhaling van de lus (deze bepaalt welk abonnement u ontvangt deze):

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Service Bus-onderwerpen ondersteunen een maximale grootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een onderwerp, maar er is een limiet voor de totale grootte van de berichten in een onderwerp. De grootte van het onderwerp wordt gedefinieerd tijdens het maken, met een bovengrens van 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangen van een abonnement
Berichten worden ontvangen van een abonnement met de `receive_subscription_message()` methode voor het **Azure::ServiceBusService** object. Standaard worden berichten zijn read(peak) en zonder deze te verwijderen uit het abonnement is vergrendeld. U kunt lezen en verwijderen van het bericht van het abonnement door in te stellen de `peek_lock` optie naar **false**.

Het standaardgedrag kunt lezen en verwijderen van een bewerking met twee fasen, waardoor dit ook ondersteuning bieden voor toepassingen die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met het verwerken van bericht (of deze op betrouwbare wijze voor toekomstige verwerking slaat), is de tweede fase van het ontvangstproces voltooid door het aanroepen van `delete_subscription_message()` methode en het geven van het bericht moet worden verwijderd als een parameter. De `delete_subscription_message()` methode wordt het bericht als verbruikt gemarkeerd en verwijderd uit het abonnement.

Als de `:peek_lock` parameter is ingesteld op **false**, lezen en verwijderen van het bericht wordt het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing niet verwerken van een bericht tolereren kan wanneer er een fout optreedt. U hebt een scenario waarin de consument problemen met de aanvraag ontvangen en vervolgens vastloopt voordat deze wordt verwerkt. Omdat Service Bus kan het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens ontbreekt wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint het het bericht dat voor het vastlopen is verbruikt.

Het volgende voorbeeld laat zien hoe berichten kunnen worden ontvangen en verwerkt met behulp `receive_subscription_message()`. Het voorbeeld eerst ontvangt en Hiermee verwijdert u een bericht van de `low-messages` abonnement met behulp van `:peek_lock` ingesteld op **false**, en vervolgens wordt ontvangen van een ander bericht uit de `high-messages` en verwijdert het bericht met behulp van `delete_subscription_message()`:

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan het bericht te verwerken voor een of andere reden niet, dan kan worden aangeroepen de `unlock_subscription_message()` methode voor het **Azure::ServiceBusService** object. Dit zorgt ervoor dat Service Bus het bericht in het abonnement ontgrendelt en het toegankelijk maken voor het opnieuw worden ontvangen door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht dat in het abonnement is vergrendeld, en als de toepassing niet kan het bericht te verwerken voordat u de vergrendeling time-out is verlopen (bijvoorbeeld, als de toepassing vastloopt), wordt de Service Bus het bericht automatisch ontgrendelen en maakt u deze beschikbaar zijn om opnieuw te ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de `delete_subscription_message()` methode wordt aangeroepen, wordt het bericht is opnieuw naar de toepassing bezorgd wanneer deze opnieuw wordt opgestart. Dit wordt vaak genoemd *ten minste één keer worden verwerkt*; dat wil zeggen, elk bericht ten minste één keer wordt verwerkt, maar in bepaalde situaties hetzelfde bericht opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. Deze logica wordt vaak bereikt met behulp van de `message_id` eigenschap van het bericht dat gelijk bij meerdere bezorgingspogingen blijft.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen
Onderwerpen en abonnementen persistent zijn en moeten expliciet worden verwijderd via de [Azure-portal] [ Azure portal] of via een programma. Het volgende voorbeeld ziet u hoe u wilt verwijderen van het onderwerp met de naam `test-topic`.

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

Als een onderwerp wordt verwijderd, worden ook alle abonnementen verwijderd die zijn geregistreerd bij het onderwerp. Abonnementen kunnen ook afzonderlijk worden verwijderd. De volgende code laat zien hoe u het abonnement met de naam wilt verwijderen `high-messages` uit de `test-topic` onderwerp:

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van Service Bus-onderwerpen hebt geleerd, volgt u deze koppelingen voor meer informatie.

* Zie [wachtrijen, onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md).
* API-naslaginformatie voor [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).
* Ga naar de [Azure SDK voor Ruby](https://github.com/Azure/azure-sdk-for-ruby) -bibliotheek op GitHub.

[Azure portal]: https://portal.azure.com
