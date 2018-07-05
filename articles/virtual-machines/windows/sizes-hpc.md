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
ms.date: 03/15/2018
ms.author: jonbeck
ms.openlocfilehash: d8ab1531c4e1fa33fbdba12a4ecbeb8908dd6a94
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2018
ms.locfileid: "34654348"
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


## <a name="using-hpc-pack"></a>Met behulp van HPC Pack

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), van Microsoft gratis HPC-cluster en taak management-oplossing, is een optie waarmee u kunt een rekencluster in Azure maken op basis van Windows MPI-toepassingen en andere HPC-workloads uit te voeren. HPC Pack 2012 R2 en latere versies bevatten een runtime-omgeving voor MS-MPI die gebruikmaakt van het Azure-RDMA-netwerk wanneer geïmplementeerd op RDMA-compatibele VM's.



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




