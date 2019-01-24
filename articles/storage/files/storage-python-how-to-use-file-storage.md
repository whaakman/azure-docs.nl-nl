---
title: Ontwikkelen voor Azure Files met Python | Microsoft Docs
description: Meer informatie over het ontwikkelen van Python-toepassingen en services die Azure Files gebruiken voor het opslaan van gegevens uit een bestand.
services: storage
author: wmgries
ms.service: storage
ms.devlang: python
ms.topic: article
ms.date: 12/14/2018
ms.author: tamram
ms.component: files
ms.openlocfilehash: 17b44dcaf6731f3882ed417a94532c6d26c7454d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54818962"
---
# <a name="develop-for-azure-files-with-python"></a>Ontwikkelen voor Azure Files met Python
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

In deze zelfstudie worden de basisbeginselen van het gebruik van Python voor het ontwikkelen van toepassingen of services die gebruikmaken van Azure Files voor het opslaan van gegevens uit een bestand. In deze zelfstudie, we maken een eenvoudige consoletoepassing en laten zien hoe u basisbewerkingen uitvoert met Python en Azure Files:

* Azure-bestandsshares maken
* Mappen maken
* Het inventariseren van bestanden en mappen in een Azure-bestandsshare
* Uploaden, downloaden en een bestand verwijderen

> [!Note]  
> Omdat Azure Files toegankelijk is via SMB, is het mogelijk om eenvoudige toepassingen die toegang hebben tot de Azure-bestandsshare met behulp van de standaard Python i/o-klassen en -functies te schrijven. In dit artikel wordt beschreven hoe u toepassingen schrijft die gebruikmaken van de Azure Storage Python SDK, die gebruikmaakt van de [Azure Files REST API](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api) om te communiceren met Azure Files.

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Download en installeer Azure Storage SDK voor Python

