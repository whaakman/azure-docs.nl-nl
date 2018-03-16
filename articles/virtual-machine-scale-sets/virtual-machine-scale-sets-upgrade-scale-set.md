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
ms.openlocfilehash: fcca912a8120a51d2f0a454ef0a6341cd5882015
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Wijzigen van een virtuele-machineschaalset
Dit artikel wordt beschreven hoe u een bestaande virtuele-machineschaalset wijzigt. Taken bevatten informatie over het wijzigen van de configuratie van de schaal is ingesteld, het wijzigen van de configuratie van de toepassingen die worden uitgevoerd op de schaal is ingesteld, het beheren van de beschikbaarheid en meer.

## <a name="fundamental-concepts"></a>basisconcepten

### <a name="scale-set-model"></a>Model-schaalset

Een schaalset heeft een model waarmee wordt vastgelegd de *gewenste* status van de schaal is ingesteld als geheel. Om te vragen het model voor een schaalset, kunt u het volgende gebruiken:

* REST-API: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` 
   
  Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/get).

* PowerShell:

  `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}`
   
  Zie voor meer informatie de [PowerShell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss).

* Azure CLI: 

  `az vmss show -g {resourceGroupName} -n {vmSaleSetName}` 
   
  Zie voor meer informatie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show).

U kunt ook [Azure Resourceverkenner (Preview)](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/) query uitvoeren op het model voor een schaalset.

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven. Hier volgt een voorbeeld van uitvoer van de Azure CLI:

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



### <a name="scale-set-instance-view"></a>Instantieweergave van de schaalset

Een schaal ook heeft een exemplaar weergave is die de huidige vastgelegd *runtime* status van de schaal is ingesteld als geheel. Om te vragen de instantieweergave voor een schaalset, kunt u het volgende gebruiken:

* REST-API: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version={apiVersion}` 
   
  Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getinstanceview).

* PowerShell: 

  `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceView` 
  
  Zie voor meer informatie de [PowerShell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss).

* Azure CLI: 

  `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName}` 
   
  Zie voor meer informatie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view).

U kunt ook [Azure Resourceverkenner (Preview)](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/) query uitvoeren op de weergave van het exemplaar voor een schaalset.

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven. Hier volgt een voorbeeld van uitvoer van de Azure CLI:

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

Zoals u ziet bieden deze eigenschappen een samenvatting van de huidige runtimestatus van de virtuele machines in de schaalset. Het overzicht bevat de status van de uitbreidingen die worden toegepast op de schaal (weggelaten als beknopt alternatief bevat) ingesteld.



### <a name="scale-set-vm-model-view"></a>Model-view VM-schaalset

Net als bij hoe een schaalset een modelweergave heeft, elke virtuele machine in de schaalset heeft een eigen model weergeven. Om te vragen van de modelweergave voor een schaalset, kunt u het volgende gebruiken:

* REST-API: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}?api-version={apiVersion}` 
  
  Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/get).

* PowerShell: 

  `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` 
  
  Zie voor meer informatie de [PowerShell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm).

* Azure CLI: 

  `az vmss show -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` 
  
  Zie voor meer informatie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show).

U kunt ook [Azure Resourceverkenner (Preview)](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/) query uitvoeren op het model voor een virtuele machine in een schaalset.

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven. Hier volgt een voorbeeld van uitvoer van de Azure CLI:

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

Zoals u ziet, wordt met deze eigenschappen de configuratie van de virtuele machine zelf, niet de configuratie van de schaal instelt als geheel beschreven. Het model van de set schaal heeft bijvoorbeeld `overprovision` als een eigenschap, dat het model voor een virtuele machine in een schaal niet. De reden voor dit verschil is dat de eigenschap voor de schaal ingesteld als een volledige, geen afzonderlijke virtuele machines in de schaalset overmatige inrichting is. (Zie voor meer informatie over overmatige inrichting [ontwerpoverwegingen voor schaalsets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning).)



### <a name="scale-set-vm-instance-view"></a>Instantieweergave van de VM-schaalset

Net als bij hoe een schaalset weergave met een exemplaar is, elke virtuele machine in de schaalset heeft een eigen instantieweergave. Om te vragen de instantieweergave voor een schaalset, kunt u het volgende gebruiken:

* REST-API: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}/instanceView?api-version={apiVersion}` 
 
  Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/getinstanceview).

