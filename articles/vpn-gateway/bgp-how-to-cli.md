---
title: 'BGP configureren op een Azure VPN-gateway: Resource Manager en CLI | Microsoft Docs'
description: In dit artikel begeleidt u bij het configureren van BGP met een Azure VPN-gateway met behulp van Azure Resource Manager en CLI.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: cherylmc
ms.openlocfilehash: 98cd606ce930624ec5c591ffd8f13e0feae1a6c4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-bgp-on-an-azure-vpn-gateway-by-using-cli"></a>BGP op een Azure VPN-gateway configureren met CLI

In dit artikel helpt u BGP inschakelen op een cross-premises Site-naar-Site (S2S) VPN-verbinding en een VNet-naar-VNet-verbinding (dat wil zeggen, een verbinding tussen virtuele netwerken) met behulp van het Azure Resource Manager-implementatiemodel en Azure CLI.

## <a name="about-bgp"></a>Over BGP

BGP is het standaardprotocol voor routering gewoonlijk op het internet gebruikt voor het uitwisselen van routerings- en bereikbaarheidsgegevens informatie tussen twee of meer netwerken. BGP maakt de VPN-gateways en uw on-premises VPN-apparaten, BGP-peers of neighbors, voor het uitwisselen van routes genoemd. De routes informeren beide gateways over de beschikbaarheid en bereikbaarheid voor voorvoegsels zodat ze via de gateways of routers die betrokken zijn. BGP ook transitroutering tussen meerdere netwerken voor het doorgeven van de routes die een BGP-gateway van één BGP-peer voor alle andere BGP-peers leert.

Zie voor meer informatie over de voordelen van BGP en inzicht krijgen in de technische vereisten en overwegingen van het gebruik van BGP [overzicht van BGP met Azure VPN-gateways](vpn-gateway-bgp-overview.md).

In dit artikel helpt u bij de volgende taken:

