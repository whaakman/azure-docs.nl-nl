---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 4/20/2019
ms.author: tamram
ms.openlocfilehash: d96f400332b7953b34a157b3b52cf00bb20db76e
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012495"
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

<sup>1</sup> enkel object doorvoer is afhankelijk van verschillende factoren, waaronder, maar niet beperkt tot: gelijktijdigheid, aanvraaggrootte, prestatielaag, snelheid van de bron voor uploads en bestemming voor het downloaden. Om te profiteren van [hoge doorvoer blok-blob](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/) prestatieverbeteringen, gebruikt u een grootte van de aanvraag Blob plaatsen of Put-blokken van > 4 MiB (> 256 MiB voor premium-prestaties blok-blobopslag of voor Data Lake Storage Gen2).