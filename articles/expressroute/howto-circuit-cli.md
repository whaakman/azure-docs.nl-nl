---
title: 'Maken en wijzigen van een Azure ExpressRoute-circuit: CLI | Microsoft Docs'
description: In dit artikel wordt beschreven hoe maken, richten, controleren, bijwerken, verwijderen en een ExpressRoute-circuit met CLI inrichting ervan ongedaan.
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
ms.date: 07/25/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: 1a1c9a96b772868e2c832e9ff57874038c0db2d4
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a>Maken en een ExpressRoute-circuit met CLI wijzigen


In dit artikel wordt beschreven hoe een Azure ExpressRoute-circuit maken via de opdrachtregelinterface (CLI). Dit artikel ziet u ook het controleren van de status, update of delete en inrichting ervan ongedaan maakt een circuit. Als u wilt een andere methode gebruiken om te werken met ExpressRoute-circuits, kunt u het artikel uit de volgende lijst:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure-CLI](howto-circuit-cli.md)
> * [Video - Azure-portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-circuit-classic.md)
> 

## <a name="before-you-begin"></a>Voordat u begint

* Installeer eerst de meest recente versie van de CLI-opdrachten (2.0 of hoger). Zie [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli) en [Aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-azure-cli) voor meer informatie over het installeren van de CLI-opdrachten.
* Controleer de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.

## <a name="create-and-provision-an-expressroute-circuit"></a>Maken en een ExpressRoute-circuit inrichten

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Aanmelden bij uw Azure-account en uw abonnement te selecteren

Om te beginnen met uw configuratie, moet u zich aanmelden bij uw Azure-account. Met de volgende voorbeelden kunt u verbinding kunt maken:

```azurecli
az login
```

Controleer de abonnementen voor het account.

```azurecli
az account list
```

Selecteer het abonnement waarvoor u wilt maken van een ExpressRoute-circuit.

```azurecli
az account set --subscription "<subscription ID>"
```

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. De lijst met ondersteunde providers, locaties en bandbreedten ophalen

Voordat u een ExpressRoute-circuit maken, moet u de lijst met ondersteunde connectiviteitsproviders, locaties en bandbreedte-opties. De CLI-opdracht 'az netwerk express route lijst-service-providers' retourneert deze informatie, die u in latere stappen:

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

U kunt nu gereed voor het maken van een ExpressRoute-circuit.

### <a name="3-create-an-expressroute-circuit"></a>3. Een ExpressRoute-circuit maken

> [!IMPORTANT]
> Uw ExpressRoute-circuit wordt gefactureerd vanaf het moment dat de sleutel van een service wordt verleend. Deze bewerking niet uitvoeren wanneer de connectiviteitsprovider gereed is voor het inrichten van het circuit.
> 
> 

Als u nog een resourcegroep hebt, moet u een maken voordat u uw ExpressRoute-circuit maken. U kunt een resourcegroep maken met de volgende opdracht:

```azurecli
az group create -n ExpressRouteResourceGroup -l "West US"
```

Het volgende voorbeeld ziet hoe u een 200-Mbps ExpressRoute-circuit via Equinix in Silicon Valley maakt. Als u een andere provider en andere instellingen gebruikt, vervangt u die informatie door wanneer u uw aanvraag. 

Zorg ervoor dat u de juiste SKU-laag en de SKU-serie opgeven:

* SKU-laag bepaalt of een standaard ExpressRoute of een premium-invoegtoepassing voor ExpressRoute is ingeschakeld. U kunt 'Standaard' voor de SKU-standard of Premium voor de premium-invoegtoepassing opgeven.
* SKU-serie, bepaalt de facturering. U kunt 'Metereddata' voor een datalimiet plannings- en 'Unlimiteddata' opgeven voor een onbeperkte gegevens-plan. U kunt ook het facturering type 'Metereddata'-'Unlimiteddata' wijzigen, maar u het type 'Unlimiteddata'-'Metereddata' niet wijzigen.


Uw ExpressRoute-circuit wordt gefactureerd vanaf het moment dat de sleutel van een service wordt verleend. Het volgende voorbeeld wordt een aanvraag voor een nieuwe servicesleutel:

