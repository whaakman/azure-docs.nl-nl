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
ms.openlocfilehash: 6381f8f0e68853183fc3e17e76b4ab93b152b48b
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
| Resource | Standaardlimiet |
| --- | --- |
| Aantal storage-accounts per regio | 200<sup>1</sup> |
| Maximum aantal opslagaccountcapaciteit | 500 TiB<sup>2</sup> |
| Maximumaantal blob-containers, blobs, bestandsshares, tabellen, wachtrijen, entiteiten of berichten per storage-account | Geen limiet |
| Percentage maximum aantal aanvragen per storage-account | aanvragen per seconde voor 20.000<sup>2</sup> |
| Maximale inkomende<sup>3</sup> per storage-account (ons regio's) | 10 Gbps als RA-GRS/GRS is ingeschakeld, 20 Gbps voor LRS/ZRS<sup>4</sup> |
| Maximum aantal uitgaande<sup>3</sup> per storage-account (ons regio's) | 20 Gbps als RA-GRS/GRS is ingeschakeld, 30 Gbps voor LRS/ZRS<sup>4</sup> |
| Maximale inkomende<sup>3</sup> per storage-account (buiten de Verenigde Staten regio's) | 5 Gbps als RA-GRS/GRS is ingeschakeld, 10 Gbps voor LRS/ZRS<sup>4</sup> |
| Maximum aantal uitgaande<sup>3</sup> per storage-account (buiten de Verenigde Staten regio's) | 10 Gbps als RA-GRS/GRS is ingeschakeld, 15 Gbps voor LRS/ZRS<sup>4</sup> |

<sup>1</sup>bevat standaard- en Premium storage-accounts. Als u meer dan 200 opslagaccounts nodig hebt, dient u een aanvraag in te dienen via de [ondersteuning van Azure](https://azure.microsoft.com/support/faq/). Het Azure Storage-team bekijkt dan uw bedrijfsscenario en kan tot 250 opslagaccounts goedkeuren. 

<sup>2</sup> als u uitgebreide limieten voor uw opslagaccount, contact op met [ondersteuning van Azure](https://azure.microsoft.com/support/faq/). Het Azure Storage-team worden de aanvragen controleren en hogere limieten op basis van geval tot geval kan goedkeuren. Zowel algemeen Blob storage-accounts op aanvraag van de grotere capaciteit, inkomend en uitgaand en snelheid van aanvragen voor ondersteunen. Zie voor de nieuwe maximumwaarden voor Blob storage-accounts [storage-accounts voor grotere, hogere schaal aangekondigd](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).

<sup>3</sup> beperkt alleen door het account inkomend en uitgaand limieten. *Inkomend* verwijst naar alle gegevens (aanvragen) verstuurd naar een opslagaccount. *Uitgaand* verwijst naar alle gegevens (reacties) die worden ontvangen van een opslagaccount.  

<sup>4</sup>azure Storage redundantieopties zijn onder andere:
* **RA-GRS**: geografisch redundante opslag met leestoegang. Als RA-GRS is ingeschakeld, zijn uitgaande doelen voor de secundaire locatie identiek aan die voor de primaire locatie.
* **GRS**: geografisch redundante opslag. 
* **ZRS**: Zone-redundante opslag.
* **LRS**: lokaal redundante opslag. 
