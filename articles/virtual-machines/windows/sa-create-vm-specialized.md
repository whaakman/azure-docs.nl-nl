---
title: Virtuele machine maken vanaf een speciale schijf in Azure | Microsoft Docs
description: Maak een nieuwe virtuele machine door het koppelen van een niet-beheerde gespecialiseerde schijf, in het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
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
ms.openlocfilehash: e92a9d5900e3e0fe71084e5003010d419e44cb39
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a>Een virtuele machine vanaf een speciale VHD in een opslagaccount maken

Maak een nieuwe virtuele machine door het koppelen van een gespecialiseerde onbeheerde schijf als de OS-schijf met behulp van Powershell. Een speciale schijf is een kopie van de VHD van een bestaande virtuele machine die de gebruikersaccounts, toepassingen en andere statusgegevens van uw oorspronkelijke VM onderhoudt. 

U hebt hiervoor twee opties:
* [Een VHD uploaden](sa-create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [Kopieer de VHD van een bestaande virtuele machine in Azure](sa-create-vm-specialized.md#option-2-copy-an-existing-azure-vm)

## <a name="before-you-begin"></a>Voordat u begint
Als u PowerShell gebruikt, zorg ervoor dat u de nieuwste versie van de AzureRM.Compute PowerShell-module hebt. Voer de volgende opdracht om deze te installeren.

```powershell
Install-Module AzureRM.Compute 
```
Zie voor meer informatie [Azure PowerShell Versioning](/powershell/azure/overview).


## <a name="option-1-upload-a-specialized-vhd"></a>Optie 1: Een gespecialiseerde VHD uploaden

U kunt de VHD van een speciale virtuele machine gemaakt met een lokale virtualisatie hulpprogramma, zoals Hyper-V of een virtuele machine die zijn geëxporteerd uit een andere cloud uploaden.

### <a name="prepare-the-vm"></a>De virtuele machine voorbereiden
U kunt een gespecialiseerde VHD die is gemaakt met een lokale virtuele machine of een VHD die is geëxporteerd uit een andere cloud uploaden. Een speciale VHD houdt de gebruikersaccounts, toepassingen en andere statusgegevens van uw oorspronkelijke VM. Als u wilt gebruiken van de VHD-is een nieuwe virtuele machine maken, controleert u de volgende stappen zijn voltooid. 
  
  * [Voorbereiden van een Windows-VHD te uploaden naar Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Geen** generalize van de virtuele machine met behulp van Sysprep.
  * Verwijder eventuele Gast virtualisatie-hulpprogramma's en de agents die zijn geïnstalleerd op de virtuele machine (dat wil zeggen VMware tools).
  * Zorg ervoor dat de virtuele machine is geconfigureerd om op te halen van de IP-adres en DNS-instellingen via DHCP. Dit zorgt ervoor dat de server een IP-adres binnen het VNet verkrijgt wanneer deze opgestart wordt. 


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
   
### <a name="upload-the-vhd-to-your-storage-account"></a>De VHD te uploaden naar uw opslagaccount
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


## <a name="option-2-copy-the-vhd-from-an-existing-azure-vm"></a>Optie 2: Kopieer de VHD van een bestaande virtuele machine in Azure

U kunt een VHD kopiëren naar een ander opslagaccount te gebruiken bij het maken van een nieuwe, dubbele virtuele machine.

### <a name="before-you-begin"></a>Voordat u begint
Zorg ervoor dat u:

* Hebt u informatie over de **bron- en storage-accounts**. Voor de bron-VM moet u de namen van de storage-account en container hebt. Normaal gesproken de containernaam is **VHD's**. U moet ook een doelopslagaccount hebben. Als u dit niet al hebt, kunt u een met ofwel de portal (**meer Services** > opslagaccounts > toevoegen) of met behulp van de [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet. 
* Hebt gedownload en geïnstalleerd de [AzCopy hulpprogramma](../../storage/common/storage-use-azcopy.md). 

### <a name="deallocate-the-vm"></a>De virtuele machine ongedaan
Toewijzing van de VM, u de VHD maakt moet worden gekopieerd. 

* **Portal**: klik op **virtuele machines** > **myVM** > stoppen
* **PowerShell**: Gebruik [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) gestopt (toewijzing ongedaan maken) de virtuele machine met de naam **myVM** in de resourcegroep **myResourceGroup**.

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

De **Status** voor de virtuele machine in Azure portal wordt gewijzigd van **gestopt** naar **gestopt (toewijzing opgeheven)**.

### <a name="get-the-storage-account-urls"></a>De storage-account-URL's ophalen
U moet de URL's van de bron- en storage-accounts. De URL's eruit: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Als u de naam van de storage-account en de container al weet, kunt u alleen de gegevens tussen de vierkante haken voor het maken van de URL van uw vervangen. 

U kunt de Azure portal of Azure Powershell gebruiken om de URL te krijgen:

* **Portal**: klik op de  **>**  voor **meer services** > **opslagaccounts**  >   *Storage-account* > **Blobs** en de bron-VHD-bestand is waarschijnlijk in de **VHD's** container. Klik op **eigenschappen** voor de container en kopieer de tekst met het label **URL**. U moet de URL's van de bron- en doelserver containers. 
* **PowerShell**: Gebruik [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) ophalen van de gegevens voor de virtuele machine met de naam **myVM** in de resourcegroep **myResourceGroup**. Zoeken in de resultaten in de **archiefprofiel** sectie voor de **Vhd-Uri**. Het eerste deel van de Uri is de URL van de container en het laatste deel is de naam van de OS-VHD voor de virtuele machine.

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-the-storage-access-keys"></a>Ophalen van de toegangssleutels voor opslag
De sneltoetsen voor de bron- en storage-accounts vinden. Zie voor meer informatie over toegangstoetsen [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md).

* **Portal**: klik op **meer services** > **opslagaccounts** > *opslagaccount*  >  **Toegangssleutels**. Kopieer de sleutel met het label **key1**.
* **PowerShell**: Gebruik [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) ophalen van de opslagsleutel voor het opslagaccount **mystorageaccount** in de resourcegroep **myResourceGroup**. Kopieer de sleutel met het label **key1**.

```powershell
Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-the-vhd"></a>Kopieer de VHD
U kunt bestanden kopiëren tussen opslagaccounts met behulp van AzCopy. Voor de doelcontainer als de opgegeven container niet bestaat, wordt deze gemaakt voor u. 

Open een opdrachtprompt op uw lokale computer voor het gebruik van AzCopy en navigeer naar de map waarin AzCopy is geïnstalleerd. Dit is vergelijkbaar met *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

Alle bestanden in een container wilt kopiëren, gebruikt u de **/S** overschakelen. Dit kan worden gebruikt voor het kopiëren van de OS-VHD en alle gegevensschijven van de als deze zich in dezelfde container. Dit voorbeeld ziet u hoe u kopieert u alle bestanden in de container **mysourcecontainer** in opslagaccount **mysourcestorageaccount** aan de container **mydestinationcontainer**in de **mydestinationstorageaccount** storage-account. De namen van de storage-accounts en containers vervangen door uw eigen. Vervang `<sourceStorageAccountKey1>` en `<destinationStorageAccountKey1>` met uw eigen sleutels.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Als u wilt kopiëren van een specifieke VHD in een container met meerdere bestanden, kunt u ook de naam van het bestand met de schakeloptie /Pattern opgeven. In dit voorbeeld wordt alleen het bestand met de naam **myFileName.vhd** worden gekopieerd.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


Als dit is voltooid, ontvangt u een bericht dat ongeveer als volgt uitziet:

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
* Wanneer u met AZCopy, als de foutmelding 'Kan niet verifiëren van de aanvraag Server', zorg ervoor dat de waarde van de autorisatie-header is een samengesteld correct met inbegrip van de handtekening. Als u sleutel 2 of de sleutel van de secundaire opslag gebruikt, probeert u de primaire of 1e-opslagsleutel.

## <a name="create-the-new-vm"></a>De nieuwe virtuele machine maken 

U moet maken van netwerken en andere VM-netwerkbronnen kunnen worden gebruikt door de nieuwe virtuele machine.

### <a name="create-the-subnet-and-vnet"></a>Het subNet en een vNet maken

Maken van het vNet en het subNet van de [virtueel netwerk](../../virtual-network/virtual-networks-overview.md).

1. Het subNet maken. In dit voorbeeld wordt een subnet met de naam **mySubNet**, in de resourcegroep **myResourceGroup**, en stelt u het adres subnetvoorvoegsel op **10.0.0.0/24**.
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Het vNet maken. Dit voorbeeld wordt de naam van het virtuele netwerk moet **myVnetName**, de locatie voor het **VS-West**, en het adresvoorvoegsel voor het virtuele netwerk naar **10.0.0.0/16**. 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
### <a name="create-the-network-security-group-and-an-rdp-rule"></a>De netwerkbeveiligingsgroep en een RDP-regel maken
Als u zich aanmelden bij uw virtuele machine met RDP, moet u een regel waarmee op poort 3389 van RDP-toegang hebben. Omdat de VHD voor de nieuwe virtuele machine is gemaakt van een bestaand kunt gespecialiseerde VM, nadat de virtuele machine u maakt een bestaand account van de virtuele bronmachine die machtiging aan te melden met RDP had gebruiken.
Dit moet worden voltooid voordat u de netwerkinterface die zal worden gekoppeld.  
In het volgende voorbeeld wordt de naam van de NSG op **myNsg** en de naam van de RDP-regel aan **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Zie voor meer informatie over eindpunten en NSG-regels [openen van poorten voor een virtuele machine in Azure met behulp van PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Een openbaar IP-adres en NIC maken
Om te kunnen communiceren met de virtuele machine in het virtuele netwerk, hebt u een [openbaar IP-adres](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface nodig.

1. Het openbare IP-adres maken. In dit voorbeeld wordt de openbare naam van de IP-adres is ingesteld op **myIP**.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Maken van de NIC. In dit voorbeeld wordt de naam van de NIC is ingesteld op **myNicName**. Deze stap wordt ook gekoppeld aan de Netwerkbeveiligingsgroep eerder hebt gemaakt met deze NIC.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
    ```

### <a name="set-the-vm-name-and-size"></a>VM-naam en de grootte instellen

Dit voorbeeld wordt de naam van de VM naar 'myVM' en de VM-grootte naar 'Standard_A2'.
```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>De NIC toevoegen
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-the-os-disk"></a>Configureer de OS-schijf

1. Stel de URI voor de VHD die u hebt geüpload of gekopieerd. In dit voorbeeld wordt het VHD-bestand met de naam **myOsDisk.vhd** wordt opgeslagen in een opslagaccount met de naam **myStorageAccount** in een container met de naam **myContainer**.

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. De OS-schijf toevoegen. Wanneer de besturingssysteemschijf is gemaakt, is de term 'osDisk' in dit voorbeeld appened op de VM-naam voor het maken van de besturingssysteemschijf. In dit voorbeeld geeft ook dat deze VHD op basis van Windows moet worden gekoppeld aan de virtuele machine als de besturingssysteemschijf.
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

Optioneel: Als u gegevensschijven die moeten worden gekoppeld aan de virtuele machine hebt, de gegevensschijven toevoegen met behulp van de URL's van de gegevens van virtuele harde schijven en de juiste Logical Unit Number (Lun).

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

Wanneer u een opslagaccount, de gegevens en de URL's van besturingssysteem schijf als volgt uitzien: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. U kunt dit vinden op de portal bladeren naar de doel-storage-container, klikt u op het besturingssysteem of gegevens VHD die is gekopieerd en vervolgens de inhoud van de URL te kopiëren.


### <a name="complete-the-vm"></a>De virtuele machine voltooien 

De virtuele machine maken met de configuraties die we zojuist hebben gemaakt.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Als deze opdracht voltooid is, ziet u uitvoer als volgt:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>Controleren of de virtuele machine is gemaakt
U ziet de zojuist gemaakte virtuele machine ofwel in de [Azure-portal](https://portal.azure.com)onder **Bladeren** > **virtuele machines**, of met behulp van de volgende PowerShell-opdrachten:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Volgende stappen
Als u wilt aanmelden bij uw nieuwe virtuele machine, blader naar de virtuele machine in de [portal](https://portal.azure.com), klikt u op **Connect**, en open het RDP-Remote Desktop-bestand. Gebruik de referenties van het account van uw oorspronkelijke virtuele machine aan te melden bij uw nieuwe virtuele machine. Zie voor meer informatie [verbinding maken met en meld u aan een virtuele machine van Azure waarop Windows wordt uitgevoerd bij](connect-logon.md).

