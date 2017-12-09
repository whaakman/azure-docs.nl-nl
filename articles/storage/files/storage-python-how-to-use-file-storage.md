---
title: Ontwikkelen voor Azure-bestanden met behulp van Python | Microsoft Docs
description: Informatie over het ontwikkelen van Python-toepassingen en services die gebruikmaken van Azure-bestanden voor het opslaan van gegevens uit een bestand.
services: storage
documentationcenter: python
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/19/2017
ms.author: tamram
ms.openlocfilehash: cee6ece907950724f6ad4a86c489a5f07dfcaaec
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="develop-for-azure-files-with-python"></a>Ontwikkelen voor Azure-bestanden met behulp van Python
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

Deze zelfstudie wordt gedemonstreerd de basisbeginselen van het gebruik van Python voor het ontwikkelen van toepassingen of services die gebruikmaken van Azure-bestanden voor het opslaan van gegevens uit een bestand. In deze zelfstudie wordt een eenvoudige consoletoepassing maken en het uitvoeren van basisbewerkingen uitvoeren met Python en Azure-bestanden weergeven:

* Azure-bestandsshares maken
* Mappen maken
* Bestanden en mappen in een Azure-bestandsshare opsommen
* Uploaden, downloaden en een bestand verwijderen

> [!Note]  
> Omdat Azure-bestanden kunnen worden geopend via SMB, is het mogelijk om eenvoudige toepassingen die toegang hebben tot de Azure-bestandsshare met behulp van de standaard Python i/o-klassen en -functies te schrijven. In dit artikel wordt beschreven hoe schrijven van toepassingen die gebruikmaken van de Azure Storage Python SDK, die gebruikmaakt van de [REST-API van Azure-bestanden](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) contact opnemen met de Azure-bestanden.

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Download en installeer Azure opslag-SDK voor Python

Azure-opslag-SDK voor Python Python 2.7, 3.3, 3.4, 3.5 of 3.6 vereist en wordt geleverd in verschillende pakketten 4: `azure-storage-blob`, `azure-storage-file`, `azure-storage-table` en `azure-storage-queue`. In deze zelfstudie gaan we gebruiken `azure-storage-file` pakket.
 
## <a name="install-via-pypi"></a>Installeren via PyPi

Als u wilt installeren via de Python Package Index (PyPI), typt u:

```bash
pip install azure-storage-file
```


> [!NOTE]
> Als u van de Azure-opslag-SDK voor Python versie 0.36 of eerder bijwerkt, moet u eerst verwijderen met behulp van `pip uninstall azure-storage` als niet langer wij de opslag-SDK voor Python in één pakket brengen.
> 
> 

