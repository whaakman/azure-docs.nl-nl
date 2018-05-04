---
title: Azure schrijven Accelerator voor SAP-oplossingen | Microsoft Docs
description: Bedieningshandleiding voor SAP HANA-systemen die zijn geïmplementeerd op virtuele machines in Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 89a4216a3893892eedd6c216c7e0e5d51cf64749
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2018
---
# <a name="azure-write-accelerator-for-sap-deployments"></a>Azure schrijven Accelerator voor SAP-implementaties
Azure schrijven Accelerator is een functionaliteit die is opgehaald uitgerold voor virtuele machines M-serie uitsluitend. De Accelerator voor het schrijven van Azure is niet beschikbaar bij alle andere VM-reeks in Azure, met uitzondering van de M-serie. Als de naam van de staat, is het doel van de functionaliteit voor het verbeteren van i/o-latentie van schrijfbewerkingen op basis van de Azure Premium-opslag. 

De Accelerator voor Azure schrijven-functionaliteit is beschikbaar voor implementatie van de M-serie als openbare preview in:

- West US2
- Oost-US2
- West-Europa
- Zuidoost-Azië

## <a name="planning-for-using-azure-write-accelerator"></a>Planning voor het gebruik van Azure schrijven Accelerator
Azure schrijven Accelerator moet worden gebruikt voor de volumes die het transactielogboek bevatten of Logboeken van een DBMS opnieuw. Het is niet raadzaam Accelerator voor Azure schrijven voor de volumes van een DBMS gebruiken. Reden voor deze beperking is dat Azure schrijven Accelerator de Azure Premium Storage VHD's te koppelen zonder de extra lees-caching die beschikbaar is voor Premium-opslag is vereist. Grotere voordelen met dit type opslaan in cache worden geconstateerd bij traditionele databases. Aangezien Accelerator schrijven alleen invloed op de activiteiten schrijven en geen leesbewerkingen versnellen, is het ontwerp van de ondersteunde voor SAP schrijven Accelerator gebruiken op basis van het transactielogboek of stations van databases ondersteund SAP opnieuw. 

