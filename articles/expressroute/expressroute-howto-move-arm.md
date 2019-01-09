---
title: 'Circuits verplaatsen van klassiek naar Resource Manager - ExpressRoute: PowerShell: Azure | Microsoft Docs'
description: Deze pagina wordt beschreven hoe u een klassieke circuit verplaatsen naar het Resource Manager-implementatiemodel met behulp van PowerShell.
services: expressroute
author: ganesr
ms.service: expressroute
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ganesr;cherylmc
ms.custom: seodec18
ms.openlocfilehash: 984ccfa9bad99281418ba891ce188536ae13d8e5
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106763"
---
# <a name="move-expressroute-circuits-from-classic-to-resource-manager-deployment-model-using-powershell"></a>ExpressRoute-circuits verplaatsen van klassiek naar Resource Manager-implementatiemodel met behulp van PowerShell

Voor het gebruik van een ExpressRoute-circuit voor zowel het klassieke en Resource Manager-implementatiemodel, moet u het circuit verplaatsen naar het Resource Manager-implementatiemodel. De volgende secties helpen u uw circuit verplaatsen met behulp van PowerShell.

## <a name="before-you-begin"></a>Voordat u begint

* Controleer of u de nieuwste versie van de Azure PowerShell-modules (ten minste versie 1.0). Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.
* Zorg ervoor dat u hebt bekeken de [vereisten](expressroute-prerequisites.md), [routeringsvereisten](expressroute-routing.md), en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.
* Lees de informatie die is opgegeven onder [een ExpressRoute-circuit verplaatsen van klassiek naar Resource Manager](expressroute-move.md). Zorg ervoor dat u volledig inzicht in limieten en beperkingen.
* Controleer of het circuit volledig operationeel zijn in het klassieke implementatiemodel.
* Zorg ervoor dat u hebt een resourcegroep die is gemaakt in het Resource Manager-implementatiemodel.

## <a name="move-an-expressroute-circuit"></a>Een ExpressRoute-circuit verplaatsen

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Stap 1: Circuit gegevens verzamelen vanuit het klassieke implementatiemodel

Aanmelden bij de klassieke Azure-omgeving en de servicesleutel verzamelen.

1. Meld u aan bij uw Azure-account.

  ```powershell
  Add-AzureAccount
  ```

2. Selecteer het juiste Azure-abonnement.

  ```powershell
  Select-AzureSubscription "<Enter Subscription Name here>"
  ```

3. Importeer de PowerShell-modules voor Azure en ExpressRoute.

  ```powershell
  Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\Azure\Azure.psd1'
  Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\ExpressRoute\ExpressRoute.psd1'
  ```

4. Gebruik de volgende cmdlet om de servicesleutels voor al uw ExpressRoute-circuits. Bij het ophalen van de sleutels, Kopieer de **servicesleutel** van het circuit die u wilt verplaatsen naar het Resource Manager-implementatiemodel.

  ```powershell
  Get-AzureDedicatedCircuit
  ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a>Stap 2: Meld u aan en maak een resourcegroep

Aanmelden bij de Resource Manager-omgeving en maak een nieuwe resourcegroep.

1. Aanmelden bij uw Azure Resource Manager-omgeving.

  ```powershell
  Connect-AzureRmAccount
  ```

2. Selecteer het juiste Azure-abonnement.

  ```powershell
  Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription
  ```

3. Wijzig het codefragment hieronder om een nieuwe resourcegroep maken als u nog een resourcegroep hebt.

  ```powershell
  New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"
  ```

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Stap 3: Het ExpressRoute-circuit verplaatsen naar het Resource Manager-implementatiemodel

U bent nu klaar om te verplaatsen van uw ExpressRoute-circuit vanuit het klassieke implementatiemodel naar het Resource Manager-implementatiemodel. Voordat u doorgaat, bekijk de informatie in [een ExpressRoute-circuit verplaatsen van het klassieke naar het Resource Manager-implementatiemodel](expressroute-move.md).

Uw circuit verplaatsen, wijzigen en voer het volgende codefragment:

```powershell
Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

Een ExpressRoute-circuit heeft in de klassieke modus geen het concept van wordt gekoppeld aan een regio. Echter, in Resource Manager elke resource moet worden toegewezen aan een Azure-regio. De regio die is opgegeven in de cmdlet Move-AzureRmExpressRouteCircuit kan technisch zijn voor elke regio. Voor organisatie-doeleinden, kunt u een regio die nauw uw peeringlocatie vertegenwoordigt kiezen.

> [!NOTE]
> Nadat de verplaatsing is voltooid, wordt de nieuwe naam die wordt vermeld in de vorige cmdlet worden gebruikt om het adres van de resource. Het circuit wordt in wezen worden gewijzigd.
> 

## <a name="modify-circuit-access"></a>Circuit wijzigen

### <a name="to-enable-expressroute-circuit-access-for-both-deployment-models"></a>Voor ExpressRoute-circuit toegang voor beide implementatiemodellen

Na het verplaatsen van uw ExpressRoute-circuit van klassiek naar het Resource Manager-implementatiemodel, kunt u toegang tot beide implementatiemodellen. Voer de volgende cmdlets voor toegang tot beide implementatiemodellen:

1. De details van het circuit ophalen.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. Ingesteld op 'Klassieke bewerkingen toestaan' op ' True '.

  ```powershell
  $ckt.AllowClassicOperations = $true
  ```

3. Bijwerken van het circuit. Nadat u deze bewerking is voltooid, kunt u zich het circuit weergeven in het klassieke implementatiemodel.

  ```powershell
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

4. Voer de volgende cmdlet als u de details van het ExpressRoute-circuit. U moet mogelijk zijn om te zien van de sleutel weergegeven.

  ```powershell
  get-azurededicatedcircuit
  ```

5. U kunt nu koppelingen naar het ExpressRoute-circuit met behulp van de klassieke implementatie model-opdrachten voor klassieke VNets en het Resource Manager-opdrachten voor Resource Manager VNets beheren. De volgende artikelen helpen u bij het beheren van koppelingen naar het ExpressRoute-circuit:

    * [Uw virtueel netwerk koppelen aan uw ExpressRoute-circuit in het Resource Manager-implementatiemodel](expressroute-howto-linkvnet-arm.md)
    * [Uw virtueel netwerk koppelen aan uw ExpressRoute-circuit in het klassieke implementatiemodel](expressroute-howto-linkvnet-classic.md)

### <a name="to-disable-expressroute-circuit-access-to-the-classic-deployment-model"></a>ExpressRoute-circuit toegang tot het klassieke implementatiemodel uitschakelen

Voer de volgende cmdlets om uit te schakelen toegang tot het klassieke implementatiemodel.

1. Details van het ExpressRoute-circuit ophalen.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. Ingesteld op 'Klassieke bewerkingen toestaan' op FALSE.

  ```powershell
  $ckt.AllowClassicOperations = $false
  ```

3. Bijwerken van het circuit. Nadat u deze bewerking is voltooid, wordt het niet mogelijk om weer te geven van het circuit in het klassieke implementatiemodel.

  ```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

## <a name="next-steps"></a>Volgende stappen

* [Routering voor uw ExpressRoute-circuit maken en wijzigen](expressroute-howto-routing-arm.md)
* [Uw virtueel netwerk koppelen aan uw ExpressRoute-circuit](expressroute-howto-linkvnet-arm.md)
