---
title: Status van de toepassing-extensie gebruiken met Azure virtual machine scale sets | Microsoft Docs
description: Informatie over het gebruik van de status van de toepassing-extensie voor het bewaken van de status van uw toepassingen die zijn geïmplementeerd op virtuele-machineschaalsets.
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2019
ms.author: manayar
ms.openlocfilehash: 1ac7b5f41c0c941db08a63c516febabaf9f07b3e
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55491379"
---
# <a name="using-application-health-extension-with-virtual-machine-scale-sets"></a>Uitbreiding met de virtuele-machineschaalset wordt ingesteld met behulp van de status van de toepassing
Bewaking van de toepassingsstatus van uw is een belangrijk signaal voor het beheren en upgraden van uw implementatie. Virtuele Azure-machine-schaalsets bieden ondersteuning voor [rolling upgrades](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) inclusief [automatische upgrades van de installatiekopie van het besturingssysteem](virtual-machine-scale-sets-automatic-upgrade.md), die afhankelijk zijn van statuscontrole van de afzonderlijke exemplaren om te upgraden van uw implementatie .

Dit artikel wordt beschreven hoe u de status van de toepassing-extensie voor het bewaken van de status van uw toepassingen die zijn geïmplementeerd op virtuele-machineschaalsets kunt gebruiken.

## <a name="pre-requisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u bekend met bent:
-   Virtuele Azure-machine [extensies](../virtual-machines/extensions/overview.md)
-   [Wijzigen van](virtual-machine-scale-sets-upgrade-scale-set.md) virtuele-machineschaalsets

## <a name="when-to-use-the-application-health-extension"></a>Wanneer u de status van de toepassing-extensie
De extensie van de status van de toepassing wordt geïmplementeerd in een instantie van virtuele-machineschaalset en rapporten op de virtuele machine wordt de status van binnen het exemplaar in de schaalset. U kunt configureren dat de extensie test op een toepassingseindpunt en de status van de toepassing op dat exemplaar. De status van dit exemplaar wordt gecontroleerd door Azure om te bepalen of een exemplaar in aanmerking voor upgrade operations.

Als de extensie rapporten wordt de status van binnen een virtuele machine, kan de extensie worden gebruikt in situaties waarbij externe tests zoals statuscontroles van toepassing (die gebruikmaken van aangepaste Azure Load Balancer [tests](../load-balancer/load-balancer-custom-probe-overview.md)) kan niet worden gebruikt.

## <a name="extension-schema"></a>Extensieschema

De volgende JSON ziet u het schema voor de extensie van de status van de toepassing. De extensie moet ten minste een 'tcp' of 'http'-aanvraag met een bijbehorende poort of Aanvraagpad respectievelijk.

```json
{
  "type": "extensions",
  "name": "HealthExtension",
  "apiVersion": "2018-10-01",
  "location": "<location>",  
  "properties": {
    "publisher": "Microsoft.ManagedServices",
    "type": "< ApplicationHealthLinux or ApplicationHealthWindows>",
    "autoUpgradeMinorVersion": true,
    "typeHandlerVersion": "1.0",
    "settings": {
      "protocol": "<protocol>",
      "port": "<port>",
      "requestPath": "</requestPath>"
    }
  }
}  
```

### <a name="property-values"></a>Waarden van eigenschappen

| Name | Waarde / voorbeeld | Gegevenstype
| ---- | ---- | ---- | ----
| apiVersion | `2018-10-01` | date |
| Uitgever | `Microsoft.ManagedServices` | string |
| type | `ApplicationHealthLinux` (Linux), `ApplicationHealthWindows` (Windows) | string |
| typeHandlerVersion | `1.0` | int |

### <a name="settings"></a>Instellingen

| Name | Waarde / voorbeeld | Gegevenstype
| ---- | ---- | ----
| protocol | `http` of `tcp` | string |
| poort | Optioneel als het protocol `http`, verplicht als het protocol `tcp` | int |
| requestPath | Verplicht als het protocol `http`, niet toegestaan als het protocol `tcp` | string |

