---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/24/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 4441ad1e2940892c1627cbc2d4ee0186e4cfda17
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51264370"
---
**HDD's van Standard beheerde virtuele machine**

| Standard-schijftype  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              | S60 *             | S70 *             | S80 *             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Schijfgrootte in GiB          | 32             | 64             | 128            | 256  | 512            | 1,024    | 2048     | 4.095    | 8.192     | 16.384     | 32.767     |
| IOP's per schijf       | Maximaal 500              | Maximaal 500              | Maximaal 500              | Maximaal 500 | Maximaal 500              | Maximaal 500              | Maximaal 500             | Maximaal 500              | Maximaal 1.300              | Maximaal 2000              | Maximaal 2000              |
| Doorvoer per schijf | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec | Maximaal 60 MiB/sec| Maximaal 300 MiB/sec | Maximaal 500 MiB/sec | Maximaal 500 MiB/sec |

**Standaard beheerde virtuele machine SSD 's**

| Type van de Standard-SSD-schijf  | E10               | E15               | E20             | E30 | E40              | E50              | E60 *             | E70 *             | E80 *             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Schijfgrootte in GiB           | 128             | 256             | 512            | 1,024  | 2048            | 4.095     | 8.192     | 16.384     | 32.767    |
| IOP's per schijf       | Maximaal 500              | Maximaal 500              | Maximaal 500              | Maximaal 500 | Maximaal 500              | Maximaal 500              | Maximaal 500             | Maximaal 500              | Maximaal 1.300              | Maximaal 2000              | Maximaal 2000              |
| Doorvoer per schijf | Maximaal 60 MB per seconde | Maximaal 60 MB per seconde | Maximaal 60 MB per seconde | Maximaal 60 MB per seconde | Maximaal 60 MB per seconde | Maximaal 60 MB per seconde | Maximaal 60 MB per seconde | Maximaal 60 MB per seconde| Maximaal 300 MiB/sec |  Maximaal 500 MiB/sec | Maximaal 500 MiB/sec |

**Premium beheerde schijven van virtuele machines: limieten per schijf**

| Schijftype Premium  | P4               | P6               | P10             | P15 | P20              | S30              | P40              | P50              | P60 *             | P70 *             | P80 *             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Schijfgrootte in GiB           | 32             | 64             | 128            | 256  | 512            | 1,024    | 2048     | 4.095    | 8.192     | 16.384     | 32.767     |
| IOP's per schijf       | Maximaal kunnen 120 | Tot 240              | Maximaal 500              | Maximaal 1100 | Maximaal 2300              | Maximaal 5.000              | Maximaal 7.500             | Maximaal 7.500              | Maximaal 12.500              | Maximaal 15.000              | Maximaal 20.000              |
| Doorvoer per schijf | Maximaal 25 MiB/sec | Maximaal 50 MiB/sec | Maximaal 100 MiB/sec | Maximaal 125 MiB/sec | Maximaal 150 MiB/sec | Maximaal 200 MiB/sec | Maximaal 250 MiB/sec | Maximaal 250 MiB/sec| Maximaal 480 MiB/sec | Maximaal 750 MiB/sec | Maximaal 750 MiB/sec |

**Premium beheerde schijven van virtuele machines: limieten per VM**

| Resource | Standaardlimiet |
| --- | --- |
| Max. aantal IOP's per VM |80.000 IOP's met GS5-VM 's |
| Max. doorvoer per VM |2000 MB/s met GS5-VM 's |
