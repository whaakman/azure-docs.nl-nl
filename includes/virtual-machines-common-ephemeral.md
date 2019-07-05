---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 05/02/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 24c2bfa4aae94642d3ed66f2cfa6e31ba1e6b19a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67457304"
---
Kortstondige OS-schijven worden gemaakt op de lokale opslag van de virtuele Machine (VM) en niet persistent gemaakt met de externe Azure-opslag. Tijdelijke besturingssysteemschijven werken goed bij staatloze werkbelastingen moeten waarbij toepassingen zijn gevoelig voor afzonderlijke VM-fouten, maar zijn meer betrokken zijn bij de tijd die nodig is voor grootschalige implementaties of tijd aan de installatiekopie van de afzonderlijke VM-exemplaren terugzetten. Het is ook geschikt voor toepassingen die zijn geïmplementeerd met behulp van het klassieke implementatiemodel naar het Resource Manager-implementatiemodel. Met Ephemeral OS Disk krijgt u een lagere lees/schrijf-wachttijd voor de OS-schijf en een snellere terugzetting van de VM-installatiekopie. Bovendien kortstondige besturingssysteemschijf gratis is, worden er geen opslagkosten voor de besturingssysteemschijf. 
 
De belangrijkste functies van tijdelijke schijven zijn: 
- Ze kunnen worden gebruikt met zowel Marketplace-installatiekopieën en aangepaste installatiekopieën.
- U kunt VM-installatiekopieën implementeren tot aan de grootte van de VM-Cache.
- De mogelijkheid snel opnieuw instellen of hun VM's naar de oorspronkelijke opstarten status installatiekopie terugzetten.  
- Lagere runtime-latentie die vergelijkbaar is met een tijdelijke schijf. 
- Er zijn geen kosten voor de besturingssysteemschijf. 
 
 
Belangrijke verschillen tussen permanente en tijdelijke besturingssysteemschijven:

|                             | Permanente OS-schijf                          | Kortstondige besturingssysteemschijf                              |    |
|-----------------------------|---------------------------------------------|------------------------------------------------|
| De maximale grootte voor de besturingssysteemschijf      | 2 TiB                                                                                        | In de cache-grootte voor de VM-grootte of 2TiB, afhankelijk van wat kleiner - is [DS](../articles/virtual-machines/linux/sizes-general.md), [ES](../articles/virtual-machines/linux/sizes-memory.md), [M](../articles/virtual-machines/linux/sizes-memory.md), [FS](../articles/virtual-machines/linux/sizes-compute.md), en [GS](../articles/virtual-machines/linux/sizes-memory.md)              |
| Ondersteunde VM-grootten          | Alle                                                                                          | DSv1, DSv2, DSv3, Esv3, Fs, FsV2, GS, M                                               |
| Ondersteuning voor het type van schijven           | Beheerde en onbeheerde schijf met besturingssysteem                                                                | Alleen beheerde OS-schijf                                                               |
| Ondersteuning voor regio              | Alle regio 's                                                                                  | Alle regio 's                              |
| Gegevenspersistentie            | OS-schijfgegevens geschreven naar de schijf met besturingssysteem zijn opgeslagen in Azure Storage                                  | Gegevens geschreven naar de schijf met besturingssysteem wordt opgeslagen in de lokale VM-opslag en is niet persistent gemaakt met Azure Storage. |
| Stop-deallocated hebben      | VM's en schaalsetinstanties kunnen stop-toewijzing ongedaan gemaakt en opnieuw opgestart vanaf de status stop-toewijzing ongedaan gemaakt | VM's en schaalsetinstanties kunnen niet worden stop-toewijzing ongedaan gemaakt                                  |
| Ondersteuning voor gespecialiseerde OS-schijven | Ja                                                                                          | Nee                                                                                 |
| Onlineformaatswijziging van OS-schijven              | Ondersteunde VM wordt gemaakt en nadat de virtuele machine is stop-toewijzing ongedaan gemaakt                                | Tijdens het maken van virtuele machine alleen wordt ondersteund                                                  |
| Formaat wijzigen naar een nieuwe VM-grootte   | OS-schijfgegevens blijven behouden                                                                    | Gegevens op de besturingssysteemschijf is verwijderd, is OS opnieuw worden ingericht                                      |

## <a name="scale-set-deployment"></a>Virtual Machine Scale sets implementatie  
Het proces voor het maken van een schaalset die gebruikmaakt van een kortstondige besturingssysteemschijf is het toevoegen van de `diffDiskSettings` eigenschap in op de `Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile` brontype in de sjabloon. Het cachebeleid moet ook worden ingesteld op `ReadOnly` voor de kortstondige besturingssysteemschijf. 


