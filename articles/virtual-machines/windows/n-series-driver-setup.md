---
title: Azure N-reeks stuurprogramma-instellingen voor Windows | Microsoft Docs
description: Het instellen van NVIDIA GPU-stuurprogramma's voor N-reeks virtuele machines waarop Windows wordt uitgevoerd in Azure
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/07/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b480d10df777a2757c073ff77e1845d33d63163a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a>GPU-stuurprogramma's instellen voor N-serie VM's met Windows Server
Als u wilt profiteren van de GPU-mogelijkheden van N-reeks virtuele machines in Azure waarop Windows Server 2016 of Windows Server 2012 R2 wordt uitgevoerd, ondersteunde NVIDIA graphics-stuurprogramma's te installeren. Dit artikel bevat de installatiestappen stuurprogramma nadat u een VM N-serie implementeren. Stuurprogramma-installatie-informatie is ook beschikbaar voor [virtuele Linux-machines](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Zie voor basic specificaties, opslagcapaciteit en details van de schijf, [GPU Windows VM-grootten](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 


[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]



## <a name="driver-installation"></a>De installatie van stuurprogramma

1. Verbinding maken met extern bureaublad voor elke VM N-serie.

2. Downloaden, ophalen en installeer het ondersteunde stuurprogramma voor uw Windows-besturingssysteem.

Op Azure NV virtuele machines, moet worden opgestart na de installatie van het stuurprogramma. Op virtuele machines NC is niet opnieuw opstarten vereist.

## <a name="verify-driver-installation"></a>Controleer of de installatie van stuurprogramma

U kunt controleren of stuurprogramma-installatiebestand in Apparaatbeheer. Het volgende voorbeeld ziet geslaagde configuratie van de kaart Tesla R80 op een NC Azure VM.

![Eigenschappen van de GPU-stuurprogramma](./media/n-series-driver-setup/GPU_driver_properties.png)

Om te vragen van de apparaatstatus GPU, voer de [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) opdrachtregelprogramma met het stuurprogramma geïnstalleerd.

1. Open een opdrachtprompt en Ga naar de **C:\Program Files\NVIDIA Corporation\NVSMI** directory.

2. Voer **nvidia smi**. Als het stuurprogramma is geïnstalleerd ziet u uitvoer die vergelijkbaar is met hieronder. Houd er rekening mee dat **GPU-Util** toont **0%** tenzij u de werkbelasting van een GPU die momenteel worden uitgevoerd op de virtuele machine.

![De apparaatstatus NVIDIA](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-for-nc24r-vms"></a>RDMA-netwerk voor NC24r VM

RDMA-netwerkverbinding kan worden ingeschakeld op NC24r VM's geïmplementeerd in dezelfde beschikbaarheidsset. De extensie HpcVmDrivers moet worden toegevoegd voor het installeren van Windows network apparaatstuurprogramma's die RDMA-verbindingen. U kunt de VM-extensie toevoegen aan een NC24r VM met [Azure PowerShell](/powershell/azure/overview) cmdlets voor Azure Resource Manager.

> [!NOTE]
> Op dit moment ondersteunt alleen Windows Server 2012 R2 de RDMA-netwerk op NC24r VM's.
> 

Voor het installeren van de meest recente versie 1.1 HpcVMDrivers-extensie op een bestaande virtuele machine van RDMA-compatibel is met de naam myVM in de regio VS-West:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Zie voor meer informatie [uitbreidingen van de virtuele machine en functies voor Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Het netwerk RDMA ondersteunt Message Passing Interface (MPI)-verkeer voor toepassingen die worden uitgevoerd met [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) of Intel MPI 5.x. 


## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de NVIDIA GPU's op de N-reeks virtuele machines:
    * [NVIDIA Tesla R80](http://www.nvidia.com/object/tesla-k80.html) (voor NC Azure VM's)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (voor NV Azure VM's)

* Ontwikkelaars die toepassingen voor de NVIDIA Tesla GPU's GPU-versnelde maken ook kunnen downloaden en installeren van de CUDA Toolkit 8 voor [Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) of [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe). Zie voor meer informatie de [CUDA installatiehandleiding](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


