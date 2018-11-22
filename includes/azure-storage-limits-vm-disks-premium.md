---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: rogarana
ms.openlocfilehash: 5ac7982d306125804fc5b7873e537f9381f717cb
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279579"
---
**Premium niet-beheerde schijven van virtuele machines: limieten per account**

| Resource | Standaardlimiet |
| --- | --- |
| Totale schijfcapaciteit per account |35 TB |
| Totale momentopnamecapaciteit per account |10 TB |
| Maximale bandbreedte per account (inkomend en uitgaand<sup>1</sup>) |<=50 Gbps |

<sup>1</sup>*Inkomend* verwijst naar alle gegevens (aanvragen) die worden verzonden naar een opslagaccount. *Uitgaand* verwijst naar alle gegevens (reacties) die worden ontvangen van een opslagaccount.

**Premium niet-beheerde schijven van virtuele machines: limieten per schijf**

| Schijftype voor Premium Storage | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- |
| Schijfgrootte |128 GiB |512 GiB |1024 GiB (1 TB) |2048 GiB (2 TB)|4095 GiB (4 TB)|
| Max. aantal IOP's per schijf |500 |2300 |5000 |7500 |7500 |
| Max. doorvoer per schijf |100 MB/s | 150 MB/s |200 MB/s |250 MB/s |250 MB/s |
| Max. aantal schijven per opslagaccount |280 |70 |35 | 17 | 8 |

**Premium niet-beheerde schijven van virtuele machines: limieten per VM**

| Resource | Standaardlimiet |
| --- | --- |
| Max. aantal IOP's per VM |80.000 IOP's met GS5-VM 's |
| Max. doorvoer per VM |2000 MB/s met GS5-VM 's |

