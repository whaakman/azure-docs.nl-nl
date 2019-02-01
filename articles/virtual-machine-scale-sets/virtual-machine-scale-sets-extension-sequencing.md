---
title: Extensievolgorde gebruiken met Azure virtual machine scale sets | Microsoft Docs
description: Informatie over het sequentiëren van de extensie wordt ingericht bij het implementeren van meerdere extensies op schaalsets voor virtuele machines.
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
ms.openlocfilehash: aa6ba07168e86b2163324c62f8bb32ca330551f3
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55501380"
---
# <a name="sequence-extension-provisioning-in-virtual-machine-scale-sets"></a>Hiermee stelt u reeks extensie wordt ingericht in virtuele-machineschaalset
Extensies voor Azure-machines bieden mogelijkheden, zoals configuratie na implementatie en beheer, bewaking, beveiliging en meer. Productie-implementaties gebruiken vaak een combinatie van meerdere extensies die zijn geconfigureerd voor de VM-exemplaren om gewenste resultaten te behalen.

Wanneer u meerdere extensies op een virtuele machine, is het belangrijk om ervoor te zorgen dat dezelfde OS resources vereisen extensies worden niet bij het ophalen van deze resources op hetzelfde moment. Aantal extensies is ook afhankelijk van andere extensies voor de vereiste configuraties, zoals omgevingsinstellingen en geheimen. Zonder de juiste volgorde en uitvoeringsvolgorde vindt u in plaats, kunnen afhankelijke-extensie-implementaties mislukken.

Dit artikel wordt uitgelegd hoe u kunt de sequencer gebruikt extensies voor de VM-exemplaren in schaalsets voor virtuele machines moet worden geconfigureerd.

## <a name="pre-requisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u bekend met bent:
-   Virtuele Azure-machine [extensies](../virtual-machines/extensions/overview.md)
-   [Wijzigen van](virtual-machine-scale-sets-upgrade-scale-set.md) virtuele-machineschaalsets

## <a name="when-to-use-extension-sequencing"></a>Wanneer u de uitvoeringsvolgorde van extensies vindt
Extensies voor sequentiëren in niet verplicht voor schaal wordt ingesteld en tenzij anders aangegeven, uitbreidingen kunnen worden ingericht op een scale set-instantie in een willekeurige volgorde.

Bijvoorbeeld, als uw scale set-model twee extensies – ExtensionA en ExtensionB heeft – opgegeven in het model, optreden klikt u vervolgens een van de inrichting reeksen:
-   ExtensionA ExtensionB ->
-   ExtensionB -> ExtensionA

Als uw toepassing een extensie vereist moet altijd worden ingericht vóór extensie B, moet u extensievolgorde zoals beschreven in dit artikel gebruiken. Met de uitvoeringsvolgorde van extensies vindt, wordt nu slechts één reeks plaatsvinden:
-   ExtensionA - > ExtensionB

Geen extensies niet is opgegeven in een gedefinieerde inrichting reeks kunnen worden ingericht op elk gewenst moment, met inbegrip van voordat u, na of tijdens een opgegeven reeks. Extensievolgorde geeft alleen aan dat een bepaalde extensie na een andere specifieke extensie worden ingericht. Dit heeft geen invloed op de inrichting van een andere extensie gedefinieerd in het model.

Bijvoorbeeld, als uw model met een schaalset drie extensies – extensie A, B-extensie en extensie C heeft – opgegeven in het model en extensie C om te worden ingericht na een extensie is ingesteld, optreden klikt u vervolgens een van de inrichting reeksen:
-   ExtensionA -> ExtensionC ExtensionB ->
-   ExtensionB -> ExtensionA ExtensionC ->
-   ExtensionA -> ExtensionB ExtensionC ->

Als u nodig hebt om ervoor te zorgen dat geen andere extensie wordt ingericht als de opgegeven extensie van de takenreeks wordt uitgevoerd, wordt u aangeraden alle uitbreidingen in uw model met een schaalset sequentiëren. In het bovenstaande voorbeeld kan de extensie B worden ingesteld op nadat de extensie C worden ingericht, zodat slechts één reeks kan plaatsvinden:
-   ExtensionA -> ExtensionC ExtensionB ->


