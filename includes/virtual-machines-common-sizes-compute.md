---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 04/02/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: f1f02f8b5488609a0c69a6d335c96d3cc9266c71
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170343"
---
<!-- F-series, Fs-series* -->

COMPUTE-geoptimaliseerde VM-grootten hebben een hoog CPU-geheugenverhouding en zijn goed voor webservers met gemiddeld verkeer, netwerkapparatuur, batchprocessen en toepassingsservers. In dit artikel bevat informatie over het aantal vcpu's, gegevensschijven, en NIC's, evenals de doorvoer en netwerkbandbreedte opslag voor elke grootte in deze groepering.

Fsv2-serie is gebaseerd op de Intel® Xeon® Platinum 8168-processor, met een langdurige alle essentiële Turbo kloksnelheid van 3,4 GHz en een maximale single-core turbo frequentie van 3,7 GHz. Intel® AVX-512-instructies die nieuw zijn in schaalbare Intel-Processors, biedt tot een 2 X prestatieverhoging op vector verwerkingsworkloads op zowel enkele als dubbele precisie drijvende-kommaberekeningen. Met andere woorden: ze zijn zeer snel voor elke rekenkundige workload. 

Tegen een lagere prijs per uur is de Fsv2-serie de beste waarde in de prijs-prestatieverhouding in de Azure-portfolio, gebaseerd op de Azure Compute Unit (ACU) per vCPU.

## <a name="fsv2-series-sup1sup"></a>Fsv2-serie <sup>1</sup>

ACU: 195 - 210

Premium-opslag:  Ondersteund

Premium Storage opslaan in cache:  Ondersteund

| Grootte             | vCPU's | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor opslag in cache en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Maximale doorvoer voor schijf zonder caching: IOPS / MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|------------------|--------|-------------|----------------|----------------|--------------------------|--------------------------|-------------------------|
| Standard_F2s_v2  | 2      | 4           | 16             | 4              | 4000 / 31 (32)           | 3200 / 47                | 2 / 875                 |
| Standard_F4s_v2  | 4      | 8           | 32             | 8              | 8000 / 63 (64)           | 6400 / 95                | 2 / 1,750               |
| Standard_F8s_v2  | 8      | 16          | 64             | 16             | 16000 / 127 (128)        | 12800 / 190              | 4 / 3,500               |
| Standard_F16s_v2 | 16     | 32          | 128            | 32             | 32000 / 255 (256)        | 25600 / 380              | 4 / 7,000               |
| Standard_F32s_v2 | 32     | 64          | 256            | 32             | 64000 / 512 (512)        | 51200 / 750              | 8 / 14,000              |
| Standard_F64s_v2 | 64     | 128         | 512            | 32             | 128000 / 1024 (1024)     | 80000 / 1100             | 8 / 28,000              |
| Standard_F72s_v2<sup>2, 3</sup> | 72 | 144 | 576         | 32             | 144000 / 1152 (1520)     | 80000 / 1100             | 8 / 30,000              |


<sup>1</sup> Fsv2-serie VM's zijn uitgerust met Intel® Hyper-Threading-technologie

<sup>2</sup> meer dan 64 vCPU's is een van deze ondersteunde gastbesturingssystemen vereist: WindowsServer 2016, Ubuntu 16.04 TNS, SLES 12 SP2 en Red Hat Enterprise Linux, CentOS 7.3 of Oracle Linux 7.3 met LIS 4.2.1

<sup>3</sup> exemplaar is geïsoleerd voor hardware toegewezen aan één klant.