* PowerShell: 

  `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -InstanceView` 
  
  Zie voor meer informatie de [PowerShell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm).

* Azure CLI: 

  `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` 
  
  Zie voor meer informatie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view).

U kunt ook [Azure Resourceverkenner (Preview)](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/) query uitvoeren op de weergave exemplaar voor een virtuele machine in een schaalset.

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven. Hier volgt een voorbeeld van uitvoer van de Azure CLI:

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

Zoals u ziet, wordt de huidige runtimestatus van de virtuele machine zelf beschreven in deze eigenschappen. De status omvat eventuele uitbreidingen die wordt toegepast op de schaal (weggelaten als beknopt alternatief bevat) ingesteld.




## <a name="techniques-for-updating-global-scale-set-properties"></a>Technieken voor het bijwerken van wereldwijde schaal eigenschappen instellen

Voor het bijwerken van een globale scale set-eigenschap, moet u de eigenschap in het model van de set schaal bijwerken. U kunt deze update via doen:

* REST-API: 

  `PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` 
  
  Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate).

  U kunt ook een Azure Resource Manager-sjabloon implementeren met behulp van de eigenschappen van de REST-API om globale scale seteigenschappen te werken.

* PowerShell: 

  `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -VirtualMachineScaleSet {scaleSetConfigPowershellObject}` 
  
  Zie voor meer informatie de [PowerShell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss).

* Azure CLI:

  * Een eigenschap te wijzigen: `az vmss update --set {propertyPath}={value}` 
  
  * Een object toevoegen aan een lijsteigenschap in een schaalset: `az vmss update --add {propertyPath} {JSONObjectToAdd}` 
  
  * Een object uit een lijsteigenschap in een schaalset verwijderen: `az vmss update --remove {propertyPath} {indexToRemove}` 
  
  Zie voor meer informatie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update). 
  
  U kunt ook als u de schaal instelt met behulp van de eerder hebt geïmplementeerd de `az vmss create` uitvoert, kunt u uitvoeren en de `az vmss create` opdracht voor het bijwerken van de schaalaanpassingsset. U doet dit door ervoor te zorgen dat alle eigenschappen in de `az vmss create` opdracht zijn hetzelfde als voorheen, met uitzondering van de eigenschappen die u wilt wijzigen.



U kunt ook [Azure Resourceverkenner (Preview)](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/) model bijwerken van de schaal instelt.

Nadat het scale set-model is bijgewerkt, wordt de nieuwe configuratie is van toepassing op nieuwe VM's in de schaalset gemaakt. Echter, de modellen voor de bestaande virtuele machines in de schaalset moeten nog steeds worden gezet up-to-date met de meest recente algehele scale set model. In het model voor elke virtuele machine, een Boole-eigenschap aangeroepen `latestModelApplied` geeft aan of de virtuele machine bijgewerkt met de meest recente algehele scale set model is. (Een waarde van `true` betekent dat de virtuele machine is bijgewerkt met de meest recente model.)




## <a name="techniques-for-bringing-vms-up-to-date-with-the-latest-scale-set-model"></a>Technieken voor virtuele machines is up-to-date met de meest recente scale set-model te brengen

-Schaalsets hebben een *Upgradebeleid* waarmee wordt bepaald hoe virtuele machines up-to-date met de meest recente model met een schaal worden gebracht. De drie beschikbare modi voor het Upgradebeleid zijn:

- **Automatische**: In deze modus kan de schaalaanpassingsset kan niet garanderen over de volgorde van virtuele machines die worden verbroken. De schaalaanpassingsset kan duren voordat u alle virtuele machines op hetzelfde moment. 
- **Rolling**: In deze modus kan de schaalaanpassingsset implementeert de update in batches met een optionele onderbreken tijd tussen batches.
- **Handmatige**: In deze modus tijdens het bijwerken van het model van de set schaal gebeurt er niets aan bestaande virtuele machines. Voor het bijwerken van bestaande virtuele machines, moet u elk criterium handmatig bijwerken. U kunt deze handmatige upgrade via doen:

  - REST-API: 
  
    `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/manualupgrade?api-version={apiVersion}` 
    
    Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/updateinstances).

  - PowerShell: 
  
    `Update-AzureRmVmssInstance -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` 
    
    Zie voor meer informatie de [PowerShell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance).

  - Azure CLI: 
  
    `az vmss update-instances -g {resourceGroupName} -n {vmScaleSetName} --instance-ids {instanceIds}` 
    
    Zie voor meer informatie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update_instances).

  U kunt ook de [Azure SDK's](https://azure.microsoft.com/downloads/) handmatig bijwerken van een virtuele machine in een schaalset.

>[!NOTE]
> Azure Service Fabric-clusters alleen automatische modus kunnen gebruiken, maar de update wordt anders afgehandeld. Zie voor meer informatie over Service Fabric-updates de [Service Fabric-documentatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade).

Een type wijziging aan de globale scale seteigenschappen niet voldoet aan het Upgradebeleid: wijzigingen in de schaal OS profiel instellen. (Voorbeelden zijn admin-gebruikersnaam en wachtwoord). Deze eigenschappen kunnen worden gewijzigd in API-versie 2017-12-01 alleen of hoger. Deze wijzigingen gelden alleen voor virtuele machines die zijn gemaakt nadat de wijziging in de schaal model instelt. Voor het maken van bestaande virtuele machines up-to-date te houden, moet u elke bestaande VM installatiekopie. U installatiekopie een virtuele machine via:

* REST-API: 

  `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` 
  
  Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage).

* PowerShell: 

  `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` 
  
  Zie voor meer informatie de [PowerShell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm).

* Azure CLI: 

  `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` 
  
  Zie voor meer informatie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage).

