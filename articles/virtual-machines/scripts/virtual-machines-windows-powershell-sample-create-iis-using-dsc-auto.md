---
title: Een virtuele machine maken met IIS gebruik van DSC en nieuw AzVM in Azure | Microsoft Docs
description: Azure PowerShell-voorbeeldscript - Maak een IIS-VM met DSC en nieuw AzVM.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/29/2017
ms.author: cynthn
ms.openlocfilehash: fb7f417912124c87ed35572a00b7da81968c8d1f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-iis-vm-with-powershell-using-new-azvm-preview"></a>Maak een IIS-VM met PowerShell met behulp van New-AzVM (preview)

Dit script wordt een Azure-virtuele Machine met Windows Server 2016 met behulp van New-AzVM gemaakt en gebruikt vervolgens het DSC-extensie van de Azure-virtuele Machine om IIS te installeren. Nadat het script is uitgevoerd, kunt u toegang tot de standaardwebsite van IIS op het openbare IP-adres van de virtuele machine.

Dit voorbeeld gebruikt de cmdlet New-AzVM, dat zich in preview bevindt. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

## <a name="sample-script"></a>Voorbeeld van een script

```azurepowershell-interactive
# Variables for common values
$location = "westeurope"
$vmName = "myVM"

New-AzVM -Name $vmName `
    -VirtualNetworkName myVNet `
    -Location $location `
    -SecurityGroupName myNSG `
    -PublicIpAddressName myPIP


# Create an inbound network security group rule for port 80

$nsgRuleHTTP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleHTTP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 80 -Access Allow

$nsg = Get-AzureRmNetworkSecurityGroup -Name myNSG -ResourceGroupName myVMResourceGroup
$nsg | Add-AzureRmNetworkSecurityRuleConfig -Name $nsgRuleHTTP -NetworkSecurityGroup myNSG 
$nsg | Set-AzureRmNetworkSecurityGroup


# Install IIS
$PublicSettings = '{"ModulesURL":"https://github.com/Azure/azure-quickstart-templates/raw/master/dsc-extension-iis-server-windows-vm/ContosoWebsite.ps1.zip", "configurationFunction": "ContosoWebsite.ps1\\ContosoWebsite", "Properties": {"MachineName": "myVM"} }'

Set-AzureRmVMExtension -ExtensionName "DSC" -ResourceGroupName myVMResourceGroup -VMName $vmName `
  -Publisher "Microsoft.Powershell" -ExtensionType "DSC" -TypeHandlerVersion 2.19 `
  -SettingString $PublicSettings -Location $location
 ```

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVMResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten om de implementatie te maken. Elk item in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Maakt een groep regel de netwerkbeveiligingsconfiguratie. Deze configuratie wordt gebruikt voor het maken van een regel voor het NSG wanneer het NSG wordt gemaakt. |
| [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup) | NSG-informatie opgehaald. |
| [Voeg AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig) | Hiermee voegt u de nieuwe regel toe aan de configuratie. |
| [Set-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/Set-AzureRmNetworkSecurityGroup) | Het NSG-updates met de nieuwe regel. |
| [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | Een VM-extensie toevoegen aan de virtuele machine. In dit voorbeeld wordt de extensie voor aangepaste scripts worden gebruikt om IIS te installeren. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep en alle resources binnen. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van extra virtuele machine PowerShell-script kunnen worden gevonden in de [virtuele machine van Windows Azure-documentatie](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
