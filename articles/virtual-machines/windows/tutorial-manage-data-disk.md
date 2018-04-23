---
title: Azure-schijven beheren met Azure PowerShell | Microsoft Docs
description: Zelfstudie - Azure-schijven beheren met Azure PowerShell
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 4d84d1f5fbd0fcf5d4c6ba374b1fb5c3df2ba5c6
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="manage-azure-disks-with-powershell"></a>Azure-schijven beheren met PowerShell

Virtuele machines in Azure gebruiken schijven voor het opslaan van het besturingssysteem, toepassingen en gegevens van virtuele machines. Bij het maken van een virtuele machine is het van belang dat u een schijfgrootte en configuratie kiest die geschikt zijn voor de verwachte werkbelasting. Deze zelfstudie bevat informatie over het implementeren en beheren van VM-schijven. U krijgt informatie over:

> [!div class="checklist"]
> * Besturingssysteemschijven en tijdelijke schijven
> * Gegevensschijven
> * Standard- en Premium-schijven
> * Schijfprestaties
> * Het koppelen en voorbereiden van gegevensschijven

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie moduleversie 5.3 of later van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 

## <a name="default-azure-disks"></a>Standaard Azure-schijven

Wanneer een virtuele Azure-machine wordt gemaakt, worden automatisch twee schijven aan de virtuele machine gekoppeld. 

**Besturingssysteemschijf**: besturingssysteemschijven kunnen tot 4 terabytes groot zijn, en huisvesten het besturingssysteem van de VM’s.  De besturingssysteemschijf krijgt standaard stationsletter *c:* toegewezen. De schijfcacheconfiguratie van de besturingssysteemschijf is geoptimaliseerd voor besturingssysteemprestaties. De besturingssysteemschijf kan **beter geen** toepassingen of gegevens bevatten. Gebruik voor toepassingen en gegevens een gegevensschijf, zoals verderop in dit artikel wordt beschreven.

**Tijdelijke schijf**: tijdelijke schijven gebruiken een SSD-schijf die zich op dezelfde Azure-host bevindt als de virtuele machine. Tijdelijke schijven leveren zeer goede prestaties en kunnen worden gebruikt voor bewerkingen als tijdelijke gegevensverwerking. Als de virtuele machine wordt verplaatst naar een nieuwe host, worden gegevens die zijn opgeslagen op een tijdelijke schijf echter verwijderd. De grootte van de tijdelijke schijf wordt bepaald door de VM-grootte. Tijdelijke schijven krijgen standaard de stationsletter van *d:* toegewezen.

### <a name="temporary-disk-sizes"></a>Groottes van tijdelijke schijven

| Type | Veelgebruikte grootten | Maximumgrootte van tijdelijke schijf (GiB) |
|----|----|----|
| [Algemeen doel](sizes-general.md) | A-, B- en D-serie | 1600 |
| [Geoptimaliseerde rekenkracht](sizes-compute.md) | F-serie | 576 |
| [Geoptimaliseerd geheugen](sizes-memory.md) | D-, E-, G- en M-serie | 6144 |
| [Geoptimaliseerde opslag](sizes-storage.md) | L-serie | 5630 |
| [GPU](sizes-gpu.md) | N-serie | 1440 |
| [Hoge prestaties](sizes-hpc.md) | A- en H-serie | 2000 |

## <a name="azure-data-disks"></a>Azure-gegevensschijven

Er kunnen extra gegevensschijven worden toegevoegd voor het installeren van toepassingen en opslaan van gegevens. Gegevensschijven moeten worden gebruikt in situaties waarin duurzame en responsieve gegevensopslag gewenst is. Elke gegevensschijf heeft een maximale capaciteit van 4 terabyte. De grootte van de virtuele machine bepaalt hoeveel gegevensschijven aan een virtuele machine kunnen worden gekoppeld. Voor elke VM-vCPU kunnen twee schijven worden gekoppeld. 

### <a name="max-data-disks-per-vm"></a>Max. aantal gegevensschijven per VM

| Type | Veelgebruikte grootten | Max. aantal gegevensschijven per VM |
|----|----|----|
| [Algemeen doel](sizes-general.md) | A-, B- en D-serie | 64 |
| [Geoptimaliseerde rekenkracht](sizes-compute.md) | F-serie | 64 |
| [Geoptimaliseerd geheugen](sizes-memory.md) | D-, E-, G- en M-serie | 64 |
| [Geoptimaliseerde opslag](sizes-storage.md) | L-serie | 64 |
| [GPU](sizes-gpu.md) | N-serie | 64 |
| [Hoge prestaties](sizes-hpc.md) | A- en H-serie | 64 |

## <a name="vm-disk-types"></a>Typen VM-schijven

Azure biedt twee typen schijven.

### <a name="standard-disk"></a>Standard-schijf

Standard Storage wordt ondersteund door HDD's en biedt voordelige en hoogwaardige opslag. Standard-schijven zijn ideaal voor een kostenefficiënte werkbelasting voor ontwikkelen en testen.

