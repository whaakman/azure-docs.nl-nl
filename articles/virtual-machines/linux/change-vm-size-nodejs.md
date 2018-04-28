---
title: Het formaat van een Linux-VM met de Azure CLI 1.0 | Microsoft Docs
description: Het opschalen of terugschroeven virtuele Linux-machine, door het wijzigen van de VM-grootte.
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2016
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4407a3662e6a50d650ab4ecc6dd1c2dad0c53bdb
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="resize-a-linux-vm-with-azure-cli-10"></a>Een virtuele Linux-machine met Azure CLI 1.0 vergroten of verkleinen

## <a name="overview"></a>Overzicht

Nadat u een virtuele machine (VM) inricht, u kunt de virtuele machine omhoog of omlaag schalen door het wijzigen van de [VM-grootte][vm-sizes]. In sommige gevallen moet u eerst de VM ongedaan. Dit kan gebeuren als de nieuwe grootte is niet beschikbaar op het cluster hardware die als host voor de virtuele machine fungeert.

In dit artikel laat zien hoe het formaat van een Linux-VM met behulp van de [Azure CLI][azure-cli].

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#resize-a-linux-vm) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel


## <a name="resize-a-linux-vm"></a>Een virtuele Linux-machine vergroten of verkleinen
Als u een virtuele machine, moet u de volgende stappen uitvoeren.

1. Voer de volgende opdracht in de CLI. Met deze opdracht worden de VM-grootten die beschikbaar zijn op de hardware-cluster waarop de virtuele machine wordt gehost.
   
    ```azurecli
    azure vm sizes -g myResourceGroup --vm-name myVM
    ```
2. Als de gewenste grootte wordt weergegeven, voer de volgende opdracht om de grootte van de virtuele machine aan.
   
    ```azurecli
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM  \
        --enable-boot-diagnostics
        --boot-diagnostics-storage-uri https://mystorageaccount.blob.core.windows.net/ 
    ```
   
    De virtuele machine wordt opnieuw opgestart tijdens dit proces. Na het opnieuw opstarten, wordt uw bestaande OS en de gegevensschijven worden toegewezen. Alles op de tijdelijke schijf zijn verloren.
   
    Gebruik de `--enable-boot-diagnostics` optie schakelt [diagnostics opstarten][boot-diagnostics], aan te melden eventuele fouten met betrekking tot opstarten.
3. Als de gewenste grootte niet wordt weergegeven, voer de volgende opdrachten toewijzing van de virtuele machine, het formaat en start de virtuele machine opnieuw op.
   
    ```azurecli
    azure vm deallocate -g myResourceGroup myVM
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM \
        --enable-boot-diagnostics --boot-diagnostics-storage-uri \
        https://mystorageaccount.blob.core.windows.net/ 
    azure vm start -g myResourceGroup myVM
    ```
   
   > [!WARNING]
   > Toewijzing van de virtuele machine ook versies van dynamische IP-adressen toegewezen aan de virtuele machine. Het besturingssysteem en de gegevensschijven worden niet getroffen.
   > 
   > 

## <a name="next-steps"></a>Volgende stappen
Voor extra schaalbaarheid, meerdere exemplaren van de VM uitvoeren en uitbreiden. Zie voor meer informatie [automatisch schalen van Linux-machines in een virtuele-Machineschaalset][scale-set]. 

<!-- links -->

[azure-cli]:../../cli-install-nodejs.md
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
