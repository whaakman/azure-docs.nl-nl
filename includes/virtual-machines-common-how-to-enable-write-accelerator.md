---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: msraiye
ms.service: virtual-machines
ms.topic: include
ms.date: 6/8/2018
ms.author: raiye
ms.custom: include file
ms.openlocfilehash: cd9b8eaf84ac4c1227c521628fd4156eec4506bf
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38746264"
---
# <a name="enable-write-accelerator"></a>Write Accelerator inschakelt

Write dat Accelerator is als een schijf-mogelijkheid voor M-serie virtuele Machines (VM's) op Premium Storage met Azure Managed Disks exclusief. Als de naam van de staat, is het doel van de functionaliteit voor het verbeteren van de i/o-latentie van schrijfbewerkingen ten opzichte van Azure Premium Storage. Write dat Accelerator is ideaal wanneer log-bestandsupdates nodig zijn om vast te leggen op schijf op een zeer krachtige manier voor moderne databases.

Write dat Accelerator is algemeen beschikbaar voor M-serie VM's in de openbare Cloud.

## <a name="planning-for-using-write-accelerator"></a>Planning voor het gebruik van Write Accelerator

Write dat Accelerator voor de volumes die het transactielogboek bevatten of Logboeken van een DBMS-systemen opnieuw moet worden gebruikt. Het wordt niet aanbevolen als de functie is geoptimaliseerd om te worden gebruikt voor logboekschijven Write Accelerator gebruikt voor de gegevensvolumes van een DBMS-systemen.

Write Accelerator werkt alleen in combinatie met [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/).

> [!IMPORTANT]
> Inschakelen van Write Accelerator voor de besturingssysteemschijf van de virtuele machine, wordt de virtuele machine opnieuw opgestart.
>
> Om in te schakelen Write Accelerator aan een bestaande Azure-schijf die geen deel uitmaakt van een volume build op basis van meerdere schijven met Windows-schijf of volume managers, Windows Storage Spaces, Scale-out van de Windows-bestandsserver (SOFS), Linux LVM, of MDADM, de werkbelasting die toegang tot de Azure-schijf moet worden afgesloten. Database-toepassingen met behulp van de Azure-schijf moeten worden afgesloten.
>
> Als u wilt in- of uitschakelen van Write Accelerator voor een bestaand volume die uit meerdere Azure Premium Storage-schijven is gebaseerd en striped verdeeld met behulp van de schijf of volume managers Windows, Windows Storage Spaces, Scale-out van de Windows-bestandsserver (SOFS), Linux LVM of MDADM, alle schijven die het bouwen van het volume moeten worden ingeschakeld of uitgeschakeld voor Write Accelerator in afzonderlijke stappen. **Afgesloten voordat het in- of uitschakelen van Write Accelerator in een dergelijke configuratie, de Azure-VM**.

Inschakelen van Write Accelerator voor OS-schijven mag geen die nodig zijn voor de virtuele machine met betrekking tot SAP-configuraties.

### <a name="restrictions-when-using-write-accelerator"></a>Beperkingen bij het gebruik van Write Accelerator

Als u Write Accelerator voor een Azure-schijf/VHD, wordt deze beperkingen gelden:

- De Premium-schijfcache moet worden ingesteld op 'None' of 'Alleen-lezen'. Alle andere opslaan in cache modi worden niet ondersteund.
- Momentopnamen op een schijf Write Accelerator is ingeschakeld wordt nog niet ondersteund. Deze beperking blokkeert Azure Backup-Service de mogelijkheid om uit te voeren een toepassingsconsistente momentopname van alle schijven van de virtuele machine.
- Alleen kleinere i/o-grootte (< = 32 KiB) het versneld pad afleggen. In de workload situaties waarin bulksgewijs is het ophalen van gegevens geladen of wanneer de buffers transactie logboek van de verschillende DBMS-systemen zijn ingevuld in een grotere mate voordat het ophalen van persistent gemaakt met de opslag, zijn kans om de i/o geschreven naar schijf neemt het versneld pad niet.

Er zijn beperkingen van Azure Premium Storage VHD's per virtuele machine die kan worden ondersteund door Write Accelerator. De huidige limieten zijn:

| VM-SKU | Aantal schijven Write Accelerator | Write Accelerator schijf-IOPS per VM |
| --- | --- | --- |
| M128ms, 128s | 16 | 8000 |
| M64ms, M64ls, M64s | 8 | 4000 |
| M32ms, M32ls, M32ts, M32s | 4 | 2000 |
| M16ms, M16s | 2 | 1000 |
| M8ms, M8s | 1 | 500 |

De IOPS-limieten gelden per VM en *niet* per schijf. Alle schijven van Write Accelerator delen de dezelfde IOPS-limiet per virtuele machine.

## <a name="enabling-write-accelerator-on-a-specific-disk"></a>Write Accelerator inschakelt op een specifieke schijf

De volgende gedeelten wordt beschreven hoe Write Accelerator op Azure Premium Storage-VHD's kunnen worden ingeschakeld.

### <a name="prerequisites"></a>Vereisten

De volgende vereisten gelden op dit moment voor het gebruik van Write Accelerator:

- De schijven die u wilt toepassen Azure Write Accelerator tegen moeten [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/) op Premium Storage.
- U moet gebruiken een VM uit de M-serie

### <a name="enabling-azure-write-accelerator-using-azure-powershell"></a>Azure Write Accelerator met behulp van Azure PowerShell inschakelen

De Azure PowerShell-module van versie 5.5.0 bevatten de wijzigingen in de relevante cmdlets voor het in- of uitschakelen van Write Accelerator voor specifieke Azure Premium Storage-schijven.
Als u wilt in- of schijven die worden ondersteund door Write Accelerator implementeren, hebt u de volgende Power Shell-opdrachten gewijzigd en uitgebreid voor het accepteren van een parameter voor Write Accelerator.

Een nieuwe switchparameter **- WriteAccelerator** is toegevoegd aan de volgende cmdlets:

- [Set-azurermvmosdisk](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/set-azurermvmosdisk?view=azurermps-6.0.0)
- [Add-AzureRmVMDataDisk](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Compute/Add-AzureRmVMDataDisk?view=azurermps-6.0.0)
- [Set-AzureRmVMDataDisk](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Compute/Set-AzureRmVMDataDisk?view=azurermps-6.0.0)
- [Add-AzureRmVmssDataDisk](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Compute/Add-AzureRmVmssDataDisk?view=azurermps-6.0.0)

Geeft niet de parameter wordt de eigenschap ingesteld op false en schijven die geen ondersteuning door Write Accelerator bieden wordt geïmplementeerd.

Een nieuwe switchparameter **- osdiskwriteaccelerator is** is toegevoegd aan de volgende cmdlets:

- [Set-AzureRmVmssStorageProfile](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Compute/Set-AzureRmVmssStorageProfile?view=azurermps-6.0.0)

De parameter niet opgeeft wordt de eigenschap op false standaard schijven die niet gebruikmaken van Write Accelerator retourneren.

Een nieuwe optionele Booleaanse waarde (niet-nullbare) parameter **- osdiskwriteaccelerator is** is toegevoegd aan de volgende cmdlets:

- [Update-AzureRmVM](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Compute/Update-AzureRmVM?view=azurermps-6.0.0)
- [Update-AzureRmVmss](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Compute/Update-AzureRmVmss?view=azurermps-6.0.0)

Geef $true of $false om te bepalen ondersteuning van Azure Write Accelerator met de schijven.

Voorbeelden van opdrachten kunnen er als volgt uitzien:

```PowerShell
New-AzureRmVMConfig | Set-AzureRmVMOsDisk | Add-AzureRmVMDataDisk -Name "datadisk1" | Add-AzureRmVMDataDisk -Name "logdisk1" -WriteAccelerator | New-AzureRmVM

Get-AzureRmVM | Update-AzureRmVM -OsDiskWriteAccelerator $true

New-AzureRmVmssConfig | Set-AzureRmVmssStorageProfile -OsDiskWriteAccelerator | Add-AzureRmVmssDataDisk -Name "datadisk1" -WriteAccelerator:$false | Add-AzureRmVmssDataDisk -Name "logdisk1" -WriteAccelerator | New-AzureRmVmss

Get-AzureRmVmss | Update-AzureRmVmss -OsDiskWriteAccelerator:$false
```

Twee hoofdscenario's kunnen scripts worden gebruikt, zoals wordt weergegeven in de volgende secties.

#### <a name="adding-a-new-disk-supported-by-write-accelerator-using-powershell"></a>Toevoegen van een nieuwe schijf wordt ondersteund door Write Accelerator met behulp van PowerShell

Met dit script kunt u een nieuwe schijf toevoegen aan uw virtuele machine. De schijf gemaakt met dit script maakt gebruik van Write Accelerator.

Vervang `myVM`, `myWAVMs`, `log001`, grootte van de schijf en LunID van de schijf met de waarden die geschikt is voor uw specifieke implementatie.

```PowerShell
# Specify your VM Name
$vmName="myVM"
#Specify your Resource Group
$rgName = "myWAVMs"
#data disk name
$datadiskname = "log001"
#LUN Id
$lunid=8
#size
$size=1023
#Pulls the VM info for later
$vm=Get-AzurermVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Add-AzureRmVMDataDisk -CreateOption empty -DiskSizeInGB $size -Name $vmname-$datadiskname -VM $vm -Caching None -WriteAccelerator:$true -lun $lunid
#Updates the VM with the disk config - does not require a reboot
Update-AzureRmVM -ResourceGroupName $rgname -VM $vm
```

#### <a name="enabling-write-accelerator-on-an-existing-azure-disk-using-powershell"></a>Write Accelerator inschakelt op een bestaande Azure-schijf met behulp van PowerShell

U kunt dit script gebruiken om in te schakelen Write Accelerator op een bestaande schijf. Vervang `myVM`, `myWAVMs`, en `test-log001` met waarden die geschikt is voor uw specifieke implementatie. Het script voegt Write Accelerator toe aan een bestaande schijf waarvan de waarde voor $newstatus is ingesteld op '$true'. Met behulp van de waarde '$false', wordt de Write Accelerator uitgeschakeld op een bepaalde schijf.

```PowerShell
#Specify your VM Name
$vmName="myVM"
#Specify your Resource Group
$rgName = "myWAVMs"
#data disk name
$datadiskname = "test-log001" 
#new Write Accelerator status ($true for enabled, $false for disabled) 
$newstatus = $true
#Pulls the VM info for later
$vm=Get-AzurermVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Set-AzureRmVMDataDisk -VM $vm -Name $datadiskname -Caching None -WriteAccelerator:$newstatus
#Updates the VM with the disk config - does not require a reboot
Update-AzureRmVM -ResourceGroupName $rgname -VM $vm
```

> [!Note]
> Uitvoeren van het bovenstaande script wordt de opgegeven schijf loskoppelen, Write Accelerator inschakelen op basis van de schijf en vervolgens de schijf opnieuw koppelen

### <a name="enabling-write-accelerator-using-the-azure-portal"></a>Inschakelen van Write Accelerator met behulp van de Azure portal

U kunt Write Accelerator inschakelen via de portal waar u de instellingen voor cache schijf opgeven: 

![Write Accelerator in Azure portal](./media/virtual-machines-common-how-to-enable-write-accelerator/wa_scrnsht.png)

### <a name="enabling-write-accelerator-using-the-azure-cli"></a>Inschakelen van Write Accelerator met de Azure CLI

U kunt de [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest) Write Accelerator inschakelen. 

Gebruiken om in te schakelen Write Accelerator op een bestaande schijf, [az vm update](https://docs.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest#az-vm-update), mag u de volgende voorbeelden gebruiken als u de diskName, -VMName en ResourceGroup door uw eigen waarden vervangen: `az vm update -g group1 -n vm1 -write-accelerator 1=true`

Het koppelen van een schijf met Write Accelerator ingeschakeld gebruik [az vm disk attach](https://docs.microsoft.com/en-us/cli/azure/vm/disk?view=azure-cli-latest#az-vm-disk-attach), mag u het volgende voorbeeld gebruiken als u in uw eigen waarden vervangt: `az vm disk attach -g group1 -vm-name vm1 -disk d1 --enable-write-accelerator`

Als u wilt uitschakelen Write Accelerator, gebruikt u [az vm update](https://docs.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest#az-vm-update), de eigenschappen instellen op false: `az vm update -g group1 -n vm1 -write-accelerator 0=false 1=false`

### <a name="enabling-through-rest-apis"></a>Inschakelen via de Rest-API 's

Als u wilt implementeren via Rest API van Azure, moet u de Azure armclient installeren.

#### <a name="install-armclient"></a>Armclient installeren

Om uit te voeren armclient, moet u deze via Chocolatey installeren. U kunt deze installeren via cmd.exe of powershell. Gebruik verhoogde bevoegdheden voor deze opdrachten ('als Administrator uitvoeren").

Met behulp van cmd.exe, voer de volgende opdracht uit: `@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"`

Met behulp van Power Shell, voert u de volgende opdracht uit: `Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

U kunt nu de armclient installeren met behulp van de volgende opdracht uit in cmd.exe of PowerShell `choco install armclient`

#### <a name="getting-your-current-vm-configuration"></a>De huidige configuratie van de virtuele machine ophalen

Als u wilt de kenmerken van de schijfconfiguratie wijzigen, moet u eerst op te halen van de huidige configuratie in een JSON-bestand. U kunt de huidige configuratie krijgen door het uitvoeren van de volgende opdracht: `armclient GET /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 > <<filename.json>>`

De voorwaarden in <> <>vervangen door uw gegevens, inclusief de bestandsnaam die het JSON-bestand moet bevatten.

De uitvoer kan er als volgt uitzien:

```JSON
{
  "properties": {
    "vmId": "2444c93e-f8bb-4a20-af2d-1658d9dbbbcb",
    "hardwareProfile": {
      "vmSize": "Standard_M64s"
    },
    "storageProfile": {
      "imageReference": {
        "publisher": "SUSE",
        "offer": "SLES-SAP",
        "sku": "12-SP3",
        "version": "latest"
      },
      "osDisk": {
        "osType": "Linux",
        "name": "mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a",
        "createOption": "FromImage",
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS",
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a"
        },
        "diskSizeGB": 30
      },
      "dataDisks": [
        {
          "lun": 0,
          "name": "data1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data1"
          },
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
      ]
    },
    "osProfile": {
      "computerName": "mylittlesapVM",
      "adminUsername": "pl",
      "linuxConfiguration": {
        "disablePasswordAuthentication": false
      },
      "secrets": []
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Network/networkInterfaces/mylittlesap518"
        }
      ]
    },
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": true,
        "storageUri": "https://mylittlesapdiag895.blob.core.windows.net/"
      }
    },
    "provisioningState": "Succeeded"
  },
  "type": "Microsoft.Compute/virtualMachines",
  "location": "westeurope",
  "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/virtualMachines/mylittlesapVM",
  "name": "mylittlesapVM"

```

Werk vervolgens de JSON-bestand en Write Accelerator inschakelen op de schijf met de naam 'log1'. Dit kan worden bereikt door dit kenmerk in het JSON-bestand toe te voegen na de vermelding in de cache van de schijf.

```JSON
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          **"writeAcceleratorEnabled": true,**
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
```

Werk vervolgens de bestaande implementatie met de volgende opdracht: `armclient PUT /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 @<<filename.json>>`

De uitvoer moet eruitzien zoals hieronder wordt weergegeven. U kunt zien dat Write Accelerator ingeschakeld voor één schijf.

```JSON
{
  "properties": {
    "vmId": "2444c93e-f8bb-4a20-af2d-1658d9dbbbcb",
    "hardwareProfile": {
      "vmSize": "Standard_M64s"
    },
    "storageProfile": {
      "imageReference": {
        "publisher": "SUSE",
        "offer": "SLES-SAP",
        "sku": "12-SP3",
        "version": "latest"
      },
      "osDisk": {
        "osType": "Linux",
        "name": "mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a",
        "createOption": "FromImage",
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS",
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a"
        },
        "diskSizeGB": 30
      },
      "dataDisks": [
        {
          "lun": 0,
          "name": "data1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data1"
          },
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          **"writeAcceleratorEnabled": true,**
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
      ]
    },
    "osProfile": {
      "computerName": "mylittlesapVM",
      "adminUsername": "pl",
      "linuxConfiguration": {
        "disablePasswordAuthentication": false
      },
      "secrets": []
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Network/networkInterfaces/mylittlesap518"
        }
      ]
    },
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": true,
        "storageUri": "https://mylittlesapdiag895.blob.core.windows.net/"
      }
    },
    "provisioningState": "Succeeded"
  },
  "type": "Microsoft.Compute/virtualMachines",
  "location": "westeurope",
  "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/virtualMachines/mylittlesapVM",
  "name": "mylittlesapVM"
```

Nadat u deze wijziging hebt aangebracht, kan het station moet worden ondersteund door Write Accelerator.