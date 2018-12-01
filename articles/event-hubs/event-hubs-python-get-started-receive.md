---
title: Gebeurtenissen ontvangen van Azure Event Hubs met behulp van Python | Microsoft Docs
description: Aan de slag voor het ontvangen van gebeurtenissen van Event Hubs met behulp van Python
services: event-hubs
author: ShubhaVijayasarathy
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 11/26/2018
ms.author: shvija
ms.openlocfilehash: 8009a2ef3f89a359bc645485dd75b80cc5d0b868
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52722833"
---
# <a name="receive-events-from-event-hubs-using-python"></a>Gebeurtenissen ontvangen van Event Hubs met behulp van Python

Azure Event Hubs is een uiterst schaalbare gebeurtenissen beheersysteem dat miljoenen verwerken kan gebeurtenissen per seconde, zodat toepassingen om te verwerken en analyseren van enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en andere systemen. Zodra de verzameld in een event hub, u kunt ontvangen en verwerken van gebeurtenissen met in-process-handlers of doorsturen naar andere systemen analytics.

Zie voor meer informatie over Event Hubs, de [overzicht van Event Hubs][Event Hubs overview].

In deze zelfstudie wordt beschreven hoe u gebeurtenissen ontvangen van een event hub vanuit een toepassing die is geschreven in Python. Zie voor het verzenden van gebeurtenissen, [de betreffende verzenden artikel](event-hubs-python-get-started-send.md).

Code in deze zelfstudie is afkomstig uit [deze GitHub-voorbeelden](https://github.com/Azure/azure-event-hubs-python/tree/master/examples), die u kunt controleren om te zien van de volledige werkende toepassing, met inbegrip van importinstructies en variabelendeclaraties. Andere voorbeelden zijn beschikbaar in de map van GitHub.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

- Een Azure-abonnement. Als u nog geen abonnement hebt, [maakt u een gratis account](https://azure.microsoft.com/free/) voordat u begint.
- Python 3.4 of hoger.
- Een bestaande Event Hubs-naamruimte en event hub. U kunt deze entiteiten maken door de instructies in [in dit artikel](event-hubs-create.md). 

## <a name="install-python-package"></a>Python-pakket installeren

Open een opdrachtprompt met Python in het pad voor het installeren van het Python-pakket voor Event Hubs, en voer vervolgens deze opdracht uit: 

```bash
pip install azure-eventhub
```

## <a name="create-a-python-script-to-receive-events"></a>Maken van een Python-script voor het ontvangen van gebeurtenissen

Maak vervolgens een Python-toepassing die gebeurtenissen vanaf een event hub ontvangt:

1. Open uw favoriete Python-editor, zoals [Visual Studio Code][Visual Studio Code].
2. Maken van een script met de naam **recv.py**.
3. Plak de volgende code in recv.py, vervang de waarden van de adres-, gebruikers- en -sleutel door de waarden die u hebt verkregen via de Azure portal in de vorige sectie: 

```python
import os
import sys
import logging
import time
from azure.eventhub import EventHubClient, Receiver, Offset

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
# "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.windows.net/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"
CONSUMER_GROUP = "$default"
OFFSET = Offset("-1")
PARTITION = "0"

total = 0
last_sn = -1
last_offset = "-1"
client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
try:
    receiver = client.add_receiver(CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)
    client.run()
    start_time = time.time()
    for event_data in receiver.receive(timeout=100):
        last_offset = event_data.offset
        last_sn = event_data.sequence_number
        print("Received: {}, {}".format(last_offset, last_sn))
        total += 1

    end_time = time.time()
    client.stop()
    run_time = end_time - start_time
    print("Received {} messages in {} seconds".format(total, run_time))

except KeyboardInterrupt:
    pass
finally:
    client.stop()
```

## <a name="receive-events"></a>Gebeurtenissen ontvangen

Voer het script uit, open een opdrachtprompt met Python in het pad en voer vervolgens deze opdracht uit:

```bash
start python recv.py
```
 
## <a name="next-steps"></a>Volgende stappen
In deze quickstart maakt u een Python-toepassing die berichten van een event hub ontvangen gemaakt. Zie voor meer informatie over het verzenden van gebeurtenissen naar een event hub met behulp van Python, [verzenden van gebeurtenissen van event hub - Python](event-hubs-python-get-started-send.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
