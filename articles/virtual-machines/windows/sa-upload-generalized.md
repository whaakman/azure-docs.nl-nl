---
title: Uploaden van een generalize VHD voor het maken van meerdere virtuele machines in Azure | Microsoft Docs
description: Een gegeneraliseerde VHD uploaden naar Azure storage-account voor het maken van een Windows-VM voor gebruik met het implementatiemodel van Resource Manager.
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
ms.date: 05/18/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: d8c700d7b23f0e86b4a190e3c1f1143d962a8d63
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="upload-a-generalized-vhd-to-azure-to-create-a-new-vm"></a>Een gegeneraliseerde VHD uploaden naar Azure te maken van een nieuwe virtuele machine

In dit onderwerp bevat informatie over het uploaden van een gegeneraliseerde onbeheerde schijf naar een opslagaccount en maak vervolgens een nieuwe virtuele machine met behulp van de geüploade schijf. Een algemene VHD-installatiekopie heeft al uw persoonlijke accountgegevens verwijderd met behulp van Sysprep. 

Als u maken van een virtuele machine vanaf een speciale VHD in een opslagaccount wilt, Zie [een virtuele machine maken vanaf een speciale VHD](sa-create-vm-specialized.md).

In dit onderwerp worden met behulp van storage-accounts, maar we raden klanten verplaatsen naar beheerd schijven te gebruiken. Zie voor een volledig overzicht van het voorbereiden, uploaden en een nieuwe virtuele machine maken met beheerde schijven [Maak een nieuwe virtuele machine vanaf een gegeneraliseerde VHD geüpload naar Azure met behulp van schijven beheerd](upload-generalized-managed.md).



## <a name="prepare-the-vm"></a>De virtuele machine voorbereiden

Een gegeneraliseerde VHD heeft al uw persoonlijke accountgegevens verwijderd met behulp van Sysprep. Als u van plan bent om de VHD te maken van nieuwe virtuele machines van als afbeelding, moet u het volgende doen:
  
  * [Voorbereiden van een Windows-VHD te uploaden naar Azure](prepare-for-upload-vhd-image.md). 
  * De virtuele machine met behulp van Sysprep generalize

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Een virtuele Windows-machine met behulp van Sysprep generalize
In deze sectie leest u hoe uw Windows-machine voor gebruik als een installatiekopie te generaliseren. Sysprep verwijdert alle persoonlijke gegevens over uw account, onder andere en voorbereiden van de machine moet worden gebruikt als een afbeelding. Zie voor meer informatie over Sysprep [hoe gebruik Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).

Zorg ervoor dat de serverfuncties die op de computer uitgevoerd worden ondersteund door Sysprep. Zie voor meer informatie [Sysprep-ondersteuning voor serverfuncties](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Als u Sysprep voordat u uw VHD uploadt naar Azure voor het eerst uitvoert, controleert u of u hebt [uw virtuele machine voorbereid](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voordat Sysprep wordt uitgevoerd. 
> 
> 

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


## <a name="upload-the-vhd"></a>De VHD te uploaden

De VHD uploaden naar Azure storage-account.

### <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Als er geen PowerShell-versie 1.4 al of hoger is geïnstalleerd, lezen [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

1. Open Azure PowerShell en meld u bij uw Azure-account. Een pop-upvenster wordt geopend voor u de referenties van uw Azure-account in te voeren.
   
    ```powershell
    Login-AzureRmAccount
    ```
2. Het abonnement-id's voor uw beschikbare abonnementen niet ophalen.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Instellen van het juiste abonnement met de abonnement-ID. Vervang `<subscriptionID>` met de ID van het juiste abonnement.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-the-storage-account"></a>Het storage-account ophalen
U moet een opslagaccount in Azure voor het opslaan van de installatiekopie van het geüploade VM. U kunt een bestaand opslagaccount gebruiken of een nieuwe maken. 

Als u wilt de beschikbare opslagruimte accounts weergeven, typt u:

```powershell
Get-AzureRmStorageAccount
```

Als u een bestaand opslagaccount gebruiken wilt, gaat u verder met de [de VM-installatiekopie uploaden](#upload-the-vm-vhd-to-your-storage-account) sectie.

Als u wilt maken van een opslagaccount, als volgt te werk:

1. U moet de naam van de resourcegroep waar het storage-account moet worden gemaakt. Voor meer informatie over de resourcegroepen die zich in uw abonnement, typt u:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Maken van een resourcegroep met de naam **myResourceGroup** in de **VS-West** regio, type:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Maken van een opslagaccount met de naam **mystorageaccount** in deze resourcegroep met behulp van de [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-the-upload"></a>Het uploaden van starten 

Gebruik de [toevoegen AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet voor het uploaden van de installatiekopie naar een container in uw opslagaccount. In dit voorbeeld wordt het bestand geüpload **myVHD.vhd** van `"C:\Users\Public\Documents\Virtual hard disks\"` om een opslagaccount met de naam **mystorageaccount** in de **myResourceGroup** resourcegroep. Het bestand worden opgenomen in de container met de naam **mycontainer** en worden de nieuwe bestandsnaam **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Als dit lukt, kunt u krijgen een antwoord dat ziet er ongeveer als volgt:

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

Afhankelijk van uw netwerkverbinding en de grootte van de VHD-bestand kan met deze opdracht duren om te voltooien.


## <a name="create-a-new-vm"></a>Een nieuwe virtuele machine maken 

U kunt nu de geüploade VHD gebruiken voor het maken van een nieuwe virtuele machine. 

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
De volgende PowerShell-script laat zien hoe de configuraties van virtuele machines instellen en gebruiken van de installatiekopie van het geüploade VM als de bron voor de nieuwe installatie.



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

## <a name="verify-that-the-vm-was-created"></a>Controleren of de virtuele machine is gemaakt
Als u klaar is, ziet u de zojuist gemaakte virtuele machine in de [Azure-portal](https://portal.azure.com) onder **Bladeren** > **virtuele machines**, of met behulp van de volgende PowerShell-opdrachten:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Volgende stappen
Zie voor het beheren van uw nieuwe virtuele machine met Azure PowerShell [virtuele machines beheren met Azure Resource Manager en PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


