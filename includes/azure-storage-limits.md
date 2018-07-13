---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/03/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 6572adb0d8d629910492603a17988b89acce2f17
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38755585"
---
| Resource | Standaardlimiet |
| --- | --- |
| Aantal storage-accounts per regio per abonnement | 200<sup>1</sup> |
| Maximale capaciteit van een opslagaccount | 500 TiB<sup>2</sup> |
| Maximumaantal blob-containers, blobs, bestandsshares, tabellen, wachtrijen, entiteiten of berichten per opslagaccount | Geen limiet |
| Maximale aantal aanvragen per opslagaccount | 20.000 aanvragen per seconde<sup>2</sup> |
| Maximum aantal inkomende<sup>3</sup> per opslagaccount (VS-regio's) | 10 Gbps als RA-GRS/GRS is ingeschakeld, 20 Gbps voor LRS/ZRS<sup>4</sup> |
| Maximum aantal uitgaande<sup>3</sup> per opslagaccount (VS-regio's) | 20 Gbps als RA-GRS/GRS is ingeschakeld, 30 Gbps voor LRS/ZRS<sup>4</sup> |
| Maximum aantal inkomende<sup>3</sup> per opslagaccount (niet-Amerikaanse regio's) | 5 Gbps als RA-GRS/GRS is ingeschakeld, 10 Gbps voor LRS/ZRS<sup>4</sup> |
| Maximum aantal uitgaande<sup>3</sup> per opslagaccount (niet-Amerikaanse regio's) | 10 Gbps als RA-GRS/GRS is ingeschakeld, 15 Gbps voor LRS/ZRS<sup>4</sup> |

<sup>1</sup>bevat zowel Standard als Premium storage-accounts. Als u meer dan 200 opslagaccounts in een bepaalde regio nodig hebt, moet u een aanvraag via [ondersteuning voor Azure](https://azure.microsoft.com/support/faq/). Het Azure Storage-team controleert uw bedrijfsscenario en kan goedkeuren tot 250 opslagaccounts voor een bepaalde regio. 

<sup>2</sup> als u uitgebreide limieten nodig hebt voor uw storage-account, contact op met [ondersteuning voor Azure](https://azure.microsoft.com/support/faq/). Het Azure Storage-team controleert de aanvraag en hogere limieten op basis van geval tot geval kan goedkeuren. Zowel voor algemeen gebruik en Blob storage-accounts bieden ondersteuning voor grotere capaciteit, Inkomend/uitgaand verkeer, en snelheid van aanvragen voor aanvraag. Zie voor de nieuwe maximumwaarden voor Blob storage-accounts, [aankondiging van grotere, hogere schaal storage-accounts](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).

<sup>3</sup> beperkt tot alleen door de limieten van Inkomend/uitgaand verkeer. *Inkomend verkeer* verwijst naar alle gegevens (aanvragen) die worden verzonden naar een opslagaccount. *Uitgaand* verwijst naar alle gegevens (reacties) die worden ontvangen van een opslagaccount.  

<sup>4</sup>azure Storage redundantieopties zijn onder andere:
* **RA-GRS**: geografisch redundante opslag met leestoegang. Als RA-GRS is ingeschakeld, is uitgaand verkeer doelen voor de secundaire locatie zijn identiek aan die voor de primaire locatie.
* **GRS**: geografisch redundante opslag. 
* **ZRS**: Zone-redundante opslag.
* **LRS**: lokaal redundante opslag. 
