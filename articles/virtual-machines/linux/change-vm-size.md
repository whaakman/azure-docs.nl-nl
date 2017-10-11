---
title: Het formaat van een Linux-VM met de Azure CLI 2.0 | Microsoft Docs
description: Het opschalen of terugschroeven virtuele Linux-machine, door het wijzigen van de VM-grootte.
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: timlt
editor: 
tags: 
ms.assetid: e163f878-b919-45c5-9f5a-75a64f3b14a0
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2017
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 23fc9f7f34732079682857d4ee685fe811751698
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="resize-a-linux-virtual-machine-using-cli-20"></a>Een virtuele Linux-machine met CLI 2.0 vergroten of verkleinen

Nadat u een virtuele machine (VM) inricht, u kunt de virtuele machine omhoog of omlaag schalen door het wijzigen van de [VM-grootte][vm-sizes]. In sommige gevallen moet u eerst de VM ongedaan. U moet de VM ongedaan gemaakt als de gewenste grootte is niet beschikbaar op het cluster hardware die als host voor de virtuele machine fungeert. Dit artikel wordt uitgelegd hoe u dit moet een Linux-VM met de Azure CLI 2.0. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="resize-a-vm"></a>De grootte van een virtuele machine wijzigen
Als u een virtuele machine, moet u de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/#login).

1. De lijst met beschikbare grootten voor virtuele machine weergeven op de hardware-cluster waarop de virtuele machine wordt gehost met [az vm-vm-formaat-opties voor](/cli/azure/vm#list-vm-resize-options). Het volgende voorbeeld worden VM-grootten voor de virtuele machine met de naam `myVM` in de resourcegroep `myResourceGroup` regio:
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. Als de gewenste VM-grootte wordt weergegeven, de grootte van de virtuele machine met [az vm vergroten of verkleinen](/cli/azure/vm#resize). Hiermee het volgende voorbeeld wordt ingesteld voor de virtuele machine met de naam `myVM` naar de `Standard_DS3_v2` grootte:
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    De virtuele machine opnieuw is opgestart tijdens dit proces. Na het opnieuw opstarten, worden uw bestaande OS en de gegevensschijven toegewezen. Alles op de tijdelijke schijf gaat verloren.

3. Als de gewenste VM-grootte niet wordt weergegeven, moet u eerst de virtuele machine met de toewijzing [az vm ongedaan](/cli/azure/vm#deallocate). Dit proces kan de virtuele machine vervolgens worden aangepast aan de grootte die beschikbaar is dat de regio ondersteunt en vervolgens worden gestart. De volgende stappen ongedaan gemaakt, vergroten of verkleinen en start vervolgens de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > Toewijzing van de virtuele machine ook versies van dynamische IP-adressen toegewezen aan de virtuele machine. Het besturingssysteem en de gegevensschijven worden niet getroffen.

## <a name="next-steps"></a>Volgende stappen
Voor extra schaalbaarheid, meerdere exemplaren van de VM uitvoeren en uitbreiden. Zie voor meer informatie [automatisch schalen van Linux-machines in een virtuele-Machineschaalset][scale-set]. 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
