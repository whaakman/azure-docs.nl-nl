---
title: 'Azure Windows VM-grootten: HPC | Microsoft Docs'
description: Geeft een lijst van de verschillende grootten beschikbaar voor Windows high performance computing-virtuele machines in Azure. Bevat informatie over het aantal vcpu's, gegevensschijven en NIC's, evenals opslag en doorvoer netwerkbandbreedte voor grootten die in deze reeks.
services: virtual-machines-windows
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2018
ms.author: jonbeck
ms.openlocfilehash: e00a4c5f5ee307a2d574702844e481894d28cb93
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340304"
---
# <a name="high-performance-compute-vm-sizes"></a>High performance computing VM-grootten

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


* **Besturingssysteem** -Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

* **MPI** -Microsoft MPI (MS-MPI) 2012 R2 of hoger, Intel MPI bibliotheek 5.x

  De Microsoft Network Direct interface ondersteunde MPI-implementaties gebruiken om te communiceren tussen instanties. 

* **De adresruimte van de RDMA-netwerk** -de RDMA-netwerk in Azure reserveert de ruimte adres 172.16.0.0/16. Voor het uitvoeren van MPI-toepassingen op exemplaren zijn geïmplementeerd in een Azure-netwerk, zorg ervoor dat de adresruimte van het virtuele netwerk niet het RDMA-netwerk overlapt.

* **HpcVmDrivers VM-extensie** - op RDMA-compatibele virtuele machines, voeg de extensie HpcVmDrivers voor het installeren van Windows network apparaatstuurprogramma's voor RDMA verbinding toe. (In sommige implementaties van A8 en A9-instanties, de HpcVmDrivers-extensie wordt automatisch toegevoegd.) De VM-extensie toevoegen aan een virtuele machine, kunt u [Azure PowerShell](/powershell/azure/overview) cmdlets. 

  
  De volgende opdracht wordt de meest recente versie 1.1 HpcVMDrivers-extensie geïnstalleerd op een bestaande RDMA-compatibele virtuele machine met de naam *myVM* geïmplementeerd in de resourcegroep met de naam *myResourceGroup* in de  *VS-West* regio:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  Zie voor meer informatie, [extensies voor virtuele machines en functies](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). U kunt ook werken met de extensies voor virtuele machines die worden geïmplementeerd de [klassieke implementatiemodel](classic/manage-extensions.md).

### <a name="cluster-configuration-options"></a>Cluster-configuratie-opties

Azure biedt verschillende opties voor het maken van clusters van Windows HPC-VM's die kan communiceren met behulp van de RDMA-netwerk, met inbegrip van: 

* **Virtuele machines** -de RDMA-compatibele HPC-VM's implementeren in dezelfde beschikbaarheidsset (bij gebruik van het implementatiemodel Azure Resource Manager). Als u het klassieke implementatiemodel, implementeert u de virtuele machines in dezelfde cloudservice. 

* **Virtuele-machineschaalsets** - In een VM-schaalset instellen, zorgt u ervoor dat u de implementatie beperken tot één plaatsingsgroep. Bijvoorbeeld in een Resource Manager-sjabloon instellen de `singlePlacementGroup` eigenschap `true`. 

* **Azure CycleCloud** -maken van een HPC-cluster in [Azure CycleCloud](/azure/cyclecloud/) aan MPI-opdrachten uitvoeren op Windows-knooppunten.

* **Azure Batch** -maakt een [Azure Batch](/azure/batch/) rekenknooppunten van de groep van toepassingen voor het uitvoeren van MPI-werkbelastingen op Windows Server. Zie voor meer informatie, [gebruik RDMA-compatibele of met GPU exemplaren in de Batch-pools](../../batch/batch-pool-compute-intensive-sizes.md). Zie ook de [Batch scheepswerf](https://github.com/Azure/batch-shipyard) voor het uitvoeren van werkbelastingen op basis van een container op Batch-project.

* **Microsoft HPC Pack** - [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) bevat een runtime-omgeving voor MS-MPI die gebruikmaakt van het Azure-RDMA-netwerk wanneer geïmplementeerd op RDMA-compatibele virtuele machines van Windows. Bijvoorbeeld implementaties, Zie [een Windows RDMA-cluster met HPC Pack instellen voor het uitvoeren van MPI-toepassingen](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Andere grootten
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Geoptimaliseerd geheugen](../virtual-machines-windows-sizes-memory.md)
- [Geoptimaliseerde opslag](../virtual-machines-windows-sizes-storage.md)
- [Geoptimaliseerde GPU](sizes-gpu.md)
- [Vorige generaties](sizes-previous-gen.md)

## <a name="next-steps"></a>Volgende stappen

- Zie voor controlelijsten voor het gebruik van de rekenintensieve instanties met HPC Pack op Windows Server [een Windows RDMA-cluster met HPC Pack instellen voor het uitvoeren van MPI-toepassingen](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- Zie voor het gebruik van rekenintensieve instanties bij het uitvoeren van MPI-toepassingen met Azure Batch, [taken met meerdere instanties gebruiken voor het uitvoeren van Message Passing Interface (MPI) applications in Azure Batch](../../batch/batch-mpi.md).

- Meer informatie over hoe u [Azure compute units (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.




