---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/13/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: fb349c9ea2178d84e367e763797d5d93d3ae4c53
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46299103"
---
De volgende tabel beschrijft de standaardlimieten voor Azure Storage. De *inkomend* limiet verwijst naar alle gegevens (aanvragen) die worden verzonden naar een opslagaccount. De *uitgaande* limiet verwijst naar alle gegevens (reacties) worden ontvangen van een storage-account.

| Resource | Standaardlimiet |
| --- | --- |
| Aantal storage-accounts per regio per abonnement, met inbegrip van zowel standard als premium-accounts | 200 |
| Maximale capaciteit van een opslagaccount | 500 TiB |
| Maximumaantal blob-containers, blobs, bestandsshares, tabellen, wachtrijen, entiteiten of berichten per opslagaccount | Geen limiet |
| Maximale aantal aanvragen per opslagaccount | 20.000 aanvragen per seconde |
| Maximale invoer per opslagaccount (VS-regio's) | 10 Gbps als RA-GRS/GRS is ingeschakeld, 20 Gbps voor LRS/ZRS<sup>1</sup> |
| Maximum aantal uitgaande per opslagaccount (VS-regio's) | 50 Gbps |
| Maximale invoer per storage-account (niet-Amerikaanse regio's) | 5 Gbps als RA-GRS/GRS is ingeschakeld, 10 Gbps voor LRS/ZRS<sup>1</sup> |
| Maximum aantal uitgaande per opslagaccount (niet-Amerikaanse regio's) | 50 Gbps |

<sup>1</sup>[azure Storage-replicatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy) opties zijn onder andere:
* **RA-GRS**: geografisch redundante opslag met leestoegang. Als RA-GRS is ingeschakeld, is uitgaand verkeer doelen voor de secundaire locatie zijn identiek aan die voor de primaire locatie.
* **GRS**: geografisch redundante opslag. 
* **ZRS**: Zone-redundante opslag.
* **LRS**: lokaal redundante opslag. 

