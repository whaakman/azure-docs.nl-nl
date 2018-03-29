---
title: bestand opnemen
description: bestand opnemen
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 03/22/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d9eaab359d76afce61cfcc72e3fa125b08856e32
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
De stappen voor deze taak wordt een VNet op basis van de waarden in de volgende configuratielijst gebruikt. Extra instellingen en namen worden ook beschreven in deze lijst. We niet deze lijst gebruiken in een van de stappen, hoewel er variabelen op basis van de waarden in deze lijst toevoegen. U kunt de lijst om te worden gebruikt als referentie, waarbij de waarden vervangt door uw eigen kopiëren.

**Lijst van configuratie-verwijzing**

* Virtuele-netwerknaam = "TestVNet"
* Virtuele adresruimte van het netwerk 192.168.0.0/16 =
* Resourcegroep = "TestRG"
* Subnet1 Name = 'FrontEnd' 
* Adresruimte Subnet1 = '192.168.1.0/24'
* De naam van de gateway-Subnet: 'GatewaySubnet' u moet altijd een gatewaysubnet de naam *GatewaySubnet*.
* Gateway-Subnet-adresruimte = "192.168.200.0/26"
* Regio = 'VS-Oost'
* Gatewaynaam = 'GW'
* De naam van de IP-gateway = "GWIP"
* IP-gatewayconfiguratie Name = "gwipconf"
* Type = "ExpressRoute" dit type is vereist voor een ExpressRoute-configuratie.
* Gateway openbare IP-naam = 'gwpip'

## <a name="add-a-gateway"></a>Een gateway toevoegen
1. Verbinding maken met uw Azure-abonnement.

  ```powershell 
  Login-AzureRmAccount
  Get-AzureRmSubscription 
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. De variabelen voor deze oefening declareren. Zorg ervoor dat het bewerken van het voorbeeld zodat de instellingen die u wilt gebruiken.

  ```powershell 
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "GW"
  $GWIPName = "GWIP"
  $GWIPconfName = "gwipconf"
  $VNetName = "TestVNet"
  ```
3. Het virtuele netwerkobject opslaan als een variabele.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  ```
4. Een gatewaysubnet toevoegen aan het virtuele netwerk. Het gatewaysubnet moet de naam 'GatewaySubnet'. U moet een gatewaysubnet toe van/27 of groter (/ 26/25 enz.).

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
  ```
5. Stel de configuratie in.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. Het gatewaysubnet opslaan als een variabele.

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
  ```
7. Vraag een openbaar IP-adres aan. Het IP-adres is aangevraagd voordat het maken van de gateway. U opgeven niet dat de IP-adres dat u gebruiken wilt. het wordt dynamisch toegewezen. U gebruikt dit IP-adres in de volgende configuratiesectie. De toewijzingsmethode gedefinieerd moet dynamisch zijn.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  ```
8. Maak de configuratie voor uw gateway. De gatewayconfiguratie bepaalt welk subnet en openbaar IP-adres moeten worden gebruikt. In deze stap geeft u de configuratie die wordt gebruikt bij het maken van de gateway. Deze stap maakt het gateway-object niet daadwerkelijk. Gebruik het voorbeeld hieronder om de gatewayconfiguratie te maken.

  ```powershell
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```
9. De gateway te maken. In deze stap de **- GatewayType** is vooral van belang. Moet u de waarde **ExpressRoute**. De gateway kan na het uitvoeren van deze cmdlets duren 45 minuten of langer maken.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
  ```

## <a name="verify-the-gateway-was-created"></a>Controleer of dat de gateway is gemaakt
Gebruik de volgende opdrachten om te controleren of de gateway is gemaakt:

```powershell
Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a>Een gateway vergroten of verkleinen
Er zijn een aantal [Gateway-SKU's](../articles/expressroute/expressroute-about-virtual-network-gateways.md). De volgende opdracht kunt u de Gateway-SKU op elk moment wijzigen.

> [!IMPORTANT]
> Met deze opdracht werkt niet voor UltraPerformance gateway. Als u wilt uw gateway uit naar een gateway UltraPerformance wijzigen, verwijdert u eerst de bestaande ExpressRoute-gateway en maak vervolgens een nieuwe UltraPerformance-gateway. Als u wilt uw gateway van een gateway UltraPerformance downgraden, verwijdert u eerst de gateway UltraPerformance en maak vervolgens een nieuwe gateway.
> 
> 

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a>Een gateway verwijderen
Gebruik de volgende opdracht om een gateway te verwijderen:

```powershell
Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```
