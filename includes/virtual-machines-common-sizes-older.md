---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows, virtual-machines-linux
author: laurenhughes
ms.service: multiple
ms.topic: include
ms.date: 04/11/2019
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: d89a9c4c4498e249dbfbd453ef9772d18ffd213f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60541598"
---
In deze sectie bevat informatie over oudere generaties groottes van virtuele machines. Deze grootten worden nog steeds ondersteund, maar ontvangt geen extra capaciteit. Er zijn nieuwere of een alternatieve grootten die algemeen beschikbaar. Raadpleeg [grootten voor Windows virtuele machines in Azure](../articles/virtual-machines/windows/sizes.md) of [grootten voor virtuele Linux-machines in Azure](../articles/virtual-machines/linux/sizes.md) kiezen van de virtuele machine groottes die het beste aanpassen aan uw behoeften.  

Zie voor meer informatie over het vergroten of verkleinen van een Linux-VM [vergroten of verkleinen van een virtuele Linux-machine met behulp van Azure CLI](../articles/virtual-machines/linux/change-vm-size.md). Als u van Windows-VM's gebruikmaakt en u liever PowerShell gebruikt, Zie [vergroten of verkleinen van een Windows-VM](../articles/virtual-machines/windows/resize-vm.md).  

<br>

### <a name="basic-a"></a>Basic A  

**De aanbeveling voor nieuwere grootte**: [Av2-series](../articles/virtual-machines/windows/sizes-general.md#av2-series)

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

De basislaaggrootten zijn met name bedoeld voor ontwikkelworkloads en andere toepassingen waarvoor geen taakverdeling, automatische schaling of geheugenintensieve virtuele machines zijn vereist.

|Grootte - grootte\naam | vCPU |Geheugen|NIC's (max.)|Maximumgrootte van tijdelijke schijf |Met maximaal gegevensschijven (1023 GB elk)|Met maximaal IOPS (300 per schijf)|
|---|---|---|---|---|---|---|
|A0\Basic_A0|1|768 MB|2| 20 GB|1|1x300|
|A1\Basic_A1|1|1,75 GB|2| 40 GB |2|2x300|
|A2\Basic_A2|2|3,5 GB|2| 60 GB|4|4x300|
|A3\Basic_A3|4|7 GB|2| 120 GB |8|8x300|
|A4\Basic_A4|8|14 GB|2| 240 GB |16|16x300|

<br>

### <a name="a-series"></a>A-serie  

**De aanbeveling voor nieuwere grootte**: [Av2-series](../articles/virtual-machines/windows/sizes-general.md#av2-series)

ACU: 50-100

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (HDD): GiB | Max. aantal gegevensschijven | Max. doorvoer gegevensschijf: IOPS | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps)  |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0&nbsp;<sup>1</sup> |1 |0,768 |20 |1 |1 x 500 |2 / 100 |
| Standard_A1 |1 |1,75 |70 |2 |2 x 500 |2 / 500  |
| Standard_A2 |2 |3,5 |135 |4 |4 x 500 |2 / 500 |
| Standard_A3 |4 |7 |285 |8 |8 x 500 |2 / 1000 |
| Standard_A4 |8 |14 |605 |16 |16 x 500 |4 / 2000 |
| Standard_A5 |2 |14 |135 |4 |4 x 500 |2 / 500 |
| Standard_A6 |4 |28 |285 |8 |8 x 500 |2 / 1000 |
| Standard_A7 |8 |56 |605 |16 |16 x 500 |4 / 2000 |

<sup>1</sup> de A0-grootte wordt overgeschreven naar de fysieke hardware. Alleen bij deze specifieke grootte kunnen implementaties van andere klanten invloed hebben op de prestaties van uw uitgevoerde workload. De relatieve prestaties worden hieronder beschreven, zoals de verwachte basislijn, met een variabiliteit van ongeveer 15 procent.

<br>

### <a name="a-series---compute-intensive-instances"></a>A-serie: rekenintensieve exemplaren  

**De aanbeveling voor nieuwere grootte**: [Av2-series](../articles/virtual-machines/windows/sizes-general.md#av2-series)

ACU: 225

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

Grootten uit de A8-A11- en H-serie worden ook wel *rekenintensieve exemplaren* genoemd. De hardware waarop deze grootten worden uitgevoerd, is ontworpen en geoptimaliseerd voor rekenintensieve en netwerkintensieve toepassingen, waaronder HPC-clustertoepassingen (high-performance computing), modellerings- en simulatietoepassingen. De A8-A11-serie gebruikt Intel Xeon E5-2670 @ 2,6 GHZ en de H-serie gebruikt Intel Xeon E5-2667 v3 @ 3,2 GHz.  

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (HDD): GiB | Max. aantal gegevensschijven | Max. doorvoer gegevensschijf: IOPS | Max. aantal NIC's|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8&nbsp;<sup>1</sup> |8 |56 |382 |32 |32 x 500 |2 |
| Standard_A9&nbsp;<sup>1</sup> |16 |112 |382 |64 |64 x 500 |4 |
| Standard_A10 |8 |56 |382 |32 |32 x 500 |2  |
| Standard_A11 |16 |112 |382 |64 |64 x 500 |4 |

<sup>1</sup>voor MPI-toepassingen, toegewezen RDMA-back-endnetwerk is ingeschakeld door FDR InfiniBand-netwerk, die voorziet in een zeer lage latentie en hoge bandbreedte.  

<br>

### <a name="d-series"></a>D-serie  

**De aanbeveling voor nieuwere grootte**: [Dv3-series](../articles/virtual-machines/windows/sizes-general.md#dv3-series-1)

ACU: 160-250 <sup>1</sup>

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

| Grootte         | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal gegevensschijven / doorvoer: IOPS | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3,5         | 50             | 3000 / 46 / 23                                           | 4 / 4 x 500                         | 2 / 500                 |
| Standard_D2  | 2         | 7           | 100            | 6000 / 93 / 46                                           | 8 / 8 x 500                         | 2 / 1000                     |
| Standard_D3  | 4         | 14          | 200            | 12.000 / 187 / 93                                         | 16 / 16 x 500                         | 4 / 2000                     |
| Standard_D4  | 8         | 28          | 400            | 24.000 / 375 / 187                                        | 32 / 32 x 500                       | 8 / 4000                     |

<sup>1</sup> VM-serie kan worden uitgevoerd op een van de volgende CPU: 2.2 GHz Intel Xeon® E5-2660 v2, 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) of 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell)  

<br>

### <a name="d-series---memory-optimized"></a>D-serie - geoptimaliseerd voor geheugen  

**De aanbeveling voor nieuwere grootte**: [Dv3-series](../articles/virtual-machines/windows/sizes-general.md#dv3-series-1)

ACU: 160-250 <sup>1</sup>

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

| Grootte         | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal gegevensschijven / doorvoer: IOPS | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000 / 93 / 46                                           | 8 / 8 x 500                         | 2 / 1000                     |
| Standard_D12 | 4         | 28          | 200            | 12.000 / 187 / 93                                         | 16 / 16 x 500                         | 4 / 2000                     |
| Standard_D13 | 8         | 56          | 400            | 24.000 / 375 / 187                                        | 32 / 32 x 500                       | 8 / 4000                     |
| Standard_D14 | 16        | 112         | 800            | 48.000 / 750 / 375                                        | 64 / 64 x 500                       | 8 / 8000                |

<sup>1</sup> VM-serie kan worden uitgevoerd op een van de volgende CPU: 2.2 GHz Intel Xeon® E5-2660 v2, 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) of 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell)  

