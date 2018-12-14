---
title: 'Configureer ExpressRoute wereldwijd bereik: Azure CLI | Microsoft Docs'
description: Dit artikel helpt u koppelen van ExpressRoute-circuits samen om te maken van een particulier netwerk tussen uw on-premises netwerken en globale bereiken inschakelen.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/12/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 9a8e0a5df9383d8e3d7159aa916b0e4fbfeea948
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384043"
---
# <a name="configure-expressroute-global-reach-using-azure-cli-preview"></a>Configureren van ExpressRoute wereldwijd bereik met behulp van Azure CLI (Preview)
Dit artikel helpt u bij het configureren van ExpressRoute globale bereiken met behulp van Azure CLI. Zie voor meer informatie, [ExpressRouteRoute globaal bereik](expressroute-global-reach.md).
 
## <a name="before-you-begin"></a>Voordat u begint
> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> 


Voordat u de configuratie begint, moet u de volgende vereisten controleren.

* Installeer de nieuwste versie van Azure CLI. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) en [aan de slag met Azure CLI](/cli/azure/get-started-with-azure-cli).
* Informatie over de ExpressRoute-circuit provisioning [werkstromen](expressroute-workflows.md).
* Zorg ervoor dat uw ExpressRoute-circuits zijn status ingericht.
* Zorg ervoor dat persoonlijke Azure-peering is geconfigureerd op uw ExpressRoute-circuits.  

### <a name="log-into-your-azure-account"></a>Meld u aan bij uw Azure-account
Voor het starten van de configuratie, moet u zich aanmelden bij uw Azure-account. De opdracht wordt de standaardbrowser geopend en u vragen om de referentie van de aanmelding voor uw Azure-account.  

```azurecli
az login
```

Als u meerdere Azure-abonnementen hebt, controleert u de abonnementen voor het account.

```azurecli
az account list
```

Geef het abonnement op dat u wilt gebruiken.

```azurecli
az account set --subscription <your subscription ID>
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Identificeren van uw ExpressRoute-circuits voor configuratie
U kunt ExpressRoute globaal bereik inschakelen tussen de twee ExpressRoute-circuits, zolang ze bevinden zich in de ondersteunde landen en ze worden gemaakt op verschillende locaties voor peering. Als uw abonnement is eigenaar van beide circuits kunt u een van beide circuit om uit te voeren van de configuratie in de onderstaande secties. Als de twee circuits zich in verschillende Azure-abonnementen, wordt u autorisatie van één Azure-abonnement nodig hebt en wordt doorgegeven in de autorisatiesleutel wanneer u de configuratieopdracht in andere Azure-abonnement uitvoert.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Connectiviteit tussen uw on-premises netwerken

Bij het uitvoeren van de opdracht uit om connectiviteit, houd rekening met de volgende waarden:

* *peer-circuit* moet de volledige resource-ID. Bijvoorbeeld: 

  ```
  /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}
  ```
* *-AddressPrefix* moet een/29 IPv4-subnet, bijvoorbeeld '10.0.0.0/29'. We gebruiken IP-adressen in dit subnet om te maken van verbinding tussen de twee ExpressRoute-circuits. U moet geen adressen gebruiken in dit subnet in uw Azure vnet's of in uw on-premises netwerken.

Voer de volgende CLI om twee ExpressRoute-circuits verbinding te maken. Gebruik de volgende opdracht:

```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29>
```

De CLI-uitvoer ziet er als volgt uit:

```azurecli
{
  "addressPrefix": "<__.__.__.__/29>",
  "authorizationKey": null,
  "circuitConnectionStatus": "Connected",
  "etag": "W/\"48d682f9-c232-4151-a09f-fab7cb56369a\"",
  "expressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<ResourceGroupName>"
  },
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering/connections/<ConnectionName>",
  "name": "<ConnectionName>",
  "peerExpressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<Circuit2ResourceGroupName>"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "<ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/peerings/connections"
}
```

Wanneer de bovenstaande bewerking voltooid is, moet u de connectiviteit tussen uw on-premises netwerken aan beide zijden via uw twee ExpressRoute-circuits hebben.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>ExpressRoute-circuits in verschillende Azure-abonnementen

Als de twee circuits zich niet in hetzelfde Azure-abonnement, moet u autorisatie. In de volgende configuratie autorisatie wordt gegenereerd in het circuit van 2-abonnement en de autorisatiesleutel wordt doorgegeven aan 1-circuit.

Genereer een autorisatiesleutel. 
```azurecli
az network express-route auth create --circuit-name <Circuit2Name> -g <Circuit2ResourceGroupName> -n <AuthorizationName>
```

De CLI-uitvoer ziet er als volgt uit.

```azurecli
{
  "authorizationKey": "<authorizationKey>",
  "authorizationUseStatus": "Available",
  "etag": "W/\"cfd15a2f-43a1-4361-9403-6a0be00746ed\"",
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/authorizations/<AuthorizationName>",
  "name": "<AuthorizationName>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<Circuit2ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/authorizations"
}
```

Maak een notitie van circuit 2 van resource-Id, evenals de autorisatiesleutel.

Voer de volgende opdracht op basis van circuit 1. Circuit 2 van resource-Id en de autorisatiesleutel worden doorgegeven 
```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29> --authorization-key <authorizationKey>
```

Wanneer de bovenstaande bewerking voltooid is, moet u de connectiviteit tussen uw on-premises netwerken aan beide zijden via uw twee ExpressRoute-circuits hebben.

## <a name="get-and-verify-the-configuration"></a>Krijgen te controleren of de configuratie

Gebruik de volgende opdracht om te controleren of de configuratie van het circuit waar de configuratie is gemaakt, dat wil zeggen circuit 1 in het bovenstaande voorbeeld.

```azurecli
az network express-route show -n <CircuitName> -g <ResourceGroupName>
```

In de CLI uitvoer zien *CircuitConnectionStatus*. Het vertelt u of de verbinding tussen de twee circuits tot stand is gebracht, 'Verbonden', of niet, 'Verbinding verbroken'. 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Connectiviteit tussen uw on-premises netwerken uitschakelen

Voer de opdrachten uitschakelen op basis van het circuit waar de configuratie is gemaakt, dat wil zeggen circuit 1 in het bovenstaande voorbeeld.

```azurecli
az network express-route peering connection delete -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName>
```

U kunt de CLI weergeven om te controleren of de status uitvoeren. 

Nadat de bovenstaande bewerking voltooid is, hebt u niet langer connectiviteit tussen uw on-premises netwerk via uw ExpressRoute-circuits. 


## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over ExpressRoute globaal bereik](expressroute-global-reach.md)
* [Controleer de ExpressRoute-verbinding](expressroute-troubleshooting-expressroute-overview.md)
* [ExpressRoute-circuit koppelen met Azure-netwerk](expressroute-howto-linkvnet-arm.md)


