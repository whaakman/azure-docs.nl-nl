---
title: Het gebruik van Queue storage met Python | Microsoft Docs
description: Informatie over het gebruik van de Azure Queue-service van Python maken en verwijderen van wachtrijen, en invoegen, ophalen en verwijderen van berichten.
services: storage
author: tamram
ms.service: storage
ms.devlang: python
ms.topic: article
ms.date: 12/14/2018
ms.author: tamram
ms.component: queues
ms.openlocfilehash: 0edb90ca7324d47beaa5133d423928e615ff33a9
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742810"
---
# <a name="how-to-use-queue-storage-from-python"></a>Queue Storage gebruiken met Python
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u algemene scenario's met behulp van de Azure Queue storage-service uitvoert. De voorbeelden zijn geschreven in Python en gebruik de [Microsoft Azure Storage SDK voor Python]. De behandelde scenario's zijn **invoegen**, **inspecteren**, **aan**, en **verwijderen** berichten in de wachtrij, evenals  **het maken en verwijderen van wachtrijen**. Raadpleeg de sectie [Vervolgstappen] voor meer informatie over wachtrijen.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Download en installeer Azure Storage SDK voor Python

De [Azure Storage SDK voor Python](https://github.com/azure/azure-storage-python) Python 2.7, 3.3, 3.4, 3.5 en 3.6 vereist.
 
### <a name="install-via-pypi"></a>Installeren via de PyPi

Als u wilt installeren via de Python Package Index (PyPI), typt u:

```bash
pip install azure-storage-queue
```

> [!NOTE]
> Als u een van de Azure Storage SDK voor Python versie 0.36 of eerder upgrade, verwijdert u de oudere SDK met `pip uninstall azure-storage` voordat u de meest recente pakket installeert.

Zie voor andere installatiemethoden, [Azure Storage SDK voor Python](https://github.com/Azure/azure-storage-python/).

## <a name="view-the-sample-application"></a>De voorbeeld-App weergeven

Als u wilt weergeven en uitvoeren van een voorbeeldtoepassing die laat zien hoe u Python gebruiken met Azure-wachtrijen, Zie [Azure Storage: Aan de slag met Azure-wachtrijen in Python](https://github.com/Azure-Samples/storage-queue-python-getting-started). 

Als u wilt de voorbeeldtoepassing uitvoert, zorg ervoor dat u hebt geïnstalleerd zowel de `azure-storage-queue` en `azure-storage-common` pakketten.

## <a name="how-to-create-a-queue"></a>Procedures: Een wachtrij maken

De **QueueService** object kunt u samenwerken met wachtrijen. De volgende code maakt een **QueueService** object. Voeg het volgende aan de bovenkant van een Python-bestand waarin u wilt programmatisch toegang verkrijgen tot Azure Storage:

```python
from azure.storage.queue import QueueService
```

De volgende code maakt een **QueueService** object met de naam en toegangssleutel van het opslagaccount. Vervang 'myaccount' en 'mykey' door uw accountnaam en de sleutel.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Procedures: Een bericht in een wachtrij invoegen
Voor het invoegen van een bericht in een wachtrij wordt geplaatst, gebruikt u de **plaatsen\_bericht** methode voor het maken van een nieuw bericht en toe te voegen aan de wachtrij.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-the-next-message"></a>Procedures: Het volgende bericht
U kunt het bericht vooraan in een wachtrij bekijken zonder deze te verwijderen uit de wachtrij door het aanroepen van de **peek\_berichten** methode. Standaard **peek\_berichten** peeks op één bericht.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a>Procedures: Berichten in wachtrij
Uw code wordt een bericht uit een wachtrij in twee stappen. Als u aanroept **ophalen\_berichten**, krijgt u het volgende bericht in een wachtrij standaard. Een bericht dat wordt geretourneerd van **ophalen\_berichten** wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. Standaard blijft het bericht onzichtbaar gedurende 30 seconden. Voor het voltooien van het bericht uit de wachtrij verwijderen, moet u ook aanroepen **verwijderen\_bericht**. Dit proces in twee stappen van het verwijderen van een bericht zorgt ervoor dat wanneer uw code niet kan verwerken van een bericht vanwege fout bij de hardware of software, een ander exemplaar van uw code kunt hetzelfde bericht en probeer het opnieuw. Uw code roept **verwijderen\_bericht** direct nadat het bericht is verwerkt.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

Er zijn twee manieren waarop u het ophalen van berichten uit een wachtrij kunt aanpassen.
Ten eerste kunt u berichten batchgewijs (maximaal 32) ophalen. Ten tweede kunt u een langere of kortere time-out voor onzichtbaarheid instellen, zodat uw code meer of minder tijd krijgt voor het volledig verwerken van elk bericht. Het volgende codevoorbeeld wordt de **ophalen\_berichten** methode voor het ophalen van 16 berichten in één aanroep. Vervolgens wordt verwerkt elke bericht met een for-lus. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor elk bericht.

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedures: De inhoud van een bericht in de wachtrij wijzigen
U kunt de inhoud van een bericht in de wachtrij wijzigen. Als het bericht een werktaak vertegenwoordigt, kunt u deze functie gebruiken om de status van de werktaak bij te werken. De code hieronder wordt de **bijwerken\_bericht** methode voor het bijwerken van een bericht. De time-out voor zichtbaarheid is ingesteld op 0, wat betekent dat het bericht verschijnt onmiddellijk en de inhoud wordt bijgewerkt.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-the-queue-length"></a>Procedures: Lengte van de wachtrij ophalen
U kunt een schatting ophalen van het aantal berichten in de wachtrij. De **ophalen\_wachtrij\_metagegevens** methode vraagt de queue-service om terug te keren metagegevens over de wachtrij en de **approximate_message_count**. Het resultaat is alleen bij benadering omdat berichten kunnen worden toegevoegd of verwijderd nadat de queue-service op uw aanvraag reageert.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a>Procedures: Een wachtrij verwijderen
Als wilt verwijderen van een wachtrij en alle berichten die erin zijn opgenomen, roept de **verwijderen\_wachtrij** methode.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van Queue storage hebt geleerd, volgt u deze koppelingen voor meer informatie.

* [Python Developer Center](https://azure.microsoft.com/develop/python/)
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355)

[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK voor Python]: https://github.com/Azure/azure-storage-python
