---
title: Queue Storage gebruiken met python-Azure Storage
description: Meer informatie over het gebruik van Azure Queue-service van python voor het maken en verwijderen van wacht rijen en het invoegen, ophalen en verwijderen van berichten.
services: storage
author: mhopkins-msft
ms.service: storage
ms.devlang: python
ms.topic: article
ms.date: 12/14/2018
ms.author: mhopkins
ms.reviewer: cbrooks
ms.subservice: queues
ms.openlocfilehash: 141999f4119ac92e2b8846477c50edf8fba027d0
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68360025"
---
# <a name="how-to-use-queue-storage-from-python"></a>Queue Storage gebruiken met Python
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht
In deze hand leiding wordt beschreven hoe u algemene scenario's uitvoert met behulp van de Azure Queue Storage-service. De voor beelden zijn geschreven in Python en gebruiken de [Microsoft Azure Storage SDK voor python]. De gedekte scenario's zijn het **Invoegen**, inspecteren, **ophalen**en **verwijderen** van wachtrij berichten, en het **maken en verwijderen van wacht rijen**. Raadpleeg de sectie [volgende stappen] voor meer informatie over wacht rijen.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Azure Storage SDK voor python downloaden en installeren

De [Azure Storage SDK voor python](https://github.com/azure/azure-storage-python) vereist python 2,7, 3,3, 3,4, 3,5 of 3,6.
 
### <a name="install-via-pypi"></a>Installeren via PyPi

Als u wilt installeren via de python-pakket index (PyPI), typt u:

```bash
pip install azure-storage-queue
```

> [!NOTE]
> Als u een upgrade uitvoert van de Azure Storage SDK voor python versie 0,36 of eerder, verwijdert u de oudere `pip uninstall azure-storage` SDK met voordat u het meest recente pakket installeert.

Zie [Azure Storage SDK voor python](https://github.com/Azure/azure-storage-python/)voor een alternatieve installatie methode.

## <a name="view-the-sample-application"></a>De voorbeeld toepassing weer geven

Zie [Azure Storage voor het weer geven en uitvoeren van een voorbeeld toepassing die laat zien hoe u python kunt gebruiken met Azure queues: Aan de slag met Azure-wacht](https://github.com/Azure-Samples/storage-queue-python-getting-started)rijen in python. 

Als u de voorbeeld toepassing wilt uitvoeren, moet u ervoor zorgen dat `azure-storage-queue` u `azure-storage-common` zowel de als-pakketten hebt geïnstalleerd.

## <a name="how-to-create-a-queue"></a>Procedure: Een wachtrij maken

Met het **QueueService** -object kunt u met wacht rijen werken. Met de volgende code wordt een **QueueService** -object gemaakt. Voeg het volgende toe aan de bovenkant van een python-bestand waarin u programmatisch toegang wilt krijgen Azure Storage:

```python
from azure.storage.queue import QueueService
```

Met de volgende code wordt een **QueueService** -object gemaakt met behulp van de naam van het opslag account en de account sleutel. Vervang ' MyAccount ' en ' MyKey ' door de naam en sleutel van uw account.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Procedure: Een bericht in een wachtrij invoegen
Als u een bericht in een wachtrij wilt invoegen, gebruikt u de methode **bericht plaatsen\_** om een nieuw bericht te maken en toe te voegen aan de wachtrij.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-the-next-message"></a>Procedure: Het volgende bericht bekijken
U kunt het bericht aan de voor kant van een wachtrij bekijken zonder het uit de wachtrij te verwijderen door de **methode\_** voor het weer geven van berichten aan te roepen. Standaard worden **\_berichten** bekijken weer gegeven in één bericht.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a>Procedure: Berichten uit de wachtrij verwijderen
Uw code verwijdert een bericht uit een wachtrij in twee stappen. Wanneer u **Get\_-berichten**aanroept, wordt standaard het volgende bericht in een wachtrij weer gegeven. Een bericht dat wordt geretourneerd door **Get\_-berichten** is niet zichtbaar voor andere code die berichten uit deze wachtrij leest. Standaard blijft het bericht onzichtbaar gedurende 30 seconden. Als u het verwijderen van het bericht uit de wachtrij wilt volt ooien, moet u ook **bericht verwijderen\_** aanroepen. Dit proces met twee stappen voor het verwijderen van een bericht zorgt ervoor dat wanneer uw code een bericht niet kan verwerken als gevolg van een hardware-of software fout, een ander exemplaar van uw code hetzelfde bericht kan ophalen en het opnieuw proberen. Uw code roept **bericht\_verwijderen** direct nadat het bericht is verwerkt.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

Er zijn twee manieren waarop u het ophalen van berichten uit een wachtrij kunt aanpassen.
Ten eerste kunt u berichten batchgewijs (maximaal 32) ophalen. Ten tweede kunt u een langere of kortere time-out voor onzichtbaarheid instellen, zodat uw code meer of minder tijd krijgt voor het volledig verwerken van elk bericht. In het volgende code voorbeeld wordt de methode **Get\_Message** gebruikt om 16 berichten in één aanroep op te halen. Vervolgens wordt elk bericht verwerkt met behulp van een for-lus. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor elk bericht.

```python
messages = queue_service.get_messages(
    'taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: De inhoud van een bericht in de wachtrij wijzigen
U kunt de inhoud van een bericht in de wachtrij wijzigen. Als het bericht een werktaak vertegenwoordigt, kunt u deze functie gebruiken om de status van de werktaak bij te werken. In de onderstaande code wordt de methode **Update\_bericht** gebruikt om een bericht bij te werken. De time-out voor de zicht baarheid is ingesteld op 0, wat betekent dat het bericht onmiddellijk wordt weer gegeven en dat de inhoud wordt bijgewerkt.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message(
        'taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-the-queue-length"></a>Procedure: De lengte van de wachtrij ophalen
U kunt een schatting ophalen van het aantal berichten in de wachtrij. De **\_meta\_gegevens** van de methode ophalen van de wachtrij vraagt de Queue-service om meta gegevens over de wachtrij en de **approximate_message_count**te retour neren. Het resultaat is alleen van benadering omdat berichten kunnen worden toegevoegd of verwijderd nadat de wachtrij service op uw aanvraag reageert.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a>Procedure: Een wachtrij verwijderen
Als u een wachtrij en alle berichten erin wilt verwijderen, roept u de methode voor het **verwijderen\_** van de wachtrij aan.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Volgende stappen
Nu u de basis principes van wachtrij opslag hebt geleerd, volgt u deze koppelingen voor meer informatie.

* [Python Developer Center](https://azure.microsoft.com/develop/python/)
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355)

[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK voor python]: https://github.com/Azure/azure-storage-python
