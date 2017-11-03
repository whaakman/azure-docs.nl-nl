---
title: Een begeleide afbeelding maken in Azure | Microsoft Docs
description: Maak een begeleide afbeelding van een gegeneraliseerde virtuele machine of VHD in Azure. Afbeeldingen kunnen worden gebruikt voor het maken van meerdere virtuele machines die gebruikmaken van beheerde schijven.
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
ms.date: 10/09/2017
ms.author: cynthn
ms.openlocfilehash: d6409ac490f530d49f82c93b07b0fd22adbec4de
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a>Maken van een begeleide afbeelding van een gegeneraliseerde virtuele machine in Azure

Een beheerde Afbeeldingsbron kan worden gemaakt vanuit een gegeneraliseerde virtuele machine die wordt opgeslagen als een beheerde schijf of een niet-beheerde schijf in een opslagaccount. De installatiekopie kan vervolgens worden gebruikt voor het maken van meerdere virtuele machines. 


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


## <a name="create-a-managed-image-in-the-portal"></a>Een begeleide afbeelding maken in de portal 

1. Open de [portal](https://portal.azure.com).
2. Klik op de virtuele Machines in het menu aan de linkerkant en selecteer vervolgens de virtuele machine in de lijst.
3. Klik op de pagina voor de virtuele machine in het bovenste menu **vastleggen**.
3. In **naam**, typ de naam die u wilt gebruiken voor de afbeelding.
4. In **resourcegroep** select **nieuw** en typ een naam in of selecteer **gebruik bestaande** en selecteer een resourcegroep te gebruiken uit de vervolgkeuzelijst.
5. Als u de bron-VM verwijderen wilt nadat de installatiekopie gemaakt, selecteer is **automatisch verwijderen van deze virtuele machine na het maken van de installatiekopie van het**.
6. Wanneer u klaar bent, klikt u op **maken**.
16. Nadat de installatiekopie is gemaakt, ziet u dit als een **installatiekopie** resource in de lijst met resources in de resourcegroep.



## <a name="create-an-image-of-a-vm-using-powershell"></a>Een installatiekopie maken van een virtuele machine met behulp van Powershell

Het maken van een installatiekopie van een rechtstreeks vanuit de virtuele machine, zorgt u ervoor dat de installatiekopie alle schijven die zijn gekoppeld aan de virtuele machine bevat, inclusief de Besturingssysteemschijf en alle gegevensschijven. In dit voorbeeld laat zien hoe een begeleide afbeelding maken van een virtuele machine of schijven die gebruikt worden beheerd.


Voordat u begint, zorg ervoor dat u de nieuwste versie van de AzureRM.Compute PowerShell-module hebt. Voer de volgende opdracht om deze te installeren.

```azurepowershell-interactive
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Zie voor meer informatie [Azure PowerShell Versioning](/powershell/azure/overview).


1. Sommige variabelen maken.

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. Zorg ervoor dat de virtuele machine is opgeheven.

    ```azurepowershell-interactive
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Zet de status van de virtuele machine **gegeneraliseerd**. 
   
    ```azurepowershell-interactive
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. Zorg dat de virtuele machine. 

    ```azurepowershell-interactive
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```

5. Maak de configuratie van de installatiekopie.

    ```azurepowershell-interactive
    $image = New-AzureRmImageConfig -Location $location -SourceVirtualMachineId $vm.ID 
    ```
6. Maken van de installatiekopie.

    ```azurepowershell-interactive
    New-AzureRmImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 
## <a name="create-an-image-from-a-managed-disk-using-powershell"></a>Maken van een installatiekopie van een beheerde schijf met behulp van PowerShell

Als u alleen maken van een installatiekopie van de schijf met het besturingssysteem wilt, kunt u ook een installatiekopie van een verpakken door te geven van de beheerde schijf-ID als de besturingssysteemschijf.

    
1. Sommige variabelen maken. 

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. De virtuele machine worden opgehaald.

   ```azurepowershell-interactive
   $vm = Get-AzureRmVm -Name myVM -ResourceGroupName $rgName
   ```

3. De ID van de beheerde schijf niet ophalen.

    ```azurepowershell-interactive
    $diskID = $vm.StorageProfile.OsDisk.ManagedDisk.Id
    ```
   
3. Maak de configuratie van de installatiekopie.

    ```azurepowershell-interactive
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -ManagedDiskId $diskID
    ```
    
4. Maken van de installatiekopie.

    ```azurepowershell-interactive
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 


## <a name="create-an-image-from-a-snapshot-using-powershell"></a>Een installatiekopie maken vanuit een momentopname met behulp van Powershell

U kunt een begeleide afbeelding maken vanuit een momentopname van een gegeneraliseerde virtuele machine.

    
1. Sommige variabelen maken. 

    ```azurepowershell-interactive
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. Ophalen van de momentopname.

   ```azurepowershell-interactive
   $snapshot = Get-AzureRmSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. Maak de configuratie van de installatiekopie.

    ```azurepowershell-interactive
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. Maken van de installatiekopie.

    ```azurepowershell-interactive
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 


## <a name="create-image-from-a-vhd-in-a-storage-account"></a>Installatiekopie van een VHD in een opslagaccount maken

Een begeleide afbeelding van een gegeneraliseerde OS-VHD in een opslagaccount maken. U moet de URI van de VHD in het opslagaccount, dat zich in de indeling https:// bevindt*mystorageaccount*.blob.core.windows.net/*container*/*vhd_filename.vhd*. In dit voorbeeld wordt de VHD die is in *mystorageaccount* in een container met de naam *vhdcontainer* en de bestandsnaam van de VHD is *osdisk.vhd*.


1.  Stel eerst de algemene parameters:

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    $osVhdUri = "https://mystorageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd"
    ```
2. Step\deallocate de virtuele machine.

    ```azurepowershell-interactive
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. De virtuele machine niet markeren als gegeneraliseerd.

    ```azurepowershell-interactive
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized 
    ```
4.  De installatiekopie via uw algemene besturingssysteem-VHD maken.

    ```azurepowershell-interactive
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

    
## <a name="next-steps"></a>Volgende stappen
- Nu u kunt [een virtuele machine maken van de installatiekopie van het algemene beheerde](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).  

