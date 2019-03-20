---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 13089a2514229c5c5bc7b40d9447719247b23405
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58124459"
---
### <a name="noconnection"></a>IP-adresvoorvoegsels wijzigen voor de gateway van een lokaal netwerk - geen gatewayverbinding

Ga als volgt te werk om aanvullende voorvoegsels toe te voegen:

1. Stel de variabele in voor LocalNetworkGateway.

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
   ```
2. Wijzig de voorvoegsels.

   ```azurepowershell-interactive
   Set-AzLocalNetworkGateway -LocalNetworkGateway $local `
   -AddressPrefix @('10.101.0.0/24','10.101.1.0/24','10.101.2.0/24')
   ```

Ga als volgt te werk om adresvoorvoegsels te verwijderen:

  Laat de voorvoegsels weg die u niet langer nodig hebt. In dit voorbeeld hebben we niet meer nodig hebt voorvoegsel 10.101.2.0/24 (uit het vorige voorbeeld), zodat we bij het bijwerken van de lokale netwerkgateway bij zonder dit voorvoegsel.

1. Stel de variabele in voor LocalNetworkGateway.

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
   ```
2. De gateway met de bijgewerkte voorvoegsels instellen.

   ```azurepowershell-interactive
   Set-AzLocalNetworkGateway -LocalNetworkGateway $local `
   -AddressPrefix @('10.101.0.0/24','10.101.1.0/24')
   ```

### <a name="withconnection"></a>IP-adresvoorvoegsels wijzigen voor de gateway van een lokaal netwerk - bestaande gatewayverbinding

Als u een gatewayverbinding hebt en u IP-adresvoorvoegsels wilt toevoegen aan of verwijderen uit uw lokale netwerkgateway, moet u de volgende stappen uitvoeren in de volgorde waarin ze staan vermeld. Dit veroorzaakt enige downtime in uw VPN-verbinding. Als u IP-adresvoorvoegsels wijzigt, hoeft u de VPN-gateway niet te verwijderen. U hoeft alleen de verbinding te verwijderen.

1. Verwijder de verbinding.

   ```azurepowershell-interactive
   Remove-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
   ```
2. De lokale netwerkgateway met de gewijzigde adresvoorvoegsels instellen.
   
   Stel de variabele in voor LocalNetworkGateway.

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
   ```
   
   Wijzig de voorvoegsels.
   
   ```azurepowershell-interactive
   Set-AzLocalNetworkGateway -LocalNetworkGateway $local `
   -AddressPrefix @('10.101.0.0/24','10.101.1.0/24')
   ```
3. Maak de verbinding. In dit voorbeeld configureren we een IPsec-verbindingstype. Wanneer u uw verbinding opnieuw maakt, gebruikt u het verbindingstype dat is opgegeven voor uw configuratie. Zie de pagina [PowerShell-cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) voor aanvullende verbindingstypen.
   
   Stel de variabele in voor VirtualNetworkGateway.

   ```azurepowershell-interactive
   $gateway1 = Get-AzVirtualNetworkGateway -Name VNet1GW  -ResourceGroupName TestRG1
   ```
   
   Maak de verbinding. In dit voorbeeld wordt de variabele $local gebruikt die u in stap 2 hebt ingesteld.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 `
   -ResourceGroupName TestRG1 -Location 'East US' `
   -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
   -ConnectionType IPsec `
   -RoutingWeight 10 -SharedKey 'abc123'
   ```