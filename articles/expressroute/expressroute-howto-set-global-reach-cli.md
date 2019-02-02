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
ms.openlocfilehash: be10489d731b9e01d148ce1ac7892cb6de956662
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55659317"
---
# <a name="configure-expressroute-global-reach-by-using-the-azure-cli-preview"></a>Globaal bereik ExpressRoute configureren met behulp van de Azure CLI (Preview)

Dit artikel helpt u bij het configureren van Azure ExpressRoute globale bereiken met behulp van de Azure CLI. Zie [ExpressRoute Global Reach](expressroute-global-reach.md) voor meer informatie.
 
## <a name="before-you-begin"></a>Voordat u begint

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Voordat u de configuratie begint, voert u de volgende vereisten:

* Installeer de nieuwste versie van de Azure CLI. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) en [Aan de slag met de Azure CLI](/cli/azure/get-started-with-azure-cli).
* Informatie over de ExpressRoute-circuit inrichting [werkstromen](expressroute-workflows.md).
* Zorg ervoor dat uw ExpressRoute-circuits zijn in de status ingericht.
* Zorg ervoor dat persoonlijke Azure-peering is geconfigureerd op uw ExpressRoute-circuits.  

### <a name="sign-in-to-your-azure-account"></a>Aanmelden bij uw Azure-account

Als u wilt gaan configuratie, moet u zich aanmelden bij uw Azure-account. De volgende opdracht wordt de standaardbrowser geopend en vraagt u om de aanmeldingsreferenties voor uw Azure-account:  

```azurecli
az login
```

Als u meerdere Azure-abonnementen hebt, controleert u de abonnementen voor het account:

```azurecli
az account list
```

Geef het abonnement dat u wilt gebruiken:

```azurecli
az account set --subscription <your subscription ID>
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Identificeren van uw ExpressRoute-circuits voor configuratie

U kunt ExpressRoute globaal bereik inschakelen tussen elke twee ExpressRoute-circuits, zolang ze bevinden zich in de ondersteunde landen en op verschillende locaties voor peering zijn gemaakt. Als uw abonnement is eigenaar van beide circuits, kunt u een van beide circuit om uit te voeren van de configuratie, zoals verderop in dit artikel wordt beschreven. Als de twee circuits zich in verschillende Azure-abonnementen, moet u toestemming van een Azure-abonnement moet hebben en in de autorisatiesleutel moet slagen, wanneer u de configuratieopdracht in andere Azure-abonnement uitvoert.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Connectiviteit tussen uw on-premises netwerken

Bij het uitvoeren van de opdracht uit om connectiviteit, houd rekening met de volgende vereisten voor parameterwaarden:

* *peer-circuit* moet de volledige resource-ID. Bijvoorbeeld:

  > /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}

* *address-prefix* must be a "/29" IPv4 subnet (for example, "10.0.0.0/29"). We gebruiken IP-adressen in dit subnet om te maken van verbinding tussen de twee ExpressRoute-circuits. U moet geen adressen gebruiken in dit subnet in uw Azure virtual networks of in uw on-premises netwerken.

Voer de volgende CLI-opdracht voor het verbinding maken met twee ExpressRoute-circuits:

```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29>
```

De CLI-uitvoer ziet er zo uit:

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

Wanneer deze bewerking voltooid is, hebt u connectiviteit tussen uw on-premises netwerken aan beide zijden via uw twee ExpressRoute-circuits.

## <a name="enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions"></a>Connectiviteit tussen de ExpressRoute-circuits in verschillende Azure-abonnementen inschakelen

Als de twee circuits niet in hetzelfde Azure-abonnement, moet u autorisatie. In de volgende configuratie autorisatie genereren in circuit van 2-abonnement en de autorisatiesleutel voor het circuit 1 doorgegeven.

1. Een autorisatiesleutel genereren:

   ```azurecli
   az network express-route auth create --circuit-name <Circuit2Name> -g <Circuit2ResourceGroupName> -n <AuthorizationName>
   ```

   De CLI-uitvoer ziet er zo uit:

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

1. Maak een notitie van zowel de resource-ID en de autorisatiesleutel voor 2-circuit.

1. Voer de volgende opdracht op basis van circuit 1, doorgeven in een circuit van 2-resource-ID en -autorisatie-sleutel:

   ```azurecli
   az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29> --authorization-key <authorizationKey>
   ```

Wanneer deze bewerking voltooid is, hebt u connectiviteit tussen uw on-premises netwerken aan beide zijden via uw twee ExpressRoute-circuits.

## <a name="get-and-verify-the-configuration"></a>Krijgen te controleren of de configuratie

Gebruik de volgende opdracht om te controleren of de configuratie van het circuit waar de configuratie (1 circuit in het voorgaande voorbeeld) is gemaakt:

```azurecli
az network express-route show -n <CircuitName> -g <ResourceGroupName>
```

In het CLI-uitvoer, ziet u *CircuitConnectionStatus*. Het vertelt u of de connectiviteit tussen de twee circuits van de is tot stand gebracht ('verbonden') of niet tot stand gebracht ('verbinding verbroken'). 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Connectiviteit tussen uw on-premises netwerken uitschakelen

Voer de volgende opdracht om uit te schakelen connectiviteit, op basis van het circuit waar de configuratie (1 circuit in het vorige voorbeeld) is gemaakt.

```azurecli
az network express-route peering connection delete -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName>
```

Gebruik de ```show``` opdracht uit om te controleren of de status.

Wanneer deze bewerking voltooid is, u niet meer hebt connectiviteit tussen uw on-premises netwerken via uw ExpressRoute-circuits.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over ExpressRoute globaal bereik](expressroute-global-reach.md)
* [Controleer de ExpressRoute-verbinding](expressroute-troubleshooting-expressroute-overview.md)
* [Een ExpressRoute-circuit koppelen met een virtueel netwerk](expressroute-howto-linkvnet-arm.md)
