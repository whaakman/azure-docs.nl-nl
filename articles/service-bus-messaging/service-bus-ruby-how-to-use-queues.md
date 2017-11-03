---
title: Azure Service Bus-wachtrijen gebruiken met Ruby | Microsoft Docs
description: Informatie over het gebruiken van Service Bus-wachtrijen in Azure Codevoorbeelden geschreven in Ruby.
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 0a11eab2-823f-4cc7-842b-fbbe0f953751
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 357a7277dd42b6973cf35a9f642b8eec36a745e3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-queues-with-ruby"></a>Service Bus-wachtrijen gebruiken met Ruby

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Deze handleiding wordt beschreven hoe u Service Bus-wachtrijen. De voorbeelden zijn geschreven in Ruby en gebruiken van de Azure-gem. De scenario's worden behandeld: **maken van wachtrijen, verzenden en ontvangen berichten**, en **verwijderen van wachtrijen**. Zie voor meer informatie over Service Bus-wachtrijen, de [Vervolgstappen](#next-steps) sectie.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]
   
[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="how-to-create-a-queue"></a>Een wachtrij maken
De **Azure::ServiceBusService** object kunt u werken met wachtrijen. Gebruik voor het maken van een wachtrij de `create_queue()` methode. Het volgende voorbeeld maakt u een wachtrij of eventuele fouten wordt afgedrukt.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

U kunt ook doorgeven een **Azure::ServiceBus::Queue** object met aanvullende opties, zodat u de standaardinstellingen wachtrij, zoals de live of Maximale wachtrijgrootte bericht negeren. Het volgende voorbeeld ziet u hoe u de maximale wachtrijgrootte ingesteld op 5 GB en tijd op 1 minuut TTL:

```ruby
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Het verzenden van berichten naar een wachtrij
Een bericht te verzenden naar een Service Bus-wachtrij, het aanroepen van uw toepassing de `send_queue_message()` methode op de **Azure::ServiceBusService** object. Berichten verzonden naar (en ontvangen van) Service Bus-wachtrijen zijn **Azure::ServiceBus::BrokeredMessage** objecten en hebben een aantal standaardeigenschappen (zoals `label` en `time_to_live`), een woordenlijst die wordt gebruikt om aangepaste toepassingsspecifieke eigenschappen te bewaren en een hoofdtekst met willekeurige toepassingsgegevens. Een toepassing kunt instellen in de hoofdtekst van het bericht een string-waarde als het bericht wordt doorgegeven en eventueel vereiste eigenschappen standaard worden ingevuld met standaardwaarden.

Het volgende voorbeeld laat zien hoe een testbericht verzenden naar de wachtrij met de naam `test-queue` met `send_queue_message()`:

```ruby
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Service Bus-wachtrijen ondersteunen een maximale berichtgrootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een wachtrij, maar er is een limiet voor de totale grootte van de berichten in een wachtrij. De grootte van de wachtrij wordt gedefinieerd tijdens het aanmaken, met een bovengrens van 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Berichten van een wachtrij ontvangen
Berichten worden ontvangen van een wachtrij met de `receive_queue_message()` methode op de **Azure::ServiceBusService** object. Standaard worden berichten lezen en zonder wordt verwijderd uit de wachtrij is vergrendeld. Echter, u kunt berichten verwijderen uit de wachtrij als ze worden gelezen door het instellen van de `:peek_lock` optie naar **false**.

Het standaardgedrag kunt lezen en verwijderen van een bewerking met twee fasen, waardoor dit ook ondersteuning bieden voor toepassingen die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met verwerking van het bericht (of veilig heeft opgeslagen voor toekomstige verwerking), is de tweede fase van het ontvangstproces voltooid door het aanroepen van `delete_queue_message()` methode en het geven van het bericht moet worden verwijderd als een parameter. De `delete_queue_message()` methode wordt het bericht als verbruikt markeren en verwijderen uit de wachtrij.

Als de `:peek_lock` parameter is ingesteld op **false**, lezen en verwijderen van het bericht wordt het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt. Omdat Service Bus is gemarkeerd als het bericht als verbruikt, wanneer de toepassing opnieuw wordt opgestart en verbruik van berichten opnieuw begint, ontbreekt het bericht dat voor het vastlopen is verbruikt.

Het volgende voorbeeld laat zien hoe ontvangen en verwerken van berichten met `receive_queue_message()`. In het voorbeeld eerst ontvangt en verwijdert van een bericht met behulp van `:peek_lock` ingesteld op **false**, en vervolgens het nog een bericht ontvangt en vervolgens het bericht met verwijdert `delete_queue_message()`:

```ruby
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan niet worden verwerkt het bericht om een bepaalde reden, dan kan worden aangeroepen de `unlock_queue_message()` methode op de **Azure::ServiceBusService** object. Deze aanroep zorgt ervoor dat Service Bus het bericht in de wachtrij ontgrendelt en het beschikbaar worden om opnieuw te ontvangen, ofwel door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht in de wachtrij is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld als de toepassing vastloopt), en vervolgens de Service Bus het bericht automatisch ontgrendelt en wordt het beschikbaar worden om opnieuw te ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de `delete_queue_message()` methode wordt aangeroepen en vervolgens het bericht is opnieuw bij de toepassing bezorgd wanneer opnieuw wordt gestart. Dit proces wordt vaak genoemd *tenminste eenmaal verwerken*; dat wil zeggen, elk bericht ten minste één keer wordt verwerkt maar in sommige situaties hetzelfde bericht opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. Dit wordt vaak bereikt met behulp van de `message_id` eigenschap van het bericht dat gelijk blijft bij meerdere bezorgingspogingen.

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Service Bus-wachtrijen hebt geleerd, kunt u deze koppelingen volgen voor meer informatie.

* Overzicht van [wachtrijen, onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md).
* Ga naar de [Azure SDK voor Ruby](https://github.com/Azure/azure-sdk-for-ruby) opslagplaats op GitHub.

Voor een vergelijking tussen de Azure Service Bus-wachtrijen in dit artikel besproken en de Azure-wachtrijen beschreven in de [hoe Queue storage gebruiken met Ruby](../storage/queues/storage-ruby-how-to-use-queue-storage.md) artikel, Zie [Azure wachtrijen en Azure Service Bus-wachtrijen - vergeleken en tegenstelling tot](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

