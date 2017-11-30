---
title: Het gebruik van Azure Blob storage (objectopslag) met Python | Microsoft Docs
description: Sla niet-gestructureerde gegevens op in de cloud met Azure Blob Storage (objectopslag).
services: storage
documentationcenter: python
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 0348e360-b24d-41fa-bb12-b8f18990d8bc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 2/24/2017
ms.author: tamram
ms.openlocfilehash: b94a0f95454f9243ef09ce37a62466bca4003a91
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-use-azure-blob-storage-from-python"></a>Hoe Azure Blob storage gebruiken met Python
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht
Azure Blob Storage is een service waarmee ongestructureerde gegevens als objecten/blobs worden opgeslagen in de cloud. In Blob Storage kan elk type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstallatieprogramma, worden opgeslagen. U kunt Blob Storage zien als een vorm van objectopslag.

In dit artikel wordt beschreven hoe u veelvoorkomende scenario's met behulp van Blob-opslag uitvoeren. De voorbeelden zijn geschreven in Python en gebruik de [Microsoft Azure-opslag-SDK voor Python]. De scenario's worden behandeld bevatten uploaden, aanbieding, downloaden en verwijderen van blobs.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Download en installeer Azure opslag-SDK voor Python

Azure-opslag-SDK voor Python Python 2.7, 3.3, 3.4, 3.5 of 3.6 vereist en wordt geleverd in verschillende pakketten 4: `azure-storage-blob`, `azure-storage-file`, `azure-storage-table` en `azure-storage-queue`. In deze zelfstudie gaan we gebruiken `azure-storage-blob` pakket.
 
### <a name="install-via-pypi"></a>Installeren via PyPi

Als u wilt installeren via de Python Package Index (PyPI), typt u:

```bash
pip install azure-storage-blob
```


> [!NOTE]
> Als u van de Azure-opslag-SDK voor Python versie 0.36 of eerder bijwerkt, moet u eerst verwijderen met behulp van `pip uninstall azure-storage` als niet langer wij de opslag-SDK voor Python in één pakket brengen.
> 
> 

Voor de van alternatieve installatiemethoden, gaat u naar de [Azure-opslag-SDK voor Python op Github](https://github.com/Azure/azure-storage-python/).

## <a name="create-a-container"></a>Een container maken
Op basis van het type van de blob die u wilt gebruiken, maakt een **BlockBlobService**, **AppendBlobService**, of **PageBlobService** object. De volgende code wordt een **BlockBlobService** object. De volgende boven aan elk Python-bestand waarin u programmatisch toegang biedt tot Azure-blok-Blob-opslag wilt toevoegen.

```python
from azure.storage.blob import BlockBlobService
```

De volgende code maakt een **BlockBlobService** object met de naam en het account opslagaccountsleutel.  'Myaccount' en 'mykey' vervangen door uw accountnaam en de sleutel.

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

In het volgende voorbeeld, kunt u een **BlockBlobService** object te maken van de container als deze niet bestaat.

```python
block_blob_service.create_container('mycontainer')
```

Standaard is de nieuwe container privé, dus u uw toegangssleutel voor opslag opgeven moet (zoals u eerder hebt gedaan) om blobs te downloaden uit deze container. Als u de blobs in de container beschikbaar maken voor iedereen wilt, kunt u de container maken en geeft het niveau van de openbare toegang met de volgende code.

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

U kunt ook een container wijzigen nadat u hebt gemaakt met behulp van de volgende code.

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

Na deze wijziging iedereen op Internet kan blobs in een openbare container zien, maar u kunt alleen wijzigen of verwijderen.

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container
Voor het maken van een blok-blob en gegevens uploaden, gebruiken de **maken\_blob\_van\_pad**, **maken\_blob\_van\_stroom**, **maken\_blob\_van\_bytes** of **maken\_blob\_van\_tekst** methoden. Op hoog niveau methoden voor het uitvoeren van de benodigde verdeling in segmenten wanneer de grootte van de gegevens is groter dan 64 MB zijn.

**maken\_blob\_van\_pad** uploadt u de inhoud van een bestand uit het opgegeven pad en **maken\_blob\_van\_stroom** de inhoud van een al geopend bestandsstroom uploadt. **maken\_blob\_van\_bytes** een bytematrix, uploadt en **maken\_blob\_van\_tekst** uploadt de opgegeven tekstwaarde met behulp van de opgegeven codering (standaard ingesteld op UTF-8).

Het volgende voorbeeld wordt de inhoud van geüpload de **sunset.png** bestand naar de **myblockblob** blob.

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven
Als de blobs in een container wilt weergeven, gebruikt de **lijst\_blobs** methode. Deze methode retourneert een generator. De volgende code uitvoer de **naam** van elke blob in een container met de console.

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a>Blobs downloaden
Als u wilt downloaden van gegevens van een blob **ophalen\_blob\_naar\_pad**, **ophalen\_blob\_naar\_stroom**, **ophalen\_blob\_naar\_bytes**, of **ophalen\_blob\_naar\_tekst**. Op hoog niveau methoden voor het uitvoeren van de benodigde verdeling in segmenten wanneer de grootte van de gegevens is groter dan 64 MB zijn.

Het volgende voorbeeld worden **ophalen\_blob\_naar\_pad** voor het downloaden van de inhoud van de **myblockblob** blob en sla deze op in de **out sunset.png** bestand.

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a>Een blob verwijderen
Tenslotte voor het verwijderen van een blob, roept **delete_blob**.

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-to-an-append-blob"></a>Schrijven naar een toevoeg-blob
Een toevoeg-blob is geoptimaliseerd voor toevoegbewerkingen, zoals logboekregistratie. Net als een blok-blob bestaat een toevoeg-blob uit blokken, maar wanneer u een nieuw blok aan een toevoeg-blob toevoegt, wordt het altijd toegevoegd aan het einde van de blob. U kunt een bestaand blok in een toevoeg-blob niet bijwerken of verwijderen. De blok-id's voor een toevoeg-blob worden niet weergegeven zoals voor een blok-blob.

Alle blokken in een toevoeg-blob kunnen verschillend van grootte zijn. De maximale grootte is 4 MB. Een toevoeg-blob kan maximaal 50.000 blokken bevatten. De maximale grootte van een toevoeg-blob is daarom iets meer dan 195 GB (4 MB X 50.000 blokken).

In onderstaand voorbeeld wordt een nieuwe toevoeg-blob gemaakt en worden er enkele gegevens aan toegevoegd, waarmee een eenvoudige logboekregistratiebewerking wordt gesimuleerd.

```python
from azure.storage.blob import AppendBlobService
append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

# The same containers can hold all types of blobs
append_blob_service.create_container('mycontainer')

# Append blobs must be created before they are appended to
append_blob_service.create_blob('mycontainer', 'myappendblob')
append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')
```

## <a name="next-steps"></a>Volgende stappen
U bent nu bekend met de basisprincipes van Blob Storage. Volg deze koppelingen voor meer informatie.

* [Python Developer Center](https://azure.microsoft.com/develop/python/)
* [REST-API voor Azure Storage-services](http://msdn.microsoft.com/library/azure/dd179355)
* [Blog van het Azure Storage-team]
* [Microsoft Azure-opslag-SDK voor Python]

[Blog van het Azure Storage-team]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure-opslag-SDK voor Python]: https://github.com/Azure/azure-storage-python
