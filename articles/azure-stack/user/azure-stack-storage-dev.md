---
title: Aan de slag met hulpprogramma's voor opslagontwikkeling Azure Stack | Microsoft Docs
description: Richtlijnen aan de slag met het gebruik van hulpprogramma's voor opslagontwikkeling Azure Stack
services: azure-stack
author: mattbriggs
ms.author: mabrigg
ms.date: 02/27/2019
ms.topic: get-started-article
ms.service: azure-stack
manager: femila
ms.reviewer: xiaofmao
ms.lastreviewed: 02/27/2019
ms.openlocfilehash: 45bd47d0a55f447cc6222bb24de8eafcf37e263b
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010647"
---
# <a name="get-started-with-azure-stack-storage-development-tools"></a>Aan de slag met hulpprogramma's voor opslagontwikkeling Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Microsoft Azure Stack biedt een set met storage-services met blob, table en queue storage.

Gebruik dit artikel als richtlijn om aan de slag met hulpprogramma's voor opslagontwikkeling Azure Stack. U vindt meer gedetailleerde informatie en voorbeelden van code in de bijbehorende Azure storage-zelfstudies.

> [!NOTE]  
> Er zijn bekende verschillen tussen Azure Stack-opslag en Azure-opslag, met inbegrip van specifieke vereisten voor elk platform. Er zijn bijvoorbeeld specifieke-clientbibliotheken en specifieke eindpunt achtervoegsel vereisten voor Azure Stack. Zie voor meer informatie, [Azure Stack-opslag: Verschillen en overwegingen](azure-stack-acs-differences.md).

## <a name="azure-client-libraries"></a>Azure-clientbibliotheken

Voor de opslagclientbibliotheken rekening met de versie die compatibel is met de REST-API. U moet ook het Azure Stack-eindpunt opgeven in uw code.

### <a name="1811-update-or-newer-versions"></a>1811 update of nieuwere versies

| Clientbibliotheek | Ondersteunde versie van Azure Stack | Koppeling | Eindpunt-specificatie |
|----------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET | 9.2.0 | Nuget-pakket:<br>https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0<br> <br>Release van GitHub:<br>https://github.com/Azure/azure-storage-net/releases/tag/v9.2.0 | bestand App.config |
| Java | 7.0.0 | Maven-pakket:<br>https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/7.0.0<br> <br>Release van GitHub:<br>https://github.com/Azure/azure-storage-java/releases/tag/v7.0.0 | Verbindingsinstellingen voor tekenreeks |
| Node.js | 2.8.3 | Koppeling van NPM:<br>https://www.npmjs.com/package/azure-storage<br>(Voer: `npm install azure-storage@2.8.3`)<br> <br>Release van Github:<br>https://github.com/Azure/azure-storage-node/releases/tag/v2.8.3 | De declaratie van de service-exemplaar |
| C++ | 5.2.0 | Nuget-pakket:<br>https://www.nuget.org/packages/Microsoft.Azure.Storage.CPP.v140/5.2.0<br> <br>Release van GitHub:<br>https://github.com/Azure/azure-storage-cpp/releases/tag/v5.2.0 | Verbindingsinstellingen voor tekenreeks |
| PHP | 1.2.0 | Release van GitHub:<br>Algemene: https://github.com/Azure/azure-storage-php/releases/tag/v1.2.0-common<br>Blob: https://github.com/Azure/azure-storage-php/releases/tag/v1.2.0-blob<br>Wachtrij:<br>https://github.com/Azure/azure-storage-php/releases/tag/v1.1.1-queue<br>Tabel: https://github.com/Azure/azure-storage-php/releases/tag/v1.1.0-table<br> <br>Installeren via de Composer (voor meer informatie, [Zie de onderstaande details](#install-php-client-via-composer---current).) | Verbindingsinstellingen voor tekenreeks |
| Python | 1.1.0 | Release van GitHub:<br>Algemene:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-common<br>Blob:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-blob<br>Wachtrij:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-queue | De declaratie van de service-exemplaar |
| Ruby | 1.0.1 | Pakket van RubyGems:<br>Algemene:<br>https://rubygems.org/gems/azure-storage-common/versions/1.0.1<br>Blob: https://rubygems.org/gems/azure-storage-blob/versions/1.0.1<br>Wachtrij: https://rubygems.org/gems/azure-storage-queue/versions/1.0.1<br>Tabel: https://rubygems.org/gems/azure-storage-table/versions/1.0.1<br> <br>Release van GitHub:<br>Algemene: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common<br>Blob: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob<br>Wachtrij: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-queue<br>Tabel: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-table | Verbindingsinstellingen voor tekenreeks |

