---
title: Begrijpen exemplaar-id's voor virtuele machines Azure VM-schaalset | Microsoft Docs
description: Exemplaar-id's voor schaal van de virtuele machine van Azure VM's ingesteld
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
ms.date: 02/22/2018
ms.author: negat
ms.openlocfilehash: 3a43dc86f1fb53dfde4bce3938faaa30e18f5a6d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="understand-instance-ids-for-azure-vm-scale-set-vms"></a>Exemplaar-id's voor schaal van de virtuele machine van Azure VM's ingesteld
In dit artikel beschrijft de exemplaar-id's voor schaalsets en de verschillende manieren waarop die ze surface.

## <a name="scale-set-instance-ids"></a>Schaalset exemplaar-id 's

Elke virtuele machine in een schaalset een unieke exemplaar-ID opgehaald. Dit exemplaar-ID wordt gebruikt in de set scale API's voor bewerkingen op een specifieke virtuele machine in de schaal is ingesteld. U kunt bijvoorbeeld een specifiek exemplaar-ID voor het terugzetten van de installatiekopie opgeven wanneer u het terugzetten van de installatiekopie API:

REST-API: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage))

PowerShell: `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (Zie voor meer informatie de [Powershell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm))

CLI: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (Zie voor meer informatie de [CLI documentatie](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage)).

U kunt de lijst met exemplaar-id's krijgen door alle exemplaren in een schaalset aan te bieden:

REST-API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualMachines?api-version={apiVersion}` (Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/list))

PowerShell: `Get-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (Zie voor meer informatie de [Powershell documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

CLI: `az vmss list-instances -g {resourceGroupName} -n {vmScaleSetName}` (Zie voor meer informatie de [CLI documentatie](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_list_instances)).

U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure SDK's](https://azure.microsoft.com/downloads/) voor een lijst met de virtuele machines in een schaalset.

De exacte presentatie van de uitvoer is afhankelijk van de opties die u met de opdracht opgeven, maar hier volgt een van voorbeelduitvoer van de CLI:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
[
  {
    "instanceId": "85",
    "latestModelApplied": true,
    "location": "westus",
    "name": "nsgvmss_85",
    .
    .
    .
```

Zoals u ziet, is de eigenschap 'instanceId' net een decimaal getal zijn. De exemplaar-id's mogelijk worden hergebruikt voor nieuwe exemplaren zodra oude instanties worden verwijderd.

>[!NOTE]
> Er is **geen garantie** op het exemplaar manier id's zijn toegewezen aan de virtuele machines in de schaalset. Ze lijken sequentieel toenemende op tijdstippen, maar dit is het niet altijd het geval is. Een afhankelijkheid worden pas van kracht op de specifieke manier waarin de exemplaar-id's zijn toegewezen aan de virtuele machines.

## <a name="scale-set-vm-names"></a>Namen van de VM-schaalset

In de bovenstaande voorbeelduitvoer is er ook een 'name' voor de virtuele machine. Deze naam heeft de vorm '_ {exemplaar-id} {scale-set-name}'. Dit is de naam die u in de Azure portal ziet als u exemplaren in een schaalset weergeven:

![](./media/virtual-machine-scale-sets-instance-ids/vmssInstances.png)

Het deel {exemplaar-id} van de naam is hetzelfde decimaal nummer als de eigenschap 'instanceId' eerder besproken.

## <a name="instance-metadata-vm-name"></a>Exemplaarnaam Metadata VM

Als u een query de [metagegevens van het exemplaar](../virtual-machines/windows/instance-metadata-service.md) van binnen een schaalset VM, u ziet een 'name' in de uitvoer:

```
{
  "compute": {
    "location": "westus",
    "name": "nsgvmss_85",
    .
    .
    .
```

Deze naam is hetzelfde als de naam van de eerder besproken.

## <a name="scale-set-vm-computer-name"></a>Computernaam van de VM-schaalset

Elke virtuele machine in een schaal ook haalt de naam van een computer toegewezen. De naam van deze computer is de hostnaam van de virtuele machine in de [Azure DNS-naamomzetting in het virtuele netwerk](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). De naam van deze computer heeft de vorm '{computer-name-prefix}{base-36-instance-id}'.

De {base-36-exemplaar-id} is in [36 baseren](https://en.wikipedia.org/wiki/Base36) en is altijd zes cijfers lang zijn. Als de basis 36 representatie van het getal minder dan zes cijfers duurt, wordt de {base-36-exemplaar-id} opgevuld met nullen zodat het zes cijfers lang zijn. Bijvoorbeeld, wordt een exemplaar met {--voorvoegsel voor de computernaam} 'nsgvmss' en de exemplaar-ID 85 computer name 'nsgvmss00002D' hebben.

>[!NOTE]
> Het voorvoegsel van de computer is een eigenschap van het scale set-model dat u instellen kunt, zodat deze van de naam van de schaal zelf afwijken kan.