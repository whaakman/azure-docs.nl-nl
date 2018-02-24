---
title: Wijzigen van een Azure virtuele-machineschaalset | Microsoft Docs
description: Een Azure virtuele-machineschaalset wijzigen
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: negat
ms.openlocfilehash: 836d56012afa9e5d5bdec35d85c37dd4b0b788ce
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Wijzigen van een virtuele-machineschaalset
Dit artikel wordt beschreven hoe u een bestaande schaalset met wijzigt. Dit omvat het wijzigen van de configuratie van de schaal is ingesteld, het wijzigen van de configuratie van de toepassingen die worden uitgevoerd op de schaal is ingesteld, het beheren van de beschikbaarheid en meer.

## <a name="fundamental-concepts"></a>basisconcepten

### <a name="the-scale-set-model"></a>De model-schaalset

Een schaalset heeft een 'scale set model' waarmee wordt vastgelegd de *gewenste* status van de schaal is ingesteld als geheel. Om te vragen het model voor een schaalset, kunt u het volgende gebruiken:

REST-API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` (Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/get))

PowerShell: `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (Zie voor meer informatie de [Powershell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss))

CLI: `az vmss show -g {resourceGroupName} -n {vmSaleSetName}` (Zie voor meer informatie de [CLI documentatie](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show))

U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/) query uitvoeren op het model voor een schaalset.

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven, maar hier volgt een van voorbeelduitvoer van de CLI:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
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
  .
  .
  .
}
```

U kunt zien, deze eigenschappen van toepassing op de schaal instelt als geheel.



### <a name="the-scale-set-instance-view"></a>De schaal instellen instantieweergave

Een schaal ook heeft een 'scale set exemplaar weergeven' waarmee de huidige vastgelegd *runtime* status van de schaal is ingesteld als geheel. Om te vragen de instantieweergave voor een schaalset, kunt u het volgende gebruiken:

REST-API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version={apiVersion}` (Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getinstanceview))

PowerShell: `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceView` (Zie voor meer informatie de [Powershell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss))

CLI: `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName}` (Zie voor meer informatie de [CLI documentatie](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view))

U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/) query uitvoeren op de weergave van het exemplaar voor een schaalset.

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven, maar hier volgt een voorbeeld van uitvoer van de CLI:

```
$ az vmss get-instance-view -g {resourceGroupName} -n {virtualMachineScaleSetName}
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
  .
  .
  .
}
```

Zoals u ziet, worden deze eigenschappen bieden een samenvatting van de huidige runtimestatus van de virtuele machines in de schaal, met inbegrip van de status van de uitbreidingen die worden toegepast op de scale-verzameling (weggelaten als beknopt alternatief bevat) omvat.



### <a name="the-scale-set-vm-model-view"></a>De schaal ingesteld VM model weergeven

Net als bij hoe een schaalset een modelweergave heeft, elke virtuele machine in de schaalset heeft een eigen model weergeven. Om te vragen van de modelweergave voor een schaalset, kunt u het volgende gebruiken:

REST-API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}?api-version={apiVersion}` (Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/get))

PowerShell: `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` (Zie voor meer informatie de [Powershell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

CLI: `az vmss show -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` (Zie voor meer informatie de [CLI documentatie](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show))

U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/) query uitvoeren op het model voor een virtuele machine in een schaalset.

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven, maar hier volgt een van voorbeelduitvoer van de CLI:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
{
  "location": "westus",
  "name": "{name}",
  "sku": {
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
  .
  .
  .
}
```

Zoals u ziet, wordt met deze eigenschappen de configuratie van de virtuele machine zelf, niet de configuratie van de schaal instelt als geheel beschreven. Het model van de set schaal heeft bijvoorbeeld `overprovision` als een eigenschap, maar niet door het model voor een virtuele machine in een schaalset. Dit verschil is omdat overmatige inrichting een eigenschap voor de schaal ingesteld als een volledige, geen afzonderlijke virtuele machines in de schaalset is (Zie voor meer informatie over overmatige inrichting [deze documentatie](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning)).



### <a name="the-scale-set-vm-instance-view"></a>De instantieweergave van de VM-schaalset

Net als bij hoe een schaalset weergave met een exemplaar is, elke virtuele machine in de schaalset heeft een eigen instantieweergave. Om te vragen de instantieweergave voor een schaalset, kunt u het volgende gebruiken:

REST-API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}/instanceView?api-version={apiVersion}` (Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/getinstanceview))

PowerShell: `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -InstanceView` (Zie voor meer informatie de [Powershell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

CLI: `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` (Zie voor meer informatie de [CLI documentatie](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view))

U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/) query uitvoeren op de weergave exemplaar voor een virtuele machine in een schaalset.

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven, maar hier volgt een van voorbeelduitvoer van de CLI:

```
$ az vmss get-instance-view -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}
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
  .
  .
  .
}
```

Zoals u ziet, beschrijf deze eigenschappen in de huidige runtimestatus van de virtuele machine zelf, inclusief eventuele uitbreidingen die wordt toegepast op de scale-verzameling (weggelaten als beknopt alternatief bevat).




## <a name="how-to-update-global-scale-set-properties"></a>Het bijwerken van wereldwijde schaal instellen eigenschappen

Voor het bijwerken van een globale scale set-eigenschap, moet u de eigenschap in het model van de set schaal bijwerken. U kunt deze update via doen:

REST-API: `PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` (Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate))

Resource Manager-sjablonen: u kunt ook de eigenschappen van de REST-API om globale scale seteigenschappen te werken met Resource Manager-sjabloon implementeren.

PowerShell: `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -VirtualMachineScaleSet {scaleSetConfigPowershellObject}` (Zie voor meer informatie de [Powershell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss))

CLI. Een eigenschap te wijzigen: `az vmss update --set {propertyPath}={value}`. Een object toevoegen aan een lijsteigenschap in een schaalset: `az vmss update --add {propertyPath} {JSONObjectToAdd}`. Een object verwijderen uit een lijsteigenschap in een set scale: `az vmss update --remove {propertyPath} {indexToRemove}`. (Zie voor meer informatie de [CLI documentatie](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update)). U kunt ook als u de schaal instelt met behulp van de eerder hebt geïmplementeerd de `az vmss create` uitvoert, kunt u uitvoeren en de `az vmss create` opdracht voor het bijwerken van de schaalaanpassingsset. Om dit te doen, moet u ervoor zorgen dat alle eigenschappen in de `az vmss create` opdracht zijn hetzelfde als voorheen, met uitzondering van de eigenschappen die u wilt wijzigen.



U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/) model bijwerken van de schaal instelt.

Zodra het scale set model wordt bijgewerkt, wordt de nieuwe configuratie is van toepassing op nieuwe VM's in de schaalset gemaakt. Echter moeten de modellen voor de bestaande virtuele machines in de schaalset nog steeds worden bijgewerkt met de meest recente algehele scale set model. In het model voor elke virtuele machine een Boole-eigenschap heet `latestModelApplied` die aangeeft of de virtuele machine bijgewerkt met de meest recente algehele scale set model is (`true` betekent dat de virtuele machine is bijgewerkt door de meest recente model).




## <a name="how-to-bring-vms-up-to-date-with-the-latest-scale-set-model"></a>Bij het plaatsen van virtuele machines up-to-date blijven met de meest recente schaal model instellen

Schaalsets 'upgrade beleid' hebben om te bepalen hoe virtuele machines up-to-date blijven met de meest recente model met een schaal worden gebracht. De drie beschikbare modi voor het Upgradebeleid zijn:

- Automatische: In deze modus kan de schaalaanpassingsset kan niet garanderen over de volgorde van virtuele machines wordt verbroken. De schaalaanpassingsset kan duren voordat u alle virtuele machines op hetzelfde moment. 
- Rolling: In deze modus kan de schaalaanpassingsset implementeert de update in batches met een optionele onderbreken tijd tussen batches.
- Handmatig: In deze modus tijdens het bijwerken van het model van de set schaal gebeurt er niets aan bestaande virtuele machines. Voor het bijwerken van bestaande virtuele machines, moet u een 'handmatige upgrade' van elke bestaande VM te doen. U kunt deze handmatige upgrade via doen:

REST-API: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/manualupgrade?api-version={apiVersion}` (Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/updateinstances))

PowerShell: `Update-AzureRmVmssInstance -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` (Zie voor meer informatie de [Powershell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance))

CLI: `az vmss update-instances -g {resourceGroupName} -n {vmScaleSetName} --instance-ids {instanceIds}` (Zie voor meer informatie de [CLI documentatie](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update_instances)).

