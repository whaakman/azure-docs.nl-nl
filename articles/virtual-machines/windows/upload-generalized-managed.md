---
title: Een beheerde Azure-virtuele machine maken van een algemene lokale VHD | Microsoft Docs
description: Een gegeneraliseerde VHD uploaden naar Azure en deze gebruiken voor het maken van nieuwe virtuele machines, in het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: cynthn
ms.openlocfilehash: 2e78ecf6bd281bd5d30f59413789eb1e6fc7b5bc
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Een gegeneraliseerde VHD uploaden en deze gebruiken voor het maken van nieuwe virtuele machines in Azure

In dit onderwerp leert u een VHD van een gegeneraliseerde virtuele uploaden naar Azure, een installatiekopie van de VHD maken en een nieuwe virtuele machine maken vanuit die installatiekopie met behulp van PowerShell. U kunt een VHD die is geëxporteerd vanuit een hulpprogramma voor het virtualisatie lokale of vanuit een andere cloud uploaden. Met behulp van [schijven beheerd](managed-disks-overview.md) voor de nieuwe virtuele machine, vereenvoudigt u het beheer van de virtuele machine en zorgt voor betere beschikbaarheid als de virtuele machine wordt geplaatst in een beschikbaarheidsset. 

Als u een voorbeeldscript gebruiken wilt, raadpleegt u [voorbeeldscript een VHD uploaden naar Azure en een nieuwe virtuele machine maken](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)

## <a name="before-you-begin"></a>Voordat u begint