```json
{ 
  "type": "Microsoft.Compute/virtualMachineScaleSets", 
  "name": "myScaleSet", 
  "location": "East US 2", 
  "apiVersion": "2018-06-01", 
  "sku": { 
    "name": "Standard_DS2_v2", 
    "capacity": "2" 
  }, 
  "properties": { 
    "upgradePolicy": { 
      "mode": "Automatic" 
    }, 
    "virtualMachineProfile": { 
       "storageProfile": { 
        "osDisk": { 
          "diffDiskSettings": { 
                "option": "Local" 
          }, 
          "caching": "ReadOnly", 
          "createOption": "FromImage" 
        }, 
        "imageReference":  { 
          "publisher": "Canonical", 
          "offer": "UbuntuServer", 
          "sku": "16.04-LTS", 
          "version": "latest" 
        } 
      }, 
      "osProfile": { 
        "computerNamePrefix": "myvmss", 
        "adminUsername": "azureuser", 
        "adminPassword": "P@ssw0rd!" 
      } 
    } 
  } 
}  
```

## <a name="vm-deployment"></a>VM-implementatie 
U kunt een virtuele machine implementeren met een kortstondige besturingssysteemschijf met behulp van een sjabloon. Het proces voor het maken van een virtuele machine die gebruikmaakt van kortstondige OS-schijven is het toevoegen van de `diffDiskSettings` eigenschap aan het brontype Microsoft.Compute/virtualMachines in de sjabloon. Het cachebeleid moet ook worden ingesteld op `ReadOnly` voor de kortstondige besturingssysteemschijf. 

```json
{ 
  "type": "Microsoft.Compute/virtualMachines", 
  "name": "myVirtualMachine", 
  "location": "East US 2", 
  "apiVersion": "2018-06-01", 
  "properties": { 
       "storageProfile": { 
            "osDisk": { 
              "diffDiskSettings": { 
                "option": "Local" 
              }, 
              "caching": "ReadOnly", 
              "createOption": "FromImage" 
            }, 
            "imageReference": { 
                "publisher": "MicrosoftWindowsServer", 
                "offer": "WindowsServer", 
                "sku": "2016-Datacenter-smalldisk", 
                "version": "latest" 
            }, 
            "hardwareProfile": { 
                 "vmSize": "Standard_DS2_v2" 
             } 
      }, 
      "osProfile": { 
        "computerNamePrefix": "myvirtualmachine", 
        "adminUsername": "azureuser", 
        "adminPassword": "P@ssw0rd!" 
      } 
    } 
 } 
```


## <a name="reimage-a-vm-using-rest"></a>Installatiekopie terugzetten van een virtuele machine met behulp van REST
Tot en met behulp van REST-API is momenteel de enige methode om nieuwe installatiekopie van een virtuele Machine-exemplaar met een kortstondige besturingssysteemschijf. Voor schaalsets is installatiekopie terugzetten al beschikbaar via Powershell, CLI en de portal.

```
POST https://management.azure.com/subscriptions/{sub-
id}/resourceGroups/{rgName}/providers/Microsoft.Compute/VirtualMachines/{vmName}/reimage?a pi-version=2018-06-01" 
```
 
## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**V: Wat is de grootte van de lokale Besturingssysteemschijven?**

A: Voor Preview-versie wordt ook ondersteuning voor platform en/of afbeeldingen tot de VM-cachegrootte, waar alle lezen/schrijven naar de schijf met het besturingssysteem zich lokaal op hetzelfde knooppunt als de virtuele Machine. 

**V: Kan de kortstondige besturingssysteemschijf worden gewijzigd?**

A: Zodra de kortstondige besturingssysteemschijf is ingericht, kunnen de besturingssysteemschijf niet het geval is, kan niet worden gewijzigd. 

**V: Kan ik een Managed Disks koppelen aan een tijdelijke virtuele machine?**

A: Ja, kunt u een beheerde gegevensschijf koppelen aan een virtuele machine die gebruikmaakt van een kortstondige besturingssysteemschijf. 

**V: Alle VM-grootten worden ondersteund bij een kortstondige besturingssysteemschijf?**

A: Nee, alle Premium Storage VM-grootten zijn ondersteunde (DS, ES, FS, GS en M), behalve de B-serie, N-serie en H-serie beschikbaar.  
 
**V: Kan de kortstondige besturingssysteemschijf worden toegepast op bestaande virtuele machines en schaalsets?**

A: Nee, tijdelijke besturingssysteemschijf kan alleen worden gebruikt tijdens de virtuele machine en het maken van een schalen. 

**V: Kunt u kortstondige en normale OS-schijven in een schaalset combineren?**

A: Nee, geen een combinatie van tijdelijke en permanente OS schijf-instanties binnen dezelfde scale set. 

**V: Kan worden de kortstondige besturingssysteemschijf gemaakt met Powershell of CLI?**

A: Ja, u kunt virtuele machines maken met een kortstondige Besturingssysteemschijf met behulp van REST, sjablonen, PowerShell en CLI.

**V: Welke functies worden niet ondersteund met een kortstondige besturingssysteemschijf?**

A: Tijdelijke schijven bieden geen ondersteuning:
- Vastleggen van VM-installatiekopieën
- Momentopnamen van schijven 
- Azure Disk Encryption 
- Azure Backup
- Azure Site Recovery  
- Wisselen van Besturingssysteemschijf 
