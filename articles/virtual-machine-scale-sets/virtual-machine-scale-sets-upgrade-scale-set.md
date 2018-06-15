---
title: Wijzigen van een Azure virtuele-machineschaalset | Microsoft Docs
description: Meer informatie over het wijzigen en bijwerken van de schaal van een virtuele machine van Azure instelt met de REST-API's, Azure PowerShell en Azure CLI 2.0
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: negat
ms.openlocfilehash: 662cea7ac47e411b127540faf5cab8b3c4d8964a
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32194043"
---
# <a name="modify-a-virtual-machine-scale-set"></a>Wijzigen van een virtuele-machineschaalset
Gedurende de levenscyclus van uw toepassingen, kunt u wellicht wijzigen of bijwerken van uw virtuele-machineschaalset. Deze updates kunnen bevatten informatie over het bijwerken van de configuratie van de schaalaanpassingsset of wijzig de configuratie van de toepassing. Dit artikel wordt beschreven hoe u een bestaande schaal ingesteld met de REST-API's, Azure PowerShell of Azure CLI 2.0 wijzigt.

## <a name="fundamental-concepts"></a>basisconcepten

### <a name="the-scale-set-model"></a>De model-schaalset
Een schaalset heeft een 'scale set model' waarmee wordt vastgelegd de *gewenste* status van de schaal is ingesteld als geheel. Om te vragen het model voor een schaalset, kunt u de 

