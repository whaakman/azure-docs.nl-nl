---
title: 'Opnieuw instellen van een mislukte Azure ExpressRoute-circuit: PowerShell | Microsoft Docs'
description: In dit artikel helpt u bij opnieuw instellen van een ExpressRoute-circuit bevindt zich in een foutstatus.
documentationcenter: na
services: expressroute
author: anzaman
manager: 
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: 0e017200193de3e4a02275cec3b09c32f1fa5c31
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="reset-a-failed-expressroute-circuit"></a>Opnieuw instellen van een mislukte ExpressRoute-circuit

Wanneer een bewerking op een ExpressRoute-circuit niet wordt voltooid, wordt het circuit kan overschakelen naar een status 'mislukt'. In dit artikel helpt u bij opnieuw instellen van een mislukte Azure ExpressRoute-circuit.

## <a name="reset-a-circuit"></a>Opnieuw instellen van een circuit

1. Installeer de meest recente versie van de PowerShell-cmdlets van Azure Resource Manager. Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps) voor meer informatie.

2. Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

  ```powershell
  Login-AzureRmAccount
  ```
3. Als u meerdere Azure-abonnementen hebt, controleert u de abonnementen voor het account.

  ```powershell
  Get-AzureRmSubscription
  ```
4. Geef het abonnement op dat u wilt gebruiken.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Voer de volgende opdrachten een circuit die zich in een mislukte status opnieuw instellen:

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

Het circuit moet nu in orde zijn. Open een ondersteuningsticket met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als het circuit nog steeds in een foutstatus is.

## <a name="next-steps"></a>Volgende stappen

Open een ondersteuningsticket met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u nog steeds problemen ondervindt.
