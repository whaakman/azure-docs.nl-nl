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
ms.openlocfilehash: 7be31a9390dfb0d663b27979a42fffe6f7a0afca
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55977523"
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Algemene PowerShell-opdrachten voor het maken en beheren van Azure Virtual Machines

In dit artikel vindt u enkele van de Azure PowerShell-opdrachten die u gebruiken kunt voor het maken en beheren van virtuele machines in uw Azure-abonnement.  Voor meer informatie over specifieke opdrachtregelparameters en -opties, kunt u de **Get-Help** *opdracht*.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

Deze variabelen kunnen zijn nuttig voor u als meer dan een van de opdrachten in dit artikel uitvoert:

- $location - de locatie van de virtuele machine. U kunt [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation) vinden een [geografische regio](https://azure.microsoft.com/regions/) die bij u past.
- $myResourceGroup - de naam van de resourcegroep met de virtuele machine.
- $myVM - de naam van de virtuele machine.

## <a name="create-a-vm---simplified"></a>Een virtuele machine maken - vereenvoudigd

| Taak | Opdracht |
| ---- | ------- |
| Een eenvoudige virtuele machine maken | [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) -Name $myVM <BR></BR><BR></BR> Nieuwe-AzVM bevat een set *vereenvoudigde* parameters, waar die vereist is, is één naam. De waarde van - naam wordt gebruikt als de naam voor alle bronnen die nodig zijn voor het maken van een nieuwe virtuele machine. U kunt informatie opgeven, maar dit is alles wat vereist is.|
| Een VM maken van een aangepaste installatiekopie | New-AzVm -ResourceGroupName $myResourceGroup -Name $myVM ImageName "myImage" -Location $location  <BR></BR><BR></BR>U moet al hebt gemaakt uw eigen [beheerde installatiekopie](capture-image-resource.md). U kunt een installatiekopie gebruiken om meerdere, identieke VM's. |



## <a name="create-a-vm-configuration"></a>Maak een VM-configuratie

| Taak | Opdracht |
| ---- | ------- |
| Maak een VM-configuratie |$vm = [New-AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig) -VMName $myVM -VMSize "Standard_D1_v1"<BR></BR><BR></BR>De VM-configuratie wordt gebruikt om instellingen voor de virtuele machine bijwerken of definiëren. De configuratie is geïnitialiseerd met de naam van de virtuele machine en de bijbehorende [grootte](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Configuratie-instellingen toevoegen |$vm = [Set-AzVMOperatingSystem](https://docs.microsoft.com/powershell/module/az.compute/set-azvmoperatingsystem) -VM $vm -Windows -ComputerName $myVM -Credential $cred -ProvisionVMAgent -EnableAutoUpdate<BR></BR><BR></BR>Instellingen van besturingssystemen met inbegrip van [referenties](https://technet.microsoft.com/library/hh849815.aspx) worden toegevoegd aan het configuratieobject dat u eerder hebt gemaakt met behulp van New-AzVMConfig. |
| Een netwerkinterface toevoegen |$vm = [toevoegen AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/Add-AzVMNetworkInterface) - VM $vm-Id $nic. ID<BR></BR><BR></BR>Een virtuele machine moet hebben een [netwerkinterface](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om te communiceren in een virtueel netwerk. U kunt ook [Get-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface) om op te halen van een bestaande interfaceobject van clusternetwerk. |
| Geef een platforminstallatiekopie |$vm = [set AzVMSourceImage](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsourceimage) - VM $vm - PublisherName "publisher_name"-"publisher_offer" bieden - SKU's 'product_sku'-'nieuwste' versie<BR></BR><BR></BR>[Afbeelding van informatie](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) wordt toegevoegd aan het configuratieobject dat u eerder hebt gemaakt met behulp van New-AzVMConfig. Het object dat wordt geretourneerd door deze opdracht wordt alleen gebruikt bij het instellen van de besturingssysteemschijf een platforminstallatiekopie gebruiken. |
| Een virtuele machine maken |[New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) -ResourceGroupName $myResourceGroup -Location $location -VM $vm<BR></BR><BR></BR>Alle resources worden gemaakt in een [resourcegroep](../../azure-resource-manager/powershell-azure-resource-manager.md). Voordat u deze opdracht uitvoert, voer New-AzVMConfig, Set-AzVMOperatingSystem, Set-AzVMSourceImage, Add-AzVMNetworkInterface en Set-AzVMOSDisk. |
| Een virtuele machine bijwerken |[Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) -ResourceGroupName $myResourceGroup -VM $vm<BR></BR><BR></BR>De huidige VM-configuratie met behulp van Get-AzVM, configuratie-instellingen op de VM-object wijzigen en voer vervolgens deze opdracht uit. |

## <a name="get-information-about-vms"></a>Informatie over virtuele machines

| Taak | Opdracht |
| ---- | ------- |
| Lijst met virtuele machines in een abonnement |[Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) |
| Lijst met virtuele machines in een resourcegroep |Get-AzVM -ResourceGroupName $myResourceGroup<BR></BR><BR></BR>Als u een lijst met resourcegroepen in uw abonnement, gebruikt [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup). |
| Informatie over een VM ophalen |Get-AzVM -ResourceGroupName $myResourceGroup -Name $myVM |

## <a name="manage-vms"></a>Virtuele machines beheren
| Taak | Opdracht |
| --- | --- |
| Een VM starten |[Start-AzVM](https://docs.microsoft.com/powershell/module/az.compute/start-azvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Een VM stoppen |[Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Een actieve virtuele machine opnieuw starten |[Restart-AzVM](https://docs.microsoft.com/powershell/module/az.compute/restart-azvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Een VM verwijderen |[Remove-AzVM](https://docs.microsoft.com/powershell/module/az.compute/remove-azvm) -ResourceGroupName $myResourceGroup -Name $myVM |


## <a name="next-steps"></a>Volgende stappen
* Zie de basisstappen voor het maken van een virtuele machine in [een Windows VM maken met Resource Manager en PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

