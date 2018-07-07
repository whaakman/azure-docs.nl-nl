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
ms.openlocfilehash: 033ae1de25fbaada0c2bce715e6bdd71818b341a
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37906799"
---
<!-- F-series, Fs-series* -->

COMPUTE-geoptimaliseerde VM-grootten hebben een hoog CPU-geheugenverhouding en zijn goed voor webservers met gemiddeld verkeer, netwerkapparatuur, batchprocessen en toepassingsservers. In dit artikel bevat informatie over het aantal vcpu's, gegevensschijven, en NIC's, evenals de doorvoer en netwerkbandbreedte opslag voor elke grootte in deze groepering.

Fsv2-serie is gebaseerd op de Intel® Xeon® Platinum 8168-processor met een frequentie core op basis van de 2,7 GHz en een maximale single-core turbo frequentie van 3,7 GHz. Intel® AVX-512-instructies die nieuw zijn in schaalbare Intel-Processors, biedt tot een 2 X prestatieverhoging op vector verwerkingsworkloads op zowel enkele als dubbele precisie drijvende-kommaberekeningen. Met andere woorden: ze zijn zeer snel voor elke rekenkundige workload. 

Tegen een lagere prijs per uur is de Fsv2-serie de beste waarde in de prijs-prestatieverhouding in de Azure-portfolio, gebaseerd op de Azure Compute Unit (ACU) per vCPU. 

De F-serie is gebaseerd op de 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell)-processor, die met Intel Turbo Boost Technology 2.0 een kloksnelheid kan halen van 3,1 GHz. Dit zijn dezelfde CPU-prestaties als de virtuele machines uit de Dv2-serie.  

Virtuele machines uit de F-serie vormen een uitstekende keuze voor workloads die snellere CPU's nodig hebben, maar niet zo veel geheugen of tijdelijke opslag per vCPU.  Voor workloads voor analysen, gameservers, webservers en batchverwerking is de F-serie wellicht een betere keuze.

De Fs-serie biedt alle voordelen van de F-serie, plus Premium-opslag.

## <a name="fsv2-series-sup1sup"></a>Fsv2-serie <sup>1</sup>

ACU: 195 - 210

Premium Storage: ondersteund

Caching van Premium Storage: ondersteund

| Grootte             | vCPU's | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|------------------------------------------------|
| Standard_F2s_v2  | 2      | 4           | 16             | 4              | 4000 (32)                                                             | 2 / 875                                        |
| Standard_F4s_v2  | 4      | 8           | 32             | 8              | 8000 (64)                                                             | 2 / 1.750                                      |
| Standard_F8s_v2   | 8      | 16          | 64             | 16             | 16000 (128)                                                           | 4 / 3500                                      |
| Standard_F16s_v2 | 16     | 32          | 128            | 32             | 32000 (256)                                                           | 4 / 7000                                      |
| Standard_F32s_v2 | 32     | 64          | 256            | 32             | 64000 (512)                                                           | 8 / 14.000                                     |
| Standard_F64s_v2 | 64     | 128         | 512            | 32             | 128000 (1024)                                                         | 8 / 28.000                                     |
| Standard_F72s_v2<sup>2, 3</sup> | 72     | 144         | 576            | 32             | 144000 (1520)                                                         | 8 / 30,000                                     |

<sup>1</sup> Fsv2-serie VM's zijn uitgerust met Intel® Hyper-Threading-technologie

<sup>2</sup> meer dan 64 vCPU's is een van deze ondersteunde gastbesturingssystemen vereist: Windows Server 2016, Ubuntu 16.04 TNS, SLES 12 SP2, en Red Hat Enterprise Linux, CentOS 7.3 of Oracle Linux 7.3 met LIS 4.2.1

<sup>3</sup> exemplaar is geïsoleerd voor hardware toegewezen aan één klant.

## <a name="fs-series-sup1sup"></a>FS-serie <sup>1</sup>

ACU: 210 - 250

Premium Storage: ondersteund

Caching van Premium Storage: ondersteund

| Grootte | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |4 |4000 / 32 (12) |3200 / 48 |2 / 750 |
| Standard_F2s |2 |4 |8 |8 |8000 / 64 (24) |6400 / 96 |2 / 1500 |
| Standard_F4s |4 |8 |16 |16 |16.000 / 128 (48) |12.800 / 192 |4 / 3000 |
| Standard_F8s |8 |16 |32 |32 |32.000 / 256 (96) |25.600 / 384 |8 / 6000 |
| Standard_F16s |16 |32 |64 |64 |64.000 / 512 (192) |51.200 / 768 |8 / 12000 |

MBps = 10^6 bytes per seconde en GiB = 1024^3 bytes.

<sup>1</sup> de maximale schijfdoorvoer (IOPS of MBps) die mogelijk is met een virtuele machine uit de Fs-serie kan worden beperkt door het aantal, grootte en de striping van de gekoppelde schijven.  Zie [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/virtual-machines/windows/premium-storage.md) (Premium Storage: opslag met hoge prestaties voor Azure VM-workloads) voor meer informatie.


<br>

## <a name="f-series"></a>F-serie

ACU: 210 - 250

Premium-opslag: Niet ondersteund

Premium Storage Caching: Niet ondersteund

| Grootte         | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Maximumaantal gegevensschijven / doorvoer: IOPS | Max. aantal NIC's / verwachte netwerkbandbreedte (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000 / 46 / 23                                           | 4 / 4 x 500                         | 2 / 750                 |
| Standard_F2  | 2         | 4           | 32             | 6000 / 93 / 46                                           | 8 / 8 x 500                         | 2 / 1500                     |
| Standard_F4  | 4         | 8           | 64             | 12.000 / 187 / 93                                         | 16 / 16 x 500                         | 4 / 3000                     |
| Standard_F8  | 8         | 16          | 128            | 24.000 / 375 / 187                                        | 32 / 32 x 500                       | 8 / 6000                     |
| Standard_F16 | 16        | 32          | 256            | 48.000 / 750 / 375                                        | 64 / 64 x 500                       | 8 / 12000           |


<br>


