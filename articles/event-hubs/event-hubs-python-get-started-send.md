---
title: Gebeurtenissen verzenden naar Azure Event Hubs met behulp van Python | Microsoft Docs
description: Aan de slag verzenden van gebeurtenissen naar Event Hubs met behulp van Python
services: event-hubs
author: sethmanheim
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 10/16/2018
ms.author: sethm
ms.openlocfilehash: bb77ed69ae8f2229cbd62afa545cac9f048689e8
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49458000"
---
# <a name="send-events-to-event-hubs-using-python"></a>Gebeurtenissen verzenden naar Event Hubs met behulp van Python

Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie voor een gedetailleerd overzicht van Event Hubs [overzicht van Event Hubs](event-hubs-about.md) en [functies van Event Hubs](event-hubs-features.md).

Deze zelfstudie wordt beschreven hoe u gebeurtenissen naar een event hub verzendt vanuit een toepassing die is geschreven in Python. 

> [!NOTE]
> U kunt deze snelstartgids downloaden als een voorbeeld van de [GitHub](https://github.com/Azure/azure-event-hubs-python/tree/master/examples), Vervang `EventHubConnectionString` en `EventHubName` tekenreeksen door uw event hub-waarden en voer deze uit. U kunt ook kunt u de stappen in deze zelfstudie om te maken van uw eigen volgen.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

- Python 3.4 of hoger.


## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken
In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Als u wilt een naamruimte en een event hub maken, volgt u de procedure in [in dit artikel](event-hubs-create.md), gaat u verder met de volgende stappen in deze zelfstudie.

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

## <a name="run-application-to-send-events"></a>Voer de toepassing voor het verzenden van gebeurtenissen

Voer het script uit, open een opdrachtprompt met Python in het pad en voer vervolgens deze opdracht uit:

```bash
start python send.py
```

Gefeliciteerd! U hebt nu berichten verzonden naar een Event Hub.
 
## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u berichten verzonden naar een event hub met behulp van Python. Zie voor meer informatie over gebeurtenissen ontvangen van een event hub met behulp van Python, [ontvangen van gebeurtenissen van event hub - Python](event-hubs-python-get-started-receive.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
