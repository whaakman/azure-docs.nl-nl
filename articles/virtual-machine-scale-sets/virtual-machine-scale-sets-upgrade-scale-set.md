---
title: Wijzigen van een schaalset voor virtuele Azure-machine | Microsoft Docs
description: Meer informatie over het wijzigen en bijwerken van een schaalset voor virtuele Azure-machine met de REST-API's, Azure PowerShell en Azure CLI
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
ms.openlocfilehash: d065f9765ca279f14b9de45e2412b75c94ef3dd8
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139007"
---
# <a name="modify-a-virtual-machine-scale-set"></a>Wijzigen van een virtuele-machineschaalset
Gedurende de levenscyclus van uw toepassingen moet u wijzigen of bijwerken van uw virtuele-machineschaalset. Deze updates kunnen bevatten informatie over het bijwerken van de configuratie van de schaalset of wijzigen van de configuratie van de toepassing. In dit artikel wordt beschreven hoe u een bestaande schaalset met de REST-API's, Azure PowerShell of Azure CLI te wijzigen.

## <a name="fundamental-concepts"></a>Basisconcepten

### <a name="the-scale-set-model"></a>Het model met een schaalset
Een schaalset is een 'model met een schaalset"die legt de *gewenste* status van de schaal die is ingesteld als geheel. Om te vragen het model voor een schaalset, kunt u de 

- REST-API met [compute/virtualmachinescalesets/get](/rest/api/compute/virtualmachinescalesets/get) als volgt:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version={apiVersion}
    ```

- Azure PowerShell gebruiken met [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss):

    ```powershell
    Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
    ```

- Azure CLI met [az vmss show](/cli/azure/vmss#az_vmss_show):

    ```azurecli
    az vmss show --resource-group myResourceGroup --name myScaleSet
    ```

- U kunt ook [resources.azure.com](https://resources.azure.com) of de taalspecifieke [Azure-SDK's](https://azure.microsoft.com/downloads/).

De exacte presentatie van de uitvoer is afhankelijk van de opties die u aan de opdracht verstrekt. Het volgende voorbeeld ziet verkorte voorbeeld van de uitvoer van de Azure CLI:

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

Deze eigenschappen zijn van toepassing op de schaalset als geheel.


### <a name="the-scale-set-instance-view"></a>De instantieweergave van de schaalset
Ook een schaalset heeft een 'scale set exemplaar weergeven"die worden vastgelegd van de huidige *runtime* status van de schaal die is ingesteld als geheel. Om te vragen de instantieweergave van een schaalset, kunt u het volgende gebruiken:

- REST-API met [compute/virtualmachinescalesets/getinstanceview](/rest/api/compute/virtualmachinescalesets/getinstanceview) als volgt:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/instanceView?api-version={apiVersion}
    ```

- Azure PowerShell gebruiken met [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss):

    ```powershell
    Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceView
    ```

