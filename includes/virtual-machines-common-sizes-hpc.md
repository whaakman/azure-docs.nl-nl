---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 05/22/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 36d59638984c31a98067b95fded9626f31a7f278
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "34669344"
---
Azure virtual machines van de H-serie zijn de nieuwste high performance computing dat VM 's, gericht op intensieve rekenbehoeften, zoals moleculaire modellering en numerieke stromingsleer. Deze 8 en 16 worden vCPU-VM's zijn gebouwd op de Intel Haswell E5 2667 V3 processor-technologie met DDR4-geheugen en tijdelijke opslag op basis van SSD. 

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

<sup>1</sup> voor MPI-toepassingen, toegewezen RDMA-back-endnetwerk is ingeschakeld door FDR InfiniBand-netwerk, die voorziet in een zeer lage latentie en hoge bandbreedte.

<br>






