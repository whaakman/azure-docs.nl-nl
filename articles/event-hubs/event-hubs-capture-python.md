---
title: Overzicht van Azure Event Hubs Capture | Microsoft Docs
description: Voorbeeld dat gebruikmaakt van de Azure Python SDK ter illustratie van de functie Event Hubs Capture gebruikt.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2018
ms.author: shvija
ms.openlocfilehash: 0733d26839cbb2dc40f4dd99b60d6a5628cc1f1a
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52727083"
---
# <a name="event-hubs-capture-walkthrough-python"></a>Overzicht van Event Hubs Capture: Python

Vastleggen is een functie van Azure Event Hubs. U kunt deze gebruiken automatisch de gegevensstroom in uw event hub naar een Azure Blob storage-account van uw keuze te bezorgen. Deze mogelijkheid kunt u eenvoudig is om uit te voeren batchverwerking voor realtime streaming-gegevens. In dit artikel wordt beschreven hoe u Event Hubs Capture gebruiken met Python. Zie voor meer informatie over Event Hubs Capture, de [overzichtsartikel](event-hubs-capture-overview.md).

In dit voorbeeld wordt de [Azure Python SDK](https://azure.microsoft.com/develop/python/) ter illustratie van de Capture-functie. Het programma sender.py stuurt gesimuleerde uitwerking telemetrie naar Event Hubs in JSON-indeling. De event hub is geconfigureerd voor het gebruik van de Capture-functie voor het schrijven van deze gegevens naar Blob-opslag in batches. De app capturereader.py deze blobs leest en maakt een bestand toevoegen per apparaat. De app wordt vervolgens de gegevens schrijft naar CSV-bestanden.

## <a name="what-youll-accomplish"></a>Wat u moet uitvoeren

1. Een Azure Blob storage-account en daarin, een blobcontainer maken met behulp van de Azure-portal.
2. Een Event Hubs-naamruimte maken met behulp van de Azure-portal.
3. Een event hub maken met de functie Capture is ingeschakeld, met behulp van de Azure-portal.
4. Gegevens verzenden naar de event hub met behulp van een Python-script.
5. De bestanden worden gelezen uit het vastleggen en ze verwerken met behulp van een andere Python-script.

## <a name="prerequisites"></a>Vereisten

- Python 2.7.x
- Een Azure-abonnement. Als u nog geen abonnement hebt, [maakt u een gratis account](https://azure.microsoft.com/free/) voordat u begint.
- Een actief [Event Hubs-naamruimte en event hub](event-hubs-create.md). 
- Schakel **vastleggen** functie voor de event hub met de instructies uit: [Event Hubs Capture inschakelen met behulp van de Azure portal](event-hubs-capture-enable-through-portal.md)

## <a name="create-an-azure-blob-storage-account"></a>Een Azure Blob storage-account maken
1. Meld u aan bij [Azure Portal][Azure portal].
2. Selecteer in het linkerdeelvenster van de portal, **nieuw** > **opslag** > **Opslagaccount**.
3. De selecties hebt gemaakt in de **storage-account maken** deelvenster en selecteer vervolgens **maken**.
   
   ![Deelvenster 'Storage-account maken'][1]
4. Nadat u ziet de **implementaties voltooid** weergegeven, selecteert u de naam van het nieuwe opslagaccount en in de **Essentials** deelvenster en selecteer vervolgens **Blobs**. Wanneer de **Blob-service** deelvenster wordt geopend, selecteert u **+ Container** aan de bovenkant. Naam van de container **vastleggen**, en sluit vervolgens de **Blob-service** deelvenster.
5. Selecteer **toegangssleutels** in het linkerdeelvenster en kopieer de naam van het opslagaccount en de waarde van **key1**. Plak deze waarde in Kladblok of een andere tijdelijke locatie.

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Een Python-script voor het verzenden van gebeurtenissen naar uw event hub maken
1. Open uw favoriete Python-editor, zoals [Visual Studio Code][Visual Studio Code].
2. Maken van een script met de naam **sender.py**. Met dit script verzonden 200 gebeurtenissen naar uw event hub. Ze zijn eenvoudige uitwerking metingen verzonden in JSON.
3. Plak de volgende code in sender.py:
   
   ```python
   import uuid
   import datetime
   import random
   import json
   from azure.servicebus import ServiceBusService
   
   sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
   devices = []
   for x in range(0, 10):
       devices.append(str(uuid.uuid4()))
   
   for y in range(0,20):
       for dev in devices:
           reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
           s = json.dumps(reading)
           sbs.send_event('INSERT YOUR EVENT HUB NAME', s)
       print y
   ```
4. De bovenstaande code voor het gebruik van uw naamruimtenaam, de sleutelwaarde en de naam van de event hub die u hebt verkregen tijdens het maken van de Event Hubs-naamruimte bijwerken.

## <a name="create-a-python-script-to-read-your-capture-files"></a>Een Python-script om te lezen van de Capture-bestanden maken

1. Vul het deelvenster en selecteer **maken**.
2. Maken van een script met de naam **capturereader.py**. Met dit script leest de vastgelegde bestanden en maakt u een bestand per apparaat om de gegevens alleen voor dat apparaat te schrijven.
3. Plak de volgende code in capturereader.py:
   
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
4. Plak de juiste waarden voor uw storage-accountnaam en de sleutel in de aanroep naar `startProcessing`.

## <a name="run-the-scripts"></a>De scripts uitvoeren
1. Open een opdrachtprompt met Python in het pad en voer vervolgens deze opdrachten om vereiste Python-pakketten te installeren:
   
   ```
   pip install azure-storage
   pip install azure-servicebus
   pip install avro
   ```
   
   Hebt u een eerdere versie van een van beide **azure-opslag** of **azure**, moet u mogelijk gebruik van de **--upgrade** optie.
   
   Mogelijk moet u ook de volgende opdracht uit (niet nodig op de meeste systemen):
   
   ```
   pip install cryptography
   ```
2. Wijzig de directory waar u sender.py en capturereader.py opgeslagen en voer deze opdracht uit:
   
   ```
   start python sender.py
   ```
   
   Met deze opdracht start een nieuwe Python-proces voor het uitvoeren van de afzender.
3. Wacht een paar minuten voor het vastleggen om uit te voeren. Typ vervolgens de volgende opdracht in uw oorspronkelijke opdrachtvenster:
   
   ```
   python capturereader.py
   ```

   Deze processor vastleggen maakt gebruik van de lokale map alle blobs downloaden van het opslagaccount /-container. Verwerkt alle die niet leeg zijn en deze schrijft de resultaten als CSV-bestanden naar de lokale map.

## <a name="next-steps"></a>Volgende stappen

U meer informatie over Event Hubs met behulp van de volgende koppelingen:

* [Overzicht van Eventhubs vastleggen][Overview of Event Hubs Capture]
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [Event Hubs-overzicht][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
