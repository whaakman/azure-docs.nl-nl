---
title: Azure Linux VM-groottes - HPC | Microsoft Docs
description: Hier worden de verschillende grootten beschikbaar voor Linux high performance computing-virtuele machines in Azure. Bevat informatie over het aantal Vcpu, gegevensschijven en NIC's, evenals opslag doorvoer en bandbreedte voor de grootte van deze reeks.
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2017
ms.author: jonbeck
ms.openlocfilehash: cdfd09d90be9696dacc151e138920944c8bbd2c9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>Hoge prestaties compute grootten van virtuele machines

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA-compatibele exemplaren
Een netwerkinterface voor remote direct memory access (RDMA) connectiviteit zijn uitgerust met een subset van de exemplaren rekenintensieve (H16r, H16mr NC24r, A8 en A9). Deze interface is naast de standaard Azure netwerkinterface beschikbaar voor andere VM-groottes. 
  
Deze interface kunt de RDMA-compatibele exemplaren om te communiceren via een InfiniBand-netwerk op FDR tarieven voor H16r H16mr en NC24r virtuele machines en QDR tarieven voor A8 en A9 virtuele machines. Deze RDMA-mogelijkheden kunnen verbeteren de schaalbaarheid en prestaties van Message Passing Interface (MPI) toepassingen die worden uitgevoerd onder Intel MPI 5.x alleen. Latere versies (2017, 2018) van Intel MPI-runtime-bibliotheek zijn niet compatibel met de Azure-RDMA-stuurprogramma's.

Implementeer de RDMA-compatibele virtuele machines in dezelfde beschikbaarheidsset (bij gebruik van het Azure Resource Manager-implementatiemodel) of in dezelfde cloudservice (bij gebruik van het klassieke implementatiemodel). Aanvullende vereisten voor RDMA-compatibele Linux VM's toegang tot het netwerk van Azure RDMA volgen.

### <a name="distributions"></a>Distributies
 
Een rekenintensieve VM implementeren vanaf een van de afbeeldingen in de Azure Marketplace die ondersteuning biedt voor RDMA-verbindingen:
  
* **Ubuntu** -Ubuntu Server 16.04 LTS. RDMA-stuurprogramma's op de virtuele machine configureren en registreren met Intel Intel MPI downloaden:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **SUSE Linux Enterprise Server** -SLES 12 SP3 voor SLES 12 HPC SP3 voor HPC (Premium) SLES 12 SP1 voor SLES 12 HPC SP1 voor HPC (Premium). RDMA-stuurprogramma's zijn geïnstalleerd en Intel MPI-pakketten worden gedistribueerd op de virtuele machine. MPI installeren met de volgende opdracht:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
    
* **Op basis van centOS HPC** -7.3 HPC op basis van CentOS, 7.1 HPC op basis van CentOS, op basis van CentOS 6,8 HPC of 6.5 HPC op basis van CentOS (voor H-serie, versie 7.1 of hoger wordt aanbevolen). RDMA-stuurprogramma's en Intel MPI 5.1 worden geïnstalleerd op de virtuele machine.  
 
  > [!NOTE]
  > Op de installatiekopieën op basis van CentOS HPC kernel-updates zijn uitgeschakeld in de **yum** configuratiebestand. Dit is omdat de Linux RDMA-stuurprogramma's zijn gedistribueerd als een RPM-pakket en updates voor stuurprogramma's niet werken mogelijk als de kernel wordt bijgewerkt.
  > 
 
### <a name="cluster-configuration"></a>Clusterconfiguratie 
    
MPI-taken uitvoeren op geclusterde virtuele machines is aanvullende configuratie vereist. Op een cluster van virtuele machines moet u bijvoorbeeld een vertrouwensrelatie tussen de rekenknooppunten. Zie voor de gebruikelijke instellingen [instellen van een cluster Linux RDMA MPI-toepassingen uitvoeren](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="network-topology-considerations"></a>Aandachtspunten voor topologie netwerk
* Eth1 is op RDMA-functionaliteit Linux virtuele machines in Azure gereserveerd voor RDMA-netwerkverkeer. Wijzig niet alle instellingen Eth1 of gegevens in het configuratiebestand verwijzen naar dit netwerk. Eth0 is gereserveerd voor reguliere Azure-netwerkverkeer.

* IP via InfiniBand (IB) wordt niet ondersteund in Azure. Alleen RDMA over IB wordt ondersteund.

## <a name="using-hpc-pack"></a>Met behulp van HPC Pack
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), van Microsoft gratis HPC-cluster en taak beheeroplossing, is een optie voor u de exemplaren rekenintensieve gebruiken met Linux. De nieuwste versies van HPC Pack ondersteuning van verschillende Linux-distributies worden uitgevoerd op rekenknooppunten in Azure Virtual machines, beheerd door een Windows Server-hoofdknooppunt geïmplementeerd. Met RDMA-compatibele Linux-rekenknooppunten Intel MPI uitgevoerd, kunt HPC Pack plannen en uitvoeren van Linux MPI-toepassingen die toegang het netwerk RDMA tot. Zie [aan de slag met Linux-rekenknooppunten in een cluster HPC Pack Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Andere grootten
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)


## <a name="next-steps"></a>Volgende stappen

- Als u wilt implementeren en gebruiken van rekenintensieve grootten met RDMA op Linux aan de slag, Zie [instellen van een cluster Linux RDMA MPI-toepassingen uitvoeren](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Meer informatie over het [Azure compute-eenheden (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.




