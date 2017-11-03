---
title: Azure Service Bus-wachtrijen gebruiken met Python | Microsoft Docs
description: Informatie over het gebruik van Azure Service Bus-wachtrijen met Python.
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm;lmazuel
ms.openlocfilehash: e1e81ad1d7b4fe0e044917f090cac59dfd5b6332
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-queues-with-python"></a>Service Bus-wachtrijen gebruiken met Python

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In dit artikel wordt het gebruik van de Service Bus-wachtrijen beschreven. De voorbeelden zijn geschreven in Python en gebruik de [Python Azure Service Bus-pakket][Python Azure Service Bus package]. De scenario's worden behandeld: **maken van wachtrijen, verzenden en ontvangen berichten**, en **verwijderen van wachtrijen**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

> [!NOTE]
> Om Python te installeren of de [Python Azure Service Bus-pakket][Python Azure Service Bus package], Zie de [Python installatiehandleiding](../python-how-to-install.md).
> 
> 

## <a name="create-a-queue"></a>Een wachtrij maken
De **ServiceBusService** object kunt u werken met wachtrijen. Voeg de volgende code aan de bovenkant van een Python-bestand waarin u wilt programmatisch toegang biedt tot de Service Bus:

```python
from azure.servicebus import ServiceBusService, Message, Queue
```

De volgende code maakt een **ServiceBusService** object. Vervang `mynamespace`, `sharedaccesskeyname`, en `sharedaccesskey` met uw naamruimte, naam van shared access signature (SAS)-sleutel en waarde.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

De waarden voor de SAS-sleutelnaam en de waarde kunnen worden gevonden in de [Azure-portal] [ Azure portal] verbindingsinformatie, of in de Visual Studio **eigenschappen** deelvenster bij het selecteren van de Service Bus-naamruimte in Server Explorer (zoals weergegeven in de vorige sectie).

```python
bus_service.create_queue('taskqueue')
```

De `create_queue` methode biedt ook ondersteuning voor extra opties waarmee u kunt de standaardinstellingen voor wachtrij zoals bericht live (TTL) of de maximale wachtrijgrootte negeren. Het volgende voorbeeld wordt de maximale wachtrijgrootte ingesteld op 5 GB en de TTL-waarde op 1 minuut:

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Berichten verzenden naar een wachtrij
Een bericht te verzenden naar een Service Bus-wachtrij, het aanroepen van uw toepassing de `send_queue_message` methode op de **ServiceBusService** object.

Het volgende voorbeeld laat zien hoe een testbericht verzenden naar de wachtrij met de naam `taskqueue` met `send_queue_message`:

```python
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Service Bus-wachtrijen ondersteunen een maximale berichtgrootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een wachtrij, maar er is een limiet voor de totale grootte van de berichten in een wachtrij. De grootte van de wachtrij wordt gedefinieerd tijdens het aanmaken, met een bovengrens van 5 GB. Zie voor meer informatie over quota [Service Bus-quota][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Berichten ontvangen uit een wachtrij
Berichten worden ontvangen van een wachtrij met de `receive_queue_message` methode op de **ServiceBusService** object:

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Berichten uit de wachtrij worden verwijderd als ze worden gelezen wanneer de parameter `peek_lock` is ingesteld op **False**. U kunt lezen (peek) en het bericht zonder het te verwijderen uit de wachtrij door de parameter vergrendelen `peek_lock` naar **True**.

Het gedrag van lezen van en het bericht is verwijderd als onderdeel van de bewerking receive is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt. Omdat Service Bus het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint, ontbreekt het bericht dat voor het vastlopen is verbruikt.

Als de `peek_lock` parameter is ingesteld op **True**, wordt het ontvangen, wordt een bewerking met twee fasen, waardoor er mogelijk worden ondersteuning voor toepassingen die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met verwerking van het bericht (of veilig heeft opgeslagen voor toekomstige verwerking), is de tweede fase van het ontvangstproces voltooid door het aanroepen van de **verwijderen** methode op de **bericht** object. De **verwijderen** methode wordt het bericht als verbruikt markeren en verwijderen uit de wachtrij.

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan niet worden verwerkt het bericht om een bepaalde reden, dan kan worden aangeroepen de **ontgrendelen** methode op de **bericht** object. Dit zorgt ervoor dat Service Bus het bericht in de wachtrij ontgrendelt en beschikbaar zijn om opnieuw te ontvangen, ofwel door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht in de wachtrij is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld als de toepassing vastloopt), en vervolgens de Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar zijn om opnieuw te ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de **verwijderen** methode wordt aangeroepen en vervolgens het bericht opnieuw bezorgd bij de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **tenminste eenmaal verwerken**, dat wil zeggen elk bericht ten minste één keer wordt verwerkt maar in sommige situaties hetzelfde bericht opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. Dit wordt vaak bereikt met behulp van de **MessageId** eigenschap van het bericht dat gelijk bij meerdere bezorgingspogingen blijft.

## <a name="next-steps"></a>Volgende stappen
U kunt de basisprincipes van Service Bus-wachtrijen hebt geleerd, Zie de volgende artikelen voor meer informatie.

* [Wachtrijen, onderwerpen en abonnementen][Queues, topics, and subscriptions]

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

