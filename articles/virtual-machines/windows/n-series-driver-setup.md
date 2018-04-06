---
title: Azure N-reeks stuurprogramma-instellingen voor Windows | Microsoft Docs
description: Het instellen van NVIDIA GPU-stuurprogramma's voor N-serie VM's met Windows Server of Windows in Azure
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/04/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: efa8c2603d6ff4493656cda41306a5dad46bc5f3
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows"></a>GPU-stuurprogramma's instellen voor N-reeks virtuele machines waarop Windows wordt uitgevoerd 
Als u wilt profiteren van de GPU-mogelijkheden van N-reeks virtuele machines in Azure met een ondersteunde versie van Windows Server of Windows, moeten de NVIDIA grafische stuurprogramma's worden geïnstalleerd. Dit artikel bevat de installatiestappen stuurprogramma nadat u een VM N-serie implementeren. Stuurprogramma-installatie-informatie is ook beschikbaar voor [virtuele Linux-machines](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

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

2. Voer `nvidia-smi` uit. Als het stuurprogramma is geïnstalleerd ziet u uitvoer die vergelijkbaar zijn met de volgende. Houd er rekening mee dat **GPU-Util** toont **0%** tenzij u de werkbelasting van een GPU die momenteel worden uitgevoerd op de virtuele machine. Het is mogelijk dat uw versie van stuurprogramma en GPU details afwijken van de namen weergegeven.

![De apparaatstatus NVIDIA](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-connectivity"></a>RDMA-netwerkverbinding

RDMA-netwerkverbinding kan worden ingeschakeld op virtuele machines RDMA-compatibele N-serie, zoals NC24r geïmplementeerd in de dezelfde beschikbaarheidsset of VM-schaalset bevatten. De extensie HpcVmDrivers moet worden toegevoegd voor het installeren van Windows network apparaatstuurprogramma's die RDMA-verbindingen. U kunt de VM-extensie toevoegen naar een virtuele machine van RDMA-functionaliteit N-reeks met [Azure PowerShell](/powershell/azure/overview) cmdlets voor Azure Resource Manager.

Voor het installeren van de meest recente versie 1.1 HpcVMDrivers-extensie op een bestaande virtuele machine van RDMA-compatibel is met de naam myVM in de regio VS-West:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Zie voor meer informatie [uitbreidingen van de virtuele machine en functies voor Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Het netwerk RDMA ondersteunt Message Passing Interface (MPI)-verkeer voor toepassingen die worden uitgevoerd met [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) of Intel MPI 5.x. 


## <a name="next-steps"></a>Volgende stappen

* Ontwikkelaars die toepassingen voor de NVIDIA Tesla GPU's GPU-versnelde maken ook kunnen downloaden en installeren de [CUDA Toolkit 9.1](https://developer.nvidia.com/cuda-downloads). Zie voor meer informatie de [CUDA installatiehandleiding](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


