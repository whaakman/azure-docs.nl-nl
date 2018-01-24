---
title: Overzicht van Azure Event Hubs vastleggen | Microsoft Docs
description: Voorbeeld dat gebruikmaakt van de Azure Python SDK voor het demonstreren van met de functie voor het vastleggen van Event Hubs.
services: event-hubs
documentationcenter: 
author: djrosanova
manager: timlt
editor: 
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 97cadbde2ddedade1a8688f1380b9ff9194613e7
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="event-hubs-capture-walkthrough-python"></a>Overzicht van Event Hubs vastleggen: Python

Vastleggen is een functie van Azure Event Hubs. U kunt deze gebruiken automatisch de streaming om gegevens te bezorgen in uw event hub naar een Azure Blob storage-account van uw keuze. Deze mogelijkheid kunt gemakkelijk voor realtime streaming-gegevens voor batchverwerking uitvoeren. Dit artikel wordt beschreven hoe u Event Hubs vastleggen met behulp van Python. Zie voor meer informatie over het vastleggen van Event Hubs, de [overzichtsartikel](event-hubs-capture-overview.md).

In dit voorbeeld gebruikt de [Azure Python SDK](https://azure.microsoft.com/develop/python/) ter illustratie van de functie vastleggen. Het programma sender.py verzendt gesimuleerde omgeving telemetrie naar Event Hubs in JSON-indeling. De event hub is geconfigureerd voor de functie vastleggen gebruiken deze gegevens naar Blob storage in batches schrijven. De app capturereader.py deze blobs leest en maakt een toevoeg-bestand per apparaat. De app schrijft vervolgens de gegevens naar CSV-bestanden.

## <a name="what-youll-accomplish"></a>Wat u moet doen

1. Maak een Azure Blob storage-account en een blob-container binnen, met behulp van de Azure-portal.
2. Maak een naamruimte Event Hubs met behulp van de Azure-portal.
3. Een event hub maken met de functie vastleggen is ingeschakeld, met behulp van de Azure-portal.
4. Gegevens verzenden naar de event hub met behulp van een pythonscript.
5. De bestanden lezen uit het vastleggen en verwerken met behulp van een andere pythonscript.

## <a name="prerequisites"></a>Vereisten

- Python 2.7.x
- Een Azure-abonnement
- Een actieve [Event Hubs-naamruimte en event hub](event-hubs-create.md)

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-blob-storage-account"></a>Een Azure Blob storage-account maken
1. Meld u aan bij [Azure Portal][Azure portal].
2. Selecteer in het linkerdeelvenster van de portal **nieuw** > **opslag** > **Opslagaccount**.
3. De selecties in de **storage-account maken** deelvenster en selecteer vervolgens **maken**.
   
   ![Deelvenster 'Storage-account maken'][1]
4. Nadat u de **implementaties is geslaagd** bericht wordt weergegeven, selecteert u de naam van het nieuwe opslagaccount en in de **Essentials** deelvenster en selecteer vervolgens **Blobs**. Wanneer de **Blob-service** deelvenster wordt geopend, selecteer **+ Container** aan de bovenkant. Naam van de container **vastleggen**, en sluit vervolgens de **Blob-service** deelvenster.
5. Selecteer **toegangssleutels** in het linkerdeelvenster en kopieer de naam van het opslagaccount en de waarde van **key1**. Deze waarden naar Kladblok of een andere tijdelijke locatie opslaan.

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Een pythonscript voor het verzenden van gebeurtenissen naar uw event hub maken
1. Open uw favoriete Python-editor, zoals [Visual Studio Code][Visual Studio Code].
2. Maken van een script met de naam **sender.py**. Dit script verzendt 200 gebeurtenissen naar uw event hub. Ze zijn eenvoudige milieu metingen in JSON verzonden.
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
4. Bijwerken van de voorafgaande code voor het gebruik van uw naamruimtenaam, de sleutelwaarde en de naam van de event hub die u hebt verkregen tijdens het maken van de Event Hubs-naamruimte.

## <a name="create-a-python-script-to-read-your-capture-files"></a>Maken van een pythonscript om te lezen van uw bestanden vastleggen

1. Vul het deelvenster en selecteer **maken**.
2. Maken van een script met de naam **capturereader.py**. Dit script leest de vastgelegde bestanden en maakt een bestand per apparaat om de gegevens alleen voor dat apparaat te schrijven.
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
4. Plak de juiste waarden voor de naam van het opslagaccount en de sleutel in de aanroep naar `startProcessing`.

## <a name="run-the-scripts"></a>De scripts uitvoeren
1. Open een opdrachtprompt met Python in het pad en voer vervolgens deze opdrachten om vereiste Python-pakketten te installeren:
   
   ```
   pip install azure-storage
   pip install azure-servicebus
   pip install avro
   ```
   
   Als er een eerdere versie van een **azure-opslag** of **azure**, moet u mogelijk gebruik van de **--upgrade** optie.
   
   Mogelijk moet u ook de volgende opdracht (niet nodig op de meeste systemen):
   
   ```
   pip install cryptography
   ```
2. Wijzig de directory waar u sender.py en capturereader.py opgeslagen en voer deze opdracht uit:
   
   ```
   start python sender.py
   ```
   
   Deze opdracht start een nieuwe Python-proces voor het uitvoeren van de afzender.
3. Wacht enkele minuten duren voordat de opname om uit te voeren. Typ de volgende opdracht in uw oorspronkelijke opdrachtvenster:
   
   ```
   python capturereader.py
   ```

   Deze processor vastleggen maakt gebruik van de lokale map alle blobs downloaden van de storage-account/container. Verwerkt die niet leeg zijn en deze schrijft de resultaten als CSV-bestanden naar de lokale map.

## <a name="next-steps"></a>Volgende stappen

U meer informatie over Event Hubs met behulp van de volgende koppelingen:

* [Overzicht van Event Hubs vastleggen][Overview of Event Hubs Capture]
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [Event Hubs-overzicht][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