## <a name="how-to-use-extension-sequencing"></a>Het gebruik van de uitvoeringsvolgorde van extensies vindt
Voor het inrichten van reeks extensie, moet u de definitie van de extensie in het model met een schaalset om op te nemen van de eigenschap 'provisionAfterExtensions', die een matrix van extensienamen accepteert bijwerken. De uitbreidingen die worden vermeld in de waarde van de eigenschap matrix moeten volledig worden gedefinieerd in het model met een schaalset.

### <a name="template-deployment"></a>Sjabloonimplementatie
Het volgende voorbeeld wordt een sjabloon waarbij de schaalset drie extensies – ExtensionA, ExtensionB en ExtensionC – heeft zodat uitbreidingen zijn ingericht in de volgorde gedefinieerd:
-   ExtensionA -> ExtensionB ExtensionC ->

```json
"virtualMachineProfile": {
  "extensionProfile": {
    "extensions": [
      {
        "name": "ExtensionA",
        "properties": {
          "publisher": "ExtensionA.Publisher",
          "settings": {},
          "typeHandlerVersion": "1.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionA"
        }
      },
      {
        "name": "ExtensionB",
        "properties": {
          "provisionAfterExtensions": [
            "ExtensionA"
          ],
          "publisher": "ExtensionB.Publisher",
          "settings": {},
          "typeHandlerVersion": "2.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionB"
        }
      }, 
      {
        "name": "ExtensionC",
        "properties": {
          "provisionAfterExtensions": [
            "ExtensionB"
          ],
          "publisher": "ExtensionC.Publisher",
          "settings": {},
          "typeHandlerVersion": "3.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionC"                   
        }
      }
    ]
  }
}
```

Omdat de eigenschap "provisionAfterExtensions" een matrix van extensienamen accepteert, kan het bovenstaande voorbeeld worden gewijzigd zodat ExtensionC na ExtensionA en ExtensionB is ingericht, maar er geen volgorde vereist tussen ExtensionA en ExtensionB is. De volgende sjabloon kan worden gebruikt voor het bereiken van dit scenario:

```json
"virtualMachineProfile": {
  "extensionProfile": {
    "extensions": [
      {
        "name": "ExtensionA",
        "properties": {
          "publisher": "ExtensionA.Publisher",
          "settings": {},
          "typeHandlerVersion": "1.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionA"
        }
      },
      {
        "name": "ExtensionB",
        "properties": {
          "publisher": "ExtensionB.Publisher",
          "settings": {},
          "typeHandlerVersion": "2.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionB"
        }
      }, 
      {
        "name": "ExtensionC",
        "properties": {
          "provisionAfterExtensions": [
            "ExtensionA","ExtensionB"
          ],
          "publisher": "ExtensionC.Publisher",
          "settings": {},
          "typeHandlerVersion": "3.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionC"                   
        }
      }
    ]
  }
}
```

### <a name="rest-api"></a>REST-API
Het volgende voorbeeld wordt een nieuwe extensie met de naam ExtensionC naar een model met een schaalset. ExtensionC heeft afhankelijkheden op ExtensionA en ExtensionB, die al zijn gedefinieerd in het model met een schaalset.

```
PUT on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/extensions/ExtensionC?api-version=2018-10-01`
```

```json
{ 
  "name": "ExtensionC",
  "properties": {
    "provisionAfterExtensions": [
      "ExtensionA","ExtensionB"
    ],
    "publisher": "ExtensionC.Publisher",
    "settings": {},
    "typeHandlerVersion": "3.0",
    "autoUpgradeMinorVersion": true,
    "type": "ExtensionC" 
  }                  
}
```

Als ExtensionC is gedefinieerd, eerder in de Virtual Machine scale sets model en u nu wilt toevoegen de afhankelijkheden ervan, kunt u uitvoeren een `PATCH` de reeds geïmplementeerde extensie-eigenschappen bewerken.

```
PATCH on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/extensions/ExtensionC?api-version=2018-10-01`
```

