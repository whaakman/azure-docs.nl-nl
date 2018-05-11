---
title: Een beheerde Azure-virtuele machine maken van een algemene lokale VHD | Microsoft Docs
description: Een gegeneraliseerde VHD uploaden naar Azure en deze gebruiken voor het maken van nieuwe virtuele machines, in het Resource Manager-implementatiemodel.
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
ms.openlocfilehash: 6f2f5eae8a4512595457d92d17832cf462b4bec4
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Een gegeneraliseerde VHD uploaden en deze gebruiken voor het maken van nieuwe virtuele machines in Azure

In dit onderwerp leert u een VHD van een gegeneraliseerde virtuele uploaden naar Azure, een installatiekopie van de VHD maken en een nieuwe virtuele machine maken vanuit die installatiekopie met behulp van PowerShell. U kunt een VHD die is geëxporteerd vanuit een hulpprogramma voor het virtualisatie lokale of vanuit een andere cloud uploaden. Met behulp van [schijven beheerd](managed-disks-overview.md) voor de nieuwe virtuele machine, vereenvoudigt u het beheer van de virtuele machine en zorgt voor betere beschikbaarheid als de virtuele machine wordt geplaatst in een beschikbaarheidsset. 

Als u een voorbeeldscript gebruiken wilt, raadpleegt u [voorbeeldscript een VHD uploaden naar Azure en een nieuwe virtuele machine maken](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)

## <a name="before-you-begin"></a>Voordat u begint

- Voordat u de VHD uploadt naar Azure, u moet volgen [voorbereiden van een Windows-VHD of VHDX uploaden naar Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Bekijk [plannen voor de migratie naar schijven beheerd](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) voordat u de migratie naar [schijven beheerd](managed-disks-overview.md).
- In dit artikel vereist de AzureRM moduleversie 5,6 of hoger. Voer ` Get-Module -ListAvailable AzureRM.Compute` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).


## <a name="generalize-the-source-vm-using-sysprep"></a>De bron-VM generalize met behulp van Sysprep

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


## <a name="get-the-storage-account"></a>Het storage-account ophalen

U moet een opslagaccount in Azure voor het opslaan van de installatiekopie van het geüploade VM. U kunt een bestaand opslagaccount gebruiken of een nieuwe maken. 

Als u de VHD worden gebruikt voor het maken van een beheerde schijf voor een virtuele machine, de locatie van de storage-account moet hetzelfde zijn de locatie waar u de virtuele machine gaat maken.

Als u wilt de beschikbare opslagruimte accounts weergeven, typt u:

```azurepowershell
Get-AzureRmStorageAccount | Format-Table
```

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

### <a name="other-options-for-uploading-a-vhd"></a>Andere opties voor het uploaden van een VHD
 
U kunt ook een VHD uploaden naar uw storage-account met behulp van een van de volgende opties:

- [AzCopy](http://aka.ms/downloadazcopy)
- [API voor Azure Storage kopiëren-Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure Storage Explorer uploaden van BLOB 's](https://azurestorageexplorer.codeplex.com/)
- [Opslag voor importeren/exporteren Service REST API-verwijzing](https://msdn.microsoft.com/library/dn529096.aspx)
-   U kunt het beste Import/Export-Service gebruikt als de geschatte tijd uploaden is langer dan zeven dagen. U kunt [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) om in te schatten het tijdstip in gegevenseenheid grootte en de overdracht. 
    Import/Export kan worden gebruikt om te kopiëren naar een standard-opslagaccount. U wilt kopiëren van de standard-opslag naar premium storage-account met behulp van een hulpprogramma zoals AzCopy.

> [!IMPORTANT]
> Als u uw VHD uploaden naar Azure AzCopy gebruikt, controleert u of u hebt ingesteld [/BlobType:page](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#blobtypeblock--page--append) voordat u script uploaden. Als de bestemming een blob is en deze optie niet is opgegeven, standaard maakt AzCopy een blok-blob.
> 
> 



## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Een begeleide afbeelding van de geüploade VHD maken 

Maak een begeleide afbeelding met behulp van uw algemene besturingssysteem-VHD. De waarden vervangt door uw eigen gegevens.


Stel eerst de sommige parameters:

```powershell
$location = "East US" 
$imageName = "myImage"
```

De installatiekopie via uw algemene besturingssysteem-VHD maken.

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

Nu dat u een installatiekopie hebt, kunt u een of meer nieuwe virtuele machines kunt maken van de installatiekopie. In dit voorbeeld wordt een virtuele machine met de naam *myVM* van de *myImage*, in de *myResourceGroup*.


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

Meld u aan de nieuwe virtuele machine. Zie voor meer informatie [verbinding maken met en meld u aan een virtuele machine van Azure waarop Windows wordt uitgevoerd bij](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

