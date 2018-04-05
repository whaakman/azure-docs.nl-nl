---
title: Gekoppelde gegevensschijven met virtuele-machineschaalsets in Azure | Microsoft Docs
description: Leer gekoppelde gegevensschijven gebruiken met virtuele-machineschaalsets
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: negat
ms.openlocfilehash: ec11a2d66530129fb61d97681e6882b887c8654c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="azure-virtual-machine-scale-sets-and-attached-data-disks"></a>Virtuele-machineschaalsets in Azure en gekoppelde gegevensschijven
Voor het uitbreiden van uw beschikbare opslag bieden [virtuele-machineschaalsets](/azure/virtual-machine-scale-sets/) in Azure ondersteuning voor VM-instanties met gekoppelde gegevensschijven. U kunt gegevensschijven koppelen wanneer de schaalset wordt gemaakt of aan een bestaande schaalset.

> [!NOTE]
>  Wanneer u een schaalset maakt met gekoppelde gegevensschijven, dient u de schijven nog steeds te koppelen en formatteren van binnen een VM om ze te gebruiken (net als bij zelfstandige virtuele Azure-machines). Een handige manier om dit proces te voltooien, is om een aangepaste scriptextensie te gebruiken die een script oproept om alle gegevensschijven op de VM te partitioneren en formatteren. Zie [Azure CLI 2.0](tutorial-use-disks-cli.md#prepare-the-data-disks) [Azure PowerShell](tutorial-use-disks-powershell.md#prepare-the-data-disks) voor voorbeelden hiervan.


## <a name="create-and-manage-disks-in-a-scale-set"></a>Schijven in een schaalset maken en beheren
Zie een van de volgende zelfstudies voor gedetailleerde informatie over het maken van een schaalset met gekoppelde gegevensschijven, het voorbereiden en formatteren of toevoegen en verwijderen van gegevensschijven:

- [Azure CLI 2.0](tutorial-use-disks-cli.md)
- [Azure PowerShell](tutorial-use-disks-powershell.md)

De rest van dit artikel bevat een overzicht van de specifieke gebruiksvoorbeelden zoals Service Fabric-clusters die gegevensschijven vereisen of het koppelen van bestaande gegevensschijven met inhoud aan een schaalset.


## <a name="create-a-service-fabric-cluster-with-attached-data-disks"></a>Een Service Fabric-cluster maken met gekoppelde gegevensschijven
Elk [knooppunttype](../service-fabric/service-fabric-cluster-nodetypes.md) in een [Service Fabric](/azure/service-fabric)-cluster dat wordt uitgevoerd in Azure, wordt ondersteund door een virtuele-machineschaalset.  U kunt met behulp van een Azure Resource Manager-sjabloon gegevensschijven koppelen aan de schaalsets die gezamenlijk het Service Fabric-cluster vormen. U kunt een [bestaande sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates) als uitgangspunt nemen. Neem in de sjabloon een sectie _dataDisks_ op in het _storageProfile_ van de _Microsoft.Compute/virtualMachineScaleSets_-resource(s) en implementeer de sjabloon. In het volgende voorbeeld wordt een gegevensschijf van 128 GB gekoppeld:

```json
"dataDisks": [
    {
    "diskSizeGB": 128,
    "lun": 0,
    "createOption": "Empty"
    }
]
```

U kunt automatisch partitioneren, formatteren en de gegevensschijven koppelen wanneer het cluster is geïmplementeerd.  Voeg een extensie voor aangepaste scripts toe aan het _extensionProfile_ van de _virtualMachineProfile_ van de schaalset(s).

Als u de gegevensschijven automatisch wilt voorbereiden in een Windows-cluster, voegt u het volgende toe:

```json
{
    "name": "customScript",    
    "properties": {    
        "publisher": "Microsoft.Compute",    
        "type": "CustomScriptExtension",    
        "typeHandlerVersion": "1.8",    
        "autoUpgradeMinorVersion": true,    
        "settings": {    
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.ps1"
        ],
        "commandToExecute": "powershell -ExecutionPolicy Unrestricted -File prepare_vm_disks.ps1"
        }
    }
}
```
Als u de gegevensschijven automatisch wilt voorbereiden in een Linux-cluster, voegt u het volgende toe:
```json
{
    "name": "lapextension",
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"
        ],
        "commandToExecute": "bash prepare_vm_disks.sh"
        }
    }
}
```


## <a name="adding-pre-populated-data-disks-to-an-existent-scale-set"></a>Vooraf gevulde gegevensschijven toevoegen aan een bestaande schaalset 
> Wanneer u schijven toevoegt aan een bestaand model met een schaalset, wordt standaard altijd een lege schijf gemaakt. Dit scenario geldt ook voor nieuwe exemplaren die met de schaalset worden gemaakt. De reden voor dit gedrag is dat de definitie van de schaalset een lege gegevensschijf bevat. Om in deze situatie vooraf gevulde gegevensschijven te maken voor een bestaand model met een schaalset, kunt u kiezen voor een van de volgende twee opties:

* Gegevens kopiëren van de VM met het exemplaar 0 naar een of meer gegevensschijven in de andere VM's door het uitvoeren van een aangepast script.
* Een beheerde installatiekopie maken van de besturingssysteemschijf plus de gegevensschijf (met de vereiste gegevens) en vervolgens een nieuwe schaalset maken aan de hand van de installatiekopie. Op deze manier beschikt elke nieuwe VM over een gegevensschijf die is opgegeven in de definitie van de schaalset. Aangezien in deze definitie wordt verwezen naar een installatiekopie met een gegevensschijf die aangepaste gegevens bevat, zijn deze wijzigingen automatisch beschikbaar op elke virtuele machine in de schaalset.

> De instructies voor het maken van een aangepaste installatiekopie vindt u hier: [Create a managed image of a generalized VM in Azure](/azure/virtual-machines/windows/capture-image-resource/) (Een beheerde installatiekopie maken van een gegeneraliseerde VM in Azure). 

> De gebruiker moet de VM met het exemplaar 0 vastleggen omdat daar de vereiste gegevens op staan en vervolgens die VHD gebruiken voor de definitie van de installatiekopie.


## <a name="additional-notes"></a>Aanvullende opmerkingen
Er is ondersteuning voor Azure Managed Disks en aan schaalsets gekoppelde gegevensschijven beschikbaar in API [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) of hoger van de Microsoft.Compute API.

Bij de eerste implementatie van ondersteuning voor gekoppelde schijven voor schaalsets, kunt u geen gegevensschijven koppelen aan of loskoppelen van afzonderlijke virtuele machines in een schaalset.

De ondersteuning in Azure Portal voor gekoppelde gegevensschijven in schaalsets is oorspronkelijk beperkt. Afhankelijk van uw vereisten kunt u Azure-sjablonen, CLI, PowerShell, SDK's en REST API gebruiken voor het beheren van gekoppelde schijven.