De [Azure Storage SDK voor Python](https://github.com/azure/azure-storage-python) Python 2.7, 3.3, 3.4, 3.5 en 3.6 vereist.
 
## <a name="install-via-pypi"></a>Installeren via de PyPi

Als u wilt installeren via de Python Package Index (PyPI), typt u:

```bash
pip install azure-storage-file
```

> [!NOTE]
> Als u een van de Azure Storage SDK voor Python versie 0.36 of eerder upgrade, verwijdert u de oudere SDK met `pip uninstall azure-storage` voordat u de meest recente pakket installeert.

Voor de van alternatieve installatiemethoden, gaat u naar de [Azure Storage SDK voor Python op GitHub](https://github.com/Azure/azure-storage-python/).

## <a name="view-the-sample-application"></a>De voorbeeld-App weergeven
f weergeven en uitvoeren van een voorbeeldtoepassing die laat zien hoe u Python gebruiken met Azure Files, Zie [Azure Storage: Aan de slag met Azure Files in Python](https://github.com/Azure-Samples/storage-file-python-getting-started). 

Als u wilt de voorbeeldtoepassing uitvoert, zorg ervoor dat u hebt geïnstalleerd zowel de `azure-storage-file` en `azure-storage-common` pakketten.

## <a name="set-up-your-application-to-use-azure-files"></a>Instellen van uw toepassing in Azure Files gebruiken
Voeg de volgende aan de bovenkant van een Python-bronbestand in die u wilt programmatisch toegang verkrijgen tot Azure Storage.

```python
from azure.storage.file import FileService
```

## <a name="set-up-a-connection-to-azure-files"></a>Instellen van een verbinding met Azure Files 
De `FileService` object kunt u samenwerken met shares, mappen en bestanden. De volgende code maakt een `FileService` object met de naam en toegangssleutel van het opslagaccount. Vervang `<myaccount>` en `<mykey>` door uw accountnaam en -sleutel.

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken
In het volgende codevoorbeeld, kunt u een `FileService` object dat wordt gemaakt van de share als deze niet bestaat.

```python
file_service.create_share('myshare')
```

## <a name="create-a-directory"></a>Een map maken
U kunt ook storage organiseren door het opslaan van bestanden in submappen in plaats van dat ze allemaal in de hoofdmap. Azure Files kunt u zo veel mappen als uw account kunt maken. De code hieronder maakt u een onderliggende map met de naam **sampledir** onder de hoofddirectory.

```python
file_service.create_directory('myshare', 'sampledir')
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Het inventariseren van bestanden en mappen in een Azure-bestandsshare
U kunt de bestanden en mappen in een share, gebruiken de **lijst\_mappen\_en\_bestanden** methode. Deze methode retourneert een generator. De volgende code uitvoer de **naam** van elk bestand en map in een share naar de console.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

## <a name="upload-a-file"></a>Bestand uploaden 
Azure-bestandsshare bevat op zijn minst een hoofdmap waarin de bestanden kunnen zich bevinden. In deze sectie leert u hoe u een bestand uit de lokale opslag naar de hoofdmap van een share te uploaden.

Als u wilt een bestand maken en uploaden van gegevens, gebruikt u de `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` of `create_file_from_text` methoden. Op hoog niveau methoden voor het uitvoeren van de benodigde logische groepen te verdelen wanneer de hoeveelheid gegevens groter is dan 64 MB zijn.

`create_file_from_path` de inhoud van het opgegeven pad van een bestand wordt geüpload en `create_file_from_stream` wordt de inhoud van een al geopend bestandsstroom geüpload. `create_file_from_bytes` een matrix van bytes, uploaden en `create_file_from_text` uploadt u de waarde van de opgegeven tekst met behulp van de opgegeven codering (standaard ingesteld op UTF-8).

Het volgende voorbeeld wordt de inhoud van geüpload de **sunset.png** bestand naar de **myfile** bestand.

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
Als u wilt downloaden van gegevens uit een bestand `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, of `get_file_to_text`. Op hoog niveau methoden voor het uitvoeren van de benodigde logische groepen te verdelen wanneer de hoeveelheid gegevens groter is dan 64 MB zijn.

Het volgende voorbeeld ziet u met behulp van `get_file_to_path` voor het downloaden van de inhoud van de **myfile** -bestand en sla het op de **out sunset.png** bestand.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

## <a name="delete-a-file"></a>Een bestand verwijderen
Ten slotte, als u wilt verwijderen van een bestand, aanroepen `delete_file`.

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="create-share-snapshot"></a>Share-momentopname maken
U kunt een punt in tijd kopie van de gehele bestandsshare maken.

```python
snapshot = file_service.snapshot_share(share_name)
snapshot_id = snapshot.snapshot
```

**Share-momentopname met metagegevens maken**

```python
metadata = {"foo": "bar"}
snapshot = file_service.snapshot_share(share_name, metadata=metadata)
```

## <a name="list-shares-and-snapshots"></a>Lijst met shares en momentopnamen 
U kunt de momentopnamen voor een bepaalde bestandsshare weergeven.

```python
shares = list(file_service.list_shares(include_snapshots=True))
```

## <a name="browse-share-snapshot"></a>Bladeren van een momentopname van bestandsshare
Inhoud van elke momentopname moet worden opgehaald van bestanden en mappen vanaf dat punt in tijd, kunt u bladeren.

```python
directories_and_files = list(file_service.list_directories_and_files(share_name, snapshot=snapshot_id))
```

## <a name="get-file-from-share-snapshot"></a>Ophalen van bestand van een momentopname van bestandsshare
U kunt een bestand downloaden vanuit een share-momentopname voor uw scenario terugzetten.

```python
with open(FILE_PATH, 'wb') as stream:
    file = file_service.get_file_to_stream(share_name, directory_name, file_name, stream, snapshot=snapshot_id)
```

## <a name="delete-a-single-share-snapshot"></a>Een momentopname van één share verwijderen  
U kunt een momentopname van een enkele share verwijderen.

```python
file_service.delete_share(share_name, snapshot=snapshot_id)
```

## <a name="delete-share-when-share-snapshots-exist"></a>Share verwijderen als momentopnamen van shares zijn
Een share die momentopnamen bevat, kan niet worden verwijderd tenzij alle momentopnamen eerst worden verwijderd.

```python
file_service.delete_share(share_name, delete_snapshots=DeleteSnapshot.Include)
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt geleerd hoe u Azure Files met Python te manipuleren, volgt u deze koppelingen voor meer informatie.

* [Python Developer Center](https://azure.microsoft.com/develop/python/)
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355)
* [Microsoft Azure Storage SDK voor Python](https://github.com/Azure/azure-storage-python)
