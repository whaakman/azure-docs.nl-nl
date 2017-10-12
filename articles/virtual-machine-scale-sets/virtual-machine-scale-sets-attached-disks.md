---
title: Gekoppelde gegevensschijven met virtuele-machineschaalsets in Azure | Microsoft Docs
description: Leer gekoppelde gegevensschijven gebruiken met virtuele-machineschaalsets
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: guybo
ms.openlocfilehash: 22c7e589efa9a9f401549ec9b95c58c4eaf07b94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-vm-scale-sets-and-attached-data-disks"></a>Virtuele-machineschaalsets in Azure en gekoppelde gegevensschijven
[Virtuele-machineschaalsets](/azure/virtual-machine-scale-sets/) in Azure ondersteunen nu virtuele machines met gekoppelde gegevensschijven. Gegevensschijven kunnen worden gedefinieerd in het opslagprofiel voor schaalsets dat is gemaakt met Azure Managed Disks. Eerder waren de enige direct gekoppelde opslagopties met virtuele machines in schaalsets het station van het besturingssysteem en tijdelijke stations.

> [!NOTE]
>  Wanneer u een schaalset maakt die is gedefinieerd met gekoppelde gegevensschijven, dient u de schijven nog steeds te koppelen en formatteren van binnen een VM om ze te gebruiken (net als bij zelfstandige virtuele Azure-machines). Een handige manier om dit te doen is om een aangepaste scriptextensie te gebruiken die een standaardscript oproept om alle gegevensschijven op de VM te partitioneren en formatteren.

## <a name="create-a-scale-set-with-attached-data-disks"></a>Een schaalset maken met gekoppelde gegevensschijven
Een eenvoudige manier om een schaalset met gekoppelde schijven te maken, is om de opdracht _vmss create_ in [Azure CLI](https://github.com/Azure/azure-cli) te gebruiken. In het volgende voorbeeld wordt een Azure-resourcegroep en een VM-schaalset van 10 virtuele Ubuntu-machines gemaakt, elk met 2 gekoppelde gegevensschijven, van respectievelijk 50 GB en 100 GB.
```bash
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```
De opdracht _vmss create_ gebruikt standaard bepaalde configuratiewaarden als u ze niet opgeeft. Probeer het volgende om de beschikbare opties te bekijken die u kunt overschrijven:
```bash
az vmss create --help
```
Een andere manier om een schaalset te maken met gekoppelde gegevensschijven, is om een schaalset te definiëren in een Azure Resource Manager-sjabloon, een _dataDisks_-sectie op te nemen in het _storageProfile_ en de sjabloon te implementeren. Het bovenstaande voorbeeld met een schijf van 50 en 100 GB zou in de sjabloon als volgt worden gedefinieerd:
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    }
]
```
U kunt hier een volledig voorbeeld bekijken van een schaalset die klaar is om geïmplementeerd te worden met een gekoppelde schijf: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).

## <a name="adding-a-data-disk-to-an-existing-scale-set"></a>Een gegevensschijf toevoegen aan een bestaande schaalset
> [!NOTE]
>  U kunt gegevensschijven alleen koppelen aan een schaalset die is gemaakt met [Azure Managed Disks](./virtual-machine-scale-sets-managed-disks.md).

U kunt een gegevensschijf toevoegen aan een VM-schaalset met behulp van de Azure CLI-opdracht _az vmss disk attach_. Geef een LUN op die nog niet in gebruik is. In het volgende CLI-voorbeeld wordt een 50GB-station aan LUN 3 toegevoegd:
```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```

In het volgende PowerShell-voorbeeld wordt een 50 GB-station aan LUN 3 toegevoegd:
```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myvmssrg -VMScaleSetName myvmss
$vmss = Add-AzureRmVmssDataDisk -VirtualMachineScaleSet $vmss -Lun 3 -Caching 'ReadWrite' -CreateOption Empty -DiskSizeGB 50 -StorageAccountType StandardLRS
Update-AzureRmVmss -ResourceGroupName myvmssrg -Name myvmss -VirtualMachineScaleSet $vmss
```

> [!NOTE]
> Andere VM-grootten hebben andere limieten voor het aantal gekoppelde schijven dat wordt ondersteund. Controleer de [formaatkenmerken van de virtuele machine](../virtual-machines/windows/sizes.md) voordat u een nieuwe schijf toevoegt.

U kunt ook een schijf toevoegen door een nieuwe vermelding toe te voegen aan de eigenschap _dataDisks_ in het _storageProfile_ van een schaalsetdefinitie en de wijziging toe te passen. U kunt dit testen door een bestaande schaalsetdefinitie te vinden in [Azure Resource Explorer](https://resources.azure.com/). Selecteer _Bewerken_ en voeg een nieuwe schijf toe aan de lijst met gegevensschijven. Bijvoorbeeld met het bovenstaande voorbeeld:
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    },
    {
    "lun": 3,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 20
    }          
]
```

