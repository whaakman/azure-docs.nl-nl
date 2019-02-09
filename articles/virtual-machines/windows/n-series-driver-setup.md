---
title: Azure N-serie GPU-stuurprogramma-instellingen voor Windows | Microsoft Docs
description: Over het instellen van NVIDIA GPU-stuurprogramma's voor N-serie VM's met Windows Server of Windows in Azure
services: virtual-machines-windows
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f86e2a5ac65274ecdb806c1cd894c89f1ca2efae
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55982179"
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-windows"></a>NVIDIA GPU-stuurprogramma's installeren op N-serie VM's waarop Windows wordt uitgevoerd 

Als u wilt profiteren van de GPU-mogelijkheden van Azure uit de N-serie VM's waarop Windows wordt uitgevoerd, moeten de NVIDIA GPU-stuurprogramma's worden geïnstalleerd. De [NVIDIA GPU-stuurprogramma extensie](../extensions/hpccompute-gpu-windows.md) juiste NVIDIA CUDA- of GRID stuurprogramma's worden geïnstalleerd op een VM uit de N-serie. Installeren of beheren van de extensie met de Azure portal of hulpprogramma's, zoals Azure PowerShell of Azure Resource Manager-sjablonen. Zie de [NVIDIA GPU-stuurprogramma extensie documentatie](../extensions/hpccompute-gpu-windows.md) voor ondersteunde besturingssystemen en implementatiestappen.

Als u ervoor kiest GPU-stuurprogramma's handmatig te installeren, wordt dit artikel bevat ondersteunde besturingssystemen, stuurprogramma's en installatie-en verificatiestappen. Stuurprogramma voor handmatige installatie-informatie is ook beschikbaar voor [virtuele Linux-machines](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Zie voor basic-specificaties, opslagcapaciteit en details van doelschijf [GPU Windows VM-grootten](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]

## <a name="driver-installation"></a>De installatie van stuurprogramma

1. Maak verbinding met extern bureaublad voor elke VM uit de N-serie.

2. Downloaden, uitpakken en installeer het ondersteunde stuurprogramma voor uw Windows-besturingssysteem.

Na de installatie van de GRID-stuurprogramma op een virtuele machine is een herstart vereist. Na de installatie van CUDA-stuurprogramma is niet opnieuw opstarten vereist.

## <a name="verify-driver-installation"></a>Controleer of de installatie van stuurprogramma

U kunt controleren of installeren van stuurprogramma's in Apparaatbeheer. Het volgende voorbeeld ziet de juiste configuratie van de Tesla R80-kaart op een Azure-NC-VM.

![Eigenschappen van de GPU-stuurprogramma](./media/n-series-driver-setup/GPU_driver_properties.png)

Uitvoeren om te vragen de GPU-Apparaatstatus, de [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) opdrachtregelprogramma moet zijn geïnstalleerd met het stuurprogramma.

1. Open een opdrachtprompt en wijzig in het **C:\Program Files\NVIDIA Corporation\NVSMI** directory.

2. Voer `nvidia-smi` uit. Als het stuurprogramma is geïnstalleerd, ziet u uitvoer die vergelijkbaar is met het volgende. De **GPU-Util** bevat **0%** , tenzij u een werkbelasting GPU momenteel worden uitgevoerd op de virtuele machine. Het is mogelijk dat de stuurprogrammaversie en -details van GPU afwijken van de namen weergegeven.

![NVIDIA-Apparaatstatus](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-connectivity"></a>RDMA-netwerkverbinding

RDMA-netwerkverbinding kan worden ingeschakeld op RDMA-compatibele N-serie VM's, zoals NC24r geïmplementeerd in dezelfde beschikbaarheidsset bevinden of in één plaatsingsgroep in een virtuele-machineschaalset. De extensie HpcVmDrivers moet worden toegevoegd aan Windows network apparaatstuurprogramma's die mogelijk van RDMA verbinding installeren. Gebruik van de VM-extensie toevoegen aan een virtuele machine van RDMA-functionaliteit uit de N-serie, [Azure PowerShell](/powershell/azure/overview) -cmdlets voor Azure Resource Manager.

Voor het installeren van de meest recente versie 1.1 HpcVMDrivers-extensie op een bestaande RDMA-compatibele virtuele machine met de naam myVM in de regio VS-West:
  ```PowerShell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Zie voor meer informatie, [extensies voor virtuele machines en functies voor Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

De RDMA-netwerk ondersteunt Message Passing Interface (MPI)-verkeer voor toepassingen die worden uitgevoerd met [Microsoft MPI](https://docs.microsoft.com/message-passing-interface/microsoft-mpi) of Intel MPI 5.x. 


## <a name="next-steps"></a>Volgende stappen

* Ontwikkelaars die het bouwen van toepassingen voor de Tesla GPU's van NVIDIA GPU-versnelde ook downloaden en installeer de meest recente [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads). Zie voor meer informatie de [CUDA-installatiehandleiding](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


