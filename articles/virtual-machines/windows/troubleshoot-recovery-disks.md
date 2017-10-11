---
title: Gebruik een virtuele machine met Azure PowerShell probleemoplossing Windows | Microsoft Docs
description: Meer informatie over het oplossen van problemen van de virtuele machine van Windows in Azure door verbinding te maken van de besturingssysteemschijf voor een herstel-VM met Azure PowerShell
services: virtual-machines-windows
documentationCenter: 
authors: genlin
manager: timlt
editor: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 8b7821b4285e73d461af426bfdfb3fdcc4454517
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-azure-powershell"></a>Een virtuele machine van Windows oplossen door de OS-schijf koppelen aan een herstel-VM met Azure PowerShell
Als uw Windows-machine (VM) in Azure een opstart- of -fout optreedt, moet u wellicht de stappen voor probleemoplossing uitvoeren op de virtuele harde schijf zelf. Een veelvoorkomend voorbeeld is een mislukte toepassingsupdate die verhindert dat de virtuele machine kunnen opstarten is. Dit artikel wordt uitgelegd hoe u Azure PowerShell gebruiken voor verbinding van de virtuele harde schijf met een andere virtuele machine van Windows op eventuele fouten te corrigeren en vervolgens opnieuw maken van de oorspronkelijke VM.


## <a name="recovery-process-overview"></a>Overzicht van het herstelproces
Het probleemoplossingsproces is als volgt:

1. Verwijder de virtuele machine zonder problemen, om de virtuele harde schijven te houden.
2. Koppelen en koppel de virtuele harde schijf aan een andere Windows-VM voor het oplossen van problemen.
3. Maak verbinding met de VM voor probleemoplossing. Bewerken van bestanden of alle hulpmiddelen voor het oplossen van problemen op de oorspronkelijke virtuele harde schijf worden uitgevoerd.
4. Koppel de virtuele harde schijf van de VM voor probleemoplossing los.
5. Een virtuele machine maken met de oorspronkelijke virtuele harde schijf.

Zorg ervoor dat u hebt [de meest recente Azure PowerShell](/powershell/azure/overview) geïnstalleerd en aangemeld bij uw abonnement:

```powershell
Login-AzureRMAccount
```

In de volgende voorbeelden kunt u parameternamen vervangen door uw eigen waarden. De namen van de voorbeeld-parameter `myResourceGroup`, `mystorageaccount`, en `myVM`.


## <a name="determine-boot-issues"></a>Opstartproblemen bepalen
U kunt een schermopname van uw virtuele machine weergeven in Azure opstartproblemen op te lossen. Deze schermafbeelding kunt nagaan waarom een virtuele machine niet kan worden opgestart. Het volgende voorbeeld wordt de schermafbeelding van de Windows-VM met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

Controleer de schermopname om te bepalen waarom de virtuele machine kan niet worden opgestart. Houd er rekening mee specifieke foutberichten of foutcodes die zijn opgegeven.


## <a name="view-existing-virtual-hard-disk-details"></a>Bestaande virtuele harde schijf details weergeven
Voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt, moet u de naam van de virtuele harde schijf (VHD).

Het volgende voorbeeld wordt de informatie voor de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Zoek naar `Vhd URI` binnen de `StorageProfile` sectie uit de uitvoer van de voorgaande opdracht. Het volgende voorbeeld ziet u uitvoer afgekapt de `Vhd URI` aan het einde van het codeblok:

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
Virtuele harde schijven en virtuele machines zijn twee verschillende resources in Azure. Een virtuele harde schijf is waar het besturingssysteem zelf, toepassingen en configuraties worden opgeslagen. De virtuele machine zelf zijn gewoon metagegevens die definieert de grootte of locatie en verwijst naar resources, zoals een virtuele harde schijf of virtuele netwerkinterfacekaart (NIC). Elke virtuele harde schijf heeft een lease toegewezen wanneer gekoppeld aan een virtuele machine. Hoewel gegevensschijven zelfs wanneer de virtuele machine wordt uitgevoerd, kunnen worden gekoppeld en losgekoppeld, kan de besturingssysteemschijf niet worden losgekoppeld tenzij de VM-resource wordt verwijderd. De lease blijft de OS-schijf koppelen aan een virtuele machine, zelfs wanneer die VM een status gestopt en de toewijzing ongedaan is gemaakt heeft.

De eerste stap voor het herstellen van uw virtuele machine is de VM-resource zelf verwijderen. Wanneer de virtuele machine wordt verwijderd, blijven de virtuele harde schijven aanwezig in uw opslagaccount. Nadat de virtuele machine wordt verwijderd, kunt u de virtuele harde schijf koppelen aan een andere virtuele machine kunt u de fouten oplossen.

