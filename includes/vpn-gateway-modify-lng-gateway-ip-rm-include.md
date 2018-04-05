---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/28/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4ee182202cf1ecbbb0845541269f7241de26c170
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
### <a name="gwipnoconnection"></a> Het 'gatewayIpAddress' van de lokale netwerkgateway wijzigen - geen gatewayverbinding

Als van het VPN-apparaat waarmee u verbinding wilt maken het openbare IP-adres is gewijzigd, moet u de gateway van het lokale netwerk aanpassen met deze wijziging. Gebruik het voorbeeld om een lokale netwerkgateway die geen gatewayverbinding heeft te wijzigen.

Wanneer u deze waarde wijzigt, kunt u tegelijkertijd ook de adresvoorvoegsels wijzigen. Zorg ervoor dat u de bestaande naam van de gateway van uw lokale netwerk gebruikt om de huidige instellingen te overschrijven. Als u een andere naam gebruikt, maakt u een nieuwe lokale netwerkgateway in plaats van de bestaande gateway te overschrijven.

```azurepowershell-interactive
New-AzureRmLocalNetworkGateway -Name Site1 `
-Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName TestRG1
```

### <a name="gwipwithconnection"></a>Het 'gatewayIpAddress' van de lokale netwerkgateway wijzigen - bestaande gatewayverbinding

Als van het VPN-apparaat waarmee u verbinding wilt maken het openbare IP-adres is gewijzigd, moet u de gateway van het lokale netwerk aanpassen met deze wijziging. Als er al een gatewayverbinding bestaat, moet u die verbinding eerst verwijderen. Nadat de verbinding is verwijderd, kunt u het IP-adres van de gateway wijzigen en een nieuwe verbinding maken. U kunt tegelijkertijd ook de adresvoorvoegsels wijzigen. Dit veroorzaakt enige downtime in uw VPN-verbinding. Als u het IP-adres van de gateway wijzigt, hoeft u de VPN-gateway niet te verwijderen. U hoeft alleen de verbinding te verwijderen.
 

1. Verwijder de verbinding. U kunt de naam van uw verbinding vinden met behulp van de cmdlet 'Get-AzureRmVirtualNetworkGatewayConnection'.

  ```azurepowershell-interactive
  Remove-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite1 `
  -ResourceGroupName TestRG1
  ```
2. Wijzig de waarde 'GatewayIpAddress'. U kunt tegelijkertijd ook de adresvoorvoegsels wijzigen. Zorg ervoor dat u de bestaande naam van de gateway van uw lokale netwerk gebruikt om de huidige instellingen te overschrijven. Als u dit niet doet, maakt u een nieuwe lokale netwerkgateway in plaats van de bestaande gateway te overschrijven.

  ```azurepowershell-interactive
  New-AzureRmLocalNetworkGateway -Name Site1 `
  -Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName TestRG1
  ```
3. Maak de verbinding. In dit voorbeeld configureren we een IPsec-verbindingstype. Wanneer u uw verbinding opnieuw maakt, gebruikt u het verbindingstype dat is opgegeven voor uw configuratie. Zie de pagina [PowerShell-cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) voor aanvullende verbindingstypen.  U kunt de cmdlet 'Get-AzureRmVirtualNetworkGateway' uitvoeren om de VirtualNetworkGateway-naam te verkrijgen.
   
    Stel de variabelen in.

  ```azurepowershell-interactive
  $local = Get-AzureRMLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1 `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
  ```
   
    Maak de verbinding.

  ```azurepowershell-interactive 
  New-AzureRmVirtualNetworkGatewayConnection -Name VNet1Site1 -ResourceGroupName TestRG1 `
  -Location "East US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```