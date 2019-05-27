---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 4/20/2019
ms.author: tamram
ms.openlocfilehash: aab17966862c57a52f252b3c4e9b757673078b0a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66114819"
---
| Resource | Doel        |
|----------|---------------|
| Maximale grootte van één blob-container | Hetzelfde als de maximale capaciteit-account |
| Maximum aantal blokken in een blok-blob of toevoeg-blob | 50.000 blokken |
| Maximale grootte van een blok in een blok-blob | 100 MiB |
| Maximale grootte van een blok-blob | 50.000 x 100 MiB (ongeveer 4.75 TiB) |
| Maximale grootte van een blok in een toevoeg-blob | 4 MiB |
| Maximale grootte van een toevoeg-blob | 50.000 x 4 MiB (ongeveer 195 GiB) |
| Maximale grootte van een pagina-blob | 8 TiB |
| Maximum aantal opgeslagen toegangsbeleid per blob-container | 5 |
|Doeldoorvoer van één blob |Maximaal Inkomend/uitgaand verkeer opslagaccountlimieten<sup>1</sup> |

<sup>1</sup> enkel object doorvoer is afhankelijk van verschillende factoren, waaronder, maar niet beperkt tot: gelijktijdigheid, aanvraaggrootte, prestatielaag, snelheid van de bron voor uploads en bestemming voor het downloaden. Om te profiteren van [hoge doorvoer blok-blob](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/) prestatieverbeteringen, gebruikt u een grootte van de aanvraag Blob plaatsen of Put-blokken van > 4 MiB (> 256 KiB voor premium-prestaties blok-blobopslag of voor Data Lake Storage Gen2).