* [BGP inschakelen voor uw VPN-gateway](#enablebgp) (vereist)

  U kunt vervolgens een van de volgende secties of beide voltooien:

* [Cross-premises verbinding maken met BGP](#crossprembgp)
* [Een VNet-naar-VNet verbinding maken met BGP](#v2vbgp)

Elk van deze drie secties vormt een basisbouwsteen voor het inschakelen van BGP in uw netwerkverbinding. Als u alle drie secties hebt voltooid, wordt de topologie build zoals weergegeven in het volgende diagram:

![BGP-topologieën](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

U kunt deze secties voor het bouwen van een complexere multihop-transit-netwerk dat voldoet aan uw behoeften kunt combineren.

## <a name ="enablebgp"></a>BGP inschakelen voor uw VPN-gateway

In deze sectie is vereist voordat u een van de stappen in de andere configuratie met twee secties uitvoert. De volgende configuratiestappen uit om de BGP-parameters van de Azure VPN-gateway te stellen zoals wordt weergegeven in het volgende diagram:

![BGP-gateway](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Voordat u begint

Installeer de nieuwste versie van de CLI-opdrachten (2.0 of hoger). Zie [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli) en [Aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-azure-cli) voor meer informatie over het installeren van de CLI-opdrachten.

### <a name="step-1-create-and-configure-testvnet1"></a>Stap 1: TestVNet1 maken en configureren

#### <a name="Login"></a>1. Verbinding maken met uw abonnement

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

#### <a name="2-create-a-resource-group"></a>2. Een resourcegroep maken

Het volgende voorbeeld maakt een resourcegroep met de naam TestRG1 op de locatie 'eastus'. Als u al een resourcegroep in de regio waar u wilt maken van het virtuele netwerk hebt, kunt u dat een in plaats daarvan.

```azurecli
az group create --name TestBGPRG1 --location eastus
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 maken

Het volgende voorbeeld wordt een virtueel netwerk met de naam TestVNet1 en drie subnetten: GatewaySubnet, FrontEnd en BackEnd. Wanneer u waarbij waarden vervangt bent, is het belangrijk dat u altijd het gatewaysubnet de naam bijzonder GatewaySubnet. Als u een andere naam kiest, mislukt het maken van de gateway.

De eerste opdracht maakt u de front-adresruimte en het subnet FrontEnd. De tweede opdracht maakt u een extra adresruimte voor het subnet voor back-end. De derde en vierde opdrachten maken de back-end-subnet en een GatewaySubnet

```azurecli
az network vnet create -n TestVNet1 -g TestBGPRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24 
 
az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestBGPRG1 
 
az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestBGPRG1 --address-prefix 10.12.0.0/24 
 
az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestBGPRG1 --address-prefix 10.12.255.0/27 
```

### <a name="step-2-create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Stap 2: De VPN-gateway voor TestVNet1 maken met de BGP-parameters

#### <a name="1-create-the-public-ip-address"></a>1. Het openbare IP-adres maken

Vraag een openbaar IP-adres aan. Het openbare IP-adres wordt toegewezen aan de VPN-gateway die u voor het virtuele netwerk maakt.

```azurecli
az network public-ip create -n GWPubIP -g TestBGPRG1 --allocation-method Dynamic 
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. De VPN-gateway maken met het AS-nummer

Maak de gateway van het virtuele netwerk voor TestVNet1. BGP is een op Route gebaseerde VPN-gateway vereist. U moet ook de extra parameter `-Asn` de autonomous system number (ASN) instellen voor TestVNet1. Maken van een gateway kan even duren (45 minuten of langer) om te voltooien. 

Als u deze opdracht met behulp van uitvoert de `--no-wait` parameter feedback of de uitvoer niet wordt weergegeven. De `--no-wait` parameter kan de gateway moet worden gemaakt op de achtergrond. Dit betekent niet dat de VPN-gateway wordt onmiddellijk gemaakt.

```azurecli
az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address GWPubIP -g TestBGPRG1 --vnet TestVNet1 --gateway-type Vpn --sku HighPerformance --vpn-type RouteBased --asn 65010 --no-wait
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. De Azure BGP-peer-IP-adres ophalen

Nadat de gateway is gemaakt, moet u de BGP-peer-IP-adres op de Azure VPN-gateway verkrijgen. Dit adres wordt gebruikt voor de VPN-gateway configureren als een BGP-peer voor uw on-premises VPN-apparaten.

Voer de volgende opdracht uit en controleer de `bgpSettings` sectie aan de bovenkant van de uitvoer:

```azurecli
az network vnet-gateway list -g TestBGPRG1 
 
  
"bgpSettings": { 
      "asn": 65010, 
      "bgpPeeringAddress": "10.12.255.30", 
      "peerWeight": 0 
    }
```

Nadat de gateway is gemaakt, kunt u deze gateway tot stand brengen van een cross-premises-verbinding of een VNet-naar-VNet-verbinding met BGP.

## <a name ="crossprembgp"></a>Cross-premises verbinding maken met BGP

Als u wilt een cross-premises-verbinding tot stand brengen, moet u een lokale netwerkgateway ter vertegenwoordiging van uw on-premises VPN-apparaat maken. U kunt vervolgens de Azure VPN-gateway verbinden met de lokale netwerkgateway. Hoewel deze stappen vergelijkbaar zijn met het maken van andere verbindingen, bevatten ze de extra eigenschappen vereist voor het opgeven van de BGP-configuratieparameters.

![BGP voor cross-premises](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)


### <a name="step-1-create-and-configure-the-local-network-gateway"></a>Stap 1: Maken en configureren van de lokale netwerkgateway

In deze oefening blijft het bouwen van de configuratie in het diagram worden weergegeven. Zorg ervoor dat u de waarden vervangt door de waarden die u voor uw configuratie wilt gebruiken. Wanneer u met lokale netwerkgateways werkt, moet u rekening houden met de volgende zaken:

* De lokale netwerkgateway kan zich in dezelfde locatie en resourcegroep als de VPN-gateway of het kan zijn in een andere locatie en de resourcegroep. Dit voorbeeld toont de gateways in verschillende resourcegroepen op verschillende locaties.
* Het voorvoegsel van de minimale die u nodig hebt om aan te geven voor de lokale netwerkgateway wordt het adres van de host van de BGP-peer IP-adres op uw VPN-apparaat. In dit geval is een /32-voorvoegsel van 10.52.255.254/32.
* Als een herinnering, moet u verschillende ASN's van BGP tussen uw on-premises netwerken en de virtuele Azure-netwerk. Als ze hetzelfde zijn, moet u uw VNet ASN gewijzigd als het ASN in uw on-premises VPN-apparaten al worden gebruikt als peer met andere BGP-neighbors.

Voordat u doorgaat, zorg ervoor dat u hebt voltooid de [BGP inschakelen voor uw VPN-gateway](#enablebgp) sectie van deze oefening en u nog bent verbonden met abonnement 1. Merk op dat in dit voorbeeld, maakt u een nieuwe resourcegroep. U ziet ook de twee extra parameters voor de lokale netwerkgateway: `Asn` en `BgpPeerAddress`.

```azurecli
az group create -n TestBGPRG5 -l eastus2 
 
az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site5 -g TestBGPRG5 --local-address-prefixes 10.51.255.254/32 --asn 65050 --bgp-peering-address 10.51.255.254
```

### <a name="step-2-connect-the-vnet-gateway-and-local-network-gateway"></a>Stap 2: Verbinding maken met de VNet-gateway en de lokale netwerkgateway

In deze stap maakt u de verbinding van TestVNet1 naar Site5. Geef de `--enable-bgp` parameter BGP inschakelen voor deze verbinding. 

In dit voorbeeld zijn de virtuele netwerkgateway en de lokale netwerkgateway in verschillende resourcegroepen. Wanneer de gateways in verschillende resourcegroepen, moet u de volledige resource-ID van de twee gateways voor het instellen van een verbinding tussen de virtuele netwerken.

#### <a name="1-get-the-resource-id-of-vnet1gw"></a>1. De resource-ID van VNet1GW

Gebruik de uitvoer van de volgende opdracht om de resource-ID voor VNet1GW:

```azurecli
az network vnet-gateway show -n VNet1GW -g TestBGPRG1
```

Zoek in de uitvoer naar de `"id":` regel. U moet de waarden binnen de aanhalingstekens de om verbinding te maken in de volgende sectie.

Voorbeelduitvoer:

```
{ 
  "activeActive": false, 
  "bgpSettings": { 
    "asn": 65010, 
    "bgpPeeringAddress": "10.12.255.30", 
    "peerWeight": 0 
  }, 
  "enableBgp": true, 
  "etag": "W/\"<your etag number>\"", 
  "gatewayDefaultSite": null, 
  "gatewayType": "Vpn", 
  "id": "/subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW",
```

Kopieer de waarden na `"id":` naar een teksteditor zoals Kladblok, zodat u ze gemakkelijk plakken kunt wanneer u de verbinding maakt. 

```
"id": "/subscriptions/<subscription ID>/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
```

#### <a name="2-get-the-resource-id-of-site5"></a>2. De resource-ID van Site5 ophalen

Gebruik de volgende opdracht om de resource-ID van Site5 uit de uitvoer:

```azurecli
az network local-gateway show -n Site5 -g TestBGPRG5
```

#### <a name="3-create-the-testvnet1-to-site5-connection"></a>3. De verbinding van TestVNet1 naar Site5 maken

In deze stap maakt u de verbinding van TestVNet1 naar Site5. Zoals eerder besproken, is het mogelijk om BGP- en niet-BGP-verbindingen voor dezelfde Azure VPN-gateway. Tenzij BGP is ingeschakeld in de eigenschap connection, wordt Azure niet BGP inschakelen voor deze verbinding, zelfs als de BGP-parameters zijn al geconfigureerd op beide gateways. Het abonnement-id's vervangen door uw eigen.

```azurecli
az network vpn-connection create -n VNet1ToSite5 -g TestBGPRG1 --vnet-gateway1 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW --enable-bgp -l eastus --shared-key "abc123" --local-gateway2 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG5/providers/Microsoft.Network/localNetworkGateways/Site5 --no-wait
```

Voor deze oefening bevat het volgende voorbeeld de parameters in te voeren in de BGP-configuratiesectie van uw on-premises VPN-apparaat:

```
Site5 ASN            : 65050
Site5 BGP IP         : 10.52.255.254
Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
Azure VNet ASN       : 65010
Azure VNet BGP IP    : 10.12.255.30
Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

De verbinding moet na enkele minuten tot stand zijn gebracht. De BGP-peeringsessie wordt gestart nadat de IPsec-verbinding tot stand is gebracht.

## <a name ="v2vbgp"></a>Een VNet-naar-VNet verbinding maken met BGP

In deze sectie voegt een VNet-naar-VNet-verbinding met BGP, zoals wordt weergegeven in het volgende diagram: 

![BGP voor VNet-naar-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

De volgende instructies worden overgenomen van de stappen in de voorgaande secties. Als u wilt maken en TestVNet1 en de VPN-gateway met BGP configureren, moet u de [BGP inschakelen voor uw VPN-gateway](#enablebgp) sectie.

### <a name="step-1-create-testvnet2-and-the-vpn-gateway"></a>Stap 1: TestVNet2 en de VPN-gateway maken

Het is belangrijk om ervoor te zorgen dat de IP-adresruimte van het nieuwe virtuele netwerk TestVNet2, niet met een van uw VNet-bereiken overlapt.

In dit voorbeeld wordt de virtuele netwerken tot hetzelfde abonnement behoren. U kunt de VNet-naar-VNet-verbindingen tussen de verschillende abonnementen instellen. Zie voor meer informatie, [een VNet-naar-VNet-verbinding configureren](vpn-gateway-howto-vnet-vnet-cli.md). Zorg ervoor dat u toevoegt `-EnableBgp $True` bij het maken van de verbindingen om in te schakelen van BGP.

#### <a name="1-create-a-new-resource-group"></a>1. Een nieuwe resourcegroep maken

```azurecli
az group create -n TestBGPRG2 -l westus
```

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. TestVNet2 in de nieuwe resourcegroep maken

De eerste opdracht maakt u de front-adresruimte en het subnet FrontEnd. De tweede opdracht maakt u een extra adresruimte voor het subnet voor back-end. De derde en vierde opdrachten maken de back-end-subnet en een GatewaySubnet

```azurecli
az network vnet create -n TestVNet2 -g TestBGPRG2 --address-prefix 10.21.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.21.0.0/24 
 
az network vnet update -n TestVNet2 --address-prefixes 10.21.0.0/16 10.22.0.0/16 -g TestBGPRG2 
 
az network vnet subnet create --vnet-name TestVNet2 -n BackEnd -g TestBGPRG2 --address-prefix 10.22.0.0/24 
 
az network vnet subnet create --vnet-name TestVNet2 -n GatewaySubnet -g TestBGPRG2 --address-prefix 10.22.255.0/27
```

#### <a name="3-create-the-public-ip-address"></a>3. Het openbare IP-adres maken

Vraag een openbaar IP-adres aan. Het openbare IP-adres wordt toegewezen aan de VPN-gateway die u voor het virtuele netwerk maakt.

```azurecli
az network public-ip create -n GWPubIP2 -g TestBGPRG2 --allocation-method Dynamic
```

#### <a name="4-create-the-vpn-gateway-with-the-as-number"></a>4. De VPN-gateway maken met het AS-nummer

De virtuele netwerkgateway voor TestVNet2 maken. U moet de standaard een ASN overschrijven op uw Azure VPN-gateways. De ASN's voor de verbonden virtuele netwerken moet verschillen van BGP en transitroutering inschakelen.
 
```azurecli
az network vnet-gateway create -n VNet2GW -l westus --public-ip-address GWPubIP2 -g TestBGPRG2 --vnet TestVNet2 --gateway-type Vpn --sku Standard --vpn-type RouteBased --asn 65020 --no-wait
```

### <a name="step-2-connect-the-testvnet1-and-testvnet2-gateways"></a>Stap 2: Verbinding maken met de TestVNet1 en TestVNet2 gateways

In deze stap maakt u de verbinding van TestVNet1 naar Site5. Om in te schakelen BGP voor deze verbinding, moet u de `--enable-bgp` parameter.

In het volgende voorbeeld wordt zich de virtuele netwerkgateway en de lokale netwerkgateway in verschillende resourcegroepen. Wanneer de gateways in verschillende resourcegroepen, moet u de volledige resource-ID van de twee gateways voor het instellen van een verbinding tussen de virtuele netwerken. 

#### <a name="1-get-the-resource-id-of-vnet1gw"></a>1. De resource-ID van VNet1GW 

De resource-ID van VNet1GW ophalen uit de uitvoer van de volgende opdracht:

```azurecli
az network vnet-gateway show -n VNet1GW -g TestBGPRG1
```

#### <a name="2-get-the-resource-id-of-vnet2gw"></a>2. De resource-ID van VNet2GW ophalen

De resource-ID van VNet2GW ophalen uit de uitvoer van de volgende opdracht:

```azurecli
az network vnet-gateway show -n VNet2GW -g TestBGPRG2
```

#### <a name="3-create-the-connections"></a>3. De verbindingen maken

De verbinding van TestVNet1 naar TestVNet2 en de verbinding maken via TestVNet2 naar TestVNet1. Het abonnement-id's vervangen door uw eigen.

```azurecli
az network vpn-connection create -n VNet1ToVNet2 -g TestBGPRG1 --vnet-gateway1 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW --enable-bgp -l eastus --shared-key "efg456" --vnet-gateway2 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG2/providers/Microsoft.Network/virtualNetworkGateways/VNet2GW
```

```azurecli
az network vpn-connection create -n VNet2ToVNet1 -g TestBGPRG2 --vnet-gateway1 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG2/providers/Microsoft.Network/virtualNetworkGateways/VNet2GW --enable-bgp -l westus --shared-key "efg456" --vnet-gateway2 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
```

> [!IMPORTANT]
> Inschakelen van BGP voor *beide* verbindingen.
> 
> 

Nadat u deze stappen hebt voltooid, wordt de verbinding over enkele minuten tot stand gebracht. De BGP-peeringsessie zijn nadat de VNet-naar-VNet-verbinding is voltooid.

## <a name="next-steps"></a>Volgende stappen

Nadat de verbinding is voltooid, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie voor stappen [maken van een virtuele machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
