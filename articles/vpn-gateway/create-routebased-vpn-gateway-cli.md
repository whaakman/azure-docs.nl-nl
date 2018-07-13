---
title: 'Maken van een op route gebaseerde Azure VPN-gateway: CLI | Microsoft Docs'
description: Snel informatie over het maken van een VPN-Gateway met behulp van CLI
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2018
ms.author: cherylmc
ms.openlocfilehash: d0f4e292c6f5a2725b4a9efe91e78c6e634ea64e
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38605974"
---
# <a name="create-a-route-based-vpn-gateway-using-cli"></a>Een op route gebaseerde VPN-gateway maken met behulp van CLI

Dit artikel helpt u snel een op route gebaseerde Azure VPN-gateway met de Azure CLI kunt maken. Een VPN-gateway wordt gebruikt bij het maken van een VPN-verbinding met uw on-premises netwerk. U kunt ook een VPN-gateway gebruiken om VNets te verbinden.

De stappen in dit artikel maakt een VNet, een subnet, een gatewaysubnet en een op route gebaseerde VPN-gateway (virtuele netwerkgateway). Een virtuele netwerkgateway kan duren voordat 45 minuten of langer te maken. Als het maken van de gateway is voltooid, kunt u vervolgens verbindingen maken. Deze stappen hebt u een Azure-abonnement nodig. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om na te gaan welke versie er is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resource-groep met de [az-groep maken](/cli/azure/group#az_group_create) opdracht. Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 


```azurecli-interactive 
az group create --name TestRG1 --location eastus
```

## <a name="vnet"></a>Een virtueel netwerk maken

Maak een virtueel netwerk met de [az network vnet maken](/cli/azure/network/vnet#az_network_vnet_create) opdracht. Het volgende voorbeeld wordt een virtueel netwerk met de naam **VNet1** in de **EastUS** locatie:

```azurecli-interactive 
az network vnet create \
  -n VNet1 \
  -g TestRG1 \
  -l eastus \
  --address-prefix 10.1.0.0/16 \
  --subnet-name Frontend \
  --subnet-prefix 10.1.0.0/24
```

## <a name="gwsubnet"></a>Een gatewaysubnet toevoegen

Het gatewaysubnet bevat de gereserveerde IP-adressen die de virtuele-netwerkgatewayservices gebruik. Gebruik de volgende voorbeelden om toe te voegen een gateway-subnet:

```azurepowershell-interactive
az network vnet subnet create \
  --vnet-name VNet1 \
  -n GatewaySubnet \
  -g TestRG1 \
  --address-prefix 10.1.255.0/27 
```

## <a name="PublicIP"></a>Vraag een openbaar IP-adres

Een VPN-gateway moet een dynamisch toegewezen openbare IP-adres hebben. Het openbare IP-adres wordt toegewezen aan de VPN-gateway die u voor het virtuele netwerk maakt. Gebruik het volgende voorbeeld om aan te vragen van een openbaar IP-adres:

```azurecli-interactive
az network public-ip create \
  -n VNet1GWIP \
  -g TestRG1 \
  --allocation-method Dynamic 
```

## <a name="CreateGateway"></a>De VPN-gateway maken

Maak de VPN Gateway met behulp van de opdracht [az network vnet-gateway create](/cli/azure/group#az_network_vnet_gateway_create).

Als u deze opdracht met behulp van uitvoert de `--no-wait` parameter, u kunt geen feedback of uitvoer niet ziet. De `--no-wait` parameter zorgt ervoor dat de gateway moet worden gemaakt op de achtergrond. Dit betekent niet dat de VPN-gateway onmiddellijk wordt gemaakt.

```azurecli-interactive
az network vnet-gateway create \
  -n VNet1GW \
  -l eastus \
  --public-ip-address VNet1GWIP \
  -g TestRG1 \
  --vnet VNet1 \
  --gateway-type Vpn \
  --sku VpnGw1 \
  --vpn-type RouteBased \
  --no-wait
```

Het maken van een VPN-gateway kan tot 45 minuten of langer duren.

## <a name="viewgw"></a>De VPN-gateway weergeven

```azurecli-interactive
az network vnet-gateway show \
  -n VNet1GW \
  -g TestRG1
```

Het antwoord ziet er ongeveer als volgt uit:

```
{
  "activeActive": false,
  "bgpSettings": null,
  "enableBgp": false,
  "etag": "W/\"6c61f8cb-d90f-4796-8697\"",
  "gatewayDefaultSite": null,
  "gatewayType": "Vpn",
  "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW",
  "ipConfigurations": [
    {
      "etag": "W/\"6c61f8cb-d90f-4796-8697\"",
      "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/vnetGatewayConfig0",
      "name": "vnetGatewayConfig0",
      "privateIpAllocationMethod": "Dynamic",
      "provisioningState": "Updating",
      "publicIpAddress": {
        "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/publicIPAddresses/VNet1GWIP",
        "resourceGroup": "TestRG1"
      },
      "resourceGroup": "TestRG1",
      "subnet": {
        "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/virtualNetworks/VNet1/subnets/GatewaySubnet",
        "resourceGroup": "TestRG1"
      }
    }
  ],
  "location": "eastus",
  "name": "VNet1GW",
  "provisioningState": "Updating",
  "resourceGroup": "TestRG1",
  "resourceGuid": "69c269e3-622c-4123-9231",
  "sku": {
    "capacity": 2,
    "name": "VpnGw1",
    "tier": "VpnGw1"
  },
  "tags": null,
  "type": "Microsoft.Network/virtualNetworkGateways",
  "vpnClientConfiguration": null,
  "vpnType": "RouteBased"
}
```

### <a name="view-the-public-ip-address"></a>Het openbare IP-adres weergeven

Als u het openbare IP-adres toegewezen aan uw gateway, gebruik het volgende voorbeeld:

```azurecli-interactive
az network public-ip show \
  --name VNet1GWIP \
  --resource-group TestRG11
```

De waarde die is gekoppeld aan de **ipAddress** veld is het openbare IP-adres van uw VPN-gateway.

Voorbeeld van een reactie:

```
{
  "dnsSettings": null,
  "etag": "W/\"a12d4d03-b27a-46cc-b222-8d9364b8166a\"",
  "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG1/providers/Microsoft.Network/publicIPAddresses/VNet1GWIP",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "13.90.195.184",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/vnetGatewayConfig0",
```
## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de resources die u hebt gemaakt niet meer nodig hebt, gebruikt u [az group delete](/cli/azure/group#az_group_delete) om de resourcegroep te verwijderen. Hiermee verwijdert u de resourcegroep en alle resources die deze bevat.

```azurecli-interactive 
az group delete --name TestRG1 --yes
```

## <a name="next-steps"></a>Volgende stappen

Zodra de gateway heeft gemaakt, kunt u een verbinding maken tussen uw virtuele netwerk en een ander VNet. Of maak een verbinding tussen uw virtuele netwerk en een on-premises locatie.

> [!div class="nextstepaction"]
> [Maak een site-naar-site-verbinding](vpn-gateway-create-site-to-site-rm-powershell.md)<br><br>
> [Een punt-naar-site-verbinding maken](vpn-gateway-howto-point-to-site-rm-ps.md)<br><br>
> [Maak een verbinding met een ander VNet](vpn-gateway-vnet-vnet-rm-ps.md)