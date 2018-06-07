---
title: Stack van Azure storage verschillen en overwegingen | Microsoft Docs
description: De verschillen tussen Azure stack-opslag- en Azure-opslag, samen met aandachtspunten voor implementatie van Azure-Stack begrijpen.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/21/2018
ms.author: jeffgilb
ms.reviwer: xiaofmao
ms.openlocfilehash: 2a6cb3f1a1f8009af411ba4d97a23194f6f089ae
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604457"
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Stack van Azure storage: verschillen en overwegingen

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Stack van Azure storage is de set van opslag cloudservices in Microsoft Azure-Stack. Stack van Azure storage biedt blob, table, wachtrij en functionaliteit voor Gebruikersaccountbeheer met Azure consistent semantiek.

In dit artikel bevat een overzicht van de bekende Azure Stack Storage verschillen van Azure Storage-services. Het bevat ook overwegingen bij het implementeren van Azure-Stack. Zie voor meer informatie over belangrijke verschillen tussen globale Azure en Azure-Stack, de [belangrijke overwegingen](azure-stack-considerations.md) onderwerp.

## <a name="cheat-sheet-storage-differences"></a>Blad cheats: opslag verschillen

| Functie | Azure (global) | Azure Stack |
| --- | --- | --- |
|File Storage|Cloud-gebaseerde SMB-bestandsshares ondersteund|Nog niet ondersteund
|Azure storage-service: versleuteling voor gegevens in rust|256-bits AES-versleuteling|BitLocker 128-bits AES-versleuteling
|Type opslagaccount|Voor algemene doeleinden en Azure blob storage-accounts|Voor algemene doeleinden alleen.
|Opties voor gegevensreplicatie|Lokaal redundante opslag, geografisch redundante opslag met leestoegang geografisch redundante opslag en zone-redundante opslag|Lokaal redundante opslag.
|Premium Storage|Volledig ondersteund|Kan worden ingericht, maar geen prestatielimiet voor of de garantie.
|Managed Disks|Premium en standard ondersteund|Nog niet ondersteund.
|Blobnaam|1024 tekens (2048 bytes)|880 tekens (1,760 bytes)
|Maximale grootte van blok-blob|4.75 TB (100 MB X 50.000 blokken)|4.75 TB (100 MB x 50.000 blokken) voor de 1802 update of een nieuwere versie. 50.000 x 4 MB (ongeveer 195 GB) voor eerdere versies.
|Pagina-blob snapshotkopieën|Back-up Azure onbeheerde VM schijven zijn gekoppeld aan een actieve virtuele machine die wordt ondersteund|Nog niet ondersteund.
|Pagina-blob incrementele snapshotkopieën|Premium en standard Azure pagina-blobs ondersteund|Nog niet ondersteund.
|Opslaglagen voor blob storage|Hot, cool en archiveren van opslaglagen.|Nog niet ondersteund.
Voorlopig verwijderen voor blob-opslag|Preview|Nog niet ondersteund.
|Maximale grootte voor pagina-blob|8 TB|1 TB
|Paginagrootte voor pagina-blob|512 bytes|4 KB
|De partitiesleutel tabel- en rijkoppen sleutelgrootte|1024 tekens (2048 bytes)|400 tekens (800 bytes)
|BLOB-momentopname|Het maximum aantal momentopnamen van een blob is niet beperkt.|Het maximum aantal momentopnamen van een blob is 1000.|

Er zijn ook verschillen met opslag metrische gegevens:

* De transactiegegevens in de metrische gegevens storage geen onderscheid tussen interne of externe netwerkbandbreedte.
* De transactiegegevens in de metrische gegevens storage bevat geen virtuele machine toegang tot de gekoppelde schijven.

## <a name="api-version"></a>API-versie

De volgende versies worden ondersteund met Azure Stack Storage:

Azure Storage-services API's:

1802 bijwerken of hoger:

 - [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
 - [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
 - [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
 - [07-2015-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
 - [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Vorige versies:

 - [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Azure Storage services management API's:

 - [2015-05-01-preview](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

## <a name="sdk-versions"></a>SDK-versies

Stack aan Azure storage ondersteunt de volgende clientbibliotheken:

| Clientbibliotheek | Ondersteunde versie van Azure Stack | Koppeling                                                                                                                                                                                                                                                                                                                                     | Specificatie van het eindpunt       |
|----------------|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET           | Van 6.2.0 naar 8.7.0.          | Nuget-pakket:<br>https://www.nuget.org/packages/WindowsAzure.Storage/<br> <br>De versie van GitHub:<br>https://github.com/Azure/azure-storage-net/releases                                                                                                                                                                                    | bestand App.config              |
| Java           | Van 4.1.0 naar 6.1.0           | Maven pakket:<br>http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage<br> <br>De versie van GitHub:<br>https://github.com/Azure/azure-storage-java/releases                                                                                                                                                                    | Verbinding tekenreeks instellen      |
| Node.js        | Van 1.1.0 naar 2.7.0           | NPM-koppeling:<br>https://www.npmjs.com/package/azure-storage<br>(Bijvoorbeeld: Voer ' npm installeren azure-storage@2.7.0')<br> <br>De versie van Github:<br>https://github.com/Azure/azure-storage-node/releases                                                                                                                                         | Service-exemplaar declaratie |
| C++            | Van 2.4.0 naar 3.1.0           | Nuget-pakket:<br>https://www.nuget.org/packages/wastorage.v140/<br> <br>De versie van GitHub:<br>https://github.com/Azure/azure-storage-cpp/releases                                                                                                                                                                                          | Verbinding tekenreeks instellen      |
| PHP            | Van 0.15.0 naar 1.0.0          | De versie van GitHub:<br>https://github.com/Azure/azure-storage-php/releases<br> <br>Installeren via de Composer (Zie hieronder voor meer informatie)                                                                                                                                                                                                                  | Verbinding tekenreeks instellen      |
| Python         | Van 0.30.0 naar 1.0.0          | De versie van GitHub:<br>https://github.com/Azure/azure-storage-python/releases                                                                                                                                                                                                                                                                | Service-exemplaar declaratie |
| Ruby           | Van 0.12.1 naar 1.0.1          | RubyGems pakket:<br>Algemene:<br>https://rubygems.org/gems/azure-storage-common/<br>BLOB: https://rubygems.org/gems/azure-storage-blob/<br>Wachtrij: https://rubygems.org/gems/azure-storage-queue/<br>Tabel: https://rubygems.org/gems/azure-storage-table/<br> <br>De versie van GitHub:<br>https://github.com/Azure/azure-storage-ruby/releases | Verbinding tekenreeks instellen      |

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Azure Stack Storage ontwikkelingsprogramma 's](azure-stack-storage-dev.md)
* [Inleiding tot Azure Stack Storage](azure-stack-storage-overview.md)
