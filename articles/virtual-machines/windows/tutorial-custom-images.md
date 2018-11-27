---
title: Zelfstudie - Aangepaste VM-installatiekopieën maken met Azure PowerShell | Microsoft Docs
description: In deze zelfstudie leert u hoe u Azure PowerShell gebruikt om een aangepaste installatiekopie van een virtuele machine te maken in Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 415652739c4987deafe820c31499132ec3829c8b
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263312"
---
# <a name="tutorial-create-a-custom-image-of-an-azure-vm-with-azure-powershell"></a>Zelfstudie: Een aangepaste installatiekopie van een Azure-VM maken met Azure PowerShell

Aangepaste installatiekopieën zijn soortgelijk aan Marketplace-installatiekopieën, maar u kunt deze zelf maken. Aangepaste installatiekopieën kunnen worden gebruikt voor het opstarten van configuraties, zoals het vooraf laden van toepassingen, toepassingsconfiguraties en andere besturingssysteemconfiguraties. In deze zelfstudie maakt u uw eigen aangepaste installatiekopie van een virtuele Azure-machine. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Systeem voorbereiden en virtuele machines generaliseren
> * Een aangepaste installatiekopie maken
> * Een VM maken van een aangepaste installatiekopie
> * Alle installatiekopieën in uw abonnement weergeven
> * Een aangepaste installatiekopie verwijderen

## <a name="before-you-begin"></a>Voordat u begint

In de onderstaande stappen wordt gedetailleerd beschreven hoe u van een bestaande virtuele machine een herbruikbare aangepaste installatiekopie maakt die u kunt gebruiken om nieuwe virtuele machines te maken.

Om het voorbeeld in deze zelfstudie uit te voeren, moet u een bestaande virtuele machine hebben. Indien nodig kan dit [voorbeeldscript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) er een voor u maken. Vervang tijdens het volgen van de zelfstudie de namen van de resourcegroep en de virtuele machine waar nodig.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie moduleversie 5.7.0 of hoger van AzureRM vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="prepare-vm"></a>VM voorbereiden

Voor het maken van een installatiekopie van een virtuele machine moet u de virtuele machine voorbereiden door de VM te generaliseren, de toewijzing ervan ongedaan te maken en de bron-VM vervolgens als gegeneraliseerd te markeren.

### <a name="generalize-the-windows-vm-using-sysprep"></a>De Windows VM generaliseren met behulp van Sysprep

Sysprep verwijdert onder meer al uw persoonlijke accountinformatie en de machine wordt voorbereid om als een installatiekopie te worden gebruikt. Raadpleeg [Sysprep gebruiken: een inleiding](https://technet.microsoft.com/library/bb457073.aspx) voor meer informatie over Sysprep.


1. Maak verbinding met de virtuele machine.
2. Open het venster met de opdrachtprompt als beheerder. Wijzig de directory in *%windir%\system32\sysprep* en voer dan *sysprep.exe* uit.
3. In het dialoogvenster **Hulpprogramma voor systeemvoorbereiding** selecteert u *OOBE (Out-of-Box Experience) van systeem starten* en zorgt u dat het selectievakje *Generaliseren* is ingeschakeld.
4. In **Opties voor afsluiten** selecteert u *Afsluiten* en klikt u vervolgens op **OK**.
5. Wanneer Sysprep is voltooid, wordt de virtuele machine afgesloten. **Start de VM niet opnieuw**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>De toewijzing van de virtuele machine ongedaan maken en de virtuele machine markeren als gegeneraliseerd

Voor het maken van een installatiekopie moet de toewijzing van de virtuele machine ongedaan worden gemaakt en moet de VM worden gemarkeerd als gegeneraliseerd in Azure.

De toewijzing van de VM wordt opgeheven met [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Force
```

Stel de status van de virtuele machine in op `-Generalized` met [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm). 
   
```azurepowershell-interactive
Set-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Generalized
```


## <a name="create-the-image"></a>De installatiekopie maken

Maak nu een installatiekopie van de VM met [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) en [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). In het volgende voorbeeld wordt een installatiekopie met de naam *myImage* gemaakt van een virtuele machine met de naam *myVM*.

Haal de virtuele machine op. 

```azurepowershell-interactive
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
```

Maak de configuratie van installatiekopie.

```azurepowershell-interactive
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Maak de installatiekopie.

```azurepowershell-interactive
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>VM's maken van de installatiekopie

Nu u een installatiekopie hebt gemaakt, kunt u een of meer nieuwe VM's van de installatiekopie maken met behulp. Het maken van een VM op basis van een aangepaste installatiekopie is vergelijkbaar met het maken van een VM met behulp van een Marketplace-installatiekopie. Wanneer u een Marketplace-installatiekopie gebruikt, hebt u de informatie over de installatiekopie, de leverancier van de installatiekopie, de aanbieding, de SKU en de versie. Met de vereenvoudigde parameterset voor de cmdlet [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm) hoeft u alleen de naam op te geven van de aangepaste installatiekopie, zolang deze zich in dezelfde resourcegroep bevindt. 

In dit voorbeeld wordt een VM met de naam *myVMfromImage* gemaakt van een installatiekopie met de naam *myImage* in *myResourceGroup*.


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

Hier volgen enkele voorbeelden van algemene taken voor beheerde installatiekopieën en hoe u deze kunt uitvoeren met PowerShell.

Geef een lijst weer van alle installatiekopieën gesorteerd op naam.

```azurepowershell-interactive
$images = Get-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Een installatiekopie verwijderen. In dit voorbeeld wordt de installatiekopie met de naam *myImage* verwijderd uit *myResourceGroup*.

```azurepowershell-interactive
Remove-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een aangepaste installatiekopie voor een virtuele machine gemaakt. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Systeem voorbereiden en virtuele machines generaliseren
> * Een aangepaste installatiekopie maken
> * Een VM maken van een aangepaste installatiekopie
> * Alle installatiekopieën in uw abonnement weergeven
> * Een aangepaste installatiekopie verwijderen

Ga door met de volgende zelfstudie voor meer informatie over virtuele machines met hoge beschikbaarheid.

> [!div class="nextstepaction"]
> [Virtuele machines met hoge beschikbaarheid maken](tutorial-availability-sets.md)



