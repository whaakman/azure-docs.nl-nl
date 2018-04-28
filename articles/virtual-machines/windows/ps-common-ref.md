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
ms.date: 07/17/2017
ms.author: cynthn
ms.openlocfilehash: 313a313764f8ba14c9661429d1f6a8463778c934
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Algemene PowerShell-opdrachten voor het maken en beheren van Azure Virtual Machines

In dit artikel bevat informatie over sommige van de Azure PowerShell-opdrachten die u kunt maken en beheren van virtuele machines in uw Azure-abonnement.  Voor meer hulp bij specifieke opdrachtregelopties en opties, kunt u **Get-Help** *opdracht*.

Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor informatie over het installeren van de nieuwste versie van Azure PowerShell, het selecteren van het abonnement en het aanmelden bij uw account.

Deze variabelen kunnen nuttig zijn voor u als meer dan één van de opdrachten in dit artikel wordt uitgevoerd:

- $location - de locatie van de virtuele machine. U kunt [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) vinden een [geografische regio](https://azure.microsoft.com/regions/) die voor u geschikt.
- $myResourceGroup - de naam van de resourcegroep die de virtuele machine bevat.
- $myVM - de naam van de virtuele machine.

## <a name="create-a-vm"></a>Een virtuele machine maken

| Taak | Opdracht |
| ---- | ------- |
| Maak een VM-configuratie |$vm = [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) -VMName $myVM -VMSize "Standard_D1_v1"<BR></BR><BR></BR>De VM-configuratie wordt gebruikt om te definiëren of instellingen voor de virtuele machine bijwerken. De configuratie is geïnitialiseerd met de naam van de virtuele machine en de bijbehorende [grootte](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Configuratie-instellingen toevoegen |$vm = [set AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) - VM $vm-Windows - ComputerName $myVM-$cred - ProvisionVMAgent referentie - EnableAutoUpdate<BR></BR><BR></BR>Instellingen van besturingssystemen met inbegrip van [referenties](https://technet.microsoft.com/library/hh849815.aspx) worden toegevoegd aan de configuration-object dat u eerder hebt gemaakt met behulp van New-AzureRmVMConfig. |
| Toevoegen van een netwerkinterface |$vm = [toevoegen AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Add-AzureRmVMNetworkInterface) - VM $vm-Id $nic. ID<BR></BR><BR></BR>Een virtuele machine moet hebben een [netwerkinterface](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kunnen communiceren in een virtueel netwerk. U kunt ook [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) voor het ophalen van een bestaande netwerkinterface-object. |
| Geef een platforminstallatiekopie |$vm = [set AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) - VM $vm - PublisherName 'publisher_name'-'publisher_offer' bieden - SKU's 'product_sku'-'nieuwste' versie<BR></BR><BR></BR>[Afbeelding van informatie](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) wordt toegevoegd aan de configuration-object dat u eerder hebt gemaakt met behulp van New-AzureRmVMConfig. Het object dat wordt geretourneerd door deze opdracht wordt alleen gebruikt bij het instellen van de schijf met het besturingssysteem een besturingssysteemkopie gebruiken. |
| OS-schijf voor gebruik van een platforminstallatiekopie instellen |$vm = [set AzureRmVMOSDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk) - VM $vm-naam 'myOSDisk' - VhdUri 'http://mystore1.blob.core.windows.net/vhds/myOSDisk.vhd' - CreateOption FromImage<BR></BR><BR></BR>De naam van de besturingssysteemschijf en de locatie in [opslag](../../storage/common/storage-powershell-guide-full.md) wordt toegevoegd aan de configuration-object dat u eerder hebt gemaakt. |
| OS-schijf voor gebruik van een algemene installatiekopie instellen |$vm = $vm set AzureRmVMOSDisk - VM-naam 'myOSDisk' - SourceImageUri 'https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk.{guid}.vhd'- VhdUri'https://mystore1.blob.core.windows.net/vhds/disk_name.vhd' - CreateOption FromImage-Windows<BR></BR><BR></BR>De naam van de schijf van het besturingssysteem, de locatie van de installatiekopie van de bron en locatie van de schijf in [opslag](../../storage/common/storage-powershell-guide-full.md) wordt toegevoegd aan de configuration-object. |
| OS-schijf voor gebruik van een gespecialiseerde installatiekopie instellen |$vm = $vm set AzureRmVMOSDisk - VM-naam 'myOSDisk' - VhdUri 'http://mystore1.blob.core.windows.net/vhds/' - CreateOption koppelen - Windows |
| Een virtuele machine maken |[Nieuwe AzureRmVM]() - ResourceGroupName $myResourceGroup-locatie $location - VM $vm<BR></BR><BR></BR>Alle resources worden gemaakt in een [resourcegroep](../../azure-resource-manager/powershell-azure-resource-manager.md). Voordat u deze opdracht uitvoert, nieuw AzureRmVMConfig, Set AzureRmVMOperatingSystem, Set AzureRmVMSourceImage, toevoegen AzureRmVMNetworkInterface en Set-AzureRmVMOSDisk uitvoeren. |

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
| Een virtuele machine generalize |[Set-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM -Generalized<BR></BR><BR></BR>Deze opdracht uitvoeren voordat u opslaan AzureRmVMImage uitvoert. |
| Een virtuele machine vastleggen |[Opslaan AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) myImagePrefix' - ResourceGroupName $myResourceGroup - VMName $myVM - DestinationContainerName 'myImageContainer' - VHDNamePrefix'-'C:\filepath\filename.json' pad<BR></BR><BR></BR>Een virtuele machine moet worden [voorbereid, afgesloten en gegeneraliseerd](prepare-for-upload-vhd-image.md) moet worden gebruikt om een installatiekopie te maken. Voordat u deze opdracht uitvoert, voert u Set-AzureRmVm. |
| Een virtuele machine bijwerken |[Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) - ResourceGroupName $myResourceGroup - VM $vm<BR></BR><BR></BR>Ophalen van de huidige VM-configuratie via de Get-AzureRmVM, configuratie-instellingen op het VM-object te wijzigen en voer deze opdracht. |
| Een gegevensschijf toevoegen aan een VM |[Voeg AzureRmVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk) - VM $vm-naam 'myDataDisk' - VhdUri 'https://mystore1.blob.core.windows.net/vhds/myDataDisk.vhd' - LUN #-Caching ReadWrite - DiskSizeinGB # - CreateOption leeg<BR></BR><BR></BR>Gebruik Get-AzureRmVM voor het VM-object. Geef het aantal LUN en de grootte van de schijf. Update-AzureRmVM om de configuratiewijzigingen toepassen voor de virtuele machine worden uitgevoerd. De schijf die u toevoegt, is niet geïnitialiseerd. |
| Een gegevensschijf verwijderen van een VM |[Remove-AzureRmVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmdatadisk) -VM $vm -Name "myDataDisk"<BR></BR><BR></BR>Gebruik Get-AzureRmVM voor het VM-object. Update-AzureRmVM om de configuratiewijzigingen toepassen voor de virtuele machine worden uitgevoerd. |
| Een uitbreiding voor een virtuele machine toevoegen |[Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) - ResourceGroupName $myResourceGroup-locatie $location - VMName $myVM-naam 'Extensienaam'-Publisher 'publisherName'-Type 'extensionType' - TypeHandlerVersion ' #. # '-Instellingen $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Deze opdracht uitvoert met de juiste [configuratiegegevens](template-description.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#extensions) voor de uitbreiding die u wilt installeren. |
| Een VM-extensie verwijderen |[Remove-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmextension) -ResourceGroupName $myResourceGroup -Name "extensionName" -VMName $myVM |

## <a name="next-steps"></a>Volgende stappen
* Zie de basisstappen voor het maken van een virtuele machine in [maken van een Windows-VM met Resource Manager en PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

