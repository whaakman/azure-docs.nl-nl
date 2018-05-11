---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: msraiye
ms.service: virtual-machines
ms.topic: include
ms.date: 5/9/2018
ms.author: raiye
ms.custom: include file
ms.openlocfilehash: 4db9fe907ab6625fcad74ceae59f17115458a3ea
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="write-accelerator"></a>Schrijven van de Accelerator
Geschreven dat Accelerator is een schijf-functionaliteit voor M-serie virtuele Machines (VM's) op Premium-opslag met Azure beheerd schijven uitsluitend. Als de naam van de staat, is het doel van de functionaliteit voor het verbeteren van de i/o-latentie van schrijfbewerkingen op basis van Azure Premium-opslag. Schrijven dat Accelerator is ideaal wanneer log-bestandsupdates nodig zijn voor het persistent maken op schijf op een maximaal zodat manier voor moderne databases.

Schrijven dat Accelerator is algemeen beschikbaar voor virtuele machines M-serie in de openbare Cloud.

## <a name="planning-for-using-write-accelerator"></a>Planning voor het gebruik van de Accelerator schrijven
Schrijven dat Accelerator voor de volumes die het transactielogboek bevatten of Logboeken van een DBMS opnieuw moet worden gebruikt. Het is niet raadzaam Accelerator schrijven als de functie is geoptimaliseerd voor tegen logboek schijven worden gebruikt voor de volumes van een DBMS gebruikt.

Schrijven Accelerator werkt alleen in combinatie met [schijven die worden beheerd Azure](https://azure.microsoft.com/services/managed-disks/). 


> [!IMPORTANT]
> Als u wilt in- of uitschakelen van de Accelerator schrijven voor een bestaand volume die wordt samengesteld uit meerdere Azure Premium-opslag-schijven en striped verdeeld met behulp van de schijf of volume managers Windows, Windows Storage Spaces, Scale-out van de Windows-bestandsserver (SOFS) Linux LVM of MDADM, alle schijven voor het bouwen van het volume moeten worden ingeschakeld of uitgeschakeld voor Accelerator schrijven in de afzonderlijke stappen. **Afgesloten voordat het in- of uitschakelen van Accelerator schrijven in een dergelijke configuratie, de Azure VM**. 


> [!IMPORTANT]
> Accelerator schrijven naar een bestaande Azure-schijf die geen deel uitmaakt van een build van het volume uit meerdere schijven met Windows-schijf of volume managers, Windows Storage Spaces, Scale-out van de Windows-bestandsserver (SOFS) Linux LVM, of MDADM, de werkbelasting die toegang tot de schijf van Azure inschakelen moet worden afgesloten. Databasetoepassingen met behulp van de schijf van Azure moeten worden afgesloten.

> [!IMPORTANT]
> Inschakelen van de Accelerator schrijven voor de besturingssysteemschijf van de virtuele machine, wordt de virtuele machine opnieuw opgestart. 

VM-configuraties schrijven Accelerator inschakelt voor OS schijven mag niet nodig zijn voor SAP zijn gerelateerd

### <a name="restrictions-when-using-write-accelerator"></a>Beperkingen bij het gebruik van de Accelerator schrijven
Wanneer u de Accelerator schrijven voor een Azure-schijf/VHD, gelden deze beperkingen:

- Schijfcache Premium moet worden ingesteld op 'None' of 'Alleen-lezen'. Alle andere gebruikersmoduscache modi worden niet ondersteund.
- Momentopname op de schijf schrijft Accelerator ingeschakeld is nog niet ondersteund. Deze beperking blokkeert Azure Backup-Service de mogelijkheid om uit te voeren van een toepassing consistente momentopname te maken van alle schijven van de virtuele machine.
- Alleen i/o-kleinere (< = 32KiB) duurt het versnelde pad. In de werkbelasting situaties waarbij bulksgewijs is het ophalen van gegevens worden geladen of wanneer de buffers transactie logboek van de verschillende DBMS worden gevuld in een grotere mate voordat het ophalen van persistent worden gemaakt voor de opslag, kans op die zijn de i/o geschreven naar schijf neemt de versnelde pad niet.

Er zijn limieten van Azure Premium Storage VHD's per virtuele machine die kan worden ondersteund door de Accelerator schrijven. De huidige limieten zijn:

| VM-SKU | Aantal schijven Accelerator schrijven | Schrijven van Accelerator schijf IOP's per VM |
| --- | --- | --- |
| M128ms | 16 | 8000 |
| M128s | 16 | 8000 |
| M64ms | 8 | 4000 |
| M64s | 8 | 4000 | 

## <a name="enabling-write-accelerator-on-a-specific-disk"></a>Schrijven Accelerator inschakelen op een specifieke schijf
De volgende secties wordt beschreven hoe Accelerator schrijven op Azure Premium Storage VHD's kunnen worden ingeschakeld.


### <a name="prerequisites"></a>Vereisten
De volgende vereisten toepassen op dit moment op het gebruik van de Accelerator schrijven:

- De schijven die u wilt toepassen Azure schrijven Accelerator tegen moeten worden [schijven die worden beheerd Azure](https://azure.microsoft.com/services/managed-disks/) op Premium-opslag.
- Moet u een VM M-serie

### <a name="enabling-through-power-shell"></a>Inschakelen via Power Shell
De Azure Power Shell-module van versie 5.5.0 bevatten de wijzigingen in de relevante cmdlets of schrijven Accelerator voor specifieke Azure Premium-opslag-schijven uit te schakelen.
Om te kunnen inschakelen of schijven wordt ondersteund door de Accelerator schrijven implementeert, hebt u de volgende Power Shell-opdrachten gewijzigd en uitgebreid met een parameter accepteren voor Accelerator schrijven.

Een nieuwe switch-parameter 'WriteAccelerator' is toegevoegd aan de volgende cmdlets: 

- Set-AzureRmVMOsDisk
- Add-AzureRmVMDataDisk
- Set-AzureRmVMDataDisk
- Add-AzureRmVmssDataDisk

Geeft niet de parameter wordt de eigenschap ingesteld op false en schijven die geen ondersteuning door schrijven Accelerator bieden gaat implementeren.

Een nieuwe switch-parameter 'OsDiskWriteAccelerator' is toegevoegd aan de volgende cmdlets: 

- Set-AzureRmVmssStorageProfile

Geeft niet de parameter worden de eigenschap ingesteld op false en levert schijven die geen gebruik maken van Accelerator schrijven.

Een nieuwe optionele Booleaanse (niet-nullbare) parameter 'OsDiskWriteAccelerator' is toegevoegd aan de volgende cmdlets: 

- Update-AzureRmVM
- Update-AzureRmVmss

Geef $true of $false om te bepalen van de ondersteuning van Azure schrijven Accelerator met de schijven.

Voorbeelden van opdrachten kunnen eruitzien als:

```

New-AzureRmVMConfig | Set-AzureRmVMOsDisk | Add-AzureRmVMDataDisk -Name "datadisk1" | Add-AzureRmVMDataDisk -Name "logdisk1" -WriteAccelerator | New-AzureRmVM

Get-AzureRmVM | Update-AzureRmVM -OsDiskWriteAccelerator $true

New-AzureRmVmssConfig | Set-AzureRmVmssStorageProfile -OsDiskWriteAccelerator | Add-AzureRmVmssDataDisk -Name "datadisk1" -WriteAccelerator:$false | Add-AzureRmVmssDataDisk -Name "logdisk1" -WriteAccelerator | New-AzureRmVmss

Get-AzureRmVmss | Update-AzureRmVmss -OsDiskWriteAccelerator:$false 

```

Twee hoofdscenario's kunnen scripts worden gebruikt, zoals wordt weergegeven in de volgende secties.

#### <a name="adding-a-new-disk-supported-by-write-accelerator"></a>Toevoegen van een nieuwe schijf wordt ondersteund door de Accelerator schrijven
Dit script kunt u een nieuwe schijf toevoegen aan uw virtuele machine. De schijf die is gemaakt met dit script wilt schrijven sneltoets gebruiken.

```

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
U moet de namen van de VM, schijf, resourcegroep of grootte van de schijf en LunID van de schijf voor uw specifieke implementatie aanpassen.


#### <a name="enabling-azure-write-accelerator-on-an-existing-azure-disk"></a>Azure schrijven Accelerator inschakelen op een bestaande Azure-schijf
Als u schrijven Accelerator inschakelen op een bestaande schijf wilt, kunt u dit script gebruiken de taak uit te voeren:

```

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

U moet de namen van de VM-, schijf- en resourcegroep aanpassen. Het bovenstaande script voegt Accelerator schrijven toe aan een bestaande schijf waarvan de waarde voor $newstatus is ingesteld op '$true'. Met behulp van de waarde '$false', wordt de Accelerator schrijven uitgeschakeld op een bepaalde schijf.

> [!Note]
> Uitvoeren van het bovenstaande script wordt Ontkoppel de schijf die is opgegeven, Accelerator schrijven tegen de schijf inschakelen en vervolgens de schijf opnieuw koppelen

### <a name="enabling-through-azure-portal"></a>Inschakelen via Azure Portal

U kunt de Accelerator schrijven via de Portal waar u uw instellingen voor de schijfcache opgeven inschakelen: 

![Accelerator schrijven in de Azure Portal](./media/virtual-machines-common-how-to-enable-write-accelerator/wa_scrnsht.png)

### <a name="enabling-through-azure-cli"></a>Inschakelen via Azure CLI
U kunt de [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest) schrijven Accelerator inschakelen. 

Gebruik de opdracht onder vervangen door de diskName, VMName en ResourceGroup voor uw eigen zodat Accelerator schrijven op een bestaande schijf: 
```
az vm update -g group1 -n vm1 –write-accelerator 1=true
```
Het koppelen van een schijf met schrijven Accelerator ingeschakeld Neem gebruik de onderstaande opdracht met de waarden:
```
az vm disk attach -g group1 –vm-name vm1 –disk d1 --enable-write-accelerator
```
Als u wilt uitschakelen Accelerator schrijven, de eigenschap in te stellen op false: 
```
az vm update -g group1 -n vm1 –write-accelerator 0=false 1=false
```

### <a name="enabling-through-rest-apis"></a>Inschakelen via de Rest-API 's
Om te implementeren via Rest API van Azure, moet u de Azure armclient installeren

#### <a name="install-armclient"></a>Armclient installeren

Armclient, hebt u nodig om deze te installeren via Chocolatey. U kunt deze installeren via cmd.exe of powershell. Gebruik verhoogde rechten voor deze opdrachten ('als Administrator uitvoeren').

Voer de volgende opdracht cmd.exe gebruiken:

```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

Met behulp van Power Shell die u moet gebruiken:

```
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Nu kunt u de armclient installeren met de opdracht hieronder in cmd.exe of Power Shell

```
choco install armclient
```

#### <a name="getting-your-current-vm-configuration"></a>Uw huidige configuratie van de virtuele machine ophalen
Om de kenmerken van de schijfconfiguratie wijzigt, moet u eerst de huidige configuratie in een JSON-bestand ophalen. U kunt de huidige configuratie krijgen door het uitvoeren van de volgende opdracht:

```
armclient GET /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 > <<filename.json>>
```
De voorwaarden in <> <>vervangen door uw gegevens, inclusief de bestandsnaam die de JSON-bestand hebt.

De uitvoer kan eruitzien als:

```
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

Volgende stap is het bijwerken van het JSON-bestand en Accelerator schrijven inschakelen op de schijf met de naam 'log1'. Deze stap kunt u doen door dit kenmerk in het JSON-bestand toe te voegen na de vermelding in de cache van de schijf. 

```
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

Werk vervolgens de bestaande implementatie met deze opdracht:

```
armclient PUT /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 @<<filename.json>>

```

De uitvoer moet eruitzien als hieronder. U ziet dat er schrijven Accelerator ingeschakeld voor één schijf.

```
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

Vanaf het moment van de wijziging op, moet het station worden ondersteund door de Accelerator schrijven.

 
