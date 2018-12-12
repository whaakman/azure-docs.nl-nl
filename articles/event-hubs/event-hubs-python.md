---
title: Python gebruiken om te verzenden en ontvangen van berichten - Azure Event Hubs | Microsoft Docs
description: Informatie over het verzenden van gebeurtenissen die moeten worden ontvangen van gebeurtenissen uit en vastleggen van gebeurtenissen streamen via Event Hubs met behulp van Python.
keywords: ''
documentationcenter: ''
services: event-hubs
author: ShubhaVijayasarathy
manager: ''
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/26/2018
ms.author: shvija
ms.openlocfilehash: 4a0e2cd7e0c768512e1aafc042fe55338fdc206e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53084971"
---
# <a name="how-to-use-azure-event-hubs-from-a-python-application"></a>Het gebruik van Azure Event Hubs in een Python-toepassing
Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie voor meer informatie, [Inleiding tot Event Hubs](event-hubs-what-is-event-hubs.md). 

In dit artikel vindt u koppelingen naar artikelen die laten hoe u de volgende taken uitvoeren vanuit een toepassing die is geschreven zien in u **Python**:

- [Gebeurtenissen verzenden naar een event hub](#send-events-to-event-hubs)
- [Gebeurtenissen ontvangen van een event hub](#receive-events-from-event-hubs)
- [Lezen vastgelegd gebeurtenisgegevens van een Azure-opslag](#capture-event-hubs-data). 

## <a name="prerequisites"></a>Vereisten
- Een event hub maken door de volgende uitvoer van deze Quick starts: [Azure-portal](event-hubs-create.md), [Azure CLI](event-hubs-quickstart-cli.md), [Azure PowerShell](event-hubs-quickstart-powershell.md), [Azure Resource Manager-sjabloon](event-hubs-resource-manager-namespace-event-hub.md). 
- Python 3.4 of hoger is geïnstalleerd op uw computer.

## <a name="install-python-package"></a>Python-pakket installeren

Open een opdrachtprompt met Python in het pad voor het installeren van het Python-pakket voor Event Hubs, en voer vervolgens deze opdracht uit: 

```bash
pip install azure-eventhub
```

## <a name="send-events-to-event-hubs"></a>Gebeurtenissen verzenden naar Event Hubs
De volgende code toont u hoe u gebeurtenissen naar een event hub verzendt vanuit een Python-toepassing: 

1. Maak variabelen op voor de URL van de event hub, de sleutelnaam en de sleutelwaarde. 

    ```python
    # Import classes from Event Hubs python package
    from azure.eventhub import EventHubClient, Receiver, Offset
    
    # Address can be in either of these formats:
    # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
    # "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
    # For example:
    ADDRESS = "amqps://<MyEventHubNamspaceName>.servicebus.windows.net/<MyEventHubName>"
    
    # SAS policy and key are not required if they are encoded in the URL
    USER = "<Name of the access key. Default name: RootManageSharedAccessKey>"
    KEY = "<The access key>"
    ```

2. Een Event Hubs-client maken, een afzender toevoegen, de client wordt uitgevoerd, verzendt de gebeurtenis met behulp van de afzender en de client vervolgens stoppen wanneer u klaar bent. 

    ```python
    # Create an Event Hubs client
    client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
    
    # Add a sender to the client
    sender = client.add_sender(partition="0")
    
    # Run the Event Hub client
    client.run()
    
    # Send event to the event hub
    sender.send(EventData("<MyEventData>"))
    
    # Stop the Event Hubs client
    client.stop()
    
    ```

Zie voor een volledige zelfstudie over hoe u gebeurtenissen naar een event hub verzendt vanuit een toepassing die is geschreven in Python, [in dit artikel](event-hubs-python-get-started-send.md).

## <a name="receive-events-from-event-hubs"></a>Gebeurtenissen ontvangen van Event Hubs
De volgende code toont u hoe u gebeurtenissen ontvangen van een event hub uit een Python-toepassing: 

```python

# Create an Event Hubs client
client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)

# Add a receiver to the client
receiver = client.add_receiver(CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)

# Run the Event Hubs client
client.run()

# Receive event data from the event hub
for event_data in receiver.receive(timeout=100):
    last_offset = event_data.offset
    last_sn = event_data.sequence_number
    print("Received: {}, {}".format(last_offset, last_sn))

# Stop the Event Hubs client
client.stop()
```

Zie voor een volledige zelfstudie over het ontvangen van gebeurtenissen van een event hub vanuit een toepassing die is geschreven in Python [in dit artikel](event-hubs-python-get-started-receive.md)

## <a name="read-capture-event-data-from-azure-storage"></a>Alleen vastleggen van gebeurtenisgegevens uit Azure Storage
De volgende code toont u het lezen van de gegevens van de vastgelegde gebeurtenis die opgeslagen in een **Azure blob-opslag** uit een Python-toepassing: inschakelen **vastleggen** functie voor de event hub met de instructies uit: [Event Hubs Capture inschakelen met behulp van de Azure-portal](event-hubs-capture-enable-through-portal.md). Klik, sommige gebeurtenissen verzenden naar de event hub voordat u test de code. 

```python
import os
import string
import json
import avro.schema
from avro.datafile import DataFileReader, DataFileWriter
from avro.io import DatumReader, DatumWriter
from azure.storage.blob import BlockBlobService

def processBlob(filename):
    reader = DataFileReader(open(filename, 'rb'), DatumReader())
    dict = {}
    for reading in reader:
        parsed_json = json.loads(reading["Body"])
        if not 'id' in parsed_json:
            return
        if not dict.has_key(parsed_json['id']):
            list = []
            dict[parsed_json['id']] = list
        else:
            list = dict[parsed_json['id']]
            list.append(parsed_json)
    reader.close()
    for device in dict.keys():
        deviceFile = open(device + '.csv', "a")
        for r in dict[device]:
            deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')

def startProcessing(accountName, key, container):
    print 'Processor started using path: ' + os.getcwd()
    block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
    generator = block_blob_service.list_blobs(container)
    for blob in generator:
        #content_length == 508 is an empty file, so only process content_length > 508 (skip empty files)
        if blob.properties.content_length > 508:
            print('Downloaded a non empty blob: ' + blob.name)
            cleanName = string.replace(blob.name, '/', '_')
            block_blob_service.get_blob_to_path(container, blob.name, cleanName)
            processBlob(cleanName)
            os.remove(cleanName)
        block_blob_service.delete_blob(container, blob.name)
startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')

```

Zie voor een volledige zelfstudie over het lezen van vastgelegde gegevens van Event Hubs in een Azure blob storage vanuit een toepassing die is geschreven in Python [in dit artikel](event-hubs-capture-python.md)

## <a name="github-samples"></a>GitHub-voorbeelden
U vindt meer voorbeelden van Python in de [Git-opslagplaats voor azure-event-hubs-python](https://github.com/Azure/azure-event-hubs-python/).

## <a name="next-steps"></a>Volgende stappen
Lees de artikelen in de sectie concepten vanaf [overzicht van de functies van Event Hubs](event-hubs-features.md).