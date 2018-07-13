---
title: Een momentopname van een VHD maken in Azure | Microsoft Docs
description: Leer hoe u een kopie van een VHD maken in Azure als een back-up of voor het oplossen van problemen.
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/11/2018
ms.author: cynthn
ms.openlocfilehash: 224f017decc3f48a23cb3fbf14f9a4e744bfaded
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007002"
---
# <a name="create-a-snapshot"></a>Een momentopname maken 

Een momentopname van een besturingssysteem of de gegevensschijf voor back-up of om problemen met virtuele machine te maken. Een momentopname is een volledige, alleen-lezen kopie van een VHD. 

## <a name="use-azure-cli"></a>Azure CLI gebruiken 

Het volgende voorbeeld is het vereist dat u [Cloud Shell](https://shell.azure.com/bash) of Azure CLI 2.0 is geïnstalleerd. Voer **az --version** uit om de versie op te vragen. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

De volgende stappen laten zien hoe u kunt een momentopname met de **maken van een momentopname van de az** opdracht met de **--bronschijf** parameter. Het volgende voorbeeld wordt ervan uitgegaan dat er een virtuele machine met de naam *myVM* in de *myResourceGroup* resourcegroep.

Ophalen van de schijf-ID met [az vm show](/cli/azure/vm#az-vm-show).

```azurecli-interactive
osDiskId=$(az vm show \
   -g myResourceGroup \
   -n myVM \
   --query "storageProfile.osDisk.managedDisk.id" \
   -o tsv)
```

Een momentopname te maken met de naam *osDisk-back-up* met behulp van [maken van een momentopname van de az](/cli/azure/snapshot#az-snapshot-create).

```azurecli-interactive
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDisk-backup
```

> [!NOTE]
> Als u wilt voor het opslaan van de momentopname in de zone-robuuste opslag, moet u deze maken in een regio die ondersteuning biedt voor [beschikbaarheidszones](../../availability-zones/az-overview.md) en bevatten de **--sku Standard_ZRS** parameter.

U kunt een overzicht van de momentopnamen die met behulp van [az snapshot list](/cli/azure/snapshot#az-snapshot-list).

```azurecli-interactive
az snapshot list \
   -g myResourceGroup \
   - table
```

## <a name="use-azure-portal"></a>Azure Portal gebruiken 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Beginnen in de linkerbovenhoek, klikt u op **een resource maken** en zoek naar de **momentopname**. Selecteer **momentopname** uit de lijst met zoekresultaten.
3. In de **momentopname** blade, klikt u op **maken**.
4. Voer een **naam** voor de momentopname.
5. Selecteer een bestaande resourcegroep of typ de naam voor een nieuwe resourcegroep. 
7. Voor **bronschijf**, selecteert u de beheerde schijf met de momentopname.
8. Selecteer de **accounttype** te gebruiken voor het opslaan van de momentopname. Gebruik **Standard HDD** , tenzij u het op te slaan op een hoog presterende SSD moet.
9. Klik op **Create**.


## <a name="next-steps"></a>Volgende stappen

 Een virtuele machine maken van een momentopname van een beheerde schijf maken op basis van de momentopname en vervolgens de nieuwe beheerde schijf als de besturingssysteemschijf te koppelen. Zie voor meer informatie de [maken van een virtuele machine van een momentopname](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) script.