## <a name="deploy-the-application-health-extension"></a>De extensie van de status van de toepassing implementeren
Er zijn meerdere manieren van de implementatie van de status van de toepassing uitbreiding van uw schaal ingesteld zoals beschreven in de onderstaande voorbeelden.

### <a name="rest-api"></a>REST-API

In het volgende voorbeeld wordt de status van de toepassing-uitbreiding (met de naam myHealthExtension) toegevoegd aan de extensionProfile in het model met een schaal van een op basis van een Windows-schaalset.

```
PUT on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/extensions/myHealthExtension?api-version=2018-10-01`
```

```json
{
  "name": "myHealthExtension",
  "properties": {
    "publisher": " Microsoft.ManagedServices",
    "type": "< ApplicationHealthWindows>",
    "autoUpgradeMinorVersion": true,
    "typeHandlerVersion": "1.0",
    "settings": {
      "protocol": "<protocol>",
      "port": "<port>",
      "requestPath": "</requestPath>"
    }
  }
}
```
Gebruik `PATCH` bewerken van een reeds geïmplementeerde uitbreiding.

### <a name="azure-powershell"></a>Azure PowerShell

Gebruik de [toevoegen AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) cmdlet om de status van de toepassing-extensie toevoegen aan de schaal modeldefinitie instellen.

Het volgende voorbeeld wordt de status van de toepassing-extensie voor de `extensionProfile` in het schaalsetmodel van een op basis van een Windows-schaalset. Het voorbeeld wordt de nieuwe Az PowerShell-module.

```azurepowershell-interactive
# Define the scale set variables
$vmScaleSetName = "myVMScaleSet"
$vmScaleSetResourceGroup = "myVMScaleSetResourceGroup"

# Define the Application Health extension properties
$publicConfig = @{"protocol" = "http"; "port" = 80; "requestPath" = "/healthEndpoint"};
$extensionName = "myHealthExtension"
$extensionType = "ApplicationHealthWindows"
$publisher = "Microsoft.ManagedServices"

# Get the scale set object
$vmScaleSet = Get-AzVmss `
  -ResourceGroupName $vmScaleSetResourceGroup `
  -VMScaleSetName $vmScaleSetName

# Add the Application Health extension to the scale set model
Add-AzVmssExtension -VirtualMachineScaleSet $vmScaleSet `
  -Name $extensionName `
  -Publisher $publisher `
  -Setting $publicConfig `
  -Type $extensionType `
  -TypeHandlerVersion "1.0" `
  -AutoUpgradeMinorVersion $True

# Update the scale set
Update-AzVmss -ResourceGroupName $vmScaleSetResourceGroup `
  -Name $vmScaleSetName `
  -VirtualMachineScaleSet $vmScaleSet
```


### <a name="azure-cli-20"></a>Azure CLI 2.0

Gebruik [az vmss extension set](/cli/azure/vmss/extension#az-vmss-extension-set) om toe te voegen van de uitbreiding van de status van de toepassing de definitie van model van de schaalset.

Het volgende voorbeeld wordt de status van de toepassing-extensie toegevoegd aan het model met een schaal van een op basis van een Windows-schaalset.

```azurecli-interactive
az vmss extension set \
  --name ApplicationHealthWindows \
  --publisher Microsoft.ManagedServices \
  --version 1.0 \
  --resource-group <myVMScaleSetResourceGroup> \
  --vmss-name <myVMScaleSet> \
  --settings ./extension.json
```


## <a name="troubleshoot"></a>Problemen oplossen
Extensie uitvoering uitvoer wordt vastgelegd in bestanden gevonden in de volgende mappen:

```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.ManagedServices.ApplicationHealthWindows\<version>\
```

```Linux
/var/lib/waagent/apphealth
```

De logboeken vastleggen ook periodiek de status van de toepassing.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [Implementeer uw toepassing](virtual-machine-scale-sets-deploy-app.md) ingesteld op de virtuele-machineschaalset.
