---
title: Overzicht van HB-serie VM - virtuele Machines van Azure | Microsoft Docs
description: Meer informatie over de Preview-versie-ondersteuning voor de HB-serie VM-grootte in Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/16/2019
ms.author: amverma
ms.openlocfilehash: bf143aa316a3d373a6303865cdc39ee0aaf27d87
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66809915"
---
# <a name="hb-series-virtual-machines-overview"></a>Overzicht van de virtuele machines van de HB-serie

High performance Computing (HPC) toepassingsprestaties op AMD EPYC maximaliseren, is een doordachte benadering geheugen plaats en proces plaatsing vereist. Hieronder we een overzicht van de architectuur AMD EPYC en onze implementatie van deze op Azure voor HPC-toepassingen. We gebruiken de term 'pNUMA' om te verwijzen naar een fysieke NUMA-domein, en "vNUMA" om te verwijzen naar een gevirtualiseerde NUMA-domein.

Een HB-serie is fysiek, 2 * 32-core EPYC 7551 CPU's voor een totaal van 64 fysieke kernen. Deze 64 kernen zijn onderverdeeld in 16 pNUMA-domeinen (8 per socket), elk met vier kernen is en bekend als een 'CPU complexe' (of 'CCX'). Elke CCX heeft een eigen L3-cache is een besturingssysteem zien hoe een grens pNUMA/vNUMA. Een combinatie van een aaneengesloten CCXs deelt toegang tot twee kanalen van fysieke DRAM (32 GB aan DRAM in HB-reeks servers).

Om ruimte voor de Azure-hypervisor om te werken zonder dat de virtuele machine Hierdoor wordt beïnvloed, behouden we ons fysieke pNUMA domein 0 (de eerste CCX). We vervolgens toewijzen pNUMA domeinen 1-15 (de resterende CCX units) voor de virtuele machine. De virtuele machine wordt weergegeven:

`(15 vNUMA domains) * (4 cores/vNUMA) = 60` kernen per VM

De virtuele machine, zelf, bekend niet is dat pNUMA 0 is niet gegeven. De virtuele machine zich van bewust pNUMA 1-15 als vNUMA 0-14, met 7 vNUMA op vSocket 0 en 8 vNUMA op vSocket 1. Hoewel deze asymmetrische, moet uw besturingssysteem worden opgestart en normaal functioneren. Verderop in deze handleiding vertelt we u het beste uitvoeren van MPI-toepassingen op deze asymmetrische NUMA-indeling.

Proces vastmaken werkt op HB-serie VM's omdat we het onderliggende silicium als openbaren-is op de Gast-VM. Het is raadzaam proces vast te maken voor optimale prestaties en consistentie.

Meer weergeven op [AMD EPYC architectuur](https://bit.ly/2Epv3kC) en [architecturen met meerdere chip](https://bit.ly/2GpQIMb) op LinkedIn. Zie voor meer informatie, de [Afstemmingshandleiding voor AMD-Processors van EPYC HPC](https://bit.ly/2T3AWZ9).

Het volgende diagram toont de scheiding van kernen gereserveerd voor Azure-Hypervisor en de VM HB-serie.

![Scheiding van kernen gereserveerd voor Azure-Hypervisor en HB-serie VM](./media/hb-series-overview/segregation-cores.png)

## <a name="hardware-specifications"></a>Hardwarespecificaties

| HW-specificaties                | VM HB-serie                     |
|----------------------------------|----------------------------------|
| Kerngeheugens                            | 60 (SMT uitgeschakeld)                |
| CPU                              | AMD EPYC 7551*                   |
| CPU-frequentie (niet-AVX)          | ~2.55 GHz (één + alle kernen)   |
| Geheugen                           | 4 GB/core (totaal 240)            |
| Lokale schijf                       | 700 GB NVMe                      |
| Infiniband                       | 100 Gb EDR Mellanox ConnectX-5 ** |
| Netwerk                          | 50 Gb Ethernet (40 Gb kan worden gebruikt) Azure tweede generatie SmartNIC *** |

## <a name="software-specifications"></a>Softwarespecificaties

| Zw specificaties           |VM HB-serie           |
|-----------------------------|-----------------------|
| Max MPI taakgrootte            | 6000 kernen (100 virtuele-machineschaalsets) 12000 kernen (200 virtuele-machineschaalsets)  |
| MPI-ondersteuning                 | MVAPICH2, OpenMPI, MPICH, Platform MPI, Intel MPI  |
| Aanvullende Frameworks       | Unified Communication X, libfabric, PGAS |
| Ondersteuning voor Azure Storage       | Std + Premium (maximaal 4 schijven) |
| Ondersteuning voor het besturingssysteem voor het maken van SRIOV RDMA   | CentOS/RHEL 7.6+, SLES 12 SP4+, WinServer 2016+  |
| Ondersteuning van Azure CycleCloud    | Ja                         |
| Ondersteuning voor Azure Batch         | Ja                         |

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over HPC VM-grootten voor [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc) en [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc) in Azure.

* Meer informatie over [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) in Azure.
