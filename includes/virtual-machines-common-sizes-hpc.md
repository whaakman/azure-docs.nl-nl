---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: ee145e5784ae6da6cce22f1b21f9a5d45335ea8b
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
Grootten uit de A8-A11- en H-serie worden ook wel *rekenintensieve exemplaren* genoemd. De hardware waarop deze grootten worden uitgevoerd, is ontworpen en geoptimaliseerd voor rekenintensieve en netwerkintensieve toepassingen, waaronder HPC-clustertoepassingen (high-performance computing), modellerings- en simulatietoepassingen. De A8-A11-serie gebruikt Intel Xeon E5-2670 @ 2,6 GHZ en de H-serie gebruikt Intel Xeon E5-2667 v3 @ 3,2 GHz.  In dit artikel bevat informatie over het aantal Vcpu, gegevensschijven, en NIC's, evenals opslag doorvoer en bandbreedte voor elke grootte in deze groepering. 

Virtuele machines in Azure H-serie zijn de meest recente in high performance computing virtuele machines die zijn gericht op het hoge einde behoeften, zoals moleculaire modellering en computational fluid dynamics. Deze 8 en 16 zijn vCPU virtuele machines op het Intel Haswell E5 2667 V3-processor-technologie met DDR4 geheugen en het tijdelijke opslag op basis van SSD gebouwd. 

Naast een zeer hoge CPU-kracht biedt de H-serie ook verschillende opties voor RDMA-netwerken met lage latentie met gebruik van FDR InfiniBand, evenals verschillende geheugenconfiguraties om geheugenintensieve rekenvereisten te ondersteunen.



## <a name="h-series"></a>H-serie

ACU: 290-300

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Max. doorvoer schijf: IOPS | Max. aantal NIC's |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |32 |32 x 500 |2  |
| Standard_H16 |16 |112 |2000 |64 |64 x 500 |4 |
| Standard_H8m |8 |112 |1000 |32 |32 x 500 |2  |
| Standard_H16m |16 |224 |2000 |64 |64 x 500 |4  |
| Standard_H16r <sup>1</sup> |16 |112 |2000 |64 |64 x 500 |4  |
| Standard_H16mr <sup>1</sup> |16 |224 |2000 |64 |64 x 500 |4 |

<sup>1</sup> voor MPI-toepassingen, speciaal RDMA back-end-netwerk is ingeschakeld door FDR InfiniBand-netwerk, waardoor ultra laag-latentie en hoge bandbreedte levert.

<br>



## <a name="a-series---compute-intensive-instances"></a>A-serie: rekenintensieve exemplaren

ACU: 225

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (HDD): GiB | Max. aantal gegevensschijven | Max. doorvoer gegevensschijf: IOPS | Max. aantal NIC's|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8 <sup>1</sup> |8 |56 |382 |32 |32 x 500 |2 |
| Standard_A9 <sup>1</sup> |16 |112 |382 |64 |64x500 |4 |
| Standard_A10 |8 |56 |382 |32 |32 x 500 |2  |
| Standard_A11 |16 |112 |382 |64 |64x500 |4 |

<sup>1</sup>voor MPI-toepassingen, speciaal RDMA back-end-netwerk is ingeschakeld door FDR InfiniBand-netwerk, waardoor ultra laag-latentie en hoge bandbreedte levert.

<br>