Het volgende voorbeeld wordt de virtuele machine met de naam `myVM` uit de resourcegroep met de naam `myResourceGroup`:

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Wacht totdat de virtuele machine verwijderen voordat u de virtuele harde schijf aan een andere virtuele machine koppelen is voltooid. De lease op de virtuele harde schijf waarin deze zijn gekoppeld aan de virtuele machine moet worden vrijgegeven voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Bestaande virtuele harde schijf koppelen aan een andere virtuele machine
Voor de volgende stappen gebruikt u een andere virtuele machine voor het oplossen van problemen. U koppelen de bestaande virtuele harde schijf aan deze VM voor probleemoplossing om te bladeren en de inhoud van de schijf bewerken. Dit proces kunt u eventuele configuratiefouten te corrigeren of extra toepassing of een systeem-logboekbestanden, bijvoorbeeld controleren. Kies of maak een andere virtuele machine moet worden gebruikt voor het oplossen van problemen.

Wanneer u de bestaande virtuele harde schijf koppelt, geef de URL naar de schijf die is verkregen in de voorgaande `Get-AzureRmVM` opdracht. Het volgende voorbeeld wordt een bestaande virtuele harde schijf voor het oplossen van problemen met de naam VM `myVMRecovery` in de resourcegroep met de naam `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> Een schijf toevoegen, moet u de grootte van de schijf opgeven. Als er een bestaande schijf koppelen de `-DiskSizeInGB` is opgegeven als `$null`. Deze waarde zorgt ervoor dat de gegevensschijf correct is gekoppeld, en zonder de noodzaak om te bepalen van de waarde true grootte van de gegevensschijf.


## <a name="mount-the-attached-data-disk"></a>Koppel de gekoppelde gegevensschijf

1. RDP met uw probleemoplossing virtuele machine met de juiste referenties. Het volgende voorbeeld het RDP-verbindingsbestand downloads voor de virtuele machine met de naam `myVMRecovery` in de resourcegroep met de naam `myResourceGroup`, en deze is gedownload `C:\Users\ops\Documents`'

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. De gegevensschijf wordt automatisch gedetecteerd en gekoppeld. De lijst met gekoppelde volumes om te bepalen van de stationsletter als volgt bekijken:

    ```powershell
    Get-Disk
    ```

    De volgende voorbeelduitvoer wordt weergegeven op de virtuele harde schijf een schijf verbonden **2**. (U kunt ook `Get-Volume` om weer te geven van de stationsletter):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a>Los problemen op de oorspronkelijke virtuele harde schijf
Met de bestaande virtuele harde schijf dat wordt gekoppeld, kunt u nu onderhoud en stappen voor probleemoplossing naar behoefte uitvoeren. Zodra u de problemen hebt opgelost, kunt u doorgaan met de volgende stappen.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Ontkoppel en loskoppelen van de oorspronkelijke virtuele harde schijf
Zodra de fouten opgelost zijn, kunt u ontkoppelen en loskoppelen van de bestaande virtuele harde schijf van uw VM voor het oplossen van problemen. U kunt de virtuele harde schijf niet gebruiken met eventuele andere virtuele machine totdat de lease koppelen van de virtuele harde schijf voor het oplossen van problemen VM is uitgebracht.

1. Ontkoppel de gegevensschijf binnen uw RDP-sessie op de herstel-VM. U moet het schijfnummer van de vorige `Get-Disk` cmdlet. Vervolgens gebruikt u `Set-Disk` de schijf als offline instellen:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Controleer de schijf is nu ingesteld als het gebruik van offline `Get-Disk` opnieuw. De volgende voorbeelduitvoer wordt weergegeven op dat de schijf is nu ingesteld als offline:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. De RDP-sessie af te sluiten. Verwijder de virtuele harde schijf van de probleemoplossing VM van Azure PowerShell-sessie.

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a>Virtuele machine van de oorspronkelijke harde schijf maken
Gebruik voor het maken van een virtuele machine van de oorspronkelijke virtuele harde schijf [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). De werkelijke JSON-sjabloon is op de volgende koppeling:

- https://RAW.githubusercontent.com/Azure/Azure-QuickStart-templates/master/201-VM-Specialized-VHD-existing-vnet/azuredeploy.JSON

De sjabloon implementeert een virtuele machine in een bestaand virtueel netwerk met behulp van de VHD-URL van de vorige opdracht. Het volgende voorbeeld implementeert u de sjabloon in de resourcegroep met de naam `myResourceGroup`:

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

Beantwoord de aanwijzingen voor de sjabloon zoals VM-naam, type besturingssysteem en VM-grootte. De `osDiskVhdUri` is hetzelfde als eerder gebruikt als de bestaande virtuele harde schijf koppelen aan de VM voor het oplossen van problemen.


## <a name="re-enable-boot-diagnostics"></a>Diagnostische gegevens over opstarten weer inschakelen

Wanneer u uw virtuele machine van de bestaande virtuele harde schijf maakt, kan boot diagnostics niet automatisch worden ingeschakeld. Het volgende voorbeeld wordt de diagnostische uitbreiding op de virtuele machine met de naam `myVMDeployed` in de resourcegroep met de naam `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a>Volgende stappen
Als u verbinding maakt met uw virtuele machine problemen ondervindt, raadpleegt u [problemen met RDP-verbindingen naar een Azure-virtuele machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Zie voor problemen met de toegang tot toepassingen die worden uitgevoerd op de virtuele machine [oplossen verbindingsproblemen van toepassing op een Windows VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Zie voor meer informatie over het gebruik van Resource Manager [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