- Azure CLI met [az vmss get-instance-view](/cli/azure/vmss#az_vmss_get_instance_view):

    ```azurecli
    az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet
    ```

- U kunt ook [resources.azure.com](https://resources.azure.com) of de taalspecifieke [Azure-SDK's](https://azure.microsoft.com/downloads/)

De exacte presentatie van de uitvoer is afhankelijk van de opties die u aan de opdracht verstrekt. Het volgende voorbeeld ziet verkorte voorbeeld van de uitvoer van de Azure CLI:

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

Deze eigenschappen bieden een overzicht van de huidige runtimestatus van de virtuele machines in de schaalset, zoals de status van de uitbreidingen die worden toegepast op de schaalset.


### <a name="the-scale-set-vm-model-view"></a>De modelweergave VM-schaalset
Net als bij hoe een schaalset een model-view is, elk VM-exemplaar in de schaalset heeft een eigen modelweergave. Om te vragen de modelweergave voor een bepaalde VM-instantie in een schaalset, kunt u het volgende gebruiken:

- REST-API met [compute/virtualmachinescalesetvms/get](/rest/api/compute/virtualmachinescalesetvms/get) als volgt:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualmachines/instanceId?api-version={apiVersion}
    ```

- Azure PowerShell gebruiken met [Get-AzureRmVmssVm](/powershell/module/azurerm.compute/get-azurermvmssvm):

    ```powershell
    Get-AzureRmVmssVm -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId
    ```

- Azure CLI met [az vmss show](/cli/azure/vmss#az_vmss_show):

    ```azurecli
    az vmss show --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure-SDK's](https://azure.microsoft.com/downloads/).

De exacte presentatie van de uitvoer is afhankelijk van de opties die u aan de opdracht verstrekt. Het volgende voorbeeld ziet verkorte voorbeeld van de uitvoer van de Azure CLI:

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

Deze eigenschappen beschrijven de configuratie van een VM-instantie in een schaalset, niet de configuratie van de schaalset als geheel. Het model met een schaalset heeft bijvoorbeeld `overprovision` als een eigenschap, maar niet door het model voor een VM-instantie in een schaalset. Dit verschil is omdat overmatige inrichting een eigenschap voor de schaalset als een volledige, niet voor afzonderlijke VM-exemplaren in de schaalset is (Zie voor meer informatie over de piekvraag [ontwerpoverwegingen voor schaalsets](virtual-machine-scale-sets-design-overview.md#overprovisioning)).


### <a name="the-scale-set-vm-instance-view"></a>De instantieweergave van de VM-schaalset
Net als bij hoe een schaalset een weergave met een exemplaar van is, elk VM-exemplaar in de schaalset heeft zijn eigen exemplaar weergeven. Om te vragen de instantieweergave van een bepaalde VM-instantie in een schaalset, kunt u het volgende gebruiken:

- REST-API met [compute/virtualmachinescalesetvms/getinstanceview](/rest/api/compute/virtualmachinescalesetvms/getinstanceview) als volgt:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualmachines/instanceId/instanceView?api-version={apiVersion}
    ```

- Azure PowerShell gebruiken met [Get-AzureRmVmssVm](/powershell/module/azurerm.compute/get-azurermvmssvm):

    ```powershell
    Get-AzureRmVmssVm -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId -InstanceView
    ```

- Azure CLI met [az vmss get-instance-view](/cli/azure/vmss#az_vmss_get_instance_view)

    ```azurecli
    az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure-SDK's](https://azure.microsoft.com/downloads/)

De exacte presentatie van de uitvoer is afhankelijk van de opties die u aan de opdracht verstrekt. Het volgende voorbeeld ziet verkorte voorbeeld van de uitvoer van de Azure CLI:

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

Deze eigenschappen beschrijven de huidige runtimestatus van een VM-instantie in een schaalset, waaronder eventuele uitbreidingen die zijn toegepast op de schaalset.


## <a name="how-to-update-global-scale-set-properties"></a>Het bijwerken van de wereldwijde schaal instellen eigenschappen
Voor het bijwerken van een wereldwijde schaal-eigenschap instellen, moet u de eigenschap in het model met een schaalset bijwerken. U kunt deze update via doen:

- REST-API met [compute/virtualmachinescalesets/createorupdate](/rest/api/compute/virtualmachinescalesets/createorupdate) als volgt:

    ```rest
    PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version={apiVersion}
    ```

- U kunt een Resource Manager-sjabloon met de eigenschappen van de REST-API voor het bijwerken van eigenschappen van de wereldwijde schaal instellen implementeren.

- Azure PowerShell gebruiken met [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss):

    ```powershell
    Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -VirtualMachineScaleSet {scaleSetConfigPowershellObject}
    ```

- Azure CLI met [az vmss update](/cli/azure/vmss#az_vmss_update):
    - Een eigenschap wijzigen:

        ```azurecli
        az vmss update --set {propertyPath}={value}
        ```

    - Een object toevoegen aan een eigenschap van de lijst in een schaalset: 

        ```azurecli
        az vmss update --add {propertyPath} {JSONObjectToAdd}
        ```

    - Een object verwijderen uit een lijst met de eigenschap in een schaalset: 

        ```azurecli
        az vmss update --remove {propertyPath} {indexToRemove}
        ```

    - Als u eerder hebt geïmplementeerd de schaalset met de `az vmss create` opdracht, kunt u uitvoeren de `az vmss create` opdracht opnieuw uit voor het bijwerken van de schaalset. Zorg ervoor dat alle eigenschappen in de `az vmss create` opdracht zijn hetzelfde als voorheen, met uitzondering van de eigenschappen die u wilt wijzigen.

- U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure-SDK's](https://azure.microsoft.com/downloads/).

Zodra het model met een schaalset wordt bijgewerkt, wordt de nieuwe configuratie is van toepassing op nieuwe VM's gemaakt in de schaalset. Echter, de modellen voor de bestaande virtuele machines in de schaalset moeten nog steeds worden bijgewerkt met de meest recente algemene model met een schaalset. In het model voor elke virtuele machine wordt een Booleaanse eigenschap genoemd `latestModelApplied` die aangeeft of de virtuele machine bijgewerkt met de meest recente algemene model met een schaalset is (`true` betekent dat de virtuele machine is bijgewerkt met de nieuwste model).


## <a name="how-to-bring-vms-up-to-date-with-the-latest-scale-set-model"></a>Het toevoegen van virtuele machines up-to-date zijn met de nieuwste model met een schaalset
Schaalsets zijn een 'Upgradebeleid' om te bepalen hoe virtuele machines zijn bijgewerkt met de nieuwste model met een schaalset. De drie modi voor het Upgradebeleid zijn:

- **Automatische** -In deze modus kan de schaalset geen garanties over de volgorde van virtuele machines wordt verbroken. De schaalset kan duren voordat alle VM's op hetzelfde moment. 
- **Rolling** -In deze modus kan de schaalset voert de in batches met een optionele onderbreken tijd tussen batches.
- **Handmatige** - In deze modus, wanneer u het model met een schaalset, gebeurt er niets op bestaande virtuele machines.
 
Voor het bijwerken van bestaande VM's, moet u een 'handmatige upgrade' van elke bestaande VM doen. U kunt deze handmatige upgrade met doen:

- REST-API met [compute/virtualmachinescalesets/updateinstances](/rest/api/compute/virtualmachinescalesets/updateinstances) als volgt:

    ```rest
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/manualupgrade?api-version={apiVersion}
    ```

- Azure PowerShell gebruiken met [Update-AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance):
    
    ```powershell
    Update-AzureRmVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId
    ```

- Azure CLI met [az vmss update-instanties](/cli/azure/vmss#az_vmss_update_instances)

    ```azurecli
    az vmss update-instances --resource-group myResourceGroup --name myScaleSet --instance-ids {instanceIds}
    ```

- U kunt ook de taalspecifieke [Azure-SDK's](https://azure.microsoft.com/downloads/).

>[!NOTE]
> Service Fabric-clusters kunnen alleen worden gebruikt *automatische* modus, maar de update wordt verwerkt. Zie voor meer informatie, [ upgrades van de Service Fabric-toepassingen](../service-fabric/service-fabric-application-upgrade.md).

Er is een type wijziging op wereldwijde schaal seteigenschappen die niet aan het beleid voor upgrades voldoet. Wijzigingen in de schaalset Besturingssysteemprofiel (zoals admin-gebruikersnaam en wachtwoord) kan alleen worden gewijzigd in API-versie *2017-12-01* of hoger. Deze wijzigingen zijn alleen van toepassing op virtuele machines die na de wijziging in het schaalsetmodel worden gemaakt. Om bestaande VM's bijgewerkt, moet u een 'installatiekopie' van elke bestaande VM doen. U kunt deze terugzetten via doen:

- REST-API met [compute/virtualmachinescalesets/terugzetten van een installatiekopie](/rest/api/compute/virtualmachinescalesets/reimage) als volgt:

    ```rest
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/reimage?api-version={apiVersion}
    ```

- Azure PowerShell gebruiken met [Set-AzureRmVmssVm](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm):

    ```powershell
    Set-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId -Reimage
    ```

- Azure CLI met [az vmss terugzetten](https://docs.microsoft.com/cli/azure/vmss#az_vmss_reimage):

    ```azurecli
    az vmss reimage --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- U kunt ook de taalspecifieke [Azure-SDK's](https://azure.microsoft.com/downloads/).


## <a name="properties-with-restrictions-on-modification"></a>Eigenschappen met beperkingen met betrekking tot wijziging

### <a name="create-time-properties"></a>Maken-time-eigenschappen
Sommige eigenschappen kunnen alleen worden ingesteld bij het maken van de schaalset. Deze eigenschappen zijn onder andere:

- Beschikbaarheidszones
- Referentie-installatiekopie-uitgever
- Afbeelding verwijzing aanbieding
- Beheerde OS-opslagaccounttype voor schijf

### <a name="properties-that-can-only-be-changed-based-on-the-current-value"></a>Eigenschappen die kunnen alleen worden gewijzigd op basis van de huidige waarde
Sommige eigenschappen kunnen worden gewijzigd, met uitzonderingen, afhankelijk van de huidige waarde. Deze eigenschappen zijn onder andere:

- **singlePlacementGroup** - als singlePlacementGroup ingesteld op true is, False worden gewijzigd. Echter, als singlePlacementGroup ingesteld op false is, het **niet** worden gewijzigd op ' True '.
- **subnet** : het subnet van een schaalset zo lang als de oorspronkelijke subnet kan worden gewijzigd en het nieuwe subnet zich in hetzelfde virtuele netwerk.

### <a name="properties-that-require-deallocation-to-change"></a>Eigenschappen waarvoor de toewijzing is opgeheven wijzigen
Sommige eigenschappen kunnen alleen worden gewijzigd in bepaalde waarden als de virtuele machines in de schaalset ongedaan wordt gemaakt. Deze eigenschappen zijn onder andere:

- **SKU-naam**- als de nieuwe VM-SKU wordt niet ondersteund in de hardware van de schaalset is momenteel ingeschakeld, moet u de toewijzing van de virtuele machines in de schaalset voordat u de naam van de SKU wijzigen. Zie voor meer informatie, [informatie over het aanpassen van een Azure-VM](../virtual-machines/windows/resize-vm.md).


## <a name="vm-specific-updates"></a>VM-specifieke updates
Bepaalde wijzigingen kunnen worden toegepast op specifieke virtuele machines in plaats van de eigenschappen van de wereldwijde schaal instellen. Op dit moment alleen VM-specifieke update die wordt ondersteund, is om te koppelen en loskoppelen van gegevensschijven naar/van virtuele machines in de schaalset. Deze functie is beschikbaar als preview-versie. Zie voor meer informatie de [preview-documentatie](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).


## <a name="scenarios"></a>Scenario's

### <a name="application-updates"></a>Toepassingsupdates
Als een toepassing wordt geïmplementeerd op een schaal die via extensies ingesteld, is een update voor de configuratie van de extensie zorgt ervoor dat de toepassing bijwerken in overeenstemming met het beleid voor upgrades. Bijvoorbeeld, als u een nieuwe versie van een script uit te voeren in een aangepaste Scriptextensie hebt, kunt u bijwerken de *fileUris* eigenschap om te verwijzen naar het nieuwe script. In sommige gevallen wilt u mogelijk een update forceren, zelfs als de configuratie van de extensie ongewijzigd ten opzichte is (bijvoorbeeld, u bijgewerkt het script zonder een wijziging naar de URI van het script). In dergelijke gevallen kunt u de *forceUpdateTag* om af te dwingen een update. Deze eigenschap wordt niet geïnterpreteerd door de Azure-platform. Als u de waarde wijzigt, is er geen invloed op hoe de extensie wordt uitgevoerd. Een wijziging is gewoon zorgt ervoor dat de extensie opnieuw uit te voeren. Voor meer informatie over de *forceUpdateTag*, Zie de [REST-API-documentatie voor uitbreidingen](/rest/api/compute/virtualmachineextensions/createorupdate). Houd er rekening mee dat de *forceUpdateTag* met alle extensies, niet alleen de aangepaste scriptextensie kan worden gebruikt.

Het is ook gebruikelijk dat toepassingen worden geïmplementeerd via een aangepaste installatiekopie. In dit scenario wordt beschreven in de volgende sectie.

### <a name="os-updates"></a>Updates van het besturingssysteem
Als u installatiekopieën van Azure-platform gebruikt, kunt u de installatiekopie bijwerken door het wijzigen van de *imageReference* (meer informatie, Zie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate)).

>[!NOTE]
> Met de platforminstallatiekopieën is het gebruikelijk dat 'nieuwste' opgeven voor de versie van de installatiekopie-verwijzing. Wanneer u maakt, de schaal vergroten en terugzetten van een installatiekopie, virtuele machines worden gemaakt met de meest recente beschikbare versie. Echter, het **niet** betekenen dat de installatiekopie van het besturingssysteem automatisch wordt bijgewerkt na verloop van tijd als nieuwe versies van een installatiekopie worden vrijgegeven. Een afzonderlijke functie is momenteel in preview waarmee automatische besturingssysteemupgrades. Zie voor meer informatie de [automatische Besturingssysteemupgrades documentatie](virtual-machine-scale-sets-automatic-upgrade.md).

Als u aangepaste installatiekopieën gebruikt, kunt u de installatiekopie bijwerken door het bijwerken van de *imageReference* ID (meer informatie, Zie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate)).

## <a name="examples"></a>Voorbeelden

### <a name="update-the-os-image-for-your-scale-set"></a>De installatiekopie van het besturingssysteem voor uw schaalset bijwerken
Mogelijk hebt u een schaalset die wordt uitgevoerd een oude versie van Ubuntu LTS, 16.04. U wilt bijwerken naar een nieuwere versie van Ubuntu-16.04 LTS, zoals versie *16.04.201801090*. De eigenschap image reference versie maakt geen deel uit van een lijst, zodat u kunt deze eigenschappen met een van de volgende opdrachten rechtstreeks wijzigen:

- Azure PowerShell gebruiken met [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) als volgt:

    ```powershell
    Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -ImageReferenceVersion 16.04.201801090
    ```

- Azure CLI met [az vmss update](/cli/azure/vmss#az_vmss_update_instances):

    ```azurecli
    az vmss update --resource-group myResourceGroup --name myScaleSet --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090
    ```


### <a name="update-the-load-balancer-for-your-scale-set"></a>De load balancer voor uw schaalset bijwerken
Stel dat u hebt een schaalset met een Azure Load Balancer en ter vervanging van de Azure Load Balancer met een Azure Application Gateway. De load balancer en Application Gateway-eigenschappen voor een schaalset uitmaken deel van een lijst, zodat u de opdrachten gebruiken kunt om te verwijderen of de lijst met elementen in plaats van rechtstreeks wijzigen van de eigenschappen toe te voegen:

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

- Azure CLI:

    ```azurecli
    # Remove the load balancer backend pool from the scale set model
    az vmss update --resource-group myResourceGroup --name myScaleSet --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0
    
    # Remove the load balancer backend pool from the scale set model; only necessary if you have NAT pools configured on the scale set
    az vmss update --resource-group myResourceGroup --name myScaleSet --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0
    
    # Add the application gateway backend pool to the scale set model
    az vmss update --resource-group myResourceGroup --name myScaleSet --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}'
    ```

>[!NOTE]
> Deze opdrachten wordt ervan uitgegaan dat er slechts één IP-configuratie en load balancer is op de schaalset. Als er meerdere zijn, moet u mogelijk anders dan gebruik van de index van een lijst met *0*.


## <a name="next-steps"></a>Volgende stappen
U kunt ook algemene beheertaken uitvoeren op scale sets met de [Azure CLI](virtual-machine-scale-sets-manage-cli.md) of [Azure PowerShell](virtual-machine-scale-sets-manage-powershell.md).
