---
title: Vereisten voor Microsoft Azure Data Box Blob-opslag | Microsoft Docs
description: Meer informatie over de ondersteunde versies voor API's, SDK's en -clientbibliotheken voor Azure Data Box-Blob-opslag
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 12/11/2018
ms.author: alkohli
ms.openlocfilehash: e7c2cc0c0ffaae11bd7bf5113c942cdb98397201
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53551352"
---
# <a name="azure-data-box-blob-storage-requirements"></a>Vereisten voor Azure Data Box-Blob-opslag

In dit artikel geeft de versies van de Azure-API's, SDK's en hulpprogramma's die worden ondersteund met de gegevens in Blob-opslag. Gegevens in Blob storage biedt beheerfunctionaliteit blob met Azure-consistente semantiek. In dit artikel bevat ook een overzicht van de bekende Azure Data Box-Blob-opslag verschillen van de Azure Storage-services.

Het is raadzaam dat u lees de informatie zorgvuldig door voordat u verbinding met de gegevens in Blob-opslag maken, en vervolgens deze indien nodig raadplegen.


## <a name="storage-differences"></a>Opslag-verschillen

|     Functie                                             |     Azure Storage                                     |     Gegevens in Blob-opslag |
|---------------------------------------------------------|-------------------------------------------------------|---------------------------|
|    Azure File storage                                   |    Cloud-gebaseerde SMB-bestandsshares wordt ondersteund              |    Niet ondersteund      |
|    Service-versleuteling voor data-at-Rest                  |    256-bits AES-versleuteling                             |    256-bits AES-versleuteling |
|    Type opslagaccount                                 |    Voor algemene doeleinden en Azure blob storage-accounts    |    Alleen voor algemeen gebruik v1|
|    Blobnaam                                            |    1024 tekens (2048 bytes)                     |    880 tekens (1,760 bytes)|
|    Maximale grootte voor blok-blob                              |    4.75 TB (100 MB X 50.000 blokken)                   |    4.75 TB (100 MB x 50.000 blokken) voor Azure Data Box v 1.7 of hoger.|
|    Maximale grootte voor pagina-blob                               |    8 TB                                               |    1 TB                   |
|    Paginaformaat voor pagina-blob                                  |    512 bytes                                          |    4 KB                   |

## <a name="supported-api-versions"></a>Ondersteunde API-versies

De volgende versies van Azure Storage service-API's worden ondersteund met gegevens in Blob-opslag:

Openbare preview-versie (Azure Data Box 1.7 en hoger)

- [2017-04-17](/rest/api/storageservices/version-2017-04-17)
- [2016-05-31](/rest/api/storageservices/version-2016-05-31)
- [2015-12-11](/rest/api/storageservices/version-2015-12-11)
- [07-2015-08](/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](/rest/api/storageservices/version-2015-04-05)

## <a name="supported-sdk-versions"></a>Ondersteunde versies van de SDK

|     Clientbibliotheek     |     Gegevens in Blob storage een ondersteunde versie     |     Koppeling             |     Eindpunt-specificatie         |
|------------------------|-------------------------------------------------|---------------------------------------------|------------------------------------|
|    .NET                |    Van 6.2.0 naar 8.7.0.                         |    Nuget-pakket:   https://www.nuget.org/packages/WindowsAzure.Storage/ <br>Release van GitHub:   https://github.com/Azure/azure-storage-net/releases                                                                      |    bestand App.config                 |
|    Java                |    Van 4.1.0 naar 6.1.0                          |    Maven-pakket:   http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage   <br>Release van GitHub:   https://github.com/Azure/azure-storage-java/releases                                                      |    Verbindingsinstellingen voor tekenreeks         |
|    Node.js             |    Van 1.1.0 naar 2.7.0                          |    Koppeling van NPM:   https://www.npmjs.com/package/azure-storage   (Bijvoorbeeld: Voer ' npm installeren azure-storage@2.7.0")   <br>Release van GitHub:   https://github.com/Azure/azure-storage-node/releases                            |    De declaratie van de service-exemplaar    |
|    C++                 |    Van 2.4.0 naar 3.1.0                          |    Nuget-pakket:   https://www.nuget.org/packages/wastorage.v140/   <br>Release van GitHub:   https://github.com/Azure/azure-storage-cpp/releases                                                                            |    Verbindingsinstellingen voor tekenreeks         |
|    PHP                 |    Van 0.15.0 naar 1.0.0                         |    Release van GitHub:   https://github.com/Azure/azure-storage-php/releases   <br>Via Composer installeren (Zie hieronder voor meer informatie)                                                                                                   |    Verbindingsinstellingen voor tekenreeks         |
|    Python              |    Van 0.30.0 naar 1.0.0                         |    Release van GitHub:   https://github.com/Azure/azure-storage-python/releases                                                                                                                                              |    De declaratie van de service-exemplaar    |
|    Ruby                |    Van 0.12.1 naar 1.0.1                         |    Pakket van RubyGems:<br>Algemene:   https://rubygems.org/gems/azure-storage-common/   <br>BLOB: https://rubygems.org/gems/azure-storage-blob/      <br>Release van GitHub:   https://github.com/Azure/azure-storage-ruby/releases    |                                   |

## <a name="supported-azure-client-libraries"></a>Ondersteunde Azure-clientbibliotheken

Voor gegevens in Blob-opslag zijn er specifieke-clientbibliotheken en vereisten voor specifieke eindpunt-achtervoegsel.

