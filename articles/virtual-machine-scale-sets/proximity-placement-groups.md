---
title: Voor beeld van proximity placement groups voor virtuele-machine schaal sets | Microsoft Docs
description: Meer informatie over het maken en gebruiken van proximity-plaatsings groepen voor schaal sets voor virtuele Windows-machines in Azure.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/01/2019
ms.author: cynthn
ms.openlocfilehash: 6a4f145c6431e98bbe9575f128ace30a23a1b972
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850351"
---
# <a name="preview-creating-and-using-proximity-placement-groups-using-powershell"></a>Preview: Proximity-plaatsings groepen maken en gebruiken met Power shell

Als u virtuele machines zo dicht mogelijk wilt ophalen, moet u de schaalset binnen een [proximity-plaatsings groep](co-location.md#preview-proximity-placement-groups)implementeren.

Een proximity-plaatsings groep is een logische groepering die wordt gebruikt om ervoor te zorgen dat Azure Compute-resources zich fysiek dicht bij elkaar bevinden. Proximity-plaatsings groepen zijn handig voor werk belastingen waarbij lage latentie een vereiste is.

> [!IMPORTANT]
> Proximity-plaatsings groepen bevindt zich momenteel in een open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
>
> Proximity-plaatsings groepen zijn niet beschikbaar in deze regio's tijdens de preview-versie: **Japan-Oost**, **Australië-Oost** en **India centraal**.


## <a name="create-a-proximity-placement-group"></a>Een nabijheidsplaatsingsgroep maken
Maak een proximity-plaatsings groep met behulp van de cmdlet [New-AzProximityPlacementGroup](https://docs.microsoft.com/powershell/module/az.compute/new-azproximityplacementgroup) . 

```azurepowershell-interactive
$resourceGroup = "myPPGResourceGroup"
$location = "East US"
$ppgName = "myPPG"
New-AzResourceGroup -Name $resourceGroup -Location $location
$ppg = New-AzProximityPlacementGroup `
   -Location $location `
   -Name $ppgName `
   -ResourceGroupName $resourceGroup `
   -ProximityPlacementGroupType Standard
```

## <a name="list-proximity-placement-groups"></a>Proximity-plaatsings groepen weer geven

U kunt alle proximity-plaatsings groepen weer geven met behulp van de cmdlet [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup) .

```azurepowershell-interactive
Get-AzProximityPlacementGroup
```


## <a name="create-a-scale-set"></a>Een schaalset maken

Maak een schaal in de locatie van de Proximity `-ProximityPlacementGroup $ppg.Id` -plaatsings groep met om te verwijzen naar de locatie van de plaatsings groep bij gebruik van [New-AzVMSS](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss) om de schaalset te maken.

```azurepowershell-interactive
$scalesetName = "myVM"

New-AzVmss `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -VMScaleSetName $scalesetName `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic" `
  -ProximityPlacementGroup $ppg.Id
```

U kunt het exemplaar in de plaatsings groep bekijken met [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup).

```azurepowershell-interactive
  Get-AzProximityPlacementGroup `
   -ResourceId $ppg.Id | Format-Table `
   -Wrap `
   -Property VirtualMachineScaleSets
```

## <a name="next-steps"></a>Volgende stappen

U kunt ook de [Azure cli](../virtual-machines/linux/proximity-placement-groups.md) gebruiken om proximity-plaatsings groepen te maken.