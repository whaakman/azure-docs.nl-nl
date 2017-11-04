---
title: Een momentopname van een VHD maken in Azure | Microsoft Docs
description: Informatie over het maken van een kopie van een VHD in Azure als een back-up of voor het oplossen van problemen.
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/09/2017
ms.author: cynthn
ms.openlocfilehash: da00c48f7da5a9be146f4fdb626c93db746c0f9b
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2017
---
# <a name="create-a-snapshot"></a>Een momentopname maken 

Een momentopname van een VHD voor back-up of om op te lossen VM uitgeeft OS- of -schijf. Een momentopname is een volledig, alleen-lezen kopie van een VHD. 

## <a name="use-azure-cli-20-to-take-a-snapshot"></a>Azure CLI 2.0 gebruiken om een momentopname

De Azure CLI-2.0 geïnstalleerd en geregistreerd in uw Azure-account is vereist voor het volgende voorbeeld. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

De volgende stappen laten zien over het maken van een momentopname met de `az snapshot create` opdracht met de `--source-disk` parameter. Het volgende voorbeeld wordt ervan uitgegaan dat er een virtuele machine aangeroepen `myVM` gemaakt met een beheerde OS-schijf in de `myResourceGroup` resourcegroep.

```azure-cli
# take the disk id with which to create a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

De uitvoer ziet er ongeveer als:

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Copy",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/osdisk_6NexYgkFQU",
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/osDisk-backup",
  "location": "westus",
  "name": "osDisk-backup",
  "osType": "Linux",
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-06T21:27:10.172980+00:00",
  "type": "Microsoft.Compute/snapshots"
}
```

## <a name="use-azure-portal-to-take-a-snapshot"></a>Azure portal gebruiken om een momentopname 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Beginnen in de linkerbovenhoek, klikt u op **nieuw** en zoek naar **momentopname**.
3. Klik op de blade momentopname **maken**.
4. Voer een **naam** voor de momentopname.
5. Selecteer een bestaande [Resourcegroep](../../azure-resource-manager/resource-group-overview.md#resource-groups) of typ de gewenste naam voor een nieuwe resourcegroep. 
6. Selecteer een Azure-datacenter locatie.  
7. Voor **bronschijf**, selecteert u de schijf worden beheerd met de momentopname.
8. Selecteer de **accounttype** moet worden gebruikt voor het opslaan van de momentopname. Het is raadzaam **Standard_LRS** tenzij u deze opgeslagen op een hoog presterende schijf nodig.
9. Klik op **Create**.

Als u van plan bent de momentopname gebruiken om te maken van een schijf beheerd en koppelt u dit een virtuele machine die moet worden hoge prestaties, gebruikt u de parameter `--sku Premium_LRS` met de `az snapshot create` opdracht. Hiermee maakt u de momentopname zodanig dat deze wordt opgeslagen als een schijf Premium beheerd. Premium-schijven beheerd sneller omdat ze SSD-schijven (SSD's zijn), maar duurder dan standaardschijven (HDD's).


## <a name="next-steps"></a>Volgende stappen

 Een virtuele machine maken vanuit een momentopname maken van een beheerde schijf van de momentopname en vervolgens de nieuwe beheerde schijf als de OS-schijf koppelen. Zie voor meer informatie de [een virtuele machine maken vanuit een momentopname](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) script.

