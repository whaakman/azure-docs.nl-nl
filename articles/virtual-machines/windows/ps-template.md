---
title: Een Windows-VM maken van een sjabloon in Azure | Microsoft Docs
description: Een Resource Manager-sjabloon en PowerShell gebruiken om eenvoudig een nieuwe Windows-VM.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 19129d61-8c04-4aa9-a01f-361a09466805
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/22/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c1ba96c76c51abcfe5bb5ef9bd66eb8a50afdfda
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58846621"
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a>Een Windows virtuele machine maken van een Resource Manager-sjabloon

Informatie over het maken van een Windows-machine met behulp van een Azure Resource Manager-sjabloon en Azure PowerShell uit de Azure Cloud shell. De sjabloon die wordt gebruikt in dit artikel wordt een enkele virtuele machine met Windows Server in een nieuw virtueel netwerk met één subnet geïmplementeerd. Zie voor het maken van een virtuele Linux-machine, [over het maken van een virtuele Linux-machine met Azure Resource Manager-sjablonen](../linux/create-ssh-secured-vm-from-template.md).

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Het maken van een virtuele machine van Azure, meestal bestaat uit twee stappen:

- Maak een resourcegroep. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Voordat een virtuele machine wordt gemaakt, moet een resourcegroep worden gemaakt.
- Hiermee maakt u een virtuele machine.

Het volgende voorbeeld wordt een VM op basis van een [Azure Quickstart-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json). Hier volgt een kopie van de sjabloon:

[!code-json[create-windows-vm](~/quickstart-templates/101-vm-simple-windows/azuredeploy.json)]

Als u wilt het PowerShell-script uitvoeren, selecteert u **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the administrator username"
$adminPassword = Read-Host -Prompt "Enter the administrator password" -AsSecureString
$dnsLabelPrefix = Read-Host -Prompt "Enter an unique DNS name for the public IP"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json" `
    -adminUsername $adminUsername `
    -adminPassword $adminPassword `
    -dnsLabelPrefix $dnsLabelPrefix

 (Get-AzVm -ResourceGroupName $resourceGroupName).name

```

Als u wilt installeren en gebruiken van de PowerShell lokaal in plaats van via de Azure Cloud shell, wordt in deze zelfstudie de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

In het vorige voorbeeld, maar u hebt opgegeven een sjabloon die is opgeslagen in GitHub. U kunt ook downloaden of een sjabloon maken en geef het lokale pad op met de `--template-file` parameter.

Hier volgen enkele aanvullende resources:

- Zie voor meer informatie over het ontwikkelen van Resource Manager-sjablonen, [Azure Resource Manager-documentatie](/azure/azure-resource-manager/).
- Zie voor de virtuele machine van Azure-schema's [Azure sjabloonverwijzing](/azure/templates/microsoft.compute/allversions).
- Zie voor meer voorbeelden van de virtuele machine-sjabloon [Azure Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular).

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

De laatste PowerShell-opdracht uit het vorige script ziet u de naam van de virtuele machine. Zie voor verbinding met de virtuele machine, [hoe u verbinding maken met en meld u aan met een Azure-machine waarop Windows wordt uitgevoerd](./connect-logon.md).

## <a name="next-steps"></a>Volgende stappen

- Als er problemen met de implementatie zijn, kunt u Kijk eens [veelvoorkomende problemen oplossen Azure-implementatie met Azure Resource Manager](../../resource-manager-common-deployment-errors.md).
- Meer informatie over het maken en beheren van een virtuele machine in [maken en beheren van Windows-VM's met de Azure PowerShell-module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Weergeven voor meer informatie over het maken van sjablonen, de eigenschappen voor de resources typen die u hebt geïmplementeerd en JSON-syntaxis:

- [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
- [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
- [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
- [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