Voor de van alternatieve installatiemethoden, gaat u naar de [Azure-opslag-SDK voor Python op Github](https://github.com/Azure/azure-storage-python/).

## <a name="set-up-your-application-to-use-azure-files"></a>Instellen van uw toepassing om Azure-bestanden te gebruiken
De volgende aan de bovenkant van een Python-bronbestand waarin u programmatisch toegang biedt tot Azure Storage wilt toevoegen.

```python
from azure.storage.file import FileService
```

## <a name="set-up-a-connection-to-azure-files"></a>Instellen van een verbinding met Azure-bestanden 
De `FileService` object kunt u samenwerken met shares, mappen en bestanden. De volgende code maakt een `FileService` object met de naam en het account opslagaccountsleutel. Vervang `<myaccount>` en `<mykey>` met uw accountnaam en de sleutel.

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken
In het volgende voorbeeld, kunt u een `FileService` -object op voor de share maken als deze niet bestaat.

```python
file_service.create_share('myshare')
```

## <a name="create-a-directory"></a>Een map maken
U kunt ook de opslag indelen door de gegevens van de bestanden in submappen in plaats van ze allemaal in de hoofdmap. Azure Files kunt u zoveel mappen als uw account kunt maken. De code hieronder maakt u een onderliggende map met de naam **sampledir** onder de hoofdmap.

```python
file_service.create_directory('myshare', 'sampledir')
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Bestanden en mappen in een Azure-bestandsshare opsommen
Als de bestanden en mappen in een share wilt weergeven, gebruikt de **lijst\_mappen\_en\_bestanden** methode. Deze methode retourneert een generator. De volgende code uitvoer de **naam** van elk bestand en de map in een share naar de console.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

## <a name="upload-a-file"></a>Bestand uploaden 
Azure File share tenminste bevat, een hoofdmap waarin de bestanden kunnen zich bevinden. In deze sectie leert u hoe een bestand van de lokale opslag naar de hoofdmap van een share te uploaden.

Voor het maken van een bestand en de gegevens uploaden, gebruiken de `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` of `create_file_from_text` methoden. Op hoog niveau methoden voor het uitvoeren van de benodigde verdeling in segmenten wanneer de grootte van de gegevens is groter dan 64 MB zijn.

`create_file_from_path`de inhoud van een bestand van het opgegeven pad, uploadt en `create_file_from_stream` de inhoud van een al geopend bestandsstroom uploadt. `create_file_from_bytes`een matrix met bytes, uploadt en `create_file_from_text` uploadt de opgegeven tekstwaarde met behulp van de opgegeven codering (standaard ingesteld op UTF-8).

Het volgende voorbeeld wordt de inhoud van geüpload de **sunset.png** bestand naar de **mijnbestand** bestand.

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want to create this blob in the root directory, so we specify None for the directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

## <a name="download-a-file"></a>Bestand downloaden
Als u wilt downloaden van gegevens uit een bestand `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, of `get_file_to_text`. Op hoog niveau methoden voor het uitvoeren van de benodigde verdeling in segmenten wanneer de grootte van de gegevens is groter dan 64 MB zijn.

Het volgende voorbeeld worden `get_file_to_path` voor het downloaden van de inhoud van de **mijnbestand** -bestand en sla deze op de **out sunset.png** bestand.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

## <a name="delete-a-file"></a>Een bestand verwijderen
Tenslotte een als bestand wilt verwijderen, roept `delete_file`.

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="create-share-snapshot-preview"></a>Share-momentopname (preview) maken
U kunt een punt in tijd kopie van uw hele bestandsshare maken.

```python
snapshot = file_service.snapshot_share(share_name)
snapshot_id = snapshot.snapshot
```

**Share momentopname met metagegevens maken**

```python
metadata = {"foo": "bar"}
snapshot = file_service.snapshot_share(share_name, metadata=metadata)
```

## <a name="list-shares-and-snapshots"></a>Lijst met shares en momentopnamen 
U kunt de momentopnamen voor een bepaalde bestandsshare aanbieden.

```python
shares = list(file_service.list_shares(include_snapshots=True))
```

## <a name="browse-share-snapshot"></a>Share momentopname bladeren
U kunt de inhoud van elke momentopname share voor het ophalen van bestanden en mappen in de tijd vanaf dat moment bladeren.

```python
directories_and_files = list(file_service.list_directories_and_files(share_name, snapshot=snapshot_id))
```

## <a name="get-file-from-share-snapshot"></a>Bestand ophalen vanuit een momentopname van de share
U kunt een bestand downloaden van een momentopname van een share voor uw scenario terugzetten.

```python
with open(FILE_PATH, 'wb') as stream:
    file = file_service.get_file_to_stream(share_name, directory_name, file_name, stream, snapshot=snapshot_id)
```

## <a name="delete-a-single-share-snapshot"></a>Verwijderen van een momentopname van een afzonderlijke share  
U kunt een momentopname van een afzonderlijke share verwijderen.

```python
file_service.delete_share(share_name, snapshot=snapshot_id)
```

## <a name="delete-share-when-share-snapshots-exist"></a>Verwijder de share als share momentopnamen bestaan
Een share met momentopnamen kan niet worden verwijderd tenzij alle momentopnamen eerst worden verwijderd.

```python
file_service.delete_share(share_name, delete_snapshots=DeleteSnapshot.Include)
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt geleerd hoe u kunt bestanden van Azure met Python bewerken, volgt u deze koppelingen voor meer informatie.

* [Python Developer Center](/develop/python/)
* [REST-API voor Azure Storage-services](http://msdn.microsoft.com/library/azure/dd179355)
* [Microsoft Azure-opslag-SDK voor Python](https://github.com/Azure/azure-storage-python)