- REST-API met [virtualmachinescalesets-compute/get](/rest/api/compute/virtualmachinescalesets/get) als volgt:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version={apiVersion}
    ```

- Azure PowerShell gebruiken met [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss):

    ```powershell
    Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
    ```

- Azure CLI 2.0 met [az vmss weergeven](/cli/azure/vmss#az_vmss_show):

    ```azurecli
    az vmss show --resource-group myResourceGroup --name myScaleSet
    ```

- U kunt ook [resources.azure.com](https://resources.azure.com) of de taalspecifieke [Azure SDK's](https://azure.microsoft.com/downloads/).

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven. Het volgende voorbeeld ziet verkorte voorbeeld van uitvoer van de Azure CLI 2.0:

```azurecli
az vmss show --resource-group myResourceGroup --name myScaleSet
{
  "location": "westus",
  "overprovision": true,
  "plan": null,
  "singlePlacementGroup": true,
  "sku": {
    "additionalProperties": {},
    "capacity": 1,
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
}
```

Deze eigenschappen zijn van toepassing op de schaal instelt als geheel.


### <a name="the-scale-set-instance-view"></a>De schaal instellen instantieweergave
Een schaal ook heeft een 'scale set exemplaar weergeven' waarmee de huidige vastgelegd *runtime* status van de schaal is ingesteld als geheel. Om te vragen de instantieweergave voor een schaalset, kunt u het volgende gebruiken:

- REST-API met [virtualmachinescalesets-compute/getinstanceview](/rest/api/compute/virtualmachinescalesets/getinstanceview) als volgt:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/instanceView?api-version={apiVersion}
    ```

- Azure PowerShell gebruiken met [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss):

    ```powershell
    Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceView
    ```

- Azure CLI 2.0 met [az vmss get--instantieweergave](/cli/azure/vmss#az_vmss_get_instance_view):

    ```azurecli
    az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet
    ```

- U kunt ook [resources.azure.com](https://resources.azure.com) of de taalspecifieke [Azure SDK's](https://azure.microsoft.com/downloads/)

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven. Het volgende voorbeeld ziet verkorte voorbeeld van uitvoer van de Azure CLI 2.0:

```azurecli
$ az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet
{
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "level": "Info",
      "message": null,
      "time": "{time}"
    }
  ],
  "virtualMachine": {
    "additionalProperties": {},
    "statusesSummary": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "count": 1
      }
    ]
  }
}
```

Deze eigenschappen bieden een samenvatting van de huidige runtimestatus van de virtuele machines in de schaal is ingesteld, zoals de status van de uitbreidingen die worden toegepast op de scale-verzameling.


### <a name="the-scale-set-vm-model-view"></a>De schaal ingesteld VM model weergeven
Net als bij hoe een schaalset een modelweergave heeft, elke virtuele machine in de schaalset heeft een eigen model weergeven. Om te vragen van de modelweergave voor een schaalset, kunt u het volgende gebruiken:

- REST-API met [virtualmachinescalesetvms-compute/get](/rest/api/compute/virtualmachinescalesetvms/get) als volgt:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualmachines/instanceId?api-version={apiVersion}
    ```

- Azure PowerShell gebruiken met [Get-AzureRmVmssVm](/powershell/module/azurerm.compute/get-azurermvmssvm):

    ```powershell
    Get-AzureRmVmssVm -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId
    ```

- Azure CLI 2.0 met [az vmss weergeven](/cli/azure/vmss#az_vmss_show):

    ```azurecli
    az vmss show --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/).

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven. Het volgende voorbeeld ziet verkorte voorbeeld van uitvoer van de Azure CLI 2.0:

```azurecli
$ az vmss show --resource-group myResourceGroup --name myScaleSet
{
  "location": "westus",
  "name": "{name}",
  "sku": {
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
}
```

Deze eigenschappen beschrijven de configuratie van de virtuele machine zelf, niet de configuratie van de schaal instelt als geheel. Het model van de set schaal heeft bijvoorbeeld `overprovision` als een eigenschap, maar niet door het model voor een virtuele machine in een schaalset. Dit verschil is omdat overmatige inrichting een eigenschap voor de schaal ingesteld als een volledige, geen afzonderlijke virtuele machines in de schaalset is (Zie voor meer informatie over overmatige inrichting [ontwerpoverwegingen voor schaalsets](virtual-machine-scale-sets-design-overview.md#overprovisioning)).


### <a name="the-scale-set-vm-instance-view"></a>De instantieweergave van de VM-schaalset
Net als bij hoe een schaalset weergave met een exemplaar is, elke virtuele machine in de schaalset heeft een eigen instantieweergave. Om te vragen de instantieweergave voor een schaalset, kunt u het volgende gebruiken:

- REST-API met [virtualmachinescalesetvms-compute/getinstanceview](/rest/api/compute/virtualmachinescalesetvms/getinstanceview) als volgt:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualmachines/instanceId/instanceView?api-version={apiVersion}
    ```

- Azure PowerShell gebruiken met [Get-AzureRmVmssVm](/powershell/module/azurerm.compute/get-azurermvmssvm):

    ```powershell
    Get-AzureRmVmssVm -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId -InstanceView
    ```

- Azure CLI 2.0 met [az vmss get-exemplaar-weergave](/cli/azure/vmss#az_vmss_get_instance_view)

    ```azurecli
    az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/)

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven. Het volgende voorbeeld ziet verkorte voorbeeld van uitvoer van de Azure CLI 2.0:

```azurecli
$ az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
{
  "additionalProperties": {
    "osName": "ubuntu",
    "osVersion": "16.04"
  },
  "disks": [
    {
      "name": "{name}",
      "statuses": [
        {
          "additionalProperties": {},
          "code": "ProvisioningState/succeeded",
          "displayStatus": "Provisioning succeeded",
          "time": "{time}"
        }
      ]
    }
  ],
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "time": "{time}"
    },
    {
      "additionalProperties": {},
      "code": "PowerState/running",
      "displayStatus": "VM running"
    }
  ],
  "vmAgent": {
    "statuses": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "displayStatus": "Ready",
        "level": "Info",
        "message": "Guest Agent is running",
        "time": "{time}"
      }
    ],
    "vmAgentVersion": "{version}"
  },
}
```

Deze eigenschappen worden de huidige runtimestatus van de VM, met eventuele uitbreidingen die wordt toegepast op de schaalaanpassingsset beschreven.


## <a name="how-to-update-global-scale-set-properties"></a>Het bijwerken van wereldwijde schaal instellen eigenschappen
Voor het bijwerken van een globale scale set-eigenschap, moet u de eigenschap in het model van de set schaal bijwerken. U kunt deze update via doen:

- REST-API met [virtualmachinescalesets-compute/createorupdate](/rest/api/compute/virtualmachinescalesets/createorupdate) als volgt:

    ```rest
    PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version={apiVersion}
    ```

- U kunt een Resource Manager-sjabloon met de eigenschappen van de REST-API om globale scale seteigenschappen te werken implementeren.

- Azure PowerShell gebruiken met [Update AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss):

    ```powershell
    Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -VirtualMachineScaleSet {scaleSetConfigPowershellObject}
    ```

- Azure CLI 2.0 met [az vmss bijwerken](/cli/azure/vmss#az_vmss_update):
    - Een eigenschap te wijzigen:

        ```azurecli
        az vmss update --set {propertyPath}={value}
        ```

    - Een object toevoegen aan een lijsteigenschap in een schaalset: 

        ```azurecli
        az vmss update --add {propertyPath} {JSONObjectToAdd}
        ```

    - Een object uit een lijsteigenschap in een schaalset verwijderen: 

        ```azurecli
        az vmss update --remove {propertyPath} {indexToRemove}
        ```

    - Als u de schaal in te stellen voor het eerder hebt geïmplementeerd de `az vmss create` uitvoert, kunt u uitvoeren en de `az vmss create` opdracht voor het bijwerken van de scale-set. Zorg ervoor dat alle eigenschappen in de `az vmss create` opdracht zijn hetzelfde als voorheen, met uitzondering van de eigenschappen die u wilt wijzigen.

- U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/).

Zodra het scale set model wordt bijgewerkt, wordt de nieuwe configuratie is van toepassing op nieuwe VM's in de schaalset gemaakt. Echter moeten de modellen voor de bestaande virtuele machines in de schaalset nog steeds worden bijgewerkt met de meest recente algehele scale set model. In het model voor elke virtuele machine een Boole-eigenschap heet `latestModelApplied` die aangeeft of de virtuele machine bijgewerkt met de meest recente algehele scale set model is (`true` betekent dat de virtuele machine is bijgewerkt door de meest recente model).


## <a name="how-to-bring-vms-up-to-date-with-the-latest-scale-set-model"></a>Bij het plaatsen van virtuele machines up-to-date blijven met de meest recente schaal model instellen
Schaalsets 'upgrade beleid' hebben om te bepalen hoe virtuele machines up-to-date blijven met de meest recente model met een schaal worden gebracht. De drie beschikbare modi voor het Upgradebeleid zijn:

- **Automatische** -In deze modus kan de schaalaanpassingsset kan niet garanderen over de volgorde van virtuele machines wordt verbroken. De schaalaanpassingsset kan duren voordat u alle virtuele machines op hetzelfde moment. 
- **Rolling** -In deze modus kan de schaalaanpassingsset implementeert de update in batches met een optionele onderbreken tijd tussen batches.
- **Handmatige** - In deze modus, wanneer u het model van de set scale bijwerkt gebeurt niets aan bestaande virtuele machines.
 
Voor het bijwerken van bestaande virtuele machines, moet u een 'handmatige upgrade' van elke bestaande VM te doen. U kunt deze handmatige upgrade met doen:

- REST-API met [virtualmachinescalesets-compute/updateinstances](/rest/api/compute/virtualmachinescalesets/updateinstances) als volgt:

    ```rest
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/manualupgrade?api-version={apiVersion}
    ```

- Azure PowerShell gebruiken met [Update AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance):
    
    ```powershell
    Update-AzureRmVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId
    ```

- Azure CLI 2.0 met [az vmss update-exemplaren](/cli/azure/vmss#az_vmss_update_instances)

    ```azurecli
    az vmss update-instances --resource-group myResourceGroup --name myScaleSet --instance-ids {instanceIds}
    ```

- U kunt ook de taalspecifieke [Azure SDK's](https://azure.microsoft.com/downloads/).

>[!NOTE]
> Service Fabric-clusters kunnen alleen worden gebruikt *automatische* modus, maar de update wordt anders afgehandeld. Zie voor meer informatie [ Service Fabric-toepassingsupgrades](../service-fabric/service-fabric-application-upgrade.md).

Er is een soort wijziging aan de globale scale seteigenschappen die niet aan het Upgradebeleid voldoet. Wijzigingen in de schaalset Besturingssysteemprofiel (zoals admin-gebruikersnaam en wachtwoord) kan alleen worden gewijzigd in API-versie *2017-12-01* of hoger. Deze wijzigingen zijn alleen van toepassing op virtuele machines die zijn gemaakt nadat de wijziging in de schaal model instelt. Voor het maken van bestaande virtuele machines up-to-date, moet u een 'terugzetten van de installatiekopie' van elke bestaande VM te doen. U kunt deze terugzetten van de installatiekopie via doen:

- REST-API met [compute/virtualmachinescalesets/terugzetten van de installatiekopie](/rest/api/compute/virtualmachinescalesets/reimage) als volgt:

    ```rest
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/reimage?api-version={apiVersion}
    ```

- Azure PowerShell gebruiken met [Set AzureRmVmssVm](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm):

    ```powershell
    Set-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId -Reimage
    ```

- Azure CLI 2.0 met [az vmss terugzetten van de installatiekopie](https://docs.microsoft.com/cli/azure/vmss#az_vmss_reimage):

    ```azurecli
    az vmss reimage --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- U kunt ook de taalspecifieke [Azure SDK's](https://azure.microsoft.com/downloads/).


## <a name="properties-with-restrictions-on-modification"></a>Eigenschappen met beperkingen voor wijziging

### <a name="create-time-properties"></a>Eigenschappen maken tijd
Sommige eigenschappen kunnen alleen worden ingesteld wanneer u de schaalaanpassingsset maakt. Deze eigenschappen omvatten:

- Beschikbaarheidszones
- referentie-installatiekopie-uitgever
- afbeelding verwijzing aanbieding
- Type opslagaccount OS schijf beheerd

### <a name="properties-that-can-only-be-changed-based-on-the-current-value"></a>Eigenschappen die kunnen alleen worden gewijzigd op basis van de huidige waarde
Sommige eigenschappen kunnen worden gewijzigd met uitzonderingen, afhankelijk van de huidige waarde. Deze eigenschappen omvatten:

- **singlePlacementGroup** - als singlePlacementGroup ingesteld op true is, False worden gewijzigd. Echter, als singlePlacementGroup false is, het **is niet toegestaan** worden gewijzigd op true.
- **subnet** : het subnet van een schaalset zo lang als de oorspronkelijke subnet kan worden gewijzigd en het nieuwe subnet bevinden zich in hetzelfde virtuele netwerk.

### <a name="properties-that-require-deallocation-to-change"></a>Eigenschappen waarvoor de toewijzing is opgeheven wijzigen
Sommige eigenschappen kunnen alleen worden gewijzigd in bepaalde waarden als de toewijzing van de virtuele machines in de schaalset worden opgeheven. Deze eigenschappen omvatten:

- **SKU-naam**- als de SKU van de nieuwe VM wordt niet ondersteund op de hardware die de schaalaanpassingsset is momenteel ingeschakeld, moet u de toewijzing van de virtuele machines in de schaal instellen voordat u de SKU-naam wijzigen. Zie voor meer informatie [hoe het formaat van een Azure VM](../virtual-machines/windows/resize-vm.md).


## <a name="vm-specific-updates"></a>VM-specifieke updates
Bepaalde wijzigingen kunnen worden toegepast op een specifieke virtuele machines in plaats van de globale scale seteigenschappen. Op dit moment de enige specifieke VM-update die wordt ondersteund is om te koppelen/loskoppelen van gegevensschijven naar/van de virtuele machines in de schaalset. Deze functie is in preview. Zie voor meer informatie de [preview-documentatie](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).


## <a name="scenarios"></a>Scenario's

### <a name="application-updates"></a>Updates voor toepassingen
Als een toepassing wordt geïmplementeerd op een schaal via uitbreidingen ingesteld, wordt een update van configuratie voor de uitbreiding in de toepassing bijwerken in overeenstemming met het Upgradebeleid. Bijvoorbeeld, als u een nieuwe versie van een script uit te voeren in een aangepaste Scriptextensie hebt, kunt u bijwerken de *fileUris* eigenschap verwijzen naar het nieuwe script. In sommige gevallen wilt u wellicht een update afdwingen, zelfs als de configuratie voor de uitbreiding is ongewijzigd ten opzichte (bijvoorbeeld, u bijgewerkt het script zonder een wijziging in de URI van het script). In dergelijke gevallen kunt u de *forceUpdateTag* om af te dwingen een update. Deze eigenschap wordt niet door de Azure-platform worden geïnterpreteerd. Als u de waarde wijzigt, is er geen invloed op hoe de uitbreiding wordt uitgevoerd. Een wijziging is gewoon zorgt ervoor dat de uitbreiding opnieuw uit te voeren. Voor meer informatie over de *forceUpdateTag*, Zie de [REST-API-documentatie voor uitbreidingen](/rest/api/compute/virtualmachineextensions/createorupdate). Houd er rekening mee dat de *forceUpdateTag* met alle extensies, niet alleen de extensie voor aangepaste scripts kunnen worden gebruikt.

Het is ook gebruikelijk dat toepassingen worden geïmplementeerd via een aangepaste installatiekopie. Dit scenario wordt beschreven in de volgende sectie.

### <a name="os-updates"></a>Updates voor het besturingssysteem
Als u installatiekopieën van het Azure-platform gebruikt, kunt u de installatiekopie bijwerken door het wijzigen van de *imageReference* (meer informatie, Zie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate)).

>[!NOTE]
> Installatiekopieën van het platform worden vaak 'nieuwste' opgeven voor de versie van de verwijzing naar afbeelding. Wanneer u maakt, uitbreiden en terugzetten van de installatiekopie, virtuele machines worden gemaakt met de meest recente versie. Echter, het **heeft geen** betekenen dat de installatiekopie van het besturingssysteem automatisch wordt bijgewerkt na verloop van tijd als nieuwe versies van een installatiekopie zijn vrijgegeven. Een afzonderlijke functie is momenteel in preview waarmee automatische upgrades voor het besturingssysteem. Zie voor meer informatie de [automatische Upgrades voor het besturingssysteem documentatie](virtual-machine-scale-sets-automatic-upgrade.md).

Als u aangepaste installatiekopieën gebruikt, kunt u de installatiekopie bijwerken door het bijwerken van de *imageReference* ID (meer informatie, Zie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate)).

## <a name="examples"></a>Voorbeelden

### <a name="update-the-os-image-for-your-scale-set"></a>Bijwerken van de installatiekopie van het besturingssysteem voor uw scale set
Mogelijk hebt u een scale-set die wordt uitgevoerd een oude versie van Ubuntu TNS 16.04. U wilt bijwerken naar een nieuwere versie van Ubuntu TNS 16.04, zoals versie *16.04.201801090*. De eigenschap image verwijzing naar versie maakt geen deel uit van een lijst, zodat u kunt deze eigenschappen met een van de volgende opdrachten rechtstreeks wijzigen:

- Azure PowerShell gebruiken met [Update AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) als volgt:

    ```powershell
    Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -ImageReferenceVersion 16.04.201801090
    ```

- Azure CLI 2.0 met [az vmss bijwerken](/cli/azure/vmss#az_vmss_update_instances):

    ```azurecli
    az vmss update --resource-group myResourceGroup --name myScaleSet --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090
    ```


### <a name="update-the-load-balancer-for-your-scale-set"></a>De load balancer voor uw scale set bijwerken
Stel dat er een schaalwaarde ingesteld met een Azure Load Balancer en u wilt vervangen van de Azure Load Balancer met een Azure Application Gateway. De load balancer en eigenschappen voor een schaalset Application Gateway uitmaken deel van een lijst, zodat u de opdrachten gebruiken kunt om te verwijderen of de lijst met elementen in plaats van rechtstreeks wijzigen van de eigenschappen toe te voegen:

- Azure Powershell:

    ```powershell
    # Get the current model of the scale set and store it in a local PowerShell object named $vmss
    $vmss=Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet"
    
    # Create a local PowerShell object for the new desired IP configuration, which includes the referencerence to the application gateway
    $ipconf = New-AzureRmVmssIPConfig "myNic" -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name
    
    # Replace the existing IP configuration in the local PowerShell object (which contains the references to the current Azure Load Balancer) with the new IP configuration
    $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf
    
    # Update the model of the scale set with the new configuration in the local PowerShell object
    Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -virtualMachineScaleSet $vmss
    ```

- Azure CLI 2.0:

    ```azurecli
    # Remove the load balancer backend pool from the scale set model
    az vmss update --resource-group myResourceGroup --name myScaleSet --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0
    
    # Remove the load balancer backend pool from the scale set model; only necessary if you have NAT pools configured on the scale set
    az vmss update --resource-group myResourceGroup --name myScaleSet --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0
    
    # Add the application gateway backend pool to the scale set model
    az vmss update --resource-group myResourceGroup --name myScaleSet --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}'
    ```

>[!NOTE]
> Deze opdrachten wordt ervan uitgegaan dat er slechts één IP-configuratie en load balancer op de scale-set is. Als er meerdere zijn, moet u mogelijk andere dan een lijstindex gebruiken *0*.


## <a name="next-steps"></a>Volgende stappen
U kunt ook algemene beheertaken uitvoeren op schaalsets met de [Azure CLI 2.0](virtual-machine-scale-sets-manage-cli.md) of [Azure PowerShell](virtual-machine-scale-sets-manage-powershell.md).
