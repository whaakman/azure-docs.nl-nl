---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/23/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 84333b26ac70db4b400f7236d4255f4b57a6ca7d
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572050"
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

<sup>1</sup> azure storage-accounts ondersteunen hogere limieten voor inkomend verkeer op aanvraag. Om aan te vragen een toename van limieten voor inkomend verkeer, neem contact op met [ondersteuning voor Azure](https://azure.microsoft.com/support/faq/).

<sup>2</sup>[azure Storage-replicatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy) opties zijn onder andere:
* **RA-GRS**: geografisch redundante opslag met leestoegang. Als RA-GRS is ingeschakeld, is uitgaand verkeer doelen voor de secundaire locatie zijn identiek aan die voor de primaire locatie.
* **GRS**: geografisch redundante opslag. 
* **ZRS**: Zone-redundante opslag.
* **LRS**: lokaal redundante opslag. 

Als de behoeften van uw toepassing de schaalbaarheidsdoelen van een enkel opslagaccount overschrijdt, kunt u uw toepassing voor het gebruik van meerdere opslagaccounts maken. U kunt vervolgens uw gegevensobjecten partitioneren voor deze opslagaccounts. Zie [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor meer informatie over Volumeprijzen.

Alle opslagaccounts worden uitgevoerd op een platte netwerktopologie en ondersteuning voor de schaalbaarheids- en prestatiedoelen die worden beschreven in dit artikel, ongeacht wanneer ze zijn gemaakt. Zie voor meer informatie over de platte netwerkarchitectuur van Azure Storage en schaalbaarheid [Microsoft Azure Storage: een maximaal beschikbare Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).

