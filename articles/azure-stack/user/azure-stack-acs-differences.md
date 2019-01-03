---
title: Azure stack opslag verschillen en overwegingen | Microsoft Docs
description: Informatie over de verschillen tussen Azure stack-opslag en Azure-opslag, samen met aandachtspunten voor Azure Stack-implementatie.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/03/2018
ms.author: mabrigg
ms.reviwer: xiaofmao
ms.openlocfilehash: 1393dd32aea8cb6d348092ea1fc56752f659beab
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53717868"
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Azure Stack-opslag: Verschillen en overwegingen

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure Stack-opslag is de set met opslag cloudservices in Microsoft Azure Stack. Azure Stack-opslag biedt blob, table, queue en functionaliteit voor Gebruikersaccountbeheer met Azure-consistente semantiek.

In dit artikel bevat een overzicht van de bekende Azure Stack Storage verschillen met Azure Storage-services. Het is ook een lijst met aandachtspunten bij het implementeren van Azure Stack. Zie voor meer informatie over belangrijke verschillen tussen de globale Azure en Azure Stack, de [belangrijke overwegingen met betrekking tot](azure-stack-considerations.md) artikel.

## <a name="cheat-sheet-storage-differences"></a>Overzichtskaart: Opslag-verschillen

| Functie | Azure (wereldwijd) | Azure Stack |
| --- | --- | --- |
|File Storage|Cloud-gebaseerde SMB-bestandsshares wordt ondersteund|Nog niet ondersteund
|Azure storage-serviceversleuteling voor data-at-Rest|256-bits AES-versleuteling. Ondersteuning voor versleuteling door de klant beheerde sleutels in Key Vault.|BitLocker-128-bits AES-versleuteling. Versleuteling door de klant beheerde sleutels wordt niet ondersteund.
|Type opslagaccount|Voor algemeen gebruik V1, V2 en Blob storage-accounts|Alleen voor algemeen gebruik V1.
|Opties voor gegevensreplicatie|Lokaal redundante opslag, geografisch redundante opslag, geografisch redundante opslag met leestoegang en zone-redundante opslag|Lokaal redundante opslag.
|Premium Storage|Volledig ondersteund|Kan worden ingericht, maar er is geen prestatielimiet voor of garanderen.
|Managed Disks|Premium en standaard worden ondersteund|Als u versie 1808 of hoger ondersteund.
|Blobnaam|1024 tekens (2048 bytes)|880 tekens (1,760 bytes)
|Maximale grootte voor blok-blob|4.75 TB (100 MB X 50.000 blokken)|4.75 TB (100 MB x 50.000 blokken) voor de update 1802 of een nieuwere versie. 50.000 x 4 MB (CA. 195 GB) voor eerdere versies.
|Pagina-blob-momentopname kopiëren|Back-up Azure niet-beheerde VM-schijven die zijn gekoppeld aan een actieve virtuele machine die wordt ondersteund|Nog niet ondersteund.
|Pagina blob-momentopname van incrementele kopie|Premium en standard-pagina in Azure blobs ondersteund|Nog niet ondersteund.
|Opslaglagen voor blob-opslag|Hot, cool en archive storage-lagen.|Nog niet ondersteund.
|Voorlopig verwijderen voor blob-opslag|Algemeen beschikbaar|Nog niet ondersteund.
|Maximale grootte voor pagina-blob|8 TB|1 TB
|Paginaformaat voor pagina-blob|512 bytes|4 KB
|Partitiesleutel van de tabel- en recordsleutels sleutelgrootte|1024 tekens (2048 bytes)|400 tekens (800 bytes)
|BLOB-momentopname|Het maximum aantal momentopnamen van een blob is niet beperkt.|Het maximum aantal momentopnamen van een blob is 1000.
|Azure AD-verificatie voor opslag|In preview|Nog niet ondersteund.
|Onveranderbare BLOB 's|Algemeen beschikbaar|Nog niet ondersteund.
|Firewall- en virtuele-netwerkregels voor opslag|Algemeen beschikbaar|Nog niet ondersteund.|

