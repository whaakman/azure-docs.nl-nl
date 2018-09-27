---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/25/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a0a777151216cb70b696da088b8a0d34779ebc39
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47396120"
---
De volgende tabel beschrijft de standaardlimieten voor Azure Storage. De *inkomend* limiet verwijst naar alle gegevens (aanvragen) die worden verzonden naar een opslagaccount. De *uitgaande* limiet verwijst naar alle gegevens (reacties) worden ontvangen van een storage-account.

| Resource | Standaardlimiet |
| --- | --- |
| Aantal storage-accounts per regio per abonnement, met inbegrip van zowel standard als premium-accounts | 200 |
| Maximale capaciteit van een opslagaccount<sup>1</sup> | 500 TiB |
| Maximumaantal blob-containers, blobs, bestandsshares, tabellen, wachtrijen, entiteiten of berichten per opslagaccount | Geen limiet |
| Maximale aantal aanvragen<sup>1</sup> per opslagaccount | 20.000 aanvragen per seconde |
| Maximum aantal inkomende<sup>1</sup> per opslagaccount (VS-regio's) | 10 Gbps als RA-GRS/GRS is ingeschakeld, 20 Gbps voor LRS/ZRS<sup>2</sup> |
| Maximum aantal uitgaande<sup>1</sup> per opslagaccount (VS-regio's) | 20 Gbps als RA-GRS/GRS is ingeschakeld, 30 Gbps voor LRS/ZRS |
| Maximum aantal inkomende<sup>1</sup> per opslagaccount (niet-Amerikaanse regio's) | 5 Gbps als RA-GRS/GRS is ingeschakeld, 10 Gbps voor LRS/ZRS<sup>2</sup> |
| Maximum aantal uitgaande<sup>1</sup> per opslagaccount (niet-Amerikaanse regio's) | 10 Gbps als RA-GRS/GRS is ingeschakeld, 15 Gbps voor LRS/ZRS |

<sup>1</sup> azure storage-accounts ondersteunen hogere limieten voor capaciteit, snelheid van aanvragen voor inkomend en uitgaand verkeer op aanvraag. Zie voor meer informatie over de verbeterde limieten [aankondiging van grotere, hogere schaal storage-accounts](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/). Om aan te vragen een toename van limieten, neem contact op met [ondersteuning voor Azure](https://azure.microsoft.com/support/faq/).

<sup>2</sup>[azure Storage-replicatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy) opties zijn onder andere:
* **RA-GRS**: geografisch redundante opslag met leestoegang. Als RA-GRS is ingeschakeld, is uitgaand verkeer doelen voor de secundaire locatie zijn identiek aan die voor de primaire locatie.
* **GRS**: geografisch redundante opslag. 
* **ZRS**: Zone-redundante opslag.
* **LRS**: lokaal redundante opslag. 

