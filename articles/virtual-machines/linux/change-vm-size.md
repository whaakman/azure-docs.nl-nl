---
title: Het formaat van een Linux-VM met de Azure CLI | Microsoft Docs
description: Informatie over het vergroten of verkleinen van een virtuele Linux-machine, door het veranderen van de VM-grootte.
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: e163f878-b919-45c5-9f5a-75a64f3b14a0
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2017
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8bd8136c61a277e730c07c8789afd1bf3214bd7c
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55754146"
---
# <a name="resize-a-linux-virtual-machine-using-azure-cli"></a>Formaat van een virtuele Linux-machine met behulp van Azure CLI 

Nadat u een virtuele machine (VM) inricht, u kunt de virtuele machine omhoog of omlaag schalen door het veranderen van de [VM-grootte][vm-sizes]. In sommige gevallen moet u eerst de virtuele machine toewijzing. U moet de toewijzing van de virtuele machine als de gewenste grootte is niet beschikbaar op de hardware-cluster die als host voor de virtuele machine fungeert. Dit artikel wordt uitgelegd hoe u dit moet een Linux-VM met de Azure CLI. 

## <a name="resize-a-vm"></a>De grootte van een virtuele machine wijzigen
Als u wilt het formaat van een virtuele machine, moet u de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index).

1. De lijst met beschikbare VM-grootten weergeven op de hardware-cluster waar de virtuele machine wordt gehost met [az vm list-vm-resize-options](/cli/azure/vm). Het volgende voorbeeld worden de VM-grootten voor de virtuele machine met de naam `myVM` in de resourcegroep `myResourceGroup` regio:
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. Als de gewenste VM-grootte wordt weergegeven, de grootte van de virtuele machine met [az vm-grootte](/cli/azure/vm). Het volgende voorbeeld wordt de grootte de virtuele machine met de naam `myVM` naar de `Standard_DS3_v2` grootte:
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    De virtuele machine opnieuw wordt opgestart tijdens dit proces. Na het opnieuw opstarten, worden uw bestaande besturingssysteem en gegevensschijven toegewezen. Alles op de tijdelijke schijf wordt verwijderd.

3. Als de gewenste VM-grootte niet wordt vermeld, moet u eerst de virtuele machine met de toewijzing [az vm deallocate](/cli/azure/vm). Dit proces kunt de virtuele machine vervolgens worden aangepast voor elke grootte beschikbaar aan dat de regio ondersteunt en klikt u vervolgens aan de slag. De volgende stappen uit toewijzing ongedaan maken, vergroten of verkleinen en start vervolgens de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > De toewijzing van de virtuele machine ongedaan maken versies een dynamische IP-adressen toegewezen aan de virtuele machine ook. Het besturingssysteem en gegevensschijven worden niet beïnvloed.

## <a name="next-steps"></a>Volgende stappen
Voor extra schaalbaarheid, uitvoeren van meerdere VM-exemplaren en de schaal vergroten. Zie voor meer informatie, [Linux-machines automatisch schalen in een virtuele-Machineschaalset][scale-set]. 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