#### <a name="install-php-client-via-composer---current"></a>PHP-client via Composer - huidige installeren

Te installeren via de Composer: (duren voordat de blob als voorbeeld).

1. Maak een bestand met de naam **composer.json** in de hoofdmap van het project met de volgende code:

    ```json
    {
      "require": {
      "Microsoft/azure-storage-blob":"1.2.0"
      }
    }
    ```

2. Download [composer.phar](http://getcomposer.org/composer.phar) naar de hoofdmap van het project.
3. Uitvoeren: `php composer.phar install`.

### <a name="previous-versions-1802-to-1809-update"></a>Vorige versies (update 1802-1809)

| Clientbibliotheek | Ondersteunde versie van Azure Stack | Koppeling | Eindpunt-specificatie |
|----------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET | 8.7.0 | Nuget-pakket:<br>https://www.nuget.org/packages/WindowsAzure.Storage/8.7.0<br> <br>Release van GitHub:<br>https://github.com/Azure/azure-storage-net/releases/tag/v8.7.0 | bestand App.config |
| Java | 6.1.0 | Maven-pakket:<br>http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/6.1.0<br> <br>Release van GitHub:<br>https://github.com/Azure/azure-storage-java/releases/tag/v6.1.0 | Verbindingsinstellingen voor tekenreeks |
| Node.js | 2.7.0 | Koppeling van NPM:<br>https://www.npmjs.com/package/azure-storage<br>(Voer: `npm install azure-storage@2.7.0`)<br> <br>Release van Github:<br>https://github.com/Azure/azure-storage-node/releases/tag/v2.7.0 | De declaratie van de service-exemplaar |
| C++ | 3.1.0 | Nuget-pakket:<br>https://www.nuget.org/packages/wastorage.v140/3.1.0<br> <br>Release van GitHub:<br>https://github.com/Azure/azure-storage-cpp/releases/tag/v3.1.0 | Verbindingsinstellingen voor tekenreeks |
| PHP | 1.0.0 | Release van GitHub:<br>Algemene: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-common<br>Blob: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-blob<br>Wachtrij:<br>https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-queue<br>Tabel: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-table<br> <br>Installeren via Composer (Zie de onderstaande gegevens).) | Verbindingsinstellingen voor tekenreeks |
| Python | 1.0.0 | Release van GitHub:<br>Algemene:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-common<br>Blob:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-blob<br>Wachtrij:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-queue | De declaratie van de service-exemplaar |
| Ruby | 1.0.1 | Pakket van RubyGems:<br>Algemene:<br>https://rubygems.org/gems/azure-storage-common/versions/1.0.1<br>Blob: https://rubygems.org/gems/azure-storage-blob/versions/1.0.1<br>Wachtrij: https://rubygems.org/gems/azure-storage-queue/versions/1.0.1<br>Tabel: https://rubygems.org/gems/azure-storage-table/versions/1.0.1<br> <br>Release van GitHub:<br>Algemene: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common<br>Blob: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob<br>Wachtrij: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-queue<br>Tabel: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-table | Verbindingsinstellingen voor tekenreeks |

#### <a name="install-php-client-via-composer---previous"></a>PHP-client via Composer - vorige installeren

Te installeren via de Composer: (nemen blob voorbeeld).

1. Maak een bestand met de naam **composer.json** in de hoofdmap van het project met de volgende code:

  ```json
    {
      "require": {
      "Microsoft/azure-storage-blob":"1.0.0"
      }
    }
  ```

