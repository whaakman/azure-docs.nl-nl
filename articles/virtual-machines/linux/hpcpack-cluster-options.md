---
title: Opties voor het cluster in Azure Linux HPC Pack | Microsoft Docs
description: Meer informatie over opties met Microsoft HPC Pack maken en beheren van een Linux-krachtige computing (HPC)-cluster in de Azure-cloud
services: virtual-machines-linux,cloud-services
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: ac60624e-aefa-40c3-8bc1-ef6d5c0ef1a2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 05/14/2018
ms.author: danlep
ms.openlocfilehash: 281867e30c78c7ed36ac739c8ae1a902463199cd
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="options-with-hpc-pack-to-create-and-manage-a-cluster-for-linux-hpc-workloads-in-azure"></a>Opties voor HPC Pack maken en beheren van een cluster voor Linux-HPC-workloads in Azure
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Dit artikel is gericht op Azure opties voor HPC Pack gebruiken voor het uitvoeren van Linux-werkbelastingen. Er zijn ook opties voor het werken met [Windows HPC-workloads met HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="hpc-pack-cluster-in-azure-vms-and-vm-scale-sets"></a>HPC Pack cluster in Azure VM's en VM-schaalsets
### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen
* (GitHub) [HPC Pack 2016 Update 1 cluster sjablonen](https://github.com/MsHpcPack/HPCPack2016)
* (GitHub) [HPC Pack 2012 R2-cluster sjablonen](https://github.com/MsHpcPack/HPCPack2012R2)
* (Quick Start) [Een HPC Pack 2012 R2-cluster maken met Linux-rekenknooppunten](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)

### <a name="azure-vm-images"></a>Installatiekopieën van Azure VM
Blader [HPC Pack afbeeldingen in Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?page=1&search=%22HPC%20%20Pack%22) als u wilt maken van uw eigen HPC Pack-cluster in Azure.

## <a name="hpc-pack-2012-r2-cluster-in-classic-deployment-model"></a>HPC Pack 2012 R2-cluster in het klassieke implementatiemodel
* [Een Linux-HPC-cluster maken met het implementatiescript HPC Pack IaaS](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Instellen van een cluster Linux RDMA MPI-toepassingen uitvoeren](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Zelfstudie: Aan de slag met Linux-rekenknooppunten in een cluster HPC Pack in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Zelfstudie: NAMD met Microsoft HPC Pack wordt uitgevoerd op Linux rekenknooppunten in Azure](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Zelfstudie: Voer OpenFOAM met Microsoft HPC Pack op een Linux RDMA-cluster in Azure](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Zelfstudie: Uitvoeren STER-CCM + met Microsoft HPC Pack op een Linux RDMA-cluster in Azure](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="job-submisssion"></a>Taak submisssion
* [Verzenden van taken naar een cluster HPC Pack in Azure](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)




