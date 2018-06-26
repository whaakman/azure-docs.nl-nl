---
title: Linux-VM in Azure maken van een sjabloon | Microsoft Docs
description: Het gebruik van de Azure CLI voor het maken van een Linux-VM met een Resource Manager-sjabloon
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/30/2018
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc2b4dde9796336112d6c8a68d16d0b3006b3fee
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936411"
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Het maken van een virtuele Linux-machine met Azure Resource Manager-sjablonen
In dit artikel laat zien hoe snel virtuele Linux-machine (VM) te implementeren met Azure Resource Manager-sjablonen en de Azure CLI. 


## <a name="templates-overview"></a>Overzicht van sjablonen
Azure Resource Manager-sjablonen zijn JSON-bestanden die de infrastructuur en configuratie van uw Azure-oplossing te definiëren. Door het gebruik van een sjabloon kunt u gedurende de levenscyclus de oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd. Zie voor meer informatie over de indeling van de sjabloon en de manier waarop u samenstelt, [maken van uw eerste Azure Resource Manager-sjabloon](../../azure-resource-manager/resource-manager-create-first-template.md). Als u de JSON-syntaxis voor resourcetypen wilt bekijken, raadpleegt u [Define resources in Azure Resource Manager templates](/azure/templates/) (Resources definiëren in Azure Resource Manager-sjablonen).


## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Voordat een virtuele machine wordt gemaakt, moet een resourcegroep worden gemaakt. Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupVM* in de *eastus* regio:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Het volgende voorbeeld wordt een virtuele machine uit [deze Azure Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) met [az implementatie maken](/cli/azure/group/deployment#az_group_deployment_create). SSH-verificatie is toegestaan. Geef desgevraagd de waarde van uw eigen openbare SSH-sleutel, zoals de inhoud van *~/.ssh/id_rsa.pub*. Als u maken van een SSH-sleutelpaar wilt, Zie [maken en gebruiken van een SSH-sleutelpaar voor virtuele Linux-machines in Azure](mac-create-ssh-keys.md).

```azurecli
az group deployment create \
    --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

In het vorige voorbeeld, kunt u een sjabloon die is opgeslagen in GitHub opgegeven. U kunt ook downloaden of een sjabloon maken en geef het lokale pad op met de `--template-file` parameter.


## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine
Om SSH met uw virtuele machine te verkrijgen van het openbare IP-adres met [az vm weergeven](/cli/azure/vm#az-vm-show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name sshvm \
    --show-details \
    --query publicIps \
    --output tsv
```

U kunt vervolgens SSH met uw virtuele machine normaal. Bieden u eigen openbare IP-adres van de voorgaande opdracht:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld moet u een basis Linux VM gemaakt. Ga voor meer Resource Manager-sjablonen die zijn toepassingsframeworks of complexere omgevingen maken, de [Azure-sjablonen snelstartgalerie](https://azure.microsoft.com/documentation/templates/).