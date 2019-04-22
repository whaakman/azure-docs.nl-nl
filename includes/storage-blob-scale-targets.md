---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 4/11/2019
ms.author: tamram
ms.openlocfilehash: b3e2f018a3f1ba2563ba8cf2df6dfd4959be592e
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59737047"
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

<sup>1</sup> enkel object doorvoer is afhankelijk van verschillende factoren, waaronder, maar niet beperkt tot: gelijktijdigheid van taken, de grootte van de bewerking, prestatielaag, snelheid van de bron voor uploads en bestemming voor het downloaden.