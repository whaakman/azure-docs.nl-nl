---
title: Maken van een installatiekopie van een niet-beheerde van een gegeneraliseerde virtuele machine in Azure | Microsoft Docs
description: Een unmanged installatiekopie maken van een algemene Windows-VM voor gebruik van meerdere exemplaren van een virtuele machine maken in Azure.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 39ac47df65743dc807b060f34a6df16977ef49a1
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="how-to-create-an-unmanaged-vm-image-from-an-azure-vm"></a>Het maken van een niet-beheerde VM-installatiekopie van een Azure VM

In dit artikel bevat informatie over met behulp van storage-accounts. Het is raadzaam dat u beheerde schijven en beheerde installatiekopieën in plaats van een opslagaccount gebruiken. Zie voor meer informatie [beheerde-installatiekopie van een gegeneraliseerde virtuele machine in Azure](capture-image-resource.md).

In dit artikel leest u hoe Azure PowerShell gebruiken om te maken van een installatiekopie van een gegeneraliseerde virtuele machine in Azure met behulp van een opslagaccount. U kunt vervolgens de installatiekopie van het maken van een andere virtuele machine. De installatiekopie bevat de besturingssysteemschijf en de gegevensschijven die zijn gekoppeld aan de virtuele machine. De afbeelding bevat geen virtuele netwerkbronnen, dus u deze resources instellen moet bij het maken van de nieuwe virtuele machine. 

## <a name="prerequisites"></a>Vereisten
U moet Azure PowerShell versie 1.0.x of hoger zijn geïnstalleerd. Als u nog niet PowerShell hebt geïnstalleerd, leest u [installeren en configureren van Azure PowerShell](/powershell/azure/overview) voor de installatiestappen.

## <a name="generalize-the-vm"></a>De virtuele machine generalize 
In deze sectie leest u hoe uw Windows-machine voor gebruik als een installatiekopie te generaliseren. Het generaliseren van een virtuele machine verwijdert alle persoonlijke gegevens over uw account, onder andere en bereidt de machine moet worden gebruikt als een afbeelding. Zie voor meer informatie over Sysprep [hoe gebruik Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).

