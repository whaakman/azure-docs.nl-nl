---
title: 'ExpressRoute-circuits verplaatsen van klassiek naar Resource Manager: PowerShell: Azure | Microsoft Docs'
description: Deze pagina wordt beschreven hoe een klassieke circuit verplaatsen naar het Resource Manager-implementatiemodel met behulp van PowerShell.
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 08152836-23e7-42d1-9a56-8306b341cd91
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/03/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: c407e01e6d881cb8adcfe55faa246468669be883
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model-using-powershell"></a>ExpressRoute-circuits verplaatsen van het klassieke naar het Resource Manager-implementatiemodel met behulp van PowerShell

Voor het gebruik van een ExpressRoute-circuit voor zowel het klassieke implementatiemodel van Resource Manager, moet u het circuit verplaatsen naar het Resource Manager-implementatiemodel. De volgende secties helpen u uw circuit verplaatsen met behulp van PowerShell.

## <a name="before-you-begin"></a>Voordat u begint

* Controleer of u de nieuwste versie van de Azure PowerShell-modules (ten minste versie 1.0). Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.
* Zorg ervoor dat u hebt bekeken de [vereisten](expressroute-prerequisites.md), [routeringsvereisten](expressroute-routing.md), en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.
* Lees de informatie die is opgegeven onder [een ExpressRoute-circuit verplaatsen van klassiek naar Resource Manager](expressroute-move.md). Zorg ervoor dat u volledig limieten en beperkingen begrijpt.
* Controleer of het circuit volledig operationeel is in het klassieke implementatiemodel.
* Zorg ervoor dat u een resourcegroep die is gemaakt in het Resource Manager-implementatiemodel.

## <a name="move-an-expressroute-circuit"></a>Een ExpressRoute-circuit verplaatsen

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Stap 1: De details van het circuit vanuit het klassieke implementatiemodel verzamelen

Aanmelden bij de klassieke Azure-omgeving en de servicesleutel verzamelen.

1. Aanmelden bij uw Azure-account.

  ```powershell
  Add-AzureAccount
  ```

2. Selecteer het juiste Azure-abonnement.

  ```powershell
  Select-AzureSubscription "<Enter Subscription Name here>"
  ```

3. Importeer de PowerShell-modules voor Azure en ExpressRoute.

  ```powershell
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
  ```

4. De onderstaande cmdlet gebruiken om op te halen van de service-sleutels voor al uw ExpressRoute-circuits. Bij het ophalen van de sleutels, kopieert u de **servicesleutel** van het circuit die u wilt verplaatsen naar het Resource Manager-implementatiemodel.

  ```powershell
  Get-AzureDedicatedCircuit
  ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a>Stap 2: Aanmelden en een resourcegroep maken

Aanmelden bij de Resource Manager-omgeving en maak een nieuwe resourcegroep.

1. Aanmelden bij uw Azure Resource Manager-omgeving.

  ```powershell
  Login-AzureRmAccount
  ```

2. Selecteer het juiste Azure-abonnement.

  ```powershell
  Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription
  ```

3. Wijzig het codefragment hieronder een nieuwe resourcegroep maken als u een resourcegroep nog geen hebt.

  ```powershell
  New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"
  ```

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Stap 3: De ExpressRoute-circuit verplaatsen naar het Resource Manager-implementatiemodel

U bent nu klaar voor het ExpressRoute-circuit verplaatsen van het klassieke implementatiemodel naar het Resource Manager-implementatiemodel. Bekijk de informatie in voordat u doorgaat, [een ExpressRoute-circuit verplaatsen van het klassieke naar het Resource Manager-implementatiemodel](expressroute-move.md).

Uw circuit te verplaatsen, wijzigen en voer het volgende fragment:

```powershell
Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

> [!NOTE]
> Nadat de verplaatsing is voltooid, wordt de nieuwe naam die wordt vermeld in de vorige cmdlet gebruikt voor het oplossen van de resource. In wezen het circuit gewijzigd.
> 

## <a name="modify-circuit-access"></a>Circuit toegang wijzigen

### <a name="to-enable-expressroute-circuit-access-for-both-deployment-models"></a>ExpressRoute-circuit toegang voor beide implementatiemodellen inschakelen

Nadat u het klassieke ExpressRoute-circuit naar het Resource Manager-implementatiemodel, kunt u toegang tot beide implementatiemodellen. Voer de volgende cmdlets voor toegang tot beide implementatiemodellen:

1. De circuit-details ophalen.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. Ingesteld op 'Klassieke bewerkingen toestaan' op TRUE.

  ```powershell
  $ckt.AllowClassicOperations = $true
  ```

3. Werk het circuit. Nadat u deze bewerking is voltooid, kunt u zich kunt u het circuit in het klassieke implementatiemodel weergeven.

  ```powershell
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

4. Voer de volgende cmdlet als u de details van het ExpressRoute-circuit. U moet de servicesleutel vermeld zien.

  ```powershell
  get-azurededicatedcircuit
  ```

5. U kunt nu koppelingen naar het ExpressRoute-circuit met de klassieke implementatie model-opdrachten voor klassieke vnet's en de Resource Manager-opdrachten voor Resource Manager VNets beheren. De volgende artikelen kunnen u koppelingen naar het ExpressRoute-circuit beheren:

    * [Het virtuele netwerk koppelen aan uw ExpressRoute-circuit in het Resource Manager-implementatiemodel](expressroute-howto-linkvnet-arm.md)
    * [Het virtuele netwerk koppelen aan uw ExpressRoute-circuit in het klassieke implementatiemodel](expressroute-howto-linkvnet-classic.md)

### <a name="to-disable-expressroute-circuit-access-to-the-classic-deployment-model"></a>ExpressRoute-circuit toegang tot het klassieke implementatiemodel uitschakelen

Voer de volgende cmdlets voor toegang tot het klassieke implementatiemodel uitschakelen.

1. Details van het ExpressRoute-circuit ophalen.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. Ingesteld op 'Klassieke bewerkingen toestaan' op FALSE.

  ```powershell
  $ckt.AllowClassicOperations = $false
  ```

3. Werk het circuit. Nadat u deze bewerking is voltooid, is het niet mogelijk het circuit weergeven in het klassieke implementatiemodel.

  ```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

## <a name="next-steps"></a>Volgende stappen

* [Maken en aanpassen van routering voor ExpressRoute-circuit](expressroute-howto-routing-arm.md)
* [Het virtuele netwerk koppelen aan uw ExpressRoute-circuit](expressroute-howto-linkvnet-arm.md)