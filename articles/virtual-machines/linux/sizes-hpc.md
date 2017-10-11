---
title: Azure Linux VM-groottes - HPC | Microsoft Docs
description: Hier worden de verschillende grootten beschikbaar voor Linux high performance computing-virtuele machines in Azure.
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
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: b7a3844ce86b4efac8a4fc3c2540e7b6460873a2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="high-performance-compute-linux-vm-sizes"></a>Hoge prestaties compute Linux VM-grootten

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA-compatibele exemplaren
Een netwerkinterface voor remote direct memory access (RDMA) connectiviteit zijn uitgerust met een subset van de exemplaren rekenintensieve (H16mr H16r A8 en A9). Deze interface is naast de standaard Azure netwerkinterface beschikbaar voor andere VM-groottes. 
  
Deze interface kunt de RDMA-compatibele exemplaren om te communiceren via een InfiniBand-netwerk op FDR tarieven voor H16r en H16mr virtuele machines en QDR tarieven voor A8 en A9 virtuele machines. Deze RDMA-mogelijkheden kunnen verbeteren de schaalbaarheid en prestaties van Message Passing Interface (MPI)-toepassingen.

Hier volgen de vereisten voor RDMA-compatibele Linux VM's toegang tot het netwerk van Azure RDMA:
 
* **Distributies** -u moet virtuele machines implementeren vanuit de RDMA-compatibele SUSE Linux Enterprise Server (SLES) of Rogue Wave-Software (voorheen OpenLogic) op basis van CentOS HPC-installatiekopieën in Azure Marketplace. De volgende Marketplace-installatiekopieën bieden ondersteuning voor RDMA-verbindingen:
  
    * SLES 12 SP1 voor HPC of SLES 12 SP1 voor HPC (Premium)
    
    * 7.3 HPC op basis van centOS, 7.1 HPC op basis van CentOS, op basis van CentOS 6,8 HPC of 6.5 HPC op basis van CentOS  
 
        > [!NOTE]
        > Voor virtuele machines H-serie, wordt aangeraden een SLES 12 SP1 voor HPC-installatiekopie of op basis van CentOS 7.1 of hoger HPC-installatiekopie.
        >
        > Op de installatiekopieën op basis van CentOS HPC kernel-updates zijn uitgeschakeld in de **yum** configuratiebestand. Dit is omdat de Linux RDMA-stuurprogramma's zijn gedistribueerd als een RPM-pakket en updates voor stuurprogramma's niet werken mogelijk als de kernel wordt bijgewerkt.
        > 
        > 
* **MPI** -Intel MPI-bibliotheek 5.x
  
    Afhankelijk van de Marketplace-installatiekopie u kiest, afzonderlijke licentieverlening, installatie, of de configuratie van Intel MPI kan nodig zijn als volgt: 
  
  * **SLES 12 SP1 voor HPC-installatiekopie** -Intel MPI-pakketten worden gedistribueerd op de virtuele machine. Installeer de volgende opdracht uit te voeren:

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * **CentOS HPC-installatiekopieën** -Intel MPI 5.1 is al geïnstalleerd.  
    
    MPI-taken uitvoeren op geclusterde virtuele machines is aanvullende configuratie vereist. Op een cluster van virtuele machines moet u bijvoorbeeld een vertrouwensrelatie tussen de rekenknooppunten. Zie voor de gebruikelijke instellingen [instellen van een cluster Linux RDMA MPI-toepassingen uitvoeren](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

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




