---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/11/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 1dd11e22361e25721effe2ed919f175d9cb1b9e4
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249630"
---
De volgende tabel beschrijft de standaardlimieten voor Azure Storage. De *inkomend* limiet verwijst naar alle gegevens (aanvragen) die worden verzonden naar een opslagaccount. De *uitgaande* limiet verwijst naar alle gegevens (reacties) worden ontvangen van een storage-account.

| Resource | Standaardlimiet |
| --- | --- |
| Aantal storage-accounts per regio per abonnement, met inbegrip van zowel standard als premium-accounts | 250 |
| Maximale capaciteit van een opslagaccount | 2 PB voor de Verenigde Staten en Europa, 500 TB voor alle andere regio's, waaronder groot-Brittannië |
| Maximumaantal blob-containers, blobs, bestandsshares, tabellen, wachtrijen, entiteiten of berichten per opslagaccount | Geen limiet |
| Maximale aantal aanvragen<sup>1</sup> per opslagaccount | 20.000 aanvragen per seconde |
| Maximum aantal inkomende<sup>1</sup> per opslagaccount (VS-regio's) | 10 Gbps als RA-GRS/GRS is ingeschakeld, 20 Gbps voor LRS/ZRS<sup>2</sup> |
| Maximum aantal inkomende<sup>1</sup> per opslagaccount (niet-Amerikaanse regio's) | 5 Gbps als RA-GRS/GRS is ingeschakeld, 10 Gbps voor LRS/ZRS<sup>2</sup> |
| Maximum aantal uitgaande voor algemeen gebruik v2 en Blob storage-accounts (alle regio's) | 50 Gbps |
| Maximum aantal uitgaande voor opslagaccounts voor algemeen gebruik v1 (Amerikaanse regio's) | 20 Gbps als RA-GRS/GRS is ingeschakeld, 30 Gbps voor LRS/ZRS <sup>2</sup> |
| Maximum aantal uitgaande voor opslagaccounts voor algemeen gebruik v1 (niet-Amerikaanse regio's) | 10 Gbps als RA-GRS/GRS is ingeschakeld, 15 Gbps voor LRS/ZRS <sup>2</sup> |

<sup>1</sup> azure storage-accounts ondersteunen hogere limieten voor inkomend en IOP's op aanvraag. Om aan te vragen een toename van limieten, neem contact op met [ondersteuning voor Azure](https://azure.microsoft.com/support/faq/).

<sup>2</sup> [azure Storage-replicatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy) opties zijn onder andere:
* **RA-GRS**: Geografisch redundante opslag met leestoegang. Als RA-GRS is ingeschakeld, is uitgaand verkeer doelen voor de secundaire locatie zijn identiek aan die voor de primaire locatie.
* **GRS**: Geografisch redundante opslag. 
* **ZRS**: Zone-redundante opslag.
* **LRS**: Lokaal redundante opslag. 

> [!NOTE]
> Microsoft raadt u aan met behulp van een opslagaccount voor algemeen gebruik v2 voor de meeste scenario's. U kunt gemakkelijk een voor algemeen gebruik v1- of Blob storage-account upgraden naar een algemeen gebruik v2-account zonder uitvaltijd en zonder de noodzaak om gegevens te kopiëren.
>
> Zie voor meer informatie over Azure Storage-accounts, [overzicht met Opslagaccounts](../articles/storage/common/storage-account-overview.md). 

Als de behoeften van uw toepassing de schaalbaarheidsdoelen van een enkel opslagaccount overschrijdt, kunt u uw toepassing voor het gebruik van meerdere opslagaccounts maken. U kunt vervolgens uw gegevensobjecten partitioneren voor deze opslagaccounts. Zie [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor meer informatie over Volumeprijzen.

Alle opslagaccounts worden uitgevoerd op een platte netwerktopologie en ondersteuning voor de schaalbaarheids- en prestatiedoelen die worden beschreven in dit artikel, ongeacht wanneer ze zijn gemaakt. Zie voor meer informatie over de platte netwerkarchitectuur van Azure Storage en schaalbaarheid [Microsoft Azure Storage: Een maximaal beschikbare Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).

