---
title: 'Azure Storage Stack: Verschillen en overwegingen'
description: Het verschil tussen Azure Stack Storage en Azure Storage, samen met aandachtspunten voor implementatie van Azure-Stack.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
ms.reviwer: xiaofmao
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/08/2017
ms.author: jeffgilb
ms.openlocfilehash: 1dc099fa234e217b682c88f2214fe271c916eec2
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Azure Storage Stack: Verschillen en overwegingen

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Azure Storage van de Stack is de set van opslag cloudservices in Microsoft Azure-Stack. Stack aan Azure Storage biedt blob, table, wachtrij en functionaliteit voor Gebruikersaccountbeheer met Azure consistent semantiek.

In dit artikel bevat een overzicht van de bekende Azure Stack Storage verschillen van Azure Storage. Ook ziet u u andere overwegingen rekening moet houden bij het implementeren van Azure-Stack. Zie voor meer informatie over belangrijke verschillen tussen Azure-Stack en Azure, de [belangrijke overwegingen](azure-stack-considerations.md) onderwerp.

## <a name="cheat-sheet-storage-differences"></a>Blad cheats: opslag verschillen

| Functie | Azure (global) | Azure Stack |
| --- | --- | --- |
|File Storage|Cloud-gebaseerde SMB-bestandsshares ondersteund|Nog niet ondersteund
|Azure Storage Service-versleuteling voor inactieve gegevens|256-bits AES-versleuteling|Nog niet ondersteund
|Type opslagaccount|Voor algemene doeleinden en Azure Blob storage-accounts|Voor algemene doeleinden alleen
|Opties voor gegevensreplicatie|Lokaal redundante opslag, geografisch redundante opslag met leestoegang geografisch redundante opslag en zone-redundante opslag|Lokaal redundante opslag
|Premium Storage|Volledig ondersteund|Kunnen worden ingericht, maar geen prestatielimiet voor of de garantie
|Managed Disks|Premium en standard ondersteund|Nog niet ondersteund
|BLOB-naam|1024 tekens (2048 bytes)|880 tekens (1,760 bytes)
|Maximale grootte van blok-blob|4.75 TB (100 MB X 50.000 blokken)|50.000 x 4 MB (ongeveer 195 GB)
|Pagina-blob snapshotkopieën|Back-up Azure onbeheerde VM schijven zijn gekoppeld aan een actieve virtuele machine die wordt ondersteund|Nog niet ondersteund
|Pagina-blob incrementele snapshotkopieën|Premium en standard Azure pagina-blobs ondersteund|Nog niet ondersteund
|Maximale grootte voor pagina-blob|8 TB|1 TB
|Paginagrootte voor pagina-blob|512 bytes|4 KB
|De partitiesleutel tabel- en rijkoppen sleutelgrootte|1024 tekens (2048 bytes)|400 tekens (800 bytes)

### <a name="metrics"></a>Metrische gegevens
Er zijn ook enkele verschillen met opslag metrische gegevens:
* De transactiegegevens in de metrische gegevens storage geen onderscheid tussen interne of externe netwerkbandbreedte.
* De transactiegegevens in de metrische gegevens storage bevat geen virtuele machine toegang tot de gekoppelde schijven.

## <a name="api-version"></a>API-versie
De volgende versies worden ondersteund met Azure Stack Storage:

* Azure Storage-services voor gegevens: [REST-API-versie 2015-04-05](https://docs.microsoft.com/rest/api/storageservices/Version-2015-04-05?redirectedfrom=MSDN)
* Azure Storage management-services: [2015-05-01-preview 2015-06-15 en 2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN) 

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Azure Stack Storage ontwikkelingsprogramma 's](azure-stack-storage-dev.md)
* [Inleiding tot Azure Stack Storage](azure-stack-storage-overview.md)

