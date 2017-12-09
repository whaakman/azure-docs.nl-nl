---
title: "Maken van aangepaste installatiekopieën voor virtuele machine met Azure PowerShell | Microsoft Docs"
description: Zelfstudie - maken van een aangepaste VM-installatiekopie met de Azure PowerShell.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/07/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 7001e5df235d65c531b9102f879bde9693c4f853
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a>Maken van een aangepaste installatiekopie van een virtuele machine in Azure met behulp van PowerShell

Aangepaste installatiekopieën zijn zoals marketplace-installatiekopieën, maar u deze zelf maken. Aangepaste installatiekopieën kunnen worden gebruikt voor de bootstrap configuraties, zoals het vooraf laden van toepassingen, toepassingsconfiguraties en andere configuraties OS. In deze zelfstudie maakt u uw eigen aangepaste installatiekopie van een virtuele machine van Azure. Procedures voor:

> [!div class="checklist"]
> * Sysprep en generalize van virtuele machines
> * Een aangepaste installatiekopie maken
> * Een virtuele machine van een aangepaste installatiekopie maken
> * Lijst van alle installatiekopieën in uw abonnement
> * Een afbeelding verwijderen

Voor deze zelfstudie is moduleversie 3,6 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Voordat u begint

De onderstaande stappen worden in detail beschreven hoe moet worden overgenomen van een bestaande virtuele machine en schakelt u deze in een herbruikbare aangepaste installatiekopie die u gebruiken kunt om nieuwe VM-exemplaren te maken.

Als u het voorbeeld in deze zelfstudie, moet u een bestaande virtuele machine hebben. Indien nodig, dit [voorbeeldscript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) kunt maken voor u. Wanneer het uitvoeren van de zelfstudie vervangt benoemt de resourcegroep en de virtuele machine waar nodig.

## <a name="prepare-vm"></a>Virtuele machine voorbereiden

Voor het maken van een installatiekopie van een virtuele machine, moet u de virtuele machine voorbereiden door het generaliseren van de virtuele machine, toewijzing en vervolgens de bron-VM als gegeneraliseerd in Azure te markeren.

### <a name="generalize-the-windows-vm-using-sysprep"></a>De virtuele machine van Windows met behulp van Sysprep generalize

Sysprep verwijdert alle persoonlijke gegevens over uw account, onder andere en voorbereiden van de machine moet worden gebruikt als een afbeelding. Zie voor meer informatie over Sysprep [hoe gebruik Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).


1. Verbinding maken met de virtuele machine.
2. Open het venster opdrachtprompt als beheerder. Wijzig de map in *%windir%\system32\sysprep*, en voer vervolgens *sysprep.exe*.
3. In de **hulpprogramma voor systeemvoorbereiding** dialoogvenster, *System Voer Out-of-Box Experience (OOBE)*, en zorg ervoor dat de *Generalize* selectievakje is ingeschakeld.
4. In **afsluitopties**, selecteer *afsluiten* en klik vervolgens op **OK**.
5. Wanneer Sysprep is voltooid, afgesloten de virtuele machine. **De virtuele machine niet opnieuw**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Toewijzing ongedaan maken en de virtuele machine niet markeren als gegeneraliseerd

Voor het maken van een installatiekopie, moet de virtuele machine worden toewijzing ongedaan gemaakt en is gemarkeerd als gegeneraliseerd in Azure.

De virtuele machine met behulp van de toewijzing opgeheven [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Force
```

Zet de status van de virtuele machine `-Generalized` met [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm). 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Generalized
```


## <a name="create-the-image"></a>De installatiekopie maken

Nu u een installatiekopie van de virtuele machine maken met behulp van kunt [nieuw AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) en [nieuw AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). Het volgende voorbeeld wordt een installatiekopie met de naam *myImage* van een virtuele machine met de naam *myVM*.

Zorg dat de virtuele machine. 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
```

Maak de configuratie van de installatiekopie.

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Maken van de installatiekopie.

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Virtuele machines van de installatiekopie maken

Nu dat u een installatiekopie hebt, kunt u een of meer nieuwe virtuele machines kunt maken van de installatiekopie. Maken van een virtuele machine van een aangepaste installatiekopie is vergelijkbaar met het maken van een virtuele machine met behulp van een Marketplace-installatiekopie. Wanneer u een Marketplace-installatiekopie gebruikt, hebt u de informatie over de installatiekopie, afbeeldingenprovider, aanbieding, SKU en versie. Met een aangepaste installatiekopie moet u alleen de ID van de aangepaste Afbeeldingsbron opgeven. 

In het volgende script maken we een variabele *$image* voor het opslaan van informatie over het gebruik van de aangepaste installatiekopie [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) en vervolgens gebruiken we [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) en geeft u de ID met de *$image* variabele we zojuist hebben gemaakt. 

Het script maakt een virtuele machine met de naam *myVMfromImage* van onze aangepaste installatiekopie in een nieuwe resourcegroep met de naam *myResourceGroupFromImage* in de *VS-West* locatie.


```powershell
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

New-AzureRmResourceGroup -Name myResourceGroupFromImage -Location EastUS

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name "mypublicdns$(Get-Random)" `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4

  $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

  $nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$vmConfig = New-AzureRmVMConfig `
    -VMName myVMfromImage `
    -VMSize Standard_D1 | Set-AzureRmVMOperatingSystem -Windows `
        -ComputerName myComputer `
        -Credential $cred 

# Here is where we create a variable to store information about the image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroup

# Here is where we specify that we want to create the VM from and image and provide the image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a>Beheer van installatiekopieën 

Hier volgen enkele voorbeelden van algemene beheertaken voor de installatiekopie en hoe u uit te voeren met behulp van PowerShell.

Lijst van alle installatiekopieën op naam.

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Een afbeelding verwijderen. Dit voorbeeld wordt verwijderd van de installatiekopie met de naam *myOldImage* van de *myResourceGroup*.

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een aangepaste installatiekopie van de virtuele machine gemaakt. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Sysprep en generalize van virtuele machines
> * Een aangepaste installatiekopie maken
> * Een virtuele machine van een aangepaste installatiekopie maken
> * Lijst van alle installatiekopieën in uw abonnement
> * Een afbeelding verwijderen

Ga naar de volgende zelfstudie voor meer informatie over hoe maximaal beschikbare virtuele machines.

> [!div class="nextstepaction"]
> [Virtuele machines met hoge beschikbaarheid maken](tutorial-availability-sets.md)



