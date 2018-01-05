---
title: 'Routering voor een Azure ExpressRoute-circuit configureren: CLI | Microsoft Docs'
description: In dit artikel helpt u bij het maken en inrichten van de persoonlijke, openbare en Microsoft-peering van een ExpressRoute-circuit. In dit artikel leest u hoe u de status controleert en peerings voor uw circuit bijwerkt of verwijdert.
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2017
ms.author: cherylmc
ms.openlocfilehash: b54f7768e64e1689e5b25b94905beea6bd5471df
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-and-modify-routing-for-an-expressroute-circuit-using-cli"></a>Maken en aanpassen van routering voor een ExpressRoute-circuit met CLI

In dit artikel helpt u bij het maken en beheren van routeringsconfiguratie voor een ExpressRoute-circuit in het Resource Manager-implementatiemodel met CLI. U kunt ook controleren van de status, update of delete en inrichting ervan ongedaan peerings voor een ExpressRoute-circuit. Als u een andere methode gebruiken om te werken met uw circuit wilt, selecteert u een artikel uit de volgende lijst:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure-CLI](howto-routing-cli.md)
> * [Video - persoonlijke peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - openbare peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - Microsoft-peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a>Configuratievereisten

* Installeer eerst de meest recente versie van de CLI-opdrachten (2.0 of hoger). Zie [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli) voor meer informatie over het installeren van de CLI-opdrachten.
* Zorg ervoor dat u hebt bekeken de [vereisten](expressroute-prerequisites.md), [routeringsvereisten](expressroute-routing.md), en [werkstroom](expressroute-workflows.md) voordat u begint met de configuratie-pagina's.
* U moet een actief ExpressRoute-circuit hebben. Volg de instructies voor het [maken van een ExpressRoute-circuit](howto-circuit-cli.md) en laat het circuit inschakelen door de connectiviteitsprovider voordat u verder gaat. Het ExpressRoute-circuit moet zich in een status ingericht en zijn ingeschakeld voor u kunt de opdrachten in dit artikel uitvoeren.

Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die services met Laag-2-connectiviteit aanbieden. Als u gebruikmaakt van een serviceprovider die beheerde laag-3-services (meestal een IPVPN, zoals MPLS), uw connectiviteitsprovider configureren en beheren van routering voor u.

U kunt een, twee of alle drie de peerings (Azure privé, Azure openbaar en Microsoft) voor een ExpressRoute-circuit configureren. U kunt peerings configureren in elke gewenste volgorde. U moet er echter wel voor zorgen dat u de configuratie van elke peering een voor een voltooit. Zie voor meer informatie over routering domeinen en peerings [ExpressRoute-Routeringsdomeinen](expressroute-circuit-peerings.md).

## <a name="msft"></a>Microsoft-peering

Deze sectie helpt u bij het maken, verkrijgen, bijwerken en verwijderen van de configuratie voor de Microsoft-peering voor een ExpressRoute-circuit.

> [!IMPORTANT]
> Microsoft-peering van ExpressRoute-circuits die zijn geconfigureerd, voordat u 1 augustus 2017 hebben alle service voorvoegsels die worden geadverteerd via de Microsoft-peering, zelfs als routefilters zijn niet gedefinieerd. Microsoft-peering van ExpressRoute-circuits die zijn geconfigureerd op of na 1 augustus 2017 geen geen voorvoegsels aangekondigd totdat een routefilter is gekoppeld aan het circuit. Zie voor meer informatie [configureren van een routefilter voor Microsoft-peering](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Microsoft-peering maken

[!INCLUDE [Premium](../../includes/expressroute-mspeering-premium-include.md)]

1. Installeer de nieuwste versie van Azure CLI. Gebruik de meest recente versie van de Azure-opdrachtregelinterface (CLI). * Controleer de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.

  ```azurecli
  az login
  ```

  Selecteer het abonnement waarvoor u wilt maken van ExpressRoute-circuit.

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Maak een ExpressRoute-circuit. Volg de instructies voor het [maken van een ExpressRoute-circuit](howto-circuit-cli.md) en laat het circuit inrichten door de connectiviteitsprovider. Als uw connectiviteitsprovider beheerde laag-3-services biedt, kunt u uw connectiviteitsprovider om in te schakelen van Microsoft-peering voor u te vragen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routering voor u, na het maken van het circuit niet beheert blijven echter uw configuratie met behulp van de volgende stappen. 

3. Controleer het ExpressRoute-circuit is ingericht en ook ingeschakeld. Gebruik het volgende voorbeeld:

  ```azurecli
  az network express-route list
  ```

  Het antwoord is vergelijkbaar met het volgende voorbeeld:

  ```azurecli
  "allowClassicOperations": false,
  "authorizations": [],
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
  "gatewayManagerEtag": null,
  "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
  "location": "westus",
  "name": "MyCircuit",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
  "serviceProviderNotes": null,
  "serviceProviderProperties": {
    "bandwidthInMbps": 200,
    "peeringLocation": "Silicon Valley",
    "serviceProviderName": "Equinix"
  },
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. Configureer Microsoft-peering voor het circuit. Zorg ervoor dat u over de volgende informatie beschikt voordat u verder gaat.

  * Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR.
  * Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR.
  * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt.
  * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.
  * Geadverteerde voorvoegsels: u moet een lijst verstrekken van alle voorvoegsels die u via de BGP-sessie wilt adverteren. Alleen openbare IP-adresvoorvoegsels worden geaccepteerd. Als u van plan bent een reeks voorvoegsels verzenden, kunt u een door komma's gescheiden lijst verzenden. Deze voorvoegsels moeten voor u zijn geregistreerd in een RIR/IRR.
  * **Optionele -** klant-ASN: als u voorvoegsels adverteert die niet zijn geregistreerd op de AS-nummer peering, kunt u het AS-nummer waaraan ze zijn geregistreerd.
  * Naam van routeringsregister: u kunt het RIR/IRR opgeven waarbij het AS-nummer en de voorvoegsels zijn geregistreerd.
  * **Optionele -** een MD5-hash, als u ervoor kiest een te gebruiken.

   Voer het volgende voorbeeld voor het Microsoft-peering voor uw circuit te configureren:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 123.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 123.0.0.4/30 --vlan-id 300 --peering-type MicrosoftPeering --advertised-public-prefixes 123.1.0.0/24
  ```

### <a name="getmsft"></a>Details van Microsoft-peering weergeven

U kunt configuratie-informatie krijgen met behulp van het volgende voorbeeld:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzureMicrosoftPeering
```

De uitvoer lijkt op die in het volgende voorbeeld:

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzureMicrosoftPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": {
    "advertisedPublicPrefixes": [
        ""
      ],
     "advertisedPublicPrefixesState": "",
     "customerASN": ,
     "routingRegistryName": ""
  }
  "name": "AzureMicrosoftPeering",
  "peerAsn": ,
  "peeringType": "AzureMicrosoftPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="updatemsft"></a>Configuratie van Microsoft-peering bijwerken

U kunt elk deel van de configuratie bijwerken. Geadverteerde voorvoegsels van het circuit worden vanuit 123.1.0.0/24 bijgewerkt naar 124.1.0.0/24 in het volgende voorbeeld:

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroup --peering-type MicrosoftPeering --advertised-public-prefixes 124.1.0.0/24
```

### <a name="addIPv6msft"></a>Instellingen van de IPv6-Microsoft-peering toevoegen aan een bestaande IPv4-configuratie

```azurecli
az network express-route peering update -g ExpressRouteResourceGroup --circuit-name MyCircuit --peering-type MicrosoftPeering --ip-version ipv6 --primary-peer-subnet 2002:db00::/126 --secondary-peer-subnet 2003:db00::/126 --advertised-public-prefixes 2002:db00::/126
```

### <a name="deletemsft"></a>Microsoft-peering verwijderen

U kunt de peeringconfiguratie verwijderen door het uitvoeren van het volgende voorbeeld:

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name MicrosoftPeering
```

## <a name="private"></a>Persoonlijke Azure-peering

Deze sectie helpt u bij het maken, verkrijgen, bijwerken en verwijderen van de Azure persoonlijke peering configuratie voor een ExpressRoute-circuit.

### <a name="to-create-azure-private-peering"></a>Persoonlijke Azure-peering maken

1. Installeer de nieuwste versie van Azure CLI. U moet de meest recente versie van de Azure-opdrachtregelinterface (CLI) gebruiken. * Controleer de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.

  ```azurecli
  az login
  ```

  Het abonnement selecteren waarmee u een ExpressRoute-circuit wilt maken

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Maak een ExpressRoute-circuit. Volg de instructies voor het [maken van een ExpressRoute-circuit](howto-circuit-cli.md) en laat het circuit inrichten door de connectiviteitsprovider. Als uw connectiviteitsprovider beheerde laag-3-services biedt, kunt u uw connectiviteitsprovider om in te schakelen persoonlijke Azure-peering voor u te vragen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routering voor u, na het maken van het circuit niet beheert blijven echter uw configuratie met behulp van de volgende stappen.

3. Controleer het ExpressRoute-circuit is ingericht en ook ingeschakeld. Gebruik het volgende voorbeeld:

  ```azurecli
  az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
  ```

  Het antwoord is vergelijkbaar met het volgende voorbeeld:

  ```azurecli
  "allowClassicOperations": false,
  "authorizations": [],
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
  "gatewayManagerEtag": null,
  "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
  "location": "westus",
  "name": "MyCircuit",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
  "serviceProviderNotes": null,
  "serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
  },
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. Configureer persoonlijke Azure-peering voor het circuit. Zorg ervoor dat u de volgende items hebt voordat u verdergaat met de volgende stappen:

  * Een /30-subnet voor de primaire koppeling. Het subnet moet deel uitmaken van een adresruimte gereserveerd voor virtuele netwerken niet.
  * Een /30-subnet voor de secundaire koppeling. Het subnet moet deel uitmaken van een adresruimte gereserveerd voor virtuele netwerken niet.
  * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt.
  * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken. U kunt een persoonlijk AS-nummer voor deze peering gebruiken. Zorg dat u niet 65515 gebruikt.
  * **Optionele -** een MD5-hash, als u ervoor kiest een te gebruiken.

  Gebruik het volgende voorbeeld voor het configureren van persoonlijke Azure-peering voor uw circuit:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering
  ```

  Als u gebruiken een MD5-hash wilt, gebruikt u het volgende voorbeeld:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > Zorg dat u uw AS-nummer als peering-ASN opgeeft, niet als klant-ASN.
  > 
  > 

### <a name="getprivate"></a>Azure details van persoonlijke peering weergeven

U kunt configuratie-informatie krijgen met behulp van het volgende voorbeeld:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

De uitvoer lijkt op die in het volgende voorbeeld:

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePrivatePeering",
  "ipv6PeeringConfig": null,
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePrivatePeering",
  "peerAsn": 7671,
  "peeringType": "AzurePrivatePeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="updateprivate"></a>Configuratie van Azure persoonlijke peering bijwerken

U kunt elk deel van de configuratie in het volgende voorbeeld bijwerken. In dit voorbeeld wordt de VLAN-ID van het circuit wordt bijgewerkt van 100 tot 500.

```azurecli
az network express-route peering update --vlan-id 500 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

### <a name="deleteprivate"></a>Persoonlijke Azure-peering verwijderen

U kunt de peeringconfiguratie verwijderen door het uitvoeren van het volgende voorbeeld:

> [!WARNING]
> U moet ervoor zorgen dat alle virtuele netwerken losgekoppeld van het ExpressRoute-circuit zijn voordat u dit voorbeeld. 
> 
> 

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

## <a name="public"></a>Openbare Azure-peering

Deze sectie helpt u bij het maken, verkrijgen, bijwerken en verwijderen van de Azure openbare peering configuratie voor een ExpressRoute-circuit.

### <a name="to-create-azure-public-peering"></a>Openbare Azure-peering maken

1. Installeer de nieuwste versie van Azure CLI. U moet de meest recente versie van de Azure-opdrachtregelinterface (CLI) gebruiken. * Controleer de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.

  ```azurecli
  az login
  ```

  Selecteer het abonnement waarvoor u wilt maken van ExpressRoute-circuit.

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Maak een ExpressRoute-circuit.  Volg de instructies voor het [maken van een ExpressRoute-circuit](howto-circuit-cli.md) en laat het circuit inrichten door de connectiviteitsprovider. Als uw connectiviteitsprovider beheerde laag-3-services biedt, kunt u uw connectiviteitsprovider om in te schakelen openbare Azure-peering voor u te vragen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routering voor u, na het maken van het circuit niet beheert blijven echter uw configuratie met behulp van de volgende stappen.

3. Controleer of de ExpressRoute circuit is ingericht en ook ingeschakeld. Gebruik het volgende voorbeeld:

  ```azurecli
  az network express-route list
  ```

  Het antwoord is vergelijkbaar met het volgende voorbeeld:

  ```azurecli
  "allowClassicOperations": false,
  "authorizations": [],
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
  "gatewayManagerEtag": null,
  "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
  "location": "westus",
  "name": "MyCircuit",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
  "serviceProviderNotes": null,
  "serviceProviderProperties": {
    "bandwidthInMbps": 200,
    "peeringLocation": "Silicon Valley",
    "serviceProviderName": "Equinix"
  },
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. Configureer openbare Azure-peering voor het circuit. Zorg ervoor dat u hebt de volgende informatie beschikt voordat u verder.

  * Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn.
  * Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn.
  * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt.
  * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.
  * **Optionele -** een MD5-hash, als u ervoor kiest een te gebruiken.

  Voer het volgende voorbeeld voor het configureren van openbare Azure-peering voor uw circuit:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering
  ```

  Als u gebruiken een MD5-hash wilt, gebruikt u het volgende voorbeeld:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > Zorg dat u uw AS-nummer als peering-ASN opgeeft, niet als klant-ASN.

### <a name="getpublic"></a>Details van een Azure openbare peering weergeven

U kunt krijgen configuratiegegevens wilt weergeven in het volgende voorbeeld:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

De uitvoer lijkt op die in het volgende voorbeeld:

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePublicPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePublicPeering",
  "peerAsn": 7671,
  "peeringType": "AzurePublicPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="updatepublic"></a>Configuratie van een Azure openbare peering bijwerken

U kunt elk deel van de configuratie in het volgende voorbeeld bijwerken. In dit voorbeeld wordt wordt de VLAN-ID van het circuit van 200 bijgewerkt op 600.

```azurecli
az network express-route peering update --vlan-id 600 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

### <a name="deletepublic"></a>Openbare Azure-peering verwijderen

U kunt de peeringconfiguratie verwijderen door het uitvoeren van het volgende voorbeeld:

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

## <a name="next-steps"></a>Volgende stappen

Volgende stap, [Een VNet koppelen aan een ExpressRoute-circuit](howto-linkvnet-cli.md).

* Voor meer informatie over ExpressRoute-werkstromen raadpleegt u [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-werkstromen).
* Voor meer informatie over circuitpeering raadpleegt u [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-circuits en -routeringsdomeinen).
* Bekijk het [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtual Network-overzicht) voor meer informatie over het gebruik van virtuele netwerken.