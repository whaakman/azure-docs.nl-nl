---
title: Hoe Queue storage gebruiken met Python | Microsoft Docs
description: Informatie over het gebruik van de Azure Queue-service met Python maken en verwijderen van wachtrijen, en invoegen, ophalen en verwijderen van berichten.
services: storage
documentationcenter: python
author: tamram
manager: timlt
editor: tysonn
ms.assetid: cc0d2da2-379a-4b58-a234-8852b4e3d99d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: c7976c01436b1c30880bfd4c57cb97f72a4f48b0
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-use-queue-storage-from-python"></a>Queue Storage gebruiken met Python
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's met behulp van de Azure Queue storage-service uitvoeren. De voorbeelden zijn geschreven in Python en gebruik de [Microsoft Azure-opslag-SDK voor Python]. De scenario's worden behandeld: **invoegen**, **inspecteren**, **ophalen**, en **verwijderen** wachtrij berichten, evenals **maken en verwijderen van wachtrijen**. Raadpleeg de sectie [Vervolgstappen] voor meer informatie over wachtrijen.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Download en installeer Azure opslag-SDK voor Python

Azure-opslag-SDK voor Python Python 2.7, 3.3, 3.4, 3.5 of 3.6 vereist en wordt geleverd in verschillende pakketten 4: `azure-storage-blob`, `azure-storage-file`, `azure-storage-table` en `azure-storage-queue`. In deze zelfstudie gaan we gebruiken `azure-storage-queue` pakket.
 
### <a name="install-via-pypi"></a>Installeren via PyPi

Als u wilt installeren via de Python Package Index (PyPI), typt u:

```bash
pip install azure-storage-queue
```


> [!NOTE]
> Als u van de Azure-opslag-SDK voor Python versie 0.36 of eerder bijwerkt, moet u eerst verwijderen met behulp van `pip uninstall azure-storage` als niet langer wij de opslag-SDK voor Python in één pakket brengen.
> 
> 

Voor de van alternatieve installatiemethoden, gaat u naar de [Azure-opslag-SDK voor Python op Github](https://github.com/Azure/azure-storage-python/).

## <a name="how-to-create-a-queue"></a>Procedure: Een wachtrij maken
De **QueueService** object kunt u samenwerken met wachtrijen. De volgende code maakt een **QueueService** object. Voeg de volgende boven aan elk Python-bestand waarin u wilt programmatisch toegang biedt tot Azure Storage:

```python
from azure.storage.queue import QueueService
```

De volgende code maakt een **QueueService** object met de naam en het account opslagaccountsleutel. 'Myaccount' en 'mykey' vervangen door uw accountnaam en de sleutel.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Procedure: Een bericht in een wachtrij invoegen
Voor het invoegen van een bericht in een wachtrij, gebruikt u de **plaatsen\_bericht** methode voor het maken van een nieuw bericht en voeg deze toe aan de wachtrij.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-the-next-message"></a>Procedure: Bekijken van het volgende bericht
U kunt het bericht vooraan in een wachtrij bekijken zonder het te verwijderen uit de wachtrij door het aanroepen van de **peek\_berichten** methode. Standaard **peek\_berichten** peeks op één bericht.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a>Procedure: Berichten in wachtrij
Uw code wordt een bericht uit een wachtrij in twee stappen. Als u aanroept **ophalen\_berichten**, krijgt u het volgende bericht in een wachtrij standaard. Een bericht dat wordt geretourneerd door **ophalen\_berichten** wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. Standaard blijft het bericht onzichtbaar gedurende 30 seconden. Voor het voltooien van het bericht uit de wachtrij te verwijderen, moet u ook aanroepen **verwijderen\_bericht**. Dit proces in twee stappen van het verwijderen van een bericht zorgt ervoor dat wanneer uw code niet kan een bericht vanwege problemen met hardware of software te verwerken, een ander exemplaar van uw code kunt het bericht verschijnt en probeer het opnieuw. Uw code haalt **verwijderen\_bericht** direct nadat het bericht is verwerkt.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

Er zijn twee manieren waarop u het ophalen van berichten uit een wachtrij kunt aanpassen.
Ten eerste kunt u berichten batchgewijs (maximaal 32) ophalen. Ten tweede kunt u een langere of kortere time-out voor onzichtbaarheid instellen, zodat uw code meer of minder tijd krijgt voor het volledig verwerken van elk bericht. Het volgende codevoorbeeld wordt de **ophalen\_berichten** methode 16 berichten in één aanroep ophalen. Vervolgens wordt verwerkt elke bericht met een for-lus. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor elk bericht.

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: De inhoud van een bericht in de wachtrij wijzigen
U kunt de inhoud van een bericht in de wachtrij wijzigen. Als het bericht een werktaak vertegenwoordigt, kunt u deze functie gebruiken om de status van de werktaak bij te werken. De code hieronder maakt gebruik van de **bijwerken\_bericht** methode voor het bijwerken van een bericht. Time-out voor de zichtbaarheid is ingesteld op 0, wat betekent dat het bericht verschijnt onmiddellijk en de inhoud wordt bijgewerkt.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-the-queue-length"></a>Procedure: De lengte van de wachtrij ophalen
U kunt een schatting ophalen van het aantal berichten in de wachtrij. De **ophalen\_wachtrij\_metagegevens** methode vraagt de queue-service om terug te keren metagegevens over de wachtrij en de **approximate_message_count**. Het resultaat is alleen een benadering omdat berichten kunnen worden toegevoegd of verwijderd nadat de queue-service op uw aanvraag reageert.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a>Procedure: Een wachtrij verwijderen
Aanroepen voor het verwijderen van een wachtrij en alle berichten hierin de **verwijderen\_wachtrij** methode.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Queue storage hebt geleerd, volgt u deze koppelingen voor meer informatie.

* [Python Developer Center](/develop/python/)
* [REST-API voor Azure Storage-services](http://msdn.microsoft.com/library/azure/dd179355)
* [Blog van het Azure Storage-team]
* [Microsoft Azure-opslag-SDK voor Python]

[Blog van het Azure Storage-team]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure-opslag-SDK voor Python]: https://github.com/Azure/azure-storage-python