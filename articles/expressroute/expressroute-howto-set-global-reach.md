---
title: Configureren van het wereldwijde bereik met Azure ExpressRoute | Microsoft Docs
description: Dit artikel helpt u koppelen van ExpressRoute-circuits samen om te maken van een particulier netwerk tussen uw on-premises netwerken en globale bereiken inschakelen.
documentationcenter: na
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: mialdrid
ms.openlocfilehash: 67fbf9dc430d615efe3ef894add1a26bbce792bc
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50237975"
---
# <a name="configure-expressroute-global-reach-preview"></a>Configureren van ExpressRoute wereldwijd bereik (Preview)
Dit artikel helpt u bij het configureren van ExpressRoute globale bereiken met behulp van PowerShell. Zie voor meer informatie, [ExpressRouteRoute globaal bereik](expressroute-global-reach.md).
 
## <a name="before-you-begin"></a>Voordat u begint
> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> 


Voordat u de configuratie begint, moet u de volgende vereisten controleren.

* Installeer de nieuwste versie van Azure PowerShell. Zie [installeren en configureren van Azure PowerShell](/powershell/azure/install-azurerm-ps).
* Informatie over de ExpressRoute-circuit provisioning [werkstromen](expressroute-workflows.md).
* Zorg ervoor dat uw ExpressRoute-circuits zijn status ingericht.
* Zorg ervoor dat persoonlijke Azure-peering is geconfigureerd op uw ExpressRoute-circuits.  

### <a name="log-into-your-azure-account"></a>Meld u aan bij uw Azure-account
Voor het starten van de configuratie, moet u zich aanmelden bij uw Azure-account. 

Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account. De opdracht wordt u gevraagd voor de referentie van de aanmelding voor uw Azure-account.  

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
U kunt ExpressRoute globaal bereik inschakelen tussen de twee ExpressRoute-circuits, zolang ze bevinden zich in de ondersteunde landen en ze worden gemaakt op verschillende locaties voor peering. Als uw abonnement is eigenaar van beide circuits kunt u een van beide circuit om uit te voeren van de configuratie in de onderstaande secties. Als de twee circuits zich in verschillende Azure-abonnementen, wordt u autorisatie van één Azure-abonnement nodig hebt en wordt doorgegeven in de autorisatiesleutel wanneer u de configuratieopdracht in andere Azure-abonnement uitvoert.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Connectiviteit tussen uw on-premises netwerken

Gebruik de volgende opdrachten om op te halen van circuit 1 en 2-circuit. De twee circuits zich in hetzelfde abonnement.

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
```

De volgende opdracht uitvoeren op circuit 1 en 2 van persoonlijke circuitpeering van id worden doorgegeven

> [!NOTE]
> De persoonlijke peering het lijkt erop dat Id */subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering*
> 
>

```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
```

> [!IMPORTANT]
> *-AddressPrefix* moet een/29 IPv4-subnet, bijvoorbeeld '10.0.0.0/29'. We gebruiken IP-adressen in dit subnet om te maken van verbinding tussen de twee ExpressRoute-circuits. U moet geen adressen gebruiken in dit subnet in uw Azure vnet's of in uw on-premises netwerken.
> 



De configuratie op te slaan op circuit 1
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Wanneer de bovenstaande bewerking voltooid is, moet u de connectiviteit tussen uw on-premises netwerken aan beide zijden via uw twee ExpressRoute-circuits hebben.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>ExpressRoute-circuits in verschillende Azure-abonnementen

Als de twee circuits zich niet in hetzelfde Azure-abonnement, moet u autorisatie. In de volgende configuratie autorisatie wordt gegenereerd in het circuit van 2-abonnement en de autorisatiesleutel wordt doorgegeven aan 1-circuit.

Genereer een autorisatiesleutel. 
```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```
Maak een notitie van circuit van 2 privépeering van Id, evenals de autorisatiesleutel.

Voer de volgende opdracht op basis van circuit 1. Doorgeven van 2 persoonlijke circuitpeering van-Id en de autorisatiesleutel 
```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
```

De configuratie op te slaan op circuit 1
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Wanneer de bovenstaande bewerking voltooid is, moet u de connectiviteit tussen uw on-premises netwerken aan beide zijden via uw twee ExpressRoute-circuits hebben.

## <a name="get-and-verify-the-configuration"></a>Krijgen te controleren of de configuratie

Gebruik de volgende opdracht om te controleren of de configuratie van het circuit waar de configuratie is gemaakt, dat wil zeggen circuit 1 in het bovenstaande voorbeeld.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Als u gewoon *$ckt1* in PowerShell u ziet *CircuitConnectionStatus* in de uitvoer. Het vertelt u of de verbinding tot stand is gebracht, 'Verbonden', of niet, 'Verbinding verbroken'. 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Connectiviteit tussen uw on-premises netwerken uitschakelen

Voer de opdrachten uitschakelen op basis van het circuit waar de configuratie is gemaakt, dat wil zeggen circuit 1 in het bovenstaande voorbeeld.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

U kunt de Get-bewerking om te controleren of de status uitvoeren. 

Nadat de bovenstaande bewerking voltooid is, hebt u niet langer connectiviteit tussen uw on-premises netwerk via uw ExpressRoute-circuits. 


## <a name="next-steps"></a>Volgende stappen
1. [Meer informatie over ExpressRoute globaal bereik](expressroute-global-reach.md)
2. [Controleer de ExpressRoute-verbinding](expressroute-troubleshooting-expressroute-overview.md)
3. [ExpressRoute-circuit koppelen met Azure-netwerk](expressroute-howto-linkvnet-arm.md)