U kunt ook de [Azure SDK's](https://azure.microsoft.com/downloads/) naar de installatiekopie van een virtuele machine in een schaalset.




## <a name="properties-with-restrictions-on-modification"></a>Eigenschappen met beperkingen voor wijziging

### <a name="create-time-properties"></a>Eigenschappen maken tijd

Sommige eigenschappen kunnen alleen worden ingesteld als u eerst de schaalaanpassingsset. Deze eigenschappen omvatten:

- Zones
- referentie-installatiekopie-uitgever
- afbeelding verwijzing aanbieding

### <a name="properties-that-can-be-changed-based-on-the-current-value-only"></a>Eigenschappen die kunnen worden gewijzigd op basis van de huidige waarde alleen

Sommige eigenschappen kunnen worden gewijzigd met uitzonderingen, afhankelijk van de huidige waarde. Deze eigenschappen omvatten:

- `singlePlacementGroup`: Als `singlePlacementGroup` is ingesteld op true, false kan worden gewijzigd. Echter, als `singlePlacementGroup` is ingesteld op false, het *kan niet* worden gewijzigd op true.
- `subnet`: Het subnet van een scale-set kan worden gewijzigd zolang het oorspronkelijke subnet en het nieuwe subnet bevinden zich in hetzelfde virtuele netwerk.

### <a name="properties-that-require-deallocation-to-change"></a>Eigenschappen waarvoor de toewijzing is opgeheven wijzigen

Sommige eigenschappen kunnen worden gewijzigd in bepaalde waarden alleen als de toewijzing van de virtuele machines in de schaalset worden opgeheven. Deze eigenschappen omvatten:

- `sku name`: Als de SKU van de nieuwe VM wordt niet ondersteund op de hardware die de schaalaanpassingsset zich momenteel op, moet u de virtuele machines in de schaal instellen voordat u wijzigingen ongedaan `sku name`. Zie voor meer informatie over het vergroten of verkleinen van virtuele machines [dit blogbericht Azure](https://azure.microsoft.com/blog/resize-virtual-machines/).


## <a name="vm-specific-updates"></a>VM-specifieke updates

Bepaalde wijzigingen kunnen worden toegepast op een specifieke virtuele machines in plaats van de globale scale seteigenschappen. Op dit moment de enige VM-specifieke update die wordt ondersteund is koppelen/loskoppelen gegevensschijven naar/van de virtuele machines in de schaalset. Deze functie is in preview. Zie voor meer informatie de [preview-documentatie](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).

## <a name="scenarios"></a>Scenario's

### <a name="application-updates"></a>Updates voor toepassingen

Als een toepassing wordt geïmplementeerd op een schaal ingesteld door middel van, het bijwerken van de configuratie voor de uitbreiding zorgt ervoor dat de toepassing om te worden bijgewerkt in overeenstemming met het Upgradebeleid. Bijvoorbeeld, als er een nieuwe versie van een script uit te voeren in een extensie voor aangepaste scripts, u mogelijk bijwerken de `fileUris` eigenschap verwijzen naar het nieuwe script. 

In sommige gevallen is het raadzaam om af te dwingen een update, ondanks dat de configuratie van de extensie niet gewijzigd is. (Bijvoorbeeld, u bijgewerkt het script zonder de URI van het script wijzigen.) In dergelijke gevallen kunt u `forceUpdateTag` om af te dwingen een update. De Azure-platform biedt deze eigenschap niet interpreteren, zodat de waarde ervan wijzigen geen invloed heeft op hoe de uitbreiding wordt uitgevoerd. Wijzigen van het simpelweg zorgt ervoor dat de uitbreiding opnieuw uit te voeren. 

Voor meer informatie over `forceUpdateTag`, Zie de [REST-API-documentatie voor uitbreidingen](https://docs.microsoft.com/rest/api/compute/virtualmachineextensions/createorupdate).

Het is ook gebruikelijk dat toepassingen worden geïmplementeerd via een aangepaste installatiekopie. Dit scenario wordt beschreven in de volgende sectie.

### <a name="os-updates"></a>Updates voor het besturingssysteem

Als u installatiekopieën van het platform, kunt u de installatiekopieën bijwerken door het wijzigen van `imageReference`. Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate).

>[!NOTE]
> Installatiekopieën van het platform worden vaak 'nieuwste' opgeven voor de versie van de verwijzing naar afbeelding. Dit betekent dat wanneer schaalsets worden gemaakt, geschaalde uit en hersteld met een installatiekopie, de virtuele machines worden gemaakt met de meest recente versie. Echter, het *heeft geen* betekenen dat de installatiekopie van het besturingssysteem automatisch na verloop van tijd bijgewerkt wordt als nieuwe versies van een installatiekopie zijn vrijgegeven. Dit is een afzonderlijke functie, die momenteel in preview. Zie voor meer informatie [OS automatische upgrades](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade).

Als u aangepaste installatiekopieën gebruikt, kunt u de installatiekopieën bijwerken door het bijwerken van de `imageReference` -id. Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate).

## <a name="examples"></a>Voorbeelden

### <a name="update-the-os-image-for-your-scale-set"></a>Bijwerken van de installatiekopie van het besturingssysteem voor uw scale set

Stel dat u hebt een ingesteld met een oude versie van Ubuntu TNS 16.04 schaal. U wilt bijwerken naar een nieuwere versie van Ubuntu TNS 16.04 (bijvoorbeeld versie 16.04.201801090). De eigenschap image verwijzing naar versie maakt geen deel uit van een lijst, zodat u deze eigenschappen rechtstreeks wijzigen kunt met behulp van deze opdrachten:

* PowerShell: 

  `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -ImageReferenceVersion 16.04.201801090`

* Azure CLI: 

  `az vmss update -g {resourceGroupName} -n {vmScaleSetName} --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090`


### <a name="update-the-load-balancer-for-your-scale-set"></a>De load balancer voor uw scale set bijwerken

Stel dat er een schaalwaarde ingesteld met een Azure load balancer en u wilt vervangen van de load balancer met een Azure application gateway. De load balancer en application gateway-eigenschappen voor een set scale uitmaken deel van een lijst. Ja, kunt u de opdrachten voor het verwijderen en de lijst met elementen in plaats van rechtstreeks wijzigen van de eigenschappen toe te voegen.

PowerShell:
```
# Get the current model of the scale set and store it in a local PowerShell object named $vmss
> $vmss=Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName}

# Create a local PowerShell object for the new desired IP configuration, which includes the reference to the application gateway
> $ipconf = New-AzureRmVmssIPConfig myNic -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name

# Replace the existing IP configuration in the local PowerShell object (which contains the references to the current Azure load balancer) with the new IP configuration
> $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf

# Update the model of the scale set with the new configuration in the local PowerShell object
> Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName} -virtualMachineScaleSet $vmss

```

Azure CLI:
```
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0 # Remove the load balancer back-end pool from the scale set model
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0 # Remove the load balancer back-end pool from the scale set model; only necessary if you have NAT pools configured on the scale set
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}' # Add the application gateway back-end pool to the scale set model
```

>[!NOTE]
> Deze opdrachten wordt ervan uitgegaan dat er slechts één IP-configuratie en load balancer op de scale-set is. Als er meerdere zijn, moet u mogelijk gebruik van een lijstindex dan 0.