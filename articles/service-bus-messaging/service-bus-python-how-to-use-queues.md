---
title: Het gebruik van Azure Service Bus-wachtrijen met Python | Microsoft Docs
description: Informatie over het gebruik van Azure Service Bus-wachtrijen met Python.
services: service-bus-messaging
documentationcenter: python
author: spelluru
manager: timlt
editor: ''
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/30/2018
ms.author: spelluru
ms.openlocfilehash: cc8fc62811fe019dcaf613a1c0b50d89183e6555
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47406189"
---
# <a name="how-to-use-service-bus-queues-with-python"></a>Het gebruik van Service Bus-wachtrijen met Python

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In dit artikel wordt het gebruik van de Service Bus-wachtrijen beschreven. De voorbeelden zijn geschreven in Python en gebruik de [Python Azure Service Bus-pakket][Python Azure Service Bus package]. De behandelde scenario's zijn **maken van wachtrijen, verzenden en ontvangen van berichten**, en **verwijderen van wachtrijen**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

> [!NOTE]
> Om Python te installeren of de [Python Azure Service Bus-pakket][Python Azure Service Bus package], Zie de [Python-installatiehandleiding](../python-how-to-install.md).
> 
> 

## <a name="create-a-queue"></a>Een wachtrij maken
De **ServiceBusService** object kunt u werken met wachtrijen. Voeg de volgende code aan de bovenkant van een Python-bestand waarin u wilt programmatisch toegang verkrijgen tot de Service Bus:

```python
from azure.servicebus import ServiceBusService, Message, Queue
```

De volgende code maakt een **ServiceBusService** object. Vervang `mynamespace`, `sharedaccesskeyname`, en `sharedaccesskey` met uw naamruimte, shared access signature (SAS)-sleutelnaam en -waarde.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

De waarden voor de SAS-sleutelnaam en de waarde kunnen worden gevonden in de [Azure-portal] [ Azure portal] verbindingsinformatie in, of in de Visual Studio **eigenschappen** deelvenster bij het selecteren van de Service Bus-naamruimte in Server Explorer (zoals weergegeven in de vorige sectie).

```python
bus_service.create_queue('taskqueue')
```

De `create_queue` methode biedt ook ondersteuning voor extra opties, zodat ze wachtrij standaardinstellingen, zoals time to live (TTL) of Maximale wachtrijgrootte bericht negeren. Het volgende voorbeeld wordt de maximale wachtrijgrootte tot 5 GB en de TTL-waarde op 1 minuut:

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Berichten verzenden naar een wachtrij
Een bericht verzenden naar een Service Bus-wachtrij, het aanroepen van uw toepassing de `send_queue_message` methode voor het **ServiceBusService** object.

Het volgende voorbeeld ziet u hoe u een testbericht verzenden naar de wachtrij met de naam `taskqueue` met behulp van `send_queue_message`:

```python
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Service Bus-wachtrijen ondersteunen een maximale berichtgrootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een wachtrij, maar er is een limiet voor de totale grootte van de berichten in een wachtrij. De grootte van de wachtrij wordt gedefinieerd tijdens het aanmaken, met een bovengrens van 5 GB. Zie voor meer informatie over quota [Service Bus-quota][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Berichten ontvangen van een wachtrij
Berichten worden ontvangen van een wachtrij met de `receive_queue_message` methode voor het **ServiceBusService** object:

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Berichten uit de wachtrij worden verwijderd als ze worden gelezen wanneer de parameter `peek_lock` is ingesteld op **False**. U kunt lezen (peek) en een vergrendeling van het bericht zonder deze te verwijderen uit de wachtrij door de parameter `peek_lock` naar **waar**.

Het gedrag van lezen en verwijderen van het bericht als onderdeel van de ontvangstbewerking, is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt. Omdat Service Bus het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint, ontbreekt het bericht dat voor het vastlopen is verbruikt.

Als de `peek_lock` parameter is ingesteld op **waar**, wordt de ontvangst een bewerking met twee fasen, waardoor er toepassingen kunnen worden ondersteund die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met het verwerken van bericht (of deze op betrouwbare wijze voor toekomstige verwerking slaat), is de tweede fase van het ontvangstproces voltooid door het aanroepen van de **verwijderen** methode voor het **bericht** -object. De **verwijderen** methode wordt het bericht als verbruikt markeren en deze te verwijderen uit de wachtrij.

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan het bericht te verwerken voor een of andere reden niet, dan kan worden aangeroepen de **ontgrendelen** methode voor het **bericht** object. Dit zorgt ervoor dat Service Bus het bericht in de wachtrij ontgrendelt en het toegankelijk maken voor het opnieuw worden ontvangen door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht dat in de wachtrij is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling is verlopen (bijvoorbeeld, als de toepassing vastloopt), Service Bus wordt het bericht automatisch ontgrendelen en maken beschikbaar voor het opnieuw worden ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de **verwijderen** methode wordt aangeroepen, wordt het bericht zal opnieuw worden bezorgd bij de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **tenminste eenmaal verwerken**, dat wil zeggen, wordt elk bericht ten minste één keer worden verwerkt, maar in bepaalde situaties hetzelfde bericht opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. Dit wordt vaak bereikt met behulp van de **MessageId** eigenschap van het bericht, bij meerdere bezorgingspogingen blijft.

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van Service Bus-wachtrijen hebt geleerd, Zie de volgende artikelen voor meer informatie.

* [Wachtrijen, onderwerpen en abonnementen][Queues, topics, and subscriptions]

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

