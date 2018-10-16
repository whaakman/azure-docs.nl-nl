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
ms.date: 10/12/2018
ms.author: jonbeck
ms.openlocfilehash: 70dca655d5300fcd34b4198093e136f6a971963b
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344486"
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
 
### <a name="cluster-configuration-options"></a>Cluster-configuratie-opties

Azure biedt verschillende opties voor het maken van clusters van Linux HPC-VM's die kan communiceren met behulp van de RDMA-netwerk, met inbegrip van: 

* **Virtuele machines** -de RDMA-compatibele HPC-VM's implementeren in dezelfde beschikbaarheidsset (bij gebruik van het implementatiemodel Azure Resource Manager). Als u het klassieke implementatiemodel, implementeert u de virtuele machines in dezelfde cloudservice. 

* **Virtuele-machineschaalsets** - In een VM-schaalset instellen, zorgt u ervoor dat u de implementatie beperken tot één plaatsingsgroep. Bijvoorbeeld in een Resource Manager-sjabloon instellen de `singlePlacementGroup` eigenschap `true`. 

* **Azure CycleCloud** -maken van een HPC-cluster in [Azure CycleCloud](/azure/cyclecloud/) MPI-opdrachten uitvoeren op Linux-knooppunten.

* **Azure Batch** -maakt een [Azure Batch](/azure/batch/) van toepassingen voor het uitvoeren van MPI-workloads op Linux-rekenknooppunten. Zie voor meer informatie, [gebruik RDMA-compatibele of met GPU exemplaren in de Batch-pools](../../batch/batch-pool-compute-intensive-sizes.md). Zie ook de [Batch scheepswerf](https://github.com/Azure/batch-shipyard) voor het uitvoeren van werkbelastingen op basis van een container op Batch-project.

* **Microsoft HPC Pack** - [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) ondersteunt diverse Linux-distributies worden uitgevoerd op rekenknooppunten in RDMA-compatibele Azure-VM's geïmplementeerd, beheerd door een Windows Server-hoofdknooppunt. Zie voor een voorbeeldimplementatie [maken HPC Pack Linux RDMA-Cluster in Azure](https://docs.microsoft.com/powershell/high-performance-computing/hpcpack-linux-openfoam).

Afhankelijk van uw eigen keuze aan voor Clusterbeheer, aanvullende configuratie nodig om MPI-opdrachten uitvoeren. Op een cluster met virtuele machines moet u mogelijk bijvoorbeeld vertrouwensrelatie tussen de knooppunten van het door de SSH-sleutels genereren of door het zonder wachtwoord uitproberen SSH vertrouwensrelatie tot stand brengen.

### <a name="network-topology-considerations"></a>Aandachtspunten voor topologie van netwerk
* Op de RDMA-functionaliteit virtuele Linux-machines in Azure worden Eth1 is gereserveerd voor RDMA-netwerkverkeer. Wijzig niet alle instellingen Eth1 of gegevens in het configuratiebestand verwijzen naar dit netwerk. Eth0 is gereserveerd voor reguliere Azure netwerkverkeer.

* De RDMA-netwerk in Azure reserveert de ruimte adres 172.16.0.0/16. 




## <a name="other-sizes"></a>Andere grootten
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)
- [Vorige generaties](sizes-previous-gen.md)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe u [Azure compute units (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.




