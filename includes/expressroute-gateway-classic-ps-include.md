---
title: bestand opnemen
description: bestand opnemen
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 12/13/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 2457ef2843b0d16359b7e47fc54c58e2ef5e6034
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53429947"
---
> [!NOTE]
> Deze voorbeelden niet van toepassing op S2S en ExpressRoute-configuraties naast elkaar bestaan.
> Zie voor meer informatie over het werken met gateways in een configuratie gebruiken [naast elkaar bestaande verbindingen configureren.](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Een gateway toevoegen

Als u een gateway aan een virtueel netwerk met behulp van de klassieke resource-model toevoegen, wijzigt u het netwerkconfiguratiebestand direct vóór het maken van de gateway. De waarden in de onderstaande voorbeelden moet aanwezig zijn in het bestand om een gateway te maken. Als uw virtuele netwerk is eerder een gateway die zijn gekoppeld, worden sommige van deze waarden al aanwezig zijn. Wijzigen van het bestand om de onderstaande waarden te weerspiegelen.

### <a name="download-the-network-configuration-file"></a>Download het netwerkconfiguratiebestand

1. Download het netwerkconfiguratiebestand met behulp van de stappen in [netwerkconfiguratiebestand](../articles/virtual-network/virtual-networks-using-network-configuration-file.md) artikel. Open het bestand met een teksteditor.
2. Een lokale netwerksite toevoegen aan het bestand. U kunt een ongeldig adresvoorvoegsel. U kunt een geldig IP-adres voor de VPN-gateway kunt toevoegen. De waarden in deze sectie worden niet gebruikt voor bewerkingen voor ExpressRoute, maar zijn vereist voor validatie van het bestand. In het voorbeeld is 'branch1' de naam van de site. U kunt een andere naam gebruiken, maar zorg ervoor dat u dezelfde waarde in de sectie van de Gateway van het bestand.

  ```
  <VirtualNetworkConfiguration>
    <Dns />
    <LocalNetworkSites>
      <LocalNetworkSite name="branch1">
        <AddressSpace>
          <AddressPrefix>165.3.1.0/27</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>3.2.1.4</VPNGatewayAddress>
    </LocalNetworkSite>
  ```
3. Navigeer naar de VirtualNetworkSites en de velden wijzigen.

  * Controleer of het Gatewaysubnet voor het virtuele netwerk bestaat. Als dit niet het geval is, kunt u een op dit moment kunt toevoegen. De naam moet 'GatewaySubnet'.
  * Controleer of dat de Gateway-sectie van het bestand bestaat. Als dat niet het geval is, deze toevoegen. Dit is vereist voor het virtuele netwerk koppelen aan de lokale netwerksite (die vertegenwoordigt het netwerk waarmee u verbinding maakt).
  * Controleer of dat de verbindingstype Dedicated =. Dit is vereist voor ExpressRoute-verbindingen.

  ```
  </LocalNetworkSites>
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myAzureVNET" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="default">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.1.0/27</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="branch1">
              <Connection type="Dedicated" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
  </NetworkConfiguration>
  ```
4. Sla het bestand en upload deze naar Azure.

### <a name="create-the-gateway"></a>De gateway maken

Gebruik de onderstaande opdracht om een gateway te maken. Vervang de waarden voor uw eigen.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Controleer of dat de gateway is gemaakt

Gebruik de volgende opdracht om te controleren dat de gateway is gemaakt. Met deze opdracht haalt ook de gateway-ID, die u nodig hebt voor andere bewerkingen.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Formaat van een gateway

Er zijn een aantal [Gateway-SKU's](../articles/expressroute/expressroute-about-virtual-network-gateways.md). De volgende opdracht kunt u de Gateway-SKU op elk gewenst moment wijzigen.

> [!IMPORTANT]
> Met deze opdracht werkt niet voor UltraPerformance-gateway. Als u wilt wijzigen van de gateway naar een UltraPerformance-gateway, verwijdert u eerst de bestaande ExpressRoute-gateway en maak vervolgens een nieuwe UltraPerformance-gateway. Als u wilt downgraden van de gateway van een UltraPerformance-gateway, verwijdert u eerst de UltraPerformance-gateway en maak vervolgens een nieuwe gateway.
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Een gateway verwijderen

Gebruik de onderstaande opdracht om een gateway te verwijderen

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```