Er zijn ook verschillen met metrische gegevens over opslag:

* De transactiegegevens in metrische opslaggegevens geen onderscheid tussen interne of externe netwerkbandbreedte.
* De transactiegegevens in metrische gegevens van storage bevat geen virtuele machine toegang tot de gekoppelde schijven.

## <a name="api-version"></a>API-versie

De volgende versies worden ondersteund met Azure Stack-opslag:

Azure Storage service-API's:

1811 update of nieuwere versies:

 - [2017-11-09](https://docs.microsoft.com/rest/api/storageservices/version-2017-11-09)
 - [2017-07-29](https://docs.microsoft.com/rest/api/storageservices/version-2017-07-29)
 - [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
 - [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
 - [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
 - [07-2015-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
 - [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

update 1802 1809 update:

- [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
- [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
- [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
- [07-2015-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Vorige versies:

- [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Azure Storage-services beheer-API's:

- [2015-05-01-preview](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

Vorige versies:

 - [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2015-05-01-preview](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 
## <a name="sdk-versions"></a>SDK-versies

Azure Stack-storage ondersteunt de volgende clientbibliotheken:

| Clientbibliotheek | Ondersteunde versie van Azure Stack | Koppeling                                                                                                                                                                                                                                                                                                                                     | Eindpunt-specificatie       |
|----------------|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET           | Van 6.2.0 naar 8.7.0.          | NuGet-pakket:<br>https://www.nuget.org/packages/WindowsAzure.Storage/<br> <br>Release van GitHub:<br>https://github.com/Azure/azure-storage-net/releases                                                                                                                                                                                    | bestand App.config              |
| Java           | Van 4.1.0 naar 6.1.0           | Maven-pakket:<br>http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage<br> <br>Release van GitHub:<br>https://github.com/Azure/azure-storage-java/releases                                                                                                                                                                    | Verbindingsinstellingen voor tekenreeks      |
| Node.js        | Van 1.1.0 naar 2.7.0           | Koppeling van NPM:<br>https://www.npmjs.com/package/azure-storage<br>(Bijvoorbeeld: Voer ' npm installeren azure-storage@2.7.0")<br> <br>Release van GitHub:<br>https://github.com/Azure/azure-storage-node/releases                                                                                                                                         | De declaratie van de service-exemplaar |
| C++            | Van 2.4.0 naar 3.1.0           | NuGet-pakket:<br>https://www.nuget.org/packages/wastorage.v140/<br> <br>Release van GitHub:<br>https://github.com/Azure/azure-storage-cpp/releases                                                                                                                                                                                          | Verbindingsinstellingen voor tekenreeks      |
| PHP            | Van 0.15.0 naar 1.0.0          | Release van GitHub:<br>https://github.com/Azure/azure-storage-php/releases<br> <br>Via Composer installeren (Zie hieronder voor meer informatie)                                                                                                                                                                                                                  | Verbindingsinstellingen voor tekenreeks      |
| Python         | Van 0.30.0 naar 1.0.0          | Release van GitHub:<br>https://github.com/Azure/azure-storage-python/releases                                                                                                                                                                                                                                                                | De declaratie van de service-exemplaar |
| Ruby           | Van 0.12.1 naar 1.0.1          | Pakket van RubyGems:<br>Algemene:<br>https://rubygems.org/gems/azure-storage-common/<br>BLOB: https://rubygems.org/gems/azure-storage-blob/<br>Wachtrij: https://rubygems.org/gems/azure-storage-queue/<br>Tabel: https://rubygems.org/gems/azure-storage-table/<br> <br>Release van GitHub:<br>https://github.com/Azure/azure-storage-ruby/releases | Verbindingsinstellingen voor tekenreeks      |

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Azure Stack hulpprogramma's voor opslagontwikkeling](azure-stack-storage-dev.md)
* [Inleiding tot Azure Stack-opslag](azure-stack-storage-overview.md)
