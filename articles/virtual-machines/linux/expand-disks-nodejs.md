---
title: OS-schijf op Linux-VM met de Azure CLI 1.0 uitbreiden | Microsoft Docs
description: Meer informatie over het uitbreiden van de virtuele schijf van besturingssysteem (OS) op een Linux-VM met de Azure CLI 1.0 en de Resource Manager-implementatiemodel
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: f81054727bb1f0e8ffa752783e866a72d573589d
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="expand-os-disk-on-a-linux-vm-using-the-azure-cli-with-the-azure-cli-10"></a>OS-schijf op een Linux-VM met de Azure CLI met Azure CLI 1.0 uitbreiden
Grootte van de virtuele harde schijf voor het besturingssysteem (OS) is doorgaans 30 GB op een Linux virtuele machine (VM) in Azure. U kunt [gegevensschijven toevoegen](add-disk.md) te voorzien in extra opslagruimte, maar u kunnen ook desgewenst de OS-schijf wilt uitbreiden. Dit artikel wordt uitgelegd hoe het uitbreiden van de besturingssysteemschijf voor een Linux-VM met niet-beheerde schijven met de Azure CLI 1.0.

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#prerequisites) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](expand-disks.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

## <a name="prerequisites"></a>Vereisten
U moet de [nieuwste Azure CLI 1.0](../../cli-install-nodejs.md) geïnstalleerd en aangemeld bij een [Azure-account](https://azure.microsoft.com/pricing/free-trial/) met de modus Resource Manager als volgt:

```azurecli
azure config mode arm
```

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. De namen van de voorbeeld-parameter *myResourceGroup* en *myVM*.

## <a name="expand-os-disk"></a>Een besturingssysteemschijf uitbreiden

1. Bewerkingen op virtuele harde schijven kunnen niet worden uitgevoerd met de virtuele machine uitgevoerd. Het volgende voorbeeld wordt gestopt en de virtuele machine met de naam deallocates *myVM* in de resourcegroep met de naam *myResourceGroup*:

    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `azure vm stop` Geeft de rekenresources niet vrij. Gebruik om rekenresources release `azure vm deallocate`. De virtuele machine moet ongedaan als u de virtuele harde schijf wilt uitbreiden.

2. Bijwerken van de grootte van de niet-beheerde OS schijf met behulp van de `azure vm set` opdracht. Het volgende voorbeeld wordt de virtuele machine met de naam bijgewerkt *myVM* in de resourcegroep met de naam *myResourceGroup* worden *50* GB:

    ```azurecli
    azure vm set \
        --resource-group myResourceGroup \
        --name myVM \
        --new-os-disk-size 50
    ```

3. Start uw VM als volgt:

    ```azurecli
    azure vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH met uw virtuele machine met de juiste referenties. Gebruiken om te controleren of de grootte van de besturingssysteemschijf is gewijzigd, `df -h`. De volgende voorbeelduitvoer wordt weergegeven voor de primaire partitie (*/dev/sda1*) is nu 50 GB:

    ```bash
    Filesystem      Size  Used Avail Use% Mounted on
    udev            1.7G     0  1.7G   0% /dev
    tmpfs           344M  5.0M  340M   2% /run
    /dev/sda1        49G  1.3G   48G   3% /
    ```

## <a name="next-steps"></a>Volgende stappen
Als u extra opslagruimte, moet u ook [gegevensschijven toevoegen aan een Linux-VM](add-disk.md). Zie voor meer informatie over schijfversleuteling [schijven op een Linux-VM met de Azure CLI versleutelen](encrypt-disks.md).