```azurecli
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

Het antwoord bevat de sleutel van de service.

### <a name="4-list-all-expressroute-circuits"></a>4. Lijst van alle ExpressRoute-circuits

Als u een lijst met alle ExpressRoute-circuits die u hebt gemaakt, voert u de opdracht 'az express route lijst met netwerken'. Met deze opdracht kunt u deze informatie op elk gewenst moment ophalen. Als alle circuits wilt weergeven, voer de aanroep zonder parameters.

```azurecli
az network express-route list
```

De sleutel van uw service wordt vermeld in de *ServiceKey* veld van het antwoord.

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

Krijgt u gedetailleerde beschrijvingen van alle parameters door het uitvoeren van de opdracht met behulp van de '-h' parameter.

```azurecli
az network express-route list -h
```

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. De servicesleutel verzenden naar uw connectiviteitsprovider voor inrichting

'ServiceProviderProvisioningState' bevat informatie over de huidige status van inrichting aan de kant van de serviceprovider. De status van de biedt status van de aan de kant van Microsoft. Zie voor meer informatie de [werkstromen artikel](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Wanneer u een nieuwe ExpressRoute-circuit maakt, wordt het circuit in de volgende status is:

```azurecli
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

Het circuit wordt gewijzigd in de volgende status wanneer de connectiviteitsprovider wordt deze voor u inschakelen:

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

Controleren of de status en de status van de sleutel van het circuit, laat u weten wanneer uw provider uw circuit is ingeschakeld. Nadat het circuit is geconfigureerd, is 'ServiceProviderProvisioningState' wordt weergegeven als 'Ingericht', zoals wordt weergegeven in het volgende voorbeeld:

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

### <a name="7-create-your-routing-configuration"></a>7. Maken van uw configuratie van de routering

Zie voor stapsgewijze instructies de [ExpressRoute-circuit routeringsconfiguratie](howto-routing-cli.md) artikel maken en wijzigen van circuit peerings.

> [!IMPORTANT]
> Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die services op laag 2-connectiviteit aanbieden. Als u een serviceprovider die beheerde laag-3-services (meestal een IP VPN, zoals MPLS), uw connectiviteitsprovider configureert en beheert routering voor u.
> 
> 

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. Een virtueel netwerk koppelen aan een ExpressRoute-circuit

Vervolgens moet u een virtueel netwerk koppelen aan uw ExpressRoute-circuit. Gebruik de [virtuele netwerken koppelen aan ExpressRoute-circuits](howto-linkvnet-cli.md) artikel.

## <a name="modify"></a>Wijzigen van een ExpressRoute-circuit

U kunt bepaalde eigenschappen van een ExpressRoute-circuit wijzigen zonder enige impact op connectiviteit. U kunt de volgende wijzigingen zonder uitvaltijd aanbrengen:

* U kunt in- of uitschakelen van een ExpressRoute premium-invoegtoepassing voor ExpressRoute-circuit.
* U kunt de bandbreedte van uw ExpressRoute-circuit vergroten, mits er capaciteit beschikbaar is op de poort. Echter, de bandbreedte van een circuit downgraden wordt niet ondersteund. 
* U kunt het plan softwarelicentiecontrole uit Datalimiet gegevens wijzigen in onbeperkte gegevens. Echter, het softwarelicentiecontrole plan wijzigen van onbeperkt in Datalimiet gegevens wordt niet ondersteund.
* U kunt inschakelen en uitschakelen *klassieke bewerkingen toestaan*.

Zie voor meer informatie over limieten en beperkingen de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>De invoegtoepassing ExpressRoute premium inschakelen

U kunt de invoegtoepassing ExpressRoute premium inschakelen voor uw bestaande circuit met de volgende opdracht:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

Het circuit heeft nu de ExpressRoute premium-invoegtoepassing voor functies ingeschakeld. We beginnen facturering voor de premium-invoegtoepassing mogelijkheid zodra de opdracht met succes is uitgevoerd.

### <a name="to-disable-the-expressroute-premium-add-on"></a>De invoegtoepassing ExpressRoute premium uitschakelen

> [!IMPORTANT]
> Deze bewerking kan mislukken als u resources die groter zijn dan wat voor het standaard circuit is toegestaan.
> 
> 