Azure schrijven Accelerator werkt alleen in combinatie met [schijven die worden beheerd Azure](https://azure.microsoft.com/services/managed-disks/). U moet daarom dienovereenkomstig plannen. 

>[!NOTE]
> Als de functionaliteit van Azure schrijven Accelerator nog steeds in de openbare preview is, worden geen productie scenario-implementaties ondersteund op de functionaliteit nog.

Er zijn limieten van Azure Premium Storage VHD's per virtuele machine die kan worden ondersteund door Azure schrijven Accelerator. De huidige limieten zijn:


| VM-SKU | Aantal schijven Accelerator schrijven | Schrijven van Accelerator IOP's per VM |
| --- | --- | --- |
| M128ms | 16 | 8000 |
| M128s | 16 | 8000 |
| M64ms | 8 | 4000 |
| M64s | 8 | 4000 | 



> [!IMPORTANT]
> Als u wilt in- of uitschakelen van Azure schrijven Accelerator voor een bestaand volume die wordt samengesteld uit meerdere Azure Premium-opslag-schijven en striped verdeeld met behulp van de schijf of volume managers Windows, Windows Storage Spaces, Scale-out van de Windows-bestandsserver (SOFS) Linux LVM of MDADM, alle schijven voor het bouwen van het volume moet worden ingeschakeld of uitgeschakeld voor Accelerator schrijven in de afzonderlijke stappen. **Afgesloten voordat het in- of uitschakelen van Accelerator schrijven in een dergelijke configuratie, de Azure VM**. 


> [!IMPORTANT]
> Inschakelen van Azure schrijven versneller aan een bestaande Azure-schijf die geen deel uit van een volume build uit meerdere schijven met Windows-schijf of volume managers, opslagruimten Windows, Windows Scale-out bestandsserver (SOFS), Linux LVM of MDADM, is de werkbelasting toegang tot de Azure-schijf moet worden afgesloten. Databasetoepassingen met behulp van de schijf van Azure moeten worden afgesloten.

> [!IMPORTANT]
> Inschakelen van schrijven Accelerator voor virtuele machine van Azure-besturingssysteemschijf van de virtuele machine, wordt de virtuele machine opnieuw opgestart. 

Azure schrijven Accelerator inschakelt voor schijven besturingssysteem niet nodig voor SAP zijn mag gerelateerde configuraties van virtuele machine

### <a name="restrictions-when-using-azure-write-accelerator"></a>Beperkingen bij het gebruik van Azure schrijven Accelerator
Wanneer u Azure schrijven Accelerator voor een Azure-schijf/VHD, gelden de volgende beperkingen:

- Schijfcache Premium moet worden ingesteld op 'None' of 'Alleen-lezen'. Alle andere gebruikersmoduscache modi worden niet ondersteund.
- Momentopname op de schijf schrijft Accelerator ingeschakeld is nog niet ondersteund. Deze beperking blokkeert Azure Backup-Service de mogelijkheid om uit te voeren van een toepassing consistente momentopname te maken van alle schijven van de virtuele machine.
- Alleen i/o-kleinere duurt het versnelde pad. In de werkbelasting situaties waarbij bulksgewijs is het ophalen van gegevens worden geladen of wanneer de buffers transactie logboek van de verschillende DBMS worden gevuld in een grotere mate voordat het ophalen van persistent worden gemaakt voor de opslag, kans op die zijn de i/o geschreven naar schijf neemt de versnelde pad niet.


## <a name="enabling-write-accelerator-on-a-specific-disk"></a>Schrijven Accelerator inschakelen op een specifieke schijf
De volgende secties wordt beschreven hoe Azure schrijven Accelerator op Azure Premium Storage VHD's kunnen worden ingeschakeld.

Op dit moment, Accelerator schrijven via Rest API van Azure en Power Shell inschakelen, zijn de enige methoden. Andere methoden tot ondersteuning in Azure portal volgen in de loop van de volgende enkele weken.

### <a name="prerequisites"></a>Vereisten
Op dit moment toepassen de volgende vereisten voor het gebruik van Azure schrijven Accelerator:

- Uw abonnements-ID die is gebruikt voor het implementeren van virtuele machine en opslag voor de virtuele machine moet wit vermeld. Neem contact op met uw Microsoft-CSA, GBB, of wit accountmanager uw abonnements-ID ophalen-vermeld. 
- De schijven die u wilt toepassen Azure schrijven Accelerator tegen moeten worden [schijven die worden beheerd Azure](https://azure.microsoft.com/services/managed-disks/).

### <a name="enabling-through-power-shell"></a>Inschakelen via Power Shell
De Azure Power Shell-module van versie 5.5.0 bevatten de wijzigingen in de relevante cmdlets voor het in- of uitschakelen van Azure schrijven Accelerator voor schijven met specifieke Azure Premium-opslag.
Om te kunnen inschakelen of schijven wordt ondersteund door de Accelerator schrijven implementeert, hebt u de volgende Power Shell-opdrachten gewijzigd en uitgebreid met een parameter accepteren voor Accelerator schrijven.

Een nieuwe switch-parameter 'WriteAccelerator' is toegevoegd aan de volgende cmdlets: 

- Set-AzureRmVMOsDisk
- Add-AzureRmVMDataDisk
- Set-AzureRmVMDataDisk
- Add-AzureRmVmssDataDisk

Geeft niet de parameter wordt de eigenschap ingesteld op false en schijven die geen ondersteuning door Azure schrijven Accelerator bieden gaat implementeren.

Een nieuwe switch-parameter 'OsDiskWriteAccelerator' is toegevoegd aan de volgende cmdlets: 

- Set-AzureRmVmssStorageProfile

Niet de eigenschap van de sets geeft op false en schijven die geen gebruik maken van Azure schrijven Accelerator levert.

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

#### <a name="adding--new-disk-supported-by-azure-write-accelerator"></a>Het toevoegen van nieuwe schijf wordt ondersteund door Azure schrijven Accelerator
Dit script kunt u een nieuwe schijf toevoegen aan uw virtuele machine. De schijf die is gemaakt met dit script gaat gebruiken de Accelerator voor Azure schrijven.

```

# Specify your VM Name
$vmName="mysapVM"
#Specify your Resource Group
$rgName = "mysap"
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
$vmName="mysapVM"
#Specify your Resource Group
$rgName = "mysap"
#data disk name
$datadiskname = "testsap-log001" 
#new Write Accelerator status ($true for enabled, $false for disabled) 
$newstatus = $true
#Pulls the VM info for later
$vm=Get-AzurermVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Set-AzureRmVMDataDisk -VM $vm -Name $datadiskname -Caching None -WriteAccelerator:$newstatus
#Updates the VM with the disk config - does not require a reboot
Update-AzureRmVM -ResourceGroupName $rgname -VM $vm

```

U moet de namen van de VM-, schijf- en resourcegroep aanpassen. Het bovenstaande script voegt dat Accelerator schrijven naar een bestaande schijf is de waarde voor $newstatus is ingesteld op '$true'. Met behulp van de waarde '$false', wordt de Accelerator schrijven uitgeschakeld op een bepaalde schijf.

> [!Note]
> Uitvoeren van het bovenstaande script wordt Ontkoppel de schijf die is opgegeven, Accelerator schrijven tegen de schijf inschakelen en vervolgens de schijf opnieuw koppelen




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

 