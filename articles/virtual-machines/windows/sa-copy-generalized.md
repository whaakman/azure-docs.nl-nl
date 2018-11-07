---
title: Maken van een niet-beheerde installatiekopie van een gegeneraliseerde VM in Azure | Microsoft Docs
description: Een installatiekopie unmanged van een gegeneraliseerde VM Windows gebruiken om te maken van meerdere exemplaren van een virtuele machine in Azure maken.
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
ms.date: 05/23/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 63fdf9cf24c7e412533f15ff0701bc8fb481602a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240610"
---
# <a name="how-to-create-an-unmanaged-vm-image-from-an-azure-vm"></a>Een niet-beheerde VM-installatiekopie maken van een Azure-VM

In dit artikel bevat informatie over opslagaccounts. U wordt aangeraden dat u beheerde schijven en beheerde installatiekopieën in plaats van een storage-account gebruiken. Zie voor meer informatie, [een beheerde installatiekopie maken van een gegeneraliseerde VM in Azure](capture-image-resource.md).

In dit artikel ziet u hoe u Azure PowerShell gebruiken om een installatiekopie van een gegeneraliseerde Azure-VM te maken met behulp van een storage-account. U kunt vervolgens de installatiekopie van het maken van een andere virtuele machine. De afbeelding bevat een schijf met het besturingssysteem en de gegevensschijven die zijn gekoppeld aan de virtuele machine. De installatiekopie bevat geen de virtuele-netwerkbronnen, dus u deze resources instellen moet wanneer u de nieuwe virtuele machine maakt. 

## <a name="prerequisites"></a>Vereisten
U moet beschikken over Azure PowerShell-versie 1.0.x of hoger zijn geïnstalleerd. Als u PowerShell nog niet hebt gedaan, leest u [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview) voor de installatiestappen.

## <a name="generalize-the-vm"></a>De virtuele machine generaliseren 
Deze sectie leest u hoe u uw Windows virtuele machine voor gebruik als een installatiekopie te generaliseren. Een virtuele machine generaliseren, verwijdert u alle persoonlijke gegevens over uw account, onder andere en bereidt de computer moet worden gebruikt als een afbeelding. Raadpleeg [Sysprep gebruiken: een inleiding](https://technet.microsoft.com/library/bb457073.aspx) voor meer informatie over Sysprep.

Zorg ervoor dat de server-functies die worden uitgevoerd op de machine worden ondersteund door Sysprep. Zie voor meer informatie, [Sysprep-ondersteuning voor Server-rollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Als u uw VHD naar Azure voor het eerst uploaden wilt, zorg ervoor dat u hebt [uw virtuele machine voorbereid](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voordat Sysprep wordt uitgevoerd. 
> 
> 

U kunt ook een Linux VM generaliseren met `sudo waagent -deprovision+user` en gebruikt u PowerShell om vast te leggen van de virtuele machine. Zie voor meer informatie over het gebruik van de CLI om vast te leggen van een virtuele machine [het generaliseren en vastleggen van een virtuele Linux-machine met de Azure CLI ](../linux/capture-image.md).


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

## <a name="log-in-to-azure-powershell"></a>Aanmelden bij Azure PowerShell
1. Open Azure PowerShell en meld u aan bij uw Azure-account.
   
    ```powershell
    Connect-AzureRmAccount
    ```
   
    Er wordt een pop-upvenster geopend voor u de referenties van uw Azure-account in te voeren.
2. Haal de abonnement-id's voor uw beschikbare abonnementen.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Instellen van het juiste abonnement met behulp van de abonnements-ID.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Wijs de virtuele machine en stelt u de status op gegeneraliseerd

> [!IMPORTANT] 
> U kan toevoegen, bewerken of verwijderen van tags van een virtuele machine nadat deze is gemarkeerd als gegeneraliseerd. Als u een code toevoegen aan de virtuele machine wilt, zorg er dan voor dat u de labels toevoegen voordat u gemarkeerd als gegeneraliseerd.
> 

1. Toewijzing van de VM-resources.
   
    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    De *Status* voor de virtuele machine in Azure portal, verandert van **gestopt** naar **gestopt (toewijzing opgeheven)**.
2. Zet de status van de virtuele machine **gegeneraliseerd**. 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. Controleer de status van de virtuele machine. De **OSState/gegeneraliseerd** sectie voor de virtuele machine moet de **DisplayStatus** ingesteld op **VM gegeneraliseerd**.  
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>De installatiekopie maken

Maak een installatiekopie van een niet-beheerde virtuele machine in de doel-storage-container met de volgende opdracht. De installatiekopie is gemaakt in hetzelfde opslagaccount als de oorspronkelijke virtuele machine. De `-Path` parameter slaat een kopie van de JSON-sjabloon voor de bron-VM naar uw lokale computer. De `-DestinationContainerName` parameter is de naam van de container die u wilt uw installatiekopieën bevatten. Als de container niet bestaat, is het voor u gemaakt.
   
```powershell
Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
U kunt de URL van uw installatiekopie ophalen uit de JSON-bestand-sjabloon. Ga naar de **resources** > **storageProfile** > **osDisk** > **installatiekopie**  >  **uri** sectie voor het volledige pad van de afbeelding. De URL van de installatiekopie lijkt: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
   
U kunt ook de URI in de portal controleren. De afbeelding wordt gekopieerd naar een container met de naam **system** in uw opslagaccount. 

## <a name="create-a-vm-from-the-image"></a>Een virtuele machine maken van de installatiekopie

U kunt nu een of meer virtuele machines van de niet-beheerde installatiekopie maken.

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
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Maak het virtuele netwerk. Het volgende voorbeeld wordt een virtueel netwerk met de naam **myVnet** in de **VS-West** locatie met het adresvoorvoegsel van **10.0.0.0/16**.  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Een openbare IP-adres en een netwerkinterface maken
Om te kunnen communiceren met de virtuele machine in het virtuele netwerk, hebt u een [openbaar IP-adres](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface nodig.

1. Maak een openbaar IP-adres. In dit voorbeeld maakt u een openbaar IP-adres met de naam **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Maken van de NIC. Dit voorbeeld maakt u een NIC met de naam **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>De netwerkbeveiligingsgroep en een RDP-regel maken
Als u zich aanmelden bij uw virtuele machine via RDP, moet u hebt een beveiligingsregel om RDP-toegang op poort 3389. 

Dit voorbeeld maakt u een NSG met de naam **myNsg** dat een regel met de naam bevat **myRdpRule** die RDP-verkeer toestaat via poort 3389. Zie voor meer informatie over nsg's [poorten openen voor een virtuele machine in Azure met behulp van PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a>Maak een variabele voor het virtuele netwerk
Maak een variabele voor de voltooide virtuele netwerk. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-the-vm"></a>De virtuele machine maken
De volgende PowerShell is voltooid de configuraties van virtuele machines en maakt gebruik van niet-beheerde installatiekopie als bron voor de nieuwe installatie.

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
Als u klaar bent, ziet u de zojuist gemaakte virtuele machine in de [Azure-portal](https://portal.azure.com) onder **Bladeren** > **virtuele machines**, of met behulp van de volgende PowerShell opdrachten:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Volgende stappen
Zie voor het beheren van uw nieuwe virtuele machine met Azure PowerShell, [virtuele machines beheren met Azure Resource Manager en PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


