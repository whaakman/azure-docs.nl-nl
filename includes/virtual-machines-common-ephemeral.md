---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: a57335eccfce1e81fe0cc85ae6fa7b12aa27e1c3
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67805868"
---
Kortstondige besturingssysteemschijven worden gemaakt op de lokale virtuele machine (VM)-opslag en niet opgeslagen in de externe Azure-opslag. Tijdelijke besturingssysteemschijven werkt goed voor staatloze werkbelastingen, wanneer toepassingen gevoelig voor afzonderlijke VM-fouten zijn, maar zijn meer beïnvloed door de implementatietijd van virtuele machine of installatiekopieën terug van de afzonderlijke VM-exemplaren. Met een kortstondige besturingssysteemschijf krijgt u lagere latentie voor lezen/schrijven naar de besturingssysteemschijf en snellere VM terugzetten. 
 
De belangrijkste functies van tijdelijke schijven zijn: 
- Ideaal voor stateless toepassingen.
- Ze kunnen worden gebruikt met zowel de Marketplace en de aangepaste installatiekopieën.
- Mogelijkheid om snel opnieuw instellen of terugzetten van een installatiekopie VM's en schaal ingesteld exemplaren op de oorspronkelijke toestand bij het opstarten.  
- Lagere latentie, vergelijkbaar met een tijdelijke schijf. 
- Tijdelijke besturingssysteemschijven zijn gratis, worden er geen opslagkosten voor de besturingssysteemschijf.
- Ze zijn beschikbaar in alle Azure-regio's. 
- Kortstondige Besturingssysteemschijf wordt ondersteund door [gedeelde afbeeldingengalerie](/azure/virtual-machines/linux/shared-image-galleries). 
 

 
Belangrijke verschillen tussen permanente en tijdelijke besturingssysteemschijven:

|                             | Permanente OS-schijf                          | Kortstondige besturingssysteemschijf                              |    |
|-----------------------------|---------------------------------------------|------------------------------------------------|
| De maximale grootte voor de besturingssysteemschijf      | 2 TiB                                                                                        | In de cache-grootte voor de VM-grootte of 2TiB, afhankelijk van wat kleiner is. Voor de **cachegrootte in GiB**, Zie [DS](../articles/virtual-machines/linux/sizes-general.md), [ES](../articles/virtual-machines/linux/sizes-memory.md), [M](../articles/virtual-machines/linux/sizes-memory.md), [FS](../articles/virtual-machines/linux/sizes-compute.md), en [GS](/azure/virtual-machines/linux/sizes-previous-gen#gs-series)              |
| Ondersteunde VM-grootten          | Alle                                                                                          | DSv1, DSv2, DSv3, Esv3, Fs, FsV2, GS, M                                               |
| Ondersteuning voor het type van schijven           | Beheerde en onbeheerde schijf met besturingssysteem                                                                | Alleen beheerde OS-schijf                                                               |
| Ondersteuning voor regio              | Alle regio 's                                                                                  | Alle regio 's                              |
| Gegevenspersistentie            | OS-schijfgegevens geschreven naar de schijf met besturingssysteem zijn opgeslagen in Azure Storage                                  | Gegevens geschreven naar de schijf met besturingssysteem wordt opgeslagen in de lokale VM-opslag en is niet persistent gemaakt met Azure Storage. |
| Stop-deallocated hebben      | VM's en schaalsetinstanties kunnen stop-toewijzing ongedaan gemaakt en opnieuw opgestart vanaf de status stop-toewijzing ongedaan gemaakt | VM's en schaalsetinstanties kunnen niet worden stop-toewijzing ongedaan gemaakt                                  |
| Ondersteuning voor gespecialiseerde OS-schijven | Ja                                                                                          | Nee                                                                                 |
| Onlineformaatswijziging van OS-schijven              | Ondersteunde VM wordt gemaakt en nadat de virtuele machine is stop-toewijzing ongedaan gemaakt                                | Tijdens het maken van virtuele machine alleen wordt ondersteund                                                  |
| Formaat wijzigen naar een nieuwe VM-grootte   | OS-schijfgegevens blijven behouden                                                                    | Gegevens op de besturingssysteemschijf is verwijderd, is OS opnieuw worden ingericht                                      |

## <a name="size-requirements"></a>Grootte-vereisten

U kunt installatiekopieën van virtuele machine en het exemplaar tot aan de grootte van de cache van de virtuele machine kunt implementeren. Standaard Windows Server-installatiekopieën van marketplace zijn bijvoorbeeld over 127 GiB, wat betekent dat u een VM-grootte waarvoor een cache die groter zijn dan 127 GiB nodig. In dit geval de [Standard_DS2_v2](/azure/virtual-machines/windows/sizes-general#dsv2-series) heeft een cachegrootte van 86 GiB die niet groot genoeg is. De Standard_DS2_v2 heeft een cachegrootte van 172 GiB die groot genoeg is. In dit geval is de Standard_DS3_v2 de kleinste grootte in de DSv2-serie die u met deze installatiekopie kunt gebruiken. Basic Linux-installatiekopieën in de Marketplace en Windows Server-installatiekopieën die worden aangeduid met `[smallsize]` zijn meestal ongeveer 30 GiB en de meeste van de beschikbare VM-grootten kunt gebruiken.

Tijdelijke schijven is ook vereist dat de VM-grootte Premium-opslag ondersteunt. De grootte meestal (maar niet altijd) hebben een `s` in de naam, zoals DSv2 en EsV3. Zie voor meer informatie, [Azure VM-grootten](../articles/virtual-machines/linux/sizes.md) voor meer informatie over welke grootten ondersteuning voor Premium storage.

## <a name="powershell"></a>PowerShell

U kunt een kortstondige schijf voor een PowerShell-VM-implementatie gebruiken [Set AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk) in uw VM-configuratie. Stel de `-DiffDiskSetting` naar `Local` en `-Caching` naar `ReadOnly`.     

```powershell
Set-AzVMOSDisk -DiffDiskSetting Local -Caching ReadOnly
```

Voor implementaties-schaalset, gebruikt u de [Set AzVmssStorageProfile](/powershell/module/az.compute/set-azvmssstorageprofile) cmdlet in uw configuratie. Stel de `-DiffDiskSetting` naar `Local` en `-Caching` naar `ReadOnly`.


```powershell
Set-AzVmssStorageProfile -DiffDiskSetting Local -OsDiskCaching ReadOnly
```

## <a name="cli"></a>CLI

Met een kortstondige schijf voor de implementatie van een CLI-VM, stelt de `--ephemeral-os-disk` parameter in [az vm maken](/cli/azure/vm#az-vm-create) naar `true` en de `--os-disk-caching` parameter `ReadOnly`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --ephemeral-os-disk true \
  --os-disk-caching ReadOnly \
  --admin-username azureuser \
  --generate-ssh-keys
```

Voor schaalsets, gebruikt u dezelfde `--ephemeral-os-disk true` parameter voor [az vmss create](/cli/azure/vmss#az-vmss-create) en stel de `--os-disk-caching` parameter `ReadOnly`.

## <a name="portal"></a>Portal   

In de Azure-portal, kunt u kiezen voor het gebruik van tijdelijke schijven bij het implementeren van een virtuele machine door het openen van de **Geavanceerd** sectie van de **schijven** tabblad. Voor **gebruik kortstondige besturingssysteemschijf** Selecteer **Ja**.

![Schermopname van het keuzerondje voor het kiezen van een kortstondige besturingssysteemschijf gebruiken](./media/virtual-machines-common-ephemeral/ephemeral-portal.png)

Als de optie voor het gebruik van een tijdelijke schijf wordt grijs weergegeven, is het mogelijk dat u een VM-grootte die geen groter is dan de installatiekopie van het besturingssysteem op de grootte van een cache of die biedt geen ondersteuning voor Premium-opslag hebt geselecteerd. Ga terug naar de **basisbeginselen** pagina en probeer het kiezen van een andere VM-grootte.

U kunt ook-schaalsets maken met een kortstondige OS-schijven met behulp van de portal. Zorg ervoor dat u selecteert de grootte van een virtuele machine met een groot genoeg cachegrootte en klik vervolgens in **gebruik kortstondige besturingssysteemschijf** Selecteer **Ja**.

![Schermopname van het keuzerondje voor het kiezen van een kortstondige besturingssysteemschijf gebruiken voor uw schaalset](./media/virtual-machines-common-ephemeral/scale-set.png)

## <a name="scale-set-template-deployment"></a>Virtual Machine Scale sets sjabloonimplementatie  
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

## <a name="vm-template-deployment"></a>Implementatie van de VM-sjabloon 
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

A: Platform- en aangepaste installatiekopieën, tot aan de VM-cache-grootte, waar alle lezen/schrijven naar de besturingssysteemschijf van lokale op hetzelfde knooppunt als de virtuele Machine worden ondersteund. 

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
