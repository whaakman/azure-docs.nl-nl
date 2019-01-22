---
title: 'Opnieuw instellen van een mislukt circuit - ExpressRoute: PowerShell: Azure | Microsoft Docs'
description: Dit artikel helpt u opnieuw instellen van een ExpressRoute-circuit bevindt zich in een foutstatus.
services: expressroute
author: anzaman
ms.service: expressroute
ms.topic: article
ms.date: 11/28/2018
ms.author: anzaman
ms.custom: seodec18
ms.openlocfilehash: e94ba2f7f6981355f4736683b891fbdced001d03
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54425336"
---
# <a name="reset-a-failed-expressroute-circuit"></a>Opnieuw instellen van een mislukte ExpressRoute-circuit

Als een bewerking op een ExpressRoute-circuit niet wordt voltooid, kan het circuit gaat u in een status 'mislukt'. Dit artikel helpt u opnieuw instellen van een mislukte Azure ExpressRoute-circuit.

## <a name="reset-a-circuit"></a>Een circuit opnieuw instellen

1. Installeer de meest recente versie van de PowerShell-cmdlets van Azure Resource Manager. Zie [Azure PowerShell installeren en configureren](/powershell/azure/azurerm/install-azurerm-ps) voor meer informatie.

2. Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

  ```azurepowershell-interactive
  Connect-AzureRmAccount
  ```
3. Als u meerdere Azure-abonnementen hebt, controleert u de abonnementen voor het account.

  ```azurepowershell-interactive
  Get-AzureRmSubscription
  ```
4. Geef het abonnement op dat u wilt gebruiken.

  ```azurepowershell-interactive
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Voer de volgende opdrachten in te stellen van een circuit die bevindt zich in een foutstatus:

  ```azurepowershell-interactive
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

Het circuit moet nu in orde zijn. Open een ondersteuningsticket met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als het circuit nog steeds in een foutstatus is.

## <a name="next-steps"></a>Volgende stappen

Open een ondersteuningsticket met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u nog steeds problemen ondervindt.
