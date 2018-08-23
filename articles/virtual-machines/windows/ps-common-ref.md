---
title: Algemene PowerShell-opdrachten voor Azure Virtual Machines | Microsoft Docs
description: Algemene PowerShell-opdrachten om aan de slag met het maken en beheren van uw Windows-VM's in Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ba3839a2-f3d5-4e19-a5de-95bfb1c0e61e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: cb9f03ab87079ba33135840e3e7599d2e8cc95e9
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2018
ms.locfileid: "42054650"
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Algemene PowerShell-opdrachten voor het maken en beheren van Azure Virtual Machines

In dit artikel vindt u enkele van de Azure PowerShell-opdrachten die u gebruiken kunt voor het maken en beheren van virtuele machines in uw Azure-abonnement.  Voor meer informatie over specifieke opdrachtregelparameters en -opties, kunt u de **Get-Help** *opdracht*.

Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor informatie over het installeren van de nieuwste versie van Azure PowerShell, het selecteren van het abonnement en het aanmelden bij uw account.

Deze variabelen kunnen zijn nuttig voor u als meer dan een van de opdrachten in dit artikel uitvoert:

- $location - de locatie van de virtuele machine. U kunt [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) vinden een [geografische regio](https://azure.microsoft.com/regions/) die bij u past.
- $myResourceGroup - de naam van de resourcegroep met de virtuele machine.
- $myVM - de naam van de virtuele machine.

## <a name="create-a-vm---simplified"></a>Een virtuele machine maken - vereenvoudigd

| Taak | Opdracht |
| ---- | ------- |
| Een eenvoudige virtuele machine maken | [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) -$myVM naam <BR></BR><BR></BR> New-AzureRMVM bevat een set *vereenvoudigde* parameters, waar die vereist is, is één naam. De waarde van - naam wordt gebruikt als de naam voor alle bronnen die nodig zijn voor het maken van een nieuwe virtuele machine. U kunt informatie opgeven, maar dit is alles wat vereist is.|
| Een VM maken van een aangepaste installatiekopie | New-AzureRmVm - ResourceGroupName $myResourceGroup-naam $myVM ImageName "myImage"-locatie $location  <BR></BR><BR></BR>U moet al hebt gemaakt uw eigen [beheerde installatiekopie](capture-image-resource.md). U kunt een installatiekopie gebruiken om meerdere, identieke VM's. |



## <a name="create-a-vm-configuration"></a>Maak een VM-configuratie

| Taak | Opdracht |
| ---- | ------- |
| Maak een VM-configuratie |$vm = [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) -VMName $myVM -VMSize "Standard_D1_v1"<BR></BR><BR></BR>De VM-configuratie wordt gebruikt om instellingen voor de virtuele machine bijwerken of definiëren. De configuratie is geïnitialiseerd met de naam van de virtuele machine en de bijbehorende [grootte](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Configuratie-instellingen toevoegen |$vm = [set AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) - VM $vm-Windows - ComputerName $myVM-referentie-$cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Instellingen van besturingssystemen met inbegrip van [referenties](https://technet.microsoft.com/library/hh849815.aspx) worden toegevoegd aan het configuratieobject dat u eerder hebt gemaakt met behulp van New-AzureRmVMConfig. |
| Een netwerkinterface toevoegen |$vm = [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/Add-AzureRmVMNetworkInterface) - VM $vm-Id $nic. ID<BR></BR><BR></BR>Een virtuele machine moet hebben een [netwerkinterface](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om te communiceren in een virtueel netwerk. U kunt ook [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) om op te halen van een bestaande interfaceobject van clusternetwerk. |
| Geef een platforminstallatiekopie |$vm = [set AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) - VM $vm - PublisherName "publisher_name"-"publisher_offer" bieden - SKU's 'product_sku'-'nieuwste' versie<BR></BR><BR></BR>[Afbeelding van informatie](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) wordt toegevoegd aan het configuratieobject dat u eerder hebt gemaakt met behulp van New-AzureRmVMConfig. Het object dat wordt geretourneerd door deze opdracht wordt alleen gebruikt bij het instellen van de besturingssysteemschijf een platforminstallatiekopie gebruiken. |
| Een virtuele machine maken |[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) - ResourceGroupName $myResourceGroup-locatie $location - VM $vm<BR></BR><BR></BR>Alle resources worden gemaakt in een [resourcegroep](../../azure-resource-manager/powershell-azure-resource-manager.md). Voordat u met deze opdracht uitvoert, voert u New-AzureRmVMConfig, Set-AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, Add-AzureRmVMNetworkInterface en Set-azurermvmosdisk. |
| Een virtuele machine bijwerken |[Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) - ResourceGroupName $myResourceGroup - VM $vm<BR></BR><BR></BR>De huidige VM-configuratie met behulp van Get-AzureRmVM, configuratie-instellingen op de VM-object wijzigen en voer vervolgens deze opdracht uit. |

## <a name="get-information-about-vms"></a>Informatie over virtuele machines

| Taak | Opdracht |
| ---- | ------- |
| Lijst met virtuele machines in een abonnement |[Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) |
| Lijst met virtuele machines in een resourcegroep |Get-AzureRmVM -ResourceGroupName $myResourceGroup<BR></BR><BR></BR>Als u een lijst met resourcegroepen in uw abonnement, gebruikt [Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresourcegroup). |
| Informatie over een VM ophalen |Get-AzureRmVM -ResourceGroupName $myResourceGroup -Name $myVM |

## <a name="manage-vms"></a>Virtuele machines beheren
| Taak | Opdracht |
| --- | --- |
| Een VM starten |[Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Een VM stoppen |[Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Een actieve virtuele machine opnieuw starten |[Restart-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/restart-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Een VM verwijderen |[Remove-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |


## <a name="next-steps"></a>Volgende stappen
* Zie de basisstappen voor het maken van een virtuele machine in [een Windows VM maken met Resource Manager en PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

