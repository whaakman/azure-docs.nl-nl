---
title: Gebruik van een Windows VM maken met Azure PowerShell probleemoplossing | Microsoft Docs
description: Informatie over het oplossen van problemen met de Windows-VM in Azure door verbinding te maken van de besturingssysteemschijf aan een virtuele machine met behulp van Azure PowerShell voor herstel
services: virtual-machines-windows
documentationCenter: ''
authors: genlin
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 1e87704e7d8cf3c7cc21e537d36f95a97265061b
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903513"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-azure-powershell"></a>Een virtuele Windows-machine oplossen door de besturingssysteemschijf koppelen aan een virtuele machine met behulp van Azure PowerShell voor herstel
Als uw Windows virtuele machine (VM) in Azure een opstart- of schijffout-fout optreedt, moet u mogelijk de stappen voor probleemoplossing uitvoeren op de virtuele harde schijf zelf. Een veelvoorkomend voorbeeld is een mislukte toepassingsupdate die verhindert de virtuele machine dat wordt het opstarten. Dit artikel wordt uitgelegd hoe u Azure PowerShell gebruiken voor de virtuele harde schijf verbinden met een andere Windows-virtuele machine om eventuele fouten te corrigeren en vervolgens de oorspronkelijke virtuele machine opnieuw te maken.


## <a name="recovery-process-overview"></a>Overzicht van het herstelproces
Het probleemoplossingsproces is als volgt:

1. Verwijder de virtuele machine problemen worden aangetroffen, de virtuele harde schijven te houden.
2. Toevoegen en koppelen van de virtuele harde schijf met een andere Windows-VM voor het oplossen van problemen.
3. Maak verbinding met de VM voor probleemoplossing. Bewerk bestanden of alle hulpmiddelen voor het oplossen van problemen op de oorspronkelijke virtuele harde schijf worden uitgevoerd.
4. Koppel de virtuele harde schijf van de VM voor probleemoplossing los.
5. Een virtuele machine met behulp van de oorspronkelijke virtuele harde schijf maken.