Voordat u de invoegtoepassing ExpressRoute premium uitschakelt, inzicht in de volgende criteria:

* Voordat u van premium op standaard downgraden, moet u ervoor zorgen dat er minder dan 10 virtuele netwerken die zijn gekoppeld aan het circuit. Als u meer dan 10 hebt, uw aanvraag bijwerken mislukt en er kosten in rekening brengen volgens de premietarieven voor.
* U moet alle virtuele netwerken in andere geopolitieke regio's ontkoppelen. Als u niet alle virtuele netwerken ontkoppelt, mislukt de updateaanvraag en we u volgens de premietarieven voor rekening.
* De routetabel moet minder dan 4000 routes voor persoonlijke peering. Als uw tabel route groter dan 4000 routes is, wordt de BGP-sessie verwijderd. De sessie won't totdat het aantal geadverteerde voorvoegsels lager dan 4000 is worden ingeschakeld.

U kunt de invoegtoepassing ExpressRoute premium voor een bestaand circuit uitschakelen met behulp van het volgende voorbeeld:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Bijwerken van de bandbreedte van het ExpressRoute-circuit

Raadpleeg voor de ondersteunde bandbreedte-opties voor uw provider de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md). U kunt de grootte die groter zijn dan de grootte van uw bestaande circuit verzamelen.

> [!IMPORTANT]
> Als er onvoldoende capaciteit op de bestaande poort, moet u wellicht opnieuw maken van het ExpressRoute-circuit. U kunt het circuit niet upgraden als er geen extra capaciteit beschikbaar is op die locatie.
>
> U kunt de bandbreedte van een ExpressRoute-circuit zonder onderbreking niet reduceren. Downgraden bandbreedte, moet u het ExpressRoute-circuit inrichting ervan ongedaan en vervolgens opnieuw inrichten van nieuwe ExpressRoute-circuit.
>

Nadat u de grootte die u nodig hebt, gebruik de volgende opdracht om het formaat van uw circuit te bepalen:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

Uw circuit wordt aangepast om aan de kant van Microsoft. U kunt vervolgens moet contact op met uw connectiviteitsprovider om bij te werken-configuraties van hun kant overeenkomen met deze wijziging. Nadat u deze melding, beginnen wij u voor de optie bijgewerkte bandbreedte facturering.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Als u wilt verplaatsen van de SKU van datalimiet op onbeperkt

U kunt de SKU van een ExpressRoute-circuit wijzigen met behulp van het volgende voorbeeld:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Toegang tot het klassieke en het Resource Manager-omgevingen

Lees de instructies in [verplaatsen ExpressRoute-circuits van het klassieke naar het Resource Manager-implementatiemodel](expressroute-howto-move-arm.md).

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Opheffen van inrichting en een ExpressRoute-circuit verwijderen

Voor inrichting ervan ongedaan maakt en een ExpressRoute-circuit verwijderen, moet dat u weten dat de volgende criteria:

* U moet alle virtuele netwerken van het ExpressRoute-circuit ontkoppelen. Als deze bewerking is mislukt, controleert u of er geen virtuele netwerken zijn gekoppeld aan het circuit.
* Als de ExpressRoute-circuit serviceprovider Inrichtingsstatus **inrichten** of **ingericht**, moet u werken met uw serviceprovider voor inrichting ervan ongedaan maakt het circuit op hun kant. We blijven resources reserveren en u in rekening brengen totdat de serviceprovider is voltooid opheffen van inrichting het circuit en waarschuwt ons.
* Als de provider heeft het circuit deprovisioned, kunt u het circuit verwijderen. Wanneer een circuit is gemaakt, de serviceprovider Inrichtingsstatus is ingesteld op **niet ingericht**. Hierdoor wordt voorkomen dat facturering voor het circuit.

U kunt uw ExpressRoute-circuit verwijderen door de volgende opdracht uit te voeren:

```azurecli
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Nadat u het circuit hebt gemaakt, zorg er dan voor dat u de volgende taken uitvoeren:

* [Maken en aanpassen van routering voor ExpressRoute-circuit](howto-routing-cli.md)
* [Het virtuele netwerk koppelen aan uw ExpressRoute-circuit](howto-linkvnet-cli.md)