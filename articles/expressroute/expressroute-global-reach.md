---
title: Wereldwijde bereik van Azure ExpressRoute | Microsoft Docs
description: Dit artikel helpt u koppelen van ExpressRoute-circuits samen om te maken van een particulier netwerk tussen uw on-premises netwerken en globale bereiken inschakelen.
documentationcenter: na
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 90f34da10e0193e31a0f70187463799013f52be8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966812"
---
# <a name="link-expressroute-circuits-to-enable-expressroute-global-reach-preview"></a>ExpressRoute-circuits koppeling om in te schakelen ExpressRoute globaal bereik (Preview)

ExpressRoute is een privé- en flexibele manier om uw on-premises netwerken verbinden met Microsoft Cloud. U kunt toegang tot veel Microsoft-cloudservices zoals Azure, Office 365 en Dynamics 365 vanuit uw persoonlijke Datacenter of uw bedrijfsnetwerk. Bijvoorbeeld, wellicht u een filiaal in San Francisco met een ExpressRoute-circuit in Silicon Valley en een ander filiaal in Baltimore met een ExpressRoute-circuit in Washington DC. 

Beide filialen kunnen zeer snelle verbindingen met Azure-resources in VS-Oost en VS-West hebben. Echter kunnen geen de filialen uitwisselen van gegevens met elkaar. Met andere woorden, kan 10.0.1.0/24 uitwisselen van gegevens met 10.0.3.0/24 en 10.0.4.0/24, maar niet met 10.0.2.0/24.

![zonder][1]

Met **ExpressRoute globaal bereik**, kunt u ExpressRoute-circuits samen om te maken van een netwerk met persoonlijke gegevens tussen uw on-premises netwerken koppelen. In het bovenstaande voorbeeld wordt met de toevoeging van ExpressRoute globaal bereik, kan uw kantoor in San Francisco (10.0.1.0/24) rechtstreeks uitwisselen van gegevens met uw Baltimore office (10.0.2.0/24) via de bestaande ExpressRoute-circuits en via het wereldwijde netwerk van Microsoft. 

![met][2]

ExpressRoute globaal bereik wordt op dat moment ondersteund in de volgende landen:

* Verenigde Staten
* Verenigd Koninkrijk 
* Japan

Uw ExpressRoute-circuits moeten worden gemaakt op de [ExpressRoute-peeringlocaties](expressroute-locations.md) in de bovenstaande landen. Om in te schakelen globaal bereik van ExpressRoute tussen [verschillende geopolitieke regio's](expressroute-locations.md), uw circuits moet Premium-SKU.


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
U kunt ExpressRoute globaal bereik inschakelen tussen de twee ExpressRoute-circuits, zolang ze bevinden zich in de ondersteunde landen en ze worden gemaakt op verschillende locaties voor peering. Als uw abonnement is eigenaar van beide circuits, kunt u een van beide circuit om uit te voeren van de configuratie in de onderstaande secties. Als de twee circuits zich in verschillende Azure-abonnementen, wordt u autorisatie van één Azure-abonnement nodig hebt en wordt doorgegeven in de autorisatiesleutel wanneer u de configuratieopdracht in andere Azure-abonnement uitvoert.

## <a name="to-enable-connectivity-between-your-on-premises-networks"></a>Om verbindingen tussen uw on-premises netwerken

Gebruik de volgende opdrachten om op te halen van circuit 1 en 2-circuit. De twee circuits zich in hetzelfde abonnement.

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
```

De volgende opdracht uitvoeren op circuit 1 en 2 van persoonlijke circuitpeering van id worden doorgegeven

Houd er rekening mee dat de persoonlijke peering van Id moet als eruitzien:

`/subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering`


```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
```

De *- AddressPrefix* moet een/29 IPv4-subnet, bijvoorbeeld '10.0.0.0/29'. We gebruiken IP-adressen in dit subnet om te maken van verbinding tussen de twee ExpressRoute-circuits. U moet geen adressen gebruiken in dit subnet in uw Azure vnet's of in uw on-premises netwerken. 


Sla de configuratie op circuit 1.

```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Wanneer de vorige bewerking voltooid is, moet u de connectiviteit tussen uw on-premises netwerken aan beide zijden via uw twee ExpressRoute-circuits hebben.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>ExpressRoute-circuits in verschillende Azure-abonnementen

Als de twee circuits zich niet in hetzelfde Azure-abonnement, moet u autorisatie. In de volgende configuratie autorisatie wordt gegenereerd in het circuit van 2-abonnement en de autorisatiesleutel wordt doorgegeven aan 1-circuit.

Genereer een autorisatiesleutel. 
```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```
Maak een notitie van circuit van 2 privépeering van ID, evenals de autorisatiesleutel.

Voer de volgende opdracht op basis van circuit 1. Doorgeven van 2 persoonlijke circuitpeering van-ID en de autorisatiesleutel

```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
```

Sla de configuratie op circuit 1.

```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Wanneer de vorige bewerkingen is voltooid, moet u de connectiviteit tussen uw on-premises netwerken aan beide zijden via uw twee ExpressRoute-circuits hebben.

## <a name="to-get-and-verify-the-configuration"></a>Om te halen en de configuratie controleren

Gebruik de volgende opdracht om te controleren of de configuratie van het circuit waar de configuratie is gemaakt. In dit voorbeeld wordt een circuit 1 uit het vorige voorbeeld.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Als u gewoon *$ckt1* in PowerShell, ziet u *CircuitConnectionStatus* in de uitvoer. Het vertelt u of de verbinding tot stand is gebracht, 'Verbonden', of niet, 'Verbinding verbroken'. 

## <a name="to-disable-connectivity-between-your-on-premises-networks"></a>Connectiviteit tussen uw on-premises netwerken uitschakelen

Voer de opdrachten om uit te schakelen, op basis van het circuit waar de configuratie is gemaakt. In dit voorbeeld wordt een circuit 1 van de eerdere voorbeelden.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

U kunt de Get-bewerking om te controleren of de status uitvoeren. 

Nadat de bovenstaande bewerking voltooid is, hebt u niet langer connectiviteit tussen uw on-premises netwerk via uw ExpressRoute-circuits. 

## <a name="next-steps"></a>Volgende stappen
1. [Meer informatie over ExpressRoute globaal bereik](expressroute-faqs.md#globalreach)
2. [Controleer de ExpressRoute-verbinding](expressroute-troubleshooting-expressroute-overview.md)
3. [ExpressRoute-circuit koppelen met Azure-netwerk](expressroute-howto-linkvnet-arm.md)


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "diagram zonder wereldwijd bereik"
[2]: ./media/expressroute-global-reach/2.png "diagram met globaal bereik"