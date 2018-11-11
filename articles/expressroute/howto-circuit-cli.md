---
title: 'Een Azure ExpressRoute-circuit maken en wijzigen: CLI | Microsoft Docs'
description: Dit artikel wordt beschreven hoe u kunt maken, inrichten, controleren, bijwerken, verwijderen en de inrichting van een ExpressRoute-circuit met behulp van CLI.
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: a53fe43365100c6d71fcc2b9e0944a221adf188d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51249231"
---
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a>Maken en aanpassen van een ExpressRoute-circuit met behulp van CLI


In dit artikel wordt beschreven hoe u een Azure ExpressRoute-circuit maken met behulp van de opdrachtregelinterface (CLI). In dit artikel leest u ook hoe om te controleren van de status, bijwerken of verwijderen en de inrichting van een circuit ongedaan maken. Als u wilt een andere methode gebruiken om te werken met ExpressRoute-circuits, kunt u het artikel in de volgende lijst:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure-CLI](howto-circuit-cli.md)
> * [Video - Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-circuit-classic.md)
> 

## <a name="before-you-begin"></a>Voordat u begint

* Installeer eerst de meest recente versie van de CLI-opdrachten (2.0 of hoger). Zie [Azure CLI installeren](/cli/azure/install-azure-cli) en [Aan de slag met Azure CLI](/cli/azure/get-started-with-azure-cli) voor meer informatie over de CLI-opdrachten.
* Controleer de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.

## <a name="create"></a>Maken en inrichten van een ExpressRoute-circuit

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Meld u aan bij uw Azure-account en selecteer uw abonnement

Als u wilt de configuratie begint, moet u zich aanmelden bij uw Azure-account. Gebruik de volgende voorbeelden kunt u verbinding maken:

```azurecli
az login
```

Controleer de abonnementen voor het account.

```azurecli
az account list
```

Selecteer het abonnement waarvoor u wenst te maken van een ExpressRoute-circuit.

```azurecli
az account set --subscription "<subscription ID>"
```

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. De lijst met ondersteunde providers, locaties en bandbreedten

Voordat u een ExpressRoute-circuit maken, moet u de lijst met ondersteunde connectiviteitsproviders, locaties en bandbreedte-opties. De CLI-opdracht 'az network express-route lijst--serviceproviders' retourneert deze informatie, die u in latere stappen gebruikt:

```azurecli
az network express-route list-service-providers
```

Het antwoord is vergelijkbaar met het volgende voorbeeld:

```azurecli
[
  {
    "bandwidthsOffered": [
      {
        "offerName": "50Mbps",
        "valueInMbps": 50
      },
      {
        "offerName": "100Mbps",
        "valueInMbps": 100
      },
      {
        "offerName": "200Mbps",
        "valueInMbps": 200
      },
      {
        "offerName": "500Mbps",
        "valueInMbps": 500
      },
      {
        "offerName": "1Gbps",
        "valueInMbps": 1000
      },
      {
        "offerName": "2Gbps",
        "valueInMbps": 2000
      },
      {
        "offerName": "5Gbps",
        "valueInMbps": 5000
      },
      {
        "offerName": "10Gbps",
        "valueInMbps": 10000
      }
    ],
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/expressRouteServiceProviders/",
    "location": null,
    "name": "AARNet",
    "peeringLocations": [
      "Melbourne",
      "Sydney"
    ],
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "tags": null,
    "type": "Microsoft.Network/expressRouteServiceProviders"
  },
```

Controleer het antwoord om te zien als uw connectiviteitsprovider wordt vermeld. Noteer de volgende informatie, u moet bij het maken van een circuit:

* Naam
* PeeringLocations
* BandwidthsOffered

U bent nu klaar om te maken van een ExpressRoute-circuit.

### <a name="3-create-an-expressroute-circuit"></a>3. Een ExpressRoute-circuit maken

> [!IMPORTANT]
> Uw ExpressRoute-circuit wordt in rekening gebracht vanaf het moment dat een servicesleutel wordt uitgegeven. Deze bewerking niet uitvoeren wanneer de connectiviteitsprovider gereed is voor de inrichting van het circuit.
> 
> 

Als u nog een resourcegroep hebt, moet u een maken voordat u uw ExpressRoute-circuit maken. U kunt een resourcegroep maken met de volgende opdracht:

```azurecli
az group create -n ExpressRouteResourceGroup -l "West US"
```

Het volgende voorbeeld ziet hoe u een 200 Mbps ExpressRoute-circuit via Equinix in Silicon Valley maakt. Als u verschillende instellingen en een andere provider gebruikt, vervangt u die informatie wanneer u uw aanvraag indienen. 

Zorg ervoor dat u opgeeft dat de juiste SKU-laag en de SKU-serie:

* SKU-laag wordt bepaald of een ExpressRoute-standard- of een premium-invoegtoepassing voor ExpressRoute is ingeschakeld. U kunt 'Standaard' voor de standard-SKU- of Premium om de premium-invoegtoepassing opgeven.
* SKU-serie, bepaalt de facturering. U kunt 'Metereddata' voor een gecontroleerde data-abonnement en 'Unlimiteddata' opgeven voor een abonnement voor onbeperkt gebruik. U kunt de facturering type van 'Metereddata' naar 'Unlimiteddata' wijzigen, maar u kunt het type van 'Unlimiteddata' naar 'Metereddata' niet wijzigen.


Uw ExpressRoute-circuit wordt in rekening gebracht vanaf het moment dat een servicesleutel wordt uitgegeven. Het volgende voorbeeld wordt een aanvraag voor een nieuwe servicesleutel:

```azurecli
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

Het antwoord bevat de sleutel van de service.

### <a name="4-list-all-expressroute-circuits"></a>4. Lijst van alle ExpressRoute-circuits

Als u een lijst met alle ExpressRoute-circuits die u hebt gemaakt, voert u de opdracht 'az network express-route list'. U kunt deze informatie op elk gewenst moment ophalen met behulp van deze opdracht. Als u alle circuits, moet u de aanroep zonder parameters.

```azurecli
az network express-route list
```

De sleutel van uw service wordt weergegeven in de *ServiceKey* veld van het antwoord.

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
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

U kunt gedetailleerde beschrijvingen van alle parameters ophalen door het uitvoeren van de installatieopdracht via de '-h "parameter.

```azurecli
az network express-route list -h
```

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. De servicesleutel verzenden naar uw connectiviteitsprovider om in te richten

'ServiceProviderProvisioningState' bevat informatie over de huidige status van inrichting aan de service-provider. De status bevat de status aan de kant van Microsoft. Zie voor meer informatie de [werkstromen artikel](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Wanneer u een nieuwe ExpressRoute-circuit maakt, wordt het circuit is in de volgende status:

```azurecli
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

Het circuit gewijzigd in de volgende status hebben de connectiviteitsprovider Bezig inschakelen voor u is:

```azurecli
"serviceProviderProvisioningState": "Provisioning"
"circuitProvisioningState": "Enabled"
```

Voordat u kunt een ExpressRoute-circuit gebruiken, moet deze de status van de volgende zijn:

```azurecli
"serviceProviderProvisioningState": "Provisioned"
"circuitProvisioningState": "Enabled
```

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Controleer regelmatig de status en de status van de sleutel van het circuit

De status en de status van de sleutel van het circuit controleren, laat u weten wanneer uw provider het circuit is ingeschakeld. Nadat het circuit is geconfigureerd, is 'ServiceProviderProvisioningState' wordt weergegeven als 'Ingericht', zoals wordt weergegeven in het volgende voorbeeld:

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
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

### <a name="7-create-your-routing-configuration"></a>7. De configuratie van de routering maken

Zie voor stapsgewijze instructies, de [ExpressRoute-circuit routeringsconfiguratie](howto-routing-cli.md) artikel maken en aanpassen-circuitpeerings.

> [!IMPORTANT]
> Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die services op laag 2-connectiviteit aanbieden. Als u een serviceprovider die beheerde laag-3-services (meestal een IP VPN, zoals MPLS), uw connectiviteitsprovider configureert en beheert routering voor u.
> 
> 

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. Een virtueel netwerk koppelen aan een ExpressRoute-circuit

Vervolgens moet u een virtueel netwerk koppelen aan uw ExpressRoute-circuit. Gebruik de [virtuele netwerken koppelen aan ExpressRoute-circuits](howto-linkvnet-cli.md) artikel.

## <a name="modify"></a>Een ExpressRoute-circuit wijzigen

U kunt bepaalde eigenschappen van een ExpressRoute-circuit wijzigen zonder gevolgen voor connectiviteit. U kunt de volgende wijzigingen zonder uitvaltijd maken:

* U kunt inschakelen of uitschakelen van een premium-invoegtoepassing voor ExpressRoute voor uw ExpressRoute-circuit.
* U kunt de bandbreedte van uw ExpressRoute-circuit vergroten, mits er capaciteit beschikbaar is op de poort. Het downgraden van de bandbreedte van een circuit wordt echter niet ondersteund. 
* U kunt de softwarelicentiecontrole abonnement wijzigen van naar gebruik voor onbeperkte gegevens. Echter, het wijzigen van de softwarelicentiecontrole plan van onbeperkte gegevens naar naar gebruik wordt niet ondersteund.
* U kunt inschakelen en uitschakelen *klassieke bewerkingen toestaan*.

Zie voor meer informatie over limieten en beperkingen, de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>De add-on ExpressRoute premium inschakelen

U kunt de invoegtoepassing ExpressRoute premium inschakelen voor uw bestaande circuit met behulp van de volgende opdracht uit:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

