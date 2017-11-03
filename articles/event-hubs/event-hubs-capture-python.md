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
ms.openlocfilehash: 5fb691ec53fed20e5df4f581da10b964c07e09b2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="event-hubs-capture-walkthrough-python"></a>Overzicht van Event Hubs vastleggen: Python

Vastleggen van Event Hubs is een functie van Event Hubs die u kunt de streaming gegevens automatisch te leveren in uw event hub naar een Azure Blob storage-account van uw keuze. Deze mogelijkheid kunt gemakkelijk voor realtime streaming-gegevens voor batchverwerking uitvoeren. Dit artikel wordt beschreven hoe u Event Hubs vastleggen met behulp van Python. Zie voor meer informatie over het vastleggen van Event Hubs, de [overzichtsartikel](event-hubs-capture-overview.md).

In dit voorbeeld gebruikt de [Azure Python SDK](https://azure.microsoft.com/develop/python/) ter illustratie van de functie vastleggen. Het programma sender.py verzendt gesimuleerde omgeving telemetrie naar Event Hubs in JSON-indeling. De event hub is geconfigureerd voor gebruik van de functie vastleggen om deze gegevens naar de blob storage in batches te schrijven. De app capturereader.py vervolgens deze blobs leest en maakt een toevoeg-bestand per apparaat en schrijft de gegevens naar CSV-bestanden.

## <a name="what-will-be-accomplished"></a>Wat wordt bereikt

1. Maak een Azure Blob Storage-account en een blob-container binnen, met de Azure portal.
2. Maak een Event Hub-naamruimte met behulp van de Azure-portal.
3. Een event hub maken met de functie vastleggen is ingeschakeld, met de Azure portal.
4. Gegevens verzenden naar de event hub met een pythonscript.
5. De bestanden lezen uit het vastleggen en deze met een ander pythonscript te verwerken.

## <a name="prerequisites"></a>Vereisten

- Python 2.7.x
- Een Azure-abonnement
- Een actieve [Event Hubs-naamruimte en event hub.](event-hubs-create.md)

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Een Azure Storage-account maken
1. Meld u aan bij [Azure Portal][Azure portal].
2. Klik in het linkernavigatievenster van de portal op **nieuw**, klikt u vervolgens op **opslag**, en klik vervolgens op **Opslagaccount**.
3. Vul de velden in de blade opslagaccount en klik vervolgens op **maken**.
   
   ![][1]
4. Nadat u de **implementaties is geslaagd** bericht wordt weergegeven, klikt u op de naam van het nieuwe opslagaccount en in de **Essentials** blade, klikt u op **Blobs**. Wanneer de **Blob-service** blade wordt geopend, klikt u op **+ Container** aan de bovenkant. Naam van de container **vastleggen**, sluit de **Blob-service** blade.
5. Klik op **toegangssleutels** in het linkerdeelvenster blade en kopieer de naam van het opslagaccount en de waarde van **key1**. Deze waarden naar Kladblok of een andere tijdelijke locatie opslaan.

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

1. Vul de blade en klik op **maken**.
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
          if blob.properties.content_length != 0:
              print('Downloaded a non empty blob: ' + blob.name)
              cleanName = string.replace(blob.name, '/', '_')
              block_blob_service.get_blob_to_path(container, blob.name, cleanName)
              processBlob(cleanName)
              os.remove(cleanName)
          block_blob_service.delete_blob(container, blob.name)
  startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')
  ```
4. Zorg ervoor dat de juiste waarden voor de opslagaccountnaam en sleutel plakken in de aanroep naar `startProcessing`.

## <a name="run-the-scripts"></a>De scripts uitvoeren
1. Open een opdrachtprompt met Python in het pad en voer vervolgens deze opdrachten om vereiste Python-pakketten te installeren:
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  Als u een eerdere versie van azure storage of azure hebt, moet u mogelijk gebruik van de **--upgrade** optie
   
  Mogelijk moet u ook het volgende uitvoeren (niet nodig op de meeste systemen):
   
  ```
  pip install cryptography
  ```
2. Wijzig de directory waar u sender.py en capturereader.py opgeslagen en voer deze opdracht uit:
   
  ```
  start python sender.py
  ```
   
  Deze opdracht start een nieuwe Python-proces voor het uitvoeren van de afzender.
3. Nu wacht een paar minuten voor het vastleggen om uit te voeren. Typ de volgende opdracht in uw oorspronkelijke opdrachtvenster:
   
   ```
   python capturereader.py
   ```

   Deze processor vastleggen maakt gebruik van de lokale map alle blobs downloaden van de storage-account/container. Deze processen die niet leeg zijn, en schrijft de resultaten als CSV-bestanden naar de lokale map.

## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Overzicht van Event Hubs vastleggen][Overview of Event Hubs Capture]
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [Event Hubs-overzicht][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
