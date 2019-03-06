---
title: 'Wereldwijd bereik - ExpressRoute configureren: Azure | Microsoft Docs'
description: Dit artikel helpt u koppelen van ExpressRoute-circuits samen om te maken van een particulier netwerk tussen uw on-premises netwerken en globale bereiken inschakelen.
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: jaredro
ms.custom: seodec18
ms.openlocfilehash: 171bf94bbccd45b9be995977c9ec2a26a75d9602
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57403478"
---
# <a name="configure-expressroute-global-reach"></a>ExpressRoute Global Reach configureren

Dit artikel helpt u bij het configureren van ExpressRoute globale bereiken met behulp van PowerShell. Zie voor meer informatie, [ExpressRouteRoute globaal bereik](expressroute-global-reach.md).

 ## <a name="before-you-begin"></a>Voordat u begint

Voordat u de configuratie begint, controleert u het volgende:

* U begrijpt het inrichten van het ExpressRoute-circuit [werkstromen](expressroute-workflows.md).
* Uw ExpressRoute-circuits zijn status ingericht.
* Persoonlijke Azure-peering is geconfigureerd op uw ExpressRoute-circuits.
* Als u PowerShell lokaal uitvoeren wilt, moet u controleren of de nieuwste versie van Azure PowerShell is geïnstalleerd op uw computer.

### <a name="working-with-azure-powershell"></a>Werken met Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="identify-circuits"></a>Circuits identificeren

1. Meld u aan bij uw Azure-account en selecteer het abonnement dat u wilt gebruiken voor het starten van de configuratie.

  [!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)]
2. Identificeer de ExpressRoute-circuits die u gebruiken wilt. U kunt ExpressRoute globaal bereik inschakelen tussen de twee ExpressRoute-circuits, zolang ze bevinden zich in de ondersteunde landen en op verschillende locaties voor peering zijn gemaakt. 

  * Als uw abonnement is eigenaar van beide circuits, kunt u een van beide circuit om uit te voeren van de configuratie in de volgende secties.
  * Als de twee circuits zich in verschillende Azure-abonnementen, moet u toestemming van een Azure-abonnement. U geeft u in de autorisatiesleutel wanneer u de configuratieopdracht in andere Azure-abonnement uitvoert.

## <a name="enable-connectivity"></a>Connectiviteit

Schakel de connectiviteit tussen uw on-premises netwerken. Er zijn verschillende sets met instructies voor het circuits die zich in hetzelfde Azure-abonnement en -circuits die zijn verschillende abonnementen.

### <a name="expressroute-circuits-in-the-same-azure-subscription"></a>ExpressRoute-circuits in hetzelfde Azure-abonnement

1. Gebruik de volgende opdrachten om op te halen van circuit 1 en 2-circuit. De twee circuits zich in hetzelfde abonnement.

  ```azurepowershell-interactive
  $ckt_1 = Get-AzExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
  $ckt_2 = Get-AzExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
  ```
2. De volgende opdracht uitvoeren op basis van circuit 1 en doorgeven in de persoonlijke peering-ID van het circuit 2. Wanneer u de opdracht uitvoert, Let op het volgende:

  * De persoonlijke peering-ID moet eruitzien zoals in het volgende voorbeeld: 

    ```
    /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering
    ```
  * *-AddressPrefix* moet een/29 IPv4-subnet, bijvoorbeeld '10.0.0.0/29'. We gebruiken IP-adressen in dit subnet om te maken van verbinding tussen de twee ExpressRoute-circuits. U mag niet de adressen in dit subnet gebruiken in uw Azure virtual networks of in uw on-premises netwerk.

    ```azurepowershell-interactive
    Add-AzExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
    ```
3. Sla de configuratie op circuit 1 als volgt:

  ```azurepowershell-interactive
  Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_1
  ```

Wanneer de vorige bewerking is voltooid, hebt u connectiviteit tussen uw on-premises netwerken aan beide zijden via uw twee ExpressRoute-circuits.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>ExpressRoute-circuits in verschillende Azure-abonnementen

Als de twee circuits zich niet in hetzelfde Azure-abonnement, moet u autorisatie. In de volgende configuratie autorisatie wordt gegenereerd in het circuit 2-abonnement en de autorisatiesleutel wordt doorgegeven aan 1-circuit.

1. Genereer een autorisatiesleutel.

  ```azurepowershell-interactive
  $ckt_2 = Get-AzExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
  Add-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
  Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_2
  ```

  Noteer de persoonlijke peering-ID van het circuit 2, evenals de autorisatiesleutel.
2. Voer de volgende opdracht op basis van circuit 1. In de persoonlijke peering-ID van het circuit doorgeven, 2 en de autorisatiesleutel.

  ```azurepowershell-interactive
  Add-AzExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
  ```
3. Sla de configuratie op circuit 1.

  ```azurepowershell-interactive
  Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_1
  ```

Wanneer de vorige bewerking is voltooid, hebt u connectiviteit tussen uw on-premises netwerken aan beide zijden via uw twee ExpressRoute-circuits.

## <a name="verify-the-configuration"></a>De configuratie controleren

Gebruik de volgende opdracht om te controleren of de configuratie van het circuit waar de configuratie is gemaakt (bijvoorbeeld 1 circuit in het vorige voorbeeld).
```azurepowershell-interactive
$ckt1 = Get-AzExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Als u gewoon *$ckt1* in PowerShell, ziet u *CircuitConnectionStatus* in de uitvoer. Het vertelt u of de verbinding is tot stand gebracht, 'Verbonden' of 'Verbinding verbroken'. 

## <a name="disable-connectivity"></a>Connectiviteit uitschakelen

Connectiviteit tussen uw on-premises netwerken, voert u de opdrachten op basis van het circuit waar de configuratie is gemaakt (bijvoorbeeld circuit 1 in het vorige voorbeeld) uitschakelen.

```azurepowershell-interactive
$ckt1 = Get-AzExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

U kunt de Get-bewerking om te controleren of de status uitvoeren.

Nadat de vorige bewerking voltooid is, hebt u niet langer connectiviteit tussen uw on-premises netwerk via uw ExpressRoute-circuits.

## <a name="next-steps"></a>Volgende stappen
1. [Meer informatie over ExpressRoute globaal bereik](expressroute-global-reach.md)
2. [Controleer de ExpressRoute-verbinding](expressroute-troubleshooting-expressroute-overview.md)
3. [Een ExpressRoute-circuit koppelen met een Azure-netwerk](expressroute-howto-linkvnet-arm.md)
