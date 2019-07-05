---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: vermagit
ms.service: virtual-machines
ms.topic: include
ms.date: 04/26/2019
ms.author: azcspmt;jonbeck;cynthn;amverma
ms.custom: include file
ms.openlocfilehash: c0383fd2ca348cd69f07ed61a7935e4fec7999b9
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538008"
---
Azure H-serie virtuele machines (VM's) zijn ontworpen voor leidinggevenden hoogwaardige prestaties, schaalbaarheid van de MPI, leveren en kostenbesparing voor tal van echte HPC-workloads.

Virtuele machines uit de HB-serie zijn geoptimaliseerd voor toepassingen die geheugenbandbreedte gebruiken, zoals vloeiende dynamics, expliciete beperkte elementanalyse en weermodellen. HB-VM’s zijn voorzien van 60 AMD EPYC 7551-processorkernen en 4 GB RAM-geheugen per CPU-kern, zonder hyperthreading. Het AMD EPYC-platform biedt meer dan 260 GB/s aan geheugenbandbreedte.

CH-serie VM's zijn geoptimaliseerd voor toepassingen die worden aangestuurd door compacte berekening, zoals impliciete eindige-elementenmethode, moleculaire dynamics en rekenkundige chemie. HC-VM’s zijn voorzien van 44 Intel Xeon Platinum 8168-processorkernen en 8 GB aan RAM-geheugen per CPU-kern, zonder hyperthreading. De Intel Xeon Platinum-platform biedt ondersteuning voor Intel uitgebreid ecosysteem van hulpprogramma's zoals de Intel Math Kernel-bibliotheek.

Zowel HB en HC VM's voorzien van 100 Gb per seconde Mellanox EDR InfiniBand in een niet-blokkerende fat-configuratie voor consistente prestaties voor RDMA structuur. HB en HC virtuele machines ondersteunen standard Mellanox/OFED-stuurprogramma's zodanig dat alle MPI-typen en versies, evenals RDMA-termen, worden ook ondersteund.

H-serie VM's zijn geoptimaliseerd voor toepassingen die worden aangestuurd door hoog CPU-frequenties of grote geheugen per kern-vereisten. H-serie VM's functie 8 of 16 Intel Xeon E5-2667 v3 processor-kernen, 7 of 14 GB aan RAM-geheugen per CPU-kern, en dat er geen hyperthreading is. H-serie biedt 56 Gb/sec Mellanox FDR InfiniBand in een niet-blokkerende fat-configuratie voor consistente prestaties voor RDMA structuur. H-serie VM's ondersteunen Intel MPI 5.x en MS-MPI.

## <a name="hb-series"></a>HB-serie

ACU: 199-216

Premium-opslag: Ondersteund

Premium Storage Caching: Ondersteund

| Size | vCPU | Processor | Geheugen (GB) | Geheugenbandbreedte GB/s | Basis CPU-frequentie (GHz) | Alle-cores frequentie (GHz, piek) | Single-core frequentie (GHz, piek) | RDMA-prestaties (Gb/s) | MPI-ondersteuning | Tijdelijke opslag (GB) | Max. aantal gegevensschijven | Maximum aantal Ethernet-NIC 's |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB60rs | 60 | AMD EPYC 7551 | 240 | 263 | 2.0 | 2.55 | 2.55 | 100 | Alle | 700 | 4 | 1 |

<br>

## <a name="hc-series"></a>HC-serie

ACU: 297-315

Premium-opslag: Ondersteund

Premium Storage Caching: Ondersteund


| Size | vCPU | Processor | Geheugen (GB) | Geheugenbandbreedte GB/s | Basis CPU-frequentie (GHz) | Alle-cores frequentie (GHz, piek) | Single-core frequentie (GHz, piek) | RDMA-prestaties (Gb/s) | MPI-ondersteuning | Tijdelijke opslag (GB) | Max. aantal gegevensschijven | Maximum aantal Ethernet-NIC 's |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HC44rs | 44 | Intel Xeon Platinum 8168 | 352 | 191 | 2.7 | 3.4 | 3.7 | 100 | Alle | 700 | 4 | 1 |


<br>

## <a name="h-series"></a>H-serie

ACU: 290-300

Premium-opslag:  Niet ondersteund

Premium Storage Caching:  Niet ondersteund

| Size | vCPU | Processor | Geheugen (GB) | Geheugenbandbreedte GB/s | Basis CPU-frequentie (GHz) | Alle-cores frequentie (GHz, piek) | Single-core frequentie (GHz, piek) | RDMA-prestaties (Gb/s) | MPI-ondersteuning | Tijdelijke opslag (GB) | Max. aantal gegevensschijven | Maximum aantal Ethernet-NIC 's |
| --- | --- |--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 | 8 | Intel Xeon E5 2667 v3 | 56 | 40 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 1000 | 32 | 2 |
| Standard_H16 | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3.6 |  - | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standard_H8m | 8 | Intel Xeon E5 2667 v3 | 112 | 40 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 1000 | 32 | 2 |
| Standard_H16m | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standard_H16r <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standard_H16mr <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 2000 | 64 | 4 |

<sup>1</sup> voor MPI-toepassingen, toegewezen RDMA-back-endnetwerk is ingeschakeld door FDR InfiniBand-netwerk.

<br>
