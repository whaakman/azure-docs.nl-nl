---
title: Gebeurtenissen verzenden naar Azure Event Hubs met behulp van Python | Microsoft Docs
description: Aan de slag verzenden van gebeurtenissen naar Event Hubs met behulp van Python
services: event-hubs
author: sethmanheim
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/26/2018
ms.author: sethm
ms.openlocfilehash: 762e21cfc7d16b614eb637c569f8bfc5b6115db1
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43703649"
---
# <a name="send-events-to-event-hubs-using-python"></a>Gebeurtenissen verzenden naar Event Hubs met behulp van Python

Azure Event Hubs is een uiterst schaalbare gebeurtenissen beheersysteem dat miljoenen verwerken kan gebeurtenissen per seconde, zodat toepassingen om te verwerken en analyseren van enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en andere systemen. Zodra de verzameld in een event hub, u kunt ontvangen en verwerken van gebeurtenissen met in-process-handlers of doorsturen naar andere systemen analytics.

Zie voor meer informatie over Event Hubs, de [overzicht van Event Hubs][Event Hubs overview].

Deze zelfstudie wordt beschreven hoe u gebeurtenissen naar een event hub verzendt vanuit een toepassing die is geschreven in Python. Zie voor het ontvangen van gebeurtenissen, [de betreffende ontvangen artikel](event-hubs-python-get-started-receive.md).

Code in deze zelfstudie is afkomstig uit [deze GitHub-voorbeelden](https://github.com/Azure/azure-event-hubs-python/tree/master/examples), die u kunt controleren om te zien van de volledige werkende toepassing, met inbegrip van importinstructies en variabelendeclaraties. Andere voorbeelden zijn beschikbaar in de map van GitHub.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

- Python 3.4 of hoger.
- Een bestaande Event Hubs-naamruimte en event hub. U kunt deze entiteiten maken door de instructies in [in dit artikel](event-hubs-create.md). 

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]


## <a name="install-python-package"></a>Python-pakket installeren

Open een opdrachtprompt met Python in het pad voor het installeren van het Python-pakket voor Event Hubs, en voer vervolgens deze opdracht uit: 

```bash
pip install azure-eventhub
```

## <a name="create-a-python-script-to-send-events"></a>Maken van een Python-script voor het verzenden van gebeurtenissen

Maak vervolgens een Python-toepassing die gebeurtenissen naar een event hub verzendt:

1. Open uw favoriete Python-editor, zoals [Visual Studio Code][Visual Studio Code].
2. Maken van een script met de naam **send.py**. Met dit script verzonden 100 gebeurtenissen naar uw event hub.
3. Plak de volgende code in send.py, vervang de waarden van de adres-, gebruikers- en -sleutel door de waarden die u hebt verkregen via de Azure portal in de vorige sectie: 

```python
import sys
import logging
import datetime
import time
import os

from azure.eventhub import EventHubClient, Sender, EventData

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
# "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.windows.net/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"

try:
    if not ADDRESS:
        raise ValueError("No EventHubs URL supplied.")

    # Create Event Hubs client
    client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
    sender = client.add_sender(partition="0")
    client.run()
    try:
        start_time = time.time()
        for i in range(100):
            print("Sending message: {}".format(i))
            sender.send(EventData(str(i)))
    except:
        raise
    finally:
        end_time = time.time()
        client.stop()
        run_time = end_time - start_time
        logger.info("Runtime: {} seconds".format(run_time))

except KeyboardInterrupt:
    pass
```

## <a name="send-events"></a>Gebeurtenissen verzenden

Voer het script uit, open een opdrachtprompt met Python in het pad en voer vervolgens deze opdracht uit:

```bash
start python send.py
```
 
## <a name="next-steps"></a>Volgende stappen

Nu dat u gebeurtenissen naar een event hub met behulp van Python verzonden hebt, voor het ontvangen van gebeurtenissen ziet [de betreffende ontvangen artikel](event-hubs-python-get-started-receive.md).

Ga naar de volgende pagina's voor meer informatie over Event Hubs:

* [Event Hubs-overzicht][Event Hubs overview]
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
