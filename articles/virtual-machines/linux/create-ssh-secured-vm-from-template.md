---
title: Een Linux-VM in Azure maken met een sjabloon | Microsoft Docs
description: Hoe u een Linux-VM maken van een Resource Manager-sjabloon met de Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 01/03/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c6e4e142025e40e77afee01d70de9cef68eca1f0
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54013193"
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Een virtuele Linux-machine maken met Azure Resource Manager-sjablonen

In dit artikel wordt beschreven hoe u snel een Linux virtuele machine (VM) implementeren met Azure Resource Manager-sjablonen en Azure CLI. U kunt ook deze stappen uitvoeren met de [Azure klassieke CLI](create-ssh-secured-vm-from-template-nodejs.md).


In dit artikel wordt beschreven hoe u snel een Linux virtuele machine (VM) implementeren met Azure Resource Manager-sjablonen en Azure CLI. 

## <a name="templates-overview"></a>Overzicht van sjablonen
Azure Resource Manager-sjablonen zijn JSON-bestanden die u de infrastructuur en configuratie van uw Azure-oplossing definieert. Door het gebruik van een sjabloon kunt u gedurende de levenscyclus de oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd. Zie voor meer informatie over de indeling van de sjabloon en hoe u deze samenstelt, [uw eerste Azure Resource Manager-sjabloon maken](../../azure-resource-manager/resource-manager-create-first-template.md). Als u de JSON-syntaxis voor resourcetypen wilt bekijken, raadpleegt u [Define resources in Azure Resource Manager templates](/azure/templates/) (Resources definiëren in Azure Resource Manager-sjablonen).

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Voordat een virtuele machine wordt gemaakt, moet een resourcegroep worden gemaakt. Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupVM* in de *eastus* regio:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Het volgende voorbeeld wordt een virtuele machine via [deze Azure Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) met [az group deployment maken](/cli/azure/group/deployment#az_group_deployment_create). Alleen SSH-verificatie is toegestaan. Als u hierom wordt gevraagd, geeft u de waarde van uw eigen openbare SSH-sleutel, zoals de inhoud van *~/.ssh/id_rsa.pub*. Als u een SSH-sleutelpaar maken wilt, Zie [maken en gebruiken van een SSH-sleutelpaar voor virtuele Linux-machines in Azure](mac-create-ssh-keys.md).

```azurecli
az group deployment create \
    --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

In het vorige voorbeeld, maar u hebt opgegeven een sjabloon die is opgeslagen in GitHub. U kunt ook downloaden of een sjabloon maken en geef het lokale pad op met de `--template-file` parameter.


## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine
Voor SSH naar uw virtuele machine, verkrijgen van het openbare IP-adres met [az vm show](/cli/azure/vm#az-vm-show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name sshvm \
    --show-details \
    --query publicIps \
    --output tsv
```

U kunt vervolgens SSH met uw virtuele machine normaal. Geef u eigen openbare IP-adres uit de voorgaande opdracht:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld moet u een eenvoudige Linux-VM gemaakt. Ga voor meer Resource Manager-sjablonen die toepassingsframeworks of meer complexe omgevingen maken, de [galerie van Azure-quickstart-sjablonen](https://azure.microsoft.com/documentation/templates/).

Weergeven voor meer informatie over het maken van sjablonen, de eigenschappen voor de resources typen die u hebt geïmplementeerd en JSON-syntaxis:

* [Microsoft.Network/networkSecurityGroups](/azure/templates/microsoft.network/networksecuritygroups)
* [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
* [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
* [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
* [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