```json
{ 
  "properties": {
    "provisionAfterExtensions": [
      "ExtensionA","ExtensionB"
    ]
  }                  
}
```
Wijzigingen in bestaande exemplaren van de schaalset worden toegepast op de volgende [upgrade](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model).

### <a name="azure-powershell"></a>Azure PowerShell
Gebruik de [toevoegen AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) cmdlet om de status van de toepassing-extensie toevoegen aan de schaal modeldefinitie instellen. Extensievolgorde vereist het gebruik van PowerShell Az 1.2.0 of hoger.

Het volgende voorbeeld wordt de [toepassingsstatus extensie](virtual-machine-scale-sets-health-extension.md) naar de `extensionProfile` in een schaalsetmodel van een op basis van een Windows-schaalset. De extensie van de status van de toepassing worden ingericht na het inrichten van de [Custom Script Extension](../virtual-machines/extensions/custom-script-windows.md), die is al gedefinieerd in de schaalset.

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
  -ProvisionAfterExtension "CustomScriptExtension" `
  -AutoUpgradeMinorVersion $True

# Update the scale set
Update-AzVmss -ResourceGroupName $vmScaleSetResourceGroup `
  -Name $vmScaleSetName `
  -VirtualMachineScaleSet $vmScaleSet
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Gebruik [az vmss extension set](/cli/azure/vmss/extension#az-vmss-extension-set) om toe te voegen van de uitbreiding van de status van de toepassing de definitie van model van de schaalset. Extensievolgorde vereist het gebruik van Azure CLI 2.0.55 of hoger.

Het volgende voorbeeld wordt de [toepassingsstatus extensie](virtual-machine-scale-sets-health-extension.md) model van een op Windows gebaseerde schaalset instellen voor de schaal. De extensie van de status van de toepassing worden ingericht na het inrichten van de [Custom Script Extension](../virtual-machines/extensions/custom-script-windows.md), die is al gedefinieerd in de schaalset.

```azurecli-interactive
az vmss extension set \
  --name ApplicationHealthWindows \
  --publisher Microsoft.ManagedServices \
  --version 1.0 \
  --resource-group <myVMScaleSetResourceGroup> \
  --vmss-name <myVMScaleSet> \
  --provision-after-extensions CustomScriptExtension \
  --settings ./extension.json
```


## <a name="troubleshoot"></a>Problemen oplossen

### <a name="not-able-to-add-extension-with-dependencies"></a>Kan geen extensie met afhankelijkheden toevoegen?
1. Controleer of de extensies die zijn opgegeven in provisionAfterExtensions zijn gedefinieerd in het model met een schaalset.
2. Controleer of er zijn geen circulaire afhankelijkheden die zijn geïntroduceerd. Bijvoorbeeld, is niet de volgende reeks toegestaan: ExtensionA -> ExtensionB ExtensionC -> ExtensionA ->
3. Zorg ervoor dat eventuele uitbreidingen die u afhankelijkheden uitvoeren op een eigenschap 'instellingen' onder 'Eigenschappen' extensie hebben. Bijvoorbeeld, als ExtentionB worden ingericht na ExtensionA moet, moet klikt u vervolgens ExtensionA hebben het veld 'instellingen' onder ExtensionA 'Eigenschappen'. U kunt een lege 'instellingen'-eigenschap opgeven als de extensie niet alle vereiste instellingen schrijft.

### <a name="not-able-to-remove-extensions"></a>Kunnen extensies verwijderen?
Zorg ervoor dat de uitbreidingen die wordt verwijderd, niet worden vermeld in provisionAfterExtensions voor eventuele andere uitbreidingen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [Implementeer uw toepassing](virtual-machine-scale-sets-deploy-app.md) ingesteld op de virtuele-machineschaalset.
