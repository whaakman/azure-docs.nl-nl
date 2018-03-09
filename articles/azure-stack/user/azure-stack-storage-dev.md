---
title: Aan de slag met Azure Stack Storage ontwikkelingsprogramma 's
description: Richtlijnen aan de slag met het gebruik van Azure Stack Storage ontwikkelingsprogramma 's
services: azure-stack
author: mabriggs
ms.author: mabrigg
ms.date: 02/21/2018
ms.topic: get-started-article
ms.service: azure-stack
manager: femila
ms.reviewer: xiaofmao
ms.openlocfilehash: 81c62fc569e9f758d08bfca0bdfc5bcc9ed5860f
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="get-started-with-azure-stack-storage-development-tools"></a>Aan de slag met Azure Stack Storage ontwikkelingsprogramma 's

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Microsoft Azure-Stack biedt een set van opslagservices, inclusief Azure Blob, Table en Queue storage.

In dit artikel biedt snelle richtlijnen voor het gebruik van Azure Stack Storage ontwikkelingsprogramma's. U vindt meer gedetailleerde informatie en voorbeeldcode in de bijbehorende zelfstudies voor Azure Storage.

Er zijn bekende verschillen tussen Azure Storage en Azure Stack opslag, met inbegrip van een aantal specifieke vereisten voor elk platform. Er zijn bijvoorbeeld specifieke clientbibliotheken en specifieke eindpunt achtervoegsel vereisten voor Azure-Stack. Zie voor meer informatie [Azure Stack Storage: verschillen en overwegingen](azure-stack-acs-differences.md).

## <a name="azure-client-libraries"></a>Azure-clientbibliotheken

De ondersteunde versies van de REST-API voor Azure Stack Storage zijn 2017-04-17, 2016-05-31 2015-12-11, 07-2015-08, 2015-04-05 voor de 1802 update of een nieuwere versies en 2015-04-05 voor eerdere versies. De Stack van Azure-eindpunten beschikt niet over volledige pariteit met de nieuwste versie van de REST-API van Azure Storage. Voor de clientbibliotheken van opslag moet u rekening houden met de versie die compatibel is met de REST-API.

### <a name="1802-update-or-newer-versions"></a>1802 update of een nieuwere versie

