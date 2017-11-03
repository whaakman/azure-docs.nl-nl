---
title: Azure grootten van virtuele machine van Windows - HPC | Microsoft Docs
description: Hier worden de verschillende grootten beschikbaar voor Windows high performance computing-virtuele machines in Azure.
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: a0596d134e9c26877848f93d72f35bfd2c957570
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="high-performance-compute-vm-sizes"></a>Hoge prestaties compute-VM-grootten

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA-compatibele exemplaren
Een netwerkinterface voor remote direct memory access (RDMA) connectiviteit zijn uitgerust met een subset van de exemplaren rekenintensieve (H16mr H16r A8 en A9). Deze interface is naast de standaard Azure netwerkinterface beschikbaar voor andere VM-groottes. 
  
Deze interface kunt de RDMA-compatibele exemplaren om te communiceren via een InfiniBand-netwerk op FDR tarieven voor H16r en H16mr virtuele machines en QDR tarieven voor A8 en A9 virtuele machines. Deze RDMA-mogelijkheden kunnen verbeteren de schaalbaarheid en prestaties van Message Passing Interface (MPI)-toepassingen.

Hier volgen de vereisten voor RDMA-compatibele Windows virtuele machines toegang hebben tot de Azure-RDMA-netwerk: 

* **Besturingssysteem**
  
  Windows Server 2012 R2, WindowsServer 2012
  
  > [!NOTE]
  > Windows Server 2016 biedt momenteel geen ondersteuning voor RDMA-connectiviteit in Azure.
  >

* **Beschikbaarheidsset of cloudservice** – de RDMA-compatibele virtuele machines in dezelfde beschikbaarheidsset (bij gebruik van het Azure Resource Manager-implementatiemodel) of in dezelfde cloudservice implementeren (bij gebruik van het klassieke implementatiemodel). Als u Azure Batch gebruikt, moet de RDMA-compatibele virtuele machines in dezelfde groep.

* **MPI** -Microsoft MPI (MS-MPI) 2012 R2 of hoger, Intel MPI-bibliotheek 5.x

  De Microsoft Network Direct interface ondersteunde MPI-implementaties gebruiken voor communicatie tussen exemplaren. 

* **RDMA netwerkadresruimte** -de RDMA-netwerk in Azure behoudt zich het adresruimte 172.16.0.0/16. MPI-toepassingen op instanties zijn geïmplementeerd in een Azure-netwerk worden uitgevoerd, zorg dat de adresruimte van het virtuele netwerk niet de RDMA-netwerk overlapt.

* **HpcVmDrivers VM-extensie** -op RDMA-compatibele virtuele machines, moet u de extensie HpcVmDrivers voor het installeren van Windows-netwerk apparaatstuurprogramma's voor connectiviteit RDMA toevoegen. (In bepaalde implementaties A8 en A9 exemplaren van de extensie HpcVmDrivers wordt automatisch toegevoegd.) Als de VM-extensie toevoegen aan een virtuele machine, kunt u [Azure PowerShell](/powershell/azure/overview) cmdlets. 

  
  De volgende opdracht wordt de meest recente versie 1.1 HpcVMDrivers uitbreiding geïnstalleerd op een bestaande RDMA-compatibele virtuele machine met de naam *myVM* geïmplementeerd in de resourcegroep met de naam *myResourceGroup* in de  *VS-West* regio:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  Zie voor meer informatie [uitbreidingen van de virtuele machine en functies](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). U kunt ook werken met uitbreidingen voor virtuele machines die zijn geïmplementeerd in de [klassieke implementatiemodel](classic/manage-extensions.md).


## <a name="using-hpc-pack"></a>Met behulp van HPC Pack

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), van Microsoft gratis HPC-cluster en taak beheeroplossing, is een optie voor u een rekencluster maken in Azure worden uitgevoerd op basis van Windows MPI-toepassingen en andere HPC-workloads. HPC Pack 2012 R2 en hoger, bevatten een runtime-omgeving voor MS-MPI die gebruikmaakt van het netwerk van Azure RDMA wanneer geïmplementeerd op RDMA-compatibele virtuele machines.




## <a name="other-sizes"></a>Andere grootten
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Geoptimaliseerd geheugen](../virtual-machines-windows-sizes-memory.md)
- [Geoptimaliseerde opslag](../virtual-machines-windows-sizes-storage.md)
- [Geoptimaliseerde GPU](sizes-gpu.md)

## <a name="next-steps"></a>Volgende stappen

- Zie voor controlelijsten voor gebruik van de exemplaren rekenintensieve met HPC Pack op Windows Server [instellen van een cluster met Windows RDMA met HPC Pack MPI-toepassingen uitvoeren](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- Als u wilt gebruiken rekenintensieve exemplaren bij het uitvoeren van MPI-toepassingen met Azure Batch, Zie [taken met meerdere instanties gebruiken voor het uitvoeren van Message Passing Interface (MPI) applications in Azure Batch](../../batch/batch-mpi.md).

- Meer informatie over het [Azure compute-eenheden (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.




