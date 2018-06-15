---
title: Een momentopname van een VHD maken in Azure | Microsoft Docs
description: Informatie over het maken van een kopie van een VHD in Azure als een back-up of voor het oplossen van problemen.
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
ms.date: 03/20/2018
ms.author: cynthn
ms.openlocfilehash: e5882b2ddc708544a7715da13c1f0d18384ce4e3
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
ms.locfileid: "30318889"
---
# <a name="create-a-snapshot"></a>Een momentopname maken 

Een momentopname van een besturingssysteem of gegevens schijf voor back-up of VM-problemen kunt oplossen. Een momentopname is een volledig, alleen-lezen kopie van een VHD. 

## <a name="use-azure-cli"></a>Azure CLI gebruiken 

De Azure CLI-2.0 geïnstalleerd en geregistreerd in uw Azure-account is vereist voor het volgende voorbeeld. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

De volgende stappen laten zien over het maken van een momentopname met de `az snapshot create` opdracht met de `--source-disk` parameter. Het volgende voorbeeld wordt ervan uitgegaan dat er een virtuele machine aangeroepen `myVM` in de `myResourceGroup` resourcegroep.

Ophalen van de schijf-ID.
```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Een momentopname met de naam *osDisk back-up-*.

```azurecli-interactive
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDisk-backup
```

> [!NOTE]
> Als u wilt voor het opslaan van uw momentopname in de zone robuuste opslag, moet u deze maken in een regio die ondersteuning biedt voor [beschikbaarheid zones](../../availability-zones/az-overview.md) en bevatten de `--sku Standard_ZRS` parameter.

## <a name="use-azure-portal"></a>Azure Portal gebruiken 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Beginnen in de linkerbovenhoek, klikt u op **maken van een resource** en zoek naar **momentopname**.
3. Klik op de blade momentopname **maken**.
4. Voer een **naam** voor de momentopname.
5. Selecteer een bestaande [Resourcegroep](../../azure-resource-manager/resource-group-overview.md#resource-groups) of typ de gewenste naam voor een nieuwe resourcegroep. 
6. Selecteer een Azure-datacenter locatie.  
7. Voor **bronschijf**, selecteert u de schijf worden beheerd met de momentopname.
8. Selecteer de **accounttype** moet worden gebruikt voor het opslaan van de momentopname. Het is raadzaam **Standard_LRS** tenzij u deze opgeslagen op een hoog presterende schijf nodig.
9. Klik op **Create**.


## <a name="next-steps"></a>Volgende stappen

 Een virtuele machine maken vanuit een momentopname maken van een beheerde schijf van de momentopname en vervolgens de nieuwe beheerde schijf als de OS-schijf koppelen. Zie voor meer informatie de [een virtuele machine maken vanuit een momentopname](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) script.