Selecteer vervolgens _PUT_ om de wijzigingen toe te passen op uw schaalset. Dit voorbeeld zou moeten werken als u gebruikmaakt van een VM-formaat dat meer dan twee gekoppelde gegevensschijven ondersteunt.

> [!NOTE]
> Wanneer u een wijziging aanbrengt in een schaalsetdefinitie, zoals het toevoegen of verwijderen van een gegevensschijf, wordt deze toegepast op alle nieuwe virtuele machines. Deze wordt echter alleen toegepast op bestaande virtuele machines als de eigenschap _upgradePolicy_ is ingesteld op 'Automatisch'. Als de eigenschap is ingesteld op 'Handmatig', dient u handmatig het nieuwe model toe te passen op bestaande virtuele machines. U kunt dit doen in de portal met behulp van de PowerShell-opdracht _Update-AzureRmVmssInstance_ of de CLI-opdracht _az vmss update-instances_.

## <a name="adding-pre-populated-data-disks-to-an-existent-scale-set"></a>Vooraf gevulde gegevensschijven toevoegen aan een bestaande schaalset 
> Wanneer u schijven toevoegt aan een bestaand model met een schaalset, wordt er standaard altijd een lege schijf gemaakt. Dit scenario geldt ook voor nieuwe exemplaren die met de schaalset worden gemaakt. De reden voor dit gedrag is dat de definitie van de schaalset een lege gegevensschijf bevat. Om in deze situatie vooraf gevulde gegevensschijven te maken voor een bestaand model met een schaalset, kunt u kiezen voor een van de volgende twee opties:

* Gegevens kopiëren van de VM met het exemplaar 0 naar een of meer gegevensschijven in de andere VM's door het uitvoeren van een aangepast script.
* Een beheerde installatiekopie maken van de besturingssysteemschijf plus de gegevensschijf (met de vereiste gegevens) en vervolgens een nieuwe schaalset maken aan de hand van de installatiekopie. Op deze manier beschikt elke nieuwe VM over een gegevensschijf die is opgegeven in de definitie van de schaalset. Aangezien in deze definitie wordt verwezen naar een installatiekopie met een gegevensschijf die aangepaste gegevens bevat, zijn deze wijzigingen automatisch beschikbaar op elke virtuele machine in de schaalset.

> De instructies voor het maken van een aangepaste installatiekopie vindt u hier: [Create a managed image of a generalized VM in Azure](/azure/virtual-machines/windows/capture-image-resource/) (Een beheerde installatiekopie maken van een gegeneraliseerde VM in Azure). 

> De gebruiker moet de VM met het exemplaar 0 vastleggen omdat daar de vereiste gegevens op staan en vervolgens die VHD gebruiken voor de definitie van de installatiekopie.

## <a name="removing-a-data-disk-from-a-scale-set"></a>Een gegevensschijf verwijderen uit een schaalset
U kunt een gegevensschijf verwijderen uit een VM-schaalset met behulp van de Azure CLI-opdracht _az vmss disk detach_. Met de volgende opdracht wordt bijvoorbeeld de schijf die is gedefinieerd op LUN 2 verwijderd:
```bash
az vmss disk detach -g dsktest -n dskvmss --lun 2
```  
U kunt ook een schijf verwijderen uit een schaalset door een invoerwaarde uit de eigenschap _dataDisks_ te verwijderen in het _storageProfile_ en de wijziging toe te passen. 

## <a name="additional-notes"></a>Aanvullende opmerkingen
Er is ondersteuning voor Azure Managed Disks en aan schaalsets gekoppelde gegevensschijven beschikbaar in API [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) of hoger van de Microsoft.Compute API.

Bij de eerste implementatie van ondersteuning voor gekoppelde schijven voor schaalsets, kunt u geen gegevensschijven koppelen aan of loskoppelen van afzonderlijke virtuele machines in een schaalset.

De ondersteuning in Azure Portal voor gekoppelde gegevensschijven in schaalsets is oorspronkelijk beperkt. Afhankelijk van uw vereisten kunt u Azure-sjablonen, CLI, PowerShell, SDK's en REST API gebruiken voor het beheren van gekoppelde schijven.