- Voordat u de VHD uploadt naar Azure, u moet volgen [voorbereiden van een Windows-VHD of VHDX uploaden naar Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Bekijk [plannen voor de migratie naar schijven beheerd](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) voordat u de migratie naar [schijven beheerd](managed-disks-overview.md).
- Zorg ervoor dat u de nieuwste versie van de AzureRM.Compute PowerShell-module hebt. Voer de volgende opdracht om deze te installeren.

    ```powershell
    Install-Module AzureRM.Compute -RequiredVersion 2.6.0
    ```
    Zie voor meer informatie [Azure PowerShell Versioning](/powershell/azure/overview).


## <a name="generalize-the-windows-vm-using-sysprep"></a>De virtuele machine van Windows met behulp van Sysprep generalize

Sysprep verwijdert alle persoonlijke gegevens over uw account, onder andere en voorbereiden van de machine moet worden gebruikt als een afbeelding. Zie voor meer informatie over Sysprep [hoe gebruik Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).

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
6. Wanneer Sysprep is voltooid, afgesloten de virtuele machine. Start de virtuele machine niet opnieuw.



## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Als er geen PowerShell-versie 1.4 al of hoger is geïnstalleerd, lezen [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

1. Open Azure PowerShell en meld u bij uw Azure-account. Een pop-upvenster wordt geopend voor u de referenties van uw Azure-account in te voeren.
   
    ```powershell
    Login-AzureRmAccount
    ```
2. Het abonnement-id's voor uw beschikbare abonnementen niet ophalen.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Instellen van het juiste abonnement met de abonnement-ID. Vervang  *<subscriptionID>*  met de ID van het juiste abonnement.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>Het storage-account ophalen
U moet een opslagaccount in Azure voor het opslaan van de installatiekopie van het geüploade VM. U kunt een bestaand opslagaccount gebruiken of een nieuwe maken. 

Als u de VHD worden gebruikt voor het maken van een beheerde schijf voor een virtuele machine, de locatie van de storage-account moet hetzelfde zijn de locatie waar u de virtuele machine gaat maken.

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

    Maken van een resourcegroep met de naam **myResourceGroup** in de **VS-Oost** regio, type:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "East US"
    ```

2. Maken van een opslagaccount met de naam **mystorageaccount** in deze resourcegroep met behulp van de [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "East US"`
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
    Geldige waarden voor - SkuName zijn:
   
   * **Standard_LRS** -lokaal redundante opslag. 
   * **Standard_ZRS** -Zone-redundante opslag.
   * **Standard_GRS** -geografisch redundante opslag. 
   * **Standard_RAGRS** -geografisch redundante opslag met leestoegang. 
   * **Premium_LRS** -Premium lokaal redundante opslag. 

## <a name="upload-the-vhd-to-your-storage-account"></a>De VHD te uploaden naar uw opslagaccount

Gebruik de [toevoegen AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) cmdlet voor het uploaden van de VHD naar een container in uw opslagaccount. In dit voorbeeld wordt het bestand geüpload *myVHD.vhd* van *' C:\Users\Public\Documents\Virtual harde schijven\"*  om een opslagaccount met de naam *mystorageaccount* in de *myResourceGroup* resourcegroep. Het bestand worden opgenomen in de container met de naam *mycontainer* en worden de nieuwe bestandsnaam *myUploadedVHD.vhd*.

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

Afhankelijk van uw netwerkverbinding en de grootte van de VHD-bestand met deze opdracht kan even duren om te voltooien

Sla de **bestemmings-URI** pad om later te gebruiken als u een beheerde schijf of een nieuwe virtuele machine met behulp van de geüploade VHD te maken.

### <a name="other-options-for-uploading-a-vhd"></a>Andere opties voor het uploaden van een VHD
 
 
U kunt ook een VHD uploaden naar uw storage-account met behulp van een van de volgende opties:

- [AzCopy](http://aka.ms/downloadazcopy)
- [API voor Azure Storage kopiëren-Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure Storage Explorer uploaden van BLOB 's](https://azurestorageexplorer.codeplex.com/)
- [Opslag voor importeren/exporteren Service REST API-verwijzing](https://msdn.microsoft.com/library/dn529096.aspx)
-   U kunt het beste Import/Export-Service gebruikt als de geschatte tijd uploaden is langer dan zeven dagen. U kunt [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) om in te schatten het tijdstip in gegevenseenheid grootte en de overdracht. 
    Import/Export kan worden gebruikt om te kopiëren naar een standard-opslagaccount. U wilt kopiëren van de standard-opslag naar premium storage-account met behulp van een hulpprogramma zoals AzCopy.

> [!IMPORTANT]
> Als u uw VHD uploaden naar Azure AzCopy gebruikt, controleert u of u hebt ingesteld [/BlobType:page](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy#blobtypeblock--page--append) voordat u script uploaden. Als de bestemming een blob is en deze optie niet is opgegeven, standaard maakt AzCopy een blok-blob.
> 
> 



## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Een begeleide afbeelding van de geüploade VHD maken 

Maak een begeleide afbeelding met behulp van uw algemene besturingssysteem-VHD. De waarden vervangt door uw eigen gegevens.


1.  Stel eerst de algemene parameters:

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    $vmSize = "Standard_DS1_v2"
    $location = "East US" 
    $imageName = "yourImageName"
    ```

4.  De installatiekopie via uw algemene besturingssysteem-VHD maken.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $urlOfUploadedImageVhd
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
Maken van het vNet en het subnet van de [virtueel netwerk](../../virtual-network/virtual-networks-overview.md).

1. Het subnet maken. In dit voorbeeld wordt een subnet met de naam *mySubnet* met het adresvoorvoegsel van *10.0.0.0/24*.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Maak het virtuele netwerk. In dit voorbeeld wordt een virtueel netwerk met de naam *myVnet* met het adresvoorvoegsel van *10.0.0.0/16*.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Een openbaar IP-adres en een netwerkinterface maken

Om te kunnen communiceren met de virtuele machine in het virtuele netwerk, hebt u een [openbaar IP-adres](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface nodig.

1. Een openbaar IP-adres maken. In dit voorbeeld wordt een openbaar IP-adres met de naam *myPip*. 
   
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

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>De netwerkbeveiligingsgroep en een RDP-regel maken

Als u zich aanmelden bij uw virtuele machine met RDP, moet u een netwerkbeveiligingsregel (NSG) waarmee op poort 3389 van RDP-toegang hebben. 

In dit voorbeeld maakt u een NSG met de naam *myNsg* die een regel aangeroepen bevat *myRdpRule* waarmee RDP-verkeer via poort 3389. Zie voor meer informatie over Nsg [openen van poorten voor een virtuele machine in Azure met behulp van PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Een variabele voor het virtuele netwerk maken

Een variabele voor het voltooide virtueel netwerk maken. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-the-credentials-for-the-vm"></a>De referenties voor de virtuele machine ophalen

De volgende cmdlet wordt een venster waarin u een nieuwe gebruikersnaam en wachtwoord gebruiken als het lokale administrator-account voor het op afstand toegang tot de virtuele machine wordt invoeren geopend. 

```powershell
$cred = Get-Credential
```

## <a name="add-the-vm-name-and-size-to-the-vm-configuration"></a>Het VM-naam en de grootte toevoegen aan de VM-configuratie.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a>De VM-installatiekopie als bronafbeelding voor de nieuwe virtuele machine instellen

Stel de broninstallatiekopie met de ID van de beheerde VM-installatiekopie.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a>Stel de configuratie van het besturingssysteem en voeg de NIC.

Voer het opslagtype (PremiumLRS of StandardLRS) en de grootte van de besturingssysteemschijf. In dit voorbeeld wordt het accounttype ingesteld op *PremiumLRS*, de schijfgrootte van de op *128 GB* en schijfcache naar *ReadWrite*.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a>De virtuele machine maken

De nieuwe virtuele machine maken met de configuratie die is opgeslagen in de **$vm** variabele.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a>Controleren of de virtuele machine is gemaakt
Als u klaar is, ziet u de zojuist gemaakte virtuele machine in de [Azure-portal](https://portal.azure.com) onder **Bladeren** > **virtuele machines**, of met behulp van de volgende PowerShell-opdrachten:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Volgende stappen

Als u wilt aanmelden bij uw nieuwe virtuele machine, blader naar de virtuele machine in de [portal](https://portal.azure.com), klikt u op **Connect**, en open het RDP-Remote Desktop-bestand. Gebruik de referenties van het account van uw oorspronkelijke virtuele machine aan te melden bij uw nieuwe virtuele machine. Zie voor meer informatie [verbinding maken met en meld u aan een virtuele machine van Azure waarop Windows wordt uitgevoerd bij](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

