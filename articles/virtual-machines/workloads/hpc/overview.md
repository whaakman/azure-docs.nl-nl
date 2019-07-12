---
title: High performance computing op uit de H-serie VM's - Azure Virtual Machines | Microsoft Docs
description: Meer informatie over de functies en mogelijkheden van de H-serie VM's die zijn geoptimaliseerd voor HPC.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: overview
ms.date: 07/02/2019
ms.author: amverma
ms.openlocfilehash: d6e857a87e4c7df8ffb2be1eefb7a0290da5b10a
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800062"
---
# <a name="high-performance-computing-on-h-series-vms"></a>High performance computing op uit de H-serie VM 's

Cloudcomputing hoge prestaties (HPC) op HB-serie en HC-serie VM's inschakelen de meest optimale HPC-prestaties van virtuele machines op Azure. HPC geoptimaliseerde VM's worden gebruikt voor het oplossen van enkele wiskundige moeilijkste problemen zoals: vloeistofdynamica, olie en gas-simulaties en weermodellen.

Dit artikel vindt u enkele belangrijke functies van HB-serie en HC-serie VM's, waarom deze virtuele machines goed in HPC-scenario's, en hoe u presteren aan de slag.

## <a name="features-and-capabilities"></a>Functies en mogelijkheden

HB-serie en HC-serie VM's zijn ontworpen om te bieden de beste prestaties voor HPC, message passing interface (MPI)-schaalbaarheid en kostenbesparing voor HPC-workloads.

### <a name="message-passing-interface"></a>Bericht doorgeven interface

HB-serie en HC-serie ondersteunen bijna alle MPI-typen en versies. Enkele van de meest algemene, ondersteunde typen MPI zijn: OpenMPI, MVAPICH2, Platform MPI, Intel MPI en alle remote direct memory access-bewerkingen (RDMA). Zie voor meer informatie, [Message Passing Interface instellen voor HPC](setup-mpi.md).

### <a name="rdma-and-infiniband"></a>RDMA en InfiniBand

De RDMA-interface is standaard op HB-serie en HC-serie VM's. RDMA-compatibele exemplaren communiceren via een InfiniBand-netwerk, besturingssysteem op de uitgebreide gegevenstarieven (EDR) voor virtuele machines HB-serie en HC-serie. RDMA-compatibele exemplaren kunnen stimuleren de schaalbaarheid en prestaties van sommige MPI-toepassingen.

Het InfiniBand-configuratie ondersteunen HB-serie en HC-serie VM's zijn niet-blokkerende fat structuren met een lage diameter ontwerp voor consistente prestaties voor RDMA.

Zie [InfiniBand inschakelen](enable-infiniband.md) voor meer informatie over het instellen van InfiniBand op uw HB-serie of HC-serie VM's.

## <a name="get-started"></a>Aan de slag

Bepaal eerst welke H-serie VM die u gaat gebruiken. Zie voor meer informatie over HPC virtuele machines geoptimaliseerde, [HB-serie overzicht](hb-series-overview.md) en [overzicht van de HC-serie](hc-series-overview.md). Zie voor specificaties, [High performance computing-VM-grootten](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc).

Nadat u hebt geselecteerd en een virtuele machine gemaakt voor uw toepassing, moet u deze door in te schakelen InfiniBand configureren. Zie voor informatie over het inschakelen van InfiniBand op zowel Windows als Linux-VM's, [InfiniBand inschakelen](enable-infiniband.md).

Een essentieel onderdeel van HPC-workloads is MPI. HB-serie en HC-serie ondersteunen bijna alle MPI-typen en versies. Zie voor meer informatie, [Message Passing Interface instellen voor HPC](setup-mpi.md).

Nadat u ervoor uw VM-reeks gekozen hebt, instellen van Infiniband en MPI, bent u klaar om te beginnen het bouwen van uw HPC-werkbelasting.

## <a name="next-steps"></a>Volgende stappen

- Controleer de [HB-serie overzicht](hb-series-overview.md) en [overzicht van de HC-serie](hc-series-overview.md) voor meer informatie over de belangrijkste verschillen en specificaties.

- Zie voor een hoger niveau, architectonische weergave HPC-workloads, uitvoeren van [High Performance Computing (HPC) op Azure](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/).