U kunt ook de [Azure SDK's](https://azure.microsoft.com/downloads/) een handmatige upgrade op een virtuele machine in een set scale doen.

>[!NOTE]
> Service Fabric-clusters automatische modus kunnen alleen worden gebruikt, maar de update wordt anders afgehandeld. Zie voor meer informatie over service fabric-updates [de documentatie van de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade).

>[!NOTE]
> Er is een soort wijziging aan de globale scale seteigenschappen die niet aan het Upgradebeleid voldoet. Deze wijzigingen zijn ingesteld tot de schaal Besturingssysteemprofiel (bijvoorbeeld admin-gebruikersnaam en wachtwoord). Deze wijzigingen zijn alleen van toepassing op virtuele machines die zijn gemaakt nadat de wijziging in de schaal model instelt. Voor het maken van bestaande virtuele machines up-to-date, moet u een 'terugzetten van de installatiekopie' van elke bestaande VM te doen. U kunt deze terugzetten van de installatiekopie via doen:

REST-API: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage))

PowerShell: `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (Zie voor meer informatie de [Powershell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm))

CLI: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (Zie voor meer informatie de [CLI documentatie](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage)).

U kunt ook de [Azure SDK's](https://azure.microsoft.com/downloads/) naar de installatiekopie van een virtuele machine in een schaalset.




## <a name="properties-with-restrictions-on-modification"></a>Eigenschappen met beperkingen voor wijziging

### <a name="create-time-properties"></a>Eigenschappen maken tijd

Sommige eigenschappen kunnen alleen worden ingesteld tijdens het instellen van de schaal in eerste instantie te maken. Deze eigenschappen omvatten:

- Zones
- referentie-installatiekopie-uitgever
- afbeelding verwijzing aanbieding

### <a name="properties-that-can-only-be-changed-based-on-the-current-value"></a>Eigenschappen die kunnen alleen worden gewijzigd op basis van de huidige waarde

Sommige eigenschappen kunnen worden gewijzigd met uitzonderingen, afhankelijk van de huidige waarde. Deze eigenschappen omvatten:

- singlePlacementGroup: als singlePlacementGroup true is, kan deze worden gewijzigd op false. Echter, als singlePlacementGroup false is, het **is niet toegestaan** worden gewijzigd op true.
- subnet: het subnet van een scale-set kan worden gewijzigd zolang het oorspronkelijke subnet en het nieuwe subnet bevinden zich in hetzelfde virtuele netwerk.

### <a name="properties-that-require-deallocation-to-change"></a>Eigenschappen waarvoor de toewijzing is opgeheven wijzigen

Sommige eigenschappen kunnen alleen worden gewijzigd in bepaalde waarden als de toewijzing van de virtuele machines in de schaalset worden opgeheven. Deze eigenschappen omvatten:

- SKU-naam: als de SKU van de nieuwe VM wordt niet ondersteund in de hardware van de schaal set is ingeschakeld, moet u toewijzing van de virtuele machines in de schaal instellen voordat u wijzigt de naam van de sku. Zie voor meer informatie over het vergroten of verkleinen van virtuele machines [dit blogbericht Azure](https://azure.microsoft.com/blog/resize-virtual-machines/).


## <a name="vm-specific-updates"></a>VM-specifieke updates

Bepaalde wijzigingen kunnen worden toegepast op een specifieke virtuele machines in plaats van de globale scale seteigenschappen. Op dit moment de enige VM-specifieke update die wordt ondersteund is koppelen/loskoppelen gegevensschijven naar/van de virtuele machines in de schaalset. Deze functie is in preview. Zie voor meer informatie de [preview-documentatie](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).

## <a name="scenarios-application-updates-os-updates-etc"></a>Scenario's: Toepassingsupdates, OS-updates, enzovoort.

### <a name="application-updates"></a>Updates voor toepassingen

Als een toepassing wordt geïmplementeerd op een schaal ingesteld door middel van, het bijwerken van de configuratie voor de uitbreiding zorgt ervoor dat de toepassing bijwerken in overeenstemming met het Upgradebeleid. Als u een nieuwe versie van een script uit te voeren in een extensie voor aangepaste scripts hebt, kunt u bijvoorbeeld de eigenschap fileUris om te verwijzen naar het nieuwe script bijwerken. In sommige gevallen, maar mogelijk wilt u een update afdwingen, zelfs als de configuratie voor de uitbreiding is ongewijzigd ten opzichte (bijvoorbeeld, u bijgewerkt het script zonder de URI van het script). In dergelijke gevallen kunt u de forceUpdateTag als een update wilt afdwingen. De Azure-platform biedt deze eigenschap niet interpreteren, zodat de waarde ervan wijzigen geen invloed heeft op hoe de uitbreiding wordt uitgevoerd. Wijzigen van het simpelweg zorgt ervoor dat de uitbreiding opnieuw uit te voeren. Zie voor meer informatie over de forceUpdateTag de [REST-API-documentatie voor uitbreidingen](https://docs.microsoft.com/rest/api/compute/virtualmachineextensions/createorupdate).

Het is ook gebruikelijk dat toepassingen worden geïmplementeerd via een aangepaste installatiekopie. Dit scenario wordt beschreven in de volgende sectie van de "OS updates"

### <a name="os-updates"></a>Updates voor het besturingssysteem

Als u van installatiekopieën van het platform gebruikmaakt, kunt u de installatiekopie bijwerken door het wijzigen van de imageReference (meer informatie in de [REST API-documentatie](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

>[!NOTE]
> Installatiekopieën van het platform worden vaak 'nieuwste' opgeven voor de versie van de verwijzing naar afbeelding. Dit betekent dat tijdens scale set maken, scale-out en terugzetten van de installatiekopie, wordt de virtuele machines worden gemaakt met de meest recente versie. Echter, het **heeft geen** betekenen dat de installatiekopie van het besturingssysteem automatisch na verloop van tijd bijgewerkt wordt als nieuwe versies van een installatiekopie zijn vrijgegeven. Dit is een afzonderlijke functie, die momenteel in preview. Zie voor meer informatie de [automatische Upgrades voor het besturingssysteem documentatie](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade).

Als u aangepaste installatiekopieën gebruikt, kunt u de installatiekopie bijwerken door het bijwerken van de ID imageReference (meer informatie in de [REST API-documentatie](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

## <a name="examples"></a>Voorbeelden

### <a name="updating-the-os-image-for-your-scale-set"></a>Bijwerken van de installatiekopie van het besturingssysteem voor uw scale set

Stel dat er een schaalwaarde ingesteld met een oude versie van Ubuntu TNS 16.04 en u wilt bijwerken naar een nieuwere versie van Ubuntu TNS 16.04 (bijvoorbeeld versie 16.04.201801090). De eigenschap image verwijzing naar versie maakt geen deel uit van een lijst, zodat u kunt deze eigenschappen met de volgende opdrachten rechtstreeks wijzigen:

PowerShell: `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -ImageReferenceVersion 16.04.201801090`

CLI: `az vmss update -g {resourceGroupName} -n {vmScaleSetName} --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090`


### <a name="updating-the-load-balancer-for-your-scale-set"></a>Bijwerken van de load balancer voor de schaalset

Stel dat er een schaalwaarde ingesteld met een Azure Load Balancer en u wilt vervangen van de Azure Load Balancer met een Azure Application Gateway. De load balancer en application gateway-eigenschappen voor een set scale uitmaken deel van een lijst, zodat u de opdrachten kunt voor het verwijderen en de lijst met elementen in plaats van rechtstreeks wijzigen van de eigenschappen toe te voegen:

PowerShell: 
```
# get the current model of the scale set and store it in a local powershell object named $vmss
> $vmss=Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName}

# create a local powershell object for the new desired IP configuration, which includes the referencerence to the application gateway
> $ipconf = New-AzureRmVmssIPConfig myNic -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name

# replace the existing IP configuration in the local powershell object (which contains the references to the current Azure Load Balancer) with the new IP configuration
> $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf

# Update the model of the scale set with the new configuration in the local powershell object
> Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName} -virtualMachineScaleSet $vmss

```

CLI: 
```
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0 # remove the load balancer backend pool from the scale set model
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0 # remove the load balancer backend pool from the scale set model; only necessary if you have NAT pools configured on the scale set
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}' # add the application gateway backend pool to the scale set model
```

>[!NOTE]
> Deze opdrachten wordt ervan uitgegaan dat er slechts één IP-configuratie en load balancer op de scale-set is. Als er meerdere, moet u wellicht een lijstindex dan 0 gebruiken.