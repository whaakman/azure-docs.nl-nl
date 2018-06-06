---
title: Algemene PowerShell-opdrachten voor Azure Virtual Machines | Microsoft Docs
description: Algemene PowerShell-opdrachten kunt u op weg maken en beheren van uw Windows-machines in Azure.
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
ms.openlocfilehash: ff861c21250a042191651ab4a4cffbf3928e4f26
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738561"
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Algemene PowerShell-opdrachten voor het maken en beheren van Azure Virtual Machines

In dit artikel bevat informatie over sommige van de Azure PowerShell-opdrachten die u kunt maken en beheren van virtuele machines in uw Azure-abonnement.  Voor meer hulp bij specifieke opdrachtregelopties en opties, kunt u de **Get-Help** *opdracht*.

Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor informatie over het installeren van de nieuwste versie van Azure PowerShell, het selecteren van het abonnement en het aanmelden bij uw account.

Deze variabelen kunnen nuttig zijn voor u als meer dan één van de opdrachten in dit artikel wordt uitgevoerd:

- $location - de locatie van de virtuele machine. U kunt [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) vinden een [geografische regio](https://azure.microsoft.com/regions/) die voor u geschikt.
- $myResourceGroup - de naam van de resourcegroep die de virtuele machine bevat.
- $myVM - de naam van de virtuele machine.

## <a name="create-a-vm---simplified"></a>Maak een VM - vereenvoudigd

| Taak | Opdracht |
| ---- | ------- |
| Een eenvoudige virtuele machine maken | [Nieuwe AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) -$myVM naam <BR></BR><BR></BR> Nieuwe AzureRMVM beschikt over een reeks *vereenvoudigde* parameters, waar nodig één naam is. De waarde voor - naam wordt gebruikt als de naam voor alle resources die zijn vereist voor het maken van een nieuwe virtuele machine. U kunt meer opgeven, maar dit is vereist.|
| Een VM maken van een aangepaste installatiekopie | Nieuwe-AzureRmVm - ResourceGroupName $myResourceGroup-naam $myVM ImageName 'myImage'-locatie $location  <BR></BR><BR></BR>U moet al hebt gemaakt uw eigen [begeleide afbeelding](capture-image-resource.md). U kunt een installatiekopie kunt gebruiken om meerdere, identieke virtuele machines. |



## <a name="create-a-vm-configuration"></a>Maak een VM-configuratie

| Taak | Opdracht |
| ---- | ------- |
| Maak een VM-configuratie |$vm = [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) -VMName $myVM -VMSize "Standard_D1_v1"<BR></BR><BR></BR>De VM-configuratie wordt gebruikt om te definiëren of instellingen voor de virtuele machine bijwerken. De configuratie is geïnitialiseerd met de naam van de virtuele machine en de bijbehorende [grootte](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Configuratie-instellingen toevoegen |$vm = [set AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) - VM $vm-Windows - ComputerName $myVM-$cred - ProvisionVMAgent referentie - EnableAutoUpdate<BR></BR><BR></BR>Instellingen van besturingssystemen met inbegrip van [referenties](https://technet.microsoft.com/library/hh849815.aspx) worden toegevoegd aan de configuration-object dat u eerder hebt gemaakt met behulp van New-AzureRmVMConfig. |
| Toevoegen van een netwerkinterface |$vm = [toevoegen AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Add-AzureRmVMNetworkInterface) - VM $vm-Id $nic. ID<BR></BR><BR></BR>Een virtuele machine moet hebben een [netwerkinterface](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kunnen communiceren in een virtueel netwerk. U kunt ook [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) voor het ophalen van een bestaande netwerkinterface-object. |
| Geef een platforminstallatiekopie |$vm = [set AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) - VM $vm - PublisherName 'publisher_name'-'publisher_offer' bieden - SKU's 'product_sku'-'nieuwste' versie<BR></BR><BR></BR>[Afbeelding van informatie](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) wordt toegevoegd aan de configuration-object dat u eerder hebt gemaakt met behulp van New-AzureRmVMConfig. Het object dat wordt geretourneerd door deze opdracht wordt alleen gebruikt bij het instellen van de schijf met het besturingssysteem een besturingssysteemkopie gebruiken. |
| Een virtuele machine maken |[Nieuwe AzureRmVM]() - ResourceGroupName $myResourceGroup-locatie $location - VM $vm<BR></BR><BR></BR>Alle resources worden gemaakt in een [resourcegroep](../../azure-resource-manager/powershell-azure-resource-manager.md). Voordat u deze opdracht uitvoert, nieuw AzureRmVMConfig, Set AzureRmVMOperatingSystem, Set AzureRmVMSourceImage, toevoegen AzureRmVMNetworkInterface en Set-AzureRmVMOSDisk uitvoeren. |
| Een virtuele machine bijwerken |[Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) - ResourceGroupName $myResourceGroup - VM $vm<BR></BR><BR></BR>Ophalen van de huidige VM-configuratie via de Get-AzureRmVM, configuratie-instellingen op het VM-object te wijzigen en voer deze opdracht. |

## <a name="get-information-about-vms"></a>Informatie ophalen over virtuele machines

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
| Een actieve virtuele machine starten |[Restart-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/restart-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Een VM verwijderen |[Remove-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |


## <a name="next-steps"></a>Volgende stappen
* Zie de basisstappen voor het maken van een virtuele machine in [maken van een Windows-VM met Resource Manager en PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