Voor de virtuele machine die gebruikmaakt van beheerde schijf, Zie [een beheerde schijf-VM oplossen door het koppelen van een nieuwe schijf met besturingssysteem](#troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk).


Zorg ervoor dat u hebt [de nieuwste Azure PowerShell](/powershell/azure/overview) geïnstalleerd en aangemeld bij uw abonnement:

```powershell
Connect-AzureRmAccount
```

In de volgende voorbeelden kunt u namen van parameters vervangen door uw eigen waarden. Voorbeeld-parameternamen bevatten `myResourceGroup`, `mystorageaccount`, en `myVM`.


## <a name="determine-boot-issues"></a>Opstartproblemen met bepalen
U kunt een schermopname van de virtuele machine in Azure om op te lossen opstartproblemen met weergeven. Deze schermopname kunt identificeren waarom een virtuele machine is mislukt om op te starten. Het volgende voorbeeld wordt de schermafbeelding van de Windows-VM met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

Bekijk de schermafbeelding om te bepalen waarom de virtuele machine is mislukt om op te starten. Houd er rekening mee specifieke foutberichten of foutcodes die zijn opgegeven.


## <a name="view-existing-virtual-hard-disk-details"></a>Bestaande virtuele harde schijfdetails weergeven
Voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt, moet u de naam van de virtuele harde schijf (VHD).

Het volgende voorbeeld wordt een van de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Zoek naar `Vhd URI` binnen de `StorageProfile` gedeelte uit de uitvoer van de voorgaande opdracht. Het volgende voorbeeld uitvoer wordt afgekapt de `Vhd URI` aan het einde van het codeblok:

```powershell
RequestId                     : 8a134642-2f01-4e08-bb12-d89b5b81a0a0
StatusCode                    : OK
ResourceGroupName             : myResourceGroup
Id                            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
Name                          : myVM
Type                          : Microsoft.Compute/virtualMachines
...
StorageProfile                :
  ImageReference              :
    Publisher                 : MicrosoftWindowsServer
    Offer                     : WindowsServer
    Sku                       : 2016-Datacenter
    Version                   : latest
  OsDisk                      :
    OsType                    : Windows
    Name                      : myVM
    Vhd                       :
      Uri                     : https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    Caching                   : ReadWrite
    CreateOption              : FromImage
```


## <a name="delete-existing-vm"></a>Bestaande virtuele machine verwijderen
Virtuele harde schijven en virtuele machines zijn twee verschillende resources in Azure. Een virtuele harde schijf is waar het besturingssysteem zelf, toepassingen en configuraties worden opgeslagen. De virtuele machine zelf bestaat uit metagegevens die definieert de grootte of locatie en verwijst naar resources, zoals een virtuele harde schijf of virtuele netwerkinterfacekaart (NIC). Elke virtuele harde schijf heeft een lease toegewezen wanneer die zijn gekoppeld aan een virtuele machine. Hoewel gegevensschijven zelfs wanneer de virtuele machine wordt uitgevoerd, kunnen worden gekoppeld en losgekoppeld, kan de besturingssysteemschijf niet worden losgekoppeld tenzij de VM-resource wordt verwijderd. De lease blijft de besturingssysteemschijf koppelen aan een virtuele machine, zelfs wanneer die virtuele machine in een status gestopt en toewijzing ongedaan gemaakt.

De eerste stap bij het herstellen van uw virtuele machine is de VM-resource zelf verwijderen. Wanneer de virtuele machine wordt verwijderd, blijven de virtuele harde schijven aanwezig in uw opslagaccount. Nadat de virtuele machine is verwijderd, kunt u de virtuele harde schijf koppelen aan een andere virtuele machine oplossen van problemen en los de fouten.

Het volgende voorbeeld wordt de virtuele machine met de naam `myVM` uit de resourcegroep met de naam `myResourceGroup`:

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Wacht totdat de virtuele machine verwijderd is voordat u de virtuele harde schijf aan een andere virtuele machine koppelen. De lease op de virtuele harde schijf die het aan de virtuele machine koppelt moet worden vrijgegeven voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Bestaande virtuele harde schijf koppelen aan een andere virtuele machine
Voor de volgende stappen gebruikt u een andere virtuele machine voor het oplossen van problemen. U kunt de bestaande virtuele harde schijf koppelen aan deze VM voor probleemoplossing om te bladeren en de inhoud van de schijf bewerken. Dit proces kunt u Corrigeer eventuele fouten in de configuratie of extra toepassing of Systeemlogboekbestanden, bijvoorbeeld kunnen beoordelen. Kies of maak een andere virtuele machine moet worden gebruikt voor het oplossen van problemen.

Wanneer u de bestaande virtuele harde schijf koppelt, geef de URL naar de schijf hebt verkregen in de voorgaande `Get-AzureRmVM` opdracht. Het volgende voorbeeld wordt een bestaande virtuele harde schijf gekoppeld aan de VM voor probleemoplossing met de naam `myVMRecovery` in de resourcegroep met de naam `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> Toevoegen van een schijf, moet u de grootte van de schijf opgeven. Als we een bestaande schijf koppelen de `-DiskSizeInGB` is opgegeven als `$null`. Deze waarde zorgt ervoor dat de gegevensschijf correct is gekoppeld, en zonder de noodzaak om te bepalen van de waarde true grootte van de gegevensschijf.


## <a name="mount-the-attached-data-disk"></a>De gekoppelde gegevensschijf koppelen

1. RDP aan uw VM voor probleemoplossing met behulp van de juiste referenties. In het volgende voorbeeld wordt het RDP-verbinding-bestand gedownload voor de virtuele machine met de naam `myVMRecovery` in de resourcegroep met de naam `myResourceGroup`, en downloadt deze `C:\Users\ops\Documents`"

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. De gegevensschijf is automatisch gedetecteerd en die is gekoppeld. De lijst met gekoppelde volumes om te bepalen van de stationsletter als volgt bekijken:

    ```powershell
    Get-Disk
    ```

    De volgende voorbeelduitvoer ziet u de virtuele harde schijf een schijf verbonden **2**. (U kunt ook `Get-Volume` om weer te geven van de stationsletter):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a>Problemen oplossen op oorspronkelijke virtuele harde schijf
Met de bestaande virtuele harde schijf dat wordt gekoppeld, kunt u nu onderhouds- en stappen voor probleemoplossing naar behoefte uitvoeren. Zodra u de problemen hebt opgelost, kunt u doorgaan met de volgende stappen.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Oorspronkelijke virtuele harde schijf loskoppelen
Zodra de fouten opgelost zijn, kunt u ontkoppelen en de bestaande virtuele harde schijf loskoppelen van uw virtuele machine voor probleemoplossing. U kunt de virtuele harde schijf met andere VM's niet gebruiken totdat de lease die de virtuele harde schijf koppelen aan de virtuele machine voor probleemoplossing is vrijgegeven.

1. Ontkoppel de gegevensschijf binnen uw RDP-sessie op de virtuele machine voor herstel. U moet het nummer van de schijf van de vorige `Get-Disk` cmdlet. Vervolgens gebruikt u `Set-Disk` om in te stellen van de schijf als offline:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Controleer of de schijf is nu ingesteld als het gebruik van offline `Get-Disk` opnieuw. De volgende voorbeelduitvoer ziet u dat de schijf is nu ingesteld als offline:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. Sluit uw RDP-sessie. Verwijder de virtuele harde schijf van de VM voor probleemoplossing vanuit de Azure PowerShell-sessie.

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a>Virtuele machine maken vanaf de oorspronkelijke harde schijf
Gebruik voor het maken van een virtuele machine van de oorspronkelijke virtuele harde schijf [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). De daadwerkelijke JSON-sjabloon is op de volgende koppeling:

- https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-specialized-vhd-new-or-existing-vnet/azuredeploy.json

De sjabloon implementeert een virtuele machine in een bestaand virtueel netwerk, met behulp van de VHD-URL van de vorige opdracht. Het volgende voorbeeld implementeert u de sjabloon naar de resourcegroep met de naam `myResourceGroup`:

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

Beantwoord de prompts voor de sjabloon, zoals VM-naam, type besturingssysteem en VM-grootte. De `osDiskVhdUri` is hetzelfde als eerder gebruikt als de bestaande virtuele harde schijf koppelen aan de VM voor probleemoplossing.


## <a name="re-enable-boot-diagnostics"></a>Diagnostische gegevens over opstarten opnieuw inschakelen

Wanneer u uw virtuele machine van de bestaande virtuele harde schijf maakt, diagnostische gegevens over opstarten mogelijk niet automatisch ingeschakeld. Het volgende voorbeeld wordt de diagnostische extensie op de virtuele machine met de naam `myVMDeployed` in de resourcegroep met de naam `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk"></a>Een beheerde schijf-VM oplossen door een nieuwe besturingssysteemschijf te koppelen
1. De betrokken beheerde schijf Windows virtuele machine stoppen.
2. [Maken van een momentopname van een beheerde schijf](snapshot-copy-managed-disk.md) van de Besturingssysteemschijf van de virtuele machine van beheerde schijf.
3. [Een beheerde schijf maken op basis van de momentopname](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md).
4. [De beheerde schijf koppelen als een gegevensschijf van de virtuele machine](attach-disk-ps.md).
5. [Wijzigen van de gegevensschijf van stap 4 in besturingssysteemschijf](os-disk-swap.md).

## <a name="next-steps"></a>Volgende stappen
Als u hebt met het maken van een verbinding met uw virtuele machine problemen, Zie [oplossen RDP-verbindingen met een Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Zie voor problemen met toegang tot toepassingen die worden uitgevoerd op de virtuele machine, [verbindingsproblemen van toepassing op een Windows-VM oplossen](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Zie voor meer informatie over het gebruik van Resource Manager [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