<br>

### <a name="ds-series"></a>DS-serie  

**De aanbeveling voor nieuwere grootte**: [DSv3-series](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv3-series-1)

ACU: 160-250 <sup>1</sup>

Premium-opslag:  Ondersteund

Premium Storage Caching:  Ondersteund

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor opslag in cache en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |4 |4000 / 32 (43) |3200 / 32 |2 / 500 |
| Standard_DS2 |2 |7 |14 |8 |8000 / 64 (86) |6400 / 64 |2 / 1000 |
| Standard_DS3 |4 |14 |28 |16 |16.000 / 128 (172) |12.800 / 128 |4 / 2000 |
| Standard_DS4 |8 |28 |56 |32 |32.000 / 256 (344) |25.600 / 256 |8 / 4000 |

<sup>1</sup> VM-serie kan worden uitgevoerd op een van de volgende CPU: 2.2 GHz Intel Xeon® E5-2660 v2, 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) of 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell)  

<br>

### <a name="ds-series---memory-optimized"></a>DS-serie - geoptimaliseerd voor geheugen  

**De aanbeveling voor nieuwere grootte**: [DSv3-series](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv3-series-1)

ACU: 160-250 <sup>1,2</sup>

Premium-opslag:  Ondersteund

Premium Storage Caching:  Ondersteund

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor opslag in cache en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |8 |8000 / 64 (72) |6400 / 64 |2 / 1000 |
| Standard_DS12 |4 |28 |56 |16 |16.000 / 128 (144) |12.800 / 128 |4 / 2000 |
| Standard_DS13 |8 |56 |112 |32 |32.000 / 256 (288) |25.600 / 256 |8 / 4000 |
| Standard_DS14 |16 |112 |224 |64 |64.000 / 512 (576) |51.200 / 512 |8 / 8000 |

<sup>1</sup> de maximale schijfdoorvoer (IOPS of MBps) die mogelijk is met een virtuele machine uit de DS-serie kan worden beperkt door het aantal, grootte en de striping van de gekoppelde schijven.  Zie voor meer informatie, [ontwerpen voor hoge prestaties](../articles/virtual-machines/windows/premium-storage-performance.md).   
<sup>2</sup> VM-serie kan worden uitgevoerd op een van de volgende CPU: 2.2 GHz Intel Xeon® E5-2660 v2, 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) of 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell)  

<br>
