---
title: Maken van aangepaste installatiekopieën voor virtuele machine met Azure PowerShell | Microsoft Docs
description: Zelfstudie - maken van een aangepaste VM-installatiekopie met de Azure PowerShell.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 443f47b98ea063c6fe1f0b3517c00b6cf3692161
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a>Maken van een aangepaste installatiekopie van een virtuele machine in Azure met behulp van PowerShell

Aangepaste installatiekopieën zijn soortgelijk aan Marketplace-installatiekopieën, maar u kunt deze zelf maken. Aangepaste installatiekopieën kunnen worden gebruikt voor het opstarten van configuraties, zoals het vooraf laden van toepassingen, toepassingsconfiguraties en andere besturingssysteemconfiguraties. In deze zelfstudie maakt u uw eigen aangepaste installatiekopie van een virtuele Azure-machine. Procedures voor:

> [!div class="checklist"]
> * Sysprep en generalize van virtuele machines
> * Een aangepaste installatiekopie maken
> * Een VM maken van een aangepaste installatiekopie
> * Alle installatiekopieën in uw abonnement weergeven
> * Een aangepaste installatiekopie verwijderen


## <a name="before-you-begin"></a>Voordat u begint

In de onderstaande stappen wordt gedetailleerd beschreven hoe u van een bestaande virtuele machine een herbruikbare aangepaste installatiekopie maakt die u kunt gebruiken om nieuwe virtuele machines te maken.

Om het voorbeeld in deze zelfstudie uit te voeren, moet u een bestaande virtuele machine hebben. Indien nodig kan dit [voorbeeldscript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) er een voor u maken. Vervang tijdens het volgen van de zelfstudie de namen van de resourcegroep en de virtuele machine waar nodig.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van de PowerShell lokaal, in deze zelfstudie vereist de AzureRM moduleversie 5.6.0 of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="prepare-vm"></a>Virtuele machine voorbereiden

Voor het maken van een installatiekopie van een virtuele machine, moet u de virtuele machine voorbereiden door het generaliseren van de virtuele machine, toewijzing en vervolgens de bron-VM als gegeneraliseerd in Azure te markeren.

### <a name="generalize-the-windows-vm-using-sysprep"></a>De virtuele machine van Windows met behulp van Sysprep generalize

Sysprep verwijdert alle persoonlijke gegevens over uw account, onder andere en voorbereiden van de machine moet worden gebruikt als een afbeelding. Zie voor meer informatie over Sysprep [hoe gebruik Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).


1. Verbinding maken met de virtuele machine.
2. Open het venster opdrachtprompt als beheerder. Wijzig de map in *%windir%\system32\sysprep*, en voer vervolgens *sysprep.exe*.
3. In de **hulpprogramma voor systeemvoorbereiding** dialoogvenster, *System Voer Out-of-Box Experience (OOBE)*, en zorg ervoor dat de *Generalize* selectievakje is ingeschakeld.
4. In **afsluitopties**, selecteer *afsluiten* en klik vervolgens op **OK**.
5. Wanneer Sysprep is voltooid, afgesloten de virtuele machine. **De virtuele machine niet opnieuw**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>De toewijzing van de virtuele machine ongedaan maken en de virtuele machine markeren als gegeneraliseerd

Voor het maken van een installatiekopie, moet de virtuele machine worden toewijzing ongedaan gemaakt en is gemarkeerd als gegeneraliseerd in Azure.

De virtuele machine met behulp van de toewijzing opgeheven [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Force
```

Zet de status van de virtuele machine `-Generalized` met [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm). 
   
```azurepowershell-interactive
Set-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Generalized
```


## <a name="create-the-image"></a>De installatiekopie maken

Nu u een installatiekopie van de virtuele machine maken met behulp van kunt [nieuw AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) en [nieuw AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). In het volgende voorbeeld wordt een installatiekopie met de naam *myImage* gemaakt van een virtuele machine met de naam *myVM*.

Zorg dat de virtuele machine. 

```azurepowershell-interactive
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
```

Maak de configuratie van de installatiekopie.

```azurepowershell-interactive
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Maken van de installatiekopie.

```azurepowershell-interactive
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>VM's maken van de installatiekopie

Nu dat u een installatiekopie hebt, kunt u een of meer nieuwe virtuele machines kunt maken van de installatiekopie. Maken van een virtuele machine van een aangepaste installatiekopie is vergelijkbaar met het maken van een virtuele machine met behulp van een Marketplace-installatiekopie. Wanneer u een Marketplace-installatiekopie gebruikt, hebt u de informatie over de installatiekopie, afbeeldingenprovider, aanbieding, SKU en versie. Met de parameter vereenvoudigde ingesteld voor de [New-AzureRMVM]() cmdlet, moet u de naam van de aangepaste installatiekopie opgeven als deze zich in dezelfde resourcegroep. 

In dit voorbeeld wordt een virtuele machine met de naam *myVMfromImage* van de *myImage*, in de *myResourceGroup*.


```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```

## <a name="image-management"></a>Installatiekopiebeheer 

Hier volgen enkele voorbeelden van algemene beheertaken voor de installatiekopie en hoe u uit te voeren met behulp van PowerShell.

Lijst van alle installatiekopieën op naam.

```azurepowershell-interactive
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Een installatiekopie verwijderen. In dit voorbeeld wordt de installatiekopie met de naam *myOldImage* verwijderd uit de *myResourceGroup*.

```azurepowershell-interactive
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een aangepaste installatiekopie voor een virtuele machine gemaakt. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Sysprep en generalize van virtuele machines
> * Een aangepaste installatiekopie maken
> * Een VM maken van een aangepaste installatiekopie
> * Alle installatiekopieën in uw abonnement weergeven
> * Een aangepaste installatiekopie verwijderen

Ga naar de volgende zelfstudie voor meer informatie over hoe maximaal beschikbare virtuele machines.

> [!div class="nextstepaction"]
> [Virtuele machines met hoge beschikbaarheid maken](tutorial-availability-sets.md)



