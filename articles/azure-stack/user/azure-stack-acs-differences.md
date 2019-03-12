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
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: mabrigg
ms.reviwer: xiaofmao
ms.lastreviewed: 01/30/2019
ms.openlocfilehash: bbf5076c195fde6a7c5fcabd8e347b7a0d433e8f
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57763246"
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
|Maximale grootte voor blok-blob|4.75 TB (100 MB X 50.000 blokken)|4.75 TB (100 MB x 50.000 blokken) voor de update 1802 of een nieuwere versie. 50.000 x 4 MB (ongeveer 195 GB) voor eerdere versies.
|Pagina-blob-momentopname kopiëren|Back-up Azure niet-beheerde VM-schijven die zijn gekoppeld aan een actieve virtuele machine die wordt ondersteund|Nog niet ondersteund.
|Pagina blob-momentopname van incrementele kopie|Premium en standard-pagina in Azure blobs ondersteund|Nog niet ondersteund.
|Pagina-blob-facturering|Kosten worden berekend voor unieke's, ongeacht of deze in de blob of in de momentopname. Kan geen extra kosten voor momentopnamen die zijn gekoppeld aan een blob, totdat het basis-blob wordt bijgewerkt.|Kosten worden berekend voor basis-blob en assiociated momentopnamen. Extra kosten in rekening voor elke afzonderlijke momentopname rekening worden gebracht.
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
- [2015-07-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Vorige versies:

- [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
- [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
- [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
- [2015-07-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Azure Storage-services beheer-API's:

1811 update of nieuwere versies:

- [2017-10-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2017-06-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2016-12-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2016-05-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-05-01-preview](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

Vorige versies:

- [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-05-01-preview](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

Zie voor meer informatie over Azure Stack ondersteund storage-clientbibliotheken: [Aan de slag met hulpprogramma's voor opslagontwikkeling Azure Stack](azure-stack-storage-dev.md).

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Azure Stack hulpprogramma's voor opslagontwikkeling](azure-stack-storage-dev.md)
* [Gebruik van hulpmiddelen voor het overbrengen van gegevens voor Azure Stack](azure-stack-storage-transfer.md)
* [Inleiding tot Azure Stack-opslag](azure-stack-storage-overview.md)