De ondersteunde versies van de REST-API voor gegevens in Blob-opslag zijn 2017-04-17, 2016-05-31 invoert, 2015-12-11, 2015-07-08 en 2015-04-05 voor de Azure Data Box-versie 1.7 of hoger. De gegevens in Blob storage-eindpunten beschikt niet over volledige pariteit met de nieuwste versie van de REST-API van Azure Blob Storage. Voor de opslagclientbibliotheken moet u rekening houden met de versie die compatibel is met de REST-API.

### <a name="azure-data-box-17-onwards"></a>Azure Data Box 1.7 of hoger

| Clientbibliotheek     |Gegevens in Blob storage een ondersteunde versie     | Koppeling   |     Eindpunt-specificatie      |
|--------------------|--------------------------------------------|--------|---------------------------------|
|    .NET                |    8.7.0                                           |    Nuget-pakket:   https://www.nuget.org/packages/WindowsAzure.Storage/8.7.0    <br>Release van GitHub:   https://github.com/Azure/azure-storage-net/releases/tag/v8.7.0                                                                                                                                                                                               |    bestand App.config                 |
|    Java                |    6.1.0                                           |    Maven-pakket:   http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/6.1.0   <br>Release van GitHub:   https://github.com/Azure/azure-storage-java/releases/tag/v6.1.0                                                                                                                                                                              |    Verbindingsinstellingen voor tekenreeks         |
|    Node.js             |    2.7.0                                           |    Koppeling van NPM:   https://www.npmjs.com/package/azure-storage   (Voer: npm installeren azure-storage@2.7.0)   <br>Release van GitHub:   https://github.com/Azure/azure-storage-node/releases/tag/v2.7.0                                                                                                                                                                        |    De declaratie van de service-exemplaar    |
|    C++                 |    3.1.0                                           |    Nuget-pakket:   https://www.nuget.org/packages/wastorage.v140/3.1.0   <br>Release van GitHub:   https://github.com/Azure/azure-storage-cpp/releases/tag/v3.1.0                                                                                                                                                                                                     |    Verbindingsinstellingen voor tekenreeks         |
|    PHP                 |    1.0.0                                           |    Release van GitHub:<br>Algemene: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-common   <br>BLOB: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-blob      <br>Installeren via de Composer (voor meer informatie, Zie de onderstaande details.)                                                                                                             |    Verbindingsinstellingen voor tekenreeks         |
|    Python              |    1.0.0                                           |    Release van GitHub:<br>Algemene:   https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-common <br>BLOB:   https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-blob                                                                                                                                                                          |    De declaratie van de service-exemplaar    |
|    Ruby                |    1.0.1                                           |    Pakket van RubyGems:<br>Algemene:   https://rubygems.org/gems/azure-storage-common/versions/1.0.1   <br>BLOB: https://rubygems.org/gems/azure-storage-blob/versions/1.0.1         <br>Release van GitHub:<br>Algemene: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common   <br>BLOB: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob          |    Verbindingsinstellingen voor tekenreeks         |



### <a name="install-php-client-via-composer---current"></a>PHP-client via Composer - huidige installeren

Te installeren via de Composer: (nemen blob voorbeeld).
Maak een bestand met de naam composer.json in de hoofdmap van het project met de volgende code:

```
 {
   "require": {
   "Microsoft/azure-storage-blob":"1.0.0"
   }
```

Download `composer.phar` naar de hoofdmap van het project.

Uitvoeren: php composer.phar installeren.

### <a name="endpoint-declaration"></a>De declaratie van eindpunt

Een Azure Data Box Blob storage-eindpunt bestaat uit twee onderdelen: de naam van een regio en het domein van de Data Box. In de gegevens in Blob storage-SDK, het standaardeindpunt is <serial no. of the device>. microsoftdatabox.com.  Voor meer informatie over het eindpunt van blob service, gaat u naar [verbinding maken via de gegevens in Blob-opslag](data-box-deploy-copy-data-via-rest.md).
 
## <a name="examples"></a>Voorbeelden

### <a name="net"></a>.NET

Voor gegevens in Blob storage, achtervoegsel van het eindpunt is opgegeven in de `app.config` bestand:

```
<add key="StorageConnectionString"
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=<<serial no. of the device>.microsoftdatabox.com  />
```

### <a name="java"></a>Java

Achtervoegsel van het eindpunt is voor gegevens in Blob storage opgegeven in de installatie van de verbindingsreeks:

```
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=<serial no. of the device>.microsoftdatabox.com ";
```

### <a name="nodejs"></a>Node.js

Achtervoegsel van het eindpunt is voor gegevens in Blob storage opgegeven in de declaratie-exemplaar:

```
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob. <serial no. of the device>.microsoftdatabox.com ');
```

### <a name="c"></a>C++

Achtervoegsel van het eindpunt is voor gegevens in Blob storage opgegeven in de installatie van de verbindingsreeks:

```
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=<serial no. of the device>.microsoftdatabox.com "));
```

### <a name="php"></a>PHP

Achtervoegsel van het eindpunt is voor gegevens in Blob storage opgegeven in de installatie van de verbindingsreeks:

```
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.<serial no. of the device>.microsoftdatabox.com /;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Achtervoegsel van het eindpunt is voor gegevens in Blob storage opgegeven in de declaratie-exemplaar:

```
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix=’<serial no. of the device>.microsoftdatabox.com’)
```

### <a name="ruby"></a>Ruby

Achtervoegsel van het eindpunt is voor gegevens in Blob storage opgegeven in de installatie van de verbindingsreeks:

```
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=<serial no. of the device>.microsoftdatabox.com
```

## <a name="next-steps"></a>Volgende stappen

* [Implementeren van uw Azure Data Box](data-box-deploy-ordered.md)