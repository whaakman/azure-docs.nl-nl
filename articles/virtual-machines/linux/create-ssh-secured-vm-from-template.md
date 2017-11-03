---
title: Linux-VM in Azure maken van een sjabloon | Microsoft Docs
description: Het gebruik van de Azure CLI 2.0 voor een Linux-VM maken van een Resource Manager-sjabloon
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 09/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 938304efe5e4a13736a50348bd0531c475149aec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Het maken van een virtuele Linux-machine met Azure Resource Manager-sjablonen
In dit artikel laat zien hoe snel implementeren virtuele Linux-machine (VM) met Azure Resource Manager-sjablonen en de Azure CLI 2.0. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).


## <a name="templates-overview"></a>Overzicht van sjablonen
Azure Resource Manager-sjablonen zijn JSON-bestanden die de infrastructuur en configuratie van uw Azure-oplossing te definiëren. Door het gebruik van een sjabloon kunt u gedurende de levenscyclus de oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd. Zie voor meer informatie over de indeling van de sjabloon en de manier waarop u samenstelt, [maken van uw eerste Azure Resource Manager-sjabloon](../../azure-resource-manager/resource-manager-create-first-template.md). Als u de JSON-syntaxis voor resourcetypen wilt bekijken, raadpleegt u [Define resources in Azure Resource Manager templates](/azure/templates/) (Resources definiëren in Azure Resource Manager-sjablonen).


## <a name="create-resource-group"></a>Een resourcegroep maken
Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Een resourcegroep moet worden gemaakt voordat een virtuele machine. Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupVM* in de *eastus* regio:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Virtuele machine maken
Het volgende voorbeeld wordt een virtuele machine uit [deze Azure Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) met [az implementatie maken](/cli/azure/group/deployment#create). Geef de waarde van uw eigen openbare SSH-sleutel, zoals de inhoud van *~/.ssh/id_rsa.pub*. Als u maken van een SSH-sleutelpaar wilt, Zie [maken en gebruiken van een SSH-sleutelpaar voor virtuele Linux-machines in Azure](mac-create-ssh-keys.md).

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
  --parameters '{"sshKeyData": {"value": "ssh-rsa AAAAB3N{snip}B9eIgoZ"}}'
```

In dit voorbeeld moet u een sjabloon die is opgeslagen in GitHub opgegeven. U kunt ook downloaden of een sjabloon maken en het lokale pad opgeven met dezelfde `--template-file` parameter.

Om SSH met uw virtuele machine te verkrijgen van het openbare IP-adres met [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#show):

```azurecli
az network public-ip show \
    --resource-group myResourceGroup \
    --name sshPublicIP \
    --query [ipAddress] \
    --output tsv
```

U kunt vervolgens SSH met uw virtuele machine normaal. Bieden u eigen openbare IP-adres van de voorgaande opdracht:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld moet u een basis Linux VM gemaakt. Ga voor meer Resource Manager-sjablonen die zijn toepassingsframeworks of complexere omgevingen maken, de [Azure-sjablonen snelstartgalerie](https://azure.microsoft.com/documentation/templates/).