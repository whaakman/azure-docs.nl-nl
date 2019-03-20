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
ms.date: 03/14/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5ff8beb1995359bad93449744718091c338e4994
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226552"
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Een virtuele Linux-machine maken met Azure Resource Manager-sjablonen

In dit artikel wordt beschreven hoe u snel een Linux virtuele machine (VM) implementeren met Azure Resource Manager-sjablonen en Azure CLI. 

## <a name="templates-overview"></a>Overzicht van sjablonen

Azure Resource Manager-sjablonen zijn JSON-bestanden die u de infrastructuur en configuratie van uw Azure-oplossing definieert. Door het gebruik van een sjabloon kunt u gedurende de levenscyclus de oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd. Zie voor meer informatie over de indeling van de sjabloon en hoe u deze samenstelt, [Quick Start: Azure Resource Manager-sjablonen maken en implementeren via Azure Portal](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md). Als u de JSON-syntaxis voor resourcetypen wilt bekijken, raadpleegt u [Define resources in Azure Resource Manager templates](/azure/templates/microsoft.compute/allversions) (Resources definiëren in Azure Resource Manager-sjablonen).

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Het maken van een virtuele machine van Azure, meestal bestaat uit twee stappen:

1. Maak een resourcegroep. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Voordat een virtuele machine wordt gemaakt, moet een resourcegroep worden gemaakt.
1. Hiermee maakt u een virtuele machine.

Het volgende voorbeeld wordt een virtuele machine via [een Azure-Quickstart-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Alleen SSH-verificatie is toegestaan voor deze implementatie. Als u hierom wordt gevraagd, geeft u de waarde van uw eigen openbare SSH-sleutel, zoals de inhoud van *~/.ssh/id_rsa.pub*. Als u een SSH-sleutelpaar maken wilt, Zie [maken en gebruiken van een SSH-sleutelpaar voor virtuele Linux-machines in Azure](mac-create-ssh-keys.md). Hier volgt een kopie van de sjabloon:

[!code-json[create-linux-vm](~/quickstart-templates/101-vm-sshkey/azuredeploy.json)]

Als u wilt de CLI-script uitvoeren, selecteert u **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
echo "Enter the project name (used for generating resource names):" &&
read projectName &&
echo "Enter the administrator username:" &&
read username &&
echo "Enter the SSH public key:" &&
read key &&
az group create --name $resourceGroupName --location "$location" &&
az group deployment create --resource-group $resourceGroupName --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json --parameters projectName=$projectName adminUsername=$username adminPublicKey='$key' &&
az vm show --resource-group $resourceGroupName --name "$projectName-vm" --show-details --query publicIps --output tsv
```

De laatste Azure CLI-opdracht laat zien dat het openbare IP-adres van de zojuist gemaakte virtuele machine. U moet het openbare IP-adres verbinding maken met de virtuele machine. Zie de volgende sectie van dit artikel.

In het vorige voorbeeld, maar u hebt opgegeven een sjabloon die is opgeslagen in GitHub. U kunt ook downloaden of een sjabloon maken en geef het lokale pad op met de `--template-file` parameter.

Hier volgen enkele aanvullende resources:

- Zie voor meer informatie over het ontwikkelen van Resource Manager-sjablonen, [Azure Resource Manager-documentatie](/azure/azure-resource-manager/).
- Zie voor de virtuele machine van Azure-schema's [Azure sjabloonverwijzing](/azure/templates/microsoft.compute/allversions).
- Zie voor meer voorbeelden van de virtuele machine-sjabloon [Azure Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular).

## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

U kunt vervolgens SSH met uw virtuele machine normaal. Geef u eigen openbare IP-adres uit de voorgaande opdracht:

```bash
ssh <adminUsername>@<ipAddress>
```

## <a name="next-steps"></a>Volgende stappen

In dit voorbeeld moet u een eenvoudige Linux-VM gemaakt. Ga voor meer Resource Manager-sjablonen die toepassingsframeworks of meer complexe omgevingen maken, de [Azure Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular).

Weergeven voor meer informatie over het maken van sjablonen, de eigenschappen voor de resources typen die u hebt geïmplementeerd en JSON-syntaxis:

* [Microsoft.Network/networkSecurityGroups](/azure/templates/microsoft.network/networksecuritygroups)
* [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
* [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
* [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
* [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
