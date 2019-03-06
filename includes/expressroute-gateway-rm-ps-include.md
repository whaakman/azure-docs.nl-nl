---
title: bestand opnemen
description: bestand opnemen
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 02/21/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c50e2b082c3181c37e9d129766d4bf400075d5a8
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57410667"
---
De stappen voor deze taak gebruik van een VNet op basis van de waarden in de volgende lijst voor de configuratie-verwijzing. Extra instellingen en de namen worden ook beschreven in deze lijst. We gebruik geen deze lijst rechtstreeks in een van de stappen, hoewel we variabelen op basis van de waarden in deze lijst toevoegen. U kunt de lijst om te gebruiken als uitgangspunt, vervang de waarden door uw eigen kopiëren.

* Virtuele-netwerknaam 'TestVNet' =
* Virtuele netwerkadresruimte = 192.168.0.0/16
* Resourcegroep = "TestRG"
* Subnet1 naam = 'FrontEnd' 
* Subnet1-adresruimte = "192.168.1.0/24"
* Naam Gatewaysubnet: 'GatewaySubnet' u moet altijd een gatewaysubnet de naam *GatewaySubnet*.
* Gateway Subnet een adresruimte = "192.168.200.0/26"
* Regio = 'VS-Oost'
* Gateway Name = "GW"
* De naam van de gateway-IP = "GWIP"
* De naam van de gateway-IP-configuratie = "gwipconf"
* Type = 'ExpressRoute' in dit type is vereist voor een ExpressRoute-configuratie.
* Gatewaynaam van openbare IP = "gwpip"

## <a name="add-a-gateway"></a>Een gateway toevoegen
1. Verbinding maken met uw Azure-abonnement.

  [!INCLUDE [Sign in](expressroute-cloud-shell-connect.md)]
2. Declareer de variabelen voor deze oefening. Zorg ervoor dat het bewerken van het voorbeeld in overeenstemming met de instellingen die u wilt gebruiken.

  ```azurepowershell-interactive 
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "GW"
  $GWIPName = "GWIP"
  $GWIPconfName = "gwipconf"
  $VNetName = "TestVNet"
  ```
3. Het virtuele netwerkobject Store als een variabele.

  ```azurepowershell-interactive
  $vnet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  ```
4. Een gatewaysubnet toevoegen aan uw Virtueelnetwerk. Het gatewaysubnet moet de naam 'GatewaySubnet'. U moet een gatewaysubnet/27 of groter (/ 26/25 enz.).

  ```azurepowershell-interactive
  Add-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
  ```
5. Stel de configuratie in.

  ```azurepowershell-interactive
  $vnet = Set-AzVirtualNetwork -VirtualNetwork $vnet
  ```
6. Het gatewaysubnet Store als een variabele.

  ```azurepowershell-interactive
  $subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
  ```
7. Vraag een openbaar IP-adres aan. Het IP-adres is aangevraagd voordat het maken van de gateway. Kan geen u het IP-adres dat u gebruiken wilt. het wordt dynamisch toegewezen. U gebruikt dit IP-adres in de volgende configuratiesectie. De AllocationMethod moet dynamisch zijn.

  ```azurepowershell-interactive
  $pip = New-AzPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  ```
8. Maak de configuratie voor uw gateway. De gatewayconfiguratie bepaalt welk subnet en openbaar IP-adres moeten worden gebruikt. In deze stap geeft u de configuratie die wordt gebruikt bij het maken van de gateway. Deze stap maakt geen daadwerkelijk het gateway-object. Gebruik het voorbeeld hieronder om de gatewayconfiguratie te maken.

  ```azurepowershell-interactive
  $ipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```
9. Maak de gateway. In deze stap is het **- GatewayType** is vooral van belang. Moet u de waarde **ExpressRoute**. Na het uitvoeren van deze cmdlets, kan de gateway duren 45 minuten of langer te maken.

  ```azurepowershell-interactive
  New-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
  ```

## <a name="verify-the-gateway-was-created"></a>Controleer of dat de gateway is gemaakt
Gebruik de volgende opdrachten om te controleren of de gateway is gemaakt:

```azurepowershell-interactive
Get-AzVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a>Het formaat van een gateway wijzigen
Er zijn een aantal [Gateway-SKU's](../articles/expressroute/expressroute-about-virtual-network-gateways.md). De volgende opdracht kunt u de Gateway-SKU op elk gewenst moment wijzigen.

> [!IMPORTANT]
> Met deze opdracht werkt niet voor UltraPerformance-gateway. Als u wilt wijzigen van de gateway naar een UltraPerformance-gateway, verwijdert u eerst de bestaande ExpressRoute-gateway en maak vervolgens een nieuwe UltraPerformance-gateway. Als u wilt downgraden van de gateway van een UltraPerformance-gateway, verwijdert u eerst de UltraPerformance-gateway en maak vervolgens een nieuwe gateway.
> 
> 

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a>Een gateway verwijderen
Gebruik de volgende opdracht om een gateway te verwijderen:

```azurepowershell-interactive
Remove-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```
