---
title: Een beheerde Azure-virtuele machine maken vanaf een gegeneraliseerde on-premises VHD | Microsoft Docs
description: Een gegeneraliseerde VHD uploaden naar Azure en het maken van nieuwe virtuele machines, in het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/26/2018
ms.author: cynthn
ms.openlocfilehash: 8fd88a0e3c5b387ce3ea586f6f23b3643a03e58d
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618164"
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Een gegeneraliseerde VHD uploaden en maken van nieuwe virtuele machines in Azure

In dit onderwerp helpt u bij het uploaden van een VHD van een gegeneraliseerde VM naar Azure, een installatiekopie van de VHD maken en een nieuwe virtuele machine maken van die installatiekopie met behulp van PowerShell. U kunt een VHD die is geëxporteerd uit een on-premises virtualisatie-hulpprogramma of een andere cloud uploaden. Met behulp van [Managed Disks](managed-disks-overview.md) voor de nieuwe virtuele machine vereenvoudigt u het VM-beheer en betere beschikbaarheid biedt wanneer de virtuele machine wordt geplaatst in een beschikbaarheidsset. 

Als u gebruiken van een voorbeeld van een script wilt, Zie [voorbeeldscript een VHD uploaden naar Azure en een nieuwe virtuele machine maken](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)

## <a name="before-you-begin"></a>Voordat u begint

- Voordat u een VHD uploaden naar Azure, u moet volgen [voorbereiden van een Windows VHD of VHDX te uploaden naar Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Beoordeling [plannen voor de migratie naar Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) voordat u begint met uw migratie naar [Managed Disks](managed-disks-overview.md).
- In dit artikel is de AzureRM-moduleversie 5.6 of hoger vereist. Voer ` Get-Module -ListAvailable AzureRM.Compute` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).


## <a name="generalize-the-source-vm-using-sysprep"></a>De bron-VM generaliseren met Sysprep

Sysprep verwijdert onder meer al uw persoonlijke accountinformatie en de machine wordt voorbereid om als een installatiekopie te worden gebruikt. Zie voor meer informatie over Sysprep de [Sysprep overzicht](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

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
6. Wanneer Sysprep is voltooid, wordt de virtuele machine afgesloten. Start de virtuele machine niet opnieuw.


## <a name="get-the-storage-account"></a>Het opslagaccount ophalen

U moet een opslagaccount in Azure voor het opslaan van de geüploade VM-installatiekopie. U kunt een bestaand opslagaccount gebruiken of een nieuwe maken. 

Als u de VHD te maken van een beheerde schijf voor een virtuele machine gebruikt, de locatie van het opslagaccount moet hetzelfde zijn de locatie waar u de virtuele machine wordt maakt.

Als u wilt de beschikbare opslag-accounts weergeven, typt u:

```azurepowershell
Get-AzureRmStorageAccount | Format-Table
```

## <a name="upload-the-vhd-to-your-storage-account"></a>De VHD uploaden naar uw storage-account

Gebruik de [Add-AzureRmVhd](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd) cmdlet voor het uploaden van de VHD naar een container in uw storage-account. In dit voorbeeld wordt het bestand geüpload *myVHD.vhd* van *"C:\Users\Public\Documents\Virtual hardeschijven\"*  naar een opslagaccount met de naam *mystorageaccount* in de *myResourceGroup* resourcegroep. Het bestand worden opgenomen in de container met de naam *mycontainer* en de nieuwe bestandsnaam worden *myUploadedVHD.vhd*.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
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

Afhankelijk van uw netwerkverbinding en de grootte van uw VHD-bestand kan met deze opdracht even duren om uit te voeren

### <a name="other-options-for-uploading-a-vhd"></a>Andere opties voor het uploaden van een VHD
 
U kunt ook een VHD uploaden naar uw opslagaccount met behulp van een van de volgende opties:

- [AzCopy](http://aka.ms/downloadazcopy)
- [Azure-opslag kopiëren van de Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure Storage Explorer uploaden van Blobs](https://azurestorageexplorer.codeplex.com/)
- [Naslaginformatie voor Storage Import/Export Service REST API](https://msdn.microsoft.com/library/dn529096.aspx)
-   U wordt aangeraden met Import/Export-Service als de geschatte tijd uploaden is langer dan zeven dagen. U kunt [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) om in te schatten van de tijd van de eenheid grootte en de overdracht van gegevens. 
    Import/Export kan worden gebruikt om te kopiëren naar een standard storage-account. U wilt kopiëren van standaardopslag naar premium storage-account met behulp van een hulpprogramma zoals AzCopy.

> [!IMPORTANT]
> Als u uw VHD uploaden naar Azure AzCopy gebruikt, zorg ervoor dat u hebt ingesteld [/BlobType:page](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#blobtypeblock--page--append) voordat u script uploaden. Als de bestemming een blob is en deze optie niet, standaard opgegeven is, wordt een blok-blob door AzCopy gemaakt.
> 
> 



## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Een beheerde installatiekopie van het geüploade VHD maken 

Maak een beheerde installatiekopie met behulp van uw gegeneraliseerde VHD met het besturingssysteem. Vervang de waarden door uw eigen gegevens.


Stel eerst de enkele parameters:

```powershell
$location = "East US" 
$imageName = "myImage"
```

Maak de installatiekopie met behulp van uw gegeneraliseerde VHD met het besturingssysteem.

```powershell
$imageConfig = New-AzureRmImageConfig `
   -Location $location
$imageConfig = Set-AzureRmImageOsDisk `
   -Image $imageConfig `
   -OsType Windows `
   -OsState Generalized `
   -BlobUri $urlOfUploadedImageVhd `
   -DiskSizeGB 20
New-AzureRmImage `
   -ImageName $imageName `
   -ResourceGroupName $rgName `
   -Image $imageConfig
```


## <a name="create-the-vm"></a>De virtuele machine maken

Nu u een installatiekopie hebt gemaakt, kunt u een of meer nieuwe VM's van de installatiekopie maken met behulp. Dit voorbeeld maakt u een virtuele machine met de naam *myVM* uit de *myImage*, in de *myResourceGroup*.


```powershell
New-AzureRmVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -ImageName $imageName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNSG" `
    -PublicIpAddressName "myPIP" `
    -OpenPorts 3389
```


## <a name="next-steps"></a>Volgende stappen

Aanmelden bij uw nieuwe virtuele machine. Zie voor meer informatie, [hoe u verbinding maken met en meld u aan een virtuele Azure-machine waarop Windows wordt uitgevoerd bij](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