Het circuit heeft nu de ExpressRoute premium-invoegtoepassing-functies ingeschakeld. We beginnen facturering voor de mogelijkheid van premium-invoegtoepassing zodra de opdracht is uitgevoerd.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Om uit te schakelen van de premium-invoegtoepassing voor ExpressRoute

> [!IMPORTANT]
> Met deze bewerking kan mislukken als u resources die groter zijn dan wat is toegestaan voor de standard-circuit.
> 
> 

Voordat u de invoegtoepassing ExpressRoute premium uitschakelt, begrijpt u de volgende criteria:

* Voordat u een downgrade van premium naar standard uitvoeren, moet u ervoor zorgen dat er minder dan 10 virtuele netwerken die zijn gekoppeld aan het circuit. Als u meer dan 10 hebt, uw aanvraag voor bijwerken is mislukt en wij factureren u premium-tarieven.
* U moet alle virtuele netwerken in andere geopolitieke regio's ontkoppelen. Als u uw virtuele netwerken niet ontkoppelt, mislukt de updateaanvraag en wij factureren u premium-tarief.
* De routetabel moet minder dan 4000 routes voor persoonlijke peering. Als de grootte van uw route-table groter dan 4000 routes is, komt de BGP-sessie. De sessie wordt niet totdat het aantal geadverteerde voorvoegsels lager dan 4000 is worden ingeschakeld.

U kunt de ExpressRoute premium-invoegtoepassing voor de bestaande circuit uitschakelen met behulp van het volgende voorbeeld:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Bijwerken van de bandbreedte van het ExpressRoute-circuit

Voor de ondersteunde bandbreedte-opties voor uw provider, Controleer de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md). U kunt de grootte die groter is dan de grootte van uw bestaande circuit kiezen.

> [!IMPORTANT]
> Als er onvoldoende capaciteit op de bestaande poort, is het mogelijk om het ExpressRoute-circuit opnieuw te maken. U kunt het circuit niet bijwerken als er geen extra capaciteit beschikbaar is op die locatie.
>
> U kunt de bandbreedte van een ExpressRoute-circuit zonder onderbreking niet reduceren. Het downgraden van bandbreedte, moet u de inrichting van het ExpressRoute-circuit ongedaan maken en vervolgens opnieuw inrichten van een nieuwe ExpressRoute-circuit.
>

Nadat u de grootte die u nodig hebt, gebruikt u de volgende opdracht om het formaat van uw circuit te bepalen:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

Uw circuit wordt bepaald door de grootte aan de kant van Microsoft. U kunt vervolgens moet contact opnemen met uw connectiviteitsprovider voor het bijwerken van configuraties op hun kant zodat deze overeenkomt met deze wijziging. Nadat u deze melding, beginnen we u facturering voor de bijgewerkte bandbreedte-optie.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Voor het verplaatsen van de SKU van naar gebruik in onbeperkt

U kunt de SKU van een ExpressRoute-circuit wijzigen met behulp van het volgende voorbeeld:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Voor het beheren van toegang tot het klassieke en Resource Manager-omgevingen

Lees de instructies in [verplaatsen ExpressRoute-circuits van het klassieke naar het Resource Manager-implementatiemodel](expressroute-howto-move-arm.md).

## <a name="delete"></a>Opheffen van inrichting en het verwijderen van een ExpressRoute-circuit

Voor inrichting ongedaan maken en verwijderen van een ExpressRoute-circuit, zorg ervoor dat de volgende criteria:

* U moet alle virtuele netwerken loskoppelen van het ExpressRoute-circuit. Als deze bewerking is mislukt, controleert u het zien als alle virtuele netwerken zijn gekoppeld aan het circuit.
* Als het ExpressRoute-circuit serviceprovider-Inrichtingsstatus **Provisioning** of **ingerichte**, moet u werken met uw serviceprovider inrichting ongedaan maken van het circuit aan hun kant. We blijven reserveer resources en factureren u totdat de service-provider is voltooid ongedaan maken van inrichting van het circuit en ons wordt geïnformeerd.
* Als de service-provider de inrichting van het circuit is beëindigd, kunt u het circuit verwijderen. Wanneer de inrichting van een circuit is beëindigd, de serviceprovider Inrichtingsstatus is ingesteld op **niet ingericht**. Hiermee wordt ook de facturering voor het circuit gestopt.

U kunt uw ExpressRoute-circuit verwijderen door de volgende opdracht uit:

```azurecli
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Nadat u uw circuit hebt gemaakt, zorg er dan voor dat u de volgende taken uitvoeren:

* [Routering voor uw ExpressRoute-circuit maken en wijzigen](howto-routing-cli.md)
* [Uw virtueel netwerk koppelen aan uw ExpressRoute-circuit](howto-linkvnet-cli.md)
