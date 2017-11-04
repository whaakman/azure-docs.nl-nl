---
title: Opties voor het cluster in Azure Linux HPC Pack | Microsoft Docs
description: Meer informatie over opties met Microsoft HPC Pack maken en beheren van een Linux-krachtige computing (HPC)-cluster in de Azure-cloud
services: virtual-machines-linux,cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: ac60624e-aefa-40c3-8bc1-ef6d5c0ef1a2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/26/2017
ms.author: danlep
ms.openlocfilehash: ae36e64c0261b1fe8d02d6dcb80df7cdee333db9
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="options-with-hpc-pack-to-create-and-manage-a-cluster-for-linux-hpc-workloads-in-azure"></a>Opties voor HPC Pack maken en beheren van een cluster voor Linux-HPC-workloads in Azure
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Dit artikel is gericht op de opties voor HPC Pack gebruiken voor het uitvoeren van Linux-werkbelastingen. Er zijn ook opties voor het werken met [Windows HPC-workloads met HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="hpc-pack-cluster-in-azure-vms-and-vm-scale-sets"></a>HPC Pack cluster in Azure VM's en VM-schaalsets
### <a name="azure-templates"></a>Azure-sjablonen
* (GitHub) [HPC Pack 2016 cluster sjablonen](https://github.com/MsHpcPack/HPCPack2016)
* (GitHub) [HPC Pack 2012 R2-cluster sjablonen](https://github.com/MsHpcPack/HPCPack2012R2)
* (Marketplace) [HPC Pack cluster voor Linux-werkbelastingen](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)
* (Quick Start) [Een HPC-cluster maken met Linux-rekenknooppunten](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)

### <a name="powershell-deployment-script-for-hpc-pack-2012-r2"></a>PowerShell-script-implementatie voor HPC Pack 2012 R2
* [Een Linux-HPC-cluster maken met het implementatiescript HPC Pack IaaS](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a>Zelfstudies
* [Zelfstudie: Aan de slag met Linux-rekenknooppunten in een cluster HPC Pack in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Zelfstudie: NAMD met Microsoft HPC Pack wordt uitgevoerd op Linux rekenknooppunten in Azure](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Zelfstudie: Voer OpenFOAM met Microsoft HPC Pack op een Linux RDMA-cluster in Azure](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Zelfstudie: Uitvoeren STER-CCM + met Microsoft HPC Pack op een Linux RDMA-cluster in Azure](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a>Clusterbeheer
* [Verzenden van taken naar een cluster HPC Pack in Azure](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Beheer van de taak in HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="rdma-clusters-for-mpi-workloads"></a>RDMA-clusters voor MPI-belastingen
* [Zelfstudie: Voer OpenFOAM met Microsoft HPC Pack op een Linux RDMA-cluster in Azure](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Instellen van een cluster Linux RDMA MPI-toepassingen uitvoeren](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

