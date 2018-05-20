---
title: Opties voor het cluster van Windows HPC Pack in Azure | Microsoft Docs
description: Meer informatie over opties met Microsoft HPC Pack maken en beheren van een Windows HPC-cluster (HPC) in de Azure-cloud
services: virtual-machines-windows,cloud-services,batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 02c5566d-2129-483c-9ecf-0d61030442d7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/14/2018
ms.author: danlep
ms.openlocfilehash: 1232228d5e2fcd05f41a096a933072dffcca2119
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="options-with-hpc-pack-to-create-and-manage-a-cluster-for-windows-hpc-workloads-in-azure"></a>Opties voor HPC Pack maken en beheren van een cluster met Windows HPC-workloads in Azure
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Dit artikel is gericht op Azure opties voor het aanmaken van HPC Pack clusters om uit te voeren Windows werkbelasting. Er zijn ook opties voor het maken van clusters HPC Pack om uit te voeren [Linux HPC-workloads](../linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="hpc-pack-cluster-in-azure-vms-and-vm-scale-sets"></a>HPC Pack cluster in Azure VM's en VM-schaalsets
### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen
* (GitHub) [HPC Pack 2016 Update 1 cluster sjablonen](https://github.com/MsHpcPack/HPCPack2016)
* (GitHub) [HPC Pack 2012 R2-cluster sjablonen](https://github.com/MsHpcPack/HPCPack2012R2)
* (Quick Start) [Een HPC Pack 2012 R2-cluster maken](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)
* (Quick Start) [Een HPC Pack 2012 R2-cluster maken met de installatiekopie van de aangepaste rekenservice-knooppunt](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Installatiekopieën van Azure VM
Blader [HPC Pack afbeeldingen in Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?page=1&search=%22HPC%20%20Pack%22) als u wilt maken van uw eigen HPC Pack-cluster in Azure.


### <a name="tutorials"></a>Zelfstudies
* [Zelfstudie: Een cluster HPC Pack 2016 in Azure implementeren](hpcpack-2016-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Een HPC Pack 2016-cluster in Azure met Azure Active Directory beheren](hpcpack-cluster-active-directory.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)


## <a name="hpc-pack-2012-r2-cluster-deployment-in-the-classic-deployment-model"></a>Implementatie van het cluster in het klassieke implementatiemodel HPC Pack 2012 R2
* [Een HPC-cluster maken met het implementatiescript HPC Pack IaaS](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Zelfstudie: Aan de slag met een HPC Pack-cluster in Azure uitvoeren van Excel- en SOA-werkbelastingen](excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Rekenknooppunten in een cluster HPC Pack in Azure beheren](classic/hpcpack-cluster-node-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Vergroten of verkleinen van Azure-rekenresources in een cluster HPC Pack](classic/hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Een Windows RDMA-cluster met HPC Pack instellen voor het uitvoeren van MPI-toepassingen](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)


## <a name="burst-to-azure-from-hpc-pack-2012-r2"></a>Burst naar Azure van HPC Pack 2012 R2
* [Burst naar Azure Worker-exemplaren met Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)
* [Azure batch met HPC Pack burst](https://technet.microsoft.com/library/mt612877.aspx)
* [Zelfstudie: Een cluster hybride HPC Pack in Azure instellen](../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

## <a name="job-submission"></a>Verzending van taak

* [Verzenden van taken naar een cluster HPC Pack in Azure](hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)






