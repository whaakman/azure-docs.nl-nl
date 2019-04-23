---
title: Het gebruik van Azure Service Bus-wachtrijen met Python | Microsoft Docs
description: Informatie over het gebruik van Azure Service Bus-wachtrijen met Python.
services: service-bus-messaging
documentationcenter: python
author: axisc
manager: timlt
editor: spelluru
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 04/10/2019
ms.author: aschhab
ms.openlocfilehash: 622b1f6f6a852251c07c5576ed10cd76adbf5231
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795012"
---
# <a name="how-to-use-service-bus-queues-with-python"></a>Het gebruik van Service Bus-wachtrijen met Python

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In deze zelfstudie leert u hoe u Python-toepassingen voor het verzenden van berichten naar en ontvangen van berichten van een Service Bus-wachtrij maakt. 

## <a name="prerequisites"></a>Vereisten
1. Een Azure-abonnement. U hebt een Azure-account nodig om deze zelfstudie te voltooien. U kunt uw [voordelen als MSDN-abonnee](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) zich ook aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
2. Volg de stappen in de [gebruik Azure portal voor het maken van een Service Bus-wachtrij](service-bus-quickstart-portal.md) artikel.
    1. Het snel lezen **overzicht** van Service Bus **wachtrijen**. 
    2. Maken van een Service Bus **naamruimte**. 
    3. Krijgen de **verbindingsreeks**. 

        > [!NOTE]
        > U maakt een **wachtrij** in de Service Bus-naamruimte met behulp van Python in deze zelfstudie. 
1. Python installeren of de [Python Azure Service Bus-pakket][Python Azure Service Bus package], Zie de [Python-installatiehandleiding](../python-how-to-install.md). Zie de volledige documentatie over Service Bus-Python SDK [hier](/python/api/overview/azure/servicebus?view=azure-python).

## <a name="create-a-queue"></a>Een wachtrij maken
De **ServiceBusClient** object kunt u werken met wachtrijen. Voeg de volgende code aan de bovenkant van een Python-bestand waarin u wilt programmatisch toegang verkrijgen tot de Service Bus:

```python
from azure.servicebus import ServiceBusClient
```

De volgende code maakt een **ServiceBusClient** object. Vervang `mynamespace`, `sharedaccesskeyname`, en `sharedaccesskey` met uw naamruimte, shared access signature (SAS)-sleutelnaam en -waarde.

```python
sb_client = ServiceBusClient.from_connection_string('<CONNECTION STRING>')
```

De waarden voor de SAS-sleutelnaam en de waarde kunnen worden gevonden in de [Azure-portal] [ Azure portal] verbindingsinformatie in, of in de Visual Studio **eigenschappen** deelvenster bij het selecteren van de Service Bus-naamruimte in Server Explorer (zoals weergegeven in de vorige sectie).

```python
sb_client.create_queue("taskqueue")
```

De `create_queue` methode biedt ook ondersteuning voor extra opties, zodat ze wachtrij standaardinstellingen, zoals time to live (TTL) of Maximale wachtrijgrootte bericht negeren. Het volgende voorbeeld wordt de maximale wachtrijgrootte tot 5 GB en de TTL-waarde op 1 minuut:

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

sb_client.create_queue("taskqueue", queue_options)
```

Zie voor meer informatie, [documentatie voor Azure Service Bus Python](/python/api/overview/azure/servicebus?view=azure-python).

## <a name="send-messages-to-a-queue"></a>Berichten verzenden naar een wachtrij
Een bericht verzenden naar een Service Bus-wachtrij, het aanroepen van uw toepassing de `send` methode voor het `ServiceBusClient` object.

Het volgende voorbeeld ziet u hoe u een testbericht verzenden naar de wachtrij met de naam `taskqueue` met behulp van `send_queue_message`:

```python
from azure.servicebus import QueueClient, Message

# Create the QueueClient 
queue_client = QueueClient.from_connection_string("<CONNECTION STRING>", "<QUEUE NAME>")

# Send a test message to the queue
msg = Message(b'Test Message')
queue_client.send(Message("Message"))
```

Service Bus-wachtrijen ondersteunen een maximale berichtgrootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een wachtrij, maar er is een limiet voor de totale grootte van de berichten in een wachtrij. De grootte van de wachtrij wordt gedefinieerd tijdens het aanmaken, met een bovengrens van 5 GB. Zie voor meer informatie over quota [Service Bus-quota][Service Bus quotas].

Zie voor meer informatie, [documentatie voor Azure Service Bus Python](/python/api/overview/azure/servicebus?view=azure-python).

## <a name="receive-messages-from-a-queue"></a>Berichten ontvangen van een wachtrij
Berichten worden ontvangen van een wachtrij met de `get_receiver` methode voor het `ServiceBusService` object:

```python
from azure.servicebus import QueueClient, Message

# Create the QueueClient 
queue_client = QueueClient.from_connection_string("<CONNECTION STRING>", "<QUEUE NAME>")

## Receive the message from the queue
with queue_client.get_receiver() as queue_receiver:
    messages = queue_receiver.fetch_next(timeout=3)
    for message in messages:
        print(message)
        message.complete()
```

Zie voor meer informatie, [documentatie voor Azure Service Bus Python](/python/api/overview/azure/servicebus?view=azure-python).


Berichten uit de wachtrij worden verwijderd als ze worden gelezen wanneer de parameter `peek_lock` is ingesteld op **False**. U kunt lezen (peek) en een vergrendeling van het bericht zonder deze te verwijderen uit de wachtrij door de parameter `peek_lock` naar **waar**.

Het gedrag van lezen en verwijderen van het bericht als onderdeel van de ontvangstbewerking, is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt. Omdat Service Bus het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint, ontbreekt het bericht dat voor het vastlopen is verbruikt.

Als de `peek_lock` parameter is ingesteld op **waar**, wordt de ontvangst een bewerking met twee fasen, waardoor er toepassingen kunnen worden ondersteund die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met het verwerken van bericht (of deze op betrouwbare wijze voor toekomstige verwerking slaat), is de tweede fase van het ontvangstproces voltooid door het aanroepen van de **verwijderen** methode voor het **bericht** -object. De **verwijderen** methode wordt het bericht als verbruikt markeren en deze te verwijderen uit de wachtrij.

```python
msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan het bericht te verwerken voor een of andere reden niet, dan kan worden aangeroepen de **ontgrendelen** methode voor het **bericht** object. Dit zorgt ervoor dat Service Bus het bericht in de wachtrij ontgrendelt en het toegankelijk maken voor het opnieuw worden ontvangen door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht dat in de wachtrij is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling is verlopen (bijvoorbeeld, als de toepassing vastloopt), Service Bus wordt het bericht automatisch ontgrendelen en maken beschikbaar voor het opnieuw worden ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de **verwijderen** methode wordt aangeroepen, wordt het bericht zal opnieuw worden bezorgd bij de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **ten minste één keer worden verwerkt**, dat wil zeggen, wordt elk bericht ten minste één keer worden verwerkt, maar in bepaalde situaties hetzelfde bericht opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. Dit wordt vaak bereikt met de **MessageId**-eigenschap van het bericht dat gelijk blijft bij meerdere bezorgingspogingen.

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van Service Bus-wachtrijen hebt geleerd, Zie de volgende artikelen voor meer informatie.

* [Wachtrijen, onderwerpen en abonnementen][Queues, topics, and subscriptions]

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

