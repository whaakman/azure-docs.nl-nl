---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 07/06/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 16e2a9cfbd9f08428fddade290117b27bc3401f7
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44369121"
---
Azure virtual machines van de H-serie zijn de nieuwste high performance computing-VM's die zijn gericht op het verwerken van workloads, zoals batchverwerking, analyses, molecuulmodellen en vloeistofdynamica. Deze 8 en 16 worden vCPU-VM's zijn gebouwd op de Intel Haswell E5 2667 V3 processor-technologie met DDR4-geheugen en tijdelijke opslag op basis van SSD. 

Naast een zeer hoge CPU-kracht biedt de H-serie ook verschillende opties voor RDMA-netwerken met lage latentie met gebruik van FDR InfiniBand, evenals verschillende geheugenconfiguraties om geheugenintensieve rekenvereisten te ondersteunen.



## <a name="h-series"></a>H-serie

ACU: 290-300

Premium-opslag: Niet ondersteund

Premium Storage Caching: Niet ondersteund

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






