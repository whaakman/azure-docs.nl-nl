---
title: 'Een virtueel netwerk koppelen aan een ExpressRoute-circuit: PowerShell: Azure | Microsoft Docs'
description: Dit document bevat een overzicht van hoe u virtuele netwerken (vnet's) koppelen aan ExpressRoute-circuits met behulp van de Resource Manager-implementatiemodel en PowerShell.
services: expressroute
documentationcenter: na
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: daacb6e5-705a-456f-9a03-c4fc3f8c1f7e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: ganesr
ms.openlocfilehash: 8c2f3036f754a98090ab860f95900416690ebf83
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a>Een virtueel netwerk verbinden met een ExpressRoute-circuit
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure-CLI](howto-linkvnet-cli.md)
> * [Video - Azure-portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-linkvnet-classic.md)
>

In dit artikel helpt u bij virtuele netwerken (vnet's) koppelen aan Azure ExpressRoute-circuits met behulp van de Resource Manager-implementatiemodel en PowerShell. Virtuele netwerken kunnen zijn in het hetzelfde abonnement of een deel van een ander abonnement. Dit artikel ziet u ook het bijwerken van de koppeling van een virtueel netwerk. 

## <a name="before-you-begin"></a>Voordat u begint
* Installeer de nieuwste versie van de Azure PowerShell-modules. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.
* Controleer de [vereisten](expressroute-prerequisites.md), [routeringsvereisten](expressroute-routing.md), en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.
* U moet een actief ExpressRoute-circuit hebben. 
  * Volg de instructies voor [maken van een ExpressRoute-circuit](expressroute-howto-circuit-arm.md) en laat het circuit inschakelen door de connectiviteitsprovider. 
  * Zorg ervoor dat u persoonlijke Azure-peering voor uw circuit is geconfigureerd. Zie de [routering configureren](expressroute-howto-routing-arm.md) artikel voor routering instructies. 
  * Zorg ervoor dat de persoonlijke Azure-peering is geconfigureerd en van de BGP-peering tussen uw netwerk en Microsoft is, zodat u kunt end-to-end-connectiviteit inschakelen.
  * Zorg ervoor dat u hebt een virtueel netwerk en een virtuele netwerkgateway gemaakt en volledig is ingericht. Volg de instructies voor [maken van een virtuele netwerkgateway voor ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). Het GatewayType 'ExpressRoute' niet VPN maakt gebruik van een virtuele netwerkgateway voor ExpressRoute.

* U kunt maximaal 10 virtuele netwerken koppelen aan een standaard ExpressRoute-circuit. Alle virtuele netwerken moet in dezelfde geopolitieke regio bij gebruik van een standaard ExpressRoute-circuit. 

* U kunt een virtuele netwerken buiten de geopolitieke regio van het ExpressRoute-circuit koppelen of verbinding maken met een groter aantal virtuele netwerken aan uw ExpressRoute-circuit als u de invoegtoepassing ExpressRoute premium ingeschakeld. Controleer de [Veelgestelde vragen over](expressroute-faqs.md) voor meer informatie over de premium-invoegtoepassing.


## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Een virtueel netwerk in hetzelfde abonnement verbinden met een circuit
U kunt een virtuele netwerkgateway aan een ExpressRoute-circuit verbinden met de volgende cmdlet. Zorg ervoor dat de virtuele netwerkgateway wordt gemaakt en gereed is voor het koppelen voordat u de cmdlet uitvoeren:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Een virtueel netwerk in een ander abonnement verbinden met een circuit
U kunt een ExpressRoute-circuit delen tussen meerdere abonnementen. De volgende afbeelding ziet een eenvoudige schematische van hoe delen werkt voor ExpressRoute-circuits voor meerdere abonnementen.

Elk van de kleinere clouds binnen de grote cloud wordt gebruikt voor abonnementen die bij verschillende afdelingen binnen een organisatie horen te vertegenwoordigen. Elk van de afdelingen binnen de organisatie kan hun eigen abonnement gebruiken voor het implementeren van hun services--, maar één ExpressRoute-circuit terugverbinding maken met uw on-premises netwerk kunnen delen. Één afdeling (in dit voorbeeld: IT) kunt eigenaar van het ExpressRoute-circuit. Andere abonnementen binnen de organisatie kunnen het ExpressRoute-circuit gebruiken.

> [!NOTE]
> Verbindingen en bandbreedte kosten voor het ExpressRoute-circuit wordt toegepast op de eigenaar van het abonnement. Alle virtuele netwerken delen de dezelfde bandbreedte.
> 
> 

![Abonnementoverschrijdende connectiviteit](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a>Beheer - circuit eigenaars en circuit gebruikers

De circuiteigenaar is een geautoriseerde gebruiker Power van de bron van ExpressRoute-circuit. De circuiteigenaar van het kunt autorisaties die kunnen worden ingewisseld door 'circuit gebruikers' maken. Circuit gebruikers kunnen eigenaren van virtuele netwerkgateways die zich niet binnen hetzelfde abonnement als het ExpressRoute-circuit. Circuit gebruikers kunnen inwisselen autorisaties (één autorisatie per virtueel netwerk).

De circuiteigenaar van het bevoegd is om te wijzigen en machtigingen intrekt op elk gewenst moment. Intrekken van een vergunning resulteert in alle koppeling verbindingen wordt verwijderd uit het abonnement waarvoor de toegang is ingetrokken.

### <a name="circuit-owner-operations"></a>Circuit eigenaar bewerkingen

**Maken van een autorisatieregels**

De circuiteigenaar van het maakt een autorisatie. Dit resulteert in het maken van een autorisatiesleutel die kan worden gebruikt door een gebruiker circuit verbinding maken hun virtuele netwerkgateways aan ExpressRoute-circuit. Een vergunning is geldig voor slechts één verbinding.

Het volgende fragment van de cmdlet toont het maken van een vergunning:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


Het antwoord op dit bevat de autorisatiesleutel en status:

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



**Machtigingen controleren**

De circuiteigenaar van het Neem alle machtigingen die zijn uitgegeven voor een bepaalde circuit door de volgende cmdlet:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**Autorisaties toevoegen**

De circuiteigenaar van het kunt autorisaties toevoegen met behulp van de volgende cmdlet:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**Autorisaties verwijderen**

De circuiteigenaar van het kan intrekken/delete autorisaties voor de gebruiker door de volgende cmdlet:

```powershell
Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a>Bewerkingen voor circuit-gebruikers

De circuit-gebruiker moet de peer-ID en een autorisatiesleutel van de circuiteigenaar van het. De autorisatiesleutel is een GUID.

Peer-ID kan worden gecontroleerd van de volgende opdracht:

```powershell
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**Voor een verbindingsverificatie inwisselen**

De circuit-gebruiker kan de volgende cmdlet als u wilt gebruikmaken van een koppeling autorisatie uitvoeren:

```powershell
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**Een verbindingsverificatie vrijgeven**

U kunt een vergunning vrijgeven door het verwijderen van de verbinding die is gekoppeld aan het virtuele netwerk in het ExpressRoute-circuit.

## <a name="modify-a-virtual-network-connection"></a>De verbinding van een virtueel netwerk wijzigen
U kunt bepaalde eigenschappen van een virtueel netwerkverbinding bijwerken. 

**Het gewicht verbinding bijwerken**

Het virtuele netwerk kan worden verbonden met meerdere ExpressRoute-circuits. U ontvangt deze mogelijk hetzelfde voorvoegsel van meer dan één ExpressRoute-circuit. Als u wilt kiezen welke verbinding met het verzenden van verkeer dat is bestemd voor dit voorvoegsel, kunt u *RoutingWeight* van een verbinding. Verkeer wordt verzonden via de verbinding met de hoogste *RoutingWeight*.

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

Het bereik van *RoutingWeight* is 0-32000. De standaardwaarde is 0.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
