---
title: Azure N-serie stuurprogramma-instellingen voor Windows | Microsoft Docs
description: Over het instellen van NVIDIA GPU-stuurprogramma's voor N-serie VM's met Windows Server of Windows in Azure
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
ms.date: 06/19/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 50d9ea88afc0e7d96d71b2ab26c8a8489ae41fee
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442260"
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows"></a>GPU-stuurprogramma's instellen voor N-serie VM's waarop Windows wordt uitgevoerd 
Als u wilt profiteren van de GPU-mogelijkheden van Azure uit de N-serie VM's waarop Windows wordt uitgevoerd, moeten de NVIDIA GPU-stuurprogramma's worden geïnstalleerd. De [NVIDIA GPU-stuurprogramma extensie](../extensions/hpccompute-gpu-windows.md) juiste NVIDIA CUDA- of GRID stuurprogramma's worden geïnstalleerd op een VM uit de N-serie. Installeren of beheren van de extensie met de Azure portal of hulpprogramma's, zoals Azure PowerShell of Azure Resource Manager-sjablonen. Zie de [NVIDIA GPU-stuurprogramma extensie documentatie](../extensions/hpccompute-gpu-windows.md) voor ondersteunde besturingssystemen en implementatiestappen.

Als u ervoor kiest GPU-stuurprogramma's handmatig te installeren, wordt dit artikel bevat ondersteunde besturingssystemen, stuurprogramma's en installatie-en verificatiestappen. Stuurprogramma voor handmatige installatie-informatie is ook beschikbaar voor [virtuele Linux-machines](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Zie voor basic-specificaties, opslagcapaciteit en details van doelschijf [GPU Windows VM-grootten](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]

## <a name="driver-installation"></a>De installatie van stuurprogramma

1. Maak verbinding met extern bureaublad voor elke VM uit de N-serie.

2. Downloaden, uitpakken en installeer het ondersteunde stuurprogramma voor uw Windows-besturingssysteem.

NV virtuele Azure-machines is opnieuw opstarten vereist na het installeren van stuurprogramma's. Op de Netwerkcontroller-VM's is niet opnieuw opstarten vereist.

## <a name="verify-driver-installation"></a>Controleer of de installatie van stuurprogramma

U kunt controleren of installeren van stuurprogramma's in Apparaatbeheer. Het volgende voorbeeld ziet de juiste configuratie van de Tesla R80-kaart op een Azure-NC-VM.

![Eigenschappen van de GPU-stuurprogramma](./media/n-series-driver-setup/GPU_driver_properties.png)

Uitvoeren om te vragen de GPU-Apparaatstatus, de [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) opdrachtregelprogramma moet zijn geïnstalleerd met het stuurprogramma.

1. Open een opdrachtprompt en wijzig in het **C:\Program Files\NVIDIA Corporation\NVSMI** directory.

2. Voer `nvidia-smi` uit. Als het stuurprogramma is geïnstalleerd ziet u uitvoer die vergelijkbaar is met het volgende. Houd er rekening mee dat **GPU-Util** bevat **0%** , tenzij u een werkbelasting GPU momenteel worden uitgevoerd op de virtuele machine. Het is mogelijk dat de stuurprogrammaversie en -details van GPU afwijken van de namen weergegeven.

![NVIDIA-Apparaatstatus](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-connectivity"></a>RDMA-netwerkverbinding

RDMA-netwerkverbinding kan worden ingeschakeld op RDMA-compatibele virtuele machines voor N-serie, zoals NC24r geïmplementeerd in dezelfde beschikbaarheidsset bevinden of in één plaatsingsgroep in een VM-schaalset. De extensie HpcVmDrivers moet worden toegevoegd aan Windows network apparaatstuurprogramma's die mogelijk van RDMA verbinding installeren. Gebruik van de VM-extensie toevoegen aan een virtuele machine van RDMA-functionaliteit uit de N-serie, [Azure PowerShell](/powershell/azure/overview) -cmdlets voor Azure Resource Manager.

Voor het installeren van de meest recente versie 1.1 HpcVMDrivers-extensie op een bestaande RDMA-compatibele virtuele machine met de naam myVM in de regio VS-West:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Zie voor meer informatie, [extensies voor virtuele machines en functies voor Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

De RDMA-netwerk ondersteunt Message Passing Interface (MPI)-verkeer voor toepassingen die worden uitgevoerd met [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) of Intel MPI 5.x. 


## <a name="next-steps"></a>Volgende stappen

* Ontwikkelaars die het bouwen van toepassingen voor de Tesla GPU's van NVIDIA GPU-versnelde ook downloaden en installeer de meest recente [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads). Zie voor meer informatie de [CUDA-installatiehandleiding](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