Zorg ervoor dat de serverfuncties die op de computer uitgevoerd worden ondersteund door Sysprep. Zie voor meer informatie [Sysprep-ondersteuning voor serverfuncties](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Als u uw VHD naar Azure voor de eerste keer uploaden wilt, controleert u of u hebt [uw virtuele machine voorbereid](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voordat Sysprep wordt uitgevoerd. 
> 
> 

U kunt ook een Linux-VM met generalize `sudo waagent -deprovision+user` en gebruik vervolgens PowerShell om vast te leggen van de virtuele machine. Zie voor meer informatie over het gebruik van de CLI voor het vastleggen van een virtuele machine [generaliseren en vastleggen van een virtuele Linux-machine met de Azure CLI ](../linux/capture-image.md).


1. Meld u aan de virtuele machine van Windows.
2. Open het venster opdrachtprompt als beheerder. Wijzig de map in **%windir%\system32\sysprep**, en voer vervolgens `sysprep.exe`.
3. In de **hulpprogramma voor systeemvoorbereiding** dialoogvenster, **System Voer Out-of-Box Experience (OOBE)**, en zorg ervoor dat de **Generalize** selectievakje is ingeschakeld.
4. In **afsluitopties**, selecteer **afsluiten**.
5. Klik op **OK**.
   
    ![Sysprep starten](./media/upload-generalized-managed/sysprepgeneral.png)
6. Wanneer Sysprep is voltooid, afgesloten de virtuele machine. 

> [!IMPORTANT]
> De virtuele machine niet opnieuw opstarten totdat u klaar bent met de VHD te uploaden naar Azure of een installatiekopie van het maken van de virtuele machine. Als de virtuele machine per ongeluk wordt opnieuw gestart met Sysprep om het opnieuw.
> 
> 

## <a name="log-in-to-azure-powershell"></a>Aanmelden bij Azure PowerShell
1. Open Azure PowerShell en meld u bij uw Azure-account.
   
    ```powershell
    Login-AzureRmAccount
    ```
   
    Een pop-upvenster wordt geopend voor u de referenties van uw Azure-account in te voeren.
2. Het abonnement-id's voor uw beschikbare abonnementen niet ophalen.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Instellen van het juiste abonnement met de abonnement-ID.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Toewijzing van de virtuele machine en de status in gegeneraliseerd instellen
1. Toewijzing van de VM-netwerkbronnen.
   
    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    De *Status* voor de virtuele machine in Azure portal wordt gewijzigd van **gestopt** naar **gestopt (toewijzing opgeheven)**.
2. Zet de status van de virtuele machine **gegeneraliseerd**. 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. Controleer de status van de virtuele machine. De **OSState/gegeneraliseerd** sectie voor de virtuele machine moet de **DisplayStatus** ingesteld op **VM is gegeneraliseerd**.  
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>De installatiekopie maken

Maak een installatiekopie van een niet-beheerde virtuele machine in de doel-storage-container met de volgende opdracht. De installatiekopie wordt gemaakt in hetzelfde opslagaccount als de oorspronkelijke virtuele machine. De `-Path` parameter een kopie van de JSON-sjabloon voor de bron-VM opslaan op uw lokale computer. De `-DestinationContainerName` parameter is de naam van de container die u wilt uw installatiekopieën bevatten. Als de container niet bestaat, wordt deze voor u gemaakt.
   
```powershell
Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
De URL van uw installatiekopie kunt u krijgen via de sjabloon JSON-bestand. Ga naar de **resources** > **storageProfile** > **osDisk** > **installatiekopie** > **uri** sectie voor het volledige pad van de afbeelding. De URL van de afbeelding eruitziet: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
   
U kunt ook controleren of de URI in de portal. De installatiekopie wordt gekopieerd naar een container met de naam **system** in uw opslagaccount. 

## <a name="create-a-vm-from-the-image"></a>Een virtuele machine van de installatiekopie maken

Nu kunt u een of meer virtuele machines van de installatiekopie van het niet-beheerd.

### <a name="set-the-uri-of-the-vhd"></a>De URI van de VHD instellen

De URI voor de VHD moet worden gebruikt, is in de indeling: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**VHD. In dit voorbeeld de VHD met de naam **myVHD** is in het opslagaccount **mystorageaccount** in de container **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
Maken van het vNet en het subnet van de [virtueel netwerk](../../virtual-network/virtual-networks-overview.md).

1. Het subnet maken. Het volgende voorbeeld maakt u een subnet met de naam **mySubnet** in de resourcegroep **myResourceGroup** met het adresvoorvoegsel van **10.0.0.0/24**.  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Maak het virtuele netwerk. Het volgende voorbeeld maakt u een virtueel netwerk met de naam **myVnet** in de **VS-West** locatie met het adresvoorvoegsel van **10.0.0.0/16**.  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Een openbaar IP-adres en een netwerkinterface maken
Om te kunnen communiceren met de virtuele machine in het virtuele netwerk, hebt u een [openbaar IP-adres](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface nodig.

1. Een openbaar IP-adres maken. In dit voorbeeld wordt een openbaar IP-adres met de naam **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Maken van de NIC. In dit voorbeeld wordt een NIC met de naam **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>De netwerkbeveiligingsgroep en een RDP-regel maken
Als u zich aanmelden bij uw virtuele machine met RDP, moet u een regel waarmee op poort 3389 van RDP-toegang hebben. 

In dit voorbeeld maakt u een NSG met de naam **myNsg** die een regel aangeroepen bevat **myRdpRule** waarmee RDP-verkeer via poort 3389. Zie voor meer informatie over Nsg [openen van poorten voor een virtuele machine in Azure met behulp van PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a>Een variabele voor het virtuele netwerk maken
Een variabele voor het voltooide virtueel netwerk maken. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-the-vm"></a>De virtuele machine maken
De volgende PowerShell de configuraties van virtuele machines is voltooid en niet-beheerde installatiekopie gebruikt als bron voor de nieuwe installatie.

</br>

```powershell
    # Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential

    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"

    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"

    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

### <a name="verify-that-the-vm-was-created"></a>Controleren of de virtuele machine is gemaakt
Als u klaar is, ziet u de zojuist gemaakte virtuele machine in de [Azure-portal](https://portal.azure.com) onder **Bladeren** > **virtuele machines**, of met behulp van de volgende PowerShell-opdrachten:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Volgende stappen
Zie voor het beheren van uw nieuwe virtuele machine met Azure PowerShell [virtuele machines beheren met Azure Resource Manager en PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