| Clientbibliotheek | Ondersteunde versie van Azure Stack | Koppeling | Specificatie van het eindpunt |
|----------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET | 8.7.0 | Nuget-pakket:<br>https://www.nuget.org/packages/WindowsAzure.Storage/8.7.0<br> <br>De versie van GitHub:<br>https://github.com/Azure/azure-storage-net/releases/tag/v8.7.0 | bestand App.config |
| Java | 6.1.0 | Maven pakket:<br>http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/6.1.0<br> <br>De versie van GitHub:<br>https://github.com/Azure/azure-storage-java/releases/tag/v6.1.0 | Verbinding tekenreeks instellen |
| Node.js | 2.7.0 | NPM-koppeling:<br>https://www.npmjs.com/package/azure-storage<br>(Voer: `npm install azure-storage@2.7.0`)<br> <br>De versie van Github:<br>https://github.com/Azure/azure-storage-node/releases/tag/v2.7.0 | Service-exemplaar declaratie |
| C++ | 3.1.0 | Nuget-pakket:<br>https://www.nuget.org/packages/wastorage.v140/3.1.0<br> <br>De versie van GitHub:<br>https://github.com/Azure/azure-storage-cpp/releases/tag/v3.1.0 | Verbinding tekenreeks instellen |
| PHP | 1.0.0 | De versie van GitHub:<br>Algemene: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-common<br>BLOB: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-blob<br>Wachtrij:<br>https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-queue<br>Tabel: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-table<br> <br>Installeren via de Composer (voor meer informatie, [Zie de details hieronder](#install-php-client-via-composer---current).) | Verbinding tekenreeks instellen |
| Python | 1.0.0 | De versie van GitHub:<br>Algemene:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-common<br>Blob:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-blob<br>Wachtrij:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-queue | Service-exemplaar declaratie |
| Ruby | 1.0.1 | RubyGems pakket:<br>Algemene:<br>https://rubygems.org/gems/azure-storage-common/versions/1.0.1<br>Blob: https://rubygems.org/gems/azure-storage-blob/versions/1.0.1<br>Queue: https://rubygems.org/gems/azure-storage-queue/versions/1.0.1<br>Tabel: https://rubygems.org/gems/azure-storage-table/versions/1.0.1<br> <br>De versie van GitHub:<br>Algemene: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common<br>BLOB: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob<br>Wachtrij: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-queue<br>Tabel: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-table | Verbinding tekenreeks instellen |

#### <a name="install-php-client-via-composer---current"></a>PHP-client via Composer - huidige installeren

Te installeren via de Composer: (blob nemen als voorbeeld).

1. Maak een bestand met de naam **composer.json** in de hoofdmap van het project met de volgende code:
  ```php
    {
      "require": {
      "Microsoft/azure-storage-blob":"1.0.0"
      }
    }
  ```
2. Download [composer.phar](http://getcomposer.org/composer.phar) naar de hoofdmap van het project.
3. Voer: `php composer.phar install`.

### <a name="previous-versions"></a>Vorige versies

|Clientbibliotheek|Ondersteunde versie van Azure Stack|Koppeling|Specificatie van het eindpunt|
|---------|---------|---------|---------|
|.NET     |6.2.0|Nuget-pakket:<br>[https://www.nuget.org/packages/WindowsAzure.Storage/6.2.0](https://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)<br><br>De versie van GitHub:<br>[https://github.com/Azure/azure-storage-net/releases/tag/v6.2.1](https://github.com/Azure/azure-storage-net/releases/tag/v6.2.1)|bestand App.config|
|Java|4.1.0|Maven pakket:<br>[http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/4.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/4.1.0)<br><br>De versie van GitHub:<br> [https://github.com/Azure/azure-storage-java/releases/tag/v4.1.0](https://github.com/Azure/azure-storage-java/releases/tag/v4.1.0)|Verbinding tekenreeks instellen|
|Node.js     |1.1.0|NPM-koppeling:<br>[https://www.npmjs.com/package/azure-storage](https://www.npmjs.com/package/azure-storage)<br>(uitvoeren: `npm install azure-storage@1.1.0)`<br><br>De versie van Github:<br>[https://github.com/Azure/azure-storage-node/releases/tag/1.1.0](https://github.com/Azure/azure-storage-node/releases/tag/1.1.0)|Service-exemplaar declaratie||C++|2.4.0|Nuget-pakket:<br>[https://www.nuget.org/packages/wastorage.v140/2.4.0](https://www.nuget.org/packages/wastorage.v140/2.4.0)<br><br>De versie van GitHub:<br>[https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0](https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0)|Verbinding tekenreeks instellen|
|C++|2.4.0|Nuget-pakket:<br>[https://www.nuget.org/packages/wastorage.v140/2.4.0](https://www.nuget.org/packages/wastorage.v140/2.4.0)<br><br>De versie van GitHub:<br>[https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0](https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0)|Verbinding tekenreeks instellen|
|PHP|0.15.0|De versie van GitHub:<br>[https://github.com/Azure/azure-storage-php/releases/tag/v0.15.0](https://github.com/Azure/azure-storage-php/releases/tag/v0.15.0)<br><br>Installeren via de Composer (Zie hieronder voor meer informatie)|Verbinding tekenreeks instellen|
|Python     |0.30.0|PIP pakket:<br> [https://pypi.python.org/pypi/azure-storage/0.30.0](https://pypi.python.org/pypi/azure-storage/0.30.0)<br>(Uitvoeren: `pip install -v azure-storage==0.30.0)`<br><br>De versie van GitHub:<br> [https://github.com/Azure/azure-storage-python/releases/tag/v0.30.0](https://github.com/Azure/azure-storage-python/releases/tag/v0.30.0)|Service-exemplaar declaratie|
|Ruby|0.12.1<br>Preview|RubyGems pakket:<br> [https://rubygems.org/gems/azure-storage/versions/0.12.1.preview](https://rubygems.org/gems/azure-storage/versions/0.12.1.preview)<br><br>De versie van GitHub:<br> [https://github.com/Azure/azure-storage-ruby/releases/tag/v0.12.1](https://github.com/Azure/azure-storage-ruby/releases/tag/v0.12.1)|Verbinding tekenreeks instellen|

#### <a name="install-php-client-via-composer---previous"></a>PHP-client via Composer - vorige installeren

Te installeren via de Composer:

1. Maak een bestand met de naam **composer.json** in de hoofdmap van het project met de volgende code:
  ```php
    {
          "require":{
          "Microsoft/azure-storage":"0.15.0"
          }
    }
  ```
2. Download [composer.phar](http://getcomposer.org/composer.phar) in de hoofdmap van het project.
3. Voer: `php composer.phar install`.

## <a name="endpoint-declaration"></a>De declaratie van het eindpunt

Een Azure-Stack-eindpunt bestaat uit twee onderdelen: de naam van een regio en het domein van Azure-Stack.
In de Azure-Stack Development Kit het standaardeindpunt is **local.azurestack.external**.
Neem contact op met de cloudbeheerder als u niet zeker weet over uw eindpunt.

## <a name="examples"></a>Voorbeelden


### <a name="net"></a>.NET

Azure-Stack, is het achtervoegsel eindpunt opgegeven in het bestand app.config:

```
<add key="StorageConnectionString" 
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=local.azurestack.external;" />
```
### <a name="java"></a>Java

Azure-Stack is het achtervoegsel van het eindpunt opgegeven in de instellingen van de verbindingsreeks:

```
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=local.azurestack.external";
```

### <a name="nodejs"></a>Node.js

Azure-Stack is het achtervoegsel van het eindpunt opgegeven in de declaratie-exemplaar:

```
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob.local.azurestack.external');
```
### <a name="c"></a>C++

Azure-Stack is het achtervoegsel van het eindpunt opgegeven in de instellingen van de verbindingsreeks:

```
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=local.azurestack.external"));
```

### <a name="php"></a>PHP

Azure-Stack is het achtervoegsel van het eindpunt opgegeven in de instellingen van de verbindingsreeks:

```
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.local.azurestack.external/;
QueueEndpoint=http:// <storage account name>.queue.local.azurestack.external/;
TableEndpoint=http:// <storage account name>.table.local.azurestack.external/;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Azure-Stack is het achtervoegsel van het eindpunt opgegeven in de declaratie-exemplaar:

```
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix='local.azurestack.external')
```
### <a name="ruby"></a>Ruby

Azure-Stack is het achtervoegsel van het eindpunt opgegeven in de instellingen van de verbindingsreeks:

```
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=local.azurestack.external
```

## <a name="blob-storage"></a>Blob Storage

De volgende zelfstudies voor Azure Blob-opslag van toepassing zijn op Azure-Stack. Let op de specifieke eindpunt achtervoegsel vereiste voor Azure-Stack beschreven in de vorige [voorbeelden](#examples) sectie.

* [Aan de slag met Azure Blob Storage met .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Blob Storage gebruiken met Java](../../storage/blobs/storage-java-how-to-use-blob-storage.md)
* [Blob Storage gebruiken met Node.js](../../storage/blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Het Blob storage gebruiken met C++](../../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Blob Storage gebruiken met PHP](../../storage/blobs/storage-php-how-to-use-blobs.md)
* [Hoe Azure Blob storage gebruiken met Python](../../storage/blobs/storage-python-how-to-use-blob-storage.md)
* [Blob Storage gebruiken met Ruby](../../storage/blobs/storage-ruby-how-to-use-blob-storage.md)

## <a name="queue-storage"></a>Queue Storage

De volgende zelfstudies voor Azure Queue storage zijn van toepassing op Azure-Stack. Let op de specifieke eindpunt achtervoegsel vereiste voor Azure-Stack beschreven in de vorige [voorbeelden](#examples) sectie.

* [Aan de slag met Azure Queue Storage met .NET](../../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Queue Storage gebruiken met Java](../../storage/queues/storage-java-how-to-use-queue-storage.md)
* [Queue Storage gebruiken met Node.js](../../storage/queues/storage-nodejs-how-to-use-queues.md)
* [Hoe Queue storage gebruiken met C++](../../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [Queue Storage gebruiken met PHP](../../storage/queues/storage-php-how-to-use-queues.md)
* [Queue Storage gebruiken met Python](../../storage/queues/storage-python-how-to-use-queue-storage.md)
* [Queue Storage gebruiken met Ruby](../../storage/queues/storage-ruby-how-to-use-queue-storage.md)


## <a name="table-storage"></a>Table Storage

De volgende zelfstudies voor Azure Table-opslag van toepassing zijn op Azure-Stack. Let op de specifieke eindpunt achtervoegsel vereiste voor Azure-Stack beschreven in de vorige [voorbeelden](#examples) sectie.

* [Aan de slag met Azure Table Storage met .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Table Storage gebruiken met Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Azure Table Storage gebruiken met Node.js](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Hoe Table storage gebruiken met C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Table Storage gebruiken met PHP](../../cosmos-db/table-storage-how-to-use-php.md)
* [Table storage gebruiken in Python](../../cosmos-db/table-storage-how-to-use-python.md)
* [Table Storage gebruiken met Ruby](../../cosmos-db/table-storage-how-to-use-ruby.md)

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Microsoft Azure Storage](../../storage/common/storage-introduction.md)