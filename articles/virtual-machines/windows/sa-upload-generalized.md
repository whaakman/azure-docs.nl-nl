---
title: Uploaden van een generalize VHD voor het maken van meerdere virtuele machines in Azure | Microsoft Docs
description: Een gegeneraliseerde VHD uploaden naar Azure storage-account te maken van een Windows-VM voor gebruik met het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: a8aa00a3bc74c811d7c57db878df0758aa054bb9
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55978643"
---
# <a name="upload-a-generalized-vhd-to-azure-to-create-a-new-vm"></a>Een gegeneraliseerde VHD uploaden naar Azure te maken van een nieuwe virtuele machine

In dit onderwerp bevat informatie over het uploaden van een gegeneraliseerde niet-beheerde schijf naar een opslagaccount en maak vervolgens een nieuwe virtuele machine met behulp van de geüploade schijf. Een gegeneraliseerde VHD-installatiekopie heeft al uw persoonlijke accountinformatie verwijderd met behulp van Sysprep. 

Als u een virtuele machine maken vanaf een gespecialiseerde VHD in een storage-account wilt, Zie [een virtuele machine maken vanaf een gespecialiseerde VHD](sa-create-vm-specialized.md).

In dit onderwerp bevat informatie over storage-accounts, maar we raden klanten verplaatsen naar Managed Disks te gebruiken. Zie voor een volledige beschrijving van de procedure voorbereiden, uploaden en een nieuwe virtuele machine maken met beheerde schijven, [maken een nieuwe virtuele machine vanaf een gegeneraliseerde VHD uploaden naar Azure met behulp van Managed Disks](upload-generalized-managed.md).

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="prepare-the-vm"></a>De virtuele machine voorbereiden

Een gegeneraliseerde VHD heeft al uw persoonlijke accountinformatie verwijderd met behulp van Sysprep. Als u van plan bent de VHD als een installatiekopie gebruiken om te maken van nieuwe virtuele machines uit, moet u het:
  
  * [Voorbereiden van een Windows-VHD te uploaden naar Azure](prepare-for-upload-vhd-image.md). 
  * Generaliseren met Sysprep de virtuele machine

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Een Windows virtuele machine met behulp van Sysprep generaliseren
Deze sectie leest u hoe u uw Windows virtuele machine voor gebruik als een installatiekopie te generaliseren. Sysprep verwijdert onder meer al uw persoonlijke accountinformatie en de machine wordt voorbereid om als een installatiekopie te worden gebruikt. Zie voor meer informatie over [Sysprep gebruiken: een inleiding](https://technet.microsoft.com/library/bb457073.aspx).

Zorg ervoor dat de server-functies die worden uitgevoerd op de machine worden ondersteund door Sysprep. Zie voor meer informatie, [Sysprep-ondersteuning voor Server-rollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Als u Sysprep voordat u uw VHD uploadt naar Azure voor het eerst uitvoert, zorg ervoor dat u hebt [uw virtuele machine voorbereid](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voordat Sysprep wordt uitgevoerd. 
> 
> 

1. Meld u aan de virtuele machine van Windows.
2. Open het venster met de opdrachtprompt als beheerder. Wijzig de map in **%windir%\system32\sysprep**, en voer `sysprep.exe`.
3. In het dialoogvenster **Hulpprogramma voor systeemvoorbereiding** selecteert u **OOBE (Out-of-Box Experience) van systeem starten** en zorgt u dat het selectievakje **Generaliseren** is ingeschakeld.
4. In **afsluitopties**, selecteer **afsluiten**.
5. Klik op **OK**.
   
    ![Sysprep start](./media/upload-generalized-managed/sysprepgeneral.png)
6. Wanneer Sysprep is voltooid, wordt de virtuele machine afgesloten. 

> [!IMPORTANT]
> De virtuele machine niet opnieuw opstarten totdat u klaar bent de VHD uploaden naar Azure of het maken van een installatiekopie van de virtuele machine. Als de virtuele machine per ongeluk wordt opnieuw gestart met Sysprep om het opnieuw.
> 
> 


## <a name="upload-the-vhd"></a>De VHD uploaden

Upload de VHD naar een Azure storage-account.

### <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Als u nog PowerShell-versie 1.4 hebt of hoger is geïnstalleerd, Lees [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).

1. Open Azure PowerShell en meld u aan bij uw Azure-account. Er wordt een pop-upvenster geopend voor u de referenties van uw Azure-account in te voeren.
   
    ```powershell
    Connect-AzAccount
    ```
2. Haal de abonnement-id's voor uw beschikbare abonnementen.
   
    ```powershell
    Get-AzSubscription
    ```
3. Instellen van het juiste abonnement met behulp van de abonnements-ID. Vervang `<subscriptionID>` met de ID van het juiste abonnement.
   
    ```powershell
    Select-AzSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-the-storage-account"></a>Het opslagaccount ophalen
U moet een opslagaccount in Azure voor het opslaan van de geüploade VM-installatiekopie. U kunt een bestaand opslagaccount gebruiken of een nieuwe maken. 

Als u wilt de beschikbare opslag-accounts weergeven, typt u:

```powershell
Get-AzStorageAccount
```

Als u een bestaand opslagaccount gebruiken wilt, gaat u verder met het uploaden van de sectie VM-installatiekopie.

Als u nodig hebt voor het maken van een storage-account, als volgt te werk:

1. U moet de naam van de resourcegroep waar het opslagaccount dat moet worden gemaakt. Als u wilt weten van alle resourcegroepen in uw abonnement, typt u:
   
    ```powershell
    Get-AzResourceGroup
    ```

    Het maken van een resourcegroep met de naam **myResourceGroup** in de **VS-West** regio, type:

    ```powershell
    New-AzResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Maak een opslagaccount met de naam **mystorageaccount** in deze resourcegroep met behulp van de [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) cmdlet:
   
    ```powershell
    New-AzStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-the-upload"></a>Beginnen met uploaden 

Gebruik de [toevoegen AzVhd](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd) cmdlet om de installatiekopie uploaden naar een container in uw storage-account. In dit voorbeeld wordt het bestand geüpload **myVHD.vhd** van `"C:\Users\Public\Documents\Virtual hard disks\"` naar een opslagaccount met de naam **mystorageaccount** in de **myResourceGroup** resourcegroep. Het bestand worden opgenomen in de container met de naam **mycontainer** en de nieuwe bestandsnaam worden **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Als dit lukt, krijgt u een reactie die er ongeveer als volgt uitziet:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Afhankelijk van uw netwerkverbinding en de grootte van uw VHD-bestand kan met deze opdracht even om te voltooien.


## <a name="create-a-new-vm"></a>Een nieuwe virtuele machine maken 

U kunt nu de geüploade VHD gebruiken om een nieuwe VM te maken. 

### <a name="set-the-uri-of-the-vhd"></a>De URI van de VHD instellen

De URI voor de VHD te gebruiken is in de indeling: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**VHD. In dit voorbeeld de VHD met de naam **myVHD** is in het opslagaccount **mystorageaccount** in de container **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
Maak de vNet en subnet van de [virtueel netwerk](../../virtual-network/virtual-networks-overview.md).

1. Maak het subnet. Het volgende voorbeeld maakt u een subnet met de naam **mySubnet** in de resourcegroep **myResourceGroup** met het adresvoorvoegsel van **10.0.0.0/24**.  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Maak het virtuele netwerk. Het volgende voorbeeld wordt een virtueel netwerk met de naam **myVnet** in de **VS-West** locatie met het adresvoorvoegsel van **10.0.0.0/16**.  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Een openbare IP-adres en een netwerkinterface maken
Om te kunnen communiceren met de virtuele machine in het virtuele netwerk, hebt u een [openbaar IP-adres](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface nodig.

1. Maak een openbaar IP-adres. In dit voorbeeld maakt u een openbaar IP-adres met de naam **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Maken van de NIC. Dit voorbeeld maakt u een NIC met de naam **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>De netwerkbeveiligingsgroep en een RDP-regel maken
Als u zich aanmelden bij uw virtuele machine via RDP, moet u hebt een beveiligingsregel om RDP-toegang op poort 3389. 

Dit voorbeeld maakt u een NSG met de naam **myNsg** dat een regel met de naam bevat **myRdpRule** die RDP-verkeer toestaat via poort 3389. Zie voor meer informatie over nsg's [poorten openen voor een virtuele machine in Azure met behulp van PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a>Maak een variabele voor het virtuele netwerk
Maak een variabele voor de voltooide virtuele netwerk. 

```powershell
$vnet = Get-AzVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-the-vm"></a>De virtuele machine maken
De volgende PowerShell-script laat zien hoe de configuraties van virtuele machines instellen en gebruiken van de geüploade VM-installatiekopie als bron voor de nieuwe installatie.



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
    $storageAcc = Get-AzStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set the VM name and size
    $vmConfig = New-AzVMConfig -VMName $vmName -VMSize $vmSize

    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzVMNetworkInterface -VM $vm -Id $nic.Id

    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create the new VM
    New-AzVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Controleren of de virtuele machine is gemaakt
Als u klaar bent, ziet u de zojuist gemaakte virtuele machine in de [Azure-portal](https://portal.azure.com) onder **Bladeren** > **virtuele machines**, of met behulp van de volgende PowerShell opdrachten:

```powershell
    $vmList = Get-AzVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Volgende stappen
Zie voor het beheren van uw nieuwe virtuele machine met Azure PowerShell, [virtuele machines beheren met Azure Resource Manager en PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


