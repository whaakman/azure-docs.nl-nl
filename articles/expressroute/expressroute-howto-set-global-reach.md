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
ms.openlocfilehash: aff283da27197b11ee496faecdd8b69571d3547e
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56821333"
---
# <a name="configure-expressroute-global-reach"></a>ExpressRoute Global Reach configureren
Dit artikel helpt u bij het configureren van ExpressRoute globale bereiken met behulp van PowerShell. Zie voor meer informatie, [ExpressRouteRoute globaal bereik](expressroute-global-reach.md).

Voordat u de configuratie begint, controleert u het volgende:

* Dat kunt u de meest recente versie van Azure PowerShell hebt geïnstalleerd. Zie [Azure PowerShell installeren en configureren](/powershell/azure/azurerm/install-azurerm-ps) voor meer informatie.
* Dat u bekend bent met het inrichten van het ExpressRoute-circuit [werkstromen](expressroute-workflows.md).
* Dat uw ExpressRoute-circuits status ingericht zijn.
* Dat persoonlijke Azure-peering is geconfigureerd op uw ExpressRoute-circuits.  

### <a name="sign-in-to-your-azure-account"></a>Aanmelden bij uw Azure-account
Voor het starten van de configuratie, moet u zich aanmelden bij uw Azure-account. 

Open de PowerShell-console met verhoogde bevoegdheden en vervolgens verbinding met uw account. De opdracht vraagt u om de referentie-in voor uw Azure-account.  

```powershell
Connect-AzureRmAccount
```

Als u meerdere Azure-abonnementen hebt, controleert u de abonnementen voor het account.

```powershell
Get-AzureRmSubscription
```

Geef het abonnement op dat u wilt gebruiken.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Identificeren van uw ExpressRoute-circuits voor configuratie
U kunt ExpressRoute globaal bereik inschakelen tussen de twee ExpressRoute-circuits, zolang ze bevinden zich in de ondersteunde landen en op verschillende locaties voor peering zijn gemaakt. Als uw abonnement is eigenaar van beide circuits, kunt u een van beide circuit om uit te voeren van de configuratie in de volgende secties. 

Als de twee circuits zich in verschillende Azure-abonnementen, moet u toestemming van een Azure-abonnement. U geeft u in de autorisatiesleutel wanneer u de configuratieopdracht in andere Azure-abonnement uitvoert.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Connectiviteit tussen uw on-premises netwerken

Gebruik de volgende opdrachten om op te halen van circuit 1 en 2-circuit. De twee circuits zich in hetzelfde abonnement.

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
```

De volgende opdracht uitvoeren op basis van circuit 1 en doorgeven in de persoonlijke peering-ID van het circuit 2. Wanneer u de opdracht uitvoert, Let op het volgende:

* De persoonlijke peering-ID moet eruitzien zoals in het volgende voorbeeld: 

  ```
  /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering
  ```
* *-AddressPrefix* moet een/29 IPv4-subnet, bijvoorbeeld '10.0.0.0/29'. We gebruiken IP-adressen in dit subnet om te maken van verbinding tussen de twee ExpressRoute-circuits. U mag niet de adressen in dit subnet gebruiken in uw Azure virtual networks of in uw on-premises netwerk.

```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
```

Sla de configuratie op circuit 1 als volgt:
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Wanneer de vorige bewerking voltooid is, moet u de connectiviteit tussen uw on-premises netwerken aan beide zijden via uw twee ExpressRoute-circuits hebben.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>ExpressRoute-circuits in verschillende Azure-abonnementen

Als de twee circuits zich niet in hetzelfde Azure-abonnement, moet u autorisatie. In de volgende configuratie autorisatie wordt gegenereerd in het circuit 2-abonnement en de autorisatiesleutel wordt doorgegeven aan 1-circuit.

Genereer een autorisatiesleutel. 
```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```
Noteer de persoonlijke peering-ID van het circuit 2, evenals de autorisatiesleutel.

Voer de volgende opdracht op basis van circuit 1. In de persoonlijke peering-ID van het circuit doorgeven, 2 en de autorisatiesleutel.
```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
```

Sla de configuratie op circuit 1.
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Wanneer de vorige bewerking voltooid is, moet u de connectiviteit tussen uw on-premises netwerken aan beide zijden via uw twee ExpressRoute-circuits hebben.

## <a name="get-and-verify-the-configuration"></a>Krijgen te controleren of de configuratie

Gebruik de volgende opdracht om te controleren of de configuratie van het circuit waar de configuratie is gemaakt (bijvoorbeeld 1 circuit in het vorige voorbeeld).

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Als u gewoon *$ckt1* in PowerShell, ziet u *CircuitConnectionStatus* in de uitvoer. Het vertelt u of de verbinding is tot stand gebracht, 'Verbonden' of 'Verbinding verbroken'. 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Connectiviteit tussen uw on-premises netwerken uitschakelen

Voer de opdrachten om uit te schakelen connectiviteit, op basis van het circuit waar de configuratie is gemaakt (bijvoorbeeld 1 circuit in het vorige voorbeeld).

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

U kunt de Get-bewerking om te controleren of de status uitvoeren. 

Nadat de vorige bewerking voltooid is, hebt u niet langer connectiviteit tussen uw on-premises netwerk via uw ExpressRoute-circuits. 


## <a name="next-steps"></a>Volgende stappen
1. [Meer informatie over ExpressRoute globaal bereik](expressroute-global-reach.md)
2. [Controleer de ExpressRoute-verbinding](expressroute-troubleshooting-expressroute-overview.md)
3. [Een ExpressRoute-circuit koppelen met een Azure-netwerk](expressroute-howto-linkvnet-arm.md)


