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
ms.openlocfilehash: 32b0f467f11cf8cb0a04657006cb5a86b11e27e9
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755205"
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>High performance computing-grootten van virtuele machines

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


### <a name="mpi"></a>MPI 

De SR-IOV ingeschakeld VM-grootten in Azure vrijwel elk smaak van MPI kunnen moet worden gebruikt.
Op niet-SR-IOV ingeschakeld virtuele machines, worden alleen Intel MPI 5.x versies ondersteund. Latere versies (2017, 2018) van de Intel MPI-runtime-bibliotheek kan of mogelijk niet compatibel is met de Azure Linux RDMA-stuurprogramma's.


### <a name="supported-os-images"></a>Ondersteunde OS-installatiekopieën
 
De Azure Marketplace bevat veel Linux-distributies die ondersteuning bieden voor RDMA verbinding:
  
* **Op basis van centOS HPC** : als u niet-SR-IOV ingeschakeld virtuele machines, op basis van CentOS-versie 6.5 HPC- of een latere versie, maximaal 7.5 geschikt zijn. Voor virtuele machines uit de H-serie, worden versie 7.1-7.5 aanbevolen. Stuurprogramma's voor RDMA en Intel MPI 5.1 zijn geïnstalleerd op de virtuele machine.
  Voor SR-IOV-VM's wordt geleverd met CentOS-HPC 7.6 geoptimaliseerde en vooraf geladen met de RDMA-stuurprogramma's en verschillende MPI-pakketten zijn geïnstalleerd.
  Voor andere RHEL/CentOS VM-installatiekopieën, toevoegen, de uitbreiding InfiniBandLinux om in te schakelen InfiniBand. Deze Linux VM-extensie wordt geïnstalleerd OFED Mellanox-stuurprogramma's (op virtuele machines SR-IOV) voor RDMA verbinding. De volgende PowerShell-cmdlet installeert de meest recente versie (versie 1.0) van de extensie InfiniBandDriverLinux op een bestaande RDMA-compatibele virtuele machine. De RDMA-compatibele virtuele machine met de naam *myVM* en wordt geïmplementeerd in de resourcegroep met de naam *myResourceGroup* in de *VS-West* regio als volgt te werk:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "InfiniBandDriverLinux" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverLinux" -TypeHandlerVersion "1.0"
  ```
  VM-extensies kunnen ook worden opgenomen in Azure Resource Manager-sjablonen voor een gemakkelijke implementatie met de volgende JSON-element:
  ```json
  "properties":{
  "publisher": "Microsoft.HpcCompute",
  "type": "InfiniBandDriverLinux",
  "typeHandlerVersion": "1.0",
  } 
  ```
 
  > [!NOTE]
  > Op de installatiekopieën op basis van CentOS HPC kernel-updates zijn uitgeschakeld in de **yum** configuratiebestand. Dit is omdat Linux RDMA-stuurprogramma's worden gedistribueerd als een RPM-pakket en updates voor stuurprogramma's niet werkt mogelijk als de kernel wordt bijgewerkt.
  >
  

* **SUSE Linux Enterprise Server** - SLES 12 SP3 for HPC, SLES 12 SP3 for HPC (Premium), SLES 12 SP1 for HPC, SLES 12 SP1 for HPC (Premium), SLES 12 SP4 and SLES 15. RDMA-stuurprogramma's zijn geïnstalleerd en Intel MPI-pakketten worden gedistribueerd op de virtuele machine. MPI installeren met de volgende opdracht:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
  
* **Ubuntu** -Ubuntu Server 16.04 LTS, 18.04 LTS. RDMA-stuurprogramma's op de virtuele machine configureren en registreren met Intel Intel MPI downloaden:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]  

  Zie voor meer informatie over het inschakelen van InfiniBand, instellen van de MPI, [InfiniBand inschakelen](https://docs.microsoft.com/azure/virtual-machines/workloads/hpc/enable-infiniband-with-sriov).


### <a name="cluster-configuration-options"></a>Cluster-configuratie-opties

Azure biedt verschillende opties voor het maken van clusters van Linux HPC-VM's die kan communiceren met behulp van de RDMA-netwerk, met inbegrip van: 

* **Virtuele machines** -de RDMA-compatibele HPC-VM's implementeren in dezelfde beschikbaarheidsset (bij gebruik van het implementatiemodel Azure Resource Manager). Als u het klassieke implementatiemodel, implementeert u de virtuele machines in dezelfde cloudservice. 

* **Virtuele-machineschaalsets** - In een virtuele-machineschaalset instellen, zorgt u ervoor dat u de implementatie beperken tot één plaatsingsgroep. Bijvoorbeeld in een Resource Manager-sjabloon instellen de `singlePlacementGroup` eigenschap `true`. 

* **Azure CycleCloud** -maken van een HPC-cluster in [Azure CycleCloud](/azure/cyclecloud/) MPI-opdrachten uitvoeren op Linux-knooppunten.

* **Azure Batch** -maakt een [Azure Batch](/azure/batch/) van toepassingen voor het uitvoeren van MPI-workloads op Linux-rekenknooppunten. Zie voor meer informatie, [gebruik RDMA-compatibele of met GPU exemplaren in de Batch-pools](../../batch/batch-pool-compute-intensive-sizes.md). Zie ook de [Batch scheepswerf](https://github.com/Azure/batch-shipyard) voor het uitvoeren van werkbelastingen op basis van een container op Batch-project.

* **Microsoft HPC Pack** - [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) ondersteunt diverse Linux-distributies worden uitgevoerd op rekenknooppunten in RDMA-compatibele Azure-VM's geïmplementeerd, beheerd door een Windows Server-hoofdknooppunt. Zie voor een voorbeeldimplementatie [maken HPC Pack Linux RDMA-Cluster in Azure](https://docs.microsoft.com/powershell/high-performance-computing/hpcpack-linux-openfoam).


### <a name="network-considerations"></a>Aandachtspunten voor netwerken
* Op niet-SR-IOV en is Linux-VM's in Azure, eth1 RDMA-functionaliteit gereserveerd voor RDMA-netwerkverkeer. Wijzig niet alle instellingen eth1 of gegevens in het configuratiebestand verwijzen naar dit netwerk.
* Voor SR-IOV ingeschakeld virtuele machines (HB en HC-serie), ib0 is gereserveerd voor RDMA-netwerkverkeer.
* De RDMA-netwerk in Azure reserveert de ruimte adres 172.16.0.0/16. Voor het uitvoeren van MPI-toepassingen op exemplaren zijn geïmplementeerd in een Azure-netwerk, zorg ervoor dat de adresruimte van het virtuele netwerk niet het RDMA-netwerk overlapt.
* Afhankelijk van uw eigen keuze aan voor Clusterbeheer, aanvullende configuratie nodig om MPI-opdrachten uitvoeren. Op een cluster met virtuele machines moet u mogelijk bijvoorbeeld vertrouwensrelatie tussen de knooppunten van het door de SSH-sleutels genereren of door het tot stand brengen van de configuratie SSH-aanmeldingen.


## <a name="other-sizes"></a>Andere grootten
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)
- [Vorige generaties](sizes-previous-gen.md)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het instellen, optimaliseren en te schalen [HPC-workloads](https://docs.microsoft.com/azure/virtual-machines/workloads/hpc) op Azure.
- Meer informatie over hoe u [Azure compute units (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.
