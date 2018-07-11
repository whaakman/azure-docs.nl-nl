---
title: 'Azure Linux VM-grootten: HPC | Microsoft Docs'
description: Geeft een lijst van de verschillende grootten beschikbaar voor Linux high performance computing-virtuele machines in Azure. Bevat informatie over het aantal vcpu's, gegevensschijven en NIC's, evenals opslag en doorvoer netwerkbandbreedte voor grootten die in deze reeks.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: jonbeck
ms.openlocfilehash: 748cb4612b2b5aed26ba8197cfad0782f2645e1e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37902126"
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>High performance computing-grootten van virtuele machines

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


### <a name="mpi"></a>MPI 

Alleen Intel MPI 5.x versies worden ondersteund. Latere versies (2017, 2018) van de Intel MPI-runtime-bibliotheek zijn niet compatibel met de Azure Linux RDMA-stuurprogramma's.


### <a name="distributions"></a>Distributies
 
Een rekenintensieve VM implementeren vanaf een van de installatiekopieën in de Azure Marketplace die ondersteuning biedt voor RDMA verbinding:
  
* **Ubuntu** -Ubuntu Server 16.04 LTS. RDMA-stuurprogramma's op de virtuele machine configureren en registreren met Intel Intel MPI downloaden:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **SUSE Linux Enterprise Server** -SLES 12 SP3 voor HPC, SLES 12 SP3 voor HPC (Premium), SLES 12 SP1 voor HPC, SLES 12 SP1 voor HPC (Premium). RDMA-stuurprogramma's zijn geïnstalleerd en Intel MPI-pakketten worden gedistribueerd op de virtuele machine. MPI installeren met de volgende opdracht:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
    
* **Op basis van centOS HPC** -op basis van een CentOS 6.5 HPC- of een latere versie (voor de H-serie, versie 7.1 of hoger wordt aanbevolen). Stuurprogramma's voor RDMA en Intel MPI 5.1 zijn geïnstalleerd op de virtuele machine.  
 
  > [!NOTE]
  > Op de installatiekopieën op basis van CentOS HPC kernel-updates zijn uitgeschakeld in de **yum** configuratiebestand. Dit is omdat de Linux RDMA-stuurprogramma's worden gedistribueerd als een RPM-pakket en updates voor stuurprogramma's niet werkt mogelijk als de kernel wordt bijgewerkt.
  > 
 
### <a name="cluster-configuration"></a>Clusterconfiguratie 
    
MPI-opdrachten uitvoeren op geclusterde virtuele machines is aanvullende configuratie vereist. Op een cluster met virtuele machines moet u bijvoorbeeld vertrouwensrelatie tussen de rekenknooppunten. Zie voor de standaardinstellingen, [een Linux RDMA-cluster instellen voor het uitvoeren van MPI-toepassingen](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="network-topology-considerations"></a>Aandachtspunten voor topologie van netwerk
* Op de RDMA-functionaliteit virtuele Linux-machines in Azure worden Eth1 is gereserveerd voor RDMA-netwerkverkeer. Wijzig niet alle instellingen Eth1 of gegevens in het configuratiebestand verwijzen naar dit netwerk. Eth0 is gereserveerd voor reguliere Azure netwerkverkeer.

* De RDMA-netwerk in Azure reserveert de ruimte adres 172.16.0.0/16. 


## <a name="using-hpc-pack"></a>Met behulp van HPC Pack
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), van Microsoft gratis HPC-cluster en taak management-oplossing, is een optie voor u de rekenintensieve instanties gebruiken met Linux. De meest recente versies van HPC Pack-ondersteuning van verschillende Linux-distributies worden uitgevoerd op rekenknooppunten geïmplementeerd in Azure VM's, beheerd door een Windows Server-hoofdknooppunt. HPC Pack kunt met RDMA-compatibele Linux-rekenknooppunten met Intel MPI, plannen en uitvoeren van Linux MPI-toepassingen die toegang hebben tot de RDMA-netwerk. Zie [aan de slag met Linux-rekenknooppunten in een HPC Pack-cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Andere grootten
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)
- [Vorige generaties](sizes-previous-gen.md)

## <a name="next-steps"></a>Volgende stappen

- Om te beginnen implementeren en gebruiken van rekenintensieve grootten met RDMA op Linux, Zie [een Linux RDMA-cluster instellen voor het uitvoeren van MPI-toepassingen](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Meer informatie over hoe u [Azure compute units (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.




