---
title: 'Een virtueel netwerk koppelen aan een ExpressRoute-circuit: CLI: Azure | Microsoft Docs'
description: Dit document bevat een overzicht van hoe u virtuele netwerken (vnet's) koppelen aan ExpressRoute-circuits met behulp van het implementatiemodel van Resource Manager en de CLI.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlit
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: anzaman,cherylmc
ms.openlocfilehash: fa55cbad9fca799faff4e4cef87f9eedb8d2023f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-cli"></a>Een virtueel netwerk verbinden met een ExpressRoute-circuit met CLI

In dit artikel helpt u bij virtuele netwerken (vnet's) koppelen aan Azure ExpressRoute-circuits met CLI. Als u wilt koppelen met Azure CLI, moeten de virtuele netwerken worden gemaakt met het implementatiemodel van Resource Manager. Ze kunnen zijn in het hetzelfde abonnement of deel uitmaken van een ander abonnement. Als u gebruiken van een andere methode wilt voor uw VNet verbinding met een ExpressRoute-circuit, kunt u een artikel uit de volgende lijst:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure-CLI](howto-linkvnet-cli.md)
> * [Video - Azure-portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a>Configuratievereisten

* U moet de meest recente versie van de opdrachtregelinterface (CLI). Zie voor meer informatie [2.0 voor Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli).
* U moet controleren de [vereisten](expressroute-prerequisites.md), [routeringsvereisten](expressroute-routing.md), en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.
* U moet een actief ExpressRoute-circuit hebben. 
  * Volg de instructies voor [maken van een ExpressRoute-circuit](howto-circuit-cli.md) en laat het circuit inschakelen door de connectiviteitsprovider. 
  * Zorg ervoor dat u persoonlijke Azure-peering voor uw circuit is geconfigureerd. Zie de [routering configureren](howto-routing-cli.md) artikel voor routering instructies. 
  * Zorg ervoor dat de persoonlijke Azure-peering is geconfigureerd. De BGP-peer tussen uw netwerk en Microsoft moet, zodat u kunt end-to-end-connectiviteit inschakelen.
  * Zorg ervoor dat u hebt een virtueel netwerk en een virtuele netwerkgateway gemaakt en volledig is ingericht. Volg de instructies voor [configureren van een virtuele netwerkgateway voor ExpressRoute](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli). Zorg ervoor dat u `--gateway-type ExpressRoute`.

* U kunt maximaal 10 virtuele netwerken koppelen aan een standaard ExpressRoute-circuit. Alle virtuele netwerken moet in dezelfde geopolitieke regio bij gebruik van een standaard ExpressRoute-circuit. 

* Als u de invoegtoepassing ExpressRoute premium inschakelt, kunt u een virtueel netwerk buiten de geopolitieke regio van het ExpressRoute-circuit koppelen of verbinding maken met een groter aantal virtuele netwerken aan uw ExpressRoute-circuit. Zie voor meer informatie over de premium-invoegtoepassing, de [Veelgestelde vragen over](expressroute-faqs.md).

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Een virtueel netwerk in hetzelfde abonnement verbinden met een circuit

U kunt een virtuele netwerkgateway aan een ExpressRoute-circuit verbinding maken met behulp van het voorbeeld. Zorg ervoor dat de virtuele netwerkgateway wordt gemaakt en gereed is voor het koppelen voordat u de opdracht uitvoert.

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Een virtueel netwerk in een ander abonnement verbinden met een circuit

U kunt een ExpressRoute-circuit delen tussen meerdere abonnementen. De onderstaande afbeelding ziet u een eenvoudige schematische van hoe delen werkt voor ExpressRoute-circuits voor meerdere abonnementen.

Elk van de kleinere clouds binnen de grote cloud wordt gebruikt voor abonnementen die bij verschillende afdelingen binnen een organisatie horen te vertegenwoordigen. Elk van de afdelingen binnen de organisatie kan hun eigen abonnement gebruiken voor het implementeren van hun services--, maar één ExpressRoute-circuit terugverbinding maken met uw on-premises netwerk kunnen delen. Één afdeling (in dit voorbeeld: IT) kunt eigenaar van het ExpressRoute-circuit. Andere abonnementen binnen de organisatie kunnen het ExpressRoute-circuit gebruiken.

> [!NOTE]
> Verbindingen en bandbreedte kosten voor het specifieke circuit wordt toegepast op de eigenaar van het ExpressRoute-Circuit. Alle virtuele netwerken delen de dezelfde bandbreedte.
> 
> 

![Abonnementoverschrijdende connectiviteit](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a>Beheer - Circuit eigenaars en Circuit gebruikers

De eigenaar van het Circuit is een geautoriseerde gebruiker Power van de bron van ExpressRoute-circuit. De eigenaar van het Circuit kunt autorisaties die kunnen worden ingewisseld door 'Circuit gebruikers' maken. Circuit gebruikers kunnen eigenaren van virtuele netwerkgateways die zich niet binnen hetzelfde abonnement als het ExpressRoute-circuit. Circuit gebruikers kunnen inwisselen autorisaties (één autorisatie per virtueel netwerk).

De eigenaar van het Circuit bevoegd is om te wijzigen en machtigingen intrekt op elk gewenst moment. Wanneer een vergunning wordt ingetrokken, worden alle koppeling verbindingen verwijderd uit het abonnement waarvoor de toegang is ingetrokken.

### <a name="circuit-owner-operations"></a>Circuit eigenaar bewerkingen

**Maken van een autorisatieregels**

De eigenaar van het Circuit maakt een autorisatie, die een autorisatiesleutel die kan worden gebruikt door een gebruiker Circuit hun virtuele netwerkgateways aan ExpressRoute-circuit verbinding maakt. Een vergunning is geldig voor slechts één verbinding.

Het volgende voorbeeld ziet u het maken van een vergunning:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

Het antwoord bevat de autorisatiesleutel en status:

```azurecli
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

**Machtigingen controleren**

De eigenaar van het Circuit Neem alle machtigingen die zijn uitgegeven voor een bepaalde circuit door te voeren in het volgende voorbeeld:

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

**Autorisaties toevoegen**

De eigenaar van het Circuit kunt autorisaties toevoegen met behulp van het volgende voorbeeld:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

**Autorisaties verwijderen**

De eigenaar van het Circuit kunt intrekken/delete autorisaties voor de gebruiker door te voeren in het volgende voorbeeld:

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a>Bewerkingen voor circuit-gebruikers

De Circuit-gebruiker moet de peer-ID en een autorisatiesleutel van de eigenaar van het Circuit. De autorisatiesleutel is een GUID.

```azurecli
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**Voor een verbindingsverificatie inwisselen**

De Circuit-gebruiker kan het volgende voorbeeld als u wilt gebruikmaken van een koppeling autorisatie uitvoeren:

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**Een verbindingsverificatie vrijgeven**

U kunt een vergunning vrijgeven door het verwijderen van de verbinding die is gekoppeld aan het virtuele netwerk in het ExpressRoute-circuit.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).