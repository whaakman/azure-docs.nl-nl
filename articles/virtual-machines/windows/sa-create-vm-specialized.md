---
title: Virtuele machine maken vanaf een gespecialiseerde schijf in Azure | Microsoft Docs
description: Maak een nieuwe virtuele machine door een gespecialiseerde niet-beheerde schijf, in het Resource Manager-implementatiemodel te koppelen.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 3465a891d91bc54bfe39b9501a7ad0e970932860
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56309621"
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a>Een virtuele machine maken vanaf een gespecialiseerde VHD in een storage-account

Maak een nieuwe virtuele machine door het koppelen van een gespecialiseerde niet-beheerde schijf als de besturingssysteemschijf met behulp van Powershell. Een gespecialiseerde schijf is een kopie van de VHD van een bestaande virtuele machine die wordt onderhouden door de gebruikersaccounts, toepassingen en andere statusgegevens van de oorspronkelijke virtuele machine. 

U hebt hiervoor twee opties:
* [Een VHD uploaden](sa-create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [Kopieer de VHD van een bestaande VM in Azure](sa-create-vm-specialized.md#option-2-copy-the-vhd-from-an-existing-azure-vm)

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]


## <a name="option-1-upload-a-specialized-vhd"></a>Optie 1: Een gespecialiseerde VHD uploaden

U kunt de VHD van een gespecialiseerde virtuele machine gemaakt met een on-premises virtualisatie hulpprogramma, zoals Hyper-V, of een virtuele machine die zijn geëxporteerd uit een andere cloud uploaden.

### <a name="prepare-the-vm"></a>De virtuele machine voorbereiden
U kunt een gespecialiseerde VHD die is gemaakt met behulp van een on-premises virtuele machine of een VHD die is geëxporteerd uit een andere cloud uploaden. Een gespecialiseerde VHD onderhoudt de gebruikersaccounts, toepassingen en andere statusgegevens van de oorspronkelijke virtuele machine. Als u van plan bent de VHD als gebruiken-is een nieuwe virtuele machine maakt, zorg ervoor dat de volgende stappen zijn voltooid. 
  
  * [Voorbereiden van een Windows-VHD te uploaden naar Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Geen** generaliseren van de virtuele machine met behulp van Sysprep.
  * Verwijder alle Gast virtualisatie-hulpprogramma's en de agents die zijn geïnstalleerd op de virtuele machine (dat wil zeggen VMware-hulpprogramma's genoemd).
  * Zorg ervoor dat de virtuele machine is geconfigureerd voor het ophalen van de IP-adres en DNS-instellingen via DHCP. Dit zorgt ervoor dat de server een IP-adres binnen het VNet wordt wanneer deze opgestart wordt. 


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
   
### <a name="upload-the-vhd-to-your-storage-account"></a>De VHD uploaden naar uw storage-account
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


## <a name="option-2-copy-the-vhd-from-an-existing-azure-vm"></a>Optie 2: Kopieer de VHD van een bestaande VM in Azure

U kunt een VHD kopiëren naar een ander opslagaccount te gebruiken bij het maken van een nieuwe, dubbele virtuele machine.

### <a name="before-you-begin"></a>Voordat u begint
Zorg ervoor dat u:

* Hebt u informatie over de **bron- en storage-accounts**. Voor de bron-VM moet u de namen van de storage-account en een container hebt. Normaal gesproken de containernaam is **VHD's**. U moet ook een doel-opslagaccount hebben. Als u dit niet al hebt, kunt u een met behulp van de portal maken (**alle Services** > opslagaccounts > toevoegen) of met behulp van de [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) cmdlet. 
* Hebt gedownload en geïnstalleerd de [AzCopy-hulpprogramma](../../storage/common/storage-use-azcopy.md). 

### <a name="deallocate-the-vm"></a>Toewijzing van de virtuele machine ongedaan maken
Wijs de virtuele machine, zodat u de VHD moet worden gekopieerd. 

* **Portal**: Klik op **virtuele machines** > **myVM** > stoppen
* **Powershell**: Gebruik [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) stoppen (toewijzing ongedaan maken) de virtuele machine met de naam **myVM** in resourcegroep **myResourceGroup**.

```powershell
Stop-AzVM -ResourceGroupName myResourceGroup -Name myVM
```

De **Status** voor de virtuele machine in Azure portal, verandert van **gestopt** naar **gestopt (toewijzing opgeheven)**.

### <a name="get-the-storage-account-urls"></a>De storage-account-URL's ophalen
U moet de URL's van de bron- en storage-accounts. De URL's eruit: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Als u de naam van het storage-account en de container al kent, kunt u alleen de gegevens tussen de haakjes te maken van uw URL vervangen. 

U kunt de Azure portal of Azure Powershell gebruiken om de URL te krijgen:

* **Portal**: Klik op de **>** voor **alle services** > **opslagaccounts** > *opslagaccount*  >  **Blobs** en uw bron-VHD-bestand is waarschijnlijk in de **VHD's** container. Klik op **eigenschappen** voor de container en kopieer de tekst met het label **URL**. U moet de URL's van zowel de bron- als containers. 
* **Powershell**: Gebruik [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) om op te halen van de gegevens voor de virtuele machine met de naam **myVM** in de resourcegroep **myResourceGroup**. In de resultaten bekijken de **opslagprofiel** sectie voor de **Vhd-Uri**. Het eerste deel van de Uri is de URL naar de container en het laatste deel is de naam van de OS VHD voor de virtuele machine.

```powershell
Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-the-storage-access-keys"></a>Ophalen van de toegangssleutels voor opslag
De toegangssleutel voor de bron- en storage-accounts vinden. Zie voor meer informatie over toegang tot sleutels, [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md).

* **Portal**: Klik op **alle services** > **opslagaccounts** > *opslagaccount* > **toegangssleutels**. Kopieer de sleutel met het label **key1**.
* **Powershell**: Gebruik [Get-AzStorageAccountKey](https://docs.microsoft.com/powershell/module/az.storage/get-azstorageaccountkey) om op te halen van de toegangssleutel voor het opslagaccount **mystorageaccount** in de resourcegroep **myResourceGroup**. Kopieer de sleutel met het label **key1**.

```powershell
Get-AzStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-the-vhd"></a>Kopieer de VHD
U kunt bestanden kopiëren tussen opslagaccounts met behulp van AzCopy. Voor de doelcontainer als de opgegeven container niet bestaat, zal deze worden voor u gemaakt. 

Voor het gebruik van AzCopy, open een opdrachtprompt op uw lokale computer en navigeer naar de map waarin AzCopy wordt geïnstalleerd. Dit is vergelijkbaar met *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

Alle bestanden in een container wilt kopiëren, gebruikt u de **/S** overschakelen. Dit kan worden gebruikt om de VHD met het besturingssysteem en alle van de gegevensschijf kopiëren als ze zich in dezelfde container. In dit voorbeeld laat zien hoe kopieer alle bestanden in de container **mysourcecontainer** in storage-account **mysourcestorageaccount** naar de container **mydestinationcontainer**in de **mydestinationstorageaccount** storage-account. De namen van de storage-accounts en de containers vervangen door uw eigen. Vervang `<sourceStorageAccountKey1>` en `<destinationStorageAccountKey1>` met uw eigen sleutels.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Als u alleen kopiëren van een specifieke VHD in een container met meerdere bestanden wilt, kunt u ook de naam van het bestand met de schakeloptie /Pattern opgeven. In dit voorbeeld wordt alleen het bestand met de naam **myFileName.vhd** moeten worden gekopieerd.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


Wanneer deze is voltooid, ontvangt u een bericht dat er ongeveer als volgt:

```
Finished 2 of total 2 file(s).
[2016/10/07 17:37:41] Transfer summary:
-----------------
Total files transferred: 2
Transfer successfully:   2
Transfer skipped:        0
Transfer failed:         0
Elapsed time:            00.00:13:07
```

### <a name="troubleshooting"></a>Problemen oplossen
* Wanneer u AZCopy gebruikt, als u ziet dat de fout 'Server kan niet verifiëren van de aanvraag', zorg ervoor dat de waarde van de autorisatie-header heeft een onjuiste indeling correct met inbegrip van de handtekening. Als u van sleutel 2 of de secundaire toegangssleutel gebruikmaakt, probeert u met behulp van de primaire of 1e opslagsleutel.

## <a name="create-the-new-vm"></a>De nieuwe virtuele machine maken 

U moet maken van netwerken en andere VM-resources kunnen worden gebruikt door de nieuwe virtuele machine.

### <a name="create-the-subnet-and-vnet"></a>Het subNet en een vNet maken

Maak de vNet en subNet van de [virtueel netwerk](../../virtual-network/virtual-networks-overview.md).

1. Maak het subNet. Dit voorbeeld maakt u een subnet met de naam **mySubNet**, in de resourcegroep **myResourceGroup**, en stelt u het adres subnetvoorvoegsel op **10.0.0.0/24**.
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Maak het vNet. In dit voorbeeld wordt de naam van het virtuele netwerk moet **myVnetName**, de locatie voor het **VS-West**, en het adresvoorvoegsel voor het virtuele netwerk naar **10.0.0.0/16**. 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
### <a name="create-the-network-security-group-and-an-rdp-rule"></a>De netwerkbeveiligingsgroep en een RDP-regel maken
Als u zich aanmelden bij uw virtuele machine via RDP, moet u hebt een beveiligingsregel om RDP-toegang op poort 3389. Omdat de VHD voor de nieuwe virtuele machine is gemaakt op basis van een bestaande kunt gespecialiseerde VM, nadat de virtuele machine u maakt een bestaand account van de virtuele bronmachine met machtiging voor aanmelding met behulp van RDP gebruiken.
Dit moet worden uitgevoerd vóór het maken van de netwerkinterface die zal worden gekoppeld.  
In het volgende voorbeeld wordt de naam van de NSG op **myNsg** en de naam van de RDP-regel op **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Zie voor meer informatie over eindpunten en NSG-regels, [poorten openen voor een virtuele machine in Azure met behulp van PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Maak een openbaar IP-adres en de NIC
Om te kunnen communiceren met de virtuele machine in het virtuele netwerk, hebt u een [openbaar IP-adres](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface nodig.

1. Het openbare IP-adres maken. In dit voorbeeld wordt de naam van het openbare IP-adres is ingesteld op **myIP**.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Maken van de NIC. In dit voorbeeld wordt de naam van de NIC is ingesteld op **myNicName**. Deze stap wordt ook gekoppeld aan de Netwerkbeveiligingsgroep die eerder hebt gemaakt met deze NIC wordt gebruikt.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
    ```

### <a name="set-the-vm-name-and-size"></a>De VM-naam en grootte instellen

Dit voorbeeld wordt de naam van de VM naar 'myVM' en de grootte van de virtuele machine op 'Standard_A2'.
```powershell
$vmName = "myVM"
$vmConfig = New-AzVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>De NIC toevoegen
    
```powershell
$vm = Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-the-os-disk"></a>Configureren van de besturingssysteemschijf

1. Stel de URI voor de VHD die u hebt geüpload of gekopieerd. In dit voorbeeld wordt het VHD-bestand met de naam **myOsDisk.vhd** wordt opgeslagen in een opslagaccount met de naam **myStorageAccount** in een container met de naam **myContainer**.

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. De OS-schijf toevoegen. Als de besturingssysteemschijf is gemaakt, is de term 'osDisk' in dit voorbeeld appened op de naam van de VM te maken van de naam van de OS-schijf. In dit voorbeeld geeft u ook dat deze op basis van een Windows VHD moet worden gekoppeld aan de virtuele machine als de besturingssysteemschijf.
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

Optioneel: Als u de gegevensschijven die moeten worden gekoppeld aan de virtuele machine hebt, moet u de gegevensschijven toevoegen met behulp van de URL's van gegevens-VHD's en de juiste Logical Unit Number (Lun).

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

Wanneer u een opslagaccount, de gegevens en besturingssysteem schijf-URL's er ongeveer als volgt uit: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. U kunt dit vinden in de portal door te bladeren naar de doel-opslagcontainer, klikt u op het besturingssysteem of de gegevens-VHD die is gekopieerd en vervolgens de inhoud van de URL te kopiëren.


### <a name="complete-the-vm"></a>De virtuele machine voltooien 

De virtuele machine maken met de configuraties die we zojuist hebben gemaakt.

```powershell
#Create the new VM
New-AzVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Als deze opdracht voltooid is, ziet u uitvoer zoals de volgende:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>Controleren of de virtuele machine is gemaakt
U ziet de zojuist gemaakte virtuele machine ofwel in het [Azure-portal](https://portal.azure.com)onder **alle services** > **virtuele machines**, of met behulp van de volgende PowerShell opdrachten:

```powershell
$vmList = Get-AzVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Volgende stappen
Aanmelden bij uw nieuwe virtuele machine. Zie voor meer informatie, [hoe u verbinding maken met en meld u aan een virtuele Azure-machine waarop Windows wordt uitgevoerd bij](connect-logon.md).