2. Download [composer.phar](http://getcomposer.org/composer.phar) naar de hoofdmap van het project.
3. Uitvoeren: `php composer.phar install`.

## <a name="endpoint-declaration"></a>De declaratie van eindpunt

Een Azure Stack-eindpunt bestaat uit twee onderdelen: de naam van een regio en het Azure Stack-domein.
In de Azure Stack Development Kit, het standaardeindpunt is **local.azurestack.external**.
Neem contact op met de cloudbeheerder als u niet zeker weet over uw eindpunt.

## <a name="examples"></a>Voorbeelden

### <a name="net"></a>.NET

Achtervoegsel van het eindpunt is voor Azure Stack opgegeven in het bestand app.config:

```xml
<add key="StorageConnectionString"
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=local.azurestack.external;" />
```

### <a name="java"></a>Java

Achtervoegsel van het eindpunt is voor Azure Stack opgegeven in de installatie van de verbindingsreeks:

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=local.azurestack.external";
```

### <a name="nodejs"></a>Node.js

Achtervoegsel van het eindpunt is voor Azure Stack opgegeven in de declaratie-exemplaar:

```nodejs
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob.local.azurestack.external');
```

### <a name="c"></a>C++

Achtervoegsel van het eindpunt is voor Azure Stack opgegeven in de installatie van de verbindingsreeks:

```cpp
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=local.azurestack.external"));
```

### <a name="php"></a>PHP

Achtervoegsel van het eindpunt is voor Azure Stack opgegeven in de installatie van de verbindingsreeks:

```php
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.local.azurestack.external/;
QueueEndpoint=http:// <storage account name>.queue.local.azurestack.external/;
TableEndpoint=http:// <storage account name>.table.local.azurestack.external/;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Achtervoegsel van het eindpunt is voor Azure Stack opgegeven in de declaratie-exemplaar:

```python
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix='local.azurestack.external')
```

### <a name="ruby"></a>Ruby

Achtervoegsel van het eindpunt is voor Azure Stack opgegeven in de installatie van de verbindingsreeks:

```ruby
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=local.azurestack.external
```

## <a name="blob-storage"></a>Blob Storage

De volgende zelfstudies voor Azure Blob-opslag zijn van toepassing op Azure Stack. Houd er rekening mee de vereiste voor het achtervoegsel van specifieke eindpunt voor Azure Stack wordt beschreven in de vorige [voorbeelden](#examples) sectie.

* [Aan de slag met Azure Blob Storage met .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Blob Storage gebruiken met Java](../../storage/blobs/storage-java-how-to-use-blob-storage.md)
* [Blob Storage gebruiken met Node.js](../../storage/blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Het Blob storage gebruiken met C++](../../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Blob Storage gebruiken met PHP](../../storage/blobs/storage-php-how-to-use-blobs.md)
* [Het gebruik van Azure Blob-opslag met Python](../../storage/blobs/storage-python-how-to-use-blob-storage.md)
* [Blob Storage gebruiken met Ruby](../../storage/blobs/storage-ruby-how-to-use-blob-storage.md)

## <a name="queue-storage"></a>Queue Storage

De volgende zelfstudies voor Azure Queue-opslag zijn van toepassing op Azure Stack. Houd er rekening mee de vereiste voor het achtervoegsel van specifieke eindpunt voor Azure Stack wordt beschreven in de vorige [voorbeelden](#examples) sectie.

* [Aan de slag met Azure Queue Storage met .NET](../../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Queue Storage gebruiken met Java](../../storage/queues/storage-java-how-to-use-queue-storage.md)
* [Queue Storage gebruiken met Node.js](../../storage/queues/storage-nodejs-how-to-use-queues.md)
* [Hoe u Queue storage gebruiken met C++](../../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [Queue Storage gebruiken met PHP](../../storage/queues/storage-php-how-to-use-queues.md)
* [Queue Storage gebruiken met Python](../../storage/queues/storage-python-how-to-use-queue-storage.md)
* [Queue Storage gebruiken met Ruby](../../storage/queues/storage-ruby-how-to-use-queue-storage.md)

## <a name="table-storage"></a>Table Storage

De volgende zelfstudies voor Azure Table-opslag zijn van toepassing op Azure Stack. Houd er rekening mee de vereiste voor het achtervoegsel van specifieke eindpunt voor Azure Stack wordt beschreven in de vorige [voorbeelden](#examples) sectie.

* [Aan de slag met Azure Table Storage met .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Table Storage gebruiken met Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Azure Table Storage gebruiken met Node.js](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Hoe u kunt Table storage gebruiken met C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Table Storage gebruiken met PHP](../../cosmos-db/table-storage-how-to-use-php.md)
* [Over het gebruik van Table storage in Python](../../cosmos-db/table-storage-how-to-use-python.md)
* [Table Storage gebruiken met Ruby](../../cosmos-db/table-storage-how-to-use-ruby.md)

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Microsoft Azure storage](../../storage/common/storage-introduction.md)
