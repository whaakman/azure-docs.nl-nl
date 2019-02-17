---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 07/06/2018
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 937ac1d892cdea849d7e5a89c8e69c6b069f2efd
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56333893"
---
In dit artikel bevat informatie over eerdere generaties groottes van virtuele machines. Met deze grootten zijn nog steeds kunnen worden gebruikt, maar er zijn nieuwere generaties beschikbaar.


## <a name="ds-series"></a>DS-serie

ACU: 160

Premium-opslag:  Ondersteund

Premium Storage Caching:  Ondersteund

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Max. doorvoer van caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijfbewerkingen zonder schijf: IOPS / MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |4 |4000 / 32 (43) |3200 / 32 |2 / 500 |
| Standard_DS2 |2 |7 |14 |8 |8000 / 64 (86) |6400 / 64 |2 / 1000 |
| Standard_DS3 |4 |14 |28 |16 |16.000 / 128 (172) |12.800 / 128 |4 / 2000 |
| Standard_DS4 |8 |28 |56 |32 |32.000 / 256 (344) |25.600 / 256 |8 / 4000 |

<br>

## <a name="ds-series---memory-optimized"></a>DS-serie - geoptimaliseerd voor geheugen

ACU: 160 <sup>1</sup>

Premium-opslag:  Ondersteund

Premium Storage Caching:  Ondersteund

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Max. doorvoer van caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijfbewerkingen zonder schijf: IOPS / MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |8 |8000 / 64 (72) |6400 / 64 |2 / 1000 |
| Standard_DS12 |4 |28 |56 |16 |16.000 / 128 (144) |12.800 / 128 |4 / 2000 |
| Standard_DS13 |8 |56 |112 |32 |32.000 / 256 (288) |25.600 / 256 |8 / 4000 |
| Standard_DS14 |16 |112 |224 |64 |64.000 / 512 (576) |51.200 / 512 |8 / 8000 |

<sup>1</sup> de maximale schijfdoorvoer (IOPS of MBps) die mogelijk is met een virtuele machine uit de DS-serie kan worden beperkt door het aantal, grootte en de striping van de gekoppelde schijven.  Zie voor meer informatie, [ontwerpen voor hoge prestaties](../articles/virtual-machines/windows/premium-storage-performance.md).



## <a name="d-series"></a>D-serie 

ACU: 160

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

| Grootte         | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal gegevensschijven / doorvoer: IOPS | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3,5         | 50             | 3000 / 46 / 23                                           | 4 / 4 x 500                         | 2 / 500                 |
| Standard_D2  | 2         | 7           | 100            | 6000 / 93 / 46                                           | 8 / 8 x 500                         | 2 / 1000                     |
| Standard_D3  | 4         | 14          | 200            | 12.000 / 187 / 93                                         | 16 / 16 x 500                         | 4 / 2000                     |
| Standard_D4  | 8         | 28          | 400            | 24.000 / 375 / 187                                        | 32 / 32 x 500                       | 8 / 4000                     |

<br>

## <a name="d-series---memory-optimized"></a>D-serie - geoptimaliseerd voor geheugen

ACU: 160

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

| Grootte         | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal gegevensschijven / doorvoer: IOPS | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000 / 93 / 46                                           | 8 / 8 x 500                         | 2 / 1000                     |
| Standard_D12 | 4         | 28          | 200            | 12.000 / 187 / 93                                         | 16 / 16 x 500                         | 4 / 2000                     |
| Standard_D13 | 8         | 56          | 400            | 24.000 / 375 / 187                                        | 32 / 32 x 500                       | 8 / 4000                     |
| Standard_D14 | 16        | 112         | 800            | 48.000 / 750 / 375                                        | 64 / 64 x 500                       | 8 / 8000                |

<br>

## <a name="a-series---compute-intensive-instances"></a>A-serie: rekenintensieve exemplaren

ACU: 225

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

Grootten uit de A8-A11- en H-serie worden ook wel *rekenintensieve exemplaren* genoemd. De hardware waarop deze grootten worden uitgevoerd, is ontworpen en geoptimaliseerd voor rekenintensieve en netwerkintensieve toepassingen, waaronder HPC-clustertoepassingen (high-performance computing), modellerings- en simulatietoepassingen. De A8-A11-serie gebruikt Intel Xeon E5-2670 @ 2,6 GHZ en de H-serie gebruikt Intel Xeon E5-2667 v3 @ 3,2 GHz.  In dit artikel bevat informatie over het aantal vcpu's, gegevensschijven, en NIC's, evenals de doorvoer en netwerkbandbreedte opslag voor elke grootte in deze groepering. 

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (HDD): GiB | Max. aantal gegevensschijven | Max. doorvoer gegevensschijf: IOPS | Max. aantal NIC's|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8 <sup>1</sup> |8 |56 |382 |32 |32 x 500 |2 |
| Standard_A9 <sup>1</sup> |16 |112 |382 |64 |64 x 500 |4 |
| Standard_A10 |8 |56 |382 |32 |32 x 500 |2  |
| Standard_A11 |16 |112 |382 |64 |64 x 500 |4 |

<sup>1</sup>voor MPI-toepassingen, toegewezen RDMA-back-endnetwerk is ingeschakeld door FDR InfiniBand-netwerk, die voorziet in een zeer lage latentie en hoge bandbreedte.

<br>

## <a name="a-series"></a>A-serie

ACU: 50-100

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (HDD): GiB | Max. aantal gegevensschijven | Max. doorvoer gegevensschijf: IOPS | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps)  |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0 <sup>1</sup> |1 |0,768 |20 |1 |1 x 500 |2 / 100 |
| Standard_A1 |1 |1,75 |70 |2 |2 x 500 |2 / 500  |
| Standard_A2 |2 |3,5 |135 |4 |4 x 500 |2 / 500 |
| Standard_A3 |4 |7 |285 |8 |8 x 500 |2 / 1000 |
| Standard_A4 |8 |14 |605 |16 |16 x 500 |4 / 2000 |
| Standard_A5 |2 |14 |135 |4 |4 x 500 |2 / 500 |
| Standard_A6 |4 |28 |285 |8 |8 x 500 |2 / 1000 |
| Standard_A7 |8 |56 |605 |16 |16 x 500 |4 / 2000 |
<br>

<sup>1</sup> de A0-grootte wordt overgeschreven naar de fysieke hardware. Alleen bij deze specifieke grootte kunnen implementaties van andere klanten invloed hebben op de prestaties van uw uitgevoerde workload. De relatieve prestaties worden hieronder beschreven, zoals de verwachte basislijn, met een variabiliteit van ongeveer 15 procent.

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard A0 - A4 met CLI en PowerShell

In het klassieke implementatiemodel verschillen sommige namen van VM-grootten enigszins in CLI en in PowerShell:

* Standard_A0 is ExtraSmall 
* Standard_A1 is Small
* Standard_A2 is Medium
* Standard_A3 is Large
* Standard_A4 is ExtraLarge

## <a name="basic-a"></a>Basic A

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
 


