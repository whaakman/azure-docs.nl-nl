---
title: Het gebruik van Blob storage (objectopslag) met Ruby | Microsoft Docs
description: Sla niet-gestructureerde gegevens op in de cloud met Azure Blob Storage (objectopslag).
services: storage
documentationcenter: ruby
author: tamram
manager: timlt
editor: tysonn
ms.assetid: e2fe4c45-27b0-4d15-b3fb-e7eb574db717
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 01/18/2018
ms.author: tamram
ms.openlocfilehash: c4c6d47511acdae7afaf4a535c24c6fcc7e389b1
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="how-to-use-blob-storage-from-ruby"></a>Blob Storage gebruiken met Ruby
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht
Azure Blob Storage is een service waarmee ongestructureerde gegevens als objecten/blobs worden opgeslagen in de cloud. In Blob Storage kan elk type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstallatieprogramma, worden opgeslagen. U kunt Blob Storage zien als een vorm van objectopslag.

Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's met behulp van Blob-opslag uitvoeren. De voorbeelden zijn geschreven met behulp van de Ruby-API. De scenario's worden behandeld: **uploaden, aanbieding, gedownload,** en **verwijderen** blobs.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Een Ruby-toepassing maken
Maak een Ruby toepassing. Zie voor instructies [een Ruby-App maken in App Service op Linux](https://docs.microsoft.com/azure/app-service/containers/quickstart-ruby).


## <a name="configure-your-application-to-access-storage"></a>Uw toepassing configureren voor toegang tot opslag
Voor het gebruik van Azure Storage, die u wilt downloaden en gebruiken van het Ruby azure pakket bevat een set met gemak bibliotheken die met de storage REST-services communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems gebruiken om het pakket te verkrijgen
1. Een opdrachtregelinterface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix).
2. Typ 'gem installeren azure-opslag-blob' in het opdrachtvenster voor het installeren van de gem en afhankelijkheden.

### <a name="import-the-package"></a>Het pakket importeren
Met behulp van uw favoriete teksteditor, voeg de volgende boven aan het Ruby bestand waarop u wilt opslag gebruiken:

```ruby
require "azure/storage/blob"
```

## <a name="set-up-an-azure-storage-connection"></a>Een Azure Storage-verbinding instellen
De azure-module leest de omgevingsvariabelen **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS_KEY** voor meer informatie verbinding maken met uw Azure storage-account vereist. Als deze omgevingsvariabelen zijn niet ingesteld, moet u het account informatie over het gebruik **Azure::Blob::BlobService:: maken** met de volgende code:

```ruby
blob_client = Azure::Storage::Blob::BlobService.create(
    storage_account_name: account_name,
    storage_access_key: account_key
    )
```

Als u deze waarden van een klassiek of Resource Manager-opslagaccount in de Azure-portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar het opslagaccount dat u wilt gebruiken.
3. Klik op de blade instellingen aan de rechterkant **toegangstoetsen**.
4. In de blade van de toegang tot sleutels die wordt weergegeven, ziet u de toegangssleutel 1 en toegangssleutel 2. U kunt een van beide gebruiken.
5. Klik op het pictogram kopiëren om de sleutel naar het Klembord kopiëren.

## <a name="create-a-container"></a>Een container maken
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

De **Azure::Storage::Blob::BlobService** object kunt u samenwerken met containers en blobs. Gebruik voor het maken van een container de **maken\_container()** methode.

Het volgende codevoorbeeld maakt een container of de fout wordt afgedrukt indien deze aanwezig is.

```ruby
azure_blob_service = Azure::Storage::Blob::BlobService.create_from_env
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

Als u de bestanden in de container openbaar maken wilt, kunt u de container machtigingen instellen.

U kunt alleen de <strong>maken\_container()</strong> aanroep om door te geven de **: openbare\_toegang\_niveau** optie:

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

Geldige waarden voor de **: openbare\_toegang\_niveau** optie zijn:

* **BLOB:** geeft openbare leestoegang voor blobs. BLOB-gegevens in deze container via anonieme aanvragen kunnen worden gelezen, maar de containergegevens is niet beschikbaar. Clients kunnen blobs in de container via anonieme aanvragen niet opsommen.
* **container:** Hiermee geeft u op volledige openbare leestoegang voor container en de blob-gegevens. Clients kunnen inventariseren blobs in de container via anonieme aanvraag, maar kunnen containers in het opslagaccount niet inventariseren.

U kunt ook het niveau van de openbare toegang van een container wijzigen met behulp van **ingesteld\_container\_acl()** methode om de openbare toegang op te geven.

Het volgende codevoorbeeld verandert het openbare toegangsniveau voor **container**:

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container
Als u wilt uploaden inhoud naar een blob, gebruiken de **maken\_blok\_blob()** methode voor het maken van de blob, een bestand of tekenreeks gebruiken als de inhoud van de blob.

De volgende code wordt het bestand geüpload **test.png** als een nieuwe blob met de naam 'installatiekopie-blob' in de container.

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven
U kunt de containers gebruiken **list_containers()** methode.
U kunt de blobs in een container gebruiken **lijst\_blobs()** methode. Om alle blobs in een container wilt weergeven, moet u een vervolgtoken geretourneerd door de service te volgen en blijven list_blobs uitvoeren met dit token. Zie de [lijst Blobs REST-API](https://docs.microsoft.com/rest/api/storageservices/list-blobs) voor meer informatie.

De volgende code wordt de uitvoer alle blobs in een container.

```ruby
nextMarker = nil
loop do
    blobs = azure_blob_service.list_blobs(container_name, { marker: nextMarker })
    blobs.each do |blob|
        puts "\tBlob name #{blob.name}"
    end
    nextMarker = blobs.continuation_token
    break unless nextMarker && !nextMarker.empty?
end
```

## <a name="download-blobs"></a>Blobs downloaden
Om blobs te downloaden, gebruiken de **ophalen\_blob()** methode voor het ophalen van de inhoud.

De volgende voorbeeldcode laat zien met **ophalen\_blob()** voor het downloaden van de inhoud van 'installatiekopie-blob' en schrijven naar een lokaal bestand.

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a>Verwijderen van een Blob
Gebruik tot slot voor het verwijderen van een blob, de **verwijderen\_blob()** methode. De volgende voorbeeldcode laat zien hoe u van een blob.

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over complexere opslagtaken, volgt u deze koppelingen:

* [Blog van het Azure Storage-team](http://blogs.msdn.com/b/windowsazurestorage/)
* [Azure-opslag-SDK voor Ruby](https://github.com/azure/azure-storage-ruby) opslagplaats op GitHub
* [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

