---
title: Inzicht in de exemplaar-id's voor Azure VM-VM's schaalset | Microsoft Docs
description: Inzicht in de dat exemplaar-id's voor Azure VM-schaalset VM's instellen
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: manayar
ms.openlocfilehash: 5d92aa78b132b661af3cdc53b47c0340a0758dd8
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54855309"
---
# <a name="understand-instance-ids-for-azure-vm-scale-set-vms"></a>Inzicht in de dat exemplaar-id's voor Azure VM-schaalset VM's instellen
Dit artikel beschrijft de exemplaar-id's voor schaalsets en de verschillende manieren om die ze ontwerpoppervlak voor pijplijnen.

## <a name="scale-set-instance-ids"></a>Virtual Machine Scale sets exemplaar-id 's

Elke virtuele machine in een schaalset opgehaald exemplaar-ID die deze kan worden aangeduid. Dit exemplaar-ID wordt gebruikt in de schaalset API's voor bewerkingen op een specifieke virtuele machine in de schaalset ingesteld. U kunt bijvoorbeeld een specifiek exemplaar-ID voor het terugzetten van een installatiekopie opgeven bij het gebruik van het terugzetten van een installatiekopie API:

REST-API: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage))

PowerShell: `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (Zie voor meer informatie de [Powershell-documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm))

CLI: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (Zie voor meer informatie de [CLI-documentatie](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage)).

U kunt de lijst met de exemplaar-id's krijgen door alle exemplaren in een schaalset weer te geven:

REST-API: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualMachines?api-version={apiVersion}` (Zie voor meer informatie de [REST API-documentatie](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/list))

PowerShell: `Get-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (Zie voor meer informatie de [Powershell-documentatie](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

CLI: `az vmss list-instances -g {resourceGroupName} -n {vmScaleSetName}` (Zie voor meer informatie de [CLI-documentatie](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest)).

U kunt ook [resources.azure.com](https://resources.azure.com) of de [Azure-SDK's](https://azure.microsoft.com/downloads/) om de virtuele machines in een schaalset weer te geven.

De exacte presentatie van de uitvoer is afhankelijk van de opties die u aan de opdracht opgeven, maar hier volgt een voorbeeld van uitvoer van de CLI:

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

Zoals u ziet, is de eigenschap 'instanceId' alleen een decimaal getal. De exemplaar-id's kan worden hergebruikt voor nieuwe exemplaren zodra oude exemplaren worden verwijderd.

>[!NOTE]
> Er is **bieden geen garantie voor** op het exemplaar manier id's zijn toegewezen aan de virtuele machines in de schaalset. Ze altijd sequentieel toenemende mogelijk lijken, maar dit is niet altijd het geval is. Een afhankelijkheid niet uitvoert op de specifieke manier waarin de exemplaar-id's zijn toegewezen aan de virtuele machines.

## <a name="scale-set-vm-names"></a>Namen van de VM-schaalset

In het voorbeeld van uitvoer hierboven is er ook een 'naam' voor de virtuele machine. Deze naam heeft de vorm '{schaal-set-name} _ {exemplaar-id}'. Dit is de naam die u in Azure portal ziet wanneer u exemplaren in een schaalset:

![](./media/virtual-machine-scale-sets-instance-ids/vmssInstances.png)

De {exemplaar-id} deel van de naam is de dezelfde decimaal getal als de eigenschap 'instanceId' die eerder zijn besproken.

## <a name="instance-metadata-vm-name"></a>Instance Metadata VM-naam

Als u een query de [metagegevens van het exemplaar](../virtual-machines/windows/instance-metadata-service.md) uit in een schaalset VM, ziet u een 'name' in de uitvoer:

```
{
  "compute": {
    "location": "westus",
    "name": "nsgvmss_85",
    .
    .
    .
```

Deze naam is hetzelfde als de naam die eerder zijn besproken.

## <a name="scale-set-vm-computer-name"></a>Computernaam van de VM-schaalset

Elke virtuele machine in een schaalset ook Hiermee haalt u de naam van een computer zijn toegewezen. De naam van deze computer is de hostnaam van de virtuele machine in de [Azure DNS-naamomzetting binnen het virtuele netwerk](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). De naam van deze computer heeft de vorm '{computer-name-prefix}{base-36-instance-id}'.

De {base-36-exemplaar-id} is in [36 baseren](https://en.wikipedia.org/wiki/Base36) en is altijd zes cijfers lang. Als de basis 36 representatie van het getal minder dan zes cijfers duurt, wordt de {base-36-exemplaar-id} opgevuld met nullen om het u gemakkelijk zes cijfers lang. Bijvoorbeeld, wordt een exemplaar met {naam-computer-voorvoegsel} "nsgvmss" en exemplaar-ID 85 computer de naam 'nsgvmss00002D' hebben.

>[!NOTE]
> Het voorvoegsel van de computer is een eigenschap van het model met een schaalset die u instellen kunt, zodat deze van de naam van de schaal zelf afwijken kan.
