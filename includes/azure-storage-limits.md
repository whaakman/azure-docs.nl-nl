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
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34852055"
---
| Resource | Standaardlimiet |
| --- | --- |
| Aantal storage-accounts per regio per abonnement | 200<sup>1</sup> |
| Maximum aantal opslagaccountcapaciteit | 500 TiB<sup>2</sup> |
| Maximumaantal blob-containers, blobs, bestandsshares, tabellen, wachtrijen, entiteiten of berichten per storage-account | Geen limiet |
| Percentage maximum aantal aanvragen per storage-account | aanvragen per seconde voor 20.000<sup>2</sup> |
| Maximale inkomende<sup>3</sup> per storage-account (ons regio's) | 10 Gbps als RA-GRS/GRS is ingeschakeld, 20 Gbps voor LRS/ZRS<sup>4</sup> |
| Maximum aantal uitgaande<sup>3</sup> per storage-account (ons regio's) | 20 Gbps als RA-GRS/GRS is ingeschakeld, 30 Gbps voor LRS/ZRS<sup>4</sup> |
| Maximale inkomende<sup>3</sup> per storage-account (buiten de Verenigde Staten regio's) | 5 Gbps als RA-GRS/GRS is ingeschakeld, 10 Gbps voor LRS/ZRS<sup>4</sup> |
| Maximum aantal uitgaande<sup>3</sup> per storage-account (buiten de Verenigde Staten regio's) | 10 Gbps als RA-GRS/GRS is ingeschakeld, 15 Gbps voor LRS/ZRS<sup>4</sup> |

<sup>1</sup>bevat standaard- en Premium storage-accounts. Als u meer dan 200 storage-accounts in een bepaald gebied nodig hebt, moet u een aanvraag via [ondersteuning van Azure](https://azure.microsoft.com/support/faq/). Het Azure Storage-team uw bedrijfsscenario controleert en maximaal 250 storage-accounts voor een bepaald gebied kan goedkeuren. 

<sup>2</sup> als u uitgebreide limieten voor uw opslagaccount, contact op met [ondersteuning van Azure](https://azure.microsoft.com/support/faq/). Het Azure Storage-team worden de aanvragen controleren en hogere limieten op basis van geval tot geval kan goedkeuren. Zowel algemeen Blob storage-accounts op aanvraag van de grotere capaciteit, inkomend en uitgaand en snelheid van aanvragen voor ondersteunen. Zie voor de nieuwe maximumwaarden voor Blob storage-accounts [storage-accounts voor grotere, hogere schaal aangekondigd](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).

<sup>3</sup> beperkt alleen door het account inkomend en uitgaand limieten. *Inkomend* verwijst naar alle gegevens (aanvragen) verstuurd naar een opslagaccount. *Uitgaand* verwijst naar alle gegevens (reacties) die worden ontvangen van een opslagaccount.  

<sup>4</sup>azure Storage redundantieopties zijn onder andere:
* **RA-GRS**: geografisch redundante opslag met leestoegang. Als RA-GRS is ingeschakeld, zijn uitgaande doelen voor de secundaire locatie identiek aan die voor de primaire locatie.
* **GRS**: geografisch redundante opslag. 
* **ZRS**: Zone-redundante opslag.
* **LRS**: lokaal redundante opslag. 
