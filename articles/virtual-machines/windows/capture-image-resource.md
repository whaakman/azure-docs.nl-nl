---
title: Een beheerde installatiekopie maken in Azure | Microsoft Docs
description: Maak een beheerde installatiekopie van een gegeneraliseerde virtuele machine of VHD in Azure. Installatiekopieën kunnen worden gebruikt om te maken van meerdere virtuele machines die gebruikmaken van beheerde schijven.
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
ms.date: 09/27/2018
ms.author: cynthn
ms.openlocfilehash: b0dc587035509606059e3620201b2061bbe4d6a0
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57341762"
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a>Maak een beheerde installatiekopie van een gegeneraliseerde VM in Azure

De resource van een beheerde installatiekopie kan worden gemaakt van een gegeneraliseerde virtuele machine (VM) die is opgeslagen als een beheerde schijf of een niet-beheerde schijf in een storage-account. De installatiekopie kan vervolgens worden gebruikt om te maken van meerdere virtuele machines. Voor meer informatie over beheerde installatiekopieën worden in rekening gebracht, Zie [Managed Disks-prijzen](https://azure.microsoft.com/pricing/details/managed-disks/). 

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="generalize-the-windows-vm-using-sysprep"></a>De Windows VM generaliseren met behulp van Sysprep

Sysprep verwijdert al uw persoonlijke account en informatie over beveiliging, en vervolgens bereidt de computer moet worden gebruikt als een afbeelding. Zie voor meer informatie over Sysprep [Sysprep overzicht](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

Zorg ervoor dat de server-functies die worden uitgevoerd op de machine worden ondersteund door Sysprep. Zie voor meer informatie, [Sysprep-ondersteuning voor serverfuncties](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep-support-for-server-roles).

> [!IMPORTANT]
> Nadat u hebt Sysprep uitgevoerd op een virtuele machine, die virtuele machine wordt beschouwd als *gegeneraliseerd* en kan niet opnieuw worden gestart. Het generaliseringsproces van een VM is onomkeerbaar. Als u behouden de oorspronkelijke virtuele machine werkt wilt, moet u een [kopie van de virtuele machine](create-vm-specialized.md#option-3-copy-an-existing-azure-vm) en de kopie te generaliseren. 
>
> Als u van plan bent om uit te voeren van Sysprep voordat u de virtuele harde schijf (VHD) uploadt naar Azure voor het eerst, zorg ervoor dat u hebt [uw virtuele machine voorbereid](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).  
> 
> 

Als u wilt uw Windows-VM generaliseren, de volgende stappen uit:

1. Aanmelden bij uw Windows-VM.
   
2. Open een opdrachtpromptvenster als beheerder. Wijzig de map in % windir%\system32\sysprep en voer `sysprep.exe`.
   
3. In de **hulpprogramma voor systeemvoorbereiding** in het dialoogvenster, selecteer **Voer System Out-of-Box Experience (OOBE)** en selecteer de **Generalize** selectievakje.
   
4. Voor **afsluitopties**, selecteer **afsluiten**.
   
5. Selecteer **OK**.
   
    ![Sysprep start](./media/upload-generalized-managed/sysprepgeneral.png)

6. Als Sysprep is voltooid, sluit de virtuele machine. Start de virtuele machine niet opnieuw.


## <a name="create-a-managed-image-in-the-portal"></a>Een beheerde installatiekopie maken in de portal 

1. Open de [Azure Portal](https://portal.azure.com).

2. Selecteer in het menu aan de linkerkant, **virtuele machines** en selecteer vervolgens de virtuele machine in de lijst.

3. In de **virtuele machine** -pagina voor de virtuele machine, op het bovenste menu en selecteer **vastleggen**.

   De **afbeelding maken** pagina wordt weergegeven.

4. Voor **naam**, accepteert u de naam van de vooraf ingestelde of voer een naam die u wilt gebruiken voor de installatiekopie.

5. Voor **resourcegroep**, selecteer **nieuw** en voer een naam in of selecteer **gebruik bestaande** en selecteer een resourcegroep in de vervolgkeuzelijst.

6. Als u verwijderen van de bron-VM wilt nadat de installatiekopie gemaakt, selecteer is **deze virtuele machine automatisch verwijderen na het maken van de installatiekopie van het**.

7. Als u wilt dat de mogelijkheid het gebruik van de installatiekopie in een [binnen een beschikbaarheidszone](../../availability-zones/az-overview.md), selecteer **op** voor **zoneflexibiliteit**.

8. Selecteer **maken** om de installatiekopie te maken.

9. Nadat de installatiekopie is gemaakt, kunt u vinden als een **installatiekopie** resource in de lijst met resources in de resourcegroep.



## <a name="create-an-image-of-a-vm-using-powershell"></a>Maken van een installatiekopie van een virtuele machine met behulp van Powershell

Het maken van een installatiekopie van een rechtstreeks vanuit de virtuele machine zorgt ervoor dat de installatiekopie alle schijven die zijn gekoppeld aan de virtuele machine bevat, met inbegrip van de schijf met het besturingssysteem en eventuele gegevensschijven. In dit voorbeeld laat zien hoe een beheerde installatiekopie maken van een virtuele machine die maakt gebruik van schijven beheerde.


Voordat u begint, zorg ervoor dat u de nieuwste versie van de AzureRM.Compute PowerShell-module versie 5.7.0 moet of hoger. Uitvoeren als u wilt zien welke versie, `Get-Module -ListAvailable AzureRM.Compute` in PowerShell. Als u upgraden wilt, raadpleegt u [Azure PowerShell installeren op Windows met PowerShellGet](/powershell/azure/azurerm/install-azurerm-ps). Als u PowerShell lokaal uitvoert, voert u `Connect-AzAccount` voor het maken van een verbinding met Azure.


> [!NOTE]
> Als u wilt voor het opslaan van uw installatiekopie in de zone-redundante opslag, moet u deze maken in een regio die ondersteuning biedt voor [beschikbaarheidszones](../../availability-zones/az-overview.md) en bevatten de `-ZoneResilient` parameter in de configuratie van de installatiekopie (`New-AzImageConfig` opdracht).

Volg deze stappen voor het maken van een VM-installatiekopie:

1. Sommige variabelen maken.

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. Zorg ervoor dat de virtuele machine is opgeheven.

    ```azurepowershell-interactive
    Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Zet de status van de virtuele machine **gegeneraliseerd**. 
   
    ```azurepowershell-interactive
    Set-AzVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. Haal de virtuele machine op. 

    ```azurepowershell-interactive
    $vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName
    ```

5. Maak de configuratie van installatiekopie.

    ```azurepowershell-interactive
    $image = New-AzImageConfig -Location $location -SourceVirtualMachineId $vm.Id 
    ```
6. Maak de installatiekopie.

    ```azurepowershell-interactive
    New-AzImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 

## <a name="create-an-image-from-a-managed-disk-using-powershell"></a>Een installatiekopie maken van een beheerde schijf met behulp van PowerShell

Als u maken van een afbeelding van de schijf van het besturingssysteem wilt, geeft u de beheerde schijf-ID als de besturingssysteemschijf:

    
1. Sommige variabelen maken. 

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```

2. Ophalen van de virtuele machine.

   ```azurepowershell-interactive
   $vm = Get-AzVm -Name $vmName -ResourceGroupName $rgName
   ```

3. Haal de ID van de beheerde schijf.

    ```azurepowershell-interactive
    $diskID = $vm.StorageProfile.OsDisk.ManagedDisk.Id
    ```
   
3. Maak de configuratie van installatiekopie.

    ```azurepowershell-interactive
    $imageConfig = New-AzImageConfig -Location $location
    $imageConfig = Set-AzImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -ManagedDiskId $diskID
    ```
    
4. Maak de installatiekopie.

    ```azurepowershell-interactive
    New-AzImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 


## <a name="create-an-image-from-a-snapshot-using-powershell"></a>Een installatiekopie maken van een momentopname met behulp van Powershell

U kunt een beheerde installatiekopie maken van een momentopname van een gegeneraliseerde virtuele machine door de volgende stappen:

    
1. Sommige variabelen maken. 

    ```azurepowershell-interactive
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. Ophalen van de momentopname.

   ```azurepowershell-interactive
   $snapshot = Get-AzSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. Maak de configuratie van installatiekopie.

    ```azurepowershell-interactive
    $imageConfig = New-AzImageConfig -Location $location
    $imageConfig = Set-AzImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. Maak de installatiekopie.

    ```azurepowershell-interactive
    New-AzImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 


## <a name="create-an-image-from-a-vhd-in-a-storage-account"></a>Een installatiekopie van een VHD in een opslagaccount maken

Een beheerde installatiekopie maken vanaf een gegeneraliseerde VHD met besturingssysteem in een storage-account. U moet de URI van de VHD in de storage-account, dat zich in de volgende indeling: https://*mystorageaccount*.blob.core.windows.net/*vhdcontainer* /  *vhdfilename.VHD*. In dit voorbeeld wordt de VHD is in *mystorageaccount*, in een container met de naam *vhdcontainer*, en de bestandsnaam van de VHD is *vhdfilename.vhd*.


1.  Sommige variabelen maken.

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    $osVhdUri = "https://mystorageaccount.blob.core.windows.net/vhdcontainer/vhdfilename.vhd"
    ```
2. Stoppen/toewijzing ongedaan maken de virtuele machine.

    ```azurepowershell-interactive
    Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. De virtuele machine markeren als gegeneraliseerd.

    ```azurepowershell-interactive
    Set-AzVm -ResourceGroupName $rgName -Name $vmName -Generalized  
    ```
4.  De installatiekopie maken met behulp van uw gegeneraliseerde VHD met het besturingssysteem.

    ```azurepowershell-interactive
    $imageConfig = New-AzImageConfig -Location $location
    $imageConfig = Set-AzImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

    
## <a name="next-steps"></a>Volgende stappen
- [Een virtuele machine maken vanaf een beheerde installatiekopie](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).    