### <a name="premium-disk"></a>Premium-schijf

Premium-schijven worden ondersteund door hoogwaardige schijven met een lage latentie op basis van SSD. Ideaal voor virtuele machines met een productiewerkbelasting. Premium Storage ondersteunt virtuele machines uit de DS-serie, DSv2-serie GS-serie en FS-serie. Er zijn vijf typen Premium-schijven (P10, P20, P30, P40, P50); de grootte van de schijf bepaalt het schijftype. Wanneer u een selectie maakt, wordt de waarde van de schijfgrootte afgerond naar het volgende type. Als de grootte bijvoorbeeld kleiner is dan 128 GB is het schijftype P10, en tussen 129 en 512 GB is de schijf P20.

### <a name="premium-disk-performance"></a>Prestaties Premium-schijf

|Schijftype voor Premium Storage | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Schijfgrootte (afronden) | 32 GB | 64 GB | 128 GB | 512 GB | 1.024 GB (1 TB) | 2.048 GB (2 TB) | 4.095 GB (4 TB) |
| Max. aantal IOP's per schijf | 120 | 240 | 500 | 2.300 | 5.000 | 7.500 | 7.500 |
Doorvoer per schijf | 25 MB/s | 50 MB/s | 100 MB/s | 150 MB/s | 200 MB/s | 250 MB/s | 250 MB/s |

In de bovenstaande tabel wordt het max. IOP's per schijf aangegeven, maar er kan een hoger prestatieniveau worden bereikt door striping van meerdere gegevensschijven. Er kunnen bijvoorbeeld 64 gegevensschijven worden gekoppeld aan Standard_GS5 VM. Als voor elk van deze schijven een P30-grootte wordt gebruikt, kan een maximum van 80.000 IOP's worden behaald. Zie [VM-typen en -grootten](./sizes.md) voor gedetailleerde informatie over het maximum aantal IOP's per VM.

## <a name="create-and-attach-disks"></a>Schijven maken en koppelen

Om het voorbeeld in deze zelfstudie uit te voeren, moet u een bestaande virtuele machine hebben. Maak indien nodig een virtuele machine met de volgende opdrachten.

Stel met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) de gebruikersnaam en het wachtwoord in die nodig zijn voor de beheerdersaccount op de virtuele machine:

```azurepowershell-interactive
$cred = Get-Credential
```

Maak de virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupDisk" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred `
    -AsJob
```

Door de parameter `-AsJob` wordt de VM gemaakt als achtergrondtaak, zodat u weer terugkeert naar de PowerShell-prompts. U kunt details van achtergrondtaken bekijken met de cmdlet `Job`.

Maak de aanvankelijke configuratie met [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig). In het volgende voorbeeld wordt een schijf van 128 gigabyte groot geconfigureerd.

```azurepowershell-interactive
$diskConfig = New-AzureRmDiskConfig `
    -Location "EastUS" `
    -CreateOption Empty `
    -DiskSizeGB 128
```

Maak de gegevensschijf met de opdracht [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk).

```azurepowershell-interactive
$dataDisk = New-AzureRmDisk `
    -ResourceGroupName "myResourceGroupDisk" `
    -DiskName "myDataDisk" `
    -Disk $diskConfig
```

Haal de virtuele machine waaraan u de gegevensschijf wilt toevoegen op met de opdracht [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm).

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroupDisk" -Name "myVM"
```

Voeg de gegevensschijf toe aan de configuratie van de virtuele machine met de opdracht [Add-AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk).

```azurepowershell-interactive
$vm = Add-AzureRmVMDataDisk `
    -VM $vm `
    -Name "myDataDisk" `
    -CreateOption Attach `
    -ManagedDiskId $dataDisk.Id `
    -Lun 1
```

Werk de virtuele machine bij met de opdracht [Update-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk).

```azurepowershell-interactive
Update-AzureRmVM -ResourceGroupName "myResourceGroupDisk" -VM $vm
```

## <a name="prepare-data-disks"></a>Gegevensschijven voorbereiden

Wanneer een schijf is gekoppeld aan de virtuele machine, moet het besturingssysteem worden geconfigureerd voor gebruik van de schijf. Het volgende voorbeeld laat zien hoe u handmatig de eerste schijf configureert die aan de VM wordt toegevoegd. Dit proces kan ook worden geautomatiseerd met de [Custom Script Extension](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Handmatige configuratie

Maak een RDP-verbinding met de virtuele machine. Open PowerShell en voer dit script uit.

```azurepowershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u meer geleerd over onderwerpen over VM-schijven, zoals:

> [!div class="checklist"]
> * Besturingssysteemschijven en tijdelijke schijven
> * Gegevensschijven
> * Standard- en Premium-schijven
> * Schijfprestaties
> * Het koppelen en voorbereiden van gegevensschijven

In de volgende zelfstudie leert u hoe u de configuratie van virtuele machines automatiseert.

> [!div class="nextstepaction"]
> [VM-configuratie automatiseren](./tutorial-automate-vm-deployment.md)
