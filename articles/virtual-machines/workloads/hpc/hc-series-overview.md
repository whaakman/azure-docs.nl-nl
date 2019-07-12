---
title: Overzicht van hybride verbinding-serie VM - virtuele Machines van Azure | Microsoft Docs
description: Meer informatie over de Preview-versie-ondersteuning voor de HC-serie VM-grootte in Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: 6cdb539846104f70dabf684925685fb062fea8af
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797561"
---
# <a name="hc-series-virtual-machine-overview"></a>Overzicht van de virtuele machine van de HC-serie

Prestaties van de HPC-toepassingen op Intel Xeon-Processors schaalbare maximaliseren, is een doordachte benadering voor het proces plaatsing op deze nieuwe architectuur vereist. We beschrijven hier, onze implementatie van deze op Azure HC-serie VM's voor HPC-toepassingen. We gebruiken de term 'pNUMA' om te verwijzen naar een fysieke NUMA-domein, en "vNUMA" om te verwijzen naar een gevirtualiseerde NUMA-domein. Op dezelfde manier gebruiken we de term 'pCore' om te verwijzen naar fysieke CPU-kernen en 'vCore' om te verwijzen naar een gevirtualiseerde CPU-kernen.

Een hybride verbinding-server is fysiek, 2 * 24-core Intel Xeon Platinum 8168 CPU's voor een totaal van 48 fysieke kernen. Elke CPU is een eenmalige pNUMA-domein en heeft toegang tot zes kanalen van DRAM unified. Intel Xeon Platinum CPU's-functie a 4 x groter L2-cache dan in eerdere generaties (1 MB/kern-> 256 KB/core), terwijl tegelijkertijd ook de L3-cache ten opzichte van eerdere Intel CPU's (2,5 MB/core-> 1.375 MB/core).

Aan de ook HC-serie hypervisor-configuratie wordt doorgevoerd in de bovenstaande topologie. Om ruimte voor de Azure-hypervisor om te werken zonder dat de virtuele machine Hierdoor wordt beïnvloed, behouden we ons pCores 0-1 en 24-25 (dat wil zeggen, de eerste 2 pCores op elke socket). We vervolgens toewijzen pNUMA domeinen alle resterende kernen aan de virtuele machine. Zo ziet de virtuele machine:

`(2 vNUMA domains) * (22 cores/vNUMA) = 44` kernen per VM

De virtuele machine heeft geen kennis pCores 0-1 en 24-25 zijn niet tot het krijgen. Het beschrijft dus elke vNUMA alsof er systeemeigen 22 kernen.

Intel Xeon Platinum, Gold en Silver CPU's moet u ook een 2D-op-chip-mesh-netwerk voor communicatie binnen en externe kennismaken met de CPU-socket. Het is raadzaam proces vast te maken voor optimale prestaties en consistentie. Omdat het onderliggende silicium wordt weergegeven als proces vastmaken werkt op HC-serie VM's-is op de Gast-VM. Zie voor meer informatie, [Intel Xeon SP architectuur](https://bit.ly/2RCYkiE).

Het volgende diagram toont de scheiding van kernen gereserveerd voor Azure-Hypervisor en de HC-series-VM.

![Scheiding van kernen gereserveerd voor Azure-Hypervisor en HC-serie VM](./media/hc-series-overview/segregation-cores.png)

## <a name="hardware-specifications"></a>Hardwarespecificaties

| Hardwarespecificaties          | VM uit de HC-serie                     |
|----------------------------------|----------------------------------|
| Kerngeheugens                            | 40 (HT uitgeschakeld)                 |
| CPU                              | Intel Xeon Platinum 8168*        |
| CPU-frequentie (niet-AVX)          | 3,7 GHz (één kern), 2.7 3.4 GHz (alle kernen) |
| Geheugen                           | 8 GB/kerngeheugens (352 totaal)            |
| Lokale schijf                       | 700 GB NVMe                      |
| Infiniband                       | 100 Gb EDR Mellanox ConnectX-5 ** |
| Netwerk                          | 50 Gb Ethernet (40 Gb kan worden gebruikt) Azure tweede generatie SmartNIC *** |

## <a name="software-specifications"></a>Softwarespecificaties

| Softwarespecificaties     | VM uit de HC-serie          |
|-----------------------------|-----------------------|
| Max MPI taakgrootte            | 4400 kernen (100 virtuele-machineschaalsets), 8800 kernen (200 virtuele-machineschaalsets) |
| MPI-ondersteuning                 | MVAPICH2, OpenMPI, MPICH, Platform MPI, Intel MPI  |
| Aanvullende Frameworks       | Unified Communication X, libfabric, PGAS |
| Ondersteuning voor Azure Storage       | Std + Premium (maximaal 4 schijven) |
| Ondersteuning voor het besturingssysteem voor het maken van SRIOV RDMA   | CentOS/RHEL 7.6+, SLES 12 SP4+, WinServer 2016+ |
| Ondersteuning van Azure CycleCloud    | Ja                         |
| Ondersteuning voor Azure Batch         | Ja                         |

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over HPC VM-grootten voor [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc) en [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc) in Azure.

* Meer informatie over [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) in Azure.
