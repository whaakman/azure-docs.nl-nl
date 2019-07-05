---
title: 'Azure Windows VM-grootten: HPC | Microsoft Docs'
description: Geeft een lijst van de verschillende grootten beschikbaar voor Windows high performance computing-virtuele machines in Azure. Bevat informatie over het aantal vcpu's, gegevensschijven en NIC's, evenals opslag en doorvoer netwerkbandbreedte voor grootten die in deze reeks.
services: virtual-machines-windows
documentationcenter: ''
author: vermagit
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
ms.author: jonbeck;amverma
ms.openlocfilehash: 5fc5b5a287a421f93d3184ded3e429c5cff8fa3c
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67566299"
---
# <a name="high-performance-compute-vm-sizes"></a>High performance computing VM-grootten

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


* **Besturingssysteem** -Windows Server 2016 op alle bovenstaande HPC reeksen virtuele machines. Windows Server 2012 R2, Windows Server 2012 worden ook ondersteund op de virtuele machines voor de ingeschakelde van niet-SR-IOV (dus exclusief HB en HC).

* **MPI** -de SR-IOV ingeschakeld VM-grootten in Azure (HB, HC) bijna elke versie van de MPI moet worden gebruikt met Mellanox OFED toestaan.
Op niet-SR-IOV ingeschakeld virtuele machines, ondersteunde MPI-implementaties de Microsoft Network Direct (ND)-interface gebruiken om te communiceren tussen instanties. Daarom kan alleen Microsoft MPI (MS-MPI) 2012 R2 of hoger en Intel MPI 5.x versies worden ondersteund. Latere versies (2017, 2018) van de Intel MPI-runtime-bibliotheek kan of mogelijk niet compatibel is met de Azure-RDMA-stuurprogramma's.

* **InfiniBandDriverWindows VM-extensie** - op RDMA-compatibele virtuele machines, voeg de extensie InfiniBandDriverWindows om in te schakelen InfiniBand toe. Deze Windows-VM-extensie wordt geïnstalleerd voor Windows Network Direct stuurprogramma's (op niet-SR-IOV-VM's) of OFED Mellanox-stuurprogramma's (op virtuele machines SR-IOV) voor RDMA verbinding.
In sommige implementaties van A8 en A9-instanties, wordt de extensie HpcVmDrivers automatisch toegevoegd. Houd er rekening mee dat de HpcVmDrivers VM-extensie wordt afgeschaft; het wordt niet bijgewerkt. De VM-extensie toevoegen aan een virtuele machine, kunt u [Azure PowerShell](/powershell/azure/overview) cmdlets. 

  De volgende opdracht wordt de meest recente versie 1.0 InfiniBandDriverWindows-extensie geïnstalleerd op een bestaande RDMA-compatibele virtuele machine met de naam *myVM* geïmplementeerd in de resourcegroep met de naam *myResourceGroup* in de  *VS-West* regio:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "InfiniBandDriverWindows" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverWindows" -TypeHandlerVersion "1.0"
  ```
  VM-extensies kunnen ook worden opgenomen in Azure Resource Manager-sjablonen voor een gemakkelijke implementatie, met de volgende JSON-element:
  ```json
  "properties":{
  "publisher": "Microsoft.HpcCompute",
  "type": "InfiniBandDriverWindows",
  "typeHandlerVersion": "1.0",
  } 
  ```

  De volgende opdracht wordt de meest recente versie 1.0 InfiniBandDriverWindows-extensie geïnstalleerd op alle RDMA-compatibele virtuele machines in een bestaande VM-schaalset met de naam *myVMSS* geïmplementeerd in de resourcegroep met de naam *myResourceGroup*:

  ```powershell
  $VMSS = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS"
  Add-AzVmssExtension -VirtualMachineScaleSet $VMSS -Name "InfiniBandDriverWindows" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverWindows" -TypeHandlerVersion "1.0"
  Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "MyVMSS" -VirtualMachineScaleSet $VMSS
  Update-AzVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS" -InstanceId "*"
  ```

  Zie voor meer informatie, [extensies voor virtuele machines en functies](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). U kunt ook werken met de extensies voor virtuele machines die worden geïmplementeerd de [klassieke implementatiemodel](classic/manage-extensions.md).

* **De adresruimte van de RDMA-netwerk** -de RDMA-netwerk in Azure reserveert de ruimte adres 172.16.0.0/16. Voor het uitvoeren van MPI-toepassingen op exemplaren zijn geïmplementeerd in een Azure-netwerk, zorg ervoor dat de adresruimte van het virtuele netwerk niet het RDMA-netwerk overlapt.


### <a name="cluster-configuration-options"></a>Cluster-configuratie-opties

Azure biedt verschillende opties voor het maken van clusters van Windows HPC-VM's die kan communiceren met behulp van de RDMA-netwerk, met inbegrip van: 

* **Virtuele machines** -de RDMA-compatibele HPC-VM's implementeren in dezelfde beschikbaarheidsset (bij gebruik van het implementatiemodel Azure Resource Manager). Als u het klassieke implementatiemodel, implementeert u de virtuele machines in dezelfde cloudservice. 

* **Virtuele-machineschaalsets** - In een virtuele-machineschaalset instellen, zorgt u ervoor dat u de implementatie beperken tot één plaatsingsgroep. Bijvoorbeeld in een Resource Manager-sjabloon instellen de `singlePlacementGroup` eigenschap `true`. 

* **MPI tussen virtuele machines** - als MPI communicatie indien nodig tussen virtuele machines (VM's), zorg ervoor dat de virtuele machines worden in dezelfde beschikbaarheidsset ingesteld of de virtuele machine hetzelfde schaalset.

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
