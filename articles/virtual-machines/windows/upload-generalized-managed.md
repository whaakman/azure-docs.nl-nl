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
ms.date: 09/25/2018
ms.author: cynthn
ms.openlocfilehash: b1ad5aa074a7719dbe6000301c8cd04e6e1ad632
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984542"
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Een gegeneraliseerde VHD uploaden en maken van nieuwe virtuele machines in Azure

In dit artikel begeleidt u bij het uploaden van een VHD van een gegeneraliseerde VM naar Azure, een installatiekopie van de VHD maken en een nieuwe virtuele machine maken van die installatiekopie met behulp van PowerShell. U kunt een VHD die is geëxporteerd uit een on-premises virtualisatie-hulpprogramma of een andere cloud uploaden. Met behulp van [Managed Disks](managed-disks-overview.md) voor de nieuwe virtuele machine vereenvoudigt u het VM-beheer en betere beschikbaarheid biedt wanneer de virtuele machine wordt geplaatst in een beschikbaarheidsset. 

Zie voor een voorbeeld van een script, [voorbeeldscript een VHD uploaden naar Azure en maak een nieuwe virtuele machine](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md).

## <a name="before-you-begin"></a>Voordat u begint

- Voordat u een VHD uploaden naar Azure, u moet volgen [voorbereiden van een Windows VHD of VHDX te uploaden naar Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- Beoordeling [plannen voor de migratie naar Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) voordat u begint met uw migratie naar [Managed Disks](managed-disks-overview.md).

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]


## <a name="generalize-the-source-vm-by-using-sysprep"></a>De bron-VM generaliseren met Sysprep

Sysprep verwijdert onder meer al uw persoonlijke accountinformatie en de machine wordt voorbereid om als een installatiekopie te worden gebruikt. Zie voor meer informatie over Sysprep de [Sysprep overzicht](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

Zorg ervoor dat de server-functies die worden uitgevoerd op de machine worden ondersteund door Sysprep. Zie voor meer informatie, [Sysprep-ondersteuning voor serverfuncties](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [!IMPORTANT]
> Als u van plan bent om uit te voeren van Sysprep voordat u uw VHD uploadt naar Azure voor het eerst, zorg ervoor dat u hebt [uw virtuele machine voorbereid](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
> 
> 

1. Meld u aan de virtuele machine van Windows.
2. Open het venster met de opdrachtprompt als beheerder. Wijzig de map in % windir%\system32\sysprep en voer `sysprep.exe`.
3. In de **hulpprogramma voor systeemvoorbereiding** in het dialoogvenster, selecteer **Voer System Out-of-Box Experience (OOBE)**, en zorg ervoor dat de **Generalize** selectievakje is ingeschakeld.
4. Voor **afsluitopties**, selecteer **afsluiten**.
5. Selecteer **OK**.
   
    ![Sysprep start](./media/upload-generalized-managed/sysprepgeneral.png)
6. Als Sysprep is voltooid, sluit de virtuele machine. Start de virtuele machine niet opnieuw.


## <a name="get-a-storage-account"></a>Een opslagaccount ophalen

U hebt een opslagaccount in Azure voor het opslaan van de geüploade VM-installatiekopie. U kunt een bestaand opslagaccount gebruiken of een nieuwe maken. 

Als u dat u de VHD worden gebruikt voor het maken van een beheerde schijf voor een virtuele machine, is de locatie van de storage-account moet dezelfde locatie waar u de virtuele machine maakt.

Als u wilt weergeven van de beschikbare opslag-accounts, typ:

```azurepowershell
Get-AzStorageAccount | Format-Table
```

## <a name="upload-the-vhd-to-your-storage-account"></a>De VHD uploaden naar uw storage-account

Gebruik de [toevoegen AzVhd](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd) cmdlet voor het uploaden van de VHD naar een container in uw storage-account. In dit voorbeeld wordt het bestand geüpload *myVHD.vhd* van *C:\Users\Public\Documents\Virtual hardeschijven\\*  naar een opslagaccount met de naam *mystorageaccount* in de *myResourceGroup* resourcegroep. Het bestand worden opgenomen in de container met de naam *mycontainer* en de nieuwe bestandsnaam worden *myUploadedVHD.vhd*.

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

### <a name="other-options-for-uploading-a-vhd"></a>Andere opties voor het uploaden van een VHD
 
U kunt ook een VHD uploaden naar uw opslagaccount met behulp van een van de volgende opties:

- [AzCopy](https://aka.ms/downloadazcopy)
- [Azure-opslag kopiëren van de Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure Storage Explorer uploaden van Blobs](https://azurestorageexplorer.codeplex.com/)
- [Naslaginformatie voor Storage Import/Export Service REST API](https://msdn.microsoft.com/library/dn529096.aspx)
-   Het is raadzaam om met behulp van de Import/Export-Service als de geschatte tijd uploaden is langer dan zeven dagen. U kunt [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) om in te schatten van de tijd van de eenheid grootte en de overdracht van gegevens. 
    Import/Export kan worden gebruikt om te kopiëren naar een standard storage-account. U moet worden gekopieerd van standaardopslag naar premium storage-account met behulp van een hulpprogramma zoals AzCopy.

> [!IMPORTANT]
> Als u AzCopy gebruikt voor uw VHD uploaden naar Azure, zorg ervoor dat u hebt ingesteld [ **/BlobType:page** ](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#blobtypeblock--page--append) voordat het script uploaden wordt uitgevoerd. Als de bestemming een blob is en deze optie niet is opgegeven, maakt AzCopy standaard een blok-blob.
> 
> 



## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Een beheerde installatiekopie van het geüploade VHD maken 

Een beheerde installatiekopie maken van uw gegeneraliseerde VHD met het besturingssysteem. De volgende waarden vervangen door uw eigen gegevens.


Stel eerst enkele parameters:

```powershell
$location = "East US" 
$imageName = "myImage"
```

Maak de installatiekopie met behulp van uw gegeneraliseerde VHD met het besturingssysteem.

```powershell
$imageConfig = New-AzImageConfig `
   -Location $location
$imageConfig = Set-AzImageOsDisk `
   -Image $imageConfig `
   -OsType Windows `
   -OsState Generalized `
   -BlobUri $urlOfUploadedImageVhd `
   -DiskSizeGB 20
New-AzImage `
   -ImageName $imageName `
   -ResourceGroupName $rgName `
   -Image $imageConfig
```


## <a name="create-the-vm"></a>De virtuele machine maken

Nu u een installatiekopie hebt gemaakt, kunt u een of meer nieuwe VM's van de installatiekopie maken met behulp. Dit voorbeeld maakt u een virtuele machine met de naam *myVM* van *myImage*in *myResourceGroup*.


```powershell
New-AzVm `
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

