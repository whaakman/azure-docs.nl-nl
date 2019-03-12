---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: rogarana
ms.openlocfilehash: e7e57c6a821731874dcb1d99a3133b6ede1da26e
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553396"
---
**Premium niet-beheerde VM-schijven: De limieten per account**

| Resource | Standaardlimiet |
| --- | --- |
| Totale schijfcapaciteit per account |35 TB |
| Totale momentopnamecapaciteit per account |10 TB |
| Maximale bandbreedte per account (inkomend en uitgaand)<sup>1</sup> |<=50 Gbps |

<sup>1</sup>*inkomend* verwijst naar alle gegevens van aanvragen die worden verzonden naar een opslagaccount. *Uitgaande* verwijst naar alle gegevens van antwoorden die worden ontvangen van een storage-account.

**Premium niet-beheerde VM-schijven: Limieten per schijf**

| Schijftype voor Premium Storage | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- |
| Schijfgrootte |128 GiB |512 GiB |1.024 giB (1 TB) |2,048 GiB (2 TB)|4095 giB (4 TB)|
| Maximum aantal IOP's per schijf |500 |2.300 |5.000 |7.500 |7.500 |
| Maximale doorvoer per schijf |100 MB/sec | 150 MB/sec |200 MB per seconde |250 MB/sec |250 MB/sec |
| Maximum aantal schijven per opslagaccount gebruikt |280 |70 |35 | 17 | 8 |

**Premium niet-beheerde VM-schijven: De limieten per VM**

| Resource | Standaardlimiet |
| --- | --- |
| Maximale IOPS per VM |80.000 IOP's met GS5-VM 's |
| Maximale doorvoer per VM |2000 MB per seconde met GS5-VM 